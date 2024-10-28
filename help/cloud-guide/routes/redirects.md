---
title: Reindirizzamenti
description: Scopri come gestire le regole di reindirizzamento per il progetto di infrastruttura cloud di Adobe Commerce.
feature: Cloud, Routes
exl-id: 7089a790-6341-4443-990a-df42091f0680
source-git-commit: 0f9a8cce6e0ed0f4dd6a902b6e4f7d3cfed9925d
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Reindirizzamenti

La gestione delle regole di reindirizzamento è un requisito comune per le applicazioni web, soprattutto nei casi in cui non si desidera perdere i collegamenti in entrata che sono stati modificati o rimossi nel tempo.

Di seguito viene illustrato come gestire le regole di reindirizzamento nei progetti Adobe Commerce su infrastrutture cloud utilizzando il file di configurazione `routes.yaml`. Se i metodi di reindirizzamento descritti in questo argomento non funzionano, puoi utilizzare le intestazioni di memorizzazione in cache per eseguire la stessa operazione.

{{route-placeholder}}

## Aggiornamenti agli ambienti Pro

{{pro-self-service-warning}}

>[!WARNING]
>
>Per i progetti Adobe Commerce su infrastrutture cloud, la configurazione di numerosi reindirizzamenti non regex e riscritture nel file `routes.yaml` può causare problemi di prestazioni. Se il file `routes.yaml` è di almeno 32 KB, scarica i reindirizzamenti non regex e riscrive in Fastly. Vedi [Offload dei reindirizzamenti non regex a Fastly invece di Nginx (route)](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html) nel _Centro assistenza Adobe Commerce_.

## Reindirizzamenti di intero percorso

Utilizzando i reindirizzamenti a route intera, è possibile definire route semplici utilizzando il file `routes.yaml`. Ad esempio, è possibile reindirizzare da un dominio apex a un sottodominio `www` come segue:

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## Reindirizzamenti a route parziale

Nel file `.magento/routes.yaml` è possibile aggiungere regole di reindirizzamento parziali alle route esistenti in base alla corrispondenza dei criteri:

```yaml
http://{default}/:
    redirects:
        expires: 1d
        paths:
          "/from": { to: "http://example.com/" }
          "/regexp/(.*)/matching": { to: "http://example.com/$1", regexp: true }
```

I reindirizzamenti parziali funzionano con qualsiasi tipo di percorso, compresi quelli serviti direttamente dall’applicazione.

Sono disponibili due chiavi in `redirects`:

- **expires** - Facoltativo, specifica il tempo necessario per memorizzare nella cache il reindirizzamento nel browser. Esempi di valori validi includono `3600s`, `1d`, `2w`, `3m`.

- **percorsi**: una o più coppie chiave-valore che specificano la configurazione per le regole di reindirizzamento parziale.

  Per ogni regola di reindirizzamento, la chiave è un’espressione per filtrare i percorsi di richiesta per il reindirizzamento. Il valore è un oggetto che specifica la destinazione di destinazione per il reindirizzamento e le opzioni per l’elaborazione del reindirizzamento.

  L&#39;oggetto value ha le seguenti proprietà:

  | Proprietà | Descrizione |
  | ---------- | ----------- |
  | `to` | Obbligatorio, percorso assoluto parziale, URL con protocollo e host o pattern che specifica la destinazione di destinazione per la regola di reindirizzamento. |
  | `regexp` | Facoltativo, il valore predefinito è `false`. Specifica se la chiave del percorso deve essere interpretata come espressione regolare PCRE. |
  | `prefix` | Specifica se il reindirizzamento si applica sia al percorso che a tutti i relativi elementi secondari o solo al percorso stesso. Impostazione predefinita: `true`. Valore non supportato se `regexp` è `true`. |
  | `append_suffix` | Determina se il suffisso viene riportato con il reindirizzamento. Impostazione predefinita: `true`. Questo valore non è supportato se la chiave `regexp` è `true` o* se la chiave `prefix` è `false`. |
  | `code` | Specifica il codice di stato HTTP. I codici di stato validi sono [`301` (spostati definitivamente)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2), [`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3), [`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8) e [`308`](https://www.rfc-editor.org/rfc/rfc7238). Impostazione predefinita: `302`. |
  | `expires` | Facoltativo, specifica per quanto tempo memorizzare in cache il reindirizzamento nel browser. Il valore predefinito è `expires`, definito direttamente nella chiave `redirects`, ma a questo livello è possibile ottimizzare la scadenza della cache per i singoli reindirizzamenti parziali. |

## Esempi di reindirizzamenti a route parziale

Negli esempi seguenti viene illustrato come specificare reindirizzamenti a route parziale nel file `routes.yaml` utilizzando diverse opzioni di configurazione di `paths`.

### Corrispondenza pattern espressione regolare

Utilizza il seguente formato per configurare le richieste di reindirizzamento in base a un’espressione regolare.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

Questa configurazione filtra i percorsi delle richieste rispetto a un&#39;espressione regolare e reindirizza le richieste corrispondenti a `https://example.com`. Ad esempio, una richiesta a `https://example.com/regexp/a/b/c/match` viene reindirizzata a `https://example.com/a/b/c`.

### Corrispondenza pattern prefisso

Utilizza il seguente formato per configurare le richieste di reindirizzamento per i percorsi che iniziano con un pattern di prefisso specificato.

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths:
        "/from": { to: "https://{default}/to", prefix: true }
```

Questa configurazione funziona come segue:

- Reindirizza le richieste che corrispondono al pattern `/from` al percorso `http://{default}/to`.

- Reindirizza le richieste che corrispondono al pattern `/from/another/path` a `https://{default}/to/another/path`.

- Se si modifica la proprietà `prefix` in `false`, le richieste che corrispondono al pattern `/from` attivano un reindirizzamento, ma le richieste che corrispondono al pattern `/from/another/path` no.

### Corrispondenza pattern suffisso

Utilizza il seguente formato per configurare le richieste di reindirizzamento che aggiungono il suffisso di percorso dalla richiesta alla destinazione di destinazione:

```yaml
http://{default}/:
    type: upstream
    redirects:
      paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

Questa configurazione funziona come segue:

- Reindirizza le richieste che corrispondono al pattern `/from/path/suffix` al percorso `https://{default}/to`.

- Se si modifica la proprietà `append_suffix` in `true`, le richieste corrispondenti a `/from/path/suffix` verranno reindirizzate al percorso `https://{default}/to/path/suffix`.

### Configurazione della cache specifica per percorso

Utilizza il seguente formato per personalizzare il tempo per memorizzare in cache un reindirizzamento da un percorso specifico:

```yaml
http://{default}/:
    type: upstream
    redirects:
    expires: 1d
      paths:
        "/from": { to: "https://example.com/" }
        "/here": { to: "https://example.com/there", expires: "2w" }
```

Questa configurazione funziona come segue:

- I reindirizzamenti dal primo percorso (`/from`) vengono memorizzati nella cache per un giorno.

- I reindirizzamenti dal secondo percorso (`/here`) vengono memorizzati nella cache per due settimane.
