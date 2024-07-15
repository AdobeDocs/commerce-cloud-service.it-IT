---
title: Configurare la distribuzione dell'applicazione
description: Scopri come configurare le proprietà nel file di configurazione dell'applicazione che controllano il modo in cui l'applicazione  [!DNL Commerce]  genera e distribuisce nell'ambiente Cloud.
feature: Cloud, Configuration, Build, Deploy
exl-id: 900da20d-98d2-4c9f-97ec-578aee775b55
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 0%

---

# Configurare la distribuzione dell&#39;applicazione

Il file `.magento.app.yaml` controlla il modo in cui l&#39;applicazione genera e distribuisce. Sebbene Adobe Commerce su infrastruttura cloud supporti più applicazioni per progetto, in genere un progetto ha una singola applicazione con il file `.magento.app.yaml` nella directory principale dell&#39;archivio.

`.magento.app.yaml` ha molti valori predefiniti. Vedere [un file `.magento.app.yaml` di esempio](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml). Rivedi sempre `.magento.app.yaml` per la versione installata. Questo file può differire tra le diverse versioni di Adobe Commerce nelle infrastrutture cloud.

Utilizzare il file `.magento.app.yaml` per definire i seguenti valori di configurazione:

- [Proprietà](properties.md) - Consente di definire i valori delle proprietà per l&#39;istanza dell&#39;applicazione.
- [Proprietà variabili](variables-property.md) - Esaminare le variabili di ambiente necessarie per la versione dell&#39;applicazione [!DNL Commerce].
- [Impostazioni PHP](php-settings.md)—Configurare le opzioni PHP di runtime.
- [Imposta cache per file statici](set-cache.md) - Imposta TTL cache per file multimediali e statici.
