---
title: Acquisizione dei dati
description: Scopri come visualizzare e gestire l’acquisizione dei dati di Commerce in New Relic.
feature: Cloud, Observability
exl-id: f88bf20c-604b-4986-b71c-bb726b2f00b8
source-git-commit: bf3debc5986d51a721537b52ffced58b2ee521ea
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Acquisizione dei dati

New Relic dipende dai dati avanzati per fornire un monitoraggio e un&#39;analisi efficaci, ma i set di dati di grandi dimensioni possono influire su risultati, prestazioni e conformità tempestivi. Questo argomento fornisce alcune indicazioni sulla gestione dell’acquisizione dei dati e sulle strategie per perfezionare i dati in modo che siano più efficaci.

New Relic offre _Gestione dei dati_ visualizzazione che riepiloga l&#39;utilizzo del piano per origine dati.

**Per visualizzare i dati e le origini di acquisizione**:

1. Dal menu utente di New Relic, fai clic su **[!UICONTROL Manage your data]**.
1. Clic **[!UICONTROL Data management]** nel _Amministrazione_ elenco.

   ![Gestione dei dati](../../assets/new-relic/data-ingestion.png)

   Il **[!UICONTROL Data ingestion]** Questa scheda mostra i dati acquisiti per il giorno e la relativa origine.
La scheda Conservazione dei dati mostra e controlla per quanto tempo vengono memorizzati i dati.

1. Seleziona la **[!UICONTROL Limits]** e visualizzare i limiti per il tuo account.

Le origini dati per Adobe Commerce includono:

- **Eventi APM**- dati evento utilizzati in grafici e dashboard
- **Infrastruttura** metriche di processo e host, ad esempio CPU, storage, reti
- **Registrazione**: registri per CDN, APM e application server

I dati di registro contribuiscono in larga misura all’acquisizione. Scopri come [Visualizzare e analizzare i dati di registro](log-management.md#view-and-analyze-log-data) e collabora con il tuo rappresentante di Adobe per definire una strategia per le esigenze di acquisizione e conservazione dei dati. Ulteriori informazioni su [gestione dell’acquisizione dei dati](https://docs.newrelic.com/docs/data-apis/manage-data/manage-data-coming-new-relic/) nel _Documentazione di New Relic_.
