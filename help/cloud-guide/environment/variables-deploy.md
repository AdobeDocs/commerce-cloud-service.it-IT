---
title: Distribuire le variabili
description: Consulta l’elenco delle variabili di ambiente che controllano le azioni nella fase di implementazione di Adobe Commerce su infrastruttura cloud.
feature: Cloud, Configuration, Cache, Deploy, SCD, Storage, Search
recommendations: noDisplay, catalog
role: Developer
exl-id: 673880b5-830b-4837-ac0c-5fa5643ae34c
source-git-commit: 7c09a0041f35890f34a00a8eea8f0154dc0f5dcd
workflow-type: tm+mt
source-wordcount: '2209'
ht-degree: 0%

---

# Distribuire le variabili

Le seguenti variabili _deploy_ controllano le azioni nella fase di distribuzione e possono ereditare ed eseguire l&#39;override dei valori dalle [variabili globali](variables-global.md). Inserisci queste variabili nella fase `deploy` del file `.magento.env.yaml`:

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

Configura la pagina Redis e il caching predefinito. Quando si imposta il parametro `cm_cache_backend_redis`, è necessario specificare le opzioni `server`, `port` e `database`.

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

Nell&#39;esempio seguente viene utilizzata la [funzionalità di precaricamento Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/redis-pg-cache.html#redis-preload-feature) definita nella _Guida alla configurazione_:

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

Per utilizzare un modello [REDIS_BACKEND](#redis_backend) personalizzato (non solo dall&#39;elenco Consentiti), impostare l&#39;opzione `_custom_redis_backend` su `true` per abilitare la convalida corretta come nell&#39;esempio seguente:

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

Abilita o disabilita la pulizia di [file di contenuto statico](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html) generati durante la fase di compilazione o distribuzione. Utilizza il valore predefinito _true_ nello sviluppo come best practice.

- **`true`** - Rimuove tutto il contenuto statico esistente prima di distribuire il contenuto statico aggiornato.
- **`false`** - La distribuzione sovrascrive i file di contenuto statico esistenti solo se il contenuto generato contiene una versione più recente.

Se modifichi il contenuto statico tramite un processo separato, imposta il valore su _false_.

```yaml
stage:
  deploy:
    CLEAN_STATIC_FILES: false
```

La mancata pulizia dei file di visualizzazione statica prima della distribuzione può causare problemi se si distribuiscono aggiornamenti ai file esistenti senza rimuovere le versioni precedenti. A causa di [regole di fallback del file statico](https://developer.adobe.com/commerce/frontend-core/guide/caching/#clean-static-files-cache), le operazioni di fallback possono visualizzare il file errato se la directory contiene più versioni dello stesso file.

## `CRON_CONSUMERS_RUNNER`

- **Predefinito**—`cron_run = false`, `max_messages = 1000`
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Utilizzare questa variabile di ambiente per verificare che le code di messaggi siano in esecuzione dopo una distribuzione.

- `cron_run` - Valore booleano che abilita o disabilita il processo cron `consumers_runner` (impostazione predefinita = `false`).
- `max_messages`—Numero che specifica il numero massimo di messaggi che ogni consumer deve elaborare prima della chiusura (impostazione predefinita = `1000`). È possibile impostare il valore su `0` per impedire al consumer di terminare.
- `consumers` - Matrice di stringhe che specifica quali consumer eseguire. Un array vuoto esegue _tutti_ i consumer.

- `multiple_processes`-Numero che specifica il numero di processi da generare per ogni consumatore. Supportato in Commerce **2.4.4** o versione successiva.

>[!NOTE]
>
>Per restituire un elenco della coda di messaggi `consumers`, eseguire il comando `./bin/magento queue:consumers:list` nell&#39;ambiente remoto.

Array di esempio che esegue `consumers` e `multiple_processes` specifici da generare per ogni consumatore:

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

Esempio di un array vuoto che esegue tutti `consumers`:

```yaml
stage:
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 1000
      consumers: []
```

Per impostazione predefinita, il processo di distribuzione sovrascrive tutte le impostazioni nel file `env.php`. Consulta [Gestione delle code di messaggi](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/manage-message-queues.html) nella _Guida alla configurazione di Commerce_ per Adobe Commerce locale.

## `CONSUMERS_WAIT_FOR_MAX_MESSAGES`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Configurare il modo in cui `consumers` elabora i messaggi dalla coda di messaggi scegliendo una delle opzioni seguenti:

- `false`—`Consumers` elabora i messaggi disponibili nella coda, chiude la connessione TCP e termina. `Consumers` non attendono messaggi aggiuntivi per entrare nella coda, anche se il numero di messaggi elaborati è inferiore al valore `max_messages` specificato nella variabile di distribuzione `CRON_CONSUMERS_RUNNER`.

- `true`—`Consumers` continua a elaborare i messaggi dalla coda dei messaggi fino a raggiungere il numero massimo di messaggi (`max_messages`) specificato nella variabile di distribuzione `CRON_CONSUMERS_RUNNER` prima di chiudere la connessione TCP e terminare il processo consumer. Se la coda si svuota prima di raggiungere `max_messages`, il consumatore attende l&#39;arrivo di altri messaggi.

>[!WARNING]
>
>Se si utilizzano i processi di lavoro per eseguire `consumers` invece di utilizzare un processo cron, impostare questa variabile su true.

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
>Impostare il valore `CRYPT_KEY` tramite [!DNL Cloud Console] anziché il file `.magento.env.yaml` per evitare di esporre la chiave nell&#39;archivio del codice sorgente per l&#39;ambiente. Consulta [Impostare le variabili di ambiente e di progetto](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/project/overview.html#configure-environment).

Quando si sposta il database da un ambiente a un altro senza un processo di installazione, è necessario disporre delle informazioni di crittografia corrispondenti. Adobe Commerce utilizza il valore della chiave di crittografia impostato in [!DNL Cloud Console] come valore `crypt/key` nel file `env.php`.

## `DATABASE_CONFIGURATION`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Se hai definito un database nella proprietà [relations](../application/properties.md#relationships) del file `.magento.app.yaml`, puoi personalizzare le connessioni al database per la distribuzione.

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

Nell&#39;esempio seguente viene utilizzato il prefisso della tabella `ece_` con le impostazioni di connessione predefinite anziché l&#39;opzione `_merge`:

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

```
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

Mantiene le impostazioni personalizzate del servizio [!DNL Elastic Suite] tra le distribuzioni e le utilizza nella sezione &#39;system/default/smile_elasticsuite_core_base_settings&#39; della configurazione principale di [!DNL Elastic Suite]. Se il pacchetto del compositore [!DNL Elastic Suite] è installato, viene configurato automaticamente.

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

>[!NOTE]
>
>In un cluster Pro Staging/Produzione con tre nodi (o tre nodi di servizio in [Architettura scalata](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/architecture/scaled-architecture#service-tier), `indices_settings` deve essere impostato come segue:
>
>```yaml
>           indices_settings:
>               number_of_shards: 3
>               number_of_replicas: 2
>```

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

- La modifica del motore di ricerca in un tipo diverso da `elasticsuite` causa un errore di distribuzione accompagnato da un errore di convalida appropriato
- La rimozione del servizio Elasticsearch causa un errore di distribuzione accompagnato da un errore di convalida appropriato

>[!NOTE]
>
>Per informazioni dettagliate sull&#39;utilizzo o sulla risoluzione dei problemi relativi al plug-in [!DNL Elastic Suite] con Adobe Commerce, consulta la [[!DNL Elastic Suite] documentazione](https://github.com/Smile-SA/elasticsuite).

## `ENABLE_GOOGLE_ANALYTICS`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Abilita e disabilita le Google Analytics durante la distribuzione in ambienti di staging e integrazione. Per impostazione predefinita, Google Analytics è true solo per l’ambiente di produzione. Imposta questo valore su `true` per abilitare le Google Analytics negli ambienti di staging e integrazione.

- **`true`** - Abilita la Google Analytics negli ambienti di staging e integrazione.
- **`false`**: disabilita le Google Analytics negli ambienti di staging e integrazione.

Aggiungere la variabile di ambiente `ENABLE_GOOGLE_ANALYTICS` alla fase `deploy` nel file `.magento.env.yaml`:

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

Durante la distribuzione negli ambienti Pro o Starter di staging e produzione, questa variabile sostituisce gli URL di base di Adobe Commerce nel database con gli URL del progetto specificati dalla variabile [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Utilizzare questa impostazione per ignorare il comportamento predefinito della variabile di distribuzione [UPDATE_URLS](#update_urls), che viene ignorata durante la distribuzione in ambienti di staging o produzione.

```yaml
stage:
  deploy:
    FORCE_UPDATE_URLS: true
```

## `LOCK_PROVIDER`

- **Predefinito**—`file`
- **Versione**—Adobe Commerce 2.2.5 e versioni successive

Il provider di blocchi impedisce l&#39;avvio di processi cron e gruppi cron duplicati. Utilizza il provider di blocchi `file` nell&#39;ambiente di produzione. Gli ambienti Starter e l&#39;ambiente di integrazione Pro non utilizzano la variabile [MAGENTO_CLOUD_LOCKS_DIR](variables-cloud.md), pertanto `ece-tools` applica automaticamente il provider di blocchi `db`.

```yaml
stage:
  deploy:
    LOCK_PROVIDER: "db"
```

Vedere [Configurare il blocco](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/lock-provider.html) nella _Guida all&#39;installazione_.

## `MYSQL_USE_SLAVE_CONNECTION`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

>[!TIP]
>
>La variabile `MYSQL_USE_SLAVE_CONNECTION` è supportata solo in Adobe Commerce negli ambienti cluster di Staging e Production Pro dell&#39;infrastruttura cloud e non nei progetti Starter.

Adobe Commerce può leggere più database in modo asincrono. Impostare su `true` per utilizzare automaticamente una connessione _di sola lettura_ al database per ricevere traffico di sola lettura su un nodo non principale. Questa connessione migliora le prestazioni tramite il bilanciamento del carico, perché solo un nodo gestisce il traffico di lettura-scrittura. Impostare su `false` per rimuovere qualsiasi array di connessione di sola lettura esistente dal file `env.php`.

```yaml
stage:
  deploy:
    MYSQL_USE_SLAVE_CONNECTION: true
```

Quando la variabile `MYSQL_USE_SLAVE_CONNECTION` è impostata su `true`, il parametro `synchronous_replication` è impostato su `true` per impostazione predefinita nel file `env.php` negli ambienti di staging e produzione Pro. Quando `MYSQL_USE_SLAVE_CONNECTION` è impostato su `false`, il parametro `synchronous_replication` non è configurato.

## `QUEUE_CONFIGURATION`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Utilizza questa variabile di ambiente per mantenere le impostazioni del servizio AMQP personalizzate tra le distribuzioni. Ad esempio, se preferisci utilizzare un servizio di coda messaggi esistente invece di affidarti all&#39;infrastruttura cloud per crearlo, utilizza la variabile di ambiente `QUEUE_CONFIGURATION` per connetterlo al sito:

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

Esempio di impostazione di `REDIS_BACKEND`

```yaml
stage:
  deploy:
    REDIS_BACKEND: '\Magento\Framework\Cache\Backend\RemoteSynchronizedCache'
```

>[!NOTE]
>
>Se si specifica `\Magento\Framework\Cache\Backend\RemoteSynchronizedCache` come modello di back-end Redis per abilitare [la cache L2](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html), `ece-tools` genera automaticamente la configurazione della cache. Vedere un esempio di [file di configurazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/level-two-cache.html#configuration-example) nella _Guida alla configurazione di Adobe Commerce_. Per ignorare la configurazione della cache generata, utilizzare la variabile di distribuzione [CACHE_CONFIGURATION](#cache_configuration).

## `REDIS_USE_SLAVE_CONNECTION`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.1.16 e versioni successive

>[!WARNING]
>
>_non_ abilitare questa variabile in un progetto [architettura ridimensionata](../architecture/scaled-architecture.md). Questo causa errori di connessione Redis. Gli schiavi Redis sono ancora attivi ma non sono usati per le letture Redis. In alternativa, Adobe consiglia di utilizzare Adobe Commerce 2.3.5 o versioni successive, implementare una nuova configurazione di back-end Redis e implementare il caching L2 per Redis.

>[!TIP]
>
>La variabile `REDIS_USE_SLAVE_CONNECTION` è supportata solo in Adobe Commerce negli ambienti cluster di Staging e Production Pro dell&#39;infrastruttura cloud e non nei progetti Starter.

Adobe Commerce può leggere più istanze Redis in modo asincrono. Imposta su `true` per utilizzare automaticamente una connessione _di sola lettura_ a un&#39;istanza Redis per ricevere traffico di sola lettura su un nodo non principale. Questa connessione migliora le prestazioni tramite il bilanciamento del carico, perché solo un nodo gestisce il traffico di lettura-scrittura. Impostare su `false` per rimuovere qualsiasi array di connessione di sola lettura esistente dal file `env.php`.

```yaml
stage:
  deploy:
    REDIS_USE_SLAVE_CONNECTION: true
```

È necessario configurare un servizio Redis nel file `.magento.app.yaml` e nel file `services.yaml`.

[ECE-Tools versione 2002.0.18](../release-notes/cloud-release-archive.md#v2002018) e successive utilizza impostazioni più a tolleranza di errore. Se Adobe Commerce non riesce a leggere i dati dall&#39;istanza Redis _slave_, legge i dati dall&#39;istanza Redis _master_.

La connessione di sola lettura non è disponibile per l&#39;utilizzo nell&#39;ambiente di integrazione o se si utilizza la variabile [`CACHE_CONFIGURATION`](#cache_configuration).

## `RESOURCE_CONFIGURATION`

- **Predefinito**—Non impostato
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Associa un nome di risorsa a una connessione al database. Questa configurazione corrisponde alla sezione `resource` del file `env.php`.

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

Specifica il livello di compressione [gzip](https://www.gnu.org/software/gzip) (da `0` a `9`) da utilizzare durante la compressione del contenuto statico; `0` disabilita la compressione.

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

È possibile configurare più impostazioni internazionali per tema. Questa personalizzazione velocizza il processo di distribuzione riducendo il numero di file dei temi non necessari. Ad esempio, puoi distribuire il tema _magento/backend_ in inglese e un tema personalizzato in altre lingue.

L&#39;esempio seguente distribuisce il tema `Magento/backend` con tre impostazioni internazionali:

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

Nella fase di distribuzione, impostare `SCD_NO_PARENT: true` in modo che la generazione di contenuto statico per i temi principali non venga eseguita durante la fase di distribuzione. Questa impostazione consente di ridurre al minimo i tempi di distribuzione e di evitare i tempi di inattività del sito che possono verificarsi se la generazione di contenuto statico non riesce durante la distribuzione. Vedi [Distribuzione di contenuto statico](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SCD_NO_PARENT: true
```

## `SCD_STRATEGY`

- **Predefinito**—`quick`
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Consente di personalizzare la [strategia di distribuzione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) per il contenuto statico. Vedere [Distribuire i file di visualizzazione statici](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

Utilizza queste opzioni _only_ se hai più di una lingua:

- `standard`: distribuisce tutti i file di visualizzazione statica per tutti i pacchetti.
- `quick`—(_default_) riduce al minimo i tempi di distribuzione.
- `compact`: consente di risparmiare spazio su disco nel server. In Adobe Commerce versione 2.2.4 e precedenti, questa impostazione sostituisce il valore per `scd_threads` con il valore di `1`.

```yaml
stage:
  deploy:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Predefinito**—Automatico
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Imposta il numero di thread per la distribuzione del contenuto statico. Il valore predefinito è impostato in base al numero di thread della CPU rilevati e non supera il valore 4. L&#39;aumento del numero di thread velocizza la distribuzione dei contenuti statici; la riduzione del numero di thread ne rallenta la distribuzione. Puoi impostare il valore del thread, ad esempio:

```yaml
stage:
  deploy:
    SCD_THREADS: 2
```

Per ridurre ulteriormente i tempi di distribuzione, utilizzare [Gestione configurazione](../store/store-settings.md) con il comando `scd-dump` per spostare la distribuzione statica nella fase di compilazione.

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

Configurare l’archiviazione della sessione Redis. Richiede le opzioni `save`, `redis`, `host`, `port` e `database` per la variabile di archiviazione della sessione. Ad esempio:

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

Imposta su `true` per saltare la distribuzione del contenuto statico durante la fase di distribuzione.

Nella fase di distribuzione, impostare `SKIP_SCD: true` in modo che la compilazione del contenuto statico non avvenga durante la fase di distribuzione. Questa impostazione consente di ridurre al minimo i tempi di distribuzione e di evitare i tempi di inattività del sito che possono verificarsi se la generazione di contenuto statico non riesce durante la distribuzione. Vedi [Distribuzione di contenuto statico](../deploy/static-content.md).

```yaml
stage:
  deploy:
    SKIP_SCD: true
```

## `UPDATE_URLS`

- **Predefinito**—`true`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Durante la distribuzione, sostituire gli URL di base di Adobe Commerce nel database con gli URL del progetto specificati dalla variabile [`MAGENTO_CLOUD_ROUTES`](variables-cloud.md). Questa configurazione è utile per lo sviluppo locale, dove gli URL di base sono impostati per l’ambiente locale. Quando distribuisci in un ambiente Cloud, gli URL vengono aggiornati in modo da poter accedere alla vetrina e all’amministratore tramite gli URL del progetto.

Se è necessario aggiornare gli URL durante la distribuzione in ambienti di staging e produzione Pro o Starter, utilizzare la variabile [`FORCE_UPDATE_URLS`](#force_update_urls).

```yaml
stage:
  deploy:
    UPDATE_URLS: false
```

## `VERBOSE_COMMANDS`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Attiva o disattiva il livello di dettaglio di debug [Symfony](https://symfony.com/doc/current/console/verbosity.html) per i comandi CLI `bin/magento` eseguiti durante la fase di distribuzione.

>[!NOTE]
>
>Per utilizzare l&#39;impostazione VERBOSE_COMMANDS per controllare i dettagli nell&#39;output del comando sia per i comandi CLI `bin/magento` riusciti che non riusciti, è necessario impostare [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Scegli il livello di dettaglio fornito nei registri:

- `-v`= output normale
- `-vv`= output più dettagliato
- `-vvv` = output dettagliato ideale per il debug

```yaml
stage:
  deploy:
    VERBOSE_COMMANDS: "-vv"
```
