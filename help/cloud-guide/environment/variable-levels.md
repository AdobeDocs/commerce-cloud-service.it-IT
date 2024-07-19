---
title: Livelli e opzioni delle variabili
description: Scopri i diversi livelli di variabili e le diverse opzioni utilizzati per personalizzare l’ambiente runtime del progetto Adobe Commerce on cloud infrastructure.
feature: Cloud, Configuration, Security
exl-id: 11aa0862-94c0-47fb-946a-0148f75cc24c
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '446'
ht-degree: 0%

---

# Livelli variabili

Le variabili di progetto si applicano a tutti gli ambienti all’interno del progetto. Le variabili di ambiente si applicano a un ambiente o a un ramo specifico. Un ambiente _eredita_ definizioni di variabili dall&#39;ambiente padre.

Puoi sovrascrivere un valore ereditato definendo la variabile specifica per l’ambiente. Ad esempio, per impostare le variabili per lo sviluppo, definisci i valori delle variabili nel file `.magento.env.yaml` nell&#39;ambiente di integrazione. Tutti gli ambienti che si diramano dall’ambiente di integrazione ereditano tali valori. Per informazioni dettagliate sulla configurazione dell&#39;ambiente tramite il file `.magento.env.yaml`, vedere [Configurazione della distribuzione](configure-env-yaml.md).

>[!BEGINTABS]

>[!TAB CLI]

**Per impostare le variabili utilizzando Cloud CLI**:

- **Variabili specifiche per il progetto** - Per impostare lo stesso valore per _tutti_ gli ambienti nel progetto. Queste variabili sono disponibili in fase di build e runtime in tutti gli ambienti.

  ```bash
  magento-cloud variable:create --level project --name <variable-name> --value <variable-value>
  ```

- **Variabili specifiche dell&#39;ambiente** - Per impostare un valore univoco per un ambiente _specifico_. Queste variabili sono disponibili in fase di runtime e vengono ereditate dagli ambienti figlio. Specificare l&#39;ambiente nel comando utilizzando l&#39;opzione `-e`.

  ```bash
  magento-cloud variable:create --level environment --name <variable-name> --value <variable-value>
  ```

Dopo aver impostato le variabili specifiche del progetto, è necessario ridistribuire manualmente l’ambiente remoto per rendere effettiva la modifica. Effettua il push dei nuovi commit per attivare una ridistribuzione.

>[!TAB Console]

**Per impostare le variabili utilizzando[!DNL Cloud Console]**:

1. In _[!DNL Cloud Console]_, fai clic sull&#39;icona di configurazione sul lato destro della navigazione del progetto.

   ![Configura progetto](../../assets/icon-configure.png){width="36"}

1. Per impostare una variabile a livello di progetto, in _Impostazioni progetto_ fare clic su **Variabili**.

   ![Variabili progetto](../../assets/ui-project-variables.png)

1. Per impostare una variabile a livello di ambiente, nell&#39;elenco _Ambienti_ selezionare un ambiente e fare clic sulla scheda **[!UICONTROL Variables]**.

   ![Scheda Variabili di ambiente](../../assets/ui-environment-variables.png)

1. Fare clic su **[!UICONTROL Create variable]**.

1. Immetti un nome e un valore per la variabile. Scegli tra le opzioni:

   - Disponibile durante il runtime
   - Disponibile durante la generazione
   - Valore JSON
   - Variabile sensibile (valore nascosto nella console e risposte CLI)
   - Rendi ereditabile (gli ambienti figlio possono ereditare le variabili a livello di ambiente)

1. Fare clic su **[!UICONTROL Create variable]**.

>[!CAUTION]
>
>L&#39;impostazione delle variabili specifiche dell&#39;ambiente in [!DNL Cloud Console] ridistribuisce automaticamente l&#39;ambiente.

>[!ENDTABS]

## Visibilità

È possibile limitare la visibilità di una variabile durante la compilazione o il runtime utilizzando il comando `--visible-<build|runtime>`. Inoltre, sono disponibili opzioni per impostare l’ereditarietà e la riservatezza.

Per evitare che una variabile venga visualizzata o ereditata, utilizza le seguenti opzioni:

- `--inheritable false`: disabilita l&#39;ereditarietà per gli ambienti figlio. Questo è utile per impostare valori di sola produzione sul ramo `master` e consentire a tutti gli altri ambienti di utilizzare una variabile a livello di progetto con lo stesso nome.
- `--sensitive true` - contrassegna la variabile come _non leggibile_ in [!DNL Cloud Console]. Non è possibile visualizzare la variabile nell’interfaccia utente; tuttavia, è possibile visualizzare la variabile dal contenitore dell’applicazione, come qualsiasi altra variabile.

Di seguito viene illustrato un caso specifico per impedire la visualizzazione o l’ereditarietà di una variabile. È possibile specificare queste opzioni solo nella CLI. Questo caso non riguarda tutte le variabili di ambiente disponibili.

```bash
magento-cloud variable:create --name <variable-name> --value <variable-value> --inheritable false --sensitive true
```

## Verificare livelli e valori delle variabili

Puoi visualizzare un elenco delle variabili esistenti utilizzando Cloud CLI.

```bash
magento-cloud variables
```

```
Variables on the project Project-Name (<project-id>), environment <environment-name>:
+----------------------------+-------------+-------------------------------------------+
| Name                       | Level       | Value                                     |
+----------------------------+-------------+-------------------------------------------+
| env:COMPOSER_AUTH          | project     | {                                         |
|                            |             |    "http-basic": {                        |
|                            |             |       "repo.magento.com": {               |
|                            |             |       "username":                         |
|                            |             | "<public-key>",                           |
|                            |             |       "password":                         |
|                            |             | "<private-key>"                           |
|                            |             |     }                                     |
|                            |             |   }                                       |
|                            |             | }                                         |
| ADMIN_EMAIL                | project     | admin@123.com                             |
| ADMIN_EMAIL                | environment | admin@123.com                             |
| ADMIN_PASSWORD             | environment | password                                  |
| ADMIN_URL                  | environment | admin123                                  |
| ADMIN_USERNAME             | environment | admin                                     |
| php:newrelic.license       | environment | xxxx71fb030366182117f955a22e4baf8exxxxxx  |
+----------------------------+-------------+-------------------------------------------+
```
