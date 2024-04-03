---
title: '[!DNL Onboarding] a Commerce'
description: Accedi al tuo account cloud e configura un progetto Adobe Commerce su infrastruttura cloud.
role: Admin
recommendations: noDisplay, catalog
exl-id: c6b768d7-d835-4a8d-aad9-1c0324f7570d
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '760'
ht-degree: 0%

---

# [!DNL Onboarding] a Commerce

Dopo l’Adobe di un abbonamento a un’infrastruttura cloud di Commerce on, il progetto iniziale e l’accesso al codice sono disponibili solo per la persona designata come proprietario della licenza (proprietario dell’account).

Il Proprietario della licenza è la persona dell’organizzazione aziendale o finanziaria che gestisce i pagamenti e altre transazioni aziendali per l’account Adobe Commerce sull’infrastruttura cloud. Questa persona funge da punto di contatto con l’Adobe. Se devi cambiare il Proprietario della licenza sul tuo account, devi contattare il tuo Account Team Adobe.

Per integrare rapidamente il progetto e iniziare a sviluppare il sito per l’implementazione live, devi completare la configurazione e [!DNL onboarding] attività. In genere, il proprietario della licenza avvia il processo proteggendo l’accesso come amministratore e creando utenti amministratori tecnici che possono essere utili per le attività di configurazione, personalizzazione e sviluppo.

## Registrati a un account Cloud

Se non disponi di un account Adobe Commerce su infrastruttura cloud, contatta [Vendite]. Quando ti registri, Adobe crea il tuo account e ti invia un’e-mail di benvenuto con le istruzioni per accedere all’interfaccia del progetto. L’e-mail contiene un collegamento che ti consente di accedere al tuo account e completare la configurazione iniziale del progetto.

### Cloud [!DNL Onboarding] UI

La pagina del progetto Adobe Commerce su infrastruttura cloud in ([!DNL Onboarding] UI) fornisce un elenco di controllo introduttivo per configurare il progetto e i servizi, determinare l’accesso e iniziare a sviluppare. Dall&#39;OBUI, è possibile:

- Aggiungi un amministratore tecnico, un utente con privilegi avanzati che può gestire il progetto e i rami
- Accedi all’ambiente del progetto, incluso un collegamento a [!DNL Cloud Console]
- Completare un elenco di controllo per il test di accettazione rapida utente (UAT) con collegamenti ad altri test

**Per aprire la pagina del progetto**:

1. Accedi al tuo [Account cliente Adobe Commerce](https://account.magento.com/customer/account/login).

1. Il giorno _Il mio account_ , fare clic su **[!UICONTROL Commerce]** per visualizzare i progetti nel tuo account.

1. Clic **Visualizza pagina progetto** nel [Sezione Progetti](https://cloud.magento.com/cloud/project/).

1. Fai clic sul nome del progetto e apri la pagina Progetto cloud ([!DNL Onboarding] UI).

   ![Pagina progetto OBUI](../assets/onboarding-ui.png)

   Sfoglia il portale per informazioni e opzioni utili per iniziare a pianificare il progetto, sviluppare il codice e prepararsi per UAT e il lancio del sito.

## Accedere al progetto e aggiungere utenti

Il Proprietario della licenza può aggiungere account utente per fornire accesso al codice, gestire rami, immettere biglietti e ambienti di supporto. Questi account utente possono includere sviluppo interno, consulenti e specialisti delle soluzioni.

In genere, l’unico utente che il proprietario della licenza deve creare è _Amministratore tecnico_. L’amministratore tecnico necessita di un account utente con accesso di amministratore per creare account utente per gli sviluppatori, impostare le autorizzazioni dell’ambiente e gestire tutti i rami e gli ambienti. L’amministratore tecnico può essere uno sviluppatore, un consulente, un [Adobe Solution Partner](https://business.adobe.com/products/magento/partners.html)o te stesso.

Puoi creare un Amministratore tecnico tramite il portale del progetto, dal [!DNL Cloud Console], o dalla riga di comando utilizzando `magento-cloud` CLI

### Registrazione utente

Puoi aggiungere utenti registrati al tuo Adobe Commerce solo a progetti e ambienti di infrastruttura cloud. Se hai un nuovo utente, chiedi loro di [registrarsi per un account](https://account.magento.com/customer/account/login/) e per fornire l’indirizzo e-mail associato al loro profilo account.

### Accesso all’account condiviso

Il proprietario della licenza può impostare l’accesso condiviso per l’account. L’accesso condiviso consente a dipendenti e fornitori di servizi affidabili di utilizzare il Centro assistenza per inviare e tenere traccia dei ticket di supporto relativi ai progetti di infrastruttura cloud di Adobe Commerce. Per le istruzioni di configurazione, vedere [Accesso condiviso] nell&#39;Help Center.

### [!DNL Cloud Console]

È possibile utilizzare [[!DNL Cloud Console]](cloud-console.md) per gestire il progetto, aggiungi account utente e inizia a sviluppare il tuo store. Il proprietario della licenza, gli utenti amministratori tecnici e gli sviluppatori possono utilizzare [!DNL Cloud Console] per gestire tutti gli ambienti e i rami, le variabili di ambiente, le impostazioni di ambiente e i percorsi.

**Per accedere al[!DNL Cloud Console]**:

1. Accedi a [Il mio account](https://account.magento.com/customer/account/login).

1. Il giorno _Il mio account_ , fare clic su **[!UICONTROL Commerce]** per visualizzare i progetti nel tuo account.

1. Fai clic su **Progetti** e seleziona un progetto.

1. Clic **Accesso all&#39;infrastruttura** e quindi fare clic su **Accesso al progetto (interfaccia Web)**.

   ![Portale dei progetti cloud](../assets/obui-project-access.png)

## Registrati per lo stato di Adobe

Ottieni aggiornamenti su Adobe Commerce negli ambienti di piattaforma dell’infrastruttura cloud e servizi correlati da [Pagina di stato].

La pagina fornisce uno stato per i componenti e i servizi di Adobe Commerce seguito da notifiche su rapporti di incidenti, aggiornamenti del servizio, interruzioni pianificate e manutenzione pianificata. Chiunque lavori sul tuo progetto può iscriversi al sito di stato di Adobe Commerce per ricevere notifiche e aggiornamenti sugli eventi tramite e-mail o Slack. Puoi personalizzare l’abbonamento allo stato dell’Adobe per tenere traccia di prodotti specifici in base alle regioni e agli eventi.

>[!TIP]
>
> Apri il nuovo [!DNL Cloud Console] e visualizzare le attività relative a progetti e ambiente.
>
>**Passaggio successivo**: [Accedi a Cl[!DNL ]oud Console](cloud-console.md)

<!-- link definitions -->

[Vendite]: https://business.adobe.com/products/magento/get-demo.html
[Accesso condiviso]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#shared-access
[Pagina di stato]: https://status.adobe.com/products/503473
