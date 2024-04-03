---
title: Flusso di lavoro di un progetto professionale
description: Scopri come utilizzare i flussi di lavoro di sviluppo e distribuzione Pro.
feature: Cloud, Iaas, Paas
exl-id: 103e90d5-2ef2-4fef-845c-439344666b00
source-git-commit: 08f43a3b0a50cdb2a5e8a45bd2e2448bc6dbca2b
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 0%

---

# Flusso di lavoro di un progetto professionale

Il progetto Pro include un singolo archivio Git con un `master` ramo e tre ambienti principali:

1. **Produzione** ambiente per il lancio e la manutenzione del sito live
1. **Staging** ambiente per il testing con tutti i servizi
1. **Integrazione** ambiente per lo sviluppo e il testing

![Elenco ambiente Pro](../../assets/pro-environments.png)

Questi ambienti sono `read-only`, accettando le modifiche del codice distribuito dai rami inviati dall’area di lavoro locale. Consulta [Architettura Pro](pro-architecture.md) per una panoramica completa degli ambienti Pro. Consulta [[!DNL Cloud Console]](../project/overview.md#cloud-console) per una panoramica dell&#39;elenco degli ambienti Pro nella vista progetto.

L’immagine seguente illustra il flusso di lavoro di sviluppo e distribuzione di Pro, che utilizza un approccio semplice e con ramificazioni Git. Tu [sviluppare](#development-workflow) codice che utilizza un ramo attivo basato su `integration` ambiente, _push_ e _tirare_ modifiche al codice da e verso il ramo attivo remoto. Distribuisci il codice verificato tramite _unione_ il ramo remoto al ramo base, che attiva un [generare e distribuire](#deployment-workflow) per tale ambiente.

![Visualizzazione di alto livello del flusso di lavoro di sviluppo dell&#39;architettura Pro](../../assets/pro-dev-workflow.png)

## Flusso di lavoro di sviluppo

L’ambiente di integrazione offre un’unica `integration` ramo contenente il codice dell’infrastruttura cloud di Adobe Commerce. Puoi creare un ulteriore ramo dell’ambiente attivo. Questo consente di implementare fino a due rami attivi nei contenitori Platform as a service (PaaS). Non esiste alcun limite al numero di ambienti inattivi.

{{enhanced-integration-envs}}

Gli ambienti di progetto supportano un processo di integrazione flessibile e continuo. Inizia clonando il file `integration` nella cartella del progetto locale. Crea un ramo o più rami, sviluppa nuove funzioni, configura modifiche, aggiungi estensioni e distribuisci aggiornamenti:

- **Recupera** modifiche da `integration`

- **Ramo** da `integration`

- **Sviluppa** codice su una workstation locale, tra cui [!DNL Composer] aggiornamenti

- **Push** modifiche al codice in remoto e convalida

- **Unisci** a `integration` e test

Con un ramo di codice sviluppato e i file di configurazione corrispondenti, le modifiche al codice sono pronte per essere unite al `integration` per test più completi. Il `integration` L’ambiente è ideale anche per:

- **Integrazione di servizi di terze parti**—Non tutti i servizi sono disponibili nell&#39;ambiente PaaS.

- **Generazione dei file di gestione della configurazione**- Alcune impostazioni di configurazione sono _Sola lettura_ in un ambiente distribuito.

- **Configurazione dello store**- Dovresti configurare completamente tutte le impostazioni dello store utilizzando l’ambiente di integrazione. È possibile trovare **URL amministratore store** il _integrazione_ visualizzazione ambiente in _[!DNL Cloud Console]_.

## Flusso di lavoro di distribuzione

Ogni volta che si invia il codice dalla workstation locale all&#39;ambiente remoto o si unisce il codice a un ramo dell&#39;ambiente, gli script di generazione e distribuzione generano nuovo codice e forniscono i servizi configurati all&#39;ambiente remoto.

Azioni script di compilazione:

- Il sito nell’ambiente di destinazione continua a essere eseguito durante una build

- Verificare ed eseguire Adobe Commerce su patch e hotfix dell’infrastruttura cloud

- Compilare il codice con un registro di compilazione e distribuzione

- Controlla la gestione della configurazione; la distribuzione del contenuto statico avviene durante questa fase

- Crea o utilizza un frammento di codice non modificato per velocizzare il processo

- Provisioning di tutti i servizi e le applicazioni back-end

Distribuisci azioni script:

- Posiziona il sito nell’ambiente di destinazione in un _Manutenzione_ modalità

- Distribuisci contenuto statico se non completato durante la generazione

- Installare o aggiornare Adobe Commerce sull’infrastruttura cloud

- Configurare il routing per il traffico

Dopo il processo di build e distribuzione, il tuo store torna online con le modifiche e le configurazioni del codice più recenti. Consulta [Processo di distribuzione](../deploy/process.md).

### Unisci all’integrazione

Combina tutte le modifiche al codice verificate unendo il ramo di sviluppo attivo nella base `integration` filiale. Puoi verificare tutte le modifiche in `integration` prima di promuovere le modifiche all&#39;ambiente di staging.

### Unisci a staging

La gestione temporanea è un ambiente di preproduzione che fornisce tutti i servizi e le impostazioni il più vicino possibile all’ambiente di produzione. Effettua sempre il push delle modifiche apportate al codice da `integration` dell&#39;ambiente `staging` in modo da poter eseguire test approfonditi con tutti i servizi. La prima volta che utilizzi l’ambiente di staging, devi configurare i servizi, come [Fastly CDN](../cdn/fastly.md) e [New Relic](../monitor/new-relic-service.md). Configura gateway di pagamento, spedizione, notifiche e altri servizi vitali con sandbox o credenziali di test.

È meglio testare accuratamente ogni servizio, verificare gli strumenti di test delle prestazioni ed eseguire test UAT come amministratore e come cliente, fino a quando non si ritiene che il negozio sia pronto per l’ambiente di produzione. Consulta [Distribuire lo store](../deploy/staging-production.md).

### Unisci a produzione

Dopo aver eseguito un test approfondito nell’ambiente di staging, esegui l’unione all’ambiente di produzione e il test completo utilizzando le credenziali live. Nel momento in cui avvii il sito di produzione, i clienti devono essere in grado di completare gli acquisti e gli amministratori devono essere in grado di gestire il negozio live. Consulta i seguenti argomenti per una procedura dettagliata e chiara per l’implementazione dello store e la pubblicazione:

- [Distribuire lo store](../deploy/staging-production.md)
- [Lancio del sito](../launch/overview.md)

### Unisci a master globale

Invia sempre una copia del codice di produzione al Global `master` nel caso in cui si presenti una necessità emergente di eseguire il debug dell’ambiente di produzione senza interrompere i servizi.

Esegui **non** crea un ramo da Globale `master`. Utilizza il `integration` per creare rami nuovi e attivi da sviluppare e correggere.
