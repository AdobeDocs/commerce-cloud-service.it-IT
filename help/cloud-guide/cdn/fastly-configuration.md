---
title: Configurare i servizi Fastly
description: Scopri come impostare e configurare i servizi Fastly per il tuo progetto Adobe Commerce.
feature: Cloud, Configuration, Iaas, Cache, Security
exl-id: c53ff3bd-3df2-45fb-933e-d3b29f7edf4e
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '1961'
ht-degree: 0%

---

# Configurare i servizi Fastly

Fastly è richiesto per Adobe Commerce sugli ambienti di staging e produzione dell’infrastruttura cloud.

Fastly funziona con Varnish per fornire funzionalità di caching veloci e [Rete di distribuzione dei contenuti](https://glossary.magento.com/content-delivery-network) (CDN) per le risorse statiche. Fastly fornisce anche un web application firewall (WAF) per proteggere il sito e l’infrastruttura cloud. Per proteggere il sito e l’infrastruttura Cloud da traffico e attacchi dannosi, indirizza tutto il traffico del sito in ingresso tramite Fastly.

>[!NOTE]
>
>Fastly non è disponibile negli ambienti di integrazione.

Completa i passaggi seguenti per abilitare, configurare e testare Fastly nelle prime fasi del processo di sviluppo del sito per abilitare l’accesso sicuro al sito.

- Ottenere credenziali rapide per gli ambienti di staging e produzione
- Abilita caching Fastly CDN
- Carica snippet VCL Fastly
- Aggiorna la configurazione DNS per indirizzare il traffico al servizio Fastly
- Test Fastly caching

>[!NOTE]
>
>Dopo aver abilitato e verificato la configurazione Fastly iniziale, puoi personalizzarla. Ad esempio, puoi abilitare opzioni aggiuntive quali ottimizzazione immagine, moduli edge e codice VCL personalizzato. Consulta [Personalizza configurazione cache](fastly-custom-cache-configuration.md).

## Ottieni credenziali rapide

Durante il provisioning del progetto, Adobe aggiunge il progetto al [Account Fastly Service](fastly.md#fastly-service-account-and-credentials) per Adobe Commerce sull’infrastruttura cloud e crea le credenziali dell’account Fastly per lo Starter `master` e Pro Staging and Production. Ogni ambiente dispone di credenziali univoche.

È necessario disporre delle credenziali Fastly per configurare i servizi CDN Fastly dall’amministratore e inviare le richieste API Fastly.

>[!NOTE]
>
>Con Adobe Commerce sull’infrastruttura cloud, non è possibile accedere direttamente a Fastly Admin. Utilizza l’amministratore per rivedere e aggiornare la configurazione Fastly per i tuoi ambienti. Se non riesci a risolvere un problema utilizzando le funzionalità Fastly nell’amministratore, invia una [Ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket).

Utilizza i seguenti metodi per trovare e salvare l’ID servizio Fastly e il token API per il tuo ambiente:

**Per visualizzare le credenziali Fastly**:

Il metodo di visualizzazione delle credenziali è diverso per i progetti Pro e Starter.

- Directory condivisa montata su IaaS: nei progetti Pro, utilizza SSH per connettersi al server e ottenere le credenziali Fastly dal `/mnt/shared/fastly_tokens.txt` file. Gli ambienti di staging e produzione dispongono di credenziali univoche. È necessario ottenere le credenziali per ogni ambiente.

- Area di lavoro locale - Dalla riga di comando, utilizzate `magento-cloud` CLI a [elenco e revisione](../environment/variables-cloud.md#viewing-environment-variables) Variabili di ambiente Fastly.

  ```bash
  magento-cloud variable:get -e <environment-ID>
  ```

- [!DNL Cloud Console]- Controlla le seguenti variabili di ambiente nella [Configurazione dell’ambiente](../project/overview.md#configure-environment).

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_API_KEY`

   - `CONFIG__DEFAULT__SYSTEM__FULL_PAGE_CACHE__FASTLY__FASTLY_SERVICE_ID`

>[!NOTE]
>
>Se non riesci a trovare le credenziali Fastly per gli ambienti di staging o produzione, contatta l’Adobe Customer Technical Advisor (CTA).

## Abilita Fastly caching

Per abilitare e configurare i servizi Fastly sono necessari i seguenti componenti:

- Ultima versione di [CDN Fastly per il modulo del Magento 2](fastly.md#fastly-cdn-module-for-magento-2) negli ambienti di staging e produzione. Consulta [Aggiorna rapidamente](#upgrade-the-fastly-module).

- [Credenziali rapide](#get-fastly-credentials) per Adobe Commerce sugli ambienti di staging e produzione dell’infrastruttura cloud

**Per abilitare il caching Fastly CDN in Staging e Produzione**:

{{admin-login-step}}

1. Clic **Negozi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema** ed espandi **Cache a pagina intera**.

   ![Espandi per selezionare Fastly](../../assets/cdn/fastly-menu.png)

1. In _Applicazione di caching_ , rimuovere la selezione da **Usa valore di sistema** e quindi selezionare **Fastly CDN** dall’elenco a discesa.

   ![Scegli Fastly](../../assets/cdn/fastly-enable-admin.png)

1. Espandi **Configurazione rapida** e [scegli opzioni di caching](https://github.com/fastly/fastly-magento2/blob/master/Documentation/CONFIGURATION.md#configure-the-module).

1. Dopo aver configurato le opzioni di caching, fai clic su **Salva configurazione** nella parte superiore della pagina.

1. Cancella la cache in base alla notifica.

1. Continua a configurare Fastly tornando a **Negozi** > **Impostazioni** > **Configurazione** > **Avanzate** > **Sistema** > **Configurazione rapida**.

### Verifica credenziali veloci

1. In Admin, passa a **Negozi** > Impostazioni > **Configurazione** > **Avanzate** > **Sistema** > **Configurazione rapida**.

1. Se necessario, aggiungi **ID servizio rapido** e **Token API** valori per l’ambiente del progetto.

   ![Amministratore credenziali Fastly](../../assets/cdn/fastly-credentials-admin-ui.png)

   >[!NOTE]
   >
   >Non selezionare il collegamento per creare il token API Fastly. Invece, utilizza [Credenziali Fastly (ID servizio e token API) fornite dall’Adobe](#get-fastly-credentials) fornite dall’Adobe.

1. Clic **Credenziali di prova**.

1. Se il test ha esito positivo, fai clic su **Salva configurazione** e quindi cancellare la cache.

   Se il test non riesce, verifica che i valori corretti dell’ID servizio e del token API corrispondano alle credenziali per l’ambiente corrente.

   Se il test non riesce di nuovo, invia un ticket di supporto Adobe Commerce o contatta il rappresentante del tuo account Adobe. Per i progetti Pro, includi gli URL per i siti di produzione e staging. Per i progetti iniziali, includi gli URL per `Master` e del sito di staging.

>[!NOTE]
>
>Per istruzioni su come modificare le credenziali del token API Fastly per un ambiente di staging o produzione, consulta [Modifica credenziali Fastly](fastly.md#change-fastly-api-token).

### Carica VCL in Fastly

Dopo aver attivato il modulo Fastly, carica il valore predefinito [Codice VCL](https://github.com/fastly/fastly-magento2/tree/master/etc/vcl_snippets) ai server Fastly. Questo codice fornisce una serie di snippet VCL che specificano le impostazioni di configurazione per abilitare il caching e altri servizi Fastly CDN per l’infrastruttura Adobe Commerce su cloud.

>[!NOTE]
>
>I servizi di Fastly caching funzionano solo dopo aver completato il caricamento iniziale del codice Fastly VCL nei siti di staging e produzione di Adobe Commerce.

**Per caricare Fastly VCL**:

1. In _Configurazione rapida_ , fare clic su **Carica VCL in Fastly** come illustrato nella figura seguente.

   ![Caricare un Magento VCL in Fastly](../../assets/cdn/fastly-upload-vcl-admin.png)

1. Al termine del caricamento, aggiorna la cache in base alla notifica nella parte superiore della pagina.

## Provisioning dei certificati SSL/TLS

L’Adobe fornisce un certificato SSL/TLS crittografato con convalida di dominio per distribuire traffico HTTPS sicuro da Fastly. Adobe fornisce un certificato per ogni ambiente Pro Production, Staging e Starter Production per proteggere tutti i domini in tale ambiente. Per informazioni dettagliate sul certificato fornito, consulta [Adobe di certificati SSL (TLS) per Adobe Commerce sull’infrastruttura cloud](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/how-to/ssl-tls-certificates-for-magento-commerce-cloud-faq.html).

>[!NOTE]
>
>Puoi fornire un certificato TLS o SSL personalizzato invece di utilizzare il certificato Let&#39;s Encrypt fornito da Adobe. Tuttavia, questo processo richiede un lavoro aggiuntivo per l&#39;impostazione e la manutenzione. Per scegliere questa opzione, invia un ticket di supporto Adobe Commerce o utilizza l’Adobe per aggiungere certificati personalizzati in hosting al tuo Adobe Commerce negli ambienti dell’infrastruttura cloud.

Per abilitare i certificati SSL/TLS per gli ambienti Adobe Commerce, l’automazione degli Adobi completa i seguenti passaggi:

- Convalida la proprietà del dominio
- Esegue il provisioning di un certificato SSL/TLS crittografato che copre domini principali e sottodomini specifici per gli archivi
- Carica il certificato nell’ambiente Cloud quando il sito è attivo

Questa automazione richiede l&#39;aggiornamento della configurazione DNS del sito per fornire informazioni di convalida del dominio. Utilizzare **uno** dei seguenti metodi:

- **Convalida DNS**-Per i siti live, aggiorna la configurazione DNS con i record CNAME che puntano al servizio Fastly
- **Record CNAME ACME challenge**-Aggiornare la configurazione DNS con i record CNAME di richiesta ACME forniti dall&#39;Adobe per ogni dominio dell&#39;ambiente

>[!TIP]
>
>Se il dominio di produzione non è attivo, utilizzare i record CNAME di verifica ACME per la convalida del dominio. L’aggiunta anticipata dei record alla configurazione DNS consente ad Adobe di eseguire il provisioning del certificato SSL/TLS con i domini corretti prima dell’avvio del sito. Prima di avviare l’attività di produzione, devi sostituire questi record segnaposto con i record CNAME forniti da Adobe.

Al termine della convalida del dominio, Adobe esegue il provisioning del certificato Let&#39;s Encrypt TLS/SSL e lo carica negli ambienti di staging o produzione live. Questo processo può richiedere fino a 12 ore. È consigliabile completare gli aggiornamenti della configurazione DNS con diversi giorni di anticipo per evitare ritardi nello sviluppo e nell’avvio del sito.

## Aggiornare la configurazione DNS con le impostazioni di sviluppo

Durante il processo di configurazione iniziale di Fastly, è possibile utilizzare i seguenti URL per configurare e testare il caching Fastly negli ambienti di staging e produzione:

- Per Pro Staging e Produzione:

   - `mcprod.<your-domain>.com`
   - `mcstaging.<your-domain>.com`

- Solo per la produzione iniziale:

   - `mcprod.<your-domain>.com`

Questi URL predefiniti di preproduzione sono disponibili dopo il provisioning del progetto. Il valore per `"your-domain"` è il nome di dominio specificato durante il processo di onboarding.

>[!NOTE]
>
>Non è possibile specificare un dominio personalizzato per un ambiente non di produzione in progetti Starter.

Per instradare il traffico dagli URL dell’archivio al servizio Fastly, aggiorna la configurazione DNS. Quando aggiorni la configurazione, Adobe esegue automaticamente il provisioning dei certificati SSL/TLS richiesti e li carica negli ambienti Cloud. Questo provisioning può richiedere fino a 12 ore.

>[!NOTE]
>
>Quando sei pronto per avviare il sito di produzione, devi aggiornare nuovamente la configurazione DNS per indirizzare i domini di produzione al servizio Fastly e completare attività di configurazione aggiuntive. Consulta [Elenco di controllo di Launch](../launch/checklist.md).

**Prerequisiti:**

- Abilita il modulo Fastly.
- Carica il codice VCL Fastly predefinito.
- Fornisci un elenco di domini principali e secondari per ogni ambiente da Adobe, oppure invia un ticket di supporto Adobe Commerce.
- Attendi la conferma che i domini specificati siano stati aggiunti agli ambienti cloud.
- Nei progetti Starter, aggiungi i domini alla configurazione del servizio Fastly. Consulta [Gestione domini](fastly-custom-cache-configuration.md#manage-domains).
- Per informazioni sull’aggiornamento della configurazione DNS, verifica con [Registrar DNS](https://lookup.icann.org/) per il metodo corretto per il servizio di dominio.

**Per aggiornare la configurazione DNS per lo sviluppo**:

1. Indirizza gli URL di preproduzione al servizio Fastly aggiungendo record CNAME: `prod.magentocloud.map.fastly.net`, ad esempio:

   | Dominio o sottodominio | CNAME |
   |---------------------------|----------------------------------|
   | mcprod.your-domain.com | prod.magentocloud.map.fastly.net |
   | mcstaging.your-domain.com | prod.magentocloud.map.fastly.net |

   Quando i record CNAME sono live, Adobe esegue il provisioning dei certificati e carica i certificati SSL/TLS.

   >[!NOTE]
   >
   >Se prevedi di utilizzare i domini APEX (`your-domain.com`) per il sito di produzione, è necessario configurare i record degli indirizzi DNS (record A) in modo che puntino agli indirizzi IP del server Fastly. Consulta [Aggiornamento della configurazione DNS con le impostazioni di produzione](../launch/checklist.md#to-update-dns-configuration-for-site-launch).


1. Aggiungi i record CNAME di richiesta ACME per la convalida del dominio e il preprovisioning dei certificati SSL/TLS di produzione, ad esempio:

   | Dominio o sottodominio | CNAME |
   |-------------------------------------------|-------------------------------------------|
   | _acme-challenge.your-domain.com | 0123456789abcdef.validation.magento.cloud |
   | _acme-challenge.www.your-domain.com | 9573186429stuvwx.validation.magento.com |
   | _acme-challenge.mystore.your-domain.com | 1234567898zxywvu.validation.magento.cloud |
   | _acme-challenge.subdomain.your-domain.com | 1098765743lmnopq.validation.magento.cloud |

   >[!NOTE]
   >
   >I record di verifica ACME in questo esempio sono segnaposto che non sono destinati al provisioning dei siti di staging e produzione di Adobe Commerce. Ottieni le informazioni corrette sui record di verifica ACME per il tuo progetto contattando un Adobe.

   Dopo l’aggiunta dei record CNAME, Adobe convalida i domini e esegue il provisioning del certificato SSL/TLS per l’ambiente. Quando aggiorni la configurazione DNS per instradare il traffico da questi domini al servizio Fastly, Adobe carica il certificato nell’ambiente.

1. Aggiorna l’URL di base di Adobe Commerce.

   - Utilizza SSH per accedere all’ambiente di produzione.

     ```bash
     magento-cloud ssh
     ```

   - Utilizza Cloud CLI per modificare l’URL di base per il tuo archivio.

     ```bash
     php bin/magento setup:store-config:set --base-url="https://mcstaging.your-domain.com/"
     ```

   >[!NOTE]
   >
   >In alternativa all’utilizzo di Cloud CLI, puoi aggiornare l’URL di base da [Amministratore](https://experienceleague.adobe.com/docs/commerce-admin/stores-sales/site-store/store-urls.html)

1. Riavvia il browser Web.

1. Verifica il tuo sito web.

## Test Fastly caching

Dopo aver completato le modifiche alla configurazione DNS, utilizzare [cURL](https://curl.se/) strumento da riga di comando per verificare il funzionamento della cache Fastly.

**Controllare le intestazioni di risposta**:

1. In un terminale, utilizzare quanto segue `curl` comando per verificare l’URL del sito live:

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 https://<live-URL>
   ```

   Se non hai impostato una route statica o completato la configurazione DNS per i domini sul sito live, utilizza `--resolve` che ignora la risoluzione dei nomi DNS.

   ```bash
   curl -vo /dev/null -H Fastly-Debug:1 --resolve <live-URL-hostname>:443:<live-IP-address>
   ```

1. Nella risposta, verifica i [intestazioni](fastly-troubleshooting.md#check-cache-hit-and-miss-response-headers) per garantire che Fastly funzioni. Dovresti visualizzare le seguenti intestazioni univoche nella risposta:

   ```http
   < Fastly-Magento-VCL-Uploaded: yes
   < X-Cache: HIT, MISS
   ```

Se i valori delle intestazioni non sono corretti, consulta [Risolvi gli errori rilevati nelle intestazioni di risposta](fastly-troubleshooting.md#curl) per assistenza nella risoluzione dei problemi.

## Aggiornare il modulo Fastly

Fastly aggiorna la rete CDN Fastly per il modulo Magento 2 per risolvere i problemi, migliorare le prestazioni e fornire nuove funzioni.
È consigliabile aggiornare il modulo Fastly negli ambienti di staging e produzione al [ultima versione](https://github.com/fastly/fastly-magento2/blob/master/VERSION).

Dopo aver aggiornato il modulo, è necessario caricare il codice VCL per applicare le modifiche alla configurazione del servizio Fastly.

>[!WARNING]
>
> Se hai personalizzato il codice VCL Fastly predefinito con una versione personalizzata, l’aggiornamento del modulo Fastly sovrascrive le modifiche. Se sono stati aggiunti snippet VCL personalizzati con nomi univoci, tali modifiche vengono mantenute durante il processo di aggiornamento. Come best practice, aggiorna l’ambiente di staging e convalida le modifiche prima di applicarle all’ambiente di produzione.

**Per verificare la versione del modulo CDN Fastly per il Magento 2**:

1. Passa alla directory principale dell’ambiente Cloud.

1. Utilizza Composer per controllare la versione installata.

   ```bash
   composer show *fastly*
   ```

1. Se il [versione più recente](https://github.com/fastly/fastly-magento2/releases) non è installato, completa i passaggi per aggiornare il modulo Fastly.

**Per aggiornare il modulo Fastly**:

1. Nell’ambiente di integrazione locale, utilizza le seguenti informazioni del modulo per [aggiornare il modulo Fastly](../store/extensions.md#upgrade-an-extension).

   ```text
   module name: fastly/magento2
   repository: https://github.com/fastly/fastly-magento2.git
   ```

1. Invio degli aggiornamenti all&#39;ambiente di staging.

1. Accedi all’amministratore per l’ambiente di staging per [carica il codice VCL](#upload-vcl-to-fastly).

1. [Verifica servizi Fastly](fastly-troubleshooting.md#verify-or-debug-fastly-services) sul sito di staging di Adobe Commerce.

Dopo aver verificato i servizi Fastly sul sito di staging, ripeti il processo di aggiornamento nell’ambiente di produzione.

>[!TIP]
>
> In caso di problemi con i servizi Fastly negli ambienti Adobe Commerce, vedi [Risoluzione dei problemi Fastly di Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/magento-fastly-troubleshooter.html).
