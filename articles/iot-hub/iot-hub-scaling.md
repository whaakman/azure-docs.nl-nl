---
title: Schalen met Azure IoT Hub | Microsoft Docs
description: Klik hier voor meer informatie over het schalen van uw IoT-hub ter ondersteuning van uw bericht verwachte doorvoer en de gewenste functies. Bevat een overzicht van de ondersteunde doorvoer voor elke laag en opties voor sharding.
author: wesmc7777
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: wesmc
ms.openlocfilehash: 0d40bfa3a4215b671fcd01402a2cbceaea0cd75d
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57536280"
---
# <a name="choose-the-right-iot-hub-tier-for-your-solution"></a>Kies de juiste IoT Hub-laag voor uw oplossing

Elke IoT-oplossing is verschillend zijn, zodat Azure IoT Hub verschillende opties op basis van prijzen en schaal biedt. Dit artikel is bedoeld voor het evalueren van de behoeften van uw IoT-Hub. Zie voor meer informatie over Prijscategorieën van IoT Hub-prijzen, [IoT Hub-prijzen](https://azure.microsoft.com/pricing/details/iot-hub). 

Om te beslissen welke IoT Hub-laag is geschikt voor uw oplossing, Stel uzelf twee vragen:

**Welke functies wilt ik gebruiken?**
Azure IoT Hub biedt twee lagen, basis en standaard, die afwijken van het aantal functies die erdoor worden ondersteund. Als uw IoT-oplossing is gebaseerd op gegevens van apparaten verzamelen en analyseren van deze centraal, zijn de basic-laag is waarschijnlijk geschikt is voor u. Als u meer geavanceerde configuraties gebruiken wilt voor het IoT-apparaten op afstand beheren of distribueren enkele van uw werklasten op de apparaten zelf, kunt u de standard-laag moet overwegen. Voor gedetailleerde informatie van deze functies zijn opgenomen in elke laag blijven [Basic en standard-laag](#basic-and-standard-tiers).

**Hoeveel gegevens wilt ik dagelijks verplaatsen?**
Elke laag van IoT Hub is beschikbaar in drie grootten, op basis van hoeveel gegevens doorvoer ze kunnen worden verwerkt in elke dag. Deze grootten zijn numeriek geïdentificeerd als 1, 2 en 3. Met elke eenheid van een niveau 1 IoT-hub kan bijvoorbeeld 400 duizend berichten per dag, verwerken, terwijl een niveau 3-eenheid 300 miljoen kan verwerken. Voor meer informatie over de richtlijnen van de gegevens nog steeds [bericht doorvoer](#message-throughput).

## <a name="basic-and-standard-tiers"></a>Basic en standard-laag

De prijscategorie standard van IoT Hub kunt u alle functies en is vereist voor een IoT-oplossingen te maken van de mogelijkheden met bi-directionele communicatie mogelijk te gebruiken. De basic-laag kunt een subset van de functies en is bedoeld voor IoT-oplossingen die hoeft alleen Unidirectioneel communicatie tussen apparaten en de cloud. Beide lagen bieden de dezelfde functies als verificatie en beveiliging.

Slechts één type [edition](https://azure.microsoft.com/pricing/details/iot-hub/) binnen een laag per IoT Hub kan worden gekozen. U kunt bijvoorbeeld een IoT-Hub maken met meerdere eenheden van S1, maar niet met een combinatie van eenheden van verschillende versies, zoals S1 en B3, of S1 en S2.

| Mogelijkheid | Basislaag | Gratis/Standard-laag |
| ---------- | ---------- | ------------- |
| [Apparaat-naar-cloud telemetrie](iot-hub-devguide-messaging.md) | Ja | Ja |
| [Per apparaat-id](iot-hub-devguide-identity-registry.md) | Ja | Ja |
| [Berichtroutering](iot-hub-devguide-messages-read-custom.md) en [Event Grid-integratie](iot-hub-event-grid.md) | Ja | Ja |
| [HTTP-, AMQP- en MQTT-protocollen](iot-hub-devguide-protocols.md) | Ja | Ja |
| [Device Provisioning Service](../iot-dps/about-iot-dps.md) | Ja | Ja |
| [Controle en diagnose](iot-hub-monitor-resource-health.md) | Ja | Ja |
| [Cloud-naar-apparaat-berichten](iot-hub-devguide-c2d-guidance.md) |   | Ja |
| [Apparaatdubbels](iot-hub-devguide-device-twins.md), [moduledubbels](iot-hub-devguide-module-twins.md), en [Apparaatbeheer](iot-hub-device-management-overview.md) |   | Ja |
| [Apparaat-streams (preview)](iot-hub-device-streams-overview.md) |   | Ja |
| [Azure IoT Edge](../iot-edge/about-iot-edge.md) |   | Ja |

IoT Hub biedt ook een gratis laag die is bedoeld voor test- en evaluatiedoeleinden. Het beschikt over alle mogelijkheden van de standard-laag, maar beperkt messaging-limiet. U upgraden niet van de laag gratis naar basis of standaard. 


## <a name="partitions"></a>Partities

Azure IoT-Hubs bevatten veel kernonderdelen van [Azure Event Hubs](../event-hubs/event-hubs-features.md), waaronder [partities](../event-hubs/event-hubs-features.md#partitions). Gebeurtenisstromen voor IoT-Hubs zijn doorgaans gevuld met binnenkomende telemetriegegevens die moet worden gerapporteerd door verschillende IoT-apparaten. Het partitioneren van de gebeurtenisstroom wordt gebruikt om contentions die zich voordoen bij het gelijktijdig lezen en schrijven naar gebeurtenisstromen verminderen. 

De limiet van de partitie wordt gekozen bij IoT Hub wordt gemaakt, en kan niet worden gewijzigd. De maximale partitielimiet voor basic-laag IoT-Hub en IoT-Hub in de standaardlaag is 32. De meeste IoT-hubs hoeft slechts 4 partities. Zie voor meer informatie over het bepalen van de partities, de Event Hubs-Veelgestelde vragen over [hoeveel partities heb ik nodig?](../event-hubs/event-hubs-faq.md#how-many-partitions-do-i-need)


## <a name="tier-upgrade"></a>Laag-upgrade

Als u uw IoT-hub maakt, kunt u upgraden vanaf de basis-laag naar de prijscategorie standard zonder dat uw bestaande bewerkingen wordt onderbroken. Zie voor meer informatie, [upgrade uitvoeren van uw IoT-hub](iot-hub-upgrade.md).

De partitieconfiguratie van de blijft ongewijzigd wanneer u van basic-laag naar de standard-laag migreert.


## <a name="iot-hub-rest-apis"></a>REST API's voor IoT Hub

Het verschil tussen de lagen basic en standard van IoT Hub-betekent dat sommige API-aanroepen die niet met hubs basic-laag werken in ondersteunde mogelijkheden. De volgende tabel ziet u welke API's zijn beschikbaar: 

| API | Basislaag | Gratis/Standard-laag |
| --- | ---------- | ------------- |
| [Apparaat verwijderen](https://docs.microsoft.com/rest/api/iothub/service/deletedevice) | Ja | Ja |
| [Apparaat](https://docs.microsoft.com/rest/api/iothub/service/getdevice) | Ja | Ja |
| Module verwijderen | Ja | Ja |
| Module ophalen | Ja | Ja |
| [Statistieken over register ophalen](https://docs.microsoft.com/rest/api/iothub/service/getdeviceregistrystatistics) | Ja | Ja |
| [Statistieken over services ophalen](https://docs.microsoft.com/rest/api/iothub/service/getservicestatistics) | Ja | Ja |
| [Maken of bijwerken van apparaat](https://docs.microsoft.com/rest/api/iothub/service/createorupdatedevice) | Ja | Ja |
| Put-module | Ja | Ja |
| [Query uitvoeren op IoT-Hub](https://docs.microsoft.com/rest/api/iothub/service/queryiothub) | Ja | Ja |
| Query-modules | Ja | Ja |
| [Bestand uploaden SAS-URI maken](https://docs.microsoft.com/rest/api/iothub/device/createfileuploadsasuri) | Ja | Ja |
| [Apparaat gebonden melding ontvangen](https://docs.microsoft.com/rest/api/iothub/device/receivedeviceboundnotification) | Ja | Ja |
| [Verzenden van apparaatgebeurtenis](https://docs.microsoft.com/rest/api/iothub/device/senddeviceevent) | Ja | Ja |
| Verzenden van gebeurtenissen van module | Ja | Ja |
| [Uploadstatus bestand bijwerken](https://docs.microsoft.com/rest/api/iothub/device/updatefileuploadstatus) | Ja | Ja |
| [Bulkbewerking voor apparaat](/rest/api/iot-dps/runbulkenrollmentoperation/runbulkenrollmentoperation) | Ja, met uitzondering van IoT Edge-mogelijkheden | Ja | 
| [Opdracht wachtrij leegmaken](https://docs.microsoft.com/rest/api/iothub/service/purgecommandqueue) |   | Ja |
| [Ophalen van apparaatdubbel](https://docs.microsoft.com/rest/api/iothub/service/gettwin) |   | Ja |
| Ophalen van de moduledubbel |   | Ja |
| [Apparaatmethode aanroepen](https://docs.microsoft.com/rest/api/iothub/service/invokedevicemethod) |   | Ja |
| [Dubbele apparaat bijwerken](https://docs.microsoft.com/rest/api/iothub/service/updatetwin) |   | Ja | 
| Bijwerken van de moduledubbel |   | Ja | 
| [Gebonden apparaatbericht afbreken](https://docs.microsoft.com/rest/api/iothub/device/abandondeviceboundnotification) |   | Ja |
| [Apparaatconfiguratie voltooien gebonden melding](https://docs.microsoft.com/rest/api/iothub/device/completedeviceboundnotification) |   | Ja |
| [Taak annuleren](https://docs.microsoft.com/rest/api/iothub/service/canceljob) |   | Ja |
| [Taak maken](https://docs.microsoft.com/rest/api/iothub/service/createjob) |   | Ja |
| [Taak ophalen](https://docs.microsoft.com/rest/api/iothub/service/getjob) |   | Ja |
| [Query-taken](https://docs.microsoft.com/rest/api/iothub/service/queryjobs) |   | Ja |

## <a name="message-throughput"></a>Doorvoer van berichten

De beste manier om de grootte van een IoT Hub-oplossing is om te evalueren van het verkeer op basis van per eenheid. In het bijzonder, houd rekening met de vereiste piek doorvoer voor de volgende categorieën van bewerkingen:

* Apparaat-naar-cloud-berichten
* Cloud-naar-apparaat-berichten
* Registerbewerkingen voor identiteit

Verkeer wordt gemeten op basis van per eenheid, niet per hub. Een niveau 1 of 2 IoT Hub-instantie kan maximaal 200 eenheden die zijn gekoppeld aan deze hebben. Een niveau 3 IoT Hub-instantie kan maximaal 10 eenheden hebben. Nadat u uw IoT-hub maken kunt u het aantal eenheden wijzigen of verplaatsen tussen de 1, 2 en 3-grootten in een specifieke laag zonder dat uw bestaande bewerkingen wordt onderbroken. Zie voor meer informatie, [upgrade uitvoeren van uw IoT-Hub](iot-hub-upgrade.md).

Als een voorbeeld van de mogelijkheden van elke laag van verkeer van apparaat-naar-cloud-berichten als volgt een doorvoer:

| Laag | Een doorvoer | Continue verzendsnelheid |
| --- | --- | --- |
| B1, S1 |Maximaal 1111 KB per minuut per eenheid<br/>(1,5 GB per dag per eenheid) |Gemiddelde van 278 berichten per minuut per eenheid<br/>(400.000 berichten per dag per eenheid) |
| B2, S2 |Maximaal 16 MB per minuut per eenheid<br/>(22.8 GB per dag per eenheid) |Gemiddelde van 4,167 berichten per minuut per eenheid<br/>(6 miljoen berichten per dag per eenheid) |
| B3, S3 |Maximaal 814 MB per minuut per eenheid<br/>(1144.4 GB per dag per eenheid) |Gemiddelde van 208,333 berichten per minuut per eenheid<br/>(300 miljoen berichten per dag per eenheid) |

Naast deze informatie doorvoer [IoT Hub-quota en vertragingen in] [ IoT Hub quotas and throttles] en ontwerpen van uw oplossing dienovereenkomstig.

### <a name="identity-registry-operation-throughput"></a>ID-register bewerking doorvoer
Registerbewerkingen voor identiteit van IoT-Hub zijn niet mag runtime-bewerkingen, zoals ze voornamelijk betrekking op het apparaat wordt ingericht hebben.

Zie voor specifieke burst prestatiecijfers [IoT Hub-quota en vertragingen in][IoT Hub quotas and throttles].

## <a name="auto-scale"></a>Automatisch schalen
Als u de limiet voor toegestane berichten op uw IoT-Hub nadert, kunt u deze [stappen voor het automatisch schalen](https://azure.microsoft.com/resources/samples/iot-hub-dotnet-autoscale/) moet worden verhoogd van een IoT Hub-eenheid in de dezelfde laag van IoT Hub.

## <a name="sharding"></a>Sharding
Terwijl een enkele IoT-hub naar miljoenen apparaten schalen kunt, vereist soms bepaalde prestatiekenmerken die een enkele IoT-hub kan niet garanderen dat uw oplossing. In dat geval kunt u uw apparaten partitioneren over meerdere IoT-hubs. Meerdere IoT-hubs vloeiende pieken in verkeer verwerken en downloaden van de vereiste doorvoer of bewerking tarieven die vereist zijn.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de mogelijkheden van IoT-Hub en details van de prestaties [IoT Hub-prijzen] [ lnk-pricing] of [IoT Hub-quota en vertragingen in] [ IoT Hub quotas and throttles].
* Als u wilt wijzigen van de laag van uw IoT Hub, volg de stappen in [Upgrade van uw IoT-hub](iot-hub-upgrade.md).

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
