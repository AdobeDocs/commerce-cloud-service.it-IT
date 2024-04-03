---
title: Gestione configurazione archivio
description: Scopri come gestire e sincronizzare le impostazioni di configurazione dell’archivio in tutti gli ambienti Adobe Commerce su infrastrutture cloud.
feature: Cloud, Configuration, SCD
exl-id: f2dd876d-24ee-4d47-b9ac-44fcf77b61b5
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 0%

---

# Gestione configurazione archivio

Le configurazioni predefinite per lo store sono memorizzate in un `config.xml` per il modulo appropriato. Quando si modificano le impostazioni in Commerce Admin o CLI `bin/magento config:set` , le modifiche si riflettono nel database di base, in particolare nel `core_config_data` tabella. Queste impostazioni sovrascrivono le configurazioni predefinite memorizzate in `config.xml` file.

Impostazioni archivio, che si riferiscono alle configurazioni nell’Admin **Negozi** > **Impostazioni** > **Configurazione** , sono archiviate nei file di configurazione della distribuzione in base al tipo di configurazione:

- `app/etc/config.php`: impostazioni di configurazione per store, siti Web, moduli o estensioni, ottimizzazione di file statici e valori di sistema correlati alla distribuzione di contenuti statici. Consulta la [riferimento config.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html) nel _Guida alla configurazione_.
- `app/etc/env.php`- valori per le sostituzioni specifiche del sistema e le impostazioni sensibili che devono _NOT_ essere memorizzato nel controllo del codice sorgente. Consulta la [riferimento env.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html) nel _Guida alla configurazione_.

>[!NOTE]
>
>Poiché Adobe Commerce su infrastruttura cloud supporta solo le modalità di produzione e manutenzione, il **Avanzate** > **Sviluppatore** non è accessibile in Admin. Devi avere [Privilegi di amministratore ambiente](../project/user-access.md) per completare le attività di gestione della configurazione. Puoi configurare altre impostazioni utilizzando [variabili di ambiente](../environment/configure-env-yaml.md).

La gestione della configurazione consente di implementare impostazioni di archiviazione coerenti negli ambienti con tempi di inattività minimi tramite l’implementazione della pipeline. Il progetto Adobe Commerce su infrastruttura cloud include il server di build, gli script di build e distribuzione e gli ambienti di distribuzione progettati con [strategia di implementazione della pipeline](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) in mente.

## Schema di sostituzione configurazione

Tutte le configurazioni di sistema vengono impostate durante le fasi di generazione e distribuzione in base al seguente schema di sostituzione:

1. Se esiste una variabile di ambiente, utilizza la configurazione personalizzata e ignora quella predefinita.
1. Se non esiste una variabile di ambiente, utilizza la configurazione di da un `MAGENTO_CLOUD_RELATIONSHIPS` coppia nome-valore nella [`.magento.app.yaml` file](../application/configure-app-yaml.md). Ignora la configurazione predefinita.
1. Se non esiste una variabile di ambiente e `MAGENTO_CLOUD_RELATIONSHIPS` non contiene una coppia nome-valore, rimuovi tutte le configurazioni personalizzate e utilizza i valori della configurazione predefinita.

Per riepilogare, le variabili di ambiente sovrascrivono tutti gli altri valori.

>[!TIP]
>
>Consulta [Gestione della configurazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) nel _Guida alla configurazione_ per ulteriori informazioni sullo schema di sostituzione per la distribuzione della pipeline.

Se la stessa impostazione è configurata in più posizioni, l’applicazione si basa sulla seguente gerarchia di configurazione per determinare quale valore applicare all’ambiente:

| Priorità | Configurazione<br>Metodo | Descrizione |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>variabili di ambiente | Valori aggiunti da _Variabili_ della configurazione dell’ambiente in [!DNL Cloud Console]. Specifica qui i valori per le configurazioni sensibili o specifiche per l’ambiente. Le impostazioni qui specificate non possono essere modificate dall&#39;amministratore. Consulta [Variabili di configurazione dell’ambiente](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | Valori aggiunti nel `variables` sezione del `.magento.app.yaml` file. Specifica qui i valori per garantire una configurazione coerente in tutti gli ambienti. **Non specificare valori sensibili nella `.magento.app.yaml` file.** Consulta [Impostazioni applicazione](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | I valori di configurazione specifici dell’ambiente memorizzati qui vengono aggiunti utilizzando `app:config:dump` comando. Imposta i valori sensibili e specifici del sistema utilizzando le variabili di ambiente o la CLI. Consulta [Dati sensibili](#sensitive-data). Il `env.php` il file è **non** incluso nel controllo del codice sorgente. |
| 4 | `app/etc/config.php` | I valori memorizzati qui vengono aggiunti utilizzando `app:config:dump` comando. I valori di configurazione condivisi vengono aggiunti a `config.php`. Imposta la configurazione condivisa dall’amministratore o utilizzando CLI. Il `config.php` file incluso nel controllo del codice sorgente. |
| 5 | Database | I valori memorizzati qui vengono aggiunti impostando le configurazioni in Admin. Le configurazioni impostate utilizzando uno dei metodi precedenti sono bloccate (disattivate) e non possono essere modificate dall’amministratore. |
| 6 | `config.xml` | In molte configurazioni i valori predefiniti sono impostati in `config.xml` per un modulo. Se Adobe Commerce non è in grado di trovare alcun valore impostato con uno dei metodi precedenti, viene utilizzato il valore predefinito, se impostato. |

{style="table-layout:auto"}

## Immagine configurazione

Puoi utilizzare quanto segue `ece-tools` per generare un `config.php` file contenente tutte le configurazioni di archivio correnti:

```bash
./vendor/bin/ece-tools config:dump
```

I dati &quot;scaricati&quot; in `app/etc/config.php` il file diventa _bloccato_, il che significa che il campo corrispondente nell’amministrazione di Commerce diventa **sola lettura**. Il `config.php` Il file include solo le impostazioni configurate. Non blocca i valori predefiniti. Il blocco solo dei valori aggiornati assicura inoltre che tutte le estensioni utilizzate negli ambienti di staging e produzione non si interrompano a causa di configurazioni di sola lettura, in particolare Fastly.

>[!WARNING]
>
>Il `ece-tools config:dump` Il comando non recupera le configurazioni dettagliate per i moduli, ad esempio B2B. Se hai bisogno di un dump di configurazione completo, utilizza `app:config:dump` ma questo comando blocca i valori di configurazione in uno stato di sola lettura.

### Dati sensibili

Tutte le configurazioni sensibili vengono esportate in `app/etc/env.php` quando si utilizza `bin/magento app:config:dump` comando. È possibile impostare valori sensibili utilizzando il comando CLI: `bin/magento config:sensitive:set`. Consulta  [Impostazioni sensibili e specifiche per l’ambiente](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) nel _Estensioni Commerce PHP_ guida per scoprire come designare le impostazioni di configurazione come sensibili o specifiche per il sistema.

Visualizza un elenco di [Impostazioni sensibili o specifiche del sistema](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html) nel _Guida alla configurazione_.

### Prestazioni SCD

A seconda delle dimensioni dell’archivio, è possibile che sia presente un numero elevato di file di contenuto statico da distribuire. Normalmente, il contenuto statico viene distribuito durante la fase di distribuzione quando l’applicazione è in modalità Manutenzione. La configurazione ottimale consiste nel generare contenuto statico durante la fase di build. Consulta [Scelta di una strategia di distribuzione](../deploy/static-content.md).

Se dopo aver scaricato le configurazioni hai abilitato la gestione della configurazione, devi spostare le variabili SCD_* dalla fase di distribuzione alla fase di build per abilitare correttamente la generazione di contenuto statico durante la fase di build. Consulta [Variabili di ambiente](../environment/configure-env-yaml.md#environment-variables).

**Prima della gestione della configurazione**:

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

**Dopo aver abilitato la gestione della configurazione**:

Sposta le variabili SCD_* nella fase di build:

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

>[!NOTE]
>
>Prima di distribuire i file statici, le fasi di build e distribuzione comprimono il contenuto statico utilizzando GZIP. La compressione dei file statici riduce i carichi del server e aumenta le prestazioni del sito. Consulta [opzioni di build](../environment/variables-build.md) informazioni sulla personalizzazione o la disattivazione della compressione dei file.

## Procedura per gestire le impostazioni

Di seguito viene illustrata una panoramica di alto livello di questo processo:

![Panoramica della gestione della configurazione Starter](../../assets/starter/configuration-management-flow.png)

**Per configurare l’archivio e generare un file di configurazione**:

1. Completa tutte le configurazioni per i negozi nell’Admin per uno degli ambienti:

   - Starter: un ramo di sviluppo attivo
   - Pro: un ramo attivo nell’ambiente di integrazione

   Queste configurazioni non includono i prodotti effettivi a meno che non si pianifichi il download del database da questo ambiente agli ambienti di staging e produzione. In genere, i database di sviluppo non includono i dati dell&#39;archivio completo.

1. Sulla workstation locale, passa alla directory del progetto.

1. Crea un dump locale del database remoto.

   ```bash
   magento-cloud db:dump
   ```

1. Aggiungere, eseguire il commit e inviare le modifiche al codice per aggiornare un ambiente remoto.

   ```bash
   git add app/etc/config.php
   ```

   ```bash
   git commit -m "Add system-specific configuration"
   ```

   ```bash
   git push origin <branch-name>
   ```

Al termine della distribuzione, accedi all’amministratore dell’ambiente aggiornato per verificare le impostazioni. Continua a unire eventuali configurazioni aggiuntive agli ambienti di staging e produzione, in base alle esigenze.

### Aggiorna configurazioni

Quando modifichi l’ambiente tramite l’amministratore ed esegui nuovamente il comando, al codice vengono aggiunte nuove configurazioni nel `config.php` file.

>[!WARNING]
>
>Mentre è possibile modificare manualmente il `config.php` negli ambienti di staging e produzione, è **non** consigliato. Il file consente di mantenere tutte le configurazioni coerenti in tutti gli ambienti. Non eliminare mai `config.php` per ricrearlo. L’eliminazione del file può rimuovere configurazioni e impostazioni specifiche necessarie per i processi di build e distribuzione.

### Ripristina file di configurazione

Copie dell’originale `app/etc/env.php` e `app/etc/config.php` I file sono stati creati durante il processo di distribuzione e archiviati nella stessa cartella. Di seguito sono riportati i file BAK (backup file) e PHP (file originali) nello stesso `app/etc` cartella:

```terminal
...
config.php.bak
di.xml
env.php.bak
vendor_path.php
config.php
db_schema.xml
env.php
...
```

Le configurazioni meno recenti utilizzavano `app/etc/config.local.php` file. Consulta [Migra configurazioni meno recenti](#migrate-older-configurations).

**Per ripristinare i file di configurazione**:

1. Sulla workstation locale, utilizzare SSH per accedere al progetto e all&#39;ambiente remoti.

   ```bash
   magento-cloud ssh
   ```

1. Verificare il percorso e la disponibilità dei file di backup.

   ```bash
   ./vendor/bin/ece-tools backup:list
   ```

   Risposta di esempio:

   ```terminal
   The list of backup files:
   app/etc/env.php
   app/etc/config.php
   ```

1. Ripristinare i file di backup.

   ```bash
   ./vendor/bin/ece-tools backup:restore
   ```

### Migra configurazioni meno recenti

Se esegui l’aggiornamento ad Adobe Commerce su infrastruttura cloud 2.2 o versione successiva, potrebbe essere utile migrare le impostazioni da `config.local.php` file nel nuovo `config.php` file. Se le impostazioni di configurazione nell’amministratore corrispondono al contenuto del file, segui le istruzioni per generare e aggiungere `config.php` file.

Se sono diverse, puoi aggiungere contenuto dalla sezione `config.local.php` file nel nuovo `config.php` file:

1. Segui le istruzioni per generare il `config.php` file.

1. Apri `config.php` ed eliminare l&#39;ultima riga.

1. Apri `config.local.php` e copiarne il contenuto.

1. Incolla il contenuto nel `config.php` file, salva e completa l’aggiunta a Git.

1. Distribuisci nei tuoi ambienti.

Puoi completare questa migrazione solo una volta. Dopo la migrazione, utilizza `config.php` file.

### Cambia impostazioni internazionali

È possibile modificare le impostazioni internazionali dello store senza seguire un complesso processo di importazione ed esportazione della configurazione. _se_ tu hai [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand) abilitato. Puoi aggiornare le impostazioni internazionali utilizzando l’amministratore.

È possibile aggiungere un’altra lingua all’ambiente di staging o produzione abilitando `SCD_ON_DEMAND` in un ramo di integrazione, genera un `config.php` file con le nuove informazioni sulle impostazioni internazionali e copia il file di configurazione nell&#39;ambiente di destinazione.

>[!WARNING]
>
>Questo processo **sovrascrive** la configurazione dell’archivio; effettua le seguenti operazioni solo se gli ambienti contengono gli stessi archivi.

1. Nell’ambiente di integrazione, abilita `SCD_ON_DEMAND` variabile utilizzando [`.magento.env.yaml` file](../environment/configure-env-yaml.md).

1. Aggiungi le lingue necessarie utilizzando il tuo amministratore.

1. Utilizzare SSH per accedere all’ambiente remoto e generare `/app/etc/config.php` file contenente tutte le impostazioni internazionali.

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. Copia il nuovo file di configurazione dall’ambiente di integrazione remota alla directory dell’ambiente locale.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Aggiungere, eseguire il commit e inviare le modifiche al codice per aggiornare un ambiente remoto.
