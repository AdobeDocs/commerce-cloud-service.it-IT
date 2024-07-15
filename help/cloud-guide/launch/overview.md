---
title: Lancio del sito
description: Scopri come iniziare la preparazione per il lancio del sito.
exl-id: a7b3f260-b76e-4220-b521-699548a9928a
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '902'
ht-degree: 0%

---

# Lancio del sito

Dopo aver completato la distribuzione e il test negli ambienti di integrazione e staging, puoi iniziare la preparazione dell’avvio del sito. Innanzitutto, devi completare tutte le attività di sviluppo e test prima di lavorare nell’ambiente di produzione. Ti senti pronto per il lancio? Consulta elenchi di controllo, best practice e passaggi finali per avviare il sito.

Se hai controllato queste informazioni prima di implementare e testare in Staging, valuta la possibilità di esaminare i vantaggi del test in Staging prima nella sezione successiva. La gestione temporanea è un ambiente di produzione in esecuzione su hardware, configurazioni, architettura e servizi simili. Consente di ridurre i tempi di inattività e di rendere vitali i componenti di estensione, servizio, configurazioni personalizzate e test di accettazione da parte degli esercenti per il rilascio di siti e store.

## Perché eseguire test completi di integrazione, staging e produzione?

Consigliamo vivamente di eseguire test negli ambienti di integrazione, staging e produzione a causa della complessità di garantire che codice personalizzato, temi, estensioni e integrazioni di terze parti funzionino tutti insieme per gestire i tuoi store. Di seguito sono riportati i problemi comuni che puoi individuare e risolvere quando completi il test negli ambienti di integrazione e staging prima di aggiornare l’ambiente di produzione:

- La gestione temporanea supporta tutti i servizi di produzione, le funzioni, i dati del database, lo stack di tecnologia, l’architettura e altro ancora. Riflette la Produzione, il che significa che se si verificano errori in Staging, viene visualizzato un avviso prima che si verifichino in Produzione.

- Il codice che funziona nell’ambiente di integrazione locale potrebbe non funzionare negli ambienti di staging e produzione.

- Gli ambienti di integrazione non supportano alcuni servizi disponibili in Staging e Produzione, come Fastly e New Relic.

- [Eseguire un test completo](../test/guidance.md) del sito con vari strumenti in Gestione temporanea per verificare il carico, lo stress, le prestazioni e le risorse del sito.

- Poiché gli ambienti di integrazione possono contenere solo database compilati con dati di test, che non corrispondono a un ambiente di produzione, è possibile che si verifichino ulteriori errori o comportamenti imprevisti durante il test negli ambienti di staging o produzione.

## Prerequisiti per l’avvio del sito

Per preparare il lancio del sito sono necessarie le seguenti informazioni e risorse:

- Informazioni sui record CNAME per la configurazione del DNS

- Elenco di tutti i domini storefront da aggiungere al certificato

- Certificato SSL/TLS

Come parte dell’abbonamento a un’infrastruttura cloud di Adobe Commerce, Adobe fornisce un certificato SSL/TLS convalidato dal dominio rilasciato da Let’s Encrypt. Ogni ambiente Pro Production, Staging e Starter Production (`master`) dispone di un certificato univoco che copre tutti i domini e i sottodomini di tale ambiente. Questi certificati vengono predisposti e caricati automaticamente sul sito dopo l’aggiornamento della configurazione DNS per lo sviluppo e la produzione. Vedere [Provisioning dei certificati SSL/TLS](../cdn/fastly-configuration.md#provision-ssltls-certificates).

>[!NOTE]
>
>Se desideri distribuire il tuo certificato SSL di convalida estesa per la tua società invece di utilizzare il certificato Let&#39;s Encrypt, contatta il tuo CTA o [Invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

## Configurare lo strumento Security Scan

>[!NOTE]
>
>Lo strumento di analisi della sicurezza utilizza i seguenti indirizzi IP pubblici:
>
>```text
>52.87.98.44
>34.196.167.176
>3.218.25.102
>```
>
>Aggiungere questi indirizzi IP a un elenco Consentiti di protezione del firewall di rete per consentire allo strumento di eseguire la scansione del sito. Lo strumento invia richieste solo alle porte 80 e 443.

Lo strumento di analisi della sicurezza consente di monitorare regolarmente i siti Web dei negozi e ricevere aggiornamenti per i rischi di sicurezza noti, malware e software obsoleto. Questo strumento è un servizio gratuito disponibile per tutte le implementazioni e le versioni di Adobe Commerce sull’infrastruttura cloud. Puoi accedere allo strumento tramite il tuo [account di Commerce Marketplace](https://account.magento.com/customer/account/login).

- Monitorare lo stato di protezione dei siti e gli aggiornamenti di protezione applicati

- Ricevi aggiornamenti sulla sicurezza e notifiche specifiche per il sito

Per informazioni sulla configurazione e l&#39;utilizzo dello strumento di analisi della sicurezza, consultare la [Guida utente](https://docs.magento.com/user-guide/magento/security-scan.html). In genere, si inizia a utilizzare questo strumento quando si inizia il test di accettazione utente (UAT).

Ogni sito digitalizzato deve essere registrato tramite la scheda Security Scan. Durante il processo di registrazione, è necessario accettare la liberatoria prima di poter iniziare la scansione. Puoi controllare sia la pianificazione che autorizzare l’utente a ricevere notifiche al termine di ogni scansione. È possibile pianificare scansioni per una data e un&#39;ora specifiche e ricorrenti oppure eseguire una scansione su richiesta in base alle esigenze.

Lo strumento Security Scan utilizza diverse stringhe dell’agente utente per simulare attività malware reali. Potresti visualizzare i seguenti agenti utente nei registri di analisi o di accesso:

```text
Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:57.0) Gecko/20100101 Firefox/57.0
GuzzleHttp/6.3.3 curl/7.29.0 PHP/7.1.18
Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/37.0.2062.120 Safari/537.36
Visbot/2.0 (+http://www.visvo.com/en/webmasters.jsp;bot@visvo.com)
```

## Analizzare il sito

1. Accedi al tuo [account di Commerce Marketplace](https://account.magento.com/customer/account/login).

1. Fare clic sulla scheda Analisi protezione e selezionare **Vai a Analisi protezione**.

1. Nella colonna _Azioni_ del sito selezionare **Esegui analisi**. Lo stato di una notifica visualizza l’analisi pianificata.

### Per esaminare il rapporto:

1. Al termine del rapporto, viene visualizzata una notifica.

1. Nella riga del sito selezionare il report che si desidera visualizzare dalla colonna **Report**. L’ordine è dal più recente al meno recente.

Il rapporto elenca i problemi che includono analisi non riuscite, risultati non identificati e analisi riuscite. Ogni voce fornisce informazioni dettagliate sulla scansione, un elenco dei problemi da analizzare e le azioni da intraprendere. Alcune di queste azioni possono richiedere il download e l&#39;installazione di patch di sicurezza. Aggiungere le patch richieste a un ramo di sviluppo sulla workstation locale prima di aggiungerle al ramo di produzione.

I risultati della scansione includono un’etichetta che descrive lo stato di superamento o errore della scansione con informazioni dettagliate sui controlli eseguiti:

- &quot;Non riuscito&quot; indica che il sito web contiene una vulnerabilità grave.

- &quot;Non identificato&quot; suggerisce che il team o il provider di hosting richiede una revisione più approfondita per determinare se è necessaria un’ulteriore azione.

I risultati della scansione forniscono anche i passaggi di correzione consigliati per ogni test di sicurezza non riuscito. I risultati dell&#39;analisi di sicurezza sono protetti e visualizzabili solo dall&#39;utente registrato. Solo gli utenti designati nel processo di registrazione del sito ricevono le notifiche di completamento dell&#39;analisi.

## Pronto per avviare il sito

Quando sei pronto per iniziare il processo di lancio del sito, vedi quanto segue:

- [Elenco di controllo di Launch](checklist.md)

- [Passaggi di avvio](steps.md)
