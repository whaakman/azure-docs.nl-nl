---
title: Informatie over de ingebouwde Azure IoT Hub-eindpunt | Microsoft Docs
description: Handleiding voor ontwikkelaars - wordt beschreven hoe u van de ingebouwde, Event Hub-compatibele eindpunt om apparaat-naar-cloud-berichten te lezen.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/26/2019
ms.openlocfilehash: 52f1316b8167d2e1c3e37dbbfc0059b68e832172
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57538558"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Apparaat-naar-cloud-berichten lezen van het geïntegreerde eindpunt

Standaard berichten worden doorgestuurd naar het eindpunt van de ingebouwde service gerichte (**berichten/gebeurtenissen**) die compatibel is met [Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Dit eindpunt is momenteel alleen beschikbaar gemaakte met behulp van de [AMQP](https://www.amqp.org/) -protocol op poort 5671. Een IoT-hub toont de volgende eigenschappen om te bepalen van het ingebouwde eindpunt van de Event Hub-compatibele-messaging **berichten/gebeurtenissen**.

| Eigenschap            | Description |
| ------------------- | ----------- |
| **Aantal partities** | Deze eigenschap instellen tijdens het maken van voor het definiëren van het aantal [partities](../event-hubs/event-hubs-features.md#partitions) voor gebeurtenisopname van apparaat-naar-cloud. |
| **Bewaartijd**  | Deze eigenschap geeft aan hoe lang in dagen berichten door de IoT Hub worden bewaard. De standaardwaarde is één dag, maar deze kan worden verhoogd tot zeven dagen. |

IoT Hub kunt het bewaren van gegevens in de ingebouwde Event Hubs maximaal 7 dagen. U kunt de bewaartijd tijdens het maken van uw IoT-Hub kunt instellen. Grootte van de gegevens bewaren in IoT Hub, is afhankelijk van uw IoT hub tier en eenheidstype. Wat betreft grootte, kan de ingebouwde Event Hubs-berichten van de maximale berichtgrootte tot ten minste 24 uur van quota bewaren. Bijvoorbeeld: voor IoT Hub voldoende opslagruimte biedt voor het behouden van ten minste 1 S1-eenheid 400K berichten van 4k het formaat van elk. Als uw apparaten kleiner berichten verzendt zijn, kunnen ze worden bewaard voor langer (tot 7 dagen), afhankelijk van hoeveel opslag wordt gebruikt. We garanderen dat die de gegevens bewaren gedurende het opgegeven bewaartijd als een minimum.

IoT Hub ook kunt u voor het beheren van consumentengroepen van de ingebouwde apparaat-naar-cloud eindpunt ontvangen.

Als u [berichtroutering](iot-hub-devguide-messages-d2c.md) en de [alternatieve route](iot-hub-devguide-messages-d2c.md#fallback-route) is ingeschakeld, alle berichten die niet overeenkomen met een query op een route gaat u naar het geïntegreerde eindpunt. Als u deze alternatieve route uitschakelt, worden de berichten die niet overeenkomen met elke query verwijderd.

U kunt de bewaartijd wijzigen via een programma met behulp van de [IoT-Hub resourceprovider REST-API's](/rest/api/iothub/iothubresource), of met de [Azure-portal](https://portal.azure.com).

IoT-Hub toont de **berichten/gebeurtenissen** ingebouwd eindpunt voor uw back-end-services met de apparaat-naar-cloud-berichten ontvangen via uw hub lezen. Dit eindpunt is Event Hub-compatibele, waarmee u een van de mechanismen voor de Event Hubs-service biedt ondersteuning voor het lezen van berichten.

## <a name="read-from-the-built-in-endpoint"></a>Lezen van het ingebouwde eindpunt

Sommige productintegraties en Event Hubs-SDK's zijn op de hoogte van IoT-Hub en kunt u de verbindingsreeks van uw IoT hub-service gebruiken voor verbinding met het ingebouwde eindpunt.

Wanneer u Event Hubs-SDK's of productintegraties gebruikt die zich niet bewust van IoT Hub gebruikt, moet u een Event Hub-compatibele eindpunt en een Event Hub-compatibele naam. U kunt deze waarden ophalen uit de portal als volgt:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw IoT-hub.

2. Klik op **ingebouwde eindpunten**.

3. De **gebeurtenissen** sectie bevat de volgende waarden: **Partities**, **Event Hub-compatibele naam**, **Event Hub-compatibele eindpunt**, **bewaartijd**, en **consumentengroepen**.

    ![Apparaat-naar-cloudinstellingen](./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png)

In de portal bevat het veld van Event Hub-compatibele eindpunt een volledige Event Hubs-verbindingsreeks dat lijkt op: **Endpoint=sb://abcd1234namespace.servicebus.windows.net/;SharedAccessKeyName=iothubowner;SharedAccessKey=keykeykeykeykeykey=;EntityPath=iothub-ehub-abcd-1234-123456**. Als de SDK u andere waarden vereist, zouden ze zijn:

| Name | Value |
| ---- | ----- |
| Eindpunt | sb://abcd1234namespace.servicebus.windows.net/ |
| Hostnaam | abcd1234namespace.servicebus.windows.net |
| Naamruimte | abcd1234namespace |

Vervolgens kunt u een beleid voor gedeelde toegang waarvoor de **ServiceConnect** machtigingen voor het verbinding maken met de opgegeven Event Hub.

De SDK's die kunt u verbinding maken met de ingebouwde Event Hub-compatibele eindpunt die IoT-Hub toont zijn onder andere:

| Taal | SDK | Voorbeeld | Opmerkingen |
| -------- | --- | ------ | ----- |
| .NET | https://github.com/Azure/azure-event-hubs-dotnet | [Snelstartgids](quickstart-send-telemetry-dotnet.md) | Event Hubs-compatibele gegevens worden gebruikt |
 Java | https://github.com/Azure/azure-event-hubs-java | [Snelstartgids](quickstart-send-telemetry-java.md) | Event Hubs-compatibele gegevens worden gebruikt |
| Node.js | https://github.com/Azure/azure-event-hubs-node | [Snelstartgids](quickstart-send-telemetry-node.md) | Maakt gebruik van IoT Hub-verbindingsreeks |
| Python | https://github.com/Azure/azure-event-hubs-python | https://github.com/Azure/azure-event-hubs-python/blob/master/examples/iothub_recv.py | Maakt gebruik van IoT Hub-verbindingsreeks |

De productintegraties die kunt u met de ingebouwde Event Hub-compatibele eindpunt dat IoT-Hub toont zijn onder andere:

* [Azure Functions](https://docs.microsoft.com/azure/azure-functions/). Zie [verwerken van gegevens uit IoT Hub met Azure Functions](https://azure.microsoft.com/resources/samples/functions-js-iot-hub-processing/).
* [Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/). Zie [gegevens als invoer in Stream Analytics Stream](../stream-analytics/stream-analytics-define-inputs.md#stream-data-from-iot-hub).
* [Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/). Zie [een IoT hub-gebeurtenisbron toevoegen aan uw Time Series Insights-omgeving](../time-series-insights/time-series-insights-how-to-add-an-event-source-iothub.md).
* [Apache Storm spout](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). U vindt de [spout bron](https://github.com/apache/storm/tree/master/external/storm-eventhubs) op GitHub.
* [Integratie van Apache Spark](../hdinsight/spark/apache-spark-eventhub-streaming.md).
* [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over IoT Hub-eindpunten, [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md).

* De [snelstartgidsen](quickstart-send-telemetry-node.md) ziet u hoe u apparaat-naar-cloud-berichten worden verzonden vanuit gesimuleerde apparaten en de berichten lezen van het ingebouwde eindpunt. 

Zie voor meer informatie de [Process IoT Hub apparaat-naar-cloud-berichten met behulp van routes](tutorial-routing.md) zelfstudie.

* Als u wilt voor het routeren van uw apparaat-naar-cloud-berichten met aangepaste eindpunten, Zie [berichtroutes en aangepaste eindpunten gebruiken voor apparaat-naar-cloud-berichten](iot-hub-devguide-messages-read-custom.md).
