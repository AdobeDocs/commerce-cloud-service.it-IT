---
title: Stack tecnologico
description: Scopri lo stack di tecnologia che forma l’infrastruttura Commerce on Cloud.
feature: Cloud, Iaas, Paas
exl-id: e456db25-c44b-4053-b96d-517d3d1606d0
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 0%

---

# Stack tecnologico

Considera Adobe Commerce sull’infrastruttura cloud come cinque livelli funzionali, come mostrato di seguito:

![Stack cloud](../../assets/CloudStack.svg)

1. [**Infrastruttura cloud**](pro-architecture.md): scegli Amazon Web Services (AWS) o Microsoft Azure as your Infrastructure as a Service (IaaS) foundation per i progetti Adobe Commerce on Cloud Infrastructure Pro.

   Adobe analizza regolarmente l&#39;utilizzo delle risorse di elaborazione virtuale (vCPU) e alloca automaticamente le risorse per ottimizzare l&#39;utilizzo a lungo termine e ridurre i rischi di superamento del limite giornaliero massimo annuo di vCPU. Se prevedi un aumento del traffico del sito per specifici periodi di tempo, devi continuare ad aprire un ticket di supporto per [richiedi un upsize temporaneo](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html).

1. [**Platform as a Service**](cloud-architecture.md): ogni progetto di infrastruttura cloud Adobe Commerce on fornisce un ambiente di integrazione Platform as a Service (PaaS) per lo sviluppo, il test e l’integrazione dei servizi.
1. [**Adobe Commerce**](../project/overview.md): Adobe Commerce sull’infrastruttura cloud fornisce un’infrastruttura prefornita che include PHP, MySQL (MariaDB), Redis, [!DNL RabbitMQ]e tecnologie di motori di ricerca supportate.
1. [**Strumenti di prestazioni**](../monitor/new-relic-service.md): gli strumenti per le prestazioni di New Relic consentono di eseguire il debug, monitorare e gestire le applicazioni e l’infrastruttura raccogliendo, analizzando e visualizzando i dati del tuo Adobe Commerce sui progetti di infrastruttura cloud.
1. [**CDN (Content Delivery Network), Firewall applicazione Web ([!DNL WAF]) e Image Optimization (IO)**](../cdn/fastly.md):

   * [Fastly CDN](../cdn/fastly.md#ddos-protection)- Fornisce servizi CDN sicuri con protezione integrata da attacchi Distributed Denial of Service (DDoS) come [!DNL Ping of Death], [!DNL Smurf] e altri attacchi di tipo Flood Protocol (ICMP) basati su Internet Control Message Protocol.
   * [Firewall applicazione Web (WAF)](../cdn/fastly-waf-service.md): i servizi WAF garantiscono la conformità PCI per i punti vendita Adobe Commerce negli ambienti di produzione e le policy WAF che proteggono le applicazioni web Adobe Commerce da attacchi di iniezione, input dannosi, vulnerabilità cross-site scripting, exfiltrazione dei dati, violazioni del protocollo HTTP e altro [[!DNL OWASP] Le dieci principali minacce per la sicurezza](https://owasp.org/www-project-top-ten/).
   * [Ottimizzazione immagine (IO)](../cdn/fastly-image-optimization.md)- Manipolazione e ottimizzazione delle immagini in tempo reale per velocizzare la distribuzione delle immagini e semplificare la manutenzione dei set di origini delle immagini per le applicazioni web dinamiche. L&#39;I/O veloce consente di ridurre il carico di elaborazione e di ridimensionamento delle immagini, consentendo ai server di elaborare gli ordini e le conversioni in modo efficiente.

Le applicazioni monolitiche richiedono un uso intensivo delle risorse, sono difficili da scalare e da gestire rapidamente. Con l’infrastruttura Cloud, i clienti Commerce hanno un accesso senza precedenti ai microservizi basati su SaaS che sono ricchi, intelligenti e performanti. Consulta [Software e servizi supportati](cloud-architecture.md#supported-software-and-services).

Utilizza il [Guida introduttiva di Commerce](../../get-started/overview.md) per configurare il nuovo programma Cloud e iniziare a gestire il [!DNL Commerce] in un ambiente nativo per il cloud.
