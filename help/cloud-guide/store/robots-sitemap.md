---
title: Aggiungi mappa del sito e robot per motori di ricerca
description: Scopri come aggiungere robot per mappe del sito e motori di ricerca ad Adobe Commerce su infrastrutture cloud.
feature: Cloud, Configuration, Search, Site Navigation
exl-id: b98f43fa-1878-466d-8ea0-1e7207af8b60
source-git-commit: ee1db75c73c086e0ea54e1a7591ca7f2b4d2b36d
workflow-type: tm+mt
source-wordcount: '517'
ht-degree: 0%

---

# Aggiungi mappa del sito e robot per motori di ricerca

Tentativo di generare e scrivere il `sitemap.xml` file nella directory principale genera il seguente errore:

```terminal
Please make sure that "/" is writable by the web-server.
```

Con Adobe Commerce sull’infrastruttura cloud, puoi scrivere solo in directory specifiche, ad esempio `var`, `pub/media`, `pub/static`, o `app/etc`. Quando si genera `sitemap.xml` mediante il pannello di amministrazione, è necessario specificare `/media/` percorso.

Non è necessario generare un `robots.txt` perché genera il file `robots.txt` contenuto on-demand e lo memorizza nel database. Puoi visualizzare il contenuto nel browser con `<domain.your.project>/robots.txt` o `<domain.your.project>/robots` collegamento.

Questo richiede la versione ECE-Tools 2002.0.12 e successive con un aggiornamento `.magento.app.yaml` file. Vedi un esempio di queste regole nel [archivio cloud magento](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**Per generare un `sitemap.xml` file nella versione 2.2 e successive**:

1. Accedi all’Admin.
1. Il giorno _Marketing_ menu, fai clic su **Mappa del sito** nel _SEO e ricerca_ sezione.
1. In _Mappa del sito_ visualizza, fai clic su **Aggiungi mappa del sito**.
1. In _Nuova mappa del sito_ , immettere i seguenti valori:

   - **Nome file**:`sitemap.xml`
   - **Percorso**:`/media/`

1. Clic **Salva e genera**. La nuova mappa del sito diventa disponibile nel _Mappa del sito_ griglia.
1. Fai clic sul percorso in _Collegamento per Google_ colonna.

**Per aggiungere contenuti al `robots.txt` file**:

1. Accedi all’Admin.
1. Il giorno _Contenuto_ menu, fai clic su **Configurazione** nel _Progettazione_ sezione.
1. In _Configurazione progettazione_ visualizza, fai clic su **Modifica** per il sito web in _Azione_ colonna.
1. In _Sito Web principale_ visualizza, fai clic su **Robot motore di ricerca**.
1. Aggiornare il **Modifica istruzione personalizzata di robots.txt** campo.
1. Clic **Salva configurazione**.
1. Verificare la `<domain.your.project>/robots.txt` file o `<domain.your.project>/robots` URL nel browser.

>[!NOTE]
>
>Se il `<domain.your.project>/robots.txt` genera un `404 error`, [Inviare un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per rimuovere il reindirizzamento da `/robots.txt` a `/media/robots.txt`.

## Riscrivi utilizzando lo snippet VCL Fastly

Se disponi di domini diversi e hai bisogno di mappe del sito separate, puoi creare un VCL per indirizzarlo alla mappa del sito corretta. Genera il `sitemap.xml` nel pannello di amministrazione come descritto in precedenza, quindi crea uno snippet VCL Fastly personalizzato per gestire il reindirizzamento. Consulta [Snippet VCL Fastly personalizzati](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> Puoi caricare snippet VCL personalizzati dall’interfaccia utente di amministrazione o utilizzando l’API Fastly. Consulta [Esempi e tutorial di snippet VCL personalizzati](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### Utilizzare uno snippet VCL Fastly per il reindirizzamento

Creare uno snippet VCL personalizzato per riscrivere il percorso `sitemap.xml` a `/media/sitemap.xml` utilizzando `type` e `content` coppie chiave-valore.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

Nell&#39;esempio seguente viene illustrato come riscrivere il percorso per `robots.txt` e `sitemap.xml` a `/media/robots.txt` e `/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**Per utilizzare uno snippet VCL Fastly per un reindirizzamento di dominio specifico**:

Creare un `pub/media/domain_robots.txt` file, dove il dominio è `domain.com`e utilizzare il successivo snippet VCL:

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

Percorsi snippet VCL `http://domain.com/robots.txt` e presenta la `pub/media/domain_robots.txt` file.

Per configurare un reindirizzamento per `robots.txt` e `sitemap.xml` in un singolo frammento, crea `pub/media/domain_robots.txt` e `pub/media/domain_sitemap.xml` file, in cui il dominio è `domain.com` e utilizzare il successivo snippet VCL:

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

In `sitemap` admin config, è necessario specificare il percorso del file utilizzando `pub/media/` anziché `/`.

### Configurare l’indicizzazione per motore di ricerca

Per attivare `robots.txt` personalizzazioni, è necessario abilitare **L’indicizzazione per motori di ricerca è attivata per`<environment-name>`** nelle impostazioni del progetto.

![Utilizza il [!DNL Cloud Console] per gestire gli ambienti](../../assets/robots-indexing-by-search-engine.png)

>[!NOTE]
>
>Se utilizzi PWA Studi e non riesci ad accedere al file configurato `robots.txt` file, aggiungi `robots.txt` al [Inserire nell&#39;elenco Consentiti Nome anteriore](https://github.com/magento/magento2-upward-connector#front-name-allowlist) a **Negozi** > Configurazione > **Generale** > **Web** > Configurazione UPWARD PWA.
