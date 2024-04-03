---
title: Imposta metodi di pagamento PayPal
description: Imposta i metodi di pagamento PayPal per Adobe Commerce sull'infrastruttura cloud.
feature: Cloud, Checkout, Payments
exl-id: e52fd719-f936-4e8b-8222-af133389d9e2
source-git-commit: aa1a334ca1383559194ca75247679c6fb5411802
workflow-type: tm+mt
source-wordcount: '669'
ht-degree: 0%

---

# Imposta metodi di pagamento PayPal

Adobe Commerce su infrastruttura cloud fornisce uno strumento di onboarding per configurare gli account di pagamento PayPal Express direttamente tramite l’Amministratore. Questo strumento è disponibile per le versioni ECE 2.1.8 e successive. Per supportare meglio la pubblicazione e la verifica dei metodi di pagamento PayPal, puoi abilitare e configurare il tuo conto PayPal Express Checkout per gli account sandbox o di produzione.

Puoi configurare l’account sandbox o l’account di produzione in ogni ambiente:

* Per gli ambienti di integrazione e staging, imposta le credenziali Sandbox.
* Per l’ambiente di produzione, imposta le credenziali Sandbox per il test iniziale, quindi sostituisci con le credenziali di produzione live per un archivio avviato.

## Conto PayPal

Anche se è meglio utilizzare un conto di esercente PayPal preparato e configurato, puoi creare un account o aggiornare un account personale tramite l&#39;Amministratore.

[!DNL PayPal onboarding] supporta la connessione con i seguenti account:

* Conto PayPal Business
* Conto personale PayPal, conversione in un conto Business. Se disponi già di un account PayPal personale, puoi accedere con tali credenziali e aggiornare l&#39;account a un account aziendale al termine della sincronizzazione.

Se non hai un conto PayPal esistente, creane uno. Immetti un indirizzo e-mail per un nuovo account. Se non viene trovato un conto PayPal corrispondente, seguire le istruzioni per creare un conto PayPal Business. Oppure puoi creare un account direttamente tramite [PayPal](https://www.paypal.com/us/webapps/mpp/account-selection).

### Limitazioni di PayPal

PayPal supporta la connessione a PayPal Express Checkout per i paesi di tutto il mondo, ad eccezione delle seguenti limitazioni:

* India e Giappone (i futuri aggiornamenti PayPal potrebbero supportare questi account)
* Israele

Per il Brasile, è necessario disporre di un account PayPal aziendale esistente per connettersi. Non puoi convertire un conto PayPal personale esistente per il Brasile durante questo processo. Se hai bisogno di un account, [creare un conto PayPal aziendale](https://www.paypal.com/us/webapps/mpp/account-selection).

## Configura pagamento PayPal Express

Per configurare PayPal Express Checkout:

1. Accedi all’Admin per l’ambiente.
1. Nel menu di navigazione a sinistra, seleziona **Negozi** > **Configurazione**, quindi seleziona **Vendite** > **Metodi di pagamento**.
1. Per PayPal, seleziona **Configura**. I campi di configurazione vengono visualizzati in sezioni espandibili per le impostazioni Pagamento rapido, Advertise PayPal Credit e Basic e Advanced.
1. Collega il tuo conto PayPal. Fino a quando l’account non è connesso, le opzioni da abilitare sono disabilitate. Per informazioni dettagliate sugli account disponibili e supportati per la connessione e le limitazioni, consulta [Conto PayPal](#paypal-account).

   * Per collegare il tuo conto PayPal in diretta, fai clic su Connetti con PayPal e segui le istruzioni. Qualsiasi cliente acquista utilizzando un PayPal live completa e addebita attivamente i clienti in un negozio live.
   * Per collegare l’account sandbox per il test, fai clic su Credenziali sandbox e segui le istruzioni. Tutti gli acquisti dei clienti che utilizzano un PayPal Sandbox sono completati senza addebitare attivamente i clienti.

1. Configura le impostazioni di pagamento rapido per l&#39;autenticazione e l&#39;utilizzo dell&#39;API PayPal:

   * **E-mail associata al conto PayPal dell&#39;esercente** (Facoltativo) inserisci l&#39;indirizzo email associato al tuo conto PayPal dell&#39;esercente. Per questa e-mail viene fatta distinzione tra maiuscole e minuscole.
   * **Metodi di autenticazione API** come firma API o certificato API.
   * Nome utente API, password e firma acquisiti dal tuo account PayPal.
   * **Modalità sandbox** selezionare Sì o No per indicare se le credenziali immesse sono per sandbox. Se sono state immesse le credenziali di produzione, selezionare No.
   * **L&#39;API utilizza il proxy** selezionare Sì o No per impostare se il sistema utilizza un server proxy per stabilire una connessione tra Adobe Commerce e il sistema di pagamento PayPal. Se Sì, immettere l&#39;host e la porta proxy.

1. Per informazioni dettagliate e passaggi per la configurazione dell’account, consulta [Pagamento PayPal Express](https://docs.magento.com/user-guide/payment/paypal-express-checkout.html) a partire dal passaggio 2, completare le impostazioni richieste.

Con l&#39;account configurato e autenticato, puoi abilitare e disabilitare le opzioni di pagamento PayPal in Impostazioni PayPal obbligatorie:

* **Abilita questa soluzione** visualizza il metodo di pagamento PayPal ai clienti tramite il sito Web.
* **Abilita esperienza di estrazione nel contesto**
* **Abilita credito PayPal** consente ai clienti di PayPal il finanziamento del credito senza costi aggiuntivi. PayPal paga l&#39;ordine anticipatamente, gestendo tutti i rimborsi per il credito direttamente con il cliente.

## Variabili PayPal

Quando utilizzi lo strumento di onboarding PayPal con Adobe Commerce sull’infrastruttura cloud, aggiungi la variabile seguente al `variables:env` sezione del `magento.app.yaml` file.

```yaml
# Environment variables
variables:
  env:
    CONFIG__DEFAULT__PAYPAL_ONBOARDING__MIDDLEMAN_DOMAIN: 'payment-broker.magento.com'
```

Se esegui l’aggiornamento a 2.2 dalla versione 2.1.8 o successiva, devi comunque aggiungere questa variabile.
