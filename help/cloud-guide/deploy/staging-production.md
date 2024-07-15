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
>L&#39;Adobe consiglia di creare un [backup](../storage/snapshots.md) dell&#39;ambiente prima delle distribuzioni.

È inoltre possibile abilitare [Tracciare le distribuzioni con New Relic](../monitor/track-deployments.md) per monitorare gli eventi di distribuzione e analizzare le prestazioni tra le distribuzioni.

## Flusso di distribuzione iniziale

L&#39;Adobe consiglia di creare un ramo `staging` dal ramo `master` per supportare al meglio lo sviluppo e la distribuzione del piano Starter. Sono quindi pronti due dei quattro ambienti attivi: `master` per la produzione e `staging` per la gestione temporanea.

Per informazioni dettagliate sul processo, vedere [Avvia sviluppo e distribuzione del flusso di lavoro](../architecture/starter-develop-deploy-workflow.md).

## Flusso di distribuzione Pro

Pro viene fornito con un ampio ambiente di integrazione con due rami attivi, un ramo `master` globale, staging e rami di produzione. Quando crei il progetto, il codice è pronto per diramarsi, sviluppare e inviare messaggi push per la creazione e la distribuzione del sito. Anche se l’ambiente di integrazione può avere molti rami, Staging e Produzione hanno un solo ramo per ogni ambiente.

Per informazioni dettagliate sul processo, vedere [Pro Develop and Deploy Workflow](../architecture/pro-develop-deploy-workflow.md).

## Distribuire il codice nello staging

L’ambiente di staging fornisce un ambiente di produzione simile a quello di, che include un database, un server web e tutti i servizi, compresi Fastly e New Relic. È possibile eseguire il push, l&#39;unione e la distribuzione completi tramite i comandi CLI [[!DNL Cloud Console]](../project/overview.md) o [Cloud](../dev-tools/cloud-cli-overview.md) tramite un&#39;applicazione terminal.

### Distribuisci il codice con [!DNL Cloud Console]

[!DNL Cloud Console] fornisce funzionalità per creare, gestire e distribuire il codice negli ambienti di integrazione, gestione temporanea e produzione per i piani Starter e Pro.

**Per i progetti Pro, distribuire il ramo di integrazione nell&#39;area di gestione temporanea**:

1. [Accedi](https://accounts.magento.cloud) al progetto.
1. Selezionare il ramo `integration`.
1. Selezionare l&#39;opzione **Unisci** per eseguire la distribuzione nell&#39;area di gestione temporanea.

   ![Unisci](../../assets/button-merge.png){width="150"}

1. Completa tutti i [test](../test/staging-and-production.md) nell&#39;ambiente di staging.
1. Quando la gestione temporanea è pronta, seleziona l&#39;opzione **Unisci** per distribuirla alla produzione.

**Per iniziare, distribuire il ramo di sviluppo nell&#39;area di gestione temporanea**:

1. [Accedi](https://accounts.magento.cloud) al progetto.
1. Seleziona il ramo del codice preparato.
1. Selezionare l&#39;opzione **Unisci** per eseguire la distribuzione nell&#39;area di gestione temporanea.

   ![Unisci](../../assets/button-merge.png){width="150"}

1. Completa tutti i [test](../test/staging-and-production.md) nell&#39;ambiente di staging.
1. Quando la gestione temporanea è pronta, selezionare l&#39;opzione **Unisci** da distribuire alla produzione (`master`).

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

[I file statici](https://experienceleague.adobe.com/docs/commerce-operations/operational-playbook/glossary.html) sono archiviati in `mounts`. Esistono due metodi per migrare i file da una posizione di montaggio di origine, ad esempio l&#39;ambiente locale, a una posizione di montaggio di destinazione. Entrambi i metodi utilizzano l&#39;utilità `rsync`, ma Adobe consiglia di utilizzare l&#39;interfaccia della riga di comando `magento-cloud` per spostare i file tra l&#39;ambiente locale e remoto. Adobe consiglia inoltre di utilizzare il metodo `rsync` per spostare i file da un&#39;origine remota a un&#39;altra posizione remota.

### Eseguire la migrazione dei file tramite CLI

È possibile utilizzare i comandi CLI `mount:upload` e `mount:download` per eseguire la migrazione dei file tra l&#39;ambiente locale e remoto. Entrambi i comandi utilizzano l&#39;utilità `rsync`, ma i comandi CLI forniscono opzioni e prompt personalizzati per l&#39;ambiente Adobe Commerce sull&#39;infrastruttura cloud. Ad esempio, se si utilizza il comando semplice senza opzioni, la CLI richiede di selezionare il mount o i mount da caricare o scaricare.

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

**Per caricare i file da una cartella `pub/media/` locale nella cartella `pub/media/` remota per l&#39;ambiente corrente**:

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

Utilizzare l&#39;opzione `--help` per i comandi `mount:upload` e `mount:download` per visualizzare altre opzioni. Ad esempio, è disponibile un&#39;opzione `--delete` per rimuovere i file estranei durante la migrazione.

### Eseguire la migrazione dei file tramite rsync

In alternativa, è possibile utilizzare l&#39;utilità `rsync` per eseguire la migrazione dei file.

```bash
rsync -azvP <source> <destination>
```

Questo comando utilizza le opzioni seguenti:

- Archivio `a`
- `z`-compressione dei file durante la migrazione
- `v`-dettagliato
- Avanzamento parziale di `P`

Consulta la Guida di [rsync](https://linux.die.net/man/1/rsync).

>[!NOTE]
>
>Per trasferire i file multimediali direttamente dagli ambienti remoti a remoti, è necessario abilitare l&#39;inoltro dell&#39;agente SSH. Vedere [Guida GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

**Migrazione diretta di file statici da ambienti remoti a remoti (approccio rapido)**:

1. Utilizza SSH per accedere all’ambiente di origine. Non utilizzare l&#39;interfaccia della riga di comando `magento-cloud`. L&#39;utilizzo dell&#39;opzione `-A` è importante perché consente l&#39;inoltro della connessione dell&#39;agente di autenticazione.

   >[!TIP]
   >
   >Per trovare il collegamento **Accesso SSH** in [!DNL Cloud Console], selezionare l&#39;ambiente e fare clic su **Accesso al sito**.

   ```bash
   ssh -A <environment_ssh_link@ssh.region.magento.cloud>
   ```

1. Utilizzare il comando `rsync` per copiare la directory `pub/media` dall&#39;ambiente di origine in un ambiente remoto diverso.

   ```bash
   rsync -azvP pub/media/ <destination_environment_ssh_link@ssh.region.magento.cloud>:pub/media/
   ```

1. Accedere all&#39;altro ambiente remoto per verificare i file migrati correttamente.

## Migrare il database

>[!WARNING]
>
>Le operazioni di importazione ed esportazione del database possono richiedere molto tempo e influire sulle prestazioni e sulla disponibilità del sito. Pianificare le operazioni di importazione ed esportazione durante le ore di minore utilizzo per evitare rallentamenti delle prestazioni o interruzioni nei siti di produzione.

>[!BEGINSHADEBOX]

**Prerequisito:** Un dump del database (vedere il passaggio 3) deve includere i trigger del database. Per scaricarli, confermare di disporre del privilegio [TRIGGER](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_trigger).

>[!IMPORTANT]
>
>Il database dell’ambiente di integrazione è destinato esclusivamente ai test di sviluppo e può includere dati di cui non desideri eseguire la migrazione nell’ambiente di staging e produzione.

Per le distribuzioni di integrazione continue, l&#39;Adobe **sconsiglia** la migrazione dei dati dall&#39;integrazione all&#39;ambiente di staging e produzione. Puoi trasmettere dati di test o sovrascrivere dati importanti. Tutte le configurazioni vitali vengono passate utilizzando il [file di configurazione](../store/store-settings.md) e il comando `setup:upgrade` durante la compilazione e la distribuzione.

>[!ENDSHADEBOX]

L&#39;Adobe **consiglia** di migrare i dati dalla produzione alla gestione temporanea per testare completamente il sito e archiviarlo in un ambiente vicino alla produzione con tutti i servizi e le impostazioni.

>[!NOTE]
>
>Per trasferire i file multimediali direttamente dagli ambienti remoti a quelli remoti, è necessario abilitare l&#39;inoltro degli agenti SSH. Vedere le [linee guida GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/using-ssh-agent-forwarding).

### Eseguire il backup del database

È consigliabile creare un backup del database. Nella procedura seguente vengono utilizzate le istruzioni di [Backup del database](../storage/database-dump.md).

**Per scaricare il database**:

1. [Utilizzare SSH per accedere all&#39;ambiente remoto](../development/secure-connections.md#use-an-ssh-command) che contiene il database da copiare.

1. Elencare le relazioni dell’ambiente e prendere nota delle informazioni di accesso al database.

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

   Per Pro Staging and Production, il nome del database è nella variabile `MAGENTO_CLOUD_RELATIONSHIPS` (in genere lo stesso nome dell&#39;applicazione e nome utente).

1. Crea un backup del database. Per scegliere una directory di destinazione per il dump del database, utilizzare l&#39;opzione `--dump-directory`.

   Per gli ambienti Starter e gli ambienti di integrazione Pro, utilizzare `main` come nome del database:

   ```bash
   php vendor/bin/ece-tools db-dump main
   ```

   Opzioni di dump:
   - `--dump-directory=<dir>` - Scegliere una directory di destinazione per il dump del database
   - `--remove-definers` - Rimuovi istruzioni DEFINER dal dump del database

1. Sebbene sia preferibile utilizzare il metodo ECE-Tools, un altro metodo consiste nel creare un file di dump del database utilizzando MySQL nativo in formato GZIP.

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers <database-name> | gzip - > /tmp/database.sql.gz
   ```

   Se nell&#39;ambiente di destinazione è stata configurata l&#39;autenticazione a due fattori, è preferibile escludere le tabelle 2FA correlate per evitare di riconfigurarle dopo la migrazione del database:

   ```bash
   mysqldump -h <database-host> --user=<database-username> --password=<password> --single-transaction --triggers --ignore-table=<database-name>.tfa_user_config --ignore-table=<database-name>.tfa_country_codes <database-name> | gzip - > /tmp/database.sql.gz
   ```

1. Digitare `logout` per terminare la connessione SSH.

### Eliminare e ricreare il database

Durante l&#39;importazione dei dati è necessario eliminare e creare un database.

**Per eliminare e ricreare il database**:

1. Stabilisci un [tunnel SSH](../development/secure-connections.md#ssh-tunneling) per l&#39;ambiente remoto.

1. Connettersi al servizio del database.

   ```bash
   mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
   ```

1. Al prompt `MariaDB [main]>`, eliminare il database.

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
