---
title: Cloud CLI
description: Scopri la CLI di Magento-Cloud e come consente di gestire gli ambienti di sviluppo locali per il progetto di infrastruttura cloud Adobe Commerce.
exl-id: 70dddd62-0269-4af4-bd2a-1a4fbf11a131
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '741'
ht-degree: 0%

---


# Cloud CLI

Il `magento-cloud` Lo strumento CLI consente agli sviluppatori e agli amministratori di sistema di gestire progetti e ambienti Cloud, eseguire routine ed eseguire attività di automazione. Il `magento-cloud` CLI estende le funzioni e le funzionalità del [[!DNL Cloud Console]](../../get-started/cloud-console.md). Dopo aver installato `magento-cloud` CLI sulla workstation locale, è possibile utilizzarla per gestire gli ambienti di integrazione Adobe Commerce sull&#39;infrastruttura cloud Starter e Pro.

**Per installare `magento-cloud` CLI**:

1. Sulla workstation locale, passa alla directory in cui intendi clonare il progetto Cloud e dove [proprietario del file system](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html) ha _scrivere_ accesso.

1. Installare `magento-cloud` CLI

   ```bash
   curl -sS https://accounts.magento.cloud/cli/installer | php
   ```

1. Aggiungi `magento-cloud` CLI al profilo Bash.

   ```bash
   export PATH=$PATH:$HOME/.magento-cloud/bin
   ```

1. Ricarica il profilo di base aggiornato.

   ```bash
   . ~/.bash_profile
   ```

1. Per avviare CLI, chiamare `magento-cloud` e immetti le credenziali del tuo account Cloud quando richiesto.

   ```bash
   magento-cloud
   ```

   ```terminal
   Welcome to Magento Cloud!
   Please log in using your Magento Cloud account.
   Your email address or username:
   ```

1. Verificare la `magento-cloud` nel percorso. Nell&#39;esempio seguente vengono elencati i comandi disponibili.

   ```bash
   magento-cloud list
   ```

## Comandi comuni

Adobe ha progettato questi comandi per gestire gli ambienti di integrazione Cloud e consiglia di eseguire il `magento-cloud` CLI da una directory di progetto per poter omettere il `-p <project-ID>` parametro.

Il seguente elenco di `magento-cloud` I comandi CLI includono solo le opzioni richieste. È possibile utilizzare `--help` con qualsiasi comando per visualizzare ulteriori informazioni.

| Comando | Descrizione |
| ------------------------------------ | -------------------------------------------------- |
| `magento-cloud login` | Accedi al progetto. |
| `magento-cloud list` | Elencare i comandi disponibili per lo strumento CLI. |
| `magento-cloud environment:list` | Elencare gli ambienti nel progetto corrente. |
| `magento-cloud environment:checkout` | Estrai un ambiente esistente. |
| `magento-cloud environment:merge -e` | Unisci le modifiche in questo ambiente con il relativo elemento padre. |
| `magento-cloud variables` | Variabili elenco in questo ambiente. |
| `magento-cloud ssh` | Utilizzare SSH per connettersi all&#39;ambiente remoto. |
| `magento-cloud url` | Apri la vetrina Adobe Commerce in un browser. |
| `magento-cloud web` | Apri [!DNL Cloud Console]. |

## Comandi di ambiente

L&#39;ambiente _nome_ è diverso dall’ambiente _ID_ solo se nel nome dell’ambiente utilizzi spazi o lettere maiuscole. Un ID ambiente è costituito da tutte le lettere minuscole, i numeri e i simboli consentiti. Le lettere maiuscole nel nome di un ambiente vengono convertite in minuscole nell’ID; gli spazi nel nome di un ambiente vengono convertiti in trattini.

Nome di ambiente _non può_ include caratteri riservati per la shell Linux o per le espressioni regolari. I caratteri non consentiti includono le parentesi graffe (`{ }`), parentesi, asterisco (`*`), parentesi angolari (`< >`), e commerciale (`&`), percentuale (`%`) e altri caratteri.

Il `magento-cloud environment:list` visualizza le gerarchie di ambiente, mentre `git branch` non lo fa. Se disponi di ambienti nidificati, utilizza quanto segue:

```bash
magento-cloud environment:list
```

### Ridistribuire l’ambiente

Attiva una ridistribuzione senza utilizzare un push. Verifica e conferma l’ambiente da ridistribuire. Non utilizzare la ridistribuzione se è presente una build in uno stato in sospeso.

```bash
magento-cloud environment:redeploy
```

Risposta di esempio:

```terminal
Are you sure you want to redeploy the environment <environment-name>? [Y/n]
```

{{redeploy-warning}}

## Comandi Git

È possibile notare che alcuni di questi comandi sono simili ai comandi Git. Il `magento-cloud` I comandi si connettono direttamente al progetto Cloud basato su Git con funzioni aggiuntive. Se si crea un ramo senza utilizzare `magento-cloud` CLI, non è &quot;attivato&quot; e non viene generato automaticamente quando si inviano le modifiche all’ambiente remoto. Il `magento-cloud` Il comando CLI include l&#39;attivazione.

Per creare un ramo, utilizza `magento-cloud` affinché il ramo venga attivato.

```bash
magento-cloud environment:branch <new-name> <parent-branch>
```

Per lo stato della filiale:

- Utilizza il `magento-cloud env` per visualizzare un elenco dei rami dell’ambiente e il loro stato: attivo o inattivo.
- Utilizza il `magento-cloud environment:activate` per attivare un ramo di ambiente.

Effettua il push di un commit Git vuoto per attivare una distribuzione. Ad esempio:

```bash
git commit --allow-empty -m "redeploy" && git push <branch-name>
```

Alcune azioni, come l’aggiunta di un utente, non comportano la distribuzione.

### Creare un ramo dell’ambiente

I passaggi seguenti illustrano l’utilizzo dei comandi CLI e Git in modo intercambiabile per gestire l’ambiente locale:

1. Sulla workstation locale, passa alla directory del progetto.

1. Passa a [proprietario del file system](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html).

1. Accedi al progetto.

   ```bash
   magento-cloud login
   ```

1. Elencare i progetti.

   ```bash
   magento-cloud project:list
   ```

1. Elencare gli ambienti nel progetto. Ogni ambiente include un ramo Git attivo che contiene il codice, il database, le variabili di ambiente, le configurazioni e i servizi.

   ```bash
   magento-cloud environment:list
   ```

   >[!NOTE]
   >
   >È importante utilizzare il `magento-cloud environment:list` perché visualizza le gerarchie di ambiente, mentre il comando `git branch` il comando non.

1. Recupera i rami di origine per ottenere il codice più recente.

   ```bash
   git fetch origin
   ```

1. Estrai o passa a un ramo e a un ambiente specifici.

   ```bash
   magento-cloud environment:checkout <environment-ID>
   ```

   I comandi Git estraggono solo il ramo Git. Il `magento-cloud checkout` Il comando estrae il ramo e passa all&#39;ambiente attivo.

   >[!TIP]
   >
   >Puoi creare un ramo di ambiente utilizzando `magento-cloud environment:branch <environment-name> <parent-environment-ID>` sintassi del comando. La creazione e l’attivazione di un ramo dell’ambiente potrebbe richiedere un po’ di tempo.

1. Utilizza l’ID dell’ambiente per richiamare il codice aggiornato nel tuo locale. Questo non è necessario se il ramo dell’ambiente è nuovo.

   ```bash
   git pull origin <environment-ID>
   ```

1. (_Facoltativo_ a) Creare un [istantanea](../storage/snapshots.md) dell’ambiente come backup.

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

## Aggiornare CLI

Il `magento-cloud` CLI verifica la disponibilità di aggiornamenti al momento dell&#39;accesso, ma è possibile verificare la disponibilità di aggiornamenti utilizzando `self:update` comando. Se è disponibile un aggiornamento, seguire le istruzioni per aggiornare CLI.

Se il `magento-cloud` CLI è aggiornato e viene visualizzata la seguente risposta:

```bash
magento-cloud update
```

```terminal
Checking for Magento Cloud CLI updates (current version: X.XX.X)
No updates found
```
