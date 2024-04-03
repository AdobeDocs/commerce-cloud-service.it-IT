---
title: servizio New Relic
description: Scopri il servizio New Relic disponibile con il tuo progetto di infrastruttura cloud Adobe Commerce on.
feature: Cloud, Observability
last-substantial-update: 2023-09-06T00:00:00Z
exl-id: 613f0694-5338-4037-8ee4-ac5eca376159
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 0%

---

# Panoramica del servizio New Relic

Tutti i progetti Adobe Commerce sull’infrastruttura cloud includono l’accesso al servizio New Relic per monitorare le prestazioni e analizzare gli eventi del [!DNL Commerce] infrastruttura cloud e applicativa.

Le seguenti funzioni di New Relic sono disponibili per l’utilizzo con gli ambienti di produzione e staging:

- [APM NEW RELIC](#new-relic-apm) (Pro e Starter)
- [Infrastruttura New Relic](#new-relic-infrastructure) (Solo Pro)
- [Gestione registro New Relic](#new-relic-logs) (Solo Pro)

>[!INFO]
>
>Altre funzioni di New Relic non sono disponibili nei progetti Adobe Commerce.

## APM NEW RELIC

[New Relic per APM (Application Performance Management)](https://docs.newrelic.com/introduction-apm/) è un prodotto di analisi software che consente di analizzare e migliorare le interazioni tra le applicazioni. New Relic APM è disponibile per tutti i progetti Adobe Commerce su infrastrutture cloud e fornisce le seguenti funzioni:

- **Concentrarsi su transazioni specifiche**: contrassegna e monitora attivamente le azioni chiave dei clienti sul sito, ad esempio l&#39;aggiunta al carrello, l&#39;estrazione o l&#39;elaborazione di un pagamento.
- **Monitoraggio delle query del database**: individuazione e monitoraggio delle query del database che influiscono sulle prestazioni.
- **Mappa app**: visualizza tutte le dipendenze dell&#39;applicazione all&#39;interno del sito, le estensioni e i servizi esterni.
- **[!DNL Apdex]punteggi**— Valuta le prestazioni e crea avvisi che identificano i problemi e ti avvisano quando si verificano, ad esempio le prestazioni del sito interessate da una vendita flash o da un evento web. Consulta [Punteggio Apdex](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/).
- **Avvisi gestiti per Adobe Commerce**- Utilizzare questo criterio di avviso New Relic per monitorare le prestazioni dell&#39;applicazione e dell&#39;infrastruttura in base alle procedure ottimali del settore. Consulta [Monitorare le prestazioni con gli avvisi gestiti per i criteri di avviso di Adobe Commerce](investigate-performance.md/#monitor-performance-with-managed-alerts).
- **Tracciare le distribuzioni** monitoraggio degli eventi di distribuzione e analisi dell&#39;impatto sulle prestazioni complessive. Consulta [Tracciare le distribuzioni](track-deployments.md).

Il progetto Adobe Commerce su infrastruttura cloud include il software per il servizio New Relic APM e un codice di licenza. Non è necessario acquistare o installare software aggiuntivo.

## Infrastruttura New Relic

I progetti Pro includono [Infrastruttura New Relic (NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/) che si connette automaticamente ai dati dell&#39;applicazione e alle analisi delle prestazioni per fornire un monitoraggio dinamico del server. Questo servizio è disponibile negli ambienti Pro Production e Staging.

## Gestione registro New Relic

Tutti i progetti di infrastruttura cloud includono [Gestione registro New Relic](log-management.md). Il servizio è preconfigurato per aggregare tutti i dati di registro dagli ambienti di staging e produzione e visualizzarli in un dashboard di gestione dei registri centralizzato.
