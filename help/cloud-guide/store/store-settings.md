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

Le configurazioni predefinite per l&#39;archivio sono archiviate in `config.xml` per il modulo appropriato. Quando si modificano le impostazioni nel comando Commerce Admin o CLI `bin/magento config:set`, le modifiche vengono applicate al database di base, in particolare alla tabella `core_config_data`. Queste impostazioni sovrascrivono le configurazioni predefinite memorizzate nel file `config.xml`.

Le impostazioni dell&#39;archivio, che fanno riferimento alle configurazioni nella sezione **Archivi** > **Impostazioni** > **Configurazione** dell&#39;amministratore, vengono memorizzate nei file di configurazione della distribuzione in base al tipo di configurazione:

- `app/etc/config.php`: impostazioni di configurazione per archivi, siti Web, moduli o estensioni, ottimizzazione di file statici e valori di sistema correlati alla distribuzione di contenuti statici. Vedere il riferimento a [config.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-configphp.html) nella _Guida alla configurazione_.
- `app/etc/env.php`—valori per le sostituzioni specifiche del sistema e le impostazioni sensibili che devono essere _NOT_ archiviate nel controllo del codice sorgente. Vedere il riferimento a [env.php](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/files/config-reference-envphp.html) nella _Guida alla configurazione_.

>[!NOTE]
>
>Poiché l&#39;infrastruttura cloud di Adobe Commerce supporta solo le modalità di produzione e manutenzione, la sezione **Avanzate** > **Sviluppatori** non è accessibile in Amministrazione. Per completare le attività di gestione della configurazione è necessario disporre di [privilegi di amministratore dell&#39;ambiente](../project/user-access.md). È possibile configurare impostazioni aggiuntive utilizzando [variabili di ambiente](../environment/configure-env-yaml.md).

La gestione della configurazione consente di implementare impostazioni di archiviazione coerenti negli ambienti con tempi di inattività minimi tramite l’implementazione della pipeline. Il progetto Adobe Commerce on cloud infrastructure include il server di compilazione, gli script di compilazione e distribuzione e gli ambienti di distribuzione progettati tenendo presente la [strategia di distribuzione della pipeline](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html).

## Schema di sostituzione configurazione

Tutte le configurazioni di sistema vengono impostate durante le fasi di generazione e distribuzione in base al seguente schema di sostituzione:

1. Se esiste una variabile di ambiente, utilizza la configurazione personalizzata e ignora quella predefinita.
1. Se non esiste una variabile di ambiente, utilizzare la configurazione di una coppia nome-valore `MAGENTO_CLOUD_RELATIONSHIPS` nel file [`.magento.app.yaml`](../application/configure-app-yaml.md). Ignora la configurazione predefinita.
1. Se una variabile di ambiente non esiste e `MAGENTO_CLOUD_RELATIONSHIPS` non contiene una coppia nome-valore, rimuovere tutte le configurazioni personalizzate e utilizzare i valori dalla configurazione predefinita.

Per riepilogare, le variabili di ambiente sovrascrivono tutti gli altri valori.

>[!TIP]
>
>Per ulteriori informazioni sullo schema di sostituzione per la distribuzione della pipeline, vedere [Gestione della configurazione](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/deployment/technical-details.html) nella _Guida alla configurazione_.

Se la stessa impostazione è configurata in più posizioni, l’applicazione si basa sulla seguente gerarchia di configurazione per determinare quale valore applicare all’ambiente:

| Priorità | Configuration<br>Method | Descrizione |
| -------- | ------------------------ | ----------- |
| 1 | [!DNL Cloud Console]<br>variabili di ambiente | Valori aggiunti dalla scheda _Variabili_ della configurazione dell&#39;ambiente in [!DNL Cloud Console]. Specifica qui i valori per le configurazioni sensibili o specifiche per l’ambiente. Le impostazioni qui specificate non possono essere modificate dall&#39;amministratore. Vedi [Variabili di configurazione dell&#39;ambiente](../project/overview.md#configure-environment). |
| 2 | `.magento.app.yaml` | Valori aggiunti nella sezione `variables` del file `.magento.app.yaml`. Specifica qui i valori per garantire una configurazione coerente in tutti gli ambienti. **Non specificare valori sensibili nel file `.magento.app.yaml`.** Vedere [Impostazioni applicazione](../application/configure-app-yaml.md). |
| 3 | `app/etc/env.php` | I valori di configurazione specifici dell&#39;ambiente memorizzati qui vengono aggiunti utilizzando il comando `app:config:dump`. Imposta i valori sensibili e specifici del sistema utilizzando le variabili di ambiente o la CLI. Vedi [Dati sensibili](#sensitive-data). Il file `env.php` è **not** incluso nel controllo del codice sorgente. |
| 4 | `app/etc/config.php` | I valori memorizzati qui vengono aggiunti utilizzando il comando `app:config:dump`. I valori della configurazione condivisa sono stati aggiunti a `config.php`. Imposta la configurazione condivisa dall’amministratore o utilizzando CLI. File `config.php` incluso nel controllo del codice sorgente. |
| 5 | Database | I valori memorizzati qui vengono aggiunti impostando le configurazioni in Admin. Le configurazioni impostate utilizzando uno dei metodi precedenti sono bloccate (disattivate) e non possono essere modificate dall’amministratore. |
| 6 | `config.xml` | Molte configurazioni hanno valori predefiniti impostati nel file `config.xml` per un modulo. Se Adobe Commerce non è in grado di trovare alcun valore impostato con uno dei metodi precedenti, viene utilizzato il valore predefinito, se impostato. |

{style="table-layout:auto"}

## Immagine configurazione

È possibile utilizzare il comando `ece-tools` seguente per generare un file `config.php` contenente tutte le configurazioni dell&#39;archivio correnti:

```bash
./vendor/bin/ece-tools config:dump
```

I dati &quot;scaricati&quot; nel file `app/etc/config.php` diventano _bloccati_, il che significa che il campo corrispondente nell&#39;amministratore di Commerce diventa **di sola lettura**. Il file `config.php` include solo le impostazioni configurate. Non blocca i valori predefiniti. Il blocco solo dei valori aggiornati assicura inoltre che tutte le estensioni utilizzate negli ambienti di staging e produzione non si interrompano a causa di configurazioni di sola lettura, in particolare Fastly.

>[!WARNING]
>
>Il comando `ece-tools config:dump` non recupera le configurazioni dettagliate per i moduli, ad esempio B2B. Se è necessario un dump di configurazione completo, utilizzare il comando `app:config:dump`, ma questo comando blocca i valori di configurazione in uno stato di sola lettura.

### Dati sensibili

Tutte le configurazioni sensibili vengono esportate nel file `app/etc/env.php` quando si utilizza il comando `bin/magento app:config:dump`. È possibile impostare valori sensibili utilizzando il comando CLI: `bin/magento config:sensitive:set`. Consulta [Impostazioni sensibili e specifiche dell&#39;ambiente](https://developer.adobe.com/commerce/php/development/configuration/sensitive-environment-settings/) nella _Guida delle estensioni Commerce PHP_ per scoprire come definire le impostazioni di configurazione sensibili o specifiche del sistema.

Vedere un elenco di [impostazioni sensibili o specifiche del sistema](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/paths/config-reference-sens.html) nella _Guida alla configurazione_.

### Prestazioni SCD

A seconda delle dimensioni dell’archivio, è possibile che sia presente un numero elevato di file di contenuto statico da distribuire. Normalmente, il contenuto statico viene distribuito durante la fase di distribuzione quando l’applicazione è in modalità Manutenzione. La configurazione ottimale consiste nel generare contenuto statico durante la fase di build. Vedere [Scelta di una strategia di distribuzione](../deploy/static-content.md).

Se dopo aver scaricato le configurazioni hai abilitato la gestione della configurazione, devi spostare le variabili SCD_* dalla fase di distribuzione alla fase di build per abilitare correttamente la generazione di contenuto statico durante la fase di build. Vedi [Variabili di ambiente](../environment/configure-env-yaml.md#environment-variables).

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

**Dopo aver abilitato Gestione configurazione**:

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
>Prima di distribuire i file statici, le fasi di build e distribuzione comprimono il contenuto statico utilizzando GZIP. La compressione dei file statici riduce i carichi del server e aumenta le prestazioni del sito. Consulta [opzioni di compilazione](../environment/variables-build.md) per informazioni sulla personalizzazione o la disattivazione della compressione dei file.

## Procedura per gestire le impostazioni

Di seguito viene illustrata una panoramica di alto livello di questo processo:

![Panoramica sulla gestione della configurazione Starter](../../assets/starter/configuration-management-flow.png)

**Per configurare l&#39;archivio e generare un file di configurazione**:

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

Quando modifichi l&#39;ambiente tramite l&#39;amministratore ed esegui nuovamente il comando, le nuove configurazioni vengono aggiunte al codice nel file `config.php`.

>[!WARNING]
>
>Anche se è possibile modificare manualmente il file `config.php` negli ambienti di staging e produzione, è **non** consigliato. Il file consente di mantenere tutte le configurazioni coerenti in tutti gli ambienti. Non eliminare mai il file `config.php` per ricrearlo. L’eliminazione del file può rimuovere configurazioni e impostazioni specifiche necessarie per i processi di build e distribuzione.

### Ripristina file di configurazione

Copie dei file `app/etc/env.php` e `app/etc/config.php` originali sono state create durante il processo di distribuzione e archiviate nella stessa cartella. Di seguito sono riportati i file BAK (backup file) e PHP (file originali) nella stessa cartella `app/etc`:

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

Le configurazioni meno recenti utilizzavano il file `app/etc/config.local.php`. Consulta [Eseguire la migrazione di configurazioni precedenti](#migrate-older-configurations).

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

Se si esegue l&#39;aggiornamento ad Adobe Commerce su infrastruttura cloud 2.2 o versione successiva, è possibile migrare le impostazioni dal file `config.local.php` al nuovo file `config.php`. Se le impostazioni di configurazione dell&#39;amministratore corrispondono al contenuto del file, seguire le istruzioni per generare e aggiungere il file `config.php`.

In caso di differenze, è possibile aggiungere il contenuto del file `config.local.php` al nuovo file `config.php`:

1. Seguire le istruzioni per generare il file `config.php`.

1. Aprire il file `config.php` ed eliminare l&#39;ultima riga.

1. Aprire il file `config.local.php` e copiare il contenuto.

1. Incollare il contenuto nel file `config.php`, salvarlo e completare l&#39;aggiunta a Git.

1. Distribuisci nei tuoi ambienti.

Puoi completare questa migrazione solo una volta. Dopo la migrazione, utilizzare il file `config.php`.

### Cambia impostazioni internazionali

È possibile modificare le impostazioni internazionali dell&#39;archivio senza seguire un processo di importazione ed esportazione di configurazione complesso, _se_ hai abilitato [SCD_ON_DEMAND](../environment/variables-global.md#scd_on_demand). Puoi aggiornare le impostazioni internazionali utilizzando l’amministratore.

È possibile aggiungere un&#39;altra lingua all&#39;ambiente di staging o produzione abilitando `SCD_ON_DEMAND` in un ramo di integrazione, generare un file `config.php` aggiornato con le nuove informazioni sulle impostazioni internazionali e copiare il file di configurazione nell&#39;ambiente di destinazione.

>[!WARNING]
>
>Questo processo **sovrascrive** la configurazione dell&#39;archivio; eseguire le operazioni seguenti solo se gli ambienti contengono gli stessi archivi.

1. Nell&#39;ambiente di integrazione, abilitare la variabile `SCD_ON_DEMAND` utilizzando il file [`.magento.env.yaml`](../environment/configure-env-yaml.md).

1. Aggiungi le lingue necessarie utilizzando il tuo amministratore.

1. Utilizzare SSH per accedere all&#39;ambiente remoto e generare il file `/app/etc/config.php` contenente tutte le impostazioni internazionali.

   ```bash
   ssh <SSH-URL> "./vendor/bin/ece-tools config:dump"
   ```

1. Copia il nuovo file di configurazione dall’ambiente di integrazione remota alla directory dell’ambiente locale.

   ```bash
   rsync <SSH-URL>:app/etc/config.php ./app/etc/config.php
   ```

1. Aggiungere, eseguire il commit e inviare le modifiche al codice per aggiornare un ambiente remoto.
