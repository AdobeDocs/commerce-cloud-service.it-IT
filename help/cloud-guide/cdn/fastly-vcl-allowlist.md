---
title: VCL personalizzato per consentire le richieste
description: Filtra le richieste in arrivo e consenti l’accesso per indirizzo IP per i siti Adobe Commerce tramite un elenco ACL Fastly Edge e uno snippet VCL personalizzato.
feature: Cloud, Configuration, Security
exl-id: a6ee958a-c3d3-47be-b2df-510707f551fc
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '848'
ht-degree: 0%

---

# VCL personalizzato per consentire le richieste

Puoi utilizzare un elenco ACL Fastly Edge con uno snippet di codice VCL personalizzato per filtrare le richieste in ingresso e consentire l’accesso per indirizzo IP. L&#39;elenco ACL specifica gli indirizzi IP da consentire.

Crea un inserisco nell&#39;elenco Consentiti di per limitare l’accesso all’ambiente di staging in modo che siano consentite solo le richieste da indirizzi IP specifici per sviluppatori interni e servizi esterni approvati. È inoltre possibile creare un inserisco nell&#39;elenco Consentiti di per proteggere l’accesso all’amministratore negli ambienti di staging e produzione.

L&#39;esempio seguente mostra come utilizzare uno snippet VCL personalizzato con un [Elenco di controllo di accesso rapido](https://docs.fastly.com/guides/access-control-lists/about-acls) per proteggere l’accesso all’Admin per un ambiente di progetto Adobe Commerce on cloud infrastructure. Quando aggiungi lo snippet VCL personalizzato all’ambiente Cloud, Fastly consente solo le richieste di indirizzi IP inclusi nell’ACL.

>[!TIP]
>
>Per gli ambienti di staging e integrazione che non devono essere accessibili al pubblico, utilizza l’opzione di controllo dell’accesso HTTP disponibile nella [[!DNL Cloud Console]](../project/overview.md#access-the-project-web-interface) per gestire l&#39;accesso all&#39;intero sito in base all&#39;indirizzo IP.

**Prerequisiti:**


{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Elenco di indirizzi IP client da includere nel inserisco nell&#39;elenco Consentiti di

## Crea ACL Edge per consentire gli indirizzi IP client

Gli ACL Edge creano elenchi di indirizzi IP per la gestione dell’accesso al sito. In questo esempio, puoi creare un ACL Edge e aggiungere l’elenco di indirizzi IP client autorizzati ad accedere all’Admin per l’ambiente del progetto.

{{admin-login-step}}

1. Clic **Negozi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandi **Cache a pagina intera** > **Configurazione rapida** > **ACL Edge**.

1. Crea il contenitore ACL:

   - Clic **Aggiungi ACL**.

   - Il giorno *Contenitore ACL* , immettere un valore **Nome ACL**—`allowlist`.

   - Seleziona **Attiva dopo la modifica** per distribuire le modifiche alla versione della configurazione del servizio Fastly che si sta modificando.

   - Clic **Carica** per collegare l’ACL alla configurazione del servizio Fastly.

1. Aggiungi l’elenco di indirizzi IP consentiti per accedere all’Admin:

   - Fai clic sull’icona Impostazioni per `allowlist` ACL

   - Aggiungi e salva *Valore IP* per ogni indirizzo IP del client.

   - Clic **Annulla** per tornare alla pagina di configurazione del sistema.

1. Clic **Salva configurazione**.

1. Aggiorna la cache in base alla notifica nella parte superiore della pagina.

## Crea lo snippet VCL personalizzato per proteggere l’accesso amministratore

Il seguente codice snippet VCL personalizzato (formato JSON) mostra la logica per filtrare le richieste all’amministratore e consentire l’accesso se l’indirizzo IP del client corrisponde a un indirizzo in `allowlist` ACL

```json
{
  "name": "allowlist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ((req.url ~ \"^/admin\") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 \"Forbidden\"; }"
}
```

Prima di [creazione di uno snippet personalizzato](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/cdn/custom-vcl-snippets/fastly-vcl-allowlist.html#add-the-custom-vcl-snippet) in questo esempio, controlla i valori per determinare se è necessario apportare modifiche. Quindi inserisci ciascun valore nei rispettivi campi, ad esempio `type` nel campo Tipo, `content` nel campo Contenuto.

- `name` — Nome dello snippet VCL. Per questo esempio, `allowlist`.

- `priority` — Determina quando viene eseguito lo snippet VCL. La priorità è `5` per eseguire immediatamente e verificare se le richieste dell’amministratore provengono da un indirizzo IP consentito. Lo snippet viene eseguito prima di qualsiasi snippet VCL di Magento predefinito (`magentomodule_*`) ha assegnato una priorità di 50. Impostare la priorità per ogni frammento personalizzato su un valore maggiore o minore di 50 a seconda di quando si desidera eseguire il frammento. I frammenti con numeri di priorità inferiore vengono eseguiti per primi.

- `type` — Specifica una posizione in cui inserire lo snippet nel codice VCL con versione. Questo VCL è un `recv` tipo di frammento che aggiunge il codice di frammento al `vcl_recv` subroutine sotto il codice VCL Fastly predefinito e sopra qualsiasi oggetto.

- `content` — Frammento di codice VCL da eseguire. In questo esempio, il codice filtra le richieste all’amministratore e consente l’accesso se l’indirizzo IP del client corrisponde a un indirizzo in `allowlist` ACL Se l’indirizzo non corrisponde, la richiesta viene bloccata con un `403 Forbidden` errore.

  Se l’URL per l’amministratore è stato modificato, sostituisci il valore di esempio `/admin` con l’URL dell’ambiente. Ad esempio: `/company-admin`.

Nell’esempio di codice, la condizione `!req.http.Fastly-FF` è importante quando si utilizza [Schermatura origine](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding). Non rimuovere o modificare questo codice.

Dopo aver esaminato e aggiornato il codice per l’ambiente, utilizza uno dei metodi seguenti per aggiungere lo snippet VCL personalizzato alla configurazione del servizio Fastly:

- [Aggiungi lo snippet VCL personalizzato dall’amministratore](#add-the-custom-vcl-snippet). Questo metodo è consigliato se puoi accedere all’Admin. (Richiede [Modulo CDN Fastly per il Magento 2 versione 1.2.58](fastly-configuration.md#upgrade) o versione successiva).

- Salva l’esempio di codice JSON in un file (ad esempio, `allowlist.json`) e [caricala utilizzando l’API Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilizza questo metodo se non riesci ad accedere all’Admin.

## Aggiungere lo snippet VCL personalizzato

{{admin-login-step}}

1. Clic **Negozi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandi **Cache a pagina intera** > **Configurazione rapida** > **Snippet VCL personalizzati**.

1. Clic **Crea snippet personalizzato**.

1. Aggiungi i valori dello snippet VCL:

   - **Nome** — `allowlist`

   - **Tipo** — `recv`

   - **Priorità** — `5`

   - Aggiungi il **VCL** contenuto frammento:

     ```conf
     if ((req.url ~ "^/admin") && !(client.ip ~ allowlist) && !req.http.Fastly-FF) { error 403 "Forbidden";}
     ```

1. Clic **Crea** per generare il file di snippet VCL con il modello di nome `type_priority_name.vcl`, ad esempio `recv_5_allowlist.vcl`

1. Dopo il ricaricamento della pagina, fai clic su **Carica VCL in Fastly** nel *Configurazione rapida* per aggiungere il file alla configurazione del servizio Fastly.

1. Al termine del caricamento, aggiorna la cache in base alla notifica nella parte superiore della pagina.

Convalida infine la versione aggiornata del codice VCL durante il processo di caricamento. Se la convalida non riesce, modifica lo snippet VCL personalizzato per risolvere il problema. Quindi, carica nuovamente il file VCL.

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
