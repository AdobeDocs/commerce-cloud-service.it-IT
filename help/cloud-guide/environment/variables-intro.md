---
title: Variabili di ambiente
description: Consulta un elenco di variabili di ambiente specifiche per Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Build, Configuration, Deploy
exl-id: bfee2f69-93a6-4d26-bb9e-be8acc5673c3
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '258'
ht-degree: 0%

---

# Variabili di ambiente

Adobe Commerce su infrastruttura cloud consente di assegnare variabili di ambiente per ignorare le opzioni di configurazione. Il pacchetto `ece-tools` imposta i valori nel file `env.php` in base ai valori di [variabili cloud](variables-cloud.md), variabili impostate nel file [!DNL Cloud Console] e nel file di configurazione `.magento.env.yaml`.

Le variabili di ambiente nel file `.magento.env.yaml` personalizzano l&#39;ambiente Cloud sovrascrivendo la configurazione di Commerce esistente. Se un valore predefinito è `Not Set`, il pacchetto `ece-tools` esegue l&#39;azione **NO** e utilizza il valore predefinito [!DNL Commerce] o il valore della configurazione `MAGENTO_CLOUD_RELATIONSHIPS`. Se è impostato il valore predefinito, il pacchetto `ece-tools` agisce per impostarlo.

I tipi di variabili di ambiente includono:

- [ADMIN](variables-admin.md)—le variabili sovrascrivono le variabili ADMIN del progetto
- [MAGENTO_CLOUD](variables-cloud.md)—Variabili specifiche per l&#39;infrastruttura cloud
- Variabili utilizzate nel file `.magento.env.yaml`:
   - [Globale](variables-global.md): le variabili influiscono sulle fasi di compilazione, distribuzione e post-distribuzione
   - [Build](variables-build.md): le variabili controllano le azioni di compilazione
   - [Distribuisci](variables-deploy.md): le azioni di distribuzione del controllo delle variabili
   - [Post-deploy](variables-post-deploy.md): le variabili controllano le azioni dopo la distribuzione

Le variabili sono _gerarchiche_, il che significa che se una variabile non viene sottoposta a override, viene ereditata dall&#39;ambiente padre.

È possibile impostare [variabili ADMIN](variables-admin.md) da [!DNL Cloud Console] o utilizzando Adobe Commerce CLI. È possibile gestire altre variabili di ambiente dal file [`.magento.env.yaml`](configure-env-yaml.md) per gestire le azioni di generazione e distribuzione in tutti gli ambienti, inclusi Pro Staging e Produzione, senza richiedere un ticket di supporto.

>[!TIP]
>
>I file YAML fanno distinzione tra maiuscole e minuscole e non consentono tabulazioni. Fare attenzione a utilizzare un rientro coerente in tutto il file `.magento.env.yaml`, altrimenti la configurazione potrebbe non funzionare come previsto. Gli esempi in questa documentazione e nel file di esempio utilizzano il rientro _two-space_. Utilizza il comando [ece-tools validate](configure-env-yaml.md#validate-configuration-file) per controllare la configurazione.
