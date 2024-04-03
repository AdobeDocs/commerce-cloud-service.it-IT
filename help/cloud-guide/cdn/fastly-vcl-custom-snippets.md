---
title: Introduzione ai frammenti VCL personalizzati
description: Scopri come utilizzare i frammenti di codice del linguaggio di controllo della vernice per personalizzare la configurazione del servizio Fastly per Adobe Commerce.
feature: Cloud, Configuration, Services
exl-id: df0f0906-8ffa-41a1-a31c-d36deb5a6a31
source-git-commit: 89c57a486545d6165e0407f913f4fa4cf6c95abe
workflow-type: tm+mt
source-wordcount: '1947'
ht-degree: 0%

---

# Guida introduttiva a VCL personalizzato

Fastly supporta una versione personalizzata del linguaggio di configurazione della vernice (VCL) per adattare la configurazione del servizio Fastly alle tue esigenze.

I snippet VCL personalizzati sono blocchi di logica VCL aggiunti alla versione VCL attiva caricata sul sito Adobe Commerce. Un frammento VCL personalizzato modifica la risposta dei servizi di memorizzazione nella cache rapida al traffico di richiesta. Ad esempio, puoi aggiungere uno snippet VCL personalizzato per consentire il traffico delle richieste solo dagli indirizzi IP dei client specificati. In alternativa, crea uno snippet per bloccare il traffico proveniente da siti web noti per l’invio di spam di riferimento ai siti Adobe Commerce.

Snippet VCL personalizzati, generati, compilati e trasmessi a tutte le cache Fastly, vengono caricati e attivati senza tempi di inattività del server.

>[!NOTE]
>
>Prima di aggiungere codice VCL personalizzato, dizionari perimetrali e ACL alla configurazione del modulo Fastly, verificare che il servizio di caching Fastly funzioni con la configurazione predefinita. Consulta [Configurare i servizi Fastly](fastly-configuration.md).

Fastly supporta due tipi di snippet VCL personalizzati:

- [Snippet regolari](https://docs.fastly.com/en/guides/about-vcl-snippets)- I normali snippet VCL personalizzati sono codificati per specifiche versioni VCL. Puoi creare, modificare e distribuire snippet VCL regolari dall’API Admin o Fastly.

- [Snippet dinamici](https://docs.fastly.com/en/guides/using-dynamic-vcl-snippets)- Snippet VCL creati utilizzando l&#39;API Fastly. È possibile modificare e distribuire snippet dinamici senza dover aggiornare la versione VCL Fastly per il servizio.

È consigliabile utilizzare snippet VCL personalizzati con dizionari Edge ed elenchi di controllo di accesso (ACL) per memorizzare i dati utilizzati nel codice personalizzato.

- [**Dizionario Edge**](https://docs.fastly.com/guides/edge-dictionaries/about-edge-dictionaries)- Memorizza i dati come coppie chiave-valore in un contenitore di dizionari a cui è possibile fare riferimento dai snippet VCL personalizzati

- [**ACL Edge**](https://docs.fastly.com/guides/access-control-lists/about-acls)- Memorizza i dati dell&#39;indirizzo IP del client che definiscono l&#39;elenco di controllo di accesso per le regole di blocco o di autorizzazione implementate utilizzando snippet VCL personalizzati

I dati del dizionario e dell&#39;ACL vengono distribuiti nei nodi Fastly Edge accessibili nelle aree di rete. Inoltre, i dati possono essere aggiornati dinamicamente in tutta la rete senza richiedere la ridistribuzione del codice VCL per l’ambiente di staging o produzione.

>[!NOTE]
>
>È possibile aggiungere snippet VCL personalizzati a un ambiente di staging o produzione solo se [servizi Fastly configurati](fastly-configuration.md) per tale ambiente.

## Esercitazione

Questo tutorial ed esempi illustrano l’utilizzo di normali snippet VCL personalizzati con dizionari Edge e ACL Edge per personalizzare la configurazione del servizio Fastly per Adobe Commerce. Per informazioni più dettagliate, consulta la documentazione Fastly:

- [Guida a Fastly VCL](https://docs.fastly.com/guides/vcl/guide-to-vcl)—Informazioni sull&#39;implementazione di Fastly Varnish, le estensioni di Fastly VCL e risorse per saperne di più su Varnish e VCL.
- [Riferimento VCL Fastly](https://docs.fastly.com/guides/vcl/)- Riferimento di programmazione dettagliato per sviluppare e risolvere i problemi relativi a snippet VCL personalizzati Fastly e personalizzati.

Puoi creare e gestire snippet VCL personalizzati dall’amministratore di Adobe Commerce o utilizzando l’API Fastly:

- [Amministratore Adobe Commerce](#manage-custom-vcl-from-admin)- Si consiglia di utilizzare l&#39;amministratore Adobe Commerce per gestire snippet VCL personalizzati, in quanto automatizza il processo di convalida, caricamento e applicazione delle modifiche VCL alla configurazione del servizio Fastly. Inoltre, puoi visualizzare e modificare gli snippet VCL personalizzati aggiunti alla configurazione del servizio Fastly dall’amministratore.

- [API Fastly](#manage-vcl-using-the-api)- Se non è possibile accedere ad Admin, utilizzare l&#39;API Fastly per gestire i snippet VCL personalizzati. Ad esempio, utilizza l’API per risolvere i problemi relativi alla configurazione del servizio Fastly quando il sito non è accessibile o per aggiungere uno snippet VCL personalizzato. Inoltre, alcune operazioni possono essere completate solo utilizzando l’API. Ad esempio, è necessario utilizzare l&#39;API per riattivare una versione VCL precedente o per visualizzare tutti i snippet VCL inclusi in una versione VCL specifica. Consulta [Riferimento rapido API per snippet VCL](#api-quick-reference-for-vcl-snippets).

### Esempio di codice snippet VCL

L’esempio seguente mostra lo snippet VCL personalizzato (formato JSON) che filtra il traffico in base all’indirizzo IP del client:

```json
{
  "service_id": "FASTLY_SERVICE_ID",
  "version": "{Editable Version #}",
  "name": "apply_acl",
  "priority": "100",
  "dynamic": "1",
  "type": "hit",
  "content": "if ((client.ip ~ {ACLNAME}) && !req.http.Fastly-FF){ error 403; }"
}
```

>[!WARNING]
>
>In questo esempio, il codice VCL è formattato come payload JSON che può essere salvato in un file e inviato in una richiesta API Fastly. Per evitare errori di convalida JSON durante l’invio dello snippet come JSON per una richiesta API, utilizza una barra rovesciata per eliminare i caratteri speciali nel codice. Consulta [Utilizzo di snippet VCL dinamici](https://docs.fastly.com/vcl/vcl-snippets/) nella documentazione di Fastly VCL. Se si invia lo snippet VCL dall&#39;amministratore, non è necessario utilizzare caratteri speciali come carattere di escape.

Logica VCL nella `content` esegue le azioni seguenti:

- Controlla l&#39;indirizzo IP in ingresso. `client.ip` su ogni richiesta

- Blocca qualsiasi richiesta con un indirizzo IP incluso nel *ACLNAME* ACL perimetrale, restituisce un `403 Forbidden` errore

Nella tabella seguente vengono forniti i dettagli sui dati chiave per i snippet VCL personalizzati. Per maggiori dettagli, vedere [Snippet VCL](https://docs.fastly.com/api/config#api-section-snippet) nella documentazione di Fastly.

| Valore | Descrizione |
|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `API_KEY` | La chiave API per accedere all’account Fastly. Consulta [Ottieni credenziali](fastly-configuration.md). |
| `active` | Stato attivo del frammento o della versione. Restituisce `true` o `false`. Se true, lo snippet o la versione è in uso. Clonare un frammento attivo utilizzando il relativo numero di versione. |
| `content` | Frammento di codice VCL da eseguire. Fastly non supporta tutte le funzionalità del linguaggio VCL. Inoltre, Fastly fornisce le estensioni con funzionalità personalizzate. Per informazioni dettagliate sulle funzioni supportate, vedi [Riferimento programmazione Fastly VCL](https://docs.fastly.com/vcl/reference/). |
| `dynamic` | Stato dinamico di uno snippet. Restituisce `false` per [snippet regolari](https://docs.fastly.com/en/guides/about-vcl-snippets) incluso nel file VCL con versione per la configurazione del servizio Fastly. Restituisce `true` per un [snippet dinamico](https://docs.fastly.com/vcl/vcl-snippets/using-dynamic-vcl-snippets/) che possono essere modificati e distribuiti senza richiedere una nuova versione VCL. |
| `number` | Numero di versione VCL in cui è incluso il frammento. Utilizzi veloci *N. versione modificabile* nei valori di esempio. Se aggiungi snippet personalizzati dall’API, includi il numero di versione nella richiesta API. Se aggiungi un file VCL personalizzato dall’amministratore, la versione viene fornita automaticamente. |
| `priority` | Valore numerico da `1` a `100` che specifica quando viene eseguito il codice snippet VCL personalizzato. I frammenti con valori di priorità inferiori vengono eseguiti per primi. Se non viene specificato, il `priority` valore predefinito `100`.<p>Qualsiasi frammento VCL personalizzato con valore di priorità `5` viene eseguito immediatamente, il che è meglio per il codice VCL che implementa l’indirizzamento delle richieste (inserisce nell&#39;elenco Consentiti e reindirizzamenti di blocchi e di). Priorità `100` è ideale per ignorare il codice frammento VCL predefinito.<p>Tutti [snippet VCL predefiniti](fastly-configuration.md#upload-vcl-snippets) inclusi nel modulo Magento-Fastly sono `priority=50`.<ul><li>Assegna una priorità alta come `100` per eseguire un codice VCL personalizzato dopo tutte le altre funzioni VCL e sostituire il codice VCL predefinito.</li></ul> |
| `service_id` | L’ID Fastly Service per un ambiente di staging o produzione specifico. Questo ID viene assegnato quando il progetto viene aggiunto all’infrastruttura cloud di Adobe Commerce [Account Fastly Service](fastly.md#fastly-service-account-and-credentials). |
| `type` | Specifica la posizione in cui inserire lo snippet generato, ad esempio `init` (sopra le subroutine) e `recv` (nelle subroutine). Per ulteriori informazioni, vedere la sezione [Snippet VCL](https://docs.fastly.com/api/config#api-section-snippet) riferimento. |

## Gestisci VCL personalizzato da amministratore

È possibile [aggiungere snippet VCL personalizzati](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md) dal *Configurazione rapida* > *Snippet VCL personalizzati* in Admin.

![Gestire snippet VCL personalizzati](../../assets/cdn/fastly-edit-snippets.png)

Il *Snippet VCL personalizzati* La vista mostra solo i frammenti aggiunti tramite l&#39;amministratore. Se si aggiungono snippet utilizzando l’API Fastly, utilizza l’API per [gestirli](#manage-vcl-using-the-api).

Gli esempi seguenti mostrano come creare e gestire snippet VCL personalizzati dall’amministratore e utilizzare i moduli Fastly Edge e i dizionari Edge:

- [Reindirizzare le richieste a un back-end CMS](fastly-vcl-wordpress.md)
- [Blocca spam di riferimento](fastly-vcl-badreferer.md)
- [Blocca spam di riferimento](fastly-vcl-badreferer.md)
- [VCL personalizzato per il inserisco nell&#39;elenco Consentiti di](fastly-vcl-allowlist.md)
- [VCL personalizzato per il inserisco nell&#39;elenco Bloccati di](fastly-vcl-blocking.md)
- [Ignora Fastly Cache](fastly-vcl-bypass-to-origin.md)

## Gestire VCL tramite l’API

La procedura dettagliata seguente illustra come creare file di snippet VCL regolari e aggiungerli alla configurazione del servizio Fastly utilizzando l’API Fastly. È possibile creare e gestire gli snippet da *terminale* applicazione. Non è necessaria una connessione SSH in un ambiente specifico.

**Prerequisiti:**

- Configura il tuo ambiente Adobe Commerce su infrastruttura cloud per i servizi Fastly. Consulta [Configura Fastly](fastly-configuration.md).

- [Ottieni credenziali API veloci](fastly-configuration.md) per autenticare le richieste nell’API Fastly. Assicurati di ottenere le credenziali per l’ambiente corretto: Staging o Produzione.

- Salva le credenziali del servizio Fastly come variabili di ambiente di base che è possibile utilizzare nei comandi cURL:

  ```bash
  export FASTLY_SERVICE_ID=<Service-ID>
  ```

  ```bash
  export FASTLY_API_TOKEN=<API-Token>
  ```

  Le variabili di ambiente esportate sono disponibili solo nella sessione di base corrente e vengono perse quando si chiude il terminale. Puoi ridefinire le variabili esportando un nuovo valore. Per visualizzare l’elenco delle variabili esportate relative a Fastly:

  ```bash
  export | grep FASTLY
  ```

## Aggiungi snippet VCL

Questo tutorial illustra i passaggi di base per aggiungere snippet personalizzati utilizzando l’API Fastly.

>[!NOTE]
>
>Per informazioni su come gestire snippet VCL personalizzati dall’amministratore di Adobe Commerce, consulta [Gestire VCL dall’amministratore di Adobe Commerce](#manage-custom-vcl-from-admin).


**Prerequisiti**

{{$include /help/_includes/vcl-snippet-prerequisites.md}}

### Passaggio 1: individuare la versione VCL attiva

Utilizzare l’API Fastly [ottieni versione](https://docs.fastly.com/api/config#version_dfde9093f4eb0aa2497bbfd1d9415987) operazione per ottenere il numero di versione VCL attivo:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
```

Nella risposta JSON, tieni presente il numero di versione VCL attivo restituito nella `number` chiave, ad esempio `"number": 99`. Il numero di versione è necessario quando clonate il file VCL per la modifica.

```json
{
  "testing": false,
  "locked": true,
  "number": 99,
  "active": true,
  "service_id": "872zhjyxhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Salva il numero di versione attivo in una variabile di ambiente di base da utilizzare nelle richieste API successive:

```bash
export FASTLY_VERSION_ACTIVE=<Version>
```

### Passaggio 2: clonare la versione VCL attiva e tutti i frammenti

Prima di poter aggiungere o modificare snippet VCL personalizzati, è necessario creare una copia della versione VCL attiva per la modifica. Utilizzare l’API Fastly [clone](https://docs.fastly.com/api/config#version_7f4937d0663a27fbb765820d4c76c709) operazione:

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION_ACTIVE/clone -X PUT
```

Nella risposta JSON, il numero di versione viene incrementato e il *attivo* il valore chiave è `false`. È possibile modificare localmente la nuova versione VCL inattiva.

```json
{
  "testing": false,
  "locked": false,
  "number": 100,
  "active": false,
  "service_id": "vW2bLFWhhto5SIRb3GAE0",
  "staging": false,
  "created_at": "2019-01-29T22:38:53Z",
  "deleted_at": null,
  "comment": "Magento Module uploaded VCL",
  "updated_at": "2019-01-29T22:39:06Z",
  "deployed": false
}
```

Salva il nuovo numero di versione in una variabile di ambiente bash da utilizzare nei comandi successivi:

```bash
export FASTLY_EDIT_VERSION=<Version>
```

### Passaggio 3: creare uno snippet VCL personalizzato

Crea e salva il codice VCL personalizzato in un file JSON con il contenuto e il formato seguenti:

```json
{
  "name": "<name>",
  "dynamic": "0",
  "type": "<type>",
  "priority": "100",
  "content": "<code all in one line>"
}
```

I valori includono:

- `name`- Nome del frammento VCL.

- `dynamic`- Indica se si tratta di un [frammento normale](https://docs.fastly.com/en/guides/about-vcl-snippets) o un [snippet dinamico](https://docs.fastly.com/guides/vcl-snippets/using-dynamic-vcl-snippets).

- `type`- Specifica la posizione in cui inserire lo snippet generato, ad esempio `init` (sopra le subroutine) e `recv` (nelle subroutine). Consulta [Valori oggetto frammento VCL Fastly](https://docs.fastly.com/api/config#snippet) per informazioni su questi valori.

- `priority`- Un valore da `1` a `100` che determina quando viene eseguito il codice snippet VCL personalizzato. Vengono eseguiti prima snippet VCL personalizzati con valori inferiori.

  Tutti i codici VCL predefiniti del modulo Fastly VCL hanno un `priority` di `50`. Se si desidera che un&#39;azione venga eseguita per ultima o che venga ignorato il codice VCL predefinito, utilizzare un numero più alto, ad esempio `100`. Per eseguire immediatamente il codice snippet VCL personalizzato, impostate la priorità su un valore inferiore, ad esempio `5`.

- `content`- Frammento di codice VCL da eseguire in una riga, senza interruzioni di riga. Consulta [Esempio di snippet VCL personalizzato](#example-vcl-snippet-code).

### Passaggio 4: aggiungere lo snippet VCL alla configurazione Fastly

Utilizzare l’API Fastly [crea snippet](https://docs.fastly.com/api/config#snippet_41e0e11c662d4d56adada215e707f30d) per aggiungere lo snippet VCL personalizzato alla versione VCL.

```bash
curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/snippet -H 'Content-Type: application/json' -X POST --data @<filename.json>
```

Il `<filename.json>` è il nome del file preparato nel passaggio precedente. Ripetete questo comando per ogni frammento VCL.

Se riceve un `500 Internal Server Error` risposta dal servizio Fastly, controlla la sintassi del file JSON per assicurarti di caricare un file valido.

### Passaggio 5: Convalidare e attivare snippet VCL personalizzati

Dopo aver aggiunto uno snippet VCL personalizzato, Fastly inserisce lo snippet nella versione VCL che si sta modificando. Per applicare le modifiche, completare i passaggi seguenti per convalidare il codice dello snippet VCL e attivare la versione VCL.

1. Utilizzare l’API Fastly [convalida versione VCL](https://docs.fastly.com/api/config#version_97f8cf7bfd5dc2e5ea1933d94dc5a9a6) operazione per verificare il codice VCL aggiornato.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/validate
   ```

   Se l’API Fastly restituisce un errore, correggi il problema e convalida di nuovo la versione VCL aggiornata.

1. Utilizzare l’API Fastly [attivare](https://docs.fastly.com/api/config#version_0b79ae1ba6aee61d64cc4d43fed1e0d5) per attivare la nuova versione VCL.

   ```bash
   curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_EDIT_VERSION/activate -X PUT
   ```

## Riferimento rapido API per snippet VCL

Questi esempi di richieste API utilizzano variabili di ambiente esportate per fornire le credenziali di autenticazione con Fastly. Per informazioni dettagliate su questi comandi, vedere [Riferimento API Fastly](https://docs.fastly.com/api/config#vcl).

>[!NOTE]
>
>Utilizza questi comandi per gestire i frammenti aggiunti utilizzando l’API Fastly. Se hai aggiunto snippet dall’amministratore, consulta [Gestire i snippet VCL tramite l&#39;amministratore](#manage-vcl-using-the-api).

- **Ottieni numero di versione VCL attivo**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/active
  ```

- **Elenca tutti i normali snippet VCL collegati a un servizio**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet
  ```

- **Esaminare un singolo frammento**

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name>
  ```

  Il `<snippet_name>` è il nome di un frammento, ad esempio `my_regular_snippet`.

- **Aggiornare uno snippet**

  Modifica il [file JSON preparato](#step-3-create-a-custom-vcl-snippet) e invia la seguente richiesta:

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -H 'Content-Type: application/json' -X PUT --data @<filename.json>
  ```

- **Eliminare un singolo frammento VCL**

  Ottenere un elenco di snippet e utilizzare quanto segue `curl` comando con il nome specifico del frammento da eliminare:

  ```bash
  curl -H "Fastly-Key: $FASTLY_API_TOKEN" https://api.fastly.com/service/$FASTLY_SERVICE_ID/version/$FASTLY_VERSION/snippet/<snippet_name> -X DELETE
  ```

- **Sostituisci i valori in [codice VCL Fastly predefinito](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets)**

  Crea uno snippet con valori aggiornati e assegna la priorità `100`.
