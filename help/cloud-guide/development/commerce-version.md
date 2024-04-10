---
title: Aggiorna versione Commerce
description: Scopri come aggiornare la versione di Adobe Commerce nel progetto di infrastruttura cloud.
feature: Cloud, Upgrade
exl-id: 87821007-4979-4a20-940b-aa3c82c192d8
source-git-commit: 99272d08a11f850a79e8e24857b7072d1946f374
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Aggiorna versione Commerce

Puoi aggiornare la base di codice di Adobe Commerce a una versione più recente. Prima di aggiornare il progetto, controlla [Requisiti di sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) nel _Installazione_ per i requisiti della versione più recente del software.

A seconda della configurazione del progetto, le attività di aggiornamento possono includere quanto segue:

- I servizi di aggiornamento, come MariaDB (MySQL), OpenSearch, RabbitMQ e Redis, sono compatibili con le nuove versioni di Adobe Commerce.
- Convertire un file di gestione della configurazione precedente.
- Aggiornare il `.magento.app.yaml` con nuove impostazioni per hook e variabili di ambiente.
- Aggiorna le estensioni di terze parti alla versione più recente supportata.
- Aggiornare il `.gitignore` file.

{{upgrade-tip}}

{{pro-update-service}}

## Aggiornamento da versioni precedenti

Se stai iniziando un aggiornamento da una versione di Commerce precedente alla 2.1, alcune restrizioni nella base di codice di Adobe Commerce possono influire sulla capacità di _aggiorna_ a una specifica release ECE-Tools o a _aggiornamento_ alla successiva versione supportata di Commerce. Utilizzare la tabella seguente per determinare il percorso migliore:

| Versione corrente | Percorso di aggiornamento |
| ----------------- | ------------ |
| 2.1.3 e versioni precedenti | Prima di continuare, aggiorna Adobe Commerce alla versione 2.1.4 o successiva. Quindi esegui una [aggiornamento una tantum per installare ECE-Tools](../dev-tools/install-package.md). |
| 2.1.4 - 2.1.14 | [Aggiorna utensili ECE](../dev-tools/update-package.md) pacchetto.<br>Consulta le note sulla versione di [2002.0.9.](../release-notes/cloud-release-archive.md#v200209) e versioni successive di 2002.0.x. |
| 2.1.15 - 2.1.16 | [Aggiorna utensili ECE](../dev-tools/update-package.md) pacchetto.<br>Consulta le note sulla versione di[2002.0.9.](../release-notes/cloud-release-archive.md#v200209) e più tardi. |
| 2.2.x e versioni successive | [Aggiorna utensili ECE](../dev-tools/update-package.md) pacchetto.<br>Consulta le note sulla versione di[2002.0.8.](../release-notes/cloud-release-archive.md#v200208) e più tardi. |

{style="table-layout:auto"}

{{ece-tools-package}}

## Gestione della configurazione

Le versioni precedenti di Adobe Commerce, ad esempio 2.1.4 o successive alla versione 2.2.x o successive, utilizzavano una `config.local.php` per la gestione della configurazione. Adobe Commerce versione 2.2.0 e successive utilizzano `config.php` , che funziona esattamente come `config.local.php` ma con impostazioni di configurazione diverse che includono un elenco dei moduli abilitati e opzioni di configurazione aggiuntive.

Quando si esegue l&#39;aggiornamento da una versione precedente, è necessario eseguire la migrazione del `config.local.php` file per utilizzare il più recente `config.php` file. Per eseguire il backup del file di configurazione e crearne uno, effettua le seguenti operazioni.

**Per creare un file temporaneo `config.php` file**:

1. Crea una copia di `config.local.php` file e denominarlo `config.php`.

1. Aggiungi questo file al `app/etc` del progetto.

1. Aggiungi e invia il file al tuo ramo.

1. Invia il file al ramo di integrazione.

1. Continuare con il processo di aggiornamento.

>[!WARNING]
>
>Dopo l&#39;upgrade, è possibile rimuovere `config.php` e generare un nuovo file completo. Puoi eliminare questo file solo una volta per sostituirlo. Dopo aver generato un nuovo, completa `config.php` file, non è possibile eliminare il file per generarne uno nuovo. Consulta [Gestione della configurazione e distribuzione della pipeline](../store/store-settings.md).

### Verificare le dipendenze del compositore Zend Framework

Durante l&#39;aggiornamento a **2.3.x o successiva a partire da 2.2.x**, verifica che le dipendenze del framework Zend siano state aggiunte al `autoload` proprietà del `composer.json` file per supportare Laminas. Questo plug-in supporta i nuovi requisiti per Zend Framework, che è migrato al progetto Laminas. Consulta [Migrazione di Zend Framework al progetto Laminas](https://community.magento.com/t5/Magento-DevBlog/Migration-of-Zend-Framework-to-the-Laminas-Project/ba-p/443251) il _DevBlog Magento_.

**Per controllare `auto-load:psr-4` configurazione**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Consulta il ramo dell’integrazione.

1. Apri `composer.json` in un editor di testo.

1. Controlla la `autoload:psr-4` sezione per l’implementazione del gestore del plug-in Zend per la dipendenza dei controller.

   ```json
    "autoload": {
       "psr-4": {
          "Magento\\Framework\\": "lib/internal/Magento/Framework/",
          "Magento\\Setup\\": "setup/src/Magento/Setup/",
          "Magento\\": "app/code/Magento/",
          "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
       },
   }
   ```

1. Se manca la dipendenza Zend, aggiorna il `composer.json` file:

   - Aggiungi la seguente riga al `autoload:psr-4` sezione.

     ```json
     "Zend\\Mvc\\Controller\\": "setup/src/Zend/Mvc/Controller/"
     ```

   - Aggiornare le dipendenze del progetto.

     ```bash
     composer update
     ```

   - Aggiungi, conferma e invia modifiche al codice.

     ```bash
     git add -A
     ```

     ```bash
     git commit -m "Add Zend plugin manager implementation for controllers dependency for Laminas support"
     ```

     ```bash
     git push origin <branch-name>
     ```

   - Unisci le modifiche apportate all’ambiente di staging e quindi a Produzione.

## File di configurazione

Prima di aggiornare l’applicazione, è necessario aggiornare i file di configurazione del progetto per tenere conto delle modifiche alle impostazioni di configurazione predefinite per Adobe Commerce sull’infrastruttura cloud o l’applicazione. I valori predefiniti più recenti si trovano nella sezione [archivio GitHub magento-cloud](https://github.com/magento/magento-cloud).

### .magento.app.yaml

Esamina sempre i valori contenuti nel [.magento.app.yaml](../application/configure-app-yaml.md) per la versione installata, in quanto controlla il modo in cui l’applicazione viene generata e distribuita nell’infrastruttura cloud. L’esempio seguente è per la versione 2.4.7 e utilizza Composer 2.7.2. Il `build: flavor:` non viene utilizzata per Composer 2.x; vedi [Installazione e utilizzo di Composer 2](../application/properties.md#installing-and-using-composer-2).

**Per aggiornare `.magento.app.yaml` file**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Apri e modifica il `magento.app.yaml` file.

1. Aggiornare le opzioni PHP.

   ```yaml
   type: php:8.3
   
   build:
       flavor: none
   dependencies:
       php:
           composer/composer: '2.7.2'
   ```

1. Modifica il `hooks` proprietà `build` e `deploy` comandi.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           composer install
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Aggiungi le seguenti variabili di ambiente alla fine del file.

   Per Adobe Commerce da 2.2.x a 2.3.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

   Per Adobe Commerce 2.4.x-

   ```yaml
   variables:
       env:
           CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
           CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
   ```

1. Salva il file. Non eseguire ancora il commit o il push delle modifiche nell&#39;ambiente remoto.

1. Continuare con il processo di aggiornamento.

### composer.json

Prima di eseguire l&#39;aggiornamento, verificare sempre che le dipendenze nella `composer.json` sono compatibili con la versione di Adobe Commerce.

**Per aggiornare `composer.json` file per Adobe Commerce versione 2.4.4 e successive**:

1. Aggiungi quanto segue `allow-plugins` al `config` sezione:

   ```json
   "config": {
      "allow-plugins": {
         "dealerdirect/phpcodesniffer-composer-installer": true,
         "laminas/laminas-dependency-plugin": true,
         "magento/*": true
      }
   },
   ```

1. Aggiungi il seguente plug-in a `require` sezione:

   ```json
   "require": {
       "magento/composer-root-update-plugin": "^2.0.3"
   },
   ```

1. Aggiungi il seguente componente al `extra:component_paths` sezione:

   ```json
   "extra": {
      "component_paths": {
         "tinymce/tinymce": "lib/web/tiny_mce_5"
      },
   },
   ```

1. Salva il file. Non eseguire ancora il commit o il push delle modifiche nel ramo.

1. Continuare con il processo di aggiornamento.

## Backup del progetto

È consigliabile creare un backup del progetto prima di un aggiornamento. Utilizza i seguenti passaggi per eseguire il backup degli ambienti di integrazione, staging e produzione.

**Per eseguire il backup del database e del codice dell’ambiente di integrazione**:

1. Creare un backup locale del database remoto.

   ```bash
   magento-cloud db:dump
   ```

   >[!NOTE]
   >
   >Il `magento-cloud db:dump` il comando esegue [mysqldump](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html) comando con `--single-transaction` che consente di eseguire il backup del database senza bloccare le tabelle.

1. Eseguire il backup del codice e dei supporti.

   ```bash
   php bin/magento setup:backup --code [--media]
   ```

   Facoltativamente, puoi omettere `[--media]` se si dispone di un numero elevato di file statici già presenti nel controllo del codice sorgente.

**Per eseguire il backup del database dell&#39;ambiente di staging o di produzione prima della distribuzione**:

1. Utilizza SSH per accedere all’ambiente remoto.

1. Creare un [dump del database](../storage/database-dump.md). Per scegliere una directory di destinazione per il dump del database, utilizzare `--dump-directory` opzione.

   ```bash
   vendor/bin/ece-tools db-dump
   ```

   L’operazione di dump crea un `dump-<timestamp>.sql.gz` archiviare il file nella directory del progetto remoto. Consulta [Backup del database](../storage/database-dump.md).

## Aggiornamento dell’applicazione

Rivedi [versioni del servizio](../services/services-yaml.md#service-versions) informazioni sui requisiti della versione più recente del software prima di aggiornare l&#39;applicazione.

**Per aggiornare la versione dell&#39;applicazione**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Imposta la versione di aggiornamento utilizzando [sintassi del vincolo di versione](overview.md#cloud-metapackage).

   ```bash
   composer require "magento/magento-cloud-metapackage":">=CURRENT_VERSION <NEXT_VERSION" --no-update
   ```

   >[!NOTE]
   >
   >È necessario utilizzare la sintassi del vincolo di versione per aggiornare correttamente `ece-tools` pacchetto. Puoi trovare il vincolo di versione in `composer.json` file per la versione di [modello di applicazione](https://github.com/magento/magento-cloud/blob/master/composer.json) stai utilizzando per l’aggiornamento.

1. Aggiorna il progetto.

   ```bash
   composer update
   ```

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Upgrade"
   ```

   ```bash
   git push origin <branch-name>
   ```

   `git add -A` è necessario per aggiungere tutti i file modificati al controllo del codice sorgente a causa del modo in cui i pacchetti di base vengono utilizzati per il marshalling dei compositori. Entrambi `composer install` e `composer update` file di marshalling dal pacchetto di base (`magento/magento2-base` e `magento/magento2-ee-base`) nella directory principale del pacchetto.

   I file che Composer marshalling appartengono alla nuova versione di Adobe Commerce, per sovrascrivere la versione obsoleta degli stessi file. Attualmente, il marshalling è disabilitato in Adobe Commerce, pertanto è necessario aggiungere i file marshallati al controllo del codice sorgente.

1. Attendere il completamento della distribuzione.

1. Verifica l’aggiornamento nell’ambiente di integrazione, staging o produzione utilizzando SSH per accedere e controllare la versione.

   ```bash
   php bin/magento --version
   ```

### Creare un file config.php

Come indicato in [Gestione della configurazione](#configuration-management), dopo l&#39;upgrade, è necessario creare un aggiornamento `config.php` file. Completa eventuali modifiche aggiuntive alla configurazione tramite l’Amministratore nell’ambiente di integrazione.

**Per creare un file di configurazione specifico per il sistema**:

1. Dal terminale, utilizza un comando SSH per generare il `/app/etc/config.php` per l&#39;ambiente.

   ```bash
   ssh <SSH-URL> "<Command>"
   ```

   Ad esempio, per Pro, per eseguire `scd-dump` il `integration` ramo:

   ```bash
   ssh <project-id-integration>@ssh.us.magentosite.cloud "php vendor/bin/ece-tools config:dump"
   ```

1. Trasferisci il `config.php` alle workstation locali utilizzando `rsync` o `scp`. Puoi aggiungere questo file al ramo solo localmente.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
   ```

   Questo genera un aggiornamento `/app/etc/config.php` file con un elenco di moduli e impostazioni di configurazione.

>[!WARNING]
>
>Per un aggiornamento, elimina il `config.php` file. Una volta aggiunto il file al codice, devi **non** eliminala. Se è necessario rimuovere o modificare le impostazioni, modificare il file manualmente.

### Aggiornare le estensioni

Controlla le pagine delle estensioni e dei moduli di terze parti nel Marketplace o in altri siti aziendali e verifica il supporto per Adobe Commerce e Adobe Commerce sull’infrastruttura cloud. Se devi aggiornare estensioni e moduli di terze parti, l’Adobe consiglia di lavorare in un nuovo ramo di integrazione con le estensioni disabilitate.

**Verificare e aggiornare le estensioni**:

1. Crea una filiale sulla workstation locale.

1. Disattiva le estensioni in base alle esigenze.

1. Se disponibile, scarica gli aggiornamenti dell&#39;estensione.

1. Installa l’aggiornamento come descritto nella documentazione di terze parti.

1. Abilita e verifica l’estensione.

1. Aggiungi, esegui il commit e invia le modifiche al codice in remoto.

1. Effettua il push e il test nell’ambiente di integrazione.

1. Effettua il push all’ambiente di staging per il test in un ambiente di pre-produzione.

L’Adobe consiglia vivamente di aggiornare l’ambiente di produzione _prima di_ inclusione delle estensioni aggiornate nel processo di lancio del sito.

>[!NOTE]
>
>Quando si aggiorna la versione dell’applicazione, il processo di aggiornamento si aggiorna alla versione più recente del [Modulo CDN Fastly](../cdn/fastly.md#fastly-cdn-module-for-magento-2) automaticamente.

## Risoluzione dei problemi di aggiornamento

Se l’aggiornamento non è riuscito, viene visualizzato un messaggio di errore nel browser che indica che non è possibile accedere alla vetrina o al pannello di amministrazione:

```terminal
There has been an error processing your request
Exception printing is disabled by default for security reasons.
  Error log record number: <error-number>
```

**Per risolvere l&#39;errore**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Apri `./app/var/report/<error number>` file.

1. [Esaminare i registri](../test/log-locations.md) e determinano l’origine del problema.

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add -A && git commit -m "Fixed deployment failure" && git push origin <branch-name>
   ```
