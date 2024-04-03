---
title: Configurare l’ambiente
description: Scopri come configurare le azioni di build e distribuzione in tutti gli ambienti dell’infrastruttura cloud di Commerce, inclusi Pro Staging e Production, utilizzando le variabili di ambiente.
feature: Cloud, Build, Configuration, Deploy, SCD
role: Developer
exl-id: 66e257e2-1eca-4af5-9b56-01348341400b
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '697'
ht-degree: 0%

---

# Configurare le variabili di ambiente per la distribuzione

Il `.magento.env.yaml` Il file utilizza le variabili di ambiente per centralizzare la gestione delle azioni di generazione e distribuzione in tutti gli ambienti, inclusi Pro Staging e Produzione. Per configurare azioni univoche in ogni ambiente, devi modificare questo file in ogni ambiente.

>[!TIP]
>
>I file YAML fanno distinzione tra maiuscole e minuscole e non consentono tabulazioni. Fare attenzione a utilizzare rientri coerenti in tutto il `.magento.env.yaml` o la configurazione potrebbe non funzionare come previsto. Gli esempi nella documentazione e nel file di esempio utilizzano _a due spazi_ rientro. Utilizza il [comando convalida strumenti ece](#validate-configuration-file) per verificare la configurazione.

## Struttura del file

Il `.magento.env.yaml` Il file contiene due sezioni: `stage` e `log`. Il `stage` La sezione controlla le azioni che si verificano durante le fasi della [Processo di distribuzione cloud](../deploy/process.md).

- `stage`- Utilizzate la sezione stadio (Stage) per definire alcune azioni per le seguenti fasi della distribuzione:
   - `global`- Controlla le azioni nelle fasi di generazione, distribuzione e post-distribuzione. Puoi ignorare queste impostazioni nelle sezioni di build, distribuzione e post-distribuzione.
   - `build`- Controlla le azioni solo nella fase di generazione. Se non specifichi impostazioni in questa sezione, la fase di build utilizza le impostazioni della sezione globale.
   - `deploy`- Controlla le azioni solo nella fase di distribuzione. Se non specifichi impostazioni in questa sezione, la fase di distribuzione utilizza le impostazioni della sezione globale.
   - `post-deploy`- Controlla le azioni _dopo_ distribuzione dell&#39;applicazione e _dopo_ il contenitore inizia ad accettare le connessioni.
- `log`- Utilizza la sezione del registro per configurare [notifiche](set-up-notifications.md), inclusi i tipi di notifica e il livello di dettaglio.
   - `slack`- Configura un messaggio da inviare a un bot di Slack.
   - `email`- Consente di configurare un messaggio e-mail da inviare a uno o più destinatari.
   - [gestori di registri](log-handlers.md)- Configurazione dei messaggi dell&#39;applicazione hardware e software inviati a un server di registrazione remoto.

### Variabili di ambiente

Il `ece-tools` il pacchetto imposta i valori in `env.php` file in base ai valori di [Variabili cloud](variables-cloud.md), le variabili impostate in [!DNL Cloud Console]e `.magento.env.yaml` file di configurazione. Le variabili di ambiente in `.magento.env.yaml` Personalizza l’ambiente Cloud ignorando la configurazione Commerce esistente. Se un valore predefinito è `Not Set`, quindi `ece-tools` pacchetti **NO** e utilizza [!DNL Commerce] predefinito o il valore della configurazione MAGENTO_CLOUD_RELATIONSHIPS. Se è impostato il valore predefinito, `ece-tools` il pacchetto agisce per impostare tale impostazione predefinita.

Gli argomenti seguenti contengono definizioni dettagliate di tutte le variabili che è possibile utilizzare in, ad esempio se è impostato o meno un valore predefinito `.magento.env.yaml` file:

- [Globale](variables-global.md)- Le variabili controllano le azioni in ogni fase: build, deploy e post-deploy
- [Genera](variables-build.md)- Le variabili controllano le azioni di generazione
- [Distribuisci](variables-deploy.md)- Azioni di distribuzione del controllo variabili
- [Post-distribuzione](variables-post-deploy.md)- le variabili controllano le azioni dopo la distribuzione

### Crea file di configurazione da CLI

Puoi generare un `.magento.env.yaml` file di configurazione per un ambiente Cloud utilizzando quanto segue `ece-tools` comandi.

>Crea un file di configurazione

```bash
php ./vendor/bin/ece-tools cloud:config:create `<configuration-json>`
```

>Aggiorna valori nel file di configurazione

```bash
php ./vendor/bin/ece-tools cloud:config:update `<configuration-json>`
```

Entrambi i comandi richiedono un singolo argomento: una matrice in formato JSON che specifica un valore per almeno una variabile di compilazione, distribuzione o post-distribuzione. Ad esempio, il comando seguente imposta i valori per `SCD_THREADS` e `CLEAN_STATIC_FILES` Variabili:

```bash
php vendor/bin/ece-tools cloud:config:create '{"stage":{"build":{"SCD_THREADS":5}, "deploy":{"CLEAN_STATIC_FILES":false}}}'
```

E crea un `.magento.env.yaml` file con le impostazioni seguenti:

```yaml
stage:
  build:
    SCD_THREADS: 5
  deploy:
    CLEAN_STATIC_FILES: false
```

È possibile utilizzare `cloud:config:update` per aggiornare il nuovo file. Ad esempio, il comando seguente modifica `SCD_THREADS` e aggiunge `SCD_COMPRESSION_TIMEOUT` configurazione:

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

Utilizza quanto segue `ece-tools` comando per convalidare `.magento.env.yaml` file di configurazione prima di inviare le modifiche all’ambiente cloud remoto.

```bash
php ./vendor/bin/ece-tools cloud:config:validate
```

La seguente risposta di esempio fornisce un elenco di elementi da correggere:

```terminal
Environment configuration is not valid. Correct the following items in your .magento.env.yaml file:
The SCD_THREADS variable contains an invalid value of type string. Use the following type: integer.
The SCD_STRATEGY variable contains an invalid value fast. Use one of the available value options: compact, quick, standard.
The NOT_EXIST_OPTION variable is not allowed in configuration.
```

## Costanti PHP

È possibile utilizzare le costanti PHP in `.magento.env.yaml` le definizioni dei file invece dei valori di codifica fissa. L&#39;esempio seguente definisce `driver_options` utilizzo di una costante PHP:

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
>L’analisi costante non funziona quando si utilizza una `symfony/yaml` versione del pacchetto precedente alla 3.2.

## Gestione degli errori

Quando si verifica un errore a causa di un valore imprevisto nel `.magento.env.yaml` file di configurazione, viene visualizzato un messaggio di errore. Ad esempio, il seguente messaggio di errore presenta un elenco di modifiche suggerite per ogni elemento con un valore imprevisto, fornendo a volte opzioni valide:

```terminal
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

Se dopo aver scaricato le configurazioni hai abilitato Gestione configurazione, devi spostare le variabili SCD_* dalla fase di distribuzione a quella di build. Consulta [Strategie di distribuzione dei contenuti statici](../deploy/static-content.md).

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
