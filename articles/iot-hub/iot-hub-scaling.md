---
title: Schalen met Azure IoT Hub | Microsoft Docs
description: Klik hier voor meer informatie over het schalen van uw IoT-hub ter ondersteuning van de verwachte bericht doorvoer. Bevat een overzicht van de ondersteunde doorvoer voor elke laag en opties voor sharding.
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 00043293eb57768f0117e912bb67f02d088934f3
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="scale-your-iot-hub-solution"></a>Schalen van uw IoT hub-oplossing
Azure IoT Hub kan maximaal een miljoen gelijktijdig verbonden apparaten ondersteunen. Zie voor meer informatie [IoT Hub prijzen][lnk-pricing]. Elke eenheid IoT-Hub kunt een bepaald aantal dagelijkse berichten.

Als u wilt schalen goed uw oplossing, kunt u uw bepaalde gebruik van IoT-Hub. Houd rekening met de vereiste piek-doorvoer voor de volgende categorieën van bewerkingen in het bijzonder:

* Apparaat-naar-cloud-berichten
* Cloud-naar-apparaat-berichten
* Registerbewerkingen voor identiteit

Naast deze informatie doorvoer Zie [IoT Hub quota en vertragingen] [ IoT Hub quotas and throttles] en dienovereenkomstig ontwerpen van uw oplossing.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Apparaat-naar-cloud- en cloud-naar-apparaat bericht doorvoer
De beste manier om het formaat van een IoT Hub-oplossing is om te evalueren van het verkeer op basis van per eenheid.

Apparaat-naar-cloudberichten Volg deze richtlijnen volgehouden doorvoer.

| Laag | Volgehouden doorvoer | Continue verzendsnelheid |
| --- | --- | --- |
| S1 |Maximaal 1111 KB per minuut per eenheid<br/>(1,5 GB/dag/unit) |Gemiddelde van 278 berichten per minuut per eenheid<br/>(400.000 berichten per dag per eenheid) |
| S2 |Maximaal 16 MB per minuut per eenheid<br/>(22.8 GB/dag/unit) |Gemiddelde van 4,167 berichten per minuut per eenheid<br/>(6 miljoen berichten per dag per eenheid) |
| S3 |Maximaal 814 MB per minuut per eenheid<br/>(1144.4 GB/dag/unit) |Gemiddelde van 208,333 berichten per minuut per eenheid<br/>(300 miljoen berichten per dag per eenheid) |

## <a name="identity-registry-operation-throughput"></a>Identiteit register bewerking doorvoer
IoT Hub identiteit registerbewerkingen zijn niet moet runtime-bewerkingen, zoals ze voornamelijk zijn gerelateerd aan apparaten inrichten.

Zie voor specifieke burst prestaties cijfers [IoT Hub quota en vertragingen][IoT Hub quotas and throttles].

## <a name="sharding"></a>Sharding
Terwijl een enkele IoT-hub naar miljoenen apparaten uitbreiden kunt, vereist soms uw oplossing specifieke prestatiekenmerken die een enkele iothub kan niet worden gegarandeerd. In dat geval wordt het aanbevolen dat u uw apparaten in meerdere IoT hubs partitioneren. Meerdere IoT hubs verkeer bursts vloeiend en verkrijgen van de vereiste doorvoer of bewerking tarieven die vereist zijn.

## <a name="next-steps"></a>Volgende stappen
Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
