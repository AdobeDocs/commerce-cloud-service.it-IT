---
title: Eseguire il backup del database
description: Scopri come utilizzare gli strumenti ECE per creare un backup del database per un progetto di infrastruttura cloud di Adobe Commerce.
feature: Cloud, Iaas, Storage
exl-id: 8a96effe-a587-4edf-b0c7-e73ca8d3b56c
source-git-commit: 4d790bff2ba5d02ef10de5c36a2f0d140e31a407
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Eseguire il backup del database

È possibile creare una copia del database utilizzando `ece-tools db-dump` senza acquisire tutti i dati dell&#39;ambiente da servizi e installazioni. Per impostazione predefinita, questo comando crea backup in `/app/var/dump-main` per tutte le connessioni al database specificate nella configurazione dell&#39;ambiente. L&#39;operazione di dump del database passa alla modalità di manutenzione dell&#39;applicazione, interrompe i processi della coda del consumatore e disattiva i processi cron prima dell&#39;inizio del dump.

Considera le seguenti linee guida per l’immagine del database:

- Per gli ambienti di produzione, Adobe consiglia di completare le operazioni di dump del database nelle ore di minore utilizzo per ridurre al minimo le interruzioni del servizio che si verificano quando il sito è in modalità manutenzione.
- Se si verifica un errore durante l&#39;operazione di dump, il comando elimina il file di dump per risparmiare spazio su disco. Per informazioni dettagliate, consulta i registri (`var/log/cloud.log`).
- Per gli ambienti di produzione Pro, questo comando esegue il dump solo da _uno_ dei tre nodi ad alta disponibilità, pertanto i dati di produzione scritti in un nodo diverso durante l’immagine potrebbero non essere copiati. Il comando genera un `var/dbdump.lock` per impedire l&#39;esecuzione del comando su più nodi.
- Per un backup di tutti i servizi dell’ambiente, l’Adobe consiglia di creare un [backup](snapshots.md).

È possibile scegliere di eseguire il backup di più database aggiungendo i nomi dei database al comando. L&#39;esempio seguente esegue il backup di due database: `main` e `sales`:

```bash
php vendor/bin/ece-tools db-dump main sales
```

Utilizza il `php vendor/bin/ece-tools db-dump --help` per altre opzioni:

- `--dump-directory=<dir>`- Scegliere una directory di destinazione per il dump del database
- `--remove-definers`- Rimuovi istruzioni DEFINER dal dump del database

**Per creare un dump del database nell&#39;ambiente di staging o di produzione**:

1. [Utilizzare SSH per accedere o creare un tunnel per connettersi all&#39;ambiente remoto](../development/secure-connections.md) che contiene il database da copiare.

1. Elencare le relazioni dell’ambiente e prendere nota delle informazioni di accesso al database.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]))->database);'
   ```

1. Crea un backup del database. Per scegliere una directory di destinazione per il dump del database, utilizzare `--dump-directory` opzione.

   ```bash
   php vendor/bin/ece-tools db-dump -- main
   ```

   Risposta di esempio:

   ```terminal
   The db-dump operation switches the site to maintenance mode, stops all active cron jobs and consumer queue processes, and disables cron jobs before starting the dump process.
   Your site will not receive any traffic until the operation completes.
   Do you wish to proceed with this process? (y/N)? y
   2020-01-28 16:38:08] INFO: Starting backup.
   [2020-01-28 16:38:08] NOTICE: Enabling Maintenance mode
   [2020-01-28 16:38:10] INFO: Trying to kill running cron jobs and consumers processes
   [2020-01-28 16:38:10] INFO: Running Magento cron and consumers processes were not found.
   [2020-01-28 16:38:10] INFO: Waiting for lock on db dump.
   [2020-01-28 16:38:10] INFO: Start creation DB dump for main database...
   [2020-01-28 16:38:10] INFO: Finished DB dump for main database, it can be found here: /tmp/qxmtlseakof6y/dump-main-1580229490.sql.gz
   [2020-01-28 16:38:10] INFO: Backup completed.
   [2020-01-28 16:38:11] NOTICE: Maintenance mode is disabled.
   ```

1. Il `db-dump` il comando crea un `dump-<timestamp>.sql.gz` file di archivio nella directory del progetto remoto.

>[!TIP]
>
>Se desideri inviare questi dati a un ambiente specifico, consulta [Eseguire la migrazione di dati e file statici](../deploy/staging-production.md#migrate-static-files).
