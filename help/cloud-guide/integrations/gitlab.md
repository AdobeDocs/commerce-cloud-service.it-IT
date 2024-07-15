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
- Strumento [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md) nell&#39;ambiente locale
- Un account GitLab
- Token di accesso personale GitLab con accesso in scrittura all&#39;archivio GitLab. Gli ambiti selezionati devono essere almeno: `api` e `read_repository`.

## Preparare l’archivio

Clona il progetto Adobe Commerce on cloud infrastructure da un ambiente esistente e migra i rami del progetto in un nuovo archivio GitLab vuoto, mantenendo gli stessi nomi di ramo. È **fondamentale** mantenere una struttura Git identica, in modo da non perdere ambienti o rami esistenti nel progetto Adobe Commerce su infrastruttura cloud.

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

   Il nome predefinito per la connessione remota può essere `origin` o `magento`. Se `origin` esiste, è possibile scegliere un nome diverso oppure rinominare o eliminare il riferimento esistente. Consulta la [documentazione Git-Remote](https://git-scm.com/docs/git-remote).

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

   Se si sta iniziando con un nuovo archivio GitLab, potrebbe essere necessario utilizzare l&#39;opzione `-f`, perché l&#39;archivio remoto non corrisponde alla copia locale.

1. Verifica che l’archivio GitLab contenga tutti i file di progetto.

## Abilitare l’integrazione con GitLab

Utilizza il comando `magento-cloud integration` per abilitare l&#39;integrazione GitLab e ottieni l&#39;URL di payload per il webhook GitLab per inviare aggiornamenti da GitLab al tuo progetto di infrastruttura cloud Adobe Commerce.

```bash
magento-cloud integration:add --type=gitlab --project=<project-ID> --token=<your-GitLab-token> [--base-url=<GitLab-url> --server-project=<GitLab-project> --build-merge-requests={true|false} --merge-requests-clone-parent-data={true|false} --fetch-branches={true|false} --prune-branches={true|false}]
```

| Opzione | Descrizione |
| ------ | ----------- |
| `<project-ID>` | ID progetto Adobe Commerce su infrastruttura cloud |
| `<your-GitLab-token>` | Token di accesso personale generato per GitLab |
| `--base-url` | URL di GitLab (`https://gitlab.com/` se GitLab è utilizzato nella versione SaaS) |
| `--server-project` | Nome del progetto in GitLab (parte dopo l’URL di base) |
| `--build-merge-requests` | Un parametro _facoltativo_ che indica ad Adobe Commerce sull&#39;infrastruttura cloud di creare un nuovo ambiente per ogni richiesta di unione (`true` per impostazione predefinita) |
| `--merge-requests-clone-parent-data` | Parametro _optional_ che indica ad Adobe Commerce sull&#39;infrastruttura cloud di clonare i dati dell&#39;ambiente padre per le richieste di unione (`true` per impostazione predefinita) |
| `--fetch-branches` | Un parametro _facoltativo_ che fa in modo che Adobe Commerce sull&#39;infrastruttura cloud recuperi tutti i rami dal remoto (come ambienti inattivi) (`true` per impostazione predefinita) |
| `--prune-branches` | Parametro _optional_ che indica ad Adobe Commerce sull&#39;infrastruttura cloud di eliminare i rami che non esistono nel remoto (`true` per impostazione predefinita) |

>[!WARNING]
>
>Il comando `magento-cloud integration` sovrascrive il codice _all_ nel progetto di infrastruttura cloud di Adobe Commerce con il codice dell&#39;archivio GitLab. Sono inclusi tutti i rami, incluso il ramo `production`. Questa azione si verifica immediatamente e non può essere annullata. Come best practice, è importante clonare tutti i rami dal progetto Adobe Commerce on cloud infrastructure e inviarli all’archivio GitLab prima di aggiungere l’integrazione GitLab.

**Per abilitare l&#39;integrazione GitLab**:

1. Dal terminale, aggiungi l’integrazione GitLab al tuo progetto di infrastruttura cloud Adobe Commerce on:

   ```bash
   magento-cloud integration:add --type gitlab --project=3txxjf32gtryos --token=qVUfeEn4ouze7A7JH --base-url=https://gitlab.com/ --server-project=my-agency/project-name --build-merge-requests=false --merge-requests-clone-parent-data=false --fetch-branches=true --prune-branches=true
   ```

1. Quando richiesto, immetti `y` per aggiungere l&#39;integrazione.

   ```terminal
   Warning: adding a 'gitlab' integration will automatically synchronize code from the external Git repository.
   This means it can overwrite all the code in your project.
   Are you sure you want to continue? [y/N] y
   ```

1. Copia l&#39;**URL hook** visualizzato dall&#39;output restituito.

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

Per comunicare eventi, ad esempio richieste push o merge, con il server Git Cloud, è necessario [creare un webhook](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#overview) per l&#39;archivio GitLab

1. Nell&#39;archivio GitLab fare clic sulla scheda **Impostazioni**.

1. Nella barra di navigazione a sinistra, fai clic su **Webhook**.

1. Nel modulo _Webhook_, modifica i campi seguenti:

   - **URL**: immetti `Hook URL` restituito quando abiliti l&#39;integrazione GitLab.
   - **Token segreto**: immetti un segreto di verifica, se necessario.
   - **Attivatore**: seleziona `Merge request events` e/o `Push events` a seconda delle tue esigenze.
   - **Abilita verifica SSL**: selezionare questa opzione.

1. Fai clic su **Aggiungi webhook**.

### Testare l’integrazione

Dopo aver configurato l&#39;integrazione GitLab, è possibile verificare che l&#39;integrazione sia operativa utilizzando l&#39;interfaccia della riga di comando `magento-cloud`:

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

1. Accedi a [[!DNL Cloud Console]](../project/overview.md) e verifica che il messaggio di commit sia visualizzato e che il progetto sia in fase di distribuzione.

## Creare un ramo Cloud

Utilizzare il comando `magento-cloud` CLI `environment:push` per creare e attivare un nuovo ambiente. Consulta [Creare un ramo cloud](bitbucket.md#create-a-cloud-branch).

## Rimuovere l’integrazione

Utilizzare il comando `magento-cloud` CLI `integration:delete` per rimuovere l&#39;integrazione. Vedi [Rimuovi l&#39;integrazione](bitbucket.md#remove-the-integration).
