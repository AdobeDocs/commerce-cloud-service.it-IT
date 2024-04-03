---
source-git-commit: b08443d937dfc18120daa0d6a1277b9c7bca67aa
workflow-type: tm+mt
source-wordcount: '829'
ht-degree: 0%

---
# Snippet cloud

## Avvertenza Elasticsearch {#elasticsearch-support}

>[!WARNING]
>
>Elasticsearch 7.11 e versioni successive non è supportato per Adobe Commerce sull’infrastruttura cloud. Le versioni di Adobe Commerce 2.3.7-p3, 2.4.3-p2 e 2.4.4 e successive supportano il servizio OpenSearch. Gli impianti locali continuano a sostenere l&#39;Elasticsearch.

## Integrazione avanzata {#enhanced-integration-envs}

>[!NOTE]
>
>I progetti eseguiti prima del 5 giugno 2020 disponevano di più ambienti di integrazione più piccoli. Se hai bisogno di un ambiente di integrazione più ampio per i test e lo sviluppo, richiedi un aggiornamento per gli ambienti di integrazione avanzata. Consulta la [Richiesta ambiente di integrazione](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html) articolo nel _Centro assistenza Adobe Commerce_ per i dettagli.

## Opzioni di unione {#merge-options}

Per impostazione predefinita, il processo di distribuzione sovrascrive tutte le impostazioni in `env.php` ; tuttavia, è possibile scegliere di unire uno o più valori per una configurazione di servizio senza sovrascrivere tutti i valori.

Imposta il `_merge` a una delle seguenti opzioni:

- `true`—**Unisci** i valori del servizio configurato con i valori delle variabili di ambiente.
- `false`—**Sovrascrivere** i valori del servizio configurato con i valori delle variabili di ambiente.

## Archivio privato {#private-repository}

>[!NOTE]
>
>L’Adobe consiglia vivamente di utilizzare un archivio privato per il progetto di infrastruttura Adobe Commerce on cloud per proteggere eventuali informazioni proprietarie o attività di sviluppo, come estensioni e configurazioni sensibili.

## Avvertenza self-service Pro {#pro-self-service-warning}

>[!WARNING]
>
>Alcuni **Progetti Pro** richiedere un ticket di supporto per aggiornare la configurazione del percorso in `routes.yaml` e la configurazione cron in `.magento.app.yaml` file. L’Adobe consiglia di aggiornare e testare i file di configurazione YAML in un ambiente di integrazione, quindi di distribuire le modifiche nell’ambiente di staging. Se le modifiche non vengono applicate ai siti di staging dopo la ridistribuzione e non sono presenti messaggi di errore correlati nel registro, **DEVE** [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) che descrive le modifiche di configurazione tentate. Includi nel ticket tutti i file di configurazione YAML aggiornati.

## Supporto dei servizi Pro {#pro-update-service}

>[!TIP]
>Per i progetti Pro, è necessario [invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per installare o aggiornare [servizi](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/services-yaml.html) in `Staging` e `Production` solo ambienti.
>
>Indica le modifiche del servizio necessarie, includi le `.magento.app.yaml` e `services.yaml` e indicare la versione PHP nel ticket. Per le modifiche self-service alle impostazioni di versione PHP, estensioni o ambiente, vedi [Impostazioni PHP](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/php-settings.html) in _Configurazione dell’applicazione_.
>
>Per le modifiche apportate a _live_ Ambiente di produzione (**Solo pro**), devi fornire un preavviso minimo di 48 ore per consentire al team dell’infrastruttura Cloud di eseguire il marshalling delle risorse e eseguire un aggiornamento sicuro.

## Backup Pro {#pro-backups}

>[!TIP]
>
>Negli ambienti di staging e produzione Pro, è necessario [invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per recuperare un backup specifico che rilevi la data, l’ora e il fuso orario nel ticket.
>
>L’Adobe fa **non** ripristinare qualsiasi ambiente da un backup automatico. Consulta [Ripristinare uno snapshot del database da Gestione temporanea o Produzione](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html) per assistenza nella scelta di un metodo per ripristinare uno snapshot di staging o produzione.

## Avviso di ridistribuzione {#redeploy-warning}

>[!WARNING]
>
>Il processo di distribuzione inizia quando si esegue un&#39;unione, un push o una sincronizzazione dell&#39;ambiente oppure quando si attiva una ridistribuzione manuale, durante la quale [!DNL Commerce] l&#39;applicazione è in modalità di manutenzione. Per un ambiente di produzione, Adobe consiglia di completare questo lavoro nelle ore di minore utilizzo per evitare interruzioni del servizio.

## Segnaposto percorso {#route-placeholder}

>[!NOTE]
>
>Negli esempi di configurazione di route riportati di seguito vengono utilizzati modelli di route con segnaposto. Il `{default}` placeholder rappresenta il dominio predefinito configurato per il sito. Se il progetto ha più domini, utilizza `{all}` segnaposto per configurare il routing per il dominio predefinito e per tutti gli alias. Consulta [Configurare le route](/help/cloud-guide/routes/routes-yaml.md).

## Tempistica SCD {#scd-timing-warning}

>[!WARNING]
>
>In caso di problemi con i file di contenuto statico nell’applicazione dopo la distribuzione, ad esempio se mancano file di tema personalizzati, aumenta il tempo di esecuzione massimo previsto a 900 secondi o più.

## Distribuzione basata su scenari {#scenarios}

>[!NOTE]
>
>Con [!DNL ECE-Tools] 2002.1.0 e versioni successive, puoi utilizzare la funzione di distribuzione basata su scenari per personalizzare i processi di build, distribuzione e post-distribuzione per il progetto di infrastruttura cloud di Adobe Commerce. Consulta [Distribuzione basata su scenari](/help/cloud-guide/deploy/scenario-based.md).

## Istruzione di servizio {#service-instruction}

Utilizzare le istruzioni seguenti per la configurazione del servizio negli ambienti di integrazione Pro e negli ambienti Starter, inclusi `master` filiale.

>[!NOTE]
>
>[Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per modificare la configurazione del servizio negli ambienti Pro Production e Staging.

## Modifica del servizio {#service-change-tip}

>[!TIP]
>
>Dopo la configurazione iniziale del servizio, è possibile modificare la versione del software per un servizio installato aggiornando `services.yaml` e `.magento.app.yaml` file di configurazione. Consulta [Modifica versione del servizio](/help/cloud-guide/services/services-yaml.md#change-service-version) per indicazioni sull&#39;aggiornamento o il downgrade di un servizio.

## Suggerimento distribuzione bloccata {#stuck-deployment-tip}

>[!TIP]
>
>Per assistenza sulle distribuzioni bloccate, utilizza [Risoluzione dei problemi di distribuzione di Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html) nel _Centro assistenza Commerce_.

## Aggiornamento a ECE-Tools {#ece-tools-package}

>[!NOTE]
>
>Se utilizzi una versione di Adobe Commerce su infrastruttura cloud che non contiene `ece-tools` , è necessario eseguire una [aggiornamento una tantum](/help/cloud-guide/dev-tools/install-package.md) nel progetto cloud per rimuovere i pacchetti obsoleti. Se al momento utilizzi il `ece-tools` e devi aggiornarlo, consulta [Aggiornare il pacchetto ECE-Strumenti](/help/cloud-guide/dev-tools/update-package.md).

## Suggerimento per l&#39;aggiornamento {#upgrade-tip}

>[!TIP]
>
>Prima di iniziare un aggiornamento o un processo di applicazione di patch, crea un ramo attivo dall’ambiente di integrazione ed estrai il nuovo ramo sulla workstation locale. La destinazione di un ramo all’aggiornamento o al processo di patch consente di evitare interferenze con il lavoro in corso.

<!-- Fastly-related snippets begin -->

## Accesso amministratore {#admin-login-step}

1. [Accedi](/help/get-started/onboarding.md#access-your-admin-panel) all’amministratore.

## Automatizzare l&#39;implementazione di snippet VCL personalizzato {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>Invece di caricare manualmente snippet VCL personalizzati, puoi aggiungere snippet al file `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` nell&#39;ambiente. I frammenti in questa directory vengono caricati automaticamente quando fate clic su _carica VCL in Fastly_ in Commerce Admin. Consulta [Distribuzione automatizzata di snippet VCL personalizzati](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) nel modulo Fastly CDN per la documentazione del Magento 2.

<!-- Fastly-related snippets end -->
