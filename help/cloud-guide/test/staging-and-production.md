---
title: Test di staging e produzione
description: Scopri come eseguire il test in ambienti di staging e produzione.
exl-id: 5b762d59-04c5-4e89-a637-719141759158
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '1329'
ht-degree: 0%

---

# Test di staging e produzione

Dopo una migrazione corretta di codice, file e dati a Staging o Produzione, utilizza gli URL dell’ambiente per testare siti e archivi. Di seguito sono riportate informazioni sulla verifica dei registri, sui test delle configurazioni Fastly, sui test di accettazione da parte degli utenti (UAT) e altro ancora.

## File di registro

Se rilevi errori di distribuzione o altri problemi durante il test, controlla i file di registro. I file di registro si trovano nella directory `var/log`.

Il registro di distribuzione è in `/var/log/platform/<prodject-ID>/deploy.log`. Il valore di `<project-ID>` dipende dall&#39;ID del progetto e dal fatto che l&#39;ambiente sia di staging o produzione. Ad esempio, con un ID progetto di `yw1unoukjcawe`, l&#39;utente di staging è `yw1unoukjcawe_stg` e l&#39;utente di produzione è `yw1unoukjcawe`.

Quando si accede ai registri in ambienti di produzione o di staging, utilizza SSH per accedere a ciascuno dei tre nodi e individuare i registri. In alternativa, è possibile utilizzare [Gestione log di New Relic](../monitor/log-management.md) per visualizzare ed eseguire query sui dati di log aggregati da tutti i nodi. Consulta [Visualizza registri](log-locations.md#application-logs).

## Verifica la base di codice

Verifica che la base di codice sia correttamente distribuita negli ambienti di staging e produzione. Gli ambienti devono avere basi di codice identiche.

## Verificare le impostazioni di configurazione

Controlla le impostazioni di configurazione tramite il pannello Amministratore, inclusi l’URL di base, l’URL amministratore di base, le impostazioni multisito e altro ancora. Se devi apportare ulteriori modifiche, completa le modifiche nel ramo Git locale e inviale al ramo `master` in Integrazione, Staging e Produzione.

## Controlla Fastly caching

[Per configurare Fastly](../cdn/fastly-configuration.md) è necessario prestare particolare attenzione ai dettagli: è necessario utilizzare le credenziali token Fastly Service ID e Fastly API corrette, caricare il codice Fastly VCL, aggiornare la configurazione DNS e applicare i certificati SSL/TLS agli ambienti. Dopo aver completato queste attività di configurazione, puoi verificare la memorizzazione nella cache rapida negli ambienti di staging e produzione.

**Per verificare la configurazione del servizio Fastly**:

1. Accedi all&#39;amministratore per staging e produzione utilizzando l&#39;URL con `/admin` o l&#39;[URL amministratore aggiornato](../environment/variables-admin.md#admin-url).

1. Passa a **Archivi** > **Impostazioni** > **Configurazione** > **Avanzate** > **Sistema**. Scorrere e fare clic su **Cache a pagina intera**.

1. Verificare che il valore dell&#39;**applicazione di memorizzazione in cache** sia impostato su _Fastly CDN_.

1. Verifica le credenziali Fastly.

   - Fare clic su **Configurazione rapida**.

   - Verifica che i valori per le credenziali del token Fastly Service ID e Fastly API siano corretti. Vedi [Ottieni credenziali rapide](/help/cloud-guide/cdn/fastly-configuration.md#get-fastly-credentials).

   - Fare clic su **Verifica credenziali**.

   >[!WARNING]
   >
   >Assicurati di aver immesso l’ID servizio Fastly e il token API corretti negli ambienti di staging e produzione. Le credenziali rapide vengono create e mappate per ogni ambiente di servizio. Se immetti le credenziali di staging nell&#39;ambiente di produzione, non puoi caricare i snippet VCL, la memorizzazione nella cache non funziona correttamente e la configurazione di memorizzazione nella cache punta al server e agli archivi errati.

**Per controllare il comportamento di Fastly caching**:

1. Controllare le intestazioni utilizzando l&#39;utilità della riga di comando `dig` per ottenere informazioni sulla configurazione del sito.

   È possibile utilizzare qualsiasi URL con il comando `dig`. Negli esempi seguenti vengono utilizzati gli URL Pro:

   - Gestione temporanea: `dig https://mcstaging.<your-domain>.com`
   - Produzione: `dig https://mcprod.<your-domain>.com`

   Per ulteriori `dig` test, vedere [Test di Fastly prima di modificare il DNS](https://docs.fastly.com/en/guides/working-with-domains).

1. Utilizzare `cURL` per verificare le informazioni dell&#39;intestazione di risposta.

   ```bash
   curl https://mcstaging.<your-domain>.com -H "host: mcstaging.<your-domain.com>" -k -vo /dev/null -H Fastly-Debug:1
   ```

   Per informazioni dettagliate sulla verifica delle intestazioni, consulta [Controllare le intestazioni di risposta](../cdn/fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers).

1. Una volta che sei attivo, usa `cURL` per controllare il tuo sito attivo.

   ```bash
   curl https://<your-domain> -k -vo /dev/null -H Fastly-Debug:1
   ```

## Completare il test UAT

Completamento del test di accettazione utente (UAT) su staging e produzione. I seguenti test sono un rapido elenco di attività e aree possibili da testare come esercente e cliente. L&#39;elenco potrebbe essere più lungo e includere test aggiuntivi per moduli personalizzati, estensioni e integrazioni di terze parti. Durante il test, utilizza desktop, notebook e dispositivi mobili.

In caso di problemi, salva i passaggi di riproduzione, i messaggi di errore, le acquisizioni di schermate anomale e i collegamenti. Utilizzare queste informazioni per individuare e risolvere i problemi nel codice e nelle configurazioni dell&#39;ambiente di integrazione o nelle impostazioni dell&#39;ambiente.

<table>
<tr>
<td style="width:150px">Gestione utente</td>
<td>
<ul>
<li>Creare e modificare gli account cliente, verificare le e-mail</li>
<li>Creare ruoli di amministratore per gli esercenti</li>
<li>Creare account esercente con ruoli specifici</li>
<li>Verifica accesso account esercente per ruolo</li>
</ul>
</td>
</tr>
<tr>
<td>Cataloghi e prodotti</td>
<td>
<ul>
<li>Creare un catalogo con i prodotti associati</li>
<li>Crea prodotti per la vetrina, compresi tutti i tipi di prodotti: semplici, configurabili, in bundle</li>
<li>Aggiungere immagini, campioni, video e altre opzioni multimediali del prodotto</li>
<li>Configurare prezzi, sconti e regole di determinazione prezzi </li>
<li>Configurare funzioni avanzate che includono fasce di prezzo, prodotti in primo piano e date di disponibilità</li>
<li>Modifica le scorte e verifica che i valori corretti vengano visualizzati e modificati in base all'incremento e all'acquisto completato</li>
</ul>
</td>
</tr>
<tr>
<td>Carrelli e pagamento</td>
<td>
<ul>
<li>Cerca i prodotti e seleziona le opzioni di filtro</li>
<li>Aggiungi prodotti al carrello da risultati di ricerca, pagine di categorie e pagine di prodotti</li>
<li>Test di tutti i tipi di prodotto</li>
<li>Visualizzare il carrello e modificare il contenuto rimuovendo o modificando gli importi </li>
<li>Effettua il pagamento per verificare gli importi degli ordini in base al carrello e alle informazioni sul prodotto</li>
<li>Verifica che le imposte vengano calcolate correttamente per il carrello</li>
<li>Completa un acquisto con diverse opzioni: aggiungi un coupon, seleziona la spedizione, inserisci le informazioni di spedizione e fatturazione e le informazioni di pagamento</li>
<li>Verifica i gateway e le opzioni di pagamento durante il pagamento</li>
<li>Verifica la presenza di notifiche su schermo, ordini elencati nell’account cliente e notifiche e-mail</li>
<li>Verifica dell'estrazione di clienti e ospiti</li>
</ul>
</td>
</tr>
<tr>
<td>Order Management</td>
<td>
<ul>
<li>Creare un ordine per un cliente</li>
<li>Cerca e visualizza gli ordini</li>
<li>Modificare un ordine aggiungendo e rimuovendo prodotti, modificando gli importi, modificando le informazioni di spedizione e fatturazione</li>
<li>Gestire un rimborso</li>
<li>Annullare un ordine</li>
<li>Applica codici coupon e sconti</li>
</ul>
</td>
</tr>
<tr>
<td>Contenuto del sito</td>
<td>
<ul>
<li>Verifica che tutti i temi e le risorse siano caricati correttamente</li>
<li>Verifica che i display CSS siano visualizzati correttamente, comprese le dimensioni dei file multimediali dinamici</li>
<li>Controlla i Termini e Condizioni, la politica di rimborso e altre informazioni sulla politica</li>
<li>Controlla le informazioni di contatto, i collegamenti e altro ancora sulla tua azienda</li>
<li>Cerca prodotti e contenuti, controlla il filtraggio dei risultati</li>
<li>Verifica il blocco del piè di pagina e i blocchi di navigazione superiori</li>
<li>Verifica le pagine 404 e Manutenzione</li>
</ul>
</td>
</tr>
<tr>
<td>Estensioni</td>
<td>
<ul>
<li>Verifica tutte le impostazioni di estensione, in particolare per eventuali moduli di tassazione, spedizione e pagamento (ad esempio: ordine inviato a warehouse e sistema di gestione finanziaria)</li>
<li>Testare tutte le interazioni del modulo personalizzato e dell’estensione installata</li>
<li>Controlla i dati per eventuali interazioni da completare (pagamenti, ordini, notifiche e-mail)</li>
<li>Verifica le configurazioni per ambiente per le estensioni</li>
<li>Verificare il funzionamento delle dipendenze tra moduli ed estensioni</li>
<li>Controlla tutte le azioni come esercente e cliente</li>
</ul>
</td>
</tr>
<tr>
<td>Integrazioni di terze parti</td>
<td>
<ul>
<li>Verifica che i dati vengano salvati correttamente in Adobe Commerce ed esportati, inviati o accessibili dal servizio di terze parti (ad esempio: gli ordini vengono visualizzati nel sistema di gestione degli ordini di terze parti)</li>
<li>Verifica configurazioni e interazioni per integrazione</li>
<li>Eseguire test di andata e ritorno da Adobe Commerce e dal servizio di terze parti</li>
<li>Verifica che l’autenticazione sia completa</li>
<li>Verifica la presenza di eventuali problemi registrati per aggiornare le integrazioni di codice o i messaggi di errore nei pannelli di controllo</li>
</ul>
</td>
</tr>
<tr>
<td>Test back-end</td>
<td>
<ul>
<li>Verifica e cancella cache </li>
<li>Eseguire le reindicizzazioni e verificare i risultati</li>
<li>Controlla processi cron, verifica la presenza di eventuali errori cron_schedule</li>
<li>Verifica e verifica eventuali problemi relativi allo script della shell</li>
<li>Controlla eventuali problemi registrati: registri applicazioni, registri PHP, registri MySQL, registri e-mail</li>
</ul>
</td>
</tr>
</table>

## Test di carico e sollecitazione

Prima dell’avvio, è consigliabile eseguire test approfonditi del traffico e delle prestazioni sugli ambienti di staging e produzione. È consigliabile eseguire test delle prestazioni per i processi front-end e back-end.

Prima di iniziare il test, inserisci un ticket con il supporto relativo agli ambienti in fase di test, agli strumenti utilizzati e all’intervallo di tempo. Aggiorna il ticket con risultati e informazioni per tenere traccia delle prestazioni. Una volta completato il test, aggiungi i risultati aggiornati e osserva che il test del ticket è stato completato con un timestamp e una data.

Rivedi le opzioni [Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) come parte del processo di preparazione pre-avvio.

Per ottenere risultati ottimali, utilizzare i seguenti strumenti:

- [Test delle prestazioni dell&#39;applicazione](../environment/variables-post-deploy.md#ttfb_tested_pages): verifica delle prestazioni dell&#39;applicazione configurando la variabile di ambiente `TTFB_TESTED_PAGES` per verificare il tempo di risposta del sito.
- [Assedio](https://www.joedog.org/siege-home/): il software di modellazione e test del traffico consente di spingere l&#39;archivio al limite. Visita il tuo sito con un numero configurabile di client simulati. Siege supporta l’autenticazione di base, i cookie, i protocolli HTTP, HTTPS e FTP.
- [Jmeter](https://jmeter.apache.org): test di carico eccellenti per valutare le prestazioni per il traffico picco, come nel caso delle vendite flash. Crea test personalizzati da eseguire sul sito.
- [New Relic](../monitor/new-relic-service.md) (fornito): consente di individuare i processi e le aree del sito causando un rallentamento delle prestazioni con tempo di rilevamento impiegato per azione, ad esempio per la trasmissione di dati, query, Redis e altro ancora.
- [WebPageTest](https://www.webpagetest.org) e [PKingdom](https://www.pingdom.com): l&#39;analisi in tempo reale delle pagine del sito viene caricata con percorsi di origine diversi. Il regno può richiedere una tassa. WebPageTest è uno strumento gratuito.

## Test funzionali

Puoi utilizzare il framework di test funzionali di Magento (MFTF) per completare i test funzionali per Adobe Commerce dall’ambiente Cloud Docker. Consulta [Test dell&#39;applicazione](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) nella _Guida di Cloud Docker per Commerce_.

## Configurare lo strumento Security Scan

È disponibile uno strumento di analisi della sicurezza gratuito per i siti. Per aggiungere i tuoi siti ed eseguire lo strumento, consulta [Strumento di analisi della sicurezza](../launch/overview.md#set-up-the-security-scan-tool).
