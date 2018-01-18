---
title: Azure IoT Hub quota's begrijpen en beperking | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijving van de quota die betrekking hebben op de IoT Hub en het verwachte gedrag voor bandbreedteregeling.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 425e1b08-8789-4377-85f7-c13131fae4ce
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: dobett
ms.openlocfilehash: 68a6e999ac0ffe97c08b6420dd6e71d7154b5de8
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referentie - IoT-Hub quota's en beperking

## <a name="quotas-and-throttling"></a>Quota en beperkingen
Elk Azure-abonnement kan maximaal 10 IoT hubs en maximaal 1 gratis hub hebben.

Elke IoT-hub is ingericht met een bepaald aantal eenheden in een specifieke SKU (Zie voor meer informatie [prijzen van Azure IoT Hub][lnk-pricing]). De SKU en het aantal eenheden bepalen het dagelijkse maximumquotum van berichten die u kunt verzenden.

De SKU bepaalt ook de bandbreedteregeling beperkingen die IoT Hub worden afgedwongen voor alle bewerkingen.

## <a name="operation-throttles"></a>Bewerking vertragingen
Bewerking vertragingen zijn snelheid beperkingen die zijn toegepast in de minuut bereiken en zijn bedoeld om te voorkomen dat misbruik. IoT Hub wordt geprobeerd om te voorkomen dat er fouten indien mogelijk wordt geretourneerd, maar deze wordt gestart uitzonderingen retourneren als de beperking wordt geschonden te lang.

De volgende tabel toont de afgedwongen vertragingen. Waarden verwijzen naar een afzonderlijke hub.

| Vertragen | Gratis en S1 hubs | S2-hubs | S3-hubs | 
| -------- | ------- | ------- | ------- |
| Identiteit registerbewerkingen (maken, ophalen, weergeven, bijwerken en verwijderen) | 1.67/sec/Unit (min-100/unit) | 1.67/sec/Unit (min-100/unit) | 83.33/sec/Unit (min-5000/unit) |
| Apparaatverbindingen | Hogere van 100 per seconde of sec-12-eenheid <br/> Twee S1 eenheden zijn bijvoorbeeld 2\*12 = 24/sec, maar u hebt ten minste 100 per seconde voor uw units. Met negen S1 eenheden, hebt u 108 per seconde (9\*12) voor uw units. | sec-120-eenheid | 6000/sec/unit |
| Apparaat-naar-cloud verzendt | Hogere van 100 per seconde of sec-12-eenheid <br/> Twee S1 eenheden zijn bijvoorbeeld 2\*12 = 24/sec, maar u hebt ten minste 100 per seconde voor uw units. Met negen S1 eenheden, hebt u 108 per seconde (9\*12) voor uw units. | sec-120-eenheid | 6000/sec/unit |
| Cloud-naar-apparaat verzendt | 1.67/sec/Unit (min-100/unit) | 1.67/sec/Unit (min-100/unit) | 83.33/sec/Unit (min-5000/unit) |
| Cloud-naar-apparaat ontvangt <br/> (alleen als apparaat maakt gebruik van HTTPS)| 16.67/sec/Unit (min-1000/unit) | 16.67/sec/Unit (min-1000/unit) | 833.33/sec/Unit (min-50000/unit) |
| Bestand uploaden | 1.67 bestand uploaden meldingen/sec/eenheid (min-100/unit) | 1.67 bestand uploaden meldingen/sec/eenheid (min-100/unit) | 83.33 bestand uploaden meldingen/sec/eenheid (min-5000/unit) |
| Directe methoden | 160KB/sec/unit<sup>1</sup> | 480KB/sec/unit<sup>1</sup> | 24MB/sec/unit<sup>1</sup> | 
| Apparaatdubbel leest | 10/sec | Hogere van 10 per seconde of sec-1-eenheid | 50/sec/unit |
| Apparaatdubbel werkt bij | 10/sec | Hogere van 10 per seconde of sec-1-eenheid | 50/sec/unit |
| Taakbewerkingen <br/> (maken, bijwerken, weergeven, verwijderen) | 1.67/sec/Unit (min-100/unit) | 1.67/sec/Unit (min-100/unit) | 83.33/sec/Unit (min-5000/unit) |
| Doorvoer van taken per apparaat bewerkingen | 10/sec | Hogere van 10 per seconde of sec-1-eenheid | 50/sec/unit |

<sup>1</sup>beperking van de grootte van de meter is 8 KB

Het is belangrijk om te verduidelijken dat de *apparaatverbindingen* versnelling bepaalt de snelheid waarmee nieuwe apparaatverbindingen worden met een IoT-hub gemaakt kunnen. De *apparaatverbindingen* versnelling heeft geen betrekking op het maximale aantal gelijktijdig verbonden apparaten. De beperking, is afhankelijk van het aantal eenheden die zijn ingericht voor de IoT-hub.

Als u één eenheid S1 koopt, krijgt u bijvoorbeeld een vertraging van 100 verbindingen per seconde. Daarom voor 100.000 apparaten verbinding, duurt het minimaal 1000 seconden (ongeveer 16 minuten). U kunt echter zo veel gelijktijdig verbonden apparaten als er apparaten zijn geregistreerd in het identiteitenregister hebben.

Voor een gedetailleerdere beschrijving van IoT Hub beperking gedrag, Zie het blogbericht [IoT-Hub bandbreedtebeperking en u][lnk-throttle-blog].

> [!NOTE]
> Op elk gewenst is het mogelijk om te quota's of versnelling limieten verhogen door het aantal ingerichte eenheden van IoT-hub te verhogen.
> 
> [!IMPORTANT]
> Bewerkingen in het systeemregister identiteit zijn bedoeld voor gebruik tijdens runtime in Apparaatbeheer en inrichting van scenario's. Lezen of bijwerken van een groot aantal apparaat-id's wordt ondersteund door [importeren en exporteren van taken][lnk-importexport].
> 
> 

## <a name="other-limits"></a>Andere limieten

IoT Hub worden afgedwongen andere operationele beperkingen:

| Bewerking | Limiet |
| --------- | ----- |
| Bestand uploaden URI 's | 10000 SAS-URI's kan worden uit voor een opslagaccount in één keer. <br/> Er kunnen 10 SAS URI's per apparaat tegelijk zijn uitgeschakeld. |
| Taken | Taakgeschiedenis blijft tot 30 dagen <br/> Maximum aantal gelijktijdige taken is 1 (voor gratis) en S1, 5 (voor S2), 10 (voor S3). |
| Extra eindpunten | SKU betaald misschien hubs 10 extra eindpunten. Beschikbare SKU-hubs kunnen één extra eindpunt hebben. |
| Regels voor het bericht doorsturen | SKU betaald misschien hubs 100 routeringsregels. Beschikbare SKU-hubs misschien vijf routeringsregels. |
| Apparaat-naar-cloud-berichten | Maximale berichtgrootte van 256 KB |
| Messaging-cloud-naar-apparaat | Maximale berichtgrootte van 64 KB |
| Messaging-cloud-naar-apparaat | Maximaal aantal berichten voor de levering van in behandeling is 50 |
| Directe methode | Directe methode maximale nettolading is 128KB |

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
