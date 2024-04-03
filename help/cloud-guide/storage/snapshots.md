---
title: Gestione dei backup
description: Scopri come creare e ripristinare manualmente un backup per il progetto di infrastruttura cloud di Adobe Commerce.
feature: Cloud, Paas, Snapshots, Storage
exl-id: 1cb00db7-2375-4761-9c07-1e20a74859e0
source-git-commit: 1d8ffabb9f903e89495d11c973a9f0a5a8dd1d43
workflow-type: tm+mt
source-wordcount: '682'
ht-degree: 0%

---

# Gestione dei backup

È possibile eseguire un backup manuale degli ambienti Starter attivi in qualsiasi momento utilizzando **[!UICONTROL Backup]** pulsante in [!DNL Cloud Console] o utilizzando `magento-cloud snapshot:create` comando.

Un backup o _istantanea_ è un backup completo dei dati dell&#39;ambiente che include tutti i dati persistenti dei servizi in esecuzione (database MySQL) ed eventuali file archiviati nei volumi montati (var, pub/media, app/ecc.). Lo snapshot _non_ includi il codice, in quanto è già memorizzato nell’archivio basato su Git. Impossibile scaricare una copia di uno snapshot.

La funzione di backup **non** applica agli ambienti Pro. Per impostazione predefinita, gli ambienti Pro Staging e Produzione ricevono backup regolari a scopo di disaster recovery. Vedere [Backup Pro e ripristino di emergenza](../architecture/pro-architecture.md#backup-and-disaster-recovery). A differenza dei backup live automatici negli ambienti Pro Staging e Production, i backup sono **non** automatica. È _tuo_ responsabilità di creare manualmente un backup o di impostare un processo cron per creare periodicamente un backup degli ambienti di integrazione Starter o Pro.

## Creare un backup manuale

È possibile creare un backup manuale di qualsiasi ambiente Starter attivo e dell&#39;ambiente di integrazione Pro dall&#39; [!DNL Cloud Console] oppure crea un’istantanea da Cloud CLI. Devi avere un [Ruolo amministratore](../project/user-access.md) per l&#39;ambiente.

**Per creare un backup di qualsiasi ambiente Starter utilizzando[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleziona un ambiente dalla barra di navigazione del progetto. L’ambiente deve essere attivo.
1. In _Backup_ visualizza, fai clic su **[!UICONTROL Backup]**. Questa opzione non è disponibile per un ambiente Pro.

   ![Backup](../../assets/button-backup.png){width="150"}

**Per creare un backup di un ambiente di integrazione utilizzando[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleziona un ambiente di integrazione/sviluppo dalla barra di navigazione del progetto. L’ambiente deve essere attivo.
1. Seleziona la **[!UICONTROL Backup]** nel menu in alto a destra. Questa opzione è disponibile per gli ambienti Starter e Pro.
1. Fai clic su **[!UICONTROL Yes]** pulsante.

**Per creare un&#39;istantanea utilizzando `magento-cloud` CLI**:

1. Sulla workstation locale, passa alla directory del progetto.
1. Estrai il ramo dell’ambiente per creare un’istantanea.
1. Crea lo snapshot.

   ```bash
   magento-cloud snapshot:create --live
   ```

   In alternativa, è possibile utilizzare `magento-cloud backup` comando breve. Il `--live` lascia l’ambiente in esecuzione per evitare tempi di inattività. Per un elenco completo delle opzioni, immetti `magento-cloud snapshot:create --help`.

   Risposta di esempio:

   ```terminal
   Creating a snapshot of develop-branch
   Waiting for the activity ID (User created a backup of develop-branch):
   
   Creating backup of develop-branch
   Created backup my-snapshot
   [============================] 45 secs (complete)
   Activity ID succeeded
   Snapshot name: my-snapshot
   ```

1. Verificare gli snapshot più recenti.

   ```bash
   magento-cloud snapshot:list
   ```

   L&#39;elenco restituisce informazioni sullo stato dello snapshot:

   ```terminal
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## Ripristinare un backup manuale

Devi avere [Accesso amministratore](../project/user-access.md) all&#39;ambiente. Hai fino a **sette giorni** a _ripristinare_ un backup manuale. Il ripristino di un backup non modifica il codice del ramo Git corrente. Il ripristino di un backup in questo modo non si applica agli ambienti di staging e produzione Pro; vedere [Backup Pro e ripristino di emergenza](../architecture/pro-architecture.md#backup-and-disaster-recovery).

I tempi di ripristino variano a seconda delle dimensioni del database:

- database di grandi dimensioni (oltre 200 GB) può richiedere 5 ore
- database di medie dimensioni (150 GB) può richiedere 2 ore e mezza
- database di piccole dimensioni (60 GB) può richiedere 1 ora

>[!TIP]
>
>Ripristino senza backup:
>
>- Per ripristinare il codice precedente o rimuovere le estensioni aggiunte in un ambiente, vedi [Codice di rollback](#roll-back-code).
>- Per ripristinare un ambiente instabile che _non_ avere un backup, vedi [Ripristinare un ambiente](../development/restore-environment.md).

**Per ripristinare un backup utilizzando[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleziona un ambiente dalla barra di navigazione del progetto.
1. In _Backup_ , scegliere un backup dalla _Archiviato_ elenco. La funzione di backup **non** applica agli ambienti Pro.
1. In ![Altro](../../assets/icon-more.png){width="32"} (_altro_), fare clic su **Ripristina**.
1. Rivedi le informazioni di ripristino dal backup e fai clic su **Sì, ripristina**.

**Per ripristinare uno snapshot utilizzando Cloud CLI**:

1. Sulla workstation locale, passa alla directory del progetto.
1. Consulta il ramo dell’ambiente da ripristinare.
1. Elenca tutte le istantanee disponibili.

   ```bash
   magento-cloud snapshot:list
   ```

   L&#39;elenco restituisce informazioni sugli snapshot disponibili:

   ```terminal
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

1. Ripristinare una copia istantanea utilizzando l&#39;ID della copia istantanea dall&#39;elenco.

   ```bash
   magento-cloud snapshot:restore <snapshot-id>
   ```

## Codice di rollback

Backup e snapshot eseguiti _non_ includi una copia del codice. Il codice è già archiviato nell’archivio basato su Git, pertanto puoi utilizzare i comandi basati su Git per eseguire il rollback (o il ripristino) del codice. Ad esempio, utilizza `git log --oneline` per scorrere le conferme precedenti, quindi utilizzare [`git revert`](https://git-scm.com/docs/git-revert) per ripristinare il codice da un commit specifico.

Inoltre, puoi scegliere di memorizzare il codice in un _inattivo_ filiale. Utilizza i comandi Git per creare un ramo invece di utilizzare `magento-cloud` comandi. Vedi informazioni su [Comandi Git](../dev-tools/cloud-cli-overview.md#git-commands) nell’argomento Cloud CLI.
