---
title: Impostare le notifiche
description: Scopri come configurare le notifiche per Adobe Commerce sugli ambienti dell’infrastruttura cloud.
feature: Cloud, Configuration, Logs
exl-id: be48abcd-4753-4e89-83fe-0aadfb0415c2
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 0%

---

# Impostare le notifiche

Per impostazione predefinita, Adobe Commerce su infrastruttura cloud scrive le azioni di compilazione e distribuzione nel file `app/var/log/cloud.log` all&#39;interno della directory dell&#39;applicazione principale Adobe Commerce. In alternativa, puoi inviare i registri a un sistema di messaggistica, ad esempio Slack e e-mail, per ricevere notifiche in tempo reale.

È possibile, ad esempio, inviare un messaggio di Slack per avvisare un gruppo di persone quando una distribuzione non riesce e avviare un&#39;indagine su ciò che è andato storto.

## Notifiche del piano

Prima di configurare le notifiche, considera quanto segue:

- Che tipo di notifiche desideri ricevere (messaggi di Slack, e-mail, entrambi)?
- Quanti dettagli vuoi visualizzare nei registri?
- Dove desideri impostare le notifiche (Integrazione, Staging, Produzione)?

Ad esempio, durante lo sviluppo iniziale è possibile preferire notifiche e-mail che mostrano registri dettagliati sull’ambiente di integrazione per aiutarti a eseguire il debug dei problemi prima di distribuirli nell’ambiente di staging. Quando sei pronto per la distribuzione nell’ambiente di staging o produzione, potresti preferire un messaggio di Slack che contenga informazioni meno dettagliate.

>[!NOTE]
>
>Il file di configurazione utilizzato per impostare le notifiche si trova nella directory principale del progetto, quindi viene applicato quando invii le modifiche a qualsiasi ambiente. Se desideri personalizzare le notifiche per ambiente, devi modificare il file di configurazione prima di inviarlo a tale ambiente.

## Configurare le notifiche

Per configurare le notifiche:

1. Sulla workstation locale, passa alla directory del progetto.
1. Nel file `.magento.env.yaml` della directory principale del progetto, aggiungi le impostazioni del sistema di messaggistica, inclusa la notifica preferita [Livelli di registro](log-handlers.md#log-levels).

   Per configurare ad esempio entrambe le configurazioni e-mail di Slack _e_, utilizzare la seguente procedura:

   ```yaml
   log:
     slack:
       token: "<your-slack-token>"
       channel: "<your-slack-channel>"
       username: "SlackHandler"
       min_level: "info"
     email:
       to: <your-email>
       from: <your-email>
       subject: "Log notification from Adobe Commerce"
       min_level: "notice"
   ```

   >[!NOTE]
   >
   >Adobe Commerce su infrastruttura cloud invia e-mail solo durante la fase di distribuzione.

1. Eseguire il commit e inviare le modifiche al server remoto.

   ```bash
   git -A && git commit -m "Configure build/deploy notifications"
   ```

   ```bash
   git push origin <branch-name>
   ```

### Esempio di configurazione dello Slack

Nell&#39;esempio seguente viene illustrata una configurazione di solo Slack:

```yaml
log:
  slack:
    token: "<your-slack-token>"
    channel: "<your-slack-channel>"
    username: "SlackHandler"
    min_level: "info"
```

- `token` - Il [token utente](https://api.slack.com/docs/token-types#user) dello Slack. Il token utente autorizza Adobe Commerce su un’infrastruttura cloud a inviare messaggi.
- `channel` - Il nome del canale di Slack Adobe Commerce sull&#39;infrastruttura cloud invia le notifiche.
- `username` - Nome utente utilizzato da Adobe Commerce sull&#39;infrastruttura cloud per inviare messaggi di notifica in Slack.
- `min_level` - Livello di registro minimo per i messaggi di notifica. È consigliabile utilizzare `info`.

### Esempio di configurazione e-mail

L’esempio seguente mostra una configurazione solo e-mail:

>[!NOTE]
>
>Adobe Commerce su infrastruttura cloud invia e-mail solo durante la fase di distribuzione.

```yaml
log:
  email:
    to: <your-email>
    from: <your-email>
    subject: "Log notification from Adobe Commerce"
    min_level: "notice"
```

- `to` - L&#39;indirizzo e-mail Adobe Commerce sull&#39;infrastruttura cloud invia messaggi di notifica.
- `from` - Indirizzo e-mail per l&#39;invio dei messaggi di notifica ai destinatari.
- `subject` - Descrizione dell&#39;e-mail.
- `min_level` - Livello di registro minimo per i messaggi di notifica. È consigliabile utilizzare `notice` o `warning`.
