De volgende tabel bevat de limieten die aan de verschillende servicelagen (S1, S2, S3, F1) zijn gekoppeld. Voor informatie over de kosten van elke *eenheid* in elke laag raadpleegt u [Prijzen van IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

| Resource | S1 Standard | S2 Standard | S3 Standard | F1 Free |
| --- | --- | --- | --- | --- |
| Berichten/dag |400,000 |6,000,000 |300,000,000 |8,000 |
| Maximum aantal eenheden |200 |200 |200 |1 |

> [!NOTE]
> Neem contact op met Microsoft Ondersteuning als u verwacht dat u meer dan 200 eenheden met een hub uit de servicelaag S1, S2 of S3 gaat gebruiken.
> 
> 

De volgende tabel bevat de limieten die van toepassing zijn op IoT Hub-resources:

| Resource | Limiet |
| --- | --- |
| Maximum aantal betaalde IoT-hubs per Azure-abonnement |10 |
| Maximum aantal gratis IoT-hubs per Azure-abonnement |1 |
| Maximum aantal apparaat-id's<br/>  dat in één aanroep wordt geretourneerd |1000 |
| Maximale bewaartermijn van IoT Hub-berichten voor apparaat-naar-cloud berichten |7 dagen |
| Maximale grootte van apparaat-naar-cloud berichten |256 kB |
| Maximale grootte van apparaat-naar-cloud batch |256 kB |
| Maximum aantal berichten in apparaat-naar-cloud batch |500 |
| Maximale grootte van cloud-naar-apparaat bericht |64 kB |
| Maximale TTL voor cloud-naar-apparaat berichten |2 dagen |
| Maximum aantal bezorgingen voor cloud-naar-apparaat <br/> berichten |100 |
| Maximum aantal leveringen voor feedbackberichten <br/> als antwoord op een cloud-naar-apparaat bericht |100 |
| Maximale TTL voor feedbackberichten als <br/> antwoord op een cloud-naar-apparaat bericht |2 dagen |
| Maximale grootte van apparaatdubbel <br/> (tags, gerapporteerde eigenschappen en gewenste eigenschappen) | 8 kB |
| Maximale grootte van tekenreekswaarde van apparaatdubbel | 512 bytes |
| Maximale diepte van object in apparaatdubbel | 5 |
| Maximale grootte van de nettolading van directe methode | 8 kB |
| Maximale bewaartermijn van taakgeschiedenis | 30 dagen |
| Maximum aantal gelijktijdige taken | 10 (voor S3), 5 voor (S2), 1 (voor S1) |
| Maximaal aantal extra eindpunten | 10 (voor S1, S2, S3) |
| Maximum aantal regels voor berichtroutering | 100 (voor S1, S2, S3) |


> [!NOTE]
> Neem contact op met Microsoft Ondersteuning als u meer dan 10 betaalde IoT-hubs in een Azure-abonnement nodig hebt.
> 
> 

De IoT Hub-service vertraagt aanvragen wanneer de volgende quota worden overschreden:

| Vertragen | Waarde per hub |
| --- | --- |
| Registerbewerkingen voor identiteit <br/> (maken, ophalen, weergeven, bijwerken, verwijderen), <br/> afzonderlijk of bulksgewijs importeren/exporteren |5000 per minuut per eenheid (voor S3) <br/> 100 per minuut per eenheid (voor S1 en S2). |
| Apparaatverbindingen |6000 per seconde per eenheid (voor S3), 120 per seconde per eenheid (voor S2), 12 per seconde per eenheid (voor S1). <br/>Minimaal 100 per seconde. |
| Apparaat-naar-cloud verzendt |6000 per seconde per eenheid (voor S3), 120 per seconde per eenheid (voor S2), 12 per seconde per eenheid (voor S1). <br/>Minimaal 100 per seconde. |
| Cloud-naar-apparaat verzendt |5000 per minuut per eenheid (voor S3), 100 per minuut per eenheid (voor S1 en S2). |
| Cloud-naar-apparaat ontvangt |50.000 per minuut per eenheid (voor S3), 1000 per minuut per eenheid (voor S1 en S2). |
| Bestandsuploadbewerkingen |5000 bestandsuploadmeldingen per minuut per eenheid (voor S3), 100 bestandsuploadmeldingen per minuut per eenheid (voor S1 en S2). <br/> Voor een Azure Storage-account kunnen&10;.000 SAS URI's tegelijk zijn uitgeschakeld.<br/> Er kunnen&10; SAS URI's per apparaat tegelijk zijn uitgeschakeld. |
| Directe methoden | 1500 per seconde per eenheid (voor S3), 30 per seconde per eenheid (voor S2), 10 per seconde per eenheid (voor S1) |
| Apparaatdubbel leest | 50 per seconde per eenheid (voor S3), maximaal 10 per seconde of 1 per seconde per eenheid (voor S2), 10 per seconde (voor S1) |
| Apparaatdubbel werkt bij | 50 per seconde per eenheid (voor S3), maximaal 10 per seconde of 1 per seconde per eenheid (voor S2), 10 per seconde (voor S1) |
| Taakbewerkingen <br/> (maken, bijwerken, weergeven, verwijderen) | 5000 per minuut per eenheid (voor S3), 100 per minuut per eenheid (voor S2), 100 per minuut per eenheid (voor S1) |
| Doorvoer van taken per apparaat bewerkingen | 50 per seconde per eenheid (voor S3), maximaal 10 per seconde of 1 per seconde per eenheid (voor S2), 10 per seconde (voor S1) |

<!--HONumber=Feb17_HO1-->


