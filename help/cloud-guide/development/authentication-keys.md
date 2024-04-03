---
title: Chiavi di autenticazione
description: Scopri come applicare le chiavi di autenticazione a un progetto di sviluppo in Adobe Commerce su un’infrastruttura cloud.
feature: Cloud, Security
topic: Security
exl-id: b05cd4c2-0804-49c8-980a-4c7b6932082b
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 0%

---

# Chiavi di autenticazione

È necessario disporre di una chiave di autenticazione per accedere all’archivio di Adobe Commerce e abilitare i comandi di installazione e aggiornamento per il progetto di infrastruttura cloud Adobe Commerce. Esistono due metodi per specificare le credenziali di autorizzazione del Compositore.

- **file di autenticazione**- Un file che contiene Adobe Commerce [credenziali di autorizzazione](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/authentication-keys.html) nella directory principale dell’infrastruttura cloud di Adobe Commerce.
- **variabile di ambiente**: variabile di ambiente per impostare le chiavi di autenticazione nel progetto di infrastruttura cloud Adobe Commerce on per prevenire l’esposizione accidentale.

>[!BEGINSHADEBOX]

**Nota sulla sicurezza**

L’Adobe consiglia di utilizzare [variabile di ambiente](#composer-auth-environment-variable) con il progetto cloud per evitare l’esposizione accidentale delle credenziali di autorizzazione.

Il metodo del file di autenticazione è ideale quando utilizzi Cloud Docker for Commerce come strumento di sviluppo locale, ma fai attenzione a non caricare `auth.json` in un archivio pubblico basato su Git. È possibile aggiungere `auth.json` file in [`.gitignore` file](../project/file-structure.md#ignoring-files).

>[!ENDSHADEBOX]

## File di autenticazione

**Per creare un `auth.json` file**:

1. Se non si dispone di un `auth.json` nella directory principale del progetto, creane una.

   - Utilizzando un editor di testo, crea un’ `auth.json` nella directory principale del progetto.
   - Copia il contenuto del [esempio `auth.json`](https://github.com/magento/magento2/blob/2.3/auth.json.sample) nel nuovo `auth.json` file.

1. Sostituisci `<public-key>` e `<private-key>` con le credenziali di autenticazione di Adobe Commerce.

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Salva le modifiche e esci dall’editor di testo.

## Variabile di ambiente di autenticazione del compositore

Il metodo seguente è il modo migliore per evitare l’esposizione accidentale di credenziali sensibili in un archivio pubblico basato su Git.

**Per aggiungere chiavi di autenticazione utilizzando una variabile di ambiente**:

1. In _[!DNL Cloud Console]_, fai clic sull’icona di configurazione a destra della navigazione del progetto.

   ![Configura progetto](../../assets/icon-configure.png){width="36"}

1. In _Impostazioni progetto_ , fare clic su **[!UICONTROL Variables]**.

1. Clic **[!UICONTROL Create variable]**.

1. In **[!UICONTROL Variable name]** campo, immetti `env:COMPOSER_AUTH`.

1. In _Valore_ , aggiungi quanto segue e sostituisci `<public-key>` e `<private-key>` con le credenziali di autenticazione di Adobe Commerce:

   ```json
   {
       "http-basic": {
           "repo.magento.com": {
               "username": "<public-key>",
               "password": "<private-key>"
           }
       }
   }
   ```

1. Seleziona **[!UICONTROL Available during buildtime]** e deseleziona **[!UICONTROL Available during runtime]**.

1. Clic **[!UICONTROL Create variable]**.

1. Rimuovi il `auth.json` da ogni ambiente.
