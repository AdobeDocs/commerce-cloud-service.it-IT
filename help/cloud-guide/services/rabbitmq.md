---
title: Configura servizio RabbitMQ
description: Scopri come abilitare il servizio RabbitMQ per gestire le code di messaggi per Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Services
exl-id: 85794b8f-2260-4a6e-b5a6-a1b4c356594e
source-git-commit: adcfbb7217c70122a4003a66d1bec1a623fbf11a
workflow-type: tm+mt
source-wordcount: '399'
ht-degree: 0%

---

# Configurazione [!DNL RabbitMQ] servizio

Il [Framework coda messaggi (MQF)](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/message-queues/message-queue-framework.html) è un sistema all’interno di Adobe Commerce che consente [modulo](https://glossary.magento.com/module) per pubblicare i messaggi nelle code. Definisce inoltre i consumatori che ricevono i messaggi in modo asincrono.

MQF utilizza [RabbitMQ](https://www.rabbitmq.com/) as the messaging broker, che fornisce una piattaforma scalabile per l’invio e la ricezione di messaggi. Include inoltre un meccanismo per l’archiviazione dei messaggi non consegnati. [!DNL RabbitMQ] è basato sulla specifica AMQP 0.9.1.

>[!WARNING]
>
>Se preferisci utilizzare un servizio basato su AMQP esistente, come [!DNL RabbitMQ], invece di affidarsi a Adobe Commerce sull’infrastruttura cloud per crearla, utilizza [`QUEUE_CONFIGURATION`](../environment/variables-deploy.md#queue_configuration) variabile di ambiente per collegarla al sito.

{{service-instruction}}

**Per abilitare RabbitMQ**:

1. Aggiungere il nome, il tipo e il valore del disco richiesti (in MB) al `.magento/services.yaml` insieme alla versione di RabbitMQ installata.

   ```yaml
   rabbitmq:
       type: rabbitmq:<version>
       disk: 1024
   ```

1. Configurare le relazioni in `.magento.app.yaml` file.

   ```yaml
   relationships:
       rabbitmq: "rabbitmq:rabbitmq"
   ```

1. Aggiungi, esegui il commit e invia le modifiche al codice.

   ```bash
   git add .magento/services.yaml .magento.app.yaml
   ```

   ```bash
   git commit -m "Enable RabbitMQ service"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. [Verificare le relazioni tra i servizi](services-yaml.md#service-relationships).

{{service-change-tip}}

## Connetti a RabbitMQ per il debug

A scopo di debug, è utile connettersi direttamente a un’istanza del servizio in uno dei seguenti modi:

- Connettersi dall’ambiente di sviluppo locale
- Connettersi dall’applicazione
- Connessione dall&#39;applicazione PHP

### Connettersi dall’ambiente di sviluppo locale

1. Accedi a `magento-cloud` CLI e progetto:

   ```bash
   magento-cloud login
   ```

1. Controlla l’ambiente con RabbitMQ installato e configurato.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Utilizza SSH per connettersi all’ambiente Cloud:

   ```bash
   magento-cloud ssh
   ```

1. Recupera i dettagli della connessione RabbitMQ e le credenziali di accesso da [$MAGENTO_CLOUD_RELATIONSHIPS](../application/properties.md#relationships) variabile:

   ```bash
   echo $MAGENTO_CLOUD_RELATIONSHIPS | base64 -d | json_pp
   ```

   o

   ```bash
   php -r 'print_r(json_decode(base64_decode($_ENV["MAGENTO_CLOUD_RELATIONSHIPS"])));'
   ```

   Nella risposta, trova le informazioni di RabbitMQ, ad esempio:

   ```json
   {
      "rabbitmq" : [
         {
            "password" : "guest",
            "ip" : "246.0.129.2",
            "scheme" : "amqp",
            "port" : 5672,
            "host" : "rabbitmq.internal",
            "username" : "guest"
         }
      ]
   }
   ```

1. Abilita l’inoltro porta locale a RabbitMQ (se il progetto si trova in un’area diversa, ad esempio Stati Uniti-3, UE-5 o area AP-3, sostituisci ``us-3``/``eu-5``/``ap-3`` per ``us``)

   ```bash
   ssh -L <port-number>:rabbitmq.internal:<port-number> <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

   Un esempio per accedere all’interfaccia web di gestione RabbitMQ all’indirizzo `http://localhost:15672` è:

   ```bash
   ssh -L 15672:rabbitmq.internal:15672 <project-ID>-<branch-ID>@ssh.us.magentosite.cloud
   ```

1. Quando la sessione è aperta, è possibile avviare un client RabbitMQ a scelta dalla workstation locale, configurato per la connessione al `localhost:<portnumber>` utilizzando le informazioni relative a numero di porta, nome utente e password della variabile MAGENTO_CLOUD_RELATIONSHIPS.

### Connettersi dall’applicazione

Per connettersi a RabbitMQ in esecuzione in un&#39;applicazione, installare un client, ad esempio [amqp-utils](https://github.com/dougbarth/amqp-utils), come una dipendenza del progetto nel tuo `.magento.app.yaml` file.

Ad esempio:

```yaml
dependencies:
    ruby:
        amqp-utils: "0.5.1"
```

Quando si accede al contenitore PHP, si immette qualsiasi `amqp-` comando disponibile per la gestione delle code.

### Connessione dall&#39;applicazione PHP

Per connettersi a RabbitMQ utilizzando l&#39;applicazione PHP, aggiungere un PHP [libreria](https://glossary.magento.com/library) alla struttura di origine.
