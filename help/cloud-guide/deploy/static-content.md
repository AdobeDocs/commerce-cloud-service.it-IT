---
title: Distribuzione di contenuti statici
description: Scopri le strategie per la distribuzione di contenuti statici, come immagini, script e CSS, su Adobe Commerce nei progetti di infrastruttura cloud.
feature: Cloud, Build, Deploy, SCD
exl-id: e128d0d5-1326-44e5-a822-009e11ba105f
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '707'
ht-degree: 0%

---

# Strategie di distribuzione dei contenuti statici

La distribuzione di contenuti statici (SCD, Static Content Deployment) ha un impatto significativo sul processo di distribuzione dello store, che dipende dalla quantità di contenuti da generare (immagini, script, CSS, video, temi, impostazioni internazionali e pagine web) e da quando generarli. Ad esempio, la strategia predefinita genera contenuto statico durante il [fase di distribuzione](process.md#deploy-phase-deploy-phase) quando il sito è in modalità manutenzione; tuttavia, questa strategia di distribuzione richiede tempo per scrivere il contenuto direttamente nel file montato `pub/static` directory. Sono disponibili diverse opzioni o strategie per migliorare i tempi di implementazione in base alle esigenze.

## Ottimizzare i contenuti JavaScript e HTML

Puoi utilizzare bundling e minimizzazione per creare contenuti JavaScript e HTML ottimizzati durante la distribuzione di contenuti statici.

### Minimizza contenuto

È possibile migliorare il tempo di caricamento SCD durante il processo di distribuzione se non si copiano i file di visualizzazione statica in `var/view_preprocessed` directory e genera _minimizzato_ HTML quando richiesto. Per attivarlo, imposta il [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skiphtmlminification) variabile di ambiente globale su `true` nel `.magento.env.yaml` file.

>[!NOTE]
>
>A partire dal `ece-tools` versione 2002.0.13, il valore predefinito per la variabile SKIP_HTML_MINIFICATION è impostato su `true`.

Puoi salvare **altro** tempi di distribuzione e spazio su disco riducendo il numero di file dei temi non necessari. Ad esempio, puoi distribuire `magento/backend` tema in inglese e un tema personalizzato in altre lingue. È possibile configurare queste impostazioni del tema con [SCD_MATRIX](../environment/variables-deploy.md#scdmatrix) variabile di ambiente.

## Scelta di una strategia di distribuzione

Le strategie di distribuzione variano a seconda che si scelga di generare contenuto statico durante il _build_ fase, la _distribuire_ fase, oppure _on-demand_. Come mostrato nel grafico seguente, la generazione di contenuto statico durante la fase di distribuzione è la scelta meno ottimale. Anche con Minified HTML, ogni file di contenuto deve essere copiato nel file montato `~/pub/static` , che può richiedere molto tempo. La generazione di contenuti statici su richiesta sembra la scelta ottimale. Tuttavia, se il file di contenuto non esiste nella cache, viene generato nel momento in cui viene richiesto e questo aggiunge il tempo di caricamento all’esperienza utente. Pertanto, la generazione di contenuto statico durante la fase di build è la più ottimale.

![Confronto caricamento SCD](../../assets/scd-load-times.png)

### Impostazione di SCD sulla build

La generazione di contenuto statico durante la fase di build con minimified HTML è la configurazione ottimale per [**tempo di inattività pari a zero** implementazioni](reduce-downtime.md), noto anche come **stato ideale**. Anziché copiare i file in un&#39;unità montata, crea un collegamento simbolico dalla `./init/pub/static` directory.

La generazione di contenuto statico richiede l’accesso a temi e impostazioni internazionali. Adobe Commerce memorizza i temi nel file system, che è accessibile durante la fase di build; tuttavia, Adobe Commerce memorizza le lingue nel database. Il database è _non_ disponibile durante la fase di build. Per generare il contenuto statico durante la fase di build, è necessario utilizzare `config:dump` comando in `ece-tools` per spostare le impostazioni locali nel file system. Legge le impostazioni locali e le salva nel `app/etc/config.php` file.

**Per configurare il progetto in modo da generare SCD durante la compilazione**:

1. Sulla workstation locale, passa alla directory del progetto.
1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Sposta le impostazioni locali nel file system, quindi aggiorna il [`config.php` file](../development/commerce-version.md#create-a-configphp-file).

1. Il `.magento.env.yaml` il file di configurazione deve contenere i seguenti valori:

   - [SKIP_HTML_MINIFICATION](../environment/variables-global.md#skip_html_minification) è `true`
   - [SKIP_SCD](../environment/variables-build.md#skip_scd) nella fase di build è `false`
   - [SCD_STRATEGY](../environment/variables-build.md#scd_strategy) è `compact`

1. Verifica la configurazione di [Hook post-distribuzione](../application/hooks-property.md) nel `.magento.app.yaml` file.

1. Verificare le impostazioni eseguendo il comando [Creazione guidata avanzata per lo stato ideale](smart-wizards.md).

   ```bash
   php ./vendor/bin/ece-tools wizard:ideal-state
   ```

### Impostazione di SCD su richiesta

La generazione di SCD on-demand è ottimale per un flusso di lavoro di sviluppo nell’ambiente di integrazione. Riduce i tempi di distribuzione in modo da poter rivedere rapidamente le implementazioni ed eseguire i test di integrazione. Abilita [SCD_ON_DEMAND](../environment/variables-global.md#scdondemand) variabile di ambiente nella fase globale della `.magento.env.yaml` file. La variabile SCD_ON_DEMAND sostituisce tutte le altre configurazioni correlate a SCD e cancella il contenuto esistente da `~/pub/static` directory.

Quando si utilizza la strategia SCD on-demand, è utile precaricare la cache con le pagine che si prevede di richiedere, ad esempio la home page. Aggiungi l’elenco delle pagine previste in [WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages) variabile di ambiente nella fase successiva alla distribuzione del `.magento.env.yaml` file.

>[!WARNING]
>
>Non utilizzare la strategia SCD on-demand nell’ambiente di produzione.

### Ignorare SCD

A volte puoi scegliere di saltare completamente la generazione del contenuto statico. È possibile impostare [SKIP_SCD](../environment/variables-build.md#skipscd) variabile di ambiente nella fase globale per ignorare altre configurazioni relative a SCD. Questo non influisce sul contenuto esistente nel `~/pub/static` directory.
