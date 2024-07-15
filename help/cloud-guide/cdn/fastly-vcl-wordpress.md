---
title: Reindirizzare le richieste a un back-end CMS
description: Scopri come reindirizzare le richieste in arrivo da un archivio Adobe Commerce a un sito WordPress separato utilizzando il modulo Fastly Edge.
feature: Cloud, Configuration, Routes
exl-id: 5bd9c56f-4412-4643-89b6-590a8ec65ac0
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '307'
ht-degree: 0%

---

# Reindirizzare le richieste a un back-end CMS

Reindirizza le richieste in arrivo da un archivio Adobe Commerce a un sito WordPress separato utilizzando il modulo Fastly Edge _Altra integrazione CMS/backend_ con un dizionario Edge. Puoi seguire un processo simile per indirizzare nuovamente le richieste ad altri backend CMS.

Utilizza Fastly Edge Modules per creare e caricare un codice VCL personalizzato dall’amministratore, anziché scrivere manualmente il codice VCL e caricarlo utilizzando l’API Fastly.

>[!NOTE]
>
>È consigliabile aggiungere configurazioni VCL personalizzate a un ambiente di staging in cui è possibile testarle prima di aggiornare la configurazione del servizio Fastly nell’ambiente di produzione.

**Prerequisiti**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

**Per reindirizzare le richieste da Adobe Commerce a WordPress**:

1. Abilitare Fastly Edge Modules nell’ambiente di staging o produzione.

   - Accedi all’amministratore.

   - Passa a **Archivi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema** > **Cache a pagina intera** > **Configurazione rapida** > **Configurazione avanzata**.

   - Imposta il valore per **Moduli Edge veloci** su **Sì**.

   - Salva la configurazione.

1. Identificare i percorsi URL da reindirizzare al backend di WordPress.

1. Completa le seguenti attività per configurare il servizio Fastly e crea il codice VCL personalizzato per reindirizzare le richieste al backend WordPress.

   - Crea un dizionario Edge che specifichi i percorsi da deviare dall’archivio Adobe Commerce al backend.

   - Aggiungi il backend WordPress alla configurazione del servizio Fastly e allega la condizione di richiesta per le riscritture dell’URL.

   - Configura l&#39;_altra integrazione CMS/backend_ modulo Edge per gestire le riscritture URL da Adobe Commerce al backend WordPress.

     Per istruzioni dettagliate, consulta [Moduli Fastly Edge - Altra integrazione CMS/Backend](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/Edge-Modules/EDGE-MODULE-OTHER-CMS-INTEGRATION.md) nella documentazione del _modulo Fastly CDN per il Magento 2_.

1. Dopo aver aggiornato la configurazione del servizio Fastly, verifica l’archivio di Adobe Commerce per assicurarti che le richieste URL specificate per WordPress vengano reindirizzate correttamente.
