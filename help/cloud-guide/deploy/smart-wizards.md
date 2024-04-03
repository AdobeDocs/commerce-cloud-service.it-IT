---
title: Procedure guidate intelligenti
description: Scopri come utilizzare le procedure guidate intelligenti per valutare se il progetto Adobe Commerce su infrastruttura cloud segue le best practice di distribuzione.
feature: Cloud, Build, Deploy, SCD
exl-id: eb79431c-8835-4ae4-b453-9c4932c5d5ac
source-git-commit: 225fba1acfd8b3ce4d7ce989c7851e7b0b218680
workflow-type: tm+mt
source-wordcount: '322'
ht-degree: 0%

---

# Procedure guidate intelligenti

Le procedure guidate intelligenti possono aiutarti a determinare se la configurazione Cloud segue le best practice. Le procedure guidate disponibili supportano le seguenti configurazioni:

- Stato ideale per ridurre al minimo i tempi di inattività dell&#39;installazione
- Configurazione del bilanciamento del carico per database e Redis
- Distribuzione di contenuti statici (SCD) per on-demand, la fase di build o la fase di distribuzione

Ciascuno dei comandi della procedura guidata avanzata fornisce una risposta di verifica e, se applicabile, un consiglio per la corretta configurazione.

| Comando | Descrizione |
| ------- | ------------|
| `wizard:ideal-state` | Verificare che SCD sia sul _build_ fase, la `SKIP_HTML_MINIFICATION` la variabile è `true`e l’hook post_deploy configurato nell’ambiente Cloud. Da non utilizzare nell’ambiente di sviluppo locale. |
| `wizard:master-slave` | Verifica che la `REDIS_USE_SLAVE_CONNECTION` variabile e `MYSQL_USE_SLAVE_CONNECTION` la variabile è `true`. |
| `wizard:scd-on-demand` | Verifica che la `SCD_ON_DEMAND` la variabile di ambiente globale è `true`. |
| `wizard:scd-on-build` | Verifica che la `SCD_ON_DEMAND` la variabile di ambiente globale è `false` e `SKIP_SCD` la variabile di ambiente è `false` per _build_ fase. Verifica che `config.php` Il file contiene informazioni per archivi, gruppi di archivi e siti Web. |
| `wizard:scd-on-deploy` | Verifica che la `SCD_ON_DEMAND` la variabile di ambiente globale è `false` e `SKIP_SCD` la variabile di ambiente è `false` per _distribuire_ fase. Verifica che `config.php` il file _NOT_ contiene l&#39;elenco dei negozi, dei gruppi di negozi e dei siti Web con le relative informazioni. |

Ad esempio, puoi verificare che la configurazione attivi correttamente la funzione SCD on-demand:

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

Una configurazione corretta restituisce:

```terminal
SCD on-demand is enabled
```

Una configurazione non riuscita restituisce:

```terminal
SCD on-demand is disabled
```

## Verifica di una configurazione ideale

Il _ideale_ La configurazione per il progetto Cloud aiuta a ridurre al minimo i tempi di inattività dell’implementazione riscaldando la cache e generando contenuto statico quando richiesto dall’utente. Questa procedura guidata viene eseguita automaticamente durante il processo di distribuzione. Se il cloud non è configurato per questo _stato ideale_, quindi ricevi un messaggio simile al seguente:

```terminal
- SCD on build is not configured
- Post-deploy hook is not configured
- Skip HTML minification is disabled

Ideal state is not configured
```

In base all’output, devi apportare le seguenti correzioni alla configurazione:

1. Abilita la variabile di minimizzazione Skip HTML.

   > .magento.env.yaml

   ```yaml
   stage:
     global:
       SKIP_HTML_MINIFICATION: true
   ```

1. Configura l’hook post-distribuzione.

   > .magento.app.yaml

   ```yaml
       post_deploy: |
           php ./vendor/bin/ece-tools post-deploy
   ```

1. Invia le modifiche al codice ed esegui di nuovo il test. Quando la configurazione è _ideale_, viene visualizzato il seguente messaggio.

   ```terminal
   Ideal state is configured
   ```
