---
title: Accedi a  [!DNL Cloud Console]
description: Scopri  [!DNL Cloud Console]  per l’infrastruttura Adobe Commerce su Cloud.
recommendations: noDisplay, catalog
last-substantial-update: 2024-02-06T00:00:00Z
exl-id: c19a36b6-e5e8-461c-a82c-68b7bf121999
source-git-commit: abe9aa36b907be8bdfdf42e6f28f1e1eac68fecf
workflow-type: tm+mt
source-wordcount: '389'
ht-degree: 0%

---


# Accedi a [!DNL Cloud Console]

[!DNL Cloud Console] fornisce metodi interattivi per generare, gestire e distribuire il codice Commerce. [!DNL Cloud Console] è un&#39;esperienza più moderna e intuitiva e getta le basi per futuri miglioramenti dell&#39;interfaccia.

[Accedi a [!DNL Cloud Console]](https://console.adobecommerce.com) per visualizzare l&#39;elenco dei progetti.

![Elenco progetti](../assets/ui-allprojects-list.png)

## Funzioni

Le funzioni nuove o migliorate includono:

- Panoramica chiara delle caratteristiche del progetto e dell’ambiente
- Flusso di attività con cronologia ordinabile
- Gestione manuale dei backup e cronologia dei progetti iniziali
- Visualizzazioni di registro migliorate
- Elenchi ordinabili
- Moduli semplici e indicazioni per aggiungere integrazioni
- Conformità alle linee guida per l’accessibilità dei contenuti web (WCAG)

![[!DNL Cloud Console]](../assets/CloudConsole.svg)

Le funzioni nuove o migliorate sono le seguenti:

| Funzionalità | Migliori |
| -------------- | ----------------------------------- |
| [Flusso attività](../cloud-guide/project/activity-stream.md) | Interagisci con un elenco ordinabile di azioni in esecuzione, in sospeso o storiche. Seleziona un’attività e visualizza i registri o annulla una build in esecuzione. |
| [Panoramica dei progetti e dell&#39;ambiente](../cloud-guide/project/overview.md#project-overview) | Apri il progetto e consulta la panoramica dei dettagli del progetto e l’elenco degli ambienti. La panoramica dell’ambiente fornisce ulteriori dettagli sullo stato dell’ambiente, sull’accesso alle applicazioni e sulle attività recenti. |
| [Moduli di integrazione](../cloud-guide/integrations/overview.md) | Utilizza semplici moduli e indicazioni per aggiungere integrazioni, ad esempio notifiche Bitbucket o di Slack. |
| [Elenco progetti](../cloud-guide/project/overview.md#cloud-console) | Nella visualizzazione _Tutti i progetti_ sono elencati tutti i progetti per i quali si dispone dell&#39;autorizzazione di accesso. È possibile fare clic su **[!UICONTROL Show filters]** e filtrare l&#39;elenco dei progetti per tipo, area geografica o piano. |
| [Opzioni di visibilità variabili](../cloud-guide/environment/variable-levels.md) | Limita la visibilità di una variabile a livello di progetto o di ambiente durante la generazione o il runtime. |

<!-- The following are features yet to be activated:
| **Apps and services topology** | The Apps & Services topology is visible on Project and Environment views. This interactive diagram allows you to select a service and view the relationship details, such as name, type, version, port, and more. Click **[!UICONTROL View details]** to access the overview and configuration panel for each service. | -->

## Domande della console

**_Dove si trova la funzionalità Snapshot_**?

Per [!DNL Starter] progetti, la funzionalità Snapshot è ora denominata _Backup_. È possibile creare un backup manuale dell&#39;ambiente [!DNL Starter] da [!DNL Cloud Console] o creare uno snapshot da Cloud CLI. È necessario disporre di un ruolo di amministratore per l’ambiente.

Seleziona un ambiente dalla barra di navigazione del progetto. L’ambiente deve essere attivo. Selezionare la scheda **[!UICONTROL Backups]**. Attualmente, questa opzione non è disponibile per gli ambienti Pro.

**_Dove si trova l&#39;elenco delle route configurate per l&#39;ambiente_**?

L&#39;elenco delle route configurate è disponibile nella scheda _Servizi_ per un ambiente.

Seleziona un ambiente dalla barra di navigazione del progetto. Selezionare la scheda **[!UICONTROL Services]**. Nella panoramica di **Router** sono visualizzate le route configurate. Attualmente non è possibile aggiungere una route dal nuovo [!DNL Cloud Console].

## Menu Account

Nell’angolo in alto a destra è presente il menu dell’account. Fare clic sulla freccia rivolta verso il basso del menu e selezionare **[!UICONTROL My Profile]**. Nella visualizzazione _Il mio profilo_, puoi controllare i dettagli utente e le impostazioni di visualizzazione, gestire [l&#39;autenticazione di sicurezza](../cloud-guide/project/user-access.md#user-authentication-requirements), [i token API](../cloud-guide/project/user-access.md#create-an-api-token) e [le chiavi SSH](../cloud-guide/development/secure-connections.md).
