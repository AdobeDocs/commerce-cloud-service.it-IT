---
title: Memorizzazione in cache
description: Scopri come abilitare la memorizzazione nella cache per il tuo Adobe Commerce negli ambienti dell’infrastruttura cloud.
feature: Cloud, Cache, Routes
exl-id: 4856aa94-2947-4dc8-b0d1-0960869dc39c
source-git-commit: 7b9c6a4cd17069c25455195bd8f273664b8a29eb
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# Memorizzazione in cache

Puoi abilitare il caching nell’ambiente di progetto dell’infrastruttura cloud. Se disattivi la memorizzazione in cache, Adobe Commerce distribuisce direttamente i file.

{{route-placeholder}}

## Configurare il caching

Abilitare il caching per l&#39;applicazione configurando le regole di cache nel file `.magento/routes.yaml` come segue:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
        headers: [ "Accept", "Accept-Language", "X-Language-Locale" ]
        cookies: ["*"]
        default_ttl: 60
```

## Memorizzazione in cache basata su route

Abilita il caching granulare impostando le regole di caching per diversi percorsi separatamente, come illustrato nell’esempio seguente:

```yaml
http://{default}/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true

http://{default}/path/:
    type: upstream
    upstream: php:php
    cache:
        enabled: false

http://{default}/path/more/:
    type: upstream
    upstream: php:php
    cache:
        enabled: true
```

L’esempio precedente memorizza in cache le seguenti route:

- `http://{default}/`
- `http://{default}/path/more/`
- `http://{default}/path/more/etc/`

E le seguenti route sono **non** memorizzate nella cache:

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>Le espressioni regolari nelle route sono **non** supportate.

## Durata cache

La durata della cache è determinata dal valore dell&#39;intestazione di risposta `Cache-Control`. Se nella risposta non è presente alcuna intestazione `Cache-Control`, viene utilizzata la chiave `default_ttl`.

## Chiave cache

Per decidere come memorizzare una risposta nella cache, Adobe Commerce crea una chiave della cache che dipende da diversi fattori e memorizza la risposta associata a tale chiave. Quando una richiesta viene fornita con la stessa chiave cache, la risposta viene riutilizzata. Il suo scopo è simile all&#39;intestazione HTTP [`Vary`](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44).

I parametri `headers` e le chiavi `cookies` consentono di modificare questa chiave della cache.

Il valore predefinito per queste chiavi è il seguente:

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## Attributi cache

### `enabled`

Se è impostato su `true`, abilitare la cache per questa route. Se è impostato su `false`, disabilitare la cache per questa route.

### `headers`

Definisce da quali valori deve dipendere la chiave della cache.

Ad esempio, se la chiave `headers` è:

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

Quindi Adobe Commerce memorizza nella cache una risposta diversa per ogni valore dell&#39;intestazione HTTP `Accept`.

### `cookies`

La chiave `cookies` definisce da quali valori deve dipendere la chiave della cache.

Ad esempio:

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

La chiave della cache dipende dal valore del cookie `value` nella richiesta.

Esiste un caso speciale se la chiave `cookies` ha il valore `["*"]`. Questo valore significa che qualsiasi richiesta con un cookie bypassa la cache. Questo è il valore predefinito.

>[!NOTE]
>
>Non è possibile utilizzare caratteri jolly nel nome del cookie. Utilizzare un nome di cookie preciso o far corrispondere tutti i cookie con un asterisco (`*`). `SESS*` o `~SESS` sono attualmente **non** valori validi.

I cookie hanno le seguenti restrizioni:

- Puoi impostare un massimo di **50 cookie** nel sistema. In caso contrario, l&#39;applicazione genera un&#39;eccezione `Unable to send the cookie. Maximum number of cookies would be exceeded`.
- La dimensione massima del cookie è **4096 byte**. In caso contrario, l&#39;applicazione genera un&#39;eccezione `Unable to send the cookie. Size of '%name' is %size bytes`.

### `default_ttl`

Se la risposta non ha un&#39;intestazione `Cache-Control`, viene utilizzata la chiave `default_ttl` per definire la durata della cache, in secondi. Il valore predefinito è `0`, il che significa che non è memorizzato nella cache.
