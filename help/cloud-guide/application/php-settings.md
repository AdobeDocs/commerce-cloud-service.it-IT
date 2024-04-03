---
title: Impostazioni PHP
description: Scopri le impostazioni PHP ottimali per la configurazione delle applicazioni Commerce nell’infrastruttura cloud.
feature: Cloud, Configuration, Extensions
exl-id: b4180265-f7a1-48e4-8c23-27835253e171
source-git-commit: 9b3772cf640ebc56063434e1aa8acb1ec51dc63c
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# Impostazioni PHP

Puoi scegliere quale [versione di PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) per eseguire in `.magento.app.yaml` file:

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>Se si esegue l’aggiornamento a PHP 8.1 e versioni successive, rimuovere JSON da [`runtime: extensions:` proprietà](properties.md#runtime) nel `.magento.app.yaml` e ridistribuirli. L’estensione JSON viene installata in ambiente Cloud a partire da PHP 8.0.

## Configurare PHP

È possibile personalizzare le impostazioni PHP per l&#39;ambiente utilizzando un `php.ini` file aggiunto alla configurazione gestita da Adobe Commerce.

Nell’archivio, aggiungi `php.ini` nella directory principale dell’applicazione (directory principale dell’archivio).

>[!TIP]
>
>La configurazione errata delle impostazioni PHP può causare problemi, pertanto solo gli amministratori esperti devono impostare queste opzioni.

### Aumentare il limite di memoria PHP

Per aumentare il limite di memoria PHP, aggiungere la seguente impostazione alla `php.ini` file:

```ini
memory_limit = 1G
```

Per il debug, aumentare il valore a 2G.

### Ottimizza configurazione realpath_cache

Imposta quanto segue `realpath_cache` per migliorare le prestazioni dell&#39;applicazione.

```conf
;
; Increase realpath cache size
;
realpath_cache_size = 10M

;
; Increase realpath cache ttl
;
realpath_cache_ttl = 7200
```

Queste impostazioni consentono ai processi PHP di memorizzare nella cache i percorsi dei file invece di cercarli per ogni caricamento di pagina. Consulta [Ottimizzazione delle prestazioni](https://www.php.net/manual/en/ini.core.php) nella documentazione PHP.

>[!NOTE]
>
>Per un elenco delle impostazioni di configurazione PHP consigliate, vedere [Impostazioni PHP richieste](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) nel _Guida all’installazione_.

### Controllare le impostazioni PHP personalizzate

Dopo aver premuto `php.ini` Modifiche all’ambiente Cloud, puoi verificare che la configurazione PHP personalizzata sia stata aggiunta all’ambiente. Ad esempio, utilizza SSH per accedere all’ambiente remoto e visualizzare il file utilizzando un metodo simile al seguente:

```bash
cat /etc/php/<php-version>/fpm/php.ini
```

>[!WARNING]
>
>Se utilizzi Cloud Docker for Commerce per lo sviluppo locale, consulta [Contenitori del servizio Docker](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container) per informazioni sull&#39;utilizzo di un `php.ini` in un ambiente Docker.

## Abilitare le estensioni

È possibile abilitare o disabilitare le estensioni PHP in `runtime:extension` sezione. Inoltre, le estensioni specificate diventano disponibili nei contenitori Docker PHP.

>[!IMPORTANT]
>
>Prima di abilitare le estensioni, è importante comprendere che la versione PHP deve essere compatibile con il sistema operativo che ospita il progetto. Prima di procedere, l&#39;ambiente del progetto potrebbe richiedere un aggiornamento del sistema operativo da parte del team dell&#39;infrastruttura.

Esempio in `.magento.app.yaml` file:

```yaml
runtime:
    extensions:
        - sockets
        - sodium
        - ssh2
    disabled_extensions:
        - bcmath
        - bz2
        - calendar
        - exif
```

Utilizza SSH per accedere a un ambiente e elencare le estensioni PHP.

```bash
php -m
```

Per informazioni dettagliate su una specifica estensione PHP, vedere [Elenco estensioni PHP](https://www.php.net/manual/en/extensions.alphabetical.php).

La tabella seguente mostra le estensioni PHP supportate durante la distribuzione di Adobe Commerce sulla piattaforma Cloud.

| Estensioni predefinite | Estensioni installate<br>che non può essere disinstallato | Estensioni installabili<br>e disinstallato in base alle esigenze |
| ------------------ | --------------------- | --------------------- |
| `bcmath`<br>`bz2`<br>`calendar`<br>`exif`<br>`gd`<br>`gettext`<br> `intl`<br> `mysqli`<br> `openswoole`<br> `pcntl`<br> `pdo_mysql`<br> `soap`<br> `sockets`<br>  `sysvmsg`<br> `sysvsem`<br> `sysvshm`<br> `opcache`<br> `zip` | `ctype`<br> `curl`<br>`date`<br> `dom`<br> `fileinfo`<br> `filter`<br> `ftp`<br> `hash`<br> `iconv`<br> `json`<br> `mbstring`<br> `mysqlnd`<br> `openssl`<br> `pcre`<br> `pdo`<br> `pdo_sqlite`<br> `phar`<br>`posix`<br> `readline`<br> `session`<br> `sqlite3`<br> `tokenizer`<br> `xml`<br> `xmlreader`<br> `xmlwriter`<br> | `geoip`<br>`gmp`<br> `igbinary`<br> `imagick`<br> `imap`<br> `ioncube` <br>`ldap`<br> `mailparse`<br> `mcrypt`<br> `msgpack`<br> `mysqli`<br> `oauth`<br> `pdo_mysql`<br> `propro`<br> `pspell`<br> `raphf`<br> `recode`<br> `redis`<br> `shmop` `sockets`<br> `sodium`<br> `ssh2`<br>`tidy`<br> `xdebug`<br> `xmlrpc`<br> `xsl`<br> `yaml` |

I requisiti del modulo PHP sono legati alla versione Adobe Commerce. Consulta [Requisiti PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html).

### Supporto per le estensioni

Per i progetti Pro, le seguenti estensioni richiedono supporto aggiuntivo per l’installazione:

- `sourceguardian`

Ad esempio, per impostare PHP in modo che esegua solo script protetti da SourceGuardian in tutti gli ambienti, è necessario impostare la seguente opzione in `php.ini` file:

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

Consulta [sezione 3.5 della documentazione di SourceGuardian](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _Questo è un collegamento a un PDF_.

[Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per assistenza nell’installazione di queste estensioni PHP in tutti gli ambienti di produzione e di staging Pro. Includi gli aggiornamenti `.magento/services.yaml` file, `.magento.app.yaml` file con la versione PHP aggiornata ed eventuali estensioni PHP aggiuntive. Per le modifiche a un ambiente di produzione live, devi fornire un preavviso minimo di 48 ore. L’aggiornamento del progetto da parte del team di infrastruttura Cloud può richiedere fino a 48 ore.

>[!WARNING]
>
>Il PHP compilato con il debug non è supportato e il probe potrebbe entrare in conflitto con [!DNL XDebug] o [!DNL XHProf]. Disattiva queste estensioni quando abiliti il Probe. Il probe è in conflitto con alcune estensioni PHP come [!DNL Pinba] o IonCube.
