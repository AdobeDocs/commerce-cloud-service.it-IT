---
title: Esempio di gestione delle impostazioni specifiche del sistema
description: Vedi un esempio di come gestire e sincronizzare le impostazioni di configurazione dell’archivio in tutti gli ambienti Adobe Commerce sull’infrastruttura cloud.
hidefromtoc: true
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '865'
ht-degree: 0%

---


# Esempio di gestione delle impostazioni specifiche del sistema

Questo esempio mostra come utilizzare la gestione della configurazione per mantenere le impostazioni dello store coerenti in tutti gli ambienti.

Nell&#39;esempio viene utilizzata la procedura seguente definita in [Impostazioni archivio](store-settings.md):

1. Immetti le configurazioni nell&#39;archivio di amministrazione dell&#39;ambiente di integrazione.
1. Crea un file `config.php` e trasferiscilo nella workstation locale.
1. Invia `config.php` all&#39;ambiente di integrazione remota.
1. Verifica che le impostazioni non siano modificabili in Admin.
1. Apporta le modifiche necessarie:

   * Modifica le impostazioni di configurazione nell’ambiente di integrazione.
   * Per aggiungere configurazioni, eseguire di nuovo il comando per creare `config.php`. Le nuove configurazioni vengono aggiunte al file.
   * Per rimuovere o modificare le configurazioni esistenti, modifica manualmente il file.
   * Eseguire il commit e il push.

Ad esempio, è possibile impostare le seguenti impostazioni:

* Disabilita [impostazioni locali](https://glossary.magento.com/locale) e impostazioni di ottimizzazione dei file statici nell&#39;ambiente di integrazione
* Abilitare l’ottimizzazione dei file statici negli ambienti di staging e produzione
* Configurare Fastly in staging e produzione con credenziali specifiche per ciascuno

_Ottimizzazione file statico_ significa unire e minimizzare i fogli di stile di JavaScript e CSS e minimizzare i modelli di HTML. Consulta [Strategie di distribuzione del contenuto statico](../deploy/static-content.md).

## Prerequisiti

Per completare queste attività di gestione della configurazione, è necessario disporre dei seguenti elementi:

* Ruolo di lettore del progetto con privilegi di [amministratore dell&#39;ambiente](../project/user-access.md)
* URL amministratore e credenziali per gli ambienti di integrazione, staging e produzione

## Configurare l’amministratore Commerce

Nell’ambiente di integrazione, puoi accedere all’amministratore per modificare le impostazioni di configurazione del sistema per store, siti web, moduli o estensioni, ottimizzazione statica dei file e valori di sistema relativi alla distribuzione di contenuti statici. Vedi [Dati di configurazione](store-settings.md#scd-performance).

**Per modificare le impostazioni locali e di ottimizzazione dei file statici**:

1. Accedi all’amministrazione dell’ambiente di integrazione. È possibile accedere a questo URL tramite [[!DNL Cloud Console]](../project/overview.md).
1. Passa a **Archivi** > Impostazioni > **Configurazione** > Generale > **Generale**.
1. Nella navigazione delle pagine, espandere **Opzioni internazionali**.
1. Nell&#39;elenco **Impostazioni locali** modificare le impostazioni locali. Potrai cambiarlo in un secondo momento.

   ![Modifica impostazioni locali](../../assets/locale-options.png)

1. Fai clic su **Salva configurazione**.
1. Se richiesto, [svuotare la cache](https://docs.magento.com/user-guide/system/cache-management.html).
1. Esci dall’Admin.

## Esportare i valori e trasferire config.php nel sistema locale

Questo passaggio crea e trasferisce il file di configurazione `config.php` nell&#39;ambiente di integrazione utilizzando un comando eseguito nel computer locale.

Questa procedura corrisponde al passaggio 2 della [procedura consigliata](store-settings.md). Dopo aver creato `config.php`, trasferiscilo nel sistema locale in modo da poterlo aggiungere a Git.

**Per creare e trasferire`config.php`**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Modifica dell’ambiente di integrazione.

   ```bash
   magento-cloud environment:checkout integration
   ```

1. Crea un dump locale del database remoto.

   ```bash
   magento-cloud db:dump
   ```

Il seguente frammento di codice da `config.php` mostra un esempio di modifica delle impostazioni locali predefinite in `en_GB` e delle impostazioni di ottimizzazione del file statico:

```php?start_inline=1
'general' => [
     'locale' => [
         'code' => 'en_GB',
         'timezone' => 'UTC',
     ],

     ... more ...

 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],
     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],

     ... more ...
```

## Effettuare il push e distribuire config.php negli ambienti

Dopo aver creato `config.php` e averlo trasferito al sistema locale, esegui il commit in Git e invialo agli ambienti. Questa procedura corrisponde ai passaggi 3 e 4 della [procedura consigliata](store-settings.md).

Il comando seguente aggiunge, conferma e invia messaggi push al ramo `master`:

```bash
git add app/etc/config.php && git commit -m "Add system-specific configuration" && git push origin master
```

Completa la distribuzione del codice in Staging e Produzione. Per iniziare, si invia a `staging` e `master` rami. Per informazioni dettagliate sui comandi di distribuzione, vedere [Distribuire l&#39;archivio](../deploy/staging-production.md).

Attendi il completamento dell’implementazione in tutti gli ambienti.

## Verificare le modifiche alla configurazione

Dopo aver inviato `config.php` agli ambienti, tutti i valori modificati devono essere di sola lettura nell&#39;amministratore. In questo esempio, le impostazioni internazionali predefinite modificate e le impostazioni di ottimizzazione statica dei file non devono essere modificabili in Admin. Queste impostazioni di configurazione sono impostate in `config.php`.

Per verificare le modifiche alla configurazione:

1. Esci dall’amministratore in uno degli ambienti.
1. Accedi di nuovo all’amministratore.
1. Fai clic su **Archivi** > Impostazioni > **Configurazione** > Generale > **Generale**.
1. Nel riquadro di destra espandere **Opzioni internazionali**.

   Non è possibile modificare diversi campi, come illustrato nell’esempio seguente. Queste impostazioni di configurazione sono gestite da `config.php`.

   ![Alcuni valori non sono più modificabili in Admin](../../assets/locale-options-disabled.png)

1. Esci dall’Admin.

## Modificare e aggiornare le impostazioni di configurazione specifiche del sistema

Se è necessario modificare una di queste impostazioni, modificare manualmente il file `config.php` con un editor di testo. Dopo aver completato le modifiche o le rimozioni, puoi eseguirne il commit e inviarlo all’ambiente remoto seguendo i passaggi precedenti.

Per aggiungere configurazioni, modifica l’ambiente di integrazione ed esegui nuovamente il comando per generare il file. Tutte le nuove configurazioni vengono aggiunte al codice nel file. Spingilo su Git seguendo i passaggi precedenti.

In questo esempio, modificare le impostazioni di ottimizzazione dei file statici e aggiungere una nuova impostazione per JavaScript.

### Aggiungere configurazioni nell’integrazione

Aggiungere i valori di configurazione nell’ambiente di integrazione Admin. In questo esempio vengono uniti i file JavaScript.

1. Esci dall’amministratore dell’integrazione.
1. Accedi di nuovo all’amministratore dell’integrazione.
1. Fai clic su **Archivi** > Impostazioni > **Configurazione** > **Avanzate** > **Sviluppatore**.
1. Nel riquadro di destra espandere **Impostazioni JavaScript**.
1. Nell&#39;elenco **Unisci file JavaScript** fare clic su **Sì**.
1. Fai clic su **Salva configurazione**.
1. Se richiesto, [svuotare la cache](https://docs.magento.com/user-guide/system/cache-management.html).
1. Esci dall’Admin.

Eseguendo nuovamente il comando dump, la nuova configurazione viene aggiunta al file.

```bash
magento-cloud db:dump
```

### Modifica config.php con nuove impostazioni

Sul tuo computer locale, utilizza un editor di testo per modificare il file `app/etc/config.php` aggiornato. Modifica queste impostazioni per abilitare la minimizzazione per i file JavaScript, HTML e CSS.

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '0',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '0',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '0',
     ],
```

Per modificare le impostazioni per consentire la minimizzazione, modificare `'0'` in `'1'` per `'minify_html'` e ogni opzione `'minify_files'`:

```php?start_inline=1
 'dev' => [
     'template' => [
         'allow_symlink' => '0',
         'minify_html' => '1',
     ],

     ... more ...

     'js' => [
         'merge_files' => '0',
         'enable_js_bundling' => '0',
         'minify_files' => '1',
     ],
     'css' => [
         'merge_css_files' => '0',
         'minify_files' => '1',
     ],
```

Salva le modifiche nel file.

### Invia le modifiche a Git

Per inviare le modifiche, immetti quanto segue:

```bash
git add app/etc/config.php
```

```bash
git commit -m "Add system-specific configuration and edit settings"
```

```bash
git push origin master
```

Attendere il completamento della distribuzione.

Ripeti il processo di distribuzione per inviare il codice a tutti gli ambienti.
