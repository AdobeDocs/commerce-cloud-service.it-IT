---
title: Patch cloud per Commerce
description: Consulta un elenco degli ultimi miglioramenti al pacchetto Patch cloud.
recommendations: noDisplay, catalog
last-substantial-update: 2024-05-21T00:00:00Z
exl-id: ae6b511b-a37d-4776-9a5e-ad7d9f9f6611
source-git-commit: 61c42a1bd1d5a28f90b8756032ee6f45be4565b2
workflow-type: tm+mt
source-wordcount: '2208'
ht-degree: 0%

---

# Patch cloud per Commerce

Il [Patch cloud](https://github.com/magento/magento-cloud-patches) fornisce un set di patch richieste che migliorano l’integrazione di tutte le versioni di Adobe Commerce con gli ambienti Cloud e supporta la distribuzione rapida di correzioni critiche.

Il pacchetto Patch cloud per Commerce è una dipendenza per il pacchetto ECE-Tools e viene installato e aggiornato al momento dell’installazione o dell’aggiornamento del pacchetto ECE-Tools. Puoi anche utilizzare e gestire le patch cloud per Commerce come pacchetto autonomo per applicare le patch a un progetto Adobe Commerce che non si trova sulla piattaforma Cloud. Queste note sulla versione descrivono gli ultimi miglioramenti apportati a questo pacchetto.

>[!TIP]
>
>Per fare in modo che il progetto contenga tutte le patch richieste, aggiornare [versione più recente di strumenti ece](../dev-tools/update-package.md).

>[!NOTE]
>
>Consulta [Applicare le patch](../development/apply-patches.md) per istruzioni sull’applicazione di patch ai progetti.

Il `magento/magento-cloud-patches` Il pacchetto utilizza la seguente sequenza di versioni: `<major>.<minor>.<patch>`

<!--Add release notes below-->

## v1.0.27 {#latest}

Data di rilascio: 21 maggio 2024

- **Supporto per PHP 8.3**- Questa patch risolve gli errori di compatibilità tra php 8.3 e la versione del pacchetto del compositore.

## v1.0.26

Data di rilascio: 8 aprile 2024

- ![nuova icona](../../assets/new.svg) **PHP** — Aggiunta del supporto per PHP 8.3.

## v1.0.25

Data di rilascio: 16 gennaio 2024

- **Miglioramenti della cache**- Questa patch migliora l&#39;efficienza della cache di layout, riducendo in modo significativo l&#39;utilizzo della memoria, per Adobe Commerce versione 2.4.4 e successive.<!-- MCLOUD-11514 -->
- **Miglioramenti dei processi CRON**-Questa patch risolve il problema in cui i processi saltati attendono inutilmente i blocchi dei processi cron per le versioni di Adobe Commerce 2.4.4 e successive.<!-- MCLOUD-11329 -->

## v1.0.24

Data di rilascio: 15 settembre 2023

- **Miglioramento delle prestazioni**-Questa patch risolve un problema che influisce sulle prestazioni riducendo il numero di volte lo stesso caricamento delle configurazioni di distribuzione per Adobe Commerce da 2.4.6 a 2.4.6-p1<!-- MCLOUD-10604 -->

## v1.0.23

Data di rilascio: 31 luglio 2023

- **Rimozione della patch MCLOUD-10604.**-La patch è stata spostata in QPT.<!-- MCLOUD-10736 -->

## v1.0.22

Data di rilascio: 19 giugno 2023

- **Creazione guidata/output CLI QPT avanzato**- Aggiunta di un avviso alla procedura guidata/output CLI QPT che ricorda di verificare i dettagli e i requisiti della patch in presenza di dipendenze.<!-- ACP2E-1963 -->
- **Sono state aggiunte patch per Commerce 2.4.6:**
   - È stato corretto il `regexp cache tag` convalida.<!-- MCLOUD-10226 -->
   - Miglioramento delle prestazioni grazie alla riduzione del numero di volte in cui viene eseguito lo stesso caricamento delle configurazioni di distribuzione.<!-- MCLOUD-10604 -->
- **Sono state aggiunte patch per Commerce da 2.3.7 a 2.4.6**- È stato risolto un problema che causava un incremento di un valore casuale invece di un incremento di 1 per la `catalog_product_entity_*` tabelle.<!-- MCLOUD-10032 -->
- **Sono state aggiunte patch per Commerce da 2.4.0 a 2.4.6**- È stato corretto un errore che indicava che `The file can't be deleted. Warning!unlink: No such file or directory`, che si è verificato durante lo scaricamento della cache JS/CSS dall’amministratore.<!-- MCLOUD-10279 -->

## v1.0.21

Data di rilascio: 10 marzo 2023

- **Supporto avanzato per PHP 8.2**—Sono stati risolti dei problemi di compatibilità con alcune versioni di PHP 8.2.x che supportavano Commerce 2.4.6.

## v1.0.20

Data di rilascio: 27 ottobre 2022

- **Aggiunta patch per miglioramenti della cache L2**- Questa patch risolve un problema relativo allo svuotamento della cache L2 locale per Commerce versione 2.4.0 e 2.4.1.<!-- MCLOUD-7845 -->

## v1.0.19

Data di rilascio: 13 settembre 2022

- **Supporto avanzato per PHP 8.1**—Sono stati risolti dei problemi di compatibilità con alcune versioni di PHP 8.1.x.

## v1.0.18

Data di rilascio: 11 agosto 2022

Patch critica per Adobe Commerce 2.4.5:

- **Emissione di ordini tramite pagamenti Braintree**- Questa patch risolve un problema critico che impedisce agli amministratori di effettuare nuovi ordini o riordini.<!-- MCLOUD-9137 -->

Consulta [L&#39;amministratore non può creare un ordine o riordinarlo quando è abilitato il pagamento Braintree](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/known-issues-patches-attached/admin-cant-create-order-reorder-when-braintree-payment-enabled.html).

## v1.0.17

Data di rilascio: 24 maggio 2022

Sono stati corretti i vincoli per le patch di sicurezza in `patches.json` file.

## v1.0.16

Data di rilascio: 31 marzo 2022

Patch critica per Adobe Commerce 2.3.3-p1 e versioni successive:

Sono state aggiornate le patch per risolvere un **critico** vulnerabilità che comporta l’esecuzione di codice remoto non autenticato.<!-- MCLOUD-8479 -->

Consulta [Adobe Bollettino sulla sicurezza APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.15

Data di rilascio: 10 marzo 2022

- **Supporto PHP 8.1**- Aggiunta del supporto per PHP 8.1 e soppressione del supporto per PHP 7.0 e 7.1.
- **È stata aggiunta la patch per Adobe Commerce 2.3.3**- Valuta fissa visualizzata nella pagina del prodotto.

## v1.0.14

Data di rilascio: 13 febbraio 2022

Patch critica per Adobe Commerce 2.3.3-p1 e versioni successive:

È stata aggiunta una patch per risolvere un **critico** vulnerabilità che comporta l’esecuzione di codice remoto non autenticato.<!-- MCLOUD-8461 -->

Consulta [Adobe Bollettino sulla sicurezza APSB22-12](https://helpx.adobe.com/security/products/magento/apsb22-12.html).

## v1.0.13

Data di rilascio: 25 ottobre 2021

- **Aggiorna monologo**- È stata aggiornata la versione minima richiesta per `monolog` pacchetto a `^2.3`.<!-- ACMP-1263 -->
- **Metodo PHP non compatibile**—È stato corretto il metodo PHP incompatibile per Adobe Commerce versioni 2.4.3 e 2.3.7-p1.<!-- AC-384 -->
- **Errore PHP**- Fisso a `PHP error 'Undefined variable: errorMessage' ...` si è verificato un errore durante l’applicazione di una patch.<!-- ACP2E-138 -->

## v1.0.12

Data di rilascio: 12 agosto 2021

Patch critica per Adobe Commerce 2.4.3 e 2.3.7-p1:

- **Problema relativo alla limitazione della frequenza API**- Questa patch corregge un limite di velocità predefinito che impediva alle API Web di elaborare richieste con più di 20 elementi in un array. Questa patch aumenta il valore predefinito del limite di velocità. Consulta Adobe Commerce [Note sulla versione 2.4.3](https://devdocs.magento.com/guides/v2.4/release-notes/commerce-2-4-3.html#apply-mc-43048__set_rate_limits__243patch-to-address-issue-with-api-rate-limiting) e [Note sulla versione 2.3.7](https://devdocs.magento.com/guides/v2.3/release-notes/2-3-7-p1.html#apply-mc-43048__set_rate_limits__237-p1patch-to-address-issue-with-api-rate-limiting).<!-- MC-43048 -->

## v1.0.11

Data di rilascio: 29 luglio 2021

- **È stato risolto un problema causato dall’applicazione della patch di navigazione a livelli B2B**- Per i clienti che hanno applicato la patch di navigazione a livelli B2B, questa correzione risolve un `Undefined offset` Errore visualizzato nella pagina Ricerca dopo il passaggio alla visualizzazione Archivio.<!--MCLOUD-5287-->

- **Paypal Checkout patch**- Corregge un problema di Adobe Commerce 2.3.7 con PayPal Express in cui viene visualizzato il prezzo dell&#39;ordine precedentemente inserito.<!--MC-42674-->

- **Supporto categoria patch**- È stato aggiunto il supporto per l&#39;elaborazione delle categorie di patch e delle origini di origine assegnate alle patch di qualità. Le categorie consentono ai clienti di utilizzare i filtri e l’ordinamento per trovare le patch più rapidamente quando si utilizza [Strumento Patch di qualità](https://github.com/magento/quality-patches) e lo strumento di analisi a livello di sito (SWAT). <!--MC-38577-->

## v1.0.10

Data di rilascio: 10 maggio 2021

- **Compatibilità con Adobe Commerce 2.3.7**- Risoluzione del conflitto delle dipendenze del compositore per l&#39;installazione in Adobe Commerce 2.3.7.<!--MC-42131-->
- **È stato risolto un problema causato dall’applicazione di una patch in bundle più volte.**- Se si applica più di una volta una patch in bundle (che include altre patch obsolete), è possibile ripristinare i pacchetti obsoleti inclusi. Tutte le patch vengono ora applicate una sola volta. Se si tenta di applicare nuovamente lo stesso pacchetto, viene visualizzato un messaggio che indica che la patch è già stata applicata.<!--MC-41912-->
- **B2B patch di navigazione a livelli**- È stato risolto un altro problema che impediva la visualizzazione di tutte le opzioni di prodotto durante la navigazione su più livelli quando l&#39;utente abilita il catalogo condiviso B2B.<!--MCLOUD-7742-->

## v1.0.9

Data di rilascio: 1 febbraio 2021

- **B2B patch di navigazione a livelli**— È stato risolto il problema che impediva la visualizzazione di tutte le opzioni di prodotto durante la navigazione su più livelli quando era abilitato il catalogo condiviso B2B.<!--MCLOUD-6923-->
- **Compatibilità con PHP 7.4**— È stato risolto un problema di compatibilità delle patch cloud con PHP 7.4.<!--MCLOUD-7367-->
- **Le patch obsolete diventano visibili**— È stato risolto un problema relativo alle patch cloud a causa del quale le patch obsolete diventavano visibili nella tabella delle patch dopo l&#39;applicazione di una patch sostitutiva contenente l&#39;intero contenuto della patch obsoleta. Ciò può accadere se si applica un cerotto che combina diversi altri cerotti.<!--MC-40626-->
- **Errori silenziosi durante l’applicazione di patch**- È stato risolto un problema relativo alle patch del cloud in cui il `git apply` Il comando non è riuscito ad applicare le patch in alcuni ambienti.<!--MC-40529-->

## v1.0.8

Data di rilascio: 14 ottobre 2020

- **Aggiornamenti della compatibilità per magento/magento-cloud-patches**- Aggiornato il `symfony` e `semver` vincoli di versione in `composer.json` per compatibilità con Adobe Commerce 2.4.1 e versioni successive.<!--MCLOUD-7111-->

## v1.0.7

Data di rilascio: 14 ottobre 2020

- **Redis patch per Adobe Commerce da 2.3.0 a 2.3.5, 2.4.0**- Aggiornamento delle patch Redis per supportare l&#39;aggiunta di prodotti a una categoria durante l&#39;implementazione di una cache di livello 2. <!--MCLOUD-6659-->

- **Braintree patch VBE**- Corregge un problema che generava un errore quando un amministratore tentava di visualizzare un rapporto sulle impostazioni delle Braintree. <!--MCLOUD-6684-->

- Ora, il `ece-patches apply` utilizza Unix. `patch` comando per applicare le patch se Git non è disponibile nel sistema host. <!--MCLOUD-7069-->

## v1.0.6

Data di rilascio:

- **Patch Redis per Adobe Commerce 2.3.0 - 2.3.4** ottimizzazione della comunicazione e miglioramento delle prestazioni
   - Ridurre le dimensioni dei trasferimenti di rete tra Redis e Adobe Commerce
   - Correzione delle race condition per le operazioni di caricamento e scrittura Redis
   - Riscrittura dell&#39;adattatore cache di base per gestire gli errori durante il salvataggio
   - Riduzione del consumo di CPU Redis<!--MCLOUD-6139-->

- **Patch Redis per Adobe Commerce 2.3.0 - 2.3.5**- Miglioramento delle prestazioni e correzione degli errori
   - Correggi l’implementazione del blocco della cache per evitare blocchi infiniti
   - Migliorare il meccanismo di blocco corrente
   - Implementa i blocchi firmati per impedire lo sblocco da richieste parallele
   - Correggere il seguente errore che si verifica durante l&#39;operazione di scrittura Redis: `OOM command not allowed when used memory > maxmemory`
   - Correggi elaborazione per pulitura cache per `cat_p` tag in esecuzione durante gli aggiornamenti del prodotto<!--MCLOUD-6110-->

- È stato risolto un problema che causava un errore durante l’applicazione del `amzn/amazon-pay-module` applica la patch ad Adobe Commerce sui progetti di infrastruttura cloud con Adobe Commerce v2.2.6 o 2.3.5, che non includono questo modulo. Ora il processo di applicazione delle patch ignora `amzn/amazon-pay-module` applicare la patch se il modulo non è installato.<!--MCLOUD-6588-->

## v1.0.5

Data di rilascio: 26 giugno 2020

- **Miglioramenti delle prestazioni Redis**- Aggiunge funzioni di ottimizzazione Redis alle versioni 2.3.3 e 2.3.4 di Adobe Commerce. Queste correzioni sono state incluse nella versione 2.3.5 di Adobe Commerce. Consulta [Miglioramenti delle prestazioni](https://devdocs.magento.com/guides/v2.3/release-notes/release-notes-2-3-5-commerce.html#performance-boosts) nel _Note sulla versione di Adobe Commerce 2.3.5_.<!--MCLOUD-5771-->

- **Arricchimento del registro di New Relic**— Aggiunge l&#39;interfaccia del processore Monolog necessaria per supportare i miglioramenti alle funzionalità di registrazione di New Relic introdotti in Cloud Components di Commerce versione 1.0.4. Questa patch è necessaria per distribuire Adobe Commerce 2.1.x. Se la patch non viene applicata, la build non riesce durante il `di:compile` processo.<!--MCLOUD-6029-->

## v1.0.4

Data di rilascio: 12 maggio 2020

- **Pagamento Amazon**: è stato risolto un problema relativo al widget di pagamento Amazon Pay che impediva ai clienti di modificare il metodo di pagamento in _Revisione e pagamenti_ passaggio durante il processo di pagamento.<!--MCLOUD-5930-->

- **Visualizzazione prodotto sulla pagina Categoria**- È stato risolto un problema che impediva la visualizzazione dei prodotti nella pagina della categoria in. _Mostra tutte le pagine_ visualizzazione.<!--MCLOUD-5684-->

- **Caricamento immagine Page Builder**- Corregge un problema di interfaccia di Page Builder che a volte causava il seguente errore durante il caricamento di immagini nella raccolta immagini: `Destination folder is not writable or does not exist`<!--MCLOUD-5837-->

- **Elimina gli avvisi non necessari relativi alla generazione di sitemap**- Aggiunge un nuovo tentativo quando si verificano errori durante la generazione di sitemap e ignora la notifica e-mail del cliente nei casi in cui gli errori possono essere recuperati automaticamente.<!--MCLOUD-3025-->

- **Miglioramento delle prestazioni del sito**- Corregge un problema di prestazioni con il `Magento\Framework\App\DeploymentConfig\Reader::load` che periodicamente ha richiesto lunghi tempi di caricamento che hanno influito sulle prestazioni del sito. <!--MCLOUD-5650-->

- È stata aggiornata l’assegnazione delle patch per il metodo di pagamento in modo che eseguano il targeting dei moduli di pagamento anziché del pacchetto base del Magento (magento/magento2-base), in modo che le patch di pagamento vengano applicate solo se sono presenti i moduli di pagamento.<!--MCLOUD-5666-->

- Sono state aggiornate le patch per compatibilità con il Magento Open Source.<!--MCLOUD-5701-->

## v1.0.3

Data di rilascio: 28 aprile 2020

- È stata aggiunta la correzione per la patch &quot;FPC si sta disattivando durante le implementazioni&quot; per supportare Adobe Commerce 2.3.5.

## v1.0.2

Data di rilascio: 27 febbraio 2020

Questa versione include le patch e le correzioni critiche seguenti:

- **Aggiornamenti della compatibilità per magento/magento-cloud-patches**

   - È stato aggiornato il `symfony` e `semver` vincoli di versione in `composer.json` per compatibilità con Adobe Commerce 2.4 e versioni successive.<!--MAGECLOUD-5127-->

   - Vincoli aggiornati in `composer.json` per la compatibilità con `ece-tools` Versioni 2002.0.22 e successive 2002.0.x.

- **Pagamento PayPal Express**- Pubblicata il 12 febbraio 2020, questa patch risolve un problema che riguarda gli ordini effettuati con PayPal Express Checkout in cui l&#39;indirizzo di spedizione dell&#39;ordine specifica un paese che è stato immesso manualmente nel campo di testo anziché selezionato dal menu a discesa nella pagina Spedizione. Vedere la descrizione completa della patch nella pagina di download della patch.

- **Correzione della distribuzione dell’applicazione**- Aggiunta di una patch per risolvere un problema che disabilita la cache di pagina intera durante il processo di distribuzione. Questa patch si applica ad Adobe Commerce 2.3.2 e versioni successive.

- **Parametro di ambito per API Async/Bulk**- La patch è stata aggiornata per correggere un errore di sintassi nella `composer.json` file. Questo cerotto si applica ai Magenti Open Source 2.3.1 e 2.3.2. Vedere la descrizione completa della patch nella pagina di download della patch.

## v1.0.1

Data di rilascio: 6 febbraio 2020

Abbiamo incluso tutte le patch del Magento Open Source 2.x dalla pagina dei download del software nella versione 1.0.1 di magento/magento-cloud-patches. Se hai copiato delle patch nel progetto in precedenza, rimuovile per evitare conflitti.

Questa versione include le patch e le correzioni critiche seguenti:

- **Correggere i deadlock cron e migliorare il blocco cron**—

   - È stato risolto un problema che impediva l’esecuzione di alcuni processi cron a causa di un valore di stato errato in `cron_schedule` tabella. Ora usiamo il framework di blocco Adobe Commerce per controllare e aggiornare lo stato del processo cron invece di utilizzare `cron_schedule` tabella. I processi cron terminati con uno stato di errore vengono ritentati durante la successiva esecuzione del cron invece di attendere 24 ore.

   - Aggiunge un _riprova_ operazione per evitare deadlock durante gli aggiornamenti ai dati nella `cron_schedule` tabella.

- **Aggiornato `magento/magento-cloud-patches` per includere tutte le patch disponibili per Magento Open Source 2.x**: il pacchetto magento/magento-cloud-patches è stato aggiornato per includere tutte le patch di Magento Open Source 2.x disponibili nella pagina dei download del software. Se in precedenza hai copiato patch di Magento Open Source nel progetto Adobe Commerce on Cloud Infrastructure, rimuovile per evitare conflitti.<!--MAGECLOUD-4606-->

- **correzione paginazione catalogo di Elasticsearch** - La patch di paginazione del catalogo Elasticsearch distribuita in magento/magento-cloud-patches v1.0 è stata sostituita con una correzione più efficace.<!--MAGECLOUD-4847-->

- **Patch di Page Builder**—In Cloud Patches per Commerce 1.0.0, sono state incluse nel bundle le le patch di Page Builder per risolvere una vulnerabilità nota di Page Builder Remote Code Execution (RCE), con la correzione iniziale basata su Adobe Commerce 2.3.3. Abbiamo aggiornato queste patch con un’implementazione più stabile basata su Adobe Commerce 2.3.4., che include più ottimizzazioni per la risoluzione del problema.<!--MAGECLOUD-4884-->

  Se disponi del pacchetto magento/magento-cloud-patches 1.0.0, sei ancora protetto dai problemi di vulnerabilità RCE di Page Builder. Se esegui l’aggiornamento alla versione 1.0.1 o successiva, l’implementazione della stessa correzione risulterà migliore.

## v1.0.0

Data di rilascio: 14 novembre 2019

Questa è la prima versione di [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches) pacchetto, una nuova dipendenza per `ece-tools` versione del pacchetto 2002.0.22 o successive.

Questa versione include le patch e le correzioni critiche seguenti:

- **Patch di sicurezza di Page Builder per le versioni 2.3.1.x e 2.3.2.x**- È stato risolto un problema nell&#39;anteprima di Page Builder che consente agli utenti non autenticati di accedere ad alcuni metodi di modelli che possono essere utilizzati per attivare l&#39;esecuzione di codice arbitrario sulla rete (RCE) con conseguente perdita di informazioni globali. Questo problema può verificarsi quando si utilizzano versioni non supportate di Page Builder con Adobe Commerce versioni 2.3.1 e 2.3.2.<!--MAGECLOUD-4649-->

- **Patch MSI**- Corregge i problemi che causavano errori di indicizzazione e problemi di prestazioni quando si utilizzavano le impostazioni di inventario predefinite per la gestione delle scorte.<!--MAGECLOUD-4428-->

- **Compatibilità con le versioni precedenti delle nuove interfacce di posta**-Corregge un problema di incompatibilità con le versioni precedenti causato da `Magento\Framework\Mail\EmailMessageInterface` Interfaccia PHP introdotta in Adobe Commerce v2.3.3. Nell’ambito di questa patch, il nuovo `EmailMessageInterface` eredita dal vecchio `MessageInterface`, e i moduli core di Adobe Commerce dipendono nuovamente da `MessageInterface`.<!--MAGECLOUD-4422-->

- **La paginazione del catalogo non funziona su Elasticsearch 6.x**- Corregge un problema critico relativo all&#39;impaginazione dei risultati di ricerca che interessa i clienti che utilizzano Elasticsearch 6.x come motore di ricerca del catalogo.<!--MAGECLOUD-4448-->
