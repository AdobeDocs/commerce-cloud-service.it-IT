---
title: Gestione dello spazio su disco
description: Scopri come gestire lo spazio su disco utilizzando l’interfaccia della riga di comando.
feature: Cloud, Storage
exl-id: 480cb33b-ac83-441d-946e-5b4de09ad84e
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '714'
ht-degree: 0%

---

# Gestione dello spazio su disco

Puoi trovare la capacità di archiviazione totale per il progetto Cloud nel tuo contratto per l&#39;infrastruttura cloud di Adobe Commerce e nella [pagina dell&#39;account](https://accounts.magento.cloud/user). Ogni scheda del progetto nel tuo account mostra il numero di _ambienti_, la capacità di _archiviazione_ in GB e il numero di _utenti_. In alternativa, puoi utilizzare il seguente comando Cloud:

```bash
magento-cloud subscription:info | grep storage
```

Risposta di esempio:

```
| storage              | 51200
```

Quando un ambiente di produzione o di staging Pro raggiunge o supera il 95% della capacità di storage, lo strumento di monitoraggio dell’infrastruttura cloud attiva un avviso di supporto che notifica un aumento automatico della capacità di storage.

Esempio di notifica:

>[!BEGINSHADEBOX]

_&quot;Il monitoraggio ha rilevato che l&#39;archiviazione dei file nel cluster (project-id-environment) è quasi piena. L&#39;utilizzo del disco è attualmente a livelli critici con meno di 1 GiB rimanente. È in corso l&#39;aggiornamento del volume di storage condiviso da 60 GiB a 70 GiB per mantenere i servizi operativi. Esaminare l&#39;utilizzo dei file di produzione e di gestione temporanea per verificare se è possibile liberare spazio.&quot;_

>[!ENDSHADEBOX]

>[!TIP]
>
>Si consiglia di monitorare regolarmente la capacità di storage e mantenerla ben al di sotto del 90% per evitare questi aumenti automatici. Una volta allocato, non è possibile ripristinare l&#39;aumento dello storage per lo staging e la produzione Pro.

## Verifica l’ambiente di integrazione

È possibile verificare l&#39;utilizzo dello spazio su disco per l&#39;ambiente di integrazione utilizzando l&#39;interfaccia CLI di `magento-cloud`.

**Per verificare l&#39;utilizzo approssimativo dello spazio su disco**:

```bash
magento-cloud db:size
```

Risposta di esempio:

```
Checking database service mysql...

+----------------+-----------------+--------+
| Allocated disk | Estimated usage | % used |
+----------------+-----------------+--------+
| 2.0 GiB        | 193.3 MiB       | ~ 9%   |
+----------------+-----------------+--------+
```

Tutti i supporti condividono un disco. È possibile verificare l&#39;utilizzo dello spazio su disco per i mount utilizzando l&#39;interfaccia della riga di comando `magento-cloud`.

**Per verificare l&#39;utilizzo approssimativo dello spazio su disco per le installazioni**:

```bash
magento-cloud mount:size
```

Risposta di esempio:

```
Checking disk usage for all mounts on <project>-<environment>-mymagento@ssh.us.magento.cloud...

+------------+-----------+---------+-----------+-----------+--------+
| Mount(s)   | Size(s)   | Disk    | Used      | Available | % Used |
+------------+-----------+---------+-----------+-----------+--------+
| app/etc    | 184 KiB   | 1.9 GiB | 481.3 MiB | 1.4 GiB   | 24.7%  |
| pub/media  | 128 KiB   |         |           |           |        |
| pub/static | 158.2 MiB |         |           |           |        |
| var        | 316.7 MiB |         |           |           |        |
+------------+-----------+---------+-----------+-----------+--------+
```

## Controlla cluster dedicati

Per gli ambienti Pro Staging e Production, è possibile verificare l&#39;utilizzo dello spazio su disco in ogni ambiente utilizzando il comando `disk free`, che indica la quantità di spazio su disco utilizzata dal file system. Per accedere a un ambiente remoto, è necessario utilizzare SSH.

```bash
df -h
```

L&#39;opzione `-h` visualizza il report in un formato leggibile (KB, MB o GB).

Nella seguente risposta di esempio, il mount `/mnt/shared` mostra lo spazio su disco per il supporto e il mount `/data/mysql/` mostra lo spazio su disco per il database:

```
Filesystem                                    Size  Used Avail Use% Mounted on
udev                                           16G     0   16G   0% /dev
tmpfs                                         3.2G  9.1M  3.2G   1% /run
/dev/xvda1                                     59G  8.9G   48G  16% /
tmpfs                                          16G   36K   16G   1% /dev/shm
tmpfs                                         5.0M     0  5.0M   0% /run/lock
tmpfs                                          16G     0   16G   0% /sys/fs/cgroup
/dev/xvdj                                     9.8G  2.3G  7.6G  23% /data/mysql
/dev/xvdi                                     9.8G  491M  9.3G   5% /data/exports
192.168.5.5:/shared                           9.8G  591M  9.3G   6% /mnt/shared
/dev/loop0                                     91M   91M     0 100% /app/project
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
192.168.5.5:/shared/project/app/etc     9.8G  591M  9.3G   6% /app/project/app/etc
192.168.5.5:/shared/project/pub/media   9.8G  591M  9.3G   6% /app/project/pub/media
192.168.5.5:/shared/project/pub/static  9.8G  591M  9.3G   6% /app/project/pub/static
```

Puoi limitare la risposta specificando una directory. Ad esempio:

```bash
df -h var/
```

Risposta di esempio:

```
Filesystem                                    Size  Used Avail Use% Mounted on
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
```

## Alloca spazio su disco

Due [file di configurazione](../environment/overview.md) controllano l&#39;allocazione dello spazio su disco negli ambienti Cloud: il file `.magento.app.yaml` e il file `.magento/services.yaml`. Ogni file contiene la proprietà `disk`, che definisce il valore della dimensione del disco in MB per la rispettiva configurazione. È possibile modificare l&#39;allocazione dello spazio su disco solo nell&#39;integrazione Pro e negli ambienti Starter.

>[!IMPORTANT]
>
>Per gli ambienti Pro Production e Staging, è necessario [inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per modificare l&#39;allocazione dello spazio su disco. Poiché è possibile aumentare le dimensioni degli ambienti di produzione e staging di Pro solo a determinati intervalli, a seconda dell&#39;utilizzo attuale dello spazio su disco, il supporto potrebbe consigliare di aumentare l&#39;allocazione dello spazio su disco di almeno 10 GB. Una volta allocato, non è possibile ripristinare l&#39;aumento dello storage per lo staging e la produzione Pro. Impossibile riallocare o ridistribuire lo storage tra le risorse. Per aggiungere più spazio di archiviazione file, ridurre lo spazio su disco allocato per MySQL.

### Spazio su disco dell&#39;applicazione

Il file `.magento.app.yaml` controlla lo [spazio su disco permanente](../application/properties.md#disk) disponibile per l&#39;applicazione.

**Per aumentare lo spazio su disco per l&#39;applicazione**:

1. Nell&#39;ambiente di sviluppo locale, aprire il file di configurazione `.magento.app.yaml`.

1. Impostare un nuovo valore per la proprietà `disk` (in MB).

   ```yaml
   disk: <value-mb>
   ```

1. Salva le modifiche nel file.

1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add .magento.app.yaml && git commit -m "Increase disk space for application" && git push origin <branch-name>
   ```

   Le modifiche diventano effettive dopo il push del file YAML aggiornato nell&#39;ambiente remoto.

### Spazio su disco del servizio

Il file `.magento/services.yaml` controlla lo spazio su disco disponibile per ogni servizio, ad esempio MySQL e Redis.

**Per aumentare lo spazio su disco per un servizio**:

1. Nell&#39;ambiente di sviluppo locale, aprire il file di configurazione `.magento/services.yaml`.

1. Aggiungi o trova un servizio nel file. Per ulteriori informazioni sulla configurazione dei servizi, consulta [](../services/services-yaml.md).

1. Impostare un nuovo valore per la proprietà del disco (in MB).

   ```yaml
   <name>:
       type: <service-name>:<service-version>
       disk: <value-mb>
   ```

1. Salva le modifiche nel file.

1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add .magento/services.yaml && git commit -m "Increase disk space for service" && git push origin <branch-name>
   ```

   Le modifiche diventano effettive dopo il push del file YAML aggiornato nell&#39;ambiente remoto.

## Monitorare lo spazio su disco

Negli ambienti di produzione Pro, è possibile monitorare lo spazio su disco e altri indicatori di prestazioni utilizzando gli avvisi gestiti per i criteri di avviso di Adobe Commerce per New Relic. Per ulteriori dettagli, vedere [Monitorare le prestazioni con avvisi gestiti](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts). Per ulteriori informazioni, vedere [Best practice per risolvere i problemi di prestazioni del database](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/resolve-database-performance-issues.html).

## Nessuno spazio disponibile

La cache di build può crescere nel tempo. Se viene visualizzato un avviso con lo stato `No space left on device`, provare a cancellare la cache di compilazione e a ridistribuire:

```bash
magento-cloud project:clear-build-cache
```
