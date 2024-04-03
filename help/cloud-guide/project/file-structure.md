---
title: Struttura del progetto
description: Scopri la struttura dei file e i modelli di progetto per Adobe Commerce sull’infrastruttura cloud.
exl-id: 6dc559bd-116b-4745-a85b-731508e113ff
source-git-commit: 47ef728ea46b137eeaabbdbada940143d8773ef0
workflow-type: tm+mt
source-wordcount: '454'
ht-degree: 0%

---

# Struttura del progetto

Un progetto Adobe Commerce su infrastruttura cloud include file essenziali per le credenziali e la configurazione dell’applicazione. Questi file sono disponibili in come modello in base alla versione di Adobe Commerce. Consulta i modelli cloud basati sulla versione di Adobe Commerce in [`magento/magento-cloud` Archivio GitHub](https://github.com/magento/magento-cloud).

Nella tabella seguente sono descritti i file inclusi in un progetto cloud:

| File | Descrizione |
| ------------------------- | ------------ |
| `/.magento/routes.yaml` | File di configurazione con reindirizzamento `www` al dominio apex e `php` da distribuire tramite HTTP. Consulta [Configurare le route](../routes/routes-yaml.md). |
| `/.magento/services.yaml` | File di configurazione che definisce un&#39;istanza MySQL (MariaDB), Redis e OpenSearch o un Elasticsearch. Consulta [Configurare i servizi](../services/services-yaml.md). |
| `/app` | Il `code` cartella viene utilizzata per i moduli personalizzati. Il `design` cartella viene utilizzata per [temi personalizzati](../store/custom-theme.md). Il `etc` la cartella contiene i file di configurazione per l&#39;applicazione. |
| `/m2-hotfixes` | Utilizzato per patch personalizzate. |
| `/update` | Cartella di servizio utilizzata dal modulo di supporto. |
| `.gitignore` | Specificare quali file e directory ignorare. Consulta [`.gitignore` riferimento](#ignoring-files). |
| `.magento.app.yaml` | Un file di configurazione che definisce le proprietà per generare l’applicazione. Consulta [Configura applicazione](../application/configure-app-yaml.md). |
| `.magento.env.yaml` | File di configurazione per le fasi di build, distribuzione e post-distribuzione. Il `ece-tools` Il pacchetto include un esempio di questo file. Consulta [Configurare gli ambienti](../environment/configure-env-yaml.md). |
| `composer.json` | Recupera Adobe Commerce e gli script di configurazione per preparare l’applicazione. Consulta [Pacchetti richiesti](../development/overview.md#required-packages). |
| `composer.lock` | Memorizza le dipendenze di versione per ogni pacchetto. Consulta [Pacchetti richiesti](../development/overview.md#required-packages). |
| `magento-vars.php` | Utilizzato per definire [più store](../store/multiple-sites.md) e siti che utilizzano variabili. |

{style="table-layout:auto"}

>[!NOTE]
>
>Quando invii le modifiche locali al server remoto, lo script di distribuzione utilizza i valori definiti dai file di configurazione in `.magento` , quindi lo script elimina la directory e il relativo contenuto. Questo non influisce sull’ambiente di sviluppo locale.

## Directory radice dell&#39;applicazione

La posizione della directory radice dell&#39;applicazione dipende dall&#39;ambiente.

- **Integrazione Starter e Pro**: `/app`
- **Avvia produzione**: `/<project-ID>`
- **Pro Staging**: `/<project-ID>_stg`
- **Produzione Pro**: `/<project-ID>`

### Directory scrivibili

Gli ambienti remoti di integrazione, staging e produzione sono di sola lettura. Le seguenti directory sono *solo* directory scrivibili per motivi di sicurezza:

- `var`
- `pub/static`
- `pub/media`
- `app/etc`
- `/tmp`

>[!NOTE]
>
>Negli ambienti di produzione e staging, ogni nodo nel cluster a tre nodi dispone di un `/tmp` directory non condivisa con gli altri nodi.

## Ignora file

Esiste una base `.gitignore` file con l’archivio dei progetti Adobe Commerce on cloud infrastructure. Scopri le ultime novità [file .gitignore nell’archivio cloud magento](https://github.com/magento/magento-cloud/blob/master/.gitignore). Per aggiungere un file presente in `.gitignore` , è possibile utilizzare `-f` (forza) opzione durante la gestione temporanea di un commit:

```bash
git add <path/filename> -f
```

## Cambia modello base

Puoi utilizzare i seguenti passaggi per modificare la struttura di un progetto esistente in modo che rifletta il modello base più recente per Adobe Commerce sull’infrastruttura cloud.

1. Clonare il progetto su una workstation locale.

1. Aggiornare il `composer.json` file con i seguenti valori per `extra` sezione.

   ```json
   "extra": {
       "magento-force": true
       "magento-deploystrategy": "copy"
   }
   ```

1. Aggiungi il `.gitignore` file progettato per il modello di base. Ad esempio, se hai bisogno di `.gitignore` per il modello versione 2.2.6, utilizza il file [.gitignore per 2.2.6](https://github.com/magento/magento-cloud/blob/2.2.6/.gitignore) come riferimento.

1. Cancella la cache Git.

   ```bash
   git rm -r --cached .
   ```

1. Aggiungere e confermare le modifiche.

   ```bash
   git add -A && git commit -m "Update base template"
   ```

{{redeploy-warning}}
