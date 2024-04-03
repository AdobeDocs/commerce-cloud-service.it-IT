---
title: Panoramica sullo sviluppo
description: Preparati allo sviluppo locale con un progetto Adobe Commerce su infrastruttura cloud.
role: Developer
feature: Cloud, Install
topic: Development
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: d4452d7d-d3dc-4f8d-8bd7-76f05d89f545
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 0%

---

# Panoramica sullo sviluppo

Gli ambienti remoti dell’infrastruttura cloud di Adobe Commerce sono **Sola lettura**, inclusi tutti gli ambienti Starter e tutti gli ambienti Pro di integrazione, staging e produzione. In un ambiente di sviluppo locale, puoi scrivere e testare il codice prima di inviarlo a un ambiente di integrazione per ulteriori test e distribuirlo a Staging e Produzione.

Prima di preparare l’area di lavoro locale, assicurati di disporre dei [credenziali](../../get-started/prepare-workspace.md). Lo sviluppo locale richiede l&#39;installazione di PHP e Composer, a meno che non si decida di utilizzare [Cloud Docker per Commerce](#docker-environment).

## Pacchetti richiesti

Adobe Commerce su infrastruttura cloud utilizza Composer per gestire le dipendenze e gli aggiornamenti per i progetti. Per lo sviluppo locale, è necessario installare le versioni PHP e Composer compatibili con il progetto Cloud. Ad esempio, se utilizzi il [!DNL Commerce] 2.4.6 modello cloud, puoi notare che il [`.magento.app.yaml`](https://github.com/magento/magento-cloud/blob/2.4.6/.magento.app.yaml) il file di configurazione utilizza **PHP 8.2** e **Compositore 2.2.21**.

Il Compositore installa le librerie e le dipendenze richieste per il progetto in `vendor` directory. I seguenti file Composer richiesti si trovano nella directory principale del progetto:

- `composer.json`- Utilizza la `composer.json` file per gestire le installazioni e gli aggiornamenti del prodotto.
- `composer.lock`- La `composer.lock` file memorizza un set di dipendenze di versione esatte che soddisfano i vincoli di versione di ogni requisito per ogni pacchetto nell&#39;albero delle dipendenze del progetto.

**Comandi comuni:**

| Comando | Descrizione |
|--------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| `composer update` | Aggiornamenti alle versioni più recenti delle dipendenze riportate nel `composer.json` file. Questo aggiorna il `composer.lock` file. |
| `composer install` | Legge il `composer.lock` file per scaricare le dipendenze. È consigliabile conservare una copia aggiornata di `composer.lock` nell’archivio del progetto. |

{style="table-layout:auto"}

Dopo aver aggiunto, confermato e inviato il codice aggiornato, il processo di distribuzione esegue automaticamente `composer install` durante il [fase di build](../deploy/process.md#build-phase-build-phase).

### Metapackage cloud

Adobe Commerce sull’infrastruttura cloud utilizza un metapacchetto che richiede `magento/product-enterprise-edition`. Per ottenere gli ultimi aggiornamenti per l’ultima versione di Commerce, utilizza la seguente sintassi di vincolo:

```text
>=current_version <next_version
```

Ad esempio, per utilizzare la versione più recente di Adobe Commerce 2.4.5, imposta `2.4.5` come versione &quot;corrente&quot; e `2.4.6` come versione &quot;successiva&quot; in `composer.json` file:

```text
"magento/magento-cloud-metapackage": ">=2.4.5 <2.4.6"
```

I pacchetti principali di questo metapackage sono i seguenti:

- **vendor/magento/ece-tools**- La `ece-tools` il pacchetto è compatibile con Adobe Commerce versione 2.1.4 e successive per fornire un set completo di funzioni da utilizzare per gestire il progetto Adobe Commerce on cloud infrastructure. Contiene script e comandi di Adobe Commerce on cloud infrastructure progettati per facilitare la gestione del codice e la creazione e distribuzione automatica dei progetti. Consulta la [`ece-tools` panoramica del pacchetto](../dev-tools/package-overview.md).
- **vendor/magento/product-enterprise-edition**: questo metapackage richiede componenti applicativi, tra cui moduli, framework, temi e altro ancora.
- **vendor/fastly2/magento2**- Questo modulo gestisce la rete CDN e i servizi Fastly per gli ambienti Pro Staging and Production e Starter Production. Consulta [Servizi Fastly](/help/cloud-guide/cdn/fastly.md#fastly-cdn-module-for-magento-2).
- **vendor/magento/module-paypal-on-boarding**- Questo modulo fornisce il pagamento PayPal gateway checkout collegandosi al tuo conto PayPal commerciante. Consulta [Strumento PayPal On-Boarding](../store/paypal.md).

>[!TIP]
>
>Consulta [Pacchetti cloud per Adobe Commerce](/help/cloud-guide/release-notes/cloud-packages.md) nel _Note sulla versione di Commerce_ per un elenco delle dipendenze e delle licenze di terze parti.

## Ambiente Docker

Puoi utilizzare lo strumento Cloud Docker for Commerce per emulare Adobe Commerce sugli ambienti di produzione e sviluppo dell’infrastruttura cloud per lo sviluppo locale. Cloud Docker for Commerce non richiede che PHP e Composer siano installati localmente.

- [Sviluppo locale con Cloud Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/) nel sito Adobe Developer
- [Architettura Docker e comandi comuni](../dev-tools/cloud-docker.md)
- [Note sulla versione di Cloud Docker](../release-notes/cloud-docker.md)

>[!TIP]
>
>Per informazioni sull’utilizzo dei servizi di hosting basati su Git con Adobe Commerce sull’infrastruttura cloud, consulta [Integrazioni](../integrations/overview.md).
