---
title: VCL personalizzato per richieste di blocco
description: Blocca le richieste in ingresso per indirizzo IP utilizzando un elenco di controllo di accesso Edge (ACL) con uno snippet VCL personalizzato.
feature: Cloud, Configuration, Security
exl-id: 1f637612-3858-49d0-91f7-9b8823933cc9
source-git-commit: 0e9ace747cc56808108781e42b97c86756089818
workflow-type: tm+mt
source-wordcount: '961'
ht-degree: 0%

---

# VCL personalizzato per richieste di blocco

Puoi utilizzare il modulo Fastly CDN per il Magento 2 per creare un ACL Edge con un elenco di indirizzi IP che desideri bloccare. Puoi quindi utilizzare tale elenco con uno snippet VCL per bloccare le richieste in ingresso. Il codice controlla l’indirizzo IP della richiesta in ingresso. Se corrisponde a un indirizzo IP incluso nell’elenco ACL, Fastly blocca la richiesta di accesso al sito e restituisce un `403 Forbidden error`. A tutti gli altri indirizzi IP client è consentito l&#39;accesso.

**Prerequisiti:**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

- Elenco di indirizzi IP client da bloccare

## Crea ACL Edge per il blocco degli indirizzi IP client

Puoi creare un ACL di Edge per definire l’elenco di indirizzi IP da bloccare. Dopo aver creato l’ACL, puoi utilizzarlo in uno snippet VCL personalizzato per gestire l’accesso al sito di staging o produzione.

Gestisci l’accesso sia per i siti di staging che per quelli di produzione creando l’ACL Edge con lo stesso nome in entrambi gli ambienti. Il codice snippet VCL si applica a entrambi gli ambienti.

1. Accedi all’amministratore.
1. Accedi a **Negozi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema** > **Cache a pagina intera** > **Configurazione rapida**.
1. Espandi **ACL Edge** sezione.
1. Clic **Aggiungi ACL** per creare un elenco. In questo esempio, assegna all’elenco il nome &quot;inserisce nell&#39;elenco Bloccati di&quot;.
1. Immettere i valori dell&#39;indirizzo IP nell&#39;elenco. Tutti gli indirizzi IP client aggiunti all&#39;elenco sono bloccati e non possono accedere al sito.
1. Facoltativamente, seleziona la **Negato** se necessario.

Puoi fare riferimento all’ACL Edge per nome nel codice dello snippet VCL.

## Creare il file VCL personalizzato per il inserisco nell&#39;elenco Bloccati di

>[!NOTE]
>
>Questo esempio mostra agli utenti avanzati come creare uno snippet di codice VCL per configurare regole di blocco personalizzato da caricare nel servizio Fastly. È possibile configurare un inserisco nell&#39;elenco Bloccati di elenco Consentiti di o di in base al paese dall’amministratore di Adobe Commerce utilizzando [Blocco](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) funzionalità disponibile nella rete CDN Fastly per il modulo Magento 2.

Dopo aver definito l&#39;ACL di Edge, è possibile utilizzarlo per creare lo snippet VCL per bloccare l&#39;accesso agli indirizzi IP specificati nell&#39;ACL. Puoi utilizzare lo stesso frammento VCL sia negli ambienti di staging che in quelli di produzione, ma è necessario caricare il frammento in ogni ambiente separatamente.

Il seguente codice snippet VCL personalizzato (formato JSON) mostra la logica necessaria per bloccare le richieste in ingresso con un indirizzo IP client corrispondente a un indirizzo nel elenco Bloccati ACL di.

```json
{
  "name": "blocklist",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.ip ~ blocklist) { error 403 \"Forbidden\"; }"
}
```

Prima di creare uno snippet basato su questo esempio, esaminare i valori per determinare se è necessario apportare modifiche:

- `name`: nome del frammento VCL. Per questo esempio, abbiamo utilizzato il nome `blocklist`.

- `priority`: determina quando viene eseguito il frammento VCL. La priorità è `5` per eseguire immediatamente e verificare se una richiesta dell’amministratore proviene da un indirizzo IP consentito. Lo snippet viene eseguito prima di qualsiasi snippet VCL di Magento predefinito (`magentomodule_*`) ha assegnato una priorità di 50. Impostare la priorità per ogni frammento personalizzato su un valore maggiore o minore di 50 a seconda di quando si desidera eseguire il frammento. I frammenti con numeri di priorità inferiore vengono eseguiti per primi.

- `type`: specifica il tipo di frammento VCL che determina la posizione del frammento nel codice VCL generato. In questo esempio utilizziamo `recv`, che inserisce il codice VCL nel `vcl_recv` sottoroutine, sotto la piastra caldaia VCL e sopra qualsiasi oggetto. Consulta la [Riferimento frammento VCL Fastly](https://docs.fastly.com/api/config#api-section-snippet) per l&#39;elenco dei tipi di snippet.

- `content`: snippet di codice VCL da eseguire, che controlla l’indirizzo IP del client. Se l’IP si trova nell’ACL di Edge, l’accesso viene bloccato con un `403 Forbidden` per l&#39;intero sito Web. A tutti gli altri indirizzi IP client è consentito l&#39;accesso.

Dopo aver esaminato e aggiornato il codice per l’ambiente, utilizza uno dei metodi seguenti per aggiungere lo snippet VCL personalizzato alla configurazione del servizio Fastly:

- [Aggiungi lo snippet VCL personalizzato dall’amministratore](#add-the-custom-vcl-snippet). Questo metodo è consigliato se puoi accedere all’Admin. (Richiede [Versione definitiva 1.2.58](fastly-configuration.md#upgrade-fastly-module) o versione successiva).

- Salva l’esempio di codice JSON in un file (ad esempio, `blocklist.json`) e [caricala utilizzando l’API Fastly](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api). Utilizza questo metodo se non riesci ad accedere all’Admin.

## Aggiungere lo snippet VCL personalizzato

{{admin-login-step}}

1. Clic **Negozi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandi **Cache a pagina intera** > **Configurazione rapida** > **Snippet VCL personalizzati**.

1. Clic **Crea snippet personalizzato**.

1. Aggiungi i valori dello snippet VCL:

   - **Nome** — `blocklist`

   - **Tipo** — `recv`

   - **Priorità** — `5`

   - Aggiungi il **VCL** contenuto frammento:

     ```conf
     if ( client.ip ~ blocklist) { error 403 "Forbidden"; }
     ```

1. Clic **Crea** per generare il file di snippet VCL con il modello di nome `type_priority_name.vcl`, ad esempio `recv_5_blocklist.vcl`

1. Dopo il ricaricamento della pagina, fai clic su **Carica VCL in Fastly** nel *Configurazione rapida* per aggiungere il file alla configurazione del servizio Fastly.

1. Dopo i caricamenti, aggiorna la cache in base alla notifica nella parte superiore della pagina.

Convalida infine la versione aggiornata del codice VCL durante il processo di caricamento. Se la convalida non riesce, modifica lo snippet VCL personalizzato per risolvere il problema. Quindi, carica nuovamente il file VCL.

## Ulteriori esempi VCL per richieste di blocco

Gli esempi seguenti mostrano come bloccare le richieste utilizzando istruzioni di condizione in linea anziché un elenco ACL.

>[!WARNING]
>
>In questi esempi, il codice VCL è formattato come payload JSON che può essere salvato in un file e inviato in una richiesta API Fastly. È possibile inviare il [Frammento VCL dall’amministratore](#add-the-custom-vcl-snippet)o come stringa JSON utilizzando l’API Fastly. Per evitare la convalida quando utilizzi l’API Fastly con una stringa JSON, devi utilizzare una barra rovesciata per eliminare i caratteri speciali.

Consulta [Utilizzo di snippet VCL dinamici](https://docs.fastly.com/vcl/vcl-snippets/) nella documentazione di Fastly VCL.

### Esempio di codice VCL: Blocco per codice paese

In questo esempio viene utilizzato il codice paese ISO 3166-1 a due caratteri per il paese associato all’indirizzo IP.

```json
{
  "name": "blockbycountrycode",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( client.geo.country_code == \"HK\" ) { error 405 \"Not allowed\";}"
}
```

>[!NOTE]
>
>Anziché utilizzare uno snippet VCL personalizzato, è possibile utilizzare l&#39;opzione Fastly [Blocco](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/BLOCKING.md) funzionalità di amministrazione dell’infrastruttura cloud di Adobe Commerce per configurare il blocco in base al codice del paese o a un elenco di codici del paese.

### Esempio di codice VCL: Blocca per intestazione di richiesta HTTP dell’agente utente

```json
{
  "name": "blockbyuseragent",
  "dynamic": "0",
  "type": "recv",
  "priority": "5",
  "content": "if ( req.http.User-Agent ~ \"(UCBrowser|MQQBrowser|LieBaoFast|Mb2345Browser)\" ) {error 405 \"Not allowed\";}"
}
```

{{automate-vcl-snippet-deployment}}

{{$include /help/_includes/vcl-snippet-modify.md}}

{{$include /help/_includes/vcl-snippet-delete.md}}
