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

Un aggiornamento al pacchetto `ece-tools` aggiorna anche l&#39;altra [suite di strumenti cloud per i pacchetti Commerce](../release-notes/cloud-tools-suite.md), che sono dipendenze per `ece-tools`. Pertanto, è necessario utilizzare una versione di Adobe Commerce sull&#39;infrastruttura cloud che supporti il pacchetto `ece-tools`.

{{ece-tools-package}}

**Prerequisiti**:

- Prima di aggiornare `ece-tools`, controlla le [note sulla versione della suite di strumenti cloud per Commerce](../release-notes/cloud-tools-suite.md).
- Se stai eseguendo l&#39;aggiornamento da `ece-tools` 2002.0.22 o versioni precedenti a 2002.1.0, rivedi [Modifiche non compatibili con le versioni precedenti](../release-notes/backward-incompatible-changes.md) e apporta le modifiche necessarie al progetto Adobe Commerce sull&#39;infrastruttura cloud.
- Rivedi [Aggiornamenti e patch](../development/commerce-version.md#upgrade-from-older-versions) per determinare le versioni ECE-Tools compatibili con il tuo progetto di infrastruttura cloud Adobe Commerce.

{{upgrade-tip}}

**Per aggiornare il pacchetto `ece-tools`**:

1. Sulla workstation locale, esegui un aggiornamento utilizzando Compositore.

   ```bash
   composer update magento/ece-tools --with-dependencies
   ```

   >[!NOTE]
   >
   >Se non è possibile eseguire l&#39;aggiornamento oltre la versione 2002.0.8 di `ece-tools`, vedere [Aggiornare il progetto per l&#39;utilizzo del pacchetto ECE-Tools](install-package.md).

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
