---
title: Gestisci rami con  [!DNL Cloud Console]
description: Scopri come gestire i rami dell'ambiente per Adobe Commerce sull'infrastruttura cloud utilizzando  [!DNL Cloud Console].
role: Developer
feature: Cloud, Install
exl-id: 2c4ef149-fdb9-473f-91fd-5e6421ac5a43
source-git-commit: a5af3cc6e174a731a8f2ff198acd794384ef4585
workflow-type: tm+mt
source-wordcount: '1589'
ht-degree: 0%

---

# Gestisci rami con [!DNL Cloud Console]

È possibile gestire gli ambienti utilizzando [!DNL Cloud Console] o la CLI di `magento-cloud`. I file di progetto vengono memorizzati in un archivio Git. È possibile utilizzare i comandi Git per gestire il codice, ma la CLI di `magento-cloud` è progettata per interagire con le funzionalità della piattaforma, mentre i comandi Git non lo sono. Vedere [Comandi Git](../dev-tools/cloud-cli-overview.md#git-commands) nell&#39;argomento CLI cloud.

In questo argomento viene illustrato come utilizzare [!DNL Cloud Console] per:

- Aggiungere o eliminare un ambiente
- Sincronizza (`git pull`) dall&#39;ambiente padre
- Unisci (`git push`) all&#39;ambiente padre

>[!TIP]
>
>Non è possibile creare rami dagli ambienti di staging e produzione di Pro. È possibile creare un branch dal branch `master`.

## Creare un ambiente

La strategia di diramazione utilizza un flusso di lavoro Git comune in cui puoi sviluppare codice e aggiungere estensioni in un ramo di sviluppo. Consulta [Starter](../architecture/starter-architecture.md) e [Pro](../architecture/starter-develop-deploy-workflow.md) panoramiche dell&#39;architettura.

- Per iniziare, crea un ramo `staging` dal ramo `master`, quindi un ramo da `staging` per lo sviluppo.
- Per Pro, creare un ramo di sviluppo dall&#39;ambiente `Integration`.

Il tuo account supporta un numero limitato di ![rami attivi](../../assets/icon-active.png){width="32"} (active) and an unlimited number of ![inactive branch](../../assets/icon-inactive.png){width="32"} (inattivi). Gestire i rami attivi e inattivi aggiungendo o eliminando un ramo utilizzando solo [!DNL Cloud Console] o Cloud CLI. Prima di eliminare un ramo, disattivalo, che rimane nell&#39;elenco _Ambienti_ come _inattivo_. Puoi riattivare il ramo in un secondo momento oppure [eliminare il ramo](../dev-tools/cloud-cli-overview.md#) nelle impostazioni dell&#39;ambiente o utilizzando Cloud CLI.

Se hai bisogno di altri ambienti attivi per lo sviluppo, invia un [ticket di supporto](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

**Per aggiungere un ramo**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selezionare un progetto dall&#39;elenco _Tutti i progetti_.

1. Seleziona un ambiente.

   >[!TIP]
   >
   >Il nuovo ramo viene clonato da questo ambiente. Scegli un ambiente principale simile all’ambiente che stai per creare.

1. Fare clic su **[!UICONTROL Branch]**.

   ![Crea un ramo](../../assets/button-branch.png){width="150"}

1. Nel modulo _Diramazione da ..._ immettere un nome di ramo.

   L&#39;ambiente _name_ è diverso dall&#39;ambiente _ID_ solo se si utilizzano spazi o lettere maiuscole nel nome dell&#39;ambiente. Un ID ambiente è costituito da tutte le lettere minuscole, i numeri e i simboli consentiti. Le lettere maiuscole nel nome di un ambiente vengono convertite in minuscole nell’ID; gli spazi nel nome di un ambiente vengono convertiti in trattini.

   Il nome di ambiente **non può** includere caratteri riservati per la shell Linux o per le espressioni regolari. I caratteri non consentiti includono parentesi graffe (`{ }`), parentesi, asterisco (`*`), parentesi angolari (`>`), e commerciale (`&`), percentuale (<code>%</code>) e altri caratteri.

1. Selezionare un **[!UICONTROL Environment type]**.

1. Fare clic su **[!UICONTROL Create Branch]**.

1. Attendere. Distribuzione dell&#39;ambiente in corso.

   Durante la distribuzione, lo stato dell&#39;ambiente è **In elaborazione**. Dopo una distribuzione riuscita, lo stato diventa un segno di spunta verde per **operazione riuscita**.

## Crea ramo inattivo

Non è possibile creare un ramo inattivo dalla console Adobe Commerce Cloud o da CLI. Se desideri creare un ramo inattivo, crealo nell’archivio Git e invialo tramite l’opzione `environment.Parent` sul comando.

```bash
git push -o "environment.Parent=<parent branch>" <origin> <branch>
```

## Eliminare un ambiente

Prima di poter eliminare un ambiente, devi disattivarlo. Una volta che un ambiente è inattivo, puoi eliminarlo.

**Per disattivare un ambiente**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selezionare un progetto dall&#39;elenco _Tutti i progetti_.

1. Selezionare l&#39;ambiente dall&#39;elenco della barra di spostamento _Ambiente_.

1. Fai clic sull’icona Configura a destra della barra di navigazione superiore, che apre le impostazioni dell’ambiente.

1. Nella scheda _[!UICONTROL General]_scorrere verso il basso fino alla sezione_[!UICONTROL Deactivate environment]_, fare clic su **[!UICONTROL Deactivate environment and delete data]** e seguire le istruzioni.

## Sincronizzare un ambiente

La sincronizzazione di un ambiente (o ramo) è uguale a `git pull origin <parent>`. Puoi sincronizzare il codice aggiornato da un ambiente principale. È possibile utilizzare questa funzionalità tramite [!DNL Cloud Console] per tutti gli ambienti Starter e Pro.

Per il piano Pro, puoi sincronizzare da Staging e Produzione al tuo ramo `master`. Questa sincronizzazione richiama e invia solo codice, non dati. Per sincronizzare i dati, scarica i dati del database e inviali al database di un altro ambiente. Vedi [Eseguire la migrazione e distribuire file e dati statici](/help/cloud-guide/deploy/staging-production.md#migrate-static-files).

**Per sincronizzare un ambiente**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selezionare un progetto dall&#39;elenco _Tutti i progetti_.

1. Nell&#39;elenco Ambiente fare clic sul nome del ramo da sincronizzare.

1. Fai clic su (sincronizza).

   ![Sincronizza un ambiente](../../assets/button-sync.png){width="150"}

1. Selezionare gli elementi da sincronizzare.

   - Sostituisci i dati: (dati e file) sincronizza le modifiche nel database e nei file di contenuto dal ramo padre.
   - Unisci—(code) sincronizza il codice aggiornato dal ramo padre.

   Viene inoltre creato un comando CLI da copiare e utilizzare.

1. Fare clic su **Sincronizza**.

## Unisci con ambiente padre

L&#39;unione di un ambiente (o ramo) è uguale a `git push origin`. Unisci per inviare il codice aggiornato da un ambiente al relativo ambiente principale. È possibile unire questo codice a `master`. È possibile distribuire a Staging e Produzione utilizzando il comando `merge`.

**Per eseguire l&#39;unione con l&#39;ambiente padre**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selezionare un progetto dall&#39;elenco _Tutti i progetti_.

1. Nell’elenco dell’ambiente, fai clic sul nome del ramo da unire.

1. Fai clic su (unisci).

   ![Unire un ambiente](../../assets/button-merge.png){width="150"}

1. Fai clic su **Unisci** e conferma l&#39;azione.

## Visualizzare i registri

Tramite [!DNL Cloud Console] è possibile esaminare vari registri per gli ambienti, inclusa la cronologia di compilazione, distribuzione e distribuzione.

Per **Starter**, puoi rivedere i registri di compilazione e distribuzione e la cronologia di distribuzione. Questi ambienti includono il ramo `master` (Produzione) e tutti i rami creati da esso.

Per **Pro**, puoi rivedere i seguenti registri in ogni ambiente:

- Integrazione: creazione, distribuzione e cronologia di distribuzione
- Staging: creazione di registri e cronologia di distribuzione. Utilizza SSH per accedere al server e visualizzare i registri di distribuzione.
- Produzione: creazione di registri e cronologia di distribuzione. Utilizza SSH per accedere al server e visualizzare i registri di distribuzione.

**Per visualizzare i registri in[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selezionare un progetto dall&#39;elenco _Tutti i progetti_.

1. Seleziona un ambiente.

   La visualizzazione dell&#39;ambiente fornisce un [elenco attività](activity-stream.md) che mostra _eventi recenti_, una voce per azione tentata tra cui sincronizzazioni, unioni, rami, backup e altro ancora. Fare clic su **Tutti** per visualizzare la cronologia completa della distribuzione.

1. Per visualizzare il registro della build, seleziona il collegamento Riuscito o Non riuscito per record di distribuzione sull’account.

>[!TIP]
>
>Fai clic sull&#39;icona **Filtra per** per un elenco a discesa e seleziona il tipo di messaggi da visualizzare.

## Estrarre il codice da un archivio Git privato

Il progetto Adobe Commerce su infrastruttura cloud può includere il codice da un archivio Git privato. Ad esempio, puoi avere il codice per un modulo personalizzato o un tema in un archivio privato. Per eseguire questa operazione, devi aggiungere la chiave SSH pubblica del progetto all&#39;archivio Git privato e aggiornare il file di progetto `composer.json`.

Per aggiungere una chiave di distribuzione all’archivio GitHub privato, devi essere l’amministratore di tale archivio. GitHub consente di utilizzare una chiave di distribuzione per un solo archivio.

Se preferisci che il progetto acceda a più archivi, puoi allegare una chiave SSH a un account utente automatizzato. Poiché questo account non è utilizzato da un utente umano, viene indicato come [utente computer](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/managing-deploy-keys). Aggiungi l’account computer come collaboratore o aggiungi l’utente computer a un team con accesso agli archivi.

>[!INFO]
>
>Adobe consiglia di aggiungere e unire questo codice agli archivi Git del progetto. Se non configuri la connessione, potrebbero verificarsi problemi di build.

**Per trovare la chiave pubblica SSH**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selezionare un progetto dall&#39;elenco _Tutti i progetti_.

1. Fai clic sull’icona di configurazione a destra della barra di navigazione superiore.

1. In _Impostazioni progetto_, fare clic su **[!UICONTROL Deploy Key]**.

1. Copia la chiave di distribuzione negli Appunti per l’utilizzo in uno dei seguenti metodi basati su Git:

>[!BEGINTABS]

>[!TAB GitHub]

### Immetti la chiave di distribuzione GitHub

Su GitHub, le chiavi di distribuzione sono di sola lettura per impostazione predefinita.

**Per immettere la chiave pubblica del progetto come chiave di distribuzione GitHub**:

1. Accedi all’archivio GitHub come amministratore.
1. Fare clic sulla scheda del repository **[!UICONTROL Settings]**.

   >[!NOTE]
   >
   >Se questa opzione non è disponibile, non si è connessi come amministratore del repository e non è possibile completare l&#39;attività. Chiedi all’amministratore dell’archivio GitHub di eseguire questa operazione.

1. Nella scheda _Impostazioni_ della barra di navigazione a sinistra, fare clic su **[!UICONTROL Deploy Keys]**.
1. Fare clic su **[!UICONTROL Add deploy key]**.
1. Seguire le istruzioni.

In `composer.json`, utilizzare il formato `<user>@<host>:<.git</code>` o `ssh://<user>@<host>:<port>/<path>.git` se si utilizza una porta non standard.

>[!TAB Bitbucket]

### Immetti la chiave di distribuzione Bitbucket

**Per immettere la chiave pubblica del progetto come chiave di distribuzione Bitbucket**:

1. Accedi all’archivio Bitbucket come amministratore.
1. Nel menu di navigazione a sinistra, fare clic su **[!UICONTROL Settings]**.
1. Fare clic su Generale > **[!UICONTROL Deployment Keys]**.
1. Fare clic su **[!UICONTROL Add Key]**.
1. Seguire le istruzioni.

>[!TAB GitLab]

### Immetti la chiave di distribuzione GitLab

**Per aggiungere la chiave SSH pubblica per il progetto come chiave di distribuzione GitLab**:

1. Accedi all’archivio GitLab come proprietario.
1. Verifica che l&#39;opzione _Pipeline_ sia abilitata per il progetto:

   1. Nelle impostazioni del progetto, espandi la sezione **[!UICONTROL Visibility, project, features, permissions]**.
   1. Se necessario, fare clic su **[!UICONTROL Pipelines]** per abilitare l&#39;opzione.

1. Aggiungi la chiave SSH pubblica alle impostazioni CI/CD.

   1. Nel menu di navigazione a sinistra, fare clic su Impostazioni > **[!UICONTROL CI / CD]**.
   1. Fai clic su Distribuisci chiavi **Espandi** per configurare la chiave.
   1. Nel modulo _Distribuisci chiave_, aggiungi un nome di chiave di distribuzione al campo **[!UICONTROL Title]** e incolla la chiave SSH pubblica nel campo **[!UICONTROL Key]**.
   1. Fare clic su **[!UICONTROL Add Key]** per salvare la configurazione.

>[!ENDTABS]

## Ambienti e rami sicuri

È possibile accedere al progetto e agli ambienti da qualsiasi posizione tramite un browser Web utilizzando [!DNL Cloud Console]. È possibile che sia stata impostata la protezione per l&#39;ambiente di produzione, gli archivi e i siti. Questa sezione ti aiuta a proteggere gli ambienti di integrazione e staging per i tuoi sviluppatori, DBA e altro ancora.

>[!WARNING]
>
>**DO NOT** utilizza i seguenti metodi per proteggere gli ambienti di staging e produzione Pro. Questo interrompe il caching rapido. Utilizza la funzionalità [Blocco](../cdn/fastly-vcl-blocking.md) disponibile nella rete CDN Fastly per Adobe Commerce.

**Per proteggere gli ambienti**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selezionare un progetto dall&#39;elenco _Tutti i progetti_.

1. Seleziona un ambiente e fai clic sull’icona di configurazione nella barra di navigazione.

1. Nella scheda Impostazioni ambiente _Generale_, fare clic su **SU** per **[!UICONTROL HTTP access control enabled]** per abilitare l&#39;accesso protetto. Puoi scegliere tra credenziali o indirizzi IP per filtrare l’accesso.

1. Per filtrare in base alle credenziali, fare clic su **[!UICONTROL Add Login]**, immettere un nome utente e una password, quindi fare clic su **[!UICONTROL Add Login]** per aggiungere.

1. Per filtrare per indirizzo IP, immettere gli indirizzi IP in un elenco con `deny` o `allow`. Ad esempio:

   ```text
   123.456.789.111/29 allow
   123.456.789.112/29 allow
   234.123.567.111/29 allow
   0.0.0.0/0 deny
   ```

1. Fare clic su **[!UICONTROL Save]**. In questo modo l’ambiente viene ridistribuito per aggiornare la sicurezza e le impostazioni. Dopo aver completato le impostazioni di protezione, Adobe consiglia di eseguire un test dell’ambiente.
