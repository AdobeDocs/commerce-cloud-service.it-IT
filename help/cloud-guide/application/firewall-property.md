---
title: Proprietà firewall
description: Vedi esempi su come configurare la proprietà firewall nel file di configurazione dell’applicazione Commerce.
feature: Cloud, Configuration, Security
exl-id: f169c008-c62a-41b7-a98d-cccd81c7291a
source-git-commit: a8ecebc87bfae5deaf0fc7ff3e7dd3b255fe3f24
workflow-type: tm+mt
source-wordcount: '844'
ht-degree: 0%

---

# Proprietà firewall

>[!IMPORTANT]
>
>Solo progetti iniziali

Per i progetti Starter, la proprietà `firewall` aggiunge un firewall _in uscita_ all&#39;applicazione. Questo firewall non ha alcun effetto sulle richieste in ingresso. Definisce quali `tcp` richieste in uscita possono _lasciare_ un sito Adobe Commerce. Questo filtro è chiamato filtro in uscita. Il firewall in uscita filtra ciò che può uscire, uscire o uscire dal sito. Limitare l’escape aggiunge un potente strumento di sicurezza al server.

## Criteri di restrizione predefiniti

Il firewall fornisce due criteri predefiniti per controllare il traffico in uscita: `allow` e `deny`. Il criterio `allow` _consente_ tutto il traffico in uscita per impostazione predefinita. Il criterio `deny` _nega_ tutto il traffico in uscita per impostazione predefinita. Tuttavia, quando si aggiunge una regola, il criterio predefinito viene ignorato e il firewall blocca **tutto** il traffico in uscita non consentito dalla regola.

Per i piani iniziali, il criterio predefinito è impostato su `allow`. Questa impostazione assicura che tutto il traffico in uscita corrente rimanga sbloccato fino all’aggiunta delle regole di filtro in uscita. Il criterio predefinito può essere impostato su `deny` su richiesta.

**Per verificare il criterio predefinito**:

```bash
magento-cloud p:curl --project PROJECT_ID /settings | grep -i outbound
```

A meno che non sia stato richiesto `deny` per il criterio, il comando dovrebbe mostrare il criterio impostato su `allow`:

```json
"outbound_restrictions_default_policy": "allow"
```

>[!NOTE]
>
>**Rimozione chiave**: quando si aggiunge una regola in uscita, viene bloccato tutto il traffico in uscita ad eccezione dei domini, degli indirizzi IP o delle porte aggiunti alla regola. È quindi importante definire e testare un elenco completo delle uscite prima di aggiungerlo al sito di produzione.

## Opzioni firewall

La configurazione di esempio seguente nel file `.magento.app.yaml` mostra tutte le opzioni di `firewall` che puoi utilizzare per aggiungere regole per il filtro in uscita.

```yaml
firewall:
    outbound:
        - # Common accessed domains
            domains:
                - newrelic.com
                - fastly.com
                - magento.com
                - magentocommerce.com
                - google.com
            ports:
                - 80
                - 443
            protocol: tcp # Can be omitted from rules.

        - # Adobe Stock integration
            domains:
                - account.adobe.com
                - stock.adobe.com
                - console.adobe.io
            ports:
                - 80
                - 443

        - # Payment services
            domains:
                - braintreepayments.com
                - paypal.com
            ports:
                - 80
                - 443

        - # Shipping services
            domains:
                - ups.com
                - usps.com
                - fedex.com
                - dhl.com
            ports:
                - 80
                - 443

        - # Vertex Integrated Address Cleansing
            domains:
                - mgcsconnect.vertexsmb.com
            ports:
                - 80
                - 443

        - # New Relic networks
            ips:
                - 162.247.240.0/22 # US region accounts
                - 185.221.84.0/22 # EU region accounts
            ports:
                - 443

        - # New Relic endpoints
            domains:
                - collector.newrelic.com, # US region accounts
                - collector.eu01.nr-data.net # EU region accounts
            ports:
                - 443

        - # Fastly IP ranges
            ips:
                - 23.235.32.0/20
                - 43.249.72.0/22
                - 103.244.50.0/24
                - 103.245.222.0/23
                - 103.245.224.0/24
                - 104.156.80.0/20
                - 146.75.0.0/16
                - 151.101.0.0/16
                - 157.52.64.0/18
                - 167.82.0.0/17
                - 167.82.128.0/20
                - 167.82.160.0/20
                - 167.82.224.0/20
                - 172.111.64.0/18
                - 185.31.16.0/22
                - 199.27.72.0/21
                - 199.232.0.0/16
            ports:
                - 80
                - 443
```

## Regole di filtro in uscita

Le configurazioni del firewall in uscita sono costituite da regole. Puoi definire tutte le regole necessarie. I requisiti relativi alle norme sono i seguenti.

**Ogni regola:**

- Deve iniziare con un trattino (`-`). L’aggiunta di un commento sulla stessa riga consente di documentare e separare visivamente una regola dalla successiva.
- È necessario definire almeno una delle seguenti opzioni: `domains`, `ips` o `ports`.
- Deve utilizzare il protocollo `tcp`. Poiché si tratta del protocollo predefinito per tutte le regole, puoi ometterlo dalla regola.
- È possibile definire `domains` o `ips`, ma non entrambi nella stessa regola.
- Può includere `yaml` commenti (`#`) e interruzioni di riga per organizzare i domini, gli indirizzi IP e le porte consentiti.

Ogni regola utilizza le seguenti proprietà:

### `domains`

L&#39;opzione `domains` consente un elenco di nomi di dominio completi (FQDN).

Se una regola definisce `domains` ma non `ports`, il firewall consente le richieste di dominio su qualsiasi porta.

### `ips`

L&#39;opzione `ips` consente un elenco di indirizzi IP nella notazione CIDR. È possibile specificare indirizzi IP singoli o intervalli di indirizzi IP.

Per specificare un indirizzo IP singolo, aggiungere il prefisso CIDR `/32` alla fine dell&#39;indirizzo IP:

```
172.217.11.174/32  # google.com
```

Per specificare un intervallo di indirizzi IP, utilizzare il calcolatore da [Intervallo IP a CIDR](https://ipaddressguide.com/cidr).

Se una regola definisce `ips` ma non `ports`, il firewall consente le richieste IP su qualsiasi porta.

### `ports`

L&#39;opzione `ports` consente un elenco di porte da 1 a 65535. Per la maggior parte delle regole nell&#39;esempio, le porte `80` e `443` consentono entrambe le richieste HTTP e HTTPS. Tuttavia, per New Relic, le regole consentono l&#39;accesso solo ai domini e agli indirizzi IP sulla porta `443`, come consigliato nella documentazione di New Relic in [Traffico di rete](https://docs.newrelic.com/docs/new-relic-solutions/get-started/networks/#agents).

Se una regola definisce solo `ports`, il firewall consente l&#39;accesso a tutti i domini e gli indirizzi IP per le porte definite.

>[!NOTE]
>
>La porta `25`, la porta SMTP per l&#39;invio dei messaggi di posta elettronica, è sempre bloccata, senza eccezioni.

### `protocol`

Come accennato, TCP è l&#39;impostazione predefinita e l&#39;unico protocollo consentito per le regole. UDP e le relative porte non sono consentiti. Per questo motivo è possibile omettere l&#39;opzione `protocol` da tutte le regole. Per includerlo comunque, è necessario impostare il valore su `tcp`, come illustrato nella prima regola dell&#39;esempio.

## Ricerca di nomi di dominio da consentire

Per identificare i domini da includere nelle regole di filtro in uscita, utilizzare il comando seguente per analizzare il file `dns.log` del server e visualizzare un elenco di tutte le richieste DNS registrate dal sito:

```shell
awk '($5 ~/query/)' /var/log/dns.log | awk '{print $6}' | sort | uniq -c | sort -rn
```

Questo comando mostra anche le richieste DNS effettuate ma bloccate dalle regole di filtro in uscita. L’output non mostra quali domini sono stati bloccati, ma solo quali richieste sono state effettuate. L’output non mostra le richieste effettuate utilizzando un indirizzo IP.

```
Example output:

97 magento.com
93 magentocommerce.com
88 google.com
70 metadata.google.internal.0
70 metadata.google.internal
65 newrelic.com
56 fastly.com
17 mcprod-0vunku5xn24ip.ap-4.magentosite.cloud
6 advancedreporting.rjmetrics.com
```

I domini, a differenza degli indirizzi IP, sono in genere più specifici e sicuri per il filtro in uscita. Ad esempio, se aggiungi un indirizzo IP per un servizio che utilizza una rete CDN, stai abilitando l’indirizzo IP per la rete CDN, che può essere utilizzata da centinaia o migliaia di altri domini. Con un indirizzo IP si potrebbe consentire l&#39;accesso in uscita a migliaia di altri server.

## Verifica delle regole di filtro in uscita

Dopo aver raccolto e configurato le regole di accesso per i domini e gli indirizzi IP necessari per il sito, è ora di eseguire il push e il test.

Per verificare le regole di filtro in uscita:

1. Creare uno script della shell di `curl` comandi per accedere ai domini e agli indirizzi IP nelle regole. Includi comandi che verificano l’accesso ai domini e agli indirizzi IP da bloccare.

1. Configurare un hook `post_deploy` nel file `.magento.app.yaml` per eseguire lo script.

1. Invia la configurazione `firewall` e lo script di prova al ramo `integration`.

1. Esaminare l&#39;output `post_deploy` dai comandi `curl`.

1. Perfeziona le regole di `firewall`, aggiorna lo script di `curl`, esegui il commit, invia e ripeti.

### Esempio di script `curl`

```shell
# curl-tests-for-egress-filtering.sh

# Use the -v option to display connection details

# Check domain access
curl -v newrelic.com
curl -v fastly.com
curl -v magento.com
curl -v magentocommerce.com
curl -v google.com
curl -v account.adobe.com
curl -v stock.adobe.com
curl -v console.adobe.io
curl -v braintreepayments.com
curl -v paypal.com
curl -v ups.com
curl -v usps.com
curl -v fedex.com
curl -v dhl.com
curl -v devdocs.magento.com

# Check domain denials
curl -v amazon.com
curl -v facebook.com
curl -v twitter.com

# IP address access
...

# IP address denials
...
```

### `post_deploy` esempio

```yaml
hooks:
    build: |
        set -e
        php ./vendor/bin/ece-tools run scenario/build/generate.xml
        php ./vendor/bin/ece-tools run scenario/build/transfer.xml
    deploy: "php ./vendor/bin/ece-tools run scenario/deploy.xml\n"
    post_deploy: |
        set -e
        php ./vendor/bin/ece-tools run scenario/post-deploy.xml
        echo "[$(date)] post-deploy hook end"
        ./curl-tests-for-egress-filtering.sh
        echo "[$(date)] curl finished"
```
