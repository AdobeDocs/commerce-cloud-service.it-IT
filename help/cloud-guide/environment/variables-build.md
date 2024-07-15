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

Le seguenti variabili _build_ controllano le azioni nella fase di compilazione e possono ereditare ed eseguire l&#39;override dei valori dalle [variabili globali](variables-global.md). Inserisci queste variabili nella fase `build` del file `.magento.env.yaml`:

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

Impostare il livello di nidificazione delle directory per il salvataggio dei file del report degli errori per evitare di riempire la directory del report con decine di migliaia di file, rendendo difficile la gestione e la revisione dei dati. Impostazione predefinita: `1`. In genere, non è necessario modificare il valore predefinito a meno che non si verifichino problemi nella gestione dei file di segnalazione errori nella directory `<magento_root>/var/report/`.

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

Vedi [Applicare le patch](../development/apply-patches.md).

## `SCD_COMPRESSION_LEVEL`

- **Predefinito**—`6`
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Specifica il livello di compressione [gzip](https://www.gnu.org/software/gzip) (da `0` a `9`) da utilizzare durante la compressione del contenuto statico; `0` disabilita la compressione.

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

Imposta su `true` per impedire la generazione di contenuto statico per i temi principali durante la fase di compilazione.

Imposta `SCD_NO_PARENT: false` durante la fase di build in modo che la generazione di contenuto statico per i temi principali non influisca sulla distribuzione del sito o non provochi inutili tempi di inattività. Vedi [Distribuzione di contenuto statico](../deploy/static-content.md).

```yaml
stage:
  build:
    SCD_NO_PARENT: false
```

## `SCD_MATRIX`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

È possibile configurare più impostazioni internazionali per tema. Questa personalizzazione consente di velocizzare il processo di creazione riducendo il numero di file dei temi non necessari. Ad esempio, puoi creare il tema _magento/backend_ in inglese e un tema personalizzato in altre lingue.

Nell&#39;esempio seguente viene creato il tema `Magento/backend` con tre impostazioni internazionali:

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

In alternativa, puoi scegliere di _non_ distribuire un tema:

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

Personalizzare la [strategia di distribuzione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-strategy.html) per il contenuto statico. Vedere [Distribuire i file di visualizzazione statici](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

Utilizza queste opzioni _only_ se hai più di una lingua:

- `standard`: distribuisce tutti i file di visualizzazione statica per tutti i pacchetti.
- `quick`—(_default_) riduce al minimo i tempi di distribuzione.
- `compact`: consente di risparmiare spazio su disco nel server. In Adobe Commerce versione 2.2.4 e precedenti, questa impostazione sostituisce il valore per `scd_threads` con il valore di `1`.

```yaml
stage:
  build:
    SCD_STRATEGY: "compact"
```

## `SCD_THREADS`

- **Predefinito**—Automatico
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Imposta il numero di thread per la distribuzione del contenuto statico. Il valore predefinito è impostato in base al numero di thread della CPU rilevati e non supera il valore 4. L&#39;aumento del numero di thread velocizza la distribuzione dei contenuti statici; la riduzione del numero di thread ne rallenta la distribuzione. Puoi impostare il valore del thread, ad esempio:

```yaml
stage:
  build:
    SCD_THREADS: 2
```

Per ridurre ulteriormente i tempi di distribuzione, utilizzare [Gestione configurazione](../store/store-settings.md) con il comando `scd-dump` per spostare la distribuzione statica nella fase di compilazione.

## `SCD_USE_BALER`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.3.0 e versioni successive

[Baler](https://github.com/magento/baler) analizza il codice JavaScript generato e crea un bundle JavaScript ottimizzato. L’implementazione del bundle ottimizzato sul sito può ridurre il numero di richieste di rete durante il caricamento del sito e migliorare i tempi di caricamento delle pagine.

Impostare su `true` per eseguire Baler dopo l&#39;esecuzione della distribuzione del contenuto statico.

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

Impostare su `true` per ignorare il comando `composer dump-autoload` durante un&#39;installazione di Cloud Docker. Questa variabile è rilevante solo per i contenitori Cloud Docker con file system scrivibili. In questi casi, ignorando il comando si impedisce agli altri comandi di tentare di accedere al codice dalla directory `generated` eliminata.

Quando Adobe Commerce esegue `composer dump-autoload`, vengono creati file di caricamento automatico con collegamenti alle classi generate nella cartella `generated`. Ciò non costituisce un problema negli ambienti di produzione con file system di sola lettura. Tuttavia, per le installazioni di Cloud Docker con file system scrivibili (create solo per il test e lo sviluppo utilizzando `./vendor/bin/ece-docker build:compose --with-test`), è possibile eseguire il comando `bin/magento -n setup:upgrade` senza l&#39;opzione `--keep-generated`, che elimina la directory `generated`. Se la directory viene eliminata, il comando `composer dump-autoload` non riuscirà perché il caricamento automatico contiene collegamenti ai file nella directory eliminata.

```yaml
stage:
  build:
    SKIP_COMPOSER_DUMP_AUTOLOAD: true
```

## `SKIP_SCD`

- **Predefinito**— _Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Imposta su `true` per saltare la distribuzione del contenuto statico durante la fase di compilazione.

Se distribuisci già contenuto statico durante la fase di compilazione con [Gestione configurazione](../store/store-settings.md), puoi saltare la distribuzione di contenuto statico per un test di compilazione rapido.

Durante la fase di compilazione, impostare `SKIP_SCD: false` in modo che la compilazione del contenuto statico avvenga durante la fase di compilazione in cui il processo non influisce sulla distribuzione del sito o causa inutili tempi di inattività del sito. Vedi [Distribuzione di contenuto statico](../deploy/static-content.md).

```yaml
stage:
  build:
    SKIP_SCD: false
```

## `VERBOSE_COMMANDS`

- **Predefinito**—_Non impostato_
- **Versione**—Adobe Commerce 2.1.4 e versioni successive

Attiva o disattiva il livello di dettaglio di debug [Symfony](https://symfony.com/doc/current/console/verbosity.html) per i comandi CLI `bin/magento` eseguiti durante la fase di distribuzione.

>[!NOTE]
>
>Per utilizzare VERBOSE_COMMANDS per controllare i dettagli nell&#39;output del comando sia per i comandi CLI `bin/magento` riusciti che non riusciti, è necessario impostare [MIN_LOGGING_LEVEL](variables-global.md#minlogginglevel) `debug`.

Scegli il livello di dettaglio fornito nei registri:

- `-v`= output normale
- `-vv`= output più dettagliato
- `-vvv` = output dettagliato ideale per il debug

```yaml
stage:
  build:
    VERBOSE_COMMANDS: "-vv"
```
