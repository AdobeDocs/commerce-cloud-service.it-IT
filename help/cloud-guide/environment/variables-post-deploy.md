---
title: Variabili post-distribuzione
description: Consulta l’elenco delle variabili di ambiente che controllano le azioni nella fase post-distribuzione di Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
exl-id: e460335f-cd2b-4c98-b1ff-32504599b33d
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Variabili post-distribuzione

Le seguenti _variabili post-distribuzione_ controllano le azioni nella fase post-distribuzione e possono ereditare e sostituire i valori dalle [variabili globali](variables-global.md). Inserisci queste variabili nella fase `post-deploy` del file `.magento.env.yaml`:

```yaml
stage:
  post-deploy:
    POST-DEPLOY_VARIABLE_NAME: value
```

Per ulteriori informazioni sulla personalizzazione del processo di compilazione e distribuzione:

- [Configurazione della distribuzione](configure-env-yaml.md)
- [Processo di distribuzione](../deploy/process.md)

## `TTFB_TESTED_PAGES`

- **Predefinito**— `[]` (un array vuoto)
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Configura _test Time To First Byte_ (TTFB) per le pagine specificate per verificare le prestazioni del sito. Specifica un riferimento di percorso assoluto, o URL con protocollo e host, per ogni pagina che richiede il test.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

Dopo aver specificato le pagine per il test e il commit delle modifiche, il test _Tempo al primo byte_ viene eseguito durante la fase di post-distribuzione e pubblica i risultati per ogni percorso nel registro cloud:

```
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

Per i percorsi reindirizzati, il registro riporta il percorso della destinazione di reindirizzamento anziché quello configurato nella variabile di ambiente. Se si specifica un percorso non valido, nel registro viene visualizzato un messaggio di avviso.

## `WARM_UP_CONCURRENCY`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Specifica il limite di richieste simultanee da inviare durante le operazioni di riscaldamento della cache per ridurre il carico del server. Questo valore limita il numero di connessioni parallele ed è utile per le configurazioni dell&#39;ambiente in cui la variabile post-distribuzione `WARM_UP_PAGES` specifica diverse pagine per il precaricamento della cache.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **Predefinito**— `index.php`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Personalizzare l&#39;elenco delle pagine utilizzate per precaricare la cache nella fase `post_deploy`. Devi configurare l’hook post-distribuzione. Vedere la [sezione degli hook](../application/hooks-property.md) del file `.magento.app.yaml`.

- **singole pagine**—Specificare una singola pagina da aggiungere alla cache. Non è necessario indicare l’URL di base predefinito. L&#39;esempio seguente memorizza nella cache la pagina `BASE_URL/index.php`:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **più domini**—Elenca più URL. L’esempio che segue memorizza in cache le pagine da due domini:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **più pagine** - Utilizza il seguente formato per memorizzare nella cache più pagine in base a uno specifico modello di espressione regolare:

  ```
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`: varianti possibili `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku`: utilizzare un pattern `regexp` o una corrispondenza esatta `url` per filtrare gli URL oppure un asterisco (\*) per tutte le pagine. Utilizza lo SKU del prodotto per il tipo di entità `product`
   - `store_id|store_code`: utilizzare l&#39;ID o il codice dell&#39;archivio o un asterisco (\*) per tutti gli archivi. È possibile trasmettere più ID archivio o codici separati da `|`

  L&#39;esempio seguente memorizza nella cache i tipi di entità `category` e `cms-page` in base a questi criteri:
   - tutte le pagine delle categorie per il punto vendita con ID `1`
   - tutte le pagine delle categorie per i negozi con codice `store1` e `store2`
   - pagina categoria `cars` per archivio con codice `store_en`
   - pagina cms `contact` per tutti gli store
   - pagina cms `contact` per gli archivi con ID `1` e `2`
   - qualsiasi pagina di categoria che contiene `car_` e termina con `html` per l&#39;archivio con ID 2
   - qualsiasi pagina categoria contenente `tires_` per l&#39;archivio con codice `store_gb`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "category:*:1"
           - "category:*:store1|store2"
           - "category:cars:store_en"
           - "cms-page:contact:*"
           - "cms-page:contact:1|2"
           - "category:|car_.*?\\.html$|:2"
           - "category:|tires_.*|:store_gb"
     ```

  L&#39;esempio seguente memorizza nella cache il tipo di entità `product` in base a questi criteri:
   - tutti i prodotti per tutti i punti vendita (limitato a 100 a livello di programmazione per evitare problemi di prestazioni)
   - tutti i prodotti per lo store `store1`
   - prodotti con `sku1` per tutti gli store
   - prodotti con `sku1` per archivi con codice `store1` e `store2`
   - prodotti con `sku1`, `sku2` e `sku3` per archivi con codice `store1` e `store2`

     ```yaml
     stage:
       post-deploy:
         WARM_UP_PAGES:
           - "product:*:*"
           - "product:*:store1"
           - "product:sku1:*"
           - "product:sku1:store1|store2"
           - "product:sku1|sku2|sku3:store1|store2"
     ```

  L&#39;esempio seguente memorizza nella cache il tipo di entità `store-page` in base a questi criteri:
   - pagina `/contact-us` per tutti gli store
   - pagina `/contact-us` per l&#39;archivio con ID `1`
   - pagina `/contact-us` per gli store con codice `code1` e `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
