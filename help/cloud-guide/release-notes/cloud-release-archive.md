---
title: Archivio delle note sulla versione per gli strumenti ece
description: Scopri i miglioramenti archiviati per gli strumenti ece.
hide: true
hidefromtoc: true
recommendations: noDisplay, noCatalog
exl-id: 96663d2f-ef00-4490-ad2e-06e8041b228e
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '7147'
ht-degree: 0%

---

# Archivio delle note sulla versione per gli strumenti ece

>[!NOTE]
>
>Queste note sulla versione forniscono informazioni e aggiornamenti per `ece-tools` v2002.0.22 e versioni successive. Consulta le [note sulla versione per la suite di strumenti cloud](cloud-tools-suite.md) per ottenere gli aggiornamenti più recenti per `ece-tools` e altri pacchetti Cloud.

## v2002.0.22

La versione 2002.0.22 di `ece-tools` modifica la struttura del pacchetto `ece-tools` per separare il rilascio di `Adobe Commerce on cloud infrastructure` patch dalla versione ECE-Tools. A partire da questa versione, le patch e le correzioni critiche verranno distribuite utilizzando il pacchetto [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches), che è una nuova dipendenza per il pacchetto `ece-tools`. Abbiamo apportato queste modifiche per ridurre la complessità della pianificazione degli aggiornamenti delle versioni e dell’utilizzo dei contributi della community.

- ![nuova icona](../../assets/new.svg) **Modifiche al pacchetto ECE-Tools**

   - ![nuova icona](../../assets/new.svg) ha spostato le patch di Adobe Commerce dal pacchetto `ece-tools` a un nuovo pacchetto del compositore [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches).

   - ![nuova icona](../../assets/new.svg) È stato aggiornato il file `composer.json` per il pacchetto `ece-tools` per aggiungere una dipendenza per il pacchetto `magento/magento-cloud-patches` v1.0.0.

   - ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che causava l&#39;interruzione del processo di applicazione delle patch di `ece-tools` quando si applicavano set di patch a partire dalle versioni di sola sicurezza, a partire dalla versione 2.3.2-p2 e successive. Questo problema è stato introdotto dal nuovo schema di versioni adottato per [patch di sola sicurezza](https://devdocs.magento.com/guides/v2.3/release-notes/bk-release-notes.html#security-only-patches).<!--MAGECLOUD-4661-->

- ![icona di correzione](../../assets/fix.svg) **Patch e correzioni critiche**-Aggiorna gli ambienti Cloud con `ece-tools` versione 2002.0.22 per applicare le patch e le correzioni critiche seguenti. Queste patch sono incluse nel pacchetto `magento/magento-cloud-patches` v1.0.0.

   - ![icona di correzione](../../assets/fix.svg) **Patch di sicurezza di Page Builder per le versioni 2.3.1.x e 2.3.2.x**-Corregge un problema nell&#39;anteprima di Page Builder che consente agli utenti non autenticati di accedere ad alcuni metodi di modelli che possono essere utilizzati per attivare l&#39;esecuzione di codice arbitrario sulla rete (RCE) con conseguente perdita di informazioni globali. Questo problema può verificarsi quando si utilizzano versioni non supportate di Page Builder con Adobe Commerce versioni 2.3.1 e 2.3.2.<!--MAGECLOUD-4649-->

   - ![icona di correzione](../../assets/fix.svg) **patch MSI**-Corregge i problemi che hanno causato errori di indicizzazione e problemi di prestazioni quando si utilizzano le impostazioni di inventario predefinite per la gestione delle scorte.<!--MAGECLOUD-4428-->

   - ![icona di correzione](../../assets/fix.svg) **Compatibilità con le versioni precedenti delle nuove interfacce di posta**-Corregge un problema di incompatibilità con le versioni precedenti causato dall&#39;interfaccia PHP `Magento\Framework\Mail\EmailMessageInterface` introdotta in Adobe Commerce v2.3.3. Nell&#39;ambito di questa patch, il nuovo `EmailMessageInterface` eredita dal vecchio `MessageInterface` e i moduli core di Adobe Commerce vengono ripristinati a dipendere da `MessageInterface`.<!--MAGECLOUD-4422-->

   - ![icona di correzione](../../assets/fix.svg) **La paginazione del catalogo non funziona in Elasticsearch 6.x**-Corregge un problema critico con la paginazione dei risultati di ricerca che interessa i clienti che utilizzano Elasticsearch 6.x come motore di ricerca del catalogo.<!--MAGECLOUD-4448-->

## v2002.0.21

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - ![nuova icona](../../assets/new.svg) **Nuove immagini Docker**—Supportate dalle versioni 2.3.3 e successive<!-- MAGECLOUD-3345 -->

      - PHP versione 7.3.<!-- MAGECLOUD-4017 -->

      - Cache vernice 6.2.0<!-- MAGECLOUD-4017 -->

   - ![nuova icona](../../assets/new.svg) Aggiunta del supporto per l&#39;applicazione della configurazione hook personalizzata specificata in `.magento.app.yaml` nell&#39;ambiente Docker. In precedenza, l&#39;ambiente Docker supportava solo la configurazione hook predefinita.<!-- MAGECLOUD-3505-->

   - ![nuovi file icona](../../assets/new.svg) Docker ENV non vengono più generati durante la build Docker e il comando `docker:config:convert` è obsoleto. I dati corrispondenti sono ora archiviati nel file `docker-compose.yml`.<!-- MAGECLOUD-3816-->

   - ![nuova icona](../../assets/new.svg) **Immagine PHP aggiornata**-Aggiunta di Node.js all&#39;immagine Docker PHP per supportare le funzionalità node, npm e grunt-cli.<!-- MAGECLOUD-3953 -->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti della variabile di ambiente**-

   - ![nuova icona](../../assets/new.svg) Aggiunta variabile di distribuzione **LOCK_PROVIDER** per configurare il provider di blocchi che impedisce l&#39;avvio di processi cron e gruppi cron duplicati. Vedi la descrizione della variabile nell&#39;argomento [distribuire variabili](../environment/variables-deploy.md#lock_provider).<!-- MAGECLOUD-4052 -->

   - ![nuova icona](../../assets/new.svg) Aggiunta della variabile di ambiente **CONSUMER_WAIT_FOR_MAX_MESSAGES** per configurare il modo in cui i consumatori elaborano i messaggi dalla coda dei messaggi quando utilizzano la variabile di ambiente `CRON_CONSUMERS_RUNNER` per gestire i processi cron. Vedi la descrizione della variabile nell&#39;argomento [distribuire variabili](../environment/variables-deploy.md#consumers_wait_for_max_messages).<!-- MAGECLOUD-4071 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema che poteva causare errori di deadlock del database quando il processo cron `consumers_runner` avviava più istanze dello stesso consumer su nodi diversi. Ora, se hai abilitato la variabile di distribuzione [**CRON_CONSUMER_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) nell&#39;ambiente, il processo `consumers_runner` utilizza l&#39;opzione `single-thread` per avviare un&#39;istanza di ogni consumer su un solo nodo.<!-- MAGECLOUD-3913 -->

   - ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che interessava la funzionalità [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) che utilizza un URL di archivio predefinito. Ora, se il comando `config:show:default-url` non è in grado di recuperare un URL di base, viene utilizzato l&#39;URL dalla variabile MAGENTO_CLOUD_ROUTES.<!-- MAGECLOUD-3866 -->

- ![nuova icona](../../assets/new.svg) Sono state aggiornate le informazioni di registrazione restituite dal comando `module:refresh`. Ora è possibile visualizzare un elenco dettagliato dei moduli abilitati nel file `cloud.log`.<!-- MAGECLOUD-2514 -->

- ![nuova icona](../../assets/new.svg) È stata migliorata la convalida della compatibilità della versione e le notifiche di avviso per i problemi di compatibilità tra la versione di Adobe Commerce e i servizi installati, ad esempio Elasticsearch, [!DNL RabbitMQ], Redis e DB.<!-- MAGECLOUD-3535 -->

- ![nuova icona](../../assets/new.svg) Aggiunto supporto per RabitMQ versione 3.8.<!-- MAGECLOUD-4674-->

- ![nuova icona](../../assets/new.svg) Sono state aggiornate le convalide interattive per la compatibilità dei servizi in modo da riflettere le versioni supportate per le nuove versioni di Adobe Commerce 2.3.3 e 2.2.10. Per le versioni consigliate, vedere [Requisiti di sistema](https://devdocs.magento.com/guides/v2.4/install-gde/system-requirements.html) nella _Guida all&#39;installazione_.<!-- MAGECLOUD-4018 -->

- ![icona correzione](../../assets/fix.svg) È stato migliorato il messaggio di registro restituito quando il processo di gestione dei processi cron nella fase di distribuzione tenta di arrestare un processo cron già terminato per chiarire che il problema non è un errore. Il livello di registro è stato modificato da `INFO` a `DEBUG`.<!-- MAGECLOUD-3653-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che si verificava durante l&#39;esecuzione del comando `setup:upgrade` e che non interrompeva il processo di distribuzione in caso di errore durante l&#39;attività `app:config:import`.<!-- MAGECLOUD-3806 -->

- ![nuova icona](../../assets/new.svg) Il livello di registro predefinito per il gestore di file è stato modificato in `debug` per ridurre la quantità di dettagli nel registro visualizzato in [!DNL Cloud Console], fornendo comunque informazioni dettagliate per il debug.<!-- MAGECLOUD-3871 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore nella distribuzione di contenuto statico durante la compilazione. Dopo un&#39;installazione e un dump di configurazione `ece-tools`, si è verificato un errore se non sono state specificate impostazioni locali per l&#39;utente amministratore nel file `config.php`. Ora esiste una lingua predefinita per l&#39;utente amministratore nel file `config.php`.<!-- MAGECLOUD-3957 -->

- ![icona correzione](../../assets/fix.svg) È stato corretto un `Undefined index error` che si verifica quando un comando CLI `magento-cloud` non riesce in un ambiente non configurato con un URL protetto (https). Ora il pacchetto ECE-Tools utilizza l&#39;URL di base (http) se l&#39;URL protetto non è disponibile.<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - ![nuova icona](../../assets/new.svg) Ora puoi eseguire test funzionali utilizzando il pacchetto `ece-tools` nell&#39;ambiente Docker. Vedi [test applicazioni](https://devdocs.magento.com/cloud/docker/docker-test-magecloud-pkg-code.html).<!-- MAGECLOUD-3129/3684 -->

   - ![nuova icona](../../assets/new.svg) Aggiunto supporto per la configurazione dei moduli PHP tramite il file `.magento.app.yaml`. Tutte le estensioni [PHP specificate nel file `.magento.app.yaml`](https://devdocs.magento.com/cloud/project/magento-app-php-application.html#php-extensions) diventano disponibili nei contenitori PHP Docker.<!-- MAGECLOUD-3357 -->

   - ![nuova icona](../../assets/new.svg) Sono disponibili nuovi comandi per migliorare l&#39;esperienza della riga di comando Docker. Vedere la sezione [`bin/magento-docker` del riferimento Docker](https://devdocs.magento.com/cloud/docker/docker-quick-reference.html#magento-cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![nuova icona](../../assets/new.svg) Aggiunta la possibilità di utilizzare Mutagen.io per sincronizzare i file durante lo sviluppo tra l&#39;host locale e Docker.<!-- MAGECLOUD-3559 -->

   - ![icona correzione](../../assets/fix.svg) È stato corretto il percorso predefinito quando si utilizza l&#39;ambiente Docker. Ora, quando si utilizza SSH per accedere al contenitore Docker, ci si trova nella directory principale del progetto nella directory `/app`, come previsto.<!-- MAGECLOUD-3582 -->

   - ![icona correzione](../../assets/fix.svg) Aggiornamento della libreria Sodio dalla versione 1.0.11 alla versione 1.0.18 e aggiornamento dell&#39;estensione PHP Sodio.<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >I clienti di Adobe Commerce su infrastrutture cloud devono [Inviare un ticket di supporto Adobe Commerce](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket) per aggiornare il pacchetto libsodium negli ambienti di produzione e staging Pro prima di eseguire l&#39;aggiornamento ad Adobe Commerce 2.3.2. Attualmente, non è possibile aggiornare gli ambienti Starter ad Adobe Commerce 2.3.2.

   - ![icona correzione](../../assets/fix.svg) Aggiunti i plug-in di Elasticsearch `analysis-icu` e `analysis-phonetic` a tutte le immagini Docker.<!-- MAGECLOUD-3446 -->

   - ![icona correzione](../../assets/fix.svg) Convalide migliorate: quando si utilizzano le opzioni per il comando `docker:build`, è necessario fornire un valore quando si utilizza un&#39;opzione. È stata inoltre aggiunta la convalida per la versione del nodo quando si utilizza il comando `docker:build run`.<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti della variabile di ambiente**—

   - ![nuova icona](../../assets/new.svg) Aggiunta del supporto per i prefissi delle tabelle di database utilizzando la [variabile di ambiente DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!-- MAGECLOUD-2901 -->

   - ![nuova icona](../../assets/new.svg) Aggiunta della variabile di distribuzione **FORCE_UPDATE_URLS** per aggiornare gli URL di base durante la distribuzione negli ambienti di produzione e staging di Pro e Starter. Vedi la definizione nel contenuto delle [variabili di distribuzione](../environment/variables-deploy.md#force_update_urls).<!-- MAGECLOUD-3602 -->

   - ![nuova icona](../../assets/new.svg) Aggiunta della variabile di post-distribuzione **TTFB_TESTED_PAGES** per configurare _Tempo al primo byte_ test di pagina per verificare le prestazioni dell&#39;applicazione sui siti distribuiti nell&#39;infrastruttura cloud. Vedi la descrizione della variabile in [variabili post-distribuzione](../environment/variables-post-deploy.md).<!-- MAGECLOUD-3643 -->

   - ![icona di correzione](../../assets/fix.svg) È stato risolto un problema relativo all&#39;SCD multithread, che causava errori casuali nella distribuzione di contenuto statico. La soluzione consiste nell&#39;impostare la variabile **SCD_THREADS** su `1`. Ora puoi aumentare il conteggio in base alle esigenze. Vedi le definizioni nelle [variabili di distribuzione](../environment/variables-deploy.md#scd_threads) e nelle [variabili di compilazione](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->

   - ![icona di correzione](../../assets/fix.svg) È possibile configurare la variabile di ambiente **WARM_UP_PAGES** per memorizzare nella cache singole pagine, più domini e più pagine. Vedi la definizione espansa nel contenuto delle [variabili post-distribuzione](../environment/variables-post-deploy.md#warm_up_pages).<!-- MAGECLOUD-3258 -->

- ![icona correzione](../../assets/fix.svg) Il file `pub/static/.htaccess` è stato aggiunto all&#39;elenco di esclusione. [Correzione inviata da Björn Kraus di PHOENIX MEDIA GmbH](https://github.com/magento/ece-tools/pull/455).<!-- MAGECLOUD-3545/Github#455 -->

- ![icona correzione](../../assets/fix.svg) È stato corretto un errore che si verificava quando tutti i messaggi di convalida venivano visualizzati come `Critical` se almeno una convalida di livello critico restituiva un errore.<!-- MAGECLOUD-3178 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore di distribuzione se l&#39;URL di base non esisteva nel database.<!-- MAGECLOUD-3075 -->

- ![nuova icona](../../assets/new.svg) Aggiunta di un nuovo comando **`env:config:show`** al pacchetto `ece-tools` che visualizza servizi di ambiente, route o variabili. Vedere [Servizi, route e variabili](https://devdocs.magento.com/cloud/reference/ece-tools-reference.html#services-routes-and-variables). [Funzionalità inviata da Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore critico durante il tentativo di installare Adobe Commerce 2.2.6 o versioni precedenti con `ece-tools` sviluppare dopo il refactoring della shell.<!-- MAGECLOUD-3665 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava il mancato funzionamento delle installazioni di Adobe Commerce 2.1.x e 2.2.x con un avviso relativo all&#39;utilizzo di una versione obsoleta di Carbon.<!-- MAGECLOUD-3704 -->

- ![icona correzione](../../assets/fix.svg) Il livello di registro `cloud.log` per l&#39;output della shell è stato ridotto da `info` a `debug`.<!-- MAGECLOUD-3277 -->

- ![icona correzione](../../assets/fix.svg) Aggiunta dell&#39;opzione `--remove-definers (-d)` al comando `ece-tools db-dump` per rimuovere i definitori dal file di dump.<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che sovrascrive il file `env.php` durante una distribuzione, causando la perdita di configurazioni personalizzate. Questo aggiornamento garantisce che Adobe Commerce su infrastruttura cloud aggiorni il file `env.php` a ogni distribuzione, mantenendo al contempo le configurazioni personalizzate.<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - ![nuova icona](../../assets/new.svg) Ora l&#39;ambiente Docker supporta la configurazione cron definita nella proprietà [crons del file .magento.app.yaml](https://devdocs.magento.com/cloud/project/magento-app-properties.html#crons).<!-- MAGECLOUD-3150 -->

   - ![nuova icona](../../assets/new.svg) **Nuovo contenitore Docker**. Aggiunta di un [contenitore proxy di terminazione TLS](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container) per facilitare la terminazione SSL di Vernice tramite HTTPS.<!-- MAGECLOUD-2890 -->

   - ![nuova icona](../../assets/new.svg) **Nuova immagine Docker**—Aggiunta di un&#39;immagine Node.js per supportare Gulp e altre funzionalità, ad esempio Test di unità JS Jasmine.<!-- MAGECLOUD-3345 -->

   - ![nuova icona](../../assets/new.svg) **Modalità build Docker**. Ora puoi scegliere di avviare l&#39;ambiente Docker in [Modalità produzione o Modalità sviluppatore](https://devdocs.magento.com/cloud/docker/docker-launch.html#set-the-launch-mode). La modalità Sviluppatore supporta lo sviluppo attivo con autorizzazioni di file system complete e scrivibili.<!-- MAGECLOUD-3152/3511 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema che ha causato un errore di distribuzione Docker con `Name or service not known` se la cache è configurata per un servizio non disponibile. È ora possibile rimuovere un servizio dal file [`.magento/services.yaml`](https://devdocs.magento.com/cloud/project/services.html). Il generatore di configurazione Docker aggiorna automaticamente il servizio nel file `docker/config.php.dist`.<!-- MAGECLOUD-3369 -->

   - ![nuova icona](../../assets/new.svg) Sono state aggiunte convalide interattive per la compatibilità del servizio. Ora, se un servizio richiesto non è compatibile con la versione di Adobe Commerce o con altri servizi, la _modalità interattiva_ richiede all&#39;utente un messaggio e la scelta di continuare. Vedi le [versioni del servizio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-containers) disponibili per Docker. Utilizzare l&#39;opzione `-n` per ignorare l&#39;interattività a scopo di CICD.<!-- MAGECLOUD-3251 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema con il comando Docker compose `db-dump` che cancellava le immagini esistenti.<!-- MAGECLOUD-3366 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema che assegnava l&#39;archiviazione della cache Redis `session`, `default` e `page_cache` allo stesso ID database.<!-- MAGECLOUD-3172 -->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti della variabile di ambiente**—

   - ![nuova icona](../../assets/new.svg) La nuova variabile di ambiente **ELASTICSUITE\_CONFIGURATION** mantiene le impostazioni del servizio personalizzate tra le distribuzioni. Vedi la definizione nel contenuto delle [variabili di distribuzione](../environment/variables-deploy.md#elasticsuite_configuration).<!-- MAGECLOUD-3205 -->

   - ![nuova icona](../../assets/new.svg) Aggiunta della variabile di ambiente **SCD_MAX_EXECUTION_TIMEOUT** per aumentare il tempo necessario per completare la distribuzione del contenuto statico dal file `.magento.env.yaml`. Vedi la definizione nelle [variabili di distribuzione](../environment/variables-deploy.md#scd_max_execution_time), [variabili di compilazione](../environment/variables-build.md#scd_max_execution_time) e [variabili globali](../environment/variables-global.md#scd_max_execution_time).<!-- MAGECLOUD-2822 -->

      - ![nuova icona](../../assets/new.svg) Aggiunta della variabile di ambiente **MAGENTO_CLOUD_LOCKS_DIR** per configurare il percorso del punto di montaggio per il provider di blocchi nell&#39;infrastruttura cloud. Il provider di blocchi impedisce l&#39;avvio di processi cron e gruppi cron duplicati. Questa variabile è supportata in Adobe Commerce versione 2.2.5 e successive e viene configurata automaticamente. Vedi la definizione in [Variabili cloud](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->

      - ![icona di correzione](../../assets/fix.svg) ha modificato i valori predefiniti della variabile di ambiente **SCD_THREADS** per determinare automaticamente il valore ottimale in base al numero di thread della CPU rilevati. Vedi le definizioni aggiornate nelle [variabili di distribuzione](../environment/variables-deploy.md#scd_threads) e nelle [variabili di compilazione](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3382 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema con una patch per DB Isolation Mechanism che causava un errore durante l&#39;aggiornamento ad Adobe Commerce on Cloud Infrastructure versione 2002.0.16.<!-- MAGECLOUD-3383 -->

- ![icona correzione](../../assets/fix.svg) Aggiunta di una patch che sostituisce _Grafici immagine di Google_ con _Grafici immagine_. Vedere l&#39;articolo di DevBlog [Google Image Charts deprecation and update for M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006).<!-- MAGECLOUD-3456 -->

- ![icona correzione](../../assets/fix.svg) Aggiunta della convalida per la [variabile SEARCH_CONFIGURATION](../environment/variables-deploy.md#search_configuration). La distribuzione non riesce se l&#39;opzione &#39;engine&#39; non è impostata e `_merge` non è richiesto.<!-- MAGECLOUD-3470 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava l&#39;esposizione di dati sensibili dopo l&#39;evento. Ora le informazioni riservate sono mascherate in modo appropriato.<!-- MAGECLOUD-3525 -->

- ![icona correzione](../../assets/fix.svg) Sono state migliorate le impostazioni di tolleranza d&#39;errore del pacchetto di Magento Open Source. Nel caso in cui Adobe Commerce non sia in grado di leggere i dati dall&#39;istanza Redis `slave`, viene eseguita una lettura dall&#39;istanza Redis `master`. Vedi [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>La versione 2002.0.17 di `ece-tools` include un&#39;importante patch di sicurezza. Consulta [Risorse tecniche: patch di Magento Open Source](https://magento.com/tech-resources/download#download2288).

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei servizi**—Supportato dalle seguenti versioni di Adobe Commerce: 2.2.8 e successive 2.2.x, 2.3.1 e successive 2.3.x

   - È stato aggiunto il supporto per Elasticsearch versione 6.x.<!-- MAGECLOUD-3196 -->

   - Aggiunto supporto per Redis versione 5.0.

- ![nuova icona](../../assets/new.svg) **Nuove immagini Docker**—Sono stati aggiunti i seguenti servizi alla build Docker:

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![nuova icona](../../assets/new.svg) **Nuova variabile di ambiente**. In precedenza, si era verificato un timeout hardcoded per la compressione SCD. Ora puoi configurare il timeout di compressione SCD utilizzando la variabile di ambiente **SCD_COMPRESSION_TIMEOUT**. Vedi le definizioni nelle [variabili build](../environment/variables-build.md#scd_compression_timeout) e nel contenuto delle [variabili di distribuzione](../environment/variables-deploy.md#scd_compression_timeout).<!-- MAGECLOUD-2870 -->

- ![icona correzione](../../assets/fix.svg) Aggiunta dell&#39;opzione `--use-rewrites` al comando install per consentire l&#39;utilizzo delle riscritture del server Web per i collegamenti generati nella vetrina e dell&#39;accesso amministratore per migliorare la sicurezza e l&#39;esperienza del cliente.<!-- MAGECLOUD-3246 -->

- ![icona correzione](../../assets/fix.svg) Aggiunta di marche temporali al file `var/log/install_upgrade.log` per visualizzare le date degli eventi di installazione e aggiornamento.<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - Ora la configurazione predefinita del servizio generata nell&#39;ambiente Docker è uguale alla configurazione predefinita nel modello Cloud.<!-- MAGECLOUD-3025 -->

   - È possibile inviare messaggi dal proprio ambiente Docker utilizzando il servizio `sendmail`.<!-- MAGECLOUD-2907 -->

   - Aggiunta la possibilità di [configurare Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html) per il debug nell&#39;ambiente Cloud Docker.<!-- MAGECLOUD-2891 -->

   - È stato risolto un problema relativo alle autorizzazioni del servizio Web durante la generazione del file `docker-compose.yml`.<!-- MAGECLOUD-2883 -->

- ![nuova icona](../../assets/new.svg) **Miglioramento dell&#39;aggiornamento**. È stata aggiunta la convalida per confermare che la proprietà `autoload` nel file `composer.json` contiene le modifiche di configurazione richieste prima dell&#39;aggiornamento ad Adobe Commerce v2.3. Vedi [Versione aggiornamento](https://devdocs.magento.com/cloud/project/project-upgrade.html).<!-- MAGECLOUD-2392 -->

- ![nuova icona](../../assets/new.svg) Il processo di compressione nella distribuzione del contenuto statico ora include tutte le risorse, generate in modo nativo o personalizzate, e si verifica durante la fase di compilazione all&#39;inizio della sezione [`build:transfer`](https://devdocs.magento.com/cloud/project/magento-app-properties.html#hooks). In precedenza, il processo di compressione si verificava prima di applicare la minimizzazione personalizzata e il bundling di risorse statiche. [Correzione inviata da Rafael Garcia Lepper di Tryzens Limited](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![icona correzione](../../assets/fix.svg) È stato corretto un errore di connessione al database che si verificava durante la distribuzione subito dopo la configurazione di una relazione di servizio e database aggiuntiva. Inoltre, questa correzione risolve un problema che si verificava durante il processo di configurazione di Commerce Reporting for Starter. Per Starter, questo aggiornamento è un &quot;must have&quot; per l&#39;utilizzo di Commerce Reporting.<!-- MAGECLOUD-3035 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema di convalida della configurazione del database che ha causato un errore nel processo di distribuzione.<!-- MAGECLOUD-3003 -->

- ![icona correzione](../../assets/fix.svg) Il vincolo è stato aggiornato con la versione appropriata del pacchetto `symfony/yaml` da utilizzare con [costanti PHP](https://devdocs.magento.com/cloud/project/magento-env-yaml.html#php-constants). L&#39;analisi costante non funziona quando si utilizza una versione del pacchetto `symfony/yaml` precedente alla 3.2. [Correzione inviata da Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![nuova icona](../../assets/new.svg) **Verifica della configurazione dell&#39;ambiente**—Aggiunta della convalida per controllare la versione PHP e avvisare gli utenti se non utilizzano la versione consigliata più recente.<!--MAGECLOUD-2903-->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema nell&#39;elaborazione di variabili JSON non valide. Ora, se una variabile JSON causa un errore di sintassi, nel file `cloud.log` viene visualizzato un avviso e la distribuzione continua utilizzando la variabile predefinita.<!-- MAGECLOUD-2851 -->

- ![icona correzione](../../assets/fix.svg) È stato corretto un errore di connessione che si verificava durante la distribuzione subito dopo la disattivazione del servizio Redis.<!-- MAGECLOUD-2747 -->

- ![nuova icona](../../assets/new.svg) **Registrazione delle modifiche**—È stato aggiornato il [livello di registro](../environment/log-handlers.md#log-levels) da `Info` a `Notice` per i seguenti eventi di processo di compilazione e distribuzione:<!--MAGECLOUD-2925-->

   - Inizio e fine del processo di riconciliazione dei moduli installati in `composer.json` con le impostazioni di configurazione condivise nel file `app/etc/config.php`

   - Inizio e fine del processo di convalida della configurazione

   - Inizio e fine del processo `setup:di:compile` per la generazione delle classi

- ![nuova icona](../../assets/new.svg) **Nuove variabili di ambiente**—

   - **[Variabile di distribuzione RESOURCE_CONFIGURATION](../environment/variables-deploy.md#resource_configuration)**. Utilizzare questa variabile per associare un nome di risorsa a una connessione di database.<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[Variabile globale X_FRAME_CONFIGURATION](../environment/variables-global.md#x_frame_configuration)**. Utilizzare questa variabile per modificare la configurazione dell&#39;intestazione `X-Frame-Options` per il rendering di una pagina Adobe Commerce in `<frame>`, `<iframe>` o `<object>`.<!-- MAGECLOUD-3048 -->

- ![icona di correzione](../../assets/fix.svg) **Aggiornamenti della variabile di ambiente**—Sono state modificate le seguenti variabili di ambiente:

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)** - Aggiunta la possibilità di precaricare la cache per le pagine specificate su tutti i domini definiti per un archivio Adobe Commerce. In precedenza, se il sito era configurato con più domini, il processo post-distribuzione non è riuscito a precaricare la cache per le pagine specificate in domini non predefiniti e ha restituito il seguente errore nel registro post-distribuzione: `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL** - Aggiornamento della documentazione e del file `.magento.env.yaml` di esempio con i valori predefiniti corretti per il livello di compressione SCD. Vedi le definizioni nelle [variabili build](../environment/variables-build.md#scd_compression_level) e nel contenuto delle [variabili di distribuzione](../environment/variables-deploy.md#scd_compression_level).<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES** - Questa variabile di ambiente è obsoleta. Utilizza [SCD_MATRIX](../environment/variables-build.md#scd_matrix) per controllare la configurazione del tema.<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX**—È stato corretto il processo di convalida per evitare un problema che si verificava quando SCD_MATRIX ignorava un valore di tema che conteneva casi di caratteri diversi. Vedi le definizioni nelle [variabili build](../environment/variables-build.md#scd_matrix) e nel contenuto delle [variabili di distribuzione](../environment/variables-deploy.md#scd_matrix).<!-- MAGECLOUD-2904 -->

   - **Variabili ADMIN**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - È stata migliorata la sicurezza durante la gestione delle credenziali per l’utente Admin utilizzando le variabili di ambiente. Non è più possibile utilizzare le variabili di ambiente ADMIN_EMAIL, ADMIN_USERNAME e ADMIN_PASSWORD per sostituire le credenziali admin durante gli aggiornamenti. Se non è possibile accedere al pannello di amministrazione, utilizzare la funzionalità _Password dimenticata_ o il comando CLI `admin:user:create` per creare un nuovo utente amministratore. Consulta [Accedere al tuo pannello di amministrazione](https://devdocs.magento.com/cloud/onboarding/onboarding-tasks.html#admin).

      - ADMIN_EMAIL non è più richiesto per l&#39;aggiornamento o l&#39;applicazione delle patch.

## v2002.0.15

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - Ora il generatore Docker utilizza i servizi specificati nei file di configurazione `.magento.app.yaml` e `.magento/services.yaml` durante la [creazione dell&#39;ambiente Docker](https://devdocs.magento.com/cloud/docker/docker-config.html). È possibile scegliere una versione del servizio diversa utilizzando i parametri di compilazione.<!-- MAGECLOUD-2888 -->

   - Aggiunta immagine PHP 7.2 - Aggiunta del supporto per PHP 7.2 in Cloud Docker; aggiornamento della [configurazione Launch Docker](https://devdocs.magento.com/cloud/docker/docker-config.html) per includere l&#39;opzione `docker:build --php` per specificare la versione di PHP compatibile con la versione di Adobe Commerce in uso.<!-- MAGECLOUD-2799 -->

   - È stato aggiunto un [contenitore Cron](https://devdocs.magento.com/cloud/docker/docker-containers-cli.html#cron-container) basato sull&#39;immagine PHP-CLI.<!-- MAGECLOUD-2565 -->

   - Sono stati aggiunti i seguenti servizi alla build Docker:

      - [!DNL RabbitMQ] 3.5 e 3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7, 2.4 e 5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2 e 4.0<!-- MAGECLOUD-2886 -->

- ![nuova icona](../../assets/new.svg) **Configura con costanti PHP**—È stato aggiunto il supporto per [costanti PHP](https://devdocs.magento.com/cloud/project/magento-env-yaml.html#php-constants) nel file di configurazione `.magento.env.yaml`.<!-- MAGECLOUD- 2575 -->

- ![nuova icona](../../assets/new.svg) **Nuova variabile di ambiente**. Per impostazione predefinita, solo l&#39;ambiente di produzione dispone di Google Analytics abilitate. È possibile abilitare le Google Analytics negli ambienti di gestione temporanea e integrazione utilizzando la variabile di ambiente [ENABLE_GOOGLE_ANALYTICS](../environment/variables-deploy.md#enable_google_analytics).<!--MAGECLOUD-2879-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava la rimozione delle configurazioni cron personalizzate dal file `env.php` dopo una ridistribuzione. Ora le configurazioni cron personalizzate rimangono nel file `env.php`.<!-- MAGECLOUD-2923 -->

- ![icona correzione](../../assets/fix.svg) Sono state corrette le incoerenze nei messaggi e nei [livelli di registro](../environment/log-handlers.md#log-levels) per le fasi di compilazione, distribuzione e post-distribuzione. Sono stati aumentati i livelli dei messaggi del registro iniziale e finale da **info** a **notice** per tutte le fasi e le sottofasi. Sono stati aggiunti i messaggi di registro iniziale e finale, se appropriato.<!-- MAGECLOUD-2919 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema relativo ai processi cron che impediva l&#39;avvio della fase di post-distribuzione, se configurata. Ora, se l&#39;hook post-distribuzione è abilitato, i processi cron vengono nuovamente attivati all&#39;inizio della fase post-distribuzione.<!-- MAGECLOUD-2862 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che impediva la corretta installazione di Adobe Commerce durante la specifica di una configurazione di database personalizzata. In precedenza, il processo di installazione utilizzava la configurazione del database dalla variabile [MAGENTO_CLOUD_RELATIONSHIP](../environment/variables-cloud.md) anche se sono state designate informazioni di connessione personalizzate nella [variabile di ambiente DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->

- ![icona correzione](../../assets/fix.svg) Il comando `config:dump` è stato corretto in modo da includere tutte le impostazioni locali del sito Web nella sezione `system` del file `config.php`.<!--MAGECLOUD-2740-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava _errori di riscaldamento_ durante la fase di post-distribuzione, correggendo il riferimento all&#39;URL di base dell&#39;origine.<!--MAGECLOUD-2797-->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che generava file in modo errato durante il processo `setup:di:compile`, che interessava il modulo Amazon Pay.<!--MAGECLOUD-2850-->

## v2002.0.14

- ![nuova icona](../../assets/new.svg) **Verifica stato ideale**. La procedura guidata `ideal-state` verifica ora la configurazione corrente durante ogni distribuzione e fornisce istruzioni chiare per l&#39;aggiornamento della configurazione in modo da ottenere una distribuzione più rapida e senza tempi di inattività.<!--MAGECLOUD-2372-->

- ![icona correzione](../../assets/fix.svg) **Conformità PCI**: i protocolli di messaggistica per Adobe Commerce sull&#39;infrastruttura cloud sono stati aggiornati per richiedere la versione 1.2 di Transport Layer Security (TLS) per la connessione con servizi di messaggistica di terze parti. Se utilizzi un servizio messaggi che non supporta la versione 1.2 di TLS, devi aggiornare il servizio. In caso contrario, viene visualizzato il seguente messaggio di errore quando l&#39;applicazione Adobe Commerce tenta di connettersi al server dei messaggi per inviare un&#39;e-mail: `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![nuova icona](../../assets/new.svg) **Miglioramento della distribuzione**. Aggiunta della convalida per avvisare i clienti se in un ambiente di gestione temporanea o di produzione sono abilitate `dev`, `debug` o `debug_logging` opzioni per evitare problemi di prestazioni causati da un&#39;attività di registrazione eccessiva.<!--MAGECLOUD-2517-->

- ![icona correzione](../../assets/fix.svg) **Correzioni distribuzione**—

   - Ora la modalità di manutenzione è abilitata all’inizio della fase di distribuzione e disabilitata alla fine. Se la distribuzione non riesce, il sito rimane in modalità di manutenzione fino alla risoluzione dei problemi di distribuzione. In precedenza, il sito è tornato in modalità di produzione anche se la distribuzione non è riuscita.<!--MAGECLOUD-2603-->

      - I controlli di convalida della fase di distribuzione sono stati rielaborati per ridurre il livello di errore per i seguenti problemi di distribuzione da `CRITICAL` a `WARNING` in modo che la distribuzione venga completata. In precedenza, questi problemi causavano un errore di distribuzione.

      - La configurazione dell’ambiente contiene valori errati per le variabili di distribuzione o cloud.

   - La versione di Elasticsearch sull’infrastruttura cloud non è compatibile con la versione del modulo elasticsearch/elasticsearch supportata da Adobe Commerce sull’infrastruttura cloud. Consulta l&#39;[Elasticsearch sulla risoluzione dei problemi](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento) nella Knowledge Base di supporto Adobe Commerce.<!--MAGECLOUD-2600-->

   - È stato risolto un problema con le impostazioni di configurazione condivise nel file `app/etc/config.php` che causava `recursion detected` errori durante la distribuzione.<!--MAGECLOUD-2173-->

- ![icona di correzione](../../assets/fix.svg) **Correzioni relative alla correzione**—

   - È stato risolto un problema di pianificazione cron che impediva l&#39;esecuzione dei processi se si specificava una frequenza cron diversa da quella predefinita (1 minuto).<!--MAGECLOUD-2602-->

   - È stato risolto un problema nella fase di distribuzione che consentiva ai processi cron di continuare a essere in esecuzione durante la distribuzione, causando blocchi del database e altri problemi critici. Ora, tutti i processi cron vengono interrotti prima dell’inizio della fase di distribuzione e riavviati al termine della stessa.&lt;!—MAGECLOUD—2537—>

   - È stato corretto il flusso di lavoro dei processi cron nelle versioni 2.2.x per sbloccare i processi cron bloccati in modo che potessero essere interrotti prima di iniziare la distribuzione. In precedenza, un processo cron bloccato causava l&#39;interruzione della distribuzione.<!--MAGECLOUD-2501-->

- ![icona correzione](../../assets/fix.svg) Il formato del file `config.php` generato dal comando `vendor/bin/ece-tools config:dump` è stato modificato in modo da utilizzare la sintassi di matrice breve e il rientro a 4 spazi per rispettare gli standard di codifica Adobe Commerce.<!--MAGECLOUD-2527-->

- ![icona correzione](../../assets/fix.svg) È stato corretto un errore di distribuzione che si verificava quando `.magento.env.yaml` conteneva `{{ base_url }}` e `{{ unsecure_base_url }}` segnaposto per le configurazioni web invece della configurazione URL predefinita per un progetto di infrastruttura cloud di Adobe Commerce./<!--MAGECLOUD-2607-->

## v2002.0.13

- ![nuova icona](../../assets/new.svg) **Abilita la distribuzione senza tempi di inattività**. Ora Adobe Commerce sull&#39;infrastruttura cloud accoda le richieste con le modifiche del database richieste durante la distribuzione e applica le modifiche non appena la distribuzione viene completata. Le richieste possono essere trattenute per un massimo di 5 minuti per evitare la perdita di sessioni. Consulta [Opzioni di distribuzione del contenuto statico per ridurre i tempi di inattività della distribuzione in Cloud](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud).<!--MAGECLOUD-2169-->

- ![nuova icona](../../assets/new.svg) **Componi Docker per cloud**—Sono stati apportati i seguenti miglioramenti al processo di [Configurazione e configurazione Docker](https://devdocs.magento.com/cloud/docker/docker-config.html):

   - È stato aggiunto il comando—`docker:config:convert` per convertire i file di configurazione PHP in formato Docker ENV per semplificare la configurazione dell&#39;ambiente. Ora è possibile copiare i file di configurazione PHP nella directory Docker e convertirli in file ENV Docker. Vedi [Avvia Docker](https://devdocs.magento.com/cloud/docker/docker-config.html).<!--MAGECLOUD-2359-->

   - Il processo di installazione dell’infrastruttura cloud di Adobe Commerce ora supporta la distribuzione sia su file system di sola lettura che di lettura-scrittura per emulare più strettamente il file system cloud. Vedere [Configurare Docker](https://devdocs.magento.com/cloud/docker/docker-config.html).&lt;!—MAGECLOUD—2357—>

   - **Supporto del servizio Redis**: aggiunta di un&#39;immagine Redis, distribuita in un contenitore Docker e configurata automaticamente per l&#39;utilizzo con l&#39;installazione Docker.&lt;!—MAGECLOUD—2442—>

   - Ora disponi della funzionalità di dump del database quando utilizzi il contenitore [database](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#database-container) di Cloud Docker. È inoltre possibile [condividere file](https://devdocs.magento.com/cloud/docker/docker-containers.html#sharing-data-between-host-machine-and-container) tra un computer host e un contenitore utilizzando la directory `docker/mnt`.<!-- MAGECLOUD-2577 -->

   - **Supporto del servizio di vernice**: aggiunta di un&#39;immagine vernice, distribuita automaticamente in un contenitore Docker. Dopo l’implementazione, puoi configurare manualmente Vernice seguendo le best practice di Adobe Commerce. Consulta [Configurare e utilizzare Vernice](https://devdocs.magento.com/guides/v2.3/config-guide/varnish/config-varnish.html).&lt;!—MAGECLOUD—2358—>

   - Accesso sicuro al sito: è stato aggiunto il supporto SSL per accedere allo store Adobe Commerce e al pannello di amministrazione.&lt;!—MAGECLOUD—2360—>

- ![icona correzione](../../assets/fix.svg) **Miglioramento del supporto per l&#39;estensione dell&#39;infrastruttura cloud di Adobe Commerce**—Downgrade del requisito di versione minima per il pacchetto guzzlehttp/guzzle in Adobe Commerce sull&#39;infrastruttura cloud [file compositore.json](https://devdocs.magento.com/cloud/reference/cloud-composer.html) alla versione 6.2 in modo che il pacchetto `ece-tools` sia compatibile con più estensioni.<!--MAGECLOUD-2205-->

- ![nuova icona](../../assets/new.svg) **Applica modifiche personalizzate all&#39;applicazione Adobe Commerce durante la fase di compilazione**. La fase di compilazione è stata suddivisa in due processi distinti in modo che sia possibile utilizzare gli hook per applicare modifiche personalizzate al contenuto statico generato prima di creare il pacchetto dell&#39;applicazione per la distribuzione. Il processo _build:generate_ genera codice, applica patch e genera contenuto statico. Il processo _build:transfer_ trasferisce il codice generato e il contenuto statico alla destinazione finale. Vedi [Hook applicazione](https://devdocs.magento.com/cloud/project/magento-app-properties.html#hooks).<!--MAGECLOUD-2363-->

- ![icona di correzione](../../assets/fix.svg) **Controlli di configurazione dell&#39;ambiente**—È stata migliorata la convalida della configurazione dell&#39;ambiente per avvisare i clienti delle incompatibilità di versione e degli errori di configurazione prima di creare e distribuire Adobe Commerce sull&#39;infrastruttura cloud.

   - È stata aggiunta la convalida specifica della versione per identificare variabili e valori di ambiente non supportati o obsoleti.<!--MAGECLOUD-2183-->

   - È stata aggiunta una verifica di compatibilità di Elasticsearch per avvisare gli utenti dei problemi di configurazione di Elasticsearch. Ora, la distribuzione non riesce se la versione del servizio Elasticsearch sul server è incompatibile con Adobe Commerce. In precedenza, la distribuzione era riuscita anche se la versione dell&#39;Elasticsearch non era compatibile e ciò ha causato problemi al catalogo prodotti dopo la distribuzione del sito.<!--MAGECLOUD-2389-->

     È possibile risolvere l&#39;incompatibilità [inviando un ticket di supporto](https://devdocs.magento.com/cloud/trouble/trouble.html) per aggiornare l&#39;Elasticsearch a una versione compatibile oppure modificare la configurazione di Adobe Commerce per specificare una versione compatibile del client PHP di Elasticsearch.

      - Per Adobe Commerce dalla versione 2.1.x alla versione 2.2.2, aggiornare Elasticsearch alla versione 2.4.

      - Per Adobe Commerce versione 2.2.3 e successive, aggiorna Elasticsearch alla versione 5.2.

      - Se disponi di Elasticsearch 1.x o 2.x e non desideri aggiornarlo, aggiorna il requisito della versione del client Adobe Commerce Elasticsearch PHP in compositore.json a `"elasticsearch/elasticsearch": "~2.0"`.

   - È stata migliorata la convalida delle variabili di ambiente per identificare le impostazioni di configurazione che possono causare conflitti durante le fasi di build, distribuzione e post-distribuzione. Ad esempio, durante il processo di installazione e aggiornamento viene visualizzato un messaggio di avviso se l&#39;impostazione globale per la distribuzione di contenuto statico è in conflitto con le impostazioni della fase di compilazione o distribuzione.<!--MAGECLOUD-2156-->

- ![icona di correzione](../../assets/fix.svg) **Aggiornamenti della variabile di ambiente**—Sono state modificate le seguenti variabili di ambiente:

   - **[Variabile globale SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification)** - Il valore predefinito è stato modificato in `true` per abilitare la minimizzazione dei contenuti HTML su richiesta, riducendo al minimo i tempi di inattività durante la distribuzione negli ambienti di staging e produzione. Questa configurazione è necessaria per le distribuzioni senza tempi di inattività.<!--MAGECLOUD-2435-->

   - **[CLEAN_STATIC_FILES: distribuzione variabile](../environment/variables-deploy.md#clean_static_files)** - Aggiunta della funzionalità di gestione dell&#39;elaborazione dei file statici puliti per il contenuto statico generato durante la fase di compilazione in base all&#39;impostazione della variabile di ambiente CLEAN_STATIC_FILES. In precedenza, i file di contenuto statico generati durante la fase di compilazione venivano sempre puliti.<!--MAGECLOUD-1506-->

- ![icona correzione](../../assets/fix.svg) **Registrazione**—Sono state apportate le seguenti modifiche per migliorare i messaggi del registro e ridurne le dimensioni:

   - Le voci del registro degli errori di distribuzione ora includono l’output del comando dalle operazioni che causano gli errori anche se la configurazione dell’ambiente non specifica la registrazione a livello di debug. Vedi [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - È stata aggiunta la registrazione per gli errori di distribuzione che si verificano quando le factory generate richieste da alcune estensioni non possono essere generate correttamente perché il file system è in stato di sola lettura.<!--MAGECLOUD-2209-->

   - Sono state ridotte le dimensioni del registro di distribuzione e sono stati risolti i problemi di formattazione causati dai comandi di installazione che utilizzano la barra di avanzamento interattiva.<!--MAGECLOUD-2402-->

   - Eliminazione della complessità non necessaria e aggiornamento dei livelli di priorità per alcune istruzioni di registro.<!--MAGECLOUD-2227-->

- ![icona di correzione](../../assets/fix.svg) **Correzioni specifiche per la regola**—

   - Le impostazioni predefinite di configurazione del processo cron per la durata della cronologia sono state modificate da 3d (4320 minuti) a 1h (60 minuti) per evitare problemi di prestazioni ed errori di distribuzione che possono verificarsi quando la coda cron si riempie troppo rapidamente.<!--MAGECLOUD-2427-->

   - È stato migliorato il processo di gestione dei processi cron durante la fase di distribuzione per evitare blocchi del database e altri problemi critici. Ora tutti i processi cron vengono interrotti durante la fase di distribuzione e riavviati al termine della distribuzione.<!--MAGECLOUD-2445-->

   - È stato risolto un problema con il meccanismo di blocco per la pianificazione dei consumer avviati dai processi cron in Adobe Commerce versioni 2.2.0 e successive per impedire che i processi cron avviino consumer duplicati.<!--MAGECLOUD-2464-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema relativo al [processo di compressione del contenuto statico](../environment/variables-intro.md) (`gzip`) che causava `not overwritten` e `no such file or directory` errori quando si faceva riferimento al file compresso durante il processo di distribuzione.<!-- MAGECLOUD-2182-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che impediva al comando `php ./vendor/bin/ece-tools config:dump` di rimuovere sezioni ridondanti dal file `config.php` durante il processo di dump se non sono specificate le impostazioni locali dell&#39;archivio. Ora è possibile spostare facilmente i file di configurazione tra ambienti diversi. Dopo l&#39;aggiornamento a `ece-tools` v2002.0.13, rigenerare i file `config.php` precedenti con il comando `config:dump` migliorato. Consulta [Gestione configurazione per le impostazioni dell&#39;archivio](https://devdocs.magento.com/cloud/live/sens-data-over.html).<!--MAGECLOUD-2444-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore durante la fase di distribuzione se la configurazione della route nel file `.magento/routes.yaml` reindirizzava da un dominio [apex](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/) a un dominio `www`.<!--MAGECLOUD-2556-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema con l&#39;opzione `_merge` per la variabile [`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) che causava risultati di unione errati se non si includeva il parametro `engine` nel file di configurazione `.magento.env.yaml` aggiornato. Ora l&#39;operazione di unione sovrascrive correttamente solo i valori specificati nell&#39;aggiornamento di `.magento.env.yaml` senza richiedere l&#39;impostazione del parametro `engine`.<!--MAGECLOUD-2520-->

- ![icona di correzione](../../assets/fix.svg) è stato risolto un problema di configurazione Redis che abilitava in modo errato il blocco delle sessioni per Adobe Commerce nelle versioni 2.2.1 e successive dell&#39;infrastruttura cloud, causando un rallentamento delle prestazioni e timeout. Ora, il blocco della sessione è disattivato per impostazione predefinita. Il problema è stato causato da una modifica del comportamento predefinito del parametro `disable_locking` introdotto nella versione 1.3.4 del pacchetto del gestore di sessione Redis. Vedi [pacchetto ](https://github.com/colinmollenhour/php-redis-session-abstract) di colinmollenhour/php-redis-session-abstract<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![nuova icona](../../assets/new.svg) **Componi Docker per cloud**—Aggiunta di un comando—`docker:build`—per generare una configurazione [Componi Docker](https://devdocs.magento.com/cloud/docker/docker-config.html) dall&#39;archivio `ece-tools` del cloud.<!-- MAGECLOUD-2250 -->

- ![nuova icona](../../assets/new.svg) **Cambia impostazioni internazionali**—Ora è possibile modificare le impostazioni locali dell&#39;archivio senza il processo di configurazione di esportazione e importazione. Quando l&#39;applicazione è in produzione e SCD_ON_DEMAND è abilitato, sono disponibili le opzioni locali dell&#39;archivio e dell&#39;amministratore.<!-- MAGECLOUD-2019 -->

- ![nuova icona](../../assets/new.svg) <!-- MAGECLOU-1998 -->**Mappa del sito e robot**—Creato un [flusso di lavoro](../store/robots-sitemap.md) per aggiungere un file `robots.txt` e generare un file `sitemap.xml` per una configurazione di dominio singolo senza richiedere una modifica all&#39;infrastruttura.

- ![nuova icona](../../assets/new.svg) **Procedure guidate**—Sono state aggiunte due [procedure guidate](../deploy/smart-wizards.md) per la configurazione cloud:<!-- MAGECLOUD-1910 -->

   - `ideal-state`: configurare lo stato ideale per ridurre al minimo i tempi di inattività dell&#39;implementazione

   - `master-slave`: configurare il bilanciamento del carico per il database e Redis

- ![nuova icona](../../assets/new.svg) **Aggiornamento modulo**—Aggiunta di un comando Cloud—`module:refresh`—per abilitare i moduli disabilitati o non abilitati in modo esplicito, in modo analogo a come viene eseguito automaticamente durante una compilazione.<!-- MAGECLOUD-1521 -->

- ![nuova icona](../../assets/new.svg) Aggiunta la possibilità di scegliere di unire o sovrascrivere la configurazione per i servizi utilizzando l&#39;opzione `_merge` nelle configurazioni [CACHE](../environment/variables-deploy.md#cache_configuration), [SESSION](../environment/variables-deploy.md#session_configuration), [QUEUE](../environment/variables-deploy.md#queue_configuration) e [SEARCH](../environment/variables-deploy.md#search_configuration).<!-- MAGECLOUD-2105 -->

- ![nuova icona](../../assets/new.svg) **File di esempio per la configurazione dell&#39;ambiente**—È stato aggiunto al pacchetto ECE-Tools un file di esempio `.magento.env.yaml` che include una descrizione dettagliata e i possibili valori per ogni variabile di ambiente.<!-- MAGECLOUD-1908 -->

   - È stata inoltre aggiunta una convalida approfondita per la configurazione `.magento.env.yaml` che impedisce errori nel processo di distribuzione causati da valori imprevisti. Quando si verifica un errore, viene visualizzato un messaggio di errore dettagliato che inizia con: `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![nuova icona](../../assets/new.svg) Aggiunte le seguenti [**Variabili di ambiente**](../environment/variables-intro.md):

   - Ora è possibile definire più impostazioni locali per ogni tema utilizzando la nuova variabile di ambiente [SCD_MATRIX](../environment/variables-deploy.md#scd_matrix), che riduce la quantità di file del tema da distribuire.<!-- MAGECLOUD-1501 -->

   - È stata aggiunta la variabile di ambiente [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) per personalizzare le connessioni al database per la distribuzione.<!-- MAGECLOUD-2047 -->

   - La nuova variabile [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) sostituisce il livello di registrazione minimo per tutti i flussi di output senza apportare modifiche al codice.<!-- MAGECLOUD-2129 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che causava tempi di inattività tra la fase di distribuzione e quella successiva. Ora la fase di post-distribuzione inizia _immediatamente_ al termine della fase di distribuzione.

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che non eliminava dalla pianificazione i processi cron riusciti, quelli con `status = success`.<!-- MAGECLOUD-2268 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema con l&#39;hook `post_deploy` che cancellava la cache nella fase di distribuzione invece della fase di post-distribuzione del progetto.<!-- MAGECLOUD-2113 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che si verificava quando si utilizzava SCD con più impostazioni locali, che generavano lo stesso file `js-translation.json` per ciascuna impostazione locale.<!-- MAGECLOUD-2034 -->

- ![icona correzione](../../assets/fix.svg) Ottimizzato il comando `db:dump` nel pacchetto `ece-tools` per evitare il blocco delle tabelle e aumentare la velocità.<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>La versione ECE-Tools 2002.0.11 è necessaria per la compatibilità con la versione 2.2.4.

- ![nuova icona](../../assets/new.svg) **Configurazione delle connessioni di sola lettura ai nodi non master**. Questa versione consente di configurare una connessione di sola lettura a un nodo non master per ricevere traffico di sola lettura (per [MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)).<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection) e per <!--MAGECLOUD-143 -->

- ![nuova icona](../../assets/new.svg) **Configurazione guidata**—Aggiunta di una procedura guidata per verificare la configurazione per la distribuzione di contenuto statico. Visualizza [Procedure guidate avanzate](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![nuova icona](../../assets/new.svg) **Supporto della console Symfony**—Aggiunta del supporto per la console Symfony 4 con Adobe Commerce 2.3.<!-- MAGECLOUD-1966 -->

- ![icona correzione](../../assets/fix.svg) **Ottimizzazioni di pianificazione Cron**—È stata migliorata la gestione della coda e la registrazione avanzata per facilitare il debug dei problemi correlati alle cron.<!-- MAGECLOUD-1607 -->

- ![icona correzione](../../assets/fix.svg) La convalida della distribuzione non riesce se un valore `ADMIN_EMAIL` o `ADMIN_USERNAME` è uguale a un account amministratore esistente.<!-- MAGECLOUD-1221 -->

- ![icona di correzione](../../assets/fix.svg) Supporto SOLR rimosso per le versioni 2.2.x. Le versioni 2.1.x mantengono la possibilità di abilitare SOLR<!-- MAGECLOUD-1282 -->

- ![icona di correzione](../../assets/fix.svg) La prima installazione degli ambienti di staging e produzione di un progetto PRO ora include diversi prefissi di indice, ad Elasticsearch per evitare possibili conflitti durante l&#39;identificazione dei record appartenenti a ciascun ambiente.<!-- MAGECLOUD-1489 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che interrompeva la fase di compilazione per l&#39;architettura legacy durante la distribuzione del contenuto statico.<!-- MAGECLOUD-2021 -->

- ![icona di correzione](../../assets/fix.svg) **Miglioramenti specifici per la regola**—L&#39;implementazione cron è stata rielaborata:<!-- MAGECLOUD-1607 -->

   - È stato risolto un problema che causava il riempimento rapido della coda cron. Ora cancella i lavori di cron obsoleti in modo più affidabile.

   - È stata riorganizzata la sequenza di job cron in modo che tutti i job in thread separati vengano avviati prima del gruppo generale.

   - È stata migliorata la registrazione per facilitare il debug dei problemi relativi ai cron.

   - **NOTA**—Questa versione risolve molti problemi correlati ai cron. Se al momento utilizzi alcune patch relative a cron in _m2-hotfix_, rimuovile.

- ![icona correzione](../../assets/fix.svg) **miglioramenti specifici per SCD**—

   - È possibile utilizzare le variabili di ambiente `VERBOSE_COMMANDS` e `SCD_COMPRESSION_LEVEL` durante entrambe le fasi _build_ e de_ploy.<!-- MAGECLOUD-1819 -->

   - È stato risolto un problema che causava un errore di distribuzione casuale quando si verificava un valore imprevisto per la variabile di ambiente `SCD_COMPRESSION_LEVEL`. È stata migliorata la convalida della configurazione per fornire notifiche significative. Vedere [`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level) per i valori accettabili.<!-- MAGECLOUD-2043 -->

   - È stato corretto il comportamento del flusso di configurazione della variabile di ambiente `SCD_COMPRESSION_LEVEL` in modo che le sostituzioni funzionino come previsto.<!-- MAGECLOUD-2044 -->

   - È stato risolto un problema che impediva la configurazione della variabile di ambiente `SCD_THREADS` nella fase _deploy_ del file `.magento.env.yaml`.<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![nuova icona](../../assets/new.svg) **Distribuzione di contenuti statici (SCD)**: è disponibile un nuovo processo di distribuzione alternativo per generare contenuto statico quando richiesto (su richiesta). In questo modo si riducono i tempi di inattività e si migliora la gestione della cache generando le risorse più critiche.<!-- MAGECLOUD-1285 -->

   - **Nuova variabile di ambiente** - Aggiunta della variabile di ambiente globale `SCD_ON_DEMAND` per generare contenuto statico quando richiesto.<!-- MAGECLOUD-1738 -->

   - **Hook di distribuzione Post** - Aggiunto un hook `post_deploy` per il file `.magento.app.yaml` che cancella la cache e precarica (riscalda) la cache _dopo_ che il contenitore inizia ad accettare le connessioni. È disponibile solo per i progetti Pro che contengono ambienti di staging e produzione in [!DNL Cloud Console] e per i progetti iniziali. Anche se non obbligatorio, questo funziona in tandem con la variabile di ambiente `SCD_ON_DEMAND`.<!-- MAGECLOUD-1788 -->

- ![nuova icona](../../assets/new.svg) **Ottimizzazione**: ottimizzazione dello spostamento o della copia di file durante la distribuzione per migliorare la velocità di distribuzione e ridurre i carichi nel file system.<!-- MAGECLOUD-1842 -->

- ![nuova icona](../../assets/new.svg) **Registrazione distribuzione** - È stata aggiunta la possibilità di abilitare i gestori di registro esteso Syslog e Graylog (GELF) per l&#39;output dei registri durante il processo di distribuzione. Vedi [Gestori di registrazione](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![nuova icona](../../assets/new.svg) Aggiunte le seguenti [**Variabili di ambiente**](../environment/variables-intro.md):

   - `CRYPT_KEY` - Fornire una chiave di crittografia a un altro ambiente durante lo spostamento di un database.<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_Variabile di ambiente globale_ che ignora la copia dei file di visualizzazione statica nella directory `var/view_preprocessed` e genera HTML minimizzato quando richiesto.<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_Variabile di ambiente globale_ per generare contenuto statico quando richiesto.<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES` - È possibile elencare le pagine da utilizzare per precaricare la cache. Disponibile nelle nuove [variabili di distribuzione di Post](../environment/variables-post-deploy.md).

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema a causa del quale una patch applicata localmente interrompeva la distribuzione in un&#39;istanza. Ora ECE-Tools è in grado di rilevare che è stata applicata una patch.<!-- MAGECLOUD-982 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un conflitto tra il bundling di JavaScript e la funzionalità GZIP. Queste funzionalità ora funzionano correttamente insieme.<!-- MAGECLOUD-1735 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che causava il mancato funzionamento dei comandi CLI di ECE-Tools quando si utilizzavano versioni precedenti di PHP 7.0.x.<!-- MAGECLOUD-1744 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che impediva la distribuzione di contenuto statico con la strategia di compattazione in più thread.<!-- MAGECLOUD-1822 -->

- ![icona di correzione](../../assets/fix.svg) è stato risolto un problema di blocco della sessione Redis che causava un ritardo di accesso dell&#39;amministratore. Inoltre, la correzione è disponibile per 2.1.x.<!-- MAGECLOUD-1853 -->

## v2002.0.9

>[!NOTE]
>
>Devi [aggiornare il metapacchetto infrastruttura cloud di Adobe Commerce](../dev-tools/install-package.md#update-the-metapackage) per ottenere questo e tutti gli aggiornamenti futuri.

- ![nuova icona](../../assets/new.svg) **strumenti ece**—Il pacchetto `ece-tools` ora supporta Adobe Commerce 2.1.x.<!-- MAGECLOUD-1086 -->

- ![nuova icona](../../assets/new.svg) **Configurazione Redis**—È ora possibile [configurare la pagina Redis](../environment/variables-deploy.md#cache_configuration) e la cache predefinita e l&#39;archiviazione della sessione Redis utilizzando una variabile di ambiente.<!-- MAGECLOUD-1552 -->

- ![nuova icona](../../assets/new.svg) **Miglioramenti al servizio Search, AMQP e Redis** - Abbiamo unificato il flusso di configurazione del servizio in modo che ora si comporti allo stesso modo per tutti i servizi. La modifica manuale del file `env.php` per la configurazione dei servizi non è più supportata. È necessario utilizzare le variabili di ambiente o il file `.magento.env.yaml`.<!-- MAGECLOUD-1437 -->

- ![icona correzione](../../assets/fix.svg) **Variabili di ambiente**—

   - L&#39;utilizzo di `env:STATIC_CONTENT_THREADS` è stato dichiarato obsoleto e verrà rimosso in una versione futura. Utilizza invece [SCD_THREADS](../environment/variables-deploy.md#scd_threads).<!-- MAGECLOUD-1507 -->

   - La variabile di ambiente `STATIC_CONTENT_EXCLUDE_THEMES` è stata dichiarata obsoleta. Utilizzare la variabile di ambiente `SCD_EXCLUDE_THEMES`.<!-- MAGECLOUD-1640 -->

- ![icona di correzione](../../assets/fix.svg) **Registrazione**. Sono state semplificate le operazioni di applicazione di patch incorporate.<!-- MAGECLOUD-1674 -->

- ![icona correzione](../../assets/fix.svg) Il supporto della modalità `developer` e la variabile di ambiente `APPLICATION_MODE` sono stati rimossi perché causavano un comportamento imprevisto.<!-- MAGECLOUD-1615 -->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che causava errori di distribuzione del contenuto statico relativi a Redis. Ora la distribuzione di contenuto statico multithread viene eseguita come previsto.<!-- MAGECLOUD-1630 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che impediva agli utenti di salvare le modifiche ai campi di configurazione nell&#39;amministratore, contrassegnati come sensibili dopo l&#39;esecuzione del comando `app:config:dump`.<!-- MAGECLOUD-1175 -->

- ![icona di correzione](../../assets/fix.svg) È stato aggiunto il supporto per una versione precedente di `symfony/yaml` per correggere i conflitti con alcuni pacchetti non ancora compatibili con la versione più recente.<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>`vendor/magento/ece-patches` è stato unito a `vendor/magento/ece-tools` in questa versione. Non è più necessario aggiornare il pacchetto `vendor/magento/ece-patches` separatamente.

**Nuove funzionalità:**

- **Registrazione migliorata**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - Abbiamo migliorato la funzione di log messaging per fornire spiegazioni migliori quando il processo di build o distribuzione sovrascrive una variabile di ambiente.
   - Ora puoi visualizzare in tempo reale lo stato di avanzamento dell’installazione e dell’aggiornamento. Suddividi il file `install_update.log` per visualizzare l&#39;avanzamento. Ad esempio:

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **Nuovo comando cron**. È ora possibile sbloccare specifici processi cron bloccati anziché arrestarli e riavviarli tutti con il comando [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451). Non disponibile in 2.1.<!-- MAGECLOUD-1367 -->

- **File di configurazione unificato**. È ora possibile configurare le fasi di compilazione e distribuzione utilizzando un file [`.magento.env.yaml`](https://devdocs.magento.com/cloud/project/magento-env-yaml.html).<!-- MAGECLOUD-1369 -->

- **Backup dei file di configurazione**. Il processo di distribuzione crea automaticamente un backup dei file di configurazione `app/etc/env.php` e `app/etc/config.php` dopo la distribuzione. È stato aggiunto anche un [nuovo comando CLI](https://support.magento.com/hc/en-us/articles/360033182871) per ripristinare questi file di configurazione da un backup.<!-- MAGECLOUD-1372 -->

- **Risoluzione dei problemi relativi agli errori di convalida**. È stato modificato il comando da utilizzare per risolvere gli errori di convalida quando `config.php` non contiene dati sufficienti per la distribuzione di contenuto statico. In precedenza, il messaggio di errore indicava di eseguire `bin/magento app:config:dump`. Ora è necessario eseguire `php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->

- **Nuove variabili di ambiente** - È ora possibile utilizzare le variabili di ambiente per connettere al sito i servizi personalizzati [search](../environment/variables-deploy.md#search_configuration) e [basati su AMQP](../environment/variables-deploy.md#queue_configuration).<!-- MAGECLOUD-1410 -->

- È stata implementata l’applicazione di patch intelligenti. Ora il pacchetto applica patch basate non sulla versione dell&#39;infrastruttura cloud Adobe Commerce, ma sulla versione del pacchetto con patch.<!--MAGECLOUD-1090-->

**Problemi risolti:**

- È stato risolto un problema di registrazione che causava errori di compilazione.<!-- MAGECLOUD-1162 -->

- È stato risolto un problema che causava eccezioni di timeout durante l&#39;esecuzione di distribuzioni in modalità interattiva.<!-- MAGECLOUD-1389 -->

- È stato risolto un problema che causava errori durante l’utilizzo della strategia compatta per la generazione di contenuti statici. Non disponibile in 2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485-->

- È stato risolto un problema che impediva allo script di distribuzione di identificare correttamente gli ambienti di staging e produzione.<!-- MAGECLOUD-1493 -->

- È stato risolto un problema che causava l&#39;interruzione delle connessioni al database da parte di problemi di rete e causava errori durante il processo di installazione e aggiornamento.<!-- MAGECLOUD-1520 -->

- È stato risolto un problema che impediva l&#39;esportazione dei file di configurazione utilizzando `app:config:dump` più di una volta. Non disponibile in 2.1.<!--  MAGECLOUD-1567  -->

- È stato risolto un problema della sessione Redis _blocco_ che causava un ritardo di accesso di _Amministratore_. Non disponibile in 2.1.<!--  MAGECLOUD-1582  -->

- È stato risolto un problema di implementazione relativo al controllo delle versioni che causava un conflitto con altri moduli di applicazione di patch basati su Compositore.<!-- MAGECLOUD-1450 -->

- È stato risolto un problema che causava problemi di memoria PHP durante l&#39;importazione.<!-- MAGECLOUD-1310 -->

- È stata rimossa la patch; è stato corretto un bug in `colinmollenhour/credis` v1.6 per abilitare il supporto per Adobe Commerce sull&#39;infrastruttura cloud 2.2.1. Non disponibile in 2.1.<!-- MAGECLOUD-1033 -->

## v2002.0.7

**Problemi risolti:**

- È stato rimosso il collegamento simbolico `var/view_preprocessed` per risolvere un problema che causava conflitti di minimizzazione di JavaScript.<!-- MAGECLOUD-1454 -->

## v2002.0.6

**Problemi risolti:**

- È stato risolto un problema che causava `gzip` errori quando un nome di file o directory conteneva spazi.<!-- MAGECLOUD-1413 -->

- È stato risolto un problema che impediva agli script di distribuzione di riconoscere e abilitare correttamente le dipendenze dei moduli.<!-- MAGECLOUD-1424 -->

## v2002.0.5

**Nuove funzionalità:**

- **Configurare un consumer cron con una variabile di ambiente**. È ora possibile configurare i consumer cron utilizzando la nuova variabile di ambiente `CRON_CONSUMERS_RUNNER`.

- **Analisi configurazione**: è ora possibile eseguire la ricerca di componenti critici durante il processo di compilazione/distribuzione e arrestare il processo se la scansione non riesce, evitando inutili tempi di inattività dovuti alla modalità di manutenzione del sito.

- **Notifiche di compilazione/distribuzione** - È stato aggiunto un file di configurazione che puoi utilizzare per [configurare le notifiche di Slack e/o e-mail](../environment/set-up-notifications.md) per le azioni di compilazione/distribuzione in tutti gli ambienti.

- **Compressione del contenuto statico**. Il contenuto statico verrà compresso utilizzando [gzip](https://www.gnu.org/software/gzip/) durante le fasi di compilazione e distribuzione. Questa compressione, associata alla compressione Fastly, consente di ridurre le dimensioni dello store e di aumentare la velocità di installazione. Se necessario, è possibile disabilitare la compressione utilizzando un&#39;opzione [build](../environment/variables-build.md) o una [variabile di distribuzione](../environment/variables-deploy.md). Per ulteriori informazioni, consulta i seguenti argomenti:

   - [Variabili di ambiente dell’applicazione](../application/variables-property.md)

   - [Prestazioni di distribuzione del contenuto statico](../deploy/static-content.md)

   - [Processo di distribuzione](https://devdocs.magento.com/cloud/reference/discover-deploy.html)

- **Gestione della configurazione** - Generazione automatica di un file `app/etc/config.php` nell&#39;archivio Git durante la fase di compilazione se non esiste già. Il file generato automaticamente include solo un elenco di moduli ed estensioni. Se il file esiste già, la fase di build continua come di consueto. Se segui [Gestione configurazione](../store/store-settings.md) in un secondo momento, i comandi aggiornano il file senza richiedere passaggi aggiuntivi. Per ulteriori informazioni, consultare [Processo di distribuzione](https://devdocs.magento.com/cloud/reference/discover-deploy.html).

- **Dump del database**. È stato aggiunto un comando CLI `magento/ece-tools` per la creazione di dump del database in tutti gli ambienti. Per gli ambienti di produzione Pro plan, questo comando esegue il dump solo da uno dei tre nodi ad alta disponibilità, pertanto i dati di produzione scritti in un nodo diverso durante il dump potrebbero non essere copiati. È consigliabile attivare la modalità di manutenzione dell’applicazione prima di eseguire un’immagine del database negli ambienti di produzione. Per ulteriori informazioni, vedere [Gestione backup](https://devdocs.magento.com/cloud/project/project-webint-snap.html#db-dump).

- **Limitazioni dell&#39;intervallo Cron revocate** - L&#39;intervallo cron predefinito per tutti gli ambienti con provisioning nelle aree us-3, eu-3 e ap-3 è di 1 minuto. L’intervallo cron predefinito in tutte le altre aree geografiche è di 5 minuti per gli ambienti di integrazione Pro e di 1 minuto per gli ambienti di staging e produzione Pro. Per modificare i processi cron esistenti, modificare le impostazioni in `.magento.app.yaml` o creare un ticket di supporto per gli ambienti di produzione/staging. Per ulteriori informazioni, consulta [Configurare i processi cron](../application/crons-property.md#set-up-cron-jobs).

**Problemi risolti:**

- È stato risolto un problema che causava tempi di distribuzione lunghi a causa del processo di distribuzione che richiamava l&#39;operazione `cache-clean` prima della distribuzione del contenuto statico.<!-- MAGECLOUD-1327 -->

- È stato risolto un problema che causava errori durante il passaggio di generazione del contenuto statico della distribuzione negli ambienti di produzione.<!-- MAGECLOUD-1322 -->

- È stato risolto un problema che impediva ad alcuni comandi `magento/ece-tools` di registrare l&#39;output in `stderr`.<!-- MAGECLOUD-1264 -->

- È stato risolto un problema che impediva l&#39;aggiornamento dei valori dell&#39;URL di base in `env.php` nei rami con fork.<!-- MAGECLOUD-1242 -->

- È stato risolto un problema a causa del quale il comando `magento setup:install` aggiungeva un prefisso non sicuro (`http://`) agli URL di base protetti.<!-- MAGECLOUD-1171 -->

- È stato risolto un problema che impediva agli errori di patch di causare errori di distribuzione.<!-- MAGECLOUD-1170 -->

- È stato risolto un problema che impediva a `ece-tools` di interrompere l&#39;esecuzione e di generare un&#39;eccezione se non era possibile applicare patch.<!-- MAGECLOUD-1152 -->

- È stato risolto un problema che causava errori durante il caricamento della vetrina dopo l&#39;abilitazione della minimizzazione di HTML in Admin.<!-- MAGECLOUD-1138 -->

## v2002.0.4

**Problemi risolti:**

- È ora possibile [reimpostare manualmente i processi cron bloccati](https://support.magento.com/hc/en-us/articles/360033099451) utilizzando un comando CLI in tutti gli ambienti tramite accesso SSH. Il processo di distribuzione reimposta automaticamente i processi cron.<!-- MAGECLOUD-1355 -->

## v2002.0.3

**Problemi risolti:**

- È stato risolto un problema che causava il timeout delle pagine perché Redis impiegava troppo tempo per la lettura/scrittura. È ora possibile utilizzare il parametro `disable_locking` nelle configurazioni Redis per evitare questo problema.<!-- MAGECLOUD-1311 -->

## v2002.0.2

**Problemi risolti:**

- Il processo di configurazione di [!DNL RabbitMQ] ottiene automaticamente tutti i parametri richiesti.<!-- MAGECLOUD-1246 -->

## v2002.0.1

**Nuove funzionalità:**

- Adobe Commerce su infrastruttura cloud ora supporta ambiti e [strategie di distribuzione dei contenuti statici](https://devdocs.magento.com/guides/v2.3/config-guide/cli/config-cli-subcommands-static-deploy-strategies.html). È stato aggiunto il parametro `–s` con impostazione predefinita `quick` per la strategia di distribuzione del contenuto statico. Puoi utilizzare la variabile di ambiente [SCD_STRATEGY](../environment/variables-deploy.md) per personalizzare e utilizzare queste strategie con le azioni di compilazione e distribuzione. Questa variabile supporta le opzioni `standard`, `quick` o `compact`. Se si seleziona `compact`, il valore `STATIC_CONTENT_THREADS` verrà sovrascritto con `1`, il che può rallentare la distribuzione, soprattutto negli ambienti di produzione. Non disponibile in 2.1.<!--- MAGECLOUD-1057 -->

- È stato creato un file di registro sugli ambienti per acquisire e compilare le azioni di build e distribuzione. Il file `var/log/cloud.log` si trova nella directory dell&#39;applicazione radice.<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**Problemi risolti:**

- È stato eseguito il refactoring del pacchetto `ece-tools` per renderlo compatibile con Adobe Commerce sull&#39;infrastruttura cloud 2.2.0 e versioni successive.<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- È stato risolto un problema che impediva a `ece-tools` di interrompere l&#39;esecuzione e di generare un&#39;eccezione se non era possibile applicare patch.<!-- MAGECLOUD-1186 -->

- È stato risolto un problema che causava la generazione di eccezioni quando la compilazione di iniezione di dipendenza (di) veniva ignorata durante le build.<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- È stato risolto un problema che causava la sovrascrittura delle configurazioni Redis personalizzate nel file `env.php` da parte del processo di distribuzione.<!-- MAGECLOUD-1019 -->

- È stato risolto un problema che causava loop di reindirizzamento a causa della disabilitazione dell&#39;amministratore protetto predefinito.<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>Questo pacchetto non è più compatibile con altre versioni di Adobe Commerce sull&#39;infrastruttura cloud e **non deve essere utilizzato**.

### Versione iniziale

Versione iniziale di `ece-tools` per Adobe Commerce sull&#39;infrastruttura cloud 2.2.0.
