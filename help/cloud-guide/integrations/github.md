---
title: Integrazione GitHub
description: Scopri come integrare il progetto di infrastruttura cloud Adobe Commerce con GitHub.
feature: Cloud, Integration
last-substantial-update: 2023-05-25T00:00:00Z
exl-id: 5305452f-4c8d-438c-ac78-e2e1ec2f8cd9
source-git-commit: b162faadf114423a7da23ecf94c96eccbcaeb72b
workflow-type: tm+mt
source-wordcount: '949'
ht-degree: 0%

---

# Integrazione GitHub

L’integrazione GitHub consente di gestire Adobe Commerce negli ambienti dell’infrastruttura cloud direttamente dall’archivio GitHub. L’integrazione gestisce i contenuti già in GitHub e si sincronizza con il tuo Adobe Commerce nell’archivio del codice dell’infrastruttura cloud. In sostanza, l’archivio del codice diventa un mirror dell’archivio GitHub.

{{private-repository}}

Questa integrazione consente di:

- Creare un ambiente quando si crea un ramo
- Ridistribuire l’ambiente quando si unisce una richiesta di pull
- Elimina l’ambiente quando elimini il ramo

Per continuare il processo, è necessario ottenere un token GitHub e un webhook.

## Prerequisiti

- Accesso amministratore al progetto di infrastruttura cloud Adobe Commerce on
- Archivio GitHub
- Token di accesso personale GitHub

## Generare un token GitHub

Crea un token di accesso personale classico nelle impostazioni per gli sviluppatori GitHub. Devi essere membro di un gruppo con accesso in scrittura all&#39;archivio GitHub, in modo da poter _inviare_ all&#39;archivio. Includi i seguenti ambiti durante la creazione del token:

- `admin:repo_hook` - Crea hook Web
- `repo`: integrazione con l&#39;archivio
- `read:org`: integrazione con il repository aziendale

Vedere [GitHub: Create](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

## Preparare l’archivio

Clona il progetto Adobe Commerce on cloud infrastructure da un ambiente esistente e migra i rami del progetto in un nuovo archivio GitHub vuoto, mantenendo gli stessi nomi di ramo. È **fondamentale** mantenere una struttura Git identica, in modo da non perdere ambienti o rami esistenti nel progetto Adobe Commerce su infrastruttura cloud.

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
   magento-cloud project:get <project-ID>
   ```

1. Aggiungi l’archivio GitHub come remoto.

   ```bash
   git remote add origin git@github.com:<user-name>/<repo-name>.git
   ```

   Il nome predefinito per la connessione remota può essere `origin` o `magento`. Se `origin` esiste, è possibile scegliere un nome diverso oppure rinominare o eliminare il riferimento esistente. Consulta la [documentazione Git-Remote](https://git-scm.com/docs/git-remote).

1. Verifica di aver aggiunto correttamente il remoto GitHub.

   ```bash
   git remote -v
   ```

   Risposta prevista:

   ```
   origin git@github.com:<user-name>/<repo-name>.git (fetch)
   origin git@github.com:<user-name>/<repo-name>.git (push)
   ```

1. Invia i file di progetto al nuovo archivio GitHub. Ricordati di mantenere invariati tutti i nomi dei rami.

   ```bash
   git push -u origin master
   ```

   Se si sta iniziando con un nuovo archivio GitHub, potrebbe essere necessario utilizzare l&#39;opzione `-f`, perché l&#39;archivio remoto non corrisponde alla copia locale.

1. Verifica che l’archivio GitHub contenga tutti i file di progetto.

## Abilitare l’integrazione con GitHub

Prima di iniziare, il codice del progetto e gli ambienti devono trovarsi nell’archivio GitHub. Dopo aver abilitato l’integrazione, l’archivio GitHub diventa l’origine del codice. Se si inviano modifiche al codice all&#39;archivio `magento` originale, queste vengono sovrascritte dall&#39;integrazione quando si inviano modifiche al codice nell&#39;archivio GitHub.

Di seguito viene abilitata l’integrazione GitHub e viene fornito un URL di payload da utilizzare durante la creazione di un webhook.

>[!WARNING]
>
>Il comando seguente sovrascrive il codice _all_ nel progetto di infrastruttura cloud di Adobe Commerce con il codice dell&#39;archivio GitHub, che include tutti i rami, incluso il ramo `production`. Questa azione si verifica immediatamente e non può essere annullata. Come best practice, è importante clonare tutti i rami dal progetto Adobe Commerce on cloud infrastructure e inviarli all&#39;archivio GitHub **prima** dell&#39;aggiunta dell&#39;integrazione GitHub.

È possibile scegliere di esaminare i prompt CLI utilizzando `magento-cloud integration:add` oppure creare il comando di integrazione con le opzioni seguenti:

| Opzione | Obbligatorio | Descrizione |
| ----------------------- | --------- | --------------------------------- |
| `--base-url` | Sì | URL di base dell&#39;installazione del server, che può essere `https://github.com/` o personalizzato. Ometti questa opzione se l’archivio è ospitato con Github pubblico o se l’archivio non è ospitato su server privati. Ometti questa opzione se l&#39;URL dell&#39;archivio è simile a `https://github.com/{account}/{repository-name}`. Questo può causare errori come `Unable to connect to GitHub: repository not found`. |
| `--token` | Sì | Token di accesso personale generato per GitHub |
| `--repository` | Sì | Nome repository: `owner-or-organisation/repository` |
| `--build-pull-requests` | Facoltativo | Indica ad Adobe Commerce su infrastruttura cloud di distribuire dopo l&#39;unione di una richiesta di pull (`true` per impostazione predefinita) |
| `--fetch-branches` | Facoltativo | Fa sì che Adobe Commerce su infrastruttura cloud tenga traccia dei rami e distribuisca dopo l&#39;aggiornamento di un ramo (`true` per impostazione predefinita) |
| `--prune-branches` | Facoltativo | Elimina rami inesistenti nel remoto (`true` per impostazione predefinita) |

Sono disponibili molte altre opzioni che possono essere visualizzate tramite l’opzione di aiuto:

```bash
magento-cloud integration:add --help
```

**Per abilitare l&#39;integrazione GitHub**:

1. Abilita l’integrazione.

   ```bash
   magento-cloud integration:add --type=github --project=<project-ID> --token=<your-GitHub-token> {--repository=USER/REPOSITORY | --repository=ORGANIZATION/REPOSITORY} [--build-pull-requests={true|false} --fetch-branches={true|false}
   ```

   **Esempio 1**: abilita l&#39;integrazione GitHub per un archivio personale privato:

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=myUserName/myrepo
   ```

   **Esempio 2**: abilitare l&#39;integrazione GitHub per un repository dell&#39;organizzazione:

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=Magento/teamrepo
   ```

1. Immetti le informazioni richieste quando richiesto.

1. Copia l&#39;**URL payload** visualizzato dall&#39;output restituito.

   ```
   Created integration <integration-ID> (type: github)
   Repository: myUserName/myrepo
   Build PRs: yes
   Fetch branches: yes
   Payload URL: https://us.magento.cloud/api/projects/<project-id>/integrations/wO8a0eoamxwcg/hook
   ```

## Aggiungere il webhook in GitHub

Per comunicare eventi, come un push, con il server Cloud Git, devi creare un webhook per l’archivio GitHub:

1. Nell&#39;archivio GitHub, fai clic sulla scheda **Impostazioni**.

1. Nella barra di navigazione a sinistra, fai clic su **Webhook**.

1. Nel riquadro _Webhook_ fare clic su **Aggiungi webhook**.

1. Nel modulo _Webhook/Aggiungi webhook_, modifica i campi seguenti:

   - **URL payload**: immetti l&#39;URL restituito quando abiliti l&#39;integrazione GitHub.
   - **Tipo di contenuto**: scegli **application/json** dall&#39;elenco.
   - **Segreto**: immetti un segreto di verifica.
   - **Quali eventi attivare questo webhook?**: Seleziona **Invia tutto**.
   - Selezionare la casella di controllo **Attivo**.

1. Fai clic su **Aggiungi webhook**.

## Testare l’integrazione

Dopo aver configurato l&#39;integrazione GitHub, è possibile verificare che l&#39;integrazione sia operativa utilizzando CLI `magento-cloud`:

```bash
magento-cloud integration:validate
```

Oppure puoi testarlo inviando una semplice modifica all’archivio GitHub.

1. Creare un file di test.

   ```bash
   touch test.md
   ```

1. Esegui il commit e invia la modifica all’archivio GitHub.

   ```bash
   git add . && git commit -m "Testing GitHub integration" && git push
   ```

1. Accedi a [[!DNL Cloud Console]](../project/overview.md) e verifica che il messaggio di commit sia visualizzato e che il progetto sia in fase di distribuzione.

## Rimuovere l’integrazione

Puoi rimuovere in modo sicuro l’integrazione GitHub dal progetto senza influire sul codice.

**Per rimuovere l&#39;integrazione GitHub**:

1. Dal terminale, accedi al tuo progetto di infrastruttura cloud Adobe Commerce on.

1. Elencare le integrazioni. Per completare il passaggio successivo, è necessario l’ID integrazione GitHub.

   ```bash
   magento-cloud integration:list
   ```

1. Elimina l’integrazione.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Inoltre, puoi rimuovere l&#39;integrazione GitHub accedendo al tuo account GitHub e rimuovendo l&#39;hook Web nella scheda _Webhook_ dell&#39;archivio _Impostazioni_.
