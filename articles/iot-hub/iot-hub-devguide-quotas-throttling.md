---
title: Informatie over Azure IoT Hub-quota en beperkingen | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijving van de quota die betrekking hebben op IoT-Hub en het verwachte gedrag voor bandbreedteregeling.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/11/2019
ms.openlocfilehash: ea9bea8b314d00db87ad7addacc49a976e0da08e
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550473"
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referentie - IoT-Hub-quota en beperkingen

## <a name="quotas-and-throttling"></a>Quota en beperkingen

Elk Azure-abonnement zijn maximaal 50 IoT-hubs en maximaal 1 gratis hub.

Elke IoT-hub is ingericht met een bepaald aantal eenheden in een bepaalde laag. De laag en het aantal eenheden bepalen de maximale dagelijkse quotum voor berichten die u kunt verzenden. De grootte van het bericht gebruikt voor het berekenen van dat het dagelijkse quotum is 0,5 KB voor een gratis laag-hub en 4KB voor alle andere lagen. Zie voor meer informatie, [prijzen van Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/).

De laag bepaalt ook de bandbreedteregeling limieten die IoT Hub wordt afgedwongen op alle bewerkingen.

## <a name="operation-throttles"></a>Vertragingen in bewerking

Bewerking vertragingen zijn tarief beperkingen die worden toegepast in minuut bereiken en zijn bedoeld om misbruik te voorkomen. Ze zijn ook onderhevig aan [vormgeven van verkeer](#traffic-shaping).

De volgende tabel ziet u de afgedwongen beperkingen. Waarden verwijzen naar een afzonderlijke hub.

| Vertragen | Gratis, B1 en S1 | B2 en S2 | B3 en S3 | 
| -------- | ------- | ------- | ------- |
| [Registerbewerkingen voor identiteit](#identity-registry-operations-throttle) (maken, ophalen, weergeven, bijwerken en verwijderen) | 1.67/sec/Unit (100 per minuut per eenheid) | 1.67/sec/Unit (100 per minuut per eenheid) | 83.33/sec/Unit (5000 per minuut per eenheid) |
| [Nieuwe apparaatverbindingen](#device-connections-throttle) (deze limiet is van toepassing op het aantal _nieuwe verbindingen_, niet het totale aantal verbindingen) | Hogere van 100 per seconde of 12 per seconde per eenheid <br/> Twee S1-eenheden zijn bijvoorbeeld 2\*12 = 24 uur per dag nieuwe verbindingen per seconde, maar u hebt ten minste 100 nieuwe verbindingen per seconde voor uw-eenheden. Met negen S1-eenheden, hebt u 108 nieuwe verbindingen per seconde (9\*12) voor uw-eenheden. | nieuwe verbindingen per seconde per eenheid van de 120 | nieuwe verbindingen per seconde per eenheid van de 6000 |
| Apparaat-naar-cloud verzendt | Hogere van 100 per seconde of 12 per seconde per eenheid <br/> Twee S1-eenheden zijn bijvoorbeeld 2\*12 = 24 uur per dag/seconde, maar u hebt ten minste 100 per seconde voor uw-eenheden. Met negen S1-eenheden, hebt u 108 per seconde (9\*12) voor uw-eenheden. | 120/sec/unit | 6000 per seconde per eenheid |
| Cloud-naar-apparaat verzendt<sup>1</sup> | 1.67/sec/Unit (100 per minuut per eenheid) | 1.67/sec/Unit (100 per minuut per eenheid) | 83.33/sec/Unit (5000 per minuut per eenheid) |
| Cloud-naar-apparaat ontvangt<sup>1</sup> <br/> (alleen als apparaat maakt gebruik van HTTPS)| 16.67/sec/Unit (1000 per minuut per eenheid) | 16.67/sec/Unit (1000 per minuut per eenheid) | 833.33/sec/Unit (50.000 per minuut per eenheid) |
| Bestand uploaden | 1.67 bestand uploaden meldingen per seconde per eenheid (100 per minuut per eenheid) | 1.67 bestand uploaden meldingen per seconde per eenheid (100 per minuut per eenheid) | 83.33 bestand uploaden meldingen per seconde per eenheid (5000 per minuut per eenheid) |
| Directe methoden<sup>1</sup> | 160KB/sec/unit<sup>2</sup> | 480KB/sec/unit<sup>2</sup> | 24MB/sec/unit<sup>2</sup> | 
| Query's | 20 per minuut per eenheid | 20 per minuut per eenheid | 1000 per minuut per eenheid |
| (Apparaat- en -module) leesbewerkingen Twin<sup>1</sup> | 100/sec | Hogere van 100 per seconde of 10 per seconde per eenheid | 500/sec/unit |
| Updates (apparaat- en -module) Twin<sup>1</sup> | 50/sec | Hogere van 50 per seconde of 5 per seconde per eenheid | 250/sec/unit |
| Taken operations<sup>1</sup> <br/> (maken, bijwerken, weergeven, verwijderen) | 1.67/sec/Unit (100 per minuut per eenheid) | 1.67/sec/Unit (100 per minuut per eenheid) | 83.33/sec/Unit (5000 per minuut per eenheid) |
| Taken apparaatbewerkingen<sup>1</sup> <br/> (dubbele bijwerken, rechtstreekse methode aanroepen) | 10/sec | Hogere van 10 per seconde of 1 per seconde per eenheid | 50/sec/unit |
| Configuraties en edge-implementaties<sup>1</sup> <br/> (maken, bijwerken, weergeven, verwijderen) | 0.33/sec/Unit (20 per minuut per eenheid) | 0.33/sec/Unit (20 per minuut per eenheid) | 0.33/sec/Unit (20 per minuut per eenheid) |
| Streamsnelheid voor het initiëren van apparaat<sup>1</sup> | 5 nieuwe gegevensstromen/sec | 5 nieuwe gegevensstromen/sec | 5 nieuwe gegevensstromen/sec |
| Maximum aantal gelijktijdig verbonden apparaat streams<sup>1</sup> | 50 | 50 | 50 |
| Overdracht van gegevens van maximaal stream<sup>1</sup> (statistische-volume per dag) | 300 MB | 300 MB | 300 MB |

<sup>1</sup>deze functie is niet beschikbaar in de basic-laag van IoT-Hub. Zie voor meer informatie, [u bij het kiezen van de juiste IoT-Hub](iot-hub-scaling.md). <br/><sup>2</sup>meter formaat beperking is 4 KB.

### <a name="traffic-shaping"></a>Vormgeven van verkeer

Om te voldoen aan burst-verkeer, accepteert IoT-Hub aanvragen boven de vertraging voor een beperkte periode. De eerste paar van deze aanvragen worden onmiddellijk verwerkt. Als het aantal aanvragen blijft schenden echter de vertraging, IoT Hub wordt gestart als u de aanvragen in een wachtrij plaatst en verwerkt tarief voor de limiet. Dit effect heet *vormgeven van verkeer*. De grootte van deze wachtrij is bovendien beperkt. Als de schending vertraging blijft, uiteindelijk de wachtrij vol en de IoT Hub wordt gestart voor het weigeren van aanvragen met `429 ThrottlingException`. 

U kunt bijvoorbeeld een gesimuleerd apparaat gebruiken 200 apparaat-naar-cloud-berichten per seconde te verzenden naar uw IoT-Hub S1 (dit is een limiet van 100 per seconde D2C verzendt). Voor de eerste minuut of twee, worden de berichten onmiddellijk verwerkt. Echter, omdat het apparaat nog steeds meer berichten dan de limiet voor vertraging verzenden, IoT-Hub begint alleen 100 om berichten te verwerken per seconde en de rest in een wachtrij geplaatst. U begint dat u hogere latentie. Uiteindelijk u begint met het ophalen van `429 ThrottlingException` als de wachtrij opvullingen van en het 'aantal vertraging fouten' in [metrische gegevens van de IoT-Hub](iot-hub-metrics.md) begint verhogen.

### <a name="identity-registry-operations-throttle"></a>Registerbewerkingen voor identiteit beperken

Registerbewerkingen voor identiteit-apparaat zijn bedoeld voor gebruik tijdens runtime in Apparaatbeheer en de inrichting van scenario's. Lezen of bijwerken van een groot aantal apparaat-id's wordt ondersteund door [importeren en exporteren van taken](iot-hub-devguide-identity-registry.md#import-and-export-device-identities).

### <a name="device-connections-throttle"></a>Apparaatverbindingen beperken

De *apparaatverbindingen* vertraging bepaalt de snelheid waarmee nieuwe apparaatverbindingen kunnen worden gemaakt met een IoT-hub. De *apparaatverbindingen* vertraging heeft geen betrekking op het maximale aantal gelijktijdig verbonden apparaten. De *apparaatverbindingen* snelheid beperken, is afhankelijk van het aantal eenheden die zijn ingericht voor de IoT hub.

Bijvoorbeeld, als u één S1-eenheid koopt, krijgt u een vertraging van 100 verbindingen per seconde. Dus als u wilt verbinding maken met 100,, 000 apparaten, duurt het ten minste 1000 seconden (ongeveer 16 minuten). U kunt echter zo veel gelijktijdig verbonden apparaten als er apparaten zijn geregistreerd in uw register-id's hebben.


## <a name="other-limits"></a>Andere limieten

IoT Hub dwingt andere operationele beperkingen:

| Bewerking | Limiet |
| --------- | ----- |
| Apparaten | Het maximum aantal apparaten dat u verbinding met een enkele IoT-hub maken kunt is 1.000.000. De enige manier om deze limiet te verhogen, is contact opnemen met [Microsoft Support](https://azure.microsoft.com/support/options/).| 
| Bestand laden URI 's | 10.000 SAS URI's kan zijn uitgeschakeld voor een opslagaccount in één keer. <br/> Er kunnen 10 SAS URI's per apparaat tegelijk zijn uitgeschakeld. |
| Jobs<sup>1</sup> | Maximum aantal gelijktijdige taken is 1 (voor gratis en S1), 5 (voor S2) en 10 (voor S3). Echter, de maximale gelijktijdige [apparaat import/export-taken](iot-hub-bulk-identity-mgmt.md) is 1 voor alle lagen. <br/>Taakgeschiedenis blijft tot 30 dagen. |
| Extra eindpunten | Betaalde SKU kunnen hubs 10 extra eindpunten hebben. Gratis SKU-hubs kunnen één extra eindpunt hebben. |
| Regels voor berichtroutering | Betaalde SKU mogelijk hubs 100 regels voor doorsturen. Gratis SKU-hubs mogelijk vijf regels voor doorsturen. |
| Apparaat-naar-cloud-berichten | Maximale berichtgrootte van 256 KB |
| Cloud-naar-apparaat-berichten<sup>1</sup> | Maximale berichtgrootte 64 KB. Maximaal aantal in behandeling zijnde berichten voor de levering is 50. |
| Directe methode<sup>1</sup> | Directe methode die maximale nettolading is 128 KB. |
| Automatische apparaatconfiguraties<sup>1</sup> | 100 configuraties per betaalde SKU-hub. 20 configuraties per gratis SKU-hub. |
| Automatische Edge-implementaties<sup>1</sup> | 20 modules per implementatie. 100 implementaties per betaalde SKU-hub. 20 implementaties per gratis SKU-hub. |
| Twins<sup>1</sup> | Maximale grootte per dubbele punt (tags, de gewenste eigenschappen, gerapporteerde eigenschappen) is 8 KB |

<sup>1</sup>deze functie is niet beschikbaar in de basic-laag van IoT-Hub. Zie voor meer informatie, [u bij het kiezen van de juiste IoT-Hub](iot-hub-scaling.md).

## <a name="increasing-the-quota-or-throttle-limit"></a>De quota of vertraging limiet te verhogen

Op elk moment kunt u quota verhogen of limieten door beperken [het aantal ingerichte eenheden in een IoT-hub](iot-hub-upgrade.md).

## <a name="latency"></a>Latentie

IoT Hub streeft ernaar voor lage latentie voor alle bewerkingen. Echter, vanwege netwerkomstandigheden en andere onvoorspelbare factoren is er geen garantie een bepaalde latentie. Bij het ontwerpen van uw oplossing, moet u:

* Vermijd een veronderstellingen over de maximale latentie van elke bewerking die IoT Hub.
* Uw IoT-hub in de Azure-regio die het dichtst bij uw apparaten inrichten.
* Overweeg het gebruik van Azure IoT Edge latentiegevoelige bewerkingen uitvoeren op het apparaat of op een gateway dicht bij het apparaat.

Meerdere IoT Hub-eenheden van invloed zijn op beperking zoals eerder beschreven, maar niet beschikken over alle voordelen van de extra latentie of garanties.

Als u onverwachte verhogingen in operationele latentie wordt weergegeven, neem dan contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Volgende stappen

Voor een uitgebreide beschrijving van IoT-Hub beperkingsgedrag, Zie het blogbericht [beperking IoT-Hub en u](https://azure.microsoft.com/blog/iot-hub-throttling-and-you/).

Er zijn andere onderwerpen met naslaginformatie in deze IoT Hub developer guide:

* [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md)
