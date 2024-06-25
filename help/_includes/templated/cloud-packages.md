---
source-git-commit: 866b958249bdeef19ffb0e284da9fac32f90baea
workflow-type: tm+mt
source-wordcount: '2191'
ht-degree: 0%

---
# Pacchetti cloud per Adobe Commerce

<!-- The 'packages' variable contains the 'packages' node of the '_data/codebase/cloud/composer_lock.json' file
 -->

<!-- The 'packages-dev' variable contains the 'packages-dev' node of the '_data/codebase/cloud/composer_lock.json' file
 -->

<!-- The 'product' variable contains data of the 'magento/magento-cloud-metapackage' package
 -->

<!-- The edition variable contains `cloud` value from the _data/names.yml file
 -->

Adobe Commerce sull’infrastruttura cloud utilizza Composer per gestire i pacchetti PHP.

Il `composer.json` file dichiara l&#39;elenco dei colli, mentre il `composer.lock` Il file memorizza un elenco completo dei pacchetti (una versione completa di ciascun pacchetto e delle relative dipendenze) utilizzati per creare un’installazione di Adobe Commerce.

La seguente documentazione di riferimento è generata da `composer.lock` e riguarda i pacchetti richiesti inclusi in Adobe Commerce on cloud infrastructure 2.4.7.

## Dipendenze

`magento/magento-cloud-metapackage 2.4.7` ha le seguenti dipendenze:

```config
fastly/magento2: ^1.2.34
magento/ece-tools: ^2002.1.0
magento/module-paypal-on-boarding: ~100.5.0
magento/product-enterprise-edition: >=2.4.7 <2.4.8
```

## Licenze di terze parti

### Apache-2.0, solo LGPL-2.1

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/elastic/elasticsearch-php.git">elasticsearch/elasticsearch</a>
    </td>
    <td>libreria</td>
    <td>Client PHP, ad Elasticsearch</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/opensearch-project/opensearch-php.git">opensearch-project/opensearch-php</a>
    </td>
    <td>libreria</td>
    <td>Client PHP per OpenSearch</td>
  </tr>
  </tbody>
</table>

### Apache-2.0

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/adobe/stock-api-libphp.git">astock/stock-api-libphp</a>
    </td>
    <td>libreria</td>
    <td>Libreria API di Adobe Stock</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/awslabs/aws-crt-php.git">aws/aws-crt-php</a>
    </td>
    <td>libreria</td>
    <td>Common Runtime di AWS per PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/aws/aws-sdk-php.git">aws/aws-sdk-php</a>
    </td>
    <td>libreria</td>
    <td>SDK di AWS per PHP - Utilizzare Amazon Web Services nel progetto PHP</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree
    </td>
    <td>metapacchetto</td>
    <td>Braintree Magento</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/wikimedia/less.php.git">wikimedia/less.php</a>
    </td>
    <td>libreria</td>
    <td>Porta PHP del processore LESS</td>
  </tr>
  </tbody>
</table>

### Clausola BSD-2

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/Bacon/BaconQrCode.git">bacon/bacon-qr-code</a>
    </td>
    <td>libreria</td>
    <td>BaconQrCode è un generatore di codici QR per PHP.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/DASPRiD/Enum.git">dasprid/enum</a>
    </td>
    <td>libreria</td>
    <td>Implementazione PHP 7.1 enum</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webimpress/safe-writer.git">webimpress/safe-writer</a>
    </td>
    <td>libreria</td>
    <td>Strumento per scrivere i file in modo sicuro, per evitare race condition</td>
  </tr>
  </tbody>
</table>

### Clausola BSD-3

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/Cm_Cache_Backend_File.git">colinmollenhour/cache-backend-file</a>
    </td>
    <td>modulo magento</td>
    <td>Il backend Zend_Cache_Backend_File di magazzino ha prestazioni estremamente scadenti per la pulizia da parte dei tag, rendendolo inutilizzabile con l’aumento del numero di elementi memorizzati in cache. Questo back-end apporta molte modifiche che determinano un notevole miglioramento delle prestazioni, in particolare per la pulizia dei tag.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/php-redis-session-abstract.git">colinmollenhour/php-redis-session-abstract</a>
    </td>
    <td>libreria</td>
    <td>Un gestore di sessione basato su Redis con blocco ottimistico</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/fastly/fastly-magento2.git">fastly/magento2</a>
    </td>
    <td>modulo magento2</td>
    <td>Modulo CDN Fastly per il Magento 2.4.x</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/firebase/php-jwt.git">firebase/php-jwt</a>
    </td>
    <td>libreria</td>
    <td>Una semplice libreria per codificare e decodificare i token web JSON (JWT) in PHP. Deve essere conforme alla specifica corrente.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/google/recaptcha.git">google/recaptcha</a>
    </td>
    <td>libreria</td>
    <td>Libreria client per reCAPTCHA, un servizio gratuito che protegge i siti web da spam e utilizzo non corretto.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-captcha.git">laminas/laminas-captcha</a>
    </td>
    <td>libreria</td>
    <td>Generare e convalidare i CAPTCHA utilizzando figure, immagini, ReCaptcha e altro ancora</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-code.git">Codice laminas/laminas</a>
    </td>
    <td>libreria</td>
    <td>Estensioni dell'API PHP Reflection, scansione del codice statico e generazione del codice</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-config.git">configurazione laminas/laminas</a>
    </td>
    <td>libreria</td>
    <td>fornisce un'interfaccia utente basata su proprietà di oggetto nidificato per accedere ai dati di configurazione nel codice dell'applicazione</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-crypt.git">laminas/laminas-crypt</a>
    </td>
    <td>libreria</td>
    <td>Potenti strumenti di crittografia e hashing delle password</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-db.git">laminas/laminas-db</a>
    </td>
    <td>libreria</td>
    <td>Livello di astrazione del database, astrazione SQL, astrazione del set di risultati e implementazioni RowDataGateway e TableDataGateway</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-di.git">laminas/laminas-di</a>
    </td>
    <td>libreria</td>
    <td>Iniezione di dipendenza automatica per contenitori PSR-11</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-escaper.git">laminas/laminas-escape</a>
    </td>
    <td>libreria</td>
    <td>Escape sicuro di HTML, attributi HTML, JavaScript, CSS e URL</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-eventmanager.git">laminas/laminas-eventmanager</a>
    </td>
    <td>libreria</td>
    <td>Attivare e ascoltare gli eventi all'interno di un'applicazione PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-feed.git">Alimentazione laminas/laminas</a>
    </td>
    <td>libreria</td>
    <td>fornisce funzionalità per creare e utilizzare feed RSS e Atom</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-file.git">File laminas/laminas</a>
    </td>
    <td>libreria</td>
    <td>Individuazione dei file di classe PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-filter.git">laminas/laminas-filter</a>
    </td>
    <td>libreria</td>
    <td>Filtrare e normalizzare a livello di programmazione dati e file</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-http.git">laminas/laminas-http</a>
    </td>
    <td>libreria</td>
    <td>Interfaccia semplice per l'esecuzione di richieste HTTP (Hyper-Text Transfer Protocol)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-i18n.git">laminas/laminas-i18n</a>
    </td>
    <td>libreria</td>
    <td>Fornisci le traduzioni per la tua applicazione, e filtra e convalida i valori internazionalizzati</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-json.git">laminas/laminas-json</a>
    </td>
    <td>libreria</td>
    <td>fornisce metodi di convenienza per serializzare da PHP nativo a JSON e decodificare da JSON a PHP nativo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-loader.git">laminas/laminas-loader</a>
    </td>
    <td>libreria</td>
    <td>Strategie di caricamento automatico e plug-in</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-mail.git">laminas/laminas-mail</a>
    </td>
    <td>libreria</td>
    <td>Fornisce funzionalità generalizzate per la composizione e l'invio di messaggi di posta elettronica in più parti sia di testo che compatibili con MIME</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-math.git">laminas/laminas-math</a>
    </td>
    <td>libreria</td>
    <td>Creazione di numeri pseudo-casuali protetti da crittografia e gestione di numeri interi di grandi dimensioni</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-mime.git">laminas/laminas-mime</a>
    </td>
    <td>libreria</td>
    <td>Creare e analizzare parti e messaggi MIME</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-modulemanager.git">laminas/laminas-modulemanager</a>
    </td>
    <td>libreria</td>
    <td>Sistema modulare per applicazioni laminas-mvc</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-mvc.git">laminas/laminas-mvc</a>
    </td>
    <td>libreria</td>
    <td>Livello MVC basato su eventi di Laminas, incluse applicazioni MVC, controller e plug-in</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-oauth.git">laminas/laminas-oauth</a>
    </td>
    <td>libreria</td>
    <td></td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-permissions-acl.git">laminas/laminas-permissions-acl</a>
    </td>
    <td>libreria</td>
    <td>Implementazione di un elenco di controllo di accesso (ACL) leggero e flessibile per la gestione dei privilegi</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-recaptcha.git">laminas/laminas-recaptcha</a>
    </td>
    <td>libreria</td>
    <td>Wrapper OOP per il servizio Web ReCaptcha</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-router.git">laminas/laminas-router</a>
    </td>
    <td>libreria</td>
    <td>Sistema di routing flessibile per applicazioni HTTP e console</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-server.git">server laminas/laminas</a>
    </td>
    <td>libreria</td>
    <td>Creazione di server RPC basati su riflessioni</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-servicemanager.git">laminas/laminas-servicemanager</a>
    </td>
    <td>libreria</td>
    <td>Contenitore Di Iniezione Di Dipendenza Basato Su Fabbrica</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-session.git">sessione laminas/laminas</a>
    </td>
    <td>libreria</td>
    <td>Interfaccia orientata agli oggetti per sessioni PHP e storage</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-soap.git">Laminas/laminas-soap</a>
    </td>
    <td>libreria</td>
    <td></td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-stdlib.git">laminas/laminas-stdlib</a>
    </td>
    <td>libreria</td>
    <td>Estensioni SPL, utility di array, gestori di errori e altro ancora</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-text.git">laminas/laminas-text</a>
    </td>
    <td>libreria</td>
    <td>Creare i file FIGlet e le tabelle basate su testo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-uri.git">laminas/laminas-uri</a>
    </td>
    <td>libreria</td>
    <td>Componente che facilita la manipolazione e la convalida degli URI (Uniform Resource Identifiers)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-validator.git">laminas/laminas-validator</a>
    </td>
    <td>libreria</td>
    <td>Classi di convalida per un'ampia gamma di domini e possibilità di concatenare le convalide per creare criteri di convalida complessi</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/laminas/laminas-view.git">Laminas/laminas</a>
    </td>
    <td>libreria</td>
    <td>Livello di vista flessibile che supporta e fornisce più livelli di vista, helper e altro ancora</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/nikic/PHP-Parser.git">nikic/php-parser</a>
    </td>
    <td>libreria</td>
    <td>Un parser PHP scritto in PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/tedious/JShrink.git">tedivm/jshrink</a>
    </td>
    <td>libreria</td>
    <td>PHP integrato nel minifier JavaScript</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/tubalmartin/YUI-CSS-compressor-PHP-port.git">tubalmartin/cssmin</a>
    </td>
    <td>libreria</td>
    <td>Porta PHP del compressore CSS YUI</td>
  </tr>
  </tbody>
</table>

### Modifica clausola BSD-3

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/Cm_Cache_Backend_Redis.git">colinmollenhour/cache-backend-redis</a>
    </td>
    <td>modulo magento</td>
    <td>Zend_Cache back-end che utilizza Redis con supporto completo per i tag.</td>
  </tr>
  </tbody>
</table>

### ISC

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/paragonie/sodium_compat.git">paragonie/sodio_compat</a>
    </td>
    <td>libreria</td>
    <td>Pura implementazione PHP di libsodium; utilizza l’estensione PHP, se presente</td>
  </tr>
  </tbody>
</table>

### LGPL-2.1-or-later

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/ezyang/htmlpurifier.git">ezyang/htmlPurificer</a>
    </td>
    <td>libreria</td>
    <td>Filtro HTML conforme agli standard scritto in PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-amqplib/php-amqplib.git">php-amqplib/php-amqplib</a>
    </td>
    <td>libreria</td>
    <td>Ex videlalvaro/php-amqplib.  Questa libreria è un’implementazione PHP pura del protocollo AMQP. È stato testato contro RabbitMQ.</td>
  </tr>
  </tbody>
</table>

### MIT

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/braintree/braintree_php.git">braintree/braintree_php</a>
    </td>
    <td>libreria</td>
    <td>Braintree libreria client PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/brick/math.git">mattone/matematica</a>
    </td>
    <td>libreria</td>
    <td>Libreria aritmetica di precisione arbitraria</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/brick/varexporter.git">brick/varexporter</a>
    </td>
    <td>libreria</td>
    <td>Una potente alternativa a var_export(), che può esportare chiusure e oggetti senza __set_state()</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/CarbonPHP/carbon-doctrine-types.git">carbonphp/carbonio-tipi-dottrina</a>
    </td>
    <td>libreria</td>
    <td>Tipi di utilizzo del carbonio nella dottrina</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ChristianRiesen/base32.git">christian-riesen/base32</a>
    </td>
    <td>libreria</td>
    <td>Codificatore/decodificatore Base32 secondo RFC 4648</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/colinmollenhour/credis.git">colinmollenhour/credis</a>
    </td>
    <td>libreria</td>
    <td>Credis è un’interfaccia leggera dell’archivio Redis di valori chiave che racchiude la libreria phpredis quando disponibile per prestazioni migliori.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/ca-bundle.git">compositore/ca-bundle</a>
    </td>
    <td>libreria</td>
    <td>Consente di trovare un percorso per il bundle CA di sistema e include un fallback al bundle CA di Mozilla.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/class-map-generator.git">compositore/class-map-generator</a>
    </td>
    <td>libreria</td>
    <td>Utilità per scansionare il codice PHP e generare mappe di classe.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/composer.git">compositore/compositore</a>
    </td>
    <td>libreria</td>
    <td>Composer consente di dichiarare, gestire e installare le dipendenze dei progetti PHP. Ti garantisce di avere la giusta pila ovunque.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/metadata-minifier.git">compositore/minificatore metadati</a>
    </td>
    <td>libreria</td>
    <td>Piccola libreria di utilità che gestisce la minimizzazione e l'espansione dei metadati.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/pcre.git">compositore/pcre</a>
    </td>
    <td>libreria</td>
    <td>Libreria di wrapping PCRE che offre sostituzioni preg_* di tipo sicuro.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/semver.git">compositore/semver</a>
    </td>
    <td>libreria</td>
    <td>Libreria semver che offre utility, analisi e convalida dei vincoli di versione.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/spdx-licenses.git">compositore/licenze spdx</a>
    </td>
    <td>libreria</td>
    <td>Elenco licenze SPDX e libreria di convalida.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/composer/xdebug-handler.git">compositore/xdebug-handler</a>
    </td>
    <td>libreria</td>
    <td>Riavvia un processo senza Xdebug.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/endroid/qr-code.git">endroid/qr-code</a>
    </td>
    <td>libreria</td>
    <td>Codice QR Endroid</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ezimuel/guzzlestreams.git">ezimuel/guzzlestream</a>
    </td>
    <td>libreria</td>
    <td>Forcella di guzzle/ruscelli (abbandonati) da utilizzare con elasticsearch-php</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ezimuel/ringphp.git">ezimuel/ringphp</a>
    </td>
    <td>libreria</td>
    <td>Forchetta di guzzle/RingPHP (abbandonato) da utilizzare con elasticsearch-php</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/FriendsOfPHP/proxy-manager-lts.git">friendlysofphp/proxy-manager-lts</a>
    </td>
    <td>libreria</td>
    <td>Supporto di una gamma più ampia di versioni PHP per ocramius/proxy-manager</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/bzikarsky/gelf-php.git">graylog2/gelf-php</a>
    </td>
    <td>libreria</td>
    <td>Un’implementazione php per inviare messaggi log a un backend compatibile GELF come Graylog2.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/guzzle.git">guzzlehttp/guzzle</a>
    </td>
    <td>libreria</td>
    <td>Guzzle è una libreria client HTTP PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/promises.git">guzzlehttp/promise</a>
    </td>
    <td>libreria</td>
    <td>Libreria Guzzle promesse</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/guzzle/psr7.git">guzzlehttp/psr7</a>
    </td>
    <td>libreria</td>
    <td>Implementazione del messaggio PSR-7 che fornisce anche metodi di utilità comuni</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/collections.git">illuminare/collezioni</a>
    </td>
    <td>libreria</td>
    <td>Il pacchetto Illuminare le raccolte.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/config.git">illuminare/configurare</a>
    </td>
    <td>libreria</td>
    <td>Il pacchetto Illuminate Config.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/contracts.git">illuminare/contratti</a>
    </td>
    <td>libreria</td>
    <td>Il pacchetto Illuminate Contracts.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/illuminate/macroable.git">illuminato/macroscopico</a>
    </td>
    <td>libreria</td>
    <td>Il pacchetto Illuminare Macroable.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/jsonrainbow/json-schema.git">justinrainbow/json-schema</a>
    </td>
    <td>libreria</td>
    <td>Libreria per convalidare uno schema JSON.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem.git">league/flysystem</a>
    </td>
    <td>libreria</td>
    <td>Estrazione archiviazione file per PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/flysystem-aws-s3-v3.git">league/flysystem-aws-s3-v3</a>
    </td>
    <td>libreria</td>
    <td>Scheda di file system AWS S3 per Flysystem.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/thephpleague/mime-type-detection.git">league/mime-type-detection</a>
    </td>
    <td>libreria</td>
    <td>Rilevamento del tipo MIME per Flysystem</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/monolog.git">monologo/monologo</a>
    </td>
    <td>libreria</td>
    <td>Invia i registri a file, socket, caselle in entrata, database e vari servizi web</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/jmespath/jmespath.php.git">mtdowling/jmespath.php</a>
    </td>
    <td>libreria</td>
    <td>Specificare in modo dichiarativo come estrarre elementi da un documento JSON</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/briannesbitt/Carbon.git">nesbot/carbonio</a>
    </td>
    <td>libreria</td>
    <td>Estensione API per DateTime che supporta 281 lingue diverse.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/paragonie/constant_time_encoding.git">paragonie/constant_time_encoding</a>
    </td>
    <td>libreria</td>
    <td>Implementazioni a tempo costante della codifica RFC 4648 (Base-64, Base-32, Base-16)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/paragonie/random_compat.git">paragonie/random_compat</a>
    </td>
    <td>libreria</td>
    <td>polyfill PHP 5.x per random_bytes() e random_int() da PHP 7</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/MyIntervals/emogrifier.git">pelago/emoglificatore</a>
    </td>
    <td>libreria</td>
    <td>Converte gli stili CSS in attributi di stile in linea nel codice HTML</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/PhpGt/CssXPath.git">phpgt/cssxpath</a>
    </td>
    <td>libreria</td>
    <td>Converti selettori CSS in query XPath.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/PhpGt/Dom.git">phpgt/dom</a>
    </td>
    <td>libreria</td>
    <td>API DOM moderna.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/PhpGt/PropFunc.git">phpgt/propfunc</a>
    </td>
    <td>libreria</td>
    <td>Funzioni della funzione di accesso alle proprietà e del mutatore.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpseclib/mcrypt_compat.git">phpseclib/mcrypt_compat</a>
    </td>
    <td>libreria</td>
    <td>Poliriempimento PHP 5.x-8.x per estensione mcrypt</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/phpseclib/phpseclib.git">phpseclib/phpseclib</a>
    </td>
    <td>libreria</td>
    <td>Libreria PHP Secure Communications: implementazioni Pure-PHP di RSA, AES, SSH2, SFTP, X.509 ecc.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/cache.git">psr/cache</a>
    </td>
    <td>libreria</td>
    <td>Interfaccia comune per il caching delle librerie</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/clock.git">psr/clock</a>
    </td>
    <td>libreria</td>
    <td>Interfaccia comune per la lettura dell'orologio.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/container.git">psr/container</a>
    </td>
    <td>libreria</td>
    <td>Interfaccia comune dei contenitori (PHP FIG PSR-11)</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/event-dispatcher.git">psr/event-dispatcher</a>
    </td>
    <td>libreria</td>
    <td>Interfacce standard per la gestione degli eventi.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-client.git">psr/http-client</a>
    </td>
    <td>libreria</td>
    <td>Interfaccia comune per i client HTTP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-factory.git">psr/http-factory</a>
    </td>
    <td>libreria</td>
    <td>PSR-17: interfacce comuni per i message factory HTTP PSR-7</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/http-message.git">psr/http-message</a>
    </td>
    <td>libreria</td>
    <td>Interfaccia comune per i messaggi HTTP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/log.git">psr/log</a>
    </td>
    <td>libreria</td>
    <td>Interfaccia comune per le librerie di registrazione</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/php-fig/simple-cache.git">psr/simple-cache</a>
    </td>
    <td>libreria</td>
    <td>Interfacce comuni per il caching semplice</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ralouphie/getallheaders.git">ralouphie/getallheaders</a>
    </td>
    <td>libreria</td>
    <td>Un polyfill per getallheaders.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ramsey/collection.git">ramsey/collection</a>
    </td>
    <td>libreria</td>
    <td>Libreria PHP per la rappresentazione e la manipolazione delle raccolte.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/ramsey/uuid.git">ramsey/uuid</a>
    </td>
    <td>libreria</td>
    <td>Libreria PHP per la generazione e l’utilizzo di identificatori universalmente univoci (UUID).</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/reactphp/promise.git">react/promise</a>
    </td>
    <td>libreria</td>
    <td>Implementazione leggera di CommonJS Promises/A per PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/MyIntervals/PHP-CSS-Parser.git">sabberworm/php-css-parser</a>
    </td>
    <td>libreria</td>
    <td>Parser per file CSS scritti in PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/jsonlint.git">seld/jsonlint</a>
    </td>
    <td>libreria</td>
    <td>Linter JSON</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/phar-utils.git">seld/phar-utils</a>
    </td>
    <td>libreria</td>
    <td>PHAR formato file utilità, per quando PHP phars si up</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Seldaek/signal-handler.git">seld/signal-handler</a>
    </td>
    <td>libreria</td>
    <td>Semplice gestore di segnali unix che genera un errore silenzioso quando i segnali non sono supportati per un facile sviluppo su più piattaforme</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/aes-key-wrap.git">spomky-labs/aes-key-wrap</a>
    </td>
    <td>libreria</td>
    <td>Key Wrap AES per PHP.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/otphp.git">spomky-labs/otphp</a>
    </td>
    <td>libreria</td>
    <td>Libreria PHP per la generazione di password monouso secondo l'RFC 4226 (HOTP Algorithm) e l'RFC 6238 (TOTP Algorithm) e compatibile con Google Authenticator</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/Spomky-Labs/pki-framework.git">spomky-labs/pki-framework</a>
    </td>
    <td>libreria</td>
    <td>Un framework PHP per la gestione dell'infrastruttura a chiave pubblica. Comprende certificati a chiave pubblica X.509, certificati di attributi, richieste di certificazione e convalida del percorso di certificazione.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/config.git">symfony/config</a>
    </td>
    <td>libreria</td>
    <td>Consente di trovare, caricare, combinare, riempire automaticamente e convalidare i valori di configurazione di qualsiasi tipo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/console.git">symfony/console</a>
    </td>
    <td>libreria</td>
    <td>Semplifica la creazione di interfacce per riga di comando belle e testabili</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/css-selector.git">symfony/css-selector</a>
    </td>
    <td>libreria</td>
    <td>Converte i selettori CSS in espressioni XPath</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/dependency-injection.git">symfony/dependency-injection</a>
    </td>
    <td>libreria</td>
    <td>Consente di standardizzare e centralizzare il modo in cui gli oggetti vengono costruiti nell'applicazione</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/deprecation-contracts.git">symfony/deprecation-CONTRACTS</a>
    </td>
    <td>libreria</td>
    <td>Funzione e convenzione generiche per attivare gli avvisi di deprecazione</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/error-handler.git">symfony/error-handler</a>
    </td>
    <td>libreria</td>
    <td>Fornisce strumenti per gestire gli errori e facilitare il debug del codice PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/event-dispatcher.git">symfony/event-dispatcher</a>
    </td>
    <td>libreria</td>
    <td>Fornisce strumenti che consentono ai componenti dell'applicazione di comunicare tra loro inviando eventi e ascoltandoli</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/event-dispatcher-contracts.git">symfony/event-dispatcher-CONTRACTS</a>
    </td>
    <td>libreria</td>
    <td>Astrazioni generiche correlate all’evento dispatching</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/filesystem.git">symfony/file system</a>
    </td>
    <td>libreria</td>
    <td>Fornisce utilità di base per il file system</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/finder.git">symfony/finder</a>
    </td>
    <td>libreria</td>
    <td>Trova file e directory tramite un'interfaccia intuitiva</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-client.git">symfony/http-client</a>
    </td>
    <td>libreria</td>
    <td>Fornisce potenti metodi per recuperare le risorse HTTP in modo sincrono o asincrono</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-client-contracts.git">symfony/http-client-contract</a>
    </td>
    <td>libreria</td>
    <td>Astrazioni generiche correlate ai client HTTP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-foundation.git">symfony/http-foundation</a>
    </td>
    <td>libreria</td>
    <td>Definisce un livello orientato agli oggetti per la specifica HTTP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/http-kernel.git">symfony/http-kernel</a>
    </td>
    <td>libreria</td>
    <td>Fornisce un processo strutturato per convertire una richiesta in una risposta</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/intl.git">symfony/intl</a>
    </td>
    <td>libreria</td>
    <td>Consente di accedere ai dati di localizzazione della libreria ICU</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-ctype.git">symfony/polyfill-ctype</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill per funzioni di tipo</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-grapheme.git">symfony/polyfill-intl-grapheme</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill per le funzioni grapheme_* di intl</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-idn.git">symfony/polyfill-intl-idn</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill per le funzioni intl idn_to_ascii e idn_to_utf8</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-intl-normalizer.git">symfony/polyfill-intl-normalizer</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill per la classe Normalizer di intl e funzioni correlate</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-mbstring.git">symfony/polyfill-mbstring</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill per l’estensione Mbstring</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php72.git">symfony/polyfill-php72</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill supporta alcune funzioni PHP 7.2+ per ridurre le versioni PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php73.git">symfony/polyfill-php73</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill supporta alcune funzioni PHP 7.3+ per ridurre le versioni PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php80.git">symfony/polyfill-php80</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill supporta alcune funzioni PHP 8.0+ per ridurre le versioni PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php81.git">symfony/polyfill-php81</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill supporta alcune funzioni PHP 8.1+ per ridurre le versioni PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/polyfill-php83.git">symfony/polyfill-php83</a>
    </td>
    <td>libreria</td>
    <td>Symfony polyfill supporta alcune funzioni PHP 8.3+ per ridurre le versioni PHP</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/process.git">symfony/process</a>
    </td>
    <td>libreria</td>
    <td>Esegue comandi nei sottoprocessi</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/proxy-manager-bridge.git">symfony/proxy-manager-bridge</a>
    </td>
    <td>symfony-bridge</td>
    <td>Integrazione di ProxyManager con vari componenti Symfony</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/serializer.git">symfony/serializzatore</a>
    </td>
    <td>libreria</td>
    <td>Gestisce la serializzazione e la deserializzazione delle strutture di dati, inclusi i grafici a oggetti, in strutture di array o in altri formati come XML e JSON.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/service-contracts.git">symfony/service-contract</a>
    </td>
    <td>libreria</td>
    <td>Astrazioni generiche relative ai servizi di scrittura</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/string.git">symfony/string</a>
    </td>
    <td>libreria</td>
    <td>Fornisce un’API orientata agli oggetti per stringhe e tratta byte, punti di codice UTF-8 e cluster di grafemi in modo unificato</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/translation.git">symfony/translation</a>
    </td>
    <td>libreria</td>
    <td>Fornisce gli strumenti per internazionalizzare l'applicazione</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/translation-contracts.git">symfony/translation-contract</a>
    </td>
    <td>libreria</td>
    <td>Astrazioni generiche relative alla traduzione</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/var-dumper.git">symfony/var-dumper</a>
    </td>
    <td>libreria</td>
    <td>Fornisce meccanismi per passare attraverso qualsiasi variabile PHP arbitraria</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/var-exporter.git">symfony/var-exporter</a>
    </td>
    <td>libreria</td>
    <td>Consente l'esportazione di qualsiasi struttura di dati PHP serializzabile nel codice PHP normale</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/symfony/yaml.git">symfony/yaml</a>
    </td>
    <td>libreria</td>
    <td>Carica ed esegue il dump dei file YAML</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/web-token/jwt-framework.git">web-token/jwt-framework</a>
    </td>
    <td>symfony-bundle</td>
    <td>Libreria di firma di oggetti e crittografia JSON per PHP e Symfony Bundle.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webmozarts/assert.git">webmozart/assert</a>
    </td>
    <td>libreria</td>
    <td>Asserzioni per convalidare l’input/output del metodo con messaggi di errore validi.</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/webonyx/graphql-php.git">webonyx/graphql-php</a>
    </td>
    <td>libreria</td>
    <td>Una porta PHP dell'implementazione di riferimento GraphQL</td>
  </tr>
  <tr>
    <td>
      <a href="https://github.com/zordius/lightncandy.git">zordius/lightncandy</a>
    </td>
    <td>libreria</td>
    <td>Un'implementazione PHP estremamente veloce di manubri ( http://handlebarsjs.com/ ) e baffi ( http://mustache.github.io/ ).</td>
  </tr>
  </tbody>
</table>

### OSL-3.0, AFL-3.0

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      paypal/module-braintree-customer-balance
    </td>
    <td>modulo magento2</td>
    <td>N/D</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-gift-card-account
    </td>
    <td>modulo magento2</td>
    <td>N/D</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-gift-wrapping
    </td>
    <td>modulo magento2</td>
    <td>N/D</td>
  </tr>
  <tr>
    <td>
      paypal/module-braintree-graph-ql
    </td>
    <td>modulo magento2</td>
    <td>N/D</td>
  </tr>
  </tbody>
</table>

### OSL-3.0

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

### PHP

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      <a href="https://github.com/2tvenom/CBOREncode.git">2tvenom/cborencode</a>
    </td>
    <td>libreria</td>
    <td>Codificatore CBOR per PHP</td>
  </tr>
  </tbody>
</table>

### Proprietario

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  </tbody>
</table>

### proprietario

<table>
  <thead>
    <tr>
      <th>Nome</th>
      <th>Tipo</th>
      <th>Descrizione</th>
    </tr>
  </thead>
  <tbody>
  <tr>
    <td>
      paypal/module-braintree-core
    </td>
    <td>modulo magento2</td>
    <td>Fork dal modulo Magento Braintree 2.2.0 di Gene Commerce per PayPal.</td>
  </tr>
  </tbody>
</table>
