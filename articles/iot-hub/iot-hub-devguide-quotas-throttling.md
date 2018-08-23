---
title: Informatie over Azure IoT Hub-quota en beperkingen | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijving van de quota die betrekking hebben op IoT-Hub en het verwachte gedrag voor bandbreedteregeling.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: 11cec9621ad72cfeaee45e4cd466430e64b9b836
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "42054854"
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referentie - IoT-Hub-quota en beperkingen

## <a name="quotas-and-throttling"></a>Quota en beperkingen
Elk Azure-abonnement zijn maximaal 50 IoT-hubs en maximaal 1 gratis hub.

Elke IoT-hub is ingericht met een bepaald aantal eenheden in een bepaalde laag. De laag en het aantal eenheden bepalen de maximale dagelijkse quotum voor berichten die u kunt verzenden. De grootte van het bericht gebruikt voor het berekenen van dat het dagelijkse quotum is 0,5 KB voor een gratis laag-hub en 4KB voor alle andere lagen. Zie voor meer informatie, [prijzen van Azure IoT Hub][lnk-pricing].

De laag bepaalt ook de bandbreedteregeling limieten die IoT Hub wordt afgedwongen op alle bewerkingen.

## <a name="operation-throttles"></a>Vertragingen in bewerking
Bewerking vertragingen zijn tarief beperkingen die worden toegepast in minuut bereiken en zijn bedoeld om misbruik te voorkomen. IoT-Hub probeert om te voorkomen dat fouten indien mogelijk worden geretourneerd, maar begint uitzonderingen retourneren als de vertraging wordt geschonden te lang.

Op elk moment kunt u quota's of beperkingslimieten verhogen door het aantal ingerichte eenheden in een IoT-hub.

De volgende tabel ziet u de afgedwongen beperkingen. Waarden verwijzen naar een afzonderlijke hub.

| Vertragen | Gratis, B1 en S1 | B2 en S2 | B3 en S3 | 
| -------- | ------- | ------- | ------- |
| Registerbewerkingen voor identiteit (maken, ophalen, weergeven, bijwerken en verwijderen) | 1.67/sec/Unit (100 per minuut per eenheid) | 1.67/sec/Unit (100 per minuut per eenheid) | 83.33/sec/Unit (5000 per minuut per eenheid) |
| Nieuwe apparaatverbindingen (deze limiet is van toepassing op de snelheid waarmee _nieuwe verbindingen_ tot stand worden gebracht, niet het totale aantal verbindingen) | Hogere van 100 per seconde of 12 per seconde per eenheid <br/> Twee S1-eenheden zijn bijvoorbeeld 2\*12 = 24 uur per dag nieuwe verbindingen per seconde, maar u hebt ten minste 100 nieuwe verbindingen per seconde voor uw-eenheden. Met negen S1-eenheden, hebt u 108 nieuwe verbindingen per seconde (9\*12) voor uw-eenheden. | nieuwe verbindingen per seconde per eenheid van de 120 | 6000 nieuwe verbindingen per seconde per eenheid |
| Apparaat-naar-cloud verzendt | Hogere van 100 per seconde of 12 per seconde per eenheid <br/> Twee S1-eenheden zijn bijvoorbeeld 2\*12 = 24 uur per dag/seconde, maar u hebt ten minste 100 per seconde voor uw-eenheden. Met negen S1-eenheden, hebt u 108 per seconde (9\*12) voor uw-eenheden. | 120 per seconde per eenheid | 6000 per seconde per eenheid |
| Cloud-naar-apparaat verzendt<sup>1</sup> | 1.67/sec/Unit (100 per minuut per eenheid) | 1.67/sec/Unit (100 per minuut per eenheid) | 83.33/sec/Unit (5000 per minuut per eenheid) |
| Cloud-naar-apparaat ontvangt<sup>1</sup> <br/> (alleen als apparaat maakt gebruik van HTTPS)| 16.67/sec/Unit (1000 per minuut per eenheid) | 16.67/sec/Unit (1000 per minuut per eenheid) | 833.33/sec/Unit (50000 per minuut per eenheid) |
| Bestand uploaden | 1.67 bestand uploaden meldingen per seconde per eenheid (100 per minuut per eenheid) | 1.67 bestand uploaden meldingen per seconde per eenheid (100 per minuut per eenheid) | 83.33 bestand uploaden meldingen per seconde per eenheid (5000 per minuut per eenheid) |
| Directe methoden<sup>1</sup> | 160KB/sec/unit<sup>2</sup> | 480KB/sec/unit<sup>2</sup> | 24MB/sec/unit<sup>2</sup> | 
| (Apparaat- en -module) leesbewerkingen Twin<sup>1</sup> | 10 per seconde | Hogere van 10 per seconde of 1 per seconde per eenheid | 50 per seconde per eenheid |
| Updates (apparaat- en -module) Twin<sup>1</sup> | 10 per seconde | Hogere van 10 per seconde of 1 per seconde per eenheid | 50 per seconde per eenheid |
| Taken operations<sup>1</sup> <br/> (maken, bijwerken, weergeven, verwijderen) | 1.67/sec/Unit (100 per minuut per eenheid) | 1.67/sec/Unit (100 per minuut per eenheid) | 83.33/sec/Unit (5000 per minuut per eenheid) |
| Taken apparaatbewerkingen<sup>1</sup> <br/> (dubbele bijwerken, rechtstreekse methode aanroepen) | 10 per seconde | Hogere van 10 per seconde of 1 per seconde per eenheid | 50 per seconde per eenheid |
| Configuraties en edge-implementaties<sup>1</sup> <br/> (maken, bijwerken, weergeven, verwijderen) | 0.33/sec/Unit (20 per minuut per eenheid) | 0.33/sec/Unit (20 per minuut per eenheid) | 0.33/sec/Unit (20 per minuut per eenheid) |


<sup>1</sup>deze functie is niet beschikbaar in de basic-laag van IoT-Hub. Zie voor meer informatie, [u bij het kiezen van de juiste IoT-Hub](iot-hub-scaling.md). <br/><sup>2</sup>meter formaat beperking is 8 KB.

De *apparaatverbindingen* vertraging bepaalt de snelheid waarmee nieuwe apparaatverbindingen kunnen worden gemaakt met een IoT-hub. De *apparaatverbindingen* vertraging heeft geen betrekking op het maximale aantal gelijktijdig verbonden apparaten. De vertraging is afhankelijk van het aantal eenheden die zijn ingericht voor de IoT hub.

Bijvoorbeeld, als u één S1-eenheid koopt, krijgt u een vertraging van 100 verbindingen per seconde. Dus als u wilt verbinding maken met 100.000 apparaten, duurt het ten minste 1000 seconden (ongeveer 16 minuten). U kunt echter zo veel gelijktijdig verbonden apparaten als er apparaten zijn geregistreerd in uw register-id's hebben.

Voor een uitgebreide beschrijving van IoT-Hub beperkingsgedrag, Zie het blogbericht [beperking IoT-Hub en u][lnk-throttle-blog].

> [!IMPORTANT]
> Registerbewerkingen voor identiteit zijn bedoeld voor gebruik tijdens runtime in Apparaatbeheer en de inrichting van scenario's. Lezen of bijwerken van een groot aantal apparaat-id's wordt ondersteund door [importeren en exporteren van taken][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Andere limieten

IoT Hub dwingt andere operationele beperkingen:

| Bewerking | Limiet |
| --------- | ----- |
| Bestand laden URI 's | 10000 SAS-URI's kan zijn uitgeschakeld voor een opslagaccount in één keer. <br/> Er kunnen 10 SAS URI's per apparaat tegelijk zijn uitgeschakeld. |
| Jobs<sup>1</sup> | Taakgeschiedenis blijft tot 30 dagen <br/> Maximum aantal gelijktijdige taken is 1 (voor gratis) en S1, 5 (voor S2), 10 (voor S3). |
| Extra eindpunten | Betaalde SKU kunnen hubs 10 extra eindpunten hebben. Gratis SKU-hubs kunnen één extra eindpunt hebben. |
| Regels voor berichtroutering | Betaalde SKU mogelijk hubs 100 regels voor doorsturen. Gratis SKU-hubs mogelijk vijf regels voor doorsturen. |
| Apparaat-naar-cloud-berichten | Maximale berichtgrootte van 256 KB |
| Cloud-naar-apparaat-berichten<sup>1</sup> | Maximale berichtgrootte 64 KB. Maximaal aantal in behandeling zijnde berichten voor de levering is 50. |
| Directe methode<sup>1</sup> | Directe methode die maximale nettolading is 128 KB. |
| Configuraties | 20 configuraties per hub. |
| Edge-implementaties | 20 implementaties per hub. 20 modules per implementatie. |
| Dubbels | Maximale grootte per dubbele punt (tags, de gewenste eigenschappen, gerapporteerde eigenschappen) is 8 KB |

<sup>1</sup>deze functie is niet beschikbaar in de basic-laag van IoT-Hub. Zie voor meer informatie, [u bij het kiezen van de juiste IoT-Hub](iot-hub-scaling.md).

> [!NOTE]
> Op dit moment is het maximum aantal apparaten dat u verbinding met een enkele IoT-hub maken kunt 500.000. Als u wilt om deze limiet te verhogen, neem dan contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="latency"></a>Latentie
IoT Hub streeft ernaar voor lage latentie voor alle bewerkingen. Echter, vanwege netwerkomstandigheden en andere onvoorspelbare factoren is er geen garantie een maximale latentie. Bij het ontwerpen van uw oplossing, moet u:

* Vermijd een veronderstellingen over de maximale latentie van elke bewerking die IoT Hub.
* Uw IoT-hub in de Azure-regio die het dichtst bij uw apparaten inrichten.
* Overweeg het gebruik van Azure IoT Edge latentiegevoelige bewerkingen uitvoeren op het apparaat of op een gateway dicht bij het apparaat.

Meerdere IoT Hub-eenheden van invloed zijn op beperking zoals eerder beschreven, maar niet beschikken over alle voordelen van de extra latentie of garanties.
Als u onverwachte verhogingen in operationele latentie wordt weergegeven, neem dan contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Volgende stappen
Er zijn andere onderwerpen met naslaginformatie in deze IoT Hub developer guide:

* [IoT Hub-eindpunten][lnk-devguide-endpoints]
* [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering][lnk-devguide-query]
* [IoT Hub MQTT-ondersteuning][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
