---
title: Note sulla versione di ECE-Tools
description: Consulta l’elenco degli ultimi miglioramenti apportati al pacchetto ECE-Strumenti.
recommendations: noDisplay, catalog
last-substantial-update: 2024-04-08T00:00:00Z
exl-id: a464b940-c56e-4a7c-9948-559539e25361
source-git-commit: e21f21e34f89b62842bd22c99ff5705f984898e0
workflow-type: tm+mt
source-wordcount: '2905'
ht-degree: 0%

---

# Note sulla versione di ECE-Tools

Il [strumenti ece](https://github.com/magento/ece-tools) Il pacchetto è un insieme di script e strumenti progettati per gestire e distribuire progetti Cloud. Queste note sulla versione descrivono gli ultimi miglioramenti apportati a questo pacchetto, che fa parte del [Suite di strumenti cloud per Commerce](cloud-tools-suite.md).

>[!NOTE]
>
>Consulta [Aggiornamento di ECE-Tools](../dev-tools/update-package.md) per informazioni sull’aggiornamento alla versione più recente di `ece-tools` pacchetto.

Il `ece-tools` Il pacchetto utilizza la seguente sequenza di versioni di rilascio: `200<major>.<minor>.<patch>`

Le note sulla versione includono:

- ![nuova icona](../../assets/new.svg) Nuove funzioni
- ![icona correzione](../../assets/fix.svg) Correzioni e miglioramenti

<!--Add release notes below-->


## v2002.1.18 {#latest}

Data di rilascio: 8 aprile 2024

- ![nuova icona](../../assets/new.svg) **PHP** — Aggiunta del supporto per PHP 8.3.
- ![icona correzione](../../assets/fix.svg) Validator - Convalida fine vita aggiornata.

## v2002.1.17

Data di rilascio: 16 gennaio 2024

- ![icona correzione](../../assets/fix.svg) **Convalida per Elasticsearch e OpenSearch**— È stato corretto il messaggio di convalida che generava un messaggio fuorviante per installare un servizio di ricerca quando LiveSearch era abilitato.<!-- MCLOUD-10167 -->
- ![icona correzione](../../assets/fix.svg) **Avviso di distribuzione**- È stato risolto un problema che causava avvisi di distribuzione relativi alle cartelle non vuote.<!-- MCLOUD-8958 -->

## v2002.1.16

Data di rilascio: 16 ottobre 2023

- ![nuova icona](../../assets/new.svg) **Variabile di ambiente globale ENABLE_WEBHOOKS**- Aggiunto il [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) variabile globale da utilizzare con i webhook Commerce per la connessione a un endpoint esterno, ad esempio un’azione di runtime di App Builder o un sistema di gestione dell’inventario di terze parti.

## v2002.1.15

Data di rilascio: 31 luglio 2023

- ![icona correzione](../../assets/fix.svg) **Codici di errore**- Aggiornamento dello schema del codice di errore e del generatore di documenti del codice di errore.
- ![icona correzione](../../assets/fix.svg) **Convalida per il modello Redis personalizzato**- È stata aggiornata la convalida per i modelli di back-end Redis personalizzati. [Vedi l’esempio per la configurazione della cache](../environment/variables-deploy.md#cache_configuration).
- ![icona correzione](../../assets/fix.svg) **Convalida per RabbitMQ**- Aggiunta del supporto per RabbitMQ 3.11
- ![icona correzione](../../assets/fix.svg) **È stato corretto il collegamento errato.**-È stato corretto il collegamento errato alla documentazione di onboarding nel modello e-mail di benvenuto.

## v2002.1.14

Data di rilascio: 10 marzo 2023

- ![nuova icona](../../assets/new.svg) **PHP**- Aggiunta del supporto per PHP 8.2.
- ![nuova icona](../../assets/new.svg) **Convalida per i servizi**- Aggiornamento delle convalide per i servizi richiesti di Commerce 2.4.6: MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x e RabbitMQ 3.9.
- ![icona correzione](../../assets/fix.svg) **db-dump strumenti ece**- È stato risolto un problema che causava la `db-dump` per arrestarsi prematuramente.

## v2002.1.13

Data di rilascio: 27 ottobre 2022

- ![nuova icona](../../assets/new.svg) **È stato aggiunto il supporto per eventi Adobe I/O per Adobe Commerce**. Gli sviluppatori di estensioni possono ora utilizzare [Eventi Adobe I/O](https://developer.adobe.com/events/docs/) framework per inviare le informazioni sugli eventi Commerce dalle istanze Cloud alle loro applicazioni scritte per [Generatore di app Adobe](https://developer.adobe.com/app-builder/docs/overview/). Adobe I/O di eventi per Adobe Commerce disponibile in Anteprima partner.<!-- CEXT-932 -->
- ![nuova icona](../../assets/new.svg) **Convalida per configurazione OPcache**- Aggiunta di una convalida per verificare la configurazione OPcache per i percorsi esclusi.<!-- MCLOUD-9485 -->
- ![icona correzione](../../assets/fix.svg) **È stato risolto un problema relativo alla configurazione della cache di GraphQL**- Ora ECE-Tools mantiene il GraphQL `id_salt` valore in `cache` configurazione in `app/etc/env.php` file.<!-- MCLOUD-9486 -->

## v2002.1.12

Data di rilascio: 13 settembre 2022

- ![nuova icona](../../assets/new.svg) **Abilita`synchronous_replication`**- Insiemi utensili ECE `synchronous_replication=>true` nel `app/etc/env.php` file quando `MYSQL_USE_SLAVE_CONNECTION` è abilitato. Questa configurazione influisce solo su Commerce 2.4.6+. Consulta la `MYSQL_USE_SLAVE_CONNECTION` descrizione della variabile in [Distribuire le variabili](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![nuova icona](../../assets/new.svg) **OpenSearch**- È stata aggiunta la funzionalità per configurare e impostare `opensearch` per la prossima versione di Adobe Commerce 2.4.6. Consulta [Configura servizio OpenSearch](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

Data di rilascio: 4 agosto 2022

- ![icona correzione](../../assets/fix.svg) **ElasticSuite Validator e OpenSearch**- È stato risolto il problema di convalida del controllo di integrità ElasticSuite quando OpenSearch è installato.<!-- MCLOUD-8767 -->
- ![icona correzione](../../assets/fix.svg) **Tipi restituiti per i comandi di distribuzione**- Sono stati corretti i tipi restituiti per i comandi di distribuzione.<!-- AC-3208 -->
- ![icona correzione](../../assets/fix.svg) **[!DNL RabbitMQ]problema con la nuova installazione di Commerce 2.4.5**- Fisso [!DNL RabbitMQ] problema di arresto anomalo nella nuova installazione di Commerce 2.4.5.<!-- MCLOUD-9059 -->

## v2002.1.10

Data di rilascio: 31 marzo 2022

- ![icona correzione](../../assets/fix.svg) **Elasticsearch 7.10**- Sono state aggiornate le convalide per supportare la versione 7.10 di Elasticsearch.<!-- MCLOUD-8548 -->

## v2002.1.9

Data di rilascio: 10 marzo 2022

- ![nuova icona](../../assets/new.svg) **OpenSearch**—È stato aggiunto il supporto per OpenSearch per Adobe Commerce versioni 2.4.4, 2.4.3-p2 e 2.3.7-p3.<!-- MCLOUD-8296 -->
- ![nuova icona](../../assets/new.svg) **PHP**- Aggiunta del supporto per PHP 8.1.
- ![icona correzione](../../assets/fix.svg) **symfony/process**- Aggiunta la compatibilità con symfony/process ^5.3.<!-- MCLOUD-8283 -->

- ![nuova icona](../../assets/new.svg) **Più processi consumer**- Aggiunto un `multiple_processes` in modo da poter specificare il numero di processi da generare per ogni consumatore. Consulta la `CRON_CONSUMERS_RUNNER` descrizione della variabile in [Distribuire le variabili](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![nuova icona](../../assets/new.svg) **Schema OpenSearch e percorso host completo**- Aggiunta la possibilità di configurare uno schema di Elasticsearch e un percorso host completo.
- ![icona correzione](../../assets/fix.svg) **AWS S3**- È stato modificato il metodo di abilitazione di AWS S3.
- ![icona correzione](../../assets/fix.svg) **Correggi lettore driver_options**- Aggiunta della configurazione driver_options di lettura per la connessione DB da `env.php` file di `ece-tools` per i validatori.<!-- MCLOUD-8420 -->

## v2002.1.8

Data di rilascio: 25 ottobre 2021

- ![nuova icona](../../assets/new.svg) **Percorso dump alternativo**- Aggiunto il `--dump-directory` in modo da poter scegliere una directory di destinazione per un dump del database. Ora `/app/var/dump-main` è la directory di destinazione predefinita per un dump del database. Consulta [Gestione dei backup: scaricamento del database](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![icona correzione](../../assets/fix.svg) **Aggiorna monologo**- È stata aggiornata la versione minima richiesta per `monolog` pacchetto a `^2.3`.<!-- ACMP-1263 -->
- ![icona correzione](../../assets/fix.svg) **Aggiorna Symfony**- Sono state aggiornate le dipendenze di Symfony per renderle compatibili con Adobe Commerce 2.4.4.<!-- ACMP-1533 -->
- ![icona correzione](../../assets/fix.svg) **Caricamento automatico funzionalità/risoluzione**: è stato risolto un problema che si verificava durante la distribuzione in un ambiente di integrazione e la visualizzazione di `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.` errore.<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

Data di rilascio: 29 luglio 2021

**Aggiornamenti della configurazione**—

- ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto per Composer 2.0.<!--MCLOUD-8003-->

- ![icona correzione](../../assets/fix.svg) **Requisiti compositore aggiornati per`symphony/console`**- Aggiornato gli utensili ECE `composer.json` requisiti di versione per `symphony/console` per risolvere un problema che ha causato la `di:compile` i comandi non vanno a buon fine e viene visualizzato il seguente errore: `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![icona correzione](../../assets/fix.svg) Sono stati aggiornati i controlli software di fine del ciclo di vita (`eol.yaml`) per includere l&#39;Elasticsearch 7.9.x.<!--MCLOUD-7938-->

## v2002.1.6

Data di rilascio: 20 aprile 2021

- ![nuova icona](../../assets/new.svg) **Redis credenziali di autenticazione**- Aggiunta la possibilità di leggere le credenziali di autorizzazione Redis dal `relationships` durante la fase di distribuzione.<!--MCLOUD-7694-->

- ![nuova icona](../../assets/new.svg) **Elasticsearch di credenziali di autorizzazione**- Aggiunta la possibilità di leggere le credenziali di autorizzazione dell&#39;Elasticsearch dal `relationships` durante la fase di distribuzione.<!--MCLOUD-7695-->

- ![nuova icona](../../assets/new.svg) **Servizio di archiviazione di sessione dedicato**- Aggiunto `redis-session` come seconda opzione per l’archiviazione della sessione. È possibile utilizzare `redis-session` servizio per memorizzare le informazioni sulla sessione e utilizzare `redis` servizio per la cache per fornire prestazioni migliori.<!--MCLOUD-7698-->

- ![nuova icona](../../assets/new.svg) **Messaggi SPLIT_DB obsoleti**- Aggiunta di avvisi di convalida e messaggi critici per gli elementi obsoleti `SPLIT_DB` opzione per Adobe Commerce 2.4.2 e la sua rimozione in Adobe Commerce 2.5.0.<!--MCLOUD-7806-->

- ![icona correzione](../../assets/fix.svg) **Elasticsearch di versione da relazioni**- È stato corretto Service validator per recuperare la versione corretta dell&#39;Elasticsearch dal `relationships` proprietà in Cloud Docker e ambienti di integrazione.<!--MCLOUD-7572-->

- ![icona correzione](../../assets/fix.svg) **Convalida della porta Redis flessibile**- Redis può ora convalidare la porta in una connessione alla cache personalizzata dal `server` URL. Ad esempio, puoi aggiungere il numero di porta all’URL del server nel modo seguente: `server: 'tcp://rfs-store-simple-page-cache:26379'`. Questo aiuta a evitare errori di convalida in cui il `port` opzione mancante o errata.<!--MCLOUD-7722-->

- ![icona correzione](../../assets/fix.svg) **Aggiornamento ad Adobe Commerce 2.4.2**- È stato risolto il problema che richiedeva l&#39;esecuzione manuale da parte degli utenti `bin/magento setup:upgrade` per rendere operativi i siti dopo l’aggiornamento ad Adobe Commerce 2.4.2.<!--MCLOUD-7776-->

## v2002.1.5

Data di rilascio: 1 febbraio 2021

- ![nuova icona](../../assets/new.svg) **Archiviazione remota**- Aggiunto il `REMOTE_STORAGE` variabile di ambiente per abilitare Progetti cloud per l’archiviazione remota di file multimediali tramite un servizio di archiviazione, come AWS S3. Questa opzione di configurazione fa parte del pacchetto ECE-Tools, ma non è supportata in Adobe Commerce sull’infrastruttura cloud.<!--MCLOUD-7153-->

- ![nuova icona](../../assets/new.svg) **Nuovo `cloud:config:validate` comando**- Comando aggiunto `php vendor/bin/ece-tools cloud:config:validate` per convalidare `.magento.env.yaml` prima di inviare le modifiche all’ambiente cloud remoto.<!--MCLOUD-7120-->

- ![nuova icona](../../assets/new.svg) **Scaricamento della opcache**- Aggiunta del supporto per `opcache.enable_cli` Opzione PHP per eseguire il flushing della OPcache prima di eseguire l’hook di distribuzione. Questa configurazione reimposta la configurazione della cache per garantire che le impostazioni di configurazione correnti vengano applicate a ogni distribuzione.<!--MCLOUD-7015-->

- ![nuova icona](../../assets/new.svg) **Convalida di Aurora DB**- La convalida del servizio di database è stata aggiornata in modo da renderla compatibile con il database Aurora.<!--MCLOUD-7269-->

- ![nuova icona](../../assets/new.svg) **Nuova variabile di ambiente SCD_NO_PARENT**- Aggiunto il `SCD_NO_PARENT` variabile di ambiente (per Adobe Commerce >=2.4.2) per gestire la generazione di contenuto statico per i temi principali.<!--MCLOUD-7284-->

- ![icona correzione](../../assets/fix.svg) **Limiti di memoria e comandi**- È stato risolto un problema in cui `php vendor/bin/ece-tools` I comandi non funzionerebbero se le dimensioni del `cloud.log` superata la memoria PHP. Invece di leggere l&#39;intero `cloud.log` file in memoria, ora si legge solo un sottoinsieme di dati più piccolo dal file di registro.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![icona correzione](../../assets/fix.svg) **Connessioni di database personalizzate**- Fisso a `.magento.env.yaml` problema di configurazione per cui sono definite connessioni di database personalizzate `DATABASE_CONFIGURATION` non sono stati utilizzati. Impossibile aggiungere le impostazioni di connessione a `app/etc/env.php`.<!--MCLOUD-7426-->

- ![icona correzione](../../assets/fix.svg) **Registri di errore vuoti**- È stato risolto un problema che impediva l&#39;esecuzione delle distribuzioni se il `cloud.error.log` era vuoto.<!--MCLOUD-7296-->

- ![icona correzione](../../assets/fix.svg) **Convalida MariaDB 10.3**—È stata corretta la convalida di MariaDB 10.3 per Adobe Commerce 2.3.6-p1.<!--MCLOUD-7416-->

- ![icona correzione](../../assets/fix.svg) **Cache:log di scaricamento**- Sono state migliorate le voci di registro per indicare l&#39;inizio e la fine della `cache:flush` passaggio.<!--MCLOUD-7503-->

## v2002.1.4

Data di rilascio: 19 novembre 2020

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore di distribuzione quando il motore di ricerca specificato in `SEARCH_CONFIGURATION` la variabile di ambiente è un valore diverso da `elasticsearch`.<!--MCLOUD-7283-->

## v2002.1.3

Data di rilascio: 9 novembre 2020

**Aggiornamenti all’infrastruttura**—

- ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto ECE-Tools per la sola lettura `pub/static` quando il contenuto statico è impostato per la distribuzione nella fase di build.<!--MC-37699-->

- ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto di Elasticsearch 7.9 e Redis 6 per la compatibilità con le prossime versioni di Adobe Commerce.<!--MCLOUD-7191-->

- ![icona correzione](../../assets/fix.svg) Aggiornato il documento ECE-Tools `composer.json` per aggiungere una dipendenza richiesta per lo strumento Patch di qualità. Questo risolve una dipendenza circolare esistente tra i pacchetti ECE-Tools e magento-cloud-patches.<!--MCLOUD-6910-->

**Miglioramenti a livello di convalida e registro**—

- ![nuova icona](../../assets/new.svg) È stata aggiunta la convalida del motore di ricerca per garantire che `elasticsearch` è impostato per Adobe Commerce su infrastruttura cloud 2.4 e versioni successive. Se la convalida non riesce, la distribuzione viene interrotta con un messaggio di errore critico che suggerisce correzioni per il problema. Consulta [Errori critici, fase di distribuzione](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![nuova icona](../../assets/new.svg) È stata aggiunta la convalida Elasticsearch per verificare la compatibilità tra la versione del servizio Elasticsearch e la versione di Adobe Commerce.<!--MCLOUD-7193-->

- ![nuova icona](../../assets/new.svg) È stato aggiornato il messaggio di errore di compatibilità di Elasticsearch per mostrare le versioni di Elasticsearch compatibili con il modulo di Elasticsearch di Adobe Commerce. Il messaggio di errore fornisce ora le versioni specifiche dell’Elasticsearch da installare nell’infrastruttura Cloud, in modo che siano compatibili con il modulo di Elasticsearch utilizzato dalla versione di Adobe Commerce in uso. Consulta [Avviso di errori, fase di distribuzione](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![nuova icona](../../assets/new.svg) Sono stati aggiunti errori di avviso `2026` e `2027` per non valido `MAGE_MODE` impostazione della variabile di ambiente. L’unico valore valido è `production`. Prima di questa correzione, `MAGE_MODE` può essere impostato su `developer` senza errori di distribuzione, solo per causare errori in un secondo momento quando si tenta di scrivere in file di sola lettura. Consulta [Errori di avvertenza](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![icona correzione](../../assets/fix.svg) È stata corretta la convalida per i servizi Redis, RabbitMQ e MySQL per garantire che queste versioni siano compatibili con la versione di Adobe Commerce. Le versioni valide di questi servizi sono ora scritte nel `cloud.log`.<!--MCLOUD-7098-->

- ![icona correzione](../../assets/fix.svg) È stato aggiornato il `cloud.log` per includere il limite di richieste simultanee per l’invio di richieste durante il riscaldamento della cache. Questo valore è configurato in [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency) variabile post-distribuzione.<!--MCLOUD-5563-->

**Aggiornamenti dei comandi CLI**—

- ![nuova icona](../../assets/new.svg) Comandi CLI aggiunti (`cloud:config:create` e `cloud:config:update`) per creare e aggiornare `.magento.env.yaml` file con una configurazione che può includere una o più variabili di build, distribuzione e post-distribuzione. Consulta [Crea file di configurazione da CLI](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**Aggiornamenti delle variabili di ambiente**—

- ![nuova icona](../../assets/new.svg) È stata aggiunta la [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload) variabile di build. Impostazione della variabile su `true` impedisce all&#39;applicazione di eseguire `composer dump-autoload` durante un’installazione di Cloud Docker for Commerce. La variabile è pertinente solo per Cloud Docker per contenitori Commerce con file system scrivibili (creati per il test e lo sviluppo tramite `./vendor/bin/ece-docker build:compose --with-test`). Con tali installazioni, ignorare `composer dump-autoload` impedisce gli errori durante l&#39;esecuzione di altri comandi che tentano di accedere ai file da un `generated` directory.<!--MCLOUD-6939-->

## v2002.1.2

Data di rilascio: 5 agosto 2020

**Miglioramenti a livello di convalida e registro**—

- ![nuova icona](../../assets/new.svg) È stata aggiunta la `schema.error.yaml` file che include tutte le notifiche di errore e di avviso che possono verificarsi durante il processo di generazione, distribuzione e post-distribuzione, insieme a suggerimenti per la risoluzione degli errori. Le informazioni contenute in questo file sono disponibili anche nel _Guida di Cloud per Commerce_. Consulta [Riferimento al messaggio di errore per gli strumenti ece](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![nuova icona](../../assets/new.svg) Registro degli errori cloud modificato (`/var/log/cloud.error.log`) in formato JSON per agevolare l&#39;analisi del registro a livello di programmazione.<!--MCLOUD-5879-->

- ![nuova icona](../../assets/new.svg) Sono stati aggiunti ulteriori controlli di errore per la generazione, la distribuzione e l’elaborazione post-distribuzione e sono stati migliorati i controlli esistenti:

   - Codice di errore 2026 - Impossibile ripristinare alcuni dati generati durante la fase di build nelle directory montate

   - Codice di errore 3004 - Impossibile creare i file di backup

   - Codice di errore 102 - Sono stati aggiunti ulteriori controlli per i problemi che si verificano quando `env.php` file non scrivibile <!--MCLOUD-6221-->

- ![nuova icona](../../assets/new.svg) È stata aggiunta la **QUALITY_PATCH** variabile di ambiente per specificare una o più patch di qualità da applicare durante il processo di distribuzione. Consulta [Variabili di build](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

Data di rilascio: 25 giugno 2020

- ![nuova icona](../../assets/new.svg) **Aggiornamenti all’infrastruttura**—

   - ![nuova icona](../../assets/new.svg) **Miglioramenti alla registrazione**: è stata migliorata la funzionalità di tracciamento del registro assegnando codici di uscita agli errori critici di distribuzione ed esponendo i codici di uscita nelle notifiche dei messaggi di errore e negli eventi di registro. Consulta [Riferimento al messaggio di errore per gli strumenti ece](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![nuova icona](../../assets/new.svg) È stato migliorato il processo per le immagini del database (`vendor/bin/ece-tools db-dump`) e i messaggi di registro aggiornati per chiarire che l&#39;operazione di dump del database passa l&#39;applicazione alla modalità di manutenzione, interrompe i processi della coda del consumatore e disabilita i processi cron prima dell&#39;inizio dell&#39;immagine.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema per garantire che l’URL del progetto venga aggiornato correttamente durante la distribuzione negli ambienti di staging e produzione. Ora, `ece-tools` utilizza l’URL per la route con `primary:true` attributo impostato nella configurazione dell&#39;instradamento del progetto. Consulta [Distribuire le variabili](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![icona correzione](../../assets/fix.svg) È stato aggiornato il `generate.xml` crea il flusso di lavoro dello scenario per applicare le patch. Le patch devono essere applicate prima per aggiornare Adobe Commerce e risolvere eventuali problemi che potrebbero causare `di:compile` e `module:refresh` passaggi per non riuscire.<!--MCLOUD-5941-->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema nel processo di installazione che restituiva in modo errato il `Crypt key missing` errore. Il `crypt/key` viene generato automaticamente durante l&#39;installazione.<!--MCLOUD-6120-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei servizi**—

   - ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto per PHP 7.4 e MariaDB 10.4.<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti delle variabili di ambiente**—

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la **SCD_USE_BALER** per abilitare il modulo Baler per il bundle JavaScript durante il processo di sviluppo dell’infrastruttura cloud di Adobe Commerce. Vedi la descrizione della variabile in [variabili di build](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la **REDIS_BACKEND** variabile di ambiente per configurare il modello di back-end Redis per la cache Redis per Adobe Commerce 2.3.5 o versione successiva. Vedi la descrizione della variabile in [distribuire variabili](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei comandi CLI**—

   - ![nuova icona](../../assets/new.svg) Sono stati aggiornati i seguenti comandi CLI con un’opzione per una registrazione più dettagliata:

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     Il livello di registrazione per ogni chiamata è determinato dalla configurazione del [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) variabile in `.magento.env.yaml` file.<!--MCLOUD-3503-->

- ![nuova icona](../../assets/new.svg) **Miglioramenti della convalida**—

   - ![nuova icona](../../assets/new.svg) **Elasticsearch 7.x controlli di compatibilità**- Convalida dell&#39;Elasticsearch aggiornata per i controlli di compatibilità software Elasticsearch 7.x.<!--MCLOUD-5542-->

   - ![nuova icona](../../assets/new.svg) **Versioni del servizio aggiornate e controlli di convalida EOL**- Convalida aggiornata per verificare le versioni del servizio installate in base ai requisiti di Adobe Commerce 2.4.<!--MCLOUD-6144-->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema di convalida a causa del quale il seguente messaggio di avviso post-distribuzione veniva visualizzato solo se `post-deploy` configurazione hook mancante dal `.magento.app.yaml` file:

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![nuova icona](../../assets/new.svg) **È stata aggiunta la convalida per le dipendenze di Zend Framework**- Aggiunta della convalida delle dipendenze del compositore per il framework Zend migrato al progetto Laminas. Se mancano le dipendenze richieste, durante il processo di compilazione viene visualizzato il seguente messaggio di errore.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     Consulta [Verificare le dipendenze di Zend Framework](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![nuova icona](../../assets/new.svg) **Convalida aggiunta per `env.php` file e dati**- Sono stati aggiunti controlli per `env.php` file e dati durante il processo di installazione e aggiornamento.<!--MCLOUD-5991-->

      - Se il `env.php` file mancante nell&#39;installazione e il `crypt/key` valore non specificato nel `.magento.app.yaml` , la distribuzione non riesce e viene visualizzata la seguente notifica:

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - Se l&#39;installazione non include `env.php` o la configurazione contiene un solo tipo di cache, il `cron:enable` il comando viene eseguito durante il processo di aggiornamento per ripristinare il file con `cache_types`. Al registro viene aggiunta la seguente notifica:

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

Data di rilascio: 6 febbraio 2020

- ![nuova icona](../../assets/new.svg) **Aggiornamenti all’infrastruttura**—

   - ![nuova icona](../../assets/new.svg) **È stato aggiunto un pacchetto separato per Cloud Docker per Commerce**: il pacchetto Docker è stato disaccoppiato dalla `ece-tools` per mantenere la qualità del codice e fornire versioni indipendenti. Aggiornamenti e correzioni relativi a `ece-tools` sono gestite dal [magento-cloud-docker](https://github.com/magento/magento-cloud-docker) Archivio GitHub.<!--MAGECLOUD-2927-->

   - ![nuova icona](../../assets/new.svg) **Funzionalità di patch aggiornate**- La funzionalità di applicazione della patch è stata spostata dal pacchetto ECE-Tools a un [magento-cloud-patches](https://github.com/magento/magento-cloud-patches) pacchetto. Durante la distribuzione, `ece-tools` utilizza il nuovo pacchetto per applicare le patch. Consulta [Note sulla versione delle patch cloud](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![nuova icona](../../assets/new.svg) **Dipendenze del compositore aggiornate**- Aggiornato il `composer.json` file per Adobe Commerce su infrastruttura cloud con una dipendenza per `magento/magento-cloud-docker` pacchetto. Ora, `ece-tools` include le dipendenze per tutti i pacchetti nel [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). Questi pacchetti vengono installati e aggiornati automaticamente al momento dell&#39;installazione o dell&#39;aggiornamento `ece-tools`.

- ![nuova icona](../../assets/new.svg) **Supporto per implementazioni basate su scenari**—<!--MAGECLOUD-4101-->

   - ![nuova icona](../../assets/new.svg) Ora è possibile personalizzare i processi di generazione, distribuzione e post-distribuzione utilizzando i file di configurazione XML per sostituire o personalizzare la configurazione predefinita.

   - ![nuova icona](../../assets/new.svg) **È stato modificato il `hooks` configurazione in`.magento.app.yaml`**- Abbiamo aggiornato il `hooks` per supportare distribuzioni basate su scenari. Il formato legacy della versione precedente di ECE-Tools 2002.0.x è ancora supportato. Tuttavia, è necessario eseguire l’aggiornamento al nuovo formato per utilizzare la funzione di distribuzione basata su scenari. Consulta [Distribuzioni basate su scenari](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>Prima di eseguire l&#39;aggiornamento alla versione ECE-Tools 2002.1.0, esaminare [modifiche non compatibili con le versioni precedenti](backward-incompatible-changes.md) per informazioni sulle modifiche che potrebbero richiedere l’aggiornamento della configurazione o dei processi del progetto Adobe Commerce on cloud infrastructure.

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei servizi**—

   - ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto per PHP 7.3.<!--MAGECLOUD-4022-->

   - ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto per RabbitMQ 3.8.<!--MAGECLOUD-4674-->

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la convalida per verificare le versioni del servizio installate rispetto alla data di fine del ciclo di vita di ciascun servizio. Ora, i clienti ricevono una notifica se la versione del servizio è entro tre mesi dalla data di fine del ciclo di vita e un avviso se la data di fine del ciclo di vita è nel passato.<!--MAGECLOUD-4076-->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema di configurazione di Elasticsearch per garantire che le impostazioni di Elasticsearch corrette siano configurate in tutti gli ambienti.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>Consulta [Versioni del servizio](../services/services-yaml.md#service-versions) per un elenco dei servizi utilizzati in Adobe Commerce sull’infrastruttura cloud e la loro compatibilità di versione con il modello Cloud.

- ![nuova icona](../../assets/new.svg) **Aggiornamenti delle variabili di ambiente**—

   - ![nuova icona](../../assets/new.svg) È stata estesa la funzionalità di `WARM_UP_PAGES` variabile di ambiente per supportare il precaricamento della cache per pagine di prodotto specifiche. Vedi la definizione espansa in [variabili post-distribuzione](../environment/variables-post-deploy.md#warm_up_pages) argomento.<!--MAGECLOUD-4444-->

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la `ERROR_REPORT_DIR_NESTING_LEVEL` variabile di ambiente per semplificare la gestione dei dati della segnalazione errori in `<magento_root>/var/report/` directory. Vedi la descrizione della variabile in [variabili di build](../environment/variables-build.md#error_report_dir_nesting_level) argomento.

   - ![icona correzione](../../assets/fix.svg) È stato rimosso il `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`,`DO_DEPLOY_STATIC_CONTENT`, e `STATIC_CONTENT_SYMLINK` variabili di ambiente. Consulta [Modifiche non compatibili con le versioni precedenti](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema nel processo di configurazione della suite elastica a causa del quale la configurazione predefinita veniva sovrascritta come previsto durante la configurazione di `ELASTICSUITE_CONFIGURATION` distribuire la variabile senza `_merge` opzione.<!--MAGECLOUD-4388-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei comandi CLI**—

   - ![nuova icona](../../assets/new.svg) **Nuovo comando cron**: ora è possibile gestire manualmente l’elaborazione cron nell’ambiente Adobe Commerce sull’infrastruttura cloud utilizzando `cron:disable` e `cron:enable` comandi. Utilizzare il comando disable per arrestare tutti i processi cron attivi e disattivare tutti i processi cron. Utilizzare il comando enable per riattivare i processi cron quando si è pronti. Consulta [Disabilita processi cron](../application/crons-property.md#disable-cron-jobs).

   - ![nuova icona](../../assets/new.svg) **Segnalazione degli errori migliorata**- È stata aggiunta una migliore registrazione per gli errori dei comandi CLI che si verificano durante l&#39;elaborazione ECE-Tools.<!--MAGECLOUD-4849-->

   - ![nuova icona](../../assets/new.svg) **Rimuovi comandi build obsoleti**— Sono stati rimossi i seguenti comandi di compilazione: `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump`, e rinominato `ece-tools docker` comandi per `ece-docker`. Consulta [Modifiche non compatibili con le versioni precedenti](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![nuova icona](../../assets/new.svg) Rimosso il obsoleto `build_options.ini` e la convalida aggiunta non riusciranno a generare la build se il file esiste. Utilizza il [.magento.env.yaml](../environment/configure-env-yaml.md) per configurare le opzioni di generazione.

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore nel processo di compilazione quando `config.php` file vuoto.<!--MAGECLOUD-4127-->

## 2002.0.23.

Data di rilascio: 27 febbraio 2020

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema di compatibilità con `ece-tools` Versioni 2002.0.x che impedivano il completamento corretto della generazione di contenuti statici su richiesta in modalità di produzione.

## Versioni precedenti

Consulta la [archivio note sulla versione](cloud-release-archive.md) per le versioni 2002.0.22 e precedenti.
