---
title: VCL personalizzato per consentire le richieste
description: Filtra le richieste in arrivo e consenti l’accesso per indirizzo IP ai siti Adobe Commerce tramite un elenco ACL Fastly Edge e uno snippet VCL personalizzato.
feature: Cloud, Configuration, Security
exl-id: a6ee958a-c3d3-47be-b2df-510707f551fc
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# VCL personalizzato per consentire le richieste

Puoi utilizzare un elenco Fastly Edge ACL con uno snippet di codice VCL personalizzato per filtrare le richieste in ingresso e consentire l’accesso per indirizzo IP. L&#39;elenco ACL specifica gli indirizzi IP da consentire.

Crea un inserisco nell&#39;elenco Consentiti di per limitare l’accesso all’ambiente di staging in modo che siano consentite solo le richieste da indirizzi IP specifici per sviluppatori interni e servizi esterni approvati. Puoi anche creare un inserisco nell&#39;elenco Consentiti di per proteggere l’accesso all’amministratore negli ambienti di staging e produzione.

Nell&#39;esempio seguente viene illustrato come utilizzare uno snippet VCL personalizzato con un elenco di controllo di accesso [Fastly](https://docs.fastly.com/guides/access-control-lists/about-acls) per proteggere l&#39;accesso all&#39;amministratore per un ambiente di progetto Adobe Commerce su infrastruttura cloud. Quando aggiungi lo snippet VCL personalizzato all’ambiente Cloud, Fastly consente solo le richieste di indirizzi IP inclusi nell’ACL.

>[!TIP]
>
>Per gli ambienti di gestione temporanea e integrazione che non devono essere accessibili pubblicamente, utilizzare l&#39;opzione di controllo dell&#39;accesso HTTP disponibile in [[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface) per gestire l&#39;accesso all&#39;intero sito in base all&#39;indirizzo IP.

**Prerequisiti:**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Elenco di indirizzi IP client da includere nel inserisco nell&#39;elenco Consentiti di

## Creare un ACL di Edge per consentire gli indirizzi IP client

Gli elenchi di indirizzi IP creati dagli ACL di Edge consentono di gestire l’accesso al sito. In questo esempio, puoi creare un ACL di Edge e aggiungere l’elenco di indirizzi IP client autorizzati ad accedere all’Admin per l’ambiente del progetto.

{{admin-login-step}}

1. Fai clic su **Archivi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandi **Cache a pagina intera** > **Configurazione rapida** > **ACL Edge**.

1. Crea il contenitore ACL:

   - Fare clic su **Aggiungi ACL**.

   - Nella pagina *Contenitore ACL* immettere un **nome ACL**—`allowlist`.

   - Seleziona **Attiva dopo la modifica** per distribuire le modifiche alla versione della configurazione del servizio Fastly che stai modificando.

   - Fai clic su **Carica** per allegare l&#39;ACL alla configurazione del servizio Fastly.

1. Aggiungi l’elenco di indirizzi IP consentiti per accedere all’Admin:

   - Fare clic sull&#39;icona Impostazioni per l&#39;ACL `allowlist`.

   - Aggiungi e salva il *valore IP* per ogni indirizzo IP del client.

   - Fai clic su **Annulla** per tornare alla pagina di configurazione del sistema.

1. Fai clic su **Salva configurazione**.

1. Aggiorna la cache in base alla notifica nella parte superiore della pagina.

## Crea lo snippet VCL personalizzato per proteggere l’accesso amministratore

Il seguente codice snippet VCL personalizzato (formato JSON) mostra la logica per filtrare le richieste all&#39;amministratore e consentire l&#39;accesso se l&#39;indirizzo IP del client corrisponde a un indirizzo nell&#39;ACL `allowlist`.

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

Prima di [creare uno snippet personalizzato](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html#add-the-custom-vcl-snippet) da questo esempio, controlla i valori per determinare se è necessario apportare modifiche. Immettere quindi ogni valore nei rispettivi campi, ad esempio `type` nel campo Tipo e `content` nel campo Contenuto.

- `name` — Nome dello snippet VCL. Per questo esempio, `allowlist`.

- `priority` — Determina quando viene eseguito lo snippet VCL. La priorità è `5` per l&#39;esecuzione immediata e verificare se le richieste dell&#39;amministratore provengono da un indirizzo IP consentito. Il frammento viene eseguito prima che a uno dei frammenti VCL di Magento predefiniti (`magentomodule_*`) sia assegnata una priorità di 50. Impostare la priorità per ogni frammento personalizzato su un valore maggiore o minore di 50 a seconda di quando si desidera eseguire il frammento. I frammenti con numeri di priorità inferiore vengono eseguiti per primi.

- `type` — Specifica una posizione in cui inserire lo snippet nel codice VCL con versione. Questo VCL è un tipo di snippet `recv` che aggiunge il codice del snippet alla subroutine `vcl_recv` sotto il codice VCL Fastly predefinito e sopra qualsiasi oggetto.

- `content` — Frammento di codice VCL da eseguire. In questo esempio, il codice filtra le richieste all&#39;amministratore e consente l&#39;accesso se l&#39;indirizzo IP del client corrisponde a un indirizzo nell&#39;ACL `allowlist`. Se l&#39;indirizzo non corrisponde, la richiesta viene bloccata con un errore `403 Forbidden`.

  Se l&#39;URL per l&#39;amministratore è stato modificato, sostituire il valore di esempio `/admin` con l&#39;URL per l&#39;ambiente. Ad esempio, `/company-admin`.

Nell&#39;esempio di codice, la condizione `!req.http.Fastly-FF` è importante quando si utilizza [Origin Shielding](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding). Non rimuovere o modificare questo codice.

Dopo aver esaminato e aggiornato il codice per l’ambiente, utilizza uno dei metodi seguenti per aggiungere lo snippet VCL personalizzato alla configurazione del servizio Fastly:

- [Aggiungi lo snippet VCL personalizzato dall&#39;amministratore](#add-the-custom-vcl-snippet). Questo metodo è consigliato se puoi accedere all’Admin. (Richiede il modulo CDN [Fastly per il Magento 2 versione 1.2.58](fastly-configuration.md#upgrade) o successiva.)

- Salva l&#39;esempio di codice JSON in un file (ad esempio, `allowlist.json`) e [caricalo utilizzando l&#39;API Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilizza questo metodo se non riesci ad accedere all’Admin.

## Aggiungere lo snippet VCL personalizzato

{{admin-login-step}}

1. Fai clic su **Archivi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandi **Cache a pagina intera** > **Configurazione rapida** > **Snippet VCL personalizzati**.

1. Fare clic su **Crea snippet personalizzato**.

1. Aggiungi i valori dello snippet VCL:

   - **Nome** - `allowlist`

   - **Tipo** - `recv`

   - **Priorità** - `5`

   - Aggiungi il contenuto dello snippet **VCL**:

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. Fai clic su **Crea** per generare il file snippet VCL con il modello nome `type_priority_name.vcl`, ad esempio `recv_5_allowlist.vcl`

1. Dopo il ricaricamento della pagina, fare clic su **Carica VCL in Fastly** nella sezione *Fastly Configuration* per aggiungere il file alla configurazione del servizio Fastly.

1. Al termine del caricamento, aggiorna la cache in base alla notifica nella parte superiore della pagina.

Convalida infine la versione aggiornata del codice VCL durante il processo di caricamento. Se la convalida non riesce, modifica lo snippet VCL personalizzato per risolvere il problema. Quindi, carica nuovamente il file VCL.

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
