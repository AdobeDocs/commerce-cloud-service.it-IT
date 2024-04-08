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

Il [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker) fornisce funzionalità e immagini Docker per distribuire Adobe Commerce in un ambiente Cloud locale. Queste note sulla versione descrivono gli ultimi miglioramenti apportati a questo pacchetto, che è un componente di [Suite di strumenti cloud per Commerce](cloud-tools-suite.md).

Il `magento/magento-cloud-docker` Il pacchetto utilizza la seguente sequenza di versioni: `<major>.<minor>.<patch>`

Le note sulla versione includono:

- ![nuova icona](../../assets/new.svg) Nuove funzioni
- ![icona correzione](../../assets/fix.svg) Correzioni e miglioramenti

<!--Add release notes below-->

## v1.3.7 {#latest}

Data di rilascio: 8 aprile 2024

- ![nuova icona](../../assets/new.svg) **PHP** — È stato aggiunto il supporto di immagini PHP 8.3 e PHP 8.3.
- ![nuova icona](../../assets/new.svg) **Nginx** — Aggiunto image index v. 1.24.
- ![nuova icona](../../assets/new.svg) **Opensearch** - Aggiunta immagine OpenSearch v. 2.12, 1.3.
- ![nuova icona](../../assets/new.svg) **Compositore** - Versione Composer aggiornata al 2.2.23.

## v1.3.6

Data di rilascio: 31 luglio 2023

- ![nuova icona](../../assets/new.svg) **È stata aggiunta una nuova versione del servizio**— OpenSearch 2.5.
- ![nuova icona](../../assets/new.svg) **Abilita cache del compositore**- Ora è possibile estendere la configurazione Docker per abilitare Composer clear cache all&#39;avvio del contenitore Docker. Consulta [Estendere la configurazione Docker](https://developer.adobe.com/commerce/cloud-tools/docker/configure/extend-docker-configuration/) nel _Cloud Docker per Commerce_ guida.

## v1.3.5

Data di rilascio: 10 marzo 2023

- ![nuova icona](../../assets/new.svg) **ionCube**- Aggiunta dell&#39;estensione ionCube per l&#39;immagine PHP 8.1.
- ![nuova icona](../../assets/new.svg) **Sono state aggiunte nuove versioni del servizio**—OpenSearch 2.3 e 2.4, PHP 8.2, vernice 7.1.1.
- ![nuova icona](../../assets/new.svg) **Supporto avanzato per PHP 8.2**—Sono stati risolti dei problemi di compatibilità con alcune versioni di PHP 8.2.x per supportare Commerce 2.4.6.
- ![icona correzione](../../assets/fix.svg) **Problema del compositore**- Sono stati risolti i problemi che si verificavano dopo l&#39;aggiornamento della versione del Compositore all&#39;interno dei contenitori Docker.

## v1.3.4

Data di rilascio: 27 ottobre 2022

- ![nuova icona](../../assets/new.svg) **Aggiunte nuove immagini vernice**- Aggiunte immagini per vernice 6.5, 7.0 e 7.1.<!-- MCLOUD-7879 -->

## v1.3.3

Data di rilascio: 13 settembre 2022

- ![nuova icona](../../assets/new.svg) **Supporto di Apple M1 (ARM64)**: sono state aggiunte modifiche alle immagini Docker per abilitare il supporto per l’architettura Apple M1 (ARM64).<!-- MCLOUD-7989-2 MCLOUD-7989 -->
- ![icona correzione](../../assets/fix.svg) **Mailhog**— È stato risolto un problema che impediva al servizio Mailhog di ricevere le e-mail in modalità sviluppatore.<!-- MCLOUD-8643 -->
- ![icona correzione](../../assets/fix.svg) **init-docker.sh**- È stata corretta la convalida delle versioni del servizio in `init-docker.sh` script.<!-- MCLOUD-8765 -->

## v1.3.2

Data di rilascio: 31 marzo 2022

- ![nuova icona](../../assets/new.svg) **È stata aggiunta l’immagine Elasticsearch 7.10.**<!-- MCLOUD-8548 -->

## v1.3.1

Data di rilascio: 10 marzo 2022

- ![nuova icona](../../assets/new.svg) **Supporto PHP 8.1**- Aggiunta del supporto per PHP 8.1.
- ![nuova icona](../../assets/new.svg) **OpenSearch**- Sono state aggiunte immagini di OpenSearch versioni 1.1 e 1.2.
- ![nuova icona](../../assets/new.svg) **Compositore 2.1**- Impostare Compositore 2.1.x per default nelle immagini PHP 8.x.
- ![nuova icona](../../assets/new.svg) **Miglioramenti delle immagini PHP**—

   - Sono state aggiunte immagini PHP 8.1
   - Aggiornamento di xDebug versione 3.1.2
   - Xmlrpc 1.0.0RC3 aggiornato

- ![icona correzione](../../assets/fix.svg) **Miglioramenti di Elasticsearch e OpenSearch**- Miglioramenti nei file Dockerfile Elasticsearch e OpenSearch; l&#39;immagine Elasticsearch 5.2 è stata rimossa.
- ![icona correzione](../../assets/fix.svg) **Estensione di sodio**- Attivazione di `sodium` per impostazione predefinita, in tutte le immagini PHP.
- ![icona correzione](../../assets/fix.svg) **Volume cache compositore**: è stato corretto il percorso del volume della cache del Compositore in modo che i pacchetti del Compositore siano memorizzati nella cache.
- ![icona correzione](../../assets/fix.svg) **Limitazione di memoria nell’indice**- Corretta limitazione della memoria nell&#39;immagine NGINX.

## v1.3.0

Data di rilascio: 25 ottobre 2021

- ![icona correzione](../../assets/fix.svg) **Migliora il flusso di lavoro in modalità Sviluppatore**- In precedenza, era necessario specificare la modalità nei passaggi di build e distribuzione. Ora, il `--mode` opzione in `build` passaggio determina la modalità in un secondo momento `deploy` passaggio. Non è più necessario impostare la modalità dopo la distribuzione. Consulta [Modalità sviluppatore](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html).<!-- ACMP-1086 -->
- ![icona correzione](../../assets/fix.svg) **Miglioramenti per il file system di sola lettura**—<!-- ACMP-1106 -->
   - È stato risolto un problema che causava l’avvio di un contenitore PHP per la configurazione della posta.
   - Può utilizzare le variabili di ambiente nei file INI.
   - Verificare che i punti di ingresso PHP non richiedano l&#39;autorizzazione di scrittura.
- ![icona correzione](../../assets/fix.svg) **Aggiorna nodo**— Aggiornare la versione Node in bundle; quando si installa Node in immagini PHP-CLI, ora viene utilizzata la versione LTS corrente.<!-- ACMP-1539 -->
- ![icona correzione](../../assets/fix.svg) **Aggiorna Symfony**- Sono state aggiornate le dipendenze di configurazione di Symfony per renderle compatibili con Adobe Commerce 2.4.4.<!-- ACMP-1533 -->

## v1.2.4

Data di rilascio: 29 luglio 2021

- ![nuova icona](../../assets/new.svg) **Nuovo `Zookeeper` contenitore**- Aggiunto un [Contenitore Zookeeper](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#zookeeper-container) per gestire la configurazione del provider di blocchi per i progetti non distribuiti nell’infrastruttura Adobe Commerce on Cloud.<!--MCLOUD-8000-->

- ![nuova icona](../../assets/new.svg) **È stato aggiunto il supporto per Composer 2.0.**—Compositore versione 2.0 aggiunto al file di configurazione del Compositore per supportare gli aggiornamenti da Compositore 1.0 che sta per terminare il ciclo di vita.<!--MCLOUD-8003-->

## v1.2.3

Data di rilascio: 14 giugno 2021

- ![nuova icona](../../assets/new.svg) **PHP 8.0 aggiunto**- Il PHP è stato aggiornato alla versione 8.0, consentendo di sfruttare tutte le nuove funzioni e le ottimizzazioni incluse in PHP 8.0.<!--MCLOUD-7941-->
- ![nuova icona](../../assets/new.svg) **Aggiornato a Vernice 6.6 e Elasticsearch 7.11.2**: i seguenti collegamenti forniscono informazioni sulla versione di [Cache vernice 6.6](https://varnish-cache.org/releases/rel6.6.0.html#rel6-6-0) e all&#39;Elasticsearch 7.11.2.<!--MCLOUD-7921-->
- ![nuova icona](../../assets/new.svg) **Aggiunto `ioncube` estensione per immagine PHP 7.4**- La `ioncube` L&#39;estensione è stata aggiunta nuovamente all&#39;immagine PHP 7.4 dopo essere stata inizialmente esclusa dall&#39;aggiornamento PHP 7.3 a PHP 7.4. *[Inviato da mattskr](https://github.com/magento/magento-cloud-docker/pull/314).*<!--PR #314-->
- ![nuova icona](../../assets/new.svg) **È stata aggiunta un’opzione di sincronizzazione file:`manual-native`**- La `manual-native` l&#39;opzione di sincronizzazione dei file consente di controllare manualmente la sincronizzazione, garantendo prestazioni ottimali per gli ambienti macOS e Windows. Ulteriori informazioni sull&#39;utilizzo di `manual-native` opzione in [Modalità sviluppatore](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html) e [Sincronizzazione dei dati in un ambiente di sviluppo Docker](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html#file-synchronization-options).<!--MCLOUD-7977-->
- ![nuova icona](../../assets/new.svg) **Eliminazioni di volumi rimosse da `up` e `down` comandi**- La `--volume` è stata rimossa dall&#39;opzione `bin/magento-docker up` e `bin/magento-docker down` comandi, sostituiti dal nuovo `bin/magento-docker init` con un avviso di perdita di dati. Questa modifica consente di evitare la perdita accidentale di dati. *[Inviato da joeshelton-wagento](https://github.com/magento/magento-cloud-docker/pull/319).*<!--PR #319-->
- ![icona correzione](../../assets/fix.svg) **Aggiornato `CN` valore per il certificato generato**- Rimosso il codice fisso `CN` valore dal file Dockerfile. Questo valore ha creato un errore di certificato (`NET::ERR_CERT_INVALID`) che ha causato `--host` opzione per `ece-docker build:compose` da ignorare.<!--MCLOUD-7934-->

## v1.2.2

Data di rilascio: 20 aprile 2021

- ![nuova icona](../../assets/new.svg) **Aggiornato `host.docker.internal` per essere indipendente dalla piattaforma**- È ora possibile creare gli stessi script di composizione Docker per Ubuntu, Windows e macOS. L’utilizzo di Xdebug su Ubuntu non richiede più una variabile di ambiente separata. [Correzione inviata da Igor Vitol](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #298-->
- ![nuova icona](../../assets/new.svg) **Init-docker.sh aggiornato**- Aggiunto il `mounts` oggetto al `MAGENTO_CLOUD_APPLICATION` variabile di ambiente. [Correzione inviata da Chiranjeevi](https://github.com/magento/magento-cloud-docker/pull/299).<!--Issue #299-->
- ![nuova icona](../../assets/new.svg) **Init-docker.sh aggiornato**- Aggiornato il `init-docker.sh` script con le versioni PHP 7.4 e Cloud Docker 1.2.1. [Correzione inviata da Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/300).<!--Issue #300-->
- ![nuova icona](../../assets/new.svg) **Sodio attivato per impostazione predefinita**- Attivazione di `sodium` estensione PHP per impostazione predefinita nelle immagini PHP Docker.<!--MCLOUD-7548-->
- ![nuova icona](../../assets/new.svg) **`custom-registry`opzione**- Aggiunto un `--custom-registry` opzione per `php ./vendor/bin/ece-docker build:compose` per l&#39;utilizzo del proprio registro immagini.<!--MCLOUD-7476-->

  ```bash
  ./vendor/bin/ece-docker build:compose --custom-registry=my-registry.example.com
  ```

- ![nuova icona](../../assets/new.svg) **Versioni Elasticsearch precedenti rimosse**- Le versioni di Elasticsearch 1.7 e 2.4 sono state rimosse dalle immagini di Elasticsearch.<!--MCLOUD-7504-->
- ![nuova icona](../../assets/new.svg) **Generazione automatica di certificati NGINX**- Rimozione dei certificati esistenti dall&#39;immagine NGINX. I certificati NGINX vengono ora generati automaticamente con ogni nuova distribuzione per migliorare la sicurezza.<!--MCLOUD-7396-->
- ![icona correzione](../../assets/fix.svg) **Abilitato`opcache.validate_timestamps`**- Attivazione di `opcache.validate_timestamps` Impostazione PHP predefinita in modalità sviluppatore. L’abilitazione di questa impostazione ha risolto il problema per cui le modifiche al file system non venivano riconosciute in Docker.<!--MCLOUD-7466-->
- ![icona correzione](../../assets/fix.svg) **Fisso`build:custom:compose`**- È stato corretto il `build:custom:compose` per generare un errore quando i file non possono essere sovrascritti durante il processo di compilazione. La generazione di un errore evita le situazioni in cui `docker-compose up` potrebbe usare i file errati.<!--MCLOUD-7457-->
- ![icona correzione](../../assets/fix.svg) **Fisso `--sync_engine="native"` opzione**- È stato risolto il problema relativo alla modalità di produzione (`--mode="production"`), il `--sync_engine="native"` non crea voci per le cartelle locali in `docker.composer.yml` file.<!--MCLOUD-7254-->
- ![icona correzione](../../assets/fix.svg) **Sono stati corretti gli errori di convalida della versione del servizio**— Sono state aggiunte versioni di servizio per [!DNL RabbitMQ], Elasticsearch e altri servizi per `type` proprietà in `MAGENTO_CLOUD_RELATIONSHIP` variabile. Aggiunta di queste versioni al `relationships` la variabile ha corretto gli errori di convalida che si verificavano durante la fase di distribuzione.<!--MCLOUD-7572-->

## v1.2.1

Data di rilascio: 21 dicembre 2020

- ![nuova icona](../../assets/new.svg) **Opzioni comando NGINX**- Sono state aggiunte opzioni di comando build per modificare il numero di NGINX `worker_processes` e NGINX `worker_connections` per TLS e servizi web. Il `worker_process` Il parametro mantiene la possibilità di impostare il valore su `auto`. Esempi: <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --nginx-worker-processes=2
  ./vendor/bin/ece-docker build:compose --nginx-worker-connections=2048
  ```

- ![nuova icona](../../assets/new.svg) **Opzione comando TLS**— È stata aggiunta l&#39;opzione di comando build per creare una configurazione senza il servizio TLS. Esempio: <!--MCLOUD-7259-->

  ```terminal
  ./vendor/bin/ece-docker build:compose --no-tls
  ```

- ![nuova icona](../../assets/new.svg) **Consumo di memoria NGINX**- Riduzione della memoria utilizzata dal processo NGINX per TLS e servizi Web.<!--MCLOUD-7259-->

- ![nuova icona](../../assets/new.svg) **Blackfire**- Blackfire estensione PHP disattivata per impostazione predefinita nell’immagine Cloud Docker.

- ![icona correzione](../../assets/fix.svg) **Contenitore PHP-FPM**- È stato corretto il controllo dello stato del contenitore PHP-FPM modificando il `WEB_PORT` da `80` a `8080`.<!--MCLOUD-7232-->

- ![icona correzione](../../assets/fix.svg) **Denominazione volume non valida**- È stato corretto un errore di denominazione del volume non valida in modalità sviluppatore.<!--MCLOUD-7442-->

- ![icona correzione](../../assets/fix.svg) **Porta upstream NGINX**- L&#39;immagine Docker NGINX 1.19 è stata aggiornata per utilizzare la porta 8080 per evitare un loop infinito. [Correzione inviata da Adarsh Manickam](https://github.com/magento/magento-cloud-docker/pull/296).<!--Issue 295-->

## v1.2.0

Data di rilascio: 9 novembre 2020

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei contenitori—**

   - ![nuova icona](../../assets/new.svg) **Contenitore PHP-FPM**- Aggiunto supporto per l&#39;estensione PHP gnupg. [Correzione inviata da G Arvind di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/210).<!--MCLOUD-5981-->

   - ![icona correzione](../../assets/fix.svg) **Contenitore database**- È stato corretto il controllo dello stato del contenitore del database aggiungendo la password del database richiesta al comando di controllo dello stato.<!--MCLOUD-7122-->

   - ![nuova icona](../../assets/new.svg) **Contenitore Elasticsearch**

      - È stato aggiunto il supporto di Elasticsearch 7.9 per la compatibilità con le prossime versioni di Adobe Commerce.<!--MCLOUD-7190-->

      - **Configurazione del plug-in di Elasticsearch**: è stato aggiunto il supporto per l’utilizzo delle informazioni di configurazione del plug-in Elasticsearch dalla sezione `services.yaml` per generare il file `docker-compose.yaml` un file per un ambiente Cloud Docker for Commerce. Consulta [Plug-in di Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-plugins).<!--MCLOUD-2789-->

      - **Supporto del plug-in di Elasticsearch**- È stato aggiunto il supporto per i seguenti plug-in di Elasticsearch: `analysis-icu`, `analysis-phonetic`, `analysis-stempel`, e `analysis-nori`. Il `analysis-icu` e `analysis-phonetic` I plug-in vengono installati per impostazione predefinita. È possibile aggiungere o rimuovere `analysis-stempel` e `analysis-nori` plug-in in base alle esigenze.<!--MCLOUD-2789-->

   - ![nuova icona](../../assets/new.svg) **Contenitore CLI**

      - **Eseguire comandi all&#39;interno dei contenitori Docker PHP**—Ora è possibile utilizzare Cloud Docker CLI per eseguire comandi all’interno dei contenitori PHP nell’ambiente Docker senza dover installare PHP sull’host. Ad esempio, il comando seguente crea la configurazione:  `./bin/magento-docker php 7.3 vendor/bin/ece-docker build:compose`. Consulta [CLI di Cloud Docker](https://devdocs.magento.com/cloud/docker/docker-quick-reference.html#magento-cloud-docker-cli). [Correzione inviata da G Arvind di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/209).<!--MCLOUD-5982-->

      - Aggiunta del client OpenSSH ai contenitori CLI PHP. Ora puoi utilizzare l’inoltro ssh-agent per Composer, se `composer.json` Il file contiene archivi Git privati che richiedono l’utilizzo di comandi Composer da parte di un client ssh.<!--MCLOUD-6008-->

   - ![icona correzione](../../assets/fix.svg) **Contenitore TLS**- Ora, il [Contenitore TLS](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container) è basato su `https://hub.docker.com/r/magento/magento-cloud-docker-nginx` Immagine Docker anziché immagine CentOS. Questa modifica risolve i problemi che causavano errori durante l’invio di richieste HTTPS tra i contenitori nell’ambiente Cloud Docker.<!--MCLOUD-6469-->

   - ![nuova icona](../../assets/new.svg) **Contenitore di prova**- È stato aggiunto un contenitore di prova per il test dell&#39;applicazione e il `--with-test` opzione per Docker `build:compose` per creare il contenitore solo durante il test nell’ambiente Docker. Consulta [test delle applicazioni](https://devdocs.magento.com/cloud/docker/docker-test-app-mftf.html).<!--MCLOUD-6394-->

   - ![nuova icona](../../assets/new.svg) **Contenitore FPM-XDEBUG**

      - ![nuova icona](../../assets/new.svg) **Configurare Xdebug su Linux**- Aggiunto il `--set-docker-host` opzione per `ece-docker build:compose` comando per configurare `host.docker.internal` valore nel contenitore Xdebug. Questa opzione è necessaria per utilizzare Xdebug su sistemi Linux. Consulta [Configurare Xdebug per Docker](https://devdocs.magento.com/cloud/docker/docker-development-debug.html).<!--MCLOUD-6430-->

      - ![icona correzione](../../assets/fix.svg) Risoluzione della configurazione della variabile Xdebug per Docker ENTRYPOINT `uninitialized "with_xdebug" variable` errori nei registri. [Correzione inviata da Florent Olivaud](https://github.com/magento/magento-cloud-docker/pull/218)<!--MCLOUD-6043-->

- ![nuova icona](../../assets/new.svg) **Modifiche alla configurazione Docker**

   - **Configurazione MailHog**- Ora potete utilizzare quanto segue `ece-docker build:compose` opzioni di comando per disattivare MailHog e specificare le porte: `--no-mailhog`, `--mailhog-http-port`, e `--mailhog-smtp-port`. Consulta [Configurare l’e-mail](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-6898, MCLOUD-6660-->

   - Per Cloud Docker for Commerce 1.2.0 e versioni successive, Adobe ora fornisce immagini Docker per ogni versione della patch e il generatore di configurazione Docker crea la configurazione Docker con una versione della patch specificata, anziché utilizzare la più recente. In precedenza, il generatore di configurazione Docker generava la configurazione utilizzando la versione della patch più recente che poteva interrompere Cloud Docker per ambienti Commerce generati utilizzando una versione precedente.<!--MCLOUD-7093-->

   - **Specificare immagini e versioni personalizzate nella configurazione personalizzata di Cloud Docker**- Aggiornato il `build:custom:compose` con opzioni per specificare immagini e versioni personalizzate durante la generazione di un file di configurazione di composizione Docker personalizzato (`docker-compose.yaml`). Consulta [Creare una configurazione Docker Compose personalizzata](https://devdocs.magento.com/cloud/docker/docker-config-sources.html#build-a-custom-docker-compose-configuration). <!--MCLOUD-7089-->

   - È stata aggiornata la configurazione host Docker per esporre la porta 443 per abilitare l’accesso ad Adobe Commerce (`https://magento2.docker`) da tutti i contenitori CLI. È possibile modificare la porta predefinita aggiungendo `--tls-port` quando viene generato il file di configurazione Docker.<!--MCLOUD-6806-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava un errore nella build di Cloud Docker for Commerce se il `app/etc/env.php` file esistente.<!--MCLOUD-6732-->

- ![icona correzione](../../assets/fix.svg) È stata aggiornata la configurazione della build per sostituire i volumi denominati con volumi regolari al fine di evitare problemi durante la distribuzione di Cloud Docker for Commerce su Linux o Windows Subsystem for Linux (WSL2).<!--MCLOUD-6732-->

- ![icona correzione](../../assets/fix.svg) Aggiornamento dei test funzionali di Cloud Docker for Commerce per supportare Composer 2.0.<!--MCLOUD-7183-->

## v1.1.2

Data di rilascio: 9 settembre 2020

- ![nuova icona](../../assets/new.svg) È stato aggiunto il supporto per l’Elasticsearch 7.7<!--MCLOUD-6219-->

## v1.1.1

Data di rilascio: 5 agosto 2020

- ![icona correzione](../../assets/fix.svg) **Configurazione e-mail aggiornata**: la configurazione predefinita di Cloud Docker for Commerce è stata aggiornata per supportare il servizio MailHog anziché utilizzare SendMail. Consulta [Configurare l’e-mail](https://devdocs.magento.com/cloud/docker/docker-config.html#set-up-email).<!--MCLOUD-5624-->

- ![icona correzione](../../assets/fix.svg) La libreria PS è stata ripristinata nella configurazione dell’ambiente Cloud Docker per la correzione `ps:  command not found` errori.<!--MCLOUD-6621-->

- ![icona correzione](../../assets/fix.svg) È stata aggiornata la configurazione predefinita di Cloud Docker for Commerce per rimuovere il montaggio automatico dei volumi entrypoint del database e MariaDB da correggere. `Cannot create container for service db` errori che possono verificarsi all’avvio dell’ambiente Cloud Docker.

  Ora puoi configurare l’ambiente Cloud Docker per il montaggio delle directory del database aggiungendo le seguenti opzioni alla `ece-docker build:compose` comando: `--with-entry-point` e `with-mariadb-conf`. Consulta [Opzioni di configurazione del servizio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-6424-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei comandi CLI**

| Azione | Comando |
| ------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Aggiungere un punto di ingresso al contenitore del database per ripristinare il database dal backup | `./vendor/bin/ece-docker build:compose --db --with-entrypoint` |
| Aggiungere un volume di configurazione MariaDB | `./vendor/bin/ece-docker build:compose --db --mariadb-conf` |

## v1.1.0

Data di rilascio: 25 giugno 2020

- ![nuova icona](../../assets/new.svg) **È stato aggiunto il supporto per la soluzione per le prestazioni del database diviso**- Ora è possibile configurare e distribuire uno store utilizzando la soluzione di suddivisione delle prestazioni del database nell&#39;ambiente Cloud Docker.<!--MCLOUD-3740-->

- ![nuova icona](../../assets/new.svg) **Supporto per l’implementazione di Adobe Commerce e di Magento Open Source**—Ora è possibile utilizzare Cloud Docker for Commerce per distribuire un ambiente di sviluppo locale per progetti che non sono ospitati su Adobe Commerce nell’infrastruttura cloud.<!--MCLOUD-5667-->

- ![nuova icona](../../assets/new.svg) **Supporto Blackfire.io**- È stato aggiunto il supporto per l&#39;utilizzo di [Estensione Blackfire.io](https://devdocs.magento.com/cloud/docker/docker-config-blackfire-io.html) per test automatizzati delle prestazioni. [Correzione inviata da Adarsh Manickam di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/202)<!--MCLOUD-5857-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei contenitori**

   - **Vernice**- Ora la vernice è la cache predefinita quando si distribuisce Adobe Commerce in un ambiente Cloud Docker utilizzando una versione supportata del modello di applicazione Cloud. Consulta [Contenitore di vernice](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container).<!--MCLOUD-2634-->

   - È stata aggiunta la `--no-varnish` opzione per saltare l’installazione del servizio Vernice quando generi il file di configurazione Cloud Docker.<!--MCLOUD-2634-->

   - ![nuova icona](../../assets/new.svg) **Database**

      - È stato aggiunto il supporto per il database MySQL. Ora è possibile configurare l’ambiente Cloud Docker con MariaDB o MySQL. Consulta [Opzioni di configurazione del servizio](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-configuration-options).<!--MCLOUD-5691-->

      - È stata aggiunta la possibilità di impostare le impostazioni di incremento e offset per la replica del database quando si genera il file di composizione Docker. Consulta [Contenitori di servizi](https://devdocs.magento.com/cloud/docker/docker-containers.html#service-containers).<!--MCLOUD-5735-->

   - ![nuova icona](../../assets/new.svg) **PHP-FPM**

      - È stato aggiunto il supporto per PHP 7.4. [Correzione inviata da Mohanela Murugan di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/198)<!--MCLOUD-198-->

      - Possibilità di copiare un `php.ini` nella directory principale del progetto all’ambiente Cloud Docker e applica le impostazioni PHP personalizzate ai contenitori PHP-FPM e CLI. Consulta [Personalizzare le impostazioni PHP](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#customize-php-settings). [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/130).<!--MCLOUD-6012-->

      - È stato aggiunto un controllo di integrità del contenitore. [Correzione inviata da Visanth Sampath di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/188).<!--MCLOUD-5752-->

   - ![icona correzione](../../assets/fix.svg) **Node.js**- Aggiornamento della versione predefinita di Node.js dalla versione 8 alla versione 10 per migliorare la sicurezza. La versione 8 di Node.js è obsoleta e non è più aggiornata con correzioni di bug o patch di sicurezza. [Correzione inviata da Mohan Elamurugan di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/183).<!--MCLOUD-5586-->

   - ![nuova icona](../../assets/new.svg) **Elasticsearch**

      - È stato aggiunto il supporto per gli Elasticsearch 6.8, 7.2, 7.5 e 7.6.<!--MCLOUD-4050, MCLOUD-5855,MCLOUD-5860-->

      - Possibilità di personalizzare il [Configurazione contenitore Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) quando si genera il file di configurazione di composizione Docker.<!--MCLOUD-3059-->

      - È stata aggiunta la `--no-es` alle opzioni di configurazione del servizio per la generazione del file di configurazione Docker Compose. Utilizzare questa opzione per ignorare l&#39;installazione del contenitore di Elasticsearch e utilizzare invece la ricerca MySQL. Questa opzione è supportata solo per Adobe Commerce versione 2.3.5 e precedenti.<!--MCLOUD-3766-->

   - ![nuova icona](../../assets/new.svg) **Contenitore FPM-XDEBUG**: è stata aggiunta un’opzione di configurazione del servizio per installare e configurare Xdebug per il debug di PHP nell’ambiente Cloud Docker. Consulta [Configura Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html).<!--MCLOUD-4098-->

- ![nuova icona](../../assets/new.svg) **Modifiche alla configurazione Docker**

   - Sono stati aggiunti controlli di integrità per i contenitori del servizio Docker PHP-FPM, Redis, Elasticsearch e MySQL.<!--MCLOUD-3335 and MCLOUD-5856-->

   - La modalità di sincronizzazione file predefinita è stata modificata in `native` in modalità Sviluppatore.<!--MCLOUD-3890 -->

   - Sono state aggiunte informazioni sulla versione all’immagine contenitore del servizio Docker generico durante la generazione del `docker-compose.yml` file.<!--MCLOUD-3878-->

   - È stata migliorata la capacità di gestire risposte di grandi dimensioni dal contenitore PHP-FPM a monte, aumentando il `fastcgi_buffers` per il server Nginx.<!--MCLOUD-5980-->

   - Sono state migliorate le prestazioni di sincronizzazione dei file mutageni aggiungendo una seconda sessione di sincronizzazione per sincronizzare i file in `vendor` directory. Questa modifica impedisce il blocco del mutageno durante il processo di sincronizzazione dei file. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

   - ![nuova icona](../../assets/new.svg) **Aggiornamenti dei comandi CLI**

| Azione | Comando |
| -------- | --------------- |
| Cancella cache Redis | `bin/magento-docker flush-redis` |
| Cancella cache vernice | `bin/magento-docker flush-varnish` |
| Ignora installazione vernice predefinita | `.vendor/bin/ece-docker build:compose --no-varnish`<!--MCLOUD-2634--> |
| [Personalizza opzioni Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --es-env-var`<!--MCLOUD-3059--> |
| [Rimuovi configurazione Elasticsearch](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#elasticsearch-container) | `.vendor/bin/ece-docker build:compose --no-es`<!--MCLOUD-3766--> |
| Configurare il contenitore del database con MySQL versione 5.6 o 5.7 | `./vendor/bin/ece-docker build:compose --db <mysql-version-number> --db-image mysql`<!--MCLOUD-5691--> |
| Specifica URL di base personalizzato | `./vendor/bin/ece-docker build:compose --host=<hostname> --port=<port-number>`<!--MCLOUD-3063--> |
| [Aggiungi contenitore per configurazione Xdebug](https://devdocs.magento.com/cloud/docker/docker-development-debug.html) | `.vendor/bin/ece-docker build:compose --mode developer --sync-engine native --with-xdebug`<!--MCLOUD-4098--> |

- ![icona correzione](../../assets/fix.svg) È stata corretta la configurazione della sincronizzazione dei file mutageni per impedire che mutageni creassero sessioni non aggiornate. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/127).<!--MCLOUD-6010-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema di configurazione che causava errori di sintassi nel registro di composizione Docker all’avvio del contenitore PHP-FPM. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/129)<!--MCLOUD-3958-->

- ![icona correzione](../../assets/fix.svg) Sono stati corretti gli errori di conflitto dei volumi che talvolta si verificavano quando si utilizzavano più ambienti Docker. [Correzione inviata da G Arvind di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/168).

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che causava la `ece-docker build:compose` se la configurazione include Blackfire.io, il comando non riesce. [Correzione inviata da G Arvind di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/199). <!--MCLOUD-5797-->

- ![icona correzione](../../assets/fix.svg) È stata aggiornata la configurazione dell’immagine PHP CLI per evitare errori di memoria insufficiente che si verificavano durante l’installazione di più pacchetti tramite Cloud Docker for Commerce. [Correzione inviata da Mohan Elamurugan di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/197).*<!--MCLOUD-5818-->

- ![icona correzione](../../assets/fix.svg) È stato aggiunto il supporto per più utenti MySQL nell’ambiente Cloud Docker. Nelle versioni precedenti, il `build:compose` operazione non riuscita se `magento.app.yaml` file ha specificato più utenti del database. [Correzione inviata da G Arvind di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/181).<!--MCLOUD-5670-->

- ![icona correzione](../../assets/fix.svg) Rimosso `rsyslog` dai contenitori Cloud Docker for Commerce PHP per risolvere i problemi di compatibilità che hanno causato notifiche di avviso durante la distribuzione. Cloud Docker non utilizza l’utility rsyslog.<!--MCLOUD-6173-->

## v1.0.0

Data di rilascio: 5 febbraio 2020

- ![nuova icona](../../assets/new.svg) **Creato un pacchetto separato da consegnare`Cloud Docker for Commerce`**: il codice sorgente è stato spostato per distribuire Cloud Docker for Commerce dal file `ece-tools` archivio in [nuovo `magento-cloud-docker` archivio](https://github.com/magento/magento-cloud-docker) per mantenere la qualità del codice e fornire versioni indipendenti. Il nuovo pacchetto è una dipendenza per ECE-Tools v2002.1.0 e versioni successive.

  Quando aggiorni gli strumenti ece, aggiorni anche il `magento/magento-cloud-docker` alla versione 1.0.0. Se hai utilizzato Cloud Docker for Commerce con una versione precedente di `ece-tools` (2002.0.x), rivedi il [incompatibilità con le versioni precedenti](backward-incompatible-changes.md) e aggiornare il progetto come script, comandi e processi secondo necessità.

- ![nuova icona](../../assets/new.svg) **Aggiunta del controllo delle versioni alle immagini Docker**- È ora necessario aggiornare `magento/magento-cloud-docker` per ottenere le immagini aggiornate.<!--MAGECLOUD-4737-->

- ![nuova icona](../../assets/new.svg) **Aggiornamenti dei contenitori**—

   - ![nuova icona](../../assets/new.svg) **Contenitore PHP-FPM**—

      - ![nuova icona](../../assets/new.svg) **Aggiunto supporto di Node.js**: l&#39;immagine PHP-FPM è stata aggiornata per supportare le funzionalità node, npm e grunt-cli all&#39;interno del contenitore PHP.<!--MAGECLOUD-3953-->

      - ![nuova icona](../../assets/new.svg) **È stato aggiunto il supporto per [ionCube](https://www.ioncube.com/)**: la configurazione Docker predefinita è stata aggiornata per supportare ionCube nell’ambiente di sviluppo Docker locale.<!--MAGECLOUD-4354-->

   - ![nuova icona](../../assets/new.svg) **Contenitore web**—

      - ![nuova icona](../../assets/new.svg) **Personalizzare la configurazione di NGINX**- Aggiunta della possibilità di montare un `nginx.conf` nell’ambiente Cloud Docker for Commerce. Consulta [Contenitore web](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#web-container).<!--MAGECLOUD-4204-->

      - ![nuova icona](../../assets/new.svg) **Certificati NGINX generati automaticamente**- Il file di configurazione Docker include ora la configurazione per la generazione automatica dei certificati NGINX per il contenitore Web.<!--MAGECLOUD-4258-->

   - ![nuova icona](../../assets/new.svg) **Nuovo contenitore Selenium**- Aggiunto un [Contenitore di selenio](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#selenium-container) per supportare i test delle applicazioni Adobe Commerce tramite il framework di test funzionali di Magento (MFTF).<!--MAGECLOUD-4040-->

   - ![nuova icona](../../assets/new.svg) **[!DNL RabbitMQ]supporto delle versioni**- Aggiornato il [!DNL RabbitMQ] configurazione del contenitore da supportare [!DNL RabbitMQ] versione 3.8.<!--MAGECLOUD-4674-->

   - ![icona correzione](../../assets/fix.svg) **Contenitore di database persistente**- La `magento-db: /var/lib/mysql` Il volume di database ora persiste dopo l&#39;arresto e la rimozione della configurazione Docker e il ripristino quando si riavvia la configurazione Docker. Ora è necessario eliminare manualmente il volume di database. Consulta [Contenitori database].<!--MAGECLOUD-3978-->

   - ![nuova icona](../../assets/new.svg) **Contenitore TLS**—

      - ![nuova icona](../../assets/new.svg) **L&#39;immagine base del contenitore è stata aggiornata per l&#39;utilizzo dell&#39;immagine ufficiale**- La [Contenitore TLS cloud](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#tls-container) l&#39;immagine è ora basata sul file ufficiale `debian:jessie` Immagine Docker.—<!--MAGECLOUD-4163-->

      - ![nuova icona](../../assets/new.svg) **È stato aggiunto il supporto per [Proxy di terminazione TLS Sterlina]**- La [File di configurazione sterlina](https://github.com/magento/magento-cloud-docker/blob/1.0/images/tls/) aggiunge le seguenti variabili ENV per personalizzare la configurazione Docker per il contenitore TLS:

         - **`TimeOut`**- Imposta il valore di timeout TTFB (Time to First Byte). Il valore predefinito è 300 secondi.

         - **`RewriteLocation`**- Determina se per impostazione predefinita il proxy Pound riscrive la posizione nell&#39;URL della richiesta. Impostazione predefinita `0` per evitare che la riscrittura interrompa i reindirizzamenti a siti Web esterni, ad esempio un sito SSO esterno. [Correzione inviata da Sorin Sugar](https://github.com/magento/magento-cloud-docker/pull/37)<!--MAGECLOUD-4061-->

      - ![nuova icona](../../assets/new.svg) Il valore di timeout nella configurazione del contenitore TLS è stato aumentato da 15 a 300 secondi. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

   - ![nuova icona](../../assets/new.svg) **Contenitore di vernice**—

      - ![nuova icona](../../assets/new.svg) **L&#39;immagine base del contenitore è stata aggiornata per l&#39;utilizzo dell&#39;immagine ufficiale**- La [Contenitore per vernice al cloud](https://devdocs.magento.com/cloud/docker/docker-containers-service.html#varnish-container) è ora basato su `centos` Immagine Docker.<!--MAGECLOUD-4163-->

      - ![nuova icona](../../assets/new.svg) **Configurazione di timeout predefinita migliorata**-Aggiunto `.first_byte_timeout` e `.between_bytes_timeout` al contenitore Vernice. Entrambi i valori di timeout sono predefiniti `300s` (5 minuti) [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/78)<!--MAGECLOUD-4460-->

      - ![icona correzione](../../assets/fix.svg) **Ignora vernice durante le sessioni Xdebug**- È stata aggiornata la configurazione del contenitore Vernice per restituire `pass` sulle richieste ricevute quando Xdebug è abilitato. Nelle versioni precedenti, non era possibile utilizzare Xdebug se l’ambiente Docker includeva Vernice. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/111).<!--MAGECLOUD-4873-->

- ![nuova icona](../../assets/new.svg) **Modifiche alla configurazione Docker**—

   - ![nuova icona](../../assets/new.svg) **Gestione di montaggi e volumi per il progetto**- Aggiunta della possibilità di gestire installazioni e volumi quando si avvia un ambiente Docker per lo sviluppo locale. Consulta [Condivisione dei dati del progetto].<!--MAGECLOUD-3248-->

   - ![nuova icona](../../assets/new.svg) **Supporto per la modalità bridge di rete**: è stato aggiunto il supporto per la modalità bridge di rete per abilitare le connessioni tra i contenitori Docker sulla rete locale.<!--MAGECLOUD-4165-->

   - ![nuova icona](../../assets/new.svg) **Contenitore Cron disabilitato per impostazione predefinita**- Per migliorare le prestazioni, il contenitore Cron non è più configurato per impostazione predefinita quando si crea l’ambiente Docker. È possibile utilizzare `--with-cron` sul comando Docker build per aggiungere un contenitore Cron all’ambiente. Consulta [Gestione dei processi cron](https://devdocs.magento.com/cloud/docker/docker-manage-cron-jobs.html).<!--MAGECLOUD-5181-->

   - ![nuova icona](../../assets/new.svg) **Interrompi la sincronizzazione di file di backup di grandi dimensioni**: sono state aggiunte le immagini del database e i file di archivio (ZIP, SQL, GZ e BZ2) all&#39;elenco di esclusione nella `dist/docker-sync.yml` e `dist/mutagen.sh` file. La sincronizzazione di file di grandi dimensioni (>1 GB) può causare un periodo di inattività e i file di backup non richiedono in genere la sincronizzazione, in quanto è possibile rigenerarli.<!--MAGECLOUD-3979-->

- ![nuova icona](../../assets/new.svg) **Modifiche al comando**—

   - ![icona correzione](../../assets/fix.svg) Rinominato il `./bin/docker` file in `./bin/magento-docker` per risolvere un problema che ha causato l’interruzione di alcuni ambienti Docker perché `./bin/docker` Il file sovrascrive i file binari Docker esistenti. Questo è un [modifica non compatibile con le versioni precedenti](backward-incompatible-changes.md) che richiede aggiornamenti agli script e ai comandi.<!-- MAGECLOUD-4038 -->

   - ![nuova icona](../../assets/new.svg) **È stata aggiunta un’opzione di configurazione del servizio per esporre la porta del database all’host**- Utilizza la `--expose-db-port= [Fix submitted by Adarsh Manickam from Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/101).<PORT>` per esporre la porta del database all&#39;host durante la creazione della `docker-compose.yml` file: `bin/ece-docker build:compose --expose-db-port=<PORT>`<!--MAGECLOUD-4454-->

   - ![nuova icona](../../assets/new.svg) **Nuovo comando post-distribuzione**- In precedenza, gli hook di post-distribuzione definiti nella `.magento.app.yaml` file eseguito automaticamente dopo aver distribuito Adobe Commerce in un contenitore Cloud Docker utilizzando `cloud-deploy` comando. Ora, devi emettere un documento separato `cloud-post-deploy` comando per eseguire gli hook post-distribuzione dopo la distribuzione. Consulta le istruzioni di lancio aggiornate per [sviluppatore](https://devdocs.magento.com/cloud/docker/docker-mode-developer.html) e [produzione](https://devdocs.magento.com/cloud/docker/docker-mode-production.html) modalità.<!--MAGECLOUD-3996-->

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la `--rm` opzione per `./bin/magento-docker` comandi per generare e distribuire i contenitori. Questo rimuove il contenitore dopo il completamento dell’attività.<!--MAGECLOUD-4205-->

   - ![nuova icona](../../assets/new.svg) **Aggiornamenti a `build:compose` comando**—

      - ![nuova icona](../../assets/new.svg) È stata aggiunta la `--sync-engine="native"` opzione per `docker-build` per disattivare la sincronizzazione dei file quando si genera il file di configurazione Docker Compose in modalità sviluppatore. Utilizzare questa opzione durante lo sviluppo su sistemi Linux, che non richiedono la sincronizzazione dei file per lo sviluppo Docker locale. Consulta [Sincronizzazione dei dati nell’ambiente Docker](https://devdocs.magento.com/cloud/docker/docker-syncing-data.html).<!--MCLOUD-3231, MCLOUD-3890-->

   - ![nuova icona](../../assets/new.svg) Impostazione di sincronizzazione file predefinita modificata da `docker-sync` a `native`. [Correzione inviata da Mathew Beane di Zilker Technology](https://github.com/magento/magento-cloud-docker/pull/124).<!--MAGECLOUD-5066-->

- ![nuova icona](../../assets/new.svg) **Miglioramenti della convalida**—

   - ![nuova icona](../../assets/new.svg) È stata aggiunta la convalida al processo di distribuzione per gli ambienti di sviluppo Docker locali per verificare che la configurazione dell’ambiente Cloud includa la chiave di crittografia necessaria per decrittografare il database. Ora viene visualizzato un messaggio di errore nel registro se la configurazione dell’ambiente non specifica un valore per la chiave di crittografia.<!--MAGECLOUD-4423-->

   - ![nuova icona](../../assets/new.svg) È stata aggiunta una verifica dello stato del contenitore al servizio Elasticsearch per garantire che sia pronto prima di continuare con l’elaborazione di build e distribuzione. Se il controllo dello stato di integrità restituisce un errore, il contenitore viene riavviato automaticamente.<!--MAGECLOUD-4456-->
