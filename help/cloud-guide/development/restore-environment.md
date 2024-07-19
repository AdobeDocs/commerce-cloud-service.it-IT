---
title: Ripristinare un ambiente
description: Scopri come disinstallare l’applicazione Adobe Commerce da un progetto di infrastruttura cloud e ripristinare un ambiente stabile.
role: Developer
topic: Development
exl-id: b76bd6c3-986e-4adc-abd0-5b27db0d8a3b
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 0%

---

# Ripristinare un ambiente

Se si verificano problemi nell&#39;ambiente di integrazione e non si dispone di un [backup valido](../storage/snapshots.md), provare a ripristinare l&#39;ambiente utilizzando uno dei metodi seguenti:

- Ripristina o ripristina il codice nel ramo Git
- Disinstalla l&#39;applicazione [!DNL Commerce]
- Forza una ridistribuzione
- Reimpostare manualmente il database

{{stuck-deployment-tip}}

## Reimposta il ramo Git

Se si ripristina il ramo Git, il codice torna a uno stato stabile nel passato.

**Per reimpostare il ramo**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Rivedi la cronologia del commit Git. Utilizza `--oneline` per mostrare i commit abbreviati su una riga:

   ```bash
   git log --oneline
   ```

   Risposta di esempio:

   ```
   6bf9f45 (HEAD -> master, magento/master, magento/develop, magento/HEAD, develop) Create composer.lock
   34d7434 2.4.6 upgrade
   b69803c Update composer.lock
   c1bca24 Add sample data
   ec604c3 Update magento/ece-tools
   ...
   ```

1. Scegli un hash di commit che rappresenti l&#39;ultimo stato stabile noto del codice.

   Per ripristinare lo stato inizializzato originale del ramo, individua il primo commit che ha creato il ramo. È possibile utilizzare `--reverse` per visualizzare la cronologia in ordine cronologico inverso.

1. Utilizza l’opzione di ripristino rigido per ripristinare il ramo. Prestare attenzione quando si utilizza questo comando, in quanto vengono ignorate tutte le modifiche dal commit scelto.

   ```bash
   git reset --hard <commit>
   ```

1. Effettua il push delle modifiche per attivare una redistribuzione, che reinstalla Adobe Commerce.

   ```bash
   git push --force <origin> <branch>
   ```

## Disinstallare Commerce

La disinstallazione dell&#39;applicazione [!DNL Commerce] ripristina lo stato originale dell&#39;ambiente ripristinando il database, rimuovendo la configurazione della distribuzione e cancellando le sottodirectory `var/`. Questa guida ripristina anche uno stato stabile precedente per il ramo Git. Se non disponi di un backup recente, ma puoi accedere all’ambiente remoto utilizzando SSH, segui la procedura seguente per ripristinare l’ambiente:

- Disattiva la gestione della configurazione
- Disinstallare Adobe Commerce
- Reimposta il ramo Git

La disinstallazione del software Adobe Commerce provoca l&#39;eliminazione e il ripristino del database, rimuove la configurazione di distribuzione e cancella le sottodirectory `var/`. È importante disabilitare [Gestione configurazione](../store/store-settings.md) in modo che non applichi automaticamente le impostazioni di configurazione precedenti durante la distribuzione successiva. Verificare che la directory `app/etc/` non contenga il file `config.php`.

**Per disinstallare il software Adobe Commerce**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Rimuovi il file di configurazione.
   - Per Adobe Commerce 2.2 e versioni successive:

     ```bash
     rm app/etc/config.php
     ```

   - Per Adobe Commerce 2.1:

     ```bash
     rm app/etc/config.local.php
     ```

1. Disinstalla l’applicazione Adobe Commerce.

   ```bash
   php bin/magento setup:uninstall -n
   ```

1. Verifica che Adobe Commerce sia stato disinstallato correttamente.

   Per confermare la disinstallazione corretta viene visualizzato il seguente messaggio:

   ```
   [SUCCESS]: Magento uninstallation complete.
   ```

1. Cancella le sottodirectory `var/`.

   ```bash
   rm -rf var/*
   ```

1. Disconnetti.

>[!TIP]
>
>Facoltativamente, è buona prassi pulire le cache di build.
>
>```bash
>magento-cloud project:clear-build-cache
>```

## Forza una ridistribuzione

Se hai tentato di disinstallare Adobe Commerce e la distribuzione continua a non riuscire, puoi provare a forzarne manualmente la ridistribuzione.

```bash
git commit --allow-empty -m "<message>" && git push <origin> <branch>
```

## Reimpostare il database

Se si è tentato di disinstallare Adobe Commerce e il comando non è riuscito o non è stato completato, è possibile reimpostare manualmente il database.

**Per reimpostare il database**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Connettersi al database.

   ```bash
   mysql -h database.internal
   ```

1. Eliminare il database `main`.

   ```shell
   drop database main;
   ```

1. Creare un database `main` vuoto.

   ```shell
   create database main;
   ```

1. Eliminare i seguenti file di configurazione.

   - `config.php`
   - `config.php.bak`
   - `env.php`
   - `env.php.bak`

1. Disconnettersi e attivare una ridistribuzione.

   ```bash
   magento-cloud environment:redeploy
   ```

{{redeploy-warning}}
