---
author: robinsh
ms.author: robin.shahan
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: ea4931bee17b57260660cc5ba80807a8ae25ca3d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57553609"
---
De volgende tabel bevat de limieten die zijn gekoppeld aan de verschillende Servicelagen, S1, S2, S3 en F1. Voor informatie over de kosten van elk *eenheid* in elke laag, Zie [prijzen van Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

| Resource | S1 Standard | S2 Standard | S3 Standard | F1 Free |
| --- | --- | --- | --- | --- |
| Berichten/dag |400,000 |6,000,000 |300,000,000 |8,000 |
| Maximum aantal eenheden |200 |200 |10 |1 |

> [!NOTE]
> Als u verwacht het gebruik van meer dan 200 eenheden met een laag S1 of S2 hub of 10 eenheden met een hub S3-laag dat, neem dan contact op met Microsoft Support.
> 
> 

De volgende tabel bevat de limieten die betrekking hebben op IoT Hub-resources.

| Resource | Limiet |
| --- | --- |
| Maximum aantal betaalde IoT-hubs per Azure-abonnement |50 |
| Maximum aantal gratis IoT-hubs per Azure-abonnement |1 |
| Maximum aantal tekens in een apparaat-ID | 128 |
| Maximumaantal apparaat-id's<br/> dat in één aanroep wordt geretourneerd |1000 |
| Maximale bewaartermijn van IoT Hub-berichten voor apparaat-naar-cloud berichten |7 dagen |
| Maximale grootte van apparaat-naar-cloud berichten |256 kB |
| Maximale grootte van apparaat-naar-cloud batch |AMQP en HTTP: 256 KB voor de hele batch <br/>MQTT: 256 KB voor elk bericht |
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
| Maximaal aantal extra eindpunten | 10 (voor S1, S2 en S3) |
| Maximum aantal regels voor berichtroutering | 100 (voor S1, S2 en S3) |
| Maximum aantal gelijktijdig verbonden apparaat stromen | 50 (voor S1, S2, S3 en alleen F1) |
| Maximaal stream-gegevensoverdracht | 300 MB per dag (voor S1, S2, S3 en alleen F1) |


> [!NOTE]
> Als u meer dan 50 betaalde IoT-hubs in een Azure-abonnement nodig hebt, neem dan contact op met Microsoft Support.


> [!NOTE]
> Op dit moment is het maximum aantal apparaten dat u verbinding met een enkele IoT-hub maken kunt 1.000.000. Als u wilt om deze limiet te verhogen, neem dan contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

IoT Hub vertraagt aanvragen wanneer de volgende quota worden overschreden.

| Vertragen | Waarde per hub |
| --- | --- |
| Registerbewerkingen voor identiteit <br/> (maken, ophalen, weergeven, bijwerken en verwijderen), <br/> afzonderlijk of bulksgewijs importeren/exporteren |83.33/sec/Unit (5000 per minuut per eenheid) (voor S3). <br/> 1.67/sec/Unit (100 per minuut per eenheid) (voor S1 en S2). |
| Apparaatverbindingen |6000 per seconde per eenheid (voor S3), 120 per seconde per eenheid (voor S2), 12 per seconde per eenheid (voor S1). <br/>Minimaal 100 per seconde. |
| Apparaat-naar-cloud verzendt |6000 per seconde per eenheid (voor S3), 120 per seconde per eenheid (voor S2), 12 per seconde per eenheid (voor S1). <br/>Minimaal 100 per seconde. |
| Cloud-naar-apparaat verzendt | 83.33/sec/Unit (5000 per minuut per eenheid) (voor S3), 1.67/sec/unit (100 per minuut per eenheid) (voor S1 en S2). |
| Cloud-naar-apparaat ontvangt |833.33/sec/Unit (50.000 per minuut per eenheid) (voor S3), 16.67/sec/unit (1000 per minuut per eenheid) (voor S1 en S2). |
| Bestandsuploadbewerkingen |83.33 bestand uploaden meldingen per seconde per eenheid (5000 per minuut per eenheid) (voor S3), 1,67 bestand uploaden meldingen per seconde per eenheid (100 per minuut per eenheid) (voor S1 en S2). <br/> 10.000 SAS URI's kan zijn uitgeschakeld voor een Azure Storage-account in één keer.<br/> Er kunnen 10 SAS URI's per apparaat tegelijk zijn uitgeschakeld. |
| Directe methoden | 24 MB per seconde/eenheid (voor S3), 480 KB/sec/eenheid (voor S2), 160 KB/sec/eenheid (voor S1).<br/> Op basis van grootte berichtenmeter bandbreedtebeperking van 8 KB. |
| Apparaatdubbel leest | 50 per seconde per eenheid (voor S3), maximaal 10 per seconde of 1/seconde per eenheid (voor S2), 10 per seconde (voor S1). |
| Apparaatdubbel werkt bij | 50 per seconde per eenheid (voor S3), maximaal 10 per seconde of 1/seconde per eenheid (voor S2), 10 per seconde (voor S1). |
| Taakbewerkingen <br/> (maken, lijst, bijwerken en verwijderen) | 83.33/sec/Unit (5000 per minuut per eenheid) (voor S3), 1.67/sec/unit (100 per minuut per eenheid) (voor S2), 1.67/sec/unit (100 per minuut per eenheid) (voor S1). |
| Doorvoer van taken per apparaat bewerkingen | 50 per seconde per eenheid (voor S3), maximaal 10 per seconde of 1/seconde per eenheid (voor S2), 10 per seconde (voor S1). |
| Streamsnelheid voor het initiëren van apparaat | 5 nieuwe stromen per seconde (voor S1, S2, S3 en alleen F1). |