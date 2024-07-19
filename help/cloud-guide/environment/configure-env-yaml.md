---
title: Configurare l’ambiente
description: Scopri come configurare le azioni di build e distribuzione in tutti gli ambienti dell’infrastruttura cloud di Commerce, inclusi Pro Staging e Production, utilizzando le variabili di ambiente.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
exl-id: 66e257e2-1eca-4af5-9b56-01348341400b
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# Configurare le variabili di ambiente per la distribuzione

Il file `.magento.env.yaml` utilizza le variabili di ambiente per centralizzare la gestione delle azioni di generazione e distribuzione in tutti gli ambienti, inclusi gli ambienti Pro Staging e Production. Per configurare azioni univoche in ogni ambiente, devi modificare questo file in ogni ambiente.

>[!TIP]
>
>I file YAML fanno distinzione tra maiuscole e minuscole e non consentono tabulazioni. Fare attenzione a utilizzare un rientro coerente in tutto il file `.magento.env.yaml`, altrimenti la configurazione potrebbe non funzionare come previsto. Gli esempi nella documentazione e nel file di esempio utilizzano il rientro _two-space_. Utilizza il comando [ece-tools validate](#validate-configuration-file) per controllare la configurazione.

## Struttura del file

Il file `.magento.env.yaml` contiene due sezioni: `stage` e `log`. La sezione `stage` controlla le azioni che si verificano durante le fasi del [processo di distribuzione cloud](../deploy/process.md).

- `stage` - Utilizzare la sezione relativa all&#39;area di visualizzazione per definire determinate azioni per le seguenti fasi della distribuzione:
   - `global` - Controlla le azioni nelle fasi di compilazione, distribuzione e post-distribuzione. Puoi ignorare queste impostazioni nelle sezioni di build, distribuzione e post-distribuzione.
   - `build` - Controlla le azioni solo nella fase di compilazione. Se non specifichi impostazioni in questa sezione, la fase di build utilizza le impostazioni della sezione globale.
   - `deploy` - Controlla le azioni solo nella fase di distribuzione. Se non specifichi impostazioni in questa sezione, la fase di distribuzione utilizza le impostazioni della sezione globale.
   - `post-deploy`—Controlla le azioni _dopo_ la distribuzione dell&#39;applicazione e _dopo_ il contenitore inizia ad accettare le connessioni.
- `log` - Utilizzare la sezione del registro per configurare [notifiche](set-up-notifications.md), inclusi i tipi di notifica e il livello di dettaglio.
   - `slack` - Configura un messaggio da inviare a un bot di Slack.
   - `email` - Configura un messaggio e-mail da inviare a uno o più destinatari e-mail.
   - [gestori di log](log-handlers.md): configurare i messaggi delle applicazioni hardware e software inviati a un server di registrazione remoto.

### Variabili di ambiente

Il pacchetto `ece-tools` imposta i valori nel file `env.php` in base ai valori di [variabili cloud](variables-cloud.md), variabili impostate nel file [!DNL Cloud Console] e nel file di configurazione `.magento.env.yaml`. Le variabili di ambiente nel file `.magento.env.yaml` personalizzano l&#39;ambiente Cloud sovrascrivendo la configurazione di Commerce esistente. Se un valore predefinito è `Not Set`, il pacchetto `ece-tools` esegue l&#39;azione **NO** e utilizza il valore predefinito [!DNL Commerce] o il valore della configurazione MAGENTO_CLOUD_RELATIONSHIPS. Se è impostato il valore predefinito, il pacchetto `ece-tools` agisce per impostarlo.

Gli argomenti seguenti contengono definizioni dettagliate di tutte le variabili che è possibile utilizzare nel file `.magento.env.yaml`, ad esempio se è impostato o meno un valore predefinito:

- [Globale](variables-global.md): le variabili controllano le azioni in ogni fase: compilazione, distribuzione e post-distribuzione
- [Build](variables-build.md): le variabili controllano le azioni di compilazione
- [Distribuisci](variables-deploy.md): le azioni di distribuzione del controllo delle variabili
- [Post-distribuzione](variables-post-deploy.md): le variabili controllano le azioni dopo la distribuzione

### Crea file di configurazione da CLI

È possibile generare un file di configurazione `.magento.env.yaml` per un ambiente Cloud utilizzando i seguenti comandi `ece-tools`.

>Crea un file di configurazione

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>Aggiorna valori nel file di configurazione

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

Entrambi i comandi richiedono un singolo argomento: una matrice in formato JSON che specifica un valore per almeno una variabile di compilazione, distribuzione o post-distribuzione. Il comando seguente, ad esempio, imposta i valori per le variabili `SCD_THREADS` e `CLEAN_STATIC_FILES`:

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

E crea un file `.magento.env.yaml` con le seguenti impostazioni:

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

È possibile utilizzare il comando `cloud:config:update` per aggiornare il nuovo file. Il comando seguente, ad esempio, modifica il valore `SCD_THREADS` e aggiunge la configurazione `SCD_COMPRESSION_TIMEOUT`:

```bash
php vendor/bin/ece-tools cloud:config:update '{"stage":{"build":{"SCD_THREADS":3, "SCD_COMPRESSION_TIMEOUT":1000}}}'
```

Il file aggiornato contiene la seguente configurazione:

```yaml
stage:
  build:
    SCD_THREADS: 3
    SCD_COMPRESSION_TIMEOUT: 1000
  deploy:
    CLEAN_STATIC_FILES: false
```

### Convalida file di configurazione

Utilizzare il comando `ece-tools` seguente per convalidare il file di configurazione `.magento.env.yaml` prima di inviare le modifiche all&#39;ambiente cloud remoto.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

La seguente risposta di esempio fornisce un elenco di elementi da correggere:

```
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## Costanti PHP

È possibile utilizzare le costanti PHP nelle definizioni dei file `.magento.env.yaml` anziché i valori di codifica fissa. L&#39;esempio seguente definisce `driver_options` utilizzando una costante PHP:

```yaml
stage:
  deploy:
    DATABASE_CONFIGURATION:
      connection:
        default:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
        indexer:
          driver_options:
            !php/const:\PDO::MYSQL_ATTR_LOCAL_INFILE : 1
      _merge: true
```

>[!WARNING]
>
>L&#39;analisi costante non funziona quando si utilizza una versione del pacchetto `symfony/yaml` precedente alla 3.2.

## Gestione degli errori

Quando si verifica un errore a causa di un valore imprevisto nel file di configurazione `.magento.env.yaml`, viene visualizzato un messaggio di errore. Ad esempio, il seguente messaggio di errore presenta un elenco di modifiche suggerite per ogni elemento con un valore imprevisto, fornendo a volte opzioni valide:

```
- Environment configuration is not valid. Please correct .magento.env.yaml file with next suggestions:
  Item CRON_CONSUMERS_RUNNER is not supposed to be in stage build. Please move it to one of possible stages: global, deploy
  Item SKIP_SCD has unexpected type string. Please use one of next types: boolean
  Item VERBOSE_COMMANDS has unexpected type boolean. Please use one of next types: string
  Item SKIP_HTML_MINIFICATION has unexpected type string. Please use one of next types: boolean
  Item CRON_CONSUMERS_RUNNER has unexpected type boolean. Please use one of next types: array
  Item VAR_WARM_UP_PAGES is not allowed in configuration.
  Item WARM_UP_PAGES has unexpected type string. Please use one of next types: array
```

Apporta le correzioni, conferma e invia le modifiche. Se non viene visualizzato alcun messaggio di errore, le modifiche apportate al file di configurazione passano la convalida.

## Ottimizzazione della gestione della configurazione

Se dopo aver scaricato le configurazioni hai abilitato Gestione configurazione, devi spostare le variabili SCD_* dalla fase di distribuzione a quella di build. Consulta [Strategie di distribuzione del contenuto statico](../deploy/static-content.md).

>Prima della gestione della configurazione:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
    REDIS_USE_SLAVE_CONNECTION: 1
```

>Dopo aver abilitato Configuration Management, sposta le variabili SCD_* nella fase di build:

```yaml
  deploy:
    CRON_CONSUMERS_RUNNER:
      cron_run: true
      consumers: []
    REDIS_USE_SLAVE_CONNECTION: 1
  build:
    SCD_STRATEGY: compact
    SCD_MATRIX:
      ...
```
