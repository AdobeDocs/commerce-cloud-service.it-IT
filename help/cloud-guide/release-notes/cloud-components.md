---
title: Componenti cloud per Commerce
description: Consulta un elenco degli ultimi miglioramenti apportati al pacchetto Componenti cloud.
recommendations: noDisplay, catalog
exl-id: b4e2508a-3558-4fa8-bae0-3eb76c7b2775
source-git-commit: c02dfd2709cdc63ac1630edaa8c89cad5f737ea1
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 0%

---

# Componenti cloud per Commerce

Il [Componenti cloud](https://github.com/magento/magento-cloud-components) fornisce funzionalità di base Adobe Commerce estese per i siti distribuiti nell’infrastruttura Cloud. Questo pacchetto è una dipendenza per il pacchetto ECE-Strumenti. Queste note sulla versione descrivono gli ultimi miglioramenti apportati a questo pacchetto, che è un componente di [Suite di strumenti cloud per Commerce](cloud-tools-suite.md).

Il `magento/magento-cloud-components` Il pacchetto utilizza la seguente sequenza di versioni: `<major>.<minor>.<patch>`

Le note sulla versione includono:

- ![nuova icona](../../assets/new.svg) Nuove funzioni
- ![icona correzione](../../assets/fix.svg) Correzioni e miglioramenti

<!--Add release notes below-->

## v1.0.14 {#latest}

Data di rilascio: 8 aprile 2024

- ![nuova icona](../../assets/new.svg) **PHP** — Aggiunta del supporto per PHP 8.3.

## v1.0.13

Data di rilascio: 10 marzo 2023

- ![nuova icona](../../assets/new.svg) **Supporto avanzato per PHP 8.2**—Sono stati risolti dei problemi di compatibilità con alcune versioni di PHP 8.2.x per supportare Commerce 2.4.6.

## v1.0.12

Data di rilascio: 13 settembre 2022

- ![icona correzione](../../assets/fix.svg) **Errori durante il riscaldamento**- È stato risolto un problema che tentava di [riscaldamento](../environment/variables-post-deploy.md#warm_up_pages) quando la visibilità della pagina è impostata su [**Non visibile singolarmente**](https://docs.magento.com/user-guide/system/data-attributes-product.html#simple-product-csv-file-structure) nell’amministratore, con conseguente `ERROR: Warming up failed: <link to page>` errori nel registro di distribuzione.<!-- MCLOUD-9134 -->

## v1.0.11

Data di rilascio: 4 agosto 2022

- ![icona correzione](../../assets/fix.svg) **È stato aggiunto il supporto per la compatibilità con Symfony 5.4**- Correzioni per la compatibilità con Symfony 5.4.<!-- AC-3550 -->

## v1.0.10

Data di rilascio: 10 marzo 2022

- ![nuova icona](../../assets/new.svg) **Supporto PHP 8.1**- Aggiunta del supporto per PHP 8.1 e soppressione del supporto per PHP 7.1.

## v1.0.9

Data di rilascio: 25 ottobre 2021

- ![icona correzione](../../assets/fix.svg) **Aggiorna monologo**- È stata aggiornata la versione minima richiesta per `monolog` pacchetto a `^2.3`.<!-- ACMP-1263 -->

## v1.0.8

Data di rilascio: 29 luglio 2021

- ![icona correzione](../../assets/fix.svg) **Le barre finali sono state rimosse dagli URL generati automaticamente**- Le barre finali sono state rimosse dagli URL delle pagine delle categorie generati durante il riscaldamento della cache.<!--MCLOUD-7192-->

## v1.0.7

Data di rilascio: 9 settembre 2020

- ![nuova icona](../../assets/new.svg) **Miglioramenti alla registrazione**- Ridurre le dimensioni del `cache.log` per migliorare le prestazioni.<!--MCLOUD-6859-->

- ![icona correzione](../../assets/fix.svg) È stato corretto un errore di tipo nei valori di configurazione della cache che causava la `php bin/magento cache:evict` Comando CLI non riuscito.

## v1.0.6

Data di rilascio: 5 agosto 2020

- ![nuova icona](../../assets/new.svg) **Miglioramento delle prestazioni di Redis**- Aggiunto il `./bin/magento cache:evict` comando per rimuovere le chiavi Redis scadute, che riduce l&#39;utilizzo della memoria Redis per migliorare le prestazioni.<!--MCLOUD-6023-->

- ![icona correzione](../../assets/fix.svg) Supporto rimosso per *Registri New Relic nel contesto* per risolvere un problema di prestazioni.<!--MCLOUD-6422-->

## v1.0.5

Data di rilascio: 25 giugno 2020

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema introdotto nella versione 1.0.4 di magento/magento-cloud-components che causava il mancato funzionamento dell’operazione di svuotamento della cache durante la fase di distribuzione, interrompendo il processo di distribuzione.

## v1.0.4

Data di rilascio: 25 giugno 2020

- ![nuova icona](../../assets/new.svg) **Registri New Relic implementati nel contesto**: i registri dell’applicazione generati da Adobe Commerce ora vengono visualizzati in tracce in New Relic per migliorare le funzionalità di risoluzione dei problemi.<!--MCLOUD-6029-->

- ![nuova icona](../../assets/new.svg) **Registrazione migliorata**- Aggiunta della registrazione per tenere traccia degli eventi di invalidamento della cache e reindicizzazione completa.<!--MCLOUD-6157-->

## v1.0.3

Data di rilascio: 27 febbraio 2020

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema di compatibilità per supportare `ece-tools` Versioni 2002.0.x che utilizzano versioni PHP precedenti.

## v1.0.2

Data di rilascio: 6 febbraio 2020

- ![nuova icona](../../assets/new.svg) È stata estesa la funzionalità di `WARM_UP_PAGES` variabile di ambiente per supportare il precaricamento della cache per pagine di prodotto specifiche. Consulta la [variabili post-distribuzione](../environment/variables-post-deploy.md#warm_up_pages) per una descrizione dettagliata della funzione.<!--MAGECLOUD-4444-->

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema a causa del quale un URL archivio non valido causava un errore dell’hook post-distribuzione quando si utilizzava `WARM_UP_PAGES` per popolare la cache. Questo problema si verificava solo quando le riscritture URL erano disabilitate.<!-- MAGECLOUD-4094 -->

## v1.0.1

Data di rilascio: 23 luglio 2019

- ![icona correzione](../../assets/fix.svg) È stato risolto un problema che interessava [**WARM_UP_PAGES**](../environment/variables-post-deploy.md#warm_up_pages) funzionalità che utilizza un URL predefinito per lo store. Ora, se `config:show:default-url` Se il comando non è in grado di recuperare un URL di base, viene utilizzato l’URL dalla variabile MAGENTO_CLOUD_ROUTES.<!-- MAGECLOUD-3866 -->

## v1.0.0

Data di rilascio: 12 giugno 2019

Questa è la prima versione di [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components) pacchetto, una nuova dipendenza per `ece-tools` versione del pacchetto 2002.0.20 e successive.

- ![nuova icona](../../assets/new.svg) È stata aggiunta la possibilità di utilizzare modelli regex per configurare **WARM_UP_PAGES** variabile di ambiente per memorizzare nella cache singole pagine, più domini e più pagine. Consulta [Variabili post-distribuzione](../environment/variables-post-deploy.md#warm_up_pages).<!--MAGECLOUD-3258-->
