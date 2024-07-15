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

Durante l’installazione di Commerce, è possibile sovrascrivere le credenziali utente amministratore con le variabili ADMIN riportate nella tabella seguente.

Se si desidera modificare i valori dopo l&#39;installazione, connettersi all&#39;ambiente utilizzando SSH e utilizzare il comando Adobe Commerce CLI [`admin:user`](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) per creare o modificare le credenziali utente amministratore.

| Variabile | Predefinito | Descrizione |
| -------------- | --------------------------- | ----------- |
| `ADMIN_USERNAME` | Indirizzo e-mail proprietario licenza | Un nome utente per l’utente amministratore con la possibilità di creare altri utenti, inclusi gli utenti amministratori. |
| `ADMIN_EMAIL` |                             | Indirizzo e-mail dell’utente amministratore. Questo indirizzo viene utilizzato per inviare le notifiche di reimpostazione della password. |
| `ADMIN_PASSWORD` |                             | Password per l&#39;utente amministratore. Al momento della creazione del progetto, viene generata una password casuale e viene inviata un’e-mail al Proprietario della licenza. Durante la creazione del progetto, il Proprietario della licenza avrebbe dovuto modificare la password. Contattare il proprietario della licenza per ottenere la password aggiornata. |
| `ADMIN_LOCALE` | `en_US` | Impostazioni locali predefinite utilizzate dall&#39;amministratore. |

## URL amministratore

Utilizza la seguente variabile di ambiente per proteggere l’accesso all’interfaccia utente di amministrazione. Se specificato, questo valore sostituisce l&#39;URL predefinito durante l&#39;installazione.

`ADMIN_URL`: l&#39;URL relativo per accedere all&#39;interfaccia utente di amministrazione. URL predefinito: `/admin`. Per motivi di sicurezza, l’Adobe consiglia di impostare l’URL predefinito su un URL amministratore univoco e personalizzato, difficilmente intuibile.

### Modificare l’URL dell’amministratore

L’Adobe consiglia di modificare la variabile a livello di ambiente per l’URL amministratore dopo l’installazione. Configurare questa impostazione per motivi di sicurezza prima di creare diramazioni dall&#39;ambiente `master` clonato. Tutti i rami creati dal ramo `master` ereditano le variabili a livello di ambiente e i relativi valori.

Utilizzare il comando `magento-cloud variable:update` per aggiornare il valore della variabile. Il comando `variable:set` è obsoleto e non è disponibile. L&#39;esempio seguente aggiorna ADMIN_URL a `newAdmin_A8v10`:

```bash
magento-cloud variable:update ADMIN_URL --value newAdmin_A8v10 -e master
```

>[!NOTE]
>
>Il valore `ADMIN_URL` accetta lettere (a-z o A-Z), numeri (0-9) e il carattere di sottolineatura (_) per un percorso di amministrazione personalizzato. Gli spazi o altri caratteri sono **non** accettati.

**Per modificare l&#39;URL utilizzando[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Selezionare un progetto dall&#39;elenco _Tutti i progetti_.

1. Nella panoramica del progetto, seleziona l’ambiente e fai clic sull’icona di configurazione.

   ![Configurazione del progetto](../../assets/icon-configure.png){width="36"}

1. Selezionare la scheda **Variabili**.

1. Fare clic su **Crea variabile**.

1. Immetti quanto segue:

   - **Nome variabile** = `ADMIN_URL`
   - **valore** = nuovo URL. Ad esempio, impostare l&#39;URL amministratore su `magento_A8v10`.

   Per impostazione predefinita, sono selezionati `Available during runtime` e `Make inheritable`.

1. Fare clic su **Crea variabile** e attendere il completamento della distribuzione. Questo pulsante è visibile solo se i campi obbligatori contengono valori.
