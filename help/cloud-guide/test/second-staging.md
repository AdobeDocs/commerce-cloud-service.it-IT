---
title: Secondo ambiente di staging
description: Scopri i vantaggi e gli utilizzi di un secondo ambiente di staging per test paralleli, isolamento dei problemi, controllo delle versioni e altro ancora.
source-git-commit: 51fa75fff952e3832151d47b80baa7f532aa277f
workflow-type: tm+mt
source-wordcount: '716'
ht-degree: 0%

---


# Secondo ambiente di staging

Nell’infrastruttura Adobe Commerce Cloud, l’ambiente di staging assicura test e convalida completi in un’impostazione in grado di rispecchiare l’ambiente di produzione. Questo ambiente consente di identificare e risolvere in modo sicuro i bug, garantendo al contempo che tutte le nuove funzioni o modifiche siano perfettamente integrate prima che influiscano sul sito live.

Alcuni progetti richiedono un flusso di lavoro di sviluppo più sofisticato. Per supportare questa esigenza, Adobe offre un ambiente di staging aggiuntivo come opzione aggiuntiva all’infrastruttura cloud. Disporre di due ambienti di staging è vantaggioso per progetti complessi e per la collaborazione simultanea tra i team.

L&#39;utilizzo di un ambiente di staging secondario offre i seguenti vantaggi:

- **Test paralleli**: con due ambienti di gestione temporanea, i team possono testare nuove funzionalità e aggiornamenti critici contemporaneamente senza interferire con lo sviluppo dell&#39;altro team. Ad esempio, puoi dedicare un ambiente ai test delle funzioni, riservando l’altro ambiente per i test delle prestazioni e delle sollecitazioni.

- **Isolamento dei problemi**: quando si verificano bug o problemi nell&#39;ambiente di staging principale, è possibile replicarli e diagnosticarli nell&#39;ambiente secondario senza interrompere l&#39;avanzamento dei test. In questo modo la risoluzione dei problemi non interferisce con i test in corso.

- **Controllo versione**: consente di gestire in modo più efficace diverse versioni dell&#39;applicazione. L’ambiente di staging primario può eseguire la build stabile più recente, mentre le funzioni sperimentali dei test secondari. Questo consente di gestire meglio i cicli di rilascio e garantisce che le modifiche sperimentali non interrompano le build stabili.

- **Pianificazione rollout avanzata**: è possibile simulare diversi scenari di distribuzione. L’ambiente di staging principale può simulare la configurazione di produzione corrente, mentre l’ambiente secondario può essere configurato per testare le prossime versioni.

- **Test di accettazione utente (UAT)**: mentre utilizzi l&#39;ambiente principale per lo sviluppo continuo, puoi dedicare l&#39;ambiente secondario a UAT. Questo consente agli utenti o ai client di testare e fornire feedback sulle nuove funzioni in un’impostazione controllata, senza influire sui test.

- **Test di regressione**: è possibile utilizzare un ambiente per i test di inoltro e l&#39;altro per i test di regressione, assicurandosi che le nuove modifiche non interrompano la funzionalità esistente.

- **Formazione e dimostrazioni**: utilizza un ambiente per la formazione dei nuovi membri del team o la dimostrazione di nuove funzionalità alle parti interessate. In questo modo le attività di formazione non interrompono i test.

- **Impostazione collegamento privato**: gli ambienti master e di integrazione non supportano la configurazione del collegamento privato. Se il flusso di lavoro di sviluppo richiede altri ambienti collegati tramite Collegamento privato per lo sviluppo, il test o qualsiasi altro scopo, è consigliabile aggiungere un ambiente di staging aggiuntivo.

L&#39;utilizzo di due ambienti di gestione temporanea può migliorare notevolmente l&#39;efficienza del flusso di lavoro, la gestione dei rischi e la qualità complessiva dell&#39;applicazione. Questi ambienti forniscono sicurezza e flessibilità che un ambiente di staging non sarebbe in grado di offrire.

>[!NOTE]
>
>Questa configurazione è un componente aggiuntivo. I clienti che desiderano un ambiente secondario devono contattare il proprio rappresentante commerciale per richiederne uno. Il rappresentante commerciale può fornire informazioni sui prezzi e sul processo di approvvigionamento.

Se il progetto dispone già di un ambiente di staging aggiuntivo o si sta valutando l’aggiornamento del progetto corrente, prendere in considerazione le seguenti tempistiche e responsabilità di provisioning:

- Il processo di provisioning può richiedere fino a due settimane dopo la conferma della richiesta con il rappresentante commerciale Adobe.

- I nuovi ambienti di staging sono disponibili solo nella stessa area degli ambienti di staging e produzione correnti.

- È necessario aggiornare l’ambiente di integrazione o l’ambiente principale e specificare su quali ambienti verrà basato l’ambiente di staging secondario. Non è possibile copiare l’ambiente di staging secondario dagli ambienti di staging o produzione.

- Dopo aver ricevuto il nuovo ambiente di staging, puoi includerlo nel flusso di sviluppo. Come con altri ambienti, il codice e gli aggiornamenti del database sono di tua responsabilità.

## Richiedi l’ambiente

Per facilitare la richiesta, segui i passaggi:

1. Aggiorna il tuo ramo.

   Aggiorna il ramo Integrazione o Master con il codice e il database che desideri utilizzare per il nuovo ambiente.

1. Contatta il tuo rappresentante commerciale.

   Contatta il tuo rappresentante commerciale Adobe e fornisci il ramo specifico che desideri utilizzare come base per il nuovo ambiente di staging (integrazione o principale).

1. Conferma i dettagli.

   Dopo aver confermato i dettagli con il rappresentante commerciale, attendere la conferma che la richiesta di provisioning è stata ricevuta e viene elaborata. Al termine del provisioning, il team Adobe ti invierà una conferma.

1. Implementare nel nuovo ambiente.

   Segui i [passaggi di distribuzione standard](../deploy/staging-production.md) per distribuire il codice e il database nel nuovo ambiente di staging.
