---
title: Passaggi di avvio
description: Scopri come completare il lancio del sito.
exl-id: cf75f89c-94c8-47c1-a23d-93cd9921aab7
source-git-commit: e62236a8937ae1dd10df3c736b2e945be155d236
workflow-type: tm+mt
source-wordcount: '344'
ht-degree: 0%

---

# Passaggi di avvio

Dopo aver testato e completato l’elenco di controllo di lancio, puoi avviare i passaggi finali per avviare. Questi passaggi includono l’immissione dei biglietti, il trasferimento dell’accesso al sito e infine il test del negozio quando è in diretta.

Il personale di supporto Adobe collabora con te durante la procedura, verificando lo stato e fornendo assistenza nel caso in cui si verifichino domande o problemi. Tutti i problemi devono essere tracciati con i ticket per acquisire al meglio ciò che è successo e come è stato risolto. Quando si iniziano iterazioni continue per la distribuzione degli aggiornamenti allo store avviato, è possibile che si verifichino di nuovo problemi simili. Questi ticket possono aiutare a individuare i problemi e a regolare le attività di distribuzione.

## Contatta l’Adobe per avviare il sito

Contatta il supporto Adobe Commerce e aggiorna tutti i ticket di lancio del sito (Go Live) con la data e l’ora previste per il passaggio e il lancio del tuo store.

## Passa DNS al nuovo sito

Il valore Time-to-Live modificato è importante per controllare il dominio modificato. Quando modifichi i record A e CNAME, l’aggiornamento richiede il tempo configurato TTL per essere aggiornato correttamente. Per ulteriori informazioni sulle impostazioni DNS, vedere [Configurazioni DNS](checklist.md#update-dns-configuration-with-production-settings).

### Per passare al nuovo sito:

1. Accedere al servizio DNS.

1. Aggiorna i record A e CNAME per i domini e i nomi host.

1. Attendi che il tempo TTL passi e riavvia il browser web.

1. Accedi al tuo store utilizzando il dominio storefront.

## Test dello store live

Completa alcuni test UAT nel tuo archivio live per verificare che tutto sia in fase di caricamento e che le azioni vengano completate correttamente. Per un elenco dei test, consulta [Distribuzione dei test](../test/staging-and-production.md#complete-uat-testing).

## Post-lancio

L’Adobe controlla il sito e lo monitora per garantire che tutti i servizi e gli accessi siano visualizzati in verde. Rimaniamo a disposizione quando necessario per esaminare e verificare che tutti i registri di sistema, i servizi, la memorizzazione in cache e le funzioni funzionino come necessario per te e per i tuoi clienti.

In caso di problemi, crea e tieni traccia dei problemi con il supporto. Includi quante più informazioni possibili tra cui data/ora, funzionalità specifiche con un problema, sintomi e comportamenti insoliti, estensioni e altro ancora. Analizziamo i registri, il problema e collaboriamo con te per risolverlo il più rapidamente possibile.
