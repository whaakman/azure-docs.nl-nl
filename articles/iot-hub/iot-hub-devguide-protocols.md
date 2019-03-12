---
title: Azure IoT Hub-communicatie-protocollen en poorten | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijving van de ondersteunde communicatieprotocollen voor communicatie van apparaat-naar-cloud en cloud-naar-apparaat en de poortnummers die geopend worden moet.
author: robinsh
manager: philmea
ms.author: robin.shahan
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.openlocfilehash: f6c39765c9133c9bf295d4225c332fda1140a13b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57532681"
---
# <a name="reference---choose-a-communication-protocol"></a>Verwijzen naar - een communicatieprotocol kiezen

IoT Hub kunt u apparaten met de volgende protocollen voor communicatie van apparaat:

* [MQTT](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf)
* MQTT via WebSockets
* [AMQP](https://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf)
* AMQP via WebSockets
* HTTPS

Zie voor meer informatie over hoe deze protocollen specifieke IoT-Hub-functies ondersteunen [richtlijnen voor communicatie van apparaat-naar-cloud](iot-hub-devguide-d2c-guidance.md) en [Cloud-to-device communications guidance](iot-hub-devguide-c2d-guidance.md).

De volgende tabel bevat de op hoog niveau aanbevelingen voor de keuze van protocol:

| Protocol | Wanneer u dit protocol moet kiezen |
| --- | --- |
| MQTT <br> MQTT via WebSocket |Gebruik op alle apparaten die niet nodig hebben om meerdere apparaten (elk met een eigen referenties per apparaat) via de dezelfde TLS-verbinding verbinding te maken. |
| AMQP <br> AMQP via WebSocket |Op veld en cloud-gateways gebruiken om te profiteren van multiplexing via apparaten verbinding. |
| HTTPS |Gebruik voor de apparaten die niet kan ondersteuning voor andere protocollen bieden. |

De volgende punten overwegen wanneer u ervoor uw protocol voor communicatie van apparaat kiest:

* **Cloud-naar-apparaat patroon**. HTTPS heeft geen een efficiënte manier voor het implementeren van server-push. Als zodanig, wanneer u HTTPS gebruikt, pollen apparaten IoT Hub voor cloud-naar-apparaat-berichten. Deze benadering is inefficiënt voor het apparaat en de IoT Hub. Onder de huidige richtlijnen voor HTTPS, moet elk apparaat gecontroleerd op berichten elke 25 minuten of langer. MQTT en AMQP ondersteuning voor server push bij het ontvangen van berichten van cloud-naar-apparaat. Ze inschakelen direct pushes van berichten uit IoT Hub op het apparaat. Als levering latentie is een probleem, zijn MQTT- of AMQP de beste protocollen gebruiken. Voor zelden verbonden apparaten, wordt HTTPS als goed werkt.

* **Veld gateways**. Wanneer u MQTT- en HTTPS, u meerdere apparaten (elk met een eigen referenties per apparaat) kan geen verbinding kunt maken met behulp van dezelfde TLS-verbinding. Voor [veld gatewayscenario's](iot-hub-devguide-endpoints.md#field-gateways) waarvoor een TLS-verbinding tussen de veldgateway en IoT Hub voor elk verbonden apparaat vereist, zijn deze protocollen suboptimale.

* **Lage resource apparaten**. Het MQTT- en HTTPS-bibliotheken hebben een kleinere footprint dan de AMQP-bibliotheken. Als het apparaat heeft beperkte resources (bijvoorbeeld minder dan 1 MB RAM-geheugen), kunnen deze protocollen daarom alleen protocolimplementatie beschikbaar zijn.

* **Netwerk-traversal**. Het AMQP-protocol gebruikt poort 5671 en MQTT luistert op poort 8883. Gebruik van deze poorten kan problemen veroorzaken in netwerken die niet-HTTPS-protocollen worden gesloten. MQTT via WebSockets, AMQP via WebSockets of HTTPS in dit scenario gebruiken.

* **Nettolading**. MQTT en AMQP zijn binaire protocollen, die leiden tot meer compacte nettoladingen dan HTTPS.

> [!WARNING]
> Wanneer u HTTPS gebruikt, moet elk apparaat voor cloud-naar-apparaatberichten pollen elke 25 minuten of langer. Tijdens de ontwikkeling is het echter aanvaardbaar is voor het pollen vaker dan elke 25 minuten.

## <a name="port-numbers"></a>Poortnummers

Apparaten kunnen communiceren met IoT Hub in Azure met behulp van verschillende protocollen. Normaal gesproken wordt de keuze van protocol bepaald door de specifieke vereisten van de oplossing. De volgende tabel bevat de uitgaande poorten die geopend voor een apparaat worden moeten te kunnen een specifieke protocol te gebruiken:

| Protocol | Poort |
| --- | --- |
| MQTT |8883 |
| MQTT via WebSockets |443 |
| AMQP |5671 |
| AMQP via WebSockets |443 |
| HTTPS |443 |

Als u een IoT-hub hebt gemaakt in een Azure-regio, blijft de IoT-hub hetzelfde IP-adres voor de levensduur van deze IoT-hub. Echter, als Microsoft de IoT-hub naar een andere schaaleenheid te handhaven van de kwaliteit van de service wordt verplaatst, wordt deze toegewezen een nieuw IP-adres.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hoe IoT Hub het MQTT-protocol implementeert, [communiceren met uw IoT-hub met behulp van het MQTT-protocol](iot-hub-mqtt-support.md).
