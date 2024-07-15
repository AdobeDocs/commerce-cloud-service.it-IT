---
title: Server-side include
description: Scopri come utilizzare le inclusioni lato server con Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Routes
exl-id: 34a38cb5-5f0e-49ac-9dba-bb581a06aeed
source-git-commit: 649c11b111aa9c9105e54908bf9c6f48741f10e4
workflow-type: tm+mt
source-wordcount: '172'
ht-degree: 0%

---

# Server-side include

[Inclusioni lato server](https://nginx.org/en/docs/http/ngx_http_ssi_module.html) (SSI) sono direttive nelle pagine HTML valutate sul server durante il rendering delle pagine. SSI consente di aggiungere contenuto generato in modo dinamico a una pagina HTML esistente senza distribuire l’intera pagina.

È possibile attivare o disattivare SSI in base al percorso in `.magento/routes.yaml`, ad esempio:

```yaml
    "http://{default}/":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: false
            ssi:
                enabled: true
    "http://{default}/time.php":
        type: upstream
        upstream: "myapp:php"
        cache:
            enabled: true
```

SSI ti consente di includere nelle direttive di risposta HTML che fanno in modo che il server compili parti del HTML, rispettando qualsiasi [configurazione di caching](caching.md) esistente.

Nell&#39;esempio seguente viene illustrato come inserire un controllo data dinamico nella parte superiore di una pagina e un altro controllo data nella parte inferiore che viene aggiornato ogni 600 secondi:

Aggiungi quanto segue a qualsiasi pagina, ad esempio `/index.php`:

```php?start_inline=1
echo date(DATE_RFC2822);
<!--#include virtual="time.php" -->
```

Aggiungi quanto segue a `time.php`:

```php?start_inline=1
header("Cache-Control: max-age=600");
echo date(DATE_RFC2822);
```

Passare alla pagina in cui è stato aggiunto il controllo. Aggiorna la pagina più volte e osserva che l’ora nella parte superiore della pagina cambia, ma quella nella parte inferiore cambia solo ogni 600 secondi.
