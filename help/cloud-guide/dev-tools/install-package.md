---
title: Aggiornamento del progetto per l’utilizzo degli strumenti ECE
description: Scopri come aggiornare il progetto di infrastruttura cloud Adobe Commerce on per utilizzare il pacchetto ECE-Tools e sfruttare le correzioni e le funzionalità più recenti.
feature: Cloud, Install
exl-id: 820cca84-2817-4881-829f-ebb78400d8c7
source-git-commit: bcdb59f0d2a17e55e8b0479ee69fac06c710638f
workflow-type: tm+mt
source-wordcount: '365'
ht-degree: 0%

---

# Aggiornamento del progetto per l’utilizzo del pacchetto ECE-Tools

L’Adobe ha dichiarato obsoleto `magento/magento-cloud-configuration` e `magento/ece-patches` pacchetti a favore del `ece-tools` che semplifica molti processi cloud. Se utilizzi un progetto Adobe Commerce on Cloud Infrastructure precedente che _non_ contengono `ece-tools` , è necessario eseguire un&#39;unica operazione manuale _aggiornamento_ al progetto.

>[!WARNING]
>
>Se il progetto contiene `ece-tools` , è possibile saltare il seguente aggiornamento. Per verificare, recupera [!DNL Commerce] versione con `php vendor/bin/ece-tools -V` nella directory principale del progetto locale.

Questo processo di aggiornamento del progetto richiede l’aggiornamento di `magento/magento-cloud-metapackage` vincolo di versione in `composer.json` nella directory principale. Questo vincolo consente di aggiornare i metapacchetti per l’infrastruttura cloud di Adobe Commerce, inclusa la rimozione dei pacchetti obsoleti, senza dover aggiornare la versione Adobe Commerce corrente.

{{upgrade-tip}}

## Rimuovi pacchetti obsoleti

Prima di eseguire un aggiornamento per utilizzare `ece-tools` pacchetto, controlla `composer.lock` file per i seguenti pacchetti obsoleti:

- `magento/magento-cloud-configuration`
- `magento/ece-patches`

## Aggiornare il metapacchetto

Ogni versione di Adobe Commerce richiede un vincolo diverso in base ai seguenti elementi:

```terminal
>=current_version <next_version
```

- Per `current_version`, specifica la versione di Adobe Commerce da installare.
- Per `next_version`, specifica la versione della patch successiva al valore specificato in `current_version`.

Se desideri installare Adobe Commerce `2.3.5-p2`, impostato `current_version` a `2.3.5` e `next_version` a `2.3.6`. Il vincolo `">=2.3.5 <2.3.6"` installa l’ultimo pacchetto disponibile per la versione 2.3.5.

Puoi sempre trovare il vincolo di metapackage più recente nel [`magento-cloud` modello](https://github.com/magento/magento-cloud/blob/master/composer.json).

Nell’esempio seguente viene impostato un vincolo per il metapackage di Adobe Commerce on cloud infrastructure su qualsiasi versione maggiore o uguale alla versione corrente 2.4.7 e minore della versione successiva 2.4.8:

```json
"require": {
    "magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
},
```

## Aggiornare il progetto

Per aggiornare il progetto e utilizzare `ece-tools` del pacchetto, è necessario aggiornare il metapackage e il `.magento.app.yaml` esegue l&#39;hook delle proprietà ed esegue un aggiornamento del Compositore.

**Per aggiornare il progetto in modo da utilizzare gli strumenti ece**:

1. Aggiornare il `magento/magento-cloud-metapackage` vincolo di versione in `composer.json` file.

   ```bash
   composer require "magento/magento-cloud-metapackage":">=2.4.7 <2.4.8" --no-update
   ```

1. Aggiorna il metapacchetto.

   ```bash
   composer update magento/magento-cloud-metapackage
   ```

1. Modificare i comandi hook in `magento.app.yaml` file.

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

1. Controllare e rimuovere il [pacchetti obsoleti](#remove-deprecated-packages). I pacchetti obsoleti possono impedire un aggiornamento corretto.

   ```bash
   composer remove magento/magento-cloud-configuration
   ```

   ```bash
   composer remove magento/ece-patches
   ```

1. Potrebbe essere necessario aggiornare `ece-tools` pacchetto.

   ```bash
   composer update magento/ece-tools
   ```

1. Aggiungi e conferma le modifiche al codice. In questo esempio sono stati aggiornati i seguenti file:

   ```terminal
   .magento.app.yaml
   composer.json
   composer.lock
   ```

1. Invia le modifiche al codice al server remoto e unisci questo ramo con `integration` filiale.

   ```bash
   git push origin <branch-name>
   ```
