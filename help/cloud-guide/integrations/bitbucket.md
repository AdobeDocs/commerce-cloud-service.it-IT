---
title: Integrazione bitbucket
description: Scopri come integrare il progetto di infrastruttura cloud Adobe Commerce con Bitbucket.
feature: Cloud, Integration
exl-id: cd3cffbe-268f-429b-a2ea-0306159f4a6b
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# Integrazione bitbucket

È possibile configurare l’archivio Bitbucket in modo da generare e distribuire automaticamente un ambiente quando si inviano modifiche al codice. Questa integrazione sincronizza l’archivio Bitbucket con l’account Adobe Commerce sull’infrastruttura cloud.

{{private-repository}}

## Prerequisiti

- Accesso amministratore al progetto di infrastruttura cloud Adobe Commerce on
- Strumento [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md) nell&#39;ambiente locale
- Un account Bitbucket
- Accesso amministratore all’archivio Bitbucket
- Una chiave di accesso SSH per l’archivio Bitbucket

## Preparare l’archivio

Clona il progetto Adobe Commerce on Cloud Infrastructure da un ambiente esistente e migra i rami del progetto in un nuovo archivio Bitbucket vuoto, mantenendo gli stessi nomi di ramo. È **fondamentale** mantenere una struttura Git identica, in modo da non perdere ambienti o rami esistenti nel progetto Adobe Commerce su infrastruttura cloud.

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

1. Aggiungi l’archivio Bitbucket come archivio remoto.

   ```bash
   git remote add origin git@bitbucket.org:<user-name>/<repo-name>.git
   ```

   Il nome predefinito per la connessione remota può essere `origin` o `magento`. Se `origin` esiste, è possibile scegliere un nome diverso oppure rinominare o eliminare il riferimento esistente. Consulta la [documentazione Git-Remote](https://git-scm.com/docs/git-remote).

1. Verificare di aver aggiunto correttamente il telecomando Bitbucket.

   ```bash
   git remote -v
   ```

   Risposta prevista:

   ```
   origin git@bitbucket.org:<user-name>/<repo-name>.git (fetch)
   origin git@bitbucket.org:<user-name>/<repo-name>.git (push)
   ```

1. Invia i file di progetto al nuovo archivio Bitbucket. Ricordati di mantenere invariati tutti i nomi dei rami.

   ```bash
   git push -u origin master
   ```

   Se si sta iniziando con un nuovo archivio Bitbucket, potrebbe essere necessario utilizzare l&#39;opzione `-f`, perché l&#39;archivio remoto non corrisponde alla copia locale.

1. Verifica che l’archivio Bitbucket contenga tutti i file di progetto.

## Creare un consumatore OAuth

L&#39;integrazione di Bitbucket richiede un consumer [OAuth](https://support.atlassian.com/bitbucket-cloud/docs/use-oauth-on-bitbucket-cloud/). Per completare la sezione successiva, sono necessari gli OAuth `key` e `secret` di questo consumer.

**Per creare un consumer OAuth in Bitbucket**:

1. Accedi al tuo account [Bitbucket](https://id.atlassian.com/login).

1. Fai clic su **Impostazioni** > **Gestione degli accessi** > **OAuth**.

1. Fare clic su **Aggiungi consumer** e configurarlo come segue:

   ![Configurazione consumer OAuth Bitbucket](../../assets/oauth-consumer-config.png)

   >[!WARNING]
   >
   >Non è necessario un **URL di richiamata** valido, ma è necessario immettere un valore in questo campo per completare correttamente l&#39;integrazione.

1. Fai clic su **Salva**.

1. Fai clic sul consumatore **Nome** per visualizzare la tua OAuth `key` e `secret`.

1. Copia il tuo OAuth `key` e `secret` per configurare l&#39;integrazione.

## Configurare l’integrazione

1. Dal terminale, accedi al tuo progetto di infrastruttura cloud Adobe Commerce on.

1. Creare un file temporaneo denominato `bitbucket.json` e aggiungere quanto segue, sostituendo le variabili tra parentesi angolari con i valori:

   ```json
   {
     "type": "bitbucket",
     "repository": "<bitbucket-user-name/bitbucket-repo-name>",
     "app_credentials": {
       "key": "<oauth-consumer-key>",
       "secret": "<oauth-consumer-secret>"
     },
     "prune_branches": true,
     "fetch_branches": true,
     "build_pull_requests": true,
     "resync_pull_requests": true
   }
   ```

   >[!TIP]
   >
   >Assicurati di utilizzare il nome dell’archivio Bitbucket e non l’URL. L’integrazione non riesce se utilizzi un URL.

1. Aggiungere l&#39;integrazione al progetto utilizzando lo strumento CLI `magento-cloud`.

   >[!WARNING]
   >
   >Il comando seguente sovrascrive il codice _all_ nel progetto di infrastruttura cloud di Adobe Commerce con il codice dell&#39;archivio Bitbucket. Sono inclusi tutti i rami, incluso il ramo `production`. Questa azione si verifica immediatamente e non può essere annullata. Come best practice, è importante clonare tutti i rami dal progetto di infrastruttura cloud Adobe Commerce on e inviarli all&#39;archivio Bitbucket **prima** dell&#39;aggiunta dell&#39;integrazione Bitbucket.

   ```bash
   magento-cloud project:curl -p '<project-ID>' /integrations -i -X POST -d "$(< bitbucket.json)"
   ```

   Restituisce una risposta HTTP lunga con intestazioni. In caso di esito positivo, l’integrazione restituisce il codice di stato 200 o 201. Lo stato 400 o superiore indica che si è verificato un errore.

1. Eliminare il file `bitbucket.json` temporaneo.

1. Verifica l’integrazione del progetto.

   ```bash
   magento-cloud integrations -p <project-ID>
   ```

   ```
   +----------+-----------+--------------------------------------------------------------------------------+
   | ID       | Type      | Summary                                                                        |
   +----------+-----------+--------------------------------------------------------------------------------+
   | <int-id> | bitbucket | Repository: bitbucket_Account/magento-int                                      |
   |          |           | Hook URL:                                                                      |
   |          |           | https://magento-url.cloud/api/projects/<project-id>/integrations/<int-id>/hook |
   +----------+-----------+--------------------------------------------------------------------------------+
   ```

   Prendere nota dell&#39;**URL hook** per configurare un webhook in BitBucket.

### Aggiungere un webhook in BitBucket

Per comunicare eventi, come un push, con il server Cloud Git, è necessario disporre di un webhook per l’archivio BitBucket. Il metodo di impostazione di un’integrazione Bitbucket descritto in questa pagina, se seguito correttamente, crea automaticamente un webhook. È importante verificare il webhook per evitare la creazione di più integrazioni.

1. Accedi al tuo account [Bitbucket](https://id.atlassian.com/login).

1. Fai clic su **Archivi** e seleziona il progetto.

1. Fai clic su **Impostazioni archivio** > **Flusso di lavoro** > **Webhook**.

1. Verifica il webhook prima di continuare.

   Se l&#39;hook è attivo, salta i passaggi rimanenti e [verifica l&#39;integrazione](#test-the-integration). L&#39;hook deve avere un nome simile a **&quot;Adobe Commerce on cloud infrastructure &lt;project_id>&quot;** e un formato URL hook simile a: `https://<zone>.magento.cloud/api/projects/<project_id>/integrations/<id>/hook`

1. Fai clic su **Aggiungi webhook**.

1. Nella visualizzazione _Aggiungi nuovo webhook_, modifica i campi seguenti:

   - **Titolo**: Integrazione Adobe Commerce
   - **URL**: utilizza l&#39;URL hook dall&#39;elenco di integrazione `magento-cloud`
   - **Triggers**: il valore predefinito è un _push di base dell&#39;archivio_

1. Fai clic su **Salva**.

### Testare l’integrazione

Dopo aver configurato l&#39;integrazione Bitbucket, è possibile verificare che l&#39;integrazione sia operativa utilizzando CLI `magento-cloud`:

```bash
magento-cloud integration:validate
```

Oppure puoi testarlo inviando una semplice modifica all’archivio Bitbucket.

1. Creare un file di test.

   ```bash
   touch test.md
   ```

1. Esegui il commit e invia la modifica all’archivio Bitbucket.

   ```bash
   git add . && git commit -m "Testing Bitbucket integration" && git push
   ```

1. Accedi a [[!DNL Cloud Console]](../project/overview.md) e verifica che il messaggio di commit sia visualizzato e che il progetto sia in fase di distribuzione.

   ![Verifica dell&#39;integrazione di Bitbucket](../../assets/bitbucket-integration.png)

## Creare un ramo Cloud

L’integrazione Bitbucket non può attivare nuovi ambienti nel progetto di infrastruttura cloud Adobe Commerce on. Se crei un ambiente con Bitbucket, devi attivarlo manualmente. Per evitare questo passaggio aggiuntivo, è consigliabile creare ambienti utilizzando lo strumento CLI `magento-cloud` o [!DNL Cloud Console].

**Per attivare un ramo creato con Bitbucket**:

1. Utilizzare l&#39;interfaccia della riga di comando `magento-cloud` per inviare il ramo.

   ```bash
   magento-cloud environment:push from-bitbucket
   ```

   ```
   Pushing from-bitbucket to the new environment from-bitbucket
   Activate from-bitbucket after pushing? [Y/n] y
   Parent environment [master]: integration
   --- (Validation and activation messages)
   ```

1. Verifica che l’ambiente sia attivo.

   ```bash
   magento-cloud environment:list
   ```

   ```
   Your environments are:
   +---------------------+----------------+--------+
   | ID                  | Name           | Status |
   +---------------------+----------------+--------+
   | master              | Master         | Active |
   |  integration        | integration    | Active |
   |    from-bitbucket * | from-bitbucket | Active |
   +---------------------+----------------+--------+
   * - Indicates the current environment
   ```

Dopo aver creato un ambiente, puoi inviare il ramo corrispondente all’archivio Bitbucket remoto utilizzando i normali comandi Git. Modifiche successive al ramo in Bitbucket generano e distribuiscono automaticamente l’ambiente.

## Rimuovere l’integrazione

Puoi rimuovere in modo sicuro l’integrazione Bitbucket dal progetto senza influire sul codice.

**Per rimuovere l&#39;integrazione di Bitbucket**:

1. Dal terminale, accedi al tuo progetto di infrastruttura cloud Adobe Commerce on.

1. Elencare le integrazioni. Per completare il passaggio successivo, è necessario disporre dell’ID integrazione Bitbucket.

   ```bash
   magento-cloud integration:list
   ```

1. Elimina l’integrazione.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Inoltre, puoi rimuovere l&#39;integrazione Bitbucket accedendo al tuo account Bitbucket e revocando la concessione OAuth nella pagina _Impostazioni_ dell&#39;account.

## Integrazione del server bitbucket

Per utilizzare l’integrazione con il server Bitbucket, è necessario quanto segue:

- [Token di accesso Bitbucket](https://confluence.atlassian.com/bitbucketserver/http-access-tokens-939515499.html) - Genera un token che concede l&#39;accesso al progetto `read` e all&#39;archivio `admin`
- [URL del server Bitbucket](https://confluence.atlassian.com/bitbucketserver/specify-the-bitbucket-base-url-776640392.html) - Aggiungi l&#39;URL di base dell&#39;istanza Bitbucket

Sebbene sia possibile utilizzare Cloud CLI per eseguire i passaggi di integrazione del server Bitbucket, il comando completo è simile al seguente:

```bash
magento-cloud integration:add --type=bitbucket_server --base-url=<bitbucket-url> --username=<username> --token=<bitbucket-access-token> --project=<project-ID>
```

Utilizzare il comando help per ulteriori requisiti di utilizzo e opzioni: `magento-cloud integration:add --help`
