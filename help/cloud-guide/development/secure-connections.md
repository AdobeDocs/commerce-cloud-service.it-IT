---
title: Connessioni sicure
description: Scopri come applicare le chiavi SSH al progetto di infrastruttura cloud Adobe Commerce e accedere agli ambienti remoti.
role: Developer
feature: Cloud, Security
topic: Security
exl-id: b5780e8e-e3da-4b10-8ca3-2778085acd4a
source-git-commit: 13e76d3e9829155995acbb72d947be3041579298
workflow-type: tm+mt
source-wordcount: '976'
ht-degree: 0%

---

# Connessioni sicure agli ambienti remoti

Secure Shell (SSH) è un protocollo comune utilizzato per accedere in modo sicuro ai server e ai sistemi remoti. È possibile utilizzare SSH per accedere agli ambienti remoti per gestire l’applicazione Adobe Commerce e accedere ai registri degli ambienti remoti. Adobe supporta solo connessioni FTP sicure (sFTP) tramite la chiave pubblica SSH. Le connessioni FTP non sono supportate.

## Generare una coppia di chiavi SSH

Crea una coppia di chiavi SSH in ogni computer e area di lavoro che richiede l’accesso al codice sorgente e agli ambienti del progetto. La chiave SSH consente di connettersi a GitHub per gestire il codice sorgente e connettersi ai server cloud senza dover fornire costantemente nome utente e password. Consulta [Connessione a GitHub con SSH](https://docs.github.com/en/authentication/connecting-to-github-with-ssh) per ulteriori istruzioni sulla creazione di una coppia di chiavi SSH.

- Il _chiave pubblica_ è sicuro da fornire per l’accesso a un sito, SSH e sFTP.
- Il _chiave privata_ rimane privato sulla workstation.

>[!CAUTION]
>
>**Non condividere mai la tua chiave privata.** Non aggiungerlo a un ticket, copiarlo in una chat o allegarlo alle e-mail.

## Aggiungi una chiave pubblica SSH al tuo account

Dopo aver aggiunto la chiave pubblica SSH all’account Adobe Commerce sull’infrastruttura cloud, ridistribuisci tutti gli ambienti attivi sull’account per installare la chiave.

Puoi aggiungere chiavi SSH al tuo account utilizzando uno dei seguenti metodi: Cloud CLI o [!DNL Cloud Console].

>[!BEGINTABS]

>[!TAB CLI]

### Aggiungere la chiave SSH utilizzando Cloud CLI

1. Sulla workstation locale, passa alla directory del progetto.

1. Accedi al progetto:

   ```bash
   magento-cloud login
   ```

1. Aggiungi la chiave pubblica.

   ```bash
   magento-cloud ssh-key:add ~/.ssh/id_rsa.pub
   ```

>[!TIP]
>
>Puoi elencare ed eliminare le chiavi SSH utilizzando i comandi CLI di Cloud `ssh-key:list` e `ssh-key:delete`.

>[!TAB Console]

### Aggiungi la chiave SSH utilizzando [!DNL Cloud Console]

**Per aggiungere una chiave SSH a un nuovo progetto**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Clic **[!UICONTROL No SSH key]**. Questa icona si trova a destra del campo del comando ed è visibile quando il progetto non contiene una chiave SSH.

1. Copiare e incollare il contenuto della chiave SSH pubblica in **Chiave pubblica** campo.

1. Seguire le istruzioni rimanenti.

**Per aggiungere una chiave SSH al profilo Cloud**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Nel menu dell’account in alto a destra, fai clic su **Il mio profilo**.

1. In _Chiavi SSH_ visualizza, fai clic su **Aggiungi chiave pubblica**.

1. In _Aggiungere una chiave SSH_ modulo, assegna alla chiave un **Titolo**, e incolla la chiave SSH pubblica nel **Chiave** campo.

1. Clic **Salva**.

>[!ENDTABS]

## Connessione a un ambiente remoto

È possibile connettersi a un ambiente remoto utilizzando `magento-cloud` CLI o un comando SSH. Il `magento-cloud` I comandi CLI possono essere utilizzati solo negli ambienti di integrazione Starter e Pro.

### Utilizzare Cloud CLI

**Per accedere a un ambiente di integrazione remota**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Elencare gli ambienti in tale progetto.

   ```bash
   magento-cloud environment:list -p <project-ID>
   ```

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh -p <project-ID> -e <environment-ID>
   ```

### Utilizzare un comando SSH

Il [!DNL Cloud Console] include un elenco di comandi di accesso Web e SSH per ogni ambiente.

**Per copiare il comando SSH**:

1. Accedi a [[!DNL Cloud Console]](https://console.adobecommerce.com).

1. Seleziona un progetto da _Tutti i progetti_ elenco.

1. Seleziona un ambiente.

1. Clic **[!UICONTROL SSH]**.

1. In _SSH_ , fare clic sul pulsante Copia per copiare il comando SSH completo negli Appunti.

1. Apri un terminale e incolla il comando SSH per creare una connessione.

   ```bash
   ssh abcdefg123abc-branch-a12b34c--mymagento@ssh.us-2.magento.cloud
   ```

>[!TIP]
>
>Per gli ambienti Pro Staging e Production, il comando SSH potrebbe essere simile al seguente:
>
>```bash
>ssh <node>.ent-<project-ID>-<environment>-<user-ID>@ssh.<region>.magento.com
>```

## sFTP

L’infrastruttura cloud di Adobe Commerce supporta l’accesso agli ambienti utilizzando sFTP (Secure FTP) con autenticazione SSH. Utilizza un client che supporta l’autenticazione della chiave SSH per sFTP e utilizza la tua chiave SSH pubblica. La chiave SSH pubblica deve essere aggiunta all’ambiente di destinazione. Per gli ambienti Starter e gli ambienti di integrazione Pro, è possibile [aggiungilo tramite il [!DNL Cloud Console]](#add-your-ssh-key-using-the-project-web-interface).

Le connessioni sFTP di sola lettura sono _non_ supportato; l’accesso sFTP viene fornito con _scrivere_ per impostazione predefinita.

Durante la configurazione di sFTP, utilizza le informazioni del comando dell’ambiente di accesso SSH: `<project-id>-<environment-id>--<app-name>@ssh<cloud-host>`

- **Nome utente**: tutto il contenuto prima del `@` nella destinazione di accesso SSH.
- **Password**: non è necessaria una password per sFTP. L’accesso sFTP utilizza l’autenticazione con chiave SSH.
- **Host**: tutto il contenuto dopo il `@` nell’accesso SSH.
- **Porta**: 22, porta SSH predefinita.
- **SSH** Chiave privata: se necessario, fornisci al client sFTP la posizione della chiave privata. Per impostazione predefinita, le chiavi private sono memorizzate in `~/.ssh` directory.

A seconda del client, potrebbero essere necessarie opzioni aggiuntive per completare l’autenticazione SSH per sFTP. Consulta la documentazione del client selezionato.

Per **Ambienti Starter e ambienti di integrazione Pro**, è inoltre possibile prendere in considerazione [aggiunta di un `mount`](../application/properties.md#mounts) per accedere a una directory specifica. Aggiungere il mount al `.magento.app.yaml` file. Per un elenco delle directory scrivibili, vedere [Struttura del progetto](../project/file-structure.md). Questo punto di montaggio funziona solo in questi ambienti.

Per **Ambienti Pro Staging e Produzione**, se non hai accesso SSH all’ambiente, devi [invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per richiedere l’accesso sFTP e un punto di montaggio per accedere alla cartella specifica, ad esempio `pub/media`.

>[!NOTE]
>Per Pro Staging e Produzione, se la connessione sFTP è per un _generico_ utente che esegue **non** devono essere [aggiunto al progetto Cloud](../project/user-access.md), è necessario [invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) con il loro **pubblico** chiave collegata. **Non fornire mai la tua chiave SSH privata.**

## Tunneling SSH

È possibile utilizzare il tunneling SSH per connettersi a un servizio dall’ambiente di sviluppo locale come se il servizio fosse locale. Prima di eseguire il tunneling, configura [SSH](#add-an-ssh-public-key-to-your-account).

Utilizza un’applicazione terminale per accedere e inviare comandi.

```bash
magento-cloud login
```

Verifica se sono aperti dei tunnel utilizzando.

```bash
magento-cloud tunnel:list
```

Per costruire un tunnel, è necessario conoscere [nome applicazione](../application/properties.md#name). È possibile controllare il nome dell&#39;applicazione utilizzando CLI:

```bash
magento-cloud apps
```

### Configurare il tunnel SSH

```bash
magento-cloud tunnel:open -e <environment-ID> --app <app-name>
```

Ad esempio, per aprire un tunnel per `sprint5` ramo in un progetto con un’app denominata `mymagento`, immetti

```bash
magento-cloud tunnel:open -e sprint5 --app mymagento
```

Risposta di esempio:

```terminal
SSH tunnel opened on port 30004 to relationship: redis
SSH tunnel opened on port 30005 to relationship: database
Logs are written to: /home/magento_user/.magento/tunnels.log

List tunnels with: magento-cloud tunnels
View tunnel details with: magento-cloud tunnel:info
Close tunnels with: magento-cloud tunnel:close
```

**Per visualizzare informazioni sul tunnel**:

```bash
magento-cloud tunnel:info -e <environment-ID>
```

### Connetti ai servizi

Dopo aver stabilito un tunnel SSH, è possibile connettersi ai servizi come se si stesse eseguendo localmente. Per connettersi al database, ad esempio, utilizzare il comando seguente:

```bash
mysql --host=127.0.0.1 --user='<database-username>' --pass='<user-password>' --database='<name>' --port='<port>'
```
