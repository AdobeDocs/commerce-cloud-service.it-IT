---
user-guide-title: Guida di Commerce su infrastruttura Cloud
user-guide-description: Scopri come gestire l’applicazione Adobe Commerce sull’infrastruttura cloud.
product: magento
feature: Cloud
source-git-commit: 54b9101d3cba535a0e2c83e482aaebe261e3bcce
workflow-type: tm+mt
source-wordcount: '356'
ht-degree: 7%

---


# Commerce su infrastruttura cloud {#user-guide}

+ [Commerce](overview.md)
+ Architettura {#architecture}
   + [Infrastruttura cloud](architecture/cloud-architecture.md)
   + [Stack tecnologico](architecture/tech-stack.md)
   + [Architettura iniziale](architecture/starter-architecture.md)
   + [Flusso di lavoro iniziale](architecture/starter-develop-deploy-workflow.md)
   + [Architettura Pro](architecture/pro-architecture.md)
   + [Flusso di lavoro professionale](architecture/pro-develop-deploy-workflow.md)
   + [Architettura scalata](architecture/scaled-architecture.md)
   + [Ridimensionamento automatico](architecture/autoscaling.md)
+ [Introduzione](https://experienceleague.adobe.com/docs/commerce-cloud-service/start/overview.html)
+ Note sulla versione {#release-notes}
   + [Suite di strumenti cloud](release-notes/cloud-tools-suite.md)
   + [Pacchetto ECE-Strumenti](release-notes/ece-tools-package.md)
   + [Patch cloud](release-notes/cloud-patches.md)
   + [Pacchetto Docker cloud](release-notes/cloud-docker.md)
   + [Componenti cloud](release-notes/cloud-components.md)
   + [Pacchetti cloud](release-notes/cloud-packages.md)
   + [Modifiche non compatibili con le versioni precedenti](release-notes/backward-incompatible-changes.md)
   + [Archivio delle note sulla versione](release-notes/cloud-release-archive.md)
+ Progetto cloud {#project}
   + [Panoramica del progetto](project/overview.md)
   + [Struttura del progetto](project/file-structure.md)
   + [Accesso utente](project/user-access.md)
   + [Autenticazione a più fattori](project/multi-factor-authentication.md)
   + [Flusso di attività](project/activity-stream.md)
   + [E-mail in uscita](project/outgoing-emails.md)
   + [Servizio e-mail SendGrid](project/sendgrid.md)
   + [Gestione dei rami della console](project/console-branches.md)
   + [Indirizzi IP regionali](project/regional-ip-addresses.md)
+ Strumenti per sviluppatori {#dev-tools}
   + [Panoramica](dev-tools/overview.md)
   + CLI cloud {#cloud-cli}
      + [Panoramica di CLI](dev-tools/cloud-cli-overview.md)
      + [Riferimento CLI](dev-tools/cloud-cli-reference.md)
   + [Docker cloud](dev-tools/cloud-docker.md)
   + Strumenti ECE {#ece-tools}
      + [Panoramica del pacchetto](dev-tools/package-overview.md)
      + [Aggiornamento una tantum per utilizzare gli strumenti ECE](dev-tools/install-package.md)
      + [Aggiorna pacchetto ECE-Tools](dev-tools/update-package.md)
      + [Riferimento CLI](dev-tools/ece-tools-cli-reference.md)
      + [Riferimento errore](dev-tools/error-reference.md)
   + Integrazioni {#integrations}
      + [Panoramica](integrations/overview.md)
      + [Bitbucket](integrations/bitbucket.md)
      + [GitHub](integrations/github.md)
      + [GitLab](integrations/gitlab.md)
      + [Notifiche stato](integrations/health-notifications.md)
+ Sviluppo {#develop}
   + [Panoramica](development/overview.md)
   + [Chiavi di autenticazione](development/authentication-keys.md)
   + [Gestione delle filiali CLI](development/cli-branches.md)
   + [Connessioni sicure](development/secure-connections.md)
   + Distribuisci {#deploy}
      + [Processo di distribuzione](deploy/process.md)
      + [Ottimizzazione](deploy/optimization.md)
      + [Best practice](deploy/best-practices.md)
      + [Distribuzione basata su scenari](deploy/scenario-based.md)
      + [Installazione senza downtime](deploy/reduce-downtime.md)
      + [Distribuzione di contenuti statici](deploy/static-content.md)
      + [Procedure guidate intelligenti](deploy/smart-wizards.md)
      + [Distribuzione a staging e produzione](deploy/staging-production.md)
      + [Ripristino da guasto componente](deploy/recover-failed-deployment.md)
   + Prova {#test}
      + [Linee guida per i test](test/guidance.md)
      + [Registri](test/log-locations.md)
      + [Xdebug](test/debug.md)
      + [Dati di esempio](test/sample-data.md)
      + [Staging e produzione](test/staging-and-production.md)
      + [Secondo ambiente di staging](test/second-staging.md)
   + [Servizio PrivateLink](development/privatelink-service.md)
   + [Blocco protettivo](development/protective-block.md)
   + [Ripristina ambiente](development/restore-environment.md)
   + Archiviazione {#storage}
      + [Gestione dello spazio su disco](storage/manage-disk-space.md)
      + [Query del database del profilo](storage/profile-database-queries.md)
      + [Eseguire il backup del database](storage/database-dump.md)
      + [Gestione dei backup](storage/snapshots.md)
   + Aggiornamenti e patch {#upgrade}
      + [Best practice](development/best-practices.md)
      + [Aggiorna versione Commerce](development/commerce-version.md)
      + [Applicare le patch](development/apply-patches.md)
+ Configurazione {#configure}
   + [Panoramica](environment/overview.md)
   + Applicazione {#app}
      + [Configurare la distribuzione dell&#39;applicazione](application/configure-app-yaml.md)
      + [Impostazioni PHP](application/php-settings.md)
      + Proprietà {#properties}
         + [Proprietà applicazione](application/properties.md)
         + [Cron](application/crons-property.md)
         + [Firewall (solo Starter)](application/firewall-property.md)
         + [Hook](application/hooks-property.md)
         + [Variabili](application/variables-property.md)
         + [Web](application/web-property.md)
         + [Lavoratori](application/workers-property.md)
      + [Imposta cache per file statici](application/set-cache.md)
   + Ambiente {#env}
      + [Configurare l’implementazione dell’ambiente](environment/configure-env-yaml.md)
      + [Livelli e opzioni delle variabili](environment/variable-levels.md)
      + Ignora variabili {#stage}
         + [Variabili di ambiente](environment/variables-intro.md)
         + [ADMIN](environment/variables-admin.md)
         + [Variabili cloud](environment/variables-cloud.md)
         + [Globale](environment/variables-global.md)
         + [Genera](environment/variables-build.md)
         + [Distribuisci](environment/variables-deploy.md)
         + [Post-distribuzione](environment/variables-post-deploy.md)
      + Configura notifiche {#log}
         + [Notifiche](environment/set-up-notifications.md)
         + [Gestori di registro](environment/log-handlers.md)
   + Route {#routes}
      + [Configurare le route](routes/routes-yaml.md)
      + [Memorizzazione in cache](routes/caching.md)
      + [Reindirizzamenti](routes/redirects.md)
      + [Server-side include](routes/server-side-includes.md)
   + Servizi {#service}
      + [Configurare i servizi](services/services-yaml.md)
      + [Elasticsearch](services/elasticsearch.md)
      + [MySQL](services/mysql.md)
      + [OpenSearch](services/opensearch.md)
      + [RabbitMQ](services/rabbitmq.md)
      + [Redis](services/redis.md)
+ Servizi Fastly {#cdn}
   + [Panoramica](cdn/fastly.md)
   + Impostazione rapida {#setup-fastly}
      + [Configurare i servizi Fastly](cdn/fastly-configuration.md)
      + [Personalizza configurazione cache](cdn/fastly-custom-cache-configuration.md)
      + [Personalizzare le pagine di errore e manutenzione](cdn/fastly-custom-response.md)
   + [Firewall applicazione Web](cdn/fastly-waf-service.md)
   + [Ottimizzazione immagine](cdn/fastly-image-optimization.md)
   + Personalizza con VCL {#custom-vcl-snippets}
      + [Introduzione](cdn/fastly-vcl-custom-snippets.md)
      + [Reindirizzare le richieste a un back-end CMS](cdn/fastly-vcl-wordpress.md)
      + [Blocca spam di riferimento](cdn/fastly-vcl-badreferer.md)
      + [INSERIRE NELL&#39;ELENCO CONSENTITI IP](cdn/fastly-vcl-allowlist.md)
      + [INSERIRE NELL&#39;ELENCO BLOCCATI IP](cdn/fastly-vcl-blocking.md)
      + [Ignora Fastly Cache](cdn/fastly-vcl-bypass-to-origin.md)
   + [Risoluzione rapida dei problemi](cdn/fastly-troubleshooting.md)
+ Impostazioni archivio {#configure-store}
   + [Panoramica](store/overview.md)
   + [Best practice](store/best-practices.md)
   + [Tema personalizzato](store/custom-theme.md)
   + [Estensioni](store/extensions.md)
   + [Modulo B2B](store/b2b-module.md)
   + [Più siti](store/multiple-sites.md)
   + [Robot di motori di ricerca e mappe del sito](store/robots-sitemap.md)
   + [Metodi di pagamento PayPal](store/paypal.md)
   + [Gestione della configurazione](store/store-settings.md)
+ Sito di lancio {#launch}
   + [Panoramica](launch/overview.md)
   + [Elenco di controllo di Launch](launch/checklist.md)
   + [Passaggi di avvio](launch/steps.md)
+ Monitora sito {#monitor}
   + [Prestazioni](monitor/performance.md)
   + servizio New Relic {#new-relic}
      + [Panoramica di New Relic](monitor/new-relic-service.md)
      + [Gestione di account e utenti](monitor/account-management.md)
      + Esaminare le prestazioni {#investigate}
         + [Criteri, avvisi e flussi di lavoro](monitor/investigate-performance.md)
         + [Acquisizione dei dati](monitor/ingest-data.md)
         + [Tracciare le distribuzioni](monitor/track-deployments.md)
      + [Gestione dei registri](monitor/log-management.md)
