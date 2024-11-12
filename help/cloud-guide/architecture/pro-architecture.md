---
title: Architettura Pro
description: Scopri gli ambienti supportati dall’architettura Pro.
feature: Cloud, Auto Scaling, Iaas, Paas, Storage
topic: Architecture
exl-id: d10d5760-44da-4ffe-b4b7-093406d8b702
source-git-commit: a1e7674b99d7e289531268e1a298f61344c7bd8f
workflow-type: tm+mt
source-wordcount: '1573'
ht-degree: 0%

---

# Architettura Pro

L&#39;architettura Pro di Adobe Commerce su infrastruttura cloud supporta più ambienti che è possibile utilizzare per sviluppare, testare e avviare lo store.

- **Master** - Fornisce un ramo `master` distribuito nei contenitori Platform as a Service (PaaS).
- **Integrazione** - Fornisce un singolo ramo `integration` per lo sviluppo, anche se è possibile creare un ramo aggiuntivo. Ciò consente di distribuire fino a due rami _attivi_ in Platform as a Service (PaaS).
- **Gestione temporanea** - Fornisce un singolo ramo `staging` distribuito nei contenitori IaaS (Infrastructure as a Service) dedicati.
- **Produzione** - Fornisce un singolo ramo `production` distribuito nei contenitori IaaS (Infrastructure as a Service) dedicati.

La tabella seguente riepiloga le differenze tra gli ambienti:

|                                        | INTEGRAZIONE | STAGING | PRODUZIONE |
| -------------------------------------- | ----------- | ----------------- | -------------------- |
| Supporta la gestione delle impostazioni in [!DNL Cloud Console] | Sì | Limitato | Limitato |
| Supporta più rami | Sì | No (solo staging) | No (solo produzione) |
| Utilizza file YAML per la configurazione | Sì | No | No |
| Esegue su hardware IaaS dedicato | No | Sì | Sì |
| Include Fastly CDN | No | Sì | Sì |
| Include il servizio New Relic | No | APM | APM + NRI |
| Backup automatici | No | Sì | Sì |

>[!NOTE]
>
>Adobe fornisce lo strumento Cloud Docker per Commerce per la distribuzione in un ambiente Cloud Docker locale, in modo da poter sviluppare e testare progetti Adobe Commerce. Consulta [Sviluppo Docker](../dev-tools/cloud-docker.md).

## Architettura dell’ambiente

Il progetto è un singolo archivio Git con tre rami dell&#39;ambiente principale: `integration`, `staging` e `production`. Il diagramma seguente mostra la relazione gerarchica degli ambienti Pro:

![Visualizzazione di alto livello dell&#39;architettura dell&#39;ambiente Pro](../../assets/pro-branch-architecture.png)

### Ambiente principale

Nei progetti Pro, il ramo `master` fornisce un ambiente PaaS attivo con l&#39;ambiente di produzione. Inviare sempre una copia del codice di produzione all&#39;ambiente `master` in modo da poter eseguire il debug dell&#39;ambiente di produzione senza interrompere i servizi.

**Avvertenze:**

- **non** creare un ramo basato sul ramo `master`. Utilizza l’ambiente di integrazione per creare rami attivi da sviluppare.

- Non utilizzare l&#39;ambiente `master` per sviluppo, UAT o test delle prestazioni

### Ambiente di integrazione

L’ambiente di integrazione viene eseguito in un contenitore Linux (LXC) su una griglia di server noti come PaaS. Ogni ambiente include un server web e un database per testare il sito. Per un elenco degli indirizzi IP di AWS e Azure, vedere [Indirizzi IP regionali](../project/regional-ip-addresses.md).

**Casi d&#39;uso consigliati:**

Gli ambienti di integrazione sono progettati per test e sviluppo limitati prima di spostare le modifiche negli ambienti di staging e produzione. Ad esempio, puoi utilizzare l’ambiente di integrazione per completare le seguenti attività:

- Assicurati che le modifiche ai processi di integrazione continua (CI) siano compatibili con cloud

- Verifica i flussi di lavoro critici su pagine chiave come Home, Categoria, Pagina dettagli prodotto (PDP), Pagamento e Amministratore

Per ottenere le migliori prestazioni nell’ambiente di integrazione, segui queste best practice:

- Limita la dimensione del catalogo: per riferimento, i dati di esempio contengono circa 2.048 prodotti. Prova a ridurre la dimensione del catalogo a circa 4.000-5.000 prodotti.
Per verificare il numero di prodotti nel catalogo, eseguire la seguente query MySQL:

  ```sql
  select distinct count(entity_id) from catalog_product_entity;
  ```

- Riduci il numero di gruppi di clienti: troppi gruppi di clienti possono influire sulle prestazioni di indicizzazione e sulle prestazioni complessive.

- Limita l&#39;utilizzo a uno o due utenti simultanei

- Disabilita i processi cron ed esegui manualmente in base alle esigenze

**Avvertenze:**

- I servizi Fastly CDN e New Relic non sono accessibili in un ambiente di integrazione

- L’architettura dell’ambiente di integrazione non corrisponde a quella di staging e produzione

- Non utilizzare l&#39;ambiente `integration` per test di sviluppo, test delle prestazioni o test di accettazione utente (UAT)

- Non utilizzare l&#39;ambiente `integration` per testare la funzionalità B2B per Adobe Commerce

- Impossibile ripristinare il database nell&#39;ambiente di integrazione dalla produzione o dalla gestione temporanea del database

{{enhanced-integration-envs}}

### Ambiente di staging

L’ambiente di staging fornisce un ambiente vicino alla produzione per testare il sito. Questo ambiente, ospitato su hardware IaaS dedicato, include tutti i servizi, come Fastly CDN, New Relic APM e search.

**Casi d&#39;uso consigliati:**

L&#39;ambiente corrisponde all&#39;architettura di produzione ed è progettato per UAT, staging dei contenuti e revisione finale prima di inviare le funzionalità all&#39;ambiente `production`. È ad esempio possibile utilizzare l&#39;ambiente `staging` per completare le attività seguenti:

- Test di regressione rispetto ai dati di produzione

- Test delle prestazioni con Fastly caching abilitato

- Testare nuove build invece di applicare patch in Produzione

- Test UAT per nuove build

- Test B2B per Adobe Commerce

- Personalizzare la configurazione cron e testare i processi cron

Vedi [Flusso di lavoro di distribuzione](pro-develop-deploy-workflow.md#deployment-workflow) e [Distribuzione di prova](../test/staging-and-production.md).

**Avvertenze:**

- Dopo aver avviato il sito di produzione, utilizza l’ambiente di staging principalmente per testare le patch per le correzioni di bug critici per la produzione.

- Impossibile creare un ramo dal ramo `staging`. È invece possibile inviare le modifiche al codice dal ramo `integration` al ramo `staging`.

### Ambiente di produzione

L’ambiente di produzione esegue le vetrine singole e multisito rivolte al pubblico. Questo ambiente viene eseguito su hardware IaaS dedicato con nodi ridondanti ad alta disponibilità per garantire ai clienti accesso continuo e protezione da failover. L&#39;ambiente di produzione include tutti i servizi dell&#39;ambiente di staging, oltre al servizio [Infrastruttura New Relic (NRI)](../monitor/new-relic-service.md#new-relic-infrastructure), che si connette automaticamente ai dati dell&#39;applicazione e all&#39;analisi delle prestazioni per fornire un monitoraggio dinamico del server.

**Avviso:**

Impossibile creare un ramo dal ramo `production`. È invece possibile inviare le modifiche al codice dal ramo `staging` al ramo `production`.

### Stack di tecnologia di produzione

L&#39;ambiente di produzione dispone di tre macchine virtuali (VM) dietro un load balancer elastico gestito da un HAProxy per VM. Ogni VM include le seguenti tecnologie:

- **Fastly CDN**: cache HTTP e CDN

- **NGINX**: server Web che utilizza PHP-FPM, un&#39;istanza con più processi di lavoro

- **GlusterFS**—file server per la gestione di tutte le distribuzioni di file statici e la sincronizzazione con quattro mount di directory:

   - `var`
   - `pub/media`
   - `pub/static`
   - `app/etc`

- **Redis**: un server per macchina virtuale con un solo server attivo e gli altri due come repliche

- **Elasticsearch**: ricerca di Adobe Commerce nell&#39;infrastruttura cloud da 2.2 a 2.4.3-p2

- **OpenSearch**—cerca Adobe Commerce nell&#39;infrastruttura cloud 2.3.7-p3, 2.4.3-p2, 2.4.4 e versioni successive

- **Galera**—cluster di database con un database MySQL MariaDB per nodo con un&#39;impostazione di incremento automatico di tre per ID univoci in ogni database

La figura seguente mostra le tecnologie utilizzate nell’ambiente di produzione:

![Stack di tecnologia di produzione](../../assets/az-stack-diagram.png)

## Hardware ridondante

Anziché eseguire un&#39;installazione tradizionale di tipo attivo-passivo `master` o primario-secondario, Adobe Commerce su infrastruttura cloud esegue un&#39;architettura _ridondante_ in cui tutte e tre le istanze accettano operazioni di lettura e scrittura. Questa architettura non comporta tempi di inattività durante la scalabilità e garantisce l&#39;integrità delle transazioni.

Grazie all&#39;hardware unico e ridondante, Adobe può fornire tre server gateway. La maggior parte dei servizi esterni consente di aggiungere più indirizzi IP a un inserisco nell&#39;elenco Consentiti di, pertanto la presenza di più indirizzi IP fissi non costituisce un problema. I tre gateway si associano ai tre server del cluster dell’ambiente di produzione e conservano gli indirizzi IP statici. È completamente ridondante e ad alta disponibilità a tutti i livelli:

- DNS
- Rete per la distribuzione dei contenuti (CDN)
- Bilanciatore di carico elastico (ELB)
- Cluster a tre server che comprende tutti i servizi Adobe Commerce, inclusi il database e il server web

## Backup e disaster recovery

Adobe Commerce sull’infrastruttura cloud utilizza un’architettura ad alta disponibilità che replica ogni progetto Pro in tre aree di disponibilità di AWS o Azure separate, ciascuna con un centro dati separato. Oltre a questa ridondanza, gli ambienti di staging e produzione Pro ricevono backup regolari e live progettati per essere utilizzati in caso di _errore irreversibile_.

**I backup automatici** includono dati persistenti provenienti da tutti i servizi in esecuzione, quali il database MySQL e i file archiviati nei volumi montati. I backup vengono salvati in EBS (Elastic Block Storage) crittografato nella stessa area dell’ambiente di produzione. I backup automatici non sono accessibili pubblicamente perché sono archiviati in un sistema separato.

>[!NOTE]
>
>I volumi montati includono/fanno riferimento solo ai [mount scrivibili](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/configure/app/properties/properties#mounts) e non includeranno tutta la directory `app/`. Per quanto riguarda gli altri file, questi vengono creati/generati dal processo di [compilazione e distribuzione](https://experienceleague.adobe.com/en/docs/commerce-cloud-service/user-guide/architecture/pro-develop-deploy-workflow#deployment-workflow) e sarà inoltre necessario controllare l&#39;archivio Git per i file rimanenti.

{{pro-backups}}

Puoi creare un **backup manuale** del database per gli ambienti di staging e produzione utilizzando i comandi CLI. Vedere [Backup del database](../storage/database-dump.md). Per gli ambienti `integration`, Adobe consiglia di creare un backup come primo passo dopo aver effettuato l&#39;accesso al progetto Adobe Commerce on Cloud Infrastructure e prima di applicare modifiche importanti. Consulta [Gestione backup](../storage/snapshots.md).

### Obiettivo punto di ripristino

L’RPO corrisponde a un tempo massimo di sei ore per l’ultimo backup (ad esempio alle 06:00, quindi alle 12:00, quindi alle 18:00). La frequenza dei backup dipende dalla pianificazione del backup e dal volume di modifiche da scrivere nel servizio di storage.

### Criterio di conservazione

Adobe mantiene i backup automatici in base ai seguenti criteri di conservazione dei dati:

| Periodo | Criterio di conservazione dei backup |
| ------------------ | ----------------------- |
| Dal giorno 1 al giorno 3 | Un backup all&#39;ora |
| Dal giorno 4 al giorno 7 | Un backup al giorno |
| Settimane da 2 a 6 | Un backup alla settimana |
| Settimane da 8 a 12 | Backup bisettimanale |
| Mese da 3 a 5 | Un backup al mese |

Questo criterio può variare a seconda del piano dell’infrastruttura cloud.

### Obiettivo del tempo di ripristino

RTO dipende dalle dimensioni dello storage. Il ripristino di volumi EBS di grandi dimensioni richiede più tempo. I tempi di ripristino possono variare a seconda delle dimensioni del database:

- Un database di grandi dimensioni (oltre 200 GB) può richiedere 5 ore
- Un database di medie dimensioni (150 GB) può richiedere 2 ore e mezza
- Un database di piccole dimensioni (60 GB) può richiedere 1 ora

## Scalabilità del cluster Pro

Le configurazioni di ridimensionamento del cluster Pro e _calcolo_ variano a seconda delle dipendenze del provider cloud scelto (AWS, Azure), dell&#39;area geografica e del servizio. L’infrastruttura cloud Adobe può scalare i cluster Pro per soddisfare le aspettative di traffico e i requisiti di servizio in base al cambiamento delle richieste.

L’architettura ridondante consente all’infrastruttura cloud Adobe di eseguire l’upscale senza tempi di inattività. Durante l&#39;upscaling, ciascuna delle tre istanze ruota per aggiornare la capacità senza influire sul funzionamento del sito. Ad esempio, puoi aggiungere altri server web a un cluster esistente se la restrizione si trova a livello PHP anziché a livello di database. In questo modo viene fornita la _scalabilità orizzontale_ per integrare la scalabilità verticale fornita dalle CPU aggiuntive a livello di database. Vedi [Architettura scalata](scaled-architecture.md).

Se prevedi un aumento significativo del traffico per un evento o per un altro motivo, puoi richiedere un aumento temporaneo della capacità. Vedi [Come richiedere un upsize temporaneo](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/how-to-request-temporary-magento-upsize.html) nel _Centro assistenza Commerce_.
