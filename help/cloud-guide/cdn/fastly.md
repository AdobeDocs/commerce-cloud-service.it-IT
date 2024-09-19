---
title: Panoramica dei servizi Fastly
description: Scopri in che modo i servizi Fastly inclusi nell’infrastruttura cloud di Adobe Commerce consentono di ottimizzare e proteggere le operazioni di distribuzione dei contenuti per i siti Adobe Commerce.
feature: Cloud, Configuration, Iaas, Paas, Cache, Security, Services
exl-id: dc4500bf-f037-47f0-b7ec-5cd1291f73a1
source-git-commit: dc331df378074af8a8776a33784b73082a39cf10
workflow-type: tm+mt
source-wordcount: '1426'
ht-degree: 0%

---

# Panoramica dei servizi Fastly

>[!WARNING]
>
>Per mantenere la conformità PCI per i siti Adobe Commerce distribuiti sulla piattaforma Cloud, configura Fastly nel tuo ramo principale Starter, negli ambienti di produzione Pro e negli ambienti di staging Pro. Se utilizzi Adobe Commerce in una distribuzione headless, ti consigliamo vivamente di utilizzare Fastly per memorizzare nella cache le risposte GraphQL. Vedi [Memorizzazione in cache con Fastly](https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly) nella *Guida per gli sviluppatori di GraphQL*.

Fastly fornisce i seguenti servizi per ottimizzare e proteggere le operazioni di distribuzione dei contenuti per i progetti Adobe Commerce su infrastrutture cloud. Questi servizi sono inclusi in Adobe Commerce sull’infrastruttura cloud senza costi aggiuntivi.

- **Content Delivery Network (CDN)**: servizio basato su vernice che memorizza nella cache pagine del sito, risorse, CSS e altro ancora nei centri dati back-end configurati. Quando i clienti accedono al sito e agli store, le richieste raggiungono Fastly per caricare più rapidamente le pagine memorizzate nella cache. Il servizio CDN offre le seguenti funzionalità:

- **Gestione della cache**: memorizza nella cache le pagine del sito, le risorse, i file CSS e altro ancora nei centri dati back-end configurati per ridurre il carico e i costi della larghezza di banda

   - Utilizza [Fastly snippet VCL personalizzati](fastly-vcl-custom-snippets.md) (conforme a Varnish 2.1) per modificare il modo in cui il caching risponde alle richieste

   - Configura il supporto del servizio [GeoIP](fastly-custom-cache-configuration.md#configure-geoip-handling)

   - [Forza richieste non crittografate su TLS](fastly-custom-cache-configuration.md#force-tls)

   - [Personalizza le impostazioni Fastly Timeout](fastly-custom-cache-configuration.md#extend-fastly-timeout) per evitare risposte 503 nelle richieste di operazioni in blocco

   - Crea [pagine di risposta di errore personalizzate](fastly-custom-response.md)

- **Sicurezza** - Dopo aver attivato i servizi Fastly per i siti Adobe Commerce, sono disponibili ulteriori funzionalità di sicurezza per proteggere i siti e la rete:

   - [Firewall applicazione Web](fastly-waf-service.md) (WAF): servizio firewall applicazione Web gestito che fornisce protezione conforme a PCI per bloccare il traffico dannoso prima che possa danneggiare l&#39;Adobe Commerce di produzione sui siti e sulla rete dell&#39;infrastruttura cloud. Il servizio WAF è disponibile solo negli ambienti di produzione Pro e Starter.

   - [Protezione Distributed Denial of Service (DDoS)](#ddos-protection): protezione DDoS incorporata contro attacchi comuni quali Ping of Death, Smurf e altri attacchi di tipo Flood basati su ICMP.

   - [Certificati SSL/TLS](fastly-configuration.md#provision-ssltls-certificates) - Il servizio Fastly richiede un certificato SSL/TLS per gestire il traffico protetto tramite HTTPS.

     Adobe Commerce fornisce un certificato SSL/TLS crittografato con convalida del dominio per ogni ambiente di staging e produzione. Adobe Commerce completa la convalida del dominio e il provisioning dei certificati durante il processo di configurazione Fastly.

- **Mascheramento origine** - Impedisce al traffico di aggirare Fastly WAF e nasconde gli indirizzi IP dei server di origine per proteggerli da accessi diretti e attacchi DDoS.

  Il cloaking dell’origine è abilitato per impostazione predefinita nei progetti di produzione Pro dell’infrastruttura cloud in Adobe Commerce. Per abilitare il cloaking dell&#39;origine su Adobe Commerce nei progetti di Starter Production dell&#39;infrastruttura cloud, invia un [ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Se si dispone di traffico che non richiede il caching, è possibile personalizzare la configurazione del servizio Fastly per consentire alle richieste di [ignorare la cache Fastly](fastly-vcl-bypass-to-origin.md).

- **[Ottimizzazione immagine](fastly-image-optimization.md)**: consente di scaricare il carico di elaborazione e ridimensionamento delle immagini sul servizio Fastly in modo da consentire ai server di elaborare gli ordini e le conversioni in modo più efficiente.

- **[Registri Fastly CDN e WAF](../monitor/new-relic-service.md#new-relic-log-management)**. Per i progetti Adobe Commerce su infrastrutture cloud Pro, puoi utilizzare il servizio Registri di New Relic per rivedere e analizzare i dati di registro Fastly CDN e WAF.

## Modulo CDN Fastly per il Magento 2

I servizi Fastly per Adobe Commerce sull&#39;infrastruttura cloud utilizzano il modulo CDN [Fastly per il Magento 2] installato nei seguenti ambienti: Pro Staging and Production, Starter Production (`master` branch).

Al momento del provisioning iniziale o dell’aggiornamento del progetto Adobe Commerce, Adobe installa la versione più recente del modulo Fastly CDN negli ambienti di staging e produzione. Quando il modulo Fastly viene aggiornato, riceverai notifiche nell’Amministratore per i tuoi ambienti. L’Adobe consiglia di aggiornare gli ambienti per utilizzare la versione più recente. Vedi [Aggiorna Fastly](fastly-configuration.md#upgrade-the-fastly-module).

## Account servizio Fastly e credenziali

Adobe Commerce sui progetti di infrastruttura cloud non dispone di un account Fastly dedicato. Il servizio Fastly viene gestito in un account centralizzato registrato da Adobe e il dashboard di gestione è accessibile solo al team di supporto Cloud.

Al contrario, ogni ambiente di staging e produzione dispone di credenziali Fastly univoche (token API e ID servizio) per configurare e gestire i servizi Fastly dall’amministratore di Commerce. L’API Fastly è disponibile per eseguire la gestione avanzata del servizio Fastly, che richiederà le credenziali per inviare tali richieste.

Durante il provisioning del progetto, Adobe aggiunge il progetto all’account Fastly Service per Adobe Commerce sull’infrastruttura cloud e aggiunge le credenziali Fastly alla configurazione per gli ambienti di staging e produzione. Vedi [Ottieni credenziali rapide](fastly-configuration.md#get-fastly-credentials).

### Modifica token API Fastly

Invia un ticket di supporto Adobe Commerce per modificare le credenziali del token API Fastly. Quando ricevi il nuovo token, aggiorna l’ambiente di staging o produzione per utilizzare il nuovo token.

**Per modificare le credenziali del token API Fastly**:

1. [Invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) richiedendo nuove credenziali API Fastly.

   Includi l’ID del progetto Adobe Commerce su infrastruttura cloud e gli ambienti che richiedono una nuova credenziale.

1. Dopo aver ricevuto il nuovo token API, aggiorna il valore del token API nella [configurazione delle credenziali Fastly](fastly-configuration.md#test-the-fastly-credentials) nell&#39;amministratore o dalle [[!DNL Cloud Console] variabili di ambiente](../project/overview.md#configure-environment).

1. [Verifica le nuove credenziali](fastly-configuration.md#test-the-fastly-credentials).

1. Dopo aver aggiornato le credenziali, invia un ticket di supporto Adobe Commerce per eliminare il vecchio token API.

### Più account Fastly e domini assegnati

Fastly consente di assegnare un dominio APEX e i sottodomini associati a un unico servizio e account Fastly. Se disponi di un account Fastly che collega gli stessi domini e sottodomini APEX utilizzati per il sito Adobe Commerce, puoi scegliere tra le seguenti opzioni:

- Rimuovi il file APEX e i sottodomini dall’account esistente prima di richiedere le credenziali Fastly Service per gli ambienti di progetto dell’infrastruttura cloud di Adobe Commerce. Vedi [Utilizzo dei domini] nella documentazione Fastly.

  Utilizza questa opzione per collegare il dominio apex e tutti i sottodomini all’account Fastly service per Adobe Commerce sull’infrastruttura cloud.

- Invia un ticket di supporto Adobe Commerce per richiedere la delega del dominio in modo che apex e sottodomini possano essere collegati a account diversi.

  Utilizza questa opzione se disponi di un dominio apex con più sottodomini per siti Adobe Commerce e non Adobe Commerce e desideri collegare questi sottodomini a diversi account Fastly.

#### Richiedi delega dominio

*Scenario 1:*

Il dominio apex (`testweb.com` e `www.testweb.com`) è collegato a un account Fastly esistente. È presente un progetto Adobe Commerce su infrastruttura cloud configurato con i seguenti sottodomini: `mcstaging.testweb.com` e `mcprod.testweb.com`. Non spostare il dominio apex sull’account Fastly Service per Adobe Commerce sull’infrastruttura cloud.

Invia un [ticket di supporto Fastly] richiedendo che i sottodomini siano delegati dall&#39;account Fastly esistente all&#39;account Fastly per Adobe Commerce sull&#39;infrastruttura cloud. Includi l’ID del progetto Adobe Commerce nel ticket.

Una volta completata la delega, i sottodomini del progetto possono essere aggiunti all’account Fastly service per Adobe Commerce sull’infrastruttura cloud. Vedi [Ottieni credenziali rapide](fastly-configuration.md#get-fastly-credentials).

*Scenario 2:*

Il dominio apex (`testweb.com` e `www.testweb.com`) è collegato all&#39;account Adobe Commerce on cloud infrastructure Fastly Service. Si desidera gestire i servizi Fastly per i sottodomini `service.testweb.com` e `product-updates.testweb.com` da un account Fastly diverso.

Invia un ticket di supporto Adobe Commerce richiedendo che i sottodomini siano delegati dall’account Adobe Commerce on cloud infrastructure Fastly service all’account Fastly. Includi l’ID servizio per l’account Fastly nel ticket.

## Protezione DoS

La protezione DDOS è integrata nel servizio Fastly CDN. Una volta abilitati i servizi Fastly per i siti Adobe Commerce, Fastly filtra tutto il traffico web e quello dell’amministratore per rilevare e bloccare potenziali attacchi.

- Per gli attacchi che hanno come target il livello 3 o 4, il servizio Fastly filtra il traffico in base alla porta e al protocollo, esaminando solo le richieste HTTP o HTTPS. Gli attacchi ICMP, UDP e altri attacchi avviati dalla rete vengono ignorati all&#39;estremità della nostra rete. Ciò include attacchi di riflessione e amplificazione, che utilizzano servizi UDP come SSDP o NTP. Fornendo questo livello di protezione, blocchiamo efficacemente diversi attacchi comuni come il Ping of Death, gli attacchi Smurf e altre inondazioni basate su ICMP.

  Gestisce in modo rapido gli attacchi a livello TCP a livello di cache. Questa strategia fornisce la scala e il contesto necessari per ogni client per gestire un attacco di tipo SYN flood e le sue numerose varianti, tra cui stack TCP, attacchi di risorse e attacchi TLS all&#39;interno dei sistemi Fastly.

- Fastly fornisce anche protezione contro gli attacchi Layer 7. Se nel tuo negozio si verificano problemi di prestazioni e sospetti un attacco DDoS di Layer 7, invia un ticket di supporto Adobe Commerce. Adobe può creare e applicare regole personalizzate al servizio Fastly per verificare e filtrare le richieste dannose in base a intestazione, payload o una combinazione di attributi che identificano il traffico dell’attacco. Consulta [Verifica degli attacchi DDoS] e [Come bloccare il traffico dannoso] nel *Centro assistenza Adobe Commerce*.

<!--Link definitions-->

[Caching with Fastly]: https://developer.adobe.com/commerce/webapi/graphql/usage/caching/#caching-with-fastly

[Controllo degli attacchi DDoS]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/checking-for-ddos-attack-from-cli.html

[Modulo CDN Fastly per il Magento 2]: https://github.com/fastly/fastly-magento2

[Ticket di supporto rapido]: https://docs.fastly.com/products/support-description-and-sla#support-requests

[Come bloccare il traffico dannoso]: https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/block-malicious-traffic-for-magento-commerce-on-fastly-level.html

[Utilizzo dei domini]: https://docs.fastly.com/en/guides/working-with-domains
