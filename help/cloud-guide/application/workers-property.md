---
title: Lavoratori
description: Scopri come configurare la proprietà worker in [!DNL Commerce] file di configurazione dell'applicazione.
feature: Cloud, Configuration
exl-id: d6816925-5912-45ca-8255-6c307e58542d
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 0%

---

# Proprietà Workers

È possibile definire un processo di lavoro da eseguire in modo indipendente dall&#39;istanza Web senza un&#39;istanza Nginx in esecuzione. Tuttavia, il processo di lavoro utilizza la stessa archiviazione di rete utilizzata dal [!DNL Commerce] applicazione. Non è necessario configurare un server Web sull&#39;istanza di lavoro (utilizzando Node.js o Go) perché il router non può indirizzare le richieste pubbliche al worker. Questo rende l’istanza di lavoro ideale per le attività in background o in esecuzione continua che rischiano di bloccare una distribuzione.

## Configurare un lavoratore

I processi di lavoro sono disponibili per l&#39;utilizzo solo con gli ambienti di staging e produzione Pro. Gli ambienti Pro Integration e Starter possono scegliere di utilizzare [CRON_CONSUMER_RUNNER](../environment/variables-deploy.md#cron_consumers_runner) variabile.

Per configurare un lavoratore in Pro Staging o Produzione, [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) e includono le seguenti informazioni:

- ID Progetto
- ID ambiente
- Nome lavoratore
- Comandi Start

È possibile configurare un processo per ogni lavoratore. Una configurazione di lavoro di base e comune in `.magento.app.yaml` il file potrebbe avere un aspetto simile al seguente:

```yaml
workers:
    queue:
        commands:
            start: |
                php ./bin/magento queue:consumers:start commerce.eventing.event.publish
```

Questo esempio definisce un singolo lavoratore denominato `queue`, con un livello ridotto (dimensione S) di allocazione delle risorse ed esegue `php ./bin/magento` all&#39;avvio. Il lavoratore `queue` viene quindi eseguito su ogni nodo come processo di lavoro. Se il comando viene chiuso, viene riavviato automaticamente.

## Comandi e sostituzioni

Il `commands.start` per avviare i comandi con l&#39;applicazione di lavoro è necessaria la chiave. Puoi utilizzare qualsiasi comando shell valido, anche se è ideale utilizzare la lingua dell’applicazione. Se il comando specificato da `start` termina, si riavvia automaticamente.

>[!IMPORTANT]
>
>Il `deploy` e `post_deploy` ganci e `crons` i comandi vengono eseguiti solo sul contenitore web, non nelle istanze di worker.

### Ereditarietà

Definizioni per `size`, `relationships`, `access`, `disk` e `mount`, e `variables` Le proprietà vengono ereditate da un lavoratore, a meno che non vengano esplicitamente ignorate.

Le seguenti proprietà sono quelle più comunemente utilizzate per sostituire [impostazioni di primo livello](properties.md):

- `size`- allocazione di un numero inferiore di risorse a un unico processo in background
- `variables`- Istruisce l&#39;esecuzione dell&#39;applicazione in modo diverso

### Tempistica e accodamento

Anche se ogni lavoratore si trova dietro un altro, la configurazione seguente produce una separazione di due secondi delle marche temporali nella `var/time.txt` indipendentemente dalla sospensione di otto secondi nel codice PHP:

```yaml
workers:
    time1:
        commands:
            start: 'php -r "sleep(8); echo time() . PHP_EOL;" >> var/time.txt& sleep 2'
```
