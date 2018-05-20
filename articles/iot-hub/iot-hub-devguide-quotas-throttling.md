---
title: Azure IoT Hub quota's begrijpen en beperking | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijving van de quota die betrekking hebben op de IoT Hub en het verwachte gedrag voor bandbreedteregeling.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 425e1b08-8789-4377-85f7-c13131fae4ce
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: 026ff0394e988081bdc581bf001417b13e44427b
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referentie - IoT-Hub quota's en beperking

## <a name="quotas-and-throttling"></a>Quota en beperkingen
Elk Azure-abonnement kan maximaal 10 IoT hubs en maximaal 1 gratis hub hebben.

Elke IoT-hub is ingericht met een bepaald aantal eenheden in een bepaalde laag. Zie voor meer informatie [prijzen van Azure IoT Hub][lnk-pricing]. De laag en het aantal eenheden bepalen het dagelijkse maximumquotum van berichten die u kunt verzenden.

De laag bepaalt ook de bandbreedteregeling beperkingen die IoT Hub worden afgedwongen voor alle bewerkingen.

## <a name="operation-throttles"></a>Bewerking vertragingen
Bewerking vertragingen zijn snelheid beperkingen die zijn toegepast in minuut bereiken en zijn bedoeld om te voorkomen dat misbruik. IoT Hub wordt geprobeerd om te voorkomen dat er fouten indien mogelijk wordt geretourneerd, maar begint uitzonderingen retourneren als de beperking wordt geschonden te lang.

Op elk gewenst kunt u quota's of versnelling limieten verhogen door het aantal ingerichte eenheden van IoT-hub te verhogen.

De volgende tabel toont de afgedwongen vertragingen. Waarden verwijzen naar een afzonderlijke hub.

| Vertragen | Gratis B1 en S1 | B2 en S2 | B3 en S3 | 
| -------- | ------- | ------- | ------- |
| Identiteit registerbewerkingen (maken, ophalen, weergeven, bijwerken en verwijderen) | 1.67/sec/Unit (min-100/unit) | 1.67/sec/Unit (min-100/unit) | 83.33/sec/Unit (min-5000/unit) |
| Nieuwe apparaatverbindingen (deze limiet is van toepassing op de snelheid waarmee _nieuwe verbindingen_ tot stand worden gebracht, niet het totale aantal verbindingen) | Hogere van 100 per seconde of sec-12-eenheid <br/> Twee S1 eenheden zijn bijvoorbeeld 2\*12 = 24 nieuwe verbindingen dat per seconde, maar u hebt u ten minste 100 nieuwe verbindingen dat per seconde in de eenheden. Met negen S1 eenheden, hebt u 108 nieuwe verbindingen dat per seconde (9\*12) voor uw units. | nieuwe verbindingen/sec/eenheid van de 120 | 6000 nieuwe verbindingen/sec/eenheid |
| Apparaat-naar-cloud verzendt | Hogere van 100 per seconde of sec-12-eenheid <br/> Twee S1 eenheden zijn bijvoorbeeld 2\*12 = 24/sec, maar u hebt ten minste 100 per seconde voor uw units. Met negen S1 eenheden, hebt u 108 per seconde (9\*12) voor uw units. | sec-120-eenheid | sec-6000-eenheid |
| Cloud-naar-apparaat verzendt<sup>1</sup> | 1.67/sec/Unit (min-100/unit) | 1.67/sec/Unit (min-100/unit) | 83.33/sec/Unit (min-5000/unit) |
| Cloud-naar-apparaat ontvangt<sup>1</sup> <br/> (alleen als apparaat maakt gebruik van HTTPS)| 16.67/sec/Unit (min-1000/unit) | 16.67/sec/Unit (min-1000/unit) | 833.33/sec/Unit (min-50000/unit) |
| Bestand uploaden | 1.67 bestand uploaden meldingen/sec/eenheid (min-100/unit) | 1.67 bestand uploaden meldingen/sec/eenheid (min-100/unit) | 83.33 bestand uploaden meldingen/sec/eenheid (min-5000/unit) |
| Directe methoden<sup>1</sup> | 160KB/sec/unit<sup>2</sup> | 480KB/sec/unit<sup>2</sup> | 24MB/sec/unit<sup>2</sup> | 
| (Apparaat en module) leest Twin<sup>1</sup> | 10 per seconde | Hogere van 10 per seconde of sec-1-eenheid | sec-50-eenheid |
| Updates (apparaat en module) Twin<sup>1</sup> | 10 per seconde | Hogere van 10 per seconde of sec-1-eenheid | sec-50-eenheid |
| Taken operations<sup>1</sup> <br/> (maken, bijwerken, weergeven, verwijderen) | 1.67/sec/Unit (min-100/unit) | 1.67/sec/Unit (min-100/unit) | 83.33/sec/Unit (min-5000/unit) |
| Taken per apparaat bewerking doorvoer<sup>1</sup> | 10 per seconde | Hogere van 10 per seconde of sec-1-eenheid | sec-50-eenheid |
| Configuraties en implementaties van de rand<sup>1</sup> <br/> (maken, bijwerken, weergeven, verwijderen) | 0.33/sec/Unit (min-20/unit) | 0.33/sec/Unit (min-20/unit) | 0.33/sec/Unit (min-20/unit) |


<sup>1</sup>deze functie is niet beschikbaar in de basisstaffel van IoT Hub. Zie voor meer informatie [het kiezen van de juiste IoT-Hub](iot-hub-scaling.md). <br/><sup>2</sup>beperking van de grootte van de meter is 8 KB.

De *apparaatverbindingen* versnelling bepaalt de snelheid waarmee nieuwe apparaatverbindingen worden met een IoT-hub gemaakt kunnen. De *apparaatverbindingen* versnelling heeft geen betrekking op het maximale aantal gelijktijdig verbonden apparaten. De beperking, is afhankelijk van het aantal eenheden die zijn ingericht voor de IoT-hub.

Als u één eenheid S1 koopt, krijgt u bijvoorbeeld een vertraging van 100 verbindingen per seconde. Daarom voor 100.000 apparaten verbinding, duurt het minimaal 1000 seconden (ongeveer 16 minuten). U kunt echter zo veel gelijktijdig verbonden apparaten als er apparaten zijn geregistreerd in het identiteitenregister hebben.

Voor een gedetailleerdere beschrijving van IoT Hub beperking gedrag, Zie het blogbericht [IoT-Hub bandbreedtebeperking en u][lnk-throttle-blog].

> [!IMPORTANT]
> Bewerkingen in het systeemregister identiteit zijn bedoeld voor gebruik tijdens runtime in Apparaatbeheer en inrichting van scenario's. Lezen of bijwerken van een groot aantal apparaat-id's wordt ondersteund door [importeren en exporteren van taken][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Andere limieten

IoT Hub worden afgedwongen andere operationele beperkingen:

| Bewerking | Limiet |
| --------- | ----- |
| Bestand uploaden URI 's | 10000 SAS-URI's kan worden uit voor een opslagaccount in één keer. <br/> Er kunnen 10 SAS URI's per apparaat tegelijk zijn uitgeschakeld. |
| Jobs<sup>1</sup> | Taakgeschiedenis blijft tot 30 dagen <br/> Maximum aantal gelijktijdige taken is 1 (voor gratis) en S1, 5 (voor S2), 10 (voor S3). |
| Extra eindpunten | SKU betaald misschien hubs 10 extra eindpunten. Beschikbare SKU-hubs kunnen één extra eindpunt hebben. |
| Regels voor het bericht doorsturen | SKU betaald misschien hubs 100 routeringsregels. Beschikbare SKU-hubs misschien vijf routeringsregels. |
| Apparaat-naar-cloud-berichten | Maximale berichtgrootte van 256 KB |
| Cloud-naar-apparaat messaging<sup>1</sup> | Maximale berichtgrootte 64 KB. Maximaal aantal berichten voor de levering van in behandeling is 50. |
| Directe methode<sup>1</sup> | Directe methode maximale nettolading is 128 KB. |
| Configuraties | 20 configuraties per hub. |
| Edge-implementaties | 20 implementaties per hub. 20 modules per implementatie. |
| Horende | Maximale grootte per twin sectie (tags, de gewenste eigenschappen, gemelde eigenschappen) is 8 KB |

<sup>1</sup>deze functie is niet beschikbaar in de basisstaffel van IoT Hub. Zie voor meer informatie [het kiezen van de juiste IoT-Hub](iot-hub-scaling.md).

> [!NOTE]
> Het maximum aantal apparaten dat u verbinding met een enkele IoT-hub maken kunt is momenteel 500.000. Als u deze limiet verhogen wilt, neem dan contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="latency"></a>Latentie
IoT Hub wil lage latentie te bieden voor alle bewerkingen. Als gevolg van netwerkomstandigheden en andere factoren onvoorspelbare garantie er een maximale latentie geen. Bij het ontwerpen van uw oplossing, moet u het volgende doen:

* Vermijd een veronderstellingen over de maximale vertraging van elke bewerking IoT Hub.
* Uw IoT-hub in de Azure-regio die het dichtst bij uw apparaten inrichten.
* Overweeg het gebruik van Azure IoT rand latentie gevoelig bewerkingen uitvoeren op het apparaat of op een gateway dicht bij het apparaat.

Meerdere IoT Hub-eenheden invloed op beperking zoals eerder beschreven, maar geen eventuele extra latentie voordelen of garanties bieden.
Als er onverwachte toename in latentie van de bewerking, neem dan contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Volgende stappen
Er zijn andere onderwerpen met naslaginformatie in deze handleiding voor ontwikkelaars van IoT-Hub:

* [Eindpunten van IoT-Hub][lnk-devguide-endpoints]
* [IoT Hub-querytaal voor apparaat horende, taken en het routeren van berichten][lnk-devguide-query]
* [IoT Hub MQTT-ondersteuning][lnk-devguide-mqtt]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[lnk-throttle-blog]: https://azure.microsoft.com/blog/iot-hub-throttling-and-you/
[lnk-importexport]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
