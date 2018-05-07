---
title: Schalen met Azure IoT Hub | Microsoft Docs
description: Klik hier voor meer informatie over het schalen van uw IoT-hub ter ondersteuning van de verwachte bericht doorvoer en de gewenste functies. Bevat een overzicht van de ondersteunde doorvoer voor elke laag en opties voor sharding.
services: iot-hub
documentationcenter: ''
author: kgremban
manager: timlt
editor: ''
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/02/2018
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 01e6c3a6fb922a649f0ae139af9c8515fcb8b2e0
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2018
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Kies de juiste IoT Hub-laag voor uw oplossing

Elke IoT-oplossing is verschillend zijn, dus Azure IoT Hub verschillende opties op basis van prijs- en schaalniveau biedt. In dit artikel is bedoeld om te evalueren van de behoeften van uw IoT-Hub. Raadpleeg voor informatie over IoT Hub categorieën prijzen [IoT Hub prijzen](https://azure.microsoft.com/pricing/details/iot-hub). 

Om te beslissen welke laag IoT Hub geschikt is voor uw oplossing, Stel uzelf twee vragen:

**Welke functies wilt ik gebruiken?**
Azure IoT Hub biedt twee lagen, basis en standaard, die afwijken van het aantal functies die ze ondersteunen. Als uw IoT-oplossing is gebaseerd op het verzamelen van gegevens van apparaten en centraal analyseren vervolgens de basisstaffel waarschijnlijk geschikt is voor u. Als u meer geavanceerde configuraties met IoT-apparaten op afstand beheren of een deel van uw werkbelastingen op de apparaten zelf distribueren, moet u rekening houden de prijscategorie standard. Voor welke functies zijn opgenomen in elke laag in detail blijven [basis en standaard lagen](#basic-and-standard-tiers).

**Hoeveel gegevens wilt ik dagelijks verplaatsen?**
Elke laag IoT Hub is beschikbaar in drie grootten, op basis van rond de doorvoer van hoeveel gegevens deze in een gegeven moment kunnen verwerken. Deze formaten worden numeriek aangeduid als 1, 2 en 3. Elke eenheid van een niveau 1 iothub kan bijvoorbeeld 400 duizend berichten per dag, verwerken, terwijl een eenheid niveau 3 300 miljoen kan verwerken. Voor meer informatie over de richtlijnen gegevens blijven [bericht doorvoer](#message-throughput).

## <a name="basic-and-standard-tiers"></a>Basis en standaard lagen

De prijscategorie standard van IoT Hub kunt u alle functies en is vereist voor een IoT-oplossingen die wilt maken gebruik van de mogelijkheden voor bidirectionele communicatie. De laag basic kunt een subset van de functies en is bedoeld voor IoT-oplossingen die slechts één richting communicatie van apparaten naar de cloud hoeft. Beide lagen bieden de dezelfde beveiligings- en -functies.

Wanneer u uw IoT-hub maakt u een upgrade kunt uitvoeren via de laag basic naar de prijscategorie standard zonder dat uw bestaande activiteiten worden onderbroken. Zie voor meer informatie [bijwerken van uw IoT-hub](iot-hub-upgrade.md).

| Mogelijkheid | Basislaag | Standaardlaag |
| ---------- | ---------- | ------------- |
| [Telemetrie voor apparaat-naar-cloud](iot-hub-devguide-messaging.md) | Ja | Ja |
| [Per apparaat-id](iot-hub-devguide-identity-registry.md) | Ja | Ja |
| [Berichtroutering](iot-hub-devguide-messages-read-custom.md) en [gebeurtenis raster-integratie](iot-hub-event-grid.md) | Ja | Ja |
| [Protocollen HTTP, AMQP en MQTT](iot-hub-devguide-protocols.md) | Ja | Ja |
| [Inrichting Device-Service](../iot-dps/about-iot-dps.md) | Ja | Ja |
| [Controle en diagnostische gegevens](iot-hub-monitor-resource-health.md) | Ja | Ja |
| [Messaging-cloud-naar-apparaat](iot-hub-devguide-c2d-guidance.md) |   | Ja |
| [Apparaat horende](iot-hub-devguide-device-twins.md), [Module horende](iot-hub-devguide-module-twins.md) en [Apparaatbeheer](iot-hub-device-management-overview.md) |   | Ja |
| [Azure IoT Edge](../iot-edge/how-iot-edge-works.md) |   | Ja |

IoT Hub biedt ook een gratis laag die is bedoeld voor testen en evalueren. De mogelijkheden van de standard-laag, maar beperkt messaging rechten heeft. U upgraden niet van de laag gratis naar basis of standaard. 

### <a name="iot-hub-rest-apis"></a>REST API's voor IoT Hub

Het verschil in ondersteunde mogelijkheden tussen de lagen van IoT Hub, basis en standaard betekent dat sommige API-aanroepen niet met hubs basisstaffel werken. De volgende tabel ziet u welke API's zijn beschikbaar: 

| API | Basislaag | Standaardlaag |
| --- | ---------- | ------------- |
| [Apparaat verwijderen](https://docs.microsoft.com/rest/api/iothub/deviceapi/deletedevice) | Ja | Ja |
| [Ophalen van apparaat](https://docs.microsoft.com/rest/api/iothub/deviceapi/getdevice) | Ja | Ja |
| Module verwijderen | Ja | Ja |
| Module ophalen | Ja | Ja |
| [Register statistieken opvragen](https://docs.microsoft.com/rest/api/iothub/deviceapi/getregistrystatistics) | Ja | Ja |
| [Ophalen van de statistieken van services](https://docs.microsoft.com/rest/api/iothub/deviceapi/getservicestatistics) | Ja | Ja |
| [Apparaat plaatsen](https://docs.microsoft.com/rest/api/iothub/deviceapi/putdevice) | Ja | Ja |
| Module plaatsen | Ja | Ja |
| [Query-apparaten](https://docs.microsoft.com/rest/api/iothub/deviceapi/querydevices) | Ja | Ja |
| Query-modules | Ja | Ja |
| [Bestand uploaden SAS-URI maken](https://docs.microsoft.com/rest/api/iothub/httpruntime/createfileuploadsasuri) | Ja | Ja |
| [Apparaat gebonden ontvangen](https://docs.microsoft.com/rest/api/iothub/httpruntime/receivedeviceboundnotification) | Ja | Ja |
| [De apparaatgebeurtenis verzenden](https://docs.microsoft.com/rest/api/iothub/httpruntime/senddeviceevent) | Ja | Ja |
| Module gebeurtenis verzenden | Ja | Ja |
| [Bestand Uploadstatus bijwerken](https://docs.microsoft.com/rest/api/iothub/httpruntime/updatefileuploadstatus) | Ja | Ja |
| [Apparaat bulkbewerking](https://docs.microsoft.com/rest/api/iothub/deviceapi/bulkdeviceoperation) | Ja, met uitzondering van IoT rand hebben | Ja | 
| [Opdracht wachtrij opschonen](https://docs.microsoft.com/rest/api/iothub/deviceapi/purgecommandqueue) |   | Ja |
| [Apparaat-twin ophalen](https://docs.microsoft.com/rest/api/iothub/devicetwinapi/getdevicetwin) |   | Ja |
| Module-twin ophalen |   | Ja |
| [Apparaat-methode worden aangeroepen](https://docs.microsoft.com/rest/api/iothub/devicetwinapi/invokedevicemethod) |   | Ja |
| [Apparaat-twin bijwerken](https://docs.microsoft.com/rest/api/iothub/devicetwinapi/updatedevicetwin) |   | Ja | 
| Module-twin bijwerken |   | Ja | 
| [Gebonden apparaatmeldingen afbreken](https://docs.microsoft.com/rest/api/iothub/httpruntime/abandondeviceboundnotification) |   | Ja |
| [Apparaat voltooien gebonden melding](https://docs.microsoft.com/rest/api/iothub/httpruntime/completedeviceboundnotification) |   | Ja |
| [Taak annuleren](https://docs.microsoft.com/rest/api/iothub/jobapi/canceljob) |   | Ja |
| [Taak maken](https://docs.microsoft.com/rest/api/iothub/jobapi/createjob) |   | Ja |
| [Taak ophalen](https://docs.microsoft.com/rest/api/iothub/jobapi/getjob) |   | Ja |
| [Query-taken](https://docs.microsoft.com/rest/api/iothub/jobapi/queryjobs) |   | Ja |

## <a name="message-throughput"></a>Bericht-doorvoer

De beste manier om het formaat van een IoT Hub-oplossing is om te evalueren van het verkeer op basis van per eenheid. Houd rekening met de vereiste piek-doorvoer voor de volgende categorieën van bewerkingen in het bijzonder:

* Apparaat-naar-cloud-berichten
* Cloud-naar-apparaat-berichten
* Registerbewerkingen voor identiteit

Verkeer wordt gemeten op basis van per eenheid, niet per hub. Een exemplaar van de Iothub niveau 1 of 2 hebben maximaal 200 eenheden die zijn gekoppeld. Een exemplaar van de Iothub niveau 3 kan maximaal 10 eenheden hebben. Wanneer u uw IoT-hub maakt kunt u het aantal eenheden of verplaatsen tussen de 1, 2 en 3 grootten binnen een bepaalde laag zonder dat uw bestaande activiteiten worden onderbroken. Zie voor meer informatie [bijwerken van uw IoT-Hub](iot-hub-upgrade.md).

Als een voorbeeld van de mogelijkheden van elke laag verkeer volgt apparaat-naar-cloudberichten volgehouden doorvoer:

| Laag | Volgehouden doorvoer | Continue verzendsnelheid |
| --- | --- | --- |
| B1, S1 |Maximaal 1111 KB per minuut per eenheid<br/>(1,5 GB/dag/unit) |Gemiddelde van 278 berichten per minuut per eenheid<br/>(400.000 berichten per dag per eenheid) |
| B2, S2 |Maximaal 16 MB per minuut per eenheid<br/>(22.8 GB/dag/unit) |Gemiddelde van 4,167 berichten per minuut per eenheid<br/>(6 miljoen berichten per dag per eenheid) |
| B3, S3 |Maximaal 814 MB per minuut per eenheid<br/>(1144.4 GB/dag/unit) |Gemiddelde van 208,333 berichten per minuut per eenheid<br/>(300 miljoen berichten per dag per eenheid) |

Naast deze informatie doorvoer Zie [IoT Hub quota en vertragingen] [ IoT Hub quotas and throttles] en dienovereenkomstig ontwerpen van uw oplossing.

### <a name="identity-registry-operation-throughput"></a>Identiteit register bewerking doorvoer
IoT Hub identiteit registerbewerkingen zijn niet moet runtime-bewerkingen, zoals ze voornamelijk zijn gerelateerd aan apparaten inrichten.

Zie voor specifieke burst prestaties cijfers [IoT Hub quota en vertragingen][IoT Hub quotas and throttles].

## <a name="sharding"></a>Sharding
Terwijl een enkele IoT-hub naar miljoenen apparaten uitbreiden kunt, vereist soms uw oplossing specifieke prestatiekenmerken die een enkele iothub kan niet worden gegarandeerd. In dat geval kunt u uw apparaten partitioneren over meerdere IoT hubs. Meerdere IoT hubs verkeer bursts vloeiend en verkrijgen van de vereiste doorvoer of bewerking tarieven die vereist zijn.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de mogelijkheden van de IoT Hub en details van de prestaties [IoT Hub prijzen] [koppeling prijzen] of [IoT Hub quota en vertragingen][IoT Hub quotas and throttles].
* Als u wilt wijzigen van uw IoT Hub-laag, volg de stappen in [Upgrade van uw IoT-hub](iot-hub-upgrade.md).

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
