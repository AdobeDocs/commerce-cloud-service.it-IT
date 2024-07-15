---
title: Note sulla versione di ECE-Tools
description: Consulta l’elenco degli ultimi miglioramenti apportati al pacchetto ECE-Strumenti.
recommendations: noDisplay, catalog
last-substantial-update: 2024-05-21T00:00:00Z
exl-id: a464b940-c56e-4a7c-9948-559539e25361
source-git-commit: 923e2114270df22e134e0676ac97f84d770bb226
workflow-type: tm+mt
source-wordcount: '2929'
ht-degree: 0%

---

# Note sulla versione di ECE-Tools

Il pacchetto [ece-tools](https://github.com/magento/ece-tools) è un insieme di script e strumenti progettati per gestire e distribuire progetti Cloud. Queste note sulla versione descrivono gli ultimi miglioramenti apportati a questo pacchetto, che fa parte della [suite di strumenti cloud per Commerce](cloud-tools-suite.md).

>[!NOTE]
>
>Per informazioni sull&#39;aggiornamento all&#39;ultima versione del pacchetto `ece-tools`, vedere [Aggiornamento degli strumenti ECE](../dev-tools/update-package.md).

Il pacchetto `ece-tools` utilizza la seguente sequenza di versioni di rilascio: `200<major>.<minor>.<patch>`

Le note sulla versione includono:

- ![nuova icona](../../assets/new.svg) Nuove funzioni
- ![icona correzione](../../assets/fix.svg) Correzioni e miglioramenti

<!--Add release notes below-->

## v2002.1.19 {#latest}

Data di rilascio: 21 maggio 2024

- ![nuova icona](../../assets/new.svg) **Lua**—Aggiunta dell&#39;opzione useLua per CACHE_CONFIGURATION.
- ![icona correzione](../../assets/fix.svg) **Convalida**—Sono state aggiornate le convalide per le nuove versioni di Redis e RabbitMQ.

## v2002.1.18

Data di rilascio: 8 aprile 2024

- ![nuova icona](../../assets/new.svg) **PHP** — Aggiunto supporto per PHP 8.3.
- ![icona correzione](../../assets/fix.svg) Convalida - Convalida fine vita aggiornata.

## v2002.1.17

Data di rilascio: 16 gennaio 2024

- ![icona correzione](../../assets/fix.svg) **Convalida per Elasticsearch &amp; OpenSearch**—È stato corretto il messaggio di convalida che generava un messaggio fuorviante per installare un servizio di ricerca quando LiveSearch è abilitato.<!-- MCLOUD-10167 -->
- ![icona di correzione](../../assets/fix.svg) **Avviso di distribuzione**—È stato risolto un problema che causava avvisi di distribuzione per le cartelle non vuote.<!-- MCLOUD-8958 -->

## v2002.1.16

Data di rilascio: 16 ottobre 2023

- ![nuova icona](../../assets/new.svg) **Variabile di ambiente globale ENABLE_WEBHOOKS** - Aggiunta della variabile globale [ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks) da utilizzare con i webhook di Commerce per la connessione a un endpoint esterno, ad esempio l&#39;azione di runtime di App Builder o un sistema di gestione dell&#39;inventario di terze parti.

## v2002.1.15

Data di rilascio: 31 luglio 2023

- ![icona correzione](../../assets/fix.svg) **Codici di errore**—Aggiornamento dello schema dei codici di errore e del generatore di documenti dei codici di errore.
- ![icona correzione](../../assets/fix.svg) **Convalida per il modello Redis personalizzato**-Aggiornamento della convalida per i modelli di back-end Redis personalizzati. [Vedere l&#39;esempio per la configurazione della cache](../environment/variables-deploy.md#cache_configuration).
- ![icona correzione](../../assets/fix.svg) **Convalida per RabbitMQ**-Aggiunto supporto per RabbitMQ 3.11
- ![icona correzione](../../assets/fix.svg) **È stato corretto il collegamento errato**-È stato corretto il collegamento errato alla documentazione di onboarding nel modello e-mail di benvenuto.

## v2002.1.14

Data di rilascio: 10 marzo 2023

- ![nuova icona](../../assets/new.svg) **PHP**—Aggiunto supporto per PHP 8.2.
- ![nuova icona](../../assets/new.svg) **Convalida per i servizi**—Sono state aggiornate le convalide per i servizi richiesti di Commerce 2.4.6: MariaDB 10.6, Redis 7.0, PHP 8.2, OpenSearch 2.x e RabbitMQ 3.9.
- ![icona correzione](../../assets/fix.svg) **strumenti ece db-dump**—È stato risolto un problema che causava l&#39;interruzione anticipata dell&#39;operazione `db-dump`.

## v2002.1.13

Data di rilascio: 27 ottobre 2022

- ![nuova icona](../../assets/new.svg) **Aggiunto supporto per eventi di Adobe I/O per Adobe Commerce**. Gli sviluppatori di estensioni possono ora utilizzare il framework [Adobe I/O Events](https://developer.adobe.com/events/docs/) per inviare informazioni sugli eventi Commerce dalle istanze Cloud alle proprie applicazioni scritte per [Adobe App Builder](https://developer.adobe.com/app-builder/docs/overview/). Eventi di Adobe I/O per Adobe Commerce in Anteprima partner.<!-- CEXT-932 -->
- ![nuova icona](../../assets/new.svg) **Convalida per la configurazione OPcache**—Aggiunta di un validatore per verificare la configurazione OPcache per i percorsi esclusi.<!-- MCLOUD-9485 -->
- ![icona correzione](../../assets/fix.svg) **È stato risolto un problema con la configurazione della cache di GraphQL**. Ora ECE-Tools mantiene il valore di GraphQL `id_salt` nella configurazione di `cache` nel file `app/etc/env.php`.<!-- MCLOUD-9486 -->

## v2002.1.12

Data di rilascio: 13 settembre 2022

- ![nuova icona](../../assets/new.svg) **Abilita`synchronous_replication`**—ECE-Tools imposta `synchronous_replication=>true` nel file `app/etc/env.php` quando `MYSQL_USE_SLAVE_CONNECTION` è abilitato. Questa configurazione interessa solo Commerce 2.4.6+. Vedi la descrizione della variabile `MYSQL_USE_SLAVE_CONNECTION` nelle [Variabili di distribuzione](../environment/variables-deploy.md#mysql_use_slave_connection).<!-- MCLOUD-9142 -->
- ![nuova icona](../../assets/new.svg) **OpenSearch**—Aggiunta della funzionalità per configurare e impostare il motore `opensearch` per la prossima versione di Adobe Commerce 2.4.6. Vedere [Configurazione del servizio OpenSearch](../services/opensearch.md).<!-- MCLOUD-9236 -->

## v2002.1.11

Data di rilascio: 4 agosto 2022

- ![icona di correzione](../../assets/fix.svg) **ElasticSuite Validator e OpenSearch**—È stato corretto un problema di convalida del controllo di integrità ElasticSuite quando OpenSearch è installato.<!-- MCLOUD-8767 -->
- ![icona correzione](../../assets/fix.svg) **Tipi restituiti per i comandi di distribuzione**—Sono stati corretti i tipi restituiti per i comandi di distribuzione.<!-- AC-3208 -->
- ![icona di correzione](../../assets/fix.svg) **[!DNL RabbitMQ]problema con la nuova installazione di Commerce 2.4.5**—È stato risolto [!DNL RabbitMQ] problema di arresto anomalo nella nuova installazione di Commerce 2.4.5.<!-- MCLOUD-9059 -->

## v2002.1.10

Data di rilascio: 31 marzo 2022

- ![icona correzione](../../assets/fix.svg) **Elasticsearch 7.10**—Sono state aggiornate le convalide per supportare la versione 7.10 di Elasticsearch.<!-- MCLOUD-8548 -->

## v2002.1.9

Data di rilascio: 10 marzo 2022

- ![nuova icona](../../assets/new.svg) **OpenSearch**—Aggiunto supporto per OpenSearch per Adobe Commerce versioni 2.4.4, 2.4.3-p2 e 2.3.7-p3.<!-- MCLOUD-8296 -->
- ![nuova icona](../../assets/new.svg) **PHP**—Aggiunto supporto per PHP 8.1.
- ![icona correzione](../../assets/fix.svg) **symfony/process**—Aggiunta compatibilità con symfony/process ^5.3.<!-- MCLOUD-8283 -->

- ![nuova icona](../../assets/new.svg) **Processi multipli consumer**—Aggiunta di un&#39;opzione `multiple_processes` che consente di specificare il numero di processi da generare per ogni consumer. Vedi la descrizione della variabile `CRON_CONSUMERS_RUNNER` nelle [Variabili di distribuzione](../environment/variables-deploy.md#cron_consumers_runner).<!-- MCLOUD-8295 -->
- ![nuova icona](../../assets/new.svg) **Schema OpenSearch e percorso host completo**. È stata aggiunta la possibilità di configurare uno schema di Elasticsearch e un percorso host completo.
- ![icona correzione](../../assets/fix.svg) **AWS S3**: metodo di abilitazione di AWS S3 modificato.
- ![icona correzione](../../assets/fix.svg) **Correggi lettore driver_options**—Aggiunta della configurazione driver_options di lettura per la connessione DB dal file `env.php` da parte di `ece-tools` per i validatori.<!-- MCLOUD-8420 -->

## v2002.1.8

Data di rilascio: 25 ottobre 2021

- ![nuova icona](../../assets/new.svg) **Percorso dump alternativo**. Aggiunta dell&#39;opzione `--dump-directory` che consente di scegliere una directory di destinazione per un dump del database. Ora `/app/var/dump-main` è la directory di destinazione predefinita per un dump del database. Consulta [Gestione backup: scarica il database](../storage/database-dump.md)<!-- MCLOUD-8063 -->
- ![icona correzione](../../assets/fix.svg) **Aggiorna monologo**—È stata aggiornata la versione minima richiesta per il pacchetto `monolog` in `^2.3`.<!-- ACMP-1263 -->
- ![icona correzione](../../assets/fix.svg) **Aggiorna Symfony**—Sono state aggiornate le dipendenze di Symfony per renderle compatibili con Adobe Commerce 2.4.4.<!-- ACMP-1533 -->
- ![icona di correzione](../../assets/fix.svg) **Caricamento automatico funzionalità/risoluzione**—È stato risolto un problema che si verificava durante la distribuzione in un ambiente di integrazione e durante la visualizzazione dell&#39;errore `CRITICAL: [9] Required configuration is missed in autoload section of composer.json file.`.<!-- https://github.com/magento/ece-tools/pull/799 -->

## v2002.1.7

Data di rilascio: 29 luglio 2021

**Aggiornamenti configurazione**—

- ![nuova icona](../../assets/new.svg) Aggiunto supporto per Composer 2.0.<!--MCLOUD-8003-->

- ![icona correzione](../../assets/fix.svg) **Requisiti compositore aggiornati per`symphony/console`**—Sono stati aggiornati i requisiti di versione `composer.json` degli strumenti ECE per il pacchetto `symphony/console` per risolvere un problema che ha causato l&#39;errore seguente ai comandi `di:compile`: `Incompatible argument type: Required type: int. Actual type: string`<!--MC-42919-->

- ![icona correzione](../../assets/fix.svg) Aggiornamento dei controlli software di fine del ciclo di vita (`eol.yaml`) per includere l&#39;Elasticsearch 7.9.x.<!--MCLOUD-7938-->

## v2002.1.6

Data di rilascio: 20 aprile 2021

- ![nuova icona](../../assets/new.svg) **Credenziali di autenticazione Redis**—Aggiunta la possibilità di leggere le credenziali di autorizzazione Redis dalla proprietà `relationships` durante la fase di distribuzione.<!--MCLOUD-7694-->

- ![nuova icona](../../assets/new.svg) **Credenziali di autorizzazione dell&#39;Elasticsearch**—Aggiunta la possibilità di leggere le credenziali di autorizzazione dell&#39;Elasticsearch dalla proprietà `relationships` durante la fase di distribuzione.<!--MCLOUD-7695-->

- ![nuova icona](../../assets/new.svg) **Servizio di archiviazione delle sessioni dedicato**. Aggiunta di `redis-session` come seconda opzione per l&#39;archiviazione delle sessioni. È possibile utilizzare il servizio `redis-session` per archiviare le informazioni sulla sessione e utilizzare il servizio `redis` per la cache per fornire prestazioni migliori.<!--MCLOUD-7698-->

- ![nuova icona](../../assets/new.svg) **Messaggi SPLIT_DB obsoleti**—Aggiunti avvisi di convalida e messaggi critici per l&#39;opzione `SPLIT_DB` obsoleta per Adobe Commerce 2.4.2 e la sua rimozione in Adobe Commerce 2.5.0.<!--MCLOUD-7806-->

- ![icona correzione](../../assets/fix.svg) **versione Elasticsearch da relazioni**—È stato corretto il modulo di convalida del servizio per recuperare la versione corretta dell&#39;Elasticsearch dalle proprietà `relationships` in Cloud Docker e negli ambienti di integrazione.<!--MCLOUD-7572-->

- ![icona correzione](../../assets/fix.svg) **Convalida porta Redis flessibile**. Redis può ora convalidare la porta in una connessione alla cache personalizzata dall&#39;URL `server`. Ad esempio, è possibile aggiungere il numero di porta all&#39;URL del server come segue: `server: 'tcp://rfs-store-simple-page-cache:26379'`. Ciò consente di evitare errori di convalida in cui l&#39;opzione `port` è mancante o non corretta.<!--MCLOUD-7722-->

- ![icona di correzione](../../assets/fix.svg) **Aggiornamento ad Adobe Commerce 2.4.2**—È stato risolto il problema che richiedeva agli utenti di eseguire manualmente `bin/magento setup:upgrade` per rendere operativi i propri siti dopo l&#39;aggiornamento ad Adobe Commerce 2.4.2.<!--MCLOUD-7776-->

## v2002.1.5

Data di rilascio: 1 febbraio 2021

- ![nuova icona](../../assets/new.svg) **Archiviazione remota**. Aggiunta variabile di ambiente `REMOTE_STORAGE` per abilitare i progetti cloud per l&#39;archiviazione remota di file multimediali tramite un servizio di archiviazione, ad esempio AWS S3. Questa opzione di configurazione fa parte del pacchetto ECE-Tools, ma non è supportata in Adobe Commerce sull&#39;infrastruttura cloud.<!--MCLOUD-7153-->

- ![nuova icona](../../assets/new.svg) **Nuovo comando `cloud:config:validate`**—È stato aggiunto il comando `php vendor/bin/ece-tools cloud:config:validate` per convalidare la configurazione `.magento.env.yaml` prima di inviare le modifiche all&#39;ambiente cloud remoto.<!--MCLOUD-7120-->

- ![nuova icona](../../assets/new.svg) **Svuotamento della opcache**. È stato aggiunto il supporto per l&#39;opzione PHP `opcache.enable_cli` per svuotare la OPcache prima di eseguire l&#39;hook di distribuzione. Questa configurazione reimposta la configurazione della cache per garantire che le impostazioni di configurazione correnti vengano applicate a ogni distribuzione.<!--MCLOUD-7015-->

- ![nuova icona](../../assets/new.svg) **Convalida di Aurora DB**: la convalida del servizio di database è stata aggiornata in modo da renderla compatibile con il database Aurora.<!--MCLOUD-7269-->

- ![nuova icona](../../assets/new.svg) **Nuova variabile di ambiente SCD_NO_PARENT** - Aggiunta della variabile di ambiente `SCD_NO_PARENT` (per Adobe Commerce >=2.4.2) per gestire la generazione di contenuto statico per i temi principali.<!--MCLOUD-7284-->

- ![icona correzione](../../assets/fix.svg) **Limiti di memoria e comandi**—È stato risolto un problema che impediva il funzionamento di `php vendor/bin/ece-tools` comandi se le dimensioni del file `cloud.log` superavano il limite di memoria PHP. Invece di leggere l&#39;intero file `cloud.log` in memoria, ora si legge solo un sottoinsieme di dati più piccolo dal file di registro.<!--MCLOUD-7275--><!--MCLOUD-7400-->

- ![icona di correzione](../../assets/fix.svg) **Connessioni di database personalizzate**—È stato risolto un problema di configurazione `.magento.env.yaml` a causa del quale non venivano utilizzate le connessioni di database personalizzate definite per `DATABASE_CONFIGURATION`. Impossibile aggiungere le impostazioni di connessione a `app/etc/env.php`.<!--MCLOUD-7426-->

- ![icona correzione](../../assets/fix.svg) **Registri di errore vuoti**—È stato risolto un problema che causava errori di distribuzione se `cloud.error.log` era vuoto.<!--MCLOUD-7296-->

- ![icona di correzione](../../assets/fix.svg) **Convalida MariaDB 10.3**—È stata corretta la convalida di MariaDB 10.3 per Adobe Commerce 2.3.6-p1.<!--MCLOUD-7416-->

- ![icona correzione](../../assets/fix.svg) **Cache:log di scaricamento**—Sono state migliorate le voci di registro per indicare l&#39;inizio e la fine del passaggio `cache:flush`.<!--MCLOUD-7503-->

## v2002.1.4

Data di rilascio: 19 novembre 2020

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore di distribuzione quando il motore di ricerca specificato nella variabile di ambiente `SEARCH_CONFIGURATION` è un valore diverso da `elasticsearch`.<!--MCLOUD-7283-->

## v2002.1.3

Data di rilascio: 9 novembre 2020

**Aggiornamenti dell&#39;infrastruttura**—

- ![nuova icona](../../assets/new.svg) Aggiunta del supporto ECE-Tools per la directory `pub/static` di sola lettura quando il contenuto statico è impostato per la distribuzione nella fase di compilazione.<!--MC-37699-->

- ![nuova icona](../../assets/new.svg) Aggiunto supporto per Elasticsearch 7.9 e Redis 6 per compatibilità con le prossime versioni di Adobe Commerce.<!--MCLOUD-7191-->

- ![icona correzione](../../assets/fix.svg) Aggiornamento degli strumenti ECE `composer.json` per aggiungere una dipendenza necessaria per lo strumento Patch di qualità. In questo modo viene corretta una dipendenza circolare esistente tra i pacchetti ECE-Tools e magento-cloud-patches.<!--MCLOUD-6910-->

**Miglioramenti di convalida e registro**—

- ![nuova icona](../../assets/new.svg) Aggiunta della convalida del motore di ricerca per garantire che `elasticsearch` sia impostato per Adobe Commerce sull&#39;infrastruttura cloud 2.4 e versioni successive. Se la convalida non riesce, la distribuzione viene interrotta con un messaggio di errore critico che suggerisce correzioni per il problema. Vedi [Errori critici, fase di distribuzione](../dev-tools/error-reference.md#deploy-stage).<!--MCLOUD-6937-->

- ![nuova icona](../../assets/new.svg) Aggiunta della convalida dell&#39;Elasticsearch per verificare la compatibilità tra la versione del servizio Elasticsearch e la versione di Adobe Commerce.<!--MCLOUD-7193-->

- ![nuova icona](../../assets/new.svg) Il messaggio di errore di compatibilità dell&#39;Elasticsearch è stato aggiornato in modo da visualizzare le versioni di Elasticsearch compatibili con il modulo Adobe Commerce Elasticsearch. Il messaggio di errore fornisce ora le versioni specifiche dell’Elasticsearch da installare nell’infrastruttura Cloud, in modo che siano compatibili con il modulo di Elasticsearch utilizzato dalla versione di Adobe Commerce in uso. Vedi [Errori di avviso, fase di distribuzione](../dev-tools/error-reference.md#deploy-stage-1).<!--MCLOUD-6698-->

- ![nuova icona](../../assets/new.svg) Sono stati aggiunti errori di avviso `2026` e `2027` per un&#39;impostazione non valida della variabile di ambiente `MAGE_MODE`. L&#39;unico valore valido è `production`. Prima di questa correzione, è possibile impostare `MAGE_MODE` su `developer` senza errori di distribuzione, solo per causare errori in un secondo momento quando si tenta di scrivere in file di sola lettura. Vedi [Errori di avviso](../dev-tools/error-reference.md#warning-errors).<!--MCLOUD-6708-->

- ![icona correzione](../../assets/fix.svg) È stata corretta la convalida per i servizi Redis, RabbitMQ e MySQL per garantire che queste versioni siano compatibili con la versione di Adobe Commerce. Versioni valide di questi servizi sono ora scritte in `cloud.log`.<!--MCLOUD-7098-->

- ![icona correzione](../../assets/fix.svg) Aggiornato `cloud.log` per includere il limite di richieste simultanee per l&#39;invio di richieste durante il riscaldamento della cache. Questo valore è configurato nella variabile post-distribuzione [WARM_UP_CONCURRENCY](../environment/variables-post-deploy.md#warm_up_concurrency).<!--MCLOUD-5563-->

**Aggiornamenti del comando CLI**—

- ![nuova icona](../../assets/new.svg) Sono stati aggiunti comandi CLI (`cloud:config:create` e `cloud:config:update`) per creare e aggiornare il file `.magento.env.yaml` con una configurazione che può includere una o più variabili di compilazione, distribuzione e post-distribuzione. Vedere [Crea file di configurazione da CLI](../environment/configure-env-yaml.md#create-configuration-file-from-cli).<!--MCLOUD-7072-->

**Aggiornamenti della variabile di ambiente**—

- ![nuova icona](../../assets/new.svg) Aggiunta variabile di compilazione [SKIP_COMPOSER_DUMP_AUTOLOAD](../environment/variables-build.md#skip_composer_dump_autoload). L&#39;impostazione della variabile su `true` impedisce all&#39;applicazione di eseguire il comando `composer dump-autoload` durante un&#39;installazione di Cloud Docker per Commerce. La variabile è rilevante solo per Cloud Docker per contenitori Commerce con file system scrivibili (creati per il test e lo sviluppo utilizzando `./vendor/bin/ece-docker build:compose --with-test`). In tali installazioni, ignorando il comando `composer dump-autoload` si evitano errori durante l&#39;esecuzione di altri comandi che tentano di accedere ai file da una directory `generated` eliminata.<!--MCLOUD-6939-->

## v2002.1.2

Data di rilascio: 5 agosto 2020

**Miglioramenti di convalida e registro**—

- ![nuova icona](../../assets/new.svg) Aggiunto il file `schema.error.yaml` che include tutte le notifiche di errore e di avviso che possono verificarsi durante il processo di compilazione, distribuzione e post-distribuzione insieme a suggerimenti per la risoluzione degli errori. Le informazioni contenute in questo file sono disponibili anche nella _Guida a Cloud per Commerce_. Vedi [Riferimento messaggio di errore per strumenti ece](../dev-tools/error-reference.md).<!--MCLOUD-5878-->

- ![nuova icona](../../assets/new.svg) Le voci del registro errori cloud (`/var/log/cloud.error.log`) sono state modificate in formato JSON per semplificare l&#39;analisi del registro a livello di programmazione.<!--MCLOUD-5879-->

- ![nuova icona](../../assets/new.svg) Sono stati aggiunti ulteriori controlli di errore per la compilazione, la distribuzione e l&#39;elaborazione post-distribuzione e sono stati migliorati i controlli esistenti:

   - Codice di errore 2026 - Impossibile ripristinare alcuni dati generati durante la fase di build nelle directory montate

   - Codice di errore 3004 - Impossibile creare i file di backup

   - Codice errore 102 - Sono state aggiunte ulteriori verifiche per i problemi che si verificano quando il file `env.php` non è scrivibile <!--MCLOUD-6221-->

- ![nuova icona](../../assets/new.svg) Aggiunta della variabile di ambiente **QUALITY_PATCH** per specificare una o più patch di qualità da applicare durante il processo di distribuzione. Vedi [Variabili di compilazione](../environment/variables-build.md#quality_patches).<!--MCLOUD-6375-->

## v2002.1.1

Data di rilascio: 25 giugno 2020

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dell&#39;infrastruttura**—

   - ![nuova icona](../../assets/new.svg) **Miglioramenti alla registrazione**—È stata migliorata la funzionalità di tracciamento del registro assegnando codici di uscita a errori critici di distribuzione ed esponendo i codici di uscita nelle notifiche dei messaggi di errore e negli eventi di registro. Vedi [Riferimento messaggio di errore per strumenti ece](../dev-tools/error-reference.md).<!-- MCLOUD-5637, 5531-->

   - ![nuova icona](../../assets/new.svg) È stato migliorato il processo per le immagini del database (`vendor/bin/ece-tools db-dump`) e i messaggi di registro aggiornati per chiarire che l&#39;operazione di dump del database passa l&#39;applicazione alla modalità di manutenzione, interrompe i processi della coda del consumatore e disabilita i processi cron prima dell&#39;inizio dell&#39;immagine.<!--MCLOUD-5324, MCLOUD-2062-->

   - ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che impediva l&#39;aggiornamento corretto dell&#39;URL del progetto durante la distribuzione negli ambienti di staging e produzione. Ora `ece-tools` utilizza l&#39;URL per la route con l&#39;attributo `primary:true` impostato nella configurazione della route del progetto. Vedi [Distribuire le variabili](../environment/variables-deploy.md#update_urls).<!--MCLOUD-5883-->

   - ![icona correzione](../../assets/fix.svg) Aggiornato il flusso di lavoro dello scenario di compilazione `generate.xml` per l&#39;applicazione di patch. Le patch devono essere applicate prima per aggiornare Adobe Commerce e risolvere eventuali problemi che potrebbero causare errori nei passaggi `di:compile` e `module:refresh`.<!--MCLOUD-5941-->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema nel processo di installazione che restituiva erroneamente l&#39;errore `Crypt key missing`. Il valore `crypt/key` viene generato automaticamente durante l&#39;installazione.<!--MCLOUD-6120-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei servizi**—

   - ![nuova icona](../../assets/new.svg) Aggiunto supporto per PHP 7.4 e MariaDB 10.4.<!--MAGECLOUD-2957, MCLOUD-4144-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti della variabile di ambiente**—

   - ![nuova icona](../../assets/new.svg) Aggiunta della variabile **SCD_USE_BALER** per abilitare il modulo Baler per il bundle JavaScript durante il processo di compilazione dell&#39;infrastruttura cloud Adobe Commerce. Vedi la descrizione della variabile nelle [variabili build](../environment/variables-build.md#scd_use_baler).<!-- MCLOUD-3456, MCLOUD-3457-->

   - ![nuova icona](../../assets/new.svg) Aggiunta della variabile di ambiente **REDIS_BACKEND** per configurare il modello di back-end Redis per la cache Redis per Adobe Commerce 2.3.5 o versione successiva. Vedi la descrizione della variabile nelle [variabili di distribuzione](../environment/variables-deploy.md#redis_backend).<!--MCLOUD-5721, MCLOUD-5865-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti del comando CLI**—

   - ![nuova icona](../../assets/new.svg) Sono stati aggiornati i seguenti comandi CLI con un&#39;opzione per una registrazione più dettagliata:

      - `app:config:dump`
      - `app:config:import`
      - `module:enable`

     Il livello di registrazione per ogni chiamata è determinato dalla configurazione della variabile [`VERBOSE_COMMANDS`](../environment/variables-build.md#verbose_commands) nel file `.magento.env.yaml`.<!--MCLOUD-3503-->

- ![nuova icona](../../assets/new.svg) **Miglioramenti alla convalida**—

   - ![nuova icona](../../assets/new.svg) **Verifica compatibilità Elasticsearch 7.x**—Convalida Elasticsearch aggiornata per i controlli compatibilità software Elasticsearch 7.x.<!--MCLOUD-5542-->

   - ![nuova icona](../../assets/new.svg) **Versioni del servizio aggiornate e controlli di convalida EOL**—Convalida aggiornata per verificare le versioni del servizio installate rispetto ai requisiti di Adobe Commerce 2.4.<!--MCLOUD-6144-->

   - ![icona di correzione](../../assets/fix.svg) È stato risolto un problema di convalida in modo che il seguente messaggio di avviso post-distribuzione venga visualizzato solo se la configurazione dell&#39;hook `post-deploy` non è presente nel file `.magento.app.yaml`:

     ```text
     Your application does not have the "post_deploy" hook enabled.
     ```

     <!--MCLOUD-4077-->

   - ![nuova icona](../../assets/new.svg) **Aggiunta della convalida per le dipendenze di Zend Framework** - Aggiunta della convalida delle dipendenze del compositore per Zend Framework migrato al progetto Laminas. Se mancano le dipendenze richieste, durante il processo di compilazione viene visualizzato il seguente messaggio di errore.

     ```text
     Required configuration is missing from the autoload section of the composer.json file.
     Add ("Laminas\Mvc\Controller\Zend\": "setupsrc/ Zend/Mvc/Controller/") to
     the `autoload -> psr-4` section. Then, re-run the "composer update" command locally, and
     commit the updated composer.json and composer.lock files.
     ```

     Vedere [Verificare le dipendenze di Zend Framework](../development/commerce-version.md#verify-zend-framework-composer-dependencies).<!--MCLOUD-4094-->

   - ![nuova icona](../../assets/new.svg) **Aggiunta della convalida per `env.php` file e dati** - Aggiunta delle verifiche per il file `env.php` e i dati durante il processo di installazione e aggiornamento.<!--MCLOUD-5991-->

      - Se il file `env.php` non è presente nell&#39;installazione e il valore `crypt/key` non è specificato nel file `.magento.app.yaml`, la distribuzione non riesce e viene visualizzata la seguente notifica:

        ```text
        The crypt/key key value does not exist in the ./app/etc/env.php file or the CRYPT_KEY cloud environment variable``Missing crypt key for upgrading Magento`.
        ```

      - Se l&#39;installazione non include il file `env.php` o la configurazione contiene un solo tipo di cache, il comando `cron:enable` viene eseguito durante il processo di aggiornamento per ripristinare il file con tutti i `cache_types`. Al registro viene aggiunta la seguente notifica:

        ```text
        Magento state indicated as installed but configuration file app/etc/env.php was empty or did not exist.
        Required data will be restored from environment configurations and from the .magento.env.yaml file.
        ```

## v2002.1.0

Data di rilascio: 6 febbraio 2020

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dell&#39;infrastruttura**—

   - ![nuova icona](../../assets/new.svg) **È stato aggiunto un pacchetto separato per Cloud Docker per Commerce**. Il pacchetto Docker è stato separato dal pacchetto `ece-tools` per mantenere la qualità del codice e fornire versioni indipendenti. Gli aggiornamenti e le correzioni relativi a `ece-tools` sono gestiti dall&#39;archivio GitHub [magento-cloud-docker](https://github.com/magento/magento-cloud-docker).<!--MAGECLOUD-2927-->

   - ![nuova icona](../../assets/new.svg) **Funzionalità di applicazione delle patch aggiornate**—La funzionalità di applicazione delle patch è stata spostata dal pacchetto ECE-Tools a un pacchetto [magento-cloud-patches](https://github.com/magento/magento-cloud-patches) separato. Durante la distribuzione, `ece-tools` utilizza il nuovo pacchetto per applicare le patch. Consulta le [note sulla versione delle patch cloud](cloud-patches.md).<!--MAGECLOUD-4567-->

   - ![nuova icona](../../assets/new.svg) **Sono state aggiornate le dipendenze del Compositore**—È stato aggiornato il file `composer.json` per Adobe Commerce nell&#39;infrastruttura cloud con una dipendenza per il pacchetto `magento/magento-cloud-docker`. Ora `ece-tools` include le dipendenze per tutti i pacchetti in [`Cloud Tools Suite for Commerce`](cloud-tools-suite.md). Questi pacchetti vengono installati e aggiornati automaticamente quando si installa o si aggiorna `ece-tools`.

- ![nuova icona](../../assets/new.svg) **Supporto per le distribuzioni basate su scenari**—<!--MAGECLOUD-4101-->

   - ![nuova icona](../../assets/new.svg) Ora è possibile personalizzare i processi di compilazione, distribuzione e post-distribuzione utilizzando i file di configurazione XML per sostituire o personalizzare la configurazione predefinita.

   - ![nuova icona](../../assets/new.svg) **È stata modificata la configurazione `hooks` in`.magento.app.yaml`**. Il formato di configurazione `hooks` è stato aggiornato per supportare le distribuzioni basate su scenari. Il formato legacy della versione precedente di ECE-Tools 2002.0.x è ancora supportato. Tuttavia, è necessario eseguire l’aggiornamento al nuovo formato per utilizzare la funzione di distribuzione basata su scenari. Consulta [Distribuzioni basate su scenari](../deploy/scenario-based.md#add-scenarios-using-build-and-deploy-hooks).

>[!NOTE]
>
>Prima di eseguire l&#39;aggiornamento a ECE-Tools versione 2002.1.0, rivedere [indietro   modifiche non compatibili](backward-incompatible-changes.md) per informazioni sulle modifiche che potrebbero richiedere   aggiorna la configurazione o i processi del progetto di infrastruttura cloud di Adobe Commerce.

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei servizi**—

   - ![nuova icona](../../assets/new.svg) Aggiunto supporto per PHP 7.3.<!--MAGECLOUD-4022-->

   - ![nuova icona](../../assets/new.svg) Aggiunto supporto per RabbitMQ 3.8.<!--MAGECLOUD-4674-->

   - ![nuova icona](../../assets/new.svg) Aggiunta della convalida per verificare le versioni del servizio installate rispetto alla data di fine del ciclo di vita per ogni servizio. Ora i clienti ricevono una notifica se la versione di un servizio è entro tre mesi dalla data di fine del ciclo di vita e un avviso se la data di fine del ciclo di vita è nel passato.<!--MAGECLOUD-4076-->

   - ![icona di correzione](../../assets/fix.svg) È stato risolto un problema di configurazione dell&#39;Elasticsearch per garantire che le impostazioni di Elasticsearch corrette siano configurate in tutti gli ambienti.<!--MAGECLOUD-4474-->

>[!NOTE]
>
>Consulta [Versioni dei servizi](../services/services-yaml.md#service-versions) per un elenco dei servizi utilizzati in Adobe Commerce sull&#39;infrastruttura cloud e la loro compatibilità di versione con il modello Cloud.

- ![nuova icona](../../assets/new.svg) **Aggiornamenti della variabile di ambiente**—

   - ![nuova icona](../../assets/new.svg) Ha esteso la funzionalità della variabile di ambiente `WARM_UP_PAGES` per supportare il precaricamento della cache per pagine di prodotto specifiche. Vedi la definizione espansa nell&#39;argomento [variabili post-distribuzione](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-4444-->

   - ![nuova icona](../../assets/new.svg) Aggiunta variabile di ambiente `ERROR_REPORT_DIR_NESTING_LEVEL` per semplificare la gestione dei dati della segnalazione errori nella directory `<magento_root>/var/report/`. Vedi la descrizione della variabile nell&#39;argomento [variabili build](../environment/variables-build.md#error_report_dir_nesting_level).

   - ![icona di correzione](../../assets/fix.svg) Ha rimosso le variabili di ambiente `SCD_EXCLUDE_THEMES`, `STATIC_CONTENT_THREADS`,`DO_DEPLOY_STATIC_CONTENT` e `STATIC_CONTENT_SYMLINK`. Visualizza [modifiche non compatibili con le versioni precedenti](backward-incompatible-changes.md#environment-configuration-changes).<!--MAGECLOUD-4407, MAGECLOUD-3873-->

   - ![icona di correzione](../../assets/fix.svg) È stato risolto un problema nel processo di configurazione della suite elastica in modo che la configurazione predefinita venga sovrascritta come previsto quando si configura la variabile di distribuzione `ELASTICSUITE_CONFIGURATION` senza l&#39;opzione `_merge`.<!--MAGECLOUD-4388-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti del comando CLI**—

   - ![nuova icona](../../assets/new.svg) **Nuovo comando cron**. È ora possibile gestire manualmente l&#39;elaborazione cron nell&#39;ambiente Adobe Commerce nell&#39;infrastruttura cloud utilizzando i comandi `cron:disable` e `cron:enable`. Utilizzare il comando disable per arrestare tutti i processi cron attivi e disattivare tutti i processi cron. Utilizzare il comando enable per riattivare i processi cron quando si è pronti. Vedi [Disabilita processi cron](../application/crons-property.md#disable-cron-jobs).

   - ![nuova icona](../../assets/new.svg) **È stata migliorata la segnalazione degli errori**. È stata aggiunta una registrazione migliore per gli errori dei comandi CLI che si verificano durante l&#39;elaborazione ECE-Tools.<!--MAGECLOUD-4849-->

   - ![nuova icona](../../assets/new.svg) **Rimuovi i comandi di compilazione obsoleti**— Sono stati rimossi i seguenti comandi di compilazione: `m2-ece-build`, `m2-ece-deploy`, `m2-ece-scd-dump` e sono stati rinominati `ece-tools docker` comandi in `ece-docker`. Visualizza [modifiche non compatibili con le versioni precedenti](backward-incompatible-changes.md)<!--MAGECLOUD-4392-->

- ![nuova icona](../../assets/new.svg) Il file `build_options.ini` obsoleto è stato rimosso e la convalida aggiunta non è riuscita a generare la build se il file esiste. Utilizza il file [.magento.env.yaml](../environment/configure-env-yaml.md) per configurare le opzioni di compilazione.

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che ha causato un errore nel processo di compilazione quando il file `config.php` è vuoto.<!--MAGECLOUD-4127-->

## 2002.0.23.

Data di rilascio: 27 febbraio 2020

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema di compatibilità con le versioni 2002.0.x di `ece-tools` che impediva il completamento della generazione di contenuto statico su richiesta in modalità di produzione.

## Versioni precedenti

Consulta l&#39;[archivio delle note sulla versione](cloud-release-archive.md) per le versioni 2002.0.22 e precedenti.
