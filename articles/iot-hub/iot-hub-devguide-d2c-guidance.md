---
title: Opties voor Azure IoT Hub apparaat-naar-cloud | Microsoft Docs
description: Handleiding voor ontwikkelaars - hulp bij het apparaat-naar-cloud-berichten, gemelde eigenschappen of uploaden bestand gebruiken voor communicatie van cloud-naar-apparaat.
author: fsautomata
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: a1881b74ba3f4f66e9e47b24d2ee8c8c17c5f05d
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634213"
---
# <a name="device-to-cloud-communications-guidance"></a>Richtlijnen voor apparaat-naar-cloud-communicatie
Bij het verzenden van informatie van de app voor het apparaat naar de oplossing voor back beschrijft-end, IoT-Hub de drie opties:

* [Apparaat-naar-cloudberichten] [ lnk-d2c] voor tijd reeks Telemetrie en waarschuwingen.
* [Apparaat-twin eigenschappen de gerapporteerd] [ lnk-twins] voor het melden van apparaat staat informatie zoals de beschikbare mogelijkheden voorwaarden of de status van langlopende werkstromen. Bijvoorbeeld, configuratie en software-updates.
* [Bestand uploaden] [ lnk-fileupload] voor media-bestanden en grote telemetrie batches geüpload door afwisselend verbonden apparaten of gecomprimeerd om bandbreedte besparen.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Hier volgt een gedetailleerde vergelijking van de verschillende communicatieopties voor apparaat-naar-cloud.

|  | Apparaat-naar-cloud-berichten | De apparaat-twin gemelde eigenschappen | Uploaden van bestanden |
| ---- | ------- | ---------- | ---- |
| Scenario | Telemetrie tijdreeks en waarschuwingen. Bijvoorbeeld, 256 KB sensor gegevensbatches verzonden om de 5 minuten. | Beschikbare mogelijkheden en voorwaarden. Bijvoorbeeld, de huidige apparaat connectiviteitsmodus zoals Mobiel of Wi-Fi. Langlopende werkstromen, zoals configuratie en software-updates synchroniseren. | Media-bestanden. Grote (meestal gecomprimeerde) telemetrie batches. |
| Opslaan en ophalen | Tijdelijk opgeslagen door de IoT Hub maximaal 7 dagen. Alleen opeenvolgende lezen. | In de apparaat-twin opgeslagen door de IoT Hub. Ophalen mogelijk gebruik van de [IoT Hub-querytaal][lnk-query]. | Opgeslagen in Azure Storage-account van gebruiker. |
| Grootte | Maximaal 256 KB-berichten. | Maximum aantal gemelde eigenschappen grootte is 8 KB. | Maximale bestandsgrootte die wordt ondersteund door Azure Blob Storage. |
| Frequentie | Hoog. Zie voor meer informatie [IoT Hub beperkt][lnk-quotas]. | Gemiddeld. Zie voor meer informatie [IoT Hub beperkt][lnk-quotas]. | Laag. Zie voor meer informatie [IoT Hub beperkt][lnk-quotas]. |
| Protocol | Beschikbaar op alle protocollen. | Beschikbaar met MQTT of AMQP. | Beschikbaar zijn wanneer u elk protocol voor, maar vereist HTTPS op het apparaat. |

Een toepassing moet mogelijk voor het verzenden van gegevens als een tijdreeks telemetrie of waarschuwing en het beschikbaar maken in de apparaat-twin. In dit scenario kunt u een van de volgende opties kiezen:

* De app apparaat verzendt een bericht apparaat-naar-cloud en rapporten van een wijziging van de eigenschap.
* De back-end oplossing kunt de gegevens opslaan in de apparaat-twin labels wanneer het bericht wordt ontvangen.

Aangezien de apparaat-naar-cloud-berichten inschakelen veel hogere doorvoer dan apparaat twin updates, is het soms wenselijk is om te voorkomen dat de apparaat-twin voor elk apparaat-naar-cloud bericht bijwerken.


[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-fileupload]: iot-hub-devguide-file-upload.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
