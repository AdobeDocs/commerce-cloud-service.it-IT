---
title: Configurare la distribuzione dell'applicazione
description: Scopri come configurare le proprietà nel file di configurazione dell’applicazione che controllano il modo in cui [!DNL Commerce] Le build e le implementazioni dell’applicazione nell’ambiente Cloud.
feature: Cloud, Configuration, Build, Deploy
exl-id: 900da20d-98d2-4c9f-97ec-578aee775b55
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Configurare la distribuzione dell&#39;applicazione

Il `.magento.app.yaml` Il file controlla il modo in cui l&#39;applicazione crea e distribuisce. Sebbene Adobe Commerce su infrastruttura cloud supporti più applicazioni per progetto, in genere un progetto ha una singola applicazione con `.magento.app.yaml` nella directory principale dell’archivio.

Il `.magento.app.yaml` ha molti valori predefiniti, vedi [un campione `.magento.app.yaml` file](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml). Rivedi sempre `.magento.app.yaml` per la versione installata. Questo file può differire tra le diverse versioni di Adobe Commerce nelle infrastrutture cloud.

Utilizza il `.magento.app.yaml` per definire i seguenti valori di configurazione:

- [Proprietà](properties.md)- Consente di definire i valori delle proprietà per l&#39;istanza dell&#39;applicazione.
- [Variables, proprietà](variables-property.md)- Rivedi le variabili di ambiente necessarie per [!DNL Commerce] versione dell&#39;applicazione.
- [Impostazioni PHP](php-settings.md)- Configurare le opzioni PHP di runtime.
- [Imposta cache per file statici](set-cache.md)- Imposta il valore TTL della cache per i file statici e multimediali.
