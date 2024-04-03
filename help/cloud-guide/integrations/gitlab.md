---
title: Integrazione con GitLab
description: Scopri come integrare il progetto di infrastruttura cloud Adobe Commerce con GitLab.
feature: Cloud, Integration
exl-id: 37fda8a0-7274-422f-9049-243f2e409f26
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '777'
ht-degree: 0%

---

# Integrazione con GitLab

Puoi configurare un archivio GitLab per generare e distribuire automaticamente un ambiente quando invii modifiche al codice. Questa integrazione sincronizza l’archivio GitLab con l’account Adobe Commerce sull’infrastruttura cloud.

{{private-repository}}

Questa integrazione consente di:

- Creare un ambiente quando si crea un ramo
- Ridistribuire l’ambiente quando si unisce una richiesta di pull
- Elimina l’ambiente quando elimini il ramo

Per continuare il processo, è necessario ottenere un token GitLab e un webhook.

## Prerequisiti

- Accesso amministratore al progetto di infrastruttura cloud Adobe Commerce on
- [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md) nell&#39;ambiente locale
- Un account GitLab
- Un token di accesso personale GitLab con accesso in scrittura all’archivio GitLab, gli ambiti selezionati devono essere almeno: `api` e `read_repository`.

## Preparare l’archivio

Clona il progetto Adobe Commerce on cloud infrastructure da un ambiente esistente e migra i rami del progetto in un nuovo archivio GitLab vuoto, mantenendo gli stessi nomi di ramo. È **critico** per mantenere una struttura Git identica, in modo da non perdere ambienti o rami esistenti nel progetto Adobe Commerce on cloud infrastructure.

1. Dal terminale, accedi al tuo progetto di infrastruttura cloud Adobe Commerce on.

   ```bash
   magento-cloud login
   ```

1. Elencare i progetti e copiare l’ID del progetto.

   ```bash
   magento-cloud project:list
   ```

1. Clona il progetto nell’ambiente locale.

   ```bash
   magento-cloud project:get <project-id>
   ```

1. Aggiungere l’archivio GitLab come remoto (supponendo che GitLab sia utilizzato nella versione SaaS).

   ```bash
   git remote add origin git@gitlab.com:<user-name>/<repo-name>.git
   ```

   Il nome predefinito per la connessione remota può essere `origin` o `magento`. Se `origin` esiste, è possibile scegliere un nome diverso oppure rinominare o eliminare il riferimento esistente. Consulta [documentazione di git-remote](https://git-scm.com/docs/git-remote).

1. Verificare di aver aggiunto correttamente il telecomando GitLab.

   ```bash
   git remote -v
   ```

   Risposta prevista:

   ```terminal
   origin git@gitlab.com:<user-name>/<repo-name>.git (fetch)
   origin git@gitlab.com:<user-name>/<repo-name>.git (push)
   ```

1. Invia i file del progetto al nuovo archivio GitLab. Ricordati di mantenere invariati tutti i nomi dei rami.

   ```bash
   git push -u origin master
   ```

   Se si inizia con un nuovo archivio GitLab, potrebbe essere necessario utilizzare `-f` perché l’archivio remoto non corrisponde alla copia locale.

1. Verifica che l’archivio GitLab contenga tutti i file di progetto.

## Abilitare l’integrazione con GitLab

Utilizza il `magento-cloud integration` comando per abilitare l’integrazione con GitLab e ottenere l’URL del payload per il webhook GitLab per inviare aggiornamenti da GitLab al progetto di infrastruttura cloud di Adobe Commerce.

```bash
magento-cloud integration:add --type=gitlab --project=<project-ID> --token=<your-GitLab-token> [--base-url=<GitLab-url> --server-project=<GitLab-project> --build-merge-requests={true|false} --merge-requests-clone-parent-data={true|false} --fetch-branches={true|false} --prune-branches={true|false}]
```

| Opzione | Descrizione |
| ------ | ----------- |
| `<project-ID>` | ID progetto Adobe Commerce su infrastruttura cloud |
| `<your-GitLab-token>` | Token di accesso personale generato per GitLab |
| `--base-url` | URL di GitLab (`https://gitlab.com/` se GitLab è utilizzato nella sua versione SaaS) |
| `--server-project` | Nome del progetto in GitLab (parte dopo l’URL di base) |
| `--build-merge-requests` | Un _facoltativo_ parametro che indica ad Adobe Commerce on cloud infrastructure di creare un nuovo ambiente per ogni richiesta di unione (`true` per impostazione predefinita) |
| `--merge-requests-clone-parent-data` | Un _facoltativo_ parametro che indica ad Adobe Commerce on cloud infrastructure di clonare i dati dell’ambiente principale per le richieste di unione (`true` per impostazione predefinita) |
| `--fetch-branches` | Un _facoltativo_ parametro che fa in modo che Adobe Commerce sull’infrastruttura cloud recuperi tutti i rami dal remoto (come ambienti inattivi) (`true` per impostazione predefinita) |
| `--prune-branches` | Un _facoltativo_ parametro che indica ad Adobe Commerce sull’infrastruttura cloud di eliminare i rami che non esistono in remoto (`true` per impostazione predefinita) |

>[!WARNING]
>
>Il `magento-cloud integration` sovrascrittura del comando _tutto_ codice nel progetto di infrastruttura cloud Adobe Commerce on con il codice proveniente dall’archivio GitLab. Ciò include tutti i rami, incluso `production` filiale. Questa azione si verifica immediatamente e non può essere annullata. Come best practice, è importante clonare tutti i rami dal progetto Adobe Commerce on cloud infrastructure e inviarli all’archivio GitLab prima di aggiungere l’integrazione GitLab.

**Per abilitare l’integrazione con GitLab**:

1. Dal terminale, aggiungi l’integrazione GitLab al tuo progetto di infrastruttura cloud Adobe Commerce on:

   ```bash
   magento-cloud integration:add --type gitlab --project=3txxjf32gtryos --token=qVUfeEn4ouze7A7JH --base-url=https://gitlab.com/ --server-project=my-agency/project-name --build-merge-requests=false --merge-requests-clone-parent-data=false --fetch-branches=true --prune-branches=true
   ```

1. Quando richiesto, immetti `y` per aggiungere l’integrazione.

   ```terminal
   Warning: adding a 'gitlab' integration will automatically synchronize code from the external Git repository.
   This means it can overwrite all the code in your project.
   Are you sure you want to continue? [y/N] y
   ```

1. Copia il **URL hook** visualizzato dall&#39;output di ritorno.

   ```terminal
   Hook URL: https://eu-3.magento.cloud/api/projects/3txxjf32gtryos/integrations/eolmpfizzg9lu/hook
   Created integration eolmpfizzg9lu (type: gitlab)
   +----------------------------------+---------------------------------------------------------------------------------------+
   | Property                         | Value                                                                                 |
   +----------------------------------+---------------------------------------------------------------------------------------+
   | id                               | <integration-id>                                                                      |
   | type                             | gitlab                                                                                |
   | token                            | ******                                                                                |
   | base_url                         | https://gitlab.com/                                                                   |
   | project                          | my-agency/project-name                                                                |
   | fetch_branches                   | true                                                                                  |
   | prune_branches                   | true                                                                                  |
   | build_merge_requests             | false                                                                                 |
   | merge_requests_clone_parent_data | false                                                                                 |
   | hook_url                         | https://eu-3.magento.cloud/api/projects/<project-id>/integrations/<integration-id>/hook |
   +----------------------------------+---------------------------------------------------------------------------------------+
   ```

### Aggiungere il webhook in GitLab

Per comunicare eventi, come richieste push o di unione, con il server Cloud Git, devi [creare un webhook](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#overview) per l’archivio GitLab

1. Nell’archivio GitLab, fai clic su **Impostazioni** scheda.

1. Nella barra di navigazione a sinistra, fai clic su **Webhook**.

1. In _Webhook_ , modificare i campi seguenti:

   - **URL**: immetti il `Hook URL` restituita quando hai abilitato l’integrazione GitLab.
   - **Token segreto**: immetti un segreto di verifica, se necessario.
   - **Trigger**: spunta `Merge request events` e/o `Push events` in base alle tue esigenze.
   - **Abilita verifica SSL**: seleziona questa opzione.

1. Clic **Aggiungi webhook**.

### Testare l’integrazione

Dopo aver configurato l’integrazione GitLab, puoi verificare che questa sia operativa utilizzando `magento-cloud` CLI:

```bash
magento-cloud integration:validate
```

Oppure puoi testarlo inviando una semplice modifica all’archivio GitLab.

1. Creare un file di test.

   ```bash
   touch test.md
   ```

1. Esegui il commit e invia la modifica all’archivio GitLab.

   ```bash
   git add . && git commit -m "Testing GitLab integration" && git push
   ```

1. Accedi a [[!DNL Cloud Console]](../project/overview.md) e verifica che venga visualizzato il messaggio di conferma e che venga distribuito il progetto.

## Creare un ramo Cloud

Utilizza il `magento-cloud` CLI `environment:push` per creare e attivare un nuovo ambiente. Consulta [Creare un ramo Cloud](bitbucket.md#create-a-cloud-branch).

## Rimuovere l’integrazione

Utilizza il `magento-cloud` CLI `integration:delete` per rimuovere l&#39;integrazione. Consulta [Rimuovere l’integrazione](bitbucket.md#remove-the-integration).
