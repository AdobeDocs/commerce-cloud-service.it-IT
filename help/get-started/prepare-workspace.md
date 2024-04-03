---
title: Prepararsi per lo sviluppo
description: Raccogli le credenziali e scopri gli strumenti disponibili per impostare un’area di lavoro di sviluppo da utilizzare con il progetto di infrastruttura cloud Commerce on.
recommendations: noDisplay, catalog
exl-id: 8f88161f-3580-453b-b977-2c6e3824cc02
source-git-commit: 85ff1283f773823ff2c6e6ab8f391fd5b4aa00e4
workflow-type: tm+mt
source-wordcount: '471'
ht-degree: 0%

---

# Prepararsi per lo sviluppo

Che tu sia un nuovo utente di Commerce o un proprietario di Commerce esistente che si sposta nell’infrastruttura cloud, segui questi passaggi per preparare un’area di lavoro di sviluppo per il progetto Cloud. Se hai già completato alcuni di questi passaggi o disponi di un ambiente di sviluppo Adobe Commerce esistente, controlla i seguenti per i risultati previsti e continua con il passaggio successivo. Alcune configurazioni e flussi di lavoro differiscono da una tipica installazione on-premise.

## Credenziali

Prima di impostare un’area di lavoro, raccogli le chiavi e l’accesso all’account seguenti:

- **Chiavi di autenticazione (chiavi compositore)**

  Le chiavi di autenticazione sono token di autenticazione di 32 caratteri che forniscono un accesso sicuro all’archivio del Compositore Adobe Commerce (`repo.magento.com`) e qualsiasi altro servizio Git necessario per lo sviluppo di applicazioni come GitHub. Il tuo account può avere più chiavi di autenticazione. Per la configurazione dell’area di lavoro, inizia con una chiave specifica per l’archivio del codice. Se non disponi di chiavi, contatta il proprietario del progetto o crea [chiavi di autenticazione](../cloud-guide/development/authentication-keys.md) te stesso.

- **Account progetto cloud**

  Il proprietario del progetto deve invitarti al progetto Adobe Commerce on Cloud Infrastructure. Quando si riceve l&#39;invito tramite posta elettronica, fare clic sul collegamento e seguire le istruzioni per creare l&#39;account. Consulta [Onboarding](onboarding.md).

- **Chiave di crittografia Adobe Commerce**

  Quando si importa solo un sistema esistente, acquisire la chiave di crittografia utilizzata per proteggere l&#39;accesso e i dati per il database. Per informazioni dettagliate su questa chiave, consulta [Risolvi i problemi relativi alla chiave di crittografia](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/resolve-issues-with-encryption-key.html)

## Strumenti per sviluppatori

- **Installare Cloud CLI**

  Installare `magento-cloud` CLI per gestire gli ambienti Cloud ed eseguire attività di automazione. Consulta [Cloud CLI](../cloud-guide/dev-tools/cloud-cli-overview.md) per le istruzioni di installazione.

- **Installare Docker per sviluppo e test locali**

  Facoltativamente, utilizza l’ambiente Docker per emulare l’infrastruttura cloud di Commerce `integration` ambiente per lo sviluppo locale. Esistono tre componenti essenziali: un modello di Adobe Commerce v2, Docker Compose e `ece-tools` pacchetto.

   - [Architettura Docker e comandi comuni](../cloud-guide/dev-tools/cloud-docker.md)
   - [Avvia ambiente di sviluppo Docker](https://developer.adobe.com/commerce/cloud-tools/docker/setup/)
   - [Pacchetto ECE-Strumenti](../cloud-guide/dev-tools/package-overview.md)

- **Integrare servizi basati su Git**

  Facoltativamente, integra un servizio di hosting basato su Git, come GitHub o GitLab, con Adobe Commerce sull’infrastruttura cloud. Consulta [Integrazioni](../cloud-guide/integrations/overview.md).

## Codice progetto

Una connessione sicura è essenziale per interagire con gli ambienti remoti. Per un nuovo progetto: [accedi a [!DNL Cloud Console]](https://console.adobecommerce.com) e fai clic su **[!UICONTROL No SSH key]**. Questa icona si trova a destra del campo del comando ed è visibile quando il progetto non contiene una chiave SSH. Consulta [Connessioni sicure](../cloud-guide/development/secure-connections.md#add-an-ssh-public-key-to-your-account).

**Per clonare la base di codice sulla workstation locale**:

1. In [[!DNL Cloud Console]](https://console.adobecommerce.com), fai clic su **[!UICONTROL code]** e seleziona la **[!UICONTROL Git]** scheda.

   ![Clona il codice](../assets/ui-git-code.png){width="450"}

1. Copia il `git clone ...` comando fornito.

1. In un terminale, crea e modifica la directory di lavoro.

1. Incolla ed esegui il comando `git clone ...` comando.

>[!TIP]
>
>Adobe esegue il provisioning dell’ambiente di progetto iniziale utilizzando un archivio di modelli che include istruzioni sui pacchetti per una versione specifica di Adobe Commerce. Rivedi [struttura del file di progetto](../cloud-guide/project/file-structure.md) e ulteriori informazioni su file di progetto e modelli cloud importanti.
