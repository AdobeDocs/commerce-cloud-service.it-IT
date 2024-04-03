---
title: Linee guida per i test
description: Scopri i tipi di test e le best practice per l’avvio di Adobe Commerce sull’infrastruttura cloud.
exl-id: 1d7897a2-af97-46ab-89c0-2ec54284190e
source-git-commit: aae285d85188bfadd73d5b4e1fea16afa5beb117
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 0%

---

# Linee guida per i test

Dopo aver configurato e personalizzato il progetto Adobe Commerce su infrastruttura cloud, è consigliabile testare l’applicazione in modo approfondito prima di avviare il sito web dello store. I test consentono di gestire correttamente le aspettative relative alle dimensioni del cluster e di adattarsi in modo appropriato alle esigenze aziendali future.

## Test funzionali

In fase di sviluppo, è importante eseguire test funzionali end-to-end sul progetto di infrastruttura cloud di Adobe Commerce. Per eseguire i test funzionali nell’ambiente Docker, consulta le seguenti linee guida:

- **Test delle applicazioni**- Utilizza la [Framework Magento di test funzionali (MFTF)](https://developer.adobe.com/commerce/cloud-tools/docker/test/application-testing/) per il test delle applicazioni nell’ambiente Cloud Docker.

- **Test del codice**- Utilizza la [Framework di test di codecezione per PHP](https://developer.adobe.com/commerce/cloud-tools/docker/test/code-testing/) per convalidare il codice destinato a contribuire agli archivi di pacchetti Cloud.

## Best practice prima del lancio

Considera i seguenti tipi di test come best practice da eseguire prima dell’avvio del sito:

- **Prova di carico**- Eseguire un test di carico per comprendere il comportamento del sistema in relazione al carico previsto. Ad esempio, verifica un numero concorrente di utenti attivi nell’applicazione, chiedendo a ogni utente di eseguire un numero specifico di transazioni entro la durata impostata. Questo test rivela il tempo di risposta di importanti transazioni business-critical, ad esempio il comportamento del database o del server applicazioni. Un test di carico può aiutare a identificare i colli di bottiglia.

- **Stress test**- Sfidare i limiti superiori di capacità all&#39;interno del sistema per determinare se il sistema funziona in modo sufficiente quando il carico corrente supera di gran lunga il massimo previsto.

- **Security Scan**- L&#39;Adobe fornisce un [Strumento Security Scan](../launch/overview.md#set-up-the-security-scan-tool) per i siti.

- **Prova di penetrazione**- È un attacco informatico simulato autorizzato su un sistema informatico progettato per valutare la sicurezza del sistema. Il test di penetrazione aiuta a individuare punti deboli o vulnerabilità, tra cui la possibilità che soggetti non autorizzati accedano alle funzioni e ai dati del sistema.

>[!WARNING]
>
>Ai clienti non è consentito condurre valutazioni sulla sicurezza dell’infrastruttura AWS o dei servizi AWS stessi. Se riscontri un problema di sicurezza in uno qualsiasi dei servizi AWS osservati nella valutazione della sicurezza, [contatta AWS Security](mailto:aws-security@amazon.com) immediatamente. Consulta [Criteri del cliente AWS per il test di penetrazione](https://aws.amazon.com/security/penetration-testing/).
