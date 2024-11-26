---
title: Best practice di implementazione
description: Scopri le best practice per l’implementazione di Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Deploy, Best Practices
exl-id: bac3ca83-0eee-4fda-9a5c-a84ab25a837a
source-git-commit: 269681efb9925d78ffb608ecbef657be740b5531
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# Best practice di implementazione

Gli script di build e distribuzione vengono attivati quando si unisce il codice in un ambiente remoto. Questi script utilizzano i [file di configurazione](../environment/overview.md) dell&#39;ambiente e il codice dell&#39;applicazione per fornire all&#39;infrastruttura cloud i dati e i servizi appropriati. Inoltre, questi script vengono utilizzati per installare o aggiornare l’applicazione Adobe Commerce, i servizi di terze parti e le estensioni personalizzate nell’ambiente cloud.

Il processo di compilazione e distribuzione è leggermente diverso per ciascun piano:

- **Piani iniziali**: per l&#39;ambiente di integrazione, ogni ramo attivo crea e distribuisce in un ambiente completo per l&#39;accesso e il testing. Verifica completa del codice dopo l&#39;unione al ramo `staging`. Per avviare il sito, invia `staging` a `master` per la distribuzione nell&#39;ambiente di produzione. Si dispone dell&#39;accesso completo a tutti i rami tramite [!DNL Cloud Console] e i comandi CLI.

- **Pro plans**: per l&#39;ambiente di integrazione, ogni ramo attivo crea e distribuisce in un ambiente completo per l&#39;accesso e il testing. Unisci il codice al ramo `integration` prima di unire negli ambienti di staging e produzione. È possibile eseguire l&#39;unione negli ambienti di staging e produzione utilizzando [!DNL Cloud Console] o i comandi SSH e CLI `magento-cloud`.

## Tracciare il processo

È possibile tenere traccia delle azioni di compilazione e distribuzione in tempo reale utilizzando il terminale o i messaggi di stato [!DNL Cloud Console]—`in-progress`, `pending`, `success` o `failed`— visualizzati durante il processo di distribuzione. È possibile visualizzare i dettagli nei file di registro. Consulta [Visualizza registri](../test/log-locations.md).

Se utilizzi archivi GitHub esterni, il registro delle operazioni non viene visualizzato nella sessione GitHub. Tuttavia, puoi comunque seguire l&#39;attività nell&#39;interfaccia per l&#39;archivio esterno e [!DNL Cloud Console]. Consulta [Integrazioni](../integrations/overview.md).

>[!NOTE]
>
>Negli ambienti di integrazione non è possibile visualizzare i registri di distribuzione da [!DNL Cloud Console]. Questa funzione è disponibile solo per gli ambienti di produzione e staging. Tuttavia, puoi visualizzare i registri per ogni fase della distribuzione in qualsiasi ambiente utilizzando i registri [build e deploy](../test/log-locations.md#build-and-deploy-logs). Per informazioni sulla risoluzione dei problemi, vedere [Riferimento errore di distribuzione](../dev-tools/error-reference.md).

È possibile abilitare [Tracciare le distribuzioni con New Relic](../monitor/track-deployments.md) per monitorare gli eventi di distribuzione e analizzare le prestazioni tra le distribuzioni.

## Best practice per le build e la distribuzione

Esamina le best practice e le considerazioni seguenti per il processo di distribuzione:

- **Assicurarsi di eseguire la versione più recente del pacchetto `ece-tools`**

  Consulta le [note sulla versione per ECE-Tools](../release-notes/ece-tools-package.md).

- **Segui il processo di compilazione e distribuzione**

  Assicurati di disporre del codice corretto in ogni ambiente per evitare la sovrascrittura delle configurazioni durante l’unione del codice tra ambienti. Ad esempio, per applicare le modifiche alla configurazione a tutti gli ambienti, modifica e verifica le modifiche nell’ambiente locale prima di distribuirle nell’ambiente di integrazione remota. Quindi, distribuisci e verifica le modifiche nell’ambiente di staging prima di implementarle nell’ambiente di produzione. Quando si esegue l’unione da un ambiente all’altro, la distribuzione sovrascrive tutto il codice dell’ambiente, ad eccezione della configurazione e delle impostazioni specifiche dell’ambiente.

- **Utilizza le stesse variabili in ambienti diversi**

  I valori di queste variabili possono variare tra gli ambienti; tuttavia, in genere sono necessarie le stesse variabili in ogni ambiente. Consulta [Gestione della configurazione per le impostazioni dell&#39;archivio](../store/store-settings.md).

- **Mantenere sensibili i valori di configurazione e i dati nelle variabili specifiche dell&#39;ambiente**

  Questi valori includono le variabili specificate mediante Cloud CLI, [!DNL Cloud Console] o aggiunte al file `env.php`. Vedi [Livelli variabili](../environment/variable-levels.md).

- **Verifica che tutto il codice sia disponibile nel ramo dell&#39;ambiente**

  Il riferimento al codice da altri rami, ad esempio un ramo privato, può causare problemi durante il processo di compilazione e distribuzione. Ad esempio, se fai riferimento a un tema da un ramo privato, il tema non è accessibile e non può essere generato con il codice dell’applicazione.

- **Aggiungere estensioni, integrazioni e codice nei rami iterati**

  Apportare ed eseguire il test delle modifiche in locale, inviare il push a `integration`, quindi a `staging` e `production`. Verifica e risolvi i problemi in ogni ambiente prima di unire gli aggiornamenti all’ambiente successivo. Alcune estensioni e integrazioni devono essere abilitate e configurate in un ordine specifico a causa delle dipendenze. L’aggiunta e il testing in gruppi possono semplificare notevolmente il processo di generazione e distribuzione e aiutare a determinare dove si verificano i problemi.

- **Verificare le versioni e le relazioni del servizio e la possibilità di connettersi**

  Verifica i servizi disponibili per l’applicazione e assicurati di utilizzare la versione più recente e compatibile. Per le versioni consigliate, vedere [Relazioni di servizio](../services/services-yaml.md#service-relationships) e [Requisiti di sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) nella _Guida all&#39;installazione_.

- **Eseguire il test in locale e nell&#39;ambiente di integrazione prima di distribuire in staging e produzione**

  Identifica e correggi i problemi negli ambienti locali e di integrazione per evitare tempi di inattività prolungati durante l’implementazione negli ambienti di staging e produzione.

  >[!TIP]
  >
  >Sono disponibili [comandi Smart Wizard](../deploy/smart-wizards.md) che è possibile utilizzare per verificare che la configurazione del progetto cloud rispetti le best practice per la configurazione di compilazione e distribuzione, inclusa la strategia di distribuzione di contenuti statici (SCD).

- **Dopo aver completato il test negli ambienti locali e di integrazione, distribuire e testare nell&#39;ambiente di staging**

  Consulta [Test di staging e produzione](../test/staging-and-production.md).

- **Verifica la configurazione dell&#39;ambiente di produzione**

  Prima di implementare in produzione, completa le seguenti attività:

   - Assicurati di poter connettersi a tutti e tre i nodi nell&#39;ambiente di produzione utilizzando [SSH](../development/secure-connections.md).

   - Verificare che gli indicizzatori siano impostati su _Aggiorna secondo pianificazione_. Consulta [Modalità di indicizzazione](https://developer.adobe.com/commerce/php/development/components/indexing/) nella _Guida per gli sviluppatori dell&#39;estensione_.

   - Prepara l’ambiente aggiornando eventuali variabili specifiche dell’ambiente nel codice di produzione, verificando la disponibilità e la compatibilità del servizio ed apportando eventuali altre modifiche di configurazione richieste.

- **Monitorare il processo di distribuzione**

  Rivedi i messaggi di stato della distribuzione e attenua i problemi, se necessario. Controlla i [registri](../test/log-locations.md#) cloud per visualizzare i messaggi di registro dettagliati.

## Cinque fasi di sviluppo e implementazione dell&#39;integrazione

Le seguenti fasi si verificano nell’ambiente di sviluppo locale e nell’ambiente di integrazione. Per i piani Pro, il codice non viene distribuito negli ambienti di staging o produzione in queste fasi iniziali.

### Fase 1: convalida del codice e della configurazione

Quando si configura inizialmente un progetto, [il modello di infrastruttura cloud](https://github.com/magento/magento-cloud) fornisce una base per i file di codice. Questo archivio di codice è clonato nel progetto come ramo `master`.

- **Per Starter**—`master` il ramo è il tuo ambiente di produzione.
- **Per Pro**—`master` inizia come ramo di origine per l&#39;ambiente di integrazione.

Crea un ramo da `master` per il codice personalizzato, le estensioni e i moduli e le integrazioni di terze parti. Esiste un ambiente di integrazione remota per testare il codice nel cloud.

Quando si invia il codice dall’area di lavoro locale all’archivio remoto, viene completata una serie di controlli e convalide del codice prima dell’avvio della generazione e della distribuzione degli script. Il server Git integrato convalida ciò che stai inviando e apporta modifiche. Ad esempio, se si aggiunge un servizio OpenSearch, il server Git incorporato verifica che la topologia del cluster sia modificata di conseguenza.

Se si verifica un errore di sintassi in un file di configurazione, il server Git rifiuta il push. Vedere [Blocco di protezione](../development/protective-block.md).

Questa fase esegue anche `composer install` per recuperare le dipendenze.

### Fase 2: generazione

>[!NOTE]
>
>Durante la fase di build, il sito non è in modalità di manutenzione e se si verificano errori o problemi non viene disattivato. Genera solo il codice che è cambiato rispetto alla build precedente.

Questa fase crea la base di codice ed esegue gli hook nella sezione `build` di `.magento.app.yaml`. L&#39;hook di compilazione predefinito è il comando `php ./vendor/bin/ece-tools` ed esegue le operazioni seguenti:

- Applica patch in `vendor/magento/ece-patches` e patch facoltative specifiche per il progetto in `m2-hotfixes`
- Rigenera il codice e la configurazione [dell&#39;iniezione di dipendenza](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary) (ovvero la directory `generated/`, che include `generated/code` e `generated/metapackage`) utilizzando `bin/magento setup:di:compile`.
- Verifica se il file [`app/etc/config.php`](../store/store-settings.md) esiste nel codebase. Adobe Commerce genera automaticamente questo file se non lo rileva durante la fase di build e include un elenco di moduli ed estensioni. Se esiste, la fase di build continua come di consueto, comprime i file statici utilizzando GZIP e distribuisce, riducendo i tempi di inattività nella fase di distribuzione. Per informazioni sulla personalizzazione o la disattivazione della compressione dei file, consultare [opzioni di compilazione](../environment/variables-build.md).

>[!WARNING]
>
>A questo punto, il cluster non è stato creato, pertanto non tentare di connettersi a un database o presumere che sia presente un processo daemon attivo.

Dopo la compilazione dell&#39;applicazione, viene montato su un **file system di sola lettura**. È possibile configurare punti di montaggio specifici da leggere/scrivere. Non è possibile inviare l&#39;FTP al server e aggiungere moduli. È invece necessario aggiungere codice all&#39;archivio locale ed eseguire `git push`, che crea e distribuisce l&#39;ambiente. Per la struttura del progetto, vedere [Struttura della directory del progetto locale](../project/file-structure.md).

### Fase 3: Preparazione del lumacone

Il risultato della fase di compilazione è un file system di sola lettura denominato _slug_. Questa fase crea un archivio e colloca il lume in un luogo di archiviazione permanente. La prossima volta che invii il codice, se un servizio non è stato modificato, utilizza il tag dall’archivio.

- Rende più rapida la creazione di un&#39;integrazione continua riutilizzando il codice invariato
- Se il codice cambia, aggiorna il tag per la build successiva da riutilizzare
- Consente il ripristino istantaneo di una distribuzione, se necessario
- Include i file statici se il file `app/etc/config.php` esiste nel codebase

Lo slug include tutti i file e le cartelle **esclusi i seguenti** mount configurati in `magento.app.yaml`:

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### Fase 4: Distribuzione di segmenti e cluster

Le applicazioni e tutti i servizi [backend](https://experienceleague.adobe.com/en/docs/commerce-operations/implementation-playbook/glossary) eseguono il provisioning come segue:

- Monta ogni servizio in un contenitore, ad esempio server Web, OpenSearch, [!DNL RabbitMQ]
- Monta il file system di lettura/scrittura (montato su una griglia di archiviazione distribuita ad alta disponibilità)
- Configura la rete in modo che i servizi possano &quot;vedersi&quot; (e solo l&#39;uno con l&#39;altro)

>[!NOTE]
>
>Apporta le modifiche in un ramo Git al termine di tutte le attività di generazione e distribuzione e invia nuovamente. Tutti i file system dell&#39;ambiente sono _di sola lettura_. Un sistema di sola lettura garantisce distribuzioni deterministiche e migliora notevolmente la sicurezza del sito, poiché nessun processo può scrivere nel file system. Funziona anche per garantire che il codice sia identico negli ambienti di integrazione, staging e produzione.

### Fase 5: ganci di distribuzione

>[!NOTE]
>
>Questa fase mette l&#39;applicazione [!DNL Commerce] in modalità di manutenzione fino al completamento della distribuzione.

Nell’ultimo passaggio viene eseguito uno script di distribuzione che consente di rendere anonimi i dati negli ambienti di sviluppo, cancellare le cache ed eseguire query su strumenti di integrazione continua esterni. Quando questo script viene eseguito, puoi accedere a tutti i servizi dell’ambiente, ad esempio Redis.

Se il file `app/etc/config.php` non esiste nel codebase, i file statici vengono compressi utilizzando `gzip` e distribuiti durante questa fase, il che aumenta la durata della fase di distribuzione e della manutenzione del sito.

>[!NOTE]
>
>Per ulteriori informazioni sulla personalizzazione o la disattivazione della compressione dei file, vedere [distribuire variabili](../environment/variables-deploy.md).

Sono disponibili due hook di distribuzione. L&#39;hook `pre-deploy.php` completa la pulizia e il recupero necessari delle risorse e del codice generati nell&#39;hook di compilazione. L&#39;hook `php ./vendor/bin/ece-tools deploy` esegue una serie di comandi e script:

- Se Adobe Commerce è **non installato**, viene installato con `bin/magento setup:install`, aggiorna la configurazione di distribuzione, `app/etc/env.php` e il database per l&#39;ambiente specificato, ad esempio Redis e gli URL del sito Web. **Importante:** Quando hai completato la [Prima distribuzione](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/launch/overview.html) durante l&#39;installazione, Adobe Commerce è stato installato e distribuito in tutti gli ambienti.

- Se Adobe Commerce **è installato**, eseguire gli aggiornamenti necessari. Lo script di distribuzione esegue `bin/magento setup:upgrade` per aggiornare lo schema e i dati del database (necessario dopo gli aggiornamenti dell&#39;estensione o del codice di base) e aggiorna anche la configurazione di distribuzione, `app/etc/env.php` e il database per l&#39;ambiente. Infine, lo script di distribuzione cancella la cache di Adobe Commerce.

- Lo script genera facoltativamente contenuto Web statico utilizzando il comando `magento setup:static-content:deploy`.

- Utilizza gli ambiti (`-s` flag negli script di compilazione) con impostazione predefinita `quick` per la strategia di distribuzione del contenuto statico. È possibile personalizzare la strategia utilizzando la variabile di ambiente [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy). Per informazioni dettagliate su queste opzioni e funzionalità, vedere [Strategie di distribuzione file statici](../deploy/static-content.md) e il flag `-s` per [Distribuire file di visualizzazione statici](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

>[!NOTE]
>
>Lo script di distribuzione utilizza i valori definiti dai file di configurazione nella directory `.magento`, quindi elimina la directory e il relativo contenuto. Questo non influisce sull’ambiente di sviluppo locale.

### Post-distribuzione: configurare il routing

Durante l’esecuzione della distribuzione, il processo interrompe il traffico in ingresso nel punto di ingresso per 60 secondi e riconfigura il routing in modo che il traffico web arrivi al cluster appena creato.

La distribuzione corretta rimuove la modalità di manutenzione per consentire l&#39;accesso normale e crea file di backup (BAK) per i file di configurazione `app/etc/env.php` e `app/etc/config.php`.

Abilita la generazione di contenuto statico utilizzando la variabile `SCD_ON_DEMAND` e configura l&#39;hook [`post_deploy`](../application/hooks-property.md) in modo che cancelli la cache e precarichi (riscaldi) la cache _dopo_ che il contenitore inizia ad accettare connessioni e _durante_ il traffico normale in ingresso.

Per esaminare i registri di compilazione e distribuzione, vedere [Visualizza i registri](../test/log-locations.md#view-and-manage-logs).
