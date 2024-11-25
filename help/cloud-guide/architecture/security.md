---
title: Sicurezza dell’infrastruttura cloud
description: Scopri come Adobe protegge l’infrastruttura cloud con Adobe Commerce.
feature: Cloud, Security
source-git-commit: 275b76aaf00c0f5c768773d51b2909b748e9e7cb
workflow-type: tm+mt
source-wordcount: '1665'
ht-degree: 0%

---


# Sicurezza

L&#39;architettura del piano Adobe Commerce [Pro](pro-architecture.md) è progettata per fornire un ambiente altamente sicuro. Ogni cliente viene distribuito nel proprio ambiente server isolato, separato dagli altri. I dettagli di sicurezza dell’ambiente di produzione sono descritti di seguito.

## Browser web

La maggior parte del traffico in entrata e in uscita dall’ambiente cloud proviene dai browser web dei consumatori. Il traffico del consumatore può essere protetto utilizzando HTTPS per tutte le pagine del sito web (utilizzando una certificazione SSL condivisa o il certificato SSL del cliente a un costo aggiuntivo). Le pagine di pagamento e account vengono sempre servite utilizzando HTTPS. La best practice prevede di distribuire tutte le pagine in HTTPS.

## Rete di distribuzione dei contenuti

Fastly fornisce una protezione CDN (Content Delivery Network) e DDoS (Distributed Denial of Service). Fastly CDN consente di isolare l’accesso diretto ai server di origine. Il DNS pubblico punta solo alla rete Fastly. La soluzione Fastly DDS protegge da attacchi di livello 3 e 4 altamente dirompenti e da attacchi di livello 7 più complessi. Gli attacchi di livello 7 possono essere bloccati utilizzando regole personalizzate basate sull’intera richiesta HTTP/HTTPS e sui criteri del client e della richiesta, tra cui intestazioni, cookie, percorso della richiesta e IP del client, oppure indicatori come la geolocalizzazione.

Consulta [Panoramica dei servizi Fastly](../cdn/fastly.md).

## Firewall applicazione Web

Fastly Web Application Firewall (WAF) viene utilizzato per fornire una protezione aggiuntiva. Il WAF basato su cloud di Fastly utilizza regole di terze parti provenienti da fonti commerciali e open source, come il set di regole di base di OWASP. Inoltre, vengono utilizzate regole specifiche per Adobe Commerce. I clienti sono protetti da attacchi chiave a livello di applicazione, tra cui attacchi di iniezione e input dannosi, vulnerabilità cross-site scripting, exfiltrazione dei dati, violazioni del protocollo HTTP e altre minacce OWASP top ten.

Le regole di WAF vengono aggiornate da Adobe Commerce nel caso in cui vengano rilevate nuove vulnerabilità che consentono a Managed Services di applicare &quot;virtualmente patch&quot; ai problemi di sicurezza prima delle patch software. Fastly WAF non fornisce servizi di limitazione della velocità o di rilevamento di bot. Se lo desideri, i clienti possono concedere in licenza un servizio di rilevamento di bot di terze parti compatibile con Fastly.

Vedere [Firewall applicazione Web (WAF)](../cdn/fastly-waf-service.md).

## Virtual Private Cloud

L’ambiente di produzione del piano Adobe Commerce Pro è configurato come Virtual Private Cloud (VPC) in modo che i server di produzione siano isolati e abbiano una capacità limitata di connettersi all’ambiente cloud e di uscirne. Sono consentite solo connessioni sicure ai server cloud. Per i trasferimenti di file è possibile utilizzare protocolli sicuri come SFTP o rsync.

I clienti possono utilizzare i tunnel SSH per proteggere le comunicazioni con l’applicazione. L’accesso a AWS PrivateLink può essere fornito a pagamento. Tutte le connessioni a questi server vengono controllate tramite AWS Security Groups, un firewall virtuale che limita le connessioni all’ambiente. Le risorse tecniche dei clienti possono accedere a questi server utilizzando SSH.

## Crittografia

Amazon Elastic Block Store (EBS) viene utilizzato per l’archiviazione. Tutti i volumi EBS vengono crittografati utilizzando l&#39;algoritmo AES-256, il che significa che i dati vengono crittografati a riposo. Il sistema crittografa anche i dati in transito tra la rete CDN e l’origine e tra i server di origine. Le password dei clienti vengono memorizzate come hash. Le credenziali riservate, incluse quelle del gateway di pagamento, vengono crittografate utilizzando l’algoritmo SHA-256.

L&#39;applicazione Adobe Commerce non supporta la crittografia a livello di colonna o di riga o la crittografia quando i dati non sono inattivi o in transito tra server diversi. Il cliente può gestire le chiavi di crittografia dall&#39;interno dell&#39;applicazione. Le chiavi utilizzate dal sistema sono memorizzate in AWS Key Management System e devono essere gestite da Managed Services per fornire parti del servizio.

## Rilevamento degli endpoint e risposta

[!DNL CrowdStrike Falcon], un agente EDR (Endpoint Detection and Response) leggero di nuova generazione è installato su tutti gli endpoint (inclusi i server) all&#39;interno di Adobe. Gli agenti EDR proteggono i dati e i sistemi Adobe con il monitoraggio e la raccolta continui in tempo reale, che consente di identificare e rispondere rapidamente alle minacce.

## Test di penetrazione

Managed Services esegue regolarmente test di penetrazione del cloud system Adobe Commerce con l’applicazione preconfigurata. I clienti sono responsabili di qualsiasi test di penetrazione delle loro applicazioni personalizzate.

## Gateway di pagamento

Adobe Commerce richiede integrazioni del gateway di pagamento in cui i dati della carta di credito vengono passati direttamente dal browser del consumatore al gateway di pagamento. I dati della scheda non sono mai disponibili in nessuno degli ambienti Adobe Commerce Pro plan Managed Services. Le azioni sulle transazioni eseguite dall’applicazione e-commerce vengono completate utilizzando un riferimento alla transazione proveniente dal gateway.

## applicazione Adobe Commerce

Adobe verifica regolarmente il codice dell’applicazione principale per individuare eventuali vulnerabilità di sicurezza. Ai clienti vengono fornite patch per i difetti e i problemi di sicurezza. Il team di sicurezza del prodotto convalida i prodotti Adobe Commerce seguendo le linee guida di sicurezza delle applicazioni OWASP. Per testare e verificare la conformità vengono utilizzati diversi strumenti di valutazione delle vulnerabilità di sicurezza e fornitori esterni. Gli strumenti di sicurezza includono:

- Veracode Static and Dynamic Scanning
- Analisi del codice sorgente RIPS
- Servizi di scansione delle vulnerabilità di Trustwave e Logica di avviso
- Burp Suite Pro
- OWASPZAP
- eSqlMap

La base di codice completa viene scansionata con questi strumenti su base bisettimanale. Le patch di sicurezza vengono notificate ai clienti tramite e-mail dirette, notifiche nell&#39;applicazione e nel [Centro sicurezza PC](https://helpx.adobe.com/security.html).

I clienti devono assicurarsi che queste patch vengano applicate alla propria applicazione personalizzata entro 30 giorni dal rilascio, in conformità alle linee guida PCI. Adobe fornisce anche uno [strumento di analisi della sicurezza](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/security/security-scan) che consente ai commercianti di monitorare regolarmente i propri siti e ricevere aggiornamenti sui rischi di sicurezza noti, il malware e l&#39;accesso non autorizzato. Lo strumento Security Scan è un servizio gratuito e può essere eseguito su qualsiasi versione di Adobe Commerce.

Per incoraggiare i ricercatori di sicurezza a identificare e segnalare le vulnerabilità, Adobe Commerce dispone di un [programma di bug-bounty](https://hackerone.com/magento) oltre a test interni. Inoltre, il cliente riceve il codice sorgente completo dell&#39;applicazione per la propria revisione, se desiderato.

## File system di sola lettura

Tutto il codice eseguibile viene distribuito in un&#39;immagine del file system di sola lettura, che riduce notevolmente le superfici disponibili per l&#39;attacco. Il processo di distribuzione crea un’immagine Squash-FS per ridurre le opportunità di inserire codice PHP o JavaScript nel sistema o modificare i file dell’applicazione Adobe Commerce.

## Distribuzione remota

L’unico modo per inserire il codice eseguibile nell’ambiente di produzione Managed Services è quello di eseguirlo attraverso un processo di provisioning. Il provisioning comporta il push del codice sorgente dal repository di origine in un repository remoto che avvia un processo di distribuzione. L’accesso a tale destinazione di distribuzione è controllato, in modo da avere il controllo completo su chi può accedere alla destinazione di distribuzione. Tutte le distribuzioni del codice dell’applicazione nell’ambiente non di produzione sono controllate dal cliente.

## Registrazione

Tutte le attività di AWS sono registrate in AWS CloudTrail. I registri del sistema operativo, del server applicazioni e del database vengono archiviati nei server di produzione e nei backup. Tutte le modifiche al codice sorgente vengono registrate in un archivio Git. La cronologia della distribuzione è disponibile in Adobe Commerce [Project Web Interface](../project/overview.md). Tutti gli accessi al supporto vengono registrati e le sessioni di supporto registrate.

Consulta [Visualizzare e gestire i registri](../test/log-locations.md).

## Dati sensibili

I dati riservati possono riguardare sia informazioni personali dei consumatori che dati riservati dei clienti Managed Services. La protezione dei dati sensibili di clienti e consumatori è un obbligo fondamentale per Adobe Commerce Managed Services. Sia i clienti Managed Services che Adobe hanno obblighi legali in merito alle informazioni personali identificabili. Oltre alle funzioni di sicurezza dell’architettura, esistono altri controlli per limitare la distribuzione e l’accesso ai dati sensibili.

I clienti possiedono i propri dati e hanno il controllo sulla loro posizione. Il cliente specifica la posizione in cui risiedono le istanze di produzione e sviluppo. Inoltre, specificano la posizione utilizzata per l’ambiente di reporting di Adobe Commerce con Commerce e se tale applicazione di reporting di Adobe Commerce ha accesso o meno a informazioni personali. Le istanze di produzione possono trovarsi nella maggior parte delle aree geografiche di AWS, mentre gli ambienti di sviluppo e reporting di Adobe Commerce si trovano attualmente negli Stati Uniti o nell’Unione Europea.

I dati sensibili possono passare attraverso la rete di server CDN Fastly ma non vengono memorizzati nella rete Fastly. Tutti i partner inclusi nell&#39;offerta Managed Services hanno obblighi contrattuali per garantire la protezione dei dati sensibili. Managed Services non sposta i dati sensibili di clienti o consumatori dalle posizioni specificate dal cliente.

Come parte dei servizi inclusi nell&#39;offerta Managed Services, il personale Managed Services deve poter accedere ai sistemi di produzione che contengono dati sensibili. Questi dipendenti sono formati sui loro obblighi di proteggere i dati sensibili di clienti e consumatori. L&#39;accesso a questi sistemi è limitato ai dipendenti che devono accedervi. Questi dipendenti hanno accesso solo per il tempo necessario a fornire questi servizi.

## Regolamento generale sulla protezione dei dati

Il Regolamento generale sulla protezione dei dati (RGPD) è un quadro giuridico che definisce le linee guida per la raccolta e il trattamento dei dati personali di singoli individui nell’Unione europea (UE). Queste normative si applicano indipendentemente da dove si trova il sito e i visitatori dell’UE vi hanno accesso.

I visitatori devono essere informati dei dati che un sito raccoglie da loro e acconsentire esplicitamente alla raccolta di informazioni. I siti devono informare i visitatori in caso di violazione dei dati personali da essi detenuti.

Il commerciante o la società che gestisce il sito deve disporre di un responsabile della protezione dei dati dedicato che sovrintende alla sicurezza dei dati del sito. Il Data Privacy Officer (o il team di gestione del sito web) deve essere disponibile per il contatto nel caso in cui un visitatore richieda la cancellazione dei propri dati.

Il RGPD richiede che tutte le informazioni personali identificabili (come nomi, razza e data di nascita) raccolte siano anonime o pseudonimizzate.

>[!NOTE]
>
>Questa pagina fornisce una panoramica generale di cosa considerare per il RGPD. Per informazioni dettagliate su come Adobe Commerce archivia le informazioni personali, consulta la _[Guida alla sicurezza e alla conformità](https://experienceleague.adobe.com/en/docs/commerce-operations/security-and-compliance/overview)_. Per determinare in che modo la tua azienda deve conformarsi a qualsiasi obbligo legale, consulta il tuo consulente legale o fai riferimento al [testo ufficiale](https://eur-lex.europa.eu/eli/reg/2016/679/oj).

## Backup

I backup vengono eseguiti ogni ora per le ultime 24 ore di funzionamento. Dopo il periodo di 24 ore, i backup vengono mantenuti secondo una pianificazione utilizzando il servizio Snapshot di AWS EBS. Vedi [Criteri di conservazione](pro-architecture.md#retention-policy).

Il servizio crea un backup indipendente sullo storage ridondante. Poiché i volumi EBS sono crittografati, anche i backup vengono crittografati. Inoltre, Managed Services esegue backup su richiesta del cliente.

L&#39;approccio di backup e ripristino Managed Services utilizza un&#39;architettura ad alta disponibilità combinata con backup di sistemi completi. Ogni progetto viene replicato (tutti i dati, il codice e le risorse) in tre aree di disponibilità AWS separate, ciascuna con un centro dati separato.

Consulta [Snapshot e gestione dei backup](../storage/snapshots.md).
