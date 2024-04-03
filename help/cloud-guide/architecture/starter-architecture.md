---
title: Architettura iniziale
description: Scopri gli ambienti supportati dall’architettura Starter.
feature: Cloud, Paas
exl-id: 03365d32-4eb4-42d4-82a7-771df5e7b3da
source-git-commit: c61d711b1041ecf76ec6468cd225a34fd77c24b1
workflow-type: tm+mt
source-wordcount: '903'
ht-degree: 0%

---

# Architettura iniziale

L’architettura Starter di Adobe Commerce su infrastruttura cloud supporta fino a **quattro** ambienti, tra cui `master` ambiente che contiene il codice del progetto iniziale, l’ambiente di staging e fino a due ambienti di integrazione.

Tutti gli ambienti sono contenuti in contenitori PaaS (Platform as a service). Questi contenitori vengono distribuiti all&#39;interno di contenitori con restrizioni elevate su una griglia di server. Questi ambienti sono di sola lettura e accettano le modifiche del codice distribuito dai rami inviati dall’area di lavoro locale. Ogni ambiente fornisce un database e un server web.

Puoi utilizzare qualsiasi metodologia di sviluppo e ramificazione che ti piace. Quando ottieni l’accesso iniziale al progetto, crea un’ `staging` ambiente da `master` ambiente. Quindi, crea il `integration` ambiente diramando da `staging`.

## Architettura dell’ambiente di partenza

Il diagramma seguente mostra le relazioni gerarchiche degli ambienti Starter.

![Visualizzazione di alto livello del progetto Starter](../../assets/starter/architecture.png)

## Ambiente di produzione

L’ambiente di produzione fornisce il codice sorgente per implementare Adobe Commerce nell’infrastruttura Cloud che esegue le vetrine di singoli e più siti pubbliche. L’ambiente di produzione utilizza il codice del `master` per configurare e abilitare il server web, il database, i servizi configurati e il codice dell&#39;applicazione.

Perché il `production` è di sola lettura, utilizza `integration` per apportare modifiche al codice, implementare nell’architettura da `integration` a `staging`, e infine al `production` ambiente. Consulta [Distribuire lo store](../deploy/staging-production.md) e [Lancio del sito](../launch/overview.md).

L’Adobe consiglia di eseguire test completi nel `staging` diramazione prima di eseguire il push al `master` ramo, che distribuisce in `production` ambiente.

## Ambiente di staging

L’Adobe consiglia di creare un ramo denominato `staging` da `master`. Il `staging` branch distribuisce il codice nell’ambiente di staging per fornire un ambiente di pre-produzione per testare codice, moduli ed estensioni, gateway di pagamento, spedizione, dati di prodotto e molto altro. Questo ambiente fornisce la configurazione per tutti i servizi in modo che corrispondano all’ambiente di produzione, inclusi Fastly, New Relic APM e search.

Altre sezioni in questa guida forniscono istruzioni per le distribuzioni finali del codice e il test delle interazioni a livello di produzione in un ambiente di staging sicuro. Per ottenere prestazioni ottimali e test delle funzionalità, replicare il database nell&#39;ambiente di staging.

>[!WARNING]
>
>L’Adobe consiglia di testare ogni interazione di esercenti e clienti nell’ambiente di staging prima di implementarla nell’ambiente di produzione. Consulta [Distribuire lo store](../deploy/staging-production.md) e [Distribuzione dei test](../test/staging-and-production.md).

## Ambiente di integrazione

Gli sviluppatori utilizzano `integration` ambiente per sviluppare, distribuire e testare:

- Codice dell’applicazione Adobe Commerce

- Codice personalizzato

- Estensioni

- Servizi

**Casi d’uso consigliati:**

Gli ambienti di integrazione sono progettati per test e sviluppo limitati. Ad esempio, puoi utilizzare l’ambiente di integrazione per completare le seguenti attività:

- Assicurati che le modifiche ai processi di integrazione continua (CI) siano compatibili con cloud

- Verifica i flussi di lavoro critici su pagine chiave come Home, Categoria, Pagina dettagli prodotto (PDP), Pagamento e Amministratore

Per ottenere le migliori prestazioni nell’ambiente di integrazione, segui queste best practice:

- Limita dimensioni catalogo

- Limita l&#39;utilizzo a uno o due utenti simultanei

- Disabilita i processi cron ed esegui manualmente in base alle esigenze

Puoi avere fino a **due** ambienti di integrazione attivi. Puoi creare un ambiente di integrazione creando un ramo dalla sezione `staging` filiale. Quando crei un ambiente di integrazione, il nome dell’ambiente corrisponde al nome del ramo. Un ambiente di integrazione include un server web e un database. Non include tutti i servizi, ad esempio Fastly CDN e New Relic non sono disponibili.

Puoi avere un numero illimitato di rami inattivi per l’archiviazione del codice. Per accedere, visualizzare e verificare un ramo inattivo, è necessario attivarlo

{{enhanced-integration-envs}}

## Stack di tecnologia di produzione e staging

Gli ambienti di produzione e staging includono le seguenti tecnologie. Puoi modificare e configurare queste tecnologie tramite il [`.magento.app.yaml`](../application/configure-app-yaml.md) file.

- Fastly per il caching HTTP e CDN
- Server web Nginx che parla con PHP-FPM, un&#39;istanza con più lavoratori
- Server Redis
- Elasticsearch di ricerca nel catalogo per Adobe Commerce da 2.2 a 2.4.3-p2
- OpenSearch per la ricerca di cataloghi per Adobe Commerce 2.3.7-p3, 2.4.3-p2, 2.4.4 e versioni successive
- Filtro in uscita (firewall in uscita)

### Servizi

Adobe Commerce on cloud infrastructure supporta attualmente i seguenti servizi: PHP, MySQL (MariaDB), Elasticsearch (Adobe Commerce da 2.2 a 2.4.3-p2), OpenSearch (2.3.7-p3, 2.4.3-p2, 2.4.4 e versioni successive), Redis e [!DNL RabbitMQ].

Ogni servizio viene eseguito in un contenitore protetto separato. I contenitori vengono gestiti insieme nel progetto. Alcuni servizi sono standard, come i seguenti:

- Router HTTP (gestione delle richieste in ingresso, ma anche caching e reindirizzamenti)

- Server applicazioni PHP

- Git

- Secure Shell (SSH)

### Versioni software

Adobe Commerce su infrastruttura cloud utilizza il sistema operativo Debian GNU/Linux e il server web NGINX. Non è possibile aggiornare questo software, ma è possibile configurare le versioni per i seguenti elementi:

- [PHP](../application/php-settings.md)

- [MySQL](../services/mysql.md)

- [Redis](../services/redis.md)

- [RabbitMQ](../services/rabbitmq.md)

- [Elasticsearch](../services/elasticsearch.md)

- [OpenSearch](../services/opensearch.md)

Negli ambienti di staging e produzione, utilizzi Fastly per CDN e caching. La versione più recente dell’estensione Fastly CDN viene installata durante il provisioning iniziale del progetto. Puoi aggiornare l’estensione per ottenere le correzioni di bug e i miglioramenti più recenti. Consulta [Modulo CDN Fastly per il Magento 2](https://github.com/fastly/fastly-magento2). Inoltre, puoi accedere a [New Relic](../monitor/account-management.md) monitoraggio delle prestazioni.

Utilizzare i seguenti file per configurare le versioni del software da utilizzare nell&#39;implementazione.

- [`.magento.app.yaml`](../application/configure-app-yaml.md)

- [&quot;route.yaml&quot;](../routes/routes-yaml.md)

- [&quot;services.yaml&quot;](../services/services-yaml.md)

### Backup e disaster recovery

È possibile creare un backup del database e del file system utilizzando [!DNL Cloud Console] o CLI. Consulta [Gestione dei backup](../storage/snapshots.md).

## Prepararsi per lo sviluppo

Il flusso di lavoro seguente riepiloga il processo per diramare il codice, sviluppare e distribuire lo store:

1. Configurare l’ambiente locale

1. Clona il `master` ramo all&#39;ambiente locale

1. Creare un `staging` ramo da `master`

1. Crea rami per lo sviluppo da `staging`

1. Invia il codice a Git che genera e distribuisce in un ambiente per il test

Consulta le sezioni seguenti per istruzioni dettagliate e procedure dettagliate per sviluppare, testare e distribuire lo store:

- [Avvio del flusso di lavoro di sviluppo e distribuzione](starter-develop-deploy-workflow.md)

- [Sviluppo Docker](../dev-tools/cloud-docker.md) (ambiente di sviluppo locale abilitato da Cloud Docker per Commerce)

- [Gestisci rami](../project/console-branches.md)

- [Distribuire lo store](../deploy/staging-production.md)

- [Lancio del sito](../launch/overview.md)
