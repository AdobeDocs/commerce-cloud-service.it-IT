---
title: Configurare i servizi
description: Scopri come configurare i servizi utilizzati da Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Configuration, Services
exl-id: 48091c10-c53f-4aad-afbe-b4516653bda1
source-git-commit: 0262fca6e94d9815e8af0fb0c026e36fecc1f939
workflow-type: tm+mt
source-wordcount: '1046'
ht-degree: 0%

---

# Configurare i servizi

Il file `services.yaml` definisce i servizi supportati e utilizzati da Adobe Commerce nell&#39;infrastruttura cloud, ad esempio MySQL, Redis e Elasticsearch o OpenSearch. Non è necessario iscriversi a provider di servizi esterni.

>[!NOTE]
>
>Il file `.magento/services.yaml` è gestito localmente nella directory `.magento` del progetto. La configurazione è accessibile durante il processo di build per definire le versioni del servizio richieste solo nell’ambiente di integrazione e viene rimossa al termine della distribuzione, in modo da non trovarle sul server.


Lo script di distribuzione utilizza i file di configurazione nella directory `.magento` per eseguire il provisioning dell&#39;ambiente con i servizi configurati. Un servizio diventa disponibile per l&#39;applicazione se è incluso nella proprietà [`relationships`](../application/properties.md#relationships) del file `.magento.app.yaml`. Il file `services.yaml` contiene i valori _type_ e _disk_. Il tipo di servizio definisce il servizio _name_ e _version_.

La modifica della configurazione di un servizio determina il provisioning dell’ambiente con i servizi aggiornati tramite una distribuzione che interessa i seguenti ambienti:

- Tutti gli ambienti Starter inclusa la produzione `master`
- Ambienti di integrazione Pro

{{pro-update-service}}

## Servizi predefiniti e supportati

L’infrastruttura cloud supporta e distribuisce i seguenti servizi:

- [MySQL](mysql.md)
- [Redis](redis.md)
- [RabbitMQ](rabbitmq.md)
- [Elasticsearch](elasticsearch.md)
- [OpenSearch](opensearch.md)

È possibile visualizzare le versioni e i valori del disco predefiniti nel [file `services.yaml` predefinito](https://github.com/magento/magento-cloud/blob/master/.magento/services.yaml) corrente. L&#39;esempio seguente mostra i servizi `mysql`, `redis`, `opensearch` o `elasticsearch` e `rabbitmq` definiti nel file di configurazione `services.yaml`:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120

redis:
    type: redis:6.2

opensearch:
    type: opensearch:2  # minor version not required; uses latest
    disk: 1024

rabbitmq:
    type: rabbitmq:3.9
    disk: 1024
```

## Valori del servizio

È necessario fornire l&#39;ID servizio e la configurazione del tipo di servizio `type: <name>:<version>`. Se il servizio utilizza l&#39;archiviazione permanente, è necessario specificare un valore del disco.

Utilizza il seguente formato:

```yaml
<service-id>:
    type: <name>:<version>
    disk: <value-MB>
```

### `service-id`

Il valore `service-id` identifica il servizio nel progetto. È possibile utilizzare solo caratteri alfanumerici minuscoli: `a` a `z` e `0` a `9`, ad esempio `redis`.

Il valore _service-id_ è utilizzato nella proprietà [`relationships`](../application/properties.md#relationships) del file di configurazione `.magento.app.yaml`:

```yaml
relationships:
    redis: "<name>:redis"
```

È possibile denominare più istanze di ciascun tipo di servizio. Ad esempio, potete utilizzare più istanze Redis, una per la sessione e una per la cache.

```yaml
redis:
    type: redis:<version>

redis2:
    type: redis:<version>
```

La ridenominazione di un servizio nel file `services.yaml` **rimuove definitivamente** quanto segue:

- Il servizio esistente prima di creare un servizio con il nuovo nome specificato.
- Tutti i dati esistenti per il servizio vengono rimossi. L&#39;Adobe consiglia di [eseguire il backup dell&#39;ambiente Starter](../storage/snapshots.md) prima di modificare il nome di un servizio esistente.

### `type`

Il valore `type` specifica il nome e la versione del servizio. Ad esempio:

```yaml
mysql:
    type: mysql:10.4
```

### `disk`

Il valore `disk` specifica le dimensioni (in MB) dello spazio di archiviazione su disco persistente da allocare al servizio. I servizi che utilizzano l&#39;archiviazione persistente, come MySQL, devono fornire un valore disco. I servizi che utilizzano la memoria invece dello storage persistente, come Redis, non richiedono un valore del disco.

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
```

L&#39;attuale capacità di storage predefinita per progetto è di 5 GB o 512 0 MB. È possibile distribuire tale importo tra l&#39;applicazione e ciascuno dei relativi servizi.

## Relazioni di servizio

Nei progetti di infrastruttura cloud di Adobe Commerce, il servizio [relazioni](../application/properties.md#relationships) configurato nel file `.magento.app.yaml` determina i servizi disponibili per l&#39;applicazione.

È possibile recuperare i dati di configurazione per tutte le relazioni di servizio dalla variabile di ambiente [`$MAGENTO_CLOUD_RELATIONSHIPS`](../environment/variables-cloud.md). I dati di configurazione includono il nome del servizio, il tipo e la versione insieme a tutti i dettagli di connessione richiesti, ad esempio il numero di porta e le credenziali di accesso.

**Per verificare le relazioni nell&#39;ambiente locale**:

1. Nell’ambiente locale, mostra le relazioni per l’ambiente attivo.

   ```bash
   magento-cloud relationships
   ```

1. Conferma `service` e `type` dalla risposta. La risposta fornisce informazioni sulla connessione, ad esempio l&#39;indirizzo IP e il numero di porta.

   >Risposta del campione abbreviata

   ```yaml
   redis:
       -
   ...
           type: 'redis:7.0'
           port: 6379
   elasticsearch:
       -
   ...
           type: 'opensearch:2'
           port: 9200
   database:
       -
   ...
           type: 'mysql:10.6'
           port: 3306
   ```

**Per verificare le relazioni negli ambienti remoti**:

1. Utilizza SSH per accedere all’ambiente remoto.

1. Elenca i dati di configurazione delle relazioni per tutti i servizi configurati nell’ambiente.

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   in alternativa, utilizzare il comando `ece-tools` seguente per visualizzare le relazioni:

   ```bash
   php ./vendor/bin/ece-tools env:config:show services
   ```

1. Conferma `service` e `type` dalla risposta. La risposta fornisce informazioni sulla connessione, ad esempio l&#39;indirizzo IP e il numero di porta, nonché eventuali credenziali di nome utente e password richieste.

## Versioni del servizio

Il supporto per la versione del servizio e la compatibilità per l’infrastruttura cloud di Adobe Commerce è determinato dalle versioni distribuite e testate nell’infrastruttura cloud e talvolta differisce dalle versioni supportate dalle distribuzioni Adobe Commerce on-premise. Consulta [Requisiti di sistema](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) nella guida _Installazione_ per un elenco delle dipendenze software di terze parti testate da Adobe con specifiche versioni di Adobe Commerce e di Magento Open Source.

### Controlli di fine del ciclo di vita del software

Durante il processo di distribuzione, il pacchetto `ece-tools` controlla le versioni del servizio installate rispetto alle date di fine del ciclo di vita (EOL) per ogni servizio.

- Se la versione di un servizio rientra nei tre mesi successivi alla data di fine del ciclo di vita, nel registro di distribuzione viene visualizzata una notifica.
- Se la data fine del ciclo di vita è nel passato, viene visualizzata una notifica di avviso.

Per mantenere la sicurezza dello store, aggiornare le versioni del software installato prima che raggiungano la fine del ciclo di vita. È possibile rivedere le date di fine del ciclo di vita nel file `eol.yaml` di [ece-tools&#39;](https://github.com/magento/ece-tools/blob/develop/config/eol.yaml).

### Migra a OpenSearch

{{elasticsearch-support}}

Per Adobe Commerce versione 2.4.4 e successive, vedere [Configurazione del servizio OpenSearch](opensearch.md).

## Modifica versione del servizio

Puoi aggiornare la versione del servizio installata per renderla compatibile con la versione di Adobe Commerce implementata nell’ambiente Cloud.

Non è possibile eseguire direttamente il downgrade della versione del servizio per un servizio installato. Tuttavia, puoi creare un servizio con la versione richiesta. Vedi [Versione servizio di downgrade](#downgrade-version).

### Aggiorna versione del servizio installata

È possibile aggiornare la versione del servizio installata aggiornando la configurazione del servizio nel file `services.yaml`.

1. Modificare il valore [`type`](#type) per il servizio nel file `.magento/services.yaml`:

   > Definizione del servizio originale

   ```yaml
   mysql:
       type: mysql:10.3
       disk: 2048
   ```

   > Definizione del servizio aggiornata

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add .magento/services.yaml
   ```

   ```bash
   git commit -m "Upgrade MySQL from MariaDB 10.3 to 10.4."
   ```

   ```bash
   git push origin <branch-name>
   ```

### Versione di downgrade

Non è possibile eseguire direttamente il downgrade di un servizio installato. Sono disponibili due opzioni:

1. Rinomina un servizio esistente con la nuova versione, che rimuove il servizio e i dati esistenti e ne aggiunge uno nuovo.

1. Crea un servizio e salva i dati dal servizio esistente.

Quando si modifica la versione del servizio, è necessario aggiornare la configurazione del servizio nel file `services.yaml` e aggiornare le relazioni nel file `.magento.app.yaml`.

**Per eseguire il downgrade di una versione del servizio rinominando un servizio esistente**:

1. Rinominare il servizio esistente nel file `.magento/services.yaml` e modificare la versione.

   >[!WARNING]
   >
   >La ridenominazione di un servizio esistente lo sostituisce ed elimina tutti i dati. Se devi conservare i dati, crea un servizio invece di rinominare quello esistente.

   Ad esempio, per eseguire il downgrade della versione di MariaDB per il servizio _mysql_ dalla versione 10.4 alla versione 10.3, modificare la configurazione esistente di _service-id_ e _type_.

   > Definizione `services.yaml` originale

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   ```

   > Nuova definizione `services.yaml`

   ```yaml
   mysql2:
        type: mysql:10.3
        disk: 5120
   ```

1. Aggiornare le relazioni nel file `.magento.app.yaml`.

   > Configurazione `.magento.app.yaml` originale

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Aggiornamento della configurazione di `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Aggiungi, esegui il commit e invia le modifiche al codice.

**Per eseguire il downgrade di un servizio creando un servizio**:

1. Aggiungere una definizione di servizio al file `services.yaml` per il progetto con la specifica della versione ridotta. Vedi _mysql2_ nell&#39;esempio seguente:

   > services.yaml

   ```yaml
   mysql:
       type: mysql:10.4
       disk: 5120
   mysql2:
       type: mysql:10.3
       disk: 5120
   ```

1. Modificare la configurazione delle relazioni nel file `.magento.app.yaml` per utilizzare il nuovo servizio.

   > Configurazione `.magento.app.yaml` originale

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

   > Nuova configurazione di `.magento.app.yaml`

   ```yaml
   relationships:
       database: "mysql2:mysql"
   ```

1. Aggiungi, esegui il commit e invia le modifiche al codice.
