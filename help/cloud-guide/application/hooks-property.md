---
title: Proprietà Hooks
description: Vedi esempi su come configurare la proprietà hook nel file di configurazione dell'applicazione  [!DNL Commerce] .
feature: Cloud, Configuration, Build, Deploy
exl-id: d9561f09-5129-4b72-978e-2e3873e8efae
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# Proprietà Hooks

Utilizzare la sezione `hooks` per eseguire i comandi della shell durante le fasi di compilazione, distribuzione e post-distribuzione:

- **`build`**—Esegui i comandi _prima_ di creare il pacchetto dell&#39;applicazione. Servizi, come il database o Redis, non sono disponibili perché l&#39;applicazione non è ancora stata distribuita. Aggiungere i comandi personalizzati _prima_ del comando predefinito `php ./vendor/bin/ece-tools` in modo che il contenuto generato personalizzato continui alla fase di distribuzione.

- **`deploy`**—Esegui i comandi _dopo_ per creare il pacchetto e distribuire l&#39;applicazione. A questo punto è possibile accedere ad altri servizi. Poiché il comando predefinito `php ./vendor/bin/ece-tools` copia la directory `app/etc` nella posizione corretta, è necessario aggiungere comandi personalizzati _dopo_ al comando deploy per evitare errori nei comandi personalizzati.

- **`post_deploy`**—Esegui comandi _dopo_ la distribuzione dell&#39;applicazione e _dopo_ il contenitore inizia ad accettare connessioni. L&#39;hook `post_deploy` cancella la cache e precarica (riscalda) la cache. È possibile personalizzare l&#39;elenco delle pagine utilizzando la variabile `WARM_UP_PAGES` nella [fase di distribuzione di Post](../environment/variables-post-deploy.md). Anche se non obbligatorio, questo funziona insieme alla variabile di ambiente `SCD_ON_DEMAND`.

L&#39;esempio seguente mostra la configurazione predefinita nel file `.magento.app.yaml`. Aggiungere comandi CLI nelle sezioni `build`, `deploy` o `post_deploy` _prima_ del comando `ece-tools`:

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

È inoltre possibile personalizzare ulteriormente la fase di compilazione utilizzando i comandi `generate` e `transfer` per eseguire azioni aggiuntive durante la creazione specifica del codice o lo spostamento dei file.

```yaml
hooks:
    # We run build hooks before your application has been packaged.
    build: |
        set -e
        php ./vendor/bin/ece-tools build:generate
        # php /path/to/your/script
        php ./vendor/bin/ece-tools build:transfer
```

- `set -e` - causa un errore degli hook sul primo comando non riuscito, anziché sul comando finale non riuscito.
- `build:generate`: applica patch, convalida la configurazione, genera ID e genera contenuto statico se SCD è abilitato per la fase di compilazione.
- `build:transfer` - trasferisce il codice generato e il contenuto statico alla destinazione finale.

Comandi eseguiti dalla directory dell&#39;applicazione (`/app`). È possibile utilizzare il comando `cd` per cambiare la directory. Gli hook hanno esito negativo se il comando finale in essi contenuto ha esito negativo. Per impedirne l&#39;esecuzione al primo comando non riuscito, aggiungere `set -e` all&#39;inizio dell&#39;hook.

**Per compilare i file Sass utilizzando grunt**:

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

Compila i file Sass utilizzando `grunt` prima della distribuzione del contenuto statico, che avviene durante la compilazione. Inserire il comando `grunt` prima del comando `build`.

{{scenarios}}
