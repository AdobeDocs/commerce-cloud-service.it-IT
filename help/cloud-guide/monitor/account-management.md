---
title: Gestione account New Relic
description: Scopri come accedere al tuo account New Relic e gestire l’accesso, le integrazioni e l’utilizzo degli strumenti per il progetto Adobe Commerce on Cloud Infrastructure.
feature: Cloud, Observability
role: Admin
exl-id: ee639e2e-4074-4384-8f68-152bc3bac93b
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '833'
ht-degree: 0%

---

# Gestione account New Relic

Quando Adobe esegue il provisioning del progetto di infrastruttura cloud, il proprietario della licenza riceve un’e-mail da New Relic con le credenziali e le istruzioni per l’accesso all’account New Relic. Se non hai ricevuto l’e-mail, utilizza l’indirizzo e-mail Proprietario licenza per reimpostare la password di New Relic.

## Gestire l’accesso degli utenti

A un account New Relic può essere assegnata una sola persona al ruolo Proprietario. Se è necessario modificare il proprietario dell&#39;account, assegnare il ruolo Amministratore al proprietario corrente, quindi assegnare il ruolo Proprietario a un altro utente. Consulta [Aggiornare il proprietario dell’account](https://docs.newrelic.com/docs/accounts/original-accounts-billing/original-users-roles/users-roles-original-user-model/) nel _Documentazione di New Relic_ per istruzioni.

Linee guida per la gestione dell&#39;accesso a New Relic:

- I proprietari dei progetti e gli utenti amministratori possono aggiungere e rimuovere utenti dall’account New Relic.
- Non creare più di cinque file ad accesso completo **Utenti**.
- Concedi l’accesso completo solo agli utenti che richiedono rigorosamente l’accesso a tutto il set di funzioni.
- Non esistono linee guida specifiche sulle **Limitato** utenti.

>[!TIP]
>
>Prima di assegnare il ruolo Proprietario a un utente, verifica che l’utente esista sull’account New Relic per Adobe Commerce nell’infrastruttura cloud. Se devi aggiungere l’utente a tale account e un proprietario o amministratore dell’account esistente non può aiutarti, qualsiasi utente con accesso a [Account proprietario partnership Adobe](https://account.newrelic.com/accounts/1311131/users) per New Relic può aggiungere utenti per conto del cliente.

Aggiungi almeno un elemento **Amministratore** l’utente al tuo account New Relic in grado di gestire tutti gli accessi, le integrazioni e l’utilizzo degli strumenti.

**Per accedere alla gestione degli utenti in New Relic**:

1. Accedi al tuo [Account New Relic](https://login.newrelic.com/login).

1. Seleziona il tuo nome utente nell’area di navigazione in basso a sinistra.

1. Clic **[!UICONTROL Administration]** e selezionare una delle opzioni seguenti dall&#39;elenco:

   - **[!UICONTROL User management]** per aggiungere un utente e gestire gli utenti attivi e gli inviti in sospeso.

   - **[!UICONTROL Access management]** per gestire gruppi di utenti, ruoli e account.

Consulta [Gestione utente](https://docs.newrelic.com/docs/accounts/accounts-billing/new-relic-one-user-management/user-management-ui-and-tasks/) nel _New Relic_ documentazione.

## Configurare New Relic per l’ambiente Starter

>[!NOTE]
>
>**Ambienti Pro** sono preconfigurati per l’utilizzo dei servizi New Relic e possono saltare le istruzioni di attivazione e connessione. Se New Relic APM non è installato negli ambienti di staging e produzione o New Relic Infrastructure non è disponibile nell&#39;ambiente di produzione, [invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per richiedere l&#39;installazione.

Per gli ambienti Starter, controlla `.magento.app.yaml` file per verificare che `runtime` include l&#39;estensione New Relic. Se l&#39;estensione non è stata configurata, aggiungi quanto segue:

> `.magento.app.yaml`

```yaml
runtime:
    extensions:
        - newrelic
```

### Applica chiave di licenza

Per collegare un ambiente Cloud a New Relic, aggiungi il codice di licenza New Relic all’ambiente.

- Per **Progetti Pro**, Adobe aggiunge la chiave di licenza agli ambienti di produzione e staging durante il processo di provisioning. Puoi accedere al tuo [Account New Relic](https://login.newrelic.com/login) per verificare la connettività tra il sito Adobe Commerce sull’infrastruttura cloud e New Relic.

- Per **Progetti iniziali**, si dispone di una chiave di licenza New Relic che supporta fino a _tre_ ambienti. Devi aggiungere manualmente la chiave alle configurazioni dell’ambiente. Per gli ambienti Starter non viene eseguito il preprovisioning per l’utilizzo del servizio New Relic.

Per gli ambienti Starter, abilita l’integrazione New Relic aggiungendo il codice di licenza New Relic alla configurazione dell’ambiente. Aggiungi la chiave agli ambienti di staging e produzione e a un altro ambiente a tua scelta. Per la configurazione è necessario solo il codice di licenza New Relic. Per informazioni sulle opzioni di configurazione aggiuntive, consulta [Generazione rapporti New Relic](https://experienceleague.adobe.com/docs/commerce-admin/config/general/new-relic-reporting.html) argomento in _Guida utente di Adobe Commerce_.

{{redeploy-warning}}

>[!PREREQUISITES]
>
>- Credenziali di accesso per la pagina dell’account Adobe Commerce o per la licenza New Relic associata al progetto
>- [Accesso a livello di amministratore](../project/user-access.md) negli ambienti Starter per configurare
>- Credenziali per accedere a [Amministratore](https://experienceleague.adobe.com/docs/commerce-admin/systems/user-accounts/permissions.html) per l&#39;ambiente

**Per configurare New Relic per gli ambienti Starter**:

1. Trovare il codice di licenza di New Relic dall&#39; [!DNL Cloud Console] o Cloud CLI.

   **[!DNL Cloud Console]metodo**:

   - Apri il progetto cloud [pagina account](https://accounts.magento.cloud/user).

   - Il giorno _Progetti_ , trova il progetto.

   - Clic **Visualizza dettagli** per informazioni sull’infrastruttura del progetto.

   - Espandi **Servizio New Relic** per visualizzare il codice di licenza.

   - Copia il codice di licenza.

   **Metodo CLI cloud**:

   ```bash
   magento-cloud subscription:info services.newrelic
   ```

1. Aggiungere il codice di licenza New Relic a un ambiente utilizzando `magento-cloud` CLI

   - Passa all’ambiente che richiede il codice di licenza.
   - Aggiorna il valore della variabile utilizzando quanto segue `magento-cloud` Comando CLI:

     ```bash
     magento-cloud variable:update php:newrelic.license --value <newrelic-license-key>
     ```

   Facoltativamente, puoi aggiungerlo dalla sezione [Amministratore Commerce](https://experienceleague.adobe.com/docs/commerce-admin/start/reporting/new-relic-reporting.html#step-3%3A-configure-your-store).

1. Accedi al tuo [Account New Relic](https://login.newrelic.com/login) per verificare di poter visualizzare i dati dall’ambiente Adobe Commerce. Consulta [Esaminare le prestazioni](investigate-performance.md).

### Rimuovi chiave di licenza

È possibile utilizzare il codice di licenza New Relic solo in tre ambienti attivi. Se la chiave è in uso in tre ambienti, devi rimuoverla da uno di essi prima di poterla aggiungere a un ambiente diverso.

**Per rimuovere una chiave di licenza da un ambiente**:

1. Elencare le variabili di ambiente.

   ```bash
   magento-cloud variable:list
   ```

   Risposta di esempio:

   ```terminal
    +----------------------+-------------+----------------------+---------+
    | Name                 | Level       | Value                | Enabled |
    +----------------------+-------------+----------------------+---------+
    | php:newrelic.license | environment | newrelic-license-key | true    |
    +----------------------+-------------+----------------------+---------+
   ```

   >[!WARNING]
   >
   >Se la chiave di licenza è stata aggiunta come _progetto_ variabile, devi rimuovere tale variabile a livello di progetto. Una variabile di progetto aggiunge la licenza a _ogni_ ramo dell’ambiente creato, che può utilizzare o superare il limite di licenza. Per elencare le variabili di progetto: `magento-cloud variable:list --level project`

1. Elimina la variabile di licenza.

   ```bash
   magento-cloud variable:delete php:newrelic.license
   ```
