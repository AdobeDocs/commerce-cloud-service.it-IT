---
title: Blocco protettivo
description: Scopri la funzione di blocco protettivo di Adobe Commerce sull’infrastruttura cloud e come funziona per proteggere il tuo sito da vulnerabilità di sicurezza note.
feature: Cloud, Configuration, Security
topic: Security
exl-id: bc1def41-9521-4005-872e-9ecaab1d4d16
source-git-commit: 7b9c6a4cd17069c25455195bd8f273664b8a29eb
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# Blocco protettivo

Adobe Commerce sull’infrastruttura cloud dispone di una funzione di blocco protettiva che, in determinate circostanze, limita l’accesso ai siti web con vulnerabilità di sicurezza. Questo metodo di blocco parziale impedisce lo sfruttamento delle vulnerabilità di sicurezza note. Il software obsoleto spesso contiene exploit, quindi è importante proteggerlo da questi exploit bloccando parzialmente l&#39;accesso a questi siti.

## Come funziona il blocco protettivo

Adobe Commerce gestisce un database di firme di vulnerabilità di sicurezza note nel software open-source, comunemente implementate nell’infrastruttura cloud. Il controllo di sicurezza analizza solo le vulnerabilità note nei progetti open-source; non può esaminare le personalizzazioni scritte.

Esecuzione analisi sicurezza:

- Quando invii un nuovo codice a Git
- Quando vengono aggiunte nuove vulnerabilità al database

Se nell’applicazione viene rilevata una vulnerabilità critica, questa rifiuta il push Git.

Esistono due tipi di blocchi che vengono eseguiti:

1. **Blocco completo**—per siti Web di sviluppo. Il messaggio di errore che accompagna `git push` fornisce informazioni dettagliate sulla vulnerabilità.

1. **Blocco parziale**—per i siti Web di produzione, che consente al sito di rimanere principalmente online. A seconda della natura della vulnerabilità, parti di una richiesta, come una stringa di query, cookie o eventuali intestazioni aggiuntive, potrebbero essere rimosse dalle richieste di GET. Tutte le altre richieste possono essere bloccate completamente, ad esempio l’accesso, l’invio di moduli o l’estrazione di prodotti.

Lo sblocco viene automatizzato dopo la risoluzione del rischio di sicurezza. Il blocco viene rimosso subito dopo l’applicazione di un aggiornamento della sicurezza che rimuove la vulnerabilità.

## Rinuncia al blocco di protezione

Il blocco protettivo serve a proteggerti da vulnerabilità note nel software che distribuisci Adobe Commerce sull’infrastruttura cloud.

Tuttavia, è possibile rinunciare al blocco di protezione aggiungendo quanto segue a [`.magento.app.yaml`](../application/configure-app-yaml.md):

```yaml
   preflight:
      enabled: false
```

Puoi rinunciare esplicitamente a un controllo specifico, ad esempio:

```yaml
   preflight:
      enabled: true
      ignore_rules: [ "drupal:SA-CORE-2014-005" ]
```
