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

Il progetto Pro include un singolo archivio Git con un ramo `master` globale e tre ambienti principali:

1. **Ambiente di produzione** per l&#39;avvio e la manutenzione del sito live
1. **Ambiente di gestione temporanea** per il test con tutti i servizi
1. **Integrazione** ambiente per sviluppo e test

![Elenco ambienti Pro](../../assets/pro-environments.png)

Questi ambienti sono `read-only` e accettano le modifiche del codice distribuito dai rami inviati dall&#39;area di lavoro locale. Per una panoramica completa degli ambienti Pro, consulta [Architettura Pro](pro-architecture.md). Per una panoramica dell&#39;elenco degli ambienti Pro nella visualizzazione del progetto, vedere [[!DNL Cloud Console]](../project/overview.md#cloud-console).

L’immagine seguente illustra il flusso di lavoro di sviluppo e distribuzione di Pro, che utilizza un approccio semplice e con ramificazioni Git. [sviluppa](#development-workflow) codice utilizzando un ramo attivo basato sull&#39;ambiente `integration`, _invia_ e _richiama_ modifiche al codice da e verso il ramo attivo remoto. Distribuisci il codice verificato _unendo_ il ramo remoto al ramo base, che attiva un processo [build e distribuzione](#deployment-workflow) automatizzato per tale ambiente.

![Visualizzazione di alto livello del flusso di lavoro di sviluppo dell&#39;architettura Pro](../../assets/pro-dev-workflow.png)

## Flusso di lavoro di sviluppo

L&#39;ambiente di integrazione fornisce un singolo ramo `integration` di base contenente il codice di infrastruttura cloud di Adobe Commerce. Puoi creare un ulteriore ramo dell’ambiente attivo. Questo consente di implementare fino a due rami attivi nei contenitori Platform as a service (PaaS). Non esiste alcun limite al numero di ambienti inattivi.

{{enhanced-integration-envs}}

Gli ambienti di progetto supportano un processo di integrazione flessibile e continuo. Iniziare clonando il ramo `integration` nella cartella del progetto locale. Crea un ramo o più rami, sviluppa nuove funzioni, configura modifiche, aggiungi estensioni e distribuisci aggiornamenti:

- **Recupera** modifiche da `integration`

- **Ramo** da `integration`

- **Sviluppa** codice in una workstation locale, inclusi [!DNL Composer] aggiornamenti

- **Invia** modifiche al codice in remoto e convalida

- **Unisci** in `integration` e verifica

Con un ramo di codice sviluppato e i file di configurazione corrispondenti, le modifiche al codice sono pronte per essere unite al ramo `integration` per test più completi. L&#39;ambiente `integration` è inoltre ideale per:

- **Integrazione di servizi di terze parti**. Non tutti i servizi sono disponibili nell&#39;ambiente PaaS.

- **Generazione dei file di gestione della configurazione**. Alcune impostazioni di configurazione sono _Sola lettura_ in un ambiente distribuito.

- **Configurazione dello store**. Configurare tutte le impostazioni dello store utilizzando l&#39;ambiente di integrazione. È possibile trovare l&#39;**URL amministratore archivio** nella visualizzazione dell&#39;ambiente _integrazione_ in _[!DNL Cloud Console]_.

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

- Posiziona il sito nell&#39;ambiente di destinazione in modalità _Manutenzione_

- Distribuisci contenuto statico se non completato durante la generazione

- Installare o aggiornare Adobe Commerce sull’infrastruttura cloud

- Configurare il routing per il traffico

Dopo il processo di build e distribuzione, il tuo store torna online con le modifiche e le configurazioni del codice più recenti. Vedere [Processo di distribuzione](../deploy/process.md).

### Unisci all’integrazione

Combina tutte le modifiche al codice verificate unendo il ramo di sviluppo attivo nel ramo `integration` di base. È possibile verificare tutte le modifiche nel ramo `integration` prima di promuovere le modifiche nell&#39;ambiente di staging.

### Unisci a staging

La gestione temporanea è un ambiente di preproduzione che fornisce tutti i servizi e le impostazioni il più vicino possibile all’ambiente di produzione. Effettua sempre il push delle modifiche al codice dall&#39;ambiente `integration` all&#39;ambiente `staging` in modo da poter eseguire test approfonditi con tutti i servizi. La prima volta che si utilizza l&#39;ambiente di gestione temporanea, è necessario configurare servizi quali [Fastly CDN](../cdn/fastly.md) e [New Relic](../monitor/new-relic-service.md). Configura gateway di pagamento, spedizione, notifiche e altri servizi vitali con sandbox o credenziali di test.

È meglio testare accuratamente ogni servizio, verificare gli strumenti di test delle prestazioni ed eseguire test UAT come amministratore e come cliente, fino a quando non si ritiene che il negozio sia pronto per l’ambiente di produzione. Consulta [Distribuire il tuo archivio](../deploy/staging-production.md).

### Unisci a produzione

Dopo aver eseguito un test approfondito nell’ambiente di staging, esegui l’unione all’ambiente di produzione e il test completo utilizzando le credenziali live. Nel momento in cui avvii il sito di produzione, i clienti devono essere in grado di completare gli acquisti e gli amministratori devono essere in grado di gestire il negozio live. Consulta i seguenti argomenti per una procedura dettagliata e chiara per l’implementazione dello store e la pubblicazione:

- [Distribuire lo store](../deploy/staging-production.md)
- [Lancio del sito](../launch/overview.md)

### Unisci a master globale

Invia sempre una copia del codice di produzione al `master` globale nel caso in cui si presenti una necessità emergente di eseguire il debug dell&#39;ambiente di produzione senza interrompere i servizi.

**non** crea un ramo da `master` globale. Utilizza il ramo `integration` per creare nuovi rami attivi da sviluppare e correggere.
