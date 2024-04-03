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

Abilita il caching per l’applicazione configurando le regole di cache in `.magento/routes.yaml` file come segue:

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

E i seguenti percorsi sono **non** memorizzato nella cache:

- `http://{default}/path/`
- `http://{default}/path/etc/`

>[!NOTE]
>
>Le espressioni regolari nelle route sono **non** supportati.

## Durata cache

La durata della cache è determinata da `Cache-Control` valore intestazione di risposta. In caso negativo `Cache-Control` l&#39;intestazione è nella risposta, il `default_ttl` viene utilizzata la chiave.

## Chiave cache

Per decidere come memorizzare una risposta nella cache, Adobe Commerce crea una chiave della cache che dipende da diversi fattori e memorizza la risposta associata a tale chiave. Quando una richiesta viene fornita con la stessa chiave cache, la risposta viene riutilizzata. Il suo scopo è simile a quello di HTTP [`Vary` intestazione](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.44).

Parametri `headers` e `cookies` Le chiavi consentono di modificare questa chiave della cache.

Il valore predefinito per queste chiavi è il seguente:

```yaml
cache:
    enabled: true
    headers: ["Accept-Language", "Accept"]
    cookies: ["*"]
```

## Attributi cache

### `enabled`

Se impostato su `true`, abilita la cache per questa route. Se impostato su `false`, disabilita la cache per questa route.

### `headers`

Definisce da quali valori deve dipendere la chiave della cache.

Ad esempio, se `headers` La chiave è la seguente:

```yaml
cache:
    enabled: true
    headers: ["Accept"]
```

Quindi Adobe Commerce memorizza nella cache una risposta diversa per ogni valore del `Accept` intestazione HTTP.

### `cookies`

Il `cookies` La chiave definisce da quali valori deve dipendere la chiave della cache.

Ad esempio:

```yaml
cache:
    enabled: true
    cookies: ["value"]
```

La chiave della cache dipende dal valore della proprietà `value` cookie nella richiesta.

Esiste un caso speciale se il `cookies` la chiave ha `["*"]` valore. Questo valore significa che qualsiasi richiesta con un cookie bypassa la cache. Questo è il valore predefinito.

>[!NOTE]
>
>Non è possibile utilizzare caratteri jolly nel nome del cookie. Utilizza un nome cookie preciso o abbina tutti i cookie con un asterisco (`*`). Ad esempio: `SESS*` o `~SESS` sono attualmente **non** valori validi.

I cookie hanno le seguenti restrizioni:

- Puoi impostare un massimo di **50 cookie** nel sistema. In caso contrario, l’applicazione genera un’ `Unable to send the cookie. Maximum number of cookies would be exceeded` eccezione.
- La dimensione massima del cookie è **4096 byte**. In caso contrario, l’applicazione genera un’ `Unable to send the cookie. Size of '%name' is %size bytes` eccezione.

### `default_ttl`

Se la risposta non dispone di un `Cache-Control` intestazione, il `default_ttl` La chiave viene utilizzata per definire la durata della cache, in secondi. Il valore predefinito è `0`, il che significa che non viene memorizzato nella cache.
