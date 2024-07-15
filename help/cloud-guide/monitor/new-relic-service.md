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

Tutti i progetti Adobe Commerce sull&#39;infrastruttura cloud includono l&#39;accesso al servizio New Relic per monitorare le prestazioni e analizzare gli eventi dell&#39;applicazione [!DNL Commerce] e dell&#39;infrastruttura cloud.

Le seguenti funzioni di New Relic sono disponibili per l’utilizzo con gli ambienti di produzione e staging:

- [New Relic APM](#new-relic-apm) (Pro e Starter)
- [Infrastruttura New Relic](#new-relic-infrastructure) (solo Pro)
- [Gestione registro New Relic](#new-relic-logs) (solo Pro)

>[!INFO]
>
>Altre funzioni di New Relic non sono disponibili nei progetti Adobe Commerce.

## APM NEW RELIC

[New Relic for application performance management (APM)](https://docs.newrelic.com/introduction-apm/) è un prodotto di analisi software che consente di analizzare e migliorare le interazioni tra le applicazioni. New Relic APM è disponibile per tutti i progetti Adobe Commerce su infrastrutture cloud e fornisce le seguenti funzioni:

- **Concentrati su transazioni specifiche**: contrassegna e monitora attivamente le azioni chiave dei clienti nel tuo sito, ad esempio l&#39;aggiunta al carrello, l&#39;estrazione o l&#39;elaborazione di un pagamento.
- **Monitoraggio delle query del database**: individuare e monitorare le query del database che influiscono sulle prestazioni.
- **Mappa app**: visualizza tutte le dipendenze dell&#39;applicazione all&#39;interno del sito, delle estensioni e dei servizi esterni.
- **[!DNL Apdex]punteggi**—Valuta le prestazioni e crea avvisi che identificano i problemi e ti avvisano quando si verificano, ad esempio le prestazioni del sito interessate da una vendita flash o da un evento web. Vedi [Punteggio Apdex](https://docs.newrelic.com/docs/apm/new-relic-apm/apdex/apdex-measure-user-satisfaction/).
- **Avvisi gestiti per Adobe Commerce**-Utilizzare questo criterio di avviso di New Relic per monitorare le prestazioni dell&#39;applicazione e dell&#39;infrastruttura in base alle best practice del settore. Vedere [Monitorare le prestazioni con gli avvisi gestiti per i criteri di avviso di Adobe Commerce](investigate-performance.md/#monitor-performance-with-managed-alerts).
- **Monitora le distribuzioni**: monitora gli eventi di distribuzione e analizza l&#39;impatto della distribuzione sulle prestazioni complessive. Vedi [Tracciare le distribuzioni](track-deployments.md).

Il progetto Adobe Commerce su infrastruttura cloud include il software per il servizio New Relic APM e un codice di licenza. Non è necessario acquistare o installare software aggiuntivo.

## Infrastruttura New Relic

I progetti Pro includono il servizio [Infrastruttura New Relic (NRI)](https://docs.newrelic.com/docs/infrastructure/infrastructure-monitoring/get-started/get-started-infrastructure-monitoring/), che si connette automaticamente ai dati dell&#39;applicazione e all&#39;analisi delle prestazioni per fornire il monitoraggio dinamico del server. Questo servizio è disponibile negli ambienti Pro Production e Staging.

## Gestione registro New Relic

Tutti i progetti di infrastruttura cloud includono [Gestione registro New Relic](log-management.md). Il servizio è preconfigurato per aggregare tutti i dati di registro dagli ambienti di staging e produzione e visualizzarli in un dashboard di gestione dei registri centralizzato.
