---
title: Integrazione GitHub
description: Scopri come integrare il progetto di infrastruttura cloud Adobe Commerce con GitHub.
feature: Cloud, Integration
last-substantial-update: 2023-05-25T00:00:00Z
exl-id: 5305452f-4c8d-438c-ac78-e2e1ec2f8cd9
source-git-commit: 4d32fc596064f01eaefe3ee509a655837fa846de
workflow-type: tm+mt
source-wordcount: '924'
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

Crea un token di accesso personale classico nelle impostazioni per gli sviluppatori GitHub. Devi essere membro di un gruppo con accesso in scrittura all’archivio GitHub, in modo da poter _push_ all’archivio. Includi i seguenti ambiti durante la creazione del token:

- `admin:repo_hook`- Creazione di hook Web
- `repo`- Integrazione con l&#39;archivio
- `read:org`integrazione con l&#39;archivio aziendale

Consulta [GitHub: Crea](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).

## Preparare l’archivio

Clona il progetto Adobe Commerce on cloud infrastructure da un ambiente esistente e migra i rami del progetto in un nuovo archivio GitHub vuoto, mantenendo gli stessi nomi di ramo. È **critico** per mantenere una struttura Git identica, in modo da non perdere ambienti o rami esistenti nel progetto Adobe Commerce on cloud infrastructure.

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

   Il nome predefinito per la connessione remota può essere `origin` o `magento`. Se `origin` esiste, è possibile scegliere un nome diverso oppure rinominare o eliminare il riferimento esistente. Consulta [documentazione di git-remote](https://git-scm.com/docs/git-remote).

1. Verifica di aver aggiunto correttamente il remoto GitHub.

   ```bash
   git remote -v
   ```

   Risposta prevista:

   ```terminal
   origin git@github.com:<user-name>/<repo-name>.git (fetch)
   origin git@github.com:<user-name>/<repo-name>.git (push)
   ```

1. Invia i file di progetto al nuovo archivio GitHub. Ricordati di mantenere invariati tutti i nomi dei rami.

   ```bash
   git push -u origin master
   ```

   Se inizi con un nuovo archivio GitHub, potrebbe essere necessario utilizzare `-f` perché l’archivio remoto non corrisponde alla copia locale.

1. Verifica che l’archivio GitHub contenga tutti i file di progetto.

## Abilitare l’integrazione con GitHub

Prima di iniziare, il codice del progetto e gli ambienti devono trovarsi nell’archivio GitHub. Dopo aver abilitato l’integrazione, l’archivio GitHub diventa l’origine del codice. Se invii le modifiche al codice all’originale `magento` viene sovrascritto dall’integrazione quando si inviano le modifiche al codice all’archivio GitHub.

Di seguito viene abilitata l’integrazione GitHub e viene fornito un URL di payload da utilizzare durante la creazione di un webhook.

>[!WARNING]
>
>Il comando seguente sovrascrive _tutto_ codice nel progetto di infrastruttura cloud Adobe Commerce on con il codice dell’archivio GitHub, che include tutti i rami, incluso `production` filiale. Questa azione si verifica immediatamente e non può essere annullata. Come best practice, è importante clonare tutti i rami dal progetto Adobe Commerce on cloud infrastructure e inviarli all’archivio GitHub **prima di** aggiunta dell’integrazione GitHub.

È possibile scegliere di visualizzare i prompt CLI utilizzando `magento-cloud integration:add` oppure puoi creare il comando di integrazione con le seguenti opzioni:

| Opzione | Obbligatorio | Descrizione |
| ----------------------- | --------- | --------------------------------- |
| `--base-url` | Sì | L&#39;URL di base dell&#39;installazione del server, che può essere `https://github.com/` o un personalizzato. Ometti questa opzione se l’archivio è ospitato con Github pubblico. |
| `--token` | Sì | Token di accesso personale generato per GitHub |
| `--repository` | Sì | Il nome dell’archivio: `owner-or-organisation/repository` |
| `--build-pull-requests` | Facoltativo | Istruisce Adobe Commerce su infrastruttura cloud da implementare dopo l’unione di una richiesta di pull (`true` per impostazione predefinita) |
| `--fetch-branches` | Facoltativo | Fa sì che Adobe Commerce sull’infrastruttura cloud tenga traccia dei rami e lo distribuisca dopo l’aggiornamento di un ramo (`true` per impostazione predefinita) |
| `--prune-branches` | Facoltativo | Elimina i rami inesistenti nel remoto (`true` per impostazione predefinita) |

Sono disponibili molte altre opzioni che possono essere visualizzate tramite l’opzione di aiuto:

```bash
magento-cloud integration:add --help
```

**Per abilitare l’integrazione con GitHub**:

1. Abilita l’integrazione.

   ```bash
   magento-cloud integration:add --type=github --project=<project-ID> --token=<your-GitHub-token> {--repository=USER/REPOSITORY | --repository=ORGANIZATION/REPOSITORY} [--build-pull-requests={true|false} --fetch-branches={true|false}
   ```

   **Esempio 1**: abilita l’integrazione GitHub per un archivio personale e privato:

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=myUserName/myrepo
   ```

   **Esempio 2**: abilita l’integrazione GitHub per un archivio dell’organizzazione:

   ```bash
   magento-cloud integration:add --type=github --project=ov58dlacU2e --base-url=https://github.com --token=<token> --repository=Magento/teamrepo
   ```

1. Immetti le informazioni richieste quando richiesto.

1. Copia il **URL payload** visualizzato dall&#39;output di ritorno.

   ```terminal
   Created integration <integration-ID> (type: github)
   Repository: myUserName/myrepo
   Build PRs: yes
   Fetch branches: yes
   Payload URL: https://us.magento.cloud/api/projects/<project-id>/integrations/wO8a0eoamxwcg/hook
   ```

## Aggiungere il webhook in GitHub

Per comunicare eventi, come un push, con il server Cloud Git, devi creare un webhook per l’archivio GitHub:

1. Nell’archivio GitHub, fai clic su **Impostazioni** scheda.

1. Nella barra di navigazione a sinistra, fai clic su **Webhook**.

1. In _Webhook_ , fare clic su **Aggiungi webhook**.

1. In _Webhook/Aggiungi webhook_ , modificare i campi seguenti:

   - **URL payload**: inserisci l’URL restituito quando hai abilitato l’integrazione GitHub.
   - **Tipo di contenuto**: Scegli **application/json** dall&#39;elenco.
   - **Segreto**: immetti un segreto di verifica.
   - **Quali eventi si desidera attivare questo webhook?**: Seleziona **Mandami tutto**.
   - Seleziona la **Attivo** casella di controllo.

1. Clic **Aggiungi webhook**.

## Testare l’integrazione

Dopo aver configurato l’integrazione GitHub, puoi verificare che sia operativa utilizzando `magento-cloud` CLI:

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

1. Accedi a [[!DNL Cloud Console]](../project/overview.md) e verifica che venga visualizzato il messaggio di conferma e che venga distribuito il progetto.

## Rimuovere l’integrazione

Puoi rimuovere in modo sicuro l’integrazione GitHub dal progetto senza influire sul codice.

**Per rimuovere l’integrazione GitHub**:

1. Dal terminale, accedi al tuo progetto di infrastruttura cloud Adobe Commerce on.

1. Elencare le integrazioni. Per completare il passaggio successivo, è necessario l’ID integrazione GitHub.

   ```bash
   magento-cloud integration:list
   ```

1. Elimina l’integrazione.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Inoltre, puoi rimuovere l’integrazione GitHub accedendo al tuo account GitHub e rimuovendo l’hook web nel _Webhook_ scheda dell’archivio _Impostazioni_.
