---
title: Configura [!DNL Xdebug]
description: Scopri come configurare l’estensione Xdebug per il debug dello sviluppo di progetti Adobe Commerce su infrastrutture cloud.
exl-id: bf2d32d8-fab7-439e-8df3-b039e53009d4
source-git-commit: 7b42174663b79b673ee5af05b794090ddc5bdd75
workflow-type: tm+mt
source-wordcount: '1765'
ht-degree: 0%

---

# Configura Xdebug

[!DNL Xdebug] è un&#39;estensione per il debug del tuo PHP. Sebbene sia possibile utilizzare un IDE scelto, di seguito viene illustrato come configurare [!DNL Xdebug] e [!DNL PhpStorm] per il debug nell&#39;ambiente locale.

>[!NOTE]
>
>È possibile configurare [!DNL Xdebug] per l&#39;esecuzione nell&#39;ambiente Cloud Docker per il debug locale senza modificare la configurazione del progetto Adobe Commerce su infrastruttura cloud. Vedere [Configurare Xdebug per Docker](https://developer.adobe.com/commerce/cloud-tools/docker/test/configure-xdebug/).

Per abilitare [!DNL Xdebug], devi configurare un file nell&#39;archivio Git, configurare l&#39;IDE e configurare l&#39;inoltro delle porte. È possibile configurare alcune impostazioni nel file `magento.app.yaml`. Dopo la modifica, invia le modifiche Git in tutti gli ambienti Starter e gli ambienti di integrazione Pro per abilitare [!DNL Xdebug]. [!DNL Xdebug] è già disponibile negli ambienti Pro Staging &amp; Production.

Una volta configurata, è possibile eseguire il debug di comandi CLI, richieste Web e codice. Ricorda che tutti gli ambienti dell’infrastruttura cloud sono di sola lettura. Clona il codice nell’ambiente di sviluppo locale per eseguire il debug. Per gli ambienti di staging e produzione Pro, consulta [ulteriori istruzioni](#debug-for-pro-staging-and-production) per [!DNL Xdebug].

## Requisiti

Per eseguire e utilizzare [!DNL Xdebug], è necessario l&#39;URL SSH per l&#39;ambiente. È possibile individuare le informazioni tramite [[!DNL Cloud Console]](../project/overview.md) o [!DNL Cloud Onboarding UI].

## Configura Xdebug

Per configurare [!DNL Xdebug], eseguire la procedura seguente:

- [Lavorare in un ramo per inviare aggiornamenti dei file](#get-started-with-a-branch)
- [Attiva [!DNL Xdebug] per ambienti](#enable-xdebug-in-your-environment)
- [Configurare l’IDE](#configure-phpstorm)
- [Configurare l’inoltro delle porte](#set-up-port-forwarding)

### Introduzione a un ramo

Per aggiungere [!DNL Xdebug], l&#39;Adobe consiglia di lavorare in [un ramo di sviluppo](../dev-tools/cloud-cli-overview.md#create-an-environment-branch).

### Abilitare Xdebug nell’ambiente

È possibile abilitare [!DNL Xdebug] direttamente in tutti gli ambienti Starter e gli ambienti di integrazione Pro. Questo passaggio di configurazione non è necessario per gli ambienti di produzione e staging Pro. Consulta [Debug per Pro Staging e Produzione](#debug-for-pro-staging-and-production).

Per abilitare [!DNL Xdebug] per il progetto, aggiungere `xdebug` alla sezione `runtime:extensions` del file `.magento.app.yaml`.

**Per abilitare Xdebug**:

1. Nel terminale locale, apri il file `.magento.app.yaml` in un editor di testo.

1. Nella sezione `runtime`, in `extensions`, aggiungi `xdebug`. Ad esempio:

   ```yaml
   runtime:
       extensions:
           - redis
           - xsl
           - newrelic
           - sodium
           - xdebug
   ```

1. Salvare le modifiche apportate al file `.magento.app.yaml` e uscire dall&#39;editor di testo.

1. Aggiungi, esegui il commit e invia le modifiche per ridistribuire l’ambiente.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Add xdebug"
   ```

   ```bash
   git push origin <environment-ID>
   ```

Quando viene implementato in ambienti Starter e Pro Integration, [!DNL Xdebug] è ora disponibile. Continua a configurare l’IDE. Per PhpStorm, vedere [Configurare PhpStorm](#configure-phpstorm).

### Configurare PhpStorm

L&#39;IDE [PhpStorm](https://www.jetbrains.com/phpstorm/) deve essere configurato per funzionare correttamente con [!DNL Xdebug].

**Per configurare PhpStorm per l&#39;utilizzo con Xdebug**:

1. Nel progetto PhpStorm, apri il pannello **Impostazioni**.

   - _macOS_—Seleziona **PhpStorm** > **Preferenze**.
   - _Windows/Linux_—Selezionare **File** > **Impostazioni**.

1. Nel pannello _Impostazioni_, espandi e individua la sezione **Lingue e framework** > **PHP** > **Server**.

1. Fare clic su **+** per aggiungere una configurazione del server. Il nome del progetto è in grigio nella parte superiore.

1. [Facoltativo] Configura le impostazioni seguenti per la nuova configurazione del server. Consulta [Nessun server di debug configurato](https://www.jetbrains.com/help/phpstorm/troubleshooting-php-debugging.html#no-debug-server-is-configured) nella documentazione di _PHPStorm_.

   - **Nome** - Immettere lo stesso nome host. Questo valore deve corrispondere al valore per la variabile `PHP_IDE_CONFIG` in [Debug dei comandi CLI](#debug-cli-commands) per utilizzare CLI per il debug.
   - **Host** - Immettere il nome host.
   - **Porta** - Immettere `443`.
   - **Debugger**—Selezionare `Xdebug`.

1. Seleziona **Usa mappature percorso**. Nel riquadro _File/Directory_ viene visualizzata la directory principale del progetto per `serverName`.

1. Nella colonna **Percorso assoluto sul server** fare clic sull&#39;icona **Modifica** e aggiungere un&#39;impostazione basata sull&#39;ambiente.

   - Per tutti gli ambienti Starter e di integrazione Pro, il percorso remoto è `/app`.
   - Per ambienti di staging e produzione Pro:

      - Produzione: `/app/<project_code>/`
      - Gestione temporanea: `/app/<project_code>_stg/`

1. Modificare la porta [!DNL Xdebug] in 9000 nel pannello **Language &amp; Frameworks** > **PHP** > **Debug** > **Xdebug** > **Debug Port**.

1. Fare clic su **Applica**.

### Configurare l’inoltro delle porte

Mappa la connessione `XDEBUG` dal server al sistema locale. Per eseguire qualsiasi tipo di debug, è necessario inoltrare la porta 9000 dal server Adobe Commerce su infrastruttura cloud al computer locale. Vedere una delle sezioni seguenti:

- [Inoltro porte in Mac o UNIX](#port-forwarding-on-mac-or-unix)
- [Inoltro porte in Windows](#port-forwarding-on-windows)

#### Inoltro porte in Mac o UNIX®

**Per impostare l&#39;inoltro delle porte in un ambiente Mac o UNIX®**:

1. Apri un terminale.

1. Utilizza SSH per stabilire la connessione.

   ```bash
   ssh -R 9000:localhost:9000 <ssh url>
   ```

   Utilizzare l&#39;opzione `-v` (dettagliata) in modo che ogni volta che un socket è connesso alla porta che viene inoltrata venga visualizzato nel terminale.

   Se viene visualizzato l&#39;errore &quot;Impossibile connettersi&quot; o &quot;Impossibile ascoltare la porta in remoto&quot;, potrebbe essere presente un&#39;altra sessione SSH attiva persistente sul server che occupa la porta 9000. Se la connessione non viene utilizzata, è possibile terminarla.

**Per risolvere i problemi di connessione**:

1. Utilizza SSH per accedere all’integrazione remota, all’ambiente di staging o di produzione.

1. Visualizza elenco sessioni SSH: `who`

1. Visualizzare le sessioni SSH esistenti per utente. Fai attenzione a non avere effetti su utenti diversi da te!

   - integrazione: i nomi utente sono simili a `dd2q5ct7mhgus`
   - Staging: i nomi utente sono simili a `dd2q5ct7mhgus_stg`
   - Produzione: i nomi utente sono simili a `dd2q5ct7mhgus`

1. Per una sessione utente precedente alla tua, trova il valore dello pseudo-terminale (PTS), ad esempio `pts/0`.

1. Termina il PID (Process ID) corrispondente al valore PTS.

   ```bash
   ps aux | grep ssh
   kill <PID>
   ```

   Risposta di esempio:

   ```
   dd2q5ct7mhgus        5504  0.0  0.0  82612  3664 ?      S    18:45   0:00 sshd: dd2q5ct7mhgus@pts/0
   ```

   Per terminare la connessione, immettere un comando kill con l&#39;ID processo (PID).

   ```bash
   kill 3664
   ```

#### Inoltro porte in Windows

Per impostare l&#39;inoltro delle porte (tunneling SSH) su Windows, è necessario configurare l&#39;applicazione terminale Windows. In questo esempio viene eseguita la creazione di un tunnel SSH utilizzando [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). È possibile utilizzare altre applicazioni, ad esempio Cygwin. Per ulteriori informazioni su altre applicazioni, consulta la documentazione del fornitore fornita con tali applicazioni.

**Per configurare un tunnel SSH in Windows utilizzando Putty**:

1. Se non lo hai già fatto, scarica [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

1. Avvia Putty.

1. Nel riquadro Categoria fare clic su **Sessione**.

1. Immettere le seguenti informazioni:

   - Campo **Nome host (o indirizzo IP)**: immetti l&#39;[URL SSH](../development/secure-connections.md#connect-to-a-remote-environment) per il server cloud
   - Campo **Porta**: immetti `22`

   ![Configurazione di Putty](../../assets/xdebug/putty-session.png)

1. Nel riquadro _Categoria_ fare clic su **Connessione** > **SSH** > **Tunnel**.

1. Immettere le seguenti informazioni:

   - Campo **Porta Source**: immetti `9000`
   - Campo **Destinazione**: immetti `127.0.0.1:9000`
   - Fai clic su **Remoto**

1. Fare clic su **Aggiungi**.

   ![Crea un tunnel SSH in Putty](../../assets/xdebug/putty-tunnels.png)

1. Nel riquadro _Categoria_ fare clic su **Sessione**.

1. Nel campo **Sessioni salvate** immettere un nome per il tunnel SSH.

1. Fai clic su **Salva**.

   ![Salva il tunnel SSH](../../assets/xdebug/putty-session-save.png)

1. Per verificare il tunnel SSH, fai clic su **Carica**, quindi su **Apri**.

   Se viene visualizzato un errore di tipo &quot;Impossibile connettersi&quot;, verificare quanto segue:

   - Tutte le impostazioni di Putty sono corrette
   - Stai eseguendo Putty sul computer in cui si trovano le chiavi SSH dell’infrastruttura cloud del tuo Adobe Commerce privato

## Accesso SSH agli ambienti Xdebug

Per avviare il debug, eseguire l’installazione e altro ancora, sono necessari i comandi SSH per accedere agli ambienti. È possibile ottenere queste informazioni tramite [[!DNL Cloud Console]](../development/secure-connections.md#use-an-ssh-command) e il foglio di calcolo del progetto.

Per gli ambienti Starter e gli ambienti di integrazione Pro, è possibile utilizzare il seguente comando CLI `magento-cloud` per SSH in tali ambienti:

```bash
magento-cloud environment:ssh --pipe -e <environment-ID>
```

Per utilizzare [!DNL Xdebug], SSH nell&#39;ambiente come segue:

```bash
ssh -R <xdebug listen port>:<host>:<xdebug listen port> <SSH-URL>
```

Ad esempio:

```bash
ssh -R 9000:localhost:9000 pwga8A0bhuk7o-mybranch@ssh.us.magentosite.cloud
```

## Debug per staging e produzione Pro

>[!NOTE]
>
>Negli ambienti Pro Staging &amp; Production, [!DNL Xdebug] è sempre disponibile in quanto tali ambienti dispongono di una configurazione speciale per [!DNL Xdebug]. Tutte le normali richieste Web vengono indirizzate a un processo PHP dedicato che non ha [!DNL Xdebug]. Pertanto, queste richieste vengono elaborate normalmente e non sono soggette al deterioramento delle prestazioni quando viene caricato [!DNL Xdebug]. Quando viene inviata una richiesta Web con la chiave [!DNL Xdebug], questa viene indirizzata a un processo PHP separato in cui è caricato [!DNL Xdebug].

Per utilizzare [!DNL Xdebug] in modo specifico nell&#39;ambiente di staging e produzione del piano Pro, è necessario creare un tunnel SSH separato e una sessione Web a cui accedere. Questo utilizzo differisce dall’accesso tipico, che fornisce accesso solo a te e non a tutti gli utenti.

Sono necessari i seguenti elementi:

- Comandi SSH per accedere agli ambienti. Puoi ottenere queste informazioni tramite [[!DNL Cloud Console]](../project/overview.md) o il tuo [!DNL Cloud Onboarding UI].
- Il valore `xdebug_key` impostato durante la configurazione degli ambienti Staging e Pro.

  È possibile trovare `xdebug_key` utilizzando SSH per accedere al nodo primario ed eseguire:

  ```bash
  cat /etc/platform/*/nginx.conf | grep xdebug.sock | head -n1
  ```

**Per impostare un tunnel SSH in un ambiente di gestione temporanea o di produzione**:

1. Apri un terminale.

1. Pulizia di tutte le sessioni SSH per ogni nodo Web del cluster.

   ```bash
   ssh USERNAME@CLUSTER.ent.magento.cloud 'rm /run/platform/USERNAME/xdebug.sock'
   ```

1. Impostare il tunnel SSH per Xdebug per ogni nodo Web del cluster.

   ```bash
   ssh -R /run/platform/USERNAME/xdebug.sock:localhost:9000 -N USERNAME@CLUSTER.ent.magento.cloud
   ```

>[!NOTE]
>
>Per ottenere il valore corretto per `USERNAME@CLUSTER.ent.magento.cloud`:
>- Metodo 1: CLI di magento-cloud: `magento-cloud ssh --all`
>- Metodo 2: Console Commerce: https://CONSOLE-URL/ENVIRONMENT, fare clic sul menu a discesa `SSH v`

**Per avviare il debug utilizzando l&#39;URL dell&#39;ambiente**:

1. Abilitare il debug remoto; visitare il sito nel browser e aggiungere quanto segue all&#39;URL in cui `KEY` è il valore per `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_START=KEY
   ```

   Questo passaggio imposta il cookie che invia le richieste del browser per attivare [!DNL Xdebug].

1. Completa il debug con [!DNL Xdebug].

1. Quando si è pronti per terminare la sessione, utilizzare il comando seguente per rimuovere il cookie e terminare il debug tramite il browser, dove `KEY` è il valore di `xdebug_key`.

   ```http
   ?XDEBUG_SESSION_STOP=KEY
   ```

   >[!NOTE]
   >
   >I `XDEBUG_SESSION_START` passati da `POST` richieste non sono supportati.

## Debug dei comandi CLI

Questa sezione descrive come eseguire il debug dei comandi CLI.

Per eseguire il debug dei comandi CLI:

1. SSH nel server di cui eseguire il debug utilizzando i comandi CLI.

1. Crea le seguenti variabili di ambiente:

   ```bash
   export XDEBUG_CONFIG='PHPSTORM'
   ```

   ```bash
   export PHP_IDE_CONFIG="serverName=<name of the server that is configured in PHPSTORM>"
   ```

   Queste variabili vengono rimosse al termine della sessione SSH.

1. Inizio debug

   Negli ambienti Starter e negli ambienti di integrazione Pro, eseguire il comando CLI per eseguire il debug.
È possibile aggiungere opzioni di runtime, ad esempio:

   ```bash
   php -d xdebug.profiler_enable=On -d xdebug.max_nesting_level=9999 bin/magento cache:clean
   ```

   Negli ambienti di staging e produzione Pro, è necessario specificare il percorso del file di configurazione PHP [!DNL Xdebug] durante il debug dei comandi CLI, ad esempio:

   ```bash
   php -c /etc/platform/USERNAME/php.xdebug.ini bin/magento cache:clean
   ```

## Debug richieste web

I passaggi seguenti sono utili per eseguire il debug delle richieste web.

1. Nel menu _Estensione_, fai clic su **Debug** per abilitare.

1. Fare clic con il pulsante destro del mouse, selezionare il menu delle opzioni e impostare il tasto IDE su **PHPSTORM**.

1. Installa il client [!DNL Xdebug] nel browser. Configuralo e attivalo.

### Esempio: configurazione di Chrome

In questa sezione viene descritto come utilizzare [!DNL Xdebug] in Chrome utilizzando l&#39;estensione helper [!DNL Xdebug]. Per informazioni sugli strumenti [!DNL Xdebug] per altri browser, consultare la documentazione del browser.

**Per utilizzare Xdebug Helper con Chrome**:

1. Crea un [tunnel SSH](#ssh-access-to-xdebug-environments) nel server cloud.

1. Installa l&#39;estensione [Xdebug Helper](https://chromewebstore.google.com/detail/eadndfjplgieldjbigjakmdgkmoaaaoc) dall&#39;archivio Chrome.

1. Abilita l’estensione in Chrome come illustrato nella figura seguente.

   ![Abilitare l&#39;estensione Xdebug in Chrome](../../assets/xdebug/enable-chrome-ext.png)

1. In Chrome, fai clic con il pulsante destro del mouse sull’icona verde helper nella barra degli strumenti di Chrome.

1. Dal menu a comparsa, fare clic su **Opzioni**.

1. Nell&#39;elenco _Chiave IDE_ fare clic su **PhpStorm**.

1. Fai clic su **Salva**.

   ![Opzioni Helper Xdebug](../../assets/xdebug/helper-options.png)

1. Apri il progetto PhpStorm.

1. Nella barra di navigazione superiore, fai clic sull&#39;icona **Avvia ascolto**.

   Se la barra di spostamento non è visualizzata, fare clic su **Visualizza** > **Barra di spostamento**.

1. Nel pannello di navigazione PhpStorm, fate doppio clic sul file PHP da testare.

## Debug del codice locale

A causa degli ambienti di sola lettura, per eseguire il debug è necessario richiamare il codice nella workstation locale da un ambiente o da un ramo Git specifico.

Il metodo che scegli dipende da te. Sono disponibili le seguenti opzioni:

- Estrai il codice da Git ed esegui `composer install`

  Questo metodo funziona a meno che `composer.json` non faccia riferimento a pacchetti in archivi privati a cui non hai accesso. Questo metodo consente di ottenere l’intera base di codice di Adobe Commerce.

- Copia le directory `vendor`, `app`, `pub`, `lib` e `setup`

  Questo metodo ti permette di avere tutto il codice che puoi eventualmente testare. A seconda del numero di risorse statiche disponibili, il trasferimento potrebbe richiedere molto tempo, con un volume elevato di file.

- Copia solo la directory `vendor`

  Poiché la maggior parte del codice si trova nella directory `vendor`, è probabile che questo metodo dia luogo a test validi, anche se non stanno testando l&#39;intera base di codice.

**Per comprimere i file e copiarli nel computer locale**:

1. Utilizza SSH per accedere all’ambiente remoto.

1. Comprimi i file.

   ```bash
   tar -czf /tmp/<file-name>.tgz <directory list>
   ```

   Ad esempio, per comprimere solo la directory `vendor`:

   ```bash
   tar -czf /tmp/vendor.tgz vendor
   ```

1. Nell&#39;ambiente locale, usare PhpStorm per comprimere i file.

   ```bash
   cd <phpstorm project root dir>
   ```

   ```bash
   rsync <SSH-URL>:/tmp/<file-name>.tgz .
   ```

   ```bash
   tar xzf <file-name>.tgz
   ```
