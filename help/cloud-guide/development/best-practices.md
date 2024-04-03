---
title: Best practice per l’aggiornamento del progetto
description: Consulta un elenco di best practice per l’aggiornamento dei file di progetto.
feature: Cloud, Best Practices, Upgrade
exl-id: 7d0a2627-e4c5-46b4-9e6c-24d20fa4f92f
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 0%

---

# Best practice per l’aggiornamento del progetto

Segui le best practice per le build e la distribuzione e utilizza [Aggiornamenti e patch](../development/commerce-version.md) per aggiornare l’applicazione. Per pianificare il lavoro di aggiornamento e post-aggiornamento, attenersi alle seguenti linee guida:

- **Esegui il backup del progetto**-Prima di aggiornare Adobe Commerce e qualsiasi estensione di terze parti o personalizzata, eseguire il backup del database negli ambienti di integrazione, staging e produzione. Consulta [Eseguire il backup del database](../development/commerce-version.md#project-backup).

- **Verifica la compatibilità**-

   - Assicurati che tutti i temi personalizzati siano compatibili con la nuova versione di Adobe Commerce

   - Dopo l&#39;aggiornamento di estensioni di terze parti e personalizzate, utilizza `magento-cloud local:build` per convalidare le dipendenze del Compositore prima della distribuzione.

   - Consulta le note sulla versione e la documentazione dell’estensione di Adobe Commerce per assicurarti di aver implementato tutte le soluzioni alternative o le modifiche alla configurazione necessarie per risolvere problemi funzionali noti e bug correlati all’aggiornamento della versione e delle estensioni di Adobe Commerce.

   - Assicurati che le versioni del servizio installate siano compatibili con la nuova versione di Adobe Commerce e aggiorna i servizi secondo necessità. Consulta [Servizi](../services/services-yaml.md).

   - Verifica il database per risolvere eventuali problemi introdotti dagli aggiornamenti della versione e delle estensioni di Adobe Commerce.

   - Apporta gli aggiornamenti necessari alle impostazioni specifiche dell’ambiente prima di distribuirle nell’ambiente remoto.

   - Verificare che la versione del servizio di ricerca sia compatibile con la versione del client PHP. Consulta [Elasticsearch configurazione](../services/elasticsearch.md) o [Configura OpenSearch](../services/opensearch.md).

- **Verifica della connettività del database e dello storage disponibile in ambienti remoti**-

   - Utilizzare SSH per accedere al server remoto e verificare la connessione al database MySQL. Consulta [Connettersi al database](../services/mysql.md#connect-to-the-database).

   - Verificare lo storage disponibile nell&#39;ambiente remoto-Utilizzare `disk free` per visualizzare e gestire lo spazio su disco disponibile negli ambienti Cloud. Consulta [Gestione dello spazio su disco](../storage/manage-disk-space.md).

      - Controllare le dimensioni del database aggiornato e verificare che `services.yaml` il file dispone di spazio su disco sufficiente.

      - Liberare spazio su disco-Cancellare la cache e pulire `/log` e `/tmp` directory prima della distribuzione.

- **Pianifica ed esegui un aggiornamento corretto sugli ambienti locali e di integrazione, prima della distribuzione in Staging**- Dopo l&#39;aggiornamento, verificare l&#39;implementazione e risolvere eventuali problemi.

- **Unisci il codice in Staging e quindi in Produzione**-Testare e risolvere eventuali problemi nell’ambiente di staging prima di inviare le modifiche all’ambiente di produzione.

- **Completa le attività di post-aggiornamento**-

   - Utilizzare SSH per accedere al server remoto e verificare quanto segue:

      - Controlla lo stato dell’indicizzatore e reindicizza in base alle esigenze. Consulta [Gestire gli indicizzatori](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/manage-indexers.html) nel _Guida alla configurazione_.

      - Controlla la `cron` registri e `cron_schedule` nel database di Adobe Commerce per verificare lo stato del cron ed eseguire nuovamente i processi cron in base alle esigenze.
Consulta [Registrazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html#logging) nel _Guida alla configurazione_.

   - Completa il test di accettazione utente post-aggiornamento sugli ambienti di staging e produzione e risolve eventuali problemi relativi agli aggiornamenti di estensioni di terze parti e personalizzati.
