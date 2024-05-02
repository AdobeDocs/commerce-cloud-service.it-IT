---
title: Configurare le e-mail in uscita
description: Scopri come abilitare le e-mail in uscita per Adobe Commerce sull’infrastruttura cloud.
exl-id: 814fe2a9-15bf-4bcb-a8de-ae288fd7f284
source-git-commit: 59f82d891bb7b1953c1e19b4c1d0a272defb89c1
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 0%

---

# Configurare le e-mail in uscita

Puoi abilitare e disabilitare le e-mail in uscita per ogni ambiente dalla sezione [!DNL Cloud Console] o dalla riga di comando. Abilita le e-mail in uscita per gli ambienti di integrazione e staging per inviare e-mail di autenticazione a due fattori o reimpostare le password per gli utenti del progetto Cloud.

Per impostazione predefinita, le e-mail in uscita sono abilitate negli ambienti di produzione e staging. Tuttavia, [!UICONTROL Enable outgoing emails] potrebbe apparire disabilitato nelle impostazioni dell&#39;ambiente fino a quando non si imposta `enable_smtp` proprietà tramite [riga di comando](#enable-emails-in-the-cli) o [Console cloud](outgoing-emails.md#enable-emails-in-the-cloud-console).

Aggiornamento di [!UICONTROL enable_smtp] valore proprietà per [riga di comando](#enable-emails-in-the-cli) modifica anche il [!UICONTROL Enable outgoing emails] impostazione del valore per questo ambiente nella console Cloud.

{{redeploy-warning}}

## Abilitare le e-mail nella console Cloud

Utilizza il **[!UICONTROL Outgoing emails]** attivare/disattivare _Configurare l’ambiente_ visualizzazione per abilitare o disabilitare il supporto e-mail.

Se le e-mail in uscita devono essere disabilitate o riabilitate negli ambienti di produzione o staging di Pro, puoi inviare una [Ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>Lo stato delle e-mail in uscita potrebbe non essere riportato per gli ambienti Pro nella console Cloud. Invece, utilizza [riga di comando](#enable-emails-in-the-cli) per abilitare e testare le e-mail in uscita.

**Per gestire il supporto e-mail da[!DNL Cloud Console]**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).
1. Seleziona un progetto da _Tutti i progetti_ elenco.
1. Nel dashboard Progetto, fai clic sull’icona di configurazione in alto a destra.
1. Clic **[!UICONTROL Environments]** e seleziona un ambiente specifico dall’elenco.
1. Per abilitare o disabilitare le e-mail in uscita, attiva/disattiva _Abilita e-mail in uscita_ **On** o **Disattivato**.

   ![Abilita configurazione e-mail in uscita](../../assets/outgoing-emails.png)

Dopo aver modificato l’impostazione, l’ambiente viene generato e distribuito con la nuova configurazione.

## Abilitare le e-mail in CLI

Puoi modificare la configurazione e-mail per un ambiente attivo utilizzando `magento-cloud` CLI `environment:info` comando per impostare `enable_smtp` proprietà. L’abilitazione di SMTP aggiorna il `MAGENTO_CLOUD_SMTP_HOST` variabile di ambiente con l’indirizzo IP dell’host SMTP per l’invio della posta.

**Per gestire il supporto e-mail dalla riga di comando**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Controlla l’impostazione dell’e-mail in uscita per l’ambiente.

   ```bash
   magento-cloud environment:info -e <environment-id> | grep enable_smtp
   ```

1. Modificare la configurazione del supporto e-mail impostando `enable_smtp` variabile di ambiente a `true` o `false`.

   ```bash
   magento-cloud environment:info --refresh -e <environment-id> enable_smtp true
   ```

   Attendi che l’ambiente venga generato e implementato.

1. Utilizza un SSH per accedere all’ambiente remoto.

1. Verifica che l’e-mail funzioni; invia un’e-mail di test a un indirizzo che puoi controllare.

   ```bash
   php -r 'mail("mail@example.com", "test message", "just testing", "From: tester@example.com");'
   ```

1. Verificare che l&#39;e-mail sia stata selezionata da SendGrid.

   ```bash
   grep mail@example.com /var/log/mail.log
   ```
