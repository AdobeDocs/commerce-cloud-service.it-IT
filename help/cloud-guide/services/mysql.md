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

Il `mysql` fornisce archiviazione dei dati persistente basata su [MariaDB](https://mariadb.com/) versioni da 10.2 a 10.4, che supportano [XtraDB](https://docs.percona.com/percona-xtradb-cluster/8.0/index.html) il motore di storage e le funzionalità reimplementate da MySQL 5.6 e 5.7.

La reindicizzazione su MariaDB 10.4 richiede più tempo rispetto ad altre versioni di MariaDB o MySQL. Consulta [Indicizzatori](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers) nel _Best practice per le prestazioni_ guida.

>[!WARNING]
>
>Presta attenzione quando aggiorni MariaDB dalla versione 10.1 alla versione 10.2. MariaDB 10.1 è l&#39;ultima versione che supporta _XtraDB_ come motore di archiviazione. MariaDB 10.2 utilizza _InnoDB_ per il motore di archiviazione. Dopo l’aggiornamento da 10.1 a 10.2, non è possibile ripristinare la modifica. Adobe Commerce supporta entrambi i motori di archiviazione; tuttavia, è necessario controllare le estensioni e gli altri sistemi utilizzati dal progetto per assicurarsi che siano compatibili con MariaDB 10.2. Consulta [Modifiche non compatibili tra 10.1 e 10.2](https://mariadb.com/kb/en/upgrading-from-mariadb-101-to-mariadb-102/#incompatible-changes-between-101-and-102).

{{service-instruction}}

**Per abilitare MySQL**:

1. Aggiungere il nome, il tipo e il valore del disco richiesti (in MB) al `.magento/services.yaml` file.

   ```yaml
   mysql:
       type: mysql:<version>
       disk: 5120
   ```

   >[!TIP]
   >
   >Errori MySQL, ad esempio `PDO Exception: MySQL server has gone away`, può verificarsi a causa di spazio su disco insufficiente. Verificare di aver allocato spazio su disco sufficiente al servizio nella [`.magento/services.yaml`](services-yaml.md#disk) file.

1. Configurare le relazioni in `.magento.app.yaml` file.

   ```yaml
   relationships:
       database: "mysql:mysql"
   ```

1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable mysql service" && git push origin <branch-name>
   ```

1. [Verificare le relazioni tra i servizi](services-yaml.md#service-relationships).

{{service-change-tip}}

## Configura database MySQL

Durante la configurazione del database MySQL sono disponibili le seguenti opzioni:

- **`schemas`**- Uno schema definisce un database. Lo schema predefinito è `main` database.
- **`endpoints`**- Ogni endpoint rappresenta una credenziale con privilegi specifici. L’endpoint predefinito è `mysql`, che ha `admin` accesso a `main` database.
- **`properties`**- Le proprietà vengono utilizzate per definire configurazioni di database aggiuntive.

Di seguito è riportato un esempio di configurazione di base in `.magento/services.yaml` file:

```yaml
mysql:
    type: mysql:10.4
    disk: 5120
    configuration:
        properties:
            optimizer_switch: "rowid_filter=off"
            optimizer_use_condition_selectivity: 1
```

Il `properties` nell&#39;esempio precedente modifica il valore predefinito `optimizer` impostazioni come [consigliato nella guida alle best practice per le prestazioni](https://experienceleague.adobe.com/docs/commerce-operations/performance-best-practices/configuration.html#indexers).

**Opzioni di configurazione MariaDB**:

| Opzioni | Descrizione | Valore predefinito |
| -------------------- | --------------------------------------------------- | ------------------ |
| `default_charset` | Il set di caratteri predefinito. | utf8mb4 |
| `default_collation` | Regole di confronto predefinite. | utf8mb4_unicode_ci |
| `max_allowed_packet` | Dimensioni massime per i pacchetti, in MB. Intervallo `1` a `100`. | 16 |
| `optimizer_switch` | Impostare i valori per l&#39;ottimizzatore di query. Consulta [Documentazione di MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_switch). | |
| `optimizer_use_condition_selectivity` | Selezionare le statistiche utilizzate dall&#39;ottimizzatore. Intervallo `1` a `5`. Consulta [Documentazione di MariaDB](https://mariadb.com/kb/en/server-system-variables/#optimizer_use_condition_selectivity). | 4 per 10.4 e versioni successive |

### Configurazione di più utenti del database

Facoltativamente, puoi impostare più utenti con autorizzazioni diverse per accedere al `main` database.

Per impostazione predefinita, esiste un endpoint denominato `mysql` che dispone dell&#39;accesso di amministratore al database. Per impostare più utenti del database, è necessario definire più endpoint nel `services.yaml` e dichiarare le relazioni in `.magento.app.yaml` file. Per ambienti di staging e produzione Pro, [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per richiedere l&#39;utente aggiuntivo.

Utilizza un array nidificato per definire gli endpoint per l’accesso utente specifico. Ogni endpoint può designare l&#39;accesso a uno o più schemi (database) e livelli di autorizzazione diversi per ciascuno di essi.

I livelli di autorizzazione validi sono:

- `ro`: sono consentite solo le query SELECT.
- `rw`: sono consentite le query SELECT e INSERT, UPDATE e DELETE.
- `admin`: sono consentite tutte le query, incluse le query DDL (CREATE TABLE, DROP TABLE e altre).

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

Nell&#39;esempio precedente, il `admin` fornisce accesso a livello di amministratore al `main` database, il `reporter` fornisce accesso in sola lettura e `importer` l&#39;endpoint fornisce accesso in lettura/scrittura, ovvero:

- Il `admin` l&#39;utente ha il controllo completo del database.
- Il `reporter` L&#39;utente dispone solo dei privilegi SELECT.
- Il `importer` L&#39;utente dispone dei privilegi SELECT, INSERT, UPDATE e DELETE.

Aggiungi gli endpoint definiti nell&#39;esempio precedente al `relationships` proprietà del `.magento.app.yaml` file. Ad esempio:

```yaml
relationships:
    database: "mysql:admin"
    databasereporter: "mysql:reporter"
    databaseimporter: "mysql:importer"
```

>[!NOTE]
>
>Se si configura un utente MySQL, non è possibile utilizzare [`DEFINER`](https://dev.mysql.com/doc/refman/8.0/en/show-grants.html) meccanismo di controllo degli accessi per stored procedure e viste.

## Connettersi al database

Per accedere direttamente al database MariaDB è necessario utilizzare un SSH per accedere all&#39;ambiente Cloud remoto e connettersi al database.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Recuperare le credenziali di accesso MySQL da `database` e `type` proprietà in [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) variabile.

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

   - Per Pro, utilizza il seguente comando con nome host, numero di porta, nome utente e password recuperati da `$MAGENTO_CLOUD_RELATIONSHIPS` variabile.

     ```bash
     mysql -h <hostname> -P <number> -u <username> -p'<password>'
     ```

>[!TIP]
>
>È possibile utilizzare `magento-cloud db:sql` per connettersi al database remoto ed eseguire i comandi SQL.

## Connetti al database secondario

>[!IMPORTANT]
>
>Questa funzione è disponibile solo nei cluster Pro Production e Staging.

A volte è necessario connettersi al database secondario per migliorare le prestazioni del database o risolvere i problemi di blocco del database. Se questa configurazione è necessaria, utilizza `"port" : 3304` per stabilire la connessione. Consulta la [Procedure consigliate per configurare la connessione slave MySQL](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/mysql-configuration.html) argomento in _Best practice di implementazione_ guida.

## Risoluzione dei problemi

Consulta i seguenti articoli di supporto Adobe Commerce per assistenza nella risoluzione dei problemi di MySQL:

- [Verifica delle query lente ed elabora MySQL](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/database/checking-slow-queries-and-processes-mysql.html)
- [Crea dump del database su Cloud](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/create-database-dump-on-cloud.html)
- [Risoluzione dei problemi relativi allo strumento di migrazione dei dati](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/data-migration-tool-troubleshooting.html)
- [Aggiornamento Adobe Commerce: da compatto a tabelle dinamiche 2.2.x, da 2.3.x a 2.4.x](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/maintenance/commerce-235-upgrade-prerequisites-mariadb.html)
