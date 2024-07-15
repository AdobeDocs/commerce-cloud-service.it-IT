---
title: Dati di esempio
description: Scopri come installare dati di esempio con Adobe Commerce sull’infrastruttura cloud.
exl-id: 517e549f-15ba-47b3-9236-a1c4d24c917d
source-git-commit: 8a0523f1714b6ea41887e99b5c31294cf5e5255e
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 0%

---

# Dati di esempio

Se hai bisogno di alcuni dati di esempio durante lo sviluppo del tuo archivio, puoi installare dati di esempio. Questi dati simulano un archivio Adobe Commerce attivo con clienti, prodotti e altri dati. Questi dati di esempio funzionano al meglio con una nuova installazione di Adobe Commerce su modello di infrastruttura cloud.

Come best practice, installa dati di esempio in ambienti di sviluppo e integrazione. Se utilizzi dati di esempio in Staging o Produzione, devi [rimuovere](#reset-or-uninstall-sample-data) le informazioni e i prodotti prima di andare &quot;live&quot;.

## Installare i dati di esempio

Per installare i dati di esempio:

1. Sulla workstation locale, passa alla directory del progetto.

1. Nella directory principale, immetti il seguente comando per aggiungere dati di esempio:

   ```bash
   ./bin/magento sampledata:deploy
   ```

1. Attendi l’aggiornamento dei componenti.

1. Eseguire il commit e il push delle modifiche:

   ```bash
   git add -A && git commit -m "Install sample data"
   ```

   ```bash
   git push origin <branch-name>
   ```

1. Attendi la distribuzione del progetto.

1. Verifica che l’installazione sia stata eseguita correttamente accedendo alla pagina della vetrina nell’ambiente di integrazione. È possibile individuare il collegamento URL alla vetrina tramite [!DNL Cloud Console].

1. Crea un’istantanea dell’ambiente:

   ```bash
   magento-cloud snapshot:create -e <environment-ID>
   ```

Puoi iniziare a testare il tuo sviluppo con i dati live.

## Reimpostare o disinstallare i dati di esempio

È possibile reimpostare o rimuovere i dati di esempio seguendo la stessa procedura utilizzata per installarli:

- Elimina dati di esempio: `./bin/magento sampledata:remove`
- Reimposta dati di esempio: `./bin/magento sampledata:reset`
