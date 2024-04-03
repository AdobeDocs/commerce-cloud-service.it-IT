---
title: Best practice di implementazione
description: Scopri le best practice per l’implementazione di Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Deploy, Best Practices
exl-id: bac3ca83-0eee-4fda-9a5c-a84ab25a837a
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '1904'
ht-degree: 0%

---

# Best practice di implementazione

Gli script di build e distribuzione vengono attivati quando si unisce il codice in un ambiente remoto. Questi script utilizzano l’ambiente [file di configurazione](../environment/overview.md) e il codice dell&#39;applicazione per fornire all&#39;infrastruttura cloud i dati e i servizi appropriati. Inoltre, questi script vengono utilizzati per installare o aggiornare l’applicazione Adobe Commerce, i servizi di terze parti e le estensioni personalizzate nell’ambiente cloud.

Il processo di compilazione e distribuzione è leggermente diverso per ciascun piano:

- **Piani iniziali**- Per l&#39;ambiente di integrazione, ogni ramo attivo crea e distribuisce in un ambiente completo per l&#39;accesso e il testing. Verifica completa del codice dopo l’unione in `staging` filiale. Per avviare il sito, premi `staging` a `master` da implementare nell’ambiente di produzione. Accesso completo a tutti i rami tramite [!DNL Cloud Console] e i comandi CLI.

- **Piani Pro**- Per l&#39;ambiente di integrazione, ogni ramo attivo crea e distribuisce in un ambiente completo per l&#39;accesso e il testing. Unisci il codice a `integration` prima dell’unione negli ambienti di staging e produzione. Puoi unire negli ambienti di staging e produzione utilizzando [!DNL Cloud Console] o utilizzando SSH e `magento-cloud` Comandi CLI.

## Tracciare il processo

Puoi tenere traccia delle azioni di build e distribuzione in tempo reale utilizzando il terminale o il [!DNL Cloud Console] Messaggi di stato—`in-progress`, `pending`, `success`, o `failed`- durante il processo di distribuzione. È possibile visualizzare i dettagli nei file di registro. Consulta [Visualizza registri](../test/log-locations.md).

Se utilizzi archivi GitHub esterni, il registro delle operazioni non viene visualizzato nella sessione GitHub. Tuttavia, puoi comunque seguire l’attività nell’interfaccia per l’archivio esterno e il [!DNL Cloud Console]. Consulta [Integrazioni](../integrations/overview.md).

>[!NOTE]
>
>Negli ambienti di integrazione, non è possibile visualizzare i registri di distribuzione da [!DNL Cloud Console]. Questa funzione è disponibile solo per gli ambienti di produzione e staging. Tuttavia, puoi visualizzare i registri di ogni fase della distribuzione in qualsiasi ambiente utilizzando [generare e distribuire](../test/log-locations.md#build-and-deploy-logs) log. Per informazioni sulla risoluzione dei problemi, vedere [Riferimento errore di distribuzione](../dev-tools/error-reference.md).

È possibile abilitare [Tracciare le implementazioni con New Relic](../monitor/track-deployments.md) per monitorare gli eventi di distribuzione e analizzare le prestazioni tra le distribuzioni.

## Best practice per le build e la distribuzione

Esamina le best practice e le considerazioni seguenti per il processo di distribuzione:

- **Assicurati di eseguire la versione più recente di `ece-tools` pacchetto**

  Consulta [Note sulla versione per gli strumenti ECE](../release-notes/ece-tools-package.md).

- **Segui il processo di compilazione e distribuzione**

  Assicurati di disporre del codice corretto in ogni ambiente per evitare la sovrascrittura delle configurazioni durante l’unione del codice tra ambienti. Ad esempio, per applicare le modifiche alla configurazione a tutti gli ambienti, modifica e verifica le modifiche nell’ambiente locale prima di distribuirle nell’ambiente di integrazione remota. Quindi, distribuisci e verifica le modifiche nell’ambiente di staging prima di implementarle nell’ambiente di produzione. Quando si esegue l’unione da un ambiente all’altro, la distribuzione sovrascrive tutto il codice dell’ambiente, ad eccezione della configurazione e delle impostazioni specifiche dell’ambiente.

- **Utilizzare le stesse variabili in ambienti diversi**

  I valori di queste variabili possono variare tra gli ambienti; tuttavia, in genere sono necessarie le stesse variabili in ogni ambiente. Consulta [Gestione della configurazione per le impostazioni dello store](../store/store-settings.md).

- **Mantenere sensibili i valori di configurazione e i dati nelle variabili specifiche dell’ambiente**

  Questi valori includono le variabili specificate utilizzando Cloud CLI, il [!DNL Cloud Console], o aggiunti al `env.php` file. Consulta [Livelli variabili](../environment/variable-levels.md).

- **Assicurati che tutto il codice sia disponibile nel ramo dell’ambiente**

  Il riferimento al codice da altri rami, ad esempio un ramo privato, può causare problemi durante il processo di compilazione e distribuzione. Ad esempio, se fai riferimento a un tema da un ramo privato, il tema non è accessibile e non può essere generato con il codice dell’applicazione.

- **Aggiungere estensioni, integrazioni e codice nei rami iterati**

  Apporta e verifica le modifiche localmente, invia a `integration`, quindi a `staging` e `production`. Verifica e risolvi i problemi in ogni ambiente prima di unire gli aggiornamenti all’ambiente successivo. Alcune estensioni e integrazioni devono essere abilitate e configurate in un ordine specifico a causa delle dipendenze. L’aggiunta e il testing in gruppi possono semplificare notevolmente il processo di generazione e distribuzione e aiutare a determinare dove si verificano i problemi.

- **Verificare le versioni e le relazioni del servizio e la capacità di connessione**

  Verifica i servizi disponibili per l’applicazione e assicurati di utilizzare la versione più recente e compatibile. Consulta [Relazioni di servizio](../services/services-yaml.md#service-relationships) e [Requisiti di sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) nel _Guida all’installazione_ versioni consigliate.

- **Eseguire test a livello locale e nell&#39;ambiente di integrazione prima di implementare in staging e produzione**

  Identifica e correggi i problemi negli ambienti locali e di integrazione per evitare tempi di inattività prolungati durante l’implementazione negli ambienti di staging e produzione.

  >[!TIP]
  >
  >Ci sono [creazione guidata avanzata](../deploy/smart-wizards.md) comandi utilizzabili per verificare che la configurazione del progetto cloud segua le best practice per la configurazione della build e della distribuzione, inclusa la strategia di distribuzione di contenuti statici (SCD).

- **Dopo aver completato il test in ambienti locali e di integrazione, implementa e testa nell’ambiente di staging**

  Consulta [Test di staging e produzione](../test/staging-and-production.md).

- **Verifica la configurazione dell’ambiente di produzione**

  Prima di implementare in produzione, completa le seguenti attività:

   - Assicurati di poter connettersi a tutti e tre i nodi nell’ambiente di produzione utilizzando [SSH](../development/secure-connections.md).

   - Verificare che gli indicizzatori siano impostati su _Aggiorna secondo programma_. Consulta [Modalità di indicizzazione](https://developer.adobe.com/commerce/php/development/components/indexing/) nel _Guida per gli sviluppatori di estensioni_.

   - Prepara l’ambiente aggiornando eventuali variabili specifiche dell’ambiente nel codice di produzione, verificando la disponibilità e la compatibilità del servizio ed apportando eventuali altre modifiche di configurazione richieste.

- **Monitorare il processo di distribuzione**

  Rivedi i messaggi di stato della distribuzione e attenua i problemi, se necessario. Rivedi il cloud [registri](../test/log-locations.md#) per messaggi di registro dettagliati.

## Cinque fasi di sviluppo e implementazione dell&#39;integrazione

Le seguenti fasi si verificano nell’ambiente di sviluppo locale e nell’ambiente di integrazione. Per i piani Pro, il codice non viene distribuito negli ambienti di staging o produzione in queste fasi iniziali.

### Fase 1: convalida del codice e della configurazione

Quando hai impostato un progetto iniziale, [il modello di infrastruttura cloud](https://github.com/magento/magento-cloud) fornisce una base per i file di codice. Questo archivio di codice viene clonato nel progetto come `master` filiale.

- **Per iniziare**—`master` ramo è l&#39;ambiente di produzione.
- **Per Pro**—`master` inizia come ramo di origine per l’ambiente di integrazione.

Crea un ramo da `master` per codice personalizzato, estensioni e moduli e integrazioni di terze parti. Esiste un ambiente di integrazione remota per testare il codice nel cloud.

Quando si invia il codice dall’area di lavoro locale all’archivio remoto, viene completata una serie di controlli e convalide del codice prima dell’avvio della generazione e della distribuzione degli script. Il server Git integrato convalida ciò che stai inviando e apporta modifiche. Ad esempio, se si aggiunge un servizio OpenSearch, il server Git incorporato verifica che la topologia del cluster sia modificata di conseguenza.

Se si verifica un errore di sintassi in un file di configurazione, il server Git rifiuta il push. Consulta [Blocco di protezione](../development/protective-block.md).

Anche questa fase viene eseguita `composer install` per recuperare le dipendenze.

### Fase 2: generazione

>[!NOTE]
>
>Durante la fase di build, il sito non è in modalità di manutenzione e se si verificano errori o problemi non viene disattivato. Genera solo il codice che è cambiato rispetto alla build precedente.

Questa fase crea la base di codice ed esegue gli hook nel `build` sezione di `.magento.app.yaml`. L’hook di build predefinito è `php ./vendor/bin/ece-tools` ed esegue le operazioni seguenti:

- Applica le patch in `vendor/magento/ece-patches`, e opzionali, patch specifiche per il progetto in `m2-hotfixes`
- Rigenera il codice e [iniezione di dipendenza](https://experienceleague.adobe.com/docs/commerce-operations/operational-playbook/glossary.html) (ovvero, il `generated/` , che include `generated/code` e `generated/metapackage`) utilizzando `bin/magento setup:di:compile`.
- Controlla se [`app/etc/config.php`](../store/store-settings.md) nel codebase. Adobe Commerce genera automaticamente questo file se non lo rileva durante la fase di build e include un elenco di moduli ed estensioni. Se esiste, la fase di build continua come di consueto, comprime i file statici utilizzando GZIP e distribuisce, riducendo i tempi di inattività nella fase di distribuzione. Fai riferimento a [opzioni di build](../environment/variables-build.md) informazioni sulla personalizzazione o la disattivazione della compressione dei file.

>[!WARNING]
>
>A questo punto, il cluster non è stato creato, pertanto non tentare di connettersi a un database o presumere che sia presente un processo daemon attivo.

Dopo la generazione dell&#39;applicazione, viene montato su un **file system di sola lettura**. È possibile configurare punti di montaggio specifici da leggere/scrivere. Non è possibile inviare l&#39;FTP al server e aggiungere moduli. È invece necessario aggiungere il codice all’archivio locale ed eseguire `git push`, che crea e distribuisce l’ambiente. Per la struttura del progetto, consulta [Struttura directory di progetto locale](../project/file-structure.md).

### Fase 3: Preparazione del lumacone

Il risultato della fase di build è un file system di sola lettura denominato _lumaca_. Questa fase crea un archivio e colloca il lume in un luogo di archiviazione permanente. La prossima volta che invii il codice, se un servizio non è stato modificato, utilizza il tag dall’archivio.

- Rende più rapida la creazione di un&#39;integrazione continua riutilizzando il codice invariato
- Se il codice cambia, aggiorna il tag per la build successiva da riutilizzare
- Consente il ripristino istantaneo di una distribuzione, se necessario
- Include i file statici se `app/etc/config.php` il file esiste nel codebase

Il tag include tutti i file e le cartelle **esclusi i seguenti** montaggi configurati in `magento.app.yaml`:

- `"var": "shared:files/var"`
- `"app/etc": "shared:files/etc"`
- `"pub/media": "shared:files/media"`
- `"pub/static": "shared:files/static"`

### Fase 4: Distribuzione di segmenti e cluster

Le tue applicazioni e tutti [backend](https://experienceleague.adobe.com/docs/commerce-operations/operational-playbook/glossary.html) fornitura di servizi come segue:

- Monta ogni servizio in un contenitore, ad esempio server Web, OpenSearch, [!DNL RabbitMQ]
- Monta il file system di lettura/scrittura (montato su una griglia di archiviazione distribuita ad alta disponibilità)
- Configura la rete in modo che i servizi possano &quot;vedersi&quot; (e solo l&#39;uno con l&#39;altro)

>[!NOTE]
>
>Apporta le modifiche in un ramo Git al termine di tutte le attività di generazione e distribuzione e invia nuovamente. Tutti i file system dell&#39;ambiente sono _sola lettura_. Un sistema di sola lettura garantisce distribuzioni deterministiche e migliora notevolmente la sicurezza del sito, poiché nessun processo può scrivere nel file system. Funziona anche per garantire che il codice sia identico negli ambienti di integrazione, staging e produzione.

### Fase 5: ganci di distribuzione

>[!NOTE]
>
>Questa fase mette il [!DNL Commerce] applicazione in modalità di manutenzione fino al completamento della distribuzione.

Nell’ultimo passaggio viene eseguito uno script di distribuzione che consente di rendere anonimi i dati negli ambienti di sviluppo, cancellare le cache ed eseguire query su strumenti di integrazione continua esterni. Quando questo script viene eseguito, puoi accedere a tutti i servizi dell’ambiente, ad esempio Redis.

Se il `app/etc/config.php` non esiste nel codebase, i file statici vengono compressi utilizzando `gzip` e implementato durante questa fase, che aumenta la durata della fase di distribuzione e della manutenzione del sito.

>[!NOTE]
>
>Fai riferimento a [distribuire variabili](../environment/variables-deploy.md) informazioni sulla personalizzazione o la disattivazione della compressione dei file.

Sono disponibili due hook di distribuzione. Il `pre-deploy.php` hook completa la pulizia e il recupero necessari delle risorse e del codice generato nell&#39;hook di compilazione. Il `php ./vendor/bin/ece-tools deploy` hook esegue una serie di comandi e script:

- Se Adobe Commerce è **non installato**, si installa con `bin/magento setup:install`, aggiorna la configurazione di distribuzione, `app/etc/env.php`e il database dell’ambiente specificato, ad esempio Redis e gli URL del sito web. **Importante:** Quando hai completato il [Prima implementazione](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/launch/overview.html) durante l’installazione, Adobe Commerce è stato installato e distribuito in tutti gli ambienti.

- Se Adobe Commerce **è installato**, esegui gli aggiornamenti necessari. Lo script di distribuzione viene eseguito `bin/magento setup:upgrade` aggiornare lo schema e i dati del database (necessario dopo l’aggiornamento dell’estensione o del codice core) e aggiornare anche la configurazione della distribuzione, `app/etc/env.php`e il database dell’ambiente. Infine, lo script di distribuzione cancella la cache di Adobe Commerce.

- Lo script genera facoltativamente contenuto web statico utilizzando il comando `magento setup:static-content:deploy`.

- Usa ambiti (`-s` negli script di build) con un&#39;impostazione predefinita di `quick` per la strategia di distribuzione dei contenuti statici. Puoi personalizzare la strategia utilizzando la variabile di ambiente [`SCD_STRATEGY`](../environment/variables-deploy.md#scd_strategy). Per informazioni dettagliate su queste opzioni e funzioni, vedi [Strategie di distribuzione dei file statici](../deploy/static-content.md) e `-s` contrassegno per [Distribuire file di visualizzazione statica](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/static-view/static-view-file-deployment.html).

>[!NOTE]
>
>Lo script di distribuzione utilizza i valori definiti dai file di configurazione in `.magento` , quindi lo script elimina la directory e il relativo contenuto. Questo non influisce sull’ambiente di sviluppo locale.

### Post-distribuzione: configurare il routing

Durante l’esecuzione della distribuzione, il processo interrompe il traffico in ingresso nel punto di ingresso per 60 secondi e riconfigura il routing in modo che il traffico web arrivi al cluster appena creato.

La corretta distribuzione rimuove la modalità di manutenzione per consentire l’accesso normale e crea file di backup (BAK) per `app/etc/env.php` e `app/etc/config.php` file di configurazione.

Abilitare la generazione di contenuti statici utilizzando `SCD_ON_DEMAND` e configurare il [`post_deploy` gancio](../application/hooks-property.md) in modo da cancellare la cache e precaricare (riscaldare) la cache _dopo_ il contenitore inizia ad accettare connessioni e _durante_ normale, traffico in arrivo.

Per rivedere i registri di build e distribuzione, consulta [Visualizza registri](../test/log-locations.md#view-and-manage-logs).
