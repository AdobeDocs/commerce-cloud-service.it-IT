---
title: Gestione estensioni
description: Scopri come installare e gestire le estensioni in Adobe Commerce su un’infrastruttura cloud.
feature: Cloud, Extensions, Upgrade
exl-id: 9c6e98ca-85da-4342-8402-d576eb382ba2
source-git-commit: f8fb9d4d43c85f91ff87686160bcddb7cd417635
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Gestione estensioni

Puoi estendere le funzionalità dell’applicazione Adobe Commerce aggiungendo un’estensione dal file [Commerce Marketplace](https://marketplace.magento.com). Ad esempio, puoi aggiungere un tema per cambiare l’aspetto della vetrina, oppure un pacchetto per la lingua per localizzare la vetrina e l’amministratore.

>[!NOTE]
>
>Per evitare problemi di installazione, tutti gli acquisti Marketplace devono essere completati utilizzando lo stesso account (MAGEID) proprietario del progetto cloud.

## Nome del compositore di un’estensione

Anche se questa sezione illustra come ottenere il nome e la versione del Compositore da Commerci Marketplace, puoi trovare il nome e la versione di _qualsiasi_ nel file Composer del modulo. Apri `composer.json` in un editor di testo e annota `"name"` e `"version"` valori.

**Ottenere il nome del compositore di un modulo dalla Commerce Marketplace**:

1. Accedi a [Commerce Marketplace](https://marketplace.magento.com) con il nome utente e la password utilizzati per acquistare il componente.

1. Nell’angolo in alto a destra, fai clic sul tuo nome utente e seleziona **Il mio profilo**.

   ![Accedi al tuo account Marketplace](../../assets/marketplace/my-profile.png)

1. Il giorno _Il mio account_ pagina, fai clic su **I miei acquisti**.

   ![Cronologia acquisti Marketplace](../../assets/marketplace/my-purchases.png)

1. Il giorno _I miei acquisti_ , seleziona un modulo acquistato e fai clic su **Dettagli tecnici**.

1. Clic **Copia** per copiare [!UICONTROL Component name] negli Appunti.

1. Apri un editor di testo e incolla il nome del componente aggiungendo un carattere due punti (`:`).

1. In entrata **Dettagli tecnici**, fai clic su **Copia** per copiare [!UICONTROL Component version] negli Appunti.

1. Nell’editor di testo, aggiungi il numero di versione al nome del componente dopo i due punti. Ad esempio:

   ```text
   extension-name/magento2:1.0.1
   ```

## Installare un’estensione

L’Adobe consiglia di lavorare in un ramo di sviluppo quando aggiungi un’estensione all’implementazione. Durante l’installazione di un’estensione, il nome (`<VendorName>_<ComponentName>`) viene inserito automaticamente nel [`app/etc/config.php`](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/deployment-files.html) file. Non è necessario modificare direttamente il file.

**Per installare un’estensione**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Creare o estrarre un ramo di sviluppo. Consulta [diramazione](../development/cli-branches.md).

1. Utilizzando il nome e la versione del Compositore, aggiungi l’estensione al `require` sezione del `composer.json` file.

   ```bash
   composer require <extension-name>:<version> --no-update
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
   git commit -m "Install <extension-name>"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!WARNING]
   >
   >Durante l&#39;installazione di un&#39;estensione, devi includere `composer.lock` quando si inviano le modifiche al codice all&#39;ambiente remoto. Il `composer install` il comando legge `composer.lock` per abilitare le dipendenze definite nell&#39;ambiente remoto.

1. Al termine della build e della distribuzione, utilizza un SSH per accedere all’ambiente remoto e verificare che l’estensione sia installata.

   ```bash
   bin/magento module:status <extension-name>
   ```

   Il nome di un’estensione utilizza il formato: `<VendorName>_<ComponentName>`.

   Risposta di esempio:

   ```terminal
   Module is enabled
   ```

   Se riscontri errori di distribuzione, consulta [distribuzione dell&#39;estensione non riuscita](../deploy/recover-failed-deployment.md).

## Gestione estensioni

Quando aggiungi un’estensione utilizzando Compositore, il processo di distribuzione la abilita automaticamente. Se l&#39;estensione è già installata, è possibile attivarla o disattivarla utilizzando CLI. Durante la gestione delle estensioni, utilizza il formato: `<VendorName>_<ComponentName>`

Non abilitare o disabilitare mai un&#39;estensione durante l&#39;accesso agli ambienti remoti.

**Per abilitare o disabilitare un’estensione**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Attivare o disattivare un modulo. Il `module` il comando aggiorna `config.php` con lo stato richiesto del modulo.

   >Abilita un modulo.

   ```bash
   bin/magento module:enable <module-name>
   ```

   >Disattiva un modulo.

   ```bash
   bin/magento module:disable <module-name>
   ```

1. Se hai attivato un modulo, utilizza `ece-tools` per aggiornare la configurazione.

   ```bash
   ./vendor/bin/ece-tools module:refresh
   ```

1. Verifica lo stato di un modulo.

   ```bash
   bin/magento module:status <module-name>
   ```

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Disable <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

## Aggiornare un’estensione

Prima di continuare, è necessario il nome e la versione del Compositore per l’estensione. Inoltre, verifica che l’estensione sia compatibile con il progetto e la versione di Adobe Commerce. In particolare: [controlla la versione PHP richiesta](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) prima di iniziare.

**Per aggiornare un’estensione**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Creare o estrarre un ramo di sviluppo. Consulta [diramazione](../development/cli-branches.md).

1. Apri `composer.json` in un editor di testo.

1. Individua l’estensione e aggiorna la versione.

1. Salva le modifiche e esci dall’editor di testo.

1. Aggiornare le dipendenze del progetto.

   ```bash
   composer update
   ```

1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update <extension-name>"
   ```

   ```bash
   git push origin <branch-names>
   ```

Se riscontri degli errori, vedi [Ripristino da guasto componente](../deploy/recover-failed-deployment.md). Per ulteriori informazioni sull&#39;utilizzo delle estensioni con Adobe Commerce, consulta [Estensioni](https://experienceleague.adobe.com/docs/commerce-admin/start/resources/extensions.html) nel _Guida per l’amministratore_.
