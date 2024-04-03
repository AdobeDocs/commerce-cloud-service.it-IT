---
title: Panoramica sulle integrazioni
description: Scopri le opzioni di integrazione di terze parti per il tuo progetto di infrastruttura cloud Adobe Commerce on.
role: Developer
feature: Cloud, Integration
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: 2dddba73-5b88-4b5d-a0e1-2f1c1f52354c
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '564'
ht-degree: 0%

---

# Panoramica sulle integrazioni

Le integrazioni sono utili per utilizzare servizi esterni, come l’hosting Git o i bot di Slack, e per mantenere i processi di sviluppo correnti, come l’utilizzo della funzione di richiesta pull di revisione del codice in GitHub. Puoi aggiungere le seguenti integrazioni al progetto di infrastruttura cloud di Adobe Commerce:

![Integrazioni](/help/assets/integrations.png)

>[!BEGINTABS]

>[!TAB CLI]

**Per aggiungere un’integrazione tramite Cloud CLI**:

Il comando seguente avvia i prompt interattivi per selezionare il tipo e le opzioni per la nuova integrazione.

```bash
magento-cloud integration:add
```

**Per elencare le integrazioni configurate per il progetto**:

```bash
magento-cloud integration:list
```

Risposta di esempio:

```terminal
+----------+--------------+---------------------------------------------------------------------------+
| ID       | Type         | Summary                                                                   |
+----------+--------------+---------------------------------------------------------------------------+
| <int-id> | bitbucket    | Repository: user/magento-int                                              |
|          |              | Hook URL:                                                                 |
|          |              | https://magento-url.cloud/api/projects/projectID/integrations/int-ID/hook |
| <int-id> | health.email | From: you@example.com                                                     |
|          |              | To: them@example.com                                                      |
+----------+--------------+---------------------------------------------------------------------------+
```

>[!TAB Console]

**Per aggiungere un’integrazione utilizzando[!DNL Cloud Console]**:

1. In entrata _Impostazioni progetto_, fai clic su **[!UICONTROL Integrations]**.

1. Fai clic su un tipo di integrazione o fai clic su **[!UICONTROL Add integration]**.

1. Segui i passaggi di selezione e configurazione del tipo di integrazione.

1. Dopo l’aggiunta dell’integrazione, questa viene visualizzata nell’elenco nella vista Integrazioni.

>[!ENDTABS]

## Webhook Commerce

Puoi configurare i webhook Commerce nel progetto Cloud con [Variabile globale ENABLE_WEBHOOKS](../environment/variables-global.md#enable_webhooks). I webhook di Commerce inviano richieste a un server esterno in risposta a eventi generati da Commerce. Il [_Guida ai webhook_](https://developer.adobe.com/commerce/extensibility/webhooks) descrive questa funzione in dettaglio.

## Webhook generici

Puoi acquisire e generare rapporti sull’infrastruttura e sugli eventi dell’archivio Cloud utilizzando un’integrazione di webhook personalizzata per `POST` Messaggi JSON in un _webhook_ URL.

**Per aggiungere un URL del webhook, utilizza la sintassi seguente**:

```bash
magento-cloud integration:add --type=webhook --url=https://hook-url.example.com
```

- `type`- Specifica la `webhook` tipo di integrazione.
- `url`- Fornisce l&#39;URL del webhook che può ricevere messaggi JSON.

La risposta di esempio mostra una serie di prompt che offrono l’opportunità di personalizzare l’integrazione. L’utilizzo della risposta predefinita (vuota) invia messaggi su tutti gli eventi in tutti gli ambienti di un progetto.

Puoi personalizzare l’integrazione in base a specifici rapporti [Eventi](#events-to-report), ad esempio per inviare il codice a un ramo. Ad esempio, puoi specificare `environment.push` evento per inviare un messaggio quando un utente invia il codice a un ramo:

```terminal
Events to report (--events)
A list of events to report, e.g. environment.push
Default: *
Enter comma-separated values (or leave this blank)
>
```

Puoi scegliere di segnalare gli eventi in un `pending`, `in_progress`, o `complete` state:

```terminal
States to report (--states)
A list of states to report, e.g. pending, in_progress, complete
Default: complete
Enter comma-separated values (or leave this blank)
>
```

E tu puoi... _include_ o _escludi_ messaggi per ambienti specifici:

```terminal
Included environments (--environments)
The environment IDs to include
Default: *
Enter comma-separated values (or leave this blank)
>

Excluded environments (--excluded-environments)
The environment IDs to exclude
Enter comma-separated values (or leave this blank)
>
```

Al termine dell’integrazione, riceverai un riepilogo dei valori:

```terminal
Created integration integration-ID (type: webhook)
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - complete                   |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Aggiorna integrazione esistente

Puoi aggiornare un’integrazione esistente. Ad esempio, modificare gli stati da `complete` a `pending` utilizzando quanto segue:

```bash
magento-cloud integration:update --states=pending <int-id>
```

Risposta di esempio:

```terminal
Integration integration-ID (webhook) updated
+-----------------------+------------------------------+
| Property              | Value                        |
+-----------------------+------------------------------+
| id                    | integration-ID               |
| type                  | webhook                      |
| events                | - '*'                        |
| environments          | - '*'                        |
| excluded_environments | {  }                         |
| states                | - pending                    |
| url                   | https://hook-url.example.com |
+-----------------------+------------------------------+
```

### Eventi da segnalare

| Evento | Descrizione |
| ----- | :-----------|
| `environment.access.add` | A un utente è stato concesso l’accesso all’ambiente |
| `environment.access.remove` | Un utente è stato rimosso dall’ambiente |
| `environment.activate` | Un ramo è stato &quot;attivato&quot; con un ambiente |
| `environment.backup` | Un utente ha attivato un’istantanea |
| `environment.branch` | Un ramo è stato creato utilizzando la console di gestione |
| `environment.deactivate` | Ramo &quot;disattivato&quot;. Il codice è ancora presente ma l&#39;ambiente è stato distrutto |
| `environment.delete` | Un ramo è stato eliminato |
| `environment.initialize` | Il `master` ramo del progetto inizializzato con un primo commit |
| `environment.merge` | Un ramo attivo è stato unito utilizzando la console di gestione o l’API |
| `environment.push` | Un utente ha inviato il codice a un ramo |
| `environment.restore` | Un utente ha ripristinato uno snapshot |
| `environment.route.create` | È stata creata una route tramite la console di gestione |
| `environment.route.delete` | Una route è stata eliminata utilizzando la console di gestione |
| `environment.route.update` | Una route è stata modificata utilizzando la console di gestione |
| `environment.subscription.update` | Il `master` L’ambiente è stato ridimensionato perché l’abbonamento è stato modificato, ma non sono presenti modifiche al contenuto |
| `environment.synchronize` | Nell’ambiente padre sono stati copiati dati o codice |
| `environment.update.http_access` | Le regole di accesso HTTP per un ambiente sono state modificate |
| `environment.update.restrict_robots` | La funzione block-all-robots è stata abilitata o disabilitata |
| `environment.update.smtp` | L’invio di e-mail è stato abilitato o disabilitato per un ambiente |
| `environment.variable.create` | È stata creata una variabile |
| `environment.variable.delete` | Una variabile è stata eliminata |
| `environment.variable.update` | Una variabile è stata modificata |
| `project.domain.create` | Un dominio è stato creato e aggiunto al progetto |
| `project.domain.delete` | Un dominio associato al progetto è stato rimosso |
| `project.domain.update` | Un dominio associato al progetto è stato aggiornato |
