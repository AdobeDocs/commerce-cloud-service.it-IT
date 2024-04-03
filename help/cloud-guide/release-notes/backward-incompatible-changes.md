---
title: Modifiche non compatibili con le versioni precedenti
description: Scopri la compatibilità con le versioni precedenti durante l’aggiornamento dei progetti Cloud esistenti.
feature: Cloud, Release Notes
recommendations: noDisplay, catalog
exl-id: 9847c565-6d59-429a-9593-c2eba5cf2da4
source-git-commit: f9edcc85c14354a2eddacbc5219557e107a367cb
workflow-type: tm+mt
source-wordcount: '791'
ht-degree: 0%

---

# Modifiche non compatibili con le versioni precedenti

Per modifiche non compatibili con le versioni precedenti potrebbe essere necessario adeguare la configurazione e i processi cloud per i progetti Cloud esistenti quando si esegue l’aggiornamento alla versione più recente del `ece-tools` pacchetto o altra suite di strumenti cloud per pacchetti Commerce.

## Modifiche apportate a `ece-tools` pacchetto

Alcune funzionalità precedentemente incluse nel `ece-tools` pacchetto ora viene fornito in pacchetti separati. Questi pacchetti sono dipendenze del compositore per `ece-tools`, che vengono installati e aggiornati automaticamente quando si installano o si aggiornano gli strumenti ece.

La nuova architettura non dovrebbe influire sui processi di installazione o aggiornamento. Tuttavia, potrebbe essere necessario modificare alcuni processi e sintassi dei comandi quando si lavora con il progetto Adobe Commerce su infrastruttura cloud. Per ulteriori informazioni, esaminare le seguenti informazioni sulle modifiche non compatibili con le versioni precedenti e [Note sulla versione della suite di strumenti cloud](cloud-tools-suite.md).

### Modifiche ai requisiti di versione del servizio

Abbiamo modificato il requisito della versione PHP minima da 7.0.x a 7.1.x per i progetti Cloud che utilizzano `ece-tools` v2002.1.0 e versioni successive. Se la configurazione dell&#39;ambiente specifica PHP 7.0, aggiornare [configurazione php](../application/php-settings.md) nel `.magento.app.yaml` file.

>[!WARNING]
>
>A causa della modifica del requisito della versione PHP, `ece-tools` 2002.1.0 supporta solo i progetti Adobe Commerce on cloud infrastructure che eseguono Adobe Commerce 2.1.15 o versioni successive. Se il progetto utilizza una versione precedente, è necessario [aggiornamento](../development/commerce-version.md) prima dell’aggiornamento a `ece-tools` 2002.1.

### Modifiche alla configurazione dell’ambiente

La tabella seguente fornisce informazioni sulle variabili di ambiente e su altri file di configurazione dell’ambiente rimossi o dichiarati obsoleti in `ece-tools` v2002.1.0.

| Elemento | Sostituto |
| -------- | ----------- |
| `SCD_EXCLUDE_THEMES` variabile | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| `STATIC_CONTENT_THREADS` variabile | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| `DO_DEPLOY_STATIC_CONTENT` variabile | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| `STATIC_CONTENT_SYMLINK` variabile | Nessuno. Ora la build crea sempre un collegamento simbolico alla directory di contenuto statico `pub/static`. |
| `build_options.ini` file | Utilizza il [`.magento.env.yaml`](../application/configure-app-yaml.md) per configurare le variabili di ambiente per gestire le azioni di generazione e distribuzione in tutti gli ambienti.<p>Se crei un ambiente Cloud che include `build_options.ini` file, la build non riesce. |

### Modifiche al comando CLI

Nella tabella seguente sono riepilogate le modifiche apportate ai comandi CLI in ECE-Tools v2002.1.0 che potrebbero richiedere l&#39;aggiornamento di comandi o script.

| Comando | Sostituto |
|-------- | ----------- |
| `m2-ece-build` | `vendor/bin/ece-tools build` |
| `m2-ece-deploy` | `vendor/bin/ece-tools deploy` |
| `m2-ece-scd-dump` | `vendor/bin/ece-tools config:dump` |
| `vendor/bin/ece-tools patch` | `vendor/bin/ece-patches apply` |
| `vendor/bin/ece-tools docker:build` | `vendor/bin/ece-docker build:compose` |
| `vendor/bin/ece-tools docker:config:convert` | `vendor/bin/ece-docker  image:generate:php` |

Nelle versioni precedenti di ECE-Tools, era possibile utilizzare `m2-ece-build` e `m2-ece-deploy` comandi per configurare gli hook di distribuzione in `.magento.app.yaml` file. Quando si esegue l&#39;aggiornamento alla versione v2002.1.0, controllare `hooks` configurazione in `.magento.app.yaml` per i comandi obsoleti e sostituirli se necessario.

## Modifiche alle patch cloud

- **Rimuovi le patch scaricate**-Il `magento/magento-cloud-patches` il pacchetto racchiude tutte le patch disponibili nella [download di software](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html) e le applica automaticamente quando distribuisci sul cloud. Per evitare conflitti di patch dopo l&#39;aggiornamento a ECE-Tools 2002.1.0 o versione successiva, rimuovere tutte le patch fornite dall&#39;Adobe scaricate e aggiunte manualmente al progetto.

- **Aggiornamento del comando applica patch**-È stato spostato il comando per l&#39;applicazione delle patch dal `vendor/bin/ece-tools` alla directory `vendor/bin/ece-patches` directory. Se si utilizza questo comando per applicare le patch manualmente, utilizzare il nuovo percorso.

  > Applicare manualmente le patch

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Modifiche a Cloud Docker

- **Il requisito minimo per la versione PHP è ora PHP 7.1**-Se nell’host Cloud Docker for Commerce è in esecuzione una versione precedente, effettua l’aggiornamento a PHP v7.1 o versione successiva.

- **Modifiche al comando Cloud Docker per Commerce**-

   - **Aggiornamento dei comandi di Cloud Docker per Commerce per le operazioni di build Docker**: i comandi di Cloud Docker for Commerce sono stati spostati dalla sezione `vendor/bin/ece-tools` alla directory `vendor/bin/ece-docker` directory. Aggiorna gli script e i comandi per utilizzare il nuovo percorso.

     Dopo l&#39;upgrade a `ece-tools` 2002.1.0, utilizzare il seguente comando per visualizzare `ece-docker` comandi.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Aggiornamento dei comandi di composizione docker cloud**-Il percorso del file di comando è stato rinominato da `./bin/docker` a `./bin/magento-docker`. Aggiorna gli script e i comandi per utilizzare il nuovo percorso.

   - **Il contenitore Cron non è più incluso nella configurazione Docker predefinita**-Ora è necessario aggiungere il `--with-cron` opzione per `ece-docker build:compose` per includere il contenitore Cron nella configurazione dell’ambiente Docker. Consulta [Gestisci processi cron](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/) nel _Cloud Docker per Commerce_ guida.

     Gli script che in precedenza generavano contenitori con processi cron ora non dispongono del contenitore cron.

   - **Utilizzo di contenitori temporanei**-Nelle versioni precedenti, i contenitori creati da `bin/magento-docker` le operazioni di comando non sono state rimosse, pertanto è possibile utilizzarle per altre operazioni. Ora, il `magento-docker` i comandi rimuovono tutti i contenitori creati al termine del comando.

     Se si desidera mantenere un contenitore creato da un&#39;operazione di composizione docker, utilizzare `docker-compose run` al posto di `bin/magento-docker` comando.

   - **Esecuzione degli hook post-distribuzione**-Il `cloud-deploy` Il comando non esegue più gli hook post-distribuzione. Utilizza il nuovo `cloud-post-deploy` comando per eseguire gli hook post-distribuzione dopo la distribuzione. Aggiorna gli script per aggiungere il comando per eseguire hook post-distribuzione.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     In alternativa, se utilizzi `docker-compose` direttamente, esegui il comando `docker-compose run deploy cloud-post-deploy` dopo il comando deploy.

- **Aggiornamento del database**-Il contenitore del database è ora memorizzato in `magento-db` volume Docker persistente. Quando si aggiorna l&#39;ambiente Docker, il database non viene più eliminato automaticamente. Se necessario, utilizzare uno dei seguenti comandi per rimuoverlo manualmente.

   - Rimuovi il `magento-db` contenitore:

     ```bash
     docker volume rm magento-db
     ```

   - Rimuovere tutti i volumi associati durante l&#39;arresto dei contenitori Docker:

     ```bash
     docker-compose down -v
     ```

- **Ignora impostazioni di sincronizzazione file per file di archivio e di backup**-I file di archivio e di backup con le seguenti estensioni non vengono più sincronizzati quando si utilizza docker-sync o mutagen: SQL, GZ, ZIP e BZ2. È possibile ignorare la sincronizzazione predefinita dei file per questi tipi di file rinominando il file in modo che termini con un&#39;estensione diversa. Ad esempio: `synchronize-me.zip-backup`
