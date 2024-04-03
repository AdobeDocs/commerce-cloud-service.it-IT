---
title: Crons, proprietà
description: Vedi esempi su come configurare la proprietà "crons" in [!DNL Commerce] file di configurazione dell'applicazione.
feature: Cloud, Configuration
exl-id: 67d592c1-2933-4cdf-b4f6-d73cd44b9f59
source-git-commit: eace5d84fa0915489bf562ccf79fde04f6b9d083
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 0%

---

# Crons, proprietà

Adobe Commerce utilizza `crons` proprietà per programmare attività ripetitive. È ideale per pianificare un&#39;attività specifica da eseguire in determinati momenti della giornata. A causa della natura degli ambienti di sola lettura, è possibile eseguire un solo processo cron alla volta sull’istanza web per i progetti di infrastruttura cloud di Adobe Commerce. È consigliabile suddividere le attività con tempi di esecuzione lunghi in attività più piccole e in coda. In alternativa, puoi creare un’ [istanza di lavoro](workers-property.md).

L’Adobe consiglia di eseguire `crons` come [proprietario del file system](https://experienceleague.adobe.com/docs/commerce-operations/installation-guide/prerequisites/file-system/configure-permissions.html). Esegui _non_ eseguire `crons` as `root` o come utente del server web.

Questa configurazione è diversa dalle distribuzioni locali di Adobe Commerce, che hanno più processi cron predefiniti. Consulta [Configurare i processi cron](https://experienceleague.adobe.com/docs/commerce-operations/configuration-guide/cli/configure-cron-jobs.html) nel _Guida alla configurazione_.

## Imposta processi cron

Il `crons` La proprietà descrive i processi attivati in base a una pianificazione. Ogni processo richiede un nome e le seguenti opzioni:

- `spec`- Espressione cron utilizzata per la programmazione.
- `cmd`- Comando su cui eseguire il comando `start` e `stop`.
- `shutdown_timeout`—(_Facoltativo_) Se un processo cron viene annullato, indica il numero di secondi dopo i quali viene inviato un segnale SIGKILL per arrestare il processo o il processo. Il valore predefinito è 10 secondi.
- `timeout`—(_Facoltativo_) La quantità massima di tempo per cui un processo cron può essere eseguito prima del timeout. Il valore predefinito è 86400 secondi (24 ore).

Per impostazione predefinita, a ogni progetto cloud di Commerce è associato il seguente `crons` configurazione in `.magento.app.yaml` file:

```yaml
crons:
    cronrun:
        spec: "* * * * *"
        cmd: "php bin/magento cron:run"
```

Se il progetto richiede processi cron personalizzati, puoi aggiungerli al predefinito `crons` configurazione. Consulta [Creare un processo cron](#build-a-cron-job).

### `crontab`

Adobe Commerce ha aggiunto un’opzione di configurazione auto-crons solo ai progetti Pro per supportare self-service `crons` negli ambienti di staging e produzione. Se questa opzione è abilitata, puoi utilizzare `crontab` per rivedere la configurazione cron. Questo è _non_ disponibile con progetti iniziali.

Anche se è possibile utilizzare `crontab` per rivedere la configurazione dei progetti Pro, Adobe Commerce non utilizza `crontab` per eseguire processi cron per i siti distribuiti nell’infrastruttura cloud.

**Per esaminare la configurazione cron in ambienti Pro**:

1. Utilizzare [SSH](../development/secure-connections.md#use-an-ssh-command) per accedere all&#39;ambiente remoto.

1. Elencare i processi cron pianificati.

   ```shell
   crontab -l
   ```

   >[!NOTE]
   >
   >Se il `crontab -l` restituisce un `Command not found` errore, è necessario [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per abilitare l&#39;opzione di configurazione self-service di auto-crons sul progetto Pro.

L&#39;esempio seguente mostra `crontab` output per un ambiente con solo il valore predefinito `crons` configurazione:

```terminal
username@hostname:~$ crontab -l
# Crontab is managed by the system, attempts to edit it directly will fail.
SHELL=/etc/platform/6fck2obu3244c/cron-run
MAILTO=""

# m h  dom mon dow  job_name

* * * * *           cronrun
```

## Creare un processo cron

Un job cron include la pianificazione e le specifiche di tempo e il comando da eseguire all&#39;ora pianificata. Per ambienti Starter e Pro `integration` degli ambienti, l&#39;intervallo minimo è una volta ogni cinque minuti. Per gli ambienti di staging e produzione Pro, l’intervallo minimo è una volta al minuto. In Adobe Commerce su infrastruttura cloud, puoi aggiungere processi cron personalizzati alla `.magento.app.yaml` file in `crons` sezione. Il formato generale è `spec` per la programmazione e `cmd` per specificare il comando o lo script personalizzato da eseguire.

### Specifiche

Adobe Commerce utilizza un’espressione con cinque valori per `crons` specifica (spec): `* * * * *`

1. Minuti (da 0 a 59) Per tutti gli ambienti Starter e Pro, la frequenza minima supportata per i processi cron è di cinque minuti. Potrebbe essere necessario configurare le impostazioni nell’amministratore.
2. Ora (da 0 a 23)
3. Giorno del mese (da 1 a 31)
4. Mese (da 1 a 12)
5. Giorno della settimana (da 0 a 6) (da domenica a sabato; 7 è anche domenica in alcuni sistemi)

Alcuni esempi:

- `00 */3 * * *` viene eseguito ogni tre ore al primo minuto (03:00, 06:00)
- `20 */8 * * *` viene eseguito ogni 8 ore al minuto 20 (12:20, 8:20, 16:20)
- `00 00 * * *` esegue una volta al giorno a mezzanotte
- `00 * * * 1` esegue una volta alla settimana il lunedì a mezzanotte.

>[!NOTE]
>
>Il `crons` tempo specificato nel `.magento.app.yaml` il file si basa sul fuso orario del server, non su quello specificato nei valori di configurazione dell&#39;archivio nel database.

Quando si determina la programmazione, considerare il tempo necessario per completare l&#39;attività. Ad esempio, se si esegue un processo ogni tre ore e il completamento dell&#39;attività richiede 40 minuti, è possibile modificare la tempistica pianificata.

### Comando

Il `cmd` specifica il comando o lo script personalizzato da eseguire. Il formato dello script di comando può includere quanto segue:

```text
<path-to-php-binary> <project-dir>/<script-command>
```

Ad esempio:

```yaml
crons:
    spec: "00 */8 * * *"
    cmd: "/usr/bin/php /app/abc123edf890/bin/magento export:start catalog_category_product"
```

In questo esempio, `<path-to-php-binary>` è `/usr/bin/php`. La directory di installazione, che include l’ID del progetto, è `/app/abc123edf890/bin/magento`e l&#39;azione script è `export:start catalog_category_product`.

### Aggiungi processi cron personalizzati al progetto

In Adobe Commerce su piattaforma di infrastruttura cloud, puoi aggiungere personalizzazioni alla `crons` sezione del [`.magento.app.yaml`](../application/configure-app-yaml.md) file.

>[!NOTE]
>
>Per ambienti Starter e Pro `integration` degli ambienti, l&#39;intervallo minimo è una volta ogni cinque minuti. Per gli ambienti di staging e produzione Pro, l’intervallo minimo è una volta al minuto. Non è possibile configurare intervalli più frequenti dei valori minimi predefiniti.

Sui progetti Adobe Commerce Pro, il [funzione auto-crons](#set-up-cron-jobs) deve essere abilitato nel progetto prima di poter aggiungere processi cron personalizzati agli ambienti di staging e produzione utilizzando `.magento.app.yaml` file. Se questa funzione non è abilitata, [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per attivare la funzione di esecuzione automatica dell&#39;esecuzione.

**Per aggiungere processi cron personalizzati**:

1. Nell’ambiente di sviluppo locale, modifica il `.magento.app.yaml` file in Adobe Commerce `/app` directory.

1. In `crons` , aggiungi la personalizzazione utilizzando il seguente formato:

   ```yaml
   crons:
       <cron_name_1>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
       <cron_name_2>:
           spec: "<schedule_time>"
           cmd: "<schedule_command>"
   ```

   Nell&#39;esempio seguente, il `productcatalog` il processo esporta il catalogo prodotti ogni otto ore, 20 minuti dopo l’ora.

   ```yaml
   crons:
       magento:
           spec: '* * * * *'
           cmd: 'php bin/magento cron:run'
       productcatalog:
           spec: '20 */8 * * *'
           cmd: 'bin/magento export:start catalog_product_category'
   ```

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add .magento.app.yaml && git commit -m "cron config updates" && git push origin <branch-name>
   ```

### Aggiorna processi cron

Per aggiungere, rimuovere o aggiornare un processo personalizzato, modificare la configurazione in `crons` sezione del `.magento.app.yaml` file. Quindi, verificare gli aggiornamenti in remoto `integration` prima di inviare le modifiche agli ambienti di staging e produzione.

## Disabilita processi cron

È possibile disabilitare manualmente i processi cron prima di completare attività di manutenzione come la reindicizzazione o la pulizia della cache per evitare problemi di prestazioni. È possibile utilizzare `ece-tools` Comando CLI `cron:disable` per disabilitare tutti i processi cron e arrestare tutti i processi cron attivi.

**Per disattivare i processi cron**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Disattivare i processi cron e arrestare i processi cron attivi.

   ```shell
   ./vendor/bin/ece-tools cron:disable
   ```

1. Dopo aver completato le attività di manutenzione necessarie, accertati di abilitare nuovamente i processi cron.

   ```shell
   ./vendor/bin/ece-tools cron:enable
   ```

## Risoluzione dei problemi dei processi cron

Adobe ha aggiornato il pacchetto di infrastruttura Adobe Commerce on cloud per ottimizzare l’elaborazione dei cron in Adobe Commerce su piattaforma di infrastruttura cloud e risolvere i problemi relativi ai cron. Se riscontri problemi con l’elaborazione del cron, assicurati che il progetto utilizzi la versione più recente del `ece-tools` pacchetto. Consulta [Aggiorna utensili ECE](../dev-tools/update-package.md).

È possibile esaminare le informazioni sull’elaborazione delle cron nei file di registro a livello di applicazione per ciascun ambiente. Consulta [Registri applicazioni](../test/log-locations.md#application-logs).

Consulta i seguenti articoli sul supporto Adobe Commerce per assistenza nella risoluzione dei problemi correlati ai cron:

- [Le attività di controllo bloccano le attività da altri gruppi](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/cron-tasks-lock-tasks-from-other-groups.html)

- [Ripristina manualmente i processi cron bloccati sul cloud](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/reset-stuck-magento-cron-jobs-manually-on-cloud.html)
