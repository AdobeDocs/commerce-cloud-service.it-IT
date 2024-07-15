---
title: Ripristino da guasto componente
description: Scopri come ripristinare se un componente non viene distribuito correttamente in Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Deploy
exl-id: 4855be0c-6883-4ab1-a364-316d10e97250
source-git-commit: b44d97f82ef09288807c648010202422c9ac04eb
workflow-type: tm+mt
source-wordcount: '220'
ht-degree: 0%

---

# Ripristino da guasto componente

In questo argomento viene illustrato come ripristinare se un componente non viene distribuito correttamente. Esempi tipici includono componenti che hanno dipendenze non soddisfatte dall’ambiente remoto, come le versioni PHP incompatibili.

È possibile eseguire il ripristino da una distribuzione non riuscita in uno dei modi seguenti:

- [Ripristinare un backup](../storage/snapshots.md#restore-a-snapshot)
- Rimuovi progetto e codice da modifiche precedenti e ridistribuiscili

## Pulizia, rimozione e ridistribuzione

Per eseguire la pulizia dalla distribuzione precedente, identifica il componente aggiunto o aggiornato e quindi rimuovilo. Eseguire innanzitutto l&#39;accesso all&#39;ambiente remoto e cancellare manualmente il contenuto della directory `var`. Quindi rimuovere il componente dal file `composer.json` e ridistribuire l&#39;ambiente.

**Per pulire le directory `var`**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Cancella le directory `var`.

   ```shell
   rm -rf var/*
   ```

1. Disconnetti.

**Per rimuovere il componente**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Cancella la cache.

   ```bash
   composer clear-cache
   ```

1. Rimuovere il componente dal file `composer.json`.

   ```bash
   composer remove <component-name>:<version>
   ```

   Se viene visualizzato il seguente messaggio, non è necessario eseguire ulteriori operazioni:

   ```terminal
   Package "<name>:<version>" listed for update is not installed. Ignoring.
   ```

1. Aggiornamento delle dipendenze in corso. Attendi.

1. Aggiungi, conferma e invia modifiche al codice.

   ```bash
   git add -A
   ```

   ```bash
   git commit -m "<message>"
   ```

   ```bash
   git push origin <environment-ID>
   ```

{{redeploy-warning}}

Ulteriori informazioni sul ripristino di un ambiente senza backup in [Ripristinare un ambiente](../development/restore-environment.md).

{{stuck-deployment-tip}}
