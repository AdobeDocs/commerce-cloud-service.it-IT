---
title: "Gestire i rami con [!DNL Cloud Console]"
description: Scopri come gestire i rami dell’ambiente per Adobe Commerce sull’infrastruttura cloud utilizzando [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
exl-id: 2c4ef149-fdb9-473f-91fd-5e6421ac5a43
source-git-commit: a5af3cc6e174a731a8f2ff198acd794384ef4585
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# Gestire i rami con [!DNL Cloud Console]

Puoi gestire gli ambienti utilizzando [!DNL Cloud Console] o `magento-cloud` CLI I file di progetto vengono memorizzati in un archivio Git. Puoi utilizzare i comandi Git per gestire il codice, ma il `magento-cloud` CLI è progettato per interagire con le funzioni della piattaforma, mentre i comandi Git no. Consulta [Comandi Git](../dev-tools/cloud-cli-overview.md#git-commands) nell’argomento cloud CLI.

Questo argomento illustra come utilizzare [!DNL Cloud Console] a:

- Aggiungere o eliminare un ambiente
- Sincronizzazione (`git pull`) dall&#39;ambiente padre
- Unisci (`git push`) all&#39;ambiente padre

>[!TIP]
>
>Non è possibile creare rami dagli ambienti di staging e produzione di Pro. È possibile diramare da `master` filiale.

## Creare un ambiente

La strategia di diramazione utilizza un flusso di lavoro Git comune in cui puoi sviluppare codice e aggiungere estensioni in un ramo di sviluppo. Consulta [Starter](../architecture/starter-architecture.md) e [Pro](../architecture/starter-develop-deploy-workflow.md) panoramiche dell’architettura.

- Per iniziare, crea un’ `staging` ramo da `master` branch, quindi branch da `staging` sviluppo.
- Per Pro, crea un ramo di sviluppo dal `Integration` ambiente.

Il tuo account supporta un numero limitato di ![ramo attivo](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} rami di sviluppo (inattivi). Gestire i rami attivi e inattivi aggiungendo o eliminando un ramo utilizzando solo [!DNL Cloud Console] o Cloud CLI. Prima di poter eliminare un ramo, disattivi il ramo, che rimane nel _Ambienti_ elenca come _inattivo_. Puoi riattivare il ramo in un secondo momento oppure [elimina il ramo](../dev-tools/cloud-cli-overview.md#) nelle impostazioni dell’ambiente o utilizzando Cloud CLI.

Se hai bisogno di altri ambienti attivi per lo sviluppo, invia una [Ticket di supporto](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

**Per aggiungere un ramo**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Seleziona un ambiente.

   >[!TIP]
   >
   >Il nuovo ramo viene clonato da questo ambiente. Scegli un ambiente principale simile all’ambiente che stai per creare.

1. Clic **[!UICONTROL Branch]**.

   ![Creare un ramo](../../assets/button-branch.png){width="150"}

1. In _Diramazione da..._ , immettere un nome di filiale.

   L&#39;ambiente _nome_ è diverso dall’ambiente _ID_ solo se nel nome dell’ambiente utilizzi spazi o lettere maiuscole. Un ID ambiente è costituito da tutte le lettere minuscole, i numeri e i simboli consentiti. Le lettere maiuscole nel nome di un ambiente vengono convertite in minuscole nell’ID; gli spazi nel nome di un ambiente vengono convertiti in trattini.

   Nome di ambiente **non può** include caratteri riservati per la shell Linux o per le espressioni regolari. I caratteri non consentiti includono le parentesi graffe (`{ }`), parentesi, asterisco (`*`), parentesi angolari (`>`), e commerciale (`&`), percentuale (<code>%</code>) e altri caratteri.

1. Seleziona un **[!UICONTROL Environment type]**.

1. Clic **[!UICONTROL Create Branch]**.

1. Attendere. Distribuzione dell&#39;ambiente in corso.

   Durante la distribuzione, lo stato dell’ambiente è  **In corso**. Dopo una distribuzione corretta, lo stato cambia in verde per **success**.

## Crea ramo inattivo

Non è possibile creare un ramo inattivo dalla console Adobe Commerce Cloud o da CLI. Se desideri creare un ramo inattivo, crealo nell’archivio Git e esegui il push utilizzando `environment.Parent` sul comando.

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## Eliminare un ambiente

Prima di poter eliminare un ambiente, devi disattivarlo. Una volta che un ambiente è inattivo, puoi eliminarlo.

**Per disattivare un ambiente**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Seleziona l’ambiente dalla barra di navigazione _Ambiente_ elenco.

1. Fai clic sull’icona Configura a destra della barra di navigazione superiore, che apre le impostazioni dell’ambiente.

1. Il giorno _[!UICONTROL General]_, scorri verso il basso fino alla scheda_[!UICONTROL Deactivate environment]_ e fai clic su **[!UICONTROL Deactivate environment and delete data]** e seguire le istruzioni.

## Sincronizzare un ambiente

La sincronizzazione di un ambiente (o ramo) è la stessa di `git pull origin <parent>`. Puoi sincronizzare il codice aggiornato da un ambiente principale. Puoi utilizzare questa funzione tramite [!DNL Cloud Console] per tutti gli ambienti Starter e Pro.

Per il piano Pro, puoi sincronizzare da Staging e Produzione al tuo `master` filiale. Questa sincronizzazione richiama e invia solo codice, non dati. Per sincronizzare i dati, scarica i dati del database e inviali al database di un altro ambiente. Consulta [Migrazione e distribuzione di file e dati statici](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**Per sincronizzare un ambiente**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Nell&#39;elenco Ambiente fare clic sul nome del ramo da sincronizzare.

1. Fai clic su (sincronizza).

   ![Sincronizzare un ambiente](../../assets/button-sync.png){width="150"}

1. Selezionare gli elementi da sincronizzare.

   - Sostituisci i dati: (dati e file) sincronizza le modifiche nel database e nei file di contenuto dal ramo padre.
   - Unisci—(code) sincronizza il codice aggiornato dal ramo padre.

   Viene inoltre creato un comando CLI da copiare e utilizzare.

1. Clic **Sincronizza**.

## Unisci con ambiente padre

L’unione di un ambiente (o di un ramo) equivale a `git push origin`. Unisci per inviare il codice aggiornato da un ambiente al relativo ambiente principale. È possibile unire questo codice in `master`. Puoi distribuire a Staging e Produzione utilizzando `merge` comando.

**Per eseguire l’unione con l’ambiente principale**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Nell’elenco dell’ambiente, fai clic sul nome del ramo da unire.

1. Fai clic su (unisci).

   ![Unire un ambiente](../../assets/button-merge.png){width="150"}

1. Clic **Unisci** e conferma l’azione.

## Visualizzare i registri

Attraverso il [!DNL Cloud Console], puoi esaminare vari registri per ambienti, inclusa la cronologia di build, distribuzione e distribuzione.

Per **Starter**, puoi rivedere i registri di build e distribuzione e la cronologia di distribuzione. Questi ambienti includono `master` (Produzione) e tutte le filiali create da esso.

Per **Pro**, in ogni ambiente è possibile esaminare i seguenti registri:

- Integrazione: creazione, distribuzione e cronologia di distribuzione
- Staging: creazione di registri e cronologia di distribuzione. Utilizza SSH per accedere al server e visualizzare i registri di distribuzione.
- Produzione: creazione di registri e cronologia di distribuzione. Utilizza SSH per accedere al server e visualizzare i registri di distribuzione.

**Per visualizzare i registri in[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Seleziona un ambiente.

   La vista ambiente fornisce un [Elenco attività](activity-stream.md) che mostra _recenti_ una voce per azione, tra cui sincronizzazioni, unioni, rami, backup e altro ancora. Clic **Tutti** per la cronologia completa della distribuzione.

1. Per visualizzare il registro della build, seleziona il collegamento Riuscito o Non riuscito per record di distribuzione sull’account.

>[!TIP]
>
>Fai clic su **Filtra per** per un elenco a discesa e selezionare il tipo di messaggi da visualizzare.

## Estrarre il codice da un archivio Git privato

Il progetto Adobe Commerce su infrastruttura cloud può includere il codice da un archivio Git privato. Ad esempio, puoi avere il codice per un modulo personalizzato o un tema in un archivio privato. A questo scopo, devi aggiungere la chiave SSH pubblica del progetto all’archivio Git privato e aggiornare il progetto `composer.json` file.

Per aggiungere una chiave di distribuzione all’archivio GitHub privato, devi essere l’amministratore di tale archivio. GitHub consente di utilizzare una chiave di distribuzione per un solo archivio.

Se preferisci che il progetto acceda a più archivi, puoi allegare una chiave SSH a un account utente automatizzato. Poiché questo account non viene utilizzato da un utente, viene indicato come [utente del computer](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). Aggiungi l’account computer come collaboratore o aggiungi l’utente computer a un team con accesso agli archivi.

>[!INFO]
>
>L’Adobe consiglia di aggiungere e unire questo codice agli archivi Git del progetto. Se non configuri la connessione, potrebbero verificarsi problemi di build.

**Per trovare la chiave pubblica SSH**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Fai clic sull’icona di configurazione a destra della barra di navigazione superiore.

1. In entrata _Impostazioni progetto_, fai clic su **[!UICONTROL Deploy Key]**.

1. Copia la chiave di distribuzione negli Appunti per l’utilizzo in uno dei seguenti metodi basati su Git:

>[!BEGINTABS]

>[!TAB GitHub]

### Immetti la chiave di distribuzione GitHub

Su GitHub, le chiavi di distribuzione sono di sola lettura per impostazione predefinita.

**Per immettere la chiave pubblica del progetto come chiave di distribuzione GitHub**:

1. Accedi all’archivio GitHub come amministratore.
1. Fai clic sull’archivio **[!UICONTROL Settings]** scheda.

   >[!NOTE]
   >
   >Se questa opzione non è disponibile, non si è connessi come amministratore del repository e non è possibile completare l&#39;attività. Chiedi all’amministratore dell’archivio GitHub di eseguire questa operazione.

1. Il giorno _Impostazioni_ nella barra di navigazione a sinistra, fai clic su **[!UICONTROL Deploy Keys]**.
1. Clic **[!UICONTROL Add deploy key]**.
1. Seguire le istruzioni.

In entrata `composer.json`, utilizza `<user>@<host>:<.git</code>` formato, oppure `ssh://<user>@<host>:<port>/<path>.git` se si utilizza una porta non standard.

>[!TAB Bitbucket]

### Immetti la chiave di distribuzione Bitbucket

**Per immettere la chiave pubblica del progetto come chiave di distribuzione Bitbucket**:

1. Accedi all’archivio Bitbucket come amministratore.
1. Nel menu di navigazione a sinistra, fai clic su **[!UICONTROL Settings]**.
1. Fai clic su Generale > **[!UICONTROL Deployment Keys]**.
1. Clic **[!UICONTROL Add Key]**.
1. Seguire le istruzioni.

>[!TAB GitLab]

### Immetti la chiave di distribuzione GitLab

**Per aggiungere la chiave SSH pubblica per il progetto come chiave di distribuzione GitLab**:

1. Accedi all’archivio GitLab come proprietario.
1. Verificare che _Pipeline_ l&#39;opzione è abilitata per il progetto:

   1. Nelle impostazioni del progetto, espandi **[!UICONTROL Visibility, project, features, permissions]** sezione.
   1. Se necessario, fai clic su **[!UICONTROL Pipelines]** per abilitare l’opzione.

1. Aggiungi la chiave SSH pubblica alle impostazioni CI/CD.

   1. Nel menu di navigazione a sinistra, fai clic su Impostazioni > **[!UICONTROL CI / CD]**.
   1. Fai clic su Distribuisci chiavi **Espandi** per configurare la chiave.
   1. In _Distribuisci chiave_ , aggiungi un nome della chiave di distribuzione al **[!UICONTROL Title]** e incollare la chiave SSH pubblica nel **[!UICONTROL Key]** campo.
   1. Clic **[!UICONTROL Add Key]** per salvare la configurazione.

>[!ENDTABS]

## Ambienti e rami sicuri

Puoi accedere al progetto e agli ambienti da qualsiasi posizione tramite un browser web utilizzando [!DNL Cloud Console]. È possibile che sia stata impostata la protezione per l&#39;ambiente di produzione, gli archivi e i siti. Questa sezione ti aiuta a proteggere gli ambienti di integrazione e staging per i tuoi sviluppatori, DBA e altro ancora.

>[!WARNING]
>
>**DO NOT** utilizzare i seguenti metodi per proteggere gli ambienti Pro Staging e Production. Questo interrompe il caching rapido. Utilizza il [Blocco](../cdn/fastly-vcl-blocking.md) funzionalità disponibile nella rete CDN Fastly per Adobe Commerce.

**Per proteggere gli ambienti**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Seleziona un ambiente e fai clic sull’icona di configurazione nella barra di navigazione.

1. Sulle impostazioni dell’ambiente _Generale_ , fare clic su **ATTIVATO** per **[!UICONTROL HTTP access control enabled]** per abilitare l&#39;accesso protetto. Puoi scegliere tra credenziali o indirizzi IP per filtrare l’accesso.

1. Per filtrare in base alle credenziali, fai clic su **[!UICONTROL Add Login]**, immettere un nome utente e una password e fare clic su **[!UICONTROL Add Login]** da aggiungere.

1. Per filtrare per indirizzo IP, inserisci gli indirizzi IP in un elenco con `deny` o `allow`. Ad esempio:

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. Clic **[!UICONTROL Save]**. In questo modo l’ambiente viene ridistribuito per aggiornare la sicurezza e le impostazioni. L’Adobe consiglia di eseguire un test dell’ambiente dopo aver completato le impostazioni di sicurezza.
