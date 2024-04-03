---
title: Aggiornare il pacchetto ECE-Strumenti
description: Scopri come aggiornare il pacchetto ECE-Tools per sfruttare le correzioni e le funzioni più recenti applicate ad Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Upgrade
exl-id: 7cce45eb-ae53-4468-b16d-4f4d3422ac52
source-git-commit: 513bc5b52f046ffd98005d80f34725b7f60b38bd
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 0%

---

# Aggiornare il pacchetto ECE-Strumenti

Un aggiornamento della sezione `ece-tools` il pacchetto aggiorna anche l’altro [Pacchetti della suite di strumenti cloud per commerce](../release-notes/cloud-tools-suite.md), che sono dipendenze per `ece-tools`. Pertanto, devi utilizzare una versione di Adobe Commerce sull’infrastruttura cloud che supporti `ece-tools` pacchetto.

{{ece-tools-package}}

**Prerequisiti**:

- Prima dell’aggiornamento `ece-tools`, rivedi [Note sulla versione di Cloud Tools Suite per Commerce](../release-notes/cloud-tools-suite.md).
- Se stai eseguendo l’aggiornamento da `ece-tools` 2002.0.22 o precedenti al 2002.1.0, revisione [Modifiche non compatibili con le versioni precedenti](../release-notes/backward-incompatible-changes.md) e apporta le modifiche necessarie al progetto Adobe Commerce on cloud infrastructure.
- Revisione [Aggiornamenti e patch](../development/commerce-version.md#upgrade-from-older-versions) per determinare le versioni ECE-Tools compatibili con il progetto di infrastruttura cloud Adobe Commerce on.

{{upgrade-tip}}

**Per aggiornare `ece-tools` pacchetto**:

1. Sulla workstation locale, esegui un aggiornamento utilizzando Compositore.

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >Se non è possibile eseguire l’aggiornamento oltre `ece-tools` versione 2002.0.8, vedere [Aggiornamento del progetto per l’utilizzo del pacchetto ECE-Tools](install-package.md).

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "Update magento/ece-tools"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Dopo la convalida del test, unisci questo ramo al ramo di integrazione.
