---
title: Accedere al pannello di amministrazione di Commerce
description: Scopri come accedere al pannello di amministrazione di Commerce.
recommendations: noDisplay, catalog
exl-id: 9a8a0a49-b108-48bd-b413-ec9431370c06
source-git-commit: 3ca09243dc0a714c1d86cccf9f0620a8a39fd1e1
workflow-type: tm+mt
source-wordcount: '323'
ht-degree: 0%

---

# Accedere al pannello di amministrazione di Commerce

Gli utenti con accesso amministrativo al pannello di amministrazione di Commerce possono aggiungere utenti, configurare servizi di store, completare il lavoro di configurazione e personalizzazione dello store e altro ancora.

Per un nuovo progetto, il primo passaggio dopo aver ricevuto l’e-mail di benvenuto è proteggere l’accesso dell’amministratore al progetto modificando la password sull’account Proprietario della licenza. Il nome utente predefinito per questo account è l’indirizzo e-mail del Proprietario della licenza.

È possibile inviare una richiesta di modifica della password utilizzando uno dei metodi seguenti:

- Individua l’e-mail di benvenuto inviata all’indirizzo e-mail del Proprietario della licenza, segui il collegamento e cambia la password.

- Copia l’URL dell’archivio da [[!DNL Cloud Console]](../cloud-guide/project/overview.md) in un browser. Quindi, aggiungi `/admin` alla fine dell&#39;URL per aprire la pagina di accesso. Fai clic su **Password dimenticata?** collegamento per inviare una richiesta di modifica della password all’indirizzo e-mail del Proprietario della licenza.

Dopo aver inviato la richiesta di modifica della password, controllare l&#39;e-mail per la notifica di reimpostazione della password. Se non ricevi l’e-mail, controlla la cartella di posta indesiderata.

>[!TIP]
>
>Se la reimpostazione della password non riesce o non è possibile accedere al pannello Amministratore, un utente con accesso amministratore può connettersi al progetto utilizzando SSH e aggiungere un utente amministratore utilizzando `admin:user:create` CLI. Consulta [Creare, modificare o sbloccare un account amministratore](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/tutorials/admin.html) nel _Guida all’installazione_.

## Monitorare lo stato del sito

Il [Strumento di analisi a livello di sito](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/intro) è uno strumento self-service proattivo e un archivio centrale che include informazioni dettagliate sul sistema e raccomandazioni per garantire la sicurezza e l’operabilità dell’installazione di Adobe Commerce. Fornisce monitoraggio delle prestazioni in tempo reale, report e consigli 24 ore su 24, 7 giorni su 7 per identificare potenziali problemi e migliorare la visibilità delle configurazioni di salute, sicurezza e applicazioni del sito. Consente di ridurre i tempi di risoluzione e di migliorare la stabilità e le prestazioni del sito. Puoi accedere allo strumento di analisi a livello di sito direttamente dal [Pannello di amministrazione](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-2-logging-in-to-your-site-wide-analysis-tool-dashboard-from-your-stores-admin-panel) o dall&#39; [dominio dedicato](https://experienceleague.adobe.com/en/docs/commerce-operations/tools/site-wide-analysis-tool/access#option-1-logging-in-to-your-site-wide-analysis-tool-dashboard-directly-from-the-site-wide-analysis-tool-domain-for-adobe-commerce-on-cloud-infrastructure-only) (solo per progetti Adobe Commerce su infrastrutture cloud).
