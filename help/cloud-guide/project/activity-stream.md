---
title: Flusso di attività
description: Scopri come leggere il flusso di attività in  [!DNL Cloud Console]  o Cloud CLI per l’infrastruttura Adobe Commerce on Cloud.
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: ffef5ab4-ef40-4073-adc8-a44c61c0d77b
source-git-commit: 85ff1283f773823ff2c6e6ab8f391fd5b4aa00e4
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# Flusso di attività

La visualizzazione principale di ciascun ambiente visualizza un elenco di **Attività** di eventi storici simile a un registro Git. L’elenco Attività è un flusso degli eventi recenti per gli ambienti attivi. Di seguito è riportato un elenco dei tipi di attività e delle relative icone visualizzate nel flusso Attività:

![Tipi di attività](../../assets/activity-types.svg){width="500" align="center"}

## Visualizzare i registri

Nell’elenco Attività fai clic sull’icona di stato di un’attività per visualizzare il registro. In alternativa, fare clic sul menu ![Altro](../../assets/icon-more.png){width="32"} (_altro_) per accedere a ulteriori opzioni per la gestione dell&#39;attività. Di seguito è riportato un breve registro che crea un backup. È possibile [utilizzare Cloud CLI](#activity-stream-with-cloud-cli) per visualizzare lo stesso registro.

![Visualizzazione registro](../../assets/log-view.png)

## Gestire un’attività

Alcune attività sono in stato _in esecuzione_ o _in sospeso_. Puoi agire su un’attività in esecuzione, ad esempio annullando una distribuzione in esecuzione. Le schede seguenti mostrano due metodi per annullare un&#39;attività: [!DNL Cloud Console] o Cloud CLI.

>[!BEGINTABS]

>[!TAB Console]

**Per annullare un&#39;attività in[!DNL Cloud Console]**:

È possibile agire su un&#39;attività in esecuzione accedendo al menu ![Altro](../../assets/icon-more.png){width="32"} (_altro_) e selezionando un&#39;azione, ad esempio `Cancel` o `View log`. Per questo esempio, selezionare l&#39;opzione **Annulla** per interrompere l&#39;attività in esecuzione.

Non tutte le attività dispongono dell’opzione di annullamento. Ad esempio, l&#39;opzione per annullare la distribuzione dell&#39;applicazione viene visualizzata solo durante la fase _build_. Una volta spostata l&#39;applicazione nella fase _distribuzione_, non è più possibile annullare l&#39;attività. Consulta [Processo di distribuzione](../deploy/process.md) sulle diverse fasi.

![Annulla attività](../../assets/activity-icons/cancel-activity.png){width="450" align="center"}

Se un terminale esegue l&#39;attività di distribuzione, l&#39;annullamento in [!DNL Cloud Console] determina l&#39;annullamento nel terminale:

![Attività annullata nel terminale](../../assets/activity-icons/activity-cancelled.png){width="300"}

>[!TAB CLI]

**Per annullare un&#39;attività in Cloud CLI**:

1. Identifica le attività in esecuzione e seleziona un ID attività.

   ```bash
   magento-cloud activity:list --state=in_progress
   ```

1. Annulla l’attività utilizzando l’ID attività:

   ```bash
   magento-cloud activity:cancel wvl5wm7s5vkhy
   ```

>[!ENDTABS]

## Flusso attività filtro

La possibilità di filtrare l’elenco delle attività è utile quando cerchi qualcosa di specifico, ad esempio un backup o un evento di unione.

**Per filtrare l&#39;elenco attività in[!DNL Cloud Console]**:

1. Selezionare un ambiente e scegliere la visualizzazione Attività **[!UICONTROL All]** per includere la cronologia completa degli eventi.

1. Fai clic su ![Filtra per](../../assets/icon-filterby.png){width="32"} e seleziona le opzioni **[!UICONTROL Filter by]**:

   ![Filtra attività](../../assets/activity-filter.png)

1. Scegliere la visualizzazione Attività **[!UICONTROL Recent]** e reimpostare l&#39;elenco.

## Visualizza flusso con Cloud CLI

L&#39;interfaccia della riga di comando `magento-cloud` offre la maggior parte delle stesse funzionalità di [!DNL Cloud Console]. Il comando `activity` può:

- `list` il flusso di attività per un ambiente
- `get` dettagli su un&#39;attività specifica
- visualizza `log` per un&#39;attività specifica
- `cancel` attività

**Per visualizzare il flusso di attività con Cloud CLI**:

1. Elencare le attività per l’ambiente corrente.

   ```bash
   magento-cloud activity:list
   ```

1. Ogni attività ha un ID univoco. Seleziona un ID dall’elenco precedente e visualizza i dettagli di tale attività.

   ```bash
   magento-cloud activity:get wvl5wm7s5vkhy
   ```

1. Visualizza il registro completo dell’attività.

   ```bash
   magento-cloud activity:log wvl5wm7s5vkhy
   ```

   Risposta di esempio:

   ```bash
   Activity ID: wvl5wm7s5vkhy
   Type: environment.backup
   Description: User created a backup of Master
   Created: 2023-09-08T14:03:33+00:00
   State: complete
   Log:
   Creating backup of master
   Created backup eg5pu63egt2dcojkljalzjdopa
   ```
