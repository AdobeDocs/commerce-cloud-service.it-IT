---
title: Configura servizio MySQL
description: Scopri come gestire il servizio MySQL per l’archiviazione persistente dei dati con Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Services, Storage
exl-id: 70820d00-8b82-4b60-87e4-ea98fd7ffcb2
source-git-commit: 9be8d1e062ab3dba86bc4d9c9ee8b8ece33d5b75
workflow-type: tm+mt
source-wordcount: '773'
ht-degree: 1%

---

# Configura servizio MySQL

Il servizio `mysql` fornisce l&#39;archiviazione dei dati persistenti in base alle versioni da 10.2 a 10.4 di [MariaDB](https://mariadb.com/), che supportano il motore di archiviazione [XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) e le funzionalità reimplementate di MySQL 5.6 e 5.7.

La reindicizzazione su MariaDB 10.4 richiede più tempo rispetto ad altre versioni di MariaDB o MySQL. Consulta [Indicizzatori](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers) nella _Guida alle best practice per le prestazioni_.

>[!WARNING]
>
>Presta attenzione quando aggiorni MariaDB dalla versione 10.1 alla versione 10.2. MariaDB 10.1 è l&#39;ultima versione che supporta _XtraDB_ come motore di archiviazione. MariaDB 10.2 utilizza _InnoDB_ per il motore di archiviazione. Dopo l’aggiornamento da 10.1 a 10.2, non è possibile ripristinare la modifica. Adobe Commerce supporta entrambi i motori di archiviazione; tuttavia, è necessario controllare le estensioni e gli altri sistemi utilizzati dal progetto per assicurarsi che siano compatibili con MariaDB 10.2. Vedere [Modifiche non compatibili tra 10.1 e 10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**Per abilitare MySQL**:

1. Aggiungere il nome, il tipo e il valore del disco richiesti (in MB) al file `.magento/services.yaml`.

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >Gli errori MySQL, ad esempio `PDO Exception: MySQL server has gone away`, possono verificarsi a causa di spazio su disco insufficiente. Verificare di aver allocato spazio su disco sufficiente al servizio nel file [`.magento/services.yaml`](services-yaml.md#disk).

1. Configurare le relazioni nel file `.magento.app.yaml`.

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [Verificare le relazioni del servizio](services-yaml.md#service-relationships).

{{service-change-tip}}

## Configura database MySQL

Durante la configurazione del database MySQL sono disponibili le seguenti opzioni:

- **`schemas`** - Uno schema definisce un database. Lo schema predefinito è il database `main`.
- **`endpoints`** - Ogni endpoint rappresenta una credenziale con privilegi specifici. L&#39;endpoint predefinito è `mysql`, che ha accesso `admin` al database `main`.
- **`properties`** - Le proprietà vengono utilizzate per definire configurazioni di database aggiuntive.

Di seguito è riportato un esempio di configurazione di base nel file `.magento/services.yaml`:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

`properties` nell&#39;esempio precedente modifica le impostazioni predefinite di `optimizer` come [consigliato nella guida alle best practice per le prestazioni](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers).

**Opzioni di configurazione MariaDB**:

| Opzioni | Descrizione | Valore predefinito |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | Il set di caratteri predefinito. | utf8mb4 |
| `default_collation` | Regole di confronto predefinite. | utf8mb4_unicode_ci |
| `max_allowed_packet` | Dimensioni massime per i pacchetti, in MB. Da `1` a `100`. | 16 |
| `optimizer_switch` | Impostare i valori per l&#39;ottimizzatore di query. Consulta la [documentazione di MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | Selezionare le statistiche utilizzate dall&#39;ottimizzatore. Da `1` a `5`. Consulta la [documentazione di MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 4 per 10.4 e versioni successive |

### Configurazione di più utenti del database

In alternativa, è possibile impostare più utenti con autorizzazioni diverse per accedere al database `main`.

Per impostazione predefinita, un endpoint denominato `mysql` dispone dell&#39;accesso di amministratore al database. Per impostare più utenti del database, è necessario definire più endpoint nel file `services.yaml` e dichiarare le relazioni nel file `.magento.app.yaml`. Per gli ambienti di staging e produzione Pro, [Invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per richiedere l&#39;utente aggiuntivo.

Utilizza un array nidificato per definire gli endpoint per l’accesso utente specifico. Ogni endpoint può designare l&#39;accesso a uno o più schemi (database) e livelli di autorizzazione diversi per ciascuno di essi.

I livelli di autorizzazione validi sono:

- `ro`: sono consentite solo le query SELECT.
- `rw`: le query SELECT e le query INSERT, UPDATE e DELETE sono consentite.
- `admin`: tutte le query sono consentite, incluse le query DDL (CREATE TABLE, DROP TABLE e altre).

Ad esempio:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        schemas:
            - main
        endpoints:
            admin:
                default_schema: main
                privileges:
                    main: admin
            reporter:
                privileges:
                    main: ro
            importer:
                privileges:
                    main: rw
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

Nell&#39;esempio precedente, l&#39;endpoint `admin` fornisce l&#39;accesso a livello di amministratore al database `main`, l&#39;endpoint `reporter` fornisce l&#39;accesso in sola lettura e l&#39;endpoint `importer` fornisce l&#39;accesso in lettura/scrittura, ovvero:

- L&#39;utente `admin` ha il controllo completo del database.
- L&#39;utente `reporter` dispone solo dei privilegi SELECT.
- L&#39;utente `importer` dispone dei privilegi SELECT, INSERT, UPDATE e DELETE.

Aggiungere gli endpoint definiti nell&#39;esempio precedente alla proprietà `relationships` del file `.magento.app.yaml`. Ad esempio:

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>Se si configura un utente MySQL, non è possibile utilizzare il meccanismo di controllo dell&#39;accesso [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) per le stored procedure e le visualizzazioni.

## Connettersi al database

Per accedere direttamente al database MariaDB è necessario utilizzare un SSH per accedere all&#39;ambiente Cloud remoto e connettersi al database.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recuperare le credenziali di accesso MySQL dalle proprietà `database` e `type` nella variabile [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships).

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Nella risposta, trovare le informazioni MySQL. Ad esempio:

   ```json
   "database" : [
      {
         "password" : "",
         "rel" : "mysql",
         "hostname" : "nnnnnnnn.mysql.service._.magentosite.cloud",
         "service" : "mysql",
         "host" : "database.internal",
         "ip" : "###.###.###.###",
         "port" : 3306,
         "path" : "main",
         "cluster" : "projectid-integration-id",
         "query" : {
            "is_master" : true
         },
         "type" : "mysql:10.3",
         "username" : "user",
         "scheme" : "mysql"
      }
   ],
   ```

1. Connettersi al database.

   - Per iniziare, utilizzare il comando seguente:

     ```bash
     mysql -h database.internal -u <username>
     ```

   - Per Pro, utilizzare il comando seguente con nome host, numero di porta, nome utente e password recuperati dalla variabile `$MAGENTO_CLOUD_RELATIONSHIPS`.

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>È possibile utilizzare il comando `magento-cloud db:sql` per connettersi al database remoto ed eseguire i comandi SQL.

## Connetti al database secondario

>[!IMPORTANT]
>
>Questa funzione è disponibile solo nei cluster Pro Production e Staging.

A volte è necessario connettersi al database secondario per migliorare le prestazioni del database o risolvere i problemi di blocco del database. Se questa configurazione è necessaria, utilizzare `"port" : 3304` per stabilire la connessione. Consulta l&#39;argomento [Best practice per configurare la connessione slave MySQL](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html) nella _Guida alle best practice per l&#39;implementazione_.

## Risoluzione dei problemi

Consulta i seguenti articoli di supporto Adobe Commerce per assistenza nella risoluzione dei problemi di MySQL:

- [Controllo delle query lente ed elaborazione di MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [Crea dump del database nel cloud](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [Risoluzione dei problemi relativi allo strumento di migrazione dei dati](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [Aggiornamento Adobe Commerce: compatta in tabelle dinamiche 2.2.x, da 2.3.x a 2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
