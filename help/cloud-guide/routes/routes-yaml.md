---
title: Configurare le route
description: Scopri come definire le route per le richieste HTTPS in arrivo per Adobe Commerce negli ambienti dell’infrastruttura cloud.
feature: Cloud, Configuration, Routes
exl-id: a33797e5-14cc-45eb-a048-96180b872a4a
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '886'
ht-degree: 0%

---

# Configurare le route

Il `routes.yaml` file in `.magento/routes.yaml` definisce le route per gli ambienti Adobe Commerce su integrazione di infrastruttura cloud, staging e produzione. Le route determinano il modo in cui l&#39;applicazione elabora le richieste HTTP e HTTPS in ingresso.

Il valore predefinito `routes.yaml` file specifica i modelli di route per l&#39;elaborazione delle richieste HTTP come HTTPS nei progetti con un singolo dominio predefinito e nei progetti configurati per più domini:

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"
"http://{all}/":
    type: upstream
    upstream: "mymagento:http"
```

Utilizza il `magento-cloud` CLI per visualizzare un elenco delle route configurate:

```bash
magento-cloud environment:routes

+-------------------+----------+---------------+
| Route             | Type     | To            |
+-------------------+----------+---------------+
| http://{default}/ | upstream | mymagento     |
+-------------------+----------+---------------+
```

## Aggiornamenti alla configurazione degli ambienti Pro

{{pro-self-service-warning}}

## Modelli di percorso

Il `routes.yaml` file è un elenco di route con modelli e relative configurazioni. Nei modelli di percorso è possibile utilizzare i segnaposto seguenti:

- Il `{default}` il segnaposto rappresenta il nome di dominio qualificato configurato come predefinito per il progetto.

  Ad esempio, un progetto con il dominio predefinito `example.com` e i seguenti modelli di percorso:

  ```text
  https://www.{default}/
  https://{default}/blog
  ```

  Questi modelli vengono risolti nei seguenti URL in un ambiente di produzione:

  ```text
  https://www.example.com/
  https://example.com/blog
  ```

- Il `{all}` il segnaposto rappresenta tutti i nomi di dominio configurati per il progetto.

  Un progetto con `example.com` e `example1.com` domini con i seguenti modelli di route:

  ```text
  https://www.{all}/
  
  https://{all}/blog
  ```

  Questi modelli consentono di risolvere i percorsi seguenti per tutti i domini del progetto:

  ```text
  https://www.example.com/
  
  https://www.example1.com/
  
  https://example.com/blog
  
  https://example1.com/blog
  ```

  Il `{all}` il segnaposto è utile per i progetti configurati per più domini. In un ramo non di produzione, `{all}` viene sostituito con l’ID del progetto e l’ID dell’ambiente per ciascun dominio.

  Se un progetto non ha configurato alcun dominio, il che è comune durante lo sviluppo, il `{all}` il segnaposto si comporta nello stesso modo del `{default}` segnaposto.

Adobe Commerce genera inoltre route per ogni ambiente di integrazione attivo. Per gli ambienti di integrazione, il `{default}` il segnaposto è sostituito dal seguente nome di dominio:

```text
[branch]-[per-environment-random-string]-[project-id].[region].magentosite.cloud
```

Ad esempio, il `refactorcss` ramo per `mswy7hzcuhcjw` progetto ospitato in `us` il cluster ha il seguente dominio:

```text
https://refactorcss-oy3m2pq-mswy7hzcuhcjw.us.magentosite.cloud/
```

>[!NOTE]
>
>Se il progetto Cloud supporta più store, segui le istruzioni di configurazione del percorso per [più siti web o store](../store/multiple-sites.md).

### Barra finale

Le definizioni di route contengono una barra finale per indicare una cartella o una directory; tuttavia, lo stesso contenuto può essere distribuito con o senza una barra finale. I seguenti URL hanno la stessa risoluzione ma possono essere interpretati come _due diversi_ URL:

```text
https://www.example.com/blog/

https://www.example.com/blog
```

>[!TIP]
>
>È consigliabile utilizzare una barra finale per le directory, ma qualunque sia il metodo scelto, è importante **rimani coerente** per evitare di generare due posizioni.

## Protocolli di route

Tutti gli ambienti supportano sia HTTP che HTTPS automaticamente.

- Se la configurazione specifica solo la route HTTP, le route HTTPS vengono create automaticamente, consentendo di gestire il sito sia da HTTP che da HTTPS senza richiedere reindirizzamenti.

  Ad esempio, un progetto con il dominio predefinito `example.com` e il seguente modello di percorso:

  ```text
  http://{default}/
  ```

  Questo modello viene risolto nei seguenti URL:

  ```text
  http://example.com/
  
  https://example.com/
  ```

- Se la configurazione specifica solo la route HTTPS, tutte le richieste HTTP vengono reindirizzate a HTTPS.

  Ad esempio, un progetto con il dominio predefinito `example.com` con il seguente modello di percorso:

  ```text
  https://{default}/
  ```

  Questo modello viene risolto nel seguente URL:

  ```text
  https://example.com/
  ```

  Inoltre, elabora il seguente reindirizzamento:

  `http://example.com/` ==> `https://example.com/`

Distribuisci tutte le pagine su TLS. Per questa configurazione, devi configurare i reindirizzamenti per tutte le richieste non crittografate all’equivalente TLS utilizzando uno dei seguenti metodi:

- Modificare il protocollo in HTTPS nel `routes.yaml` file.

  ```yaml
  "https://{default}/":
      type: upstream
      upstream: "mymagento:http"
  "https://{all}/":
      type: upstream
      upstream: "mymagento:http"
  ```

- Per gli ambienti di staging e produzione, abilita [Forza TLS su Fastly](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/redirect-http-to-https-for-all-pages-on-cloud-force-tls.html) dall’interfaccia utente di amministrazione. Quando si utilizza questa opzione, Fastly gestisce il reindirizzamento a HTTPS, quindi non è necessario aggiornare il `routes.yaml` configurazione.

## Opzioni ciclo di lavorazione

Configurare ogni route separatamente utilizzando le proprietà seguenti:

| Proprietà | Descrizione |
| ---------------- | ----------- |
| `type: upstream` | Distribuisce un&#39;applicazione. Inoltre, presenta un `upstream` che specifica il nome dell&#39;applicazione (come definito in `.magento.app.yaml`) seguito da `:http` endpoint. |
| `type: redirect` | Reindirizza a un&#39;altra route. È seguito da `to` , che è un reindirizzamento HTTP a un altro percorso identificato dal relativo modello. |
| `cache:` | Controlli [caching per la route](caching.md). |
| `redirects:` | Controlli [regole di reindirizzamento](redirects.md). |
| `ssi:` | Controlla l’abilitazione di [Server Side Include](server-side-includes.md). |

## Percorsi semplici

Negli esempi seguenti, la configurazione della route indirizza il dominio apex e `www` sottodominio del `mymagento` applicazione. Questa route non reindirizza le richieste HTTPS.

**Esempio 1:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: redirect
    to: "http://{default}/"
```

In questo esempio, il ciclo di richieste segue le regole seguenti:

- Il server risponde direttamente alle richieste con il seguente pattern URL:

  ```text
  http://example.com/path
  ```

- Il server invia una _Reindirizzamento 301_ per le richieste con il seguente pattern URL:

  ```text
  http://www.example.com/mypath
  ```

  Queste richieste vengono reindirizzate al dominio apex, ad esempio:

  ```text
  http://example.com/mypath
  ```

Nell’esempio seguente, la configurazione della route non reindirizza gli URL dal dominio www al dominio apex. Le richieste vengono invece gestite sia dal dominio www che da quello apex.

**Esempio 2:**

```yaml
"http://{default}/":
    type: upstream
    upstream: "mymagento:http"

"http://www.{default}/":
    type: upstream
    upstream: "mymagento:http"
```

## Route con caratteri jolly

Adobe Commerce su infrastruttura cloud supporta percorsi con caratteri jolly, in modo da poter mappare più sottodomini alla stessa applicazione. Questo funziona per le route di reindirizzamento e upstream. La route deve essere preceduta da un asterisco (\*). Ad esempio, le seguenti route alla stessa applicazione:

- `*.example.com`
- `www.example.com`
- `blog.example.com`
- `us.example.com`

Questo funziona come dominio onnicomprensivo in un ambiente live.

### Indirizzare un dominio non mappato

Puoi indirizzare a un sistema che non è mappato a un dominio utilizzando un punto (`.`) per separare il sottodominio.

**Esempio:**

Un progetto con un `add-theme` il ramo viene indirizzato al seguente URL:

```text
http://add-theme-projectID.us.magento.com/
```

Se si definisce il seguente modello di ciclo di lavorazione:

```text
http://www.{default}/
```

La route viene risolta nel seguente URL:

```text
http://www.add-theme-projectID.us.magento.com/
```

Puoi inserire qualsiasi sottodominio prima che il punto e la route si risolvano.

**Esempio:**

Definire il seguente modello di ciclo di lavorazione:

```text
http://*.{default}/
```

Questo modello risolve entrambi i seguenti URL:

```text
http://foo.add-theme-projectID.us.magentosite.cloud/
http://bar.add-theme-projectID.us.magentosite.cloud/
```

È possibile visualizzare il pattern di instradamento per i domini non mappati stabilendo una connessione SSH all’ambiente e utilizzando `magento-cloud` CLI per elencare le route:

```terminal
web@mymagento.0:~$ vendor/bin/ece-tools env:config:show routes

Magento Cloud Routes:
+------------------------------------------+--------------------------------------------------------------+
| Route configuration                      | Value                                                        |
+------------------------------------------+--------------------------------------------------------------+
| http://www.add-theme-projectID.us.magento.com/:                                                         |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https:/{default}/                                            |
+------------------------------------------+--------------------------------------------------------------+
| https://*.add-theme-projectID.us.magentosite.cloud/:                                                    |
+------------------------------------------+--------------------------------------------------------------+
| primary                                  | false                                                        |
| id                                       | null                                                         |
| attributes                               |                                                              |
| type                                     | upstream                                                     |
| to                                       | mymagento                                                    |
| original_url                             | https://*.{default}/                                         |
+------------------------------------------+--------------------------------------------------------------+
```

## Reindirizzamenti e caching

Come discusso più dettagliatamente in [Reindirizzamenti](redirects.md), puoi gestire regole di reindirizzamento complesse, ad esempio _reindirizzamenti parziali_ e specificare le regole per il ciclo di lavorazione [caching](caching.md):

```yaml
https://www.{default}/:
    type: redirect
    to: https://{default}/
https://{default}/:
    cache:
        cookies: [""]
        default_ttl: 0
        enabled: true
        headers:
            - Accept
            - Accept-Language
    ssi:
        enabled: false
    type: upstream
    upstream: mymagento:http
```
