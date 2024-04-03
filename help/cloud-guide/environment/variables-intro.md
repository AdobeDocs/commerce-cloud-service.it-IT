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

Adobe Commerce su infrastruttura cloud consente di assegnare variabili di ambiente per ignorare le opzioni di configurazione. Il `ece-tools` il pacchetto imposta i valori in `env.php` file in base ai valori di [Variabili cloud](variables-cloud.md), le variabili impostate in [!DNL Cloud Console]e `.magento.env.yaml` file di configurazione.

Le variabili di ambiente in `.magento.env.yaml` Personalizza l’ambiente Cloud ignorando la configurazione Commerce esistente. Se un valore predefinito è `Not Set`, quindi `ece-tools` pacchetti **NO** e utilizza [!DNL Commerce] predefinito o il valore da `MAGENTO_CLOUD_RELATIONSHIPS` configurazione. Se è impostato il valore predefinito, `ece-tools` il pacchetto agisce per impostare tale impostazione predefinita.

I tipi di variabili di ambiente includono:

- [ADMIN](variables-admin.md)- le variabili sovrascrivono le variabili ADMIN del progetto
- [MAGENTO_CLOUD](variables-cloud.md)—variabili specifiche per l’infrastruttura cloud
- Variabili utilizzate in `.magento.env.yaml` file:
   - [Globale](variables-global.md): le variabili influiscono sulle fasi di build, distribuzione e post-distribuzione
   - [Genera](variables-build.md)- Le variabili controllano le azioni di generazione
   - [Distribuisci](variables-deploy.md)- Azioni di distribuzione del controllo variabili
   - [Post-distribuzione](variables-post-deploy.md)- le variabili controllano le azioni dopo la distribuzione

Le variabili sono _gerarchico_, il che significa che se una variabile non viene sottoposta a override, viene ereditata dall’ambiente padre.

È possibile impostare [Variabili ADMIN](variables-admin.md) dal [!DNL Cloud Console] o utilizzando Adobe Commerce CLI. Puoi gestire altre variabili di ambiente dalla sezione [`.magento.env.yaml`](configure-env-yaml.md) file per gestire azioni di generazione e implementazione in tutti gli ambienti, inclusi Pro Staging e Produzione, senza richiedere un ticket di supporto.

>[!TIP]
>
>I file YAML fanno distinzione tra maiuscole e minuscole e non consentono tabulazioni. Fare attenzione a utilizzare rientri coerenti in tutto il `.magento.env.yaml` o la configurazione potrebbe non funzionare come previsto. Gli esempi presenti in questa documentazione e nel file di esempio utilizzano _a due spazi_ rientro. Utilizza il [comando convalida strumenti ece](configure-env-yaml.md#validate-configuration-file) per verificare la configurazione.
