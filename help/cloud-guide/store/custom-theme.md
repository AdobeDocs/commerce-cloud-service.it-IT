---
title: Tema personalizzato
description: Scopri come installare un tema personalizzato con Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Themes
exl-id: f08134ab-daea-471d-a927-02531d36a809
source-git-commit: bb7a866b1896a8a43d01ad3f83dc655bcf383374
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 0%

---

# Tema personalizzato

Puoi installare uno o più temi da utilizzare per uno o tutti i tuoi store e siti nel progetto. I temi includono più file statici tra cui immagini, font, CSS, JavaScript, PHP e altro per progettare completamente i tuoi store. È possibile aggiungere il tema estraendone il codice nel file system oppure utilizzando Compositore.

## Installare manualmente un tema

Per installare manualmente un tema, è necessario che il codice del tema sia in un archivio compresso o in una struttura di directory simile alla seguente:

```text
<VendorName>
  ├── composer.json
      ├── etc
      │   └── view.xml
      ├── media
      ├── registration.php
      ├── theme.xml
      └── web
          ├── css
          │   └── source
          ├── fonts
          ├── images
          └── js
```

**Per installare manualmente un tema**:

1. Copiare il codice del tema in `<Project root dir>/app/design/frontend` per un tema vetrina o `<Project root dir>/app/design/adminhtml` per un tema amministratore. Verificare che la directory di primo livello sia `<VendorName>`. In caso contrario, il tema non viene installato correttamente.

   ```bash
   cp -r ExampleTheme <project-root>/app/design/frontend
   ```

1. Confermate il tema copiato nella posizione corretta.

   * Tema vetrina: `ls <project-root>/app/design/frontend`
   * Tema amministratore: `ls <project-root>/app/design/adminhtml`

   Di seguito è riportato un esempio:

   EsempioTema Adobe Commerce

1. Aggiungere e confermare i file.

   ```bash
   git add -A && git commit -m "Add theme"
   ```

1. Invia i file al ramo.

   ```bash
   git push origin <branch name>
   ```

1. Attendere il completamento della distribuzione.
1. Accedi all’amministratore.
1. Fai clic su **Contenuto** > Progettazione > **Temi**.

   Il tema viene visualizzato nel riquadro di destra.

## Installare un tema tramite Compositore

L’installazione di un tema con Composer è identica all’installazione di qualsiasi altra estensione con Composer. Per informazioni dettagliate, vedere [Installare, gestire e aggiornare i moduli](extensions.md).

Per installare un tema utilizzando Compositore:

1. Acquista il tema da Commerce Marketplace.
1. Ottieni il nome del Compositore del tema.
1. Passa alla directory principale di Adobe Commerce e immetti il comando:

   ```bash
   composer require <vendor>/<name>:<version>
   ```

   Ad esempio:

   ```bash
   composer require zero1/theme-fashionista-theme:1.0.0
   ```

1. Attendere l&#39;aggiornamento delle dipendenze.
1. Immettete i seguenti comandi:

   ```bash
   git add -A && git commit -m "Add theme"
   ```

   ```bash
   git push origin <branch name>
   ```

1. Accedi all’amministratore.
1. Fai clic su **Contenuto** > Progettazione > **Temi**.

   Il tema viene visualizzato nel riquadro di destra.

## Più temi

Quando si utilizzano più temi, ad esempio temi diversi per lingua, esaminare la variabile di ambiente `SCD_MATRIX` per personalizzare la distribuzione dei temi. Vedere le fasi [build](../environment/variables-build.md#scd_matrix) o [deploy](../environment/variables-deploy.md#scd_matrix) nella [configurazione ambiente](../environment/configure-env-yaml.md).
