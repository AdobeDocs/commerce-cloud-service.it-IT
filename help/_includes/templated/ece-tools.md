---
source-git-commit: 28aaae20fa03f31107bcd3fb569350a68842b152
workflow-type: tm+mt
source-wordcount: '3125'
ht-degree: 0%

---
# strumenti ece

<!-- The template to render with above values -->
**Versione**: 2002.1.14

Questo riferimento contiene 32 comandi disponibili tramite `ece-tools` strumento da riga di comando.
L’elenco iniziale viene generato automaticamente utilizzando `ece-tools list` su Adobe Commerce su infrastruttura cloud.

>[!NOTE]
>
>Questo riferimento viene generato dalla base di codice dell&#39;applicazione. Per modificare il contenuto, puoi aggiornare il codice sorgente per l’implementazione del comando corrispondente in [codebase](https://github.com/magento/magento-cloud-cli) e inviare le modifiche per la revisione. Un altro modo consiste nel _Inviaci feedback_ (trovi il collegamento in alto a destra). Per le linee guida sui contributi, consulta [Contributi codice](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

## `build`

Genera l&#39;applicazione.

```bash
ece-tools build
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `db-dump`

Crea backup del database.

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```


### `databases`

Database per il backup. Valori disponibili: [vendite dell&#39;offerta principale]. Se il valore dell&#39;argomento non viene specificato, i backup del database verranno creati utilizzando le credenziali memorizzate in `MAGENTO_CLOUD_RELATIONSHIP` variabile di ambiente e/o `stage.deploy.DATABASE_CONFIGURATION` proprietà del file di configurazione .magento.env.yaml.

- Predefinito: `[]`

- Array

### `--remove-definers`, `-d`

Rimuovi i definitori dal dump del database

- Predefinito: `false`
- Non accetta un valore

### `--dump-directory`, `-a`

Usa directory alternativa per salvare l’immagine

- Richiede un valore

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `deploy`

Distribuisce l&#39;applicazione.

```bash
ece-tools deploy
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `help`

Visualizza la Guida per un comando

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```


### `command_name`

Nome del comando

- Predefinito: `help`


### `--format`

Il formato di output (txt, xml, json o md)

- Predefinito: `txt`
- Richiede un valore

### `--raw`

Per visualizzare la guida dei comandi raw

- Predefinito: `false`
- Non accetta un valore

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `list`

Comandi elenco

```bash
ece-tools list [--raw] [--format FORMAT] [--] [<namespace>]
```


### `namespace`

Nome dello spazio dei nomi


### `--raw`

Per generare l&#39;elenco dei comandi raw

- Predefinito: `false`
- Non accetta un valore

### `--format`

Il formato di output (txt, xml, json o md)

- Predefinito: `txt`
- Richiede un valore


## `patch`

Applica patch personalizzate.

```bash
ece-tools patch
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `post-deploy`

Esegue dopo le operazioni di distribuzione.

```bash
ece-tools post-deploy
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `run`

Esegui uno o più scenari.

```bash
ece-tools run <scenario>...
```


### `scenario`

Scenario/i

- Predefinito: `[]`

- Obbligatorio
- Array

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `backup:list`

Mostra l&#39;elenco dei file di backup.

```bash
ece-tools backup:list
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `backup:restore`

Ripristinare i file di configurazione importanti. Esegui backup:elenco per visualizzare l&#39;elenco dei file di backup.

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

### `--force`, `-f`

Sovrascrivi i file esistenti durante il ripristino di un backup

- Predefinito: `false`
- Non accetta un valore

### `--file`

Un percorso di ripristino file specifico

- Accetta un valore

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `build:generate`

Genera tutti i file necessari per la fase di build.

```bash
ece-tools build:generate
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `build:transfer`

Trasferisce i file generati nella directory iniziale.

```bash
ece-tools build:transfer
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `cloud:config:create`

Crea un `.magento.env.yaml` con la configurazione specificata per la generazione, la distribuzione e la post-distribuzione delle variabili. Sovrascrive eventuali `.magento,.env.yaml` file.

```bash
ece-tools cloud:config:create <configuration>
```


### `configuration`

Configurazione in formato JSON

- Obbligatorio

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `cloud:config:update`

Aggiorna il `.magento.env.yaml` con la configurazione specificata. Crea `.magento.env.yaml` se non esiste.

```bash
ece-tools cloud:config:update <configuration>
```


### `configuration`

Configurazione in formato JSON

- Obbligatorio

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `cloud:config:validate`

Convalida `.magento.env.yaml` file di configurazione

```bash
ece-tools cloud:config:validate
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `config:dump`

Configurazione del dump per la distribuzione di contenuti statici.

```bash
ece-tools config:dump
```


```bash
ece-tools dump
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `cron:disable`

Disattiva tutti i processi cron di Magento e interrompe tutti i processi in esecuzione.

```bash
ece-tools cron:disable
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `cron:enable`

Abilita i processi cron di Magento.

```bash
ece-tools cron:enable
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `cron:kill`

Termina tutti i processi cron del Magento.

```bash
ece-tools cron:kill
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `cron:unlock`

Sblocca i processi cron bloccati nello stato &quot;in esecuzione&quot;.

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

### `--job-code`

Cron codice processo da sbloccare.

- Predefinito: `[]`
- Accetta più valori

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `dev:generate:schema-error`

Genera il file dist/error-codes.md dal file schema.error.yaml.

```bash
ece-tools dev:generate:schema-error
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `dev:git:update-composer`

Aggiorna il compositore per la distribuzione da Git.

```bash
ece-tools dev:git:update-composer
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `env:config:show`

Visualizza le variabili dell’ambiente di configurazione cloud codificate.

```bash
ece-tools env:config:show [<variable>...]
```


### `variable`

Variabili di ambiente da visualizzare, opzioni possibili: servizi, percorsi, variabili

- Predefinito: `[]`

- Array

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `error:show`

Visualizza informazioni sull’errore per ID errore o informazioni su tutti gli errori dell’ultima distribuzione.

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```


### `error-code`

Codice di errore, se non viene passato il comando visualizza informazioni su tutti gli errori dell’ultima distribuzione


### `--json`, `-j`

Utilizzato per ottenere risultati in formato JSON

- Predefinito: `false`
- Non accetta un valore

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `module:refresh`

Aggiorna la configurazione per abilitare i moduli appena aggiunti.

```bash
ece-tools module:refresh
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `schema:generate`

Genera il file *.dist dello schema.

```bash
ece-tools schema:generate
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `wizard:ideal-state`

Verifica lo stato ideale della configurazione.

```bash
ece-tools wizard:ideal-state
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `wizard:master-slave`

Verifica la configurazione master-slave.

```bash
ece-tools wizard:master-slave
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `wizard:scd-on-build`

Verifica SCD sulla configurazione della build.

```bash
ece-tools wizard:scd-on-build
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `wizard:scd-on-demand`

Verifica la configurazione di SCD su richiesta.

```bash
ece-tools wizard:scd-on-demand
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `wizard:scd-on-deploy`

Verifica SCD sulla configurazione di distribuzione.

```bash
ece-tools wizard:scd-on-deploy
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `wizard:split-db-state`

Verifica la possibilità di suddividere il database e se il database è già stato suddiviso o meno.

```bash
ece-tools wizard:split-db-state
```

### `--help`, `-h`

Visualizza questo messaggio della Guida

- Predefinito: `false`
- Non accetta un valore

### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

### `--ansi`

Forza uscita ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-ansi`

Disattiva output ANSI

- Predefinito: `false`
- Non accetta un valore

### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore

