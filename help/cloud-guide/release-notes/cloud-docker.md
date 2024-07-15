---
title: Pacchetto Docker cloud
description: Consulta un elenco degli ultimi miglioramenti apportati al pacchetto Cloud Docker.
feature: Cloud, Docker, Release Notes
recommendations: noDisplay, catalog
last-substantial-update: 2024-04-08T00:00:00Z
exl-id: 907d977f-2e9c-4553-a46b-000bc6a57b28
source-git-commit: bc76cba0219f16fd055c20289811b51c35c9b026
workflow-type: tm+mt
source-wordcount: '3662'
ht-degree: 0%

---

# Pacchetto Docker cloud

Il pacchetto [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) fornisce funzionalità e immagini Docker per distribuire Adobe Commerce in un ambiente cloud locale. Queste note sulla versione descrivono gli ultimi miglioramenti apportati a questo pacchetto, che è un componente di [Cloud Tools Suite per Commerce](cloud-tools-suite.md).

Il pacchetto `magento/magento-cloud-docker` utilizza la seguente sequenza di versioni: `<major>.<minor>.<patch>`

Le note sulla versione includono:

- ![nuova icona](../../assets/new.svg) Nuove funzioni
- ![icona correzione](../../assets/fix.svg) Correzioni e miglioramenti

<!--Add release notes below-->

## v1.3.7 {#latest}

Data di rilascio: 8 aprile 2024

- ![nuova icona](../../assets/new.svg) **PHP** — Aggiunta del supporto per le immagini PHP 8.3 e PHP 8.3.
- ![nuova icona](../../assets/new.svg) **Nginx** — Aggiunta dell&#39;indice dell&#39;immagine v. 1.24.
- ![nuova icona](../../assets/new.svg) **Opensearch** - Aggiunta immagine OpenSearch v. 2.12, 1.3.
- ![nuova icona](../../assets/new.svg) **Compositore** - Versione Compositore aggiornata al 2.2.23.

## v1.3.6

Data di rilascio: 31 luglio 2023

- ![nuova icona](../../assets/new.svg) **Aggiunta nuova versione del servizio**—OpenSearch 2.5.
- ![nuova icona](../../assets/new.svg) **Abilita cache compositore**. Ora è possibile estendere la configurazione Docker per abilitare la cache di cancellazione compositore all&#39;avvio del contenitore Docker. Vedi [Estendere la configurazione Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/) nella guida _Cloud Docker per Commerce_.

## v1.3.5

Data di rilascio: 10 marzo 2023

- ![nuova icona](../../assets/new.svg) **ionCube** - Aggiunta dell&#39;estensione ionCube per l&#39;immagine PHP 8.1.
- ![nuova icona](../../assets/new.svg) **Aggiunte nuove versioni del servizio**—OpenSearch 2.3 e 2.4, PHP 8.2, Varnish 7.1.1.
- ![nuova icona](../../assets/new.svg) **Supporto avanzato per PHP 8.2**—Sono stati risolti alcuni problemi di compatibilità con alcune versioni di PHP 8.2.x per supportare Commerce 2.4.6.
- ![icona di correzione](../../assets/fix.svg) **Problema del Compositore**—Sono stati risolti i problemi che si verificavano dopo l&#39;aggiornamento della versione del Compositore all&#39;interno dei contenitori Docker.

## v1.3.4

Data di rilascio: 27 ottobre 2022

- ![nuova icona](../../assets/new.svg) **Nuove immagini vernice**—Aggiunte immagini per vernice 6.5, 7.0 e 7.1.<!-- MCLOUD-7879 -->

## v1.3.3

Data di rilascio: 13 settembre 2022

- ![nuova icona](../../assets/new.svg) **Supporto di Apple M1 (ARM64)**—Sono state aggiunte modifiche alle immagini Docker per abilitare il supporto per l&#39;architettura di Apple M1 (ARM64).<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![icona correzione](../../assets/fix.svg) **Mailhog**—È stato risolto un problema che impediva al servizio Mailhog di ricevere le e-mail in modalità sviluppatore.<!-- MCLOUD-8643 -->
- ![icona correzione](../../assets/fix.svg) **init-docker.sh**—È stato corretto il programma di convalida delle versioni del servizio nello script `init-docker.sh`.<!-- MCLOUD-8765 -->

## v1.3.2

Data di rilascio: 31 marzo 2022

- ![nuova icona](../../assets/new.svg) **Aggiunta immagine Elasticsearch 7.10**<!-- MCLOUD-8548 -->

## v1.3.1

Data di rilascio: 10 marzo 2022

- ![nuova icona](../../assets/new.svg) **Supporto PHP 8.1**—Aggiunta del supporto per PHP 8.1.
- ![nuova icona](../../assets/new.svg) **OpenSearch**—Sono state aggiunte immagini di OpenSearch versioni 1.1 e 1.2.
- ![nuova icona](../../assets/new.svg) **Compositore 2.1**—Imposta il compositore 2.1.x per impostazione predefinita nelle immagini PHP 8.x.
- ![nuova icona](../../assets/new.svg) **Miglioramenti immagini PHP**—

   - Sono state aggiunte immagini PHP 8.1
   - Aggiornamento di xDebug versione 3.1.2
   - Xmlrpc 1.0.0RC3 aggiornato

- ![icona correzione](../../assets/fix.svg) **Miglioramenti di Elasticsearch e OpenSearch**—Miglioramenti nei Dockerfile Elasticsearch e OpenSearch; l&#39;immagine Elasticsearch 5.2 è stata rimossa.
- ![icona correzione](../../assets/fix.svg) **Estensione sodio**—Abilitazione dell&#39;estensione `sodium` per impostazione predefinita in tutte le immagini PHP.
- ![icona di correzione](../../assets/fix.svg) **Volume della cache del compositore**—È stato corretto il percorso del volume della cache del compositore per la memorizzazione nella cache dei pacchetti del compositore.
- ![icona correzione](../../assets/fix.svg) **Limitazione di memoria in nginx**: è stata corretta la limitazione di memoria nell&#39;immagine NGINX.

## v1.3.0

Data di rilascio: 25 ottobre 2021

- ![icona correzione](../../assets/fix.svg) **Migliora il flusso di lavoro in modalità sviluppatore**. In precedenza era necessario specificare la modalità nei passaggi di compilazione e distribuzione. Ora, l&#39;opzione `--mode` nel passaggio `build` determina la modalità nel passaggio `deploy` successivo. Non è più necessario impostare la modalità dopo la distribuzione. Vedere [Modalità sviluppatore](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html).<!-- ACMP-1086 -->
- ![icona correzione](../../assets/fix.svg) **Miglioramenti per il file system di sola lettura**—<!-- ACMP-1106 -->
   - È stato risolto un problema che causava l’avvio di un contenitore PHP per la configurazione della posta.
   - Può utilizzare le variabili di ambiente nei file INI.
   - Verificare che i punti di ingresso PHP non richiedano l&#39;autorizzazione di scrittura.
- ![icona di correzione](../../assets/fix.svg) **Aggiorna nodo**—Aggiorna la versione del nodo nel bundle; durante l&#39;installazione di Node in immagini PHP-CLI, ora viene utilizzata la versione LTS corrente.<!-- ACMP-1539 -->
- ![icona correzione](../../assets/fix.svg) **Aggiorna Symfony**—Sono state aggiornate le dipendenze di configurazione Symfony per renderle compatibili con Adobe Commerce 2.4.4.<!-- ACMP-1533 -->

## v1.2.4

Data di rilascio: 29 luglio 2021

- ![nuova icona](../../assets/new.svg) **Nuovo contenitore `Zookeeper`**—Aggiunto un contenitore [Zookeeper](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#zookeeper-container) per gestire la configurazione del provider di blocchi per i progetti non distribuiti in Adobe Commerce nell&#39;infrastruttura cloud.<!--MCLOUD-8000-->

- ![nuova icona](../../assets/new.svg) **È stato aggiunto il supporto per Composer 2.0.**—È stata aggiunta la versione 2.0 di Composer al file di configurazione Composer per supportare gli aggiornamenti da Composer 1.0 che si sta avvicinando alla fine del ciclo di vita.<!--MCLOUD-8003-->

## v1.2.3

Data di rilascio: 14 giugno 2021

- ![nuova icona](../../assets/new.svg) **PHP 8.0** aggiunto—PHP aggiornato alla versione 8.0, consente di sfruttare tutte le nuove funzionalità e le ottimizzazioni incluse in PHP 8.0.<!--MCLOUD-7941-->
- ![nuova icona](../../assets/new.svg) **Aggiornato a Vernice 6.6 e Elasticsearch 7.11.2**—I seguenti collegamenti forniscono informazioni sulla versione di [Vernice cache 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) e Elasticsearch 7.11.2.<!--MCLOUD-7921-->
- ![nuova icona](../../assets/new.svg) **Aggiunta estensione `ioncube` per l&#39;immagine PHP 7.4**. L&#39;estensione `ioncube` è stata aggiunta nuovamente all&#39;immagine PHP 7.4 dopo essere stata inizialmente esclusa dall&#39;aggiornamento PHP 7.3 a PHP 7.4. *[Inviato da mattskr](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![nuova icona](../../assets/new.svg) **È stata aggiunta un&#39;opzione di sincronizzazione file:`manual-native`**. L&#39;opzione di sincronizzazione file `manual-native` consente il controllo manuale della sincronizzazione, garantendo prestazioni ottimali per gli ambienti macOS e Windows. Leggi informazioni sull&#39;utilizzo dell&#39;opzione `manual-native` in [modalità sviluppatore](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html) e [Sincronizzazione dei dati in un ambiente di sviluppo Docker](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html#file-synchronization-options).<!--MCLOUD-7977-->
- ![nuova icona](../../assets/new.svg) **Eliminazione del volume rimossa dai comandi `up` e `down`**. L&#39;opzione `--volume` è stata rimossa dai comandi `bin/magento-docker up` e `bin/magento-docker down`, sostituita dal nuovo comando `bin/magento-docker init` con un avviso di perdita di dati. Questa modifica consente di evitare la perdita accidentale di dati. *[Inviato da joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![icona correzione](../../assets/fix.svg) **Valore `CN` aggiornato per il certificato generato**—Il valore `CN` hardcoded è stato rimosso dal Dockerfile. Questo valore ha creato un errore di certificato (`NET::ERR_CERT_INVALID`) che ha causato l&#39;ignoramento dell&#39;opzione `--host` per il comando `ece-docker build:compose`.<!--MCLOUD-7934-->

## v1.2.2

Data di rilascio: 20 aprile 2021

- ![nuova icona](../../assets/new.svg) **Aggiornamento di `host.docker.internal` per renderlo indipendente dalla piattaforma**. È ora possibile creare gli stessi script di composizione Docker per Ubuntu, Windows e macOS. L’utilizzo di Xdebug su Ubuntu non richiede più una variabile di ambiente separata. [Correzione inviata da Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![nuova icona](../../assets/new.svg) **Aggiornamento di init-docker.sh**. L&#39;oggetto `mounts` è stato aggiunto alla variabile di ambiente `MAGENTO_CLOUD_APPLICATION`. [Correzione inviata da Chiranjeevi](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![nuova icona](../../assets/new.svg) **Aggiornato init-docker.sh**—Aggiornato lo script `init-docker.sh` con le versioni PHP 7.4 e Cloud Docker 1.2.1. [Correzione inviata da Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![nuova icona](../../assets/new.svg) **Sodio abilitato per impostazione predefinita**. Abilitazione dell&#39;estensione PHP `sodium` per impostazione predefinita nelle immagini Docker PHP.<!--MCLOUD-7548-->
- ![nuova icona](../../assets/new.svg) **`custom-registry`opzione**—Aggiunta di un&#39;opzione `--custom-registry` al comando `php ./vendor/bin/ece-docker build:compose` per l&#39;utilizzo del Registro di sistema delle immagini personale.<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![nuova icona](../../assets/new.svg) **Versioni Elasticsearch precedenti rimosse**—Versioni Elasticsearch 1.7 e 2.4 rimosse dalle immagini Elasticsearch.<!--MCLOUD-7504-->
- ![nuova icona](../../assets/new.svg) **Generazione automatica dei certificati NGINX**—Rimossi i certificati esistenti dall&#39;immagine NGINX. I certificati NGINX vengono ora generati automaticamente con ogni nuova distribuzione per migliorare la sicurezza.<!--MCLOUD-7396-->
- ![icona correzione](../../assets/fix.svg) **Abilitato`opcache.validate_timestamps`**. Impostazione PHP `opcache.validate_timestamps` abilitata per impostazione predefinita in modalità sviluppatore. L&#39;abilitazione di questa impostazione ha risolto il problema che impediva il riconoscimento delle modifiche al file system nel Docker.<!--MCLOUD-7466-->
- ![icona correzione](../../assets/fix.svg) **Correzione di`build:custom:compose`**. Il comando `build:custom:compose` è stato corretto in modo da generare un errore quando i file non possono essere sovrascritti durante il processo di compilazione. La generazione di un errore impedisce le situazioni in cui `docker-compose up` potrebbe utilizzare i file errati.<!--MCLOUD-7457-->
- ![icona correzione](../../assets/fix.svg) **Correzione dell&#39;opzione `--sync_engine="native"`**—È stato risolto il problema che impediva all&#39;opzione `--sync_engine="native"` in modalità di produzione (`--mode="production"`) di creare voci per cartelle locali nel file `docker.composer.yml`.<!--MCLOUD-7254-->
- ![icona correzione](../../assets/fix.svg) **Sono stati corretti gli errori di convalida della versione del servizio**. Sono state aggiunte versioni del servizio per [!DNL RabbitMQ], Elasticsearch e altri servizi alla proprietà `type` nella variabile `MAGENTO_CLOUD_RELATIONSHIP`. L&#39;aggiunta di queste versioni alla variabile `relationships` ha corretto gli errori di convalida che si verificavano durante la fase di distribuzione.<!--MCLOUD-7572-->

## v1.2.1

Data di rilascio: 21 dicembre 2020

- ![nuova icona](../../assets/new.svg) **Opzioni comando NGINX**—Sono state aggiunte opzioni comando build per modificare il numero di NGINX `worker_processes` e NGINX `worker_connections` per TLS e servizi Web. Il parametro `worker_process` mantiene la possibilità di impostare il valore su `auto`. Esempi: <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![nuova icona](../../assets/new.svg) **Opzione comando TLS**—Aggiunta dell&#39;opzione comando build per creare una configurazione senza il servizio TLS. Esempio: <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![nuova icona](../../assets/new.svg) **Consumo di memoria NGINX**—È stata ridotta la memoria utilizzata dal processo NGINX per TLS e servizi Web.<!--MCLOUD-7259-->

- ![nuova icona](../../assets/new.svg) **Blackfire**—L&#39;estensione Blackfire PHP è disabilitata per impostazione predefinita nell&#39;immagine Cloud Docker.

- ![icona correzione](../../assets/fix.svg) **contenitore PHP-FPM**—È stato corretto il controllo di integrità del contenitore PHP-FPM modificando `WEB_PORT` da `80` a `8080`.<!--MCLOUD-7232-->

- ![icona correzione](../../assets/fix.svg) **Denominazione volume non valida**. È stato corretto un errore di denominazione volume non valida in modalità sviluppatore.<!--MCLOUD-7442-->

- ![icona correzione](../../assets/fix.svg) **Porta a monte NGINX**. Aggiornamento dell&#39;immagine Docker NGINX 1.19 per l&#39;utilizzo della porta 8080 per evitare un loop infinito. [Correzione inviata da Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

Data di rilascio: 9 novembre 2020

- ![nuova icona](../../assets/new.svg) **Aggiornamenti contenitore—**

   - ![nuova icona](../../assets/new.svg) **contenitore PHP-FPM**—Aggiunto supporto per l&#39;estensione PHP gnupg. [Correzione inviata da G Arvind da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![icona correzione](../../assets/fix.svg) **Contenitore database**—È stato corretto il controllo dello stato del contenitore del database aggiungendo la password del database richiesta al comando di controllo dello stato.<!--MCLOUD-7122-->

   - ![nuova icona](../../assets/new.svg) **Contenitore Elasticsearch**

      - È stato aggiunto il supporto per l&#39;Elasticsearch 7.9 per la compatibilità con le prossime versioni di Adobe Commerce.<!--MCLOUD-7190-->

      - **Configurazione del plug-in di Elasticsearch**. È stato aggiunto il supporto per l&#39;utilizzo delle informazioni di configurazione del plug-in di Elasticsearch dal file `services.yaml` per generare il file `docker-compose.yaml` per un ambiente Cloud Docker per Commerce. Vedi [plugin di Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Supporto plug-in di Elasticsearch** - Aggiunto supporto per i plug-in di Elasticsearch seguenti: `analysis-icu`, `analysis-phonetic`, `analysis-stempel` e `analysis-nori`. I plug-in `analysis-icu` e `analysis-phonetic` sono installati per impostazione predefinita. È possibile aggiungere o rimuovere i plug-in `analysis-stempel` e `analysis-nori` in base alle esigenze.<!--MCLOUD-2789-->

   - ![nuova icona](../../assets/new.svg) **Contenitore CLI**

      - **Eseguire comandi all&#39;interno dei contenitori Docker PHP**. Ora è possibile utilizzare Cloud Docker CLI per eseguire comandi all&#39;interno dei contenitori PHP nell&#39;ambiente Docker senza dover installare PHP sull&#39;host. Il comando seguente, ad esempio, genera la configurazione: `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`. Consulta [Cloud Docker CLI](https://devdocs.magento.com/cloud/docker/docker-quick-reference.html#magento-cloud-docker-cli). [Correzione inviata da G Arvind da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - Aggiunta del client OpenSSH ai contenitori CLI PHP. È ora possibile utilizzare l&#39;inoltro ssh-agent per Composer se il file `composer.json` contiene archivi Git privati che richiedono l&#39;utilizzo di comandi Composer da parte di un client SSH.<!--MCLOUD-6008-->

   - ![icona di correzione](../../assets/fix.svg) **contenitore TLS**. Ora il [contenitore TLS](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container) si basa sull&#39;immagine Docker `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` anziché sull&#39;immagine CentOS. Questa modifica risolve i problemi che causavano errori durante l&#39;invio di richieste HTTPS tra contenitori nell&#39;ambiente Docker Cloud.<!--MCLOUD-6469-->

   - ![nuova icona](../../assets/new.svg) **Contenitore di test**. È stato aggiunto un contenitore di test per i test dell&#39;applicazione e l&#39;opzione `--with-test` al comando Docker `build:compose` per creare il contenitore solo quando si esegue il test nell&#39;ambiente Docker. Vedi [test applicazioni](https://devdocs.magento.com/cloud/docker/docker-test-app-mftf.html).<!--MCLOUD-6394-->

   - ![nuova icona](../../assets/new.svg) **FPM-XDEBUG contenitore**

      - ![nuova icona](../../assets/new.svg) **Configura Xdebug su Linux**. Aggiunta dell&#39;opzione `--set-docker-host` al comando `ece-docker build:compose` per configurare il valore `host.docker.internal` nel contenitore Xdebug. Questa opzione è necessaria per utilizzare Xdebug su sistemi Linux. Vedere [Configurare Xdebug per Docker](https://devdocs.magento.com/cloud/docker/docker-development-debug.html).<!--MCLOUD-6430-->

      - ![icona correzione](../../assets/fix.svg) È stata corretta la configurazione della variabile Xdebug per Docker ENTRYPOINT per risolvere `uninitialized "with_xdebug" variable` errori nei registri. [Correzione inviata da Florent Olivaud](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![nuova icona](../../assets/new.svg) **Modifiche alla configurazione Docker**

   - **Configurazione MailHog**. È ora possibile utilizzare le seguenti opzioni di comando `ece-docker build:compose` per disabilitare MailHog e specificare le porte: `--no-mailhog`, `--mailhog-http-port` e `--mailhog-smtp-port`. Vedi [Configura e-mail](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - Per Cloud Docker per Commerce 1.2.0 e versioni successive, Adobe ora fornisce immagini Docker per ogni versione della patch e il generatore di configurazione Docker crea la configurazione Docker con una versione della patch specificata, anziché utilizzare la più recente. In precedenza, il generatore di configurazione Docker generava la configurazione utilizzando la versione della patch più recente che poteva interrompere Cloud Docker per gli ambienti Commerce generati utilizzando una versione precedente.<!--MCLOUD-7093-->

   - **Specificare le immagini e le versioni personalizzate nella configurazione personalizzata di Cloud Docker**. Il comando `build:custom:compose` è stato aggiornato con opzioni che consentono di specificare le immagini e le versioni personalizzate durante la generazione di un file di configurazione di composizione Docker personalizzato (`docker-compose.yaml`). Vedi [Creare una configurazione Docker Compose personalizzata](https://devdocs.magento.com/cloud/docker/docker-config-sources.html#build-a-custom-docker-compose-configuration). <!--MCLOUD-7089-->

   - Aggiornamento della configurazione host Docker per esporre la porta 443 per abilitare l&#39;accesso ad Adobe Commerce (`https://magento2.docker`) da tutti i contenitori CLI. È possibile modificare la porta predefinita aggiungendo l&#39;opzione `--tls-port` quando si genera il file di configurazione Docker.<!--MCLOUD-6806-->

- ![icona di correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore nella compilazione di Cloud Docker for Commerce se il file `app/etc/env.php` esiste.<!--MCLOUD-6732-->

- ![icona correzione](../../assets/fix.svg) Aggiornamento della configurazione di compilazione per sostituire volumi con nomi con volumi regolari per evitare problemi durante la distribuzione di Cloud Docker per Commerce su Linux o Windows Subsystem for Linux (WSL2).<!--MCLOUD-6732-->

- ![icona di correzione](../../assets/fix.svg) Aggiornamento di Cloud Docker per i test funzionali di Commerce per supportare Composer 2.0.<!--MCLOUD-7183-->

## v1.1.2

Data di rilascio: 9 settembre 2020

- ![nuova icona](../../assets/new.svg) Aggiunto supporto per Elasticsearch 7.7<!--MCLOUD-6219-->

## v1.1.1

Data di rilascio: 5 agosto 2020

- ![icona correzione](../../assets/fix.svg) **Configurazione e-mail aggiornata** - Aggiornamento della configurazione predefinita di Cloud Docker per Commerce per supportare il servizio MailHog anziché utilizzare SendMail. Vedi [Configura e-mail](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-5624-->

- ![icona di correzione](../../assets/fix.svg) La libreria PS è stata ripristinata nella configurazione dell&#39;ambiente Cloud Docker per correggere `ps:  command not found` errori.<!--MCLOUD-6621-->

- ![icona di correzione](../../assets/fix.svg) Aggiornamento della configurazione predefinita di Cloud Docker per Commerce per rimuovere il montaggio automatico dei volumi entrypoint del database e MariaDB per correggere `Cannot create container for service db` errori che possono verificarsi all&#39;avvio dell&#39;ambiente Cloud Docker.

  Ora è possibile configurare l&#39;ambiente Cloud Docker per il montaggio delle directory del database aggiungendo le seguenti opzioni al comando `ece-docker build:compose`: `--with-entry-point` e `with-mariadb-conf`. Vedi [Opzioni di configurazione del servizio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-6424-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti del comando CLI**

| Azione | Comando |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Aggiungere un punto di ingresso al contenitore del database per ripristinare il database dal backup | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| Aggiungere un volume di configurazione MariaDB | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

Data di rilascio: 25 giugno 2020

- ![nuova icona](../../assets/new.svg) **Aggiunto supporto per la soluzione delle prestazioni del database diviso**. Ora è possibile configurare e distribuire un archivio utilizzando la soluzione delle prestazioni del database diviso nell&#39;ambiente Cloud Docker.<!--MCLOUD-3740-->

- ![nuova icona](../../assets/new.svg) **Supporto per la distribuzione di Adobe Commerce e di Magento Open Source**. Ora puoi utilizzare Cloud Docker per Commerce per distribuire un ambiente di sviluppo locale per progetti non ospitati in Adobe Commerce nell&#39;infrastruttura cloud.<!--MCLOUD-5667-->

- ![nuova icona](../../assets/new.svg) **Supporto Blackfire.io**—È stato aggiunto il supporto per l&#39;utilizzo dell&#39;[estensione Blackfire.io](https://devdocs.magento.com/cloud/docker/docker-config-blackfire-io.html) per il test automatico delle prestazioni. [Correzione inviata da Adarsh Manickam da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti contenitore**

   - **Vernice**: ora la vernice è la cache predefinita quando si distribuisce Adobe Commerce in un ambiente Cloud Docker utilizzando una versione supportata del modello di applicazione Cloud. Vedi [Contenitore vernice](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container).<!--MCLOUD-2634-->

   - È stata aggiunta l&#39;opzione `--no-varnish` per ignorare l&#39;installazione del servizio Varnish quando si genera il file di configurazione Cloud Docker.<!--MCLOUD-2634-->

   - ![nuova icona](../../assets/new.svg) **Database**

      - È stato aggiunto il supporto per il database MySQL. Ora è possibile configurare l’ambiente Cloud Docker con MariaDB o MySQL. Vedi [Opzioni di configurazione del servizio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-5691-->

      - È stata aggiunta la possibilità di impostare le impostazioni di incremento e offset per la replica del database quando si genera il file di composizione Docker. Vedi [Contenitori di servizi](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-containers).<!--MCLOUD-5735-->

   - ![nuova icona](../../assets/new.svg) **PHP-FPM**

      - È stato aggiunto il supporto per PHP 7.4. [Correzione inviata da Mohanela Murugan di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - È stata aggiunta la possibilità di copiare un file `php.ini` nella directory principale del progetto nell&#39;ambiente Cloud Docker e di applicare impostazioni PHP personalizzate ai contenitori PHP-FPM e CLI. Consulta [Personalizzare le impostazioni PHP](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#customize-php-settings). [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - È stato aggiunto un controllo di integrità del contenitore. [Correzione inviata da Visanth Sampath dalla tecnologia Zilker](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![icona correzione](../../assets/fix.svg) **Node.js**—Aggiornamento della versione predefinita di Node.js dalla versione 8 alla versione 10 per migliorare la sicurezza. La versione 8 di Node.js è obsoleta e non è più aggiornata con correzioni di bug o patch di sicurezza. [Correzione inviata da Mohan Elamurugan da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![nuova icona](../../assets/new.svg) **Elasticsearch**

      - È stato aggiunto il supporto per gli Elasticsearch 6.8, 7.2, 7.5 e 7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - È stata aggiunta la possibilità di personalizzare la [configurazione contenitore Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) quando si genera il file di configurazione di composizione Docker.<!--MCLOUD-3059-->

      - Opzione `--no-es` aggiunta alle opzioni di configurazione del servizio per la generazione del file di configurazione Docker Compose. Utilizzare questa opzione per ignorare l&#39;installazione del contenitore di Elasticsearch e utilizzare invece la ricerca MySQL. Questa opzione è supportata solo per Adobe Commerce versione 2.3.5 e precedenti.<!--MCLOUD-3766-->

   - ![nuova icona](../../assets/new.svg) **FPM-XDEBUG container**—È stata aggiunta un&#39;opzione di configurazione del servizio per installare e configurare Xdebug per il debug di PHP nell&#39;ambiente Cloud Docker. Vedere [Configurare Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html).<!--MCLOUD-4098-->

- ![nuova icona](../../assets/new.svg) **Modifiche alla configurazione Docker**

   - Sono stati aggiunti controlli di integrità per i contenitori del servizio Docker PHP-FPM, Redis, Elasticsearch e MySQL.<!--MCLOUD-3335 and MCLOUD-5856-->

   - La modalità di sincronizzazione file predefinita è stata modificata in `native` in modalità Sviluppatore.<!--MCLOUD-3890 -->

   - Sono state aggiunte informazioni sulla versione all&#39;immagine contenitore del servizio Docker generico durante la generazione del file `docker-compose.yml`.<!--MCLOUD-3878-->

   - È stata migliorata la capacità di gestire risposte di grandi dimensioni dal contenitore PHP-FPM a monte aumentando il valore `fastcgi_buffers` per il server Nginx.<!--MCLOUD-5980-->

   - Sono state migliorate le prestazioni di sincronizzazione dei file mutageni aggiungendo una seconda sessione di sincronizzazione per sincronizzare i file nella directory `vendor`. Questa modifica impedisce il blocco del mutageno durante il processo di sincronizzazione dei file. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![nuova icona](../../assets/new.svg) **Aggiornamenti del comando CLI**

| Azione | Comando |
| -------- | --------------- |
| Cancella cache Redis | `bin/magento-docker flush-redis` |
| Cancella cache vernice | `bin/magento-docker flush-varnish` |
| Ignora installazione vernice predefinita | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Personalizza opzioni Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Rimuovi configurazione Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| Configurare il contenitore del database con MySQL versione 5.6 o 5.7 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| Specifica URL di base personalizzato | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Aggiungi contenitore per la configurazione Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![icona correzione](../../assets/fix.svg) È stata corretta la configurazione della sincronizzazione dei file mutageni per impedire che mutageni creino sessioni non aggiornate. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema di configurazione che causava errori di sintassi nel registro di composizione Docker all&#39;avvio del contenitore PHP-FPM. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![icona di correzione](../../assets/fix.svg) sono stati corretti gli errori di conflitto dei volumi che talvolta si verificavano quando si utilizzano più ambienti Docker. [Correzione inviata da G Arvind della tecnologia Zilker](https://github.com/magento/magento-cloud-docker/pull/168).

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava il mancato funzionamento del comando `ece-docker build:compose` se la configurazione includeva Blackfire.io. [Correzione inviata da G Arvind dalla tecnologia Zilker](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![icona di correzione](../../assets/fix.svg) Aggiornamento della configurazione dell&#39;immagine PHP CLI per evitare errori di memoria insufficiente che si sono verificati durante l&#39;installazione di più pacchetti tramite Cloud Docker per Commerce. [Correzione inviata da Mohan Elamurugan da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![icona di correzione](../../assets/fix.svg) Aggiunto supporto per più utenti MySQL nell&#39;ambiente Cloud Docker. Nelle versioni precedenti, l&#39;operazione `build:compose` non è riuscita se il file `magento.app.yaml` ha specificato più utenti del database. [Correzione inviata da G Arvind da Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![icona di correzione](../../assets/fix.svg) Rimosso `rsyslog` dai contenitori Cloud Docker for Commerce PHP per risolvere i problemi di compatibilità che hanno causato le notifiche di avviso durante la distribuzione. Cloud Docker non utilizza l&#39;utilità rsyslog.<!--MCLOUD-6173-->

## v1.0.0

Data di rilascio: 5 febbraio 2020

- ![nuova icona](../../assets/new.svg) **È stato creato un pacchetto separato per il recapito di`Cloud Docker for Commerce`**. Il codice sorgente è stato spostato per il recapito di Cloud Docker per Commerce dall&#39;archivio `ece-tools` all&#39;archivio [nuovo `magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) per mantenere la qualità del codice e fornire versioni indipendenti. Il nuovo pacchetto è una dipendenza per ECE-Tools v2002.1.0 e versioni successive.

  Quando si aggiorna ece-tools, si aggiorna anche il pacchetto `magento/magento-cloud-docker` alla versione 1.0.0. Se hai utilizzato Cloud Docker per Commerce con una versione precedente di `ece-tools` (2002.0.x), controlla le [incompatibilità con le versioni precedenti](backward-incompatible-changes.md) e aggiorna il progetto come script, comandi e processi, in base alle esigenze.

- ![nuova icona](../../assets/new.svg) **È stato aggiunto il controllo delle versioni alle immagini Docker**. Per ottenere le immagini aggiornate, è ora necessario aggiornare il pacchetto `magento/magento-cloud-docker`.<!--MAGECLOUD-4737-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti contenitore**—

   - ![nuova icona](../../assets/new.svg) **contenitore PHP-FPM**—

      - ![nuova icona](../../assets/new.svg) **Aggiunta del supporto Node.js**—Aggiornamento dell&#39;immagine PHP-FPM per supportare le funzionalità node, npm e grunt-cli all&#39;interno del contenitore PHP.<!--MAGECLOUD-3953-->

      - ![nuova icona](../../assets/new.svg) **Aggiunto supporto per [ionCube](https://www.ioncube.com/)**—È stata aggiornata la configurazione Docker predefinita per supportare ionCube nell&#39;ambiente di sviluppo Docker locale.<!--MAGECLOUD-4354-->

   - ![nuova icona](../../assets/new.svg) **Contenitore Web**—

      - ![nuova icona](../../assets/new.svg) **Personalizza configurazione NGINX**. È stata aggiunta la possibilità di montare un file `nginx.conf` personalizzato nell&#39;ambiente Cloud Docker per Commerce. Vedi [Contenitore Web](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#web-container).<!--MAGECLOUD-4204-->

      - ![nuova icona](../../assets/new.svg) **Certificati NGINX generati automaticamente**. Il file di configurazione Docker include ora la configurazione per la generazione automatica dei certificati NGINX per il contenitore Web.<!--MAGECLOUD-4258-->

   - ![nuova icona](../../assets/new.svg) **Nuovo contenitore Selenium**. Aggiunta di un [contenitore Selenium](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#selenium-container) per supportare il test delle applicazioni Adobe Commerce tramite il Framework di test funzionali di Magento (MFTF).<!--MAGECLOUD-4040-->

   - ![nuova icona](../../assets/new.svg) **[!DNL RabbitMQ]supporto versione**—Aggiornamento della configurazione del contenitore [!DNL RabbitMQ] per supportare [!DNL RabbitMQ] versione 3.8.<!--MAGECLOUD-4674-->

   - ![icona correzione](../../assets/fix.svg) **Contenitore di database persistente**. Il volume di database `magento-db: /var/lib/mysql` ora persiste dopo l&#39;arresto e la rimozione della configurazione Docker e il ripristino quando si riavvia la configurazione Docker. Ora è necessario eliminare manualmente il volume di database. Vedi [Contenitori di database].<!--MAGECLOUD-3978-->

   - ![nuova icona](../../assets/new.svg) **Contenitore TLS**—

      - ![nuova icona](../../assets/new.svg) **L&#39;immagine base del contenitore è stata aggiornata per l&#39;utilizzo dell&#39;immagine ufficiale**. L&#39;immagine del contenitore [Cloud TLS](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container) è ora basata sull&#39;immagine ufficiale Docker `debian:jessie`.—<!--MAGECLOUD-4163-->

      - ![nuova icona](../../assets/new.svg) **Aggiunto supporto per il proxy di terminazione TLS [Pound]**. Il [file di configurazione Pound](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) aggiunge le seguenti variabili ENV per personalizzare la configurazione Docker per il contenitore TLS:

         - **`TimeOut`** - Imposta il valore di timeout TTFB (Time to First Byte). Il valore predefinito è 300 secondi.

         - **`RewriteLocation`** - Determina se il proxy Pound riscrive la posizione nell&#39;URL della richiesta per impostazione predefinita. Impostazione predefinita: `0` per impedire che la riscrittura interrompa i reindirizzamenti a siti Web esterni come un sito SSO esterno. [Correzione inviata da Sorin Sugar](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![nuova icona](../../assets/new.svg) Il valore di timeout nella configurazione del contenitore TLS è stato aumentato da 15 a 300 secondi. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![nuova icona](../../assets/new.svg) **Contenitore vernice**—

      - ![nuova icona](../../assets/new.svg) **L&#39;immagine base del contenitore è stata aggiornata per l&#39;utilizzo dell&#39;immagine ufficiale**. Il [contenitore vernice cloud](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container) è ora basato sull&#39;immagine Docker ufficiale `centos`.<!--MAGECLOUD-4163-->

      - ![nuova icona](../../assets/new.svg) **Configurazione di timeout predefinita migliorata**-Aggiunta della configurazione `.first_byte_timeout` e `.between_bytes_timeout` al contenitore di Varnish. Entrambi i valori di timeout sono impostati per impostazione predefinita su `300s` (5 minuti). [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![icona correzione](../../assets/fix.svg) **Ignora vernice durante le sessioni Xdebug**—È stata aggiornata la configurazione del contenitore Vernice per restituire `pass` nelle richieste ricevute quando Xdebug è abilitato. Nelle versioni precedenti, non era possibile utilizzare Xdebug se l’ambiente Docker includeva Vernice. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![nuova icona](../../assets/new.svg) **Modifiche alla configurazione Docker**—

   - ![nuova icona](../../assets/new.svg) **Gestione di installazioni e volumi per il progetto**. È stata aggiunta la possibilità di gestire installazioni e volumi all&#39;avvio di un ambiente Docker per lo sviluppo locale. Vedi [Condivisione dei dati del progetto].<!--MAGECLOUD-3248-->

   - ![nuova icona](../../assets/new.svg) **Supporto per la modalità bridge di rete**. Aggiunta del supporto per la modalità bridge di rete per abilitare le connessioni tra i contenitori Docker sulla rete locale.<!--MAGECLOUD-4165-->

   - ![nuova icona](../../assets/new.svg) **Contenitore Cron disabilitato per impostazione predefinita**. Per migliorare le prestazioni, il contenitore Cron non è più configurato per impostazione predefinita quando si genera l&#39;ambiente Docker. È possibile utilizzare l&#39;opzione `--with-cron` nel comando di build Docker per aggiungere un contenitore Cron all&#39;ambiente. Vedi [Gestione dei processi cron](https://devdocs.magento.com/cloud/docker/docker-manage-cron-jobs.html).<!--MAGECLOUD-5181-->

   - ![nuova icona](../../assets/new.svg) **Interrompi la sincronizzazione di file di backup di grandi dimensioni**. Sono stati aggiunti file di archivio e immagini DB (ZIP, SQL, GZ e BZ2) all&#39;elenco di esclusione nei file `dist/docker-sync.yml` e `dist/mutagen.sh`. La sincronizzazione di file di grandi dimensioni (>1 GB) può causare un periodo di inattività e i file di backup non richiedono in genere la sincronizzazione, poiché è possibile rigenerarli.<!--MAGECLOUD-3979-->

- ![nuova icona](../../assets/new.svg) **Modifiche al comando**—

   - ![icona correzione](../../assets/fix.svg) Il file `./bin/docker` è stato rinominato `./bin/magento-docker` per risolvere un problema che ha causato l&#39;interruzione di alcuni ambienti Docker perché il file `./bin/docker` sovrascrive i file binari Docker esistenti. Si tratta di una [modifica non compatibile con le versioni precedenti](backward-incompatible-changes.md) che richiede aggiornamenti agli script e ai comandi.<!-- MAGECLOUD-4038 -->

   - ![nuova icona](../../assets/new.svg) **Aggiunta di un&#39;opzione di configurazione del servizio per esporre la porta del database all&#39;host**. Utilizzare l&#39;opzione `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` per esporre la porta del database all&#39;host durante la creazione del file `docker-compose.yml`: `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![nuova icona](../../assets/new.svg) **Nuovo comando post-distribuzione**. In precedenza, gli hook post-distribuzione definiti nel file `.magento.app.yaml` venivano eseguiti automaticamente dopo la distribuzione di Adobe Commerce in un contenitore Cloud Docker tramite il comando `cloud-deploy`. Ora è necessario emettere un comando `cloud-post-deploy` separato per eseguire gli hook post-distribuzione dopo la distribuzione. Vedere le istruzioni di avvio aggiornate per la modalità [sviluppatore](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html) e [produzione](https://devdocs.magento.com/cloud/docker/docker-mode-production.html).<!--MAGECLOUD-3996-->

   - ![nuova icona](../../assets/new.svg) Aggiunta dell&#39;opzione `--rm` ai comandi `./bin/magento-docker` per i contenitori di compilazione e distribuzione. Questo rimuove il contenitore dopo il completamento dell&#39;attività.<!--MAGECLOUD-4205-->

   - ![nuova icona](../../assets/new.svg) **Aggiornamenti al comando `build:compose`**—

      - ![nuova icona](../../assets/new.svg) Aggiunta dell&#39;opzione `--sync-engine="native"` al comando `docker-build` per disabilitare la sincronizzazione dei file quando si genera il file di configurazione Docker Compose in modalità sviluppatore. Utilizzare questa opzione durante lo sviluppo su sistemi Linux, che non richiedono la sincronizzazione dei file per lo sviluppo Docker locale. Vedere [Sincronizzazione dei dati nell&#39;ambiente Docker](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![nuova icona](../../assets/new.svg) ha cambiato l&#39;impostazione predefinita di sincronizzazione file da `docker-sync` a `native`. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![nuova icona](../../assets/new.svg) **Miglioramenti alla convalida**—

   - ![nuova icona](../../assets/new.svg) Aggiunta della convalida al processo di distribuzione per gli ambienti di sviluppo Docker locali per verificare che la configurazione dell&#39;ambiente Cloud includa la chiave di crittografia necessaria per decrittografare il database. Ora viene visualizzato un messaggio di errore nel registro se la configurazione dell&#39;ambiente non specifica un valore per la chiave di crittografia.<!--MAGECLOUD-4423-->

   - ![nuova icona](../../assets/new.svg) Aggiunta di un controllo dello stato del contenitore al servizio Elasticsearch per verificare che il servizio sia pronto prima di continuare con l&#39;elaborazione di compilazione e distribuzione. Se la verifica stato restituisce un errore, il contenitore viene riavviato automaticamente.<!--MAGECLOUD-4456-->
