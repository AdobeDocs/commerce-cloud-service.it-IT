---
title: Ridimensionamento automatico
description: Scopri come Adobe Commerce sull’infrastruttura cloud può essere scalato per soddisfare le richieste di risorse.
feature: Cloud, Auto Scaling
topic: Architecture
exl-id: 2ba49c55-d821-4934-965f-f35bd18ac95f
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# Ridimensionamento automatico

Il ridimensionamento automatico aggiunge o rimuove automaticamente le risorse all’infrastruttura cloud per mantenere prestazioni ottimali e costi ragionevoli. Attualmente, questa funzione è disponibile solo per i progetti configurati con [Architettura scalata](scaled-architecture.md).

## Nodi del server web

Il [livello web](scaled-architecture.md#web-tier) scalabilità per soddisfare un aumento delle richieste di elaborazione e requisiti di traffico più elevati. Attualmente, la funzione di ridimensionamento automatico viene ridimensionata solo orizzontalmente aggiungendo o rimuovendo nodi del server web.

Un evento di ridimensionamento automatico si verifica quando l’utilizzo della CPU e il traffico raggiungono una soglia predefinita:

- **Nodi aggiunti**: CPU/core in tutti i nodi web attivi hanno una capacità del 75% per 1 minuto e il traffico aumenta del 20% per 5 minuti consecutivi.
- **Nodi rimossi**: CPU/core in tutti i nodi web attivi vengono caricati al 60% per 20 minuti. I nodi vengono rimossi nell’ordine in cui sono stati aggiunti.

Le soglie minima e massima sono determinate e impostate in base ai limiti delle risorse contrattuali di ciascun commerciante; ciò riduce il rischio di scalabilità infinita.

## Monitorare le soglie con New Relic

È possibile utilizzare [servizio New Relic](../monitor/new-relic-service.md) per monitorare determinate soglie, ad esempio il numero di host e l&#39;utilizzo della CPU. Le seguenti query New Relic utilizzano una notazione variabile per `cluster-id` ad esempio.

>[!TIP]
>
>Per un riferimento sulla creazione di query, consulta [Sintassi, clausole e funzioni NRQL](https://docs.newrelic.com/docs/query-your-data/nrql-new-relic-query-language/get-started/nrql-syntax-clauses-functions/) nel _New Relic_ documentazione.
>Utilizzare le query per generare un [Dashboard di New Relic](https://docs.newrelic.com/docs/query-your-data/explore-query-data/dashboards/introduction-dashboards/).

### Numero di host

L’esempio di query New Relic che segue mostra il conteggio degli host all’interno dell’ambiente:

```sql
SELECT uniqueCount(SystemSample.entityId) AS 'Infrastructure hosts', uniqueCount(Transaction.host) AS 'APM hosts seen' FROM SystemSample, Transaction where (Transaction.appName = 'cluster-id_stg' AND Transaction.transactionType = 'Web') OR SystemSample.apmApplicationNames LIKE '%|cluster-id_stg|%' TIMESERIES SINCE 3 HOURS AGO
```

Nella schermata seguente: **Host APM visualizzati** si riferisce al numero di host con transazioni registrate durante il periodo selezionato.

![Numero di host New Relic](../../assets/new-relic/host-count.png)

### Utilizzo CPU

L’esempio di query New Relic seguente mostra l’utilizzo della CPU per i nodi web:

```sql
SELECT average(cpuPercent) FROM SystemSample FACET hostname, apmApplicationNames WHERE instanceType LIKE 'c%' TIMESERIES SINCE 3 HOURS AGO
```

![Utilizzo della CPU nei nodi web New Relic](../../assets/new-relic/web-node-cpu-usage.png)

## Abilita ridimensionamento automatico

Per abilitare o disabilitare il ridimensionamento automatico per il progetto di infrastruttura cloud Adobe Commerce on, [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Scegli i seguenti motivi nel ticket:

- **Motivo del contatto**: richiesta di modifica dell’infrastruttura
- **Motivo del contatto per l’infrastruttura Adobe Commerce**: altra richiesta di modifica dell’infrastruttura

>[!IMPORTANT]
>
>La funzione di ridimensionamento automatico acquisisce gli eventi imprevisti. Anche se è stato abilitato il ridimensionamento automatico, l’Adobe consiglia di continuare con [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) se prevedi un evento imminente.

### Test di carico

Adobe abilita il ridimensionamento automatico nel progetto Cloud _staging_ cluster. Dopo aver eseguito e completato il test di carico nell’ambiente, Adobe abilita il ridimensionamento automatico nel cluster di produzione. Per informazioni sul test di carico, consulta [Test delle prestazioni](../launch/checklist.md#performance-testing).

### INSERIRE NELL&#39;ELENCO CONSENTITI IP

Dopo aver abilitato il ridimensionamento automatico, il traffico del nodo web in uscita proviene dagli indirizzi IP dei nodi di servizio. Se utilizzi un elenco Consentiti di con un servizio di terze parti che non è incluso nel pacchetto con il progetto Adobe Commerce on Cloud Infrastructure, verifica gli indirizzi IP nel inserisco nell&#39;elenco Consentiti di  del servizio di terze parti.

Ad esempio:

- Se il inserisco nell&#39;elenco Consentiti di contiene gli indirizzi IP dei nodi di servizio (1, 2 e 3), non è richiesta alcuna azione.
- Se il inserisco nell&#39;elenco Consentiti di contiene gli indirizzi IP per i nodi di servizio (1, 2 e 3) e i nodi web (4, 5 e 6), in questo caso tutti e sei i nodi, non è richiesta alcuna azione.
- Se il inserisco nell&#39;elenco Consentiti di contiene gli indirizzi IP _solo_ per i nodi web (4, 5 e 6), è necessario aggiornare il inserisco nell&#39;elenco Consentiti di per includere gli indirizzi IP dei nodi di servizio.
