---
title: Aggiornamento del progetto per l’utilizzo degli strumenti ECE
description: Scopri come aggiornare il progetto di infrastruttura cloud Adobe Commerce on per utilizzare il pacchetto ECE-Tools e sfruttare le correzioni e le funzionalità più recenti.
feature: Cloud, Install
exl-id: 820cca84-2817-4881-829f-ebb78400d8c7
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Aggiornamento del progetto per l’utilizzo del pacchetto ECE-Tools

L&#39;Adobe ha dichiarato obsoleti i pacchetti `magento/magento-cloud-configuration` e `magento/ece-patches` a favore del pacchetto `ece-tools`, semplificando così molti processi cloud. Se utilizzi un progetto Adobe Commerce on Cloud Infrastructure precedente che _non_ contiene il pacchetto `ece-tools`, devi eseguire un processo _aggiornamento_ manuale e una tantum al tuo progetto.

>[!WARNING]
>
>Se il progetto contiene il pacchetto `ece-tools`, è possibile saltare il seguente aggiornamento. Per verificare il problema, recuperare la versione [!DNL Commerce] utilizzando il comando `php vendor/bin/ece-tools -V` nella directory principale del progetto locale.

Questo processo di aggiornamento del progetto richiede l&#39;aggiornamento del vincolo di versione `magento/magento-cloud-metapackage` nel file `composer.json` nella directory radice. Questo vincolo consente di aggiornare i metapacchetti per l’infrastruttura cloud di Adobe Commerce, inclusa la rimozione dei pacchetti obsoleti, senza dover aggiornare la versione Adobe Commerce corrente.

{{upgrade-tip}}

## Rimuovi pacchetti obsoleti

Prima di eseguire un aggiornamento per utilizzare il pacchetto `ece-tools`, controllare il file `composer.lock` per i seguenti pacchetti obsoleti:

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## Aggiornare il metapacchetto

Ogni versione di Adobe Commerce richiede un vincolo diverso in base ai seguenti elementi:

```
>=current_version <next_version
```

- Per `current_version`, specificare la versione di Adobe Commerce da installare.
- Per `next_version`, specificare la versione successiva della patch dopo il valore specificato in `current_version`.

Per installare Adobe Commerce `2.3.5-p2`, impostare `current_version` su `2.3.5` e `next_version` su `2.3.6`. Il vincolo `">=2.3.5 <2.3.6"` installa l&#39;ultimo pacchetto disponibile per la versione 2.3.5.

È sempre possibile trovare il vincolo di metapackage più recente nel modello [`magento-cloud`](https://github.com/magento/magento-cloud/blob/master/composer.json).

Nell’esempio seguente viene impostato un vincolo per il metapackage di Adobe Commerce on cloud infrastructure su qualsiasi versione maggiore o uguale alla versione corrente 2.4.7 e minore della versione successiva 2.4.8:

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
},
```

## Aggiornare il progetto

Per aggiornare il progetto in modo da utilizzare il pacchetto `ece-tools`, è necessario aggiornare il metapackage e le proprietà degli hook `.magento.app.yaml` ed eseguire un aggiornamento del Compositore.

**Per aggiornare il progetto in modo da utilizzare gli strumenti ece**:

1. Aggiornare il vincolo di versione `magento/magento-cloud-metapackage` nel file `composer.json`.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.7 <2.4.8" --no-update
   ```

1. Aggiorna il metapacchetto.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. Modificare i comandi hook nel file `magento.app.yaml`.

   ```yaml
   hooks:
       # We run build hooks before your application has been packaged.
       build: |
           set -e
           php ./vendor/bin/ece-tools run scenario/build/generate.xml
           php ./vendor/bin/ece-tools run scenario/build/transfer.xml
       # We run deploy hook after your application has been deployed and started.
       deploy: |
           php ./vendor/bin/ece-tools run scenario/deploy.xml
       # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
       post_deploy: |
           php ./vendor/bin/ece-tools run scenario/post-deploy.xml
   ```

1. Controllare e rimuovere i [pacchetti obsoleti](#remove-deprecated-packages). I pacchetti obsoleti possono impedire un aggiornamento corretto.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. Potrebbe essere necessario aggiornare il pacchetto `ece-tools`.

   ```bash
   composer update magento/ece-tools
   ```

1. Aggiungi e conferma le modifiche al codice. In questo esempio sono stati aggiornati i seguenti file:

   ```
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. Invia le modifiche al codice al server remoto e unisci il ramo con il ramo `integration`.

   ```bash
   git push origin <branch-name>
   ```
