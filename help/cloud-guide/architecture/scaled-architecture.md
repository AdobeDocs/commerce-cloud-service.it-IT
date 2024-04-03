---
title: Architettura scalata
description: Scopri l’architettura split-tier e come viene scalata per soddisfare la domanda.
feature: Cloud, Auto Scaling, Iaas, Logs
exl-id: c54d8772-b6cc-41cc-b1ab-bef7d6f13bf2
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Architettura scalata

L’infrastruttura Cloud è scalabile in base ai requisiti delle risorse per ottenere una maggiore efficienza. L’infrastruttura cloud di Adobe Commerce esegue il monitoraggio delle applicazioni e può regolare la capacità per mantenere prestazioni costanti e prevedibili. La conversione in questa architettura consente di mitigare i problemi, ad esempio latenza o picchi di traffico elevati.

>[!NOTE]
>
>L’architettura scalata è disponibile per gli account Adobe Commerce su infrastrutture cloud con il cluster Pro 48 o versione successiva.

## Architettura split-tier

Storicamente, l’architettura Pro era costituita da tre nodi, ciascuno contenente uno stack tecnologico completo. Ora esiste un&#39;infrastruttura scalabile che fornisce un&#39;architettura su più livelli con un minimo di sei nodi: tre nodi per il database e i servizi principali e tre nodi per il server web. Questa architettura split-tier offre la possibilità di scalare i livelli in modo indipendente per ottenere un equilibrio ottimale delle prestazioni.

### Livello di servizio

Esistono tre nodi di servizio per l’archiviazione dei dati, la cache e i servizi: **OpenSearch** o **Elasticsearch**, **MariaDB**, **Redis**, e altro ancora. Quando il livello di servizio si avvicina alla capacità, l&#39;unico modo per scalare è aumentare le dimensioni del server, ad esempio aumentando la potenza della CPU e la memoria. La capacità è limitata alla dimensione del nodo disponibile. Poiché il cluster di database è progettato per un&#39;elevata disponibilità, non è possibile scalare orizzontalmente in modo affidabile con le tecnologie utilizzate.

![Scalabilità a livello di servizio](../../assets/scaling-service.png)

Considera un esempio che il tipo di istanza del nodo del servizio è _m5.2xlarge_ con 32 Gb di RAM. Un servizio, come il database, utilizza una notevole quantità di memoria (30 Gb). Ridimensionamento alla successiva dimensione istanza disponibile _m5.4xlarge_ fornisce una RAM da 64 Gb, che raddoppia la memoria e soddisfa le crescenti esigenze del database.

È possibile ottimizzare ulteriormente le prestazioni del livello servizio instradando il traffico in base al tipo di nodo. Per impostazione predefinita, il nodo del database è isolato dal traffico web. Ad esempio, puoi scegliere di gestire il traffico web sul nodo del database.

### Livello web

Esistono tre nodi web per l’elaborazione delle richieste e del traffico web: **php-fpm** e **NGINX**. Oltre al ridimensionamento verticale aumentando la potenza e la memoria, il livello web può essere scalato orizzontalmente aggiungendo server web a un cluster esistente quando è limitato a livello PHP. Consulta [Ridimensionamento automatico](autoscaling.md) per scoprire come i nodi web vengono scalati automaticamente.

![Scalabilità a livello web](../../assets/scaling-web.png)

Questa funzione integra la scalabilità verticale fornita dal livello di servizio. Con la scalabilità del livello di servizio in termini di dimensioni e potenza per soddisfare un crescente utilizzo di database e servizi, il livello web viene scalato in termini di dimensioni, potenza e istanze per soddisfare un aumento delle richieste di processi e requisiti di traffico più elevati.

Considera un esempio di tipo di istanza del nodo web _C5.2xlarge con otto CPU e 16 Gb di RAM_. Il numero di richieste al sito è aumentato notevolmente. È possibile aggiungere un nodo C5.2xlarge per gestire l&#39;aumento dei processi php-fpm oppure modificare ogni tipo di istanza in _C5.4xlarge con 16 CPU e 32 Gb di RAM_. L’aggiunta di un nodo riduce il rischio di una capacità di sovratensione insufficiente.

## Struttura del progetto

In minima parte, i progetti Pro con architettura Scaled dispongono di sei nodi.

- 3 nodi web c5.2xlarge (8 CPU, 16 Gb di RAM)

- 3 nodi di servizio m5.2xlarge (8 CPU, 32 Gb di RAM)

Tuttavia, ogni progetto è univoco e richiede il monitoraggio delle prestazioni per analizzare correttamente la gestione delle risorse. Ogni account include [servizio New Relic](../monitor/new-relic-service.md), che si connette automaticamente ai dati dell&#39;applicazione e alle analisi delle prestazioni per fornire un monitoraggio dinamico del server. In particolare, è possibile utilizzare il servizio New Relic per monitorare l&#39;utilizzo della CPU e della RAM per determinare quali nodi richiedono risorse aggiuntive. Quando una risorsa raggiunge la capacità o si osserva un peggioramento delle prestazioni in base all’analisi, è possibile creare una richiesta per scalare l’infrastruttura per soddisfare la domanda.

### Accesso SSH

Alcuni file e registri, ad esempio `/app/<project-id>/var/log` non sono condivisi tra nodi. Ogni nodo dispone di un accesso SSH univoco. Non è possibile utilizzare `magento-cloud` CLI per accedere al servizio o ai nodi web, ma puoi trovare gli indirizzi dei nodi nell’elenco degli accessi SSH nel tuo [!DNL Cloud Console].

```bash
ssh <node>.<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
```

- `node` Da 1 a 3: indirizzi per l&#39;accesso ai nodi di servizio

- `node` Da 4 a _n_- Indirizzi per l&#39;accesso ai nodi Web

>[!TIP]
>
>Dopo aver effettuato l’accesso, puoi confermare l’ID server e il ruolo: i nodi del servizio utilizzano _unificato_ ruolo e i nodi web utilizzano _web_ ruolo.

Esempio di risposta durante l’accesso a **nodo di servizio** include _unificato_ ruolo:

```terminal
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:unified.

project-id@server-id:~$
```

Esempio di risposta durante l’accesso a **nodo web** include _web_ ruolo:

```terminal
 __  __                   _          ___ _             _
|  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
| |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
|_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
            |___/

 Welcome to Magento Cloud.

 This is server unique-server-id, role project-id:web.

project-id@server-id:~$
```

### Posizioni del registro

Le posizioni del registro variano leggermente a seconda del nodo. Ad esempio, un registro di database, ad esempio **Registro errori MySQL**, è disponibile su un nodo di servizio (`/var/log/mysql/mysql-error.log`), ma non è disponibile su un nodo web.

Ogni account Pro include [Servizio registri di New Relic](../monitor/new-relic-service.md), che si connette automaticamente ai dati di registro dell’applicazione per fornire una gestione dinamica dei registri. I dati di registro aggregati da tutti i nodi vengono visualizzati nell’applicazione Registri di New Relic in modo da poter risolvere i problemi di prestazioni su nodi specifici da un singolo dashboard.
