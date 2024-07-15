---
title: Panoramica sullo sviluppo
description: Preparati allo sviluppo locale con un progetto Adobe Commerce su infrastruttura cloud.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: d4452d7d-d3dc-4f8d-8bd7-76f05d89f545
source-git-commit: 99272d08a11f850a79e8e24857b7072d1946f374
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# Panoramica sullo sviluppo

Gli ambienti remoti dell&#39;infrastruttura cloud Adobe Commerce sono **di sola lettura**, inclusi tutti gli ambienti Starter e tutti gli ambienti di integrazione, staging e produzione Pro. In un ambiente di sviluppo locale, puoi scrivere e testare il codice prima di inviarlo a un ambiente di integrazione per ulteriori test e distribuirlo a Staging e Produzione.

Prima di preparare l&#39;area di lavoro locale, verificare di disporre delle [credenziali](../../get-started/prepare-workspace.md). Lo sviluppo locale richiede l&#39;installazione di PHP e Composer, a meno che non si decida di utilizzare [Cloud Docker per Commerce](#docker-environment).

## Pacchetti richiesti

Adobe Commerce su infrastruttura cloud utilizza Composer per gestire le dipendenze e gli aggiornamenti per i progetti. Per lo sviluppo locale, è necessario installare le versioni PHP e Composer compatibili con il progetto Cloud. Se ad esempio si utilizza il modello cloud [!DNL Commerce] 2.4.7, è possibile notare che il file di configurazione [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.7/.magento.app.yaml) utilizza **PHP 8.3** e **Composer 2.7.2**.

Composer installa le librerie e le dipendenze richieste per il progetto nella directory `vendor`. I seguenti file Composer richiesti si trovano nella directory principale del progetto:

- `composer.json`: utilizzare il file `composer.json` per gestire le installazioni e gli aggiornamenti del prodotto.
- `composer.lock` - Il file `composer.lock` memorizza un insieme di dipendenze di versione esatte che soddisfano i vincoli di versione di ogni requisito per ogni pacchetto nell&#39;albero delle dipendenze del progetto.

**Comandi comuni:**

| Comando | Descrizione |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | Aggiornamenti alle versioni più recenti delle dipendenze riportate nel file `composer.json`. Il file `composer.lock` verrà aggiornato. |
| `composer install` | Legge il file `composer.lock` per scaricare le dipendenze. È consigliabile mantenere una copia aggiornata di `composer.lock` nell&#39;archivio dei progetti. |

{style="table-layout:auto"}

Dopo aver aggiunto, confermato e inviato il codice aggiornato, il processo di distribuzione esegue automaticamente il comando `composer install` durante la [fase di compilazione](../deploy/process.md#build-phase-build-phase).

### Metapackage cloud

Adobe Commerce sull&#39;infrastruttura cloud utilizza un metapacchetto che richiede `magento/product-enterprise-edition`. Per ottenere gli ultimi aggiornamenti per l’ultima versione di Commerce, utilizza la seguente sintassi di vincolo:

```text
>=current_version <next_version
```

Ad esempio, per utilizzare la versione più recente di Adobe Commerce 2.4.7, impostare `2.4.7` come versione &quot;corrente&quot; e `2.4.8` come versione &quot;successiva&quot; nel file `composer.json`:

```text
"magento/magento-cloud-metapackage": ">=2.4.7 <2.4.8"
```

I pacchetti principali di questo metapackage sono i seguenti:

- **vendor/magento/ece-tools** - Il pacchetto `ece-tools` è compatibile con Adobe Commerce versione 2.1.4 e successive per fornire un set completo di funzionalità che è possibile utilizzare per gestire il progetto Adobe Commerce on cloud infrastructure. Contiene script e comandi di Adobe Commerce on cloud infrastructure progettati per facilitare la gestione del codice e la creazione e distribuzione automatica dei progetti. Vedere la panoramica del pacchetto [`ece-tools`](../dev-tools/package-overview.md).
- **vendor/magento/product-enterprise-edition**: questo metapackage richiede componenti dell&#39;applicazione, inclusi moduli, framework, temi e altro ancora.
- **vendor/fastly2/magento2**: questo modulo gestisce la rete CDN e i servizi Fastly per gli ambienti di staging e produzione Pro e di produzione Starter. Vedi [Servizi Fastly](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **vendor/magento/module-paypal-on-boarding**—Questo modulo fornisce il pagamento tramite checkout del gateway PayPal tramite la connessione al tuo conto PayPal. Consulta [Strumento di registrazione PayPal](../store/paypal.md).

>[!TIP]
>
>Per un elenco delle dipendenze e delle licenze di terze parti, consulta [Pacchetti cloud per Adobe Commerce](/help/cloud-guide/release-notes/cloud-packages.md) nelle _Note sulla versione di Commerce_.

## Ambiente Docker

Puoi utilizzare lo strumento Cloud Docker for Commerce per emulare Adobe Commerce sugli ambienti di produzione e sviluppo dell’infrastruttura cloud per lo sviluppo locale. Cloud Docker per Commerce non richiede che PHP e Composer siano installati localmente.

- [Sviluppo locale con Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/) nel sito Adobe Developer
- [Architettura Docker e comandi comuni](../dev-tools/cloud-docker.md)
- [Note sulla versione di Cloud Docker](../release-notes/cloud-docker.md)

>[!TIP]
>
>Per informazioni sull&#39;utilizzo dei servizi di hosting basati su Git con Adobe Commerce sull&#39;infrastruttura cloud, consulta [Integrazioni](../integrations/overview.md).
