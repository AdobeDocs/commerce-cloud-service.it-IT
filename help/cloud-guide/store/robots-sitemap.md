---
title: Aggiungi mappa del sito e robot per motori di ricerca
description: Scopri come aggiungere robot per mappe del sito e motori di ricerca ad Adobe Commerce su infrastrutture cloud.
feature: Cloud, Configuration, Search, Site Navigation
exl-id: b98f43fa-1878-466d-8ea0-1e7207af8b60
source-git-commit: fda88fdf03afa4a59f40e52802c16b6136d8716f
workflow-type: tm+mt
source-wordcount: '537'
ht-degree: 0%

---

# Aggiungi mappa del sito e robot per motori di ricerca

Un tentativo di generare e scrivere il file `sitemap.xml` nella directory radice genera il seguente errore:

```
Please make sure that "/" is writable by the web-server.
```

Con Adobe Commerce sull&#39;infrastruttura cloud, è possibile scrivere solo in directory specifiche, ad esempio `var`, `pub/media`, `pub/static` o `app/etc`. Quando si genera il file `sitemap.xml` tramite il pannello di amministrazione, è necessario specificare il percorso `/media/`.

Non è necessario generare un file `robots.txt` perché genera il contenuto di `robots.txt` su richiesta e lo memorizza nel database. Puoi visualizzare il contenuto nel browser con il collegamento `<domain.your.project>/robots.txt` o `<domain.your.project>/robots`.

A tal fine è necessario ECE-Tools versione 2002.0.12 e successive con un file `.magento.app.yaml` aggiornato. Vedi un esempio di queste regole nell&#39;archivio [magento-cloud](https://github.com/magento/magento-cloud/blob/master/.magento.app.yaml#L43-L49).

**Per generare un file `sitemap.xml` nelle versioni 2.2 e successive**:

1. Accedi all’Admin.
1. Nel menu _Marketing_, fai clic su **Mappa sito** nella sezione _SEO &amp; Search_.
1. Nella visualizzazione _Mappa sito_, fare clic su **Aggiungi mappa sito**.
1. Nella visualizzazione _Nuova mappa del sito_, immettere i valori seguenti:

   - **Nome file**:`sitemap.xml`
   - **Percorso**:`/media/`

1. Fai clic su **Salva e genera**. La nuova mappa del sito diventa disponibile nella griglia _Mappa sito_.
1. Fare clic sul percorso nella colonna _Collegamento per Google_.

**Per aggiungere contenuto al file `robots.txt`**:

1. Accedi all’Admin.
1. Nel menu _Contenuto_, fai clic su **Configurazione** nella sezione _Progettazione_.
1. Nella visualizzazione _Configurazione progettazione_, fare clic su **Modifica** per il sito Web nella colonna _Azione_.
1. Nella visualizzazione _Sito Web principale_, fare clic su **Robot motore di ricerca**.
1. Aggiorna il campo **Modifica istruzione personalizzata di robots.txt**.
1. Fare clic su **Salva configurazione**.
1. Verificare il file `<domain.your.project>/robots.txt` o l&#39;URL `<domain.your.project>/robots` nel browser.

>[!NOTE]
>
>Se il file `<domain.your.project>/robots.txt` genera un `404 error`, [Invia un ticket di supporto Adobe Commerce](https://experienceleague.adobe.com/docs/commerce-knowledge-base/kb/help-center-guide/magento-help-center-user-guide.html#submit-ticket) per rimuovere il reindirizzamento da `/robots.txt` a `/media/robots.txt`.

## Riscrivi utilizzando lo snippet VCL Fastly

Se disponi di domini diversi e hai bisogno di mappe del sito separate, puoi creare un VCL per indirizzarlo alla mappa del sito corretta. Genera il file `sitemap.xml` nel pannello di amministrazione come descritto in precedenza, quindi crea uno snippet Fastly VCL personalizzato per gestire il reindirizzamento. Vedi [Frammenti personalizzati VCL Fastly](../cdn/fastly-vcl-custom-snippets.md).

>[!NOTE]
>
> Puoi caricare snippet VCL personalizzati dall’interfaccia utente di amministrazione o utilizzando l’API Fastly. Consulta [Esempi e tutorial di snippet VCL personalizzato](../cdn/fastly-vcl-custom-snippets.md#example-vcl-snippet-code).

### Utilizzare uno snippet VCL Fastly per il reindirizzamento

Creare uno snippet VCL personalizzato per riscrivere il percorso da `sitemap.xml` a `/media/sitemap.xml` utilizzando le coppie chiave-valore `type` e `content`.

```json
{
  "name": "sitemapxml_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; }"
}
```

Nell&#39;esempio seguente viene illustrato come riscrivere il percorso per `robots.txt` e `sitemap.xml` in `/media/robots.txt` e `/media/sitemap.xml`

```json
{
  "name": "sitemaprobots_rewrite",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path ~ \"^/?sitemap.xml$\" ) { set req.url = \"/media/sitemap.xml\"; } else if (req.url.path ~ \"^/?robots.txt$\") { set req.url = \"/media/robots.txt\";}"
}
```

**Per utilizzare uno snippet VCL Fastly per un reindirizzamento di dominio particolare**:

Creare un file `pub/media/domain_robots.txt`, dove il dominio è `domain.com`, e utilizzare il successivo snippet VCL:

```json
{
  "name": "domain_robots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }}"
}
```

Il frammento VCL instrada `http://domain.com/robots.txt` e presenta il file `pub/media/domain_robots.txt`.

Per configurare un reindirizzamento per `robots.txt` e `sitemap.xml` in un singolo snippet, creare `pub/media/domain_robots.txt` e `pub/media/domain_sitemap.xml` file, dove il dominio è `domain.com` e utilizzare il successivo snippet VCL:

```json
{
  "name": "domain_sitemaprobots",
  "dynamic": "0",
  "type": "recv",
  "priority": "90",
  "content": "if ( req.url.path == \"/robots.txt\" ) { if ( req.http.host ~ \"(domain).com$\" ) { set req.url = \"/media/\" re.group.1 \"_robots.txt\"; }} else if ( req.url.path == \"/sitemap.xml\" ) { if ( req.http.host ~ \"(domain).com$\" ) {  set req.url = \"/media/\" re.group.1 \"_sitemap.xml\"; }}"
}
```

Nella configurazione dell&#39;amministratore `sitemap`, è necessario specificare il percorso del file utilizzando `pub/media/` anziché `/`.

### Configurare l’indicizzazione per motore di ricerca

Per attivare le personalizzazioni di `robots.txt` in Produzione, è necessario abilitare l&#39;opzione **Indicizzazione tramite motori di ricerca attivata per`<environment-name>`** nelle impostazioni del progetto.

![Utilizza [!DNL Cloud Console] per gestire gli ambienti](../../assets/robots-indexing-by-search-engine.png)

>[!NOTE]
>
>- L’indicizzazione tramite motori di ricerca può essere abilitata solo in Produzione, ma non in nessuno degli ambienti inferiori.
>
>- Se utilizzi PWA Studio e non riesci ad accedere al file `robots.txt` configurato, aggiungi `robots.txt` al [Inserisco nell&#39;elenco Consentiti di dei nomi anteriori](https://github.com/magento/magento2-upward-connector#front-name-allowlist) in **Archivi** > Configurazione > **Generale** > **Web** > Configurazione PWA superiori.
