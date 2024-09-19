---
title: Impostazioni PHP
description: Scopri le impostazioni PHP ottimali per la configurazione delle applicazioni Commerce nell’infrastruttura cloud.
feature: Cloud, Configuration, Extensions
exl-id: b4180265-f7a1-48e4-8c23-27835253e171
source-git-commit: 9d20cd11e541d343af158eb3eadcf641fb2751bd
workflow-type: tm+mt
source-wordcount: '536'
ht-degree: 0%

---

# Impostazioni PHP

Puoi scegliere quale [versione di PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/system-requirements.html) eseguire nel file `.magento.app.yaml`:

```yaml
name: mymagento
type: php:<version>
```

>[!TIP]
>
>Se si esegue l&#39;aggiornamento a PHP 8.1 e versioni successive, rimuovere JSON dalla proprietà [`runtime: extensions:`](properties.md#runtime) nel file `.magento.app.yaml` e ridistribuirlo. L’estensione JSON viene installata in ambiente Cloud a partire da PHP 8.0.

## Configurare PHP

È possibile personalizzare le impostazioni PHP per l&#39;ambiente utilizzando un file `php.ini` aggiunto alla configurazione gestita da Adobe Commerce.

Nel repository, aggiungere il file `php.ini` alla radice dell&#39;applicazione (la radice del repository).

>[!TIP]
>
>La configurazione errata delle impostazioni PHP può causare problemi, pertanto solo gli amministratori esperti devono impostare queste opzioni.

### Aumentare il limite di memoria PHP

Per aumentare il limite di memoria PHP, aggiungere la seguente impostazione al file `php.ini`:

```ini
memory_limit = 1G
```

Per il debug, aumentare il valore a 2G.

### Ottimizza configurazione realpath_cache

Imposta le seguenti impostazioni di `realpath_cache` per migliorare le prestazioni dell&#39;applicazione.

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

Queste impostazioni consentono ai processi PHP di memorizzare nella cache i percorsi dei file invece di cercarli per ogni caricamento di pagina. Vedi [Ottimizzazione delle prestazioni](https://www.php.net/manual/en/ini.core.php) nella documentazione di PHP.

>[!NOTE]
>
>Per un elenco delle impostazioni di configurazione PHP consigliate, vedere [Impostazioni PHP richieste](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html) nella _Guida all&#39;installazione_.

### Controllare le impostazioni PHP personalizzate

Dopo aver inviato le modifiche di `php.ini` all&#39;ambiente Cloud, puoi verificare che la configurazione PHP personalizzata sia stata aggiunta all&#39;ambiente. Ad esempio, utilizza SSH per accedere all’ambiente remoto e visualizzare il file utilizzando un metodo simile al seguente:

```bash
cat /etc/php/<php-version>/fpm/php.ini
```

>[!WARNING]
>
>Se utilizzi Cloud Docker per Commerce per lo sviluppo locale, consulta [Docker service container](https://developer.adobe.com/commerce/cloud-tools/docker/containers/service/#fpm-container) per informazioni sull&#39;utilizzo di un file `php.ini` personalizzato in un ambiente Docker.

## Abilitare le estensioni

È possibile abilitare o disabilitare le estensioni PHP nella sezione `runtime:extension`. Inoltre, le estensioni specificate diventano disponibili nei contenitori Docker PHP.

>[!IMPORTANT]
>
>Prima di abilitare le estensioni, è importante comprendere che la versione PHP deve essere compatibile con il sistema operativo che ospita il progetto. Prima di procedere, l&#39;ambiente del progetto potrebbe richiedere un aggiornamento del sistema operativo da parte del team dell&#39;infrastruttura.

Esempio nel file `.magento.app.yaml`:

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

Per informazioni dettagliate su un&#39;estensione PHP specifica, vedere [Elenco estensioni PHP](https://www.php.net/manual/en/extensions.alphabetical.php).

La tabella seguente mostra le estensioni PHP supportate durante la distribuzione di Adobe Commerce sulla piattaforma Cloud.

{{$include /help/_includes/templated/php-extensions-cloud.md}}

I requisiti del modulo PHP sono legati alla versione Adobe Commerce. Consulta [Requisiti PHP](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/php-settings.html).

### Supporto per le estensioni

Per i progetti Pro, le seguenti estensioni richiedono supporto aggiuntivo per l’installazione:

- `ioncube`
- `sourceguardian`

Ad esempio, per impostare PHP in modo che esegua solo script protetti da SourceGuardian in tutti gli ambienti, è necessario impostare nel file `php.ini` l&#39;opzione seguente:

```ini
[SourceGuardian]
sourceguardian.restrict_unencoded = "1"
```

Consulta la [sezione 3.5 della documentazione di SourceGuardian](https://sourceguardian.com/demofiles/files/SourceGuardian%20for%20Linux%20User%20Manual.pdf). _Collegamento a un PDF_.

[Invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per assistenza sull&#39;installazione di queste estensioni PHP in tutti gli ambienti di produzione e di staging Pro. Includi il file `.magento/services.yaml` aggiornato, il file `.magento.app.yaml` con la versione PHP aggiornata ed eventuali estensioni PHP aggiuntive. Per le modifiche a un ambiente di produzione live, devi fornire un preavviso minimo di 48 ore. L’aggiornamento del progetto da parte del team di infrastruttura Cloud può richiedere fino a 48 ore.

>[!WARNING]
>
>PHP compilato con debug non supportato. Il probe potrebbe essere in conflitto con [!DNL XDebug] o [!DNL XHProf]. Disattiva queste estensioni quando abiliti il Probe. Il probe è in conflitto con alcune estensioni PHP come [!DNL Pinba] o IonCube.
