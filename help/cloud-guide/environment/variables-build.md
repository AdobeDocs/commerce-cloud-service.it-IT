---
title: Variabili di build
description: Consulta l’elenco delle variabili di ambiente che controllano le azioni nella fase di build di Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Configuration, Build, SCD, Upgrade
recommendations: noDisplay, catalog
role: Developer
exl-id: 243aaa45-a5ef-4ed2-8800-3d0f07bf3740
source-git-commit: 7b9c6a4cd17069c25455195bd8f273664b8a29eb
workflow-type: tm+mt
source-wordcount: '920'
ht-degree: 0%

---

# Variabili di build

I seguenti elementi _build_ Le variabili controllano le azioni nella fase di build e possono ereditare e sostituire i valori dalla [Variabili globali](variables-global.md). Inserisci queste variabili in `build` fase del `.magento.env.yaml` file:

```yaml
stage:
  build:
    BUILD_VARIABLE_NAME: value
```

Per ulteriori informazioni sulla personalizzazione del processo di compilazione e distribuzione:

- [Configurazione della distribuzione](configure-env-yaml.md)
- [Processo di distribuzione](../deploy/process.md)

Le seguenti variabili sono state rimosse nella versione v2.2:

- `skip_di_clearing`
- `skip_di_compilation`

## `ERROR_REPORT_DIR_NESTING_LEVEL`

- **Predefinito**—`1`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Impostare il livello di nidificazione delle directory per il salvataggio dei file del report degli errori per evitare di riempire la directory del report con decine di migliaia di file, rendendo difficile la gestione e la revisione dei dati. L&#39;impostazione predefinita è `1`. In genere, non è necessario modificare il valore predefinito a meno che non si verifichino problemi nella gestione dei file delle segnalazioni errori in `<magento_root>/var/report/` directory.

```yaml
stage:
  build:
    ERROR_REPORT_DIR_NESTING_LEVEL: 2
```

## `QUALITY_PATCHES`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Specificare un elenco di patch di qualità Adobe Commerce da applicare durante la distribuzione.

```yaml
stage:
  build:
    QUALITY_PATCHES: [ ]
```

Nell&#39;esempio seguente vengono specificate tre patch da applicare durante la distribuzione.

```yaml
stage:
  build:
    QUALITY_PATCHES:
      - MC-31387
      - MDVA-4567
      - MC-456345
```

Consulta [Applicare le patch](../development/apply-patches.md).

## `SCD_COMPRESSION_LEVEL`

- **Predefinito**—`6`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Specifica quale [gzip](https://www.gnu.org/software/gzip) livello di compressione (`0` a `9`) da utilizzare per la compressione di contenuto statico; `0` disabilita la compressione.

```yaml
stage:
  build:
    SCD_COMPRESSION_LEVEL: 4
```

## `SCD_COMPRESSION_TIMEOUT`

- **Predefinito**—`600`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Quando il tempo necessario per comprimere le risorse statiche supera il limite di timeout di compressione, il processo di distribuzione viene interrotto. Imposta il tempo massimo di esecuzione, in secondi, per il comando di compressione del contenuto statico.

```yaml
stage:
  build:
    SCD_COMPRESSION_TIMEOUT: 800
```

## `SCD_NO_PARENT`

- **Predefinito**—`false`
- **Versione**—Adobe Commerce 2.4.2 e versioni successive

Imposta su `true` per impedire la generazione di contenuto statico per i temi principali durante la fase di build.

Imposta `SCD_NO_PARENT: false` durante la fase di build, in modo che la generazione di contenuto statico per i temi principali non influisca sulla distribuzione del sito o non provochi inutili tempi di inattività. Consulta [Distribuzione di contenuti statici](../deploy/static-content.md).

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

È possibile configurare più impostazioni internazionali per tema. Questa personalizzazione consente di velocizzare il processo di creazione riducendo il numero di file dei temi non necessari. Ad esempio, puoi generare il _magento/backend_ tema in inglese e un tema personalizzato in altre lingue.

L&#39;esempio seguente crea `Magento/backend` tema con tre lingue:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

L’esempio seguente crea tre temi con tre impostazioni internazionali:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/blank":
        language:
          - en_US
          - fr_FR
          - af_ZA
      "Magento/luma":
        language:
          - en_US
          - fr_FR
          - af_ZA
```

Oppure puoi scegliere di _non_ distribuire un tema:

```yaml
stage:
  build:
    SCD_MATRIX:
      "Magento/backend": [ ]
```

## `SCD_MAX_EXECUTION_TIME`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Consente di aumentare il tempo massimo di esecuzione previsto per la distribuzione del contenuto statico.

Per impostazione predefinita, Adobe Commerce su infrastruttura cloud imposta l’esecuzione massima prevista su 900 secondi, ma in alcuni scenari potrebbe essere necessario più tempo per completare la distribuzione di contenuti statici per un progetto Cloud.

```yaml
stage:
  build:
    SCD_MAX_EXECUTION_TIME: 3600
```

{{scd-timing-warning}}

## `SCD_STRATEGY`

- **Predefinito**—`quick`
- **Versione**—Adobe Commerce 2.2.0 e versioni successive

Personalizzare [strategia di implementazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) per il contenuto statico. Consulta [Distribuire file di visualizzazione statica](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

Usa queste opzioni _solo_ se si dispone di più impostazioni locali:

- `standard`- distribuisce tutti i file di visualizzazione statica per tutti i pacchetti.
- `quick`—(_predefinito_) riduce al minimo i tempi di installazione.
- `compact`- consente di risparmiare spazio su disco sul server. In Adobe Commerce versione 2.2.4 e precedenti, questa impostazione sostituisce il valore per `scd_threads` con un valore di `1`.

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Predefinito**- Automatico
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Imposta il numero di thread per la distribuzione del contenuto statico. Il valore predefinito è impostato in base al numero di thread della CPU rilevati e non supera il valore 4. L&#39;aumento del numero di thread velocizza la distribuzione dei contenuti statici; la riduzione del numero di thread ne rallenta la distribuzione. Puoi impostare il valore del thread, ad esempio:

```yaml
stage:
  build:
    SCD_THREADS: 2
```

Per ridurre ulteriormente i tempi di installazione, utilizza [Gestione configurazione](../store/store-settings.md) con `scd-dump` per spostare la distribuzione statica nella fase di build.

## `SCD_USE_BALER`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.3.0 e versioni successive

[Baler](https://github.com/magento/baler) analizza il codice JavaScript generato e crea un bundle JavaScript ottimizzato. L’implementazione del bundle ottimizzato sul sito può ridurre il numero di richieste di rete durante il caricamento del sito e migliorare i tempi di caricamento delle pagine.

Imposta su `true` per eseguire Baler dopo l’esecuzione della distribuzione del contenuto statico.

```yaml
stage:
  build:
    SCD_USE_BALER: true
```

>[!NOTE]
>
>Poiché Baler è in fase di rilascio alfa, non è consigliabile utilizzarlo negli ambienti di produzione.

## `SKIP_COMPOSER_DUMP_AUTOLOAD`

- **Predefinito**— _Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Imposta su `true` per saltare `composer dump-autoload` durante un’installazione di Cloud Docker. Questa variabile è rilevante solo per i contenitori Cloud Docker con file system scrivibili. In questi casi, ignorando il comando si evitano errori da parte di altri comandi che tentano di accedere al codice dal comando eliminato `generated` directory.

Quando Adobe Commerce viene eseguito `composer dump-autoload`, crea file di caricamento automatico con collegamenti alle classi generate nel `generated` che non rappresenta un problema negli ambienti di produzione con file system di sola lettura. Tuttavia, per le installazioni di Cloud Docker con file system scrivibili (create solo per il test e lo sviluppo utilizzando `./vendor/bin/ece-docker build:compose --with-test`), è possibile eseguire il comando `bin/magento -n setup:upgrade` comando senza `--keep-generated` , che elimina il `generated` directory. Se la directory viene eliminata, `composer dump-autoload` il comando non riesce perché il caricamento automatico contiene collegamenti ai file nella directory eliminata.

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **Predefinito**— _Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Imposta su `true` per saltare la distribuzione di contenuti statici durante la fase di build.

Se distribuisci già contenuto statico durante la fase di build con [Gestione configurazione](../store/store-settings.md), puoi saltare la distribuzione del contenuto statico per un test di compilazione rapido.

Nella fase di build, imposta `SKIP_SCD: false` in modo che la generazione di contenuto statico avvenga durante la fase di generazione in cui il processo non influisce sulla distribuzione del sito o causa inutili tempi di inattività del sito. Consulta [Distribuzione di contenuti statici](../deploy/static-content.md).

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Attiva o disattiva la [Symfony](https://symfony.com/doc/current/console/verbosity.html) livello di dettaglio debug per `bin/magento` Comandi CLI eseguiti durante la fase di distribuzione.

>[!NOTE]
>
>Per utilizzare VERBOSE_COMMANDS per controllare i dettagli nell&#39;output dei comandi sia per operazioni riuscite che per operazioni non riuscite `bin/magento` CLI, è necessario impostare [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Scegli il livello di dettaglio fornito nei registri:

- `-v`= uscita normale
- `-vv`= output più dettagliato
- `-vvv` = output dettagliato ideale per il debug

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
