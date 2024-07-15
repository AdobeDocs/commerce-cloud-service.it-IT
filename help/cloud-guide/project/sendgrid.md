---
title: Servizio e-mail SendGrid
description: Scopri il servizio e-mail SendGrid per Adobe Commerce sull’infrastruttura cloud e come verificare la configurazione DNS.
exl-id: 30d3c780-603d-4cde-ab65-44f73c04f34d
source-git-commit: 2b106edcaaacb63c0e785f094b7e1b755885abd0
workflow-type: tm+mt
source-wordcount: '1090'
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
>Puoi trovare i dettagli di SendGrid per il tuo account nella [interfaccia utente di onboarding](https://cloud.magento.com) e selezionare la scheda **Dettagli progetto** > **Informazioni hosting**.

## Abilitare o disabilitare l’e-mail

Puoi abilitare o disabilitare le e-mail in uscita per ogni ambiente dalla console Cloud o dalla riga di comando.

Per impostazione predefinita, le e-mail in uscita sono abilitate negli ambienti di produzione e staging di Pro. Tuttavia, [!UICONTROL Outgoing emails] potrebbe apparire disabilitato nelle impostazioni dell&#39;ambiente finché non si imposta la proprietà `enable_smtp` tramite la [riga di comando](outgoing-emails.md#enable-emails-in-the-cli) o la [console cloud](outgoing-emails.md#enable-emails-in-the-cloud-console). Puoi abilitare le e-mail in uscita per gli ambienti di integrazione e staging per inviare e-mail di autenticazione a due fattori o reimpostare le e-mail con password per gli utenti del progetto Cloud. Consulta [Configurare le e-mail per il test](outgoing-emails.md).

Se le e-mail in uscita devono essere disabilitate o riabilitate negli ambienti di produzione o staging di Pro, puoi inviare un [ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide).

>[!TIP]
>
>L&#39;aggiornamento del valore della proprietà [!UICONTROL enable_smtp] da [riga di comando](outgoing-emails.md#enable-emails-in-the-cli) comporta la modifica del valore dell&#39;impostazione [!UICONTROL Enable outgoing emails] per questo ambiente nella [console cloud](outgoing-emails.md#enable-emails-in-the-cloud-console).

## Dashboard SendGrid

Tutti i progetti Cloud vengono gestiti con un account centrale, pertanto solo l’assistenza ha accesso al dashboard SendGrid. SendGrid non fornisce funzionalità di restrizione dell&#39;account secondario.

Per esaminare i registri attività per verificare lo stato di consegna o un elenco di indirizzi e-mail non recapitati, rifiutati o bloccati, [invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket). Il team di supporto **non può** recuperare i registri attività più vecchi di 30 giorni.

Se possibile, includi le seguenti informazioni nella richiesta:

* l’indirizzo o gli indirizzi e-mail interessati
* il periodo in questione (solo negli ultimi 30 giorni)
* oggetto dell’e-mail

## Posta identificata DomainKeys (DKIM)

DKIM è una tecnologia di autenticazione e-mail che consente ai provider di servizi Internet (ISP) di identificare sia indirizzi di mittente legittimi che falsi, una tecnica comunemente utilizzata nelle truffe di phishing e e-mail. DKIM si basa su un proprietario di dominio che gestisce i record DNS. Quando si utilizza DKIM, il server di invio utilizza una chiave privata per firmare i messaggi. Inoltre, il proprietario del dominio aggiunge un record DKIM, che è un record `TXT` modificato, ai record DNS del dominio mittente. Questo record `TXT` contiene una chiave pubblica utilizzata dai server di posta dei destinatari per verificare la firma di un messaggio. La procedura di crittografia a chiave pubblica DKIM consente ai destinatari di verificare l&#39;autenticità di un mittente. Vedi [Record DKIM spiegati](https://docs.sendgrid.com/ui/account-and-settings/dkim-records).

>[!WARNING]
>
>Le firme DKIM di SendGrid e il supporto dell’autenticazione di dominio sono disponibili solo per i progetti Pro e non per i progetti Starter. Di conseguenza, le e-mail transazionali in uscita potrebbero essere segnalate da filtri anti-spam. L’utilizzo di DKIM migliora la velocità di consegna in quanto mittente di e-mail autenticato. Per migliorare il tasso di consegna dei messaggi, puoi effettuare l’aggiornamento da Starter a Pro oppure utilizzare il tuo server SMTP o il provider di servizi di consegna e-mail. Consulta [Configurare le connessioni e-mail](https://experienceleague.adobe.com/en/docs/commerce-admin/systems/communications/email-communications) nella _Guida di Admin Systems_.

### Autenticazione del mittente e del dominio

Affinché SendGrid possa inviare e-mail transazionali per tuo conto dagli ambienti di produzione Pro o di staging, devi configurare le impostazioni DNS in modo da includere le tre voci DNS del sottodominio SendGrid. A ogni account SendGrid viene assegnato un record `TXT` univoco utilizzato per autenticare le e-mail in uscita.

**Per abilitare l&#39;autenticazione del dominio**:

1. Invia un [ticket di supporto](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) per richiedere l&#39;abilitazione di DKIM per un dominio specifico (**Solo per ambienti di staging e produzione Pro**).
1. Aggiorna la configurazione DNS con i record `TXT` e `CNAME` forniti nel ticket di supporto.

**Esempio di record `TXT` con ID account**:

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

Dopo aver configurato l&#39;autenticazione del dominio, SendGrid gestisce automaticamente i record SPF (Security Policy Framework) e DKIM. Dopo che SendGrid fornisce i record `CNAME` da aggiungere ai record DNS, è possibile aggiungere indirizzi IP dedicati e apportare altri aggiornamenti all&#39;account senza dover gestire manualmente i record SPF. Consulta [Sicurezza automatizzata e firma DKIM](https://docs.sendgrid.com/ui/account-and-settings/dkim-records#automated-security-and-your-dkim-signature).

Per verificare la configurazione DNS:

```terminal
dig CNAME em.domain_name
dig CNAME s1._domainkey.domain_name
dig CNAME s2._domainkey.domain_name
```

## Soglia e-mail transazionale

La soglia delle e-mail transazionali si riferisce al numero di messaggi e-mail transazionali che puoi inviare dagli ambienti Pro entro un periodo di tempo specifico, ad esempio 12.000 e-mail al mese da ambienti non di produzione. La soglia è progettata per proteggere dall’invio di spam e potenzialmente danneggiare la reputazione delle e-mail.

Non vi sono limiti rigidi al numero di e-mail che possono essere inviate nell’ambiente di produzione, purché il punteggio di reputazione del mittente sia superiore al 95%. La reputazione è influenzata dal numero di e-mail non recapitate o rifiutate e dal fatto che i registri di posta indesiderata basati su DNS abbiano contrassegnato il dominio come potenziale origine di posta indesiderata. Vedere [Messaggi e-mail non inviati quando i crediti SendGrid sono stati superati in Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/troubleshooting/miscellaneous/emails-not-being-sent-sendgrid-credits-exceeded) nella _Knowledge Base di supporto di Commerce_.

**Per verificare se il numero massimo di crediti è stato superato**:

1. Sulla workstation locale, passa alla directory del progetto.

1. Utilizza SSH per accedere all’ambiente remoto.

   ```bash
   magento-cloud ssh
   ```

1. Controlla `/var/log/mail.log` per `authentication failed : Maxium credits exceeded` voci.

   Se sono presenti `authentication failed` voci di registro e la **reputazione di invio e-mail** è di almeno 95, puoi [inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide#submit-ticket) per richiedere un aumento dell&#39;assegnazione del credito.

### Reputazione dell’invio e-mail

La reputazione di invio di un’e-mail è un punteggio assegnato da un provider di servizi Internet (ISP) a una società che invia messaggi e-mail. Più alto è il punteggio, più alta è la probabilità che un ISP distribuisca messaggi alla casella in entrata di un destinatario. Se il punteggio scende al di sotto di un certo livello, l’ISP può indirizzare i messaggi alla cartella di posta indesiderata dei destinatari o persino rifiutare completamente i messaggi. Il punteggio di reputazione è determinato da diversi fattori, come una media di 30 giorni degli indirizzi IP rispetto ad altri indirizzi IP e la percentuale di reclami per spam. Consulta [8 Modi per controllare la reputazione di invio delle e-mail](https://sendgrid.com/en-us/blog/5-ways-check-sending-reputation).
