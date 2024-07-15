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
| Funzioni principali | <ul><li>[Tutte le funzionalità di Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>Strumento di onboarding PayPal</li><li>[Rapporti di Commerce](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li></ul> | <ul><li>[Tutte le funzionalità di Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-operations/release/features.html)</li><li>Strumento di onboarding PayPal</li><li>[Rapporti di Commerce](https://business.adobe.com/products/magento/business-intelligence.html?_ga=2.85288604.442698376.1665067470-1322106587.1655147209)</li><li>[Modulo B2B](https://business.adobe.com/products/magento/b2b-ecommerce.html?_ga=2.105948422.442698376.1665067470-1322106587.1655147209)</li></ul> |
| Infrastruttura e installazione | <ul><li>Strumenti di integrazione cloud continua con utenti illimitati</li><li>Fastly Content Delivery Network (CDN), Image Optimization (IO) e maggiore sicurezza con ampi margini di larghezza di banda. Il servizio Web Application Firewall (WAF) è disponibile solo negli ambienti di produzione.</li><li>[New Relic](../monitor/new-relic-service.md) APM (Monitoraggio delle prestazioni) in 3 rami: `master` e 2 di tua scelta<br>Platform as a Service (PaaS) ambienti di produzione, staging e sviluppo (4 ambienti attivi totali) ottimizzati per Adobe Commerce</li><li>Filtro in uscita (firewall in uscita)</li></ul> | <ul><li>Strumenti di integrazione cloud continua con utenti illimitati</li><li>Fastly Content Delivery Network (CDN), Image Optimization (IO) e maggiore sicurezza con ampi margini di larghezza di banda. Il servizio Web Application Firewall (WAF) è disponibile solo negli ambienti di produzione.</li><li>[Infrastruttura New Relic](../monitor/new-relic-service.md) in produzione + APM (Monitoraggio delle prestazioni) in staging e produzione. Il criterio [Avvisi gestiti](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts) per Adobe Commerce implementa le procedure consigliate di monitoraggio per inviare notifiche proattive sui problemi dell&#39;applicazione e dell&#39;infrastruttura che influiscono sulle prestazioni del sito.</li><li>[Ambienti di sviluppo dell&#39;integrazione](pro-architecture.md#integration-environment) basati su Platform as a Service (PaaS) (2 ambienti attivi in totale) ottimizzati per Adobe Commerce</li><li>Infrastructure as a Service (IaaS): infrastruttura virtuale dedicata per gli ambienti di staging e produzione</li></ul> |
| Infrastruttura ad alta disponibilità | | [Architettura ad alta disponibilità](pro-architecture.md#redundant-hardware) con configurazione a tre server nel servizio IaaS (Infrastructure as a Service) sottostante per garantire affidabilità e disponibilità di livello enterprise |
| Hardware dedicato | | Hardware isolato e dedicato nell&#39;infrastruttura sottostante come servizio (IaaS) per fornire livelli ancora più elevati di affidabilità e disponibilità |
| Supporto e-mail 24x7 | Monitoraggio e supporto e-mail 24 ore su 24, 7 giorni su 7 per l’applicazione di base e l’infrastruttura cloud | Monitoraggio e supporto e-mail 24 ore su 24, 7 giorni su 7 per l’applicazione di base e l’infrastruttura cloud |
| Un consulente tecnico dedicato del cliente (CTA) | | Gestione tecnica dedicata dell’account per il periodo di avvio iniziale, a partire dall’abbonamento fino all’avvio iniziale del sito |
| Componenti aggiuntivi\* | <ul><li>[Modulo B2B](https://business.adobe.com/products/magento/b2b-ecommerce.html)</li></ul> |

\* _Disponibile a pagamento_

## Progetti iniziali

L&#39;architettura del piano [Starter](starter-architecture.md) dispone di quattro ambienti:

- **Integrazione**: l&#39;ambiente di integrazione fornisce due ambienti testabili. Ogni ambiente include un ramo Git attivo, un database, un server web, la memorizzazione in cache, alcuni servizi, variabili di ambiente e configurazioni.

- **Staging**: quando il codice e le estensioni superano i test, è possibile unire il ramo `integration` all&#39;ambiente di staging, che diventa l&#39;ambiente di test di pre-produzione. Include il ramo attivo `staging`, il database, il server Web, la memorizzazione nella cache, i servizi di terze parti, le variabili di ambiente, le configurazioni e i servizi, come Fastly e New Relic.

- **Produzione**: quando il codice è pronto e testato, tutto il codice viene unito a `master` per la distribuzione nel sito live di produzione. Questo ambiente include il ramo `master` attivo, il database, il server Web, la memorizzazione nella cache, i servizi di terze parti, le variabili di ambiente e le configurazioni.

- **Inattivo** - Hai un numero illimitato di rami inattivi.

## Progetti Pro

L&#39;architettura del piano [Pro](pro-architecture.md) ha un `master` globale con tre ambienti:

- **Integrazione**: l&#39;ambiente di integrazione fornisce un ambiente testabile che include un database, un server Web, il caching, alcuni servizi, variabili di ambiente e configurazioni. Puoi sviluppare, distribuire e testare il codice prima di unirlo all’ambiente di staging.

   - _Inattivo_ - È possibile avere un numero illimitato di rami inattivi in base all&#39;ambiente `integration`, ma un solo ramo attivo (escluso `integration`).

- **Gestione temporanea**: l&#39;ambiente di gestione temporanea è destinato a test di pre-produzione e include un database, un server Web, la memorizzazione nella cache, servizi di terze parti, variabili di ambiente, configurazioni e servizi, ad esempio Fastly.

- **Produzione**: l&#39;ambiente di produzione include un&#39;architettura a tre nodi ad alta disponibilità per i dati, i servizi, la memorizzazione in cache e l&#39;archiviazione. La produzione è il tuo ambiente di store pubblico live con variabili di ambiente, configurazioni e servizi di terze parti.

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
>Per le versioni consigliate, vedere [Requisiti di sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) nella _Guida all&#39;installazione_.

Il modulo Fastly CDN viene utilizzato per i servizi CDN e di caching negli ambienti di staging e produzione. Vedere [Configurare Fastly Services](../cdn/fastly.md).

Per informazioni sulla configurazione delle versioni del software da utilizzare nell’implementazione, consulta i seguenti file di configurazione di Adobe Commerce sull’infrastruttura cloud:

- [Configurazione dell’applicazione (.magento.app.yaml)](../application/configure-app-yaml.md)
- [Configurazione dell’ambiente (.magento.env.yaml)](../environment/configure-env-yaml.md)
- [Configurazione route (route.yaml)](../routes/routes-yaml.md)
- [Configurazione servizi (services.yaml)](../services/services-yaml.md)
