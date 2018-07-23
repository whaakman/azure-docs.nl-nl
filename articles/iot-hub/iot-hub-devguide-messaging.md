---
title: Informatie over Azure IoT Hub-berichten | Microsoft Docs
description: Handleiding voor ontwikkelaars - apparaat-naar-cloud en cloud-naar-apparaat met IoT Hub-berichten. Bevat informatie over berichtindelingen en ondersteunde communicatieprotocollen.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: b0667f820145f16c75a07ebe1849e20d2de36cc7
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185506"
---
# <a name="device-to-cloud-and-cloud-to-device-messaging-with-iot-hub"></a>Apparaat-naar-cloud en cloud-naar-apparaat met IoT Hub-berichten

Gebruik IoT Hub-berichten om te communiceren met uw apparaten met:

* Verzenden van [apparaat-naar-cloud] [ lnk-d2c] berichten van uw apparaten in uw oplossing voor back-end.
* Verzenden van [cloud-naar-apparaat] [ lnk-c2d] berichten van de oplossing back-end voor uw apparaten.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Core-eigenschappen van de IoT Hub-berichten functionaliteit zijn de betrouwbaarheid en duurzaamheid van berichten. Deze eigenschappen inschakelen herstelmogelijkheden bij onregelmatige connectiviteit op het apparaat plaats en voor het laden van pieken in het aan de cloud voor verwerking van gebeurtenissen. IoT Hub implementeert *ten minste één keer* levering garanties voor zowel de apparaat-naar-cloud en de cloud-naar-apparaat-berichten.

Zie voor een inleiding tot de mogelijkheden van IoT-Hub, de [overzicht van de service Azure IoT Hub][lnk-iot-hub-overview].

## <a name="when-to-use-iot-hub-messaging"></a>Bij het gebruik van IoT Hub-berichten

Gebruik apparaat-naar-cloud-berichten voor het verzenden van time series telemetriegegevens en waarschuwingen van uw apparaat-app en cloud-naar-apparaat-berichten voor eenrichtingsverkeer meldingen naar uw apparaat-app.

* Raadpleeg [richtlijnen voor communicatie van apparaat-naar-cloud] [ lnk-d2c-guidance] als bij twijfel tussen het gebruik van apparaat-naar-cloud-berichten, gerapporteerde eigenschappen of bestand uploaden.
* Raadpleeg [Cloud-naar-apparaat communicatie richtlijnen] [ lnk-c2d-guidance] als bij twijfel tussen het gebruik van cloud-naar-apparaat-berichten, gewenste eigenschappen of directe methoden.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over IoT-Hub [apparaat-naar-cloud-berichten][lnk-d2c].
* Meer informatie over IoT-Hub [cloud-naar-apparaat-berichten][lnk-c2d].

[lnk-azure-iot]: ../iot-fundamentals/index.yml
[lnk-iot-hub-overview]: about-iot-hub.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-c2d]: iot-hub-devguide-messages-c2d.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md