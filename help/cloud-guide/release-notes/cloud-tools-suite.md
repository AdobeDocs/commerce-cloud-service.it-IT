---
title: Note sulla versione della suite di strumenti cloud
description: Scopri gli ultimi miglioramenti alla suite di strumenti cloud per Adobe Commerce.
feature: Cloud, Release Notes
exl-id: 6a652e93-46a2-4590-97fc-fb5d114ece9a
source-git-commit: 40c0d4ca6b70d7cea988209e7e3c8b7e3cd3522e
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 1%

---

# Note sulla versione per la suite di strumenti Commerce Cloud

Queste informazioni sulla versione descrivono gli ultimi miglioramenti apportati ai pacchetti Cloud Tools Suite for Commerce progettati per distribuire e gestire installazioni e aggiornamenti di Adobe Commerce sulla piattaforma Cloud.

| Note sulla versione | Versione | Descrizione | Sorgente |
| ----------------- |-----------| ---------------------------------------- | --------------------------- |
| [`ece-tools` pacchetto](ece-tools-package.md) | 2002.1.18. | Un set di script e strumenti progettati per gestire e distribuire progetti Cloud | [`magento/ece-tools`](https://github.com/magento/ece-tools/tree/2002.1) |
| [Patch cloud per Commerce](cloud-patches.md) | 1.0.26 | Un set di patch che migliorano l’integrazione di tutte le versioni di Adobe Commerce con gli ambienti Cloud. Questo pacchetto include patch di Adobe Commerce e hotfix disponibili che vengono applicati quando utilizzi `ece-tools` da distribuire | [`magento/magento-cloud-patches`](https://github.com/magento/magento-cloud-patches/tree/1.0.1) |
| [Cloud Docker per Commerce](cloud-docker.md) | 1.3.7. | Funzionalità e file di configurazione per le immagini Docker per distribuire Adobe Commerce in un ambiente cloud locale | [`magento/magento-cloud-docker`](https://github.com/magento/magento-cloud-docker/tree/1.0) |
| [Componenti cloud di Commerce](cloud-components.md) | 1.0.14 | Funzionalità Adobe Commerce di base estesa per i siti distribuiti nell’infrastruttura Cloud | [`magento/magento-cloud-components`](https://github.com/magento/magento-cloud-components/tree/1.0.2) |

Quando si esegue l&#39;aggiornamento a ECE-Tools 2002.1.0 o versione successiva, si esegue automaticamente l&#39;aggiornamento alle versioni più recenti degli altri pacchetti, che rappresentano le dipendenze per `ece-tools` pacchetto. Consulta [Metapackage cloud](../development/overview.md#cloud-metapackage) per un elenco di dipendenze.
