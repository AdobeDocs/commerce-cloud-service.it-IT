---
source-git-commit: 2d902a3926c6bbc6a9dc8afcbd667eddeaf3be7e
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---
# Includi file per modificare VCL personalizzato per Fastly

## Modificare lo snippet VCL personalizzato

1. [Accedi](/help/get-started/onboarding.md#access-your-admin-panel) all&#39;amministratore.

1. Fai clic su **Archivi** > **Impostazioni** > **Configurazione** > **Avanzate** > **Sistema**.

1. Espandi **Cache a pagina intera** > **Configurazione rapida** > **Snippet VCL personalizzati**.

   ![Gestione snippet VCL personalizzati](/help/assets/cdn/fastly-manage-snippets.png)

1. Nella colonna _Azione_ fare clic sull&#39;icona delle impostazioni accanto al frammento da modificare.

1. Dopo il ricaricamento della pagina, fai clic su **Carica VCL in Fastly** nella sezione _Fastly Configuration_.

1. Al termine del caricamento, aggiorna la cache in base alla notifica nella parte superiore della pagina.

>[!WARNING]
>
>L&#39;opzione dell&#39;interfaccia utente _Snippet VCL personalizzati_ mostra solo i frammenti aggiunti tramite l&#39;amministratore Adobe Commerce. Se aggiungi snippet utilizzando l&#39;API Fastly, utilizza l&#39;API per [gestirli](/help/cloud-guide/cdn/fastly-vcl-custom-snippets.md#manage-custom-vcl-snippets-using-the-api).
