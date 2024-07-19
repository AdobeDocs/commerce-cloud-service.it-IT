---
title: Gestione account New Relic
description: Scopri come accedere al tuo account New Relic e gestire l’accesso, le integrazioni e l’utilizzo degli strumenti per il progetto Adobe Commerce on Cloud Infrastructure.
feature: Cloud, Observability
role: Admin
exl-id: ee639e2e-4074-4384-8f68-152bc3bac93b
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# Gestione account New Relic

Quando Adobe esegue il provisioning del progetto di infrastruttura cloud, il proprietario della licenza riceve un’e-mail da New Relic con le credenziali e le istruzioni per l’accesso all’account New Relic. Se non hai ricevuto l’e-mail, utilizza l’indirizzo e-mail Proprietario licenza per reimpostare la password di New Relic.

## Gestire l’accesso degli utenti

A un account New Relic può essere assegnata una sola persona al ruolo Proprietario. Se è necessario modificare il proprietario dell&#39;account, assegnare il ruolo Amministratore al proprietario corrente, quindi assegnare il ruolo Proprietario a un altro utente. Per istruzioni, consulta [Aggiornare il proprietario dell&#39;account](https://docs.newrelic.com/docs/accounts/original-accounts-billing/original-users-roles/users-roles-original-user-model/) nella _documentazione di New Relic_.

Linee guida per la gestione dell&#39;accesso a New Relic:

- I proprietari dei progetti e gli utenti amministratori possono aggiungere e rimuovere utenti dall’account New Relic.
- Non creare più di cinque **Utenti** ad accesso completo.
- Concedi l’accesso completo solo agli utenti che richiedono rigorosamente l’accesso a tutto il set di funzioni.
- Non sono disponibili indicazioni specifiche per **utenti con restrizioni**.

>[!TIP]
>
>Prima di assegnare il ruolo Proprietario a un utente, verifica che l’utente esista sull’account New Relic per Adobe Commerce nell’infrastruttura cloud. Se è necessario aggiungere l&#39;utente a tale account e un proprietario o un amministratore dell&#39;account esistente non può fornire assistenza, qualsiasi utente con accesso all&#39;[account Proprietario partnership Adobe](https://account.newrelic.com/accounts/1311131/users) per New Relic può aggiungere utenti per conto del cliente.

Aggiungi almeno un utente **Amministratore** al tuo account New Relic per gestire tutti gli accessi, le integrazioni e l&#39;utilizzo degli strumenti.

**Per accedere alla gestione utenti in New Relic**:

1. Accedi al tuo [account New Relic](https://login.newrelic.com/login).

1. Seleziona il tuo nome utente nell’area di navigazione in basso a sinistra.

1. Fare clic su **[!UICONTROL Administration]** e selezionare uno dei seguenti elementi dall&#39;elenco:

   - **[!UICONTROL User management]** per aggiungere un utente e gestire gli utenti attivi e gli inviti in sospeso.

   - **[!UICONTROL Access management]** per gestire gruppi di utenti, ruoli e account.

Consulta [Gestione utente](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) nella documentazione di _New Relic_.

## Configurare New Relic per l’ambiente Starter

>[!NOTE]
>
>**Gli ambienti Pro** sono preconfigurati per l&#39;utilizzo dei servizi New Relic e possono ignorare le istruzioni di attivazione e connessione. Se New Relic APM non è installato negli ambienti di staging e produzione o New Relic Infrastructure non è disponibile nell&#39;ambiente di produzione, [invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per richiedere l&#39;installazione.

Per gli ambienti Starter, è necessario controllare il file `.magento.app.yaml` per verificare che la sezione `runtime` includa l&#39;estensione New Relic. Se l&#39;estensione non è stata configurata, aggiungi quanto segue:

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### Applica chiave di licenza

Per collegare un ambiente Cloud a New Relic, aggiungi il codice di licenza New Relic all’ambiente.

- Per i **progetti Pro**, Adobe aggiunge la chiave di licenza agli ambienti di produzione e staging durante il processo di provisioning. Puoi accedere al tuo [account New Relic](https://login.newrelic.com/login) per verificare la connettività tra il tuo sito Adobe Commerce sull&#39;infrastruttura cloud e New Relic.

- Per i **progetti iniziali**, si dispone di un codice di licenza New Relic che supporta fino a _tre_ ambienti. Devi aggiungere manualmente la chiave alle configurazioni dell’ambiente. Per gli ambienti Starter non viene eseguito il preprovisioning per l’utilizzo del servizio New Relic.

Per gli ambienti Starter, abilita l’integrazione New Relic aggiungendo il codice di licenza New Relic alla configurazione dell’ambiente. Aggiungi la chiave agli ambienti di staging e produzione e a un altro ambiente a tua scelta. Per la configurazione è necessario solo il codice di licenza New Relic. Puoi trovare informazioni sulle opzioni di configurazione aggiuntive nell&#39;argomento [Rapporti di New Relic](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html) nella _Guida utente di Adobe Commerce_.

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Credenziali di accesso per la pagina dell’account Adobe Commerce o per la licenza New Relic associata al progetto
>- [Accesso a livello di amministratore](../project/user-access.md) agli ambienti Starter da configurare
>- Credenziali per accedere a [Admin](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html) per l&#39;ambiente

**Per configurare New Relic per gli ambienti Starter**:

1. Trovare il codice di licenza New Relic da [!DNL Cloud Console] o Cloud CLI.

   Metodo **[!DNL Cloud Console]**:

   - Apri la [pagina dell&#39;account](https://accounts.magento.cloud/user) del progetto cloud.

   - Nella scheda _Progetti_, individua il progetto.

   - Fare clic su **Visualizza dettagli** per informazioni sull&#39;infrastruttura del progetto.

   - Espandere la sezione **Servizio New Relic** per visualizzare il codice di licenza.

   - Copia il codice di licenza.

   **Metodo CLI cloud**:

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. Aggiungere la chiave di licenza New Relic a un ambiente utilizzando CLI `magento-cloud`.

   - Passa all’ambiente che richiede il codice di licenza.
   - Aggiornare il valore della variabile utilizzando il comando CLI `magento-cloud` seguente:

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   Facoltativamente, puoi aggiungerlo dall&#39;[Amministratore Commerce](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html#step-3%3A-configure-your-store).

1. Accedi al tuo [account New Relic](https://login.newrelic.com/login) per verificare di poter visualizzare i dati dall&#39;ambiente Adobe Commerce. Vedere [Analisi delle prestazioni](investigate-performance.md).

### Rimuovi chiave di licenza

È possibile utilizzare il codice di licenza New Relic solo in tre ambienti attivi. Se la chiave è in uso in tre ambienti, devi rimuoverla da uno di essi prima di poterla aggiungere a un ambiente diverso.

**Per rimuovere una chiave di licenza da un ambiente**:

1. Elencare le variabili di ambiente.

   ```bash
   magento-cloud variable:list
   ```

   Risposta di esempio:

   ```
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >Se la chiave di licenza è stata aggiunta come variabile _project_, è necessario rimuovere tale variabile a livello di progetto. Una variabile di progetto aggiunge la licenza a _ogni_ ramo di ambiente creato, che può utilizzare o superare il limite di licenza. Per elencare le variabili di progetto: `magento-cloud variable:list --level project`

1. Elimina la variabile di licenza.

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```
