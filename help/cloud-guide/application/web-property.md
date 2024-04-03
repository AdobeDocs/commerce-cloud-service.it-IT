---
title: Proprietà web
description: Vedi esempi su come configurare la proprietà web in [!DNL Commerce] file di configurazione dell'applicazione.
feature: Cloud, Configuration
exl-id: 2ca94908-6fe1-42fd-bc3b-ef2dd473f1bb
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Proprietà web

Il `web` definisce il modo in cui l’applicazione viene esposta al web (in HTTP), determina il modo in cui l’applicazione web distribuisce il contenuto e controlla il modo in cui il contenitore dell’applicazione risponde alle richieste in ingresso impostando regole in ogni posizione _blocco_. Un blocco rappresenta un percorso assoluto che porta con una barra (`/`).

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

È possibile ottimizzare il `locations` utilizzando i seguenti valori chiave per ciascuna `locations` blocco:

| Attributo | Descrizione |
| :--- | :--- |
| `allow` | Distribuisci file che non corrispondono alle &quot;regole&quot;. Valore predefinito = `true` |
| `expires` | Imposta il numero di secondi per la memorizzazione del contenuto nella cache nel browser. Questa chiave abilita `cache-control` e `expires` intestazioni per il contenuto statico. Se questo valore non è impostato, il `expires` La direttiva e le intestazioni risultanti non sono incluse quando si inviano file di contenuto statico. A negativo 1 (`-1`) non viene memorizzato in cache ed è il valore predefinito. Puoi esprimere il valore temporale con le seguenti unità:  `ms` (millisecondi) `s` (secondi), `m` (minuti), `h` (ore), `d` (giorni), `w` (settimane), `M` (mesi, 30 quinquies), oppure `y` (anni, 365d) |
| `headers` | Impostare intestazioni personalizzate, ad esempio `X-Frame-Options`, per il contenuto statico fornito da questa posizione. |
| `index` | Elencare i file statici da distribuire nell&#39;applicazione, ad esempio `index.html` file. Per questa chiave è prevista una raccolta. Questo funziona solo se l’accesso al file o ai file è &quot;consentito&quot; dal `allow` o `rules` chiave per questa posizione. |
| `rules` | Specificare le sostituzioni per una posizione. Utilizza un’espressione regolare per corrispondere a una richiesta. Se una richiesta in ingresso corrisponde alla regola, la gestione regolare della richiesta viene ignorata dalle chiavi utilizzate nella regola. |
| `passthru` | Impostare l&#39;URL utilizzato nel caso in cui non sia possibile trovare un file statico o un file PHP. In genere, questo URL è il controller front per le applicazioni, ad esempio `/index.php` o `/app.php`. |
| `root` | Imposta il percorso relativo alla radice dell&#39;applicazione esposta sul Web. La directory pubblica (posizione &quot;/&quot;) di un progetto Cloud è impostata su &quot;pub&quot; per impostazione predefinita. |
| `scripts` | Consenti il caricamento di script in questa posizione. Imposta il valore su `true` per consentire gli script. |

La configurazione predefinita consente quanto segue:

- Dalla directory principale (`/`), è possibile accedere solo al web e ai file multimediali
- Dalla sezione `~/pub/static` e `~/pub/media` percorsi, è possibile accedere a qualsiasi file

L&#39;esempio seguente mostra la configurazione predefinita in `.magento.app.yaml` per un set di posizioni accessibili sul web associate a una voce nella  [`mounts` proprietà](properties.md#mounts):

```yaml
 # The configuration of app when it is exposed to the web.
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
            root: "pub"
            # The front-controller script to send non-static requests to.
            passthru: "/index.php"
            index:
                - index.php
            expires: -1
            scripts: true
            allow: false
            rules:
                \.(css|js|map|hbs|gif|jpe?g|png|tiff|wbmp|ico|jng|bmp|svgz|midi?|mp?ga|mp2|mp3|m4a|ra|weba|3gpp?|mp4|mpe?g|mpe|ogv|mov|webm|flv|mng|asx|asf|wmv|avi|ogx|swf|jar|ttf|eot|woff|otf|html?)$:
                    allow: true
                ^/sitemap(.*)\.xml$:
                    passthru: "/media/sitemap$1.xml"
        "/media":
            root: "pub/media"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/get.php"
        "/static":
            root: "pub/static"
            allow: true
            scripts: false
            expires: 1y
            passthru: "/front-static.php"
            rules:
                ^/static/version\d+/(?<resource>.*)$:
                    passthru: "/static/$resource"
```

>[!NOTE]
>
>Questo esempio mostra la configurazione web predefinita per un progetto Cloud configurato per supportare un singolo dominio. Per un progetto che richiede il supporto per più siti Web o store, il `web` la configurazione deve essere impostata per supportare i domini condivisi. Consulta [Configurare le posizioni per i domini condivisi](../store/multiple-sites.md#configure-locations-for-shared-domains).
