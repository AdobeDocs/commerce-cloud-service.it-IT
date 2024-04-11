---
title: Servizio e-mail SendGrid
description: Scopri il servizio e-mail SendGrid per Adobe Commerce sull’infrastruttura cloud e come verificare la configurazione DNS.
exl-id: 30d3c780-603d-4cde-ab65-44f73c04f34d
source-git-commit: 7c22dc3b0e736043a3e176d2b7ae6c9dcbbf1eb5
workflow-type: tm+mt
source-wordcount: '1019'
ht-degree: 0%

---

# Servizio e-mail SendGrid

Il servizio proxy SMTP (Simple Mail Transfer Protocol) di SendGrid fornisce l&#39;autenticazione e la reputazione delle e-mail in uscita, incluso il supporto per:

* Tutte le e-mail transazionali in uscita
* Indirizzi IP dedicati
* Registrazione del dominio, firme DKIM (DomainKeys Identified Mail) per la convalida del dominio e-mail (solo per gli ambienti Pro Staging e Production)
* Registrazione del dominio personalizzato (solo per Pro)
* Integrazione automatizzata per gli ambienti di integrazione Starter e Pro. Gli ambienti Pro Production e Staging richiedono il provisioning e la configurazione manuali durante il processo di provisioning hardware IaaS (Infrastructure as a Service)

Il proxy SMTP SendGrid non deve essere utilizzato come server e-mail generico per ricevere e-mail in arrivo o per essere utilizzato con campagne di e-mail marketing.

>[!TIP]
>
>È possibile trovare i dettagli di SendGrid per il proprio account in [Interfaccia utente di onboarding](https://cloud.magento.com) e seleziona la **Dettagli progetto** > **Informazioni di hosting** scheda.

## Abilitare o disabilitare l’e-mail

Per impostazione predefinita, l’e-mail in uscita è abilitata negli ambienti di produzione e staging di Pro. Il [!UICONTROL Outgoing emails] può apparire disattivato nelle impostazioni dell&#39;ambiente indipendentemente dallo stato fino a quando non si imposta `enable_smtp` proprietà. Puoi abilitare le e-mail in uscita per altri ambienti per inviare e-mail di autenticazione a due fattori agli utenti del progetto Cloud. Consulta [Configurare le e-mail per il test](outgoing-emails.md).

## Dashboard SendGrid

Tutti i progetti Cloud vengono gestiti con un account centrale, pertanto solo l’assistenza ha accesso al dashboard SendGrid. SendGrid non fornisce funzionalità di restrizione dell&#39;account secondario.

Per esaminare i registri attività relativi allo stato di consegna o a un elenco di indirizzi e-mail non recapitati, rifiutati o bloccati, [invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket). Il team di supporto **non può** recupera i registri attività più vecchi di 30 giorni.

Se possibile, includi le seguenti informazioni nella richiesta:

* l’indirizzo o gli indirizzi e-mail interessati
* il periodo in questione (solo negli ultimi 30 giorni)
* oggetto dell’e-mail

## Posta identificata DomainKeys (DKIM)

DKIM è una tecnologia di autenticazione e-mail che consente ai provider di servizi Internet (ISP) di identificare sia indirizzi di mittente legittimi che falsi, una tecnica comunemente utilizzata nelle truffe di phishing e e-mail. DKIM si basa su un proprietario di dominio che gestisce i record DNS. Quando si utilizza DKIM, il server di invio utilizza una chiave privata per firmare i messaggi. Inoltre, il proprietario del dominio aggiunge un record DKIM, che è un `TXT` ai record DNS del dominio mittente. Questo `TXT` il record contiene una chiave pubblica utilizzata dai server di posta dei destinatari per verificare la firma di un messaggio. La procedura di crittografia a chiave pubblica DKIM consente ai destinatari di verificare l&#39;autenticità di un mittente. Consulta [Informazioni sui record DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records).

>[!WARNING]
>
>Le firme DKIM di SendGrid e il supporto dell’autenticazione di dominio sono disponibili solo per i progetti Pro e non per Starter. Di conseguenza, le e-mail transazionali in uscita potrebbero essere segnalate da filtri anti-spam. L’utilizzo di DKIM migliora la velocità di consegna in quanto mittente di e-mail autenticato. Per migliorare il tasso di consegna dei messaggi, puoi effettuare l’aggiornamento da Starter a Pro oppure utilizzare il tuo server SMTP o il provider di servizi di consegna e-mail. Consulta [Configurare le connessioni e-mail](https://experienceleague.adobe.com/docs/commerce-admin/systems/communications/email-communications.html) nel _Guida di Admin Systems_.

### Autenticazione del mittente e del dominio

Affinché SendGrid possa inviare e-mail transazionali per tuo conto dagli ambienti di produzione Pro o di staging, devi configurare le impostazioni DNS in modo da includere le tre voci DNS del sottodominio SendGrid. A ogni account SendGrid viene assegnato un account univoco `TXT` record utilizzato per autenticare le e-mail in uscita.

**Per abilitare l&#39;autenticazione del dominio**:

1. Invia un [ticket di supporto](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) che richiede di abilitare il DKIM per un dominio specifico (**Solo ambienti Pro Staging e Produzione**).
1. Aggiornare la configurazione DNS con `TXT` e `CNAME` i documenti forniti nel ticket di supporto.

**Esempio `TXT` record con ID account**:

```text
v=spf1 include:u17504801.wl.sendgrid.net -all
```

**Esempio `CNAME` record**:

| Dominio | Punta a | Tipo di record |
| ---------- | ---------- | ------------- |
| em.emaildomain.com | uxxxxxx.wl.sendgrid.net | CNAME |
| s1._domainkey.emaildomain.com | s1.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |
| s2._domainkey.emaildomain.com | s2.domainkey.uxxxxxx.wl.sendgrid.net | CNAME |

### Firme DKIM e protezione automatizzata

Durante la configurazione di un dominio autenticato è possibile scegliere tra protezione automatica e manuale. Se si sceglie la protezione automatica, SendGrid gestisce automaticamente i record DKIM e SPF. Quando aggiungi un nuovo indirizzo IP di invio dedicato al tuo account, SendGrid aggiorna immediatamente le impostazioni DNS e la firma DKIM. Se si disattiva la protezione automatica, l&#39;utente è responsabile dell&#39;aggiornamento della firma DKIM ogni volta che si modifica il dominio di invio.

**Esempio di protezione automatizzata abilitata**:

```text
subdomain.mydomain.com. | CNAME | uxxxxxx.wl.sendgrid.net
s1._domainkey.mydomain.com. | CNAME | s1.domainkey.uxxxxxx.wl.sendgrid.net
s2._domainkey.mydomain.com. | CNAME | s2.domainkey.uxxxxxx.wl.sendgrid.net
```

**Esempio di protezione automatizzata disabilitata**:

```text
me12345.mydomain.com | MX | mx.sendgrid.net
me12345.mydomain.com | TXT | v=spf1 include:sendgrid.net ~all
m1._mydomain.com | TXT | k=rsa; t=s; p=<public-key>
```

Dopo aver configurato l&#39;autenticazione del dominio, SendGrid gestisce automaticamente i record SPF (Security Policy Framework) e DKIM. Dopo che SendGrid fornisce `CNAME` record da aggiungere ai record DNS, è possibile aggiungere indirizzi IP dedicati ed eseguire altri aggiornamenti dell&#39;account senza dover gestire manualmente i record SPF. Consulta [Sicurezza automatizzata e firma DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature).

Per verificare la configurazione DNS:

```terminal
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## Soglia e-mail transazionale

La soglia delle e-mail transazionali si riferisce al numero di messaggi e-mail transazionali che puoi inviare dagli ambienti Pro entro un periodo di tempo specifico, ad esempio 12.000 e-mail al mese da ambienti non di produzione. La soglia è progettata per proteggere dall’invio di spam e potenzialmente danneggiare la reputazione delle e-mail.

Non vi sono limiti rigidi al numero di e-mail che possono essere inviate nell’ambiente di produzione, purché il punteggio di reputazione del mittente sia superiore al 95%. La reputazione è influenzata dal numero di e-mail non recapitate o rifiutate e dal fatto che i registri di posta indesiderata basati su DNS abbiano contrassegnato il dominio come potenziale origine di posta indesiderata. Consulta [Messaggi e-mail non inviati quando in Adobe Commerce sono stati superati i crediti SendGrid](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded.html) nel _Knowledge Base del supporto Commerce_.

**Per verificare se il numero massimo di crediti è stato superato**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Controlla la `/var/log/mail.log` per `authentication failed : Maxium credits exceeded` voci.

   Se ne vede qualcuno `authentication failed` voci di registro e **Reputazione dell’invio e-mail** è di almeno 95, è possibile [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per richiedere un aumento dell&#39;assegnazione di credito.

### Reputazione dell’invio e-mail

La reputazione di invio di un’e-mail è un punteggio assegnato da un provider di servizi Internet (ISP) a una società che invia messaggi e-mail. Più alto è il punteggio, più alta è la probabilità che un ISP distribuisca messaggi alla casella in entrata di un destinatario. Se il punteggio scende al di sotto di un certo livello, l’ISP può indirizzare i messaggi alla cartella di posta indesiderata dei destinatari o persino rifiutare completamente i messaggi. Il punteggio di reputazione è determinato da diversi fattori, come una media di 30 giorni degli indirizzi IP rispetto ad altri indirizzi IP e la percentuale di reclami per spam. Consulta [5 modi per controllare la reputazione di invio](https://sendgrid.com/blog/5-ways-check-sending-reputation/).