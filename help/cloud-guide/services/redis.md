---
title: Configurazione del servizio Redis
description: Scopri come impostare e ottimizzare Redis come soluzione di cache back-end per Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Cache, Services
exl-id: d6971875-d302-495a-ad10-a81c507c2bc9
source-git-commit: 1253d8357fd2554050d1775fefbc420a2097db5f
workflow-type: tm+mt
source-wordcount: '255'
ht-degree: 0%

---

# Configurazione del servizio Redis

[Redis](https://redis.io) è una soluzione cache back-end opzionale che sostituisce Zend Framework Zend_Cache_Backend_File, utilizzato da Adobe Commerce per impostazione predefinita.

Vedere [Configurare Redis](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cache/redis/config-redis.html) nella _Guida alla configurazione_.

{{service-instruction}}

**Per abilitare Redis**:

1. Aggiungere il nome e il tipo richiesti al file `.magento/services.yaml`.

   ```yaml
   myredis:
       type: redis:<version>
   ```

   Per fornire la tua configurazione Redis, aggiungi una chiave `core_config` nel file `.magento/services.yaml`:

   ```yaml
   cache:
       type: redis:<version>
   ```

1. Configurare le relazioni nel file `.magento.app.yaml`.

   ```yaml
   runtime:
       extensions:
           - redis
   
   relationships:
       redis: "redis:redis"
   ```

1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add .magento/services.yaml .magento.app.yaml && git commit -m "Enable redis service" && git push origin <branch-name>
   ```

1. [Verificare le relazioni del servizio](services-yaml.md#service-relationships).

{{service-change-tip}}

## Utilizzo di Redis CLI

Se la relazione Redis si chiama `redis`, è possibile accedervi utilizzando lo strumento `redis-cli`.

1. Utilizza SSH per connettersi all’ambiente di integrazione con Redis installato e configurato.

1. Aprire un tunnel SSH a un host.

   ```bash
   redis-cli -h redis.internal
   ```

## Ottieni versione Redis installata

Utilizza il seguente comando per installare la versione di Redis in un ambiente di integrazione:

```bash
redis-cli -h redis.internal info | grep version
```

Risposta di esempio:

```terminal
redis_version:7.0.5
gcc_version:8.3.0
```

### Redis su staging e produzione Pro

Per ottenere la versione Redis installata in un ambiente di staging o produzione, utilizzare il comando `redis-server`:

```bash
redis-server -v
```

```terminal
Redis server v=7.0.5 ...
```

Utilizzate il seguente comando per installare la configurazione Redis in un ambiente Pro Staging o Production:

```bash
echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
```

Risposta di esempio:

```terminal
"redis" : [
    {
        "cluster" : "project-master-123abc4",
        "fragment" : null,
        "host" : "redis.internal",
        "host_mapped" : false,
        "hostname" : "oblahblahblahblahe.redis.service._.magentosite.cloud",
        "ip" : "169.254.10.10",
        "password" : null,
        "path" : null,
        "port" : 6379,
        "public" : false,
        "query" : {},
        "rel" : "redis",
        "scheme" : "redis",
        "service" : "redis",
        "type" : "redis:7.0.5",
        "username" : null
    }
]
```

## Risoluzione dei problemi di Redis

Consulta i seguenti articoli sul supporto Adobe Commerce per assistenza nella risoluzione dei problemi Redis:

- [L&#39;accesso o l&#39;estrazione dell&#39;amministratore in seguito a un problema Redis ritarda](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-issue-delay-magento-admin-login-or-checkout.html)
- [Implementazione della cache Redis estesa Adobe Commerce 2.3.5+](https://experienceleague.adobe.com/docs/commerce-operations/implementation-playbook/best-practices/planning/redis-service-configuration.html)
- [MDVA-30102: cache Redis piena](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/patches/v1-0-6/mdva-30102-magento-patch-redis-cache-getting-full.html)
- [Avvisi gestiti su Adobe Commerce: avviso di memoria Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-warning-alert.html)
- [Avvisi gestiti su Adobe Commerce: avviso critico memoria Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/support-tools/managed-alerts/managed-alerts-on-magento-commerce-redis-memory-critical-alert.html)
- [Risoluzione dei problemi di Redis](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/redis-troubleshooter.html)
