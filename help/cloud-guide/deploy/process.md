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

Il processo di distribuzione inizia quando si esegue un&#39;unione, un push o una sincronizzazione dell&#39;ambiente oppure quando si attiva una [ridistribuzione manuale](../dev-tools/cloud-cli-overview.md#redeploy-the-environment). Il processo di distribuzione richiede tempo, ma esistono modi per ottimizzarla che dipendono dal fatto che si stia sviluppando e testando o lavorando con un sito attivo. In particolare, puoi controllare la [distribuzione del contenuto statico](static-content.md).

Il processo di distribuzione prevede tre fasi distinte: compilazione, distribuzione e post-distribuzione. Ogni fase esegue azioni specifiche con risorse limitate:

## ![Fase di compilazione](../../assets/status-build.png) Fase di compilazione

La fase _build_ assembla i contenitori per i servizi definiti nei file di configurazione, installa le dipendenze in base al file `composer.lock` ed esegue gli hook di compilazione definiti nel file `.magento.app.yaml`. Senza la possibilità di connettersi a qualsiasi servizio o di accedere al database, la fase di build dipende dalle risorse limitate all’ambiente.

## ![Fase di distribuzione](../../assets/status-deploy.png) Fase di distribuzione

La fase _deploy_ blocca temporaneamente le richieste in ingresso e fa passare il sito alla [modalità di manutenzione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html). La fase di distribuzione utilizza i nuovi contenitori e, dopo il montaggio del file system, apre le connessioni di rete, attiva i servizi definiti nella sezione `relationships` del file `.magento.app.yaml` ed esegue gli hook di distribuzione definiti nel file `.magento.app.yaml`. Tutto è _di sola lettura_, ad eccezione delle directory definite nel file `.magento.app.yaml`. Per impostazione predefinita, la proprietà [`mounts`](../application/properties.md#mounts) include le directory seguenti:

- `app/etc`—contiene i file di configurazione `env.php` e `config.php`
- `pub/media`: contiene tutti i dati multimediali, ad esempio prodotti o categorie
- `pub/static`: contiene i file statici generati
- `var`: contiene i file temporanei creati durante il runtime

Tutte le altre directory dispongono di autorizzazioni di sola lettura. Il nuovo sito diventa attivo alla fine della fase di distribuzione, mentre passa dalla modalità di manutenzione e rilascia l’attesa temporanea sulle richieste in ingresso.

Nella fase di distribuzione, le copie dei file di configurazione della distribuzione `app/etc/config.php` e `app/etc/env.php` vengono salvate con l&#39;estensione BAK. Consulta [Impostazioni archivio](../store/store-settings.md#restore-configuration-files) per informazioni sul ripristino di questi file.

## ![Fase di distribuzione Post](../../assets/status-post-deploy.png) fase di distribuzione Post

La fase _post-distribuzione_ esegue gli hook post-distribuzione definiti nel file `.magento.app.yaml`. L&#39;esecuzione di un&#39;azione in questa fase può influire sulle prestazioni del sito. Tuttavia, è possibile utilizzare la variabile di ambiente [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) per popolare la cache.

## ![Verifica stato](../../assets/status-verify.png) Verifica configurazioni

È possibile verificare la configurazione ottimale per lo stato del progetto eseguendo le [Smart Wizard](smart-wizards.md).

>[!NOTE]
>
>Con `ece-tools` 2002.1.0 e versioni successive, è possibile utilizzare la funzionalità di distribuzione basata su scenari per personalizzare i processi di compilazione, distribuzione e post-distribuzione per il progetto Adobe Commerce su infrastruttura cloud. Vedi [Distribuzione basata su scenari](scenario-based.md).
