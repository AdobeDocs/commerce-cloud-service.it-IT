---
title: Integrazione bitbucket
description: Scopri come integrare il progetto di infrastruttura cloud Adobe Commerce con Bitbucket.
feature: Cloud, Integration
exl-id: cd3cffbe-268f-429b-a2ea-0306159f4a6b
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 0%

---

# Integrazione bitbucket

È possibile configurare l’archivio Bitbucket in modo da generare e distribuire automaticamente un ambiente quando si inviano modifiche al codice. Questa integrazione sincronizza l’archivio Bitbucket con l’account Adobe Commerce sull’infrastruttura cloud.

{{private-repository}}

## Prerequisiti

- Accesso amministratore al progetto di infrastruttura cloud Adobe Commerce on
- [`magento-cloud` CLI](../dev-tools/cloud-cli-overview.md) nell&#39;ambiente locale
- Un account Bitbucket
- Accesso amministratore all’archivio Bitbucket
- Una chiave di accesso SSH per l’archivio Bitbucket

## Preparare l’archivio

Clona il progetto Adobe Commerce on Cloud Infrastructure da un ambiente esistente e migra i rami del progetto in un nuovo archivio Bitbucket vuoto, mantenendo gli stessi nomi di ramo. È **critico** per mantenere una struttura Git identica, in modo da non perdere ambienti o rami esistenti nel progetto Adobe Commerce on cloud infrastructure.

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

   Il nome predefinito per la connessione remota può essere `origin` o `magento`. Se `origin` esiste, è possibile scegliere un nome diverso oppure rinominare o eliminare il riferimento esistente. Consulta [documentazione di git-remote](https://git-scm.com/docs/git-remote).

1. Verificare di aver aggiunto correttamente il telecomando Bitbucket.

   ```bash
   git remote -v
   ```

   Risposta prevista:

   ```terminal
   origin git@bitbucket.org:<user-name>/<repo-name>.git (fetch)
   origin git@bitbucket.org:<user-name>/<repo-name>.git (push)
   ```

1. Invia i file di progetto al nuovo archivio Bitbucket. Ricordati di mantenere invariati tutti i nomi dei rami.

   ```bash
   git push -u origin master
   ```

   Se si inizia con un nuovo archivio Bitbucket, potrebbe essere necessario utilizzare `-f` perché l’archivio remoto non corrisponde alla copia locale.

1. Verifica che l’archivio Bitbucket contenga tutti i file di progetto.

## Creare un consumatore OAuth

L’integrazione di Bitbucket richiede un [Consumer OAuth](https://support.atlassian.com/bitbucket-cloud/docs/use-oauth-on-bitbucket-cloud/). Hai bisogno dell’OAuth `key` e `secret` da questo consumatore per completare la sezione successiva.

**Per creare un consumer OAuth in Bitbucket**:

1. Accedi al tuo [Bitbucket](https://id.atlassian.com/login) account.

1. Clic **Impostazioni** > **Gestione degli accessi** > **OAuth**.

1. Clic **Aggiungi consumatore** e configuralo come segue:

   ![Configurazione consumer OAuth Bitbucket](../../assets/oauth-consumer-config.png)

   >[!WARNING]
   >
   >Un valore valido **URL callback** non è obbligatorio, ma devi immettere un valore in questo campo per completare correttamente l’integrazione.

1. Clic **Salva**.

1. Fai clic sul consumatore **Nome** per visualizzare OAuth `key` e `secret`.

1. Copia OAuth `key` e `secret` per la configurazione dell’integrazione.

## Configurare l’integrazione

1. Dal terminale, accedi al tuo progetto di infrastruttura cloud Adobe Commerce on.

1. Crea un file temporaneo denominato `bitbucket.json` e aggiungi quanto segue, sostituendo le variabili tra parentesi angolari con i valori:

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

1. Aggiungi l’integrazione al progetto utilizzando `magento-cloud` CLI.

   >[!WARNING]
   >
   >Il comando seguente sovrascrive _tutto_ codice nel progetto di infrastruttura cloud Adobe Commerce on con il codice proveniente dall’archivio Bitbucket. Ciò include tutti i rami, incluso `production` filiale. Questa azione si verifica immediatamente e non può essere annullata. Come best practice, è importante clonare tutti i rami dal progetto Adobe Commerce on cloud infrastructure e inviarli all’archivio Bitbucket **prima di** aggiunta dell’integrazione Bitbucket.

   ```bash
   magento-cloud project:curl -p '<project-ID>' /integrations -i -X POST -d "$(< bitbucket.json)"
   ```

   Restituisce una risposta HTTP lunga con intestazioni. In caso di esito positivo, l’integrazione restituisce il codice di stato 200 o 201. Lo stato 400 o superiore indica che si è verificato un errore.

1. Elimina il file temporaneo `bitbucket.json` file.

1. Verifica l’integrazione del progetto.

   ```bash
   magento-cloud integrations -p <project-ID>
   ```

   ```terminal
   +----------+-----------+--------------------------------------------------------------------------------+
   | ID       | Type      | Summary                                                                        |
   +----------+-----------+--------------------------------------------------------------------------------+
   | <int-id> | bitbucket | Repository: bitbucket_Account/magento-int                                      |
   |          |           | Hook URL:                                                                      |
   |          |           | https://magento-url.cloud/api/projects/<project-id>/integrations/<int-id>/hook |
   +----------+-----------+--------------------------------------------------------------------------------+
   ```

   Prendi nota della **URL hook** per configurare un webhook in BitBucket.

### Aggiungere un webhook in BitBucket

Per comunicare eventi, come un push, con il server Cloud Git, è necessario disporre di un webhook per l’archivio BitBucket. Il metodo di impostazione di un’integrazione Bitbucket descritto in questa pagina, se seguito correttamente, crea automaticamente un webhook. È importante verificare il webhook per evitare la creazione di più integrazioni.

1. Accedi al tuo [Bitbucket](https://id.atlassian.com/login) account.

1. Clic **Archivi** e seleziona il progetto.

1. Clic **Impostazioni archivio** > **Flusso di lavoro** > **Webhook**.

1. Verifica il webhook prima di continuare.

   Se l’hook è attivo, salta i passaggi rimanenti e [Testare l’integrazione](#test-the-integration). L&#39;hook deve avere un nome simile a **&quot;Adobe Commerce sull’infrastruttura cloud &lt;project_id>&quot;** e un formato URL hook simile a: `https://<zone>.magento.cloud/api/projects/<project_id>/integrations/<id>/hook`

1. Clic **Aggiungi webhook**.

1. In _Aggiungi nuovo webhook_ visualizzare, modificare i campi seguenti:

   - **Titolo**: integrazione Adobe Commerce
   - **URL**: utilizza l’URL Hook dal tuo `magento-cloud` elenco di integrazione
   - **Triggers**: il valore predefinito è un _Push dell’archivio_

1. Clic **Salva**.

### Testare l’integrazione

Dopo aver configurato l’integrazione Bitbucket, puoi verificare che sia operativa utilizzando `magento-cloud` CLI:

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

1. Accedi a [[!DNL Cloud Console]](../project/overview.md) e verifica che venga visualizzato il messaggio di conferma e che venga distribuito il progetto.

   ![Verifica dell’integrazione di Bitbucket](../../assets/bitbucket-integration.png)

## Creare un ramo Cloud

L’integrazione Bitbucket non può attivare nuovi ambienti nel progetto di infrastruttura cloud Adobe Commerce on. Se crei un ambiente con Bitbucket, devi attivarlo manualmente. Per evitare questo passaggio aggiuntivo, è consigliabile creare ambienti utilizzando `magento-cloud` CLI o [!DNL Cloud Console].

**Per attivare un ramo creato con Bitbucket**:

1. Utilizza il `magento-cloud` CLI per inviare il ramo.

   ```bash
   magento-cloud environment:push from-bitbucket
   ```

   ```terminal
   Pushing from-bitbucket to the new environment from-bitbucket
   Activate from-bitbucket after pushing? [Y/n] y
   Parent environment [master]: integration
   --- (Validation and activation messages)
   ```

1. Verifica che l’ambiente sia attivo.

   ```bash
   magento-cloud environment:list
   ```

   ```terminal
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

**Per rimuovere l’integrazione Bitbucket**:

1. Dal terminale, accedi al tuo progetto di infrastruttura cloud Adobe Commerce on.

1. Elencare le integrazioni. Per completare il passaggio successivo, è necessario disporre dell’ID integrazione Bitbucket.

   ```bash
   magento-cloud integration:list
   ```

1. Elimina l’integrazione.

   ```bash
   magento-cloud integration:delete <int-ID>
   ```

Inoltre, puoi rimuovere l’integrazione Bitbucket accedendo al tuo account Bitbucket e revocando la concessione OAuth sull’account _Impostazioni_ pagina.

## Integrazione del server bitbucket

Per utilizzare l’integrazione con il server Bitbucket, è necessario quanto segue:

- [Token di accesso bitbucket](https://confluence.atlassian.com/bitbucketserver/http-access-tokens-939515499.html)- Genera un token per la concessione di progetti `read` accesso e archivio `admin` accesso
- [URL server bitbucket](https://confluence.atlassian.com/bitbucketserver/specify-the-bitbucket-base-url-776640392.html): aggiungi l’URL di base dell’istanza Bitbucket.

Sebbene sia possibile utilizzare Cloud CLI per eseguire i passaggi di integrazione del server Bitbucket, il comando completo è simile al seguente:

```bash
magento-cloud integration:add --type=bitbucket_server --base-url=<bitbucket-url> --username=<username> --token=<bitbucket-access-token> --project=<project-ID>
```

Utilizzare il comando help per ulteriori requisiti e opzioni di utilizzo: `magento-cloud integration:add --help`
