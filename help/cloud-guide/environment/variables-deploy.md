---
title: Distribuire le variabili
description: Consulta l’elenco delle variabili di ambiente che controllano le azioni nella fase di implementazione di Adobe Commerce su infrastruttura cloud.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 673880b5-830b-4837-ac0c-5fa5643ae34c
source-git-commit: b7307faf046884c13cba852df69d4fa9977e9a17
workflow-type: tm+mt
source-wordcount: '2185'
ht-degree: 0%

---

# Distribuire le variabili

I seguenti elementi _distribuire_ Le variabili controllano le azioni nella fase di distribuzione e possono ereditare e sovrascrivere i valori da [Variabili globali](variables-global.md). Inserisci queste variabili in `deploy` fase del `.magento.env.yaml` file:

```yaml
stage:
  deploy:
    DEPLOY_VARIABLE_NAME: value
```

Per ulteriori informazioni sulla personalizzazione del processo di compilazione e distribuzione:

- [Configurazione della distribuzione](configure-env-yaml.md)
- [Processo di distribuzione](../deploy/process.md)

## `CACHE_CONFIGURATION`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Configura la pagina Redis e il caching predefinito. Quando si imposta `cm_cache_backend_redis` , è necessario specificare il `server`, `port`, e `database` opzioni.

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          backend: file
        page_cache:
          backend: file
```

{{merge-options}}

Nell&#39;esempio seguente i nuovi valori vengono uniti a una configurazione esistente:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          backend_options:
            database: 10
        page_cache:
          backend_options:
            database: 11
```

Nell&#39;esempio seguente viene utilizzato [Funzione di precaricamento Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html#redis-preload-feature) come definito nella _Guida alla configurazione_:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      _merge: true
      frontend:
        default:
          id_prefix: '061_'
          backend_options:
            preload_keys:
              - '061_EAV_ENTITY_TYPES:hash'
              - '061_GLOBAL_PLUGIN_LIST:hash'
              - '061_DB_IS_UP_TO_DATE:hash'
              - '061_SYSTEM_DEFAULT:hash'
```

Per utilizzare un [REDIS_BACKEND](#redis_backend) (non solo dall&#39;elenco Consentiti), imposta il `_custom_redis_backend` opzione per `true` per attivare la convalida corretta, come nell&#39;esempio seguente:

```yaml
stage:
  deploy:
    CACHE_CONFIGURATION:
      frontend:
        default:
          _custom_redis_backend: true
          backend: '\CustomRedisModel'
```

## `CLEAN_STATIC_FILES`

- **Predefinito**—`true`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Attiva o disattiva la pulizia [file di contenuto statico](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html) generate durante la fase di build o distribuzione. Usa il valore predefinito _true_ in via di sviluppo come best practice.

- **`true`**- Rimuove tutto il contenuto statico esistente prima di distribuire il contenuto statico aggiornato.
- **`false`**- La distribuzione sovrascrive i file di contenuto statico esistenti solo se il contenuto generato contiene una versione più recente.

Se si modifica il contenuto statico tramite un processo separato, impostare il valore su _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

La mancata pulizia dei file di visualizzazione statica prima della distribuzione può causare problemi se si distribuiscono aggiornamenti ai file esistenti senza rimuovere le versioni precedenti. A causa di [fallback di file statici](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache) regole, le operazioni di fallback possono visualizzare il file errato se la directory contiene più versioni dello stesso file.

## `CRON_CONSUMERS_RUNNER`

- **Predefinito**—`cron_run = false`, `max_messages = 1000`
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Utilizzare questa variabile di ambiente per verificare che le code di messaggi siano in esecuzione dopo una distribuzione.

- `cron_run`- Valore booleano che attiva o disattiva la `consumers_runner` processo cron (predefinito = `false`).
- `max_messages`- Numero che specifica il numero massimo di messaggi che ogni consumatore deve elaborare prima di terminare (valore predefinito = `1000`). Puoi impostare il valore su `0` per impedire al consumatore di terminare.
- `consumers`- Matrice di stringhe che specifica i consumer da eseguire. Viene eseguito un array vuoto _tutto_ consumatori.

- `multiple_processes`-Numero che specifica il numero di processi da generare per ciascun consumatore. Supportato in Commerce **2.4.4.** o superiore.

>[!NOTE]
>
>Per restituire un elenco di code di messaggi `consumers`, esegui `./bin/magento queue:consumers:list` nell&#39;ambiente remoto.

Esempio di array che esegue `consumers` e `multiple_processes` per la riproduzione per ciascun consumatore:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers:
        - example_consumer_1
        - example_consumer_2
-     multiple_processes:
        example_consumer_1: 4
        example_consumer_2: 3
```

Esempio di array vuoto che esegue tutti `consumers`:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

Per impostazione predefinita, il processo di distribuzione sovrascrive tutte le impostazioni in `env.php` file. Consulta [Gestire le code dei messaggi](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html) nel _Guida alla configurazione di Commerce_ per Adobe Commerce on-premise.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Configurare come `consumers` elabora i messaggi dalla coda di messaggi scegliendo una delle opzioni seguenti:

- `false`—`Consumers` elabora i messaggi disponibili nella coda, chiude la connessione TCP e termina. `Consumers` non attendere che altri messaggi entrino nella coda, anche se il numero di messaggi elaborati è inferiore a `max_messages` valore specificato in `CRON_CONSUMERS_RUNNER` distribuire la variabile.

- `true`—`Consumers` continua a elaborare i messaggi dalla coda di messaggi fino a raggiungere il numero massimo di messaggi (`max_messages`) specificato in `CRON_CONSUMERS_RUNNER` distribuire la variabile prima di chiudere la connessione TCP e terminare il processo consumer. Se la coda si svuota prima di raggiungere `max_messages`, il consumatore attende che arrivino più messaggi.

>[!WARNING]
>
>Se si utilizzano i processi di lavoro per eseguire `consumers` invece di utilizzare un processo cron, imposta questa variabile su true.

```yaml
stage:
  deploy:
    CONSUMERS_WAIT_FOR_MAX_MESSAGES: false
```

## `CRYPT_KEY`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

>[!WARNING]
>
>Imposta il `CRYPT_KEY` valore attraverso [!DNL Cloud Console] invece del `.magento.env.yaml` per evitare di esporre la chiave nell’archivio del codice sorgente per il tuo ambiente. Consulta [Impostare le variabili di ambiente e di progetto](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html#configure-environment).

Quando si sposta il database da un ambiente a un altro senza un processo di installazione, è necessario disporre delle informazioni di crittografia corrispondenti. Adobe Commerce utilizza il valore della chiave di crittografia impostato in [!DNL Cloud Console] come `crypt/key` valore in `env.php` file.

## `DATABASE_CONFIGURATION`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Se è stato definito un database in [proprietà relations](../application/properties.md#relationships) del `.magento.app.yaml` è possibile personalizzare le connessioni al database per la distribuzione.

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_value'
```

{{merge-options}}

Nell&#39;esempio seguente i nuovi valori vengono uniti a una configurazione esistente:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      some_config: 'some_new_value'
      _merge: true
```

È inoltre possibile configurare un prefisso di tabella.

>[!WARNING]
>
>Se non utilizzi l’opzione di unione con il prefisso della tabella, devi fornire le impostazioni di connessione predefinite, altrimenti la distribuzione non riesce la convalida.

Nell&#39;esempio seguente viene utilizzato `ece_` prefisso della tabella con impostazioni di connessione predefinite anziché utilizzare `_merge` opzione:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          username: user
          host: host
          dbname: magento
          password: password
      table_prefix: 'ece_'
```

Output di esempio:

```terminal
MariaDB [main]> SHOW TABLES;
+-------------------------------------+
| Tables_in_main                      |
+-------------------------------------+
| ece_admin_passwords                 |
| ece_admin_system_messages           |
| ece_admin_user                      |
| ece_admin_user_session              |
| ece_adminnotification_inbox         |
| ece_amazon_customer                 |
| ece_authorization_rule              |
| ece_cache                           |
| ece_cache_tag                       |
| ece_captcha_log                     |
...
```

## `ELASTICSUITE_CONFIGURATION`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Mantiene personalizzati [!DNL Elastic Suite] impostazioni del servizio tra le distribuzioni e lo utilizza nella sezione &#39;system/default/smile_elasticsuite_core_base_settings&#39; della sezione principale [!DNL Elastic Suite] configurazione. Se il [!DNL Elastic Suite] viene installato, viene configurato automaticamente.

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      es_client:
        servers: 'remote-host:9200'
      indices_settings:
        number_of_shards: 1
        number_of_replicas: 0
```

{{merge-options}}

L’esempio che segue unisce un nuovo valore alla configurazione esistente:

```yaml
stage:
  deploy:
    ELASTICSUITE_CONFIGURATION:
      indices_settings:
        number_of_shards: 3
        number_of_replicas: 2
      _merge: true
```

**Limitazioni note**:

- Modifica del motore di ricerca in un tipo diverso da `elasticsuite` causa un errore di distribuzione accompagnato da un errore di convalida appropriato
- La rimozione del servizio Elasticsearch causa un errore di distribuzione accompagnato da un errore di convalida appropriato

>[!NOTE]
>
>Per informazioni dettagliate sull&#39;utilizzo o sulla risoluzione dei problemi di [!DNL Elastic Suite] con Adobe Commerce, consulta la sezione [[!DNL Elastic Suite] documentazione](https://github.com/Smile-SA/elasticsuite).

## `ENABLE_GOOGLE_ANALYTICS`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Abilita e disabilita le Google Analytics durante la distribuzione in ambienti di staging e integrazione. Per impostazione predefinita, Google Analytics è true solo per l’ambiente di produzione. Imposta questo valore su `true` per abilitare le Google Analytics negli ambienti di staging e integrazione.

- **`true`**- Abilita la Google Analytics negli ambienti di staging e integrazione.
- **`false`**- Disattiva le Google Analytics negli ambienti di staging e integrazione.

Aggiungi il `ENABLE_GOOGLE_ANALYTICS` variabile di ambiente al `deploy` fase nel `.magento.env.yaml` file:

```yaml
stage:
  deploy:
    ENABLE_GOOGLE_ANALYTICS: true
```

>[!NOTE]
>
>Il processo di distribuzione abilita sempre le Google Analytics negli ambienti di produzione.

## `FORCE_UPDATE_URLS`

- **Predefinito**—`true`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Al momento della distribuzione negli ambienti Pro o Starter di staging e produzione, questa variabile sostituisce gli URL di base di Adobe Commerce nel database con gli URL del progetto specificati da [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) variabile. Utilizzare questa impostazione per ignorare il comportamento predefinito del [UPDATE_URLS](#update_urls) variabile di distribuzione, che viene ignorata durante la distribuzione in ambienti di staging o produzione.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Predefinito**—`file`
- **Versione**—Adobe Commerce 2.2.5 e versioni successive

Il provider di blocchi impedisce l&#39;avvio di processi cron e gruppi cron duplicati. Utilizza il `file` blocca il provider nell’ambiente di produzione. Gli ambienti Starter e l&#39;ambiente di integrazione Pro non utilizzano [MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md) variabile, quindi `ece-tools` applica il `db` blocca automaticamente il provider.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

Consulta [Configurare il blocco](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html) nel _Guida all’installazione_.

## `MYSQL_USE_SLAVE_CONNECTION`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

>[!TIP]
>
>Il `MYSQL_USE_SLAVE_CONNECTION` La variabile è supportata solo in ambienti cluster Adobe Commerce on cloud infrastructure Staging and Production Pro e non in progetti Starter.

Adobe Commerce può leggere più database in modo asincrono. Imposta su `true` per utilizzare automaticamente un _sola lettura_ connessione al database per ricevere traffico di sola lettura su un nodo non principale. Questa connessione migliora le prestazioni tramite il bilanciamento del carico, perché solo un nodo gestisce il traffico di lettura-scrittura. Imposta su `false` per rimuovere qualsiasi array di connessione di sola lettura esistente dal `env.php` file.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Quando `MYSQL_USE_SLAVE_CONNECTION` variabile impostata su `true`, il `synchronous_replication` il parametro è impostato su `true` per impostazione predefinita, in `env.php` negli ambienti di staging e produzione Pro. Quando `MYSQL_USE_SLAVE_CONNECTION` è impostato su `false`, il `synchronous_replication` parametro non configurato.

## `QUEUE_CONFIGURATION`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Utilizza questa variabile di ambiente per mantenere le impostazioni del servizio AMQP personalizzate tra le distribuzioni. Ad esempio, se preferisci utilizzare un servizio di coda messaggi esistente invece di affidarti all’infrastruttura cloud per crearlo, utilizza `QUEUE_CONFIGURATION` variabile di ambiente per collegarla al sito:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      amqp:
        host: test.host
        port: 1234
      amqp2:
        host: test.host2
        port: 12345
      mq:
        host: mq.host
        port: 1234
```

{{merge-options}}

Nell&#39;esempio seguente i nuovi valori vengono uniti a una configurazione esistente:

```yaml
stage:
  deploy:
    QUEUE_CONFIGURATION:
      _merge: true
      amqp:
        host: changed1.host
        port: 5672
      amqp2:
        host: changed2.host2
        port: 12345
      mq:
        host: changedmq.host
        port: 1234
```

## `REDIS_BACKEND`

- **Predefinito**—`Cm_Cache_Backend_Redis`
- **Versione**—Adobe Commerce 2.3.0 e versioni successive

Specifica la configurazione del modello back-end per la cache Redis.

Adobe Commerce versione 2.3.0 e successive include i seguenti modelli di back-end:

- `Cm_Cache_Backend_Redis`
- `\Magento\Framework\Cache\Backend\Redis`
- `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache`

L’esempio come impostare `REDIS_BACKEND`

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Se si specifica `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` come modello di back-end Redis per abilitare [Cache L2](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html), `ece-tools` genera automaticamente la configurazione della cache. Vedi un esempio [file di configurazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example) nel _Guida alla configurazione di Adobe Commerce_. Per ignorare la configurazione della cache generata, utilizza [CACHE_CONFIGURATION](#cache_configuration) distribuire la variabile.

## `REDIS_USE_SLAVE_CONNECTION`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.1.16 e versioni successive

>[!WARNING]
>
>Esegui _non_ abilita questa variabile in un [architettura scalata](../architecture/scaled-architecture.md) progetto. Questo causa errori di connessione Redis. Gli schiavi Redis sono ancora attivi ma non sono usati per le letture Redis. In alternativa, Adobe consiglia di utilizzare Adobe Commerce 2.3.5 o versioni successive, implementare una nuova configurazione di back-end Redis e implementare il caching L2 per Redis.

>[!TIP]
>
>Il `REDIS_USE_SLAVE_CONNECTION` La variabile è supportata solo in ambienti cluster Adobe Commerce on cloud infrastructure Staging and Production Pro e non in progetti Starter.

Adobe Commerce può leggere più istanze Redis in modo asincrono. Imposta su `true` per utilizzare automaticamente un _sola lettura_ connessione a un’istanza Redis per ricevere traffico di sola lettura su un nodo non principale. Questa connessione migliora le prestazioni tramite il bilanciamento del carico, perché solo un nodo gestisce il traffico di lettura-scrittura. Imposta su `false` per rimuovere qualsiasi array di connessione di sola lettura esistente dal `env.php` file.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

È necessario configurare un servizio Redis in `.magento.app.yaml` e nel `services.yaml` file.

[ECE-Tools versione 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) e in seguito utilizza impostazioni più fault-tolerant. Se Adobe Commerce non è in grado di leggere i dati dal Redis _schiavo_ , quindi legge i dati dal Redis _principale_ dell&#39;istanza.

La connessione di sola lettura non è disponibile per l’utilizzo nell’ambiente di integrazione o se utilizzi [`CACHE_CONFIGURATION` variabile](#cache_configuration).

## `RESOURCE_CONFIGURATION`

- **Predefinito**- Non impostato
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Associa un nome di risorsa a una connessione al database. Questa configurazione corrisponde a `resource` sezione del `env.php` file.

{{merge-options}}

Nell&#39;esempio seguente i nuovi valori vengono uniti a una configurazione esistente:

```yaml
stage:
  deploy:
    RESOURCE_CONFIGURATION:
      _merge: true
      default_setup:
        connection: default
```

## `SCD_COMPRESSION_LEVEL`

- **Predefinito**—`4`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Specifica quale [gzip](https://www.gnu.org/software/gzip) livello di compressione (`0` a `9`) da utilizzare per la compressione di contenuto statico; `0` disabilita la compressione.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_LEVEL: 5
```

## `SCD_COMPRESSION_TIMEOUT`

- **Predefinito**—`600`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Quando il tempo necessario per comprimere le risorse statiche supera il limite di timeout di compressione, il processo di distribuzione viene interrotto. Imposta il tempo massimo di esecuzione, in secondi, per il comando di compressione del contenuto statico.

```yaml
stage:
  deploy:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_MATRIX`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

È possibile configurare più impostazioni internazionali per tema. Questa personalizzazione velocizza il processo di distribuzione riducendo il numero di file dei temi non necessari. Ad esempio, puoi distribuire _magento/backend_ tema in inglese e un tema personalizzato in altre lingue.

Nell&#39;esempio seguente viene distribuito `Magento/backend` tema con tre lingue:

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Inoltre, puoi scegliere di _non_ distribuire un tema:

```yaml
stage:
  deploy:
    SCD_MATRIX:
      "magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Consente di aumentare il tempo massimo di esecuzione previsto per la distribuzione del contenuto statico.

Per impostazione predefinita, Adobe Commerce imposta l’esecuzione massima prevista su 900 secondi, ma in alcuni scenari potrebbe essere necessario più tempo per completare la distribuzione di contenuto statico per un progetto Cloud.

```yaml
stage:
  deploy:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.4.2 e versioni successive

Nella fase di distribuzione, imposta `SCD_NO_PARENT: true` in modo che la generazione di contenuto statico per i temi principali non avvenga durante la fase di distribuzione. Questa impostazione consente di ridurre al minimo i tempi di distribuzione e di evitare i tempi di inattività del sito che possono verificarsi se la generazione di contenuto statico non riesce durante la distribuzione. Consulta [Distribuzione di contenuti statici](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Predefinito**—`quick`
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Consente di personalizzare [strategia di implementazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) per il contenuto statico. Consulta [Distribuire file di visualizzazione statica](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

Usa queste opzioni _solo_ se si dispone di più impostazioni locali:

- `standard`- distribuisce tutti i file di visualizzazione statica per tutti i pacchetti.
- `quick`—(_predefinito_) riduce al minimo i tempi di installazione.
- `compact`- consente di risparmiare spazio su disco sul server. In Adobe Commerce versione 2.2.4 e precedenti, questa impostazione sostituisce il valore per `scd_threads` con un valore di `1`.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Predefinito**- Automatico
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Imposta il numero di thread per la distribuzione del contenuto statico. Il valore predefinito è impostato in base al numero di thread della CPU rilevati e non supera il valore 4. L&#39;aumento del numero di thread velocizza la distribuzione dei contenuti statici; la riduzione del numero di thread ne rallenta la distribuzione. Puoi impostare il valore del thread, ad esempio:

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Per ridurre ulteriormente i tempi di installazione, utilizza [Gestione configurazione](../store/store-settings.md) con `scd-dump` per spostare la distribuzione statica nella fase di build.

## `SEARCH_CONFIGURATION`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Utilizza questa variabile di ambiente per mantenere le impostazioni del servizio di ricerca personalizzate tra le distribuzioni. Ad esempio:

Elasticsearch di configurazione:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_hostname: http://elasticsearch.internal
      elasticsearch_server_port: '9200'
      elasticsearch_index_prefix: magento2
      elasticsearch_server_timeout: '15'
```

Configurazione di OpenSearch (per Commerce 2.4.6 e versioni successive):

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: opensearch
      opensearch_server_hostname: 'http://opensearch.internal'
      opensearch_server_port: '9200'
      opensearch_index_prefix: 'magento2'
      opensearch_server_timeout: '15'
```

{{merge-options}}

L’esempio che segue unisce un nuovo valore alla configurazione esistente:

```yaml
stage:
  deploy:
    SEARCH_CONFIGURATION:
      engine: elasticsearch
      elasticsearch_server_port: '9200'
      _merge: true
```

## `SESSION_CONFIGURATION`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Configurare l’archiviazione della sessione Redis. Richiede `save`, `redis`, `host`, `port`, e `database` opzioni per la variabile di archiviazione della sessione. Ad esempio:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      redis:
        bot_first_lifetime: 100
        bot_lifetime: 10001
        database: 0
        disable_locking: 1
        host: redis.internal
        max_concurrency: 10
        max_lifetime: 10001
        min_lifetime: 100
        port: 6379
      save: redis
```

{{merge-options}}

L’esempio che segue unisce un nuovo valore alla configurazione esistente:

```yaml
stage:
  deploy:
    SESSION_CONFIGURATION:
      _merge: true
      redis:
        max_concurrency: 10
```

## `SKIP_SCD`

- **Predefinito**— _Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Imposta su `true` per saltare la distribuzione di contenuti statici durante la fase di distribuzione.

Nella fase di distribuzione, imposta `SKIP_SCD: true` in modo che la generazione di contenuto statico non avvenga durante la fase di distribuzione. Questa impostazione consente di ridurre al minimo i tempi di distribuzione e di evitare i tempi di inattività del sito che possono verificarsi se la generazione di contenuto statico non riesce durante la distribuzione. Consulta [Distribuzione di contenuti statici](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Predefinito**—`true`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Durante la distribuzione, sostituisci gli URL di base di Adobe Commerce nel database con gli URL del progetto specificati da [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md) variabile. Questa configurazione è utile per lo sviluppo locale, dove gli URL di base sono impostati per l’ambiente locale. Quando distribuisci in un ambiente Cloud, gli URL vengono aggiornati in modo da poter accedere alla vetrina e all’amministratore tramite gli URL del progetto.

Se è necessario aggiornare gli URL durante la distribuzione in ambienti di staging e produzione Pro o Starter, utilizzare [`FORCE_UPDATE_URLS`](#force_update_urls) variabile.

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Attiva o disattiva la [Symfony](https://symfony.com/doc/current/console/verbosity.html) livello di dettaglio debug per `bin/magento` Comandi CLI eseguiti durante la fase di distribuzione.

>[!NOTE]
>
>Per utilizzare l&#39;impostazione VERBOSE_COMMANDS per controllare i dettagli nell&#39;output dei comandi sia per operazioni riuscite che per operazioni non riuscite `bin/magento` CLI, è necessario impostare [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Scegli il livello di dettaglio fornito nei registri:

- `-v`= uscita normale
- `-vv`= output più dettagliato
- `-vvv` = output dettagliato ideale per il debug

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
