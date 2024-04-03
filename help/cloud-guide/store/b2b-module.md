---
title: Abilitare il modulo B2B
description: Scopri come abilitare il modulo business-to-business per Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, B2B
exl-id: 01d02ea0-1e7d-4608-adbf-1dad7f5e2182
source-git-commit: bb7a866b1896a8a43d01ad3f83dc655bcf383374
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 0%

---

# Abilitare il modulo B2B

Se i tuoi clienti sono aziende, puoi installare il modulo B2B per Adobe Commerce per estendere il progetto Adobe Commerce on Cloud Infrastructure Pro in modo da adattarlo a un modello business-to-business. Sebbene questo argomento fornisca informazioni specifiche sull’installazione e la configurazione del modulo B2B per Adobe Commerce sull’infrastruttura cloud, puoi trovare ulteriori informazioni B2B nelle seguenti guide:

- [Guida per gli sviluppatori B2B di Adobe Commerce](https://developer.adobe.com/commerce/webapi/rest/b2b/)
- [Guida utente di Adobe Commerce B2B](https://experienceleague.adobe.com/docs/commerce-admin/b2b/guide-overview.html)

>[!TIP]
>
>Poiché B2B è un modulo per Adobe Commerce sull’infrastruttura cloud, Adobe consiglia di distribuire l’applicazione Adobe Commerce in un ambiente di integrazione o staging prima di iniziare.

## Installare il modulo B2B

L’Adobe consiglia di lavorare in un ramo di sviluppo quando aggiungi il modulo B2B al progetto. Se non hai un ramo, consulta [Creare un ramo per lo sviluppo](../development/cli-branches.md#create-a-branch-for-development). Durante l’installazione del modulo B2B, il `Magento_B2b` il nome del modulo viene inserito automaticamente nel `app/etc/config.php` file. Non è necessario modificare direttamente il file.

**Per installare il modulo B2B**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Creare o estrarre un ramo di sviluppo.

1. Aggiungere il modulo B2B al `require` sezione del `composer.json` file.

   ```bash
   composer require magento/extension-b2b --no-update
   ```

1. Aggiornare le dipendenze del progetto.

   ```bash
   composer update
   ```

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Install the B2B module."
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Al termine della generazione e della distribuzione, utilizza SSH per accedere all’ambiente remoto e verificare che il modulo B2B sia installato.

   ```bash
   bin/magento module:status Magento_B2b
   ```

   Il nome di un’estensione utilizza il formato: `<VendorName>_<ComponentName>`.

   Risposta di esempio:

   ```terminal
   Magento_B2b : Module is enabled
   ```

   Se riscontri errori di distribuzione, consulta [Ripristino da guasto componente](../deploy/recover-failed-deployment.md).

## Abilitare il modulo B2B

Quando installi il modulo B2B utilizzando Composer, il processo di distribuzione abilita automaticamente il modulo. Se hai già installato il modulo B2B, puoi abilitare o disabilitare il modulo utilizzando CLI

Abilita il modulo B2B:

```bash
bin/magento module:enable Magento_B2b
```

Risposta di esempio:

```terminal
The following modules have been enabled:
- Magento_B2b

Cache cleared successfully.
Generated classes cleared successfully. Please run the 'setup:di:compile' command to generate classes.
Info: Some modules might require static view files to be cleared. To do this, run 'module:enable' with the --clear-static-content option to clear them.
```

Consulta [Gestione estensioni](extensions.md).

## Configurare il modulo B2B

Dopo aver installato il modulo B2B per Adobe Commerce, devi [avvia il messaggio consumer](https://experienceleague.adobe.com/docs/commerce-admin/b2b/install.html#start-message-consumers) in modo da poter abilitare _Catalogo condiviso_ modulo e devi [abilitare le funzioni B2B](https://experienceleague.adobe.com/docs/commerce-admin/b2b/enable-basic-features.html).
