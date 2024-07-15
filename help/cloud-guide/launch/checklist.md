---
title: Elenco di controllo di Launch
description: Verificare gli elementi dell'elenco di controllo per l'avvio del sito.
exl-id: 4525742e-18c5-40d1-975d-00ba3f3a51a0
source-git-commit: 5b0a691a4355f5eda31d42cd3da9925439dfb510
workflow-type: tm+mt
source-wordcount: '1104'
ht-degree: 0%

---

# Elenco di controllo di Launch

Prima di eseguire la distribuzione nell&#39;ambiente di produzione, scaricare l&#39;[elenco di controllo di Launch](../../assets/adobe-commerce-cloud-prelaunch-checklist.pdf) e utilizzarlo con queste istruzioni per verificare di aver completato tutte le operazioni di configurazione e test richieste. Vedi una panoramica del processo di distribuzione completo per Starter e Pro all&#39;indirizzo [Distribuisci il tuo archivio](../deploy/staging-production.md).

## Test completo in produzione

Consulta [Distribuzione dei test](../test/staging-and-production.md) per testare tutti gli aspetti dei siti, degli archivi e degli ambienti. Questi test includono la verifica Fastly, i test di accettazione utente (UAT, User Acceptance Test) e i test delle prestazioni.

## TLS e Fastly

In questo Adobe viene fornito un certificato crittografato SSL/TLS per ogni ambiente. Questo certificato è necessario affinché Fastly possa gestire il traffico protetto tramite HTTPS.

Per utilizzare questo certificato, devi aggiornare la configurazione DNS in modo che Adobe possa completare la convalida del dominio e applicare il certificato all’ambiente. Ogni ambiente dispone di un certificato univoco che copre i domini di Adobe Commerce sui siti dell’infrastruttura cloud implementati in tale ambiente. È consigliabile completare e aggiornare la configurazione durante il [processo di configurazione rapido](../cdn/fastly-configuration.md).

## Aggiornamento della configurazione DNS con le impostazioni di produzione

Quando sei pronto per avviare il sito, devi aggiornare la configurazione DNS per instradare il traffico dall’ambiente di produzione tramite il servizio Fastly.

**Prerequisiti:**

- [Configurazione e test Fastly nell’ambiente di sviluppo](../cdn/fastly-configuration.md#)

- La configurazione dell’ambiente di produzione è stata aggiornata con tutti i domini richiesti

  In genere, si lavora con il proprio consulente tecnico clienti per aggiungere tutti i domini e i sottodomini di livello superiore necessari per gli store. Per aggiungere o modificare i domini per l&#39;ambiente di produzione, [Invia un ticket di supporto Adobe Commerce](https://support.magento.com/hc/en-us/articles/360019088251). Attendi la conferma che la configurazione del progetto è stata aggiornata.

  Nei progetti iniziali, devi aggiungere i domini al progetto. Vedi [Gestione domini](../cdn/fastly-custom-cache-configuration.md#manage-domains).

- È stato eseguito il provisioning del certificato SSL/TLS per gli ambienti di produzione.

  Se hai aggiunto i record di verifica ACME per i domini di produzione durante la procedura di configurazione Fastly, Adobe carica automaticamente il certificato SSL/TLS nell’ambiente di produzione quando aggiorni la configurazione DNS per indirizzare il traffico al servizio Fastly. Se non hai eseguito il preprovisioning del certificato o se hai aggiornato i tuoi domini, Adobe deve completare la convalida del dominio ed eseguire il provisioning del certificato, che può richiedere fino a 12 ore.

### Per aggiornare la configurazione DNS per l&#39;avvio del sito:

1. Aggiorna la seguente configurazione DNS per il sito di produzione:

   - Imposta tutti i reindirizzamenti necessari, soprattutto se stai eseguendo la migrazione da un sito esistente

   - Imposta il record di risorse radice della zona per indirizzare il nome host

   - Abbassa il valore TTL (Time-to-Live) per aggiornare le informazioni DNS in modo da indirizzare i clienti all’archivio di produzione corretto

     È consigliabile impostare un valore TTL significativamente inferiore quando si cambia il record DNS. Questo valore indica al DNS per quanto tempo memorizzare in cache il record DNS. In caso di riduzione, il DNS viene aggiornato più rapidamente. Ad esempio, puoi modificare il valore TTL da tre giorni a 10 minuti durante l’aggiornamento del sito. Tieni presente che la riduzione del valore TTL aggiunge un carico all’infrastruttura DNS. Ripristina il valore precedente più alto dopo l’avvio del sito.


1. Aggiungere record CNAME per far puntare i sottodomini dell&#39;ambiente di produzione al servizio Fastly `prod.magentocloud.map.fastly.net`, ad esempio:

   | Dominio o sottodominio | CNAME |
   | ----------------------- | -------------------------------- |
   | `www.<domain-name>.com` | prod.magentocloud.map.fastly.net |
   | `mystore.<domain-name>.com` | prod.magentocloud.map.fastly.net |

1. Se necessario, aggiungere i record A per mappare il dominio apex (`<domain-name>.com`) ai seguenti indirizzi IP Fastly:

   | Dominio apex | NOME |
   | --------------- | ----------------- |
   | `<domain-name>.com` | `151.101.1.124` |
   | `<domain-name>.com` | `151.101.65.124` |
   | `<domain-name>.com` | `151.101.129.124` |
   | `<domain-name>.com` | `151.101.193.124` |

>[!IMPORTANT]
>
>Le istruzioni DNS in [RFC1034](https://www.rfc-editor.org/rfc/rfc1912) (**sezione 2.4**) indicano che:
>_Un record CNAME non può coesistere con altri dati. In altre parole, se suzy.podunk.xx è un alias per sue.podunk.xx, non è possibile avere un record MX per suzy.podunk.edu, un record A o persino un record TXT._
>
>Per questo motivo, i record DNS devono essere di tipo `CNAME` per i sottodomini e di tipo `A` per i domini APEX (domini radice). L’eliminazione di questa regola può causare interruzioni del servizio di posta o della propagazione DNS, perché non è più possibile aggiungere altri record, ad esempio MX o NS. Alcuni provider DNS possono aggirare questo problema utilizzando personalizzazioni interne, ma il rispetto dello standard garantisce stabilità e flessibilità (ad esempio la modifica del provider DNS).

1. Aggiornare l’URL di base.

   - Utilizza SSH per accedere all’ambiente di produzione.

     ```bash
     magento-cloud ssh -e production
     ```

   - Utilizza CLI per modificare l’URL di base per lo store.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://www.<domain-name>.com/"
     ```

   **NOTA**: puoi anche aggiornare l&#39;URL di base dall&#39;amministratore. Consulta [URL store](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html) nella _Guida agli store e all&#39;esperienza di acquisto di Adobe Commerce_.

1. Attendi alcuni minuti per l’aggiornamento del sito.

1. Verifica il sito.

## Verifica la configurazione di produzione

Esegui un passaggio finale per convalidare la configurazione di produzione per uno o più archivi. Puoi aggiornare la configurazione nell’ambiente di produzione. Se le impostazioni sono di sola lettura, potrebbe essere necessario aprire una connessione SSH e utilizzare i comandi CLI per modificare la configurazione o apportare modifiche alla configurazione nell’ambiente locale. Dopo aver completato gli aggiornamenti, puoi distribuire le modifiche negli ambienti di staging e produzione.

Di seguito sono riportate le modifiche e i controlli consigliati:

- [Test e-mail in uscita completato](../project/outgoing-emails.md)

- [Configurazione sicura per le credenziali amministratore e URL amministratore di base](https://docs.magento.com/user-guide/stores/security-admin.html)

- [Ottimizza tutte le immagini per il web](../cdn/fastly-image-optimization.md)

- [Verifica le impostazioni di minimizzazione per HTML, JavaScript e CSS](../deploy/static-content.md)

## Verificare il caching rapido

- Verifica e controlla che il caching Fastly funzioni correttamente sul sito di produzione. Per ulteriori verifiche e test, vedere [Test rapidi](../test/staging-and-production.md#check-fastly-caching).

- [Verifica che nell’ambiente di produzione sia installata la versione più recente del modulo Fastly CDN per Commerce](../cdn/fastly-configuration.md#upgrade-the-fastly-module)

- [Verifica che sia stata caricata la versione più recente del codice VCL Fastly](../cdn/fastly-configuration.md#upload-vcl-to-fastly)

## Test delle prestazioni

È consigliabile esaminare le opzioni di [Performance Toolkit](https://github.com/magento/magento2/tree/2.4/setup/performance-toolkit) nell&#39;ambito del processo di preparazione precedente all&#39;avvio.

Puoi eseguire il test anche utilizzando le seguenti opzioni di terze parti:

- [Assedio](https://www.joedog.org/siege-home/): verifica e definizione del traffico del software per spingere l&#39;archivio al limite. Visita il tuo sito con un numero configurabile di client simulati. Siege supporta l’autenticazione di base, i cookie, i protocolli HTTP, HTTPS e FTP.

- [Jmeter](https://jmeter.apache.org/): test di carico eccellenti per valutare le prestazioni per il traffico picco, come nel caso delle vendite flash. Crea test personalizzati da eseguire sul sito.

- [New Relic](https://support.newrelic.com/s/) (fornito): consente di individuare i processi e le aree del sito causando un rallentamento delle prestazioni con tempo di rilevamento trascorso per azione, ad esempio la trasmissione di dati, query, Redis e altro ancora.

- [WebPageTest](https://www.webpagetest.org/) e [PKingdom](https://www.pingdom.com/): l&#39;analisi in tempo reale delle pagine del sito viene caricata con percorsi di origine diversi. Il pnl può costare una tassa. WebPageTest è uno strumento gratuito.

## Configurazione della sicurezza

- [Configurare l&#39;analisi della protezione](overview.md#set-up-the-security-scan-tool)

- [Configurazione sicura per l&#39;utente amministratore](https://docs.magento.com/user-guide/stores/security-admin.html)

- [Configurazione sicura per URL amministratore](https://docs.magento.com/user-guide/stores/store-urls-custom-admin.html)

- [Rimuovere gli utenti che non fanno più parte del progetto di infrastruttura cloud di Adobe Commerce](../project/user-access.md)

- [Configura autenticazione a due fattori](https://devdocs.magento.com/guides/v2.4/security/two-factor-authentication.html)

## Monitoraggio delle prestazioni

È possibile utilizzare i servizi New Relic per il monitoraggio delle prestazioni in ambienti Pro e Starter. Negli account Pro plan, vengono forniti gli avvisi gestiti per i criteri di avviso di Adobe Commerce per monitorare le prestazioni delle applicazioni e dell&#39;infrastruttura utilizzando gli agenti New Relic APM e Infrastructure. Per informazioni dettagliate sull&#39;utilizzo di questi servizi, vedere [Monitorare le prestazioni con avvisi gestiti](../monitor/investigate-performance.md#monitor-performance-with-managed-alerts).

### Passaggio successivo

[Passaggi di avvio](steps.md)
