---
title: Installazione senza downtime
description: Scopri come ridurre i tempi di inattività complessivi durante l’implementazione di Adobe Commerce su progetti di infrastruttura cloud.
feature: Cloud, Deploy, SCD, Themes
exl-id: ff89d2e1-dfc8-4f6d-bd98-947559af13f0
source-git-commit: 225fba1acfd8b3ce4d7ce989c7851e7b0b218680
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# Installazione senza downtime

Adobe Commerce su infrastruttura cloud esegue l’applicazione in [_manutenzione_ modalità](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/setup/application-modes.html#production-mode) durante la fase di distribuzione, che porta il sito offline fino al completamento della distribuzione. Il periodo di tempo in cui il sito di produzione è in modalità di manutenzione dipende dalle dimensioni del sito, dal numero di modifiche applicate durante la distribuzione e dalla configurazione per la distribuzione di contenuto statico. È possibile configurare il progetto in modo che venga distribuito con **zero** interruzione delle attività.

Durante il processo di distribuzione, tutte le connessioni si accodano per un massimo di 5 minuti mantenendo tutte le sessioni attive e le azioni in sospeso, ad esempio l’aggiunta al carrello o l’estrazione. Dopo la distribuzione, la coda viene rilasciata e le connessioni continuano senza interruzioni. Per utilizzare questo _blocco connessione_ a tuo vantaggio e riduci l&#39;implementazione a _zero_ tempi di inattività, è necessario configurare il progetto in modo da utilizzare la strategia di distribuzione più efficiente.

Utilizza i seguenti passaggi per ridurre il tempo necessario allo store per distribuire un aggiornamento in produzione:

1. [Esegui l’aggiornamento a `ece-tools` pacchetto](../dev-tools/install-package.md) o [aggiorna il `ece-tools` version](../dev-tools/update-package.md)
Il progetto Adobe Commerce su infrastruttura cloud deve disporre dell’ultima versione `ece-tools` in modo da disporre degli strumenti necessari per configurare una distribuzione ottimale. Se si dispone della più recente `ece-tools`, continuare con il passaggio successivo.

   >[!NOTE]
   >
   >Anche se è una best practice utilizzare i più recenti `ece-tools` , il metodo di distribuzione senza tempi di inattività funziona con `ece-tools` [versione 2002.0.13](../release-notes/cloud-release-archive.md#v2002013) e più tardi.

1. [Configurare la distribuzione di contenuti statici](static-content.md)
Se la distribuzione del contenuto statico non riesce nella fase di distribuzione, il sito si blocca in modalità di manutenzione. Quando si verifica un errore durante la fase di build, il processo evita i tempi di inattività perché non inizia mai la fase di distribuzione. [Generazione di contenuto statico durante la fase di build con minimified HTML](static-content.md#setting-the-scd-on-build), noto anche come stato ideale, è la configurazione ottimale per le distribuzioni senza tempi di inattività e _impedisce_ tempi di inattività in caso di errore.

1. [Configurare l’hook post-distribuzione](../application/hooks-property.md)
Devi configurare l’hook post-distribuzione per pulire e riscaldare la cache. Per impostazione predefinita, la pulizia della cache si verifica durante la fase di distribuzione quando il sito è inattivo. Se si sposta la cache nella fase di post-distribuzione, la cache rimane attiva fino al completamento della fase di distribuzione, quindi è possibile pulire la cache in modo sicuro.

   Personalizza l’elenco delle pagine utilizzate per precaricare la cache con [Variabile di ambiente WARM_UP_PAGES](../environment/variables-post-deploy.md#warmuppages).

1. [Riduci file di tema](../environment/variables-deploy.md#scdmatrix)
È possibile ridurre il numero di file di tema non necessari configurando la variabile di ambiente SCD\_MATRIX.

1. [Velocizzare l&#39;implementazione di contenuti statici](../environment/variables-deploy.md#scdthreads)
È possibile velocizzare il processo di distribuzione aggiornando la variabile di ambiente SCD\_THREADS per aumentare il numero di thread per la distribuzione del contenuto statico.

>[!NOTE]
>
>Puoi convalidare la configurazione del progetto per una distribuzione ottimale tramite [esecuzione della procedura guidata stato ideale](smart-wizards.md#verifying-an-ideal-configuration).
