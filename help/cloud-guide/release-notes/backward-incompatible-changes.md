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

Le modifiche non compatibili con le versioni precedenti potrebbero richiedere la modifica della configurazione e dei processi cloud per i progetti Cloud esistenti quando si esegue l&#39;aggiornamento alla versione più recente del pacchetto `ece-tools` o di altri pacchetti della suite di strumenti cloud per Commerce.

## Modifiche al pacchetto `ece-tools`

Alcune funzionalità precedentemente incluse nel pacchetto `ece-tools` sono ora disponibili in pacchetti separati. Questi pacchetti sono dipendenze del compositore per `ece-tools`, che vengono installate e aggiornate automaticamente quando si installano o si aggiornano gli strumenti ece.

La nuova architettura non dovrebbe influire sui processi di installazione o aggiornamento. Tuttavia, potrebbe essere necessario modificare alcuni processi e sintassi dei comandi quando si lavora con il progetto Adobe Commerce su infrastruttura cloud. Per informazioni dettagliate, controlla le seguenti informazioni sulle modifiche non compatibili con le versioni precedenti e le [note sulla versione della suite di strumenti cloud](cloud-tools-suite.md).

### Modifiche ai requisiti di versione del servizio

Abbiamo modificato il requisito minimo per la versione PHP da 7.0.x a 7.1.x per i progetti Cloud che utilizzano `ece-tools` v2002.1.0 e versioni successive. Se la configurazione dell&#39;ambiente specifica PHP 7.0, aggiornare la [configurazione php](../application/php-settings.md) nel file `.magento.app.yaml`.

>[!WARNING]
>
>A causa della modifica dei requisiti di versione del PHP, `ece-tools` 2002.1.0 supporta solo i progetti di infrastruttura cloud Adobe Commerce con Adobe Commerce 2.1.15 o versioni successive. Se il progetto utilizza una versione precedente, è necessario [aggiornare](../development/commerce-version.md) prima di eseguire l&#39;aggiornamento a `ece-tools` 2002.1.0.

### Modifiche alla configurazione dell’ambiente

Nella tabella seguente vengono fornite informazioni sulle variabili di ambiente e su altri file di configurazione dell&#39;ambiente rimossi o dichiarati obsoleti in `ece-tools` v2002.1.0.

| Elemento | Sostituto |
| -------- | ----------- |
| Variabile `SCD_EXCLUDE_THEMES` | [`SCD_MATRIX`](../environment/variables-build.md#scd_matrix) |
| Variabile `STATIC_CONTENT_THREADS` | [`SCD_THREADS`](../environment/variables-build.md#scd_threads) |
| Variabile `DO_DEPLOY_STATIC_CONTENT` | [`SKIP_SCD`](../environment/variables-build.md#skip_scd) |
| Variabile `STATIC_CONTENT_SYMLINK` | Nessuno. Ora la build crea sempre un collegamento simbolico alla directory del contenuto statico `pub/static`. |
| `build_options.ini` file | Utilizza il file [`.magento.env.yaml`](../application/configure-app-yaml.md) per configurare le variabili di ambiente per gestire le azioni di generazione e distribuzione in tutti gli ambienti.<p>Se si crea un ambiente Cloud che include il file `build_options.ini`, la compilazione non riesce. |

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

Nelle versioni precedenti di ECE-Tools, era possibile utilizzare i comandi `m2-ece-build` e `m2-ece-deploy` per configurare gli hook di distribuzione nel file `.magento.app.yaml`. Quando si esegue l&#39;aggiornamento a v2002.1.0, verificare la presenza di comandi obsoleti nella configurazione `hooks` nel file `.magento.app.yaml` e sostituirli, se necessario.

## Modifiche alle patch cloud

- **Rimuovi le patch scaricate**-Il pacchetto `magento/magento-cloud-patches` raccoglie tutte le patch disponibili nella pagina [download del software](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/commerce.html) e le applica automaticamente quando si distribuisce nel cloud. Per evitare conflitti di patch dopo l&#39;aggiornamento a ECE-Tools 2002.1.0 o versione successiva, rimuovere tutte le patch fornite dall&#39;Adobe scaricate e aggiunte manualmente al progetto.

- **Aggiornamento del comando Applica patch**. Il comando per l&#39;applicazione delle patch è stato spostato dalla directory `vendor/bin/ece-tools` alla directory `vendor/bin/ece-patches`. Se si utilizza questo comando per applicare le patch manualmente, utilizzare il nuovo percorso.

  > Applicare manualmente le patch

  ```bash
  php ./vendor/bin/ece-patches apply
  ```

## Modifiche a Cloud Docker

- **Il requisito minimo per la versione PHP è ora PHP 7.1**-Se il tuo host Cloud Docker per Commerce esegue una versione precedente, effettua l&#39;aggiornamento a PHP v7.1 o versione successiva.

- **Modifiche al comando Cloud Docker per Commerce**-

   - **Aggiornamento dei comandi di Cloud Docker per Commerce per le operazioni di build Docker**-I comandi di Cloud Docker per Commerce sono stati spostati dalla directory `vendor/bin/ece-tools` alla directory `vendor/bin/ece-docker`. Aggiorna gli script e i comandi per utilizzare il nuovo percorso.

     Dopo l&#39;aggiornamento a `ece-tools` 2002.1.0, utilizzare il comando seguente per visualizzare i comandi `ece-docker` disponibili.

     ```bash
     php ./vendor/bin/ece-docker list
     ```

   - **Aggiornamento dei comandi docker-compose di Cloud** - Il percorso del file di comando è stato rinominato da `./bin/docker` a `./bin/magento-docker`. Aggiorna gli script e i comandi per utilizzare il nuovo percorso.

   - **Il contenitore Cron non è più incluso nella configurazione Docker predefinita**-Ora è necessario aggiungere l&#39;opzione `--with-cron` al comando `ece-docker build:compose` per includere il contenitore Cron nella configurazione dell&#39;ambiente Docker. Vedi [Gestione dei processi cron](https://developer.adobe.com/commerce/cloud-tools/docker/configure/manage-cron-jobs/) nella guida _Cloud Docker per Commerce_.

     Gli script che in precedenza generavano contenitori con processi cron ora non dispongono del contenitore cron.

   - **Utilizzo di contenitori temporanei**-Nelle versioni precedenti, i contenitori creati dalle operazioni di comando `bin/magento-docker` non sono stati rimossi, pertanto è possibile utilizzarli per altre operazioni. Ora i comandi `magento-docker` rimuovono tutti i contenitori creati al termine del comando.

     Se si desidera mantenere un contenitore creato da un&#39;operazione docker-compose, utilizzare il comando `docker-compose run` anziché il comando `bin/magento-docker`.

   - **Esecuzione degli hook post-distribuzione**-Il comando `cloud-deploy` non esegue più gli hook post-distribuzione. Utilizza il nuovo comando `cloud-post-deploy` per eseguire hook post-distribuzione dopo la distribuzione. Aggiorna gli script per aggiungere il comando per eseguire hook post-distribuzione.

     ```shell
     bin/magento-docker ece-deploy
     bin/magento-docker ece-post-deploy
     ```

     In alternativa, se si utilizzano direttamente i comandi `docker-compose`, eseguire il comando `docker-compose run deploy cloud-post-deploy` dopo il comando deploy.

- **Aggiornamento del database**-Il contenitore del database è ora archiviato nel volume Docker permanente `magento-db`. Quando si aggiorna l&#39;ambiente Docker, il database non viene più eliminato automaticamente. Se necessario, utilizzare uno dei seguenti comandi per rimuoverlo manualmente.

   - Rimuovi il contenitore `magento-db`:

     ```bash
     docker volume rm magento-db
     ```

   - Rimuovere tutti i volumi associati durante l&#39;arresto dei contenitori Docker:

     ```bash
     docker-compose down -v
     ```

- **Ignora le impostazioni di sincronizzazione dei file per i file di archivio e di backup**-I file di archivio e di backup con le estensioni seguenti non vengono più sincronizzati quando si utilizza docker-sync o mutagen: SQL, GZ, ZIP e BZ2. È possibile ignorare la sincronizzazione predefinita dei file per questi tipi di file rinominando il file in modo che termini con un&#39;estensione diversa. Esempio: `synchronize-me.zip-backup`
