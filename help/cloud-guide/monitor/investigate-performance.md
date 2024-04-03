---
title: Monitoraggio New Relic
description: Scopri come accedere al dashboard di New Relic e analizzare i dati del progetto di infrastruttura cloud di Adobe Commerce.
feature: Cloud, Observability
topic: Performance
exl-id: 8d1e2ad6-14d0-4754-9703-960d93e135a9
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# Monitoraggio New Relic

New Relic collega e monitora l&#39;infrastruttura e [!DNL Commerce] mediante agenti PHP. Dopo che un ambiente Cloud si connette a New Relic, puoi accedere al tuo account New Relic per rivedere i dati raccolti dall’agente.

Il giorno _APM e servizi_ , seleziona la **Riepilogo** per visualizzare informazioni sulle transazioni relative all&#39;applicazione. Questa visualizzazione consente di identificare potenziali errori e di verificare lo stato complessivo dell’applicazione e dei servizi.

![Panoramica di Cloud Project New Relic](../../assets/new-relic/dashboard.png)

Da questa vista è possibile tenere traccia delle transazioni che incontrano risposte lente o colli di bottiglia, velocità effettiva delle applicazioni, errori Web e altro ancora.

Revisione dei dati tracciati:

- **Più dispendioso in termini di tempo**- Determina il consumo di tempo tenendo traccia delle richieste in parallelo. Ad esempio, potresti avere il tempo di transazione più alto trascorso nelle visualizzazioni di prodotti e categorie. Se una pagina dell’account del cliente si classifica improvvisamente in un consumo elevato di tempo, l’applicazione potrebbe essere interessata da una chiamata o da prestazioni di trascinamento delle query.

- **Throughput massimo**- Identifica le pagine più visitate in base alle dimensioni e alla frequenza dei byte trasmessi.

Tutti i dati raccolti descrivono il tempo impiegato per le azioni di trasmissione di dati, query o _Redis_ dati. Se le query causano problemi, New Relic fornisce informazioni per tenere traccia di tali problemi e rispondervi.

>[!TIP]
>
>Per informazioni dettagliate sull&#39;utilizzo di questi dati per la risoluzione dei problemi relativi alle prestazioni delle applicazioni, vedere [Risolvere i problemi relativi alle prestazioni con New Relic](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/troubleshoot-performance-using-new-relic-on-magento-commerce.html) nel _Centro assistenza Adobe Commerce_.

## Monitorare le prestazioni con avvisi gestiti

L&#39;Adobe fornisce _Avvisi gestiti per Adobe Commerce_ criterio di avviso per tenere traccia delle metriche delle prestazioni. La policy include una raccolta di avvisi che impostano soglie e attivano avvisi e notifiche critiche quando problemi di infrastruttura o applicazioni influiscono sulle prestazioni del sito. Il criterio tiene traccia delle metriche seguenti negli ambienti di produzione:

| Metrica | Raccolta dati | Disponibilità |
|:-------------------|:----------------|:----------------|
| [!DNL Apdex] punteggio | APM | Pro e Starter |
| Utilizzo CPU | NRI | Pro |
| Spazio su disco | NRI | Pro |
| Percentuale di errori | APM | Pro e Starter |
| Utilizzo della memoria | NRI | Pro |
| Caricamento query MariaDB | NRI | Pro |
| Memoria Redis | NRI | Pro |

Quando l’infrastruttura del sito o le condizioni dell’applicazione attivano una soglia di avviso, New Relic invia notifiche di avviso in modo da poter affrontare il problema in modo proattivo. Consulta [Avvisi gestiti per Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html) nel _Centro assistenza Adobe Commerce_ per informazioni dettagliate sulle soglie di avviso e sui passaggi di risoluzione dei problemi che hanno attivato l’avviso.

>[!TIP]
>
>Per gli ambienti di staging e integrazione Pro e per gli ambienti Starter, utilizzare [Notifiche stato](../integrations/health-notifications.md) per monitorare lo spazio su disco.

>[!PREREQUISITES]
>
>- **Credenziali New Relic**: credenziali per accedere all’account New Relic per il progetto Cloud
>- **Integrazione Active New Relic**- Verificare che l&#39;ambiente Cloud sia connesso a New Relic
>- **Notifica flusso di lavoro**- Configura almeno un elemento [workflow](#set-up-a-workflow-for-notifications) per ricevere le notifiche di avviso

**Per rivedere il criterio Avvisi gestiti per Adobe Commerce**:

1. Accedi al tuo [Account New Relic](https://login.newrelic.com/login).

1. Individua il _Avvisi gestiti per Adobe Commerce_ criterio:

   - Nel menu di navigazione di Explorer, fai clic su **[!UICONTROL Alerts & AI]**.

   - Sotto _Rileva_, fai clic su **[!UICONTROL Alert Conditions & Policies]**.

   - Verifica che il tuo account sia selezionato nella parte superiore della sezione _Criteri e condizioni degli avvisi_ visualizzazione.

   - In _Policy_ elenco, seleziona **Avvisi gestiti per Adobe Commerce** policy.

     ![Criteri di avviso generati](../../assets/new-relic/managed-alerts-policy.png)

     >[!NOTE]
     >
     >Se il _Avvisi gestiti per Adobe Commerce_ il criterio non è disponibile, vedi [Avvisi gestiti per Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-for-magento-commerce.html) nel _Centro assistenza Adobe Commerce_.

1. Fai clic su **[!UICONTROL Alert conditions]** per rivedere le condizioni di avviso definite nel criterio.

## Creare criteri di avviso

Non modificare gli avvisi inclusi nel criterio Avvisi gestiti per Adobe Commerce. Adobe aggiorna e migliora nel tempo le condizioni di avviso in questo criterio, sovrascrivendo tutte le personalizzazioni aggiunte al criterio.

Anziché modificare un avviso esistente, è possibile creare un criterio di avviso. Quindi, copia le condizioni di avviso nel nuovo criterio. Consulta [Aggiorna criteri o condizioni](https://docs.newrelic.com/docs/alerts-applied-intelligence/new-relic-alerts/alert-policies/update-or-disable-policies-conditions/) nel _New Relic_ documentazione.

>[!TIP]
>
>Consulta [Introduzione agli avvisi](https://docs.newrelic.com/docs/alerts-applied-intelligence/new-relic-alerts/learn-alerts/alerts-concepts-workflow/) nel _New Relic_ documentazione di per informazioni più dettagliate su avvisi, criteri per gli avvisi e flussi di lavoro.

## Configurare un flusso di lavoro per le notifiche

Ora puoi impostare una _workflow_, precedentemente denominato canale di notifica, per ricevere notifiche sulle prestazioni del sito in base ai dati filtrati, ad esempio i criteri di avviso. Le notifiche sui problemi relativi alle prestazioni vengono inviate a tutti i flussi di lavoro associati a un criterio di avviso quando le condizioni dell&#39;applicazione o dell&#39;infrastruttura attivano un avviso. Ricevi inoltre notifiche quando un problema viene riconosciuto e chiuso.

New Relic fornisce modelli per configurare diversi tipi di notifiche del flusso di lavoro, tra cui e-mail, Slack, ServizioPager, webhook e altro ancora.

**Per configurare un flusso di lavoro**:

1. Accedi al tuo [Account New Relic](https://login.newrelic.com/login).

1. Crea un flusso di lavoro.

   - Nel menu di navigazione di Explorer, fai clic su **[!UICONTROL Alerts & AI]**.

   - Nella barra di navigazione a sinistra sotto _Arricchisci e notifica_, fai clic su **[!UICONTROL Workflows]**.

   - Clic **[!UICONTROL Add a workflow]** sul lato destro.

     ![New Relic aggiungi un flusso di lavoro](../../assets/new-relic/add-a-workflow.png)

   - Il giorno _Configurare il flusso di lavoro_ , immettere un nome per il workflow.

   - In _Filtrare i dati_ sezione, seleziona **[!UICONTROL Managed Alerts for Adobe Commerce]** dal **[!UICONTROL Policy]** elenco a discesa.

   - In _Notifica_ , selezionare un canale e seguire le istruzioni.

   - Clic **[!UICONTROL Test workflow]** per verificare la configurazione.

1. Clic **[!UICONTROL Activate workflow]**.

Consulta la documentazione di New Relic su [Flussi di lavoro](https://docs.newrelic.com/docs/alerts-applied-intelligence/applied-intelligence/incident-workflows/incident-workflows/).

>[!WARNING]
>
>Gli avvisi nel criterio Avvisi gestiti per Adobe Commerce dispongono di flussi di lavoro predefiniti configurati per inviare notifiche ai team di Adobi che supportano Adobe Commerce sui clienti dell’infrastruttura cloud. Non modificare la configurazione di questi canali predefiniti e non rimuovere i criteri di avviso ad essi assegnati.
