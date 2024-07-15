---
title: Notifiche stato
description: Scopri come configurare le notifiche di Slack, e-mail e PagerDuty per l’utilizzo dello spazio su disco nel progetto di infrastruttura cloud Adobe Commerce.
feature: Cloud, Observability, Integration
exl-id: d3173098-78ed-42a8-aeb3-9ccbaccc4d32
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '312'
ht-degree: 0%

---

# Notifiche stato

Adobe Commerce su infrastruttura cloud monitora l’utilizzo dello spazio su disco in tutte le applicazioni e i servizi nell’ambiente Starter o nell’ambiente di integrazione Pro. Un disco di database che esaurisce lo spazio disponibile potrebbe danneggiare i dati. Il controllo dello stato di integrità si verifica ogni 5 minuti e può inviare una notifica tramite e-mail o altro servizio esterno. Sono disponibili tre avvisi per le notifiche di stato su disco in esaurimento:

- **Avviso**: lo spazio su disco disponibile scende al di sotto del 20%
- **Critico**: lo spazio disponibile su disco scende al di sotto del 10%
- **Tutto chiaro**: lo spazio disponibile su disco restituisce più del 20% dopo un evento di disco insufficiente

>[!NOTE]
>
>Negli ambienti di produzione Pro, è possibile utilizzare gli avvisi gestiti per i criteri di avviso di Adobe Commerce per New Relic per monitorare lo spazio su disco. Vedere [Monitorare le prestazioni con avvisi gestiti](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts).

## Notifiche e-mail

L’integrazione e-mail per l’integrità richiede un indirizzo di origine e almeno un indirizzo del destinatario. È possibile utilizzare lo stesso indirizzo di posta elettronica per `from-address` e l&#39;indirizzo `recipients`. L’esempio seguente registra un’integrazione e-mail di stato con due destinatari:

```bash
magento-cloud integration:add --type health.email --from-address you@example.com --recipients them@example.com --recipients others@example.com
```

## Notifiche del canale di Slack

Slack è un servizio esterno che utilizza app interattive denominate bot per pubblicare messaggi in una chat room. Prima di poter ricevere le notifiche di stato in Slack, è necessario creare un [utente bot](https://api.slack.com/bot-users) personalizzato per il proprio Slack. Dopo aver configurato l&#39;utente bot per il canale o i canali, salva il [token bot](https://api.slack.com/docs/token-types#bot) fornito da Slack per registrare l&#39;integrazione. Nell&#39;esempio seguente le notifiche di stato vengono registrate in un canale di Slack:

```bash
magento-cloud integration:add --type health.slack --token SLACK_BOT_TOKEN --channel '#slack-channel-name'
```

## Notifiche di PagerDuty

PagerDuty è un servizio esterno in grado di notificare ai membri del gruppo di chiamata problemi importanti. Prima di poter ricevere le notifiche di integrità in PagerDuty, è necessario creare un&#39;integrazione [PagerDuty](https://developer.pagerduty.com/v2/docs/integrating) che utilizza la versione 2 dell&#39;API degli eventi. Utilizza la chiave di integrazione o _chiave di routing_ per registrare l&#39;integrazione. Nell&#39;esempio seguente vengono registrate le notifiche per PagerDuty utilizzando una chiave di routing:

```bash
magento-cloud integration:add --type health.pagerduty --routing-key PAGERDUTY_ROUTING_KEY
```
