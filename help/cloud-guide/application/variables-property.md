---
title: Variables, proprietà
description: Utilizzare la proprietà variables per personalizzare le opzioni di configurazione dell'archivio per [!DNL Commerce] applicazione.
feature: Cloud, Configuration
exl-id: 5cd92fbb-8bff-48b1-9658-500140591344
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '72'
ht-degree: 0%

---

# Variables, proprietà

È possibile utilizzare variabili di ambiente basate su applicazioni per personalizzare le configurazioni degli archivi. Queste variabili utilizzano una sintassi specifica. Consulta [Ignora impostazioni di configurazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/override-config-settings.html) nel _Guida alla configurazione_.

Le seguenti variabili di ambiente incluse nel `.magento.app.yaml` sono necessari per versioni specifiche di [!DNL Commerce] applicazione.

Richiesto per Adobe Commerce da 2.2.x a 2.3.x:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYMENT__BRAINTREE__CHANNEL: 'Magento_Enterprise_Cloud_BT'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```

Per Adobe Commerce 2.4.x, imposta le seguenti variabili:

```yaml
variables:
    env:
        CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
        CONFIG__STORES__DEFAULT__PAYPAL__NOTATION_CODE: 'Magento_Enterprise_Cloud'
```
