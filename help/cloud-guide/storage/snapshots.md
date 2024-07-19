---
title: Gestione dei backup
description: Scopri come creare e ripristinare manualmente un backup per il progetto di infrastruttura cloud di Adobe Commerce.
feature: Cloud, Paas, Snapshots, Storage
exl-id: 1cb00db7-2375-4761-9c07-1e20a74859e0
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 0%

---

# Gestione dei backup

È possibile eseguire un backup manuale degli ambienti Starter attivi in qualsiasi momento utilizzando il pulsante **[!UICONTROL Backup]** in [!DNL Cloud Console] o il comando `magento-cloud snapshot:create`.

Un backup o _snapshot_ è un backup completo dei dati dell&#39;ambiente che include tutti i dati persistenti dei servizi in esecuzione (database MySQL) e di tutti i file archiviati nei volumi montati (var, pub/media, app/ecc.). Lo snapshot _non_ include codice, poiché il codice è già archiviato nell&#39;archivio basato su Git. Impossibile scaricare una copia di uno snapshot.

La funzionalità di backup/snapshot non è applicabile **not** agli ambienti di staging e produzione Pro, che ricevono backup regolari a scopo di disaster recovery per impostazione predefinita. Per ulteriori informazioni, consultare [Backup Pro e Disaster Recovery](../architecture/pro-architecture.md#backup-and-disaster-recovery). A differenza dei backup live automatici negli ambienti Pro Staging e Production, i backup sono **non** automatici. È _tua_ responsabilità creare manualmente un backup o impostare un processo cron per creare periodicamente un backup degli ambienti di integrazione Starter o Pro.

## Creare un backup manuale

È possibile creare un backup manuale di qualsiasi ambiente Starter attivo e dell&#39;ambiente di integrazione Pro da [!DNL Cloud Console] o creare un&#39;istantanea da Cloud CLI. Devi avere un [Ruolo amministratore](../project/user-access.md) per l&#39;ambiente.

**Per creare un backup di qualsiasi ambiente Starter utilizzando[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleziona un ambiente dalla barra di navigazione del progetto. L’ambiente deve essere attivo.
1. Nella visualizzazione _Backup_, fare clic su **[!UICONTROL Backup]**. Questa opzione non è disponibile per un ambiente Pro.

   ![Backup](../../assets/button-backup.png){width="150"}

**Per creare un backup di un ambiente di integrazione utilizzando[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleziona un ambiente di integrazione/sviluppo dalla barra di navigazione del progetto. L’ambiente deve essere attivo.
1. Selezionare l&#39;opzione **[!UICONTROL Backup]** nel menu in alto a destra. Questa opzione è disponibile per gli ambienti Starter e Pro.
1. Fare clic sul pulsante **[!UICONTROL Yes]**.

**Per creare uno snapshot utilizzando `magento-cloud` CLI**:

1. Sulla workstation locale, passa alla directory del progetto.
1. Estrai il ramo dell’ambiente per creare un’istantanea.
1. Crea lo snapshot.

   ```bash
   magento-cloud snapshot:create --live
   ```

   In alternativa, è possibile utilizzare il comando breve `magento-cloud backup`. L&#39;opzione `--live` lascia l&#39;ambiente in esecuzione per evitare tempi di inattività. Per un elenco completo delle opzioni, immettere `magento-cloud snapshot:create --help`.

   Risposta di esempio:

   ```
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

   ```
   Snapshots on the project (project-id), environment develop-branch (type: development):
   +---------------------------+----------------------+------------+
   | Created                   | Snapshot ID          | Restorable |
   +---------------------------+----------------------+------------+
   | 2023-03-08T17:07:01+00:00 | my-snapshot          | true       |
   +---------------------------+----------------------+------------+
   ```

## Ripristinare un backup manuale

Devi avere [accesso amministratore](../project/user-access.md) all&#39;ambiente. Hai fino a **sette giorni** per _ripristinare_ un backup manuale. Il ripristino di un backup non modifica il codice del ramo Git corrente. Il ripristino di un backup in questo modo non è applicabile agli ambienti di staging e produzione Pro; vedere [Backup Pro e ripristino di emergenza](../architecture/pro-architecture.md#backup-and-disaster-recovery).

I tempi di ripristino variano a seconda delle dimensioni del database:

- database di grandi dimensioni (oltre 200 GB) può richiedere 5 ore
- database di medie dimensioni (150 GB) può richiedere 2 ore e mezza
- database di piccole dimensioni (60 GB) può richiedere 1 ora

>[!TIP]
>
>Ripristino senza backup:
>
>- Per ripristinare il codice precedente o rimuovere le estensioni aggiunte in un ambiente, vedere [Codice di rollback](#roll-back-code).
>- Per ripristinare un ambiente instabile che non dispone di un backup di _not_, vedere [Ripristinare un ambiente](../development/restore-environment.md).

**Per ripristinare un backup utilizzando[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleziona un ambiente dalla barra di navigazione del progetto.
1. Nella visualizzazione _Backup_, scegliere un backup dall&#39;elenco _Archiviato_. La funzionalità di backup **non** è applicabile agli ambienti Pro.
1. Nel menu ![Altro](../../assets/icon-more.png){width="32"} (_Altro_), fare clic su **Ripristina**.
1. Rivedere le informazioni di ripristino dal backup e fare clic su **Sì, ripristino**.

**Per ripristinare uno snapshot utilizzando Cloud CLI**:

1. Sulla workstation locale, passa alla directory del progetto.
1. Consulta il ramo dell’ambiente da ripristinare.
1. Elenca tutte le istantanee disponibili.

   ```bash
   magento-cloud snapshot:list
   ```

   L&#39;elenco restituisce informazioni sugli snapshot disponibili:

   ```
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

## Ripristino di un&#39;istantanea di disaster recovery

Per ripristinare lo snapshot del ripristino di emergenza negli ambienti di staging e produzione Pro, [Importare l&#39;immagine del database direttamente dal server](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production#meth3).

## Codice di rollback

I backup e gli snapshot non includono _not_ una copia del codice. Il codice è già archiviato nell’archivio basato su Git, pertanto puoi utilizzare i comandi basati su Git per eseguire il rollback (o il ripristino) del codice. Utilizzare ad esempio `git log --oneline` per scorrere i commit precedenti, quindi utilizzare [`git revert`](https://git-scm.com/docs/git-revert) per ripristinare il codice da un commit specifico.

Inoltre, puoi scegliere di archiviare il codice in un ramo _inattivo_. Utilizza i comandi Git per creare un ramo anziché i comandi `magento-cloud`. Per informazioni sui [comandi Git](../dev-tools/cloud-cli-overview.md#git-commands), vedere l&#39;argomento CLI di Cloud.
