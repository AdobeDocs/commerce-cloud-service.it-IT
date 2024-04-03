---
title: Reindirizzamenti
description: Scopri come gestire le regole di reindirizzamento per il progetto di infrastruttura cloud di Adobe Commerce.
feature: Cloud, Routes
exl-id: 7089a790-6341-4443-990a-df42091f0680
source-git-commit: 649c11b111aa9c9105e54908bf9c6f48741f10e4
workflow-type: tm+mt
source-wordcount: '646'
ht-degree: 0%

---

# Reindirizzamenti

La gestione delle regole di reindirizzamento è un requisito comune per le applicazioni web, soprattutto nei casi in cui non si desidera perdere i collegamenti in entrata che sono stati modificati o rimossi nel tempo.

Di seguito viene illustrato come gestire le regole di reindirizzamento nei progetti di infrastruttura cloud di Adobe Commerce tramite `routes.yaml` file di configurazione. Se i metodi di reindirizzamento descritti in questo argomento non funzionano, puoi utilizzare le intestazioni di memorizzazione in cache per eseguire la stessa operazione.

{{route-placeholder}}

## Aggiornamenti agli ambienti Pro

{{pro-self-service-warning}}

>[!WARNING]
>
>Per i progetti Adobe Commerce su infrastrutture cloud, configurare numerosi reindirizzamenti non regex e riscritture in `routes.yaml` può causare problemi di prestazioni. Se il `routes.yaml` è pari o superiore a 32 KB, scarica i reindirizzamenti non regex e riscrive in Fastly. Consulta [Offload di reindirizzamenti non regex a Fastly anziché a Nginx (route)](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/offload-non-regex-redirects-to-fastly-instead-of-nginx-routes.html) nel _Centro assistenza Adobe Commerce_.

## Reindirizzamenti di intero percorso

Utilizzando i reindirizzamenti di route complete, puoi definire route semplici utilizzando `routes.yaml` file. Ad esempio, puoi reindirizzare da un dominio apex a un `www` sottodominio come segue:

```yaml
http://{default}/:
    type: redirect
    to: http://www.{default}/
```

## Reindirizzamenti a route parziale

In `.magento/routes.yaml` file, è possibile aggiungere regole di reindirizzamento parziali ai percorsi esistenti in base alla corrispondenza dei pattern:

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

- **scade**- Facoltativo, specifica per quanto tempo memorizzare in cache il reindirizzamento nel browser. Esempi di valori validi includono `3600s`, `1d`, `2w`, `3m`.

- **percorsi**- Una o più coppie chiave-valore che specificano la configurazione per le regole di reindirizzamento parziale.

  Per ogni regola di reindirizzamento, la chiave è un’espressione per filtrare i percorsi di richiesta per il reindirizzamento. Il valore è un oggetto che specifica la destinazione di destinazione per il reindirizzamento e le opzioni per l’elaborazione del reindirizzamento.

  L&#39;oggetto value ha le seguenti proprietà:

  | Proprietà | Descrizione |
  | ---------- | ----------- |
  | `to` | Obbligatorio, percorso assoluto parziale, URL con protocollo e host o pattern che specifica la destinazione di destinazione per la regola di reindirizzamento. |
  | `regexp` | Facoltativo, impostazione predefinita `false`. Specifica se la chiave del percorso deve essere interpretata come espressione regolare PCRE. |
  | `prefix` | Specifica se il reindirizzamento si applica sia al percorso che a tutti i relativi elementi secondari o solo al percorso stesso. Impostazione predefinita `true`. Valore non supportato se `regexp` è `true`. |
  | `append_suffix` | Determina se il suffisso viene riportato con il reindirizzamento. Impostazione predefinita `true`. Valore non supportato se `regexp` la chiave è `true` o* se `prefix` la chiave è `false`. |
  | `code` | Specifica il codice di stato HTTP. I codici di stato validi sono [`301` (Spostato in modo permanente)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2), [`302`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.3), [`307`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.8), e [`308`](https://www.rfc-editor.org/rfc/rfc7238). Impostazione predefinita `302`. |
  | `expires` | Facoltativo, specifica per quanto tempo memorizzare in cache il reindirizzamento nel browser. Impostazione predefinita `expires` valore definito direttamente sotto `redirects` , ma a questo livello puoi regolare con precisione la scadenza della cache per i singoli reindirizzamenti parziali. |

## Esempi di reindirizzamenti a route parziale

Gli esempi seguenti mostrano come specificare i reindirizzamenti a route parziale nel `routes.yaml` file utilizzando vari `paths` opzioni di configurazione.

### Corrispondenza pattern espressione regolare

Utilizza il seguente formato per configurare le richieste di reindirizzamento in base a un’espressione regolare.

```yaml
http://{default}/:
    type: upstream
    redirects:
    paths:
        "/regexp/(.*)/match": { to: "http://example.com/$1", regexp: true }
```

Questa configurazione filtra i percorsi delle richieste rispetto a un’espressione regolare e reindirizza le richieste corrispondenti a `https://example.com`. Ad esempio, una richiesta a `https://example.com/regexp/a/b/c/match` reindirizza a `https://example.com/a/b/c`.

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

- Reindirizza le richieste che corrispondono al modello `/from` al percorso `http://{default}/to`.

- Reindirizza le richieste che corrispondono al modello `/from/another/path` a `https://{default}/to/another/path`.

- Se si modifica il `prefix` proprietà a `false`, richieste che corrispondono a `/from` pattern attiva un reindirizzamento, ma le richieste che corrispondono a `/from/another/path` pattern non lo fanno.

### Corrispondenza pattern suffisso

Utilizza il seguente formato per configurare le richieste di reindirizzamento che aggiungono il suffisso di percorso dalla richiesta alla destinazione di destinazione:

```yaml
http://{default}/:
    type: upstream
    redirects:
    paths: "/from": { to: "https://{default}/to", append_suffix: false }
```

Questa configurazione funziona come segue:

- Reindirizza le richieste che corrispondono al modello `/from/path/suffix` al percorso `https://{default}/to`.

- Se si modifica il `append_suffix` proprietà a `true`, quindi richieste corrispondenti `/from/path/suffix`  reindirizzare al percorso `https://{default}/to/path/suffix`.

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

- Reindirizza dal primo percorso (`/from`) vengono memorizzati nella cache per un giorno.

- Reindirizza dal secondo percorso (`/here`) sono memorizzati nella cache per due settimane.
