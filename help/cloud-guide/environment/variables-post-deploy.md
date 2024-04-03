---
title: Variabili post-distribuzione
description: Consulta l’elenco delle variabili di ambiente che controllano le azioni nella fase post-distribuzione di Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Configuration, Cache
recommendations: noDisplay, catalog
role: Developer
exl-id: e460335f-cd2b-4c98-b1ff-32504599b33d
source-git-commit: 8b02757591c4e8f607e936de4eda74d76953d9b7
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 0%

---

# Variabili post-distribuzione

I seguenti elementi _post-distribuzione_ Le variabili controllano le azioni nella fase post-distribuzione e possono ereditare e sovrascrivere i valori da [Variabili globali](variables-global.md). Inserisci queste variabili in `post-deploy` fase del `.magento.env.yaml` file:

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

Configura _Tempo al primo byte_ (TTFB) test per pagine specifiche per verificare le prestazioni del sito. Specifica un riferimento di percorso assoluto, o URL con protocollo e host, per ogni pagina che richiede il test.

```yaml
stage:
  post-deploy:
    TTFB_TESTED_PAGES:
       - "index.php"
       - "index.php/customer/account/create"
       - "https://example.com/catalog/some-category"
```

Dopo aver specificato le pagine per il test e il commit delle modifiche, il _Tempo al primo byte_ il test viene eseguito durante la fase di post-distribuzione e pubblica i risultati per ogni percorso del registro cloud:

```terminal
[2019-06-20 20:42:22] INFO: TTFB test result: 0.313s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/customer/account/create","status":200}
[2019-06-20 20:42:22] INFO: TTFB test result: 0.408s {"url":"https://staging-tkyicst-xkmwgjkwmwfuk.us-4.magentosite.cloud/checkout/cart","status":200}
```

Per i percorsi reindirizzati, il registro riporta il percorso della destinazione di reindirizzamento anziché quello configurato nella variabile di ambiente. Se si specifica un percorso non valido, nel registro viene visualizzato un messaggio di avviso.

## `WARM_UP_CONCURRENCY`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Specifica il limite di richieste simultanee da inviare durante le operazioni di riscaldamento della cache per ridurre il carico del server. Questo valore limita il numero di connessioni parallele ed è utile per le configurazioni dell&#39;ambiente in cui `WARM_UP_PAGES` variabile post-distribuzione specifica diverse pagine per il precaricamento della cache.

```yaml
stage:
  post-deploy:
    WARM_UP_CONCURRENCY: 4
```

## `WARM_UP_PAGES`

- **Predefinito**— `index.php`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Personalizza l’elenco delle pagine utilizzate per precaricare la cache nel `post_deploy` fase. Devi configurare l’hook post-distribuzione. Consulta la [sezione hook](../application/hooks-property.md) del `.magento.app.yaml` file.

- **pagine singole**- Specifica una singola pagina da aggiungere alla cache. Non è necessario indicare l’URL di base predefinito. L’esempio che segue memorizza in cache la `BASE_URL/index.php` pagina:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - "index.php"
  ```

- **più domini**- Elenca più URL. L’esempio che segue memorizza in cache le pagine da due domini:

  ```yaml
  stage:
    post-deploy:
      WARM_UP_PAGES:
        - 'http://example1.com/test'
        - 'http://example2.com/test'
  ```

- **più pagine**- Utilizza il seguente formato per memorizzare in cache più pagine in base a uno specifico pattern di espressioni regolari:

  ```terminal
  <entity_type>:<pattern|url|product_sku>:<store_id|store_code>
  ```

   - `entity_type`: varianti possibili `category`, `cms-page`, `product`, `store-page`
   - `pattern|url|product_sku`: utilizza una `regexp` pattern o corrispondenza esatta `url` per filtrare gli URL o utilizza un asterisco (\*) per tutte le pagine. Utilizza lo SKU del prodotto per `product` tipo di entità
   - `store_id|store_code`: utilizza l’ID o il codice del negozio o un asterisco (\*) per tutti i negozi; puoi trasmettere diversi ID store o codici separati con `|`

  L’esempio seguente memorizza in cache per `category` e `cms-page` tipi di entità basati su questi criteri:
   - tutte le pagine categoria per store con ID `1`
   - tutte le pagine delle categorie per i negozi con codice `store1` e `store2`
   - pagina categoria `cars` per store con codice `store_en`
   - pagina cms `contact` per tutti i negozi
   - pagina cms `contact` per store con ID `1` e `2`
   - qualsiasi pagina di categoria contenente `car_` e termina con `html` per store con ID 2
   - qualsiasi pagina di categoria contenente `tires_` per store con codice `store_gb`

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

  L’esempio che segue memorizza in cache per `product` tipo di entità in base ai seguenti criteri:
   - tutti i prodotti per tutti i punti vendita (limitato a 100 a livello di programmazione per evitare problemi di prestazioni)
   - tutti i prodotti per il negozio `store1`
   - prodotti con `sku1` per tutti i negozi
   - prodotti con `sku1` per store con codice `store1` e `store2`
   - prodotti con `sku1`, `sku2` e `sku3` per store con codice `store1` e `store2`

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

  L’esempio che segue memorizza in cache per `store-page` tipo di entità in base ai seguenti criteri:
   - pagina `/contact-us` per tutti i negozi
   - pagina `/contact-us` per store con ID `1`
   - pagina `/contact-us` per store con codice `code1` e `code2`

  ```yaml
        stage:
          post-deploy:
            WARM_UP_PAGES:
              - "store-page:/contact-us:*"
              - "store-page:/contact-us:1"
              - "store-page:/contact-us:code1|code2"
  ```
