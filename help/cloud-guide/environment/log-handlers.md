---
title: Gestori di registro
description: Scopri come configurare i gestori di registro per Adobe Commerce sull’infrastruttura cloud.
feature: Cloud, Logs, Configuration
role: Developer
exl-id: d3be7b6d-5778-4c32-865b-31bdb2852a23
source-git-commit: f8e35ecff4bcafda874a87642348e2d2bff5247b
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---

# Gestori di registro

È possibile configurare i gestori dei registri per l&#39;invio di messaggi a un server di registrazione remoto. Un gestore di registri invia i registri di generazione e distribuzione ad altri sistemi in modo simile al modo in cui invii i registri agli Slack e invii per e-mail. È possibile abilitare un gestore _syslog_, ideale per la registrazione di messaggi relativi all&#39;hardware, oppure un gestore GELF (Graylog Extended Log Format), ideale per la registrazione di messaggi da applicazioni software.

Nell&#39;esempio seguente vengono configurati entrambi i gestori aggiungendo la configurazione al file `.magento.env.yaml`. Per i valori del livello di registrazione minimo (`min_level`), vedere [Livelli di registro](#log-levels).

```yaml
log:
  syslog:
    ident: "<syslog-ident>"
    facility: 8 # https://php.net/manual/en/network.constants.php
    min_level: "info"
    logopts: <syslog-logopts>

  syslog_udp:
    host: "<syslog-host>"
    port: <syslog-port>
    facility: 8  # https://php.net/manual/en/network.constants.php
    ident: "<syslog-ident>"
    min_level: "info"

  gelf:
    min_level: "info"
    use_default_formatter: true
    additional: # Some additional information for each log message
      project: "<some-project-id>"
      app_id: "<some-app-id>"
    transport:
      http:
        host: "<http-host>"
        port: <http-port>
        path: "<http-path>"
        connection_timeout: 60
      tcp:
        host: "<tcp-host>"
        port: <tcp-port>
        connection_timeout: 60
      udp:
        host: "<udp-host>"
        port: <udp-port>
        chunk_size: 1024
```

## Livelli di registro

I livelli di registro determinano il livello di dettaglio dei messaggi di notifica. Le seguenti categorie a livello di registro includono ogni livello di registro al di sotto di esso. Ad esempio, un livello `debug` include la registrazione da ogni livello, mentre un livello `alert` mostra solo avvisi ed emergenze.

- **debug**—informazioni di debug dettagliate
- **info**—eventi interessanti, ad esempio l&#39;accesso utente o il registro SQL
- **avviso**—eventi normali ma significativi
- **avvertenza**—eventi eccezionali che non sono errori, ad esempio l&#39;utilizzo di un&#39;API obsoleta o l&#39;utilizzo non corretto di un&#39;API
- **errore**—errori di runtime che non richiedono un&#39;azione immediata
- **critico**: condizioni critiche, ad esempio un componente dell&#39;applicazione non disponibile o un&#39;eccezione imprevista
- **avviso**: è necessaria un&#39;azione immediata, ad esempio un sito Web non disponibile o il database non è disponibile, che attiva un avviso SMS
- **emergenza**—il sistema è inutilizzabile
