---
title: Applicare le patch
description: Scopri come applicare le patch nel progetto Adobe Commerce su infrastruttura cloud.
feature: Cloud, Upgrade
exl-id: a7bf672f-7b89-45cd-8436-e885bca9029d
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '856'
ht-degree: 0%

---

# Applicare le patch

Le [patch cloud per Commerce](https://github.com/magento/magento-cloud-patches) e lo [strumento per patch di qualità](https://github.com/magento/quality-patches) distribuiscono le patch nell&#39;applicazione Adobe Commerce installata.

- Il pacchetto Patch cloud per Commerce fornisce le patch necessarie con correzioni critiche
- Le patch di qualità forniscono correzioni di qualità facoltative a basso impatto come [singole patch](https://experienceleague.adobe.com/docs/commerce-operations/release/planning/versioning-policy.html#individual-patch) che non contengono modifiche non compatibili con le versioni precedenti

Per un elenco completo delle patch rilasciate, vedere [Patch disponibili](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html) nella _Guida agli strumenti operativi di Commerce_.

Entrambi i pacchetti migliorano l’integrazione di tutte le versioni di Adobe Commerce con gli ambienti Cloud e supportano la distribuzione rapida di correzioni critiche, opzionali e personalizzate. È possibile utilizzare questi pacchetti per applicare, ripristinare e visualizzare informazioni generali su tutte le singole patch disponibili per Commerce.

>[!TIP]
>
>Puoi utilizzare lo [strumento Patch di qualità](https://experienceleague.adobe.com/tools/commerce-quality-patches/index.html) e Patch cloud per Commerce come pacchetti autonomi per i progetti Magento Open Source e Adobe Commerce. È consigliabile utilizzare lo strumento Patch di qualità per i progetti non cloud.

Quando si distribuiscono le modifiche all&#39;ambiente remoto, il pacchetto `ece-tools` utilizza `magento/magento-cloud-patches` e `magento/quality-patches` per verificare la presenza di patch in sospeso e le applica automaticamente nell&#39;ordine seguente:

1. Applica tutte le patch Commerce richieste incluse nel pacchetto Patch cloud per Commerce.
1. Applicare le patch Commerce opzionali selezionate incluse nello strumento Patch di qualità.
1. Applicare le patch personalizzate nella directory `/m2-hotfixes` in ordine alfabetico in base al nome della patch.

>[!NOTE]
>
>Quando si aggiorna il pacchetto `ece-tools` o il pacchetto Patch cloud per Commerce, le ultime patch richieste vengono applicate alla successiva distribuzione del progetto oppure è possibile distribuirle immediatamente utilizzando il comando CLI `ece-patches apply` e ridistribuendo l&#39;ambiente Cloud. Impossibile ignorare [patch richieste](https://github.com/magento/magento-cloud-patches/tree/develop/patches) durante il processo di distribuzione.

## Prerequisiti

{{upgrade-tip}}

Lo strumento Quality Patches è una dipendenza per le patch cloud per Commerce e il pacchetto `ece-tools`. Per applicare le ultime patch, è necessario che sia installata la [versione più recente di ECE-Tools](../dev-tools/update-package.md). La versione minima richiesta di ECE-Tools è la 2002.1.2.

## Visualizza patch e stato disponibili

Per visualizzare l&#39;elenco delle singole patch disponibili:

```bash
php ./vendor/bin/ece-patches status
```

Risposta di esempio:

```
More detailed information about patches you can find on https://support.magento.com/
╔════════════════╤═════════════════════════════════════════════════╤══════════╤═════════════╤═════════════════════════════════╗
║ Id             │ Title                                           │ Type     │ Status      │ Details                         ║
╠════════════════╪═════════════════════════════════════════════════╪══════════╪═════════════╪═════════════════════════════════╣
║ MAGECLOUD-5069 │ FPC is getting disabled during deployments      │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-page-cache    ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5650    │ Hold deployment config after reading from file  │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MCLOUD-5684    │ Pagination Not working - product_list_limit=all │ Required │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-elasticsearch ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-65837       │ Fix load balancer issue                         │Deprecated│ Applied     │ Recommended replacement: MC-1   ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/framework            ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ BUNDLE-2554    │ Set Payment info bug                            │ Required │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - amzn/amazon-pay-module       ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-1           │ Fixes issue 1                                   │ Optional │ Applied     │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-2           │ Fixes issue 2                                   │ Optional │ Not applied │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ MC-3           │ Fixes issue 3                                   │ Optional │ Not applied │ Required patches:               ║
║                │                                                 │          │             │  - MC-2                         ║
║                │                                                 │          │             │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-cms           ║
╟────────────────┼─────────────────────────────────────────────────┼──────────┼─────────────┼─────────────────────────────────╢
║ N/A            │ ../m2-hotfixes/MDVA_custom__2.3.5_ce.patch      │ Custom   │ N/A         │ Affected components:            ║
║                │                                                 │          │             │  - magento/module-framework     ║
╚════════════════╧═════════════════════════════════════════════════╧══════════╧═════════════╧═════════════════════════════════╝
Magento 2 Enterprise Edition, version 2.3.5.0
```

La tabella di stato contiene i seguenti tipi di informazioni:

- **Tipo**:
   - `Optional` - Tutte le patch dello strumento Quality Patches e del pacchetto Cloud Patches sono opzionali per le installazioni di Adobe Commerce e di Magento Open Source. Per l’infrastruttura cloud di Adobe Commerce, tutte le patch sono opzionali.
   - `Required` - Tutte le patch del pacchetto Patch cloud per Commerce sono necessarie per i clienti Cloud.
   - `Deprecated` - La singola patch è contrassegnata come obsoleta. Si consiglia di ripristinarla se è stata applicata. Dopo aver ripristinato una patch obsoleta, questa non verrà più visualizzata nella tabella di stato.
   - `Custom` - Tutte le patch dalla directory &#39;m2-hotfixes&#39;.

- **Stato**:
   - `Applied` - La patch è stata applicata.
   - `Not applied` - La patch non è stata applicata.
   - `N/A` - Impossibile definire lo stato della patch a causa di conflitti.

- **Dettagli**:
   - `Affected components` - Elenco dei moduli interessati.
   - `Required patches` - Elenco delle patch richieste (dipendenze).
   - `Recommended replacement`—La patch che si consiglia di sostituire con una patch obsoleta.

## Applicare una patch in un ambiente locale

È possibile applicare le patch manualmente in un ambiente locale e testarle prima della distribuzione.

**Per applicare singole patch in un ambiente di sviluppo locale**:

1. Aggiungere la variabile &#39;QUALITY_PATCH&#39; al file `.magento.env.yaml` ed elencare le patch richieste al di sotto di esso.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

1. Dalla directory principale del progetto, applica le patch.

   ```bash
   php ./vendor/bin/ece-patches apply
   ```

   Il comando `ece-patches apply` applica le patch nell&#39;ordine seguente:
   - Patch richieste
   - Singole patch facoltative
   - Patch personalizzate dalla directory `/m2-hotfixes`

1. Cancella la cache.

   ```bash
   php ./bin/magento cache:clean
   ```

1. Testare le patch e apportare le modifiche necessarie alle patch personalizzate.

## Applicare una patch in un ambiente remoto

>[!WARNING]
>
>Si consiglia vivamente di sottoporre a test tutte le patch in un ambiente di integrazione o di staging prima di implementarle nell&#39;ambiente di produzione.

**Per applicare le patch in un ambiente remoto**:

1. Aggiungere la variabile `QUALITY_PATCHES` al file `.magento.env.yaml` ed elencare le patch richieste al di sotto.

   ```yaml
   stage:
     build:
       QUALITY_PATCHES:
         - MCTEST-1002
         - MCTEST-1003
   ```

   >[!NOTE]
   >
   >Dopo l&#39;aggiornamento a una nuova versione di Adobe Commerce, è necessario riapplicare le patch se non sono incluse nella nuova versione.

1. Aggiungere, eseguire il commit e inviare il file `.magento.env.yaml` aggiornato.

   ```bash
   git add .magento.env.yaml
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

## Applicare una patch personalizzata

Durante la distribuzione, ECE-Tools applica tutte le patch di Adobe ed eventuali patch personalizzate aggiunte alla directory `/m2-hotfixes` nella directory principale del progetto.

>[!NOTE]
>
>Tutti i nomi dei file patch devono terminare con l&#39;estensione `.patch`.

**Per applicare e testare una patch personalizzata in un ambiente cloud**:

1. Nella directory principale del progetto, creare una directory denominata `m2-hotfixes` se non esiste

   ```bash
   mkdir m2-hotfixes
   ```

1. Copiare il file patch nella directory `/m2-hotfixes`.

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Apply patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Assicurarsi di sottoporre a test tutte le patch in un ambiente di pre-produzione. Per Adobe Commerce sull&#39;infrastruttura cloud, è possibile creare rami con il comando CLI `magento-cloud environment:branch <branch-name>`.

## Ripristinare una patch personalizzata

Per ripristinare o disinstallare una patch personalizzata precedentemente applicata:

1. Eliminare il file patch dalla directory `/m2-hotfixes`.

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add m2-hotfixes/
   ```

   ```bash
   git commit -m "Revert patch"
   ```

   ```bash
   git push origin <branch-name>
   ```

   >[!NOTE]
   >
   >Assicurati di eseguire il test in un ambiente di pre-produzione. Per Adobe Commerce sull&#39;infrastruttura cloud, è possibile creare rami con il comando CLI `magento-cloud environment:branch <branch-name>`.

## Applicare patch a un progetto non Cloud

Utilizza lo strumento [Patch di qualità](https://github.com/magento/quality-patches) per i progetti Magento Open Source e Adobe Commerce.

## Ripristinare una patch in un ambiente locale

È possibile ripristinare tutte le patch applicate in precedenza in un ambiente di sviluppo locale utilizzando l&#39;interfaccia CLI `ece-patches`.

Per ripristinare tutte le patch applicate:

```bash
php ./vendor/bin/ece-patches revert
```

Questo comando ripristina tutte le patch nell&#39;ordine seguente:

- Ripristina tutte le patch personalizzate applicate dalla directory /m2-hotfixes.
- Ripristina tutte le singole patch facoltative applicate.
- Ripristina tutte le patch richieste applicate.

## Registrazione

Lo strumento Quality Patches registra tutte le operazioni nel file `<Project_root>/var/log/patch.log`.
