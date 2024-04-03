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
>Queste note sulla versione forniscono informazioni e aggiornamenti per `ece-tools` v2002.0.22 e versioni successive. Consulta [Note sulla versione della suite di strumenti cloud](cloud-tools-suite.md) per ottenere gli aggiornamenti più recenti per `ece-tools` e altri pacchetti Cloud.

## v2002.0.22

Il `ece-tools` La versione 2002.0.22 modifica la struttura della `ece-tools` pacchetto per disaccoppiare la versione di `Adobe Commerce on cloud infrastructure` le patch del rilascio ECE-Tools. A partire da questa versione, le patch e le correzioni critiche verranno distribuite utilizzando [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) pacchetto, una nuova dipendenza per `ece-tools` pacchetto. Abbiamo apportato queste modifiche per ridurre la complessità della pianificazione degli aggiornamenti delle versioni e dell’utilizzo dei contributi della community.

- ![nuova icona](../../assets/new.svg) **Modifiche al pacchetto ECE-Strumenti**

   - ![nuova icona](../../assets/new.svg) Le patch di Adobe Commerce sono state spostate dal `ece-tools` pacchetto in un nuovo [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) pacchetto del compositore.

   - ![nuova icona](../../assets/new.svg) È stato aggiornato il `composer.json` file per `ece-tools` pacchetto per aggiungere una dipendenza per `magento/magento-cloud-patches` v1.0.0.

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava la `ece-tools` processo di applicazione delle patch da interrompere quando si applicano set di patch a partire dalle versioni di sola protezione, a partire dalla versione 2.3.2-p2 e successive. Questo problema è stato introdotto dal nuovo schema di versioni adottato per [patch di sicurezza](https://devdocs.magento.com/guides/v2.3/release-notes/bk-release-notes.html#security-only-patches).<!--MAGECLOUD-4661-->

- ![icona correzione](../../assets/fix.svg) **Patch e correzioni critiche**- Aggiornare gli ambienti cloud con `ece-tools` versione 2002.0.22 per applicare le seguenti patch e correzioni critiche. Queste patch sono incluse nel `magento/magento-cloud-patches` v1.0.0.

   - ![icona correzione](../../assets/fix.svg) **Patch di sicurezza di Page Builder per le versioni 2.3.1.x e 2.3.2.x**-È stato risolto un problema nell&#39;anteprima di Page Builder che consente agli utenti non autenticati di accedere ad alcuni metodi di modelli che possono essere utilizzati per attivare l&#39;esecuzione di codice arbitrario sulla rete (RCE) con conseguente perdita di informazioni globali. Questo problema può verificarsi quando si utilizzano versioni non supportate di Page Builder con Adobe Commerce versioni 2.3.1 e 2.3.2.<!--MAGECLOUD-4649-->

   - ![icona correzione](../../assets/fix.svg) **Patch MSI**-Corregge i problemi che causavano errori di indicizzazione e problemi di prestazioni quando si utilizzavano le impostazioni di inventario predefinite per la gestione delle scorte.<!--MAGECLOUD-4428-->

   - ![icona correzione](../../assets/fix.svg) **Compatibilità con le versioni precedenti delle nuove interfacce di posta**-Corregge un problema di incompatibilità con le versioni precedenti causato da `Magento\Framework\Mail\EmailMessageInterface` Interfaccia PHP introdotta in Adobe Commerce v2.3.3. Nell’ambito di questa patch, il nuovo `EmailMessageInterface` eredita dal vecchio `MessageInterface`, e i moduli core di Adobe Commerce dipendono nuovamente da `MessageInterface`.<!--MAGECLOUD-4422-->

   - ![icona correzione](../../assets/fix.svg) **La paginazione del catalogo non funziona su Elasticsearch 6.x**-Corregge un problema critico relativo all&#39;impaginazione dei risultati di ricerca che interessa i clienti che utilizzano Elasticsearch 6.x come motore di ricerca del catalogo.<!--MAGECLOUD-4448-->

## v2002.0.21

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - ![nuova icona](../../assets/new.svg) **Nuove immagini Docker**- Supportato dalle versioni 2.3.3 e successive<!-- MAGECLOUD-3345 -->

      - PHP versione 7.3.<!-- MAGECLOUD-4017 -->

      - Cache vernice 6.2.0<!-- MAGECLOUD-4017 -->

   - ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto per applicare la configurazione hook personalizzata specificata in `.magento.app.yaml` nell’ambiente Docker. In precedenza, l’ambiente Docker supportava solo la configurazione hook predefinita.<!-- MAGECLOUD-3505-->

   - ![nuova icona](../../assets/new.svg) I file ENV Docker non vengono più generati durante la build Docker e il `docker:config:convert` è obsoleto. I dati corrispondenti vengono ora memorizzati nella `docker-compose.yml` file.<!-- MAGECLOUD-3816-->

   - ![nuova icona](../../assets/new.svg) **Immagine PHP aggiornata**-Aggiunto Node.js all&#39;immagine Docker PHP per supportare le funzionalità node, npm e grunt-cli.<!-- MAGECLOUD-3953 -->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti delle variabili di ambiente**-

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la **LOCK_PROVIDER** distribuire la variabile per configurare il provider di blocchi che impedisce l&#39;avvio di processi cron e gruppi cron duplicati. Vedi la descrizione della variabile in [distribuire variabili](../environment/variables-deploy.md#lock_provider) argomento.<!-- MAGECLOUD-4052 -->

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la **CONSUMER_WAIT_FOR_MAX_MESSAGES** variabile di ambiente per configurare il modo in cui i consumatori elaborano i messaggi dalla coda dei messaggi quando utilizzano `CRON_CONSUMERS_RUNNER` variabile di ambiente per gestire i processi cron. Vedi la descrizione della variabile in [distribuire variabili](../environment/variables-deploy.md#consumers_wait_for_max_messages) argomento.<!-- MAGECLOUD-4071 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema che poteva causare errori di deadlock del database quando `consumers_runner` il processo cron avvia più istanze dello stesso consumer su nodi diversi. Ora, se hai abilitato la funzione [**CRON_CONSUMER_RUNNER**](../environment/variables-deploy.md#cron_consumers_runner) implementare la variabile nell’ambiente, il `consumers_runner` il processo utilizza `single-thread` per avviare un’istanza di ciascun consumatore su un solo nodo.<!-- MAGECLOUD-3913 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema che interessava [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) funzionalità che utilizza un URL predefinito per lo store. Ora, se `config:show:default-url` Se il comando non è in grado di recuperare un URL di base, viene utilizzato l’URL dalla variabile MAGENTO_CLOUD_ROUTES.<!-- MAGECLOUD-3866 -->

- ![nuova icona](../../assets/new.svg) Sono state aggiornate le informazioni di registrazione restituite da `module:refresh` comando. Ora puoi visualizzare un elenco dettagliato dei moduli abilitati nella sezione `cloud.log` file.<!-- MAGECLOUD-2514 -->

- ![nuova icona](../../assets/new.svg) Sono state migliorate le notifiche di convalida della compatibilità delle versioni e di avviso per i problemi di compatibilità tra la versione di Adobe Commerce e i servizi installati, come ad Elasticsearch [!DNL RabbitMQ], Redis e DB.<!-- MAGECLOUD-3535 -->

- ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto per RabitMQ versione 3.8.<!-- MAGECLOUD-4674-->

- ![nuova icona](../../assets/new.svg) Sono state aggiornate le convalide interattive per la compatibilità dei servizi, in modo da riflettere le versioni supportate per le nuove versioni di Adobe Commerce 2.3.3 e 2.2.10. Consulta [Requisiti di sistema](https://devdocs.magento.com/guides/v2.4/install-gde/system-requirements.html) nel _Guida all’installazione_ versioni consigliate.<!-- MAGECLOUD-4018 -->

- ![icona correzione](../../assets/fix.svg) È stato migliorato il messaggio di registro restituito quando il processo di gestione dei processi cron nella fase di distribuzione tenta di arrestare un processo cron già terminato per chiarire che questo problema non è un errore. È stato modificato il livello di registro da `INFO` a `DEBUG`.<!-- MAGECLOUD-3653-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che si verificava durante l’esecuzione di `setup:upgrade` comando che non ha interrotto il processo di distribuzione quando si è verificato un errore durante `app:config:import` attività.<!-- MAGECLOUD-3806 -->

- ![nuova icona](../../assets/new.svg) Il livello di registro predefinito per il gestore file è stato modificato in `debug` per ridurre la quantità di dettagli nel registro visualizzato nel [!DNL Cloud Console], pur fornendo informazioni dettagliate per il debug.<!-- MAGECLOUD-3871 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore nella distribuzione di contenuto statico durante la generazione. Dopo un&#39;installazione e `ece-tools` config dump, si è verificato un errore se non è stata specificata alcuna lingua per l’utente amministratore in `config.php` file. Ora esiste una lingua predefinita per l’utente amministratore in `config.php` file.<!-- MAGECLOUD-3957 -->

- ![icona correzione](../../assets/fix.svg) È stato corretto un `Undefined index error` che si verifica quando un `magento-cloud` Il comando CLI ha esito negativo in un ambiente non configurato con un URL protetto (https). Ora, il pacchetto ECE-Tools utilizza l’URL di base (http) se l’URL protetto non è disponibile.<!-- MAGECLOUD-4009 -->

## v2002.0.20

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - ![nuova icona](../../assets/new.svg) È ora possibile eseguire test funzionali utilizzando `ece-tools` nell’ambiente Docker. Consulta [test delle applicazioni](https://devdocs.magento.com/cloud/docker/docker-test-magecloud-pkg-code.html).<!-- MAGECLOUD-3129/3684 -->

   - ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto per la configurazione dei moduli PHP tramite `.magento.app.yaml` file. Qualsiasi [Estensioni PHP specificate in `.magento.app.yaml` file](https://devdocs.magento.com/cloud/project/magento-app-php-application.html#php-extensions) diventano disponibili nei contenitori Docker PHP.<!-- MAGECLOUD-3357 -->

   - ![nuova icona](../../assets/new.svg) Sono disponibili nuovi comandi per migliorare l’esperienza della riga di comando Docker. Consulta la [`bin/magento-docker` sezione del riferimento Docker](https://devdocs.magento.com/cloud/docker/docker-quick-reference.html#magento-cloud-docker-cli).<!-- MAGECLOUD-3569 -->

   - ![nuova icona](../../assets/new.svg) Aggiunta la possibilità di utilizzare Mutagen.io per sincronizzare i file durante lo sviluppo tra l&#39;host locale e Docker.<!-- MAGECLOUD-3559 -->

   - ![icona correzione](../../assets/fix.svg) È stato corretto il percorso predefinito quando si utilizza l’ambiente Docker. Ora, quando utilizzi SSH per accedere al contenitore Docker, ti trovi nella directory principale del progetto in `/app` come previsto.<!-- MAGECLOUD-3582 -->

   - ![icona correzione](../../assets/fix.svg) La libreria Sodio è stata aggiornata dalla versione 1.0.11 alla versione 1.0.18 ed è stata aggiornata l’estensione Sodio PHP.<!-- MAGECLOUD-3832 -->

     >[!WARNING]
     >
     >I clienti di Adobe Commerce su infrastruttura cloud devono [Inviare un ticket di supporto Adobe Commerce](https://support.magento.com/hc/en-us/articles/360000913794#submit-ticket) per aggiornare il pacchetto libsodium negli ambienti di produzione e staging Pro prima dell’aggiornamento ad Adobe Commerce 2.3.2. Attualmente, non è possibile aggiornare gli ambienti Starter ad Adobe Commerce 2.3.2.

   - ![icona correzione](../../assets/fix.svg) È stata aggiunta la `analysis-icu` e `analysis-phonetic` Elasticsearch di plug-in per tutte le immagini Docker.<!-- MAGECLOUD-3446 -->

   - ![icona correzione](../../assets/fix.svg) Convalide migliorate: quando si utilizzano le opzioni per la `docker:build` , è necessario fornire un valore quando si utilizza un&#39;opzione. Inoltre, è stata aggiunta la convalida per la versione del nodo quando si utilizza `docker:build run` comando.<!-- MAGECLOUD-3486 & MAGECLOUD-3678 -->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti delle variabili di ambiente**—

   - ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto per i prefissi delle tabelle del database utilizzando [Variabile di ambiente DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!-- MAGECLOUD-2901 -->

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la **FORCE_UPDATE_URLS** distribuire la variabile per aggiornare gli URL di base durante la distribuzione negli ambienti di produzione e staging di Pro e Starter. Vedi la definizione nella sezione [distribuire variabili](../environment/variables-deploy.md#force_update_urls) contenuto.<!-- MAGECLOUD-3602 -->

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la **TTFB_TESTED_PAGES** variabile post-distribuzione da configurare _Tempo al primo byte_ test delle pagine per verificare le prestazioni delle applicazioni sui siti distribuiti nell’infrastruttura cloud. Vedi la descrizione della variabile in [variabili post-distribuzione](../environment/variables-post-deploy.md).<!-- MAGECLOUD-3643 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema relativo alla SCD multithread, che causava errori casuali nella distribuzione di contenuti statici. La soluzione alternativa consisteva nell’impostare **SCD_THREADS** variabile a `1`. Ora puoi aumentare il conteggio in base alle esigenze. Vedi le definizioni in [distribuire variabili](../environment/variables-deploy.md#scd_threads) e [variabili di build](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3611 -->

   - ![icona correzione](../../assets/fix.svg) È possibile configurare **WARM_UP_PAGES** variabile di ambiente per memorizzare nella cache singole pagine, più domini e più pagine. Vedi la definizione espansa in [variabili post-distribuzione](../environment/variables-post-deploy.md#warm_up_pages) contenuto.<!-- MAGECLOUD-3258 -->

- ![icona correzione](../../assets/fix.svg) È stata aggiunta la `pub/static/.htaccess` nell&#39;elenco di esclusione. [Correzione presentata da Björn Kraus di PHOENIX MEDIA GmbH](https://github.com/magento/ece-tools/pull/455).<!-- MAGECLOUD-3545/Github#455 -->

- ![icona correzione](../../assets/fix.svg) È stato corretto un errore a causa del quale tutti i messaggi di convalida venivano visualizzati come `Critical` se almeno una convalida di livello critico ha restituito un errore.<!-- MAGECLOUD-3178 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore di distribuzione se l’URL di base non esisteva nel database.<!-- MAGECLOUD-3075 -->

- ![nuova icona](../../assets/new.svg) È stato aggiunto un nuovo **`env:config:show`comando** al `ece-tools` pacchetto che visualizza servizi dell&#39;ambiente, route o variabili. Consulta [Servizi, route e variabili](https://devdocs.magento.com/cloud/reference/ece-tools-reference.html#services-routes-and-variables). [Caratteristica inviata da Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/486).<!-- MAGECLOUD-3451 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore critico durante il tentativo di installare Adobe Commerce 2.2.6 o versioni precedenti con `ece-tools` sviluppare dopo il refactoring della shell.<!-- MAGECLOUD-3665 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava il mancato funzionamento delle installazioni di Adobe Commerce 2.1.x e 2.2.x con un avviso relativo all’utilizzo di una versione obsoleta di Carbon.<!-- MAGECLOUD-3704 -->

- ![icona correzione](../../assets/fix.svg) Ha ridotto il `cloud.log` livello di registro per l&#39;output shell da `info` a `debug`.<!-- MAGECLOUD-3277 -->

- ![icona correzione](../../assets/fix.svg) È stata aggiunta la `--remove-definers (-d)` opzione per `ece-tools db-dump` per rimuovere i definitori dal file di dump.<!-- MAGECLOUD-3510 -->

## v2002.0.19

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che sovrascrive il `env.php` durante una distribuzione, con conseguente perdita di configurazioni personalizzate. Questo aggiornamento garantisce che Adobe Commerce sull’infrastruttura cloud aggiorni il `env.php` con ogni distribuzione, mantenendo al contempo le configurazioni personalizzate.<!-- MAGECLOUD-3668 -->

## v2002.0.18

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - ![nuova icona](../../assets/new.svg) Ora l’ambiente Docker supporta la configurazione cron definita nella sezione [proprietà crons del file .magento.app.yaml](https://devdocs.magento.com/cloud/project/magento-app-properties.html#crons).<!-- MAGECLOUD-3150 -->

   - ![nuova icona](../../assets/new.svg) **Nuovo contenitore Docker**- Aggiunto un [Contenitore proxy di terminazione TLS](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container) per facilitare la terminazione SSL di Vernice su HTTPS.<!-- MAGECLOUD-2890 -->

   - ![nuova icona](../../assets/new.svg) **Nuova immagine Docker**- Aggiunta di un&#39;immagine Node.js per il supporto di Gulp e altre funzionalità, ad esempio Test di unità JS Jasmine.<!-- MAGECLOUD-3345 -->

   - ![nuova icona](../../assets/new.svg) **Modalità build Docker**- Ora è possibile scegliere di avviare l&#39;ambiente Docker in [Modalità di produzione o modalità Sviluppatore](https://devdocs.magento.com/cloud/docker/docker-launch.html#set-the-launch-mode). La modalità Sviluppatore supporta lo sviluppo attivo con autorizzazioni di file system complete e scrivibili.<!-- MAGECLOUD-3152/3511 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore di distribuzione Docker con un `Name or service not known` errore se la cache è configurata per un servizio non disponibile. Ora è possibile rimuovere un servizio dal [`.magento/services.yaml` file](https://devdocs.magento.com/cloud/project/services.html). Il generatore di configurazione Docker aggiorna il servizio in `docker/config.php.dist` file automaticamente.<!-- MAGECLOUD-3369 -->

   - ![nuova icona](../../assets/new.svg) Sono state aggiunte convalide interattive per la compatibilità dei servizi. Ora, se un servizio richiesto è incompatibile con la versione di Adobe Commerce o con altri servizi, il _modalità interattiva_ richiede all&#39;utente un messaggio e la scelta di continuare. Consulta la [Versioni del servizio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-containers) disponibile per Docker. Utilizza il `-n` per saltare l’interattività ai fini di CICD.<!-- MAGECLOUD-3251 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema relativo alla composizione Docker `db-dump` comando che ha cancellato le immagini esistenti.<!-- MAGECLOUD-3366 -->

   - ![icona correzione](../../assets/fix.svg) È stato risolto un problema che assegnava Redis `session`, `default`, e `page_cache` memorizza la cache nello stesso ID database.<!-- MAGECLOUD-3172 -->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti delle variabili di ambiente**—

   - ![nuova icona](../../assets/new.svg) Il nuovo **ELASTICSUITE\_CONFIGURATION** la variabile di ambiente mantiene le impostazioni di servizio personalizzate tra le distribuzioni. Vedi la definizione nella sezione [distribuire variabili](../environment/variables-deploy.md#elasticsuite_configuration) contenuto.<!-- MAGECLOUD-3205 -->

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la **SCD_MAX_EXECUTION_TIMEOUT** variabile di ambiente in modo da poter aumentare il tempo necessario per completare la distribuzione del contenuto statico da `.magento.env.yaml` file. Vedi la definizione nella sezione [distribuire variabili](../environment/variables-deploy.md#scd_max_execution_time), il [variabili di build](../environment/variables-build.md#scd_max_execution_time)e [variabili globali](../environment/variables-global.md#scd_max_execution_time).<!-- MAGECLOUD-2822 -->

      - ![nuova icona](../../assets/new.svg) È stata aggiunta la **MAGENTO_CLOUD_LOCKS_DIR** variabile di ambiente per configurare il percorso del punto di montaggio per il provider di blocchi nell’infrastruttura cloud. Il provider di blocchi impedisce l&#39;avvio di processi cron e gruppi cron duplicati. Questa variabile è supportata in Adobe Commerce versione 2.2.5 e successive e viene configurata automaticamente. Vedi la definizione in [Variabili cloud](../environment/variables-cloud.md).<!-- MAGECLOUD-3135 -->

      - ![icona correzione](../../assets/fix.svg) È stato modificato il **SCD_THREADS** valori predefiniti delle variabili di ambiente per determinare automaticamente il valore ottimale in base al numero di thread della CPU rilevati. Vedi le definizioni aggiornate in [distribuire variabili](../environment/variables-deploy.md#scd_threads) e [variabili di build](../environment/variables-build.md#scd_threads).<!-- MAGECLOUD-3382 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema con una patch per DB Isolation Mechanism che causava un errore durante l’aggiornamento ad Adobe Commerce sulla versione 2002.0.16 dell’infrastruttura cloud.<!-- MAGECLOUD-3383 -->

- ![icona correzione](../../assets/fix.svg) È stata aggiunta una patch che sostituisce _Grafici immagine Google_ con _Image-Charts_. Consulta l’articolo su DevBlog [Google Image Charts obsoleto e aggiornamento per M1](https://community.magento.com/t5/Magento-DevBlog/Google-Image-Charts-deprecation-and-update-for-M1/ba-p/125006).<!-- MAGECLOUD-3456 -->

- ![icona correzione](../../assets/fix.svg) Aggiunta della convalida per [Variabile SEARCH_CONFIGURATION](../environment/variables-deploy.md#search_configuration). La distribuzione non riesce se l’opzione &quot;engine&quot; non è impostata e `_merge` non è obbligatorio.<!-- MAGECLOUD-3470 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava l’esposizione di dati sensibili dopo il verificarsi di un’eccezione. Ora le informazioni sensibili vengono mascherate in modo appropriato.<!-- MAGECLOUD-3525 -->

- ![icona correzione](../../assets/fix.svg) Sono state migliorate le impostazioni di tolleranza d&#39;errore del pacchetto di Magento Open Source. Nel caso in cui Adobe Commerce non sia in grado di leggere i dati dal Redis: `slave` ad esempio, viene eseguita una lettura dal Redis `master` dell&#39;istanza. Consulta [REDIS_USE_SLAVE_CONNECTION](../environment/variables-deploy.md#redis_use_slave_connection).<!-- MAGECLOUD-2899 -->

## v2002.0.17

>[!NOTE]
>
>Il `ece-tools` La versione 2002.0.17 include un’importante patch di sicurezza. Consulta [Risorse tecniche: patch di Magento Open Source](https://magento.com/tech-resources/download#download2288).

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei servizi**- Supportato dalle seguenti versioni di Adobe Commerce: 2.2.8 e successive 2.2.x, 2.3.1 e successive 2.3.x

   - È stato aggiunto il supporto per Elasticsearch versione 6.x.<!-- MAGECLOUD-3196 -->

   - Aggiunto supporto per Redis versione 5.0.

- ![nuova icona](../../assets/new.svg) **Nuove immagini Docker**: sono stati aggiunti i seguenti servizi alla build Docker:

   - Elasticsearch 6.5<!-- MAGECLOUD-3196 -->

   - Redis 5.0<!-- MAGECLOUD-3223 -->

- ![nuova icona](../../assets/new.svg) **Nuova variabile di ambiente**- In precedenza, si verificava un timeout hardcoded per la compressione SCD. Ora puoi configurare il timeout di compressione SCD utilizzando **SCD_COMPRESSION_TIMEOUT** variabile di ambiente. Vedi le definizioni in [variabili di build](../environment/variables-build.md#scd_compression_timeout) e [distribuire variabili](../environment/variables-deploy.md#scd_compression_timeout) contenuto.<!-- MAGECLOUD-2870 -->

- ![icona correzione](../../assets/fix.svg) È stata aggiunta la `--use-rewrites` opzione del comando install che consente di utilizzare le riscritture del server web per i collegamenti generati nella vetrina e l’accesso amministratore per migliorare la sicurezza e l’esperienza del cliente.<!-- MAGECLOUD-3246 -->

- ![icona correzione](../../assets/fix.svg) Sono state aggiunte marche temporali al `var/log/install_upgrade.log` in modo da visualizzare le date degli eventi di installazione e aggiornamento.<!-- MAGECLOUD-2895 -->

## v2002.0.16

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - Ora la configurazione del servizio predefinita generata nell’ambiente Docker è uguale alla configurazione predefinita nel modello Cloud.<!-- MAGECLOUD-3025 -->

   - È possibile inviare messaggi dall’ambiente Docker utilizzando `sendmail` servizio.<!-- MAGECLOUD-2907 -->

   - Aggiunta la possibilità di [configurare Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html) per eseguire il debug nell’ambiente Cloud Docker.<!-- MAGECLOUD-2891 -->

   - È stato risolto un problema relativo alle autorizzazioni del servizio web durante la generazione di `docker-compose.yml` file.<!-- MAGECLOUD-2883 -->

- ![nuova icona](../../assets/new.svg) **Miglioramento dell’aggiornamento**- Aggiunta della convalida per confermare che `autoload` proprietà in `composer.json` Il file contiene le modifiche di configurazione necessarie prima dell’aggiornamento ad Adobe Commerce v2.3. Consulta [Versione aggiornamento](https://devdocs.magento.com/cloud/project/project-upgrade.html).<!-- MAGECLOUD-2392 -->

- ![nuova icona](../../assets/new.svg) Il processo di compressione nella distribuzione del contenuto statico ora include tutte le risorse, generate in modo nativo o personalizzate, e si verifica durante la fase di build all’inizio della [`build:transfer` sezione](https://devdocs.magento.com/cloud/project/magento-app-properties.html#hooks). In precedenza, il processo di compressione si verificava prima di applicare la minimizzazione personalizzata e il bundling di risorse statiche. [Correzione inviata da Rafael Garcia Lepper da Tryzens Limited](https://github.com/magento/ece-tools/pull/413).<!-- MAGECLOUD-3104 -->

- ![icona correzione](../../assets/fix.svg) È stato corretto un errore di connessione al database che si verificava durante la distribuzione subito dopo la configurazione di una relazione di database e servizio aggiuntiva. Inoltre, questa correzione risolve un problema che si verificava durante il processo di configurazione di Commerce Reporting for Starter. Per Starter, questo aggiornamento è un &quot;must have&quot; per l’utilizzo di Commerce Reporting.<!-- MAGECLOUD-3035 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema di convalida della configurazione del database che causava un errore nel processo di distribuzione.<!-- MAGECLOUD-3003 -->

- ![icona correzione](../../assets/fix.svg) Il vincolo è stato aggiornato con la versione appropriata del `symfony/yaml` pacchetto da utilizzare con [Costanti PHP](https://devdocs.magento.com/cloud/project/magento-env-yaml.html#php-constants). L’analisi costante non funziona quando si utilizza una `symfony/yaml` versione del pacchetto precedente alla 3.2. [Correzione inviata da Vladimir Kerkhoff](https://github.com/magento/ece-tools/pull/404).<!-- MAGECLOUD-2956 -->

- ![nuova icona](../../assets/new.svg) **Verifica della configurazione dell’ambiente**- Aggiunta della convalida per controllare la versione PHP e avvisare gli utenti se non utilizzano la versione più recente consigliata.<!--MAGECLOUD-2903-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema relativo all’elaborazione di variabili JSON non valide. Ora, se una variabile JSON causa un errore di sintassi, viene visualizzato un avviso nel `cloud.log` Il file e la distribuzione continuano a utilizzare la variabile predefinita.<!-- MAGECLOUD-2851 -->

- ![icona correzione](../../assets/fix.svg) È stato corretto un errore di connessione che si verificava durante la distribuzione subito dopo la disattivazione del servizio Redis.<!-- MAGECLOUD-2747 -->

- ![nuova icona](../../assets/new.svg) **Registrazione delle modifiche**- Aggiornato il [livello di registro](../environment/log-handlers.md#log-levels) da `Info` a `Notice` per i seguenti eventi di processo di compilazione e distribuzione:<!--MAGECLOUD-2925-->

   - Inizio e fine del processo di riconciliazione dei moduli installati in `composer.json` con le impostazioni di configurazione condivise in `app/etc/config.php` file

   - Inizio e fine del processo di convalida della configurazione

   - Inizio e fine del `setup:di:compile` processo per la generazione delle classi

- ![nuova icona](../../assets/new.svg) **Nuove variabili di ambiente**—

   - **[Variabile di distribuzione RESOURCE_CONFIGURATION](../environment/variables-deploy.md#resource_configuration)**- Utilizzare questa variabile per associare un nome di risorsa a una connessione di database.<!-- MAGECLOUD-3026 & MAGECLOUD-2963-->

   - **[Variabile globale X_FRAME_CONFIGURATION](../environment/variables-global.md#x_frame_configuration)**- Utilizzate questa variabile per modificare la `X-Frame-Options` configurazione dell’intestazione per il rendering di una pagina Adobe Commerce in una `<frame>`, `<iframe>`, o `<object>`.<!-- MAGECLOUD-3048 -->

- ![icona correzione](../../assets/fix.svg) **Aggiornamenti delle variabili di ambiente**- Sono state modificate le seguenti variabili di ambiente:

   - **[WARM_UP_PAGES](../environment/variables-post-deploy.md)**- Aggiunta della possibilità di precaricare la cache per pagine specifiche su tutti i domini definiti per un archivio Adobe Commerce. In precedenza, se il sito era configurato con più domini, il processo post-distribuzione non riusciva a precaricare la cache per le pagine specificate su domini non predefiniti e restituiva il seguente errore nel registro post-distribuzione: `ERROR: Warming up failed: <uri>`<!-- MAGECLOUD-2466 -->

   - **SCD_COMPRESSION_LEVEL**- Aggiornamento della documentazione e dell&#39;esempio `.magento.env.yaml` con i valori predefiniti corretti per il livello di compressione SCD. Vedi le definizioni in [variabili di build](../environment/variables-build.md#scd_compression_level) e [distribuire variabili](../environment/variables-deploy.md#scd_compression_level) contenuto.<!-- MAGECLOUD-2823 -->

   - **SCD_EXCLUDE_THEMES**- Questa variabile di ambiente è obsoleta. Utilizza il [SCD_MATRIX](../environment/variables-build.md#scd_matrix) per controllare la configurazione del tema.<!--MAGECLOUD-2882-->

   - **SCD\_MATRIX**- È stato corretto il processo di convalida per evitare un problema che si verificava quando SCD_MATRIX ignorava un valore di tema che conteneva casi di caratteri diversi. Vedi le definizioni in [variabili di build](../environment/variables-build.md#scd_matrix) e [distribuire variabili](../environment/variables-deploy.md#scd_matrix) contenuto.<!-- MAGECLOUD-2904 -->

   - **Variabili ADMIN**—<!-- MAGECLOUD-2573/MAGECLOUD-2848 -->

      - È stata migliorata la sicurezza durante la gestione delle credenziali per l’utente Admin utilizzando le variabili di ambiente. Non è più possibile utilizzare le variabili di ambiente ADMIN_EMAIL, ADMIN_USERNAME e ADMIN_PASSWORD per sostituire le credenziali admin durante gli aggiornamenti. Se non riesci ad accedere al pannello di amministrazione, utilizza _Password dimenticata_ funzionalità o `admin:user:create` Comando CLI per creare un nuovo utente amministratore. Consulta [Accedere al pannello di amministrazione](https://devdocs.magento.com/cloud/onboarding/onboarding-tasks.html#admin).

      - ADMIN_EMAIL non è più richiesto per l&#39;aggiornamento o l&#39;applicazione delle patch.

## v2002.0.15

- ![nuova icona](../../assets/new.svg) **Aggiornamenti Docker**—

   - Ora il generatore Docker utilizza i servizi specificati nella `.magento.app.yaml` e `.magento/services.yaml` file di configurazione quando [creazione dell’ambiente Docker](https://devdocs.magento.com/cloud/docker/docker-config.html). È possibile scegliere una versione del servizio diversa utilizzando i parametri di compilazione.<!-- MAGECLOUD-2888 -->

   - È stata aggiunta l’immagine PHP 7.2: in Cloud Docker è stato aggiunto il supporto per PHP 7.2; aggiornato il [Configurazione di Launch Docker](https://devdocs.magento.com/cloud/docker/docker-config.html) per includere `docker:build --php` per specificare la versione di PHP compatibile con la versione di Adobe Commerce in uso.<!-- MAGECLOUD-2799 -->

   - È stato aggiunto un [Contenitore Cron](https://devdocs.magento.com/cloud/docker/docker-containers-cli.html#cron-container) basato sull&#39;immagine PHP-CLI.<!-- MAGECLOUD-2565 -->

   - Sono stati aggiunti i seguenti servizi alla build Docker:

      - [!DNL RabbitMQ] 3.5 e 3.7<!-- MAGECLOUD-2567 & 2889-->

      - Elasticsearch 1.7, 2.4 e 5.2<!-- MAGECLOUD-2569 & 2887 -->

      - Redis 3.2 e 4.0<!-- MAGECLOUD-2886 -->

- ![nuova icona](../../assets/new.svg) **Configurare con costanti PHP**- È stato aggiunto il supporto per [Costanti PHP](https://devdocs.magento.com/cloud/project/magento-env-yaml.html#php-constants) nel `.magento.env.yaml` file di configurazione.<!-- MAGECLOUD- 2575 -->

- ![nuova icona](../../assets/new.svg) **Nuova variabile di ambiente**- Per impostazione predefinita, le Google Analytics sono abilitate solo nell&#39;ambiente di produzione. È possibile abilitare le Google Analytics negli ambienti di staging e integrazione utilizzando  [Variabile di ambiente ENABLE_GOOGLE_ANALYTICS](../environment/variables-deploy.md#enable_google_analytics).<!--MAGECLOUD-2879-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che rimuoveva le configurazioni cron personalizzate da `env.php` dopo una ridistribuzione. Ora, le configurazioni cron personalizzate rimangono in modo sicuro nel `env.php` file.<!-- MAGECLOUD-2923 -->

- ![icona correzione](../../assets/fix.svg) Sono state corrette le incoerenze nei messaggi e nei [livelli di registro](../environment/log-handlers.md#log-levels) per le fasi di build, distribuzione e post-distribuzione. Aumento dei livelli dei messaggi di registro iniziale e finale da **info** a **avviso** per tutte le fasi e le sottofasi. Sono stati aggiunti i messaggi di registro iniziale e finale, se necessario.<!-- MAGECLOUD-2919 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema relativo ai processi cron che impediva l’avvio della fase post-distribuzione, se configurata. Ora, se hai abilitato l’hook post-distribuzione, i processi cron vengono nuovamente abilitati all’inizio della fase post-distribuzione.<!-- MAGECLOUD-2862 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che impediva la corretta installazione di Adobe Commerce durante la specifica di una configurazione di database personalizzata. In precedenza, il processo di installazione utilizzava la configurazione del database dalla [Variabile MAGENTO_CLOUD_RELATIONSHIP](../environment/variables-cloud.md) anche se sono state specificate informazioni di connessione personalizzate nel [Variabile di ambiente DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration).<!--MAGECLOUD-2736-->

- ![icona correzione](../../assets/fix.svg) È stato corretto il `config:dump` in modo da includere tutte le impostazioni locali del sito Web nel `system` sezione del `config.php` file.<!--MAGECLOUD-2740-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava _riscaldamento_ errori durante la fase di post-distribuzione correggendo il riferimento all’URL di base dell’origine.<!--MAGECLOUD-2797-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava la generazione errata di file durante il `setup:di:compile` processo, che ha interessato il modulo Amazon Pay.<!--MAGECLOUD-2850-->

## v2002.0.14

- ![nuova icona](../../assets/new.svg) **Verifica stato ideale**- La `ideal-state` la procedura guidata ora verifica la configurazione corrente durante ogni distribuzione e fornisce istruzioni chiare per aggiornare la configurazione in modo da ottenere una distribuzione più rapida e senza tempi di inattività.<!--MAGECLOUD-2372-->

- ![icona correzione](../../assets/fix.svg) **Conformità PCI**: i protocolli di messaggistica per Adobe Commerce sull’infrastruttura cloud sono stati aggiornati per richiedere la versione 1.2 di Transport Layer Security (TLS) durante la connessione con servizi di messaggistica di terze parti. Se utilizzi un servizio messaggi che non supporta la versione 1.2 di TLS, devi aggiornare il servizio. In caso contrario, quando l’applicazione Adobe Commerce tenta di connettersi al server dei messaggi per inviare un’e-mail viene visualizzato il seguente messaggio di errore: `Unable to connect via TLS`.<!--MAGECLOUD-2521-->

- ![nuova icona](../../assets/new.svg) **Miglioramento dell’implementazione**: aggiunta della convalida per avvisare i clienti se un ambiente di staging o produzione ha `dev`, `debug`, o `debug_logging` opzioni abilitate per evitare problemi di prestazioni causati da un’attività di registrazione eccessiva.<!--MAGECLOUD-2517-->

- ![icona correzione](../../assets/fix.svg) **Correzioni di distribuzione**—

   - Ora la modalità di manutenzione è abilitata all’inizio della fase di distribuzione e disabilitata alla fine. Se la distribuzione non riesce, il sito rimane in modalità di manutenzione fino alla risoluzione dei problemi di distribuzione. In precedenza, il sito tornava alla modalità di produzione anche se la distribuzione non riusciva.<!--MAGECLOUD-2603-->

      - I controlli di convalida della fase di distribuzione sono stati rielaborati per ridurre il livello di errore per i seguenti problemi di distribuzione da `CRITICAL` a `WARNING` in modo che la distribuzione venga completata. In precedenza, questi problemi causavano un errore di distribuzione.

      - La configurazione dell’ambiente contiene valori errati per le variabili di distribuzione o cloud.

   - La versione di Elasticsearch sull’infrastruttura cloud non è compatibile con la versione del modulo elasticsearch/elasticsearch supportata da Adobe Commerce sull’infrastruttura cloud. Consulta la [Elasticsearch di risoluzione dei problemi](https://support.magento.com/hc/en-us/articles/360015758471-Deployment-fails-or-interrupts-with-cloud-log-error-Elasticsearch-version-is-not-compatible-with-current-version-of-magento) nella Knowledge Base di supporto di Adobe Commerce.<!--MAGECLOUD-2600-->

   - È stato risolto un problema relativo alle impostazioni di configurazione condivise in `app/etc/config.php` file che ha causato `recursion detected` errori durante la distribuzione.<!--MAGECLOUD-2173-->

- ![icona correzione](../../assets/fix.svg) **Correzioni relative alle corone**—

   - È stato risolto un problema di pianificazione cron che impediva l’esecuzione dei processi se si specificava una frequenza cron diversa da quella predefinita (1 minuto).<!--MAGECLOUD-2602-->

   - È stato risolto un problema nella fase di distribuzione che consentiva ai processi cron di continuare a essere in esecuzione durante la distribuzione, causando blocchi del database e altri problemi critici. Ora, tutti i processi cron vengono interrotti prima dell’inizio della fase di distribuzione e riavviati al termine della stessa.&lt;!—MAGECLOUD—2537—>

   - È stato corretto il flusso di lavoro dei processi cron nelle versioni 2.2.x per sbloccare i processi cron bloccati in modo che potessero essere interrotti prima di iniziare la distribuzione. In precedenza, un processo cron bloccato causava l’arresto della distribuzione.<!--MAGECLOUD-2501-->

- ![icona correzione](../../assets/fix.svg) È stato modificato il formato del file `config.php` file generato da `vendor/bin/ece-tools config:dump` comando per utilizzare la sintassi di array breve e il rientro a 4 spazi per rispettare gli standard di codifica Adobe Commerce.<!--MAGECLOUD-2527-->

- ![icona correzione](../../assets/fix.svg) È stato corretto un errore di distribuzione che si verificava quando `.magento.env.yaml` contiene `{{ base_url }}` e `{{ unsecure_base_url }}` segnaposto per configurazioni web invece della configurazione URL predefinita per un progetto di infrastruttura cloud di Adobe Commerce./<!--MAGECLOUD-2607-->

## v2002.0.13

- ![nuova icona](../../assets/new.svg) **Abilita distribuzione senza tempi di inattività**—Ora Adobe Commerce su infrastruttura cloud accoda le richieste con le modifiche richieste al database durante la distribuzione e applica le modifiche non appena la distribuzione viene completata. Le richieste possono essere trattenute per un massimo di 5 minuti per evitare la perdita di sessioni. Consulta [Opzioni di distribuzione dei contenuti statici per ridurre i tempi di inattività dell’implementazione su Cloud](https://support.magento.com/hc/en-us/articles/360004861194-Static-content-deployment-options-to-reduce-deployment-downtime-on-Cloud).<!--MAGECLOUD-2169-->

- ![nuova icona](../../assets/new.svg) **Docker - Componi per cloud**- Sono stati apportati i seguenti miglioramenti alla [Configurazione e configurazione Docker](https://devdocs.magento.com/cloud/docker/docker-config.html) processo:

   - È stato aggiunto un comando:`docker:config:convert` per convertire i file di configurazione PHP in formato Docker ENV per semplificare la configurazione dell&#39;ambiente. Ora è possibile copiare i file di configurazione PHP nella directory Docker e convertirli in file ENV Docker. Consulta [Avvia Docker](https://devdocs.magento.com/cloud/docker/docker-config.html).<!--MAGECLOUD-2359-->

   - Il processo di installazione dell’infrastruttura cloud di Adobe Commerce ora supporta la distribuzione sia su file system di sola lettura che di lettura-scrittura per emulare più strettamente il file system cloud. Consulta [Configura Docker](https://devdocs.magento.com/cloud/docker/docker-config.html).&lt;!—MAGECLOUD—2357—>

   - **Supporto del servizio Redis**: aggiunta di un&#39;immagine Redis, distribuita in un contenitore Docker e configurata automaticamente per l&#39;utilizzo con l&#39;installazione Docker.&lt;!—MAGECLOUD—2442—>

   - Ora disponi della funzionalità di dump del database quando utilizzi Cloud Docker [contenitore database](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#database-container). Inoltre, è possibile [condividere file](https://devdocs.magento.com/cloud/docker/docker-containers.html#sharing-data-between-host-machine-and-container) tra un computer host e un contenitore utilizzando `docker/mnt` directory.<!-- MAGECLOUD-2577 -->

   - **Supporto per vernice**— Aggiunta di un&#39;immagine di vernice che viene distribuita automaticamente in un contenitore Docker. Dopo l’implementazione, puoi configurare manualmente Vernice seguendo le best practice di Adobe Commerce. Consulta [Configurare e utilizzare vernice](https://devdocs.magento.com/guides/v2.3/config-guide/varnish/config-varnish.html).&lt;!—MAGECLOUD—2358—>

   - Accesso sicuro al sito: è stato aggiunto il supporto SSL per accedere allo store Adobe Commerce e al pannello di amministrazione.&lt;!—MAGECLOUD—2360—>

- ![icona correzione](../../assets/fix.svg) **Miglioramento del supporto per l’estensione dell’infrastruttura cloud di Adobe Commerce**- È stato ridotto il requisito della versione minima per il pacchetto guzzlehttp/guzzle nell’infrastruttura cloud di Adobe Commerce [file compositore.json](https://devdocs.magento.com/cloud/reference/cloud-composer.html) alla versione 6.2 in modo che il `ece-tools` è compatibile con più estensioni.<!--MAGECLOUD-2205-->

- ![nuova icona](../../assets/new.svg) **Applicare modifiche personalizzate all’applicazione Adobe Commerce durante la fase di build**: la fase di build è stata suddivisa in due processi separati, in modo da poter utilizzare gli hook per applicare modifiche personalizzate al contenuto statico generato prima di creare il pacchetto dell’applicazione per la distribuzione. Il _generare:generare_ Il processo genera codice, applica patch e genera contenuto statico. Il _build:trasferimento_ process trasferisce il codice generato e il contenuto statico alla destinazione finale. Consulta [Hook dell’applicazione](https://devdocs.magento.com/cloud/project/magento-app-properties.html#hooks).<!--MAGECLOUD-2363-->

- ![icona correzione](../../assets/fix.svg) **Controlli della configurazione dell’ambiente**: è stata migliorata la convalida della configurazione dell’ambiente per avvisare i clienti in merito a incompatibilità di versione ed errori di configurazione prima di creare e distribuire Adobe Commerce sull’infrastruttura cloud.

   - È stata aggiunta la convalida specifica della versione per identificare variabili e valori di ambiente non supportati o obsoleti.<!--MAGECLOUD-2183-->

   - È stata aggiunta una verifica di compatibilità di Elasticsearch per avvisare gli utenti dei problemi di configurazione di Elasticsearch. Ora, la distribuzione non riesce se la versione del servizio Elasticsearch sul server è incompatibile con Adobe Commerce. In precedenza, la distribuzione veniva eseguita correttamente anche se la versione dell’Elasticsearch non era compatibile, causando problemi nel catalogo dei prodotti dopo la distribuzione del sito.<!--MAGECLOUD-2389-->

     Per risolvere l’incompatibilità: [invio di un ticket di supporto](https://devdocs.magento.com/cloud/trouble/trouble.html) per aggiornare Elasticsearch a una versione compatibile o modificare la configurazione di Adobe Commerce per specificare una versione compatibile del client PHP di Elasticsearch.

      - Per Adobe Commerce dalla versione 2.1.x alla versione 2.2.2, aggiornare Elasticsearch alla versione 2.4.

      - Per Adobe Commerce versione 2.2.3 e successive, aggiorna Elasticsearch alla versione 5.2.

      - Se disponi di Elasticsearch 1.x o 2.x e non desideri aggiornarlo, aggiorna il requisito della versione del client Adobe Commerce Elasticsearch PHP in compositore.json in `"elasticsearch/elasticsearch": "~2.0"`.

   - È stata migliorata la convalida delle variabili di ambiente per identificare le impostazioni di configurazione che possono causare conflitti durante le fasi di build, distribuzione e post-distribuzione. Ad esempio, se l’impostazione globale per la distribuzione di contenuto statico è in conflitto con le impostazioni della fase di build o distribuzione, durante il processo di installazione e aggiornamento viene visualizzato un messaggio di avviso.<!--MAGECLOUD-2156-->

- ![icona correzione](../../assets/fix.svg) **Aggiornamenti delle variabili di ambiente**- Sono state modificate le seguenti variabili di ambiente:

   - **[Variabile globale SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification)**- Il valore di default è stato modificato in `true` per abilitare la minimizzazione dei contenuti HTML su richiesta, che riduce al minimo i tempi di inattività durante l’implementazione negli ambienti di staging e produzione. Questa configurazione è necessaria per le distribuzioni senza tempi di inattività.<!--MAGECLOUD-2435-->

   - **[Variabile di distribuzione CLEAN_STATIC_FILES](../environment/variables-deploy.md#clean_static_files)**- È stata aggiunta la possibilità di gestire l&#39;elaborazione di file statici puliti per il contenuto statico generato durante la fase di creazione in base all&#39;impostazione della variabile di ambiente CLEAN_STATIC_FILES. In precedenza, i file di contenuto statico generati durante la fase di build venivano sempre puliti.<!--MAGECLOUD-1506-->

- ![icona correzione](../../assets/fix.svg) **Registrazione**- Sono state apportate le seguenti modifiche per migliorare i messaggi di registro e ridurre le dimensioni del registro:

   - Le voci del registro degli errori di distribuzione ora includono l’output del comando dalle operazioni che causano gli errori anche se la configurazione dell’ambiente non specifica la registrazione a livello di debug. Consulta [`MIN_LOGGING_LEVEL`](../environment/variables-global.md#min_logging_level).<!--MAGECLOUD-2489-->

   - È stata aggiunta la registrazione per gli errori di distribuzione che si verificano quando le factory generate richieste da alcune estensioni non possono essere generate correttamente perché il file system è in modalità di sola lettura.<!--MAGECLOUD-2209-->

   - Sono state ridotte le dimensioni del registro di distribuzione e sono stati risolti i problemi di formattazione causati dai comandi di configurazione che utilizzano la barra di avanzamento interattiva.<!--MAGECLOUD-2402-->

   - È stata eliminata la complessità superflua e sono stati aggiornati i livelli di priorità per alcune istruzioni di registro.<!--MAGECLOUD-2227-->

- ![icona correzione](../../assets/fix.svg) **Correzioni specifiche per la fotocamera**—

   - Le impostazioni predefinite di configurazione del processo cron per la durata della cronologia sono state modificate da 3d (4320 min) a 1h (60 min) per evitare problemi di prestazioni e di distribuzione che possono verificarsi quando la coda cron si riempie troppo rapidamente.<!--MAGECLOUD-2427-->

   - È stato migliorato il processo di gestione dei processi cron durante la fase di distribuzione per evitare blocchi del database e altri problemi critici. Ora tutti i processi cron si interrompono durante la fase di distribuzione e vengono riavviati al termine della distribuzione.<!--MAGECLOUD-2445-->

   - È stato risolto un problema con il meccanismo di blocco per la pianificazione dei consumer lanciati dai processi cron in Adobe Commerce versioni 2.2.0 e successive, al fine di impedire che i processi cron avviassero consumer duplicati.<!--MAGECLOUD-2464-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema relativo al [processo di compressione dei contenuti statici](../environment/variables-intro.md) (`gzip`) che ha causato `not overwritten` e `no such file or directory` errori durante il riferimento al file compresso durante il processo di distribuzione.<!-- MAGECLOUD-2182-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che impediva alla funzione `php ./vendor/bin/ece-tools config:dump` dalla rimozione di sezioni ridondanti dal `config.php` durante il processo di dump se non si specifica la lingua dell&#39;archivio. Ora è possibile spostare facilmente i file di configurazione tra ambienti diversi. Dopo l’aggiornamento a `ece-tools` v2002.0.13, rigenera versioni precedenti `config.php` file con il `config:dump` comando. Consulta [Gestione della configurazione per le impostazioni dello store](https://devdocs.magento.com/cloud/live/sens-data-over.html).<!--MAGECLOUD-2444-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore durante la fase di distribuzione se la configurazione della route in `.magento/routes.yaml` reindirizzamenti file da un [apice](https://blog.cloudflare.com/zone-apex-naked-domain-root-domain-cname-supp/) dominio a un `www` dominio.<!--MAGECLOUD-2556-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema relativo al `_merge` opzione per [`SEARCH_CONFIGURATION`](../environment/variables-deploy.md#search_configuration) che ha causato risultati di unione errati se non si include la variabile `engine` parametro nel file aggiornato `.magento.env.yaml` file di configurazione. Ora l’operazione di unione sovrascrive correttamente solo i valori specificati nell’aggiornamento `.magento.env.yaml` senza richiedere di impostare `engine` parametro.<!--MAGECLOUD-2520-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema di configurazione Redis che abilitava in modo errato il blocco delle sessioni per Adobe Commerce sulle versioni 2.2.1 e successive dell’infrastruttura cloud, causando un rallentamento delle prestazioni e timeout. Ora, il blocco della sessione è disattivato per impostazione predefinita. Il problema è stato causato da una modifica nel comportamento predefinito del `disable_locking` parametro introdotto nella versione 1.3.4 del pacchetto del gestore di sessione Redis. Consulta [pacchetto colinmollenhour/php-redis-session-abstract](https://github.com/colinmollenhour/php-redis-session-abstract).<!-- MAGECLOUD-2515-->

## v2002.0.12

- ![nuova icona](../../assets/new.svg) **Docker - Componi per cloud**— Aggiunto un comando —`docker:build`- per generare un [Componi Docker](https://devdocs.magento.com/cloud/docker/docker-config.html) configurazione dal cloud `ece-tools` archivio.<!-- MAGECLOUD-2250 -->

- ![nuova icona](../../assets/new.svg) **Cambia impostazioni internazionali**- Ora è possibile modificare le impostazioni locali dell&#39;archivio senza il processo di configurazione di esportazione e importazione. Quando l&#39;applicazione è in produzione e SCD_ON_DEMAND è abilitato, sono disponibili le opzioni locali dell&#39;archivio e dell&#39;amministratore.<!-- MAGECLOUD-2019 -->

- ![nuova icona](../../assets/new.svg) <!-- MAGECLOU-1998 -->**Mappa del sito e robot**- Creazione di un [workflow](../store/robots-sitemap.md) per aggiungere un `robots.txt` e generare un `sitemap.xml` per la configurazione di un singolo dominio senza richiedere una modifica all&#39;infrastruttura.

- ![nuova icona](../../assets/new.svg) **Procedure guidate**- Aggiunte due [procedure guidate](../deploy/smart-wizards.md) per aiutarti nella configurazione cloud:<!-- MAGECLOUD-1910 -->

   - `ideal-state`: configurazione dello stato ideale per ridurre al minimo i tempi di inattività dell&#39;installazione

   - `master-slave`: configurazione del bilanciamento del carico per database e Redis

- ![nuova icona](../../assets/new.svg) **Aggiornamento modulo**— Aggiunto un comando Cloud —`module:refresh`: per abilitare i moduli disabilitati o non abilitati in modo esplicito, in modo analogo a come viene eseguito automaticamente durante una build.<!-- MAGECLOUD-1521 -->

- ![nuova icona](../../assets/new.svg) È stata aggiunta la possibilità di scegliere di unire o sovrascrivere la configurazione per i servizi utilizzando `_merge` opzione in [CACHE](../environment/variables-deploy.md#cache_configuration), [SESSIONE](../environment/variables-deploy.md#session_configuration), [CODA](../environment/variables-deploy.md#queue_configuration), e [RICERCA](../environment/variables-deploy.md#search_configuration) configurazioni.<!-- MAGECLOUD-2105 -->

- ![nuova icona](../../assets/new.svg) **File di esempio per la configurazione dell’ambiente**- È stato aggiunto un `.magento.env.yaml` file di esempio per il pacchetto ECE-Tools che include una descrizione dettagliata e i possibili valori per ciascuna variabile di ambiente.<!-- MAGECLOUD-1908 -->

   - È stata inoltre aggiunta una convalida approfondita per `.magento.env.yaml` che impedisce errori nel processo di distribuzione causati da valori imprevisti. Quando si verifica un errore, viene visualizzato un messaggio di errore dettagliato che inizia con: `Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:`<!-- MAGECLOUD-1907 -->

- ![nuova icona](../../assets/new.svg) È stato aggiunto quanto segue [**Variabili di ambiente**](../environment/variables-intro.md):

   - Ora è possibile definire più lingue per ciascun tema utilizzando il nuovo [SCD_MATRIX](../environment/variables-deploy.md#scd_matrix) variabile di ambiente, che riduce la quantità di file tema da distribuire.<!-- MAGECLOUD-1501 -->

   - È stata aggiunta la [DATABASE_CONFIGURATION](../environment/variables-deploy.md#database_configuration) variabile di ambiente per personalizzare le connessioni al database per la distribuzione.<!-- MAGECLOUD-2047 -->

   - Il nuovo [MIN_LOGGING_LEVEL](../environment/variables-global.md#min_logging_level) la variabile sostituisce il livello minimo di registrazione per tutti i flussi di output senza apportare modifiche al codice.<!-- MAGECLOUD-2129 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava tempi di inattività tra la fase di distribuzione e quella successiva. Ora inizia la fase di post-distribuzione _immediatamente_ al termine della fase di distribuzione.

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che non eliminava i processi cron riusciti, quelli con `status = success`, dalla pianificazione.<!-- MAGECLOUD-2268 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema relativo al `post_deploy` hook che ha cancellato la cache nella fase di distribuzione anziché nella fase di post-distribuzione del progetto.<!-- MAGECLOUD-2113 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che si verificava con l’utilizzo di SCD con più impostazioni internazionali, che generava lo stesso `js-translation.json` per ciascuna lingua.<!-- MAGECLOUD-2034 -->

- ![icona correzione](../../assets/fix.svg) Ottimizzato il `db:dump` comando in `ece-tools` per evitare il blocco delle tabelle e aumentare la velocità.<!-- MAGECLOUD-2033 -->

## v2002.0.11

>[!NOTE]
>
>La versione ECE-Tools 2002.0.11 è necessaria per la compatibilità con la versione 2.2.4.

- ![nuova icona](../../assets/new.svg) **Configurazione delle connessioni di sola lettura ai nodi non master**- Questa versione aggiunge la possibilità di configurare una connessione di sola lettura a un nodo non principale per ricevere traffico di sola lettura (per  [MariaDB](../environment/variables-deploy.md#mysql_use_slave_connection)).<!--MAGECLOUD-143 -->[Redis](../environment/variables-deploy.md#redis_use_slave_connection) e per <!--MAGECLOUD-143 -->

- ![nuova icona](../../assets/new.svg) **Configurazione guidata**: aggiunta di una procedura guidata per verificare la configurazione per la distribuzione di contenuto statico. Consulta [Procedure guidate intelligenti](../deploy/smart-wizards.md).<!--MAGECLOUD-1910 -->

- ![nuova icona](../../assets/new.svg) **Supporto della console Symfony**- Aggiunta del supporto per Symfony Console 4 con Adobe Commerce 2.3.<!-- MAGECLOUD-1966 -->

- ![icona correzione](../../assets/fix.svg) **Ottimizzazioni della pianificazione delle celle**- È stata migliorata la gestione delle code e la registrazione avanzata per facilitare il debug dei problemi correlati alle cron.<!-- MAGECLOUD-1607 -->

- ![icona correzione](../../assets/fix.svg) La convalida della distribuzione non riesce se un `ADMIN_EMAIL` o `ADMIN_USERNAME` è lo stesso di un account amministratore esistente.<!-- MAGECLOUD-1221 -->

- ![icona correzione](../../assets/fix.svg) Supporto SOLR rimosso per le versioni 2.2.x. Le versioni 2.1.x mantengono la capacità di abilitare la funzione SOLR.<!-- MAGECLOUD-1282 -->

- ![icona correzione](../../assets/fix.svg) La prima installazione degli ambienti di staging e produzione di un progetto PRO ora include diversi prefissi di indice, ad Elasticsearch per evitare possibili conflitti durante l’identificazione dei record appartenenti a ciascun ambiente.<!-- MAGECLOUD-1489 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che interrompeva la fase di build per l’architettura legacy durante la distribuzione di contenuti statici.<!-- MAGECLOUD-2021 -->

- ![icona correzione](../../assets/fix.svg) **Miglioramenti specifici per le celle**- Rielaborazione dell&#39;implementazione cron:<!-- MAGECLOUD-1607 -->

   - È stato risolto un problema che causava il riempimento rapido della coda cron. Ora cancella i lavori di cron obsoleti in modo più affidabile.

   - È stata riorganizzata la sequenza di job cron in modo che tutti i job in thread separati vengano avviati prima del gruppo generale.

   - È stata migliorata la registrazione per facilitare il debug dei problemi relativi ai cron.

   - **NOTA**- Questa versione risolve molti problemi correlati ai cron. Se al momento utilizzi alcune patch correlate a cron in _hotfix m2_, rimuoverle.

- ![icona correzione](../../assets/fix.svg) **Miglioramenti specifici per SCD**—

   - È possibile utilizzare `VERBOSE_COMMANDS` e `SCD_COMPRESSION_LEVEL` variabili di ambiente durante entrambi _build_ e de_distribuisci le fasi.<!-- MAGECLOUD-1819 -->

   - È stato risolto un problema che causava un errore di distribuzione casuale quando si verificava un valore imprevisto per `SCD_COMPRESSION_LEVEL` variabile di ambiente. È stata migliorata la convalida della configurazione per fornire notifiche significative. Consulta [`SCD_COMPRESSION_LEVEL`](../environment/variables-build.md#scd_compression_level) per valori accettabili.<!-- MAGECLOUD-2043 -->

   - È stato corretto il comportamento di `SCD_COMPRESSION_LEVEL` flusso di configurazione delle variabili di ambiente in modo che le sostituzioni funzionino come previsto.<!-- MAGECLOUD-2044 -->

   - È stato risolto un problema che impediva la configurazione di `SCD_THREADS` variabile di ambiente in `.magento.env.yaml` file _distribuire_ fase.<!-- MAGECLOUD-2046 -->

## v2002.0.10

- ![nuova icona](../../assets/new.svg) **Distribuzione di contenuti statici (SCD)**: esiste un nuovo processo di distribuzione alternativo per generare contenuto statico quando richiesto (on-demand). In questo modo si riducono i tempi di inattività e si migliora la gestione della cache generando le risorse più critiche.<!-- MAGECLOUD-1285 -->

   - **Nuova variabile di ambiente**- Aggiunto il `SCD_ON_DEMAND` variabile di ambiente globale per generare contenuto statico quando richiesto.<!-- MAGECLOUD-1738 -->

   - **Hook post-distribuzione**- Aggiunto un `post_deploy` aggancio per `.magento.app.yaml` file che cancella la cache e precarica (riscalda) la cache _dopo_ il contenitore inizia ad accettare le connessioni. È disponibile solo per i progetti Pro che contengono ambienti di staging e produzione in [!DNL Cloud Console] e per progetti iniziali. Anche se non obbligatorio, questo funziona insieme al `SCD_ON_DEMAND` variabile di ambiente.<!-- MAGECLOUD-1788 -->

- ![nuova icona](../../assets/new.svg) **Ottimizzazione**- Ottimizzazione dello spostamento o della copia dei file durante la distribuzione per migliorare la velocità di distribuzione e ridurre i carichi sul file system.<!-- MAGECLOUD-1842 -->

- ![nuova icona](../../assets/new.svg) **Registrazione della distribuzione**— È stata aggiunta la possibilità di abilitare i gestori GELF (Extended Log Format) Syslog e Graylog per l&#39;output dei log durante il processo di distribuzione. Consulta [Gestori di registrazione](../environment/log-handlers.md).<!-- MAGECLOUD-1751 -->

- ![nuova icona](../../assets/new.svg) È stato aggiunto quanto segue [**Variabili di ambiente**](../environment/variables-intro.md):

   - `CRYPT_KEY`- Fornisce una chiave di crittografia a un altro ambiente durante lo spostamento di un database.<!-- MAGECLOUD-1556 -->

   - `SKIP_HTML_MINIFICATION`—_Globale_ variabile di ambiente che ignora la copia dei file della vista statica in `var/view_preprocessed` e genera un HTML minimizzato quando richiesto.<!-- MAGECLOUD-1621 and MAGECLOUD-1736-->

   - `SCD_ON_DEMAND`—_Globale_ variabile di ambiente per generare contenuto statico quando richiesto.<!-- MAGECLOUD-1738 -->

   - `WARM_UP_PAGES`- È possibile elencare le pagine da utilizzare per il precaricamento della cache. Disponibile nel nuovo [Variabili post-distribuzione](../environment/variables-post-deploy.md).

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema a causa del quale una patch applicata localmente interrompeva la distribuzione in un’istanza. Ora, ECE-Tools può rilevare che è stata applicata una patch.<!-- MAGECLOUD-982 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un conflitto tra il bundling JavaScript e la funzionalità GZIP. Queste funzioni ora funzionano correttamente insieme.<!-- MAGECLOUD-1735 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava il mancato funzionamento dei comandi CLI di ECE-Tools quando si utilizzavano versioni precedenti di PHP 7.0.x.<!-- MAGECLOUD-1744 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che impediva l’implementazione di contenuti statici con la strategia compatta in più thread.<!-- MAGECLOUD-1822 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema di blocco della sessione Redis che causava un ritardo di accesso dell’amministratore. Inoltre, la correzione è disponibile per 2.1.x.<!-- MAGECLOUD-1853 -->

## v2002.0.9

>[!NOTE]
>
>Devi [aggiornamento del metapackage Adobe Commerce on cloud infrastructure](../dev-tools/install-package.md#update-the-metapackage) per ottenere questo e tutti gli aggiornamenti futuri.

- ![nuova icona](../../assets/new.svg) **strumenti ece**- La `ece-tools` ora supporta Adobe Commerce 2.1.x.<!-- MAGECLOUD-1086 -->

- ![nuova icona](../../assets/new.svg) **Configurazione Redis**- Ora è possibile [configurare Redis](../environment/variables-deploy.md#cache_configuration) pagina e cache predefinita e archiviazione della sessione Redis utilizzando una variabile di ambiente.<!-- MAGECLOUD-1552 -->

- ![nuova icona](../../assets/new.svg) **Miglioramenti ai servizi Search, AMQP e Redis**- Abbiamo unificato il flusso di configurazione del servizio in modo che ora si comporti allo stesso modo per tutti i servizi. Modifica manuale del `env.php` il file per la configurazione dei servizi non è più supportato. È necessario utilizzare le variabili di ambiente o `.magento.env.yaml` al suo posto.<!-- MAGECLOUD-1437 -->

- ![icona correzione](../../assets/fix.svg) **Variabili di ambiente**—

   - L&#39;uso di `env:STATIC_CONTENT_THREADS` è stato dichiarato obsoleto e verrà rimosso in una versione futura. Utilizza il [SCD_THREADS](../environment/variables-deploy.md#scd_threads) invece.<!-- MAGECLOUD-1507 -->

   - Il `STATIC_CONTENT_EXCLUDE_THEMES` variabile di ambiente obsoleta. È necessario utilizzare il `SCD_EXCLUDE_THEMES` variabile di ambiente.<!-- MAGECLOUD-1640 -->

- ![icona correzione](../../assets/fix.svg) **Registrazione**- È stato semplificato il log intorno alle operazioni di applicazione delle patch incorporate.<!-- MAGECLOUD-1674 -->

- ![icona correzione](../../assets/fix.svg) Abbiamo rimosso `developer` e la `APPLICATION_MODE` perché causavano un comportamento imprevisto.<!-- MAGECLOUD-1615 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava errori di distribuzione del contenuto statico relativi a Redis. Ora la distribuzione di contenuti statici multithread viene eseguita come previsto.<!-- MAGECLOUD-1630 -->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che impediva agli utenti di salvare le modifiche ai campi di configurazione nell’amministratore, contrassegnati come sensibili dopo l’esecuzione di `app:config:dump` comando.<!-- MAGECLOUD-1175 -->

- ![icona correzione](../../assets/fix.svg) È stato aggiunto il supporto per una versione precedente di `symfony/yaml` per risolvere i conflitti con alcuni pacchetti non ancora compatibili con la versione più recente.<!-- MAGECLOUD-1674 -->

## v2002.0.8

>[!NOTE]
>
>Ci siamo fusi `vendor/magento/ece-patches` con `vendor/magento/ece-tools` in questa versione. Non è più necessario aggiornare `vendor/magento/ece-patches` separatamente.

**Nuove funzioni:**

- **Registrazione migliorata**<!-- MAGECLOUD-1253 -MAGECLOUD-1495 -->
   - Abbiamo migliorato la funzione di log messaging per fornire spiegazioni migliori quando il processo di build o distribuzione sovrascrive una variabile di ambiente.
   - Ora puoi visualizzare in tempo reale lo stato di avanzamento dell’installazione e dell’aggiornamento. Coda la `install_update.log` per visualizzare l&#39;avanzamento. Ad esempio:

     ```bash
     tail -f var/log/install_upgrade.log
     ```

- **Nuovo comando cron**- È ora possibile sbloccare specifici processi bloccati del cron invece di arrestarli e riavviarli tutti con il [`cron:unlock`](https://support.magento.com/hc/en-us/articles/360033099451) comando. Non disponibile in 2.1.<!-- MAGECLOUD-1367 -->

- **File di configurazione unificato**: ora puoi configurare le fasi di build e distribuzione utilizzando una [`.magento.env.yaml`](https://devdocs.magento.com/cloud/project/magento-env-yaml.html) file.<!-- MAGECLOUD-1369 -->

- **File di configurazione del backup**: il processo di distribuzione ora crea automaticamente un backup del `app/etc/env.php` e `app/etc/config.php` file di configurazione dopo la distribuzione. Abbiamo anche aggiunto un’ [nuovo comando CLI](https://support.magento.com/hc/en-us/articles/360033182871) per ripristinare questi file di configurazione da un backup.<!-- MAGECLOUD-1372 -->

- **Risoluzione dei problemi di convalida**- È stato modificato il comando da utilizzare per risolvere gli errori di convalida durante la `config.php` non contiene dati sufficienti per la distribuzione di contenuto statico. In precedenza, il messaggio di errore indicava di eseguire `bin/magento app:config:dump`. Ora devi eseguire `php ./vendor/bin/ece-tools config:dump`.<!-- MAGECLOUD-1491 -->

- **Nuove variabili di ambiente**- È ora possibile utilizzare le variabili di ambiente per la connessione [ricerca](../environment/variables-deploy.md#search_configuration) e [Basato su AMQP](../environment/variables-deploy.md#queue_configuration) servizi al sito.<!-- MAGECLOUD-1410 -->

- È stata implementata l’applicazione di patch intelligenti. Ora il pacchetto applica patch basate non sulla versione dell’infrastruttura cloud Adobe Commerce, ma sulla versione del pacchetto aggiornata.<!--MAGECLOUD-1090-->

**Problemi risolti:**

- È stato risolto un problema di registrazione che causava errori di compilazione.<!-- MAGECLOUD-1162 -->

- È stato risolto un problema che causava eccezioni di timeout durante l’esecuzione di distribuzioni in modalità interattiva.<!-- MAGECLOUD-1389 -->

- È stato risolto un problema che causava errori durante l’utilizzo della strategia compatta per la generazione di contenuti statici. Non disponibile in 2.1.<!-- MAGECLOUD-1446 MAGECLOUD-1485-->

- È stato risolto un problema che impediva allo script di distribuzione di identificare correttamente gli ambienti di staging e produzione.<!-- MAGECLOUD-1493 -->

- È stato risolto un problema che causava l’interruzione delle connessioni al database da parte di problemi di rete e causava errori durante il processo di installazione e aggiornamento.<!-- MAGECLOUD-1520 -->

- È stato risolto un problema che impediva l’esportazione dei file di configurazione tramite `app:config:dump` più di una volta Non disponibile in 2.1.<!--  MAGECLOUD-1567  -->

- È stata corretta una sessione Redis _blocco_ problema che ha causato un _Amministratore_ ritardo di accesso. Non disponibile in 2.1.<!--  MAGECLOUD-1582  -->

- È stato risolto un problema di implementazione relativo al controllo delle versioni che causava un conflitto con altri moduli di applicazione di patch basati su Compositore.<!-- MAGECLOUD-1450 -->

- È stato risolto un problema che causava problemi di memoria PHP durante l’importazione.<!-- MAGECLOUD-1310 -->

- È stata rimossa la patch; è stato corretto un bug in `colinmollenhour/credis` v1.6 per abilitare il supporto per Adobe Commerce sull’infrastruttura cloud 2.2.1. Non disponibile in 2.1.<!-- MAGECLOUD-1033 -->

## v2002.0.7

**Problemi risolti:**

- Abbiamo rimosso `var/view_preprocessed` symlink per risolvere un problema che causava conflitti di minimizzazione JavaScript.<!-- MAGECLOUD-1454 -->

## v2002.0.6

**Problemi risolti:**

- È stato risolto un problema che causava `gzip` errori quando un nome di file o directory contiene spazi.<!-- MAGECLOUD-1413 -->

- È stato risolto un problema che impediva agli script di distribuzione di riconoscere e abilitare correttamente le dipendenze dei moduli.<!-- MAGECLOUD-1424 -->

## v2002.0.5

**Nuove funzioni:**

- **Configurare un consumer cron con una variabile di ambiente**- È ora possibile configurare i consumatori cron utilizzando il nuovo `CRON_CONSUMERS_RUNNER` variabile di ambiente.

- **Analisi configurazione**: è ora possibile eseguire la scansione dei componenti critici durante il processo di generazione/distribuzione e arrestare il processo in caso di errore della scansione, evitando inutili tempi di inattività dovuti alla modalità di manutenzione del sito.

- **Genera/implementa notifiche**- È stato aggiunto un file di configurazione che è possibile utilizzare per [configurare le notifiche di Slack e/o e-mail](../environment/set-up-notifications.md) per azioni di build/implementazione in tutti gli ambienti.

- **Compressione del contenuto statico**- Compriamo ora il contenuto statico utilizzando [gzip](https://www.gnu.org/software/gzip/) durante le fasi di build e distribuzione. Questa compressione, associata alla compressione Fastly, consente di ridurre le dimensioni dello store e di aumentare la velocità di installazione. Se necessario, è possibile disattivare la compressione utilizzando [opzione build](../environment/variables-build.md) o [variabile di distribuzione](../environment/variables-deploy.md). Per ulteriori informazioni, consulta i seguenti argomenti:

   - [Variabili di ambiente dell’applicazione](../application/variables-property.md)

   - [Prestazioni di distribuzione del contenuto statico](../deploy/static-content.md)

   - [Processo di distribuzione](https://devdocs.magento.com/cloud/reference/discover-deploy.html)

- **Gestione della configurazione**- Viene generata automaticamente una `app/etc/config.php` nell’archivio Git durante la fase di build, se non esiste già. Il file generato automaticamente include solo un elenco di moduli ed estensioni. Se il file esiste già, la fase di build continua come di consueto. Se segue [Gestione configurazione](../store/store-settings.md) in un secondo momento, i comandi aggiornano il file senza richiedere passaggi aggiuntivi. Fai riferimento a [Processo di distribuzione](https://devdocs.magento.com/cloud/reference/discover-deploy.html) per ulteriori informazioni.

- **Dump del database**- È stato aggiunto un `magento/ece-tools` Comando CLI per la creazione di immagini di database in tutti gli ambienti. Per gli ambienti di produzione Pro plan, questo comando esegue il dump solo da uno dei tre nodi ad alta disponibilità, pertanto i dati di produzione scritti in un nodo diverso durante il dump potrebbero non essere copiati. È consigliabile attivare la modalità di manutenzione dell’applicazione prima di eseguire un’immagine del database negli ambienti di produzione. Consulta [Gestione dei backup](https://devdocs.magento.com/cloud/project/project-webint-snap.html#db-dump) per ulteriori informazioni.

- **Limitazioni dell’intervallo di Cron revocate**- L’intervallo cron predefinito per tutti gli ambienti con provisioning nelle aree us-3, eu-3 e ap-3 è di 1 minuto. L’intervallo cron predefinito in tutte le altre aree geografiche è di 5 minuti per gli ambienti di integrazione Pro e di 1 minuto per gli ambienti di staging e produzione Pro. Per modificare i processi cron esistenti, modifica le impostazioni in `.magento.app.yaml` o crea un ticket di supporto per gli ambienti di produzione/staging. Fai riferimento a [Imposta processi cron](../application/crons-property.md#set-up-cron-jobs) per ulteriori informazioni.

**Problemi risolti:**

- È stato risolto un problema che causava tempi di distribuzione lunghi a causa del processo di distribuzione che richiamava `cache-clean` prima della distribuzione del contenuto statico.<!-- MAGECLOUD-1327 -->

- È stato risolto un problema che causava errori durante il passaggio di generazione del contenuto statico della distribuzione negli ambienti di produzione.<!-- MAGECLOUD-1322 -->

- È stato risolto un problema che impediva ad alcuni `magento/ece-tools` comandi dall&#39;output di registrazione a `stderr`.<!-- MAGECLOUD-1264 -->

- È stato risolto un problema che impediva ai valori dell’URL di base di `env.php` dall’aggiornamento nei rami con forking.<!-- MAGECLOUD-1242 -->

- È stato risolto un problema che causava la `magento setup:install` comando per aggiungere un prefisso non sicuro (`http://`) per proteggere gli URL di base.<!-- MAGECLOUD-1171 -->

- È stato risolto un problema che impediva agli errori di patch di causare errori di distribuzione.<!-- MAGECLOUD-1170 -->

- È stato risolto un problema che impediva `ece-tools` dall’interruzione dell’esecuzione e dalla generazione di un’eccezione se non è possibile applicare patch.<!-- MAGECLOUD-1152 -->

- È stato risolto un problema che causava errori durante il caricamento della vetrina dopo l’abilitazione della minimizzazione di HTML in Admin.<!-- MAGECLOUD-1138 -->

## v2002.0.4

**Problemi risolti:**

- Ora puoi [ripristina manualmente i processi cron bloccati](https://support.magento.com/hc/en-us/articles/360033099451) utilizzo di un comando CLI in tutti gli ambienti tramite accesso SSH. Il processo di distribuzione reimposta automaticamente i processi cron.<!-- MAGECLOUD-1355 -->

## v2002.0.3

**Problemi risolti:**

- È stato risolto un problema che causava il timeout delle pagine perché Redis impiegava troppo tempo per la lettura/scrittura. Ora puoi utilizzare la `disable_locking` nelle configurazioni Redis per evitare questo problema.<!-- MAGECLOUD-1311 -->

## v2002.0.2

**Problemi risolti:**

- Il [!DNL RabbitMQ] Il processo di configurazione ora ottiene automaticamente tutti i parametri richiesti.<!-- MAGECLOUD-1246 -->

## v2002.0.1

**Nuove funzioni:**

- Adobe Commerce su infrastruttura cloud ora supporta ambiti e [strategie di distribuzione dei contenuti statici](https://devdocs.magento.com/guides/v2.3/config-guide/cli/config-cli-subcommands-static-deploy-strategies.html). È stato aggiunto il `–s` con un&#39;impostazione predefinita di `quick` per la strategia di distribuzione dei contenuti statici. È possibile utilizzare la variabile di ambiente [SCD_STRATEGY](../environment/variables-deploy.md) personalizzare e utilizzare queste strategie con le azioni di build e distribuzione. Questa variabile supporta le opzioni `standard`, `quick`, o `compact`. Se si seleziona `compact`, sovrascriveremo il `STATIC_CONTENT_THREADS` valore con `1`, che può rallentare l’implementazione, soprattutto negli ambienti di produzione. Non disponibile in 2.1.<!--- MAGECLOUD-1057 -->

- È stato creato un file di registro sugli ambienti per acquisire e compilare le azioni di build e distribuzione. Il `var/log/cloud.log` file nella directory principale dell&#39;applicazione.<!--- MAGECLOUD-1014 & MAGECLOUD-1023 -->

**Problemi risolti:**

- Refactoring di `ece-tools` per renderlo compatibile con Adobe Commerce on cloud infrastructure 2.2.0 e versioni successive.<!-- MAGECLOUD-919 & MAGECLOUD-1030 -->

- È stato risolto un problema che impediva `ece-tools` dall’interruzione dell’esecuzione e dalla generazione di un’eccezione se non è possibile applicare patch.<!-- MAGECLOUD-1186 -->

- È stato risolto un problema che causava la generazione di eccezioni quando la compilazione di iniezione di dipendenza (di) veniva saltata durante le build.<!-- MAGECLOUD-1047 & MAGECLOUD-1049 -->

- È stato risolto un problema che causava la sovrascrittura delle configurazioni Redis personalizzate da parte del processo di distribuzione in `env.php` file.<!-- MAGECLOUD-1019 -->

- È stato risolto un problema che causava loop di reindirizzamento a causa della disabilitazione dell’amministratore protetto predefinito.<!-- MAGECLOUD-1020 -->

## v2002.0.0

>[!WARNING]
>
>Questo pacchetto non è più compatibile con altre versioni di Adobe Commerce sull’infrastruttura cloud e **non deve** essere utilizzati.

### Versione iniziale

Versione iniziale di `ece-tools` per Adobe Commerce su infrastruttura cloud 2.2.0.
