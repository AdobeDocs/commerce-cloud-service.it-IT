---
title: Risoluzione rapida dei problemi
description: Scopri come risolvere e gestire i problemi relativi al modulo e ai servizi Fastly CDN per Adobe Commerce.
feature: Cloud, Configuration, Cache, Services
exl-id: e4c47035-cbad-4838-8d44-fa5eaaac42d1
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '1834'
ht-degree: 0%

---

# Risoluzione rapida dei problemi

Utilizza le seguenti informazioni per risolvere i problemi e gestire il modulo Fastly CDN per il Magento 2 nell’ambiente di progetto Adobe Commerce on Cloud Infrastructure. Ad esempio, puoi analizzare i valori delle intestazioni di risposta e il comportamento di caching per risolvere i problemi di servizio e prestazioni Fastly.

Negli ambienti di produzione e staging di Pro è possibile utilizzare [registri New Relic](../monitor/log-management.md) per visualizzare e analizzare i dati di registro Fastly CDN e WAF per la risoluzione di errori e problemi di prestazioni.

>[!NOTE]
>
>Per informazioni sulla configurazione di Fastly, vedere [Configurazione Fastly](fastly.md).

## Individua ID servizio Fastly

È necessario l’ID servizio Fastly per configurare Fastly dall’amministratore o per inviare richieste API Fastly per la configurazione e la risoluzione dei problemi Fastly avanzati.

Se Fastly è abilitato nell’ambiente del progetto, puoi ottenere l’ID servizio dall’Amministratore. Vedi [Ottieni credenziali rapide](fastly-configuration.md#get-fastly-credentials).

Gli sviluppatori e gli utenti VCL avanzati possono utilizzare VCL personalizzato per recuperare l&#39;ID servizio utilizzando la variabile Fastly `req.service_id`. Ad esempio, puoi aggiungere `req.service_id` alla direttiva di registrazione personalizzata nel file VCL per acquisire il valore dell&#39;ID servizio:

```json
log {"syslog"} req.service_id {" my_logging_endpoint_name :: "}
```

È possibile utilizzare lo stesso VCL per gli ambienti di produzione e staging. Vedi [Come configurare vcl_log](https://support.fastly.com/hc/en-us/community/posts/360040447172-How-to-configure-vcl-log).

## Problemi relativi a prestazioni del sito, eliminazione e cache

Utilizza il seguente elenco per identificare e risolvere i problemi relativi alla configurazione del servizio Fastly per l’ambiente Adobe Commerce sull’infrastruttura cloud.

- **Il menu Store non viene visualizzato o non funziona**. È possibile che si stia utilizzando un collegamento o un collegamento temporaneo direttamente al server di origine anziché l&#39;URL del sito attivo oppure che si sia utilizzato `-H "host:URL"` in un comando [cURL](#check-live-site-through-fastly). Se salti Fastly al server di origine, il menu principale non funziona e vengono visualizzate intestazioni non corrette che consentono il caching sul lato browser.

- **La navigazione superiore non funziona**. La navigazione superiore si basa sull&#39;elaborazione ESI (Edge Side Includes) abilitata quando si caricano i frammenti VCL Fastly del Magento predefinito. Se la navigazione non funziona, [carica Fastly VCL](fastly-configuration.md#upload-vcl-to-fastly) e ricontrolla il sito.

- **Geolocalizzazione/GeoIP non funziona**. I frammenti VCL Fastly Magento predefiniti aggiungono il codice del paese all&#39;URL. Se il codice del paese non funziona, [carica il file VCL](fastly-configuration.md#upload-vcl-to-fastly) Fastly e controlla nuovamente il sito.

- **Le pagine non sono memorizzate in cache**. Per impostazione predefinita, Fastly non memorizza in cache le pagine con l&#39;intestazione `Set-Cookies`. Adobe Commerce imposta i cookie anche su pagine memorizzabili in cache (TTL > 0). Il Magento predefinito Fastly VCL elimina tali cookie dalle pagine memorizzabili in cache. Se le pagine non vengono memorizzate in cache, [carica Fastly VCL](fastly-configuration.md#upload-vcl-to-fastly) e ricontrolla il sito.

  Questo problema può verificarsi anche se un blocco di pagina in un modello è contrassegnato come non memorizzabile in cache. In tal caso, il problema è probabilmente causato da un modulo o da un’estensione di terze parti che blocca o rimuove le intestazioni di Adobe Commerce. Per risolvere il problema, vedere [X-Cache contiene solo messaggi non recapitati, nessun HIT](#x-cache-contains-only-miss-no-hit).

- **Le richieste di eliminazione non sono riuscite**. Fastly restituisce il seguente errore quando si invia una richiesta di eliminazione:

  ```text
  The purge request was not processed successfully.
  ```

  Questo problema può essere causato da uno dei seguenti problemi:

   - Credenziali Fastly non valide nella configurazione del servizio Fastly per l’ambiente di progetto Adobe Commerce su infrastruttura cloud
   - Codice non valido in uno snippet VCL personalizzato

  Per risolvere il problema, vedere [Errore durante l&#39;eliminazione della cache Fastly su Cloud](https://support.magento.com/hc/en-us/articles/115001853194-Error-purging-Fastly-cache-on-Cloud-The-purge-request-was-not-processed-successfully-) nel Centro assistenza di Adobe Commerce.

## 503 errori di Fastly

Se Fastly restituisce errori di timeout 503, controllare i registri di errore e la pagina di errore 503 per identificare la causa principale.

>[!NOTE]
>
>Se il timeout si verifica durante l&#39;esecuzione di operazioni in blocco, puoi [estendere il timeout Fastly per l&#39;amministratore](fastly-custom-cache-configuration.md#extend-fastly-timeout).

Se ricevi un errore 503, controlla il registro degli errori dell’ambiente di produzione o di staging e il registro di accesso php per risolvere il problema.

**Per controllare i registri errori**:

- [Registro errori](../test/log-locations.md#application-logs)

  ```text
  /var/log/platform/<project-ID>/error.log
  ```

  Questo registro include tutti gli errori dell&#39;applicazione o del motore PHP, ad esempio `memory_limit` o `max_execution_time exceeded` errori. Se non trovate alcun errore correlato Fastly, controllate il registro degli accessi PHP.

- Registro degli accessi PHP

  ```text
  /var/log/platform/<project-ID>/php.access.log
  ```

  Cerca nel registro le risposte HTTP 200 per l’URL che ha restituito l’errore 503. Se trovi la risposta 200, significa che Adobe Commerce ha restituito la pagina senza errori. Ciò indica che il problema potrebbe essersi verificato dopo l&#39;intervallo che supera il valore `first_byte_timeout` impostato nella configurazione del servizio Fastly.

Quando si verifica un errore 503, Fastly restituisce il motivo nella pagina di errore e manutenzione. Potresti non essere in grado di visualizzare il motivo se hai aggiunto il codice per una [pagina di risposta personalizzata](fastly-custom-response.md). Per visualizzare il codice motivo nella pagina di errore predefinita, è possibile rimuovere il codice HTML per la pagina di errore personalizzata.

**Per controllare la pagina di errore Fastly 503**:

{{admin-login-step}}

1. Fai clic su **Archivi** > **Impostazioni** > **Configurazione** > **Avanzate** > **Sistema**.

1. Nel riquadro di destra espandere **Cache a pagina intera**.

1. Nella sezione **Fastly Configuration**, espandere **Pagine sintetiche personalizzate** come illustrato nella figura seguente.

   ![Pagina di errore 503 personalizzata](../../assets/cdn/fastly-custom-synthetic-pages-edit-html.png)

1. Fare clic su **Imposta HTML**.

1. Rimuovi il codice personalizzato. È possibile salvarlo in un programma di testo per aggiungerlo nuovamente in un secondo momento.

1. Fai clic su **Carica** per inviare gli aggiornamenti a Fastly.

1. Fai clic su **Salva configurazione** nella parte superiore della pagina.

1. Riapri l’URL che ha causato l’errore 503. In Fastly restituisce una pagina di errore con il motivo, come illustrato nell&#39;esempio seguente.

   ![Errore rapido](../../assets/cdn/fastly-503-example.png)

## Apex e sottodomini già associati a un account Fastly

Se il dominio e i sottodomini APEX per il progetto di infrastruttura cloud Adobe Commerce on sono già associati a un account Fastly esistente con un ID servizio assegnato, non puoi avviarli finché non aggiorni la configurazione Fastly:

- Aggiorna la configurazione di apex e sottodominio sull’account Fastly esistente. Vedi [Più account Fastly e domini assegnati](fastly.md#domain).

- [Attiva e configura Fastly](fastly-configuration.md#enable-fastly-caching) e completa la [configurazione DNS](../launch/checklist.md#update-dns-configuration-with-production-settings)

## Verificare o eseguire il debug dei servizi Fastly

Puoi risolvere i problemi di prestazioni o caching per un sito Adobe Commerce sull’infrastruttura cloud testando gli URL del sito ed esaminando i valori di intestazione restituiti nella risposta.

### Controlla il sito live tramite Fastly

Utilizza l&#39;API Fastly per controllare le intestazioni di risposta `Fastly-Magento-VCL-Uploaded` e `X-Cache` restituite dal tuo sito live.

Le richieste API Fastly vengono trasmesse tramite l’estensione Fastly per ottenere una risposta dai server di origine. Se la risposta restituisce intestazioni non corrette, verificare direttamente i [server di origine](#bypass-fastly-cache-to-check-adobe-commerce-sites).

**Per controllare le intestazioni di risposta**:

1. In un terminale, utilizza il seguente comando `curl` per testare l&#39;URL live del sito:

   ```bash
   curl https://<live URL> -vo /dev/null -H Fastly-Debug:1
   ```

   Se non hai impostato una route statica o completato la configurazione DNS per i domini sul sito live, utilizza il flag `--resolve`, che ignora la risoluzione dei nomi DNS.

   ```bash
   curl -svo /dev/null --resolve '<your_hostname>:443:<IP-address-of-cache-node>' <https-URL>
   ```

   >[!NOTE]
   >
   >Per utilizzare questo comando con l&#39;opzione `--resolve`, è necessario che TLS sia abilitato con Fastly tramite un certificato SSL/TLS e che sia stato trovato l&#39;indirizzo IP del nodo della cache.

1. Nella risposta, verifica le [intestazioni](#check-cache-hit-and-miss-response-headers) per assicurarti che Fastly funzioni. Dovresti visualizzare le seguenti intestazioni univoche nella risposta:

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Se i valori delle intestazioni non sono corretti, consulta le seguenti informazioni:

- [Verifica caricamento VCL](#fastly-vcl-has-not-been-uploaded)

- [X-Cache contiene solo mancanti, nessun HIT](#x-cache-contains-only-miss-no-hit)

### Ignora Fastly Cache per controllare i siti Adobe Commerce

Se il servizio Fastly restituisce intestazioni non corrette, puoi creare uno snippet VCL che consente di inviare richieste ignorando la cache Fastly. Vedere [Ignora Fastly Cache](fastly-vcl-bypass-to-origin.md).

Dopo aver aggiunto lo snippet VCL, utilizzare i comandi cURL per inviare le richieste al server di origine dall&#39;indirizzo IP specificato. Quindi, controlla le risposte per individuare eventuali errori.

### Controllare le intestazioni di risposta cache HIT e MISS

Verifica che la risposta restituita contenga le seguenti informazioni:

- Include l&#39;intestazione `X-Magento-Tags`

- Il valore dell&#39;intestazione `Fastly-Module-Enabled` è `Yes` o il numero di versione del modulo Fastly per CDN Magento 2 installato nell&#39;ambiente del progetto

- [Cache-Control: max-age](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9) è maggiore di 0

- [Pragma](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.32) impostazione `cache`

L&#39;estratto seguente dell&#39;output del comando cURL mostra i valori corretti per le intestazioni `Pragma`, `X-Magento-Tags` e `Fastly-Module-Enabled`:

```terminal
* STATE: INIT => CONNECT handle 0x600057800; line 1402 (connection #-5000)
* Rebuilt URL to: https://www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud/
* Added connection 0. The cache now contains 1 members
* Trying 192.0.2.31...
* STATE: CONNECT => WAITCONNECT handle 0x600057800; line 1455 (connection #0)

% Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* Connected to www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud (54.229.163.31) port 443 (#0)

* STATE: WAITCONNECT => SENDPROTOCONNECT handle 0x600057800; line 1562 (connection #0)
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0* ALPN, offering h2

... portion omitted for brevity ...

< Set-Cookie: mage-messages=%5B%5D; expires=Wed, 22-Nov-2017 17:39:58 GMT; Max-Age=31536000; path=/
< Pragma: cache
< Expires: Wed, 23 Nov 2016 17:39:56 GMT
< Cache-Control: max-age=86400, public, s-maxage=86400, stale-if-error=5, stale-while-revalidate=5
< X-Magento-Tags: cb_welcome_popup store cb cb_store_info_mobile cb_header_promotional_bar cb_store_info cb_discount-promo-bar cpg_2 cb_83 cb_81 cb_84 cb_85 cb_86 cb_87 cb_88 cb_89 p5646 catalog_product p5915 p6040 p6197 p6227 p7095 p6109 p6122 p6331 p7592 p7651 p7690
< Fastly-Module-Enabled: yes
< Strict-Transport-Security: max-age=31536000
    < Content-Security-Policy: upgrade-insecure-requests
    < X-Content-Type-Options: nosniff
    < X-XSS-Protection: 1; mode=block
    < X-Frame-Options: SAMEORIGIN
    < X-Platform-Server: i-dff64b52
    <
    * STATE: PERFORM => DONE handle 0x600057800; line 1955 (connection #0)
    * multi_done
      0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
    * Connection #0 to host www.mymagento.biz.c.sv7gVom4qrpek.ent.magento.cloud left intact
```

>[!NOTE]
>
>Per informazioni dettagliate sugli hit e sui mancati riscontri, consulta [Informazioni sulle intestazioni HIT e MISS della cache con servizi schermati](https://docs.fastly.com/guides/performance-tuning/understanding-cache-hit-and-miss-headers-with-shielded-services) nella documentazione Fastly.

### Risolvi gli errori rilevati nelle intestazioni di risposta

Questa sezione fornisce suggerimenti per la risoluzione degli errori restituiti durante il controllo delle intestazioni di risposta tramite l’API Fastly.

#### Il modulo Fastly non è abilitato

Se il modulo Fastly non è abilitato (`Fastly-Module-Enabled: no`) o se manca l&#39;intestazione, [utilizzare SSH per accedere](../development/secure-connections.md#connect-to-a-remote-environment) al progetto. Quindi, esegui il seguente comando per controllare lo stato del modulo.

```bash
php bin/magento module:status Fastly_Cdn
```

In base allo stato restituito, utilizza le istruzioni seguenti per aggiornare la configurazione Fastly.

- `Module does not exist` - Se il modulo non esiste [installare e configurare](https://github.com/fastly/fastly-magento2/blob/master/Documentation/INSTALLATION.md) il modulo CDN Fastly per il Magento 2 in un ramo di integrazione. Al termine dell’installazione, abilita e configura il modulo. Vedi [Configura Fastly](fastly-configuration.md).

- `Module is disabled` - Se il modulo Fastly è disabilitato, aggiornare la configurazione dell&#39;ambiente in un ramo `integration` nell&#39;ambiente locale per abilitarlo. Quindi, invia le modifiche a Staging e Produzione. Consulta [Gestione estensioni](../store/extensions.md#install-an-extension).

  Se si utilizza [Gestione configurazione](../store/store-settings.md#configure-store), controllare lo stato del modulo Fastly CDN nel file di configurazione `app/etc/config.php` prima di inviare le modifiche all&#39;ambiente di produzione o di gestione temporanea.

  Se il modulo non è abilitato (`Fastly_CDN => 0`) nel file `config.php`, eliminare il file ed eseguire il comando seguente per aggiornare `config.php` con le impostazioni di configurazione più recenti.

  ```bash
  bin/magento magento-cloud:scd-dump
  ```

#### VCL Fastly non è stato caricato

Se il file VCL Fastly non è stato caricato (`Fastly-Magento-VCL-Uploaded`: `false`), utilizzare l&#39;opzione *Carica VCL* nell&#39;amministratore per caricarlo. Consulta [Caricare frammenti VCL Fastly](fastly-configuration.md#upload-vcl-to-fastly).

#### X-Cache contiene solo mancanti, nessun HIT

Se l&#39;intestazione `X-Cache` contiene `HIT` (`HIT, HIT` o `HIT, MISS`), indica che Fastly restituisce correttamente il contenuto memorizzato nella cache.

Se l&#39;intestazione `X-Cache` è `MISS, MISS` e non contiene `HIT`, eseguire nuovamente il comando `curl` per assicurarsi che la pagina non sia stata eliminata di recente dalla cache.

Se ottieni lo stesso risultato, utilizza i [`curl` comandi](#check-live-site-through-fastly) e verifica le [intestazioni di risposta](#check-cache-hit-and-miss-response-headers):

- `Pragma` è `cache`
- `X-Magento-Tags` esiste già
- `Cache-Control: max-age` è maggiore di 0

Se il problema persiste, è probabile che un’altra estensione ripristini queste intestazioni. Ripeti la procedura seguente nell’ambiente di staging disabilitando tutte le estensioni e riabilitando ciascuna per determinare quale estensione sta reimpostando le intestazioni. Dopo aver identificato l’estensione che causa il problema, devi disabilitarla nell’ambiente di produzione.

**Per identificare un&#39;estensione che ripristina le intestazioni di risposta:**

{{admin-login-step}}

1. Passa a **Archivi** > **Impostazioni** > **Configurazione** > **Avanzate** > **Avanzate**.

1. Nella sezione *Disabilita output moduli* nel riquadro di destra, individua tutte le estensioni e disabilitale.

1. Fai clic su **Salva configurazione**.

1. Fare clic su **Sistema** > **Strumenti** > **Gestione cache**.

1. Fare clic su **Svuota cache Magento**.

1. Completa i seguenti passaggi per ogni estensione che può causare problemi con le intestazioni Fastly:

   - Abilita un’estensione alla volta, salva la configurazione e svuota la cache di Adobe Commerce.

   - Esegui i comandi [`curl`](#check-live-site-through-fastly) per verificare le [intestazioni di risposta](#check-cache-hit-and-miss-response-headers).

   Ripeti questa procedura per ogni estensione. Se le intestazioni di risposta Fastly non vengono più visualizzate, hai identificato l’estensione che sta causando problemi con Fastly.

Dopo aver identificato l’estensione che sta reimpostando le intestazioni Fastly, contatta lo sviluppatore di estensioni per ulteriore assistenza. Non è possibile fornire correzioni o aggiornamenti per far funzionare le estensioni di terze parti con il caching Fastly.

## Rollback della configurazione Fastly

Se gli aggiornamenti personalizzati del frammento di codice VCL o altre modifiche alla configurazione Fastly causano l&#39;interruzione o la restituzione di errori in un sito di infrastruttura cloud di Adobe Commerce, utilizza il comando Fastly API [activate](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) per tornare a una versione VCL precedente. Non è possibile eseguire il rollback della versione VCL dall&#39;amministratore.

**Per eseguire il rollback della versione VCL**:

1. Per ottenere un elenco delle versioni VCL disponibili per un servizio, eseguire il comando seguente

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Accept: application/json" https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version
   ```

1. Eseguire il comando seguente per modificare la versione VCL attiva in una versione specificata.

   ```bash
   curl -H "Fastly-Key: <FASTLY_API_TOKEN>" -H "Content-Type: application/x-www-form-urlencoded" -H "Accept: application/json" -X PUT https://api.fastly.com/service/<FASTLY_SERVICE_ID>/version/<VERSION_ID>/activate
   ```

Per informazioni dettagliate sull&#39;utilizzo dell&#39;API Fastly per rivedere e gestire VCL, vedere [Gestire VCL utilizzando l&#39;API](fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
