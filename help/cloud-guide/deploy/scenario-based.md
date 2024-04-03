---
title: Distribuzione basata su scenari
description: Scopri come personalizzare le implementazioni di Adobe Commerce sull’infrastruttura cloud utilizzando file di configurazione personalizzati.
feature: Cloud, Configuration, Deploy, Build
exl-id: df243068-c7a3-46dd-abe9-9e05198fa343
source-git-commit: 9b3772cf640ebc56063434e1aa8acb1ec51dc63c
workflow-type: tm+mt
source-wordcount: '808'
ht-degree: 0%

---

# Distribuzione basata su scenari

Con `ece-tools` 2002.1.0 e versioni successive, è possibile utilizzare la funzionalità di distribuzione basata su scenari per personalizzare il comportamento di distribuzione predefinito.
Questa funzione utilizza **scenari** e **passaggi** nella configurazione:

- **Configurazione scenario**-Ogni hook di distribuzione è un *scenario*, file di configurazione XML che descrive la sequenza e i parametri di configurazione necessari per completare le attività di distribuzione. Puoi configurare gli scenari in `hooks` sezione del `.magento.app.yaml` file.

- **Configurazione del passaggio**-Ogni scenario utilizza una sequenza di *passaggi* che descrivono a livello di programmazione le operazioni necessarie per completare le attività di distribuzione. È possibile configurare i passaggi in un file di configurazione dello scenario basato su XML.

Adobe Commerce su infrastruttura cloud fornisce una serie di [scenari predefiniti](https://github.com/magento/ece-tools/tree/2002.1/scenario) e [passaggi predefiniti](https://github.com/magento/ece-tools/tree/2002.1/src/Step) nel `ece-tools` pacchetto. È possibile personalizzare il comportamento di distribuzione creando file di configurazione XML personalizzati per sostituire o personalizzare la configurazione predefinita. Puoi anche utilizzare scenari e passaggi per eseguire il codice dai moduli personalizzati.

## Aggiungere scenari utilizzando gli hook di compilazione e distribuzione

Puoi aggiungere gli scenari per la creazione e la distribuzione di Adobe Commerce al `hooks` sezione del `.magento.app.yaml` file. Ogni hook specifica gli scenari da eseguire durante ogni fase. L’esempio seguente mostra la configurazione dello scenario predefinita.

> `magento.app.yaml` ganci

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!NOTE]
>
>Con il rilascio di `ece-tools` 2002.1.x, è disponibile una nuova [configurazione hook](https://experienceleague.adobe.com/docs/commerce-cloud-service/user-guide/configure/app/properties/hooks-property.html) formato. Il formato legacy da `ece-tools` Le versioni 2002.0.x sono ancora supportate. Tuttavia, è necessario eseguire l’aggiornamento al nuovo formato per utilizzare la funzione di distribuzione basata su scenari.

## Passaggi dello scenario di revisione

Nella configurazione hook, ogni scenario è un file XML che contiene i passaggi per eseguire attività di compilazione, distribuzione o post-distribuzione. Ad esempio, il `scenario/transfer` Il file include tre passaggi: `compress-static-content`, `clear-init-directory`, e `backup-data`

> `scenario/transfer.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="compress-static-content" type="Magento\MagentoCloud\Step\Build\CompressStaticContent" priority="100"/>
    <step name="clear-init-directory" type="Magento\MagentoCloud\Step\Build\ClearInitDirectory" priority="200"/>
    <step name="backup-data" type="Magento\MagentoCloud\Step\Build\BackupData" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="static-content" xsi:type="object" priority="100">Magento\MagentoCloud\Step\Build\BackupData\StaticContent</item>
                <item name="writable-dirs" xsi:type="object"  priority="200">Magento\MagentoCloud\Step\Build\BackupData\WritableDirectories</item>
            </argument>
        </arguments>
    </step>
</scenario>
```

## Estendere uno scenario predefinito

L&#39;esempio seguente estende lo scenario di distribuzione predefinito aggiungendo ulteriori file di configurazione della distribuzione alla configurazione hook.

```yaml
hooks:
  deploy: |
    php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/<vendor-name>/<module-name>/deploy.xml vendor/<vendor-name>/<module-name>/deploy2.xml
```

Durante la distribuzione, gli scenari personalizzati si fondono con lo scenario predefinito basato sulle regole seguenti:

- Gli scenari vengono classificati in base alla loro sequenza nella definizione dell’hook, con l’ultimo scenario elencato che ha la priorità più alta.

  Nell’esempio, gli scenari hanno la seguente priorità:

   1. `vendor/vendor-name/module-name/deploy2.xml`
   1. `vendor/vendor-name/module-name/deploy.xml`
   1. `scenario/deploy.xml` (scenario predefinito o di base)

- I passaggi dello scenario con priorità più alta sostituiscono i passaggi con lo stesso nome negli altri scenari. Vengono aggiunti nuovi passaggi alla configurazione. Le stesse regole si applicano a più di due scenari con ogni scenario con priorità da destra a sinistra, ad esempio (C → B → A).

### Rimuovi passaggi predefiniti

È possibile rimuovere i passaggi dagli scenari predefiniti utilizzando `skip` parametro.

Ad esempio, per saltare il `enable-maintenance-mode` e `set-production-mode` passaggi dello scenario di distribuzione predefinito, crea un file di configurazione che include la configurazione seguente.

> `vendor/vendor-name/module-name/deploy-custom-mode-config.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <step name="enable-maintenance-mode" skip="true" />
    <step name="set-production-mode"  skip="true" />
</scenario>
```

Per utilizzare il file di configurazione personalizzato, aggiorna il valore predefinito `.magento.app.yaml` file.

> `.magento.app.yaml` con scenario di distribuzione personalizzato

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-custom-mode-config.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

### Sostituisci passaggi predefiniti

Gli scenari personalizzati possono sostituire i passaggi predefiniti per fornire un’implementazione personalizzata. A tale scopo, utilizzate il nome di default del passo come nome del passo personalizzato.

Ad esempio, nel [scenario di distribuzione predefinito] il `enable-maintenance-mode` il passaggio esegue il valore predefinito [Script PHP EnableMaintenanceMode].

```xml
<step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="300"/>
```

Per ignorare questo passaggio, crea un file di configurazione dello scenario personalizzato per eseguire uno script diverso quando `enable-maintenance-mode` esecuzione di un passaggio.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="VendorName\VendorModule\Step\CustomEnableMaintenanceMode" priority="300"/>
</scenario>
```

### Modificare la priorità del passaggio

Gli scenari personalizzati possono modificare la priorità dei passaggi predefiniti. Il passaggio seguente cambia la priorità del `enable-maintenance-mode` passaggio da `300` a `10` in modo che il passaggio venga eseguito prima nello scenario di distribuzione.

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
<scenario>
    <step name="enable-maintenance-mode" type="Magento\MagentoCloud\Step\EnableMaintenanceMode" priority="10"/>
</scenario>
```

In questo esempio, la proprietà `enable-maintenance-mode` step si sposta all’inizio dello scenario perché ha una priorità inferiore rispetto a tutti gli altri passaggi dello scenario di distribuzione predefinito.

### Esempio: estendere lo scenario di distribuzione

L&#39;esempio seguente personalizza [scenario di distribuzione predefinito] con le seguenti modifiche:

- Sostituisce il `remove-deploy-failed-flag` passaggio con un passaggio personalizzato
- Ignora il `clean-redis-cache` substep nel passaggio di pre-distribuzione
- Ignora il `unlock-cron-jobs` passaggio
- Ignora il `validate-config` passaggio per disabilitare le convalide critiche
- Aggiunge un nuovo passaggio di predistribuzione

> `vendor/vendor-name/module-name/deploy-extended.xml`

```xml
<?xml version="1.0"?>
<scenario xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:ece-tools:config/scenario.xsd">
    <!-- Replace "remove-deploy-failed-flag" step with custom step -->
    <step name="remove-deploy-failed-flag" type="Vendor\ModuleName\Step\Deploy\RemoveDeployFailedFlag" priority="100"/>

    <!-- Skip "clean-redis-cache" sub-step in pre-deploy step -->
    <step name="pre-deploy" type="Magento\MagentoCloud\Step\Deploy\PreDeploy" priority="200">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="steps" xsi:type="array">
                <item name="clean-redis-cache" xsi:type="object" skip="true"/>
            </argument>
        </arguments>
    </step>

    <!-- Skip step "unlock-cron-jobs" -->
    <step name="unlock-cron-jobs" skip="true"/>

    <!-- Skip critical validators -->
    <step name="validate-config" type="Magento\MagentoCloud\Step\ValidateConfiguration" priority="300">
        <arguments>
            <argument name="logger" xsi:type="object">Psr\Log\LoggerInterface</argument>
            <argument name="validators" xsi:type="array">
                <item name="critical" xsi:type="array">
                    <item name="database-configuration" xsi:type="object" skip="true"/>
                    <item name="search-configuration" xsi:type="object" skip="true"/>
                </item>
            </argument>
        </arguments>
    </step>

    <!-- Add new step into the beginning of the deploy scenario -->
    <step name="new-pre-deploy-step" type="Vendor\ModuleName\Step\Deploy\PreDeploy" priority="10"/>
</scenario>
```

Per utilizzare questo script nel progetto, aggiungi la seguente configurazione alla `.magento.app.yaml` file per il progetto Adobe Commerce on cloud infrastructure:

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: |
        php ./vendor/bin/ece-tools run scenario/deploy.xml vendor/vendor-name/module-name/deploy-extended.xml
    post_deploy: |
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
```

>[!TIP]
>
>È possibile esaminare [scenari predefiniti](https://github.com/magento/ece-tools/tree/2002.1/scenario) e [configurazione passaggio predefinita](https://github.com/magento/ece-tools/tree/2002.1/src/Step) nel `ece-tools` Archivio GitHub per determinare quali scenari e passaggi personalizzare per le attività di build, distribuzione e post-distribuzione del progetto.

## Aggiungere un modulo personalizzato da estendere `ece-tools`

Il `ece-tools` fornisce interfacce API predefinite che seguono gli standard della versione semantica. Tutte le interfacce API sono contrassegnate con **@api** annotazione. Puoi sostituire l’implementazione API predefinita con la tua creando un modulo personalizzato e modificando il codice predefinito in base alle esigenze.

Per utilizzare il modulo personalizzato con Adobe Commerce sull’infrastruttura cloud, è necessario registrare il modulo nell’elenco delle estensioni per `ece-tools` pacchetto. Il processo di registrazione è simile a quello utilizzato per registrare i moduli in Adobe Commerce.

**Per registrare un modulo con `ece-tools` pacchetto**:

1. Creare o estendere `registration.php` nella directory principale del modulo.

   ```php?start_inline=1
   \Magento\MagentoCloud\ExtensionRegistrar::register('module-name', __DIR__);
   ```

1. Aggiornare il `autoload` sezione del file di configurazione del modulo da includere `registration.php` file per caricare automaticamente i file del modulo in `composer.json`.

   ```json
   {
     "name": "vendor/ece-tools-extend",
     "description": "Extension for ece-tools",
     "type": "magento2-component",
     "version": "1.0.0",
     "license": "OSL-3.0",
     "autoload": {
       "files": [ "registration.php" ],
       "psr-4": {
         "Vendor\\EceToolExtend\\": "src/"
       }
     }
   }
   ```

1. Aggiungi il `config/services.xml` nel modulo. Questa configurazione viene unita `config/services.xml` da `ece-tools` pacchetto.

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <container xmlns="http://symfony.com/schema/dic/services"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://symfony.com/schema/dic/services https://symfony.com/schema/dic/services/services-1.0.xsd">
       <services>
           <defaults autowire="true" autoconfigure="true" public="true"/>
   
           <prototype namespace="Vendor\EceToolExtend\" resource="../src/*" exclude="../src/{Test}"/>
   
           <!-- Use your own implementation of EnvironmentDataInterface -->
           <service id="Magento\MagentoCloud\Config\EnvironmentDataInterface" alias="Vendor\EceToolExtend\Config\CustomEnvironmentData" />
       </services>
   </container>
   ```

Per ulteriori informazioni sull’iniezione di dipendenza, consulta [Iniezione di dipendenza di Symfony](https://symfony.com/doc/current/components/dependency_injection.html).

<!-- link definitions -->

[scenario di distribuzione predefinito]: https://github.com/magento/ece-tools/blob/develop/scenario/deploy.xml
[Script PHP EnableMaintenanceMode]: https://github.com/magento/ece-tools/blob/develop/src/Step/EnableMaintenanceMode.php
