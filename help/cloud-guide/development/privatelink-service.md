---
title: Servizio PrivateLink
description: Scopri come utilizzare il servizio PrivateLink per stabilire una connessione sicura tra un cloud privato e la piattaforma cloud Adobe Commerce nella stessa area geografica.
feature: Cloud, Iaas, Security
exl-id: b25548b8-312b-4a74-b242-f4e2ac6cf945
source-git-commit: b49a51aba56f79b5253eeacb1adf473f42bb8959
workflow-type: tm+mt
source-wordcount: '1609'
ht-degree: 0%

---

# Servizio PrivateLink

Adobe Commerce sull&#39;infrastruttura cloud supporta l&#39;integrazione con il servizio [AWS PrivateLink](https://aws.amazon.com/privatelink/) o [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/). È possibile utilizzare PrivateLink per stabilire comunicazioni private sicure tra Adobe Commerce in ambienti di infrastruttura cloud con servizi e applicazioni ospitati su sistemi esterni. Sia l’applicazione Adobe Commerce che i sistemi esterni devono essere accessibili tramite endpoint Virtual Private Cloud (VPC) configurati sulla stessa piattaforma cloud (AWS o Azure) all’interno della stessa area cloud.

>[!TIP]
>
>PrivateLink è indicato per la protezione delle connessioni per integrazioni non HTTP(S), ad esempio trasferimenti di database o file. Se prevedi di integrare l&#39;applicazione con le API di Adobe Commerce, consulta la sezione su come creare una [Mesh API Adobe](https://developer.adobe.com/graphql-mesh-gateway/gateway/create-mesh/) in _Mesh API per Adobe Developer App Builder_.

## Funzioni e supporto

L’integrazione del servizio PrivateLink per i progetti di infrastruttura cloud di Adobe Commerce include le seguenti funzioni e supporto:

- Una connessione sicura tra un Virtual Private Cloud (VPC) del cliente e il VPC Adobe sulla stessa piattaforma cloud (AWS o Azure) all’interno della stessa area cloud.
- Supporto per la comunicazione unidirezionale o bidirezionale tra i servizi endpoint disponibili presso Adobe e le VPC del cliente.
- Abilitazione servizio:

   - Aprire le porte richieste nell’ambiente Adobe Commerce su infrastruttura cloud
   - Stabilire la connessione iniziale tra il cliente e i VPC Adobi
   - Risoluzione dei problemi di connessione durante l’abilitazione

## Limitazioni

- Il supporto per PrivateLink è disponibile solo negli ambienti Pro Production e Staging. Non è disponibile in ambienti locali o di integrazione, né in progetti iniziali.
- Impossibile stabilire connessioni SSH utilizzando PrivateLink. Vedere [Abilitare le chiavi SSH](secure-connections.md).
- Il supporto Adobe Commerce non copre la risoluzione dei problemi di AWS PrivateLink oltre l’abilitazione iniziale.
- I clienti sono responsabili dei costi associati alla gestione del proprio VPC.
- Non è possibile utilizzare il protocollo HTTPS (porta 443) per connettersi ad Adobe Commerce sull&#39;infrastruttura cloud tramite Azure Private Link a causa di [Cloaking Fastly Origine](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/faq/fastly-origin-cloaking-enablement-faq.html). Questa limitazione non si applica ad AWS PrivateLink.
- PrivateDNS non disponibile.

## Tipi di connessione PrivateLink

Sono disponibili due tipi di connessione PrivateLink, illustrati nel diagramma di rete seguente, per stabilire una comunicazione sicura tra il tuo store e i sistemi esterni ospitati al di fuori dell’ambiente Cloud.

![Diagramma rete PrivateLink](../../assets/privatelink-architecture-diagram.png)

Scegli uno dei tipi di connessione PrivateLink più adatti al tuo Adobe Commerce negli ambienti dell’infrastruttura cloud:

- **Collegamento privato unidirezionale**-Scegliere questa configurazione per recuperare i dati in modo sicuro da un archivio Adobe Commerce sull&#39;infrastruttura cloud.
- **Collegamento privato bidirezionale**-Scegliere questa configurazione per stabilire connessioni sicure da e verso sistemi esterni all&#39;ambiente Adobe Commerce nell&#39;infrastruttura cloud. L&#39;opzione bidirezionale richiede due connessioni:

   - Connessione tra il VPC del cliente e il VPC dell&#39;Adobe
   - Connessione tra il VPC Adobe e il VPC cliente

>[!TIP]
>
>Rivolgiti all’amministratore di rete o al provider di piattaforme Cloud per ricevere assistenza nella selezione del tipo di connessione PrivateLink o nella configurazione e amministrazione di VPC. Consulta la documentazione di Cloud Platform PrivateLink: [AWS PrivateLink](https://aws.amazon.com/privatelink/) o [Azure Private Link](https://learn.microsoft.com/en-us/azure/private-link/).

## Richiedi abilitazione PrivateLink

>[!WARNING]
>
>L&#39;abilitazione di PrivateLink può richiedere fino a _cinque_ giorni lavorativi. Fornire informazioni incomplete o imprecise può ritardare il processo.

### Prerequisiti

![controllare](../../assets/fix.svg) un account cloud (AWS o Azure) nella stessa area dell&#39;istanza di Adobe Commerce sull&#39;infrastruttura cloud.

![controllare](../../assets/fix.svg) un VPC nell&#39;ambiente del cliente che ospita i servizi per la connessione tramite PrivateLink. Consulta la documentazione di AWS o Azure per assistenza sulla configurazione VPC o contatta l’amministratore di rete.

![verifica](../../assets/fix.svg) Per le connessioni PrivateLink bidirezionali, è necessario creare la configurazione del servizio endpoint per l&#39;applicazione o il servizio e creare un endpoint nell&#39;ambiente VPC prima di richiedere l&#39;abilitazione di PrivateLink. Vedi [Configurazione per connessioni PrivateLink bidirezionali](#set-up-for-bidirectional-privatelink-connections).

Raccogli i seguenti dati necessari per l’abilitazione di PrivateLink:

- **Numero account cloud cliente** (AWS o Azure): deve trovarsi nella stessa area dell&#39;istanza di Adobe Commerce sull&#39;infrastruttura cloud
- **Area cloud**: specifica l&#39;area cloud in cui è ospitato l&#39;account a scopo di verifica
- **Porte dei servizi e delle comunicazioni**. L&#39;Adobe deve aprire le porte per abilitare la comunicazione dei servizi tra VPC, ad esempio la porta SQL 3306 e la porta SFTP 2222
- **ID progetto**: specifica l&#39;ID progetto Pro di Adobe Commerce su infrastruttura cloud. È possibile ottenere l&#39;ID progetto e altre informazioni sul progetto utilizzando il comando [Cloud CLI](../dev-tools/cloud-cli-overview.md) seguente: `magento-cloud project:info`
- **Tipo di connessione**—Specificare unidirezionale o bidirezionale per il tipo di connessione
- **Servizio endpoint** - Per le connessioni PrivateLink bidirezionali, fornire l&#39;URL DNS per il servizio endpoint VPC a cui l&#39;Adobe deve connettersi, ad esempio: `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`
- **Accesso al servizio endpoint concesso**. Per connettersi al servizio esterno, consentire l&#39;accesso al servizio endpoint all&#39;entità account AWS seguente: `arn:aws:iam::402592597372:root`

  >[!WARNING]
  >
  >Se non viene fornito l&#39;accesso al servizio endpoint, la connessione bidirezionale PrivateLink al servizio nel tuo VPC è **non** aggiunta, il che ritarda la configurazione.

#### Prerequisiti aggiuntivi specifici per l’abilitazione di Azure Private Link

- Specificare l&#39;ID cluster. Utilizzando SSH, accedere al remoto e utilizzare il comando: `cat /etc/platform_cluster`
- Per la connessione di un servizio esterno al cluster Adobe Commerce Pro è necessario:

   - Elenco di porte del cluster Pro da esporre al nuovo endpoint privato esterno
   - Elenco degli ID di sottoscrizione di Azure per le connessioni degli endpoint privati

- Per collegare il cluster Adobe Commerce Pro a un servizio esterno, è necessario:

   - Elenco di ID di risorse per i servizi di destinazione. Gli ID del servizio External Private Link hanno un aspetto simile a quanto segue:

  ```text
  /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/privateLinkServices/{svcNameID}
  ```

### Flusso di lavoro di attivazione

Il seguente flusso di lavoro illustra il processo di abilitazione per l’integrazione di PrivateLink con Adobe Commerce sull’infrastruttura cloud.

1. **Il cliente** invia un ticket di supporto richiedendo l&#39;abilitazione di PrivateLink con l&#39;oggetto `PrivateLink support for <company>`. Includi i [dati necessari per l&#39;abilitazione](#prerequisites) nel ticket. L’Adobe utilizza il ticket di supporto per coordinare la comunicazione durante il processo di abilitazione.

1. **L&#39;Adobe** abilita l&#39;accesso dell&#39;account cliente al servizio endpoint nel VPC di Adobe.

   - Aggiorna la configurazione del servizio endpoint Adobe per accettare le richieste avviate dall’account AWS o Azure del cliente.
   - Aggiornare il ticket di supporto per fornire il nome del servizio per l&#39;endpoint VPC Adobe a cui connettersi, ad esempio `com.amazonaws.vpce.<cloud-region>.vpce-svc-<service-id>`.

1. **Il cliente** aggiunge il servizio endpoint Adobe al proprio account Cloud (AWS o Azure), che attiva una richiesta di connessione a Adobe. Per istruzioni, consulta la documentazione della piattaforma Cloud:

   - Per AWS, vedere [Accettazione e rifiuto delle richieste di connessione dell&#39;endpoint di interfaccia].
   - Per Azure, vedere [Gestire le richieste di connessione].

1. **Adobe** approva la richiesta di connessione.

1. Dopo l&#39;approvazione della richiesta di connessione, **il cliente** [verifica la connessione](#test-vpc-endpoint-service-connection) tra il proprio VPC e il VPC Adobe.

1. Passaggi aggiuntivi per abilitare le connessioni bidirezionali:

   - **Adobe** fornisce l&#39;entità principale dell&#39;account Adobe (utente root per l&#39;account AWS o Azure) e richiede l&#39;accesso al servizio endpoint VPC del cliente.
   - **Cliente** abilita l&#39;accesso Adobe al servizio endpoint nel VPC del cliente. Ciò presuppone che l&#39;entità account Adobe abbia accesso a `arn:aws:iam::402592597372:root`, come descritto in precedenza nel prerequisito **Accesso al servizio endpoint concesso**.

      - Aggiorna la configurazione del servizio endpoint cliente per accettare le richieste avviate dall&#39;account Adobe. Per istruzioni, consulta la documentazione della piattaforma Cloud:

         - Per AWS, consulta [Aggiunta e rimozione delle autorizzazioni per il servizio endpoint].
         - Per Azure, vedere [Gestire una connessione a un endpoint privato]

      - Fornisci un Adobe con il nome del servizio endpoint per il VPC del cliente.

   - **Adobe** aggiunge il servizio endpoint cliente all&#39;account piattaforma Adobe (AWS o Azure), che attiva una richiesta di connessione al VPC del cliente.
   - **Il cliente** approva la richiesta di connessione di Adobe per completare la configurazione.
   - **Cliente** [verifica la connessione](#test-vpc-endpoint-service-connection) dal VPC Adobe.

## Verifica connessione servizio endpoint VPC

È possibile utilizzare l&#39;applicazione Telnet per verificare la connessione al servizio endpoint VPC.

**Per verificare la connessione al servizio endpoint VPC**:

1. Dalla directory principale del progetto, **estrai** l&#39;ambiente di staging o produzione configurato per accedere al servizio endpoint PrivateLink.

   ```bash
   magento-cloud environment:checkout <environment-id>
   ```

1. Esegui il seguente comando CURL:

   ```bash
   curl -v telnet://<endpoint-service-dns-url>:<port>/
   ```

   Esempio:

   ```
   $ curl -v telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80 -vvv
   ```

   Esempio di risposta corretta:

   ```
   * Rebuilt URL to: telnet://vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce.amazonaws.com:80
   * Connected to vpce-0088d56482571241d-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.us-east-1.vpce. amazonaws.com (191.210.82.246) port 80 (#0)
   ```

   Risposta di esempio non riuscita:

   ```
   Failed to connect to vpce-007ffnb9qkcnjgult-yfhmywqh.vpce-svc-083cqvm2ta3rxqat5v.ap-southeast-1.vpce.amazonaws.com port 80: Connection timed out
   * Closing connection 0
   ```

1. Verificare che il servizio sia in ascolto sulla macchina virtuale.

   ```bash
   netstat -na | grep <port>
   ```

1. Controlla il flusso dei pacchetti.

   ```bash
   tcpdump -i <ethernet-interface> -tt -nn port <destination-port> and host <source-host>
   ```

   Controlla le seguenti impostazioni interne per assicurarti che la configurazione sia valida:

   - Impostazioni dei servizi endpoint ed endpoint
   - Impostazioni di Bilanciamento carico di rete
   - I gruppi target in Bilanciamento carico di rete e verificare che siano integri
   - L&#39;URL dell&#39;endpoint netcat/curl di ogni VM (elencato sopra)

   Consulta i seguenti articoli per assistenza nella risoluzione dei problemi di connessione:

   - [AWS: risoluzione dei problemi relativi alle connessioni del servizio endpoint]
   - [Amazon: risoluzione dei problemi di connettività di Azure Private Link]

   Se non riesci a risolvere gli errori, aggiorna il ticket di supporto Adobe Commerce per richiedere assistenza per stabilire la connessione.

## Modifica configurazione PrivateLink

[Invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per modificare una configurazione PrivateLink esistente. Ad esempio, puoi richiedere modifiche come segue:

- Rimuovi la connessione PrivateLink dall’ambiente di produzione o staging di Adobe Commerce on cloud infrastructure Pro.
- Modifica il numero di account della piattaforma cloud del cliente per accedere al servizio endpoint Adobe.
- Aggiungere o rimuovere connessioni PrivateLink dal VPC Adobe ad altri servizi endpoint disponibili nell&#39;ambiente VPC del cliente.

## Configurazione per connessioni PrivateLink bidirezionali

Il VPC del cliente deve disporre delle seguenti risorse per supportare connessioni PrivateLink bidirezionali:

- Un Bilanciatore del carico di rete
- Configurazione del servizio endpoint che consente l&#39;accesso a un&#39;applicazione o a un servizio dal VPC del cliente
- Un [endpoint di interfaccia] (AWS) o [endpoint privato] (Azure) che consente ad Adobe di connettersi ai servizi endpoint ospitati nel VPC

Se queste risorse non sono disponibili nel VPC del cliente, devi accedere al tuo account della piattaforma Cloud per aggiungere la configurazione.

- Console VPC Amazon - `https://console.aws.amazon.com/vpc/`
- Portale di Azure - `https://portal.azure.com`

Consulta la documentazione della piattaforma Cloud per le istruzioni di configurazione di PrivateLink:

- **Documentazione di AWS PrivateLink**
   - [Creare un servizio di bilanciamento del carico di rete]
   - [Creare una configurazione del servizio endpoint]
   - [Creare un endpoint di interfaccia]
   - [Ciclo di vita endpoint interfaccia]

- **Documentazione di Azure PrivateLink**
   - [Creare un load balancer]
   - [Flusso di lavoro del collegamento privato di Azure]

<!--Link definitions-->

[Accettazione e rifiuto delle richieste di connessione dell’endpoint di interfaccia]: https://docs.aws.amazon.com/vpc/latest/userguide/accept-reject-endpoint-requests.html
[Aggiunta e rimozione delle autorizzazioni per il servizio endpoint]: https://docs.aws.amazon.com/vpc/latest/userguide/add-endpoint-service-permissions.html
[Amazon: risoluzione dei problemi di connettività di Azure Private Link]: https://docs.microsoft.com/en-us/azure/private-link/troubleshoot-private-link-connectivity
[AWS: risoluzione dei problemi relativi alle connessioni del servizio endpoint]: https://aws.amazon.com/premiumsupport/knowledge-center/connect-endpoint-service-vpc/
[Flusso di lavoro di Azure Private Link]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#workflow
[Creare un load balancer]: https://docs.microsoft.com/en-us/azure/load-balancer/quickstart-load-balancer-standard-public-portal
[Creare un servizio di bilanciamento del carico di rete]: https://docs.aws.amazon.com/elasticloadbalancing/latest/network/create-network-load-balancer.html
[Creare una configurazione del servizio endpoint]: https://docs.aws.amazon.com/vpc/latest/userguide/create-endpoint-service.html
[Creare un endpoint di interfaccia]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint
[interface endpoint lifecycle]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-interface-lifecycle
[endpoint di interfaccia]: https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html
[Gestire una connessione a un endpoint privato]: https://docs.microsoft.com/en-us/azure/private-link/manage-private-endpoint
[Gestire le richieste di connessione]: https://docs.microsoft.com/en-us/azure/private-link/private-link-service-overview#manage-your-connection-requests
[endpoint privato]: https://docs.microsoft.com/en-us/azure/private-link/private-endpoint-overview
