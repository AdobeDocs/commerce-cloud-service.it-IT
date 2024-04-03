---
title: Gestione dello spazio su disco
description: Scopri come gestire lo spazio su disco utilizzando l’interfaccia della riga di comando.
feature: Cloud, Storage
exl-id: 480cb33b-ac83-441d-946e-5b4de09ad84e
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 0%

---

# Gestione dello spazio su disco

Puoi trovare la capacità di archiviazione totale per il progetto Cloud nel contratto per l’infrastruttura cloud di Adobe Commerce e nel [pagina account](https://accounts.magento.cloud/user). Ogni scheda del progetto nel tuo account mostra il numero di _ambienti_, il _archiviazione_ capacità in GB e il numero di _utenti_. In alternativa, puoi utilizzare il seguente comando Cloud:

```bash
magento-cloud subscription:info | grep storage
```

Risposta di esempio:

```terminal
| storage              | 51200
```

Quando un ambiente di produzione o di staging Pro raggiunge o supera il 95% della capacità di storage, lo strumento di monitoraggio dell’infrastruttura cloud attiva un avviso di supporto che notifica un aumento automatico della capacità di storage.

Esempio di notifica:

>[!BEGINSHADEBOX]

_&quot;Il monitoraggio ha rilevato che l&#39;archiviazione dei file nel cluster (project-id-environment) è quasi completa. L&#39;utilizzo del disco è attualmente a livelli critici con meno di 1 GiB rimanente. È in corso l&#39;aggiornamento del volume di storage condiviso da 60 GiB a 70 GiB per mantenere i servizi operativi. Osserva l’utilizzo dei file di produzione e di staging per scoprire se è possibile liberare spazio.&quot;_

>[!ENDSHADEBOX]

>[!TIP]
>
>Si consiglia di monitorare regolarmente la capacità di storage e mantenerla ben al di sotto del 90% per evitare questi aumenti automatici. Una volta allocato, non è possibile ripristinare l&#39;aumento dello storage per lo staging e la produzione Pro.

## Verifica l’ambiente di integrazione

È possibile verificare l’utilizzo dello spazio su disco per l’ambiente di integrazione utilizzando `magento-cloud` CLI

**Per verificare l&#39;utilizzo approssimativo dello spazio su disco**:

```bash
magento-cloud db:size
```

Risposta di esempio:

```terminal
Checking database service mysql...

+----------------+-----------------+--------+
| Allocated disk | Estimated usage | % used |
+----------------+-----------------+--------+
| 2.0 GiB        | 193.3 MiB       | ~ 9%   |
+----------------+-----------------+--------+
```

Tutti i supporti condividono un disco. È possibile controllare l&#39;utilizzo dello spazio su disco per i mount utilizzando `magento-cloud` CLI

**Per verificare l&#39;utilizzo approssimativo dello spazio su disco per le installazioni**:

```bash
magento-cloud mount:size
```

Risposta di esempio:

```terminal
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

Per gli ambienti Pro Staging e Production, è possibile verificare l&#39;utilizzo dello spazio su disco in ogni ambiente utilizzando `disk free` che indica la quantità di spazio su disco utilizzata dal file system. Per accedere a un ambiente remoto, è necessario utilizzare SSH.

```bash
df -h
```

Il `-h` Il report viene visualizzato in un formato leggibile (KB, MB o GB).

Nella seguente risposta di esempio, `/mnt/shared` mount mostra lo spazio su disco per i supporti e `/data/mysql/` mount mostra lo spazio su disco per il database:

```terminal
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

```terminal
Filesystem                                    Size  Used Avail Use% Mounted on
192.168.5.5:/shared/project/var         9.8G  591M  9.3G   6% /app/project/var
```

## Alloca spazio su disco

Due [file di configurazione](../environment/overview.md) controllare l’allocazione dello spazio su disco negli ambienti Cloud: `.magento.app.yaml` file e `.magento/services.yaml` file. Ogni file contiene `disk` che definisce il valore di dimensione del disco in MB per la rispettiva configurazione. È possibile modificare l&#39;allocazione dello spazio su disco solo nell&#39;integrazione Pro e negli ambienti Starter.

>[!IMPORTANT]
>
>Per gli ambienti di produzione e staging professionali, è necessario [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per modificare l&#39;allocazione dello spazio su disco. Poiché è possibile aumentare le dimensioni degli ambienti di produzione e staging di Pro solo a determinati intervalli, a seconda dell&#39;utilizzo attuale dello spazio su disco, il supporto potrebbe consigliare di aumentare l&#39;allocazione dello spazio su disco di almeno 10 GB. Una volta allocato, non è possibile ripristinare l&#39;aumento dello storage per lo staging e la produzione Pro.

### Spazio su disco dell&#39;applicazione

Il `.magento.app.yaml` il file controlla [spazio su disco permanente](../application/properties.md#disk) disponibile per l&#39;applicazione.

**Per aumentare lo spazio su disco per l&#39;applicazione**:

1. Nell’ambiente di sviluppo locale, apri la sezione `.magento.app.yaml` file di configurazione.

1. Imposta un nuovo valore per `disk` (in MB).

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

Il `.magento/services.yaml` Il file controlla lo spazio su disco disponibile per ciascun servizio, ad esempio MySQL e Redis.

**Per aumentare lo spazio su disco per un servizio**:

1. Nell’ambiente di sviluppo locale, apri la sezione `.magento/services.yaml` file di configurazione.

1. Aggiungi o trova un servizio nel file. Consulta [ulteriori informazioni sulla configurazione dei servizi](../services/services-yaml.md).

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

Negli ambienti di produzione Pro, è possibile monitorare lo spazio su disco e altri indicatori di prestazioni utilizzando gli avvisi gestiti per i criteri di avviso di Adobe Commerce per New Relic. Per ulteriori informazioni, consulta [Monitorare le prestazioni con avvisi gestiti](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts). Per maggiori informazioni, consulta [Best practice per risolvere i problemi di prestazioni del database](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/resolve-database-performance-issues.html).

## Nessuno spazio disponibile

La cache di build può crescere nel tempo. Se ricevi un avviso che indica che `No space left on device`, prova a cancellare la cache di build e a ridistribuire:

```bash
magento-cloud project:clear-build-cache
```
