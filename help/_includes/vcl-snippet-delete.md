---
source-git-commit: 8f1ed3067f6daed897151052c8b9f987d3df3a50
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# Includi file per eliminare VCL personalizzato per Fastly

## Elimina lo snippet VCL personalizzato

1. [Accedi](/help/get-started/onboarding.md#access-your-admin-panel) all’amministratore.

1. Clic **Negozi** > **Impostazioni** > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandi **Cache a pagina intera** > **Configurazione rapida** > **Snippet VCL personalizzati**.

   ![Gestire snippet VCL personalizzati](/help/assets/cdn/fastly-manage-snippets.png)

1. In _Azione_ fare clic sull&#39;icona cestino accanto al frammento da eliminare.

1. Nella finestra modale successiva, fai clic su **DELETE** e attiva una nuova versione.

>[!WARNING]
>
>Il _Snippet VCL personalizzati_ L’opzione dell’interfaccia utente mostra solo i frammenti aggiunti tramite l’amministratore Adobe Commerce. Se aggiungi snippet utilizzando l’API Fastly, utilizza l’API per [gestirli](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api).
