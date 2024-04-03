---
title: Configurazione di più siti Web o store
description: Scopri come configurare più siti web o store per Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Configuration, Routes, Site Navigation
exl-id: 16e932ef-f083-44d7-977f-0c78899e151a
source-git-commit: 85aa54af10e7ea44adde5403b69ff03d4a0c622f
workflow-type: tm+mt
source-wordcount: '1013'
ht-degree: 0%

---

# Configurazione di più siti Web o store

Puoi configurare Adobe Commerce in modo che abbia più siti web o store, ad esempio uno in inglese, uno in francese e uno in tedesco. Consulta [Informazioni su siti Web, store e visualizzazioni dello store](best-practices.md#store-views).

>[!WARNING]
>
>I dati del catalogo aumentano con l’aumentare del numero di siti web e store. A seconda dell’architettura del progetto, gli archivi aggiuntivi possono comportare un processo di indicizzazione più lungo e tempi di risposta più lenti per le pagine di catalogo non memorizzate nella cache. L’Adobe consiglia di monitorare attentamente le prestazioni del sito.

Il processo di impostazione di più archivi dipende dalla scelta di utilizzare domini univoci o condivisi.

Più archivi con domini univoci:

```terminal
https://first.store.com/
https://second.store.com/
```

Più archivi con lo stesso dominio:

```terminal
https://store.com/first/
https://store.com/second/
```

>[!TIP]
>
>Per aggiungere una visualizzazione archivio all&#39;URL di base del sito, non è necessario creare più directory. Consulta [Aggiungi il codice store all’URL di base](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) nel _Guida alla configurazione_.

## Aggiungi domini

I domini personalizzati possono essere aggiunti a Pro Staging e a qualsiasi ambiente di produzione; non possono essere aggiunti agli ambienti di integrazione.

Il processo di aggiunta di un dominio dipende dal tipo di account Cloud:

- Produzione e staging Pro

  Aggiungi il nuovo dominio a Fastly, vedi [Gestione domini](../cdn/fastly-custom-cache-configuration.md#manage-domains), oppure apri un ticket di supporto per richiedere assistenza. Inoltre, devi [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per richiedere l&#39;aggiunta di nuovi domini a un cluster.

- Solo per la produzione iniziale

  Aggiungi il nuovo dominio a Fastly, vedi [Gestione domini](../cdn/fastly-custom-cache-configuration.md#manage-domains), o [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per richiedere assistenza. Inoltre, devi aggiungere il nuovo dominio al **Domini** scheda in [!DNL Cloud Console]: `https://<zone>.magento.cloud/projects/<project-ID>/edit`

## Configurare l’installazione locale

Per configurare l&#39;installazione locale per l&#39;utilizzo di più archivi, vedere [Più siti Web o store][config-multiweb] nel _Guida alla configurazione_.

Dopo aver creato e testato correttamente l’installazione locale per utilizzare più archivi, è necessario preparare l’ambiente di integrazione:

1. **Configurare percorsi o posizioni**: specifica il modo in cui Adobe Commerce gestisce gli URL in ingresso

   - [Routing per domini separati](#configure-routes-for-separate-domains)
   - [Posizioni per i domini condivisi](#configure-locations-for-shared-domains)

1. **Configurare siti Web, store e visualizzazioni dello store**: configura utilizzando l’interfaccia utente di amministrazione di Adobe Commerce
1. **Modificare le variabili**- specifica i valori della `MAGE_RUN_TYPE` e `MAGE_RUN_CODE` variabili in `magento-vars.php` file
1. **Distribuire e testare gli ambienti**: distribuzione e test di `integration` filiale

>[!TIP]
>
>È possibile utilizzare un ambiente locale per configurare più siti Web o store. Consulta le istruzioni di Cloud Docker per [Configurazione di più siti Web o store](https://developer.adobe.com/commerce/cloud-tools/docker/configure/multiple-sites/).

### Aggiornamenti alla configurazione degli ambienti Pro

{{pro-self-service-warning}}

### Configurare route per domini separati

Le route definiscono come elaborare gli URL in ingresso. Più store con domini univoci richiedono di definire ogni dominio nel `routes.yaml` file. La modalità di configurazione delle route dipende dal funzionamento del sito.

**Per configurare le route in un ambiente di integrazione**:

1. Sulla workstation locale, aprire `.magento/routes.yaml` in un editor di testo.

1. Definisci il dominio e i sottodomini. Il `mymagento` il valore upstream è lo stesso della proprietà name nel `.magento.app.yaml` file.

   ```yaml
   "http://{default}/":
       type: upstream
       upstream: "mymagento:http"
   
   "http://<second-site>.{default}/":
       type: upstream
       upstream: "mymagento:http"
   ```

1. Salva le modifiche in `routes.yaml` file.

1. Continua con [Configurare siti Web, store e visualizzazioni dello store](#set-up-websites-stores-and-store-views).

### Configurare le posizioni per i domini condivisi

Se la configurazione delle route definisce il modo in cui vengono elaborati gli URL, il `web` proprietà in `.magento.app.yaml` definisce il modo in cui l’applicazione viene esposta al web. Web _posizioni_ consente maggiore granularità per le richieste in ingresso. Ad esempio, se il dominio è `store.com`, è possibile utilizzare `/first` (sito predefinito) e `/second` per le richieste a due store diversi che condividono un dominio.

**Per configurare una nuova posizione Web**:

1. Crea un alias per la radice (`/`). In questo esempio, l’alias è `&app` sulla linea 3.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
   ```

1. Creare un pass-through per il sito Web (`/website`) e fare riferimento alla directory principale utilizzando l&#39;alias del passaggio precedente.

   L’alias consente `website` per accedere ai valori dalla posizione radice. In questo esempio, il sito Web `passthru` è sulla linea 21.

   ```yaml
   web:
       locations:
           "/": &app
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static":
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>": *app
             ...
   ```

**Per configurare una posizione con una directory diversa**:

1. Crea un alias per la radice (`/`) e per l&#39;elemento statico (`/static`).

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/static": &static
               root: "pub/static"
   ```

1. Crea una sottodirectory per il sito web sotto `pub` directory: `pub/<website>`

1. Copia il `pub/index.php` file in `pub/<website>` e aggiorna la `bootstrap` percorso (`/../../app/bootstrap.php`).

   ```
   try {
       require __DIR__ . '/../../app/bootstrap.php';
   } catch (\Exception $e) { 
   ```

1. Creare un pass-through per `index.php` file.

   ```yaml
   web:
       locations:
           "/": &root
               # The public directory of the app, relative to its root.
               root: "pub"
               passthru: "/index.php"
               index:
               - index.php
               ...
           "/media":
               root: "pub/media"
               ...
           "/static": &static
               root: "pub/static"
               allow: true
               scripts: false
               passthru: "/front-static.php"
               rules:
                   ^/static/version\d+/(?<resource>.*)$:
                       passthru: "/static/$resource"
           "/<website>":
               <<: *root
               passthru: "<website>/index.php"
           "/<website>/static": *static
             ...
   ```

1. Eseguire il commit e il push dei file modificati.

   - `pub/<website>/index.php` (Se il file è in `.gitignore`, la pressione potrebbe richiedere l&#39;opzione di forza.)
   - `.magento.app.yaml`

### Configurare siti Web, store e visualizzazioni dello store

In _Interfaccia utente amministratore_, configura il tuo Adobe Commerce **Siti Web**, **Negozi**, e **Visualizzazioni store**. Consulta [Configurare più siti web, store e visualizzazioni dello store in Admin](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-admin.html) nel _Guida alla configurazione_.

È importante usare lo stesso nome e lo stesso codice dei siti web, degli store e delle visualizzazioni dello store dall’amministratore quando configuri l’installazione locale. Questi valori sono necessari quando si aggiorna il `magento-vars.php` file.

### Modificare le variabili

Invece di configurare un host virtuale NGINX, trasmettere `MAGE_RUN_CODE` e `MAGE_RUN_TYPE` variabili che utilizzano `magento-vars.php` nella directory principale del progetto.

**Per trasmettere le variabili utilizzando `magento-vars.php` file**:

1. Apri `magento-vars.php` in un editor di testo.

   Il [predefinito `magento-vars.php` file](https://github.com/magento/magento-cloud/blob/master/magento-vars.php) deve avere un aspetto simile al seguente:

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   //if (isHttpHost("example.com")) {
   //    $_SERVER["MAGE_RUN_CODE"] = "default";
   //    $_SERVER["MAGE_RUN_TYPE"] = "store";
   //}
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] === $host;
   }
   ```

1. Sposta il commento `if` blocco in modo che sia _dopo_ il `function` e non ha più commentato.

   ```php
   <?php
   // enable, adjust and copy this code for each store you run
   // Store #0, default one
   
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
           return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("example.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "default";
       $_SERVER["MAGE_RUN_TYPE"] = "store";
   }
   ```

1. Sostituisci i seguenti valori in `if (isHttpHost("example.com"))` blocco:
   - `example.com`: con l’URL di base del tuo _sito web_
   - `default`- con il CODICE univoco per il _sito web_ o _visualizzazione store_
   - `store`- con uno dei seguenti valori:
      - `website`- Carica il _sito web_ nella vetrina
      - `store`- Carica un _visualizzazione store_ nella vetrina

   Per più siti che utilizzano domini univoci:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("second.store.com")) {
       $_SERVER["MAGE_RUN_CODE"] = "<second-site>";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }elseif (isHttpHost("store.com")){
       $_SERVER["MAGE_RUN_CODE"] = "base";
       $_SERVER["MAGE_RUN_TYPE"] = "website";
   }
   ```

   Per più siti con lo stesso dominio, devi controllare il _host_ e _URI_:

   ```php
   <?php
   function isHttpHost($host)
   {
       if (!isset($_SERVER['HTTP_HOST'])) {
       return false;
       }
       return $_SERVER['HTTP_HOST'] ===  $host;
   }
   
   if (isHttpHost("store.com")) {
      $code = "base";
      $type = "website";
   
      $uri = explode('/', $_SERVER['REQUEST_URI']);
      if (isset($uri[1]) && $uri[1] == 'second') {
        $code = '<second-site>';
        $type = 'website';
      }
      $_SERVER["MAGE_RUN_CODE"] = $code;
      $_SERVER["MAGE_RUN_TYPE"] = $type;
   }
   ```

1. Salva le modifiche in `magento-vars.php` file.

### Distribuire e testare nel server di integrazione

Invia le modifiche all’ambiente di integrazione dell’infrastruttura cloud di Adobe Commerce on e verifica il sito.

1. Aggiungi, conferma e invia modifiche al codice al ramo remoto.

   ```bash
   git add -A && git commit -m "Implement multiple sites" && git push origin <branch-name>
   ```

1. Attendere il completamento della distribuzione.

1. Dopo la distribuzione, apri l’URL dello store in un browser web.

   Con un dominio univoco, utilizza il formato: `http://<magento-run-code>.<site-URL>`

   Ad esempio: `http://french.master-name-projectID.us.magentosite.cloud/`

   Con un dominio condiviso, utilizza il formato: `http://<site-URL>/<magento-run-code>`

   Ad esempio: `http://master-name-projectID.us.magentosite.cloud/french/`

1. Verifica a fondo il tuo sito e unisci il codice in `integration` per un&#39;ulteriore distribuzione.

## Distribuzione a staging e produzione

Segui il processo di distribuzione per [distribuzione in staging e produzione](../deploy/staging-production.md). Per gli ambienti Starter e Pro, si utilizza [!DNL Cloud Console] per inviare il codice tra ambienti diversi.

L’Adobe consiglia di eseguire test completi nell’ambiente di staging prima di eseguire il push all’ambiente di produzione. Apporta modifiche al codice nell’ambiente di integrazione e avvia di nuovo il processo di distribuzione tra gli ambienti.

<!-- link definitions -->

[config-multiweb]: https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/multi-sites/ms-overview.html
