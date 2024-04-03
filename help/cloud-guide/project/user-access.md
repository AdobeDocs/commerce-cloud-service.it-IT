---
title: Gestire l’accesso degli utenti
description: Scopri come gestire l’accesso degli utenti a Adobe Commerce su progetti e ambienti di infrastruttura cloud.
role: Admin
feature: Cloud, Roles/Permissions
last-substantial-update: 2023-06-27T00:00:00Z
topic: Security
exl-id: 3357a3ea-bf86-4a65-95d1-6b24f1152248
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1441'
ht-degree: 0%

---

# Gestire l’accesso degli utenti

I progetti Adobe Commerce sull’infrastruttura cloud utilizzano l’accesso basato sui ruoli. A livello di progetto sono disponibili due ruoli:

- **Amministratore progetto**: consente di accedere in scrittura a tutti gli ambienti del progetto e di gestire gli utenti, il codice push e aggiornare le impostazioni del progetto.
- **Visualizzatore progetti**- Accesso in sola visualizzazione a tutti gli ambienti di progetto.

I visualizzatori dei progetti non possono eseguire attività in alcun ambiente; tuttavia, è possibile concedere ai visualizzatori dei progetti l&#39;accesso in scrittura a un tipo di ambiente specifico.

L’accesso a livello di ambiente si basa sul tipo di ambiente: produzione, staging e sviluppo. Concessione di un utente _visualizzatore_ autorizzazione a _sviluppo_ gli ambienti consentono loro di visualizzare **tutto** ambienti di sviluppo nel progetto. La tabella seguente chiarisce le capacità concesse a ciascun livello di autorizzazione:

| Livello di autorizzazione | Accesso | Accesso SSH |
| ------------------ | ----------- | :----------: |
| **Amministratore** | Eseguire attività di amministrazione, ad esempio modificare le impostazioni, inviare codice, eseguire attività e gestire i rami, inclusa l&#39;unione con l&#39;ambiente padre | Sì |
| **Collaboratore** | Invio di codice e diramazione dell&#39;ambiente; impossibile modificare le impostazioni o eseguire azioni | Sì |
| **Visualizzatore** | Accesso in sola visualizzazione al tipo di ambiente | No |
| **Nessun accesso** | Nessun accesso al tipo di ambiente | No |

{style="table-layout:auto"}

Puoi aggiungere utenti e assegnare ruoli utilizzando `magento-cloud` CLI o [!DNL Cloud Console].

>[!BEGINSHADEBOX]

**Prerequisiti:**

- Un utente registrato con un Adobe ID. Un utente deve [registrarsi per un account di Adobe](https://account.adobe.com) e poi [inizializza il loro account Cloud](https://console.adobecommerce.com) prima di poterli aggiungere a un progetto Cloud.
- Un utente ha assegnato **Amministratore** il ruolo non può gestire gli utenti con `magento-cloud` CLI Solo gli utenti a cui è concesso il **Proprietario account** Il ruolo può gestire gli utenti.

>[!ENDSHADEBOX]

## Gestione degli utenti con CLI

Utilizza il `magento-cloud` CLI per la gestione degli utenti e l&#39;integrazione con i sistemi automatizzati:

- `magento-cloud user:add`-aggiungere un utente al progetto
- `magento-cloud user:delete`-eliminare un utente
- `magento-cloud user:list [users]`-elenca utenti progetto
- `magento-cloud user:role`-visualizzare o modificare il ruolo utente
- `magento-cloud user:update`-aggiornare il ruolo utente in un progetto

Gli esempi seguenti utilizzano `magento-cloud` CLI per aggiungere un utente, configurare ruoli, modificare le assegnazioni del progetto e assegnare ruoli utente.

**Per aggiungere un utente e assegnare ruoli**:

1. Utilizza il `magento-cloud` CLI per aggiungere l&#39;utente.

   ```bash
   magento-cloud user:add
   ```

   >[!IMPORTANT]
   >
   >L’utente deve disporre di un Adobe ID; consulta [prerequisiti](#add-users-and-manage-access).

1. Segui i prompt: specifica l’indirizzo e-mail dell’utente, imposta i ruoli di progetto e tipo di ambiente, quindi aggiungi l’utente.

   > Prompt di esempio

   ```terminal
   Enter the user's email address: alice@example.com
   
   Email address: alice@example.com
   
   The user's project role can be admin (a) or viewer (v).
   
   Project role (default: viewer) [a/v]: viewer
   
   The user's environment type role(s) can be admin (a), viewer (v), contributor (c) or none (n).
   
   Role on type development (default: none) [a/v/c/n]: none
   Role on type production (default: none) [a/v/c/n]: admin
   Role on type staging (default: none) [a/v/c/n]: admin
   
   Adding the user alice@example.com to (project_id):
   Project role: viewer
     Role on type production: admin
     Role on type staging: admin
   
   Are you sure you want to add this user? [Y/n] y
   Adding the user to the project
   ```

   Dopo aver aggiunto l’utente, Adobe invia un’e-mail all’indirizzo specificato con le istruzioni per l’accesso al progetto Adobe Commerce on Cloud Infrastructure.

### Visualizzare il ruolo di progetto di un utente

```bash
magento-cloud user:get alice@example.com
```

>Risposta di esempio:

```terminal
Current role(s) of User (alice@example.com) on Production (project_id):
  Project role: admin
```

### Aggiungere un utente a più ambienti

Per aggiungere un utente come `viewer` su un `Production` e as a `contributor` su un `Integration` ambiente:

```bash
magento-cloud user:add alice@example.com -r production:v -r integration:c
```

### Aggiornare le autorizzazioni degli ambienti utente

Per aggiornare le autorizzazioni dell&#39;ambiente utente a `admin` il `Production` ambiente:

```bash
magento-cloud user:update alice@example.com -r production:a
```

## Gestire gli utenti da [!DNL Cloud Console]

È possibile utilizzare [[!DNL Cloud Console]](../../get-started/cloud-console.md) per aggiungere autorizzazioni e utilizzare _Modifica_ per modificare le autorizzazioni per un utente esistente.

>[!IMPORTANT]
>
>L’utente deve disporre di un Adobe ID; consulta [prerequisiti](#add-users-and-manage-access).

### Aggiungere un utente al progetto

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com/).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Nel dashboard Progetto, fai clic sull’icona di configurazione in alto a destra.

1. Sotto _Impostazioni progetto_, fai clic su **[!UICONTROL Access]**.

1. In _Accesso_ visualizza, fai clic su **[!UICONTROL Add]**.

1. Completa il _[!UICONTROL Add User]_forma:

   - Immetti l’indirizzo e-mail dell’utente.

   - **[!UICONTROL Project admin]**- concede i diritti di amministratore a tutte le impostazioni e a tutti i tipi di ambiente.

   - **[!UICONTROL Environment types and permissions]**: concedi l’accesso e specifici livelli di autorizzazione a determinati tipi di ambiente. _Nessun accesso_, _Amministratore_ (modifica impostazioni, esegui azione, unisci codice), _Collaboratore_ (codice push), oppure _Visualizzatore_ (solo visualizzazione).

   >[!TIP]
   >
   >Solo un **Amministratore progetto** può gestire gli utenti in qualsiasi ambiente. Per concedere a un utente l&#39;accesso a **Accesso** scheda, altro **Amministratore progetto** o **Proprietario account** deve assegnare a tale utente il **Amministratore progetto** ruolo.

1. Clic **[!UICONTROL Add User]**.

1. Dopo aver aggiunto gli utenti, ridistribuisci tutti gli ambienti per applicare le modifiche. L’aggiunta di un utente non attiva automaticamente una distribuzione. La ridistribuzione è un passaggio importante per garantire che l’utente possa accedere a un ambiente utilizzando SSH.

Dopo aver aggiunto l’utente, Adobe invia un’e-mail all’indirizzo specificato con le istruzioni per l’accesso al progetto Adobe Commerce on Cloud Infrastructure.

## Requisiti di autenticazione utente

Per una maggiore sicurezza, Adobe fornisce l’applicazione dell’autenticazione a più fattori (MFA) a livello di progetto per richiedere l’autenticazione a due fattori (TFA) per l’accesso SSH ad Adobe Commerce sul codice sorgente e sugli ambienti del progetto di infrastruttura cloud. Consulta [Abilita MFA per SSH](multi-factor-authentication.md).

Quando l’applicazione MFA è abilitata in un progetto di infrastruttura cloud Adobe Commerce on, tutti gli utenti con accesso SSH a un ambiente in tale progetto devono abilitare TFA sul proprio account di infrastruttura cloud Adobe Commerce. Per i processi automatizzati, puoi creare un token API e un utente del computer per l’autenticazione dalla riga di comando.

Dopo aver aggiunto un utente a un progetto Cloud, chiedi all’utente di rivedere le impostazioni di sicurezza dell’account e aggiungere le seguenti configurazioni di sicurezza, in base alle esigenze:

- **Abilita TFA** conformità agli standard di sicurezza e conformità attraverso la configurazione dell&#39;autenticazione a due fattori. Progetti configurati con [Applicazione dell&#39;AMF](multi-factor-authentication.md) richiedere TFA sugli account che utilizzano SSH per accedere ai progetti.

- **Abilita chiavi SSH**: gli utenti che richiedono l’accesso ad Adobe Commerce sugli archivi del codice sorgente dell’infrastruttura cloud devono abilitare le chiavi SSH sul proprio account. Consulta [Connessioni sicure](../development/secure-connections.md).

- **Creare un token API**- Gli utenti devono generare un token API utilizzato per l&#39;accesso SSH a un ambiente. È necessario il token per abilitare i flussi di lavoro di autenticazione per i processi automatizzati.

  Nei progetti in cui è abilitata l’imposizione MFA, è necessario utilizzare il token API per autenticare le richieste di accesso SSH da account automatizzati. Il token consente ai processi automatizzati di ignorare i flussi di lavoro di autenticazione che richiedono TFA.

### Abilita TFA per gli account Cloud

Adobe Commerce su infrastruttura cloud supporta TFA utilizzando una delle seguenti applicazioni:

- [Autenticatore Google (Android/iPhone)](https://support.google.com/accounts/answer/1066447?hl=en)
- [Authy (Android/iPhone)](https://authy.com/features/)
- [FreeOTP (Android)](https://play.google.com/store/apps/details?id=org.fedorahosted.freeotp)
- [Autenticatore GAuth (Firefox OS, desktop, altri)](https://github.com/gbraad-apps/gauth)

Le istruzioni per l’installazione dell’applicazione di autenticazione e l’abilitazione di TFA sono disponibili sul sito _Impostazioni account_ pagina in [!DNL Cloud Console].

**Per abilitare TFA nell’account utente**:

1. Accedi a [account personale](https://console.adobecommerce.com).

1. Nel menu dell’account in alto a destra, fai clic su **[!UICONTROL My Profile]**.

1. Il giorno _Sicurezza_ , fare clic su **[!UICONTROL Set up application]**.

1. Se non disponi di un’applicazione di autenticazione approvata sul tuo dispositivo mobile, utilizza le istruzioni collegate per installarne una.

1. Aggiungi l’account Adobe Commerce su infrastruttura cloud all’applicazione di autenticazione.

   - Sul dispositivo mobile, apri l’applicazione di autenticazione. Quindi, aggiungi il codice di installazione all’applicazione.

   - In [!UICONTROL **[!UICONTROL TFA set up - Application]**] , digita il codice TFA dal tuo dispositivo mobile in **[!UICONTROL Application verification code]** campo.

   - Clic **[!UICONTROL Verify and save]**.

     Se il codice è valido, Adobe invia una notifica all’indirizzo e-mail dell’account confermando che l’account ora ha TFA.

1. Facoltativo. Abilita _Browser attendibile_ impostazioni per memorizzare il codice di autenticazione nella cache del browser per 30 giorni.

   Questa configurazione riduce il numero di problemi di autenticazione durante l’accesso al progetto.

1. Clic **Salva** o **Ignora**.

1. Salvare i codici di ripristino.

   - Il giorno _Configurazione TFA - Ripristino_ nella pagina dei codici, copia e salva i codici di ripristino in modo da poter accedere al progetto Adobe Commerce on cloud infrastructure quando non è possibile accedere al dispositivo mobile o all’applicazione di autenticazione.

   - Copiare i codici di ripristino in un&#39;altra posizione o annotarli nel caso si perda l&#39;accesso al dispositivo o all&#39;applicazione di autenticazione.

   - Clic **Salva** per salvare i codici nel tuo account in modo da poterli visualizzare e gestire dalle impostazioni di sicurezza dell’account.

     >[!WARNING]
     >
     >Se si perde l&#39;accesso a un account con TFA e non si dispone dell&#39;elenco dei codici di ripristino, è necessario contattare l&#39;amministratore del progetto oppure [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per ripristinare l&#39;applicazione TFA.

1. Dopo aver completato la configurazione TFA, fai clic su **Salva** per aggiornare l&#39;account.

1. Autentica la sessione corrente con TFA.

   - Esci dal tuo account.
   - Accedi con il tuo nome utente e la tua password.
   - Quando richiesto, immetti il codice TFA per `accounts.magento.cloud` voce dall&#39;applicazione di autenticazione sul dispositivo mobile.

### Gestione dei codici di configurazione e ripristino TFA

Puoi gestire la configurazione TFA per un account Adobe Commerce su infrastruttura cloud da _Sicurezza_ sezione sul _Il mio profilo_ pagina.

1. Accedi a [account personale](https://console.adobecommerce.com).

1. Nel menu dell’account in alto a destra, fai clic su **[!UICONTROL My Profile]**.

1. Il giorno _Il mio profilo_ , fare clic su **[!UICONTROL Security]** scheda.

1. Utilizza i collegamenti disponibili per aggiornare le impostazioni TFA per il tuo account Adobe Commerce sull’infrastruttura cloud:

   - Disattiva TFA
   - Reimposta l&#39;applicazione di autenticazione
   - Aggiungere o rimuovere browser attendibili
   - Visualizza o aggiorna i codici di recupero TFA sul tuo account

### Creare un token API

È possibile scambiare un token API con un token di accesso OAuth 2, che può quindi essere utilizzato per autenticare le richieste.

Nei progetti in cui è abilitata l’imposizione MFA, è necessario disporre di un token API per abilitare l’accesso SSH per gli utenti del computer e i processi automatizzati.

>[!IMPORTANT]
>
>Valori dei token API di Protect per l’account. Non esporre il valore in esempi di codice, acquisizioni di schermate o comunicazioni client-server non sicure. Inoltre, non esporre il valore nel codice sorgente memorizzato negli archivi pubblici.

**Per creare un token API**:

1. Accedi a [account personale](https://console.adobecommerce.com).

1. Nel menu dell’account in alto a destra, fai clic su **[!UICONTROL My Profile]**.

1. Il giorno _Il mio profilo_ , fare clic su **[!UICONTROL API tokens]** scheda.

1. Clic **[!UICONTROL Create API token]** e inserisci un nome, ad esempio, specifica un nome che corrisponda a quello dell’utente del computer o del processo automatizzato che utilizza il token API.

   ![Token API](../../assets/api-token-name.png)

1. Clic **[!UICONTROL Create API token]**.
