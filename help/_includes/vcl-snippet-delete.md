---
source-git-commit: 8f1ed3067f6daed897151052c8b9f987d3df3a50
workflow-type: tm+mt
source-wordcount: '93'
ht-degree: 0%

---
# Includi file per eliminare VCL personalizzato per Fastly

## Elimina lo snippet VCL personalizzato

1. [Accedi](/help/get-started/onboarding.md#access-your-admin-panel) all&#39;amministratore.

1. Fai clic su **Archivi** > **Impostazioni** > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandi **Cache a pagina intera** > **Configurazione rapida** > **Snippet VCL personalizzati**.

   ![Gestione snippet VCL personalizzati](/help/assets/cdn/fastly-manage-snippets.png)

1. Nella colonna _Azione_, fai clic sull&#39;icona del cestino accanto allo snippet da eliminare.

1. Nella finestra modale successiva, fai clic su **DELETE** e attiva una nuova versione.

>[!WARNING]
>
>L&#39;opzione dell&#39;interfaccia utente _Snippet VCL personalizzati_ mostra solo i frammenti aggiunti tramite l&#39;amministratore Adobe Commerce. Se aggiungi snippet utilizzando l&#39;API Fastly, utilizza l&#39;API per [gestirli](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-vcl-using-the-api).
