---
title: Imposta cache per file statici
description: Scopri come impostare le opzioni di archiviazione della cache nel file di configurazione dell'applicazione  [!DNL Commerce] .
feature: Cloud, Configuration, Cache, SCD
exl-id: ca6db004-47fc-45ea-b8db-c0ecc3c2136b
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '113'
ht-degree: 0%

---

# Imposta cache per file statici

Il valore TTL della cache (time-to-live) per i file multimediali e statici è impostato nel file di configurazione `.magento.app.yaml` utilizzando la chiave `expires`.

>[!NOTE]
>
>Prima di aggiornare l’ambiente di produzione, è importante testare le modifiche all’interno dell’ambiente di staging. [Invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per assistenza sull&#39;aggiornamento della configurazione in questi ambienti.

1. Specificare il tempo TTL (in secondi) nella proprietà [`web`](web-property.md) del file `.magento.app.yaml`. È possibile aggiungere la chiave `expires` in `locations` o in `"/media"` e `"/static"`.

   Per evitare che la cache scada, utilizzare la coppia chiave-valore `expires: -1`. Vedi l’esempio seguente:

   ```yaml
   # The configuration of app when it is exposed to the web.
   web:
     locations:
       "/media":
         ...
         expires: -1
   
       "/static":
         ...
         expires: -1
   ```

1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add -A && git commit -m "Set cache TTL for static files" && git push origin <branch-name>
   ```
