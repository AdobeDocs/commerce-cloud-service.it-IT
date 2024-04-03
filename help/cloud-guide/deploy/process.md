---
title: Processo di distribuzione
description: Scopri come funziona l’implementazione per Adobe Commerce sui progetti di infrastruttura cloud.
feature: Cloud, Build, Deploy, SCD
exl-id: 378fa290-5a71-4ac2-816a-a7c837e45b2f
source-git-commit: 3d9e3294872fedcc43744f54a71c71d8951ed853
workflow-type: tm+mt
source-wordcount: '396'
ht-degree: 0%

---

# Processo di distribuzione

Il processo di distribuzione inizia quando esegui un’unione, un push o una sincronizzazione dell’ambiente oppure quando attivi un [ridistribuzione manuale](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). Il processo di distribuzione richiede tempo, ma esistono modi per ottimizzarla che dipendono dal fatto che si stia sviluppando e testando o lavorando con un sito attivo. In particolare, puoi controllare [distribuzione di contenuti statici](static-content.md).

Il processo di distribuzione prevede tre fasi distinte: compilazione, distribuzione e post-distribuzione. Ogni fase esegue azioni specifiche con risorse limitate:

## ![Fase di build](../../assets/status-build.png) Fase di build

Il _build_ phase assembla i contenitori per i servizi definiti nei file di configurazione, installa le dipendenze in base al `composer.lock` ed esegue gli hook di compilazione definiti nel file `.magento.app.yaml` file. Senza la possibilità di connettersi a qualsiasi servizio o di accedere al database, la fase di build dipende dalle risorse limitate all’ambiente.

## ![Fase di distribuzione](../../assets/status-deploy.png) Fase di distribuzione

Il _distribuire_ applica un blocco temporaneo alle richieste in arrivo e fa transitare il sito in [modalità di manutenzione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html). La fase di distribuzione utilizza i nuovi contenitori e, dopo aver installato il file system, apre le connessioni di rete e attiva i servizi definiti nella `relationships` sezione del `.magento.app.yaml` ed esegue gli hook di distribuzione definiti nel file `.magento.app.yaml` file. Tutto è _sola lettura_, ad eccezione delle directory definite nella `.magento.app.yaml` file. Per impostazione predefinita, il [`mounts` proprietà](../application/properties.md#mounts) include le seguenti directory:

- `app/etc`- contiene il `env.php` e `config.php` file di configurazione
- `pub/media`: contiene tutti i dati multimediali, ad esempio prodotti o categorie
- `pub/static`- contiene i file statici generati
- `var`: contiene i file temporanei creati durante il runtime

Tutte le altre directory dispongono di autorizzazioni di sola lettura. Il nuovo sito diventa attivo alla fine della fase di distribuzione, mentre passa dalla modalità di manutenzione e rilascia l’attesa temporanea sulle richieste in ingresso.

Nella fase di implementazione, copie del `app/etc/config.php` e `app/etc/env.php` I file di configurazione della distribuzione vengono salvati con l&#39;estensione BAK. Consulta [Impostazioni store](../store/store-settings.md#restore-configuration-files) informazioni sul ripristino di questi file.

## ![Fase post-distribuzione](../../assets/status-post-deploy.png) Fase post-distribuzione

Il _post-distribuzione_ esegue gli hook post-distribuzione definiti nella sezione `.magento.app.yaml` file. L&#39;esecuzione di qualsiasi azione in questa fase può influire sulle prestazioni del sito, tuttavia è possibile utilizzare [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) variabile di ambiente per popolare la cache.

## ![Verifica stato](../../assets/status-verify.png) Verifica configurazioni

Puoi verificare la configurazione ottimale per lo stato del progetto eseguendo il comando [Procedure guidate intelligenti](smart-wizards.md).

>[!NOTE]
>
>Con `ece-tools` 2002.1.0 e versioni successive, puoi utilizzare la funzione di distribuzione basata su scenari per personalizzare i processi di build, distribuzione e post-distribuzione per il progetto di infrastruttura cloud di Adobe Commerce. Consulta [Distribuzione basata su scenari](scenario-based.md).
