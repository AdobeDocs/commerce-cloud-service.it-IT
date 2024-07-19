---
title: Procedure guidate intelligenti
description: Scopri come utilizzare le procedure guidate intelligenti per valutare se il progetto Adobe Commerce su infrastruttura cloud segue le best practice di distribuzione.
feature: Cloud, Build, Deploy, SCD
exl-id: eb79431c-8835-4ae4-b453-9c4932c5d5ac
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
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
| `wizard:ideal-state` | Verificare che SCD si trovi nella fase _build_, che la variabile `SKIP_HTML_MINIFICATION` sia `true` e che l&#39;hook post_deploy sia configurato nell&#39;ambiente cloud. Da non utilizzare nell’ambiente di sviluppo locale. |
| `wizard:master-slave` | Verificare che la variabile `REDIS_USE_SLAVE_CONNECTION` e la variabile `MYSQL_USE_SLAVE_CONNECTION` siano `true`. |
| `wizard:scd-on-demand` | Verificare che la variabile di ambiente globale `SCD_ON_DEMAND` sia `true`. |
| `wizard:scd-on-build` | Verificare che la variabile di ambiente globale `SCD_ON_DEMAND` sia `false` e la variabile di ambiente `SKIP_SCD` sia `false` per la fase _build_. Verifica che il file `config.php` contenga informazioni per archivi, gruppi di archivi e siti Web. |
| `wizard:scd-on-deploy` | Verificare che la variabile di ambiente globale `SCD_ON_DEMAND` sia `false` e la variabile di ambiente `SKIP_SCD` sia `false` per la fase _deploy_. Verifica che il file `config.php` non contenga _NOT_ l&#39;elenco di store, gruppi di store e siti Web con le relative informazioni. |

Ad esempio, puoi verificare che la configurazione attivi correttamente la funzione SCD on-demand:

```bash
./vendor/bin/ece-tools wizard:scd-on-demand
```

Una configurazione corretta restituisce:

```
SCD on-demand is enabled
```

Una configurazione non riuscita restituisce:

```
SCD on-demand is disabled
```

## Verifica di una configurazione ideale

La configurazione _ideale_ per il progetto Cloud consente di ridurre al minimo i tempi di inattività della distribuzione riscaldando la cache e generando contenuto statico quando richiesto dall&#39;utente. Questa procedura guidata viene eseguita automaticamente durante il processo di distribuzione. Se il cloud non è configurato per questo _stato ideale_, riceverai un messaggio simile al seguente:

```
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

   ```
   Ideal state is configured
   ```
