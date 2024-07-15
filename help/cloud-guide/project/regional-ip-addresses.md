---
title: Indirizzi IP regionali
description: Consulta un elenco di indirizzi IP per le aree geografiche di AWS e Azure utilizzate da Adobe Commerce sull’infrastruttura cloud per gli ambienti di integrazione.
exl-id: 27d8b489-7ecd-4701-ad92-06aa7cf98c8d
source-git-commit: 7b9c6a4cd17069c25455195bd8f273664b8a29eb
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 0%

---

# Indirizzi IP regionali

Nelle tabelle seguenti sono elencati gli indirizzi IP in entrata e in uscita utilizzati da Adobe Commerce negli ambienti di integrazione [dell&#39;infrastruttura cloud](../architecture/pro-architecture.md#integration-environment). Questi indirizzi IP sono stabili, ma potrebbero cambiare. Adobe avvisa i clienti prima di apportare qualsiasi modifica all’indirizzo IP.

La sintassi per indirizzare gli ambienti di integrazione è la seguente:

```text
<branch>-<unique-ID>-<project-ID>.<region>.magentosite.cloud
```

- **ID univoco** = 7 caratteri alfanumerici casuali
- **ID progetto** = ID progetto di 13 caratteri
- **Area** = nome area AWS o Azure

È possibile utilizzare il comando `ping` per recuperare l&#39;indirizzo IP in ingresso:

```bash
ping integration-abcd123-abcd78910.us-3.magentosite.cloud
```

Risposta di esempio:

```console
PING integration-abcd123-abcd78910.us-3.magentosite.cloud (34.210.133.187): 56 data bytes
Request timeout for icmp_seq 0
Request timeout for icmp_seq 1
Request timeout for icmp_seq 2
```

Se disponi di un firewall aziendale che blocca le connessioni SSH in uscita, puoi aggiungere al inserisco nell&#39;elenco Consentiti gli indirizzi IP in entrata.

## Aree geografiche di AWS

|     | Stati Uniti |       |      | Europa |      |      |      | Asia-Pacifico |
| --- | :------------ | :---- | :--- | :----- | :--- | :--- | :--- | :----------- |
|     | STATI UNITI | US-3 | US-5 | UE | UE-3 | UE-5 | UE-6 | AP-3 |
| In entrata | <!--US-->52 200 159 23<p>52 200 159 125<p>52 200 160,5 | <!--US-3-->34 210 133 187<p>34 214 72 239<p>34 215 10 85 | <!--US-5-->50 112 160 58<p>54 213 195 223<p>35 163 170 185 | <!--EU-->52 209 44 44<p>52 209 23 96<p>52 51 117 101 | <!--EU-3-->34 240 75 192<p>34 251 110 37<p>52 19 113 35 | <!--EU-5-->35 157 81 88<p>3 122 198 131<p>52 28 102 195 | <!--EU-6-->35 181 23 47<p>35 181 24 165<p>35 180 237 48 | <!--AP-3-->52 65 39 201<p>52 65 10 202<p>52 65 30 37 |
| In uscita | <!--US-->52 200 155 111<p>52 200 149 44<p>50 17 163 75 | <!--US-3-->34 210 166 180<p>34 215 83 92<p>34 213 20 158 | <!--US-5-->54 70 238 217<p>52 88 113 98<p>52 36 188 230 | <!--EU-->52 51 163 159<p>52 209 44 60<p>52 208 156 247 | <!--EU-3-->34 240 57 142<p>52 16 140 48<p>52 209 134 55 | <!--EU-5-->3 121 163 221<p>3 121 79 229<p>18.197.3.230 | <!--EU-6-->52 47 155 26<p>35 181 0 157<p>35 181 12 15 | <!--AP-3-->52 65 143 178<p>13 54 80 197<p>52 62 224,4 |

## Aree geografiche di Azure

|          | Stati Uniti | Europa |
| -------- | :-------------- | :-------------- |
|          | US-A1 | AZ-EUROPA-1 |
| In entrata | <!--US-A1--> 20 186 27 68<p>20 186 58 163<p>20 186 113,8 | <!--AZ-W-1-->50 112 160 58<p>54 213 195 223<p>35 163 170 185 |
| In uscita | <!--US-A1-->20 186 58 163<p>20 186 27 68<p>20 186 113,8 | <!--AZ-W-1-->104 45 78 98<p>51 105 168 218<p>51 105 163 143 |
