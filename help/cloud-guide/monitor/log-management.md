---
title: Gestione registro New Relic
description: Scopri come utilizzare il registro di New Relic
feature: Cloud, Logs, Observability
exl-id: d8afb5c0-9727-4123-8944-81cd6ad7cbb7
source-git-commit: ebe1746ee9fd08480da5ad07d7f1f8299ad9af7e
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# Gestione registro New Relic

Tutti i progetti di infrastruttura cloud includono [Gestione registro New Relic](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/). Il servizio è preconfigurato per aggregare tutti i dati di registro dagli ambienti di staging e produzione e visualizzarli in un dashboard di gestione dei registri centralizzato.

I dati aggregati includono informazioni provenienti dai seguenti registri:

- Tutti `ece-tools` e i registri delle applicazioni da `~/var/log` directory
- Registri per servizi cloud da `var/log/platform/<project-ID>` directory
- Fastly CDN e WAF

Quando il progetto è connesso a New Relic, è possibile utilizzare il servizio Registri di New Relic per completare attività come le seguenti:

- Utilizzare le query New Relic per cercare dati di registro aggregati
- Visualizzare i dati di registro tramite l’applicazione New Relic Logs
- Creare grafici, dashboard e avvisi personalizzati
- Risolvere i problemi relativi alle prestazioni da un singolo dashboard

## Visualizzare e analizzare i dati di registro

Utilizza l’applicazione New Relic Logs per eseguire ricerche nei dati di registro aggregati e risolvere gli errori relativi ad applicazioni, infrastrutture, CDN e WAF. È possibile creare grafici, dashboard e avvisi utilizzando i dati di registro raccolti dai servizi New Relic APM e Infrastructure.

**Per utilizzare l&#39;applicazione New Relic Logs**:

1. Accedi al tuo [Account New Relic](https://login.newrelic.com/login).

1. Seleziona **Registri** dal menu di navigazione di Explorer.

1. Verifica che il tuo account sia selezionato nella parte superiore della sezione _Tutti i registri_ visualizzazione.

1. Selezionare un intervallo di tempo per la query Registri.

1. Per esaminare i dati di registro dell’infrastruttura per i servizi cloud (registri da `~/var/log/`), inserisci la stringa di query `has: "filePath"` nel _Cerca registri_ campo. Quindi, fai clic su **[!UICONTROL Query logs]**.

   I nomi dei file di registro vengono memorizzati nel `filePath` con percorsi completi del file di registro.

   ![Dati registro servizio New Relic progetto cloud](../../assets/new-relic/var-log-query.png)

1. Per esaminare i dati di registro Fastly, immettere la stringa di query `has: "client_ip"` nel _Cerca registri_ campo. Quindi, fai clic su **[!UICONTROL Query logs]**.

1. Per filtrare i risultati del registro Fastly in base al codice del paese, fare clic su **[!UICONTROL Add column]**, quindi seleziona **[!UICONTROL geo_country_code]**.

   ![Filtro attributi di registro CDN New Relic per progetto cloud](../../assets/new-relic/fastly-countrycode-filter.png)

>[!TIP]
>
>È possibile salvare la visualizzazione della query dal _Viste salvate_ a discesa. Clic **[!UICONTROL Create new]**, specifica un nome, seleziona le opzioni e fai clic su **[!UICONTROL Save view]**.
>
>Consulta [Introduzione alla gestione dei registri](https://docs.newrelic.com/docs/logs/get-started/get-started-log-management/) e [Introduzione al linguaggio di query New Relic](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/introduction-nrql-new-relics-query-language/) il _Documentazione di New Relic_ sito.
