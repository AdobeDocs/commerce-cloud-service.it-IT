---
source-git-commit: 63c86bab0f3feb5a3a641a7a785ea625338045f4
workflow-type: tm+mt
source-wordcount: '921'
ht-degree: 0%

---
# strumenti ece

**Versione**: 2002.2.0

Questo riferimento contiene 34 comandi disponibili tramite lo strumento della riga di comando `ece-tools`.
L&#39;elenco iniziale viene generato automaticamente utilizzando il comando `ece-tools list` in Adobe Commerce sull&#39;infrastruttura cloud.

## Generale

Questo riferimento viene generato dalla base di codice dell&#39;applicazione. Per modificare il contenuto, _Invia un feedback_ (trova il collegamento in alto a destra). Per le linee guida sui contributi, consulta [Contributi codice](https://developer.adobe.com/commerce/contributor/guides/code-contributions/).

### Opzioni globali

#### `--help`, `-h`

Visualizza la Guida per il comando specificato. Se non viene assegnato alcun comando, visualizzare la Guida per il comando list

- Predefinito: `false`
- Non accetta un valore

#### `--quiet`, `-q`

Non inviare messaggi

- Predefinito: `false`
- Non accetta un valore

#### `--verbose`, `-v|-vv|-vvv`

Aumenta la gravità dei messaggi: 1 per l’output normale, 2 per l’output più dettagliato e 3 per il debug

- Predefinito: `false`
- Non accetta un valore

#### `--version`, `-V`

Visualizza questa versione dell&#39;applicazione

- Predefinito: `false`
- Non accetta un valore

#### `--ansi`

Forza (o disattiva — senza ansi) output ANSI

- Non accetta un valore

#### `--no-ansi`

Ignora l&#39;opzione &quot;—ansi&quot;

- Predefinito: `false`
- Non accetta un valore

#### `--no-interaction`, `-n`

Non porre domande interattive

- Predefinito: `false`
- Non accetta un valore


## `_complete`

```bash
ece-tools _complete [-s|--shell SHELL] [-i|--input INPUT] [-c|--current CURRENT] [-a|--api-version API-VERSION] [-S|--symfony SYMFONY]
```

Comando interno per fornire suggerimenti per il completamento della shell

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).

#### `--shell`, `-s`

Tipo di conchiglia (&quot;bash&quot;, &quot;fish&quot;, &quot;zsh&quot;)

- Richiede un valore

#### `--input`, `-i`

Array di token di input (ad esempio, COMP_WORDS o argv)

- Predefinito: `[]`
- Richiede un valore

#### `--current`, `-c`

Indice dell&#39;array &quot;input&quot; in cui si trova il cursore (ad esempio, COMP_CWORD)

- Richiede un valore

#### `--api-version`, `-a`

Versione API dello script di completamento

- Richiede un valore

#### `--symfony`, `-S`

obsoleto

- Richiede un valore


## `build`

```bash
ece-tools build
```

Genera l&#39;applicazione.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `completion`

```bash
ece-tools completion [--debug] [--] [<shell>]
```

Scarica lo script di completamento della shell

```
The completion command dumps the shell completion script required
to use shell autocompletion (currently, bash, fish, zsh completion are supported).

Static installation
-------------------

Dump the script to a global completion file and restart your shell:

    bin/ece-tools completion  | sudo tee /etc/bash_completion.d/ece-tools

Or dump the script to a local file and source it:

    bin/ece-tools completion  > completion.sh

    # source the file whenever you use the project
    source completion.sh

    # or add this line at the end of your "~/.bashrc" file:
    source /path/to/completion.sh

Dynamic installation
--------------------

Add this to the end of your shell configuration file (e.g. "~/.bashrc"):

    eval "$(/var/jenkins/workspace/gendocs-ece-tools-cli/bin/ece-tools completion )"
```

### Argomenti

#### `shell`

Se non specificato, verrà utilizzato il tipo di shell (ad esempio &quot;bash&quot;) e il valore dell’env var &quot;$SHELL&quot;

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).

#### `--debug`

Suddividi il registro di debug di completamento

- Predefinito: `false`
- Non accetta un valore


## `db-dump`

```bash
ece-tools db-dump [-d|--remove-definers] [-a|--dump-directory DUMP-DIRECTORY] [--] [<databases>...]
```

Crea backup del database.

### Argomenti

#### `databases`

Database per il backup. Valori disponibili: [vendite preventivo principale]. Se il valore dell&#39;argomento non viene specificato, i backup del database verranno creati utilizzando le credenziali archiviate nella variabile di ambiente `MAGENTO_CLOUD_RELATIONSHIP` o/e la proprietà `stage.deploy.DATABASE_CONFIGURATION` del file di configurazione .magento.env.yaml.

- Predefinito: `[]`
- Array

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).

#### `--remove-definers`, `-d`

Rimuovi i definitori dal dump del database

- Predefinito: `false`
- Non accetta un valore

#### `--dump-directory`, `-a`

Usa directory alternativa per salvare l’immagine

- Richiede un valore


## `deploy`

```bash
ece-tools deploy
```

Distribuisce l&#39;applicazione.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `help`

```bash
ece-tools help [--format FORMAT] [--raw] [--] [<command_name>]
```

Visualizza la Guida per un comando

```
The help command displays help for a given command:

  bin/ece-tools help list

You can also output the help in other formats by using the --format option:

  bin/ece-tools help --format=xml list

To display the list of available commands, please use the list command.
```

### Argomenti

#### `command_name`

Nome del comando

- Predefinito: `help`

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).

#### `--format`

Il formato di output (txt, xml, json o md)

- Predefinito: `txt`
- Richiede un valore

#### `--raw`

Per visualizzare la guida dei comandi raw

- Predefinito: `false`
- Non accetta un valore


## `list`

```bash
ece-tools list [--raw] [--format FORMAT] [--short] [--] [<namespace>]
```

Comandi elenco

```
The list command lists all commands:

  bin/ece-tools list

You can also display the commands for a specific namespace:

  bin/ece-tools list test

You can also output the information in other formats by using the --format option:

  bin/ece-tools list --format=xml

It's also possible to get raw list of commands (useful for embedding command runner):

  bin/ece-tools list --raw
```

### Argomenti

#### `namespace`

Nome dello spazio dei nomi

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).

#### `--raw`

Per generare l&#39;elenco dei comandi raw

- Predefinito: `false`
- Non accetta un valore

#### `--format`

Il formato di output (txt, xml, json o md)

- Predefinito: `txt`
- Richiede un valore

#### `--short`

Per ignorare la descrizione degli argomenti dei comandi

- Predefinito: `false`
- Non accetta un valore


## `patch`

```bash
ece-tools patch
```

Applica patch personalizzate.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `post-deploy`

```bash
ece-tools post-deploy
```

Esegue dopo le operazioni di distribuzione.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `run`

```bash
ece-tools run <scenario>...
```

Esegui uno o più scenari.

### Argomenti

#### `scenario`

Scenario/i

- Predefinito: `[]`
- Obbligatorio

- Array

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `backup:list`

```bash
ece-tools backup:list
```

Mostra l&#39;elenco dei file di backup.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `backup:restore`

```bash
ece-tools backup:restore [-f|--force] [--file [FILE]]
```

Ripristinare i file di configurazione importanti. Esegui backup:elenco per visualizzare l&#39;elenco dei file di backup.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).

#### `--force`, `-f`

Sovrascrivi i file esistenti durante il ripristino di un backup

- Predefinito: `false`
- Non accetta un valore

#### `--file`

Un percorso di ripristino file specifico

- Accetta un valore


## `build:generate`

```bash
ece-tools build:generate
```

Genera tutti i file necessari per la fase di build.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `build:transfer`

```bash
ece-tools build:transfer
```

Trasferisce i file generati nella directory iniziale.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `cloud:config:create`

```bash
ece-tools cloud:config:create <configuration>
```

Crea un file `.magento.env.yaml` con la configurazione specificata per le variabili di compilazione, distribuzione e post-distribuzione. Sovrascrive qualsiasi file `.magento,.env.yaml` esistente.

### Argomenti

#### `configuration`

Configurazione in formato JSON

- Obbligatorio

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `cloud:config:update`

```bash
ece-tools cloud:config:update <configuration>
```

Aggiorna il file `.magento.env.yaml` esistente con la configurazione specificata. Crea il file `.magento.env.yaml` se non esiste.

### Argomenti

#### `configuration`

Configurazione in formato JSON

- Obbligatorio

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `cloud:config:validate`

```bash
ece-tools cloud:config:validate
```

Convalida il file di configurazione `.magento.env.yaml`

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `config:dump`

```bash
ece-tools config:dumpdump
```

Configurazione del dump per la distribuzione di contenuti statici.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `cron:disable`

```bash
ece-tools cron:disable
```

Disattiva tutti i processi cron di Magento e interrompe tutti i processi in esecuzione.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `cron:enable`

```bash
ece-tools cron:enable
```

Abilita i processi cron di Magento.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `cron:kill`

```bash
ece-tools cron:kill
```

Termina tutti i processi cron del Magento.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `cron:unlock`

```bash
ece-tools cron:unlock [--job-code [JOB-CODE]]
```

Sblocca i processi cron bloccati nello stato &quot;in esecuzione&quot;.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).

#### `--job-code`

Cron codice processo da sbloccare.

- Predefinito: `[]`
- Accetta più valori


## `dev:generate:schema-error`

```bash
ece-tools dev:generate:schema-error
```

Genera il file dist/error-codes.md dal file schema.error.yaml.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `dev:git:update-composer`

```bash
ece-tools dev:git:update-composer
```

Aggiorna il compositore per la distribuzione da Git.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `env:config:show`

```bash
ece-tools env:config:show [<variable>...]
```

Visualizza le variabili dell’ambiente di configurazione cloud codificate.

### Argomenti

#### `variable`

Variabili di ambiente da visualizzare, opzioni possibili: servizi, percorsi, variabili

- Predefinito: `[]`
- Array

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `error:show`

```bash
ece-tools error:show [-j|--json] [--] [<error-code>]
```

Visualizza informazioni sull’errore per ID errore o informazioni su tutti gli errori dell’ultima distribuzione.

### Argomenti

#### `error-code`

Codice di errore, se non viene passato il comando visualizza informazioni su tutti gli errori dell’ultima distribuzione

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).

#### `--json`, `-j`

Utilizzato per ottenere risultati in formato JSON

- Predefinito: `false`
- Non accetta un valore


## `module:refresh`

```bash
ece-tools module:refresh
```

Aggiorna la configurazione per abilitare i moduli appena aggiunti.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `schema:generate`

```bash
ece-tools schema:generate
```

Genera il file *.dist dello schema.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `wizard:ideal-state`

```bash
ece-tools wizard:ideal-state
```

Verifica lo stato ideale della configurazione.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `wizard:master-slave`

```bash
ece-tools wizard:master-slave
```

Verifica la configurazione master-slave.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `wizard:scd-on-build`

```bash
ece-tools wizard:scd-on-build
```

Verifica SCD sulla configurazione della build.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `wizard:scd-on-demand`

```bash
ece-tools wizard:scd-on-demand
```

Verifica la configurazione di SCD su richiesta.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `wizard:scd-on-deploy`

```bash
ece-tools wizard:scd-on-deploy
```

Verifica SCD sulla configurazione di distribuzione.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).


## `wizard:split-db-state`

```bash
ece-tools wizard:split-db-state
```

Verifica la possibilità di suddividere il database e se il database è già stato suddiviso o meno.

### Opzioni

Per le opzioni globali, vedi [Opzioni globali](#global-options).
