---
title: Variabili specifiche per il cloud
description: Consulta un elenco di variabili di ambiente specifiche per Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Configuration
recommendations: noDisplay, catalog
role: Developer
exl-id: 84b7c0fc-f0b0-4ff5-9f33-9d17180a9306
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# Variabili specifiche per il cloud

Le variabili di ambiente specifiche di Adobe Commerce sull&#39;infrastruttura cloud utilizzano il prefisso `MAGENTO_CLOUD_*`:

| Variabile | Descrizione |
| -------- | --------------- |
| `MAGENTO_CLOUD_APP_DIR` | Percorso assoluto della directory dell&#39;applicazione. |
| `MAGENTO_CLOUD_APPLICATION` | Oggetto JSON con codifica base64 che descrive l’applicazione. È mappato al contenuto del file `.magento.app.yaml` e dispone di sottochiavi. |
| `MAGENTO_CLOUD_APPLICATION_NAME` | Il nome dell&#39;applicazione configurata nel file `.magento.app.yaml`. |
| `MAGENTO_CLOUD_DOCUMENT_ROOT` | Percorso assoluto della directory principale del documento web, se applicabile. |
| `MAGENTO_CLOUD_ENVIRONMENT` | Nome del ramo dell’ambiente. |
| `MAGENTO_CLOUD_PROJECT` | ID del progetto. |
| `MAGENTO_CLOUD_RELATIONSHIPS` | Oggetto JSON con codifica base64 che rappresenta la definizione dell’endpoint chiave (nome della relazione) e valore (matrici di coppie di relazioni). Ogni definizione di endpoint di relazione è una forma scomposta di un URL. Contiene `scheme`, `host`, `port` e _facoltativamente_ a `username`, `password`, `path` e alcune informazioni aggiuntive in `query`. |
| `MAGENTO_CLOUD_ROUTES` | Descrivere le route definite nel file di ambiente `.magento/routes.yaml`. |
| `MAGENTO_CLOUD_TREE_ID` | L’ID della struttura per l’applicazione, che corrisponde all’SHA della struttura in Git. |
| `MAGENTO_CLOUD_VARIABLES` | Oggetto JSON con codifica base64 con coppie chiave-valore, ad esempio `"key":"value"`. |
| `MAGENTO_CLOUD_LOCKS_DIR` | Fornisce il percorso del punto di montaggio per il provider di blocchi nell’infrastruttura cloud. Il provider di blocchi impedisce l&#39;avvio di processi cron e gruppi cron duplicati. |

>[!WARNING]
>
>Per aggiungere variabili di ambiente a [sostituire le impostazioni di configurazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) utilizzando [[!DNL Cloud Console]](../project/overview.md), è necessario anteporre al nome della variabile `env:` come nell&#39;esempio seguente:
>
>![Esempio di variabile di ambiente](../../assets/set-env-variable-ui.png)

Poiché i valori possono cambiare nel tempo, è consigliabile esaminare la variabile in fase di esecuzione e utilizzarla per configurare l’applicazione. Ad esempio, utilizzare la variabile `MAGENTO_CLOUD_RELATIONSHIPS` per recuperare le relazioni relative all&#39;ambiente nel modo seguente:

```php
<?php
/**
  * Get relationships information from cloud environment variable.
  *
  * @return mixed
  */
    protected function getRelationships()
    {
        return json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"]), true);
    }
```

## Visualizzazione delle variabili di ambiente

È possibile utilizzare il comando `env:config:show` da [il pacchetto `ece-tools`](../dev-tools/package-overview.md) per visualizzare un elenco di variabili per l&#39;ambiente corrente.

```bash
php ./vendor/bin/ece-tools env:config:show variables
```

Output di esempio per l&#39;opzione `variables`:

```terminal
Magento Cloud Environment Variables:
+-----------------------------------+----------------------------------+
| Variable name                     | Value                            |
+-----------------------------------+----------------------------------+
| ADMIN_EMAIL                       | commerceadmin@company.com        |
| ADMIN_PASSWORD                    | 123123q                          |
+-----------------------------------+----------------------------------+
```
