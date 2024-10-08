---
title: Firewall applicazione Web (WAF)
description: Scopri in che modo il servizio Fastly WAF rileva, registra e blocca il traffico di richieste dannoso prima che possa danneggiare la rete o i siti Adobe Commerce.
feature: Cloud, Configuration, Security
exl-id: 40bfe983-7f32-4155-ae77-7cd18866f6e2
source-git-commit: 48ac1759fc052175e01998703e7f4ee5eaac5224
workflow-type: tm+mt
source-wordcount: '888'
ht-degree: 0%

---

# Firewall applicazione Web (WAF)

Basato su Fastly, il servizio web application firewall (WAF) per Adobe Commerce sull’infrastruttura cloud rileva, registra e blocca il traffico delle richieste dannoso prima che possa danneggiare i siti o la rete. Il servizio WAF è disponibile solo negli ambienti di produzione.

Il servizio WAF offre i seguenti vantaggi:

- **Conformità PCI**: l&#39;abilitazione di WAF garantisce che gli storefront Adobe Commerce negli ambienti di produzione soddisfino i requisiti di sicurezza PCI DSS 6.6.
- **Criterio predefinito di WAF**: il criterio predefinito di WAF, configurato e gestito da Fastly, fornisce una raccolta di regole di sicurezza personalizzate per proteggere le applicazioni Web Adobe Commerce da un&#39;ampia gamma di attacchi, tra cui attacchi di iniezione, input dannosi, vulnerabilità cross-site scripting, exfiltrazione dei dati, violazioni del protocollo HTTP e altre [minacce di sicurezza OWASP Top Ten](https://owasp.org/www-project-top-ten/).
- **Onboarding e abilitazione di WAF**: Adobe distribuisce e abilita i criteri predefiniti di WAF nell&#39;ambiente di produzione entro 2-3 settimane dal completamento del provisioning.
- **Supporto operazioni e manutenzione**—
   - Adobe e Fastly imposta e gestisci registri, regole e avvisi per il servizio WAF.
   - L’Adobe esegue il triage dei ticket di assistenza clienti relativi ai problemi del servizio WAF che bloccano il traffico legittimo come problemi con priorità 1.
   - Gli aggiornamenti automatizzati alla versione del servizio WAF garantiscono una copertura immediata per gli exploit nuovi o in evoluzione. Consulta [Manutenzione e aggiornamenti di WAF](#waf-maintenance-and-updates).

>[!TIP]
>
>Per ulteriori informazioni sul mantenimento della conformità PCI per gli archivi dell&#39;infrastruttura cloud Adobe Commerce, vedere [Conformità PCI](https://business.adobe.com/products/magento/pci-compliance.html).

## Abilitazione di WAF

Adobe abilita il servizio WAF sui nuovi account entro 2-3 settimane dopo il provisioning finale. WAF viene implementato tramite il servizio Fastly CDN. Non è necessario installare o gestire hardware o software.

>[!NOTE]
>
>Prima di poter utilizzare il servizio WAF, è necessario indirizzare tutto il traffico esterno al progetto di infrastruttura cloud Adobe Commerce on all’interno del servizio Fastly. Vedi [Configura Fastly](fastly-configuration.md).

## Come funziona

Il servizio WAF si integra con Fastly e utilizza la logica della cache all’interno del servizio Fastly CDN per filtrare il traffico nei nodi globali Fastly. Il servizio WAF viene attivato nell&#39;ambiente di produzione con un criterio WAF predefinito basato su [Regole di sicurezza ModSecurity di Trustwave SpiderLabs](https://github.com/owasp-modsecurity/ModSecurity) e sulle dieci minacce di sicurezza OWASP Top Ten.

Il servizio WAF ispeziona il traffico HTTP e HTTPS (richieste GET e POST) in base al set di regole WAF e blocca il traffico dannoso o non conforme a regole specifiche. Il servizio controlla solo il traffico associato all’origine che tenta di aggiornare la cache. Di conseguenza, la maggior parte del traffico di attacchi viene interrotta nella cache Fastly, proteggendo il traffico di origine da attacchi dannosi. Elaborando solo il traffico di origine, il servizio WAF mantiene le prestazioni della cache, introducendo solo una latenza stimata da 1,5 a 20 millisecondi per ogni richiesta non memorizzata in cache.

## Risoluzione dei problemi relativi alle richieste bloccate

Quando il servizio WAF è abilitato, analizza tutto il traffico web e di amministrazione in base alle regole di WAF e blocca eventuali richieste web che attivano una regola. Quando una richiesta viene bloccata, il richiedente visualizza una pagina di errore `403 Forbidden` predefinita che include un ID di riferimento per l&#39;evento di blocco.

![Pagina di errore WAF](../../assets/cdn/fastly-waf-403-error.png)

Puoi personalizzare questa pagina di risposta all’errore dall’Amministratore. Consulta [Personalizzare la pagina di risposta di WAF](fastly-custom-response.md#customize-the-waf-error-page).

Se la pagina di amministrazione o la vetrina di Adobe Commerce restituisce una pagina di errore `403 Forbidden` in risposta a una richiesta URL legittima, invia un [ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Copia l’ID di riferimento dalla pagina di risposta dell’errore e incollalo nella descrizione del ticket.

## Manutenzione e aggiornamenti di WAF

Fastly aggiorna e implementa patch per nuovi CVE/regole basate su modelli basate su aggiornamenti di regole di terze parti commerciali, ricerca Fastly e open source. Aggiorna in modo rapido le regole pubblicate in una policy in base alle esigenze o quando sono disponibili modifiche alle regole dalle rispettive sorgenti. Inoltre, Fastly può aggiungere regole che corrispondono alle classi pubblicate di regole nell’istanza WAF di qualsiasi servizio dopo l’abilitazione del servizio WAF. Questi aggiornamenti garantiscono una copertura immediata per gli exploit nuovi o in evoluzione.

Adobe e Fastly gestiscono il processo di aggiornamento per garantire che le regole WAF nuove o modificate funzionino in modo efficace nell’ambiente di produzione prima che gli aggiornamenti vengano distribuiti in modalità di blocco.

## Problemi

Se scopri che WAF blocca le richieste legittime, spesso si tratta di falsi positivi che devono essere ignorati o per i quali è stata implementata una soluzione alternativa nel servizio WAF. Invia un ticket di supporto e includi l’URL interessato, i passaggi esatti per riprodurre l’errore e il riferimento dell’errore in formato testo (anziché uno screenshot) per evitare errori di trascrizione.

## Limitazioni

Il servizio WAF standard fornito da Fastly non supporta le seguenti funzionalità:

- Protezione da malware o mitigazione bot: è consigliabile utilizzare [elenchi di controllo di accesso](./fastly-vcl-allowlist.md) o un servizio di terze parti.
- Limitazione di frequenza: vedere [Limitazione di frequenza](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/RATE-LIMITING.md) nella documentazione Fastly oppure [Limitazione di frequenza](https://developer.adobe.com/commerce/webapi/get-started/rate-limiting/) nella sezione di protezione _API Web Commerce_.
- Configurazione di un endpoint di registrazione per il cliente. Vedere [Servizio PrivateLink](../development/privatelink-service.md) come alternativa.

Il servizio WAF ti consente di bloccare o consentire il traffico in base agli indirizzi IP. È possibile aggiungere elenchi di controllo di accesso (ACL) e snippet VCL personalizzati al servizio Fastly per specificare gli indirizzi IP e la logica VCL per bloccare o consentire il traffico. Vedi [Frammenti personalizzati VCL Fastly](fastly-vcl-custom-snippets.md).

Il filtro per le richieste TCP, UDP o ICMP non è supportato dal servizio WAF. Tuttavia, questa funzionalità è fornita dalla protezione DDoS incorporata inclusa con il servizio Fastly CDN. Vedere [Protezione DDoS](fastly.md#ddos-protection).
