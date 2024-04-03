---
title: Configura servizio OpenSearch
description: Scopri come abilitare il servizio OpenSearch per Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Search, Services
exl-id: 10dc6367-3f90-4ab6-a84e-15e8c3b32a38
source-git-commit: d4c36b084094846cfad69adc2bffd567a58fab26
workflow-type: tm+mt
source-wordcount: '651'
ht-degree: 0%

---

# Configura servizio OpenSearch

Il [OpenSearch](https://www.opensearch.org) Il servizio è un fork open-source dell&#39;Elasticsearch 7.10.2, ad Elasticsearch in seguito alle modifiche delle licenze. Consulta la [Progetto OpenSource](https://github.com/opensearch-project) in GitHub.

{{elasticsearch-support}}

OpenSearch consente di estrarre dati da qualsiasi origine, qualsiasi formato, nonché di eseguire ricerche e visualizzazioni in tempo reale.

- Ricerche rapide e avanzate sui prodotti nel catalogo dei prodotti
- Gli analizzatori OpenSearch supportano più lingue
- Supporta parole non significative e sinonimi
- L’indicizzazione non influisce sui clienti fino al completamento dell’operazione di reindicizzazione

{{service-instruction}}

>[!TIP]
>
>L’Adobe consiglia di impostare sempre OpenSearch per il progetto di infrastruttura cloud di Adobe Commerce, anche se si intende configurare uno strumento di ricerca di terze parti per l’applicazione Adobe Commerce. L&#39;impostazione di OpenSearch fornisce un&#39;opzione di fallback se lo strumento di ricerca di terze parti non riesce.

**Per abilitare OpenSearch**:

1. Per gli ambienti di integrazione Starter e Pro, aggiungere `opensearch` servizio al `.magento/services.yaml` con la versione appropriata e lo spazio su disco allocato in MB. In questo caso, è appropriata la versione 2. La versione secondaria non è necessaria perché l’infrastruttura cloud utilizza la versione più recente di OpenSearch.

   ```yaml
   opensearch:
       type: opensearch:2
       disk: 1024
   ```

   Per i progetti Pro, è necessario [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per modificare la versione di OpenSearch negli ambienti di staging e produzione.

1. Impostare o verificare `relationships` proprietà in `.magento.app.yaml` file.

   ```yaml
   relationships:
       opensearch: "opensearch:opensearch"
   ```

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable OpenSearch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   Per informazioni su come queste modifiche influiscono sugli ambienti, consulta [Configurare i servizi](services-yaml.md).

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

## Compatibilità con il software OpenSearch

Quando installi o aggiorni il progetto Adobe Commerce on Cloud Infrastructure, verifica sempre la compatibilità tra la versione del servizio OpenSearch e [OpenSearch PHP](https://github.com/opensearch-project/opensearch-php) client per Adobe Commerce.

- **Prima impostazione**-Confermare che la versione di OpenSearch specificata in `services.yaml` è compatibile con il client OpenSearch PHP configurato per Adobe Commerce.

- **Aggiornamento del progetto**-Verificare che il client OpenSearch PHP nella nuova versione dell&#39;applicazione sia compatibile con la versione del servizio OpenSearch installata nell&#39;infrastruttura cloud.

Il supporto per la versione del servizio e la compatibilità è determinato dalle versioni testate e distribuite nell’infrastruttura Cloud e talvolta è diverso dalle versioni supportate dalle distribuzioni locali di Adobe Commerce. Consulta [Requisiti di sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) nel _Guida all’installazione_ per un elenco delle versioni supportate.

**Per verificare la compatibilità del software OpenSearch**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Mostra i dettagli di OpenSearch per l&#39;ambiente attivo.

   ```bash
   magento-cloud relationships --property=opensearch
   ```

1. In alternativa, è possibile utilizzare SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recuperare i dettagli della connessione al servizio OpenSearch.

   ```bash
   vendor/bin/ece-tools env:config:show services
   ```

   Nella risposta, trovare l&#39;indirizzo IP e la porta per l&#39;endpoint del servizio OpenSearch:

   ```terminal
   +------------------------------------------+--------------------------------------------------------+
   | opensearch:                                                                                       |
   +------------------------------------------+--------------------------------------------------------+
   | username                                 | null                                                   |
   | scheme                                   | http                                                   |
   | service                                  | opensearch                                             |
   | fragment                                 | null                                                   |
   | ip                                       | 169.254.220.11                                         |
   | hostname                                 | hostf75wi3sd24l.opensearch.service._.magentosite.cloud |
   | port                                     | 9200                                                   |
   | cluster                                  | projectID-develop-4ranwui                              |
   | host                                     | opensearch.internal                                    |
   | rel                                      | opensearch                                             |
   | path                                     | null                                                   |
   | query                                    |                                                        |
   | password                                 | null                                                   |
   | type                                     | opensearch:2                                           |
   | public                                   | false                                                  |
   | host_mapped                              | false                                                  |
   ```

1. Recuperare il servizio OpenSearch installato `version:number` dall’endpoint del servizio.

   ```bash
   curl -XGET <opensearch-service-endpoint-ip-address>:9200
   ```

   ```terminal
   {
      "name" : "opensearch.0",
      "cluster_name" : "opensearch",
      "cluster_uuid" : "_yzaae6-ywSEW1MaAF8ZPWyQ",
      "version" : {
        "distribution" : "opensearch",
        "number" : "2.5.0",
        "build_type" : "deb",
        "build_hash" : "aaaaaaa",
        "build_date" : "2023-01-23T12:07:18.760675Z",
        "build_snapshot" : false,
        "lucene_version" : "9.4.2",
        "minimum_wire_compatibility_version" : "7.10.0",
        "minimum_index_compatibility_version" : "7.0.0"
   },
   "tagline" : "The OpenSearch Project: https://opensearch.org/"
   }
   ```

{{pro-update-service}}

## Riavviare il servizio OpenSearch

Se devi riavviare il servizio OpenSearch, devi contattare il supporto Adobe Commerce.

## Configurazione di ricerca aggiuntiva

- Per impostazione predefinita, la configurazione di ricerca per gli ambienti Cloud viene rigenerata ogni volta che si distribuisce. È possibile utilizzare `SEARCH_CONFIGURATION` distribuisci variabile per mantenere le impostazioni di ricerca personalizzate tra le distribuzioni. Consulta [Distribuire le variabili](../environment/variables-deploy.md#search_configuration).

- Dopo aver configurato il servizio OpenSearch per il progetto, utilizzare l&#39;interfaccia utente di amministrazione per verificare la connessione OpenSearch e personalizzare le impostazioni di OpenSearch per Adobe Commerce.

### Aggiungi plug-in per OpenSearch

Facoltativamente, è possibile aggiungere plug-in per OpenSearch aggiungendo il `configuration:plugins` al servizio OpenSearch nella sezione `.magento/services.yaml` file. Ad esempio, il codice seguente abilita i plug-in di analisi ICU e analisi fonetica.

```yaml
opensearch:
    type: opensearch:2
    disk: 1024
    configuration:
        plugins:
            - analysis-icu
            - analysis-phonetic
```

Consulta la [Progetto OpenSearch](https://github.com/opensearch-project) per ulteriori informazioni sui plug-in.

### Rimuovi i plug-in per OpenSearch

Rimozione delle voci del plug-in da `opensearch:` sezione del `.magento/services.yaml` il file **non** disinstalla o disabilita il servizio. Per disabilitare completamente il servizio, è necessario reindicizzare i dati OpenSearch dopo aver rimosso i plug-in dal `.magento/services.yaml` file. Questa progettazione evita la possibile perdita o danneggiamento di dati che dipendono da questi plug-in.

**Per rimuovere i plug-in di OpenSearch**:

1. Rimuovi le voci del plug-in OpenSearch dal tuo `.magento/services.yaml` file.
1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Remove OpenSearch plugin"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Eseguire il commit di `.magento/services.yaml` modifiche all’archivio cloud.
1. Reindicizza l’indice di ricerca del catalogo.

   ```bash
   bin/magento indexer:reindex catalogsearch_fulltext
   ```

1. Pulire la cache.

   ```bash
   bin/magento cache:clean
   ```
