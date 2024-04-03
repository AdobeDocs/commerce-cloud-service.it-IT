---
title: Abilita l’autenticazione a più fattori per l’accesso SSH
description: Scopri come gestire i requisiti di autenticazione per l’accesso SSH ad Adobe Commerce negli ambienti dell’infrastruttura cloud.
feature: Cloud, Security
topic: Security
exl-id: 754b2c22-f197-49be-a699-fb3bedf053fc
source-git-commit: ec1e59c3aafae6452ad1590fdb9de37c68b94ed9
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 0%

---

# Abilita l’autenticazione a più fattori per l’accesso SSH

Per una maggiore sicurezza, Adobe Commerce sull’infrastruttura cloud fornisce l’applicazione dell’autenticazione a più fattori (MFA) per gestire i requisiti di autenticazione per l’accesso SSH agli ambienti cloud.

Quando MFA è abilitato in un progetto, tutti gli account utente con accesso SSH richiedono un codice TFA (Two-Factor Authentication) o un token API e un certificato SSH per accedere all’ambiente.

>[!NOTE]
>
>Per impostazione predefinita, l’autenticazione MFA non è abilitata nei progetti Cloud. Il proprietario dell’account per il progetto di infrastruttura cloud di Adobe Commerce deve [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per abilitarlo. Quando MFA è abilitato, tutti gli utenti devono avere l’autenticazione a due fattori abilitata sul proprio account Adobe Commerce on Cloud Infrastructure per accedere SSH agli ambienti del progetto.

## Certificati per accesso SSH

MFA consente agli utenti di scambiare un token di accesso OAUTH con un certificato SSH di breve durata generato dall’API Adobe Cloud Certifier. Se l’utente ha il ruolo di Amministratore o Collaboratore, una chiave SSH valida e un codice TFA o un token API valido, Adobe Commerce on Cloud Infrastructure utilizza queste credenziali per generare il certificato SSH temporaneo. La scadenza del certificato è impostata su un&#39;ora, ma viene aggiornata automaticamente durante la sessione corrente.

Dopo l’accesso a un progetto con MFA, gli utenti devono utilizzare `magento-cloud` CLI per generare il certificato SSH:

```bash
magento-cloud ssh-cert:load
```

Il `ssh-cert:load` Il comando genera il certificato SSH e lo installa nell&#39;agente SSH dell&#39;utente locale.

### Genera automaticamente certificato all&#39;accesso

È possibile configurare l’ambiente locale per generare automaticamente il certificato SSH quando si esegue l’autenticazione in `magento-cloud` CLI

**Per aggiungere la generazione automatica del certificato SSH al `magento-cloud` Configurazione CLI**:

1. Nella workstation locale, crea un file denominato `config.yaml` nel `.magento-cloud` nella home directory, se non esiste.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Aggiungi la seguente configurazione a `config.yaml` file.

   ```yaml
   api:
      auto_load_ssh_cert: true
   ```

1. Utilizza il `magento-cloud` CLI per eseguire nuovamente l&#39;autenticazione:

   >Disconnetti:

   ```bash
   magento-cloud logout
   ```

   >Accesso:

   ```bash
   magento-cloud login
   ```

   >Segui la risposta:

   ```terminal
   Please open the following URL in a browser and log in:
   http://127.0.0.1:5000
   
   Help:
     Leave this command running during login.
     If you need to quit, use Ctrl+C.
   
     To log in using an API token, run: magento-cloud auth:api-token-login
   
   Login information received. Verifying...
   You are logged in.
   
   Generating SSH certificate...
   A new SSH certificate has been generated.
   It will be automatically refreshed when necessary.
   The certificate is included in your SSH configuration: /Users/<user-name>/.ssh/config
   ```

## Connettersi a un ambiente utilizzando SSH con TFA

Quando MFA è abilitato in un progetto, è necessario che TFA sia abilitato sul proprio account prima di poter connettersi a un ambiente remoto utilizzando un SSH. Consulta [Abilita TFA](user-access.md#enable-tfa-for-cloud-accounts).

>[!BEGINSHADEBOX]

**Prerequisiti:**

Per i progetti abilitati con l’applicazione MFA, l’accesso SSH richiede le seguenti autorizzazioni e impostazioni account:

- [Accesso all’ambiente da parte dell’amministratore o del collaboratori](user-access.md)
- [Chiave di accesso SSH configurata per l’account](../development/secure-connections.md#add-an-ssh-public-key-to-your-account)
- [TFA abilitato per l&#39;account](user-access.md#enable-tfa-for-cloud-accounts)

>[!ENDSHADEBOX]

**Per connettersi utilizzando SSH con le credenziali dell’account utente TFA**:

1. Accedi a [account personale](https://console.adobecommerce.com).

1. Sulla workstation locale, utilizzare `magento-cloud` CLI per generare il certificato SSH.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Risposta di esempio:

   ```terminal
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Utilizzare un SSH per connettersi all&#39;ambiente remoto.

   ```bash
   ssh abcdef7uyxabce-master-7rqtwti--mymagento@ssh.us-5.magento.cloud
   ```

   ```terminal
    __  __                   _          ___ _             _
   |  \/  |__ _ __ _ ___ _ _| |_ ___   / __| |___ _  _ __| |
   | |\/| / _` / _` / -_) ' \  _/ _ \ | (__| / _ \ || / _` |
   |_|  |_\__,_\__, \___|_||_\__\___/  \___|_\___/\_,_\__,_|
               |___/
   
    Welcome to Magento Cloud.
   
    This is environment master-7rqtwti
    of project abcdef7uyxabce.
   
   web@mymagento.0:~$
   ```

## Gestire il codice sorgente utilizzando SSH con TFA

Quando gestisci il codice sorgente per i progetti di infrastruttura cloud di Adobe Commerce, utilizzi SSH per l’autenticazione nell’archivio Git del progetto. Se nel progetto è abilitata l’imposizione MFA, è necessario generare un certificato SSH prima di poter eseguire operazioni della riga di comando utilizzando l’archivio Git.

**Per connettersi utilizzando SSH con le credenziali dell’account utente TFA**:

1. Accedi a [account personale](https://console.adobecommerce.com) e di eseguire l’autenticazione utilizzando TFA.

   >[!NOTE]
   >
   >Se sul tuo account non è abilitato TFA, devi attivarlo. Consulta [Abilitare TFA sugli account cloud](user-access.md#enable-tfa-for-cloud-accounts).

1. Sulla workstation locale, utilizzare `magento-cloud` CLI per generare il certificato SSH.

   ```bash
   magento-cloud ssh-cert:load
   ```

   > Risposta di esempio:

   ```terminal
   Generating SSH certificate...
     Expires at: 2020-07-13T15:28:13-04:00
     Multi-factor authentication: verified
     Mode: interactive
   The certificate will be automatically refreshed when necessary.
   Checking SSH configuration file: /Users/<user-name>/.ssh/config
   Do you want to update the file automatically? [Y/n] Y
   Configuration file updated successfully: /Users/<user-name>/.ssh/config
   ```

1. Clona l’archivio Git per l’ambiente del progetto:

   ```bash
   git clone --branch integration abcdef7uyxabce@git.us-3.magento.cloud:abcdef7uyxabce.git myproject
   ```

   > Risposta di esempio:

   ```terminal
   Cloning into 'myproject'...
   Connection to git.us-3.magento.cloud port 22 [tcp/ssh] succeeded!
   remote: counting objects: 22, done.
   Receiving objects: 100% (22/22), 82.42 KiB | 16.48 MiB/s, done.
   ```

## Connessione a un ambiente tramite SSH con un token API

Quando MFA è abilitato in un progetto, i processi automatizzati che richiedono l’accesso SSH a un ambiente Cloud richiedono un token API. Puoi generare il token da un account Adobe Commerce su infrastruttura cloud con accesso amministratore o collaboratore sul progetto.

L’autenticazione con un token API richiede ancora la generazione di un certificato SSH. I processi automatizzati devono inoltre automatizzare la generazione di un certificato SSH.

>[!BEGINSHADEBOX]

**Prerequisiti:**

- [Accesso amministratore o collaboratore all’ambiente dell’infrastruttura cloud di Adobe Commerce](user-access.md)
- [Token API valido disponibile sull’account](user-access.md#create-an-api-token)

>[!ENDSHADEBOX]

**Per connettersi utilizzando SSH con credenziali token API**:

1. Accedi al progetto Cloud utilizzando l’autenticazione con chiave API.

   ```bash
   magento-cloud auth:api-token
   ```

1. Al prompt, immetti il valore per un token API valido.

   ```terminal
   Please enter an API token:
   >
   
   The API token is valid.
   You are logged in.
   ```

### Esempio: script SSH automatizzato

Sono disponibili due opzioni per l’archiviazione del token API.

>[!NOTE]
>
>Se viene memorizzato un token API, il `magento-cloud` CLI esegue automaticamente l&#39;autenticazione e non è necessario eseguire `magento-cloud login` comando.

**Opzione 1**: crea una variabile di ambiente per memorizzare il token API

Scrivi il token sul tuo profilo_base

```bash
echo "export MAGENTO_CLOUD_CLI_TOKEN=<your api token>" >> ~/.bash_profile
```

**Opzione 2**: aggiungi il token al `config.yaml` file

1. Nella workstation locale, crea un file denominato `config.yaml` nel `.magento-cloud` nella home directory, se non esiste.

   ```bash
   touch ~/.magento-cloud/config.yaml
   ```

1. Aggiungi la seguente configurazione a `config.yaml` file.

   ```yaml
   api:
      token: <your api token>
   ```

>Script di base di esempio

```shell
#!/bin/bash
magento-cloud ssh-cert:load
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud "tail -n 10 ~/var/log/cloud.log"
```

## Risoluzione dei problemi

Utilizza le seguenti informazioni per risolvere gli errori delle richieste di connessione SSH dovuti a errori di autenticazione come `access requires MFA` o `permission denied`.

### La richiesta non fornisce un certificato valido

Se la richiesta non fornisce un certificato valido, viene visualizzato un messaggio simile al seguente:

```terminal
to Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully
authenticated, but could not connect to service abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud:>
(reason: access requires MFA)
```

Per risolvere il problema di connessione, provare le procedure di risoluzione dei problemi seguenti:

- Verifica la configurazione TFA dell’account
- Esegui di nuovo l&#39;autenticazione, quindi ricarica il certificato

**Verificare la configurazione e l’autenticazione TFA**:

1. Accedi a [account personale](https://console.adobecommerce.com).

1. Nel menu dell’account in alto a destra, fai clic su **[!UICONTROL My Profile]**.

1. Il giorno _Il mio profilo_ , fare clic su **[!UICONTROL Security]** scheda.

   Se TFA è abilitato, la sezione Sicurezza fornisce opzioni per gestire la configurazione TFA.

1. Se TFA non è impostato, fai clic su **[!UICONTROL Set up application]** e seguire le istruzioni per abilitarlo. Consulta [Abilita TFA](user-access.md#enable-tfa-for-cloud-accounts).

1. Se TFA è configurato, prova a eseguire di nuovo l’autenticazione.

**Per autenticare e ricaricare il certificato SSH**:

1. Utilizza il `magento-cloud` CLI per eseguire nuovamente l&#39;autenticazione:

   ```bash
   magento-cloud logout
   ```

   ```bash
   magento-cloud login
   ```

1. Ricarica il certificato SSH:

   ```bash
   magento-cloud ssh-cert:load
   ```

### Autorizzazione negata

Se la chiave SSH è mancante o non valida, la richiesta di connessione SSH restituisce un `Permission denied (publickey)` errore.

```terminal
Hello user-test (UUID: abaacca12-5cd1-4b123-9096-411add578998), you successfully authenticated, but could not connect to service oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento (reason: service doesn't exist or you do not have access to it)
oh2wi6klp5ytk-mc-35985-integration-nnulm4a--mymagento@ssh.eu-3.magento.cloud: Permission denied (publickey).
```

Per risolvere il problema, aggiungi la chiave SSH alla sessione corrente oppure aggiorna il file di configurazione SSH per caricare automaticamente le chiavi SSH. Consulta [Aggiungi una chiave SSH pubblica](../development/secure-connections.md#add-an-ssh-public-key-to-your-account).

### Impossibile accedere ai progetti senza MFA

Se esegui l’autenticazione in un progetto con Multi-Factor Authentication (MFA) abilitato, potresti ricevere il seguente errore durante la connessione ad altri progetti che non richiedono MFA:

```bash
ssh abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud
```

Risposta di esempio:

```terminal
abcdef7uyxabce-master-7rqtabc--mymagento@ssh.us-3.magento.cloud: Permission denied (publickey).
```

Durante la generazione del certificato SSH, il `magento-cloud` CLI aggiunge una chiave SSH aggiuntiva all’ambiente locale. Tale chiave viene utilizzata per impostazione predefinita se la configurazione SSH locale non include la chiave SSH per l’accesso al progetto.

**Per aggiungere la chiave SSH alla configurazione locale**:

1. Creare `config` se non esiste.

   ```bash
   touch ~/.ssh/config
   ```

1. Aggiungi un `IdentityFile` configurazione.

   ```yaml
   Host *
     IdentityFile ~/.ssh/id_rsa
   ```

>[!NOTE]
>
>È possibile specificare più chiavi SSH aggiungendo più chiavi `IdentityFile` voci nella configurazione.
