---
title: Panoramica dei file di configurazione
description: Scopri come configurare l’ambiente dell’infrastruttura cloud per supportare l’implementazione e la gestione dello store Adobe Commerce personalizzato.
feature: Cloud, Configuration, Services, Iaas, Paas
exl-id: f469a0ec-e459-413f-9725-66a0fbf34f01
source-git-commit: 47b66d0d2bbff14e76ce49182a68d5e6c9fb13a7
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 0%

---

# Panoramica dei file di configurazione

Gli ambienti in Adobe Commerce su infrastrutture cloud includono contenitori con applicazioni, servizi e un database per fornire un sistema completo per la base di codice e i file dell’applicazione Adobe Commerce.

Puoi configurare le impostazioni dell’applicazione, i percorsi, le azioni di build e distribuzione e le notifiche per supportare gli ambienti di progetto utilizzando i seguenti file di configurazione:

| Configurazione | Nome file | Descrizione |
| ------------- | -------- | ----------- |
| [Applicazione](../application/configure-app-yaml.md) | `.magento.app.yaml` | Definisce come generare e distribuire Adobe Commerce, inclusi servizi, hook e processi cron. |
| [Ambiente](configure-env-yaml.md) | `.magento.env.yaml` | Centralizza la gestione delle azioni di generazione e implementazione in tutti gli ambienti, inclusi Pro Staging e Produzione, utilizzando variabili di ambiente. |
| [Percorsi](../routes/routes-yaml.md) | `.magento/routes.yaml` | Configura la memorizzazione in cache, i reindirizzamenti e le inclusioni lato server. |
| [Servizio](../services/services-yaml.md) | `.magento/services.yaml` | Definisce i servizi utilizzati da Adobe Commerce per nome e versione. Ad esempio, questo file può includere versioni di MariaDB, estensioni PHP, Redis, RabbitMQ e Elasticsearch o OpenSearch. È necessario aprire un ticket di supporto per inviare queste modifiche agli ambienti Pro plan di staging e produzione. |
| [Impostazioni PHP](../application/php-settings.md#configure-php) | `php.ini` | File facoltativo che può essere aggiunto al progetto. Le impostazioni contenute in questo file vengono aggiunte a quelle gestite dall’infrastruttura cloud. |

{style="table-layout:auto"}

## Aggiornamenti alla configurazione degli ambienti Pro

Per gli ambienti di produzione e staging Pro di Adobe Commerce su infrastruttura cloud, puoi aggiornare molte opzioni di configurazione nell’ambiente di sviluppo locale e confermare le modifiche per applicarle a tali ambienti. Tuttavia, è necessario [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per aggiornare le seguenti opzioni di configurazione:

- Installare o aggiornare i servizi in `.magento/services.yaml` file.
- Modificare la configurazione per `mounts` e `disk` proprietà in `.magento.app.yaml` file.

{{pro-self-service-warning}}
