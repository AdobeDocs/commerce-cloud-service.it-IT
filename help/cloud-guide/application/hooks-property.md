---
title: Proprietà Hooks
description: Vedi esempi su come configurare la proprietà hook in [!DNL Commerce] file di configurazione dell'applicazione.
feature: Cloud, Configuration, Build, Deploy
exl-id: d9561f09-5129-4b72-978e-2e3873e8efae
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Proprietà Hooks

Utilizza il `hooks` sezione per eseguire i comandi della shell durante le fasi di compilazione, distribuzione e post-distribuzione:

- **`build`**- Esecuzione di comandi _prima di_ creazione del pacchetto dell’applicazione. Servizi, come il database o Redis, non sono disponibili perché l&#39;applicazione non è ancora stata distribuita. Aggiungere comandi personalizzati _prima di_ impostazione predefinita `php ./vendor/bin/ece-tools` affinché il contenuto generato personalizzato continui alla fase di distribuzione.

- **`deploy`**- Esecuzione di comandi _dopo_ creazione di pacchetti e distribuzione dell&#39;applicazione. A questo punto è possibile accedere ad altri servizi. Dal valore predefinito `php ./vendor/bin/ece-tools` copia il comando `app/etc` nella posizione corretta, è necessario aggiungere comandi personalizzati _dopo_ il comando deploy per evitare errori nei comandi personalizzati.

- **`post_deploy`**- Esecuzione di comandi _dopo_ distribuzione dell&#39;applicazione e _dopo_ il contenitore inizia ad accettare le connessioni. Il `post_deploy` hook cancella la cache e precarica (riscalda) la cache. È possibile personalizzare l’elenco delle pagine utilizzando `WARM_UP_PAGES` variabile in [Fase post-distribuzione](../environment/variables-post-deploy.md). Anche se non obbligatorio, questo funziona insieme al `SCD_ON_DEMAND` variabile di ambiente.

L&#39;esempio seguente mostra la configurazione predefinita in `.magento.app.yaml` file. Aggiungere comandi CLI in `build`, `deploy`, o `post_deploy` sezioni _prima di_ il `ece-tools` comando:

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        composer install
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    # We run deploy hook after your application has been deployed and started.
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    # We run post deploy hook to clean and warm the cache. Available with ECE-Tools 2002.0.10.
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

Inoltre, puoi personalizzare ulteriormente la fase di build utilizzando `generate` e `transfer` comandi per eseguire azioni aggiuntive durante la creazione specifica di codice o lo spostamento di file.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e`- causa il fallimento degli hook sul primo comando non riuscito, anziché sul comando non riuscito finale.
- `build:generate`: applica patch, convalida la configurazione, genera ID e genera contenuto statico se SCD è abilitato per la fase di generazione.
- `build:transfer`- trasferisce il codice generato e il contenuto statico alla destinazione finale.

Comandi eseguiti dall&#39;applicazione (`/app`). È possibile utilizzare `cd` per cambiare la directory. Gli hook hanno esito negativo se il comando finale in essi contenuto ha esito negativo. Per impedire il completamento del primo comando non riuscito, aggiungere `set -e` all&#39;inizio dell&#39;amo.

**Per compilare i file Sass mediante grunt**:

```yaml
dependencies:
    ruby:
        sass: "3.4.7"
    nodejs:
        grunt-cli: "~0.1.13"

hooks:
    build: |
        cd public/profiles/project_name/themes/custom/theme_name
        npm install
        grunt
        cd
        php ./vendor/bin/ece-tools build
```

Compila file Sass tramite `grunt` prima della distribuzione del contenuto statico, che avviene durante la generazione. Posiziona `grunt` comando prima di `build` comando.

{{scenarios}}
