---
title: Messaggi di errore per il pacchetto ECE-Tools
description: Visualizza un elenco di codici di errore e messaggi che possono verificarsi durante i processi di creazione, distribuzione e post-distribuzione dell’infrastruttura cloud di Adobe Commerce.
recommendations: noDisplay
role: Developer
exl-id: d8cc8d49-32da-43cf-a105-aa56b5334000
source-git-commit: 9dda6fe7f6a9d6064436820a3c8426ec982b5230
workflow-type: tm+mt
source-wordcount: '2763'
ht-degree: 4%

---

# Messaggi di errore per gli strumenti ECE

Questo riferimento ai messaggi di errore fornisce informazioni per la risoluzione dei problemi che possono verificarsi durante i processi di creazione, distribuzione e post-distribuzione di Adobe Commerce su infrastrutture cloud.

Tutti i messaggi di errore critici e di avvertenza che si verificano durante la distribuzione vengono scritti in entrambi `var/log/cloud.log` e `/var/log/cloud.error.log` file. Il file di registro degli errori del cloud contiene solo errori della distribuzione più recente. Un file vuoto indica una distribuzione riuscita senza errori.

In `cloud.error.log` file, ogni voce viene formattata come stringa JSON per facilitarne l’analisi:

```json
{"errorCode":1006,"stage":"build","step":"validate-config","suggestion":"No stores/website/locales found in config.php\n  To speed up the deploy process do the following:\n  1. Using SSH, log in to your Magento Cloud account\n  2. Run \"php ./vendor/bin/ece-tools config:dump\"\n  3. Using SCP, copy the app/etc/config.php file to your local repository\n  4. Add, commit, and push your changes to the app/etc/config.php file","title":"The configured state is not ideal","type":"warning"}
```

I messaggi di errore sono suddivisi in categorie in base a una delle fasi di distribuzione: compilazione, distribuzione e post-distribuzione. Ogni sezione fornisce un elenco degli errori associati con le seguenti informazioni per ogni errore:

- **Codice di errore**: identificatore assegnato da Adobe Commerce per il messaggio di errore
- **Fase**: indica se l’errore si è verificato durante la fase di build, distribuzione o post-distribuzione
- **Passaggio**: indica il passaggio nello scenario di distribuzione che può restituire l’errore. Se il _Passaggio_ è vuota, l’errore è generale e può essere restituito da più passaggi o durante le operazioni di pre-elaborazione. Consulta [Distribuzione basata su scenari](../deploy/scenario-based.md) per ulteriori informazioni sui passaggi di build, distribuzione e post-distribuzione.
- **Suggerimento**: fornisce indicazioni per la risoluzione dei problemi e la risoluzione dell’errore
- **Titolo (descrizione errore)**: descrizione che riepiloga la causa dell’errore
- **Tipo**: indica se l’errore è critico o di avvertenza

<!-- Note: The error code tables in this file are auto-generated from source code. To request changes to error code descriptions or suggestions, submit a GitHub issue to the magento/ece-tools repository. -->

## Errori critici

Gli errori critici indicano un problema nella configurazione del progetto Commerce su infrastruttura cloud che causa un errore di distribuzione, ad esempio una configurazione errata, non supportata o mancante per le impostazioni richieste. Prima di poter distribuire, devi aggiornare la configurazione per risolvere questi errori.

### Fase build

| Codice di errore | Passaggio build | Descrizione errore (titolo) | Azione suggerita |
| - | - | - | - |
| 2 |  | Impossibile scrivere su `./app/etc/env.php` file | Lo script di distribuzione non può apportare le modifiche necessarie al `/app/etc/env.php` file. Verifica le autorizzazioni del file system. |
| 3 |  | La configurazione non è definita nel `schema.yaml` file | La configurazione non è definita nel `./vendor/magento/ece-tools/config/schema.yaml` file. Verifica che il nome della variabile di configurazione sia corretto e definito. |
| 4 |  | Impossibile analizzare `.magento.env.yaml` file | Il `./.magento.env.yaml` formato file non valido. Utilizza un parser YAML per verificare la sintassi e correggere eventuali errori. |
| 5 |  | Impossibile leggere `.magento.env.yaml` file | Impossibile leggere `./.magento.env.yaml` file. Verificare le autorizzazioni del file. |
| 6 |  | Impossibile leggere `.schema.yaml` file | Impossibile leggere `./vendor/magento/ece-tools/config/magento.env.yaml` file. Controllare le autorizzazioni per i file e ridistribuirli (`magento-cloud environment:redeploy`). |
| 7 | refresh-module | Impossibile scrivere su `./app/etc/config.php` file | Lo script di distribuzione non può apportare le modifiche necessarie al `/app/etc/config.php` file. Verifica le autorizzazioni del file system. |
| 8 | validate-config | Impossibile leggere `composer.json` file | Impossibile leggere `./composer.json` file. Verificare le autorizzazioni del file. |
| 9 | validate-config | Il `composer.json` nel file manca la sezione di caricamento automatico richiesta | Obbligatorio `autoload` manca dalla sezione `composer.json` file. Confrontare la sezione del caricamento automatico con `composer.json` nel modello Cloud, quindi aggiungi la configurazione mancante. |
| 10 | validate-config | Il `.magento.env.yaml` il file contiene un’opzione non dichiarata nello schema o un’opzione configurata con un valore o una fase non validi | Il `./.magento.env.yaml` il file contiene una configurazione non valida. Per informazioni dettagliate, consulta il registro degli errori. |
| 11 | refresh-module | Comando non riuscito: `/bin/magento module:enable --all` | Prova a eseguire `composer update` localmente. Quindi, esegui il commit e invia l&#39;aggiornamento `composer.lock` file. Inoltre, seleziona la `cloud.log` per ulteriori informazioni. Per un output di comando più dettagliato, aggiungere `VERBOSE_COMMANDS: '-vvv'` opzione per `.magento.env.yaml` file. |
| 12 | apply-patches | Impossibile applicare la patch |  |
| 13 | set-report-dir-nesting-level | Impossibile scrivere nel file `/pub/errors/local.xml` |  |
| 14 | copy-sample-data | Impossibile copiare i file di dati di esempio |  |
| 15 | compile-di | Comando non riuscito: `/bin/magento setup:di:compile` | Controlla la `cloud.log` per ulteriori informazioni. Aggiungi `VERBOSE_COMMANDS: '-vvv'` in `.magento.env.yaml` per un output di comando più dettagliato. |
| 16 | dump-autoload | Comando non riuscito: `composer dump-autoload` | Il `composer dump-autoload` comando non riuscito. Controlla la `cloud.log` per ulteriori informazioni. |
| 17 | frullatore | Comando da eseguire `Baler` per bundling JavaScript non riuscito | Controlla la `SCD_USE_BALER` variabile di ambiente per verificare che il modulo Baler sia configurato e abilitato per il bundle JS. Se non è necessario il modulo Baler, impostare `SCD_USE_BALER: false`. |
| 18 | compress-static-content | Utility richiesta non trovata (timeout, bash) |  |
| 19 | deploy-static-content | Comando `/bin/magento setup:static-content:deploy` non riuscito | Controlla la `cloud.log` per ulteriori informazioni. Per un output di comando più dettagliato, aggiungere `VERBOSE_COMMANDS: '-vvv'` opzione per `.magento.env.yaml` file. |
| 20 | compress-static-content | Compressione del contenuto statico non riuscita | Controlla la `cloud.log` per ulteriori informazioni. |
| 21 | backup-data: static-content | Impossibile copiare il contenuto statico in `init` directory | Controlla la `cloud.log` per ulteriori informazioni. |
| 22 | backup-data: writable-dirs | Impossibile copiare alcune directory scrivibili in `init` directory | Impossibile copiare le directory scrivibili in `./init` cartella. Verifica le autorizzazioni del file system. |
| 23 |  | Impossibile creare un oggetto logger |  |
| 24 | backup-data: static-content | Impossibile pulire il `./init/pub/static/` directory | Pulizia non riuscita `./init/pub/static` cartella. Verifica le autorizzazioni del file system. |
| 25 |  | Impossibile trovare il pacchetto Compositore | Se hai installato la versione dell’applicazione Adobe Commerce direttamente dall’archivio GitHub, verifica che `DEPLOYED_MAGENTO_VERSION_FROM_GIT` variabile di ambiente configurata. |
| 26 | validate-config | Rimuovi la configurazione del modulo Braintree di Magento che non è più supportata in Adobe Commerce e Magento Open Source 2.4 e versioni successive. | Il supporto per il modulo Braintree non è più incluso in Magento 2.4.0 e versioni successive. Rimuovere la variabile CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL dalla sezione delle variabili del `.magento.app.yaml` file. Per il supporto del pagamento Braintree, utilizza invece un&#39;estensione ufficiale dalla Commerce Marketplace. |

### Fase di distribuzione

| Codice di errore | Passaggio di distribuzione | Descrizione errore (titolo) | Azione suggerita |
| - | - | - | - |
| 101 | pre-distribuzione: cache | Configurazione cache non corretta (porta o host mancante) | Parametri richiesti mancanti nella configurazione della cache `server` o `port`. Controlla la `cloud.log` per ulteriori informazioni. |
| 102 |  | Impossibile scrivere su `./app/etc/env.php` file | Lo script di distribuzione non può apportare le modifiche necessarie al `/app/etc/env.php` file. Verifica le autorizzazioni del file system. |
| 103 |  | La configurazione non è definita nel `schema.yaml` file | La configurazione non è definita nel `./vendor/magento/ece-tools/config/schema.yaml` file. Verifica che il nome della variabile di configurazione sia corretto e che sia definito. |
| 104 |  | Impossibile analizzare `.magento.env.yaml` file | La configurazione non è definita nel `./vendor/magento/ece-tools/config/schema.yaml` file. Verifica che il nome della variabile di configurazione sia corretto e che sia definito. |
| 105 |  | Impossibile leggere `.magento.env.yaml` file | Impossibile leggere `./.magento.env.yaml` file. Verificare le autorizzazioni del file. |
| 106 |  | Impossibile leggere `.schema.yaml` file |  |
| 107 | pre-distribuzione: clean-redis-cache | Impossibile pulire la cache Redis | Impossibile pulire la cache Redis. Verificare che la configurazione della cache Redis sia corretta e che il servizio Redis sia disponibile. Consulta [Configurazione del servizio Redis](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/redis.html). |
| 108 | pre-distribuzione: set-production-mode | Comando `/bin/magento maintenance:enable` non riuscito | Controlla la `cloud.log` per ulteriori informazioni. Per un output di comando più dettagliato, aggiungere `VERBOSE_COMMANDS: '-vvv'` opzione per `.magento.env.yaml` file. |
| 109 | validate-config | Configurazione del database errata | Verifica che la `DATABASE_CONFIGURATION` la variabile di ambiente è configurata correttamente. |
| 110 | validate-config | Configurazione di sessione errata | Verifica che la `SESSION_CONFIGURATION` la variabile di ambiente è configurata correttamente. La configurazione deve contenere almeno `save` parametro. |
| 111 | validate-config | Configurazione di ricerca errata | Verifica che la `SEARCH_CONFIGURATION` la variabile di ambiente è configurata correttamente. La configurazione deve contenere almeno `engine` parametro. |
| 112 | validate-config | Configurazione risorsa errata | Verifica che la `RESOURCE_CONFIGURATION` la variabile di ambiente è configurata correttamente. La configurazione deve contenere almeno `connection` parametro. |
| 113 | validate-config:elasticsuite-integrity | ElasticSuite è installato, ma il servizio Elasticsearch non è disponibile | Verifica che la `SEARCH_CONFIGURATION` la variabile di ambiente è configurata correttamente e verifica che il servizio Elasticsearch sia disponibile. |
| 114 | validate-config:elasticsuite-integrity | ElasticSuite è installato, ma viene utilizzato un altro motore di ricerca | ElasticSuite è installato, ma è configurato un altro motore di ricerca. Aggiornare il `SEARCH_CONFIGURATION` per abilitare Elasticsearch e verificare la configurazione del servizio Elasticsearch in `services.yaml` file. |
| 115 |  | Esecuzione query database non riuscita |  |
| 116 | install-update: setup | Comando `/bin/magento setup:install` non riuscito | Controlla la `cloud.log` e `install_upgrade.log` per ulteriori informazioni. Per un output di comando più dettagliato, aggiungere `VERBOSE_COMMANDS: '-vvv'` opzione per `.magento.env.yaml` file. |
| 117 | install-update: config-import | Comando `app:config:import` non riuscito | Controlla la `cloud.log` per ulteriori informazioni. Per un output di comando più dettagliato, aggiungere `VERBOSE_COMMANDS: '-vvv'` opzione per `.magento.env.yaml` file. |
| 118 |  | Utility richiesta non trovata (timeout, bash) |  |
| 119 | install-update: deploy-static-content | Comando `/bin/magento setup:static-content:deploy` non riuscito | Controlla la `cloud.log` per ulteriori informazioni. Per un output di comando più dettagliato, aggiungere `VERBOSE_COMMANDS: '-vvv'` opzione per `.magento.env.yaml` file. |
| 120 | compress-static-content | Compressione del contenuto statico non riuscita | Controlla la `cloud.log` per ulteriori informazioni. |
| 121 | deploy-static-content:generate | Impossibile aggiornare la versione distribuita | Impossibile aggiornare `./pub/static/deployed_version.txt` file. Verifica le autorizzazioni del file system. |
| 122 | clean-static-content | Impossibile pulire i file di contenuto statico |  |
| 123 | install-update: split-db | Comando `/bin/magento setup:db-schema:split` non riuscito | Controlla la `cloud.log` per ulteriori informazioni. Per un output di comando più dettagliato, aggiungere `VERBOSE_COMMANDS: '-vvv'` opzione per `.magento.env.yaml` file. |
| 124 | clean-view-pre-processed | Impossibile pulire il `var/view_preprocessed` cartella | Impossibile pulire `./var/view_preprocessed` cartella. Verifica le autorizzazioni del file system. |
| 125 | install-update: reset-password | Impossibile aggiornare `/var/credentials_email.txt` file | Impossibile aggiornare `/var/credentials_email.txt` file. Verifica le autorizzazioni del file system. |
| 126 | install-update: update | Comando `/bin/magento setup:upgrade` non riuscito | Controlla la `cloud.log` e `install_upgrade.log` per ulteriori informazioni. Per un output di comando più dettagliato, aggiungere `VERBOSE_COMMANDS: '-vvv'` opzione per `.magento.env.yaml` file. |
| 127 | clean-cache | Comando `/bin/magento cache:flush` non riuscito | Controlla la `cloud.log` per ulteriori informazioni. Per un output di comando più dettagliato, aggiungere `VERBOSE_COMMANDS: '-vvv'` opzione per `.magento.env.yaml` file. |
| 128 | disable-maintenance-mode | Comando `/bin/magento maintenance:disable` non riuscito | Controlla la `cloud.log` per ulteriori informazioni. Aggiungi `VERBOSE_COMMANDS: '-vvv'` in `.magento.env.yaml` per un output di comando più dettagliato. |
| 129 | install-update: reset-password | Impossibile leggere il modello di reimpostazione password |  |
| 130 | install-update: cache_type | Comando non riuscito: `php ./bin/magento cache:enable` | Comando `php ./bin/magento cache:enable` viene eseguito solo quando è stato installato Adobe Commerce, ma `./app/etc/env.php` il file era assente o vuoto all’inizio della distribuzione. Controlla la `cloud.log` per ulteriori informazioni. Aggiungi `VERBOSE_COMMANDS: '-vvv'` in `.magento.env.yaml` per un output di comando più dettagliato. |
| 131 | install-update | Il `crypt/key`  il valore chiave non esiste in `./app/etc/env.php` file o `CRYPT_KEY` variabile di ambiente cloud | Questo errore si verifica se `./app/etc/env.php` non è presente all’inizio della distribuzione di Adobe Commerce o se `crypt/key` valore non definito. Se è stata eseguita la migrazione del database da un altro ambiente, recuperare il valore della chiave di crittografia da tale ambiente. Quindi, aggiungi il valore alla [CHIAVE_CRITTOGRAFIA](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/env/stage/variables-deploy.html#crypt_key) variabile di ambiente cloud nell’ambiente corrente. Consulta [Chiave di crittografia Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/develop/overview.html#gather-credentials). Se ha rimosso accidentalmente il `./app/etc/env.php` file, utilizza il seguente comando per ripristinarlo dai file di backup creati da una distribuzione precedente: `./vendor/bin/ece-tools backup:restore` Comando CLI.&quot; |
| 132 |  | Impossibile connettersi al servizio Elasticsearch | Verifica che siano presenti credenziali di Elasticsearch valide e che il servizio sia in esecuzione |
| 137 |  | Impossibile connettersi al servizio OpenSearch | Verificare che siano presenti credenziali OpenSearch valide e che il servizio sia in esecuzione |
| 133 | validate-config | Rimuovi la configurazione del modulo Braintree di Magento che non è più supportata in Adobe Commerce o Magento Open Source 2.4 e versioni successive. | Il supporto per il modulo Braintree non è più incluso in Adobe Commerce o Magento Open Source 2.4.0 e versioni successive. Rimuovere la variabile CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL dalla sezione delle variabili del `.magento.app.yaml` file. Per il supporto Braintree, utilizza invece un’estensione ufficiale per i pagamenti di Braintree dalla Commerce Marketplace. |
| 134 | validate-config | Adobe Commerce e Magento Open Source 2.4.0 richiedono l’installazione del servizio Elasticsearch | Installa servizio Elasticsearch |
| 138 | validate-config | Adobe Commerce e Magento Open Source 2.4.4 richiedono l’installazione del servizio OpenSearch o Elasticsearch | Installare il servizio OpenSearch |
| 135 | validate-config | Il motore di ricerca deve essere impostato su Elasticsearch per Adobe Commerce e Magento Open Source >= 2.4.0 | Controlla la variabile SEARCH_CONFIGURATION per `engine` opzione. Se è configurata, rimuovi l’opzione o imposta il valore su &quot;elasticsearch&quot;. |
| 136 | validate-config | Il database diviso è stato rimosso a partire da Adobe Commerce e dal Magento Open Source 2.5.0. | Se si utilizza un database diviso, è necessario ripristinare o eseguire la migrazione a un singolo database oppure utilizzare un approccio alternativo. |
| 139 | validate-config | Motore di ricerca errato | Questa versione di Adobe Commerce o Magento Open Source non supporta OpenSearch. È necessario utilizzare le versioni 2.3.7-p3, 2.4.3-p2 o successive |

### Fase post-distribuzione

| Codice di errore | Passaggio post-distribuzione | Descrizione errore (titolo) | Azione suggerita |
| - | - | - | - |
| 201 | is-deploy-failed | Fase di distribuzione non riuscita |  |
| 202 |  | Il `./app/etc/env.php` file non scrivibile | Lo script di distribuzione non può apportare le modifiche necessarie al `/app/etc/env.php` file. Verifica le autorizzazioni del file system. |
| 203 |  | La configurazione non è definita nel `schema.yaml` file | La configurazione non è definita nel `./vendor/magento/ece-tools/config/schema.yaml` file. Verifica che il nome della variabile di configurazione sia corretto e che sia definito. |
| 204 |  | Impossibile analizzare `.magento.env.yaml` file | Il `./.magento.env.yaml` formato file non valido. Utilizza un parser YAML per verificare la sintassi e correggere eventuali errori. |
| 205 |  | Impossibile leggere `.magento.env.yaml` file | Verificare le autorizzazioni del file. |
| 206 |  | Impossibile leggere `.schema.yaml` file |  |
| 207 | riscaldamento | Impossibile precaricare alcune pagine di riscaldamento |  |
| 208 | time-to-firs-byte | Impossibile verificare il tempo al primo byte (TTFB) |  |
| 227 | clean-cache | Comando `/bin/magento cache:flush` non riuscito | Controlla la `cloud.log` per ulteriori informazioni. Aggiungi `VERBOSE_COMMANDS: '-vvv'` in `.magento.env.yaml` per un output di comando più dettagliato. |

### Generale

| Codice di errore | Passaggio generale | Descrizione errore (titolo) | Azione suggerita |
| - | - | - | - |
| 243 |  | La configurazione non è definita nel `schema.yaml` file | Verifica che il nome della variabile di configurazione sia corretto e che sia stato definito. |
| 244 |  | Impossibile analizzare `.magento.env.yaml` file | Il `./.magento.env.yaml` formato file non valido. Utilizza un parser YAML per verificare la sintassi e correggere eventuali errori. |
| 245 |  | Impossibile leggere `.magento.env.yaml` file | Impossibile leggere `./.magento.env.yaml` file. Verificare le autorizzazioni del file. |
| 246 |  | Impossibile leggere `.schema.yaml` file |  |
| 247 |  | Impossibile generare un modulo per l’evento | Controlla la `cloud.log` per ulteriori informazioni. |
| 248 |  | Impossibile abilitare un modulo per l’evento | Controlla la `cloud.log` per ulteriori informazioni. |
| 249 |  | Impossibile generare il modulo AdobeCommerceWebPlugins | Controlla la `cloud.log` per ulteriori informazioni. |
| 250 |  | Impossibile abilitare il modulo AdobeCommerceWebhookPlugins | Controlla la `cloud.log` per ulteriori informazioni. |

## Errori di avvertenza

Gli errori di avvertenza indicano un problema nella configurazione del progetto di infrastruttura cloud di Commerce, ad esempio impostazioni di configurazione errate, obsolete, non supportate o mancanti per le funzioni facoltative che possono influire sul funzionamento del sito. Anche se un avviso non causa errori di distribuzione, è necessario rivedere i messaggi di avviso e aggiornare la configurazione per risolverli.

### Fase build

| Codice di errore | Passaggio build | Descrizione errore (titolo) | Azione suggerita |
| - | - | - | - |
| 1001 | validate-config | Il file app/etc/config.php non esiste |  |
| 1002 | validate-config | Il.Il file /build_options.ini non è più supportato |  |
| 1003 | validate-config | Sezione dei moduli mancante nel file di configurazione condiviso |  |
| 1004 | validate-config | Configurazione non compatibile con questa versione del Magento |  |
| 1005 | validate-config | Opzioni SCD ignorate |  |
| 1006 | validate-config | Lo stato configurato non è ideale |  |
| 1007 | frullatore | Impossibile utilizzare il bundling Baler JS |  |

### Fase di distribuzione

| Codice di errore | Passaggio di distribuzione | Descrizione errore (titolo) | Azione suggerita |
| - | - | - | - |
| 2001 | pre-distribuire:cache | Cache configurata per un servizio Redis non disponibile. La configurazione verrà ignorata. |  |
| 2002 | validate-config | Lo stato configurato non è ideale |  |
| 2003 | validate-config | Il valore del livello di nidificazione della directory per la segnalazione degli errori non è stato configurato |  |
| 2004 | validate-config | Configurazione non valida in .file /pub/errors/local.xml. |  |
| 2005 | validate-config | I dati di amministrazione vengono utilizzati per creare un utente amministratore solo durante l’installazione iniziale. Eventuali modifiche apportate ai dati dell’amministratore vengono ignorate durante il processo di aggiornamento. | Dopo l’installazione iniziale, puoi rimuovere i dati di amministrazione dalla configurazione. |
| 2006 | validate-config | L’utente amministratore non è stato creato perché l’e-mail amministratore non è stata impostata | Dopo l’installazione, puoi creare manualmente un utente amministratore: utilizza ssh per connettersi all’ambiente. Quindi, esegui `bin/magento admin:user:create` comando. |
| 2007 | validate-config | Aggiorna versione php alla versione consigliata |  |
| 2008 | validate-config | Il supporto Solr è stato dichiarato obsoleto in Adobe Commerce e Magento Open Source 2.1. |  |
| 2009 | validate-config | Solr non è più supportato da Adobe Commerce e Magento Open Source 2.2 o versioni successive. |  |
| 2010 | validate-config | Il servizio Elasticsearch viene installato a livello di infrastruttura, ma non viene utilizzato come motore di ricerca. | Prendi in considerazione la rimozione del servizio Elasticsearch dal livello infrastruttura per ottimizzare l’utilizzo delle risorse. |
| 2011 | validate-config | La versione del servizio Elasticsearch a livello di infrastruttura non è compatibile con la versione corrente del modulo elasticsearch/elasticsearch, utilizzato dall’applicazione Adobe Commerce. |  |
| 2012 | validate-config | La configurazione corrente non è compatibile con questa versione di Adobe Commerce |  |
| 2013 | validate-config | Opzioni SCD ignorate perché il processo di distribuzione non è stato eseguito nella fase di compilazione |  |
| 2014 | validate-config | La configurazione contiene variabili o valori obsoleti |  |
| 2015 | validate-config | Configurazione ambiente non valida |  |
| 2016 | validate-config | Impossibile decodificare la configurazione del tipo JSON |  |
| 2017 | validate-config | La configurazione corrente non è compatibile con questa versione di Adobe Commerce |  |
| 2018 | validate-config | Alcuni servizi hanno superato la fine del ciclo di vita |  |
| 2019 | validate-config | L&#39;opzione di configurazione della ricerca MySQL è obsoleta | Utilizza invece Elasticsearch. |
| 2029 | validate-config | La suddivisione del database è stata dichiarata obsoleta in Adobe Commerce e nel Magento Open Source 2.4.2 e verrà rimossa nella versione 2.5. | Se si utilizza un database diviso, è necessario iniziare a pianificare il ripristino o la migrazione a un singolo database oppure utilizzare un approccio alternativo. |
| 2020 | install-update | L&#39;installazione di Adobe Commerce è stata completata, ma `app/etc/env.php` file di configurazione mancante o vuoto. | I dati richiesti verranno ripristinati dalle configurazioni dell’ambiente e dal file .magento.env.yaml. |
| 2021 | install-update:db-connection | Per i database suddivisi utilizzati connessioni personalizzate |  |
| 2022 | install-update:db-connection | La configurazione del database utilizzata non è compatibile con la connessione slave. |  |
| 2023 | install-update:split-db | L&#39;abilitazione di un database diviso verrà ignorata. |  |
| 2024 | install-update:split-db | Nella variabile SPLIT_DB manca la configurazione per i tipi di connessione suddivisi. |  |
| 2025 | install-update:split-db | Connessione slave non impostata. |  |
| 2026 | pre-distribuzione:restore-writable-dirs | Impossibile ripristinare alcuni dati generati durante la fase di build nelle directory montate | Controlla la `cloud.log` per ulteriori informazioni. |
| 2027 | validate-config:mage-mode-variable | Valore di modalità per la variabile di ambiente MAGE_MODE non supportato | Rimuovere la variabile di ambiente MAGE_MODE o modificarne il valore in &quot;production&quot;. Adobe Commerce su infrastruttura cloud supporta solo la modalità di &quot;produzione&quot;. |
| 2028 | storage remoto | Impossibile abilitare l&#39;archiviazione remota. | Verificare le credenziali dell&#39;archivio remoto. |
| 2030 | validate-config | I servizi Elasticsearch e OpenSearch sono entrambi installati a livello di infrastruttura. Adobe Commerce e Magento Open Source 2.4.4 e versioni successive utilizzano OpenSearch per impostazione predefinita | Prendi in considerazione la rimozione del servizio Elasticsearch o OpenSearch dal livello infrastruttura per ottimizzare l’utilizzo delle risorse. |

### Fase post-distribuzione

| Codice di errore | Passaggio post-distribuzione | Descrizione errore (titolo) | Azione suggerita |
| - | - | - | - |
| 3001 | validate-config | La registrazione di debug è abilitata in Adobe Commerce | Per risparmiare spazio su disco, non abilitare la registrazione di debug per gli ambienti di produzione. |
| 3002 | riscaldamento | Impossibile recuperare gli URL dell’archivio |  |
| 3003 | riscaldamento | Impossibile recuperare l’URL dell’archivio |  |
| 3004 | backup | Impossibile creare i file di backup |  |

### Generale

| Codice di errore | Passaggio generale | Descrizione errore (titolo) | Azione suggerita |
| - | - | - | - |
| 4001 |  | Impossibile ottenere il numero di processori di sistema: |  |
