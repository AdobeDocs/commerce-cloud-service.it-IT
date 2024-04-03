---
title: Configura servizio Elasticsearch
description: Scopri come abilitare il servizio Elasticsearch per Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Search, Services
exl-id: ac559cbb-342a-4756-ade5-49eba4827965
source-git-commit: 8147b43b26370d9305c3c7dc47865ddcbae1904d
workflow-type: tm+mt
source-wordcount: '798'
ht-degree: 0%

---

# Configura servizio Elasticsearch

[Elasticsearch](https://www.elastic.co) è un prodotto open-source che consente di estrarre dati da qualsiasi origine, qualsiasi formato, nonché di ricercarli e visualizzarli in tempo reale.

{{elasticsearch-support}}

Per Adobe Commerce versione 2.4.4 e successive, vedi [Configura servizio OpenSearch](opensearch.md).

- Elasticsearch esegue ricerche rapide e avanzate sui prodotti nel catalogo dei prodotti
- Gli analizzatori Elasticsearch supportano più lingue
- Supporta parole non significative e sinonimi
- L’indicizzazione non influisce sui clienti fino al completamento dell’operazione di reindicizzazione

>[!TIP]
>
>L’Adobe consiglia di impostare sempre Elasticsearch per il progetto di infrastruttura cloud di Adobe Commerce, anche se prevedi di configurare uno strumento di ricerca di terze parti per l’applicazione Adobe Commerce. L’Elasticsearch di configurazione fornisce un’opzione di fallback nel caso in cui lo strumento di ricerca di terze parti non riesca.

{{service-instruction}}

**Per abilitare l&#39;Elasticsearch**:

1. Per i progetti iniziali, aggiungi `elasticsearch` servizio al `.magento/services.yaml` file con la versione di Elasticsearch e spazio su disco allocato in MB.

   ```yaml
   elasticsearch:
       type: elasticsearch:<version>
       disk: 1024
   ```

   Per i progetti Pro, è necessario inviare un ticket di supporto Adobe Commerce per modificare la versione dell’Elasticsearch negli ambienti di staging e produzione.

1. Imposta il `relationships` proprietà in `.magento.app.yaml` file.

   ```yaml
   relationships:
       elasticsearch: "elasticsearch:elasticsearch"
   ```

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable Elasticsearch" && git push origin <branch-name>
   ```

   Per informazioni su come queste modifiche influiscono sugli ambienti, consulta [Servizi](services-yaml.md).

1. Al termine del processo di distribuzione, utilizzare SSH per accedere all&#39;ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Reindicizza l’indice di ricerca del catalogo.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Pulire la cache.

   ```bash
   bin/magento cache:clean
   ```

{{service-change-tip}}

## Elasticsearch compatibilità software

Quando installi o aggiorni il progetto Adobe Commerce on Cloud Infrastructure, verifica sempre la compatibilità tra la versione del servizio Elasticsearch e [ELASTICSEARCH PHP](https://github.com/elastic/elasticsearch-php) client per Adobe Commerce.

- **Prima impostazione**-Confermare che la versione dell&#39;Elasticsearch specificata in `services.yaml` è compatibile con il client PHP Elasticsearch configurato per Adobe Commerce.

- **Aggiornamento del progetto**-Verificare che il client PHP Elasticsearch nella nuova versione dell&#39;applicazione sia compatibile con la versione del servizio Elasticsearch installata nell&#39;infrastruttura cloud.

Il supporto per la versione del servizio e la compatibilità per l’infrastruttura cloud di Adobe Commerce è determinato dalle versioni distribuite nell’infrastruttura cloud e talvolta differisce dalle versioni supportate dalle distribuzioni Adobe Commerce on-premise. Consulta [Versioni del servizio](services-yaml.md#service-versions).

**Per verificare la compatibilità del software di Elasticsearch**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Mostra i dettagli dell’Elasticsearch per l’ambiente attivo.

   ```bash
   magento-cloud relationships --property=elasticsearch
   ```

1. In alternativa, è possibile utilizzare SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Controlla la versione del pacchetto Compositore per `elasticsearch/elasticsearch`.

   ```bash
   composer show elasticsearch/elasticsearch
   ```

   Nella risposta, controlla la versione installata nella sezione `versions` proprietà.

   ```terminal
   name     : elasticsearch/elasticsearch
   descrip. : PHP Client for Elasticsearch
   keywords : client, elasticsearch, search
   versions : * v7.17.1
   type     : library
   license  : Apache License 2.0 (Apache-2.0) (OSI approved) https://spdx.org/licenses/Apache-2.0.html#licenseText
   license  : GNU Lesser General Public License v2.1 only (LGPL-2.1-only) (OSI approved) https://spdx.org/licenses/LGPL-2.1-only.html#licenseText
   homepage :
   source   : [git] git@github.com:elastic/elasticsearch-php.git f1b8918f411b837ce5f6325e829a73518fd50367
   dist     : [zip] https://api.github.com/repos/elastic/elasticsearch-php/zipball/f1b8918f411b837ce5f6325e829a73518fd50367 f1b8918f411b837ce5f6325e829a73518fd50367
   path     : ~/vendor/elasticsearch/elasticsearch
   names    : elasticsearch/elasticsearch
   ```

   Inoltre, è possibile trovare la versione client PHP di Elasticsearch in  `composer.lock` nella directory principale dell’ambiente.

1. Dalla riga di comando, recupera i dettagli della connessione al servizio Elasticsearch.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   Nella risposta, individua l’indirizzo IP per l’endpoint del servizio Elasticsearch:

   ```terminal
   | elasticsearch:                                                                                                  |
   +------------------------------------------+----------------------------------------------------------------------+
   | username                                 | null                                                                 |
   | scheme                                   | http                                                                 |
   | service                                  | elasticsearch                                                        |
   | fragment                                 | null                                                                 |
   | ip                                       | 169.254.220.11                                                       |
   | hostname                                 | dzggu33f75wi3sd24lgwtoupxm.elasticsearch.service._.magentosite.cloud |
   | public                                   | false                                                                |
   | cluster                                  | fo3qdoxtla4j4-master-7rqtwti                                         |
   | host                                     | elasticsearch.internal                                               |
   | rel                                      | elasticsearch                                                        |
   | query                                    |                                                                      |
   | path                                     | null                                                                 |
   | password                                 | null                                                                 |
   | type                                     | elasticsearch:6.5                                                    |
   | port                                     | 9200                                                                 |
   +------------------------------------------+----------------------------------------------------------------------+
   ```

1. Recupera il servizio Elasticsearch installato `version:number` dall’endpoint del servizio.

   ```bash
   curl -XGET <elasticsearch-service-endpoint-ip-address>:9200/
   ```

   ```terminal
   {
      "name" : "-AqGi9D",
      "cluster_name" : "elasticsearch",
      "cluster_uuid" : "_yze6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "number" : "6.5.4",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "82a8aa7",
        "build_date" : "2019-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "7.5.0",
        "minimum_wire_compatibility_version" : "5.6.0",
        "minimum_index_compatibility_version" : "5.0.0"
   },
   "  tagline" : "You Know, for Search"
   }
   ```

1. Verificare la compatibilità della versione tra il servizio Elasticsearch e il client PHP.

   Se le versioni non sono compatibili, effettua uno dei seguenti aggiornamenti alla configurazione dell’ambiente:

   - Modificare il client PHP Elasticsearch in una versione compatibile con la versione del servizio Elasticsearch.

     ```bash
     composer require "elasticsearch/elasticsearch:~<version>"
     ```

   - Modificare la versione del servizio Elasticsearch in `services.yaml` in una versione compatibile con il client PHP Elasticsearch.

     {{pro-update-service}}

## Riavvia il servizio Elasticsearch

Per riavviare il [Elasticsearch](https://www.elastic.co) , è necessario contattare il supporto Adobe Commerce.

## Configurazione di ricerca aggiuntiva

- Per impostazione predefinita, la configurazione di ricerca per gli ambienti Cloud viene rigenerata ogni volta che si distribuisce. È possibile utilizzare `SEARCH_CONFIGURATION` distribuisci variabile per mantenere le impostazioni di ricerca personalizzate tra le distribuzioni. Consulta [Distribuire le variabili](../environment/variables-deploy.md#search_configuration).

- Dopo aver configurato il servizio Elasticsearch per il progetto, utilizza l’interfaccia utente di amministrazione per testare la connessione Elasticsearch e personalizzare le impostazioni Elasticsearch per Adobe Commerce.

### Aggiungi plug-in per Elasticsearch

Facoltativamente, puoi aggiungere plug-in, ad Elasticsearch, aggiungendo il `configuration:plugins` al servizio di Elasticsearch nella sezione `.magento/services.yaml` file. Ad esempio, il codice seguente abilita i plug-in di analisi ICU e analisi fonetica.

```yaml
elasticsearch:
    type: elasticsearch:<service-version>
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Se utilizzi il plug-in di terze parti Elastic Suite, devi [aggiorna il `ece-tools` pacchetto](../dev-tools/update-package.md) alla versione 2002.0.19 o successiva.
Durante la configurazione di Elastic Suite, aggiungi le impostazioni di configurazione alla `ELASTICSUITE_CONFIGURATION` distribuire la variabile. Questa configurazione salva le impostazioni tra le distribuzioni.

### Rimuovi i plug-in per Elasticsearch

Rimozione delle voci del plug-in da `elasticsearch:` in `.magento/services.yaml` non li disinstalla o li disattiva come previsto. Devi reindicizzare i dati dell’Elasticsearch. Questo comportamento è intenzionale per evitare la possibile perdita o danneggiamento dei dati che dipendono da questi plug-in.

**Per rimuovere i plug-in di Elasticsearch**:

1. Rimuovi le voci del plug-in di Elasticsearch dal tuo `.magento/services.yaml` file.
1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove Elasticsearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Eseguire il commit di `.magento/services.yaml` modifiche all’archivio Cloud.
1. Reindicizza l’indice di ricerca del catalogo.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Pulire la cache.

   ```bash
   bin/magento cache:clean
   ```

>[!TIP]
>
>Per informazioni dettagliate sull’utilizzo o sulla risoluzione dei problemi relativi al plug-in Elastic Suite con Adobe Commerce, consulta la sezione [Documentazione di Elastic Suite](https://github.com/Smile-SA/elasticsuite).

## Risoluzione dei problemi

Consulta i seguenti articoli di supporto Adobe Commerce per assistenza nella risoluzione dei problemi di Elasticsearch:

- [L&#39;Elasticsearch 5 è configurato, ma la pagina di ricerca non viene caricata con l&#39;errore &quot;Dati campo è disabilitato...&quot;](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-5-is-configured-but-search-page-does-not-load-with-fielddata-is-disabled...-error.html)
- [La paginazione del catalogo non funziona quando si utilizza l’Elasticsearch 6.x](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/catalog-pagination-doesn-t-work-when-elasticsearch-6.x-is-used.html)
- [Elasticsearch di risoluzione dei problemi in Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-in-magento-troubleshooter.html)
- [Lo stato dell’indice Elasticsearch è `yellow` o `red`](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/elasticsearch/elasticsearch-index-status-is-yellow-or-red.html)
