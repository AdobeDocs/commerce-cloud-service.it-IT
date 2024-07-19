---
title: '[!DNL ECE-Tools] pacchetto'
description: Scopri il pacchetto  [!DNL ECE-Tools]  e come consente di gestire e distribuire Adobe Commerce.
exl-id: 5583a685-29c5-4de5-8d2e-94cff5ff37ab
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '431'
ht-degree: 0%

---

# Pacchetto utensili ECE

Il pacchetto [!DNL ECE-Tools] è un insieme di script e strumenti progettati per gestire e distribuire l&#39;applicazione [!DNL Commerce]. Il pacchetto `ece-tools` semplifica molti processi, ad esempio la gestione dei processi cron, la verifica della configurazione del progetto e l&#39;applicazione di patch e hotfix di Adobe. Puoi visualizzare e contribuire all&#39;archivio del codice [open-source [!DNL ECE-Tools] su GitHub][ece-repo].

{{ece-tools-package}}

Il pacchetto `ece-tools` è compatibile con Adobe Commerce, a partire dalla versione 2.1.4, e contiene script e comandi Adobe Commerce su infrastruttura cloud progettati per semplificare la gestione del codice e generare e distribuire automaticamente i progetti.

Nell&#39;elenco seguente sono elencati i comandi `ece-tools` disponibili:

```bash
php ./vendor/bin/ece-tools list
```

## Generare e distribuire

Il pacchetto `ece-tools` contiene comandi per eseguire operazioni per le fasi di compilazione, distribuzione e post-distribuzione dell&#39;avvio dell&#39;applicazione Adobe Commerce sull&#39;infrastruttura cloud. Il comando `php ./vendor/bin/ece-tools build`, ad esempio, avvia il processo di compilazione dell&#39;applicazione.

Per impostazione predefinita, questi `ece-tools` comandi si trovano nella [proprietà hooks](../application/hooks-property.md) del file di configurazione `.magento.app.yaml`.

## Generatore di configurazione Docker

Il pacchetto `ece-tools` include una dipendenza per il pacchetto [magento/magento-cloud-docker], che fornisce funzionalità e file di configurazione per le immagini Docker per avviare un ambiente di sviluppo Docker per Adobe Commerce sull&#39;infrastruttura cloud. Puoi anche eseguire Cloud Docker per Commerce come pacchetto autonomo. Consulta [Sviluppo Docker](../dev-tools/cloud-docker.md).

## Servizi, route e variabili

È possibile utilizzare il pacchetto `ece-tools` per visualizzare informazioni dettagliate sulle [variabili cloud](../environment/variables-cloud.md) con codifica Base64 utilizzate in qualsiasi ambiente Cloud. Il comando seguente mostra tutti i servizi, le route e le variabili.

```bash
php ./vendor/bin/ece-tools env:config:show
```

Per visualizzare un set specifico di informazioni, utilizzare il formato seguente:

```bash
php ./vendor/bin/ece-tools env:config:show <option>
```

- `services` - Visualizza i dati di relazione della variabile di ambiente `MAGENTO_CLOUD_RELATIONSHIPS`, definita nel file `services.yaml`.
- `routes` - Visualizza le route configurate per il progetto utilizzando la variabile di ambiente `MAGENTO_CLOUD_ROUTES`.
- `variables` - Visualizza le variabili configurate per il progetto utilizzando la variabile di ambiente `MAGENTO_CLOUD_VARIABLES`.

Output di esempio per l&#39;opzione `services`:

```
Magento Cloud Services:
+-----------------------------------+----------------------------------+
| Service Configuration             | Value                            |
+-----------------------------------+----------------------------------+
| database:                                                            |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| password                          | <password>                       |
| port                              | 3306                             |
+-----------------------------------+----------------------------------+
| opensearch:                                                          |
+-----------------------------------+----------------------------------+
| host                              | 127.0.0.1                        |
| port                              | 9200                             |
...
```

## Verificare la configurazione dell’ambiente

È disponibile una serie di comandi di verifica per valutare la configurazione del progetto. Per una descrizione dettagliata di ogni comando della procedura guidata, vedere [Procedure guidate avanzate](../deploy/smart-wizards.md) nella sezione _Ottimizza distribuzione_. Il comando `wizard:ideal-state` viene eseguito automaticamente durante la fase di compilazione. Per verificare lo stato ideale del progetto:

```bash
php ./vendor/bin/ece-tools wizard:ideal-state
```

>[!NOTE]
>
>Eseguire il comando `wizard:ideal-state` nell&#39;ambiente cloud remoto. Il comando restituisce sempre l&#39;errore `The configured state is not ideal` quando viene eseguito nell&#39;ambiente di sviluppo locale.

Output di esempio:

```
Ideal state is configured
```

Consulta le [note sulla versione per gli strumenti ece](../release-notes/cloud-tools-suite.md).

## patch di Adobe e patch personalizzate

Il pacchetto `ece-tools` include una dipendenza per il pacchetto [magento/magento-cloud-patches], che fornisce patch e hotfix di Adobe che migliorano l&#39;integrazione di tutte le versioni di Adobe Commerce con gli ambienti Cloud e supportano la distribuzione rapida di correzioni critiche. &quot;offre anche patch personalizzate da aggiungere al progetto di infrastruttura cloud di Adobe Commerce. Vedi [Applicare le patch](../development/apply-patches.md).

<!-- link definitions -->

[ece-repo]: https://github.com/magento/ece-tools
[magento/magento-cloud-docker]: https://github.com/magento/magento-cloud-docker
[magento/magento-cloud-patches]: https://github.com/magento/magento-cloud-patches
