---
title: Architettura cloud per Commerce
description: Scopri il contrasto tra le architetture di progetto Starter e Pro per l’infrastruttura Commerce on Cloud.
feature: Cloud, Iaas, Paas
topic: Architecture
recommendations: noDisplay
exl-id: 37cd6733-c10a-4d06-b784-171da576f9fc
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 0%

---

# Architettura cloud per Commerce

Adobe Commerce sull’infrastruttura cloud dispone di un piano Starter e Pro. Ogni piano dispone di un’architettura unica per guidare il processo di sviluppo e distribuzione di Adobe Commerce. Sia il piano Starter che l&#39;architettura del piano Pro implementano database, server web e server di caching in più ambienti per test end-to-end, supportando al contempo l&#39;integrazione continua.

Per un confronto, ogni piano include le seguenti caratteristiche dell&#39;infrastruttura e i prodotti supportati.

|          | Starter | Pro |
| -------- | --------------------| ------------------ |
| Funzioni principali | <ul><li>[Tutte le funzioni di Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>Strumento di onboarding PayPal</li><li>[Reporting Commerce](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[Tutte le funzioni di Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>Strumento di onboarding PayPal</li><li>[Reporting Commerce](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[Modulo B2B](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| Infrastruttura e installazione | <ul><li>Strumenti di integrazione cloud continua con utenti illimitati</li><li>Fastly Content Delivery Network (CDN), Image Optimization (IO) e maggiore sicurezza con ampi margini di larghezza di banda. Il servizio Web Application Firewall (WAF) è disponibile solo negli ambienti di produzione.</li><li>[New Relic](../monitor/new-relic-service.md) APM (Monitoraggio delle prestazioni) su 3 rami: `master` e 2 a scelta<br>Ambienti PaaS (Platform as a Service) di produzione, staging e sviluppo (4 ambienti attivi totali) ottimizzati per Adobe Commerce</li><li>Filtro in uscita (firewall in uscita)</li></ul> | <ul><li>Strumenti di integrazione cloud continua con utenti illimitati</li><li>Fastly Content Delivery Network (CDN), Image Optimization (IO) e maggiore sicurezza con ampi margini di larghezza di banda. Il servizio Web Application Firewall (WAF) è disponibile solo negli ambienti di produzione.</li><li>[New Relic](../monitor/new-relic-service.md) Infrastruttura di produzione + APM (Performance Monitoring) su staging e produzione. Il [Criteri degli avvisi gestiti](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) per Adobe Commerce policy implementa le best practice di monitoraggio per segnalare in modo proattivo i problemi relativi all’applicazione e all’infrastruttura che influiscono sulle prestazioni del sito.</li><li>Basato su Platform as a Service (PaaS) [Sviluppo dell’integrazione](pro-architecture.md#integration-environment) ambienti (2 ambienti attivi totali) ottimizzati per Adobe Commerce</li><li>Infrastructure as a Service (IaaS): infrastruttura virtuale dedicata per gli ambienti di staging e produzione</li></ul> |
| Infrastruttura ad alta disponibilità | | [Architettura ad alta disponibilità](pro-architecture.md#redundant-hardware) con una configurazione a tre server nell&#39;infrastruttura sottostante come servizio (IaaS) per garantire affidabilità e disponibilità di livello enterprise |
| Hardware dedicato | | Hardware isolato e dedicato nell&#39;infrastruttura sottostante come servizio (IaaS) per fornire livelli ancora più elevati di affidabilità e disponibilità |
| Supporto e-mail 24x7 | Monitoraggio e supporto e-mail 24 ore su 24, 7 giorni su 7 per l’applicazione di base e l’infrastruttura cloud | Monitoraggio e supporto e-mail 24 ore su 24, 7 giorni su 7 per l’applicazione di base e l’infrastruttura cloud |
| Un consulente tecnico dedicato del cliente (CTA) | | Gestione tecnica dedicata dell’account per il periodo di avvio iniziale, a partire dall’abbonamento fino all’avvio iniziale del sito |
| Componenti aggiuntivi\* | <ul><li>[Modulo B2B](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> |

\* _Disponibile a pagamento_

## Progetti iniziali

Il [Architettura del piano iniziale](starter-architecture.md) dispone di quattro ambienti:

- **Integrazione**: l’ambiente di integrazione fornisce due ambienti testabili. Ogni ambiente include un ramo Git attivo, un database, un server web, la memorizzazione in cache, alcuni servizi, variabili di ambiente e configurazioni.

- **Staging**- Quando il codice e le estensioni superano i test, è possibile unire `integration` si sposta nell’ambiente di staging, che diventa l’ambiente di test di pre-produzione. Include `staging` ramo attivo, database, server web, caching, servizi di terze parti, variabili di ambiente, configurazioni e servizi, come Fastly e New Relic.

- **Produzione**- Quando il codice è pronto e testato, tutto il codice viene unito in `master` per la distribuzione al sito live di produzione. Questo ambiente include il `master` branch, database, server web, caching, servizi di terze parti, variabili di ambiente e configurazioni.

- **Inattivo**- È presente un numero illimitato di rami inattivi.

## Progetti Pro

Il [Architettura del piano Pro](pro-architecture.md) ha una dimensione globale `master` con tre ambienti:

- **Integrazione**: l&#39;ambiente di integrazione fornisce un ambiente testabile che include un database, un server web, il caching, alcuni servizi, variabili di ambiente e configurazioni. Puoi sviluppare, distribuire e testare il codice prima di unirlo all’ambiente di staging.

   - _Inattivo_- È possibile disporre di un numero illimitato di rami inattivi in base al `integration` ma solo un ramo attivo (escluso `integration` ).

- **Staging**: l&#39;ambiente di staging è destinato ai test di pre-produzione e include un database, un server web, il caching, servizi di terze parti, variabili di ambiente, configurazioni e servizi, come Fastly.

- **Produzione**: l&#39;ambiente di produzione include un&#39;architettura a tre nodi ad elevata disponibilità per i dati, i servizi, il caching e l&#39;archiviazione. La produzione è il tuo ambiente di store pubblico live con variabili di ambiente, configurazioni e servizi di terze parti.

## Software e servizi supportati

Adobe Commerce sull’infrastruttura cloud utilizza:

- Sistema operativo: Debian GNU/Linux
- Server web: Nginx
- Database: MySQL (MariaDB)
- Content Delivery Network (CDN): rete CDN Fastly

Puoi configurare i seguenti servizi:

- [PHP](../application/php-settings.md)
- [MySQL](../services/mysql.md)
- [Redis](../services/redis.md)
- [RabbitMQ](../services/rabbitmq.md)
- [Elasticsearch](../services/elasticsearch.md)
- [OpenSearch](../services/opensearch.md)

{{elasticsearch-support}}

>[!NOTE]
>
>Consulta [Requisiti di sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) nel _Guida all’installazione_ versioni consigliate.

Il modulo Fastly CDN viene utilizzato per i servizi CDN e di caching negli ambienti di staging e produzione. Consulta [Configurare i servizi Fastly](../cdn/fastly.md).

Per informazioni sulla configurazione delle versioni del software da utilizzare nell’implementazione, consulta i seguenti file di configurazione di Adobe Commerce sull’infrastruttura cloud:

- [Configurazione dell’applicazione (.magento.app.yaml)](../application/configure-app-yaml.md)
- [Configurazione dell’ambiente (.magento.env.yaml)](../environment/configure-env-yaml.md)
- [Configurazione route (route.yaml)](../routes/routes-yaml.md)
- [Configurazione servizi (services.yaml)](../services/services-yaml.md)
