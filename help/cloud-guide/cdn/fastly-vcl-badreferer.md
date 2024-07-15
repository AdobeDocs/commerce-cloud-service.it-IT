---
title: Blocca spam di riferimento
description: Blocca lo spam di riferimento dal sito utilizzando il dizionario Fastly Edge e uno snippet VCL personalizzato.
feature: Cloud, Configuration, Security
exl-id: 665bac93-75db-424f-be2c-531830d0e59a
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '684'
ht-degree: 0%

---

# Blocca spam di riferimento

Nell&#39;esempio seguente viene illustrato come configurare il [dizionario Fastly Edge](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) con uno snippet VCL personalizzato per bloccare lo spam di riferimento dal sito Adobe Commerce sul sito dell&#39;infrastruttura cloud.

>[!NOTE]
>
>È consigliabile aggiungere configurazioni VCL personalizzate a un ambiente di staging in cui è possibile testarle prima di eseguirle nell’ambiente di produzione.

**Prerequisiti:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Controlla i registri del sito per individuare falsi URL di riferimento e crea un elenco di domini da bloccare.

## Creazione di un elenco Bloccati di referrer

I dizionari Edge creano coppie chiave-valore accessibili alle funzioni VCL durante l&#39;elaborazione dello snippet VCL. In questo esempio, crei un dizionario perimetrale che fornisce l’elenco dei siti web di provenienza da bloccare.

{{admin-login-step}}

1. Fai clic su **Archivi** > **Impostazioni** > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandere **Cache a pagina intera** > **Configurazione rapida** > **Dizionari Edge**.

1. Crea il contenitore Dizionario:

   - Fai clic su **Aggiungi contenitore**.

   - Nella pagina *Contenitore* immettere un **Nome dizionario**—`referrer_blocklist`.

   - Seleziona **Attiva dopo la modifica** per distribuire le modifiche alla versione della configurazione del servizio Fastly che stai modificando.

   - Fai clic su **Carica** per allegare il dizionario alla configurazione del servizio Fastly.

1. Aggiungere l&#39;elenco dei nomi di dominio da bloccare al dizionario `referrer_blocklist`:

   - Fare clic sull&#39;icona Impostazioni per il dizionario `referrer_blocklist`.

   - Aggiungi e salva coppie chiave-valore nel nuovo dizionario. Per questo esempio, ogni **Chiave** è il nome di dominio di un URL referente da bloccare e **Valore** è `true`.

     ![Aggiungi elementi dizionario referrer non validi](../../assets/cdn/fastly-referrer-blocklist-dictionary.png)

   - Fai clic su **Annulla** per tornare alla pagina di configurazione del sistema.

1. Fai clic su **Salva configurazione**.

1. Aggiorna la cache in base alla notifica nella parte superiore della pagina.

Per ulteriori informazioni sui dizionari di Edge, vedere [Creazione e utilizzo di dizionari di Edge](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api) e [snippet VCL personalizzati](https://docs.fastly.com/guides/edge-dictionaries/working-with-dictionaries-using-the-api#custom-vcl-examples) nella documentazione Fastly.

## Creare un frammento VCL personalizzato per bloccare lo spam del referente

Il seguente codice snippet VCL personalizzato (formato JSON) mostra la logica per controllare e bloccare le richieste. Lo snippet VCL acquisisce l&#39;host di un sito Web di provenienza in un&#39;intestazione, quindi confronta il nome host con l&#39;elenco di URL nel dizionario `referrer_blocklist`. Se il nome host corrisponde, la richiesta viene bloccata con un errore `403 Forbidden`.

```json
{
  "name": "block_bad_referrer",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "set req.http.Referer-Host = regsub(req.http.Referer, \"^https?:\/\/?([^:\/s]+).*$\", \"\\1\"); if (table.lookup(referrer_blocklist, req.http.Referer-Host)) { error 403 \"Forbidden\"; }"
}
```

Prima di creare uno snippet basato su questo esempio, esaminare i valori per determinare se è necessario apportare modifiche:

- `name` — Nome dello snippet VCL. In questo esempio è stato utilizzato `block_bad_referrer`.

- `dynamic` — Il valore 0 indica un [frammento normale](https://docs.fastly.com/en/guides/using-regular-vcl-snippets) da caricare nella VCL con versione per la configurazione Fastly.

- `priority` — Determina quando viene eseguito lo snippet VCL. La priorità è `5` per eseguire questo codice snippet prima che a uno qualsiasi dei snippet VCL di Magento predefiniti (`magentomodule_*`) sia assegnata una priorità di 50. Impostare la priorità per ogni frammento personalizzato su un valore maggiore o minore di 50 a seconda di quando si desidera eseguire il frammento. I frammenti con numeri di priorità inferiore vengono eseguiti per primi.

- `type` — specifica un percorso in cui inserire lo snippet nella versione VCL. In questo esempio, lo snippet VCL è uno snippet `recv`. Quando il frammento viene inserito nella versione VCL, viene aggiunto alla subroutine `vcl_recv`, sotto il codice VCL Fastly predefinito e sopra qualsiasi oggetto.

- `content` — Frammento di codice VCL da eseguire in una riga, senza interruzioni di riga.

Dopo aver esaminato e aggiornato il codice per l’ambiente, utilizza uno dei metodi seguenti per aggiungere lo snippet VCL personalizzato alla configurazione del servizio Fastly:

- [Aggiungi lo snippet VCL personalizzato dall&#39;amministratore](#add-the-custom-vcl-snippet). Questo metodo è consigliato se puoi accedere all’Admin. (Richiede [Fastly versione 1.2.58](fastly-configuration.md#upgrade) o successiva.)

- Salva l&#39;esempio di codice JSON in un file (ad esempio, `allowlist.json`) e [caricalo utilizzando l&#39;API Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilizza questo metodo se non riesci ad accedere all’Admin.

## Aggiungere lo snippet VCL personalizzato

{{admin-login-step}}

1. Fai clic su **Archivi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandi **Cache a pagina intera** > **Configurazione rapida** > **Snippet VCL personalizzati**.

1. Fare clic su **Crea snippet personalizzato**.

1. Aggiungi i valori dello snippet VCL:

   - **Nome** - `block_bad_referrer`

   - **Tipo** - `recv`

   - **Priorità** - `5`

   - **VCL** contenuto frammento —

     ```conf
     set req.http.Referer-Host = regsub(req.http.Referer,
     "^https?://?([^:/\s]+).*$", "1");
     if (table.lookup(referrer_blocklist, req.http.Referer-Host)) {
       error 403 "Forbidden";
     }
     ```

1. Fai clic su **Crea**.

   ![Crea snippet VCL blocco referente personalizzato](/help/assets/cdn/fastly-create-referrer-block-snippet.png)

1. Dopo il ricaricamento della pagina, fai clic su **Carica VCL in Fastly** nella sezione *Fastly Configuration*.

1. Al termine del caricamento, aggiorna la cache in base alla notifica nella parte superiore della pagina.

Convalida in breve la versione VCL aggiornata durante il processo di caricamento. Se la convalida non riesce, modifica il frammento VCL personalizzato per risolvere eventuali problemi. Quindi, carica nuovamente il file VCL.

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
