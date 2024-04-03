---
title: Variabili ADMIN
description: Consulta un elenco delle variabili di ambiente utilizzate per l’installazione di Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Configuration, Install, Roles/Permissions
role: Developer
exl-id: 2829a9dc-40bb-4665-886e-a56d98468fc1
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 0%

---

# Variabili amministratore

Gli utenti con accesso amministrativo al progetto Adobe Commerce on Cloud Infrastructure possono utilizzare le seguenti variabili di ambiente del progetto per ignorare le impostazioni di configurazione dell’account utente amministratore per accedere all’interfaccia utente amministratore.

## Credenziali amministratore

Durante l’installazione di Commerce, puoi sovrascrivere le credenziali utente amministratore con le variabili ADMIN riportate nella tabella seguente.

Se desideri modificare i valori dopo l’installazione, connettiti all’ambiente utilizzando SSH e utilizza Adobe Commerce CLI [`admin:user` comando](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) per creare o modificare le credenziali utente Admin.

| Variabile | Predefinito | Descrizione |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Indirizzo e-mail proprietario licenza | Un nome utente per l’utente amministratore con la possibilità di creare altri utenti, inclusi gli utenti amministratori. |
| `ADMIN_EMAIL` |                             | Indirizzo e-mail dell’utente amministratore. Questo indirizzo viene utilizzato per inviare le notifiche di reimpostazione della password. |
| `ADMIN_PASSWORD` |                             | Password per l&#39;utente amministratore. Al momento della creazione del progetto, viene generata una password casuale e viene inviata un’e-mail al Proprietario della licenza. Durante la creazione del progetto, il Proprietario della licenza avrebbe dovuto modificare la password. Contattare il proprietario della licenza per ottenere la password aggiornata. |
| `ADMIN_LOCALE` | `en_US` | Impostazioni locali predefinite utilizzate dall&#39;amministratore. |

## URL amministratore

Utilizza la seguente variabile di ambiente per proteggere l’accesso all’interfaccia utente di amministrazione. Se specificato, questo valore sostituisce l&#39;URL predefinito durante l&#39;installazione.

`ADMIN_URL`- URL relativo per accedere all&#39;interfaccia utente di amministrazione. L’URL predefinito è `/admin`. Per motivi di sicurezza, l’Adobe consiglia di impostare l’URL predefinito su un URL amministratore univoco e personalizzato, difficilmente intuibile.

### Modificare l’URL dell’amministratore

L’Adobe consiglia di modificare la variabile a livello di ambiente per l’URL amministratore dopo l’installazione. Configura questa impostazione per motivi di sicurezza prima di creare diramazioni dal duplicato `master` ambiente. Tutti i rami creati da `master` branch eredita le variabili a livello di ambiente e i relativi valori.

Utilizza il `magento-cloud variable:update` per aggiornare il valore della variabile. (Il `variable:set` il comando è stato dichiarato obsoleto e non è disponibile.) L’esempio che segue aggiorna ADMIN_URL in `newAdmin_A8v10`:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>Il `ADMIN_URL` value accetta lettere (a-z o A-Z), numeri (0-9) e il carattere di sottolineatura (_) per un percorso amministratore personalizzato. Gli spazi o altri caratteri sono **non** accettato.

**Per modificare l’URL utilizzando[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Nella panoramica del progetto, seleziona l’ambiente e fai clic sull’icona di configurazione.

   ![Configurazione del progetto](../../assets/icon-configure.png){width="36"}

1. Seleziona la **Variabili** scheda.

1. Clic **Crea variabile**.

1. Immetti quanto segue:

   - **Nome variabile** = `ADMIN_URL`
   - **valore** = Nuovo URL. Ad esempio, imposta l’URL amministratore su `magento_A8v10`.

   Per impostazione predefinita, `Available during runtime` e `Make inheritable` sono selezionati.

1. Clic **Crea variabile** e attendi il completamento dell’implementazione. Questo pulsante è visibile solo se i campi obbligatori contengono valori.
