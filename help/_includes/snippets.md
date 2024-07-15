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
>I progetti eseguiti prima del 5 giugno 2020 disponevano di più ambienti di integrazione più piccoli. Se hai bisogno di un ambiente di integrazione più ampio per i test e lo sviluppo, richiedi un aggiornamento per gli ambienti di integrazione avanzata. Per informazioni dettagliate, consulta l&#39;articolo [Richiesta ambiente di integrazione](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/announcements/commerce-announcements/integration-environment-enhancement-request-pro-and-starter.html) nel _Centro assistenza Adobe Commerce_.

## Opzioni di unione {#merge-options}

Per impostazione predefinita, il processo di distribuzione sovrascrive tutte le impostazioni nel file `env.php`. Tuttavia, è possibile scegliere di unire uno o più valori per una configurazione del servizio senza sovrascrivere tutti i valori.

Impostare l&#39;opzione `_merge` su una delle opzioni seguenti:

- `true`—**Unisci** i valori del servizio configurati con i valori delle variabili di ambiente.
- `false`—**Sovrascrivi** i valori del servizio configurati con i valori delle variabili di ambiente.

## Archivio privato {#private-repository}

>[!NOTE]
>
>L’Adobe consiglia vivamente di utilizzare un archivio privato per il progetto di infrastruttura Adobe Commerce on cloud per proteggere eventuali informazioni proprietarie o attività di sviluppo, come estensioni e configurazioni sensibili.

## Avvertenza self-service Pro {#pro-self-service-warning}

>[!WARNING]
>
>Alcuni **progetti Pro** richiedono un ticket di supporto per aggiornare la configurazione della route nel file `routes.yaml` e la configurazione cron nel file `.magento.app.yaml`. L’Adobe consiglia di aggiornare e testare i file di configurazione YAML in un ambiente di integrazione, quindi di distribuire le modifiche nell’ambiente di staging. Se le modifiche non vengono applicate ai siti di gestione temporanea dopo la ridistribuzione e non sono presenti messaggi di errore correlati nel registro, è **NECESSARIO** [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) che descriva le modifiche di configurazione tentate. Includi nel ticket tutti i file di configurazione YAML aggiornati.

## Supporto dei servizi Pro {#pro-update-service}

>[!TIP]
>Per i progetti Pro, è necessario [inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per installare o aggiornare [servizi](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/service/services-yaml.html) solo negli ambienti `Staging` e `Production`.
>
>Indicare le modifiche necessarie al servizio, includere i file `.magento.app.yaml` e `services.yaml` aggiornati e indicare la versione PHP nel ticket. Per le modifiche self-service alle impostazioni di versione PHP, estensioni o ambiente, vedere [Impostazioni PHP](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/php-settings.html) in _Configurazione applicazione_.
>
>Per le modifiche a un ambiente di produzione _live_ (**Solo Pro**), devi fornire un preavviso minimo di 48 ore per consentire al team di infrastruttura cloud di disporre del tempo sufficiente per eseguire il marshalling delle risorse e eseguire un aggiornamento sicuro.

## Backup Pro {#pro-backups}

>[!TIP]
>
>Negli ambienti Pro Staging e Production, è necessario [inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per recuperare un backup specifico che rilevi la data, l&#39;ora e il fuso orario nel ticket.
>
>L&#39;Adobe **not** ripristina gli ambienti da un backup automatico. Per informazioni su come scegliere un metodo per ripristinare uno snapshot di staging o produzione, vedere [Ripristinare uno snapshot del database da staging o produzione](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/restore-a-db-snapshot-from-staging-or-production.html).

## Avviso di ridistribuzione {#redeploy-warning}

>[!WARNING]
>
>Il processo di distribuzione inizia quando si esegue un&#39;unione, un push o una sincronizzazione dell&#39;ambiente oppure quando si attiva una ridistribuzione manuale, durante la quale l&#39;applicazione [!DNL Commerce] è in modalità manutenzione. Per un ambiente di produzione, Adobe consiglia di completare questo lavoro nelle ore di minore utilizzo per evitare interruzioni del servizio.

## Segnaposto percorso {#route-placeholder}

>[!NOTE]
>
>Negli esempi di configurazione di route riportati di seguito vengono utilizzati modelli di route con segnaposto. Il segnaposto `{default}` rappresenta il dominio predefinito configurato per il sito. Se il progetto ha più domini, utilizzare il segnaposto `{all}` per configurare il routing per il dominio predefinito e per tutti gli alias. Vedere [Configurare le route](/help/cloud-guide/routes/routes-yaml.md).

## Tempistica SCD {#scd-timing-warning}

>[!WARNING]
>
>In caso di problemi con i file di contenuto statico nell’applicazione dopo la distribuzione, ad esempio se mancano file di tema personalizzati, aumenta il tempo di esecuzione massimo previsto a 900 secondi o più.

## Distribuzione basata su scenari {#scenarios}

>[!NOTE]
>
>Con [!DNL ECE-Tools] 2002.1.0 e versioni successive, è possibile utilizzare la funzionalità di distribuzione basata su scenari per personalizzare i processi di compilazione, distribuzione e post-distribuzione per il progetto Adobe Commerce su infrastruttura cloud. Vedi [Distribuzione basata su scenari](/help/cloud-guide/deploy/scenario-based.md).

## Istruzione di servizio {#service-instruction}

Utilizzare le istruzioni seguenti per la configurazione del servizio negli ambienti di integrazione Pro e negli ambienti Starter, incluso il ramo `master`.

>[!NOTE]
>
>[Invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per modificare la configurazione del servizio negli ambienti di produzione e staging di Pro.

## Modifica del servizio {#service-change-tip}

>[!TIP]
>
>Dopo la configurazione iniziale del servizio, è possibile modificare la versione del software per un servizio installato aggiornando i file di configurazione `services.yaml` e `.magento.app.yaml`. Per informazioni sull&#39;aggiornamento o il downgrade di un servizio, vedere [Modifica versione del servizio](/help/cloud-guide/services/services-yaml.md#change-service-version).

## Suggerimento distribuzione bloccata {#stuck-deployment-tip}

>[!TIP]
>
>Per informazioni sulle distribuzioni bloccate, utilizzare lo strumento di risoluzione dei problemi di distribuzione di [Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/deployment/magento-deployment-troubleshooter.html) nel _Centro assistenza di Commerce_.

## Aggiornamento a ECE-Tools {#ece-tools-package}

>[!NOTE]
>
>Se utilizzi una versione di Adobe Commerce sull&#39;infrastruttura cloud che non contiene il pacchetto `ece-tools`, devi eseguire un [aggiornamento una tantum](/help/cloud-guide/dev-tools/install-package.md) al progetto cloud per rimuovere i pacchetti obsoleti. Se si utilizza il pacchetto `ece-tools` e si desidera aggiornarlo, vedere [Aggiornare il pacchetto ECE-Tools](/help/cloud-guide/dev-tools/update-package.md).

## Suggerimento per l&#39;aggiornamento {#upgrade-tip}

>[!TIP]
>
>Prima di iniziare un aggiornamento o un processo di applicazione di patch, crea un ramo attivo dall’ambiente di integrazione ed estrai il nuovo ramo sulla workstation locale. La destinazione di un ramo all’aggiornamento o al processo di patch consente di evitare interferenze con il lavoro in corso.

<!-- Fastly-related snippets begin -->

## Accesso amministratore {#admin-login-step}

1. [Accedi](/help/get-started/onboarding.md#access-your-admin-panel) all&#39;amministratore.

## Automatizzare l&#39;implementazione di snippet VCL personalizzato {#automate-vcl-snippet-deployment}

>[!NOTE]
>
>Anziché caricare manualmente snippet VCL personalizzati, è possibile aggiungere snippet alla directory `$MAGENTO_CLOUD_APP_DIR/var/vcl_snippets_custom` nell&#39;ambiente. I frammenti in questa directory vengono caricati automaticamente quando si fa clic su _carica VCL in Fastly_ in Commerce Admin. Consulta [Distribuzione automatizzata di snippet VCL personalizzati](https://github.com/fastly/fastly-magento2/blob/master/Documentation/Guides/CUSTOM-VCL-SNIPPETS.md#automated-custom-vcl-snippets-deployment) nel modulo Fastly CDN per la documentazione del Magento 2.

<!-- Fastly-related snippets end -->
