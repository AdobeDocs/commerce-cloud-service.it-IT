---
title: Proprietà
description: Utilizzare l'elenco delle proprietà come riferimento durante la configurazione del [!DNL Commerce] per la generazione e la distribuzione nell'infrastruttura cloud.
feature: Cloud, Configuration, Build, Deploy, Roles/Permissions, Storage
exl-id: 58a86136-a9f9-4519-af27-2f8fa4018038
source-git-commit: 99272d08a11f850a79e8e24857b7072d1946f374
workflow-type: tm+mt
source-wordcount: '797'
ht-degree: 0%

---

# Proprietà per la configurazione dell&#39;applicazione

Il `.magento.app.yaml` utilizza le proprietà per gestire il supporto dell&#39;ambiente per [!DNL Commerce] applicazione.

| Nome | Descrizione | Predefinito | Obbligatorio |
| ------ | --------------------------------- | ------- | -------- |
| [`access`](#access) | Personalizzare i ruoli utente | — | No |
| [`crons`](crons-property.md) | Aggiorna le specifiche e pianifica i processi cron | — | No |
| [`dependencies`](#dependencies) | Abilita dipendenze aggiuntive | `php:composer/composer: '2.2.4'` | No |
| [`disk`](#disk) | Definire la dimensione del disco persistente | `5120` | Sì |
| [`firewall`](firewall-property.md) | (Solo Starter) Controlla il traffico in uscita | — | No |
| [`hooks`](hooks-property.md) | Personalizzare i comandi della shell per le fasi di compilazione, distribuzione e post-distribuzione | — | No |
| [`mounts`](#mounts) | Impostare i percorsi | Percorsi:<ul><li>`"var": "shared:files/var"`</li><li>`"app/etc": "shared:files/etc"`</li><li>`"pub/media": "shared:files/media"`</li><li>`"pub/static": "shared:files/static"`</li></ul> | No |
| [`name`](#name) | Definisci il nome dell’applicazione | `mymagento` | Sì |
| [`relationships`](#relationships) | Servizi mappa | Servizi:<ul><li>`database: "mysql:mysql"`</li><li>`redis: "redis:redis"`</li><li>`opensearch: "opensearch:opensearch"`</li></ul> | No |
| [`runtime`](#runtime) | La proprietà runtime include le estensioni richieste da [!DNL Commerce] applicazione. | Estensioni:<ul><li>`xsl`</li><li>`newrelic`</li><li>`sodium`</li></ul> | Sì |
| [`type`](#type-and-build) | Impostare l&#39;immagine contenitore di base | `php:8.3` | Sì |
| [`variables`](variables-property.md) | Applicare una variabile di ambiente per una versione di Commerce specifica | — | No |
| [`web`](web-property.md) | Gestire le richieste esterne | — | Sì |
| [`workers`](workers-property.md) | Gestire le richieste esterne | — | Sì, se non si utilizza la proprietà web |

{style="table-layout:auto"}

## `name`

Il `name` fornisce il nome dell&#39;applicazione utilizzato nel [`routes.yaml`](../routes/routes-yaml.md) per definire il flusso HTTP a monte (per impostazione predefinita, `mymagento:http`). Ad esempio, se il valore di `name` è `app`, è necessario utilizzare `app:http` nel campo a monte.

>[!WARNING]
>
>Non modificare il nome dell&#39;applicazione dopo che è stata distribuita. In questo modo si verifica una perdita di dati.

## `type` e `build`

Il `type`  e `build` Le proprietà forniscono informazioni sull&#39;immagine contenitore di base per generare ed eseguire il progetto.

Supportata `type` Il linguaggio è PHP. Specificare la versione PHP come segue:

```yaml
type: php:<version>
```

Il `build` determina ciò che accade per impostazione predefinita durante la creazione del progetto. Il `flavor` specifica un set predefinito di attività di compilazione da eseguire. L’esempio seguente mostra la configurazione predefinita per `type` e `build` da `magento-cloud/.magento.app.yaml`:

```yaml
# The toolstack used to build the application.
type: php:8.3
build:
    flavor: none

dependencies:
    php:
        composer/composer: '2.7.2'
```

### Installazione e utilizzo di Composer 2

Il `build: flavor:` La proprietà non viene utilizzata per Composer 2.x; pertanto, è necessario installare manualmente Composer durante la fase di build. Per installare e utilizzare Composer 2.x nei progetti Starter e Pro, è necessario apportare tre modifiche al `.magento.app.yaml` configurazione:

1. Rimuovi `composer` come `build: flavor:` e aggiungi `none`. Questa modifica impedisce a Cloud di utilizzare la versione predefinita 1.x di Composer per eseguire attività di build.
1. Aggiungi `composer/composer: '^2.0'` as a `php` dipendenza per l’installazione di Composer 2.x.
1. Aggiungi il `composer` creare attività in un `build` per eseguire le attività di generazione mediante Composer 2.x.

Utilizza i seguenti frammenti di configurazione nel tuo `.magento.app.yaml` configurazione:

```yaml
# 1. Change flavor to none.
build:
    flavor: none

# 2. Add Composer ^2.0 as a php dependency.
dependencies:
    php:
        composer/composer: '^2.0'

# 3. Add a build hook to run the build tasks using Composer 2.x.
hooks:
    build: |
        set -e
        composer --no-ansi --no-interaction install --no-progress --prefer-dist --optimize-autoloader
```

Consulta [Pacchetti richiesti](../development/overview.md#required-packages) per ulteriori informazioni su Compositore.

## `dependencies`

Specifica le dipendenze che potrebbero essere necessarie all&#39;applicazione durante il processo di compilazione.

Adobe Commerce supporta le dipendenze dalle seguenti lingue:

- PHP
- Rubino
- Node.js

Tali dipendenze sono indipendenti dalle dipendenze finali dell’applicazione e sono disponibili nel `PATH`, durante il processo di generazione e nell&#39;ambiente di runtime dell&#39;applicazione.

È possibile specificare tali dipendenze nel modo seguente:

```yaml
ruby:
   sass: "~3.4"
nodejs:
   grunt-cli: "~0.3"
```

## `runtime`

Utilizzare per modificare la configurazione PHP in fase di esecuzione, ad esempio per abilitare le estensioni. Sono necessarie le seguenti estensioni:

```yaml
runtime:
    extensions:
        - xsl
        - newrelic
        - sodium
```

Consulta [Impostazioni PHP](php-settings.md) per informazioni dettagliate sull’abilitazione delle estensioni.

## `disk`

Definisce la dimensione del disco persistente dell&#39;applicazione in MB.

```yaml
disk: 5120
```

La dimensione minima consigliata del disco è 256 MB. Se viene visualizzato l&#39;errore `UserError: Error building the project: Disk size may not be smaller than 128MB`, aumentano le dimensioni a 256 MB.

>[!NOTE]
>
>Per gli ambienti di staging e produzione Pro, è necessario [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per aggiornare `mounts` e `disk` per la tua applicazione. Quando invii il ticket, indica le modifiche di configurazione richieste e includi una versione aggiornata del `.magento.app.yaml` file.

## `relationships`

Definisce il mapping del servizio nell&#39;applicazione.

La relazione `name` è disponibile per l’applicazione in `MAGENTO_CLOUD_RELATIONSHIPS` variabile di ambiente. Il `<service-name>:<endpoint-name>` la relazione viene mappata ai valori di nome e tipo definiti nella `.magento/services.yaml` file.

```yaml
relationships:
    <name>: "<service-name>:<endpoint-name>"
```

Di seguito è riportato un esempio delle relazioni predefinite:

```yaml
relationships:
    database: "mysql:mysql"
    redis: "redis:redis"
    opensearch: "opensearch:opensearch"
    rabbitmq: "rabbitmq:rabbitmq"
```

Consulta [Servizi](../services/services-yaml.md) per un elenco completo dei tipi di servizio e degli endpoint attualmente supportati.

## `mounts`

Oggetto le cui chiavi sono percorsi relativi alla radice dell&#39;applicazione. Il montaggio è un&#39;area scrivibile sul disco per i file. Di seguito è riportato un elenco predefinito di mount configurati in `magento.app.yaml` file che utilizza `volume_id[/subpath]` sintassi:

```yaml
 # The mounts that will be performed when the package is deployed.
mounts:
    "var": "shared:files/var"
    "app/etc": "shared:files/etc"
    "pub/media": "shared:files/media"
    "pub/static": "shared:files/static"
```

Il formato per l&#39;aggiunta del montaggio all&#39;elenco è il seguente:

```bash
"/public/sites/default/files": "shared:files/files"
```

- `shared`- Consente di condividere un volume tra le applicazioni all&#39;interno di un ambiente.
- `disk`- Definisce la dimensione disponibile per il volume condiviso.

>[!NOTE]
>
>Per gli ambienti di staging e produzione Pro, è necessario [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per aggiornare `mounts` e `disk` per la tua applicazione. Quando invii il ticket, indica le modifiche di configurazione richieste e includi una versione aggiornata del `.magento.app.yaml` file.

Per rendere accessibile il mount web, aggiungilo al [`web`](web-property.md) blocco di posizioni.

>[!WARNING]
>
>Una volta che il sito contiene dati, non modificare `subpath` parte del nome del mount. Questo valore è l’identificatore univoco per `files` area. Se si modifica questo nome, si perderanno tutti i dati del sito archiviati nella posizione precedente.

## `access`

Il `access` indica un livello di ruolo utente minimo che può accedere SSH agli ambienti. I ruoli utente disponibili sono:

- `admin`- Può modificare le impostazioni ed eseguire azioni nell&#39;ambiente; ha _collaboratore_ e _visualizzatore_ diritti.
- `contributor`: può inviare il codice a questo ambiente e ramificarlo dall’ambiente; ha _visualizzatore_ diritti.
- `viewer`- Può visualizzare solo l&#39;ambiente.

Il ruolo utente predefinito è `contributor`, che limita l’accesso SSH agli utenti con _visualizzatore_ diritti. Puoi cambiare il ruolo utente in `viewer` per consentire l’accesso SSH solo agli utenti con _visualizzatore_ diritti:

```yaml
access:
    ssh: viewer
```
