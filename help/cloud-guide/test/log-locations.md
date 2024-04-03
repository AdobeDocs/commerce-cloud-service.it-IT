---
title: Visualizzare e gestire i registri
description: Comprendi i tipi di file di registro disponibili nell’infrastruttura cloud e dove trovarli.
last-substantial-update: 2023-05-23T00:00:00Z
exl-id: d7f63dab-23bf-4b95-b58c-3ef9b46979d4
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1024'
ht-degree: 0%

---

# Visualizzare e gestire i registri

I registri per i progetti di infrastruttura cloud di Adobe Commerce sono utili per la risoluzione dei problemi relativi a [generare e distribuire hook](../application/hooks-property.md), servizi cloud e l’applicazione Adobe Commerce.

È possibile visualizzare i registri dal file system, il [!DNL Cloud Console]e `magento-cloud` CLI

- **File system**- La `/var/log` La directory di sistema contiene i registri di tutti gli ambienti. Il `var/log/` la directory contiene registri specifici per l’app specifici per un particolare ambiente. Queste directory non sono condivise tra i nodi di un cluster. Negli ambienti di produzione e staging di Pro, è necessario controllare i registri su ciascun nodo.

- **[!DNL Cloud Console]**: nell’ambiente è possibile visualizzare le informazioni sui registri di build, distribuzione e post-distribuzione _messaggi_ elenco.

- **Cloud CLI**: puoi visualizzare i registri dell’ambiente locale utilizzando `magento-cloud log` registri di comandi o ambienti remoti utilizzando `magento-cloud ssh` comando.

## Posizioni del registro

I registri di sistema sono archiviati nei seguenti percorsi:

- Integrazione: `/var/log/<log-name>.log`
- Staging Pro: `/var/log/platform/<project-ID>_stg/<log-name>.log`
- Produzione Pro: `/var/log/platform/<project-ID>/<log-name>.log`

Il valore di `<project-ID>` dipende dal progetto e dal fatto che l’ambiente sia di staging o produzione. Ad esempio, con un ID progetto di `yw1unoukjcawe`, l’utente dell’ambiente di staging è `yw1unoukjcawe_stg` e l’utente dell’ambiente di produzione è `yw1unoukjcawe`.

Utilizzando tale esempio, il registro di distribuzione è: `/var/log/platform/yw1unoukjcawe_stg/deploy.log`

### Visualizzare i registri dell’ambiente remoto

La maggior parte dei registri include eventi che si verificano nell’ambiente remoto. Per Pro, ci sono più nodi e ogni nodo ha registri univoci. Utilizzare quanto segue per visualizzare un elenco di tutti gli host:

```bash
magento-cloud ssh -p <project-ID> -e <environment-ID> --all
```

Risposta di esempio:

```terminal
1.ent-project-environment-id@ssh.region.magento.cloud
2.ent-project-environment-id@ssh.region.magento.cloud
3.ent-project-environment-id@ssh.region.magento.cloud
```

**Per visualizzare un elenco di registri dell’ambiente remoto**:

```bash
magento-cloud ssh -e <environment-ID> "ls var/log"
```

Esempio per Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "ls var/log | grep error"
```

**Per visualizzare un registro remoto**:

```bash
magento-cloud ssh -e <environment-ID> "cat var/log/cron.log"
```

Esempio per Pro:

```bash
ssh 1.ent-project-environment-id@ssh.region.magento.cloud "cat var/log/cron.log"
```

>[!TIP]
>
>Per gli ambienti Pro, la rotazione, la compressione e la rimozione automatica del registro sono attivate per i file di registro con un nome di file fisso. Ogni tipo di file di registro ha un pattern e una durata di rotazione. Gli ambienti Starter non dispongono della rotazione del registro. Per informazioni dettagliate sulla rotazione dei registri dell’ambiente e sulla durata dei registri compressi, consulta: `/etc/logrotate.conf` e `/etc/logrotate.d/<various>`

## Creare e distribuire i registri

Dopo aver inviato le modifiche all’ambiente, puoi rivedere la registrazione da ogni hook in `var/log/cloud.log` file. Il registro contiene messaggi di avvio e di arresto per ogni hook. Nell’esempio seguente, i messaggi sono &quot;`Starting post-deploy.`&quot; e &quot;`Post-deploy is complete.`&quot;

Controlla i timestamp sulle voci di registro, verifica e individua i registri per una distribuzione specifica. Di seguito è riportato un esempio sintetico di output di registro che è possibile utilizzare per la risoluzione dei problemi:

```terminal
Re-deploying environment project-integration-ID
  Executing post deploy hook for service `mymagento`
    [2019-01-03 19:44:11] NOTICE: Starting post-deploy.
    [2019-01-03 19:44:11] INFO: Validating configuration
    [2019-01-03 19:44:11] INFO: End of validation
    [2019-01-03 19:44:11] INFO: Enable cron
    [2019-01-03 19:44:11] INFO: Create backup of important files.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/env.php.bak for /app/app/etc/env.php was created.
    [2019-01-03 19:44:11] INFO: Backup /app/app/etc/config.php.bak for /app/app/etc/config.php was created.
    [2019-01-03 19:44:11] INFO: php ./bin/magento cache:flush --ansi --no-interaction
    [2019-01-03 19:44:32] INFO: Warming up failed: http://integration-id-project.us.magentosite.cloud/
    [2019-01-03 19:44:32] NOTICE: Post-deploy is complete.
```

>[!TIP]
>
>Quando configuri l’ambiente Cloud, puoi impostare [Slack basato su registro e notifiche e-mail](../environment/set-up-notifications.md) per azioni di build e distribuzione.

I seguenti registri hanno una posizione comune per tutti i progetti Cloud:

- **Registro di distribuzione**: `var/log/cloud.log`
- **Registro degli errori dell’ultima distribuzione**: `var/log/cloud.error.log`
- **Registro di debug**: `var/log/debug.log`
- **Registro eccezioni**: `var/log/exception.log`
- **Registro di sistema**: `var/log/system.log`
- **Registro di supporto**: `var/log/support_report.log`
- **Rapporti**: `var/report/`

Anche se `cloud.log` contiene feedback da ogni fase del processo di distribuzione; i registri creati dall’hook di distribuzione sono univoci per ogni ambiente. Il registro di distribuzione specifico dell’ambiente si trova nelle seguenti directory:

- **Integrazione Starter e Pro**: `/var/log/deploy.log`
- **Pro Staging**: `/var/log/platform/<project-ID>_stg/deploy.log`
- **Produzione Pro**: `/var/log/platform/<project-ID>/deploy.log`

### Distribuisci registro

Il registro di ciascuna distribuzione si concatena al `deploy.log` file. L’esempio seguente stampa il registro di distribuzione dell’ambiente corrente nel terminale:

```bash
magento-cloud log -e <environment-ID> deploy
```

Risposta di esempio:

```terminal
Reading log file projectID-branchname-ID--mymagento@ssh.zone.magento.cloud:/var/log/'deploy.log'

[2023-04-24 18:58:03.080678] Launching command 'b'php ./vendor/bin/ece-tools run scenario/deploy.xml\n''.

[2023-04-24T18:58:04.129888+00:00] INFO: Starting scenario(s): scenario/deploy.xml (magento/ece-tools version: 2002.1.14, magento/magento2-base version: 2.4.6)
[2023-04-24T18:58:04.364714+00:00] NOTICE: Starting pre-deploy.
...
```

{{scd-timing-warning}}

### Registro errori

I messaggi di errore e di avviso generati durante il processo di distribuzione vengono scritti in entrambi `var/log/cloud.log` e `var/log/cloud.error.log` file. Il file di registro degli errori di Cloud contiene solo errori e avvisi dell’ultima distribuzione. Un file vuoto indica una distribuzione riuscita senza errori.

È possibile visualizzare il file di registro utilizzando [SSH CLI cloud](#view-remote-environment-logs), oppure utilizzare ECE-Tools per visualizzare gli errori con suggerimenti:

```bash
magento-cloud ssh -e <environment-ID> "./vendor/bin/ece-tools error:show"
```

Risposta di esempio:

```terminal
errorCode: 1001
stage: build
step: validate-config
suggestion: Please run the following commands:
1. bin/magento module:enable --all
2. git add -f app/etc/config.php
3. git commit -m 'Adding config.php'
4. git push
title: File app/etc/config.php does not exist
type: warning
---------------

errorCode: 1006
stage: build
step: validate-config
suggestion: Your application does not have the "post_deploy" hook enabled.
  In order to minimize downtime, add the following to ".magento.app.yaml":
  hooks:
      post_deploy: |
          php ./vendor/bin/ece-tools run scenario/post-deploy.xml
title: The configured state is not ideal
type: warning
```

La maggior parte dei messaggi di errore contiene una descrizione e un’azione suggerita. Utilizza il [Riferimento al messaggio di errore per gli strumenti ECE](../dev-tools/error-reference.md) per ulteriori informazioni, vedere il codice di errore. Per ulteriori informazioni, utilizzare [Risoluzione dei problemi di distribuzione di Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html).

## Registri applicazioni

Analogamente ai registri di distribuzione, i registri dell’applicazione sono univoci per ciascun ambiente:

| File di registro | Integrazione Starter e Pro | Descrizione |
| ------------------- | --------------------------- | ------------------------------------------------- |
| **Distribuisci registro** | `/var/log/deploy.log` | Attività da [implementa hook](../application/hooks-property.md). |
| **Registro post-distribuzione** | `/var/log/post_deploy.log` | Attività da [hook post-distribuzione](../application/hooks-property.md). |
| **Registro celle** | `/var/log/cron.log` | Output da processi cron. |
| **Registro accessi Nginx** | `/var/log/access.log` | All’avvio di Nginx, si verificano errori HTTP per le directory mancanti e i tipi di file esclusi. |
| **Registro errori Nginx** | `/var/log/error.log` | Messaggi di avvio utili per il debug degli errori di configurazione associati a Nginx. |
| **Registro degli accessi PHP** | `/var/log/php.access.log` | Richieste al servizio PHP. |
| **Registro FPM PHP** | `/var/log/app.log` | |

Per gli ambienti di staging e produzione Pro, i registri di distribuzione, post-distribuzione e Cron sono disponibili solo sul primo nodo del cluster:

| File di registro | Pro Staging | Produzione Pro |
| ------------------- | --------------------------------------------------- | ----------------------------------------------- |
| **Distribuisci registro** | Solo primo nodo:<br>`/var/log/platform/<project-ID>_stg/deploy.log` | Solo primo nodo:<br>`/var/log/platform/<project-ID>/deploy.log` |
| **Registro post-distribuzione** | Solo primo nodo:<br>`/var/log/platform/<project-ID>_stg/post_deploy.log` | Solo primo nodo:<br>`/var/log/platform/<project-ID>/post_deploy.log` |
| **Registro celle** | Solo primo nodo:<br>`/var/log/platform/<project-ID>_stg/cron.log` | Solo primo nodo:<br>`/var/log/platform/<project-ID>/cron.log` |
| **Registro accessi Nginx** | `/var/log/platform/<project-ID>_stg/access.log` | `/var/log/platform/<project-ID>/access.log` |
| **Registro errori Nginx** | `/var/log/platform/<project-ID>_stg/error.log` | `/var/log/platform/<project-ID>/error.log` |
| **Registro degli accessi PHP** | `/var/log/platform/<project-ID>_stg/php.access.log` | `/var/log/platform/<project-ID>/php.access.log` |
| **Registro FPM PHP** | `/var/log/platform/<project-ID>_stg/php5-fpm.log` | `/var/log/platform/<project-ID>/php5-fpm.log` |

### File di registro archiviati

I registri delle applicazioni vengono compressi e archiviati una volta al giorno e conservati per un anno. I registri compressi vengono denominati utilizzando un ID univoco che corrisponde al `Number of Days Ago + 1`. Ad esempio, negli ambienti di produzione Pro viene memorizzato un registro di accesso PHP per 21 giorni nel passato, denominato come segue:

```terminal
/var/log/platform/<project-ID>/php.access.log.22.gz
```

I file di registro archiviati vengono sempre memorizzati nella directory in cui si trovava il file originale prima della compressione.

>[!NOTE]
>
>**Distribuisci** e **Post-distribuzione** i file di registro non vengono ruotati e archiviati. L&#39;intera cronologia della distribuzione viene scritta all&#39;interno di tali file di registro.

## Registri del servizio

Poiché ogni servizio viene eseguito in un contenitore separato, i registri del servizio non sono disponibili nell’ambiente di integrazione. Adobe Commerce su infrastruttura cloud fornisce l’accesso al contenitore del server web solo nell’ambiente di integrazione. Le seguenti posizioni dei log di servizio sono per gli ambienti Pro Production e Staging:

- **Registro Redis**: `/var/log/platform/<project-ID>_stg/redis-server-<project-ID>_stg.log`
- **Registro Elasticsearch**: `/var/log/elasticsearch/elasticsearch.log`
- **Registro raccolta oggetti inattivi Java**: `/var/log/elasticsearch/gc.log`
- **Registro posta**: `/var/log/mail.log`
- **Registro errori MySQL**: `/var/log/mysql/mysql-error.log`
- **Registro lento MySQL**: `/var/log/mysql/mysql-slow.log`
- **Registro RabbitMQ**: `/var/log/rabbitmq/rabbit@host1.log`

I registri del servizio vengono archiviati e salvati per diversi periodi di tempo, a seconda del tipo di registro. Ad esempio, i registri MySQL hanno una durata più breve, che viene rimossa dopo sette giorni.

>[!TIP]
>
>Le posizioni dei file di registro nell’architettura ridimensionata dipendono dal tipo di nodo. Consulta [Registra le posizioni nell’architettura scalata](../architecture/scaled-architecture.md#log-locations) argomento.

## Registra i dati per Pro Production e Staging

Negli ambienti di produzione e staging Pro, utilizza [Gestione registro New Relic](../monitor/log-management.md) integrato con il tuo progetto per gestire i dati di registro aggregati da tutti i registri associati al progetto Adobe Commerce on cloud infrastructure.

L’applicazione New Relic Logs fornisce un dashboard di gestione dei registri centralizzato per la risoluzione dei problemi e il monitoraggio di Adobe Commerce negli ambienti di produzione e staging dell’infrastruttura cloud. La dashboard consente inoltre di accedere ai dati di registro per i servizi Fastly CDN, Image Optimization e Web Application Firewall (WAF). Consulta [Servizi New Relic](../monitor/new-relic-service.md).
