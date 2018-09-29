---
title: Opties voor Azure IoT Hub apparaat-naar-cloud | Microsoft Docs
description: Handleiding voor ontwikkelaars - richtlijnen over het gebruik van apparaat-naar-cloud-berichten, gerapporteerde eigenschappen of bestand uploaden voor cloud-naar-apparaat-berichten.
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: cd20c835fbb08ca0d44f6c77374ba52e19536d63
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452187"
---
# <a name="device-to-cloud-communications-guidance"></a>Richtlijnen voor communicatie van apparaat-naar-cloud

Bij het verzenden van informatie van de apparaat-app op de oplossing voor back-end, IoT Hub wordt aangegeven dat drie opties:

* [Apparaat-naar-cloud-berichten](iot-hub-devguide-messages-d2c.md) voor time series telemetriegegevens en waarschuwingen.

* [Apparaatdubbel de gerapporteerde eigenschappen](iot-hub-devguide-device-twins.md) voor het melden van informatie over de apparaatstatus, zoals de beschikbare mogelijkheden voorwaarden of de status van langlopende werkstromen. Bijvoorbeeld, configuratie en software-updates.

* [Bestand uploaden](iot-hub-devguide-file-upload.md) voor media-bestanden en grote telemetrie batches geüpload door tijdelijk verbonden apparaten of gecomprimeerd om bandbreedte te sparen.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Hier volgt een gedetailleerde vergelijking van de verschillende opties voor de communicatie van apparaat-naar-cloud.

|  | Apparaat-naar-cloud-berichten | Gerapporteerde eigenschappen van de apparaatdubbel | Uploaden van bestanden |
| ---- | ------- | ---------- | ---- |
| Scenario | Tijdreeks Telemetrie en waarschuwingen. Bijvoorbeeld, 256 KB sensor gegevensbatches verzonden om de 5 minuten. | Beschikbare mogelijkheden en voorwaarden. Bijvoorbeeld, de huidige apparaat connectiviteitsmodus zoals Mobiel of Wi-Fi. Langlopende werkstromen, zoals configuratie- en software-updates synchroniseren. | Mediabestanden. Groot (meestal gecomprimeerde) telemetrie batches. |
| Opslaan en ophalen | Tijdelijk opgeslagen door IoT Hub, maximaal 7 dagen. Alleen sequentieel lezen. | Opgeslagen door de IoT Hub in de apparaatdubbel. Met behulp van ophalen mogelijk de [IoT Hub-querytaal](iot-hub-devguide-query-language.md). | Opgeslagen in Azure Storage-account van gebruiker. |
| Grootte | Maximaal 256 KB-berichten. | Gerapporteerde eigenschappen maximale grootte is 8 KB. | Maximale bestandsgrootte die wordt ondersteund door Azure Blob Storage. |
| Frequentie | Hoog. Zie voor meer informatie, [IoT-Hub beperkt](iot-hub-devguide-quotas-throttling.md). | Gemiddeld. Zie voor meer informatie, [IoT-Hub beperkt](iot-hub-devguide-quotas-throttling.md). | Laag. Zie voor meer informatie, [IoT-Hub beperkt](iot-hub-devguide-quotas-throttling.md). |
| Protocol | Beschikbaar op alle protocollen. | Beschikbaar met behulp van MQTT- of AMQP. | Beschikbaar zijn wanneer u elk protocol voor, maar vereist HTTPS op het apparaat. |

Een toepassing mogelijk nodig voor het verzenden van gegevens als een tijdreeks telemetrie of waarschuwing en het beschikbaar maken in de apparaatdubbel. In dit scenario kunt u een van de volgende opties kiezen:

* De apparaat-app verzendt een bericht apparaat-naar-cloud en rapporten van een eigenschap wijzigen.
* De back-end oplossing kunt de gegevens opslaan in de apparaatdubbel-tags wanneer wordt het bericht ontvangen.

Sinds de apparaat-naar-cloud-berichten inschakelen voor een veel hogere doorvoer dan apparaatdubbel werkt, is het soms wenselijk is om te voorkomen dat het bijwerken van de apparaatdubbel voor elk bericht dat apparaat-naar-cloud.