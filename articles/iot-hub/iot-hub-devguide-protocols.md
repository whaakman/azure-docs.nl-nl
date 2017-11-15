---
title: Azure IoT Hub communicatieprotocollen en poorten | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijft de ondersteunde communicatieprotocollen voor apparaat-naar-cloud-en cloud-naar-apparaat en de poortnummers die geopend zijn moeten.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3fc5f1a3-3711-4611-9897-d4db079b4250
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/31/2017
ms.author: dobett
ms.openlocfilehash: 37602bf78f7a43fb8255ddc0aad21f24095cb43c
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="reference---choose-a-communication-protocol"></a>Verwijzen naar - een communicatieprotocol kiezen

IoT-Hub kunt u apparaten met de volgende protocollen voor apparaat-side '-communicatie:

* [MQTT][lnk-mqtt]
* MQTT via WebSockets
* [AMQP][lnk-amqp]
* AMQP via WebSockets
* HTTPS

Zie voor meer informatie over hoe deze protocollen specifieke IoT-Hub-functies ondersteunen [apparaat-naar-cloud communicatie richtlijnen] [ lnk-d2c-guidance] en [Cloud-naar-apparaat communicatie richtlijnen] [lnk-c2d-guidance].

De volgende tabel bevat de op hoog niveau aanbevelingen voor uw keuze van het protocol:

| Protocol | Wanneer u dit protocol moet kiezen |
| --- | --- |
| MQTT <br> MQTT via WebSocket |Gebruik op alle apparaten die niet hoeven via dezelfde TLS-verbinding verbinding te maken van meerdere apparaten (elk met zijn eigen referenties per apparaat). |
| AMQP <br> AMQP via WebSocket |Op veld en cloud-gateways gebruiken om te profiteren van de verbinding multiplex verschillende apparaten. |
| HTTPS |Gebruik dit voor apparaten die niet kan, andere protocollen ondersteunen. |

Houd rekening met de volgende punten wanneer u ervoor uw protocol voor het apparaat aan clientzijde communicatie kiest:

* **Cloud-naar-apparaat patroon**. HTTPS heeft geen een efficiënte manier voor het implementeren van server-push. Als zodanig wanneer u HTTPS gebruikt, pollen apparaten IoT Hub voor cloud-naar-apparaat-berichten. Deze aanpak is inefficiënt voor het apparaat en de IoT-Hub. Onder de huidige richtlijnen voor HTTPS, moet elk apparaat voor berichten pollen, elke 25 minuten of langer. MQTT en AMQP ondersteuning voor server push tijdens het ontvangen van berichten van de cloud-naar-apparaat. Ze inschakelen direct pushes van berichten uit IoT Hub naar het apparaat. Als levering latentie is een probleem, zijn MQTT of AMQP de aanbevolen protocollen gebruiken. Voor zelden verbonden apparaten werkt HTTPS als goed.
* **Veld gateways**. Wanneer u MQTT en HTTPS, u meerdere apparaten (elk met zijn eigen referenties per apparaat) kan geen verbinding kunt maken met behulp van dezelfde TLS-verbinding. Voor [veld gatewayscenario's] [ lnk-azure-gateway-guidance] waarvoor een TLS-verbinding tussen de veldgateway en IoT Hub voor elk aangesloten apparaat vereist, zijn deze protocollen suboptimale.
* **Resource apparaten geringe**. De protocollen MQTT- en HTTPS-bibliotheken hebben een kleiner oppervlak dan het AMQP-bibliotheken. Als zodanig als het apparaat resources (voor bijvoorbeeld minder dan 1 MB RAM) beperkt heeft, zijn deze protocollen de enige protocol-implementatie.
* **{De passage netwerk**. Het standaard AMQP-protocol gebruikt poort 5671 en MQTT luistert op poort 8883. Gebruik van deze poorten kan problemen veroorzaken in netwerken die niet-HTTPS-protocollen worden gesloten. Gebruik MQTT via WebSockets, AMQP via WebSockets of HTTPS in dit scenario.
* **Pakketgrootte**. MQTT en AMQP zijn binaire protocollen, waardoor het meer compacte nettoladingen dan HTTPS.

> [!WARNING]
> Wanneer u HTTPS gebruikt, moet elk apparaat voor cloud-naar-apparaat-berichten pollen elke 25 minuten of langer. Tijdens de ontwikkeling is het echter aanvaardbaar is voor het pollen vaker dan elke 25 minuten.

## <a name="port-numbers"></a>Poortnummers

Apparaten kunnen communiceren met IoT Hub in Azure met behulp van verschillende protocollen. Normaal gesproken wordt de keuze van het protocol bepaald door de specifieke vereisten van de oplossing. De volgende tabel bevat de uitgaande poorten die moeten zijn geopend voor een apparaat om te kunnen gebruiken van een specifiek protocol:

| Protocol | Poort |
| --- | --- |
| MQTT |8883 |
| MQTT via WebSockets |443 |
| AMQP |5671 |
| AMQP via WebSockets |443 |
| HTTPS |443 |

Nadat u een IoT-hub hebt gemaakt in een Azure-regio, houdt de IoT-hub hetzelfde IP-adres voor de levensduur van iothub. Echter, als de IoT-hub Microsoft naar een andere schaaleenheid quality of service-onderhouden verplaatst, wordt het toegewezen een nieuw IP-adres.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over hoe IoT Hub het MQTT-protocol implementeert, [communiceren met uw iothub met het protocol MQTT][lnk-mqtt-support].

[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-mqtt-support]: iot-hub-mqtt-support.md
[lnk-amqp]: http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf
[lnk-mqtt]: http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.pdf
[lnk-azure-gateway-guidance]: iot-hub-devguide-endpoints.md#field-gateways
