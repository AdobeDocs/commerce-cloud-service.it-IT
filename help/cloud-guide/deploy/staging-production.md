---
title: Distribuzione a staging e produzione
description: Scopri come implementare il codice dell’infrastruttura cloud Adobe Commerce negli ambienti di staging e produzione per ulteriori test.
feature: Cloud, Console, Deploy, SCD, Storage
exl-id: 4b82289f-ee04-4b14-a0ed-7a8a19fc6a6a
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1289'
ht-degree: 0%

---

# Distribuzione a staging e produzione

Il processo per la distribuzione e la pubblicazione inizia con lo sviluppo, continua con la gestione temporanea e termina con la pubblicazione in produzione. Adobe fornisce una soluzione di ambiente end-to-end per garantire configurazioni coerenti. Ogni ambiente supporta l’accesso diretto agli URL della vetrina e l’accesso amministratore e SSH per i comandi CLI.

Quando sei pronto per distribuire l’archivio, devi completare la distribuzione e il test nell’ambiente di staging prima di distribuirlo nell’ambiente di produzione. In questa sezione vengono fornite istruzioni e informazioni dettagliate sul processo di creazione e distribuzione, sulla migrazione di dati e contenuti e sui test.

>[!TIP]
>
>L’Adobe consiglia di creare un [backup](../storage/snapshots.md) dell’ambiente prima delle implementazioni.

Inoltre, puoi abilitare [Tracciare le implementazioni con New Relic](../monitor/track-deployments.md) per monitorare gli eventi di distribuzione e analizzare le prestazioni tra le distribuzioni.

## Flusso di distribuzione iniziale

L’Adobe consiglia di creare un `staging` ramo da `master` per supportare al meglio lo sviluppo e la distribuzione del piano Starter. Sono quindi pronti due dei quattro ambienti attivi: `master` per la produzione e `staging` per staging.

Per informazioni dettagliate sul processo, consulta [Avvia sviluppo e implementazione del flusso di lavoro](../architecture/starter-develop-deploy-workflow.md).

## Flusso di distribuzione Pro

Pro viene fornito con un ampio ambiente di integrazione con due rami attivi, un `master` rami di branca, staging e produzione. Quando crei il progetto, il codice è pronto per diramarsi, sviluppare e inviare messaggi push per la creazione e la distribuzione del sito. Anche se l’ambiente di integrazione può avere molti rami, Staging e Produzione hanno un solo ramo per ogni ambiente.

Per informazioni dettagliate sul processo, consulta [Flusso di lavoro Pro Develop and Deploy](../architecture/pro-develop-deploy-workflow.md).

## Distribuire il codice nello staging

L’ambiente di staging fornisce un ambiente di produzione simile a quello di, che include un database, un server web e tutti i servizi, compresi Fastly e New Relic. Puoi eseguire operazioni push, merge e implementa complete tramite [[!DNL Cloud Console]](../project/overview.md) o [Comandi Cloud CLI](../dev-tools/cloud-cli-overview.md) tramite un&#39;applicazione terminale.

### Distribuire il codice con [!DNL Cloud Console]

Il [!DNL Cloud Console] fornisce funzioni per creare, gestire e implementare il codice negli ambienti di integrazione, staging e produzione per i piani Starter e Pro.

**Per i progetti Pro, implementa il ramo di integrazione nell’ambiente di staging**:

1. [Accedi](https://accounts.magento.cloud) al progetto.
1. Seleziona la `integration` filiale.
1. Seleziona la **Unisci** opzione da distribuire nell&#39;area di gestione temporanea.

   ![Unisci](../../assets/button-merge.png){width="150"}

1. Completa tutto [test](../test/staging-and-production.md) nell&#39;ambiente di staging.
1. Quando Staging è pronto, seleziona la **Unisci** opzione da implementare nell’ambiente di produzione.

**Per iniziare, distribuisci il ramo di sviluppo nell’ambiente di staging**:

1. [Accedi](https://accounts.magento.cloud) al progetto.
1. Seleziona il ramo del codice preparato.
1. Seleziona la **Unisci** opzione da distribuire nell&#39;area di gestione temporanea.

   ![Unisci](../../assets/button-merge.png){width="150"}

1. Completa tutto [test](../test/staging-and-production.md) nell&#39;ambiente di staging.
1. Quando Staging è pronto, seleziona la **Unisci** opzione da implementare nell’ambiente di produzione (`master`).

### Distribuire il codice con la riga di comando

Cloud CLI fornisce i comandi per distribuire il codice. Hai bisogno dell’accesso SSH e Git al progetto.

#### Passaggio 1: distribuire e testare l’ambiente di integrazione

1. Dopo aver effettuato l’accesso al progetto, controlla l’ambiente di integrazione:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Sincronizzare l&#39;ambiente di integrazione locale con l&#39;ambiente remoto:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Creare un&#39;istantanea dell&#39;ambiente come backup:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Aggiorna il codice nel ramo locale in base alle esigenze.

1. Aggiungi, esegui il commit e invia le modifiche all’ambiente.

   ```bash
   git add -A && git commit -m "Commit message" && git push origin <environment-ID>
   ```

1. Completare il test del sito.

#### Passaggio 2: unire le modifiche a Staging e distribuzione

1. Consulta l’ambiente di staging:

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

1. Sincronizzare l&#39;ambiente di staging locale con l&#39;ambiente remoto:

   ```bash
   magento-cloud environment:synchronize <environment-ID>
   ```

1. Creare un&#39;istantanea dell&#39;ambiente come backup:

   ```bash
   magento-cloud snapshot: create -e <environment-ID>
   ```

1. Unisci l’ambiente di integrazione a Staging per distribuire:

   ```bash
   magento-cloud environment:merge <integration-ID>
   ```

1. Completare il test del sito.

#### Passaggio 3: distribuire in produzione

1. Estrai, sincronizza e crea un’istantanea dell’ambiente di produzione locale.

1. Unisci l’ambiente di staging alla produzione per distribuire:

   ```bash
   magento-cloud environment:merge <staging-ID>
   ```

1. Completare il test del sito.

## Migrazione di file statici

[File statici](https://experienceleague.adobe.com/docs/commerce-operations/operational-playbook/glossary.html) sono archiviati in `mounts`. Esistono due metodi per migrare i file da una posizione di montaggio di origine, ad esempio l&#39;ambiente locale, a una posizione di montaggio di destinazione. Entrambi i metodi utilizzano `rsync` , ma l&#39;Adobe consiglia di utilizzare il `magento-cloud` CLI per lo spostamento di file tra l&#39;ambiente locale e remoto. E l’Adobe consiglia di utilizzare `rsync` metodo per spostare file da un&#39;origine remota a un&#39;altra posizione remota.

### Eseguire la migrazione dei file tramite CLI

È possibile utilizzare `mount:upload` e `mount:download` Comandi CLI per eseguire la migrazione dei file tra l&#39;ambiente locale e remoto. Entrambi i comandi utilizzano `rsync` ma i comandi CLI forniscono opzioni e prompt personalizzati per l&#39;ambiente Adobe Commerce on cloud infrastructure. Ad esempio, se si utilizza il comando semplice senza opzioni, la CLI richiede di selezionare il mount o i mount da caricare o scaricare.

```bash
magento-cloud mount:download
```

Risposta di esempio:

```terminal
Enter a number to choose a mount to download from:
  [0] app/etc
  [1] pub/static
  [2] var
  [3] pub/media
  [4] All mounts
 > 3

Target directory: ~/pub/media/

Downloading files from the remote mount pub/media to pub/media

Are you sure you want to continue? [Y/n] Y
```

**Per caricare i file da un `pub/media/` cartella al telecomando `pub/media/` cartella per l&#39;ambiente corrente**:

```bash
magento-cloud mount:upload --source /path/to/project/pub/media/ --mount pub/media/
```

Risposta di esempio:

```terminal
Uploading files from pub/media to the remote mount pub/media

Are you sure you want to continue? [Y/n] Y

  building file list ...   done
  ./
  sample-file.jpeg

  sent 8.43K bytes  received 48 bytes  3.39K bytes/sec
  total size is 154.57K  speedup is 18.23
```

Utilizza il `--help` opzione per `mount:upload` e `mount:download` per visualizzare altre opzioni. Ad esempio, è presente un `--delete` per rimuovere i file estranei durante la migrazione.

### Eseguire la migrazione dei file tramite rsync

In alternativa, è possibile utilizzare `rsync` per migrare i file.

```bash
rsync -azvP <source> <destination>
```

Questo comando utilizza le opzioni seguenti:

- `a`-archivio
- `z`-comprimere i file durante la migrazione
- `v`-verboso
- `P`-avanzamento parziale

Consulta [rsync](https://linux.die.net/man/1/rsync) aiuto.

>[!NOTE]
>
>Per trasferire i file multimediali direttamente dagli ambienti remoti a remoti, è necessario abilitare l’inoltro dell’agente SSH, vedi [Guida di GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

**Migrazione diretta di file statici da ambienti remoti a remoti (approccio rapido)**:

1. Utilizza SSH per accedere all’ambiente di origine. Non utilizzare il `magento-cloud` CLI Utilizzo di `-A` è importante perché consente l’inoltro della connessione dell’agente di autenticazione.

   >[!TIP]
   >
   >Per trovare **Accesso SSH** collegamento nel tuo [!DNL Cloud Console], seleziona l’ambiente e fai clic su **Accedi al sito**.

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. Utilizza il `rsync` comando per copiare `pub/media` dall&#39;ambiente di origine a un ambiente remoto diverso.

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. Accedere all&#39;altro ambiente remoto per verificare i file migrati correttamente.

## Migrare il database

>[!WARNING]
>
>Le operazioni di importazione ed esportazione del database possono richiedere molto tempo e influire sulle prestazioni e sulla disponibilità del sito. Pianificare le operazioni di importazione ed esportazione durante le ore di minore utilizzo per evitare rallentamenti delle prestazioni o interruzioni nei siti di produzione.

>[!BEGINSHADEBOX]

**Prerequisito:** Un’immagine del database (vedi Passaggio 3) deve includere i trigger del database. Per scaricarli, conferma di disporre della [Privilegio TRIGGER](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger).

>[!IMPORTANT]
>
>Il database dell’ambiente di integrazione è destinato esclusivamente ai test di sviluppo e può includere dati di cui non desideri eseguire la migrazione nell’ambiente di staging e produzione.

Per distribuzioni di integrazione continue, Adobe **non consiglia** migrazione dei dati da Integration a Staging e Produzione. Puoi trasmettere dati di test o sovrascrivere dati importanti. Tutte le configurazioni vitali vengono passate utilizzando [file di configurazione](../store/store-settings.md) e `setup:upgrade` durante la generazione e la distribuzione.

>[!ENDSHADEBOX]

Adobe **consiglia** migrazione dei dati da Produzione a Staging per testare completamente il sito e archiviarlo in un ambiente di produzione vicino con tutti i servizi e le impostazioni.

>[!NOTE]
>
>Per trasferire i file multimediali direttamente dagli ambienti remoti a remoti, è necessario abilitare l’inoltro degli agenti ssh, consulta [Guida di GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

### Eseguire il backup del database

È consigliabile creare un backup del database. Nella procedura seguente vengono utilizzate le istruzioni [Eseguire il backup del database](../storage/database-dump.md).

**Per scaricare il database**:

1. [Utilizzare SSH per accedere all’ambiente remoto](../development/secure-connections.md#use-an-ssh-command) che contiene il database da copiare.

1. Elencare le relazioni dell’ambiente e prendere nota delle informazioni di accesso al database.

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   Per Pro Staging e Produzione, il nome del database si trova nel `MAGENTO_CLOUD_RELATIONSHIPS` (in genere corrisponde al nome dell’applicazione e al nome utente).

1. Crea un backup del database. Per scegliere una directory di destinazione per il dump del database, utilizzare `--dump-directory` opzione.

   Per gli ambienti Starter e gli ambienti di integrazione Pro, utilizzare `main` come nome del database:

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   Opzioni di dump:
   - `--dump-directory=<dir>`- Scegliere una directory di destinazione per il dump del database
   - `--remove-definers`- Rimuovi istruzioni DEFINER dal dump del database

1. Sebbene sia preferibile utilizzare il metodo ECE-Tools, un altro metodo consiste nel creare un file di dump del database utilizzando MySQL nativo in formato GZIP.

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   Se nell&#39;ambiente di destinazione è stata configurata l&#39;autenticazione a due fattori, è preferibile escludere le tabelle 2FA correlate per evitare di riconfigurarle dopo la migrazione del database:

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. Tipo `logout` per terminare la connessione SSH.

### Eliminare e ricreare il database

Durante l&#39;importazione dei dati è necessario eliminare e creare un database.

**Per eliminare e ricreare il database**:

1. Stabilire un [Tunnel SSH](../development/secure-connections.md#ssh-tunneling) all&#39;ambiente remoto.

1. Connettersi al servizio del database.

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. Alla `MariaDB [main]>` , eliminare il database.

   Per l&#39;integrazione con Starter e Pro:

   ```shell
   drop database main;
   ```

   Per la produzione:

   ```shell
   drop database <cluster-id>;
   ```

   Per staging:

   ```shell
   drop database <cluster-ID_stg>;
   ```

1. Ricreare il database.

   Per l&#39;integrazione con Starter e Pro:

   ```shell
   create database main;
   ```

   Importa per produzione:

   ```shell
   zcat <cluster-ID>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```

   Importa per staging:

   ```shell
   zcat <cluster-ID_stg>.sql.gz | sed -e 's/DEFINER[ ]*=[ ]*[^*]*\*/\*/' | mysql -h 127.0.0.1 -p -u <database-username> <database-name>;
   ```
