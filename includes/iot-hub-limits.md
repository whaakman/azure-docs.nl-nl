De volgende tabel bevat de limieten die aan de verschillende servicelagen (S1, S2, S3, F1) zijn gekoppeld. Voor informatie over de kosten van elke *eenheid* in elke laag raadpleegt u [Prijzen van IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

| Resource | S1 Standard | S2 Standard | S3 Standard | F1 Free |
| --- | --- | --- | --- | --- |
| Berichten/dag |400,000 |6,000,000 |300,000,000 |8,000 |
| Maximum aantal eenheden |200 |200 |10 |1 |

> [!NOTE]
> Neem contact op met Microsoft Ondersteuning als u verwacht dat u meer dan 200 eenheden met een hub uit de servicelaag S1 of S2, of meer dan 10 eenheden met een hub uit S3 gaat gebruiken.
> 
> 

De volgende tabel bevat de limieten die van toepassing zijn op IoT Hub-resources:

| Resource | Limiet |
| --- | --- |
| Maximum aantal betaalde IoT-hubs per Azure-abonnement |10 |
| Maximum aantal gratis IoT-hubs per Azure-abonnement |1 |
| Maximumaantal apparaat-id's<br/> dat in één aanroep wordt geretourneerd |1000 |
| Maximale bewaartermijn van IoT Hub-berichten voor apparaat-naar-cloud berichten |7 dagen |
| Maximale grootte van apparaat-naar-cloud berichten |256 kB |
| Maximale grootte van apparaat-naar-cloud batch |256 kB |
| Maximum aantal berichten in apparaat-naar-cloud batch |500 |
| Maximale grootte van cloud-naar-apparaat bericht |64 kB |
| Maximale TTL voor cloud-naar-apparaat berichten |2 dagen |
| Maximumaantal leveringen voor cloud-naar-apparaat <br/> berichten |100 |
| Maximumaantal leveringen voor feedbackberichten <br/> als antwoord op een cloud-naar-apparaat bericht |100 |
| Maximale TTL voor feedbackberichten <br/> als antwoord op een cloud-naar-apparaat bericht |2 dagen |
| Maximale grootte van apparaatdubbel <br/> (tags, gerapporteerde eigenschappen en gewenste eigenschappen) | 8 kB |
| Maximale grootte van tekenreekswaarde van apparaatdubbel | 4 KB |
| Maximale diepte van object in apparaatdubbel | 5 |
| Maximale grootte van de nettolading van directe methode | 128 KB |
| Maximale bewaartermijn van taakgeschiedenis | 30 dagen |
| Maximum aantal gelijktijdige taken | 10 (voor S3), 5 voor (S2), 1 (voor S1) |
| Maximaal aantal extra eindpunten | 10 (voor S1, S2, S3) |
| Maximum aantal regels voor berichtroutering | 100 (voor S1, S2, S3) |


> [!NOTE]
> Neem contact op met Microsoft Ondersteuning als u meer dan 10 betaalde IoT-hubs in een Azure-abonnement nodig hebt.


> [!NOTE]
> Het maximum aantal apparaten dat u verbinding met een enkele IoT-hub maken kunt is momenteel 500.000. Als u deze limiet verhogen wilt, neem dan contact op met [Microsoft Support](https://azure.microsoft.com/en-us/support/options/).

De IoT Hub-service vertraagt aanvragen wanneer de volgende quota worden overschreden:

| Vertragen | Waarde per hub |
| --- | --- |
| Registerbewerkingen voor identiteit <br/> (maken, ophalen, weergeven, bijwerken, verwijderen), <br/> afzonderlijk of bulksgewijs importeren/exporteren |83.33/sec/Unit (min-5000/eenheid) (voor S3) <br/> 1.67/sec/Unit (min-100/eenheid) (voor S1 en S2). |
| Apparaatverbindingen |6000 per seconde per eenheid (voor S3), 120 per seconde per eenheid (voor S2), 12 per seconde per eenheid (voor S1). <br/>Minimaal 100 per seconde. |
| Apparaat-naar-cloud verzendt |6000 per seconde per eenheid (voor S3), 120 per seconde per eenheid (voor S2), 12 per seconde per eenheid (voor S1). <br/>Minimaal 100 per seconde. |
| Cloud-naar-apparaat verzendt | 83.33/sec/Unit (5000/min/eenheid) (voor S3) 1.67/sec/unit (min-100/eenheid) (voor S1 en S2). |
| Cloud-naar-apparaat ontvangt |833.33/sec/Unit (50000/min/eenheid) (voor S3) 16.67/sec/unit (1000/min/eenheid) (voor S1 en S2). |
| Bestandsuploadbewerkingen |83.33 bestand uploaden sec-meldingen-eenheid (5000/min/eenheid) (voor S3), 1,67 bestand uploaden meldingen/sec/eenheid (min-100/eenheid) (voor S1 en S2). <br/> Voor een Azure Storage-account kunnen 10.000 SAS URI's tegelijk zijn uitgeschakeld.<br/> Er kunnen 10 SAS URI's per apparaat tegelijk zijn uitgeschakeld. |
| Directe methoden | 24MB/sec/eenheid (voor S3), 160KB/sec/eenheid van de 480KB/sec/eenheid (voor S2) (voor S1)<br/> Op basis van 8KB beperking van de grootte van de meter. |
| Apparaatdubbel leest | 50 per seconde per eenheid (voor S3), maximaal 10 per seconde of 1 per seconde per eenheid (voor S2), 10 per seconde (voor S1) |
| Apparaatdubbel werkt bij | 50 per seconde per eenheid (voor S3), maximaal 10 per seconde of 1 per seconde per eenheid (voor S2), 10 per seconde (voor S1) |
| Taakbewerkingen <br/> (maken, bijwerken, weergeven, verwijderen) | 83.33/sec/Unit (5000/min/eenheid) (voor S3) 1.67/sec/unit (min-100/eenheid) (voor S2), 1.67/sec/unit (min-100/eenheid) (voor S1) |
| Doorvoer van taken per apparaat bewerkingen | 50 per seconde per eenheid (voor S3), maximaal 10 per seconde of 1 per seconde per eenheid (voor S2), 10 per seconde (voor S1) |
