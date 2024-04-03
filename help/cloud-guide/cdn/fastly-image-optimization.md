---
title: Ottimizzazione rapida delle immagini
description: Scopri come ottimizzare la consegna delle immagini e semplificare la gestione delle immagini per il sito Adobe Commerce abilitando e configurando l’ottimizzazione delle immagini Fastly.
feature: Cloud, Configuration, Media
exl-id: 20f5c5c3-7c43-493b-b651-82b023cf5db5
source-git-commit: 7a181af2149eef7bfaed4dd4d256b8fa19ae1dda
workflow-type: tm+mt
source-wordcount: '1275'
ht-degree: 0%

---

# Ottimizzazione rapida delle immagini

Fastly Image Optimization (Fastly IO) consente di manipolare e ottimizzare le immagini in tempo reale per velocizzarne la distribuzione e semplificare la manutenzione dei set di origini delle immagini per le applicazioni web dinamiche. Una volta configurato Fastly IO, offre le seguenti funzioni di ottimizzazione delle immagini:

- Forza conversione perdita
- Ottimizzazione immagine profonda
- Rapporti di pixel adattivi
- Supporto per formati immagine comuni: PNG, JPEG, GIF e WebP

Prima di abilitare e configurare l&#39;opzione I/O Fastly, è necessario impostare il servizio Fastly e configurare la schermatura Origin.

In base alle impostazioni di configurazione, lo snippet Fastly Image Optimization (Fastly IO) inserisce il codice VCL per eseguire l’ottimizzazione dell’immagine in modo da velocizzare la consegna dell’immagine del prodotto nella vetrina. Per configurare l&#39;I/O Fastly sono necessari tre passaggi: Abilita, Configura e Verifica.

## Attiva I/O veloce

Abilita Ottimizzazione Fastly Image (Fastly IO) dal pannello Amministratore caricando lo snippet VCL Fastly IO. Il frammento fornisce le istruzioni di configurazione Fastly per elaborare tutte le immagini mediante gli ottimizzatori di immagini, utilizzando le configurazioni predefinite.

**Prerequisiti:**

- Installazione o aggiornamento al modulo Fastly versione 1.2.62 o successiva
- [Configurare Fastly Origin shield e back-end](fastly-custom-cache-configuration.md#configure-back-ends-and-origin-shielding)

**Per abilitare Fastly IO**:

1. Accedi al tuo account [Amministratore](../../get-started/onboarding.md#access-your-admin-panel) come amministratore.

1. Seleziona **Negozi** > **Impostazioni** > **Configurazione** > **Avanzate** > **Sistema**.

1. Nel riquadro di destra, espandere **Cache a pagina intera**.

1. Seleziona **Configurazione rapida** > **Ottimizzazione immagine** per specificare le impostazioni di configurazione.

1. In _Frammento I/O veloce_ campo, seleziona **Attiva/Disattiva**.

1. Carica lo snippet I/O Fastly:

   - Seleziona **Opzioni di configurazione I/O predefinite** per aprire la pagina Opzioni di configurazione predefinite di ottimizzazione immagine.
   - Seleziona **Carica** per caricare lo snippet VCL sul server.

## Configura I/O veloce

Se necessario, rivedere e aggiornare le impostazioni di configurazione I/O predefinite per l&#39;ottimizzazione dell&#39;immagine. Ad esempio, potrebbe essere utile modificare i livelli di qualità di WebP e JPEG per i formati con perdita di dati o modificare il formato per la trasmissione delle immagini JPEG in _Progressivo_ o _Linea di base_. Inoltre, è possibile utilizzare Fastly IO per funzioni di ottimizzazione delle immagini più granulari, ad esempio:

- Forza conversione perdita
- Ottimizzazione immagine profonda
- Rapporti di pixel adattivi

**Per aggiornare Fastly I/O**:

1. Il giorno _Configurazione rapida_ pagina in _Opzioni di configurazione I/O predefinite_ campo, seleziona **Configura**.

   ![Visualizzare le impostazioni di configurazione Fastly IO](../../assets/cdn/fastly-io-default-config.png)

1. Revisiona e aggiorna le impostazioni di configurazione Fastly IO sul _Opzioni di configurazione predefinite per l’ottimizzazione dell’immagine_ pagina:

   ![Verifica configurazione I/O veloce](../../assets/cdn/fastly-io-config-options.png)

   - **Auto WebP?**- lascia l&#39;impostazione di default (`Yes`) per convertire immagini nel formato WebP nei browser che lo supportano. Se si modifica l&#39;impostazione in **No**, Fastly utilizza il tipo di file di immagine anziché convertire l&#39;immagine in formato WebP.

   - **Qualità WebP predefinita (con perdita di dati)**- lascia l&#39;impostazione di default (`85`) o digitare il livello di compressione per le immagini con formato file con perdita di dati. È possibile specificare un numero intero compreso tra 1 e 100.

   - **Controlli di formato predefiniti di JPEG** — lascia l&#39;impostazione predefinita (`Auto`) o selezionare il tipo di JPEG da utilizzare per la trasmissione di un&#39;immagine. Se il valore è impostato su _Automatico_, Fastly distribuisce le immagini con il tipo di output corrispondente al tipo di input. Seleziona _Linea di base_ per visualizzare le immagini linea per linea a partire dall&#39;alto a sinistra e verso il basso a destra. Seleziona _Progressivo_ per visualizzare un&#39;immagine sfocata che diventa chiara durante il caricamento.

   - **Qualità predefinita JPEG**- lascia l&#39;impostazione di default (`85`) o digitare il livello di compressione per la qualità dei formati di file con perdita di dati. Specificare un numero intero compreso tra 1 e 100.

   - **Consentire l&#39;upscaling?**- lascia l&#39;impostazione di default (`No`), oppure seleziona `Yes` per restituire immagini di dimensioni maggiori rispetto al file di origine originale in modo che possano adattarsi alle dimensioni richieste.

   - **Ridimensiona filtro**- lascia l&#39;impostazione di default (`Lancsoz3`) o selezionare un&#39;alternativa. Questa impostazione specifica il filtro utilizzato per fornire un&#39;immagine ridimensionata. A seconda del filtro selezionato, l’immagine ridimensionata può avere un numero di pixel maggiore o minore.

      - `Lanczos3` (impostazione predefinita) - Offre un&#39;immagine di qualità ottimale. Aumenta la capacità di rilevare bordi e feature lineari all&#39;interno di un&#39;immagine e utilizza _[!DNL sinc]_ricampionamento per fornire la migliore ricostruzione possibile.
      - `Lanczos2`- Utilizza lo stesso filtro di `Lancsoz3` ma con un&#39;approssimazione meno accurata del _[!DNL sinc]_ricampionamento.
      - `Bicubic`- Ha un effetto di nitidezza naturale quando si rimpicciolisce un&#39;immagine.
      - `Bilinear`- Ha un effetto di arrotondamento naturale quando si ingrandisce un&#39;immagine.
      - `Nearest`- Ha un effetto di pixel naturale durante il ridimensionamento della grafica.

1. Dopo aver specificato le impostazioni di configurazione I/O per il servizio Fastly, selezionare **Annulla** per tornare alle impostazioni di configurazione Fastly.

1. Nella configurazione di ottimizzazione immagine _Abilita ottimizzazione immagine profonda_ campo, seleziona **Sì** per attivare l&#39;ottimizzazione immagine in profondità.

   ![Abilita ottimizzazione immagine Fastly IO](../../assets/cdn/fastly-io-deep-image-config.png)

   L&#39;ottimizzazione immagine profonda è disattivata per impostazione predefinita. Quando questa funzione è abilitata, la funzione di ridimensionamento incorporata in Adobe Commerce viene disattivata e il lavoro di ridimensionamento viene scaricato nel servizio I/O Fastly. L’ottimizzazione immagine si applica solo alle immagini del prodotto. Le immagini CMS non vengono ridimensionate. Consulta la [Documentazione rapida](#deep-image-optimization).

1. Dopo aver attivato l&#39;ottimizzazione immagine, abilita [proporzioni pixel adattivi](#adaptive-pixel-ratios) per generare immagini ottimizzate per l’utilizzo in siti web dinamici.

   ![Abilita rapporti pixel adattativi I/O veloci](../../assets/cdn/fastly-io-config-adaptive-pixel.png)

   - In _Abilita rapporti pixel per dispositivi adattivi_ campo, seleziona **Sì**.
   - In _Rapporti pixel del dispositivo_ , accettare l&#39;impostazione predefinita o selezionare **Ingresso di sistema** per rimuovere l&#39;impostazione. Quindi, selezionare il rapporto desiderato. Un&#39;impostazione con proporzioni pixel del dispositivo più elevate offre immagini più grandi.

1. Seleziona **Salva configurazione**.

### Forza conversione perdita

Per impostazione predefinita, il servizio Fastly IO forza la conversione di formati senza perdita di dati come PNG, BMP o WEBP in formato JPEG/WEBP.

Il vantaggio di forzare la conversione con perdita di dati è che vengono distribuite immagini più piccole.
Ad esempio, utilizzando il formato JPEG o WEBp invece di PNG, la dimensione può essere ridotta del 60-70% a seconda del livello di qualità specificato nella configurazione di I/O Fastly.

A seconda del livello di qualità selezionato per l&#39;ottimizzazione dell&#39;immagine, è possibile percepire differenze visive nelle immagini. Ad Alpha, i canali e le trasparenze vengono eliminati e sostituiti con uno sfondo bianco, a meno che non si utilizzi l&#39;ottimizzazione immagine profonda che utilizza il colore di sfondo del tema.

Se disattivi la conversione con perdita di dati (`WebP Auto? = No`), Fastly IO modifica le immagini JPEG in formato WEBP solo per i browser compatibili. Nessun altro tipo di immagine viene modificato. Ad esempio, se l&#39;immagine originale è PNG, l&#39;output del servizio I/O Fastly è PNG.

### Ottimizzazione immagine profonda

L&#39;ottimizzazione immagine profonda è disattivata per impostazione predefinita. L’abilitazione di questa opzione disattiva il ridimensionamento integrato di Adobe Commerce e lo scarica completamente sul servizio Fastly IO.
Questa funzione viene ridimensionata solo _prodotto_ immagini. Le immagini CMS non vengono ridimensionate.

Se abiliti l’ottimizzazione deep image, viene aggiunta una definizione del colore di sfondo a ogni immagine definita nel tema. Di conseguenza, le immagini WebP passano da non perdenti di WebP a perdenti di WebP. Una delle principali differenze tra lossless e lossy è che lossy rilascia il canale alfa dalle immagini PNG, che fornisce immagini molto più piccole. Tuttavia, le immagini con trasparenza possono avere un aspetto dispari sulle pagine di prodotti e campagne che utilizzano uno sfondo diverso.

Ad esempio, il codice seguente rappresenta l’origine originale di un’immagine dal tema Luma:

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/cache/f073062f50e48eb0f0998593e568d857/m/b/mb02-gray-0.jpg"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

Quando la funzione Fastly IO Deep Image Optimization è abilitata, il codice sorgente originale per l&#39;immagine viene riscritto come mostrato nell&#39;esempio seguente:

```html
<img class="product-image-photo"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

### Rapporti di pixel adattivi

La funzione Adaptive pixel ratio è utile per ottimizzare le immagini per le applicazioni web progressive. Consente di fornire più dimensioni e risoluzioni di immagini da un file di origine aggiungendo un `srcset` per ogni immagine del prodotto.

Quando la funzione Adaptive pixel ratios è abilitata, il servizio I/O Fastly fornisce un&#39;immagine a larghezza fissa in grado di adattarsi alle variazioni `device-pixel-ratios`.
Ad esempio, il servizio modifica la definizione dell’immagine del prodotto come illustrato nell’esempio seguente:

```html
<img class="product-image-photo"
     srcset="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=2 2x,
  https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds&dpr=3 3x"
     src="https://mymagentosite/pub/media/catalog/product/m/b/mb02-gray-0.jpg?width=240&height=300&quality=80&bg-color=255,255,255&fit=bounds"
     width="240"
     height="300"
     alt="Fusion Backpack"/>
```

Consulta `srcset` [supporto browser](https://caniuse.com/#feat=srcset) e [specifica](https://html.spec.whatwg.org/multipage/embedded-content.html#attr-img-srcset).

## Convalida I/O veloce

Dopo aver abilitato e configurato Fastly IO, convalidare la configurazione eseguendo test di velocità della pagina Web con e senza Fastly IO abilitato. Inoltre, controlla le immagini nel tuo negozio per verificare le dimensioni e l&#39;aspetto delle immagini per eventuali problemi.
