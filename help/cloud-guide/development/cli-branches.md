---
title: Gestire i rami con CLI
description: Scopri come gestire i rami dell’ambiente per Adobe Commerce sull’infrastruttura cloud utilizzando Cloud CLI.
role: Developer
feature: Cloud, Install
exl-id: a871c7e2-4506-4a05-8fc2-fc5ef2afe609
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '676'
ht-degree: 0%

---

# Gestire i rami con CLI

Per installare `magento-cloud` CLI, vedere [Riferimento CLI cloud](../dev-tools/cloud-cli-overview.md). Dopo aver installato `magento-cloud` e configurare le chiavi SSH per l&#39;accesso remoto all&#39;infrastruttura cloud, è possibile utilizzare `magento-cloud` Comandi CLI per gestire gli ambienti per i progetti. Per informazioni sull’architettura dell’ambiente, consulta [Architettura iniziale](../architecture/starter-architecture.md) o [Architettura Pro](../architecture/pro-architecture.md).

Per gestire rami e ambienti con [!DNL Cloud Console], vedi [Gestire i rami con [!DNL Cloud Console]](../project/console-branches.md).

## Utilizzare i comandi CLI

Il `magento-cloud` I comandi CLI sono simili ai comandi Git. Puoi utilizzarli per connetterti al progetto e gestire gli ambienti. Sebbene sia possibile eseguire i comandi da qualsiasi directory, si consiglia di eseguirli da una directory di progetto. Quando viene eseguito da una directory di progetto, è possibile omettere `-p <project-ID>` parametro. Consulta la [Riferimento CLI cloud](../dev-tools/cloud-cli-overview.md).

## Clona il progetto

Le istruzioni seguenti utilizzano una combinazione di `magento-cloud` Comandi CLI e comandi Git per clonare il progetto sulla workstation locale. Per visualizzare un elenco completo di `magento-cloud` Comandi CLI, utilizzare `magento-cloud list` comando.

>[!IMPORTANT]
>
>Alcuni comandi Git non possono completare un’azione nel progetto di infrastruttura cloud di Adobe Commerce. Ad esempio, puoi creare un ramo utilizzando un comando Git, ma non puoi creare e attivare un nuovo ambiente. È necessario creare un ambiente utilizzando `magento-cloud environment:branch <branch-name>` affinché l’ambiente diventi _attivo_. In alternativa, è possibile utilizzare [!DNL Cloud Console] per creare ambienti attivi. Consulta [Riferimento CLI cloud](../dev-tools/cloud-cli-overview.md#git-commands).

**Per clonare un progetto `master` ambiente**:

1. Accedere alla workstation locale con un [proprietario del file system](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html) account.

1. Passare al server web o all&#39;host virtuale _docroot_ directory.

1. Accedi utilizzando `magento-cloud` CLI

   ```bash
   magento-cloud login
   ```

1. Elencare i progetti.

   ```bash
   magento-cloud project:list
   ```

1. Clona un progetto.

   ```bash
   magento-cloud project:get <project-ID>
   ```

   Quando richiesto, fornisci un nome di directory.

1. Cambia in `magento2` directory.

1. Elencare gli ambienti disponibili per il progetto.

   ```bash
   magento-cloud environment:list
   ```

   >[!IMPORTANT]
   >
   >Il `magento-cloud environment:list` visualizza le gerarchie di ambiente, mentre il comando `git branch` il comando non.

1. Recupera i rami remoti.

   ```bash
   git fetch origin
   ```

1. Recupera il codice aggiornato.

   ```bash
   git pull origin <environment-ID>
   ```

>[!TIP]
>
>Consulta [Integrazioni](../integrations/overview.md) per informazioni sull’utilizzo dei servizi di hosting basati su Git con Adobe Commerce sull’infrastruttura cloud.

## Creare un ramo per lo sviluppo

Dopo aver clonato il progetto e aver aggiornato la configurazione dell’account amministratore di Adobe Commerce, puoi creare un ramo per lo sviluppo. Come indicato in precedenza, è necessario creare un ambiente utilizzando `magento-cloud environment:branch <branch-name>` comando o [!DNL Cloud Console] affinché l&#39;ambiente diventi _attivo_.

- Per [Starter](../architecture/starter-develop-deploy-workflow.md#clone-and-branch), è consigliabile creare un ramo per `staging`, quindi crea un ramo di sviluppo basato su `staging` filiale.
- Per [Pro](../architecture/pro-develop-deploy-workflow.md#development-workflow), creare rami di sviluppo in base al `Integration` filiale.

**Per creare un ramo di sviluppo**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Crea un ambiente basato sul ramo consigliato per il flusso di lavoro del progetto.

   ```bash
   magento-cloud branch <new-environment-name> integration
   ```

1. Aggiornare le dipendenze.

   ```bash
   composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
   ```

1. [_facoltativo_] Creare un [backup](../storage/snapshots.md) dell&#39;ambiente.

### Unire un ramo

Dopo aver completato lo sviluppo, unisci questo ramo all’elemento padre:

1. Modifiche al codice di commit e push:

   ```bash
   git add -A && git commit -m "Add message here"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Unisci con l’ambiente principale:

   ```bash
   magento-cloud environment:merge <environment-ID>
   ```

### Eliminare un ambiente

Elimina un ambiente solo se sei sicuro di non averne più bisogno. Non è possibile ripristinare un ambiente dopo averlo eliminato.

>[!WARNING]
>
>Impossibile eliminare `master` di qualsiasi progetto.

Per eseguire questa attività è necessario essere un amministratore di progetto, un amministratore dell&#39;ambiente o un proprietario account. Consulta [Gestire l’accesso degli utenti ai progetti Cloud](../project/user-access.md).

Quando elimini un ambiente, questo viene impostato su _inattivo_. Il codice è ancora disponibile nel ramo Git, ma non contiene più i servizi o il database. Per eliminare completamente l’ambiente, devi eliminare anche il ramo Git remoto corrispondente.

**Per eliminare un ambiente**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Recupera gli aggiornamenti dal server remoto.

   ```bash
   git fetch
   ```

1. Elimina il ramo ambiente.

   ```bash
   magento-cloud environment:delete <environment-ID>
   ```

   Facoltativamente, è possibile eliminare più ambienti alla volta aggiungendo più ID ambiente al comando delete.

   ```bash
   magento-cloud environment:delete <environment-1-ID> <environment-2-ID>
   ```

1. Rispondere alle richieste di eliminazione dell&#39;ambiente locale e dell&#39;ambiente remoto corrispondente.

   ```terminal
   The environment <environment-ID> is currently active: deleting it will delete all associated data.
   Are you sure you want to delete the environment <environment-ID>? [Y/n]
   ```

   L’eliminazione dell’ambiente lo inserisce in un _inattivo_ stato.

   ```terminal
   Delete the remote Git branch too? [Y/n]
   ```

   L’eliminazione del ramo Git remoto rimuove l’ambiente dal progetto.

1. Attendi l’eliminazione dell’ambiente.

   ```terminal
   Deleting environment <environment-ID>
   Waiting for the activity...
     Deleting environment <project-id>-<environment-ID>-xxxxxx
   
     [============================]  1 min (complete)
   Activity ID succeeded
   Deleted remote Git branch <environment-ID>
   Run git fetch --prune to remove deleted branches from your local cache.
   ```

>[!TIP]
>
>Per attivare un ambiente inattivo, utilizza `magento-cloud environment:activate` comando.

## Interagire con gli ambienti remoti

Dopo di te [configurare le chiavi SSH](../development/secure-connections.md), è possibile [connettersi dall’area di lavoro locale a un ambiente remoto](../development/secure-connections.md#connect-to-a-remote-environment) e interagire con i servizi di progetto e modificare le impostazioni.
