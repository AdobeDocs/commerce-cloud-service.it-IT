---
title: Variabili globali
description: Consulta l’elenco delle variabili di ambiente che controllano le azioni nel processo di implementazione di Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Configuration, Build, Deploy, Eventing, Logs, SCD
recommendations: noDisplay, catalog
role: Developer
exl-id: 04c2861d-746d-42d4-a678-f6c7b464eb51
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '752'
ht-degree: 0%

---

# Variabili globali

Le variabili globali controllano le azioni in ogni fase della [!DNL Commerce] processo di distribuzione: generazione, distribuzione e post-distribuzione. Poiché le variabili globali influiscono su ogni fase, è necessario impostarle nella `global` fase del `.magento.env.yaml` file:

```yaml
stage:
  global:
    GLOBAL_VARIABLE_NAME: value
```

Per ulteriori informazioni sulla personalizzazione del processo di compilazione e distribuzione:

- [Configurazione della distribuzione](configure-env-yaml.md)
- [Processo di distribuzione](../deploy/process.md)

## `ENABLE_EVENTING`

- **Predefinito**-_Non impostato_
- **Versione**—Adobe Commerce 2.4.5 e versioni successive

Se impostato su `true`, consente a cron di eseguire i consumer della coda di messaggi. Gli eventi di Adobe I/O per Adobe Commerce utilizzano le code di messaggi per accelerare la consegna di eventi critici.

L’Adobe consiglia di aggiungere anche [`CRON_CONSUMERS_RUNNER`](./variables-deploy.md#cron_consumers_runner) variabile a `deploy` fase del `.magento.env.yaml` file con `cron_run` imposta su `true`.

L’esempio seguente mostra una configurazione completa `ENABLE_EVENTING` variabile.

```yaml
stage:
  global:
    ENABLE_EVENTING: true
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      max_messages: 0
      consumers: []
```

## ENABLE_WEBHOOKS

- **Predefinito**-_Non impostato_
- **Versione**—Adobe Commerce 2.4.4 e versioni successive

Se impostato su `true`, abilita i webhook di Commerce. Il webhook viene eseguito su un endpoint esterno, ad esempio un’azione di runtime di App Builder o un sistema di gestione dell’inventario di terze parti. Il [_Guida ai webhook_](https://developer.adobe.com/commerce/extensibility/webhooks) descrive questa funzione in dettaglio.

```yaml
stage:
  global:
    ENABLE_WEBHOOKS: true
```

## `MIN_LOGGING_LEVEL`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Ignora il livello minimo di registrazione per tutti i flussi di output senza modificare il codice, il che è utile per la risoluzione dei problemi di distribuzione. Ad esempio, se la distribuzione non riesce, puoi utilizzare questa variabile per aumentare la granularità della registrazione a livello globale. Consulta [Livelli di registro](log-handlers.md#log-levels). Il `min_level` Il valore nei gestori di registrazione sovrascrive questa impostazione.

```yaml
stage:
  global:
    MIN_LOGGING_LEVEL: debug
```

>[!WARNING]
>
>L&#39;impostazione per `MIN_LOGGING_LEVEL` la variabile non modifica la configurazione a livello di registro per il gestore di file, che è impostato su `debug` per impostazione predefinita.

## `SCD_ON_DEMAND`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Abilita la generazione di contenuto statico quando richiesto da un utente (SCD). I contenuti statici on-demand sono ideali per il flusso di lavoro di sviluppo e test, in quanto riducono i tempi di implementazione.

Precaricamento della cache utilizzando [`post_deploy` gancio](../application/hooks-property.md) riduce i tempi di inattività del sito. Il riscaldamento della cache è disponibile solo per i progetti Pro che contengono ambienti di staging e produzione nel [!DNL Cloud Console] e per progetti iniziali. Aggiungi il `SCD_ON_DEMAND` variabile di ambiente al `global` fase nel `.magento.env.yaml` file:

```yaml
stage:
  global:
    SCD_ON_DEMAND: true
```

Il `SCD_ON_DEMAND` ignora l&#39;SCD in entrambe le fasi (generazione e distribuzione), cancella `pub/static` e `var/view_preprocessed` e scrive quanto segue in `app/etc/env.php` file:

```php?start_inline=1
return array(
   ...
   'static_content_on_demand_in_production' => 1,
   ...
);
```

## `SCD_MAX_EXECUTION_TIME`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Consente di aumentare il tempo massimo di esecuzione previsto per la distribuzione del contenuto statico.

Per impostazione predefinita, Adobe Commerce imposta l’esecuzione massima prevista su 900 secondi, ma in alcuni scenari potrebbe essere necessario più tempo per completare la distribuzione di contenuto statico per un progetto Cloud.

```yaml
stage:
  global:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_NO_PARENT`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.4.2 e versioni successive

Imposta su `true` per evitare la generazione di contenuto statico per i temi principali durante le fasi di build e distribuzione. Quando questa opzione è impostata su `true`, vengono generati meno contenuti statici, il che migliora i tempi complessivi di generazione e implementazione.

```yaml
stage:
  global:
    SCD_NO_PARENT: true
```

## `SCD_USE_BALER`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.3.0 e versioni successive

[Baler](https://github.com/magento/baler) è un modulo che analizza il codice JavaScript generato e crea un bundle JavaScript ottimizzato. L’implementazione del bundle ottimizzato sul sito può ridurre il numero di richieste di rete durante il caricamento del sito e migliorare i tempi di caricamento delle pagine.

Imposta su `true` per eseguire Baler dopo l’esecuzione della distribuzione del contenuto statico.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Installare e configurare il modulo Baler prima di utilizzare questa funzione. Poiché Baler è in fase di rilascio alfa, abilita questa opzione solo negli ambienti di staging.

## `SKIP_HTML_MINIFICATION`

- **Predefinito**:
   - `true`—per `ece-tools` 2002.0.13 e versioni successive
   - `false`- per le versioni precedenti di `ece-tools`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Abilita o disabilita la copia dei file di visualizzazione statica in `<magento_root>/init/` alla fine della fase di build. Se impostato su `true`, i file non vengono copiati e la minimizzazione di HTML è disponibile su richiesta. Imposta questo valore su `true` per ridurre i tempi di inattività durante l’implementazione in ambienti di staging e produzione.

- **`false`**- Copia il `view_preprocessed` alla directory `<magento_root>/init/` alla fine della fase di build e ripristina la directory nella `<magento_root>/var` all’inizio della fase di distribuzione.
- **`true`**- Consente la minimizzazione dei HTML su richiesta; consente _non_ copia `<magento_root>var/view_preprocessed` al `<magento_root>/init/` alla fine della fase di build.

Aggiungi il `SKIP_HTML_MINIFICATION` variabile di ambiente al `global` fase nel `.magento.env.yaml` file:

```yaml
stage:
  global:
    SKIP_HTML_MINIFICATION: true
```

## `X_FRAME_CONFIGURATION`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Utilizza il `X_FRAME_CONFIGURATION` variabile per modificare la [`X-Frame-Options`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/security/xframe-options.html) configurazione dell’intestazione per il sito Adobe Commerce. Questa configurazione controlla come il browser esegue il rendering di una pagina in una `<frame>`, `<iframe>`, o `<object>`. Utilizza una delle seguenti opzioni:

- `DENY`- La pagina non può essere visualizzata in un frame.
- `SAMEORIGIN`— (Impostazione predefinita di Adobe Commerce). La pagina può essere visualizzata solo in un frame sulla stessa origine della pagina stessa.

>[!WARNING]
>
>Il `ALLOW-FROM <uri>` L’opzione è stata rimossa perché i browser supportati da Adobe Commerce non la supportano più. Consulta [Compatibilità browser](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility).

Aggiungi il `X_FRAME_CONFIGURATION` variabile di ambiente al `global` fase nel `.magento.env.yaml` file:

```yaml
stage:
  global:
    X_FRAME_CONFIGURATION: SAMEORIGIN
```
