---
title: Proprietà web
description: Vedi esempi su come configurare la proprietà web nel file di configurazione dell'applicazione  [!DNL Commerce] .
feature: Cloud, Configuration
exl-id: 2ca94908-6fe1-42fd-bc3b-ef2dd473f1bb
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '410'
ht-degree: 0%

---

# Proprietà web

La proprietà `web` definisce il modo in cui l&#39;applicazione viene esposta al Web (in HTTP), determina il modo in cui l&#39;applicazione Web distribuisce il contenuto e controlla il modo in cui il contenitore dell&#39;applicazione risponde alle richieste in ingresso impostando regole in ogni posizione _block_. Un blocco rappresenta un percorso assoluto che porta con una barra (`/`).

```yaml
web:
    locations:
        "/":
            # The public directory of the app, relative to its root.
```

È possibile ottimizzare la configurazione di `locations` utilizzando i seguenti valori chiave per ogni blocco di `locations`:

| Attributo | Descrizione |
| :--- | :--- |
| `allow` | Distribuisci file che non corrispondono alle &quot;regole&quot;. Valore predefinito = `true` |
| `expires` | Imposta il numero di secondi per la memorizzazione del contenuto nella cache nel browser. Questa chiave abilita le intestazioni `cache-control` e `expires` per il contenuto statico. Se questo valore non è impostato, la direttiva `expires` e le intestazioni risultanti non vengono incluse quando si inviano file di contenuto statico. Un valore negativo 1 (`-1`) non comporta alcun caching ed è il valore predefinito. È possibile esprimere il valore del tempo con le unità seguenti: `ms` (millisecondi), `s` (secondi), `m` (minuti), `h` (ore), `d` (giorni), `w` (settimane), `M` (mesi, 30d) o `y` (anni, 365d) |
| `headers` | Impostare intestazioni personalizzate, ad esempio `X-Frame-Options`, per il contenuto statico distribuito da questa posizione. |
| `index` | Elencare i file statici da distribuire nell&#39;applicazione, ad esempio il file `index.html`. Per questa chiave è prevista una raccolta. Funziona solo se l&#39;accesso al file o ai file è &quot;consentito&quot; dalla chiave `allow` o `rules` per questo percorso. |
| `rules` | Specificare le sostituzioni per una posizione. Utilizza un’espressione regolare per corrispondere a una richiesta. Se una richiesta in ingresso corrisponde alla regola, la gestione regolare della richiesta viene ignorata dalle chiavi utilizzate nella regola. |
| `passthru` | Impostare l&#39;URL utilizzato nel caso in cui non sia possibile trovare un file statico o un file PHP. In genere, questo URL è il controller front per le applicazioni, ad esempio `/index.php` o `/app.php`. |
| `root` | Imposta il percorso relativo alla radice dell&#39;applicazione esposta sul Web. La directory pubblica (posizione &quot;/&quot;) di un progetto Cloud è impostata su &quot;pub&quot; per impostazione predefinita. |
| `scripts` | Consenti il caricamento di script in questa posizione. Impostare il valore su `true` per consentire gli script. |

La configurazione predefinita consente quanto segue:

- Dal percorso radice (`/`) è possibile accedere solo al Web e ai file multimediali
- Dai percorsi `~/pub/static` e `~/pub/media` è possibile accedere a qualsiasi file

Nell&#39;esempio seguente viene illustrata la configurazione predefinita nel file `.magento.app.yaml` per un set di percorsi accessibili dal Web associati a una voce nella proprietà [`mounts`](properties.md#mounts):

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
>Questo esempio mostra la configurazione web predefinita per un progetto Cloud configurato per supportare un singolo dominio. Per un progetto che richiede il supporto per più siti Web o archivi, è necessario impostare la configurazione `web` per il supporto di domini condivisi. Consulta [Configurare le posizioni per i domini condivisi](../store/multiple-sites.md#configure-locations-for-shared-domains).
