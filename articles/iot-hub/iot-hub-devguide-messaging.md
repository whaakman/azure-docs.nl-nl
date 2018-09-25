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
ms.openlocfilehash: 1915c4bc6cd611479c7575179d8fe64def8895eb
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46956369"
---
# <a name="send-device-to-cloud-and-cloud-to-device-messages-with-iot-hub"></a>Apparaat-naar-cloud en cloud-naar-apparaat-berichten verzenden met IoT Hub

IoT Hub kunt bidirectionele communicatie met uw apparaten. Gebruik IoT Hub berichten om te communiceren met uw apparaten met het verzenden van berichten van uw apparaten naar de back-end van uw oplossingen en het verzenden van opdrachten van de back-end van uw IoT-oplossingen op uw apparaten. Meer informatie over de [IoT Hub-berichtindeling](../iot-hub/iot-hub-devguide-messages-construct.md).

## <a name="sending-device-to-cloud-messages-to-iot-hub"></a>Verzenden van apparaat-naar-cloud-berichten naar IoT Hub

IoT Hub is een ingebouwde service-eindpunt dat kan worden gebruikt door de back-end-services om te lezen van berichten over telemetrie van uw apparaten. Dit eindpunt is compatibel met [Event Hubs](https://docs.microsoft.com/azure/event-hubs/) en u kunt standard IoT Hub-SDK's voor [lezen van deze ingebouwde eindpunt]((https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin)).

IoT Hub worden ook ondersteund [aangepaste eindpunten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-endpoints#custom-endpoints) die kunnen worden gedefinieerd door gebruikers voor het verzenden van apparaat telemetriegegevens en gebeurtenissen voor Azure-services met behulp van [berichtroutering](iot-hub-devguide-messages-d2c.md).

## <a name="sending-cloud-to-device-messages-from-iot-hub"></a>Verzenden van cloud-naar-apparaat-berichten uit IoT Hub

U kunt verzenden [cloud-naar-apparaat](iot-hub-devguide-messages-c2d.md) berichten van de oplossing back-end voor uw apparaten.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Core-eigenschappen van de IoT Hub-berichten functionaliteit zijn de betrouwbaarheid en duurzaamheid van berichten. Deze eigenschappen inschakelen herstelmogelijkheden bij onregelmatige connectiviteit op het apparaat plaats en voor het laden van pieken in het aan de cloud voor verwerking van gebeurtenissen. IoT Hub implementeert *ten minste één keer* levering garanties voor zowel de apparaat-naar-cloud en de cloud-naar-apparaat-berichten.

## <a name="choosing-the-right-type-of-iot-hub-messaging"></a>Kies het juiste type IoT-Hub-berichten

Gebruik apparaat-naar-cloud-berichten voor het verzenden van time series telemetriegegevens en waarschuwingen van uw apparaat-app en cloud-naar-apparaat-berichten voor eenrichtingsverkeer meldingen naar uw apparaat-app.

* Raadpleeg [richtlijnen voor communicatie van apparaat-naar-cloud](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-d2c-guidance) om te kiezen tussen het apparaat-naar-cloud-berichten, gerapporteerde eigenschappen of bestand uploaden.
* Raadpleeg [Cloud-naar-apparaat communicatie richtlijnen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-c2d-guidance) te kiezen tussen cloud-naar-apparaat-berichten, gewenste eigenschappen of directe methoden.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over IoT-Hub [berichtroutering](iot-hub-devguide-messages-d2c.md).
* Meer informatie over IoT-Hub [cloud-naar-apparaat-berichten](iot-hub-devguide-messages-c2d.md).