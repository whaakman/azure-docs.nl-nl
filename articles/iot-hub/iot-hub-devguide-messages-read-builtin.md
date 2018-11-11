---
title: Informatie over de ingebouwde Azure IoT Hub-eindpunt | Microsoft Docs
description: Handleiding voor ontwikkelaars - wordt beschreven hoe u van de ingebouwde, Event Hub-compatibele eindpunt om apparaat-naar-cloud-berichten te lezen.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.author: dobett
ms.openlocfilehash: 02ea4b94f8d1442360bebb36fdbba13d973f8555
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51242401"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Apparaat-naar-cloud-berichten lezen van het geïntegreerde eindpunt

Standaard berichten worden doorgestuurd naar het eindpunt van de ingebouwde service gerichte (**berichten/gebeurtenissen**) die compatibel is met [Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/
). Dit eindpunt is momenteel alleen beschikbaar gemaakte met behulp van de [AMQP](https://www.amqp.org/) -protocol op poort 5671. Een IoT-hub toont de volgende eigenschappen om te bepalen van het ingebouwde eindpunt van de Event Hub-compatibele-messaging **berichten/gebeurtenissen**.

| Eigenschap            | Beschrijving |
| ------------------- | ----------- |
| **Aantal partities** | Deze eigenschap instellen tijdens het maken van voor het definiëren van het aantal [partities](../event-hubs/event-hubs-features.md#partitions) voor gebeurtenisopname van apparaat-naar-cloud. |
| **Bewaartijd**  | Deze eigenschap geeft aan hoe lang in dagen berichten door de IoT Hub worden bewaard. De standaardwaarde is één dag, maar deze kan worden verhoogd tot zeven dagen. |

IoT Hub ook kunt u voor het beheren van consumentengroepen van de ingebouwde apparaat-naar-cloud eindpunt ontvangen.

Als u [berichtroutering](iot-hub-devguide-messages-d2c.md) en de [alternatieve route](iot-hub-devguide-messages-d2c.md#fallback-route) is ingeschakeld, worden alle berichten die niet overeenkomen met een query op een route geschreven naar het ingebouwde eindpunt. Als u deze alternatieve route uitschakelt, worden de berichten die niet overeenkomen met elke query verwijderd.

U kunt de bewaartijd wijzigen via een programma met behulp van de [IoT-Hub resourceprovider REST-API's](/rest/api/iothub/iothubresource), of met de [Azure-portal](https://portal.azure.com).

IoT-Hub toont de **berichten/gebeurtenissen** ingebouwd eindpunt voor uw back-end-services met de apparaat-naar-cloud-berichten ontvangen via uw hub lezen. Dit eindpunt is Event Hub-compatibele, waarmee u een van de mechanismen voor de Event Hubs-service biedt ondersteuning voor het lezen van berichten.

## <a name="read-from-the-built-in-endpoint"></a>Lezen van het ingebouwde eindpunt

Wanneer u gebruikt de [Azure Service Bus-SDK voor .NET](https://www.nuget.org/packages/WindowsAzure.ServiceBus) of de [Event Hubs - Event Processor Host](..//event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md), kunt u een IoT Hub-verbindingsreeksen met de juiste machtigingen. Gebruik vervolgens **berichten/gebeurtenissen** als de naam van de Event Hub.

Wanneer u SDK's (of productintegraties) gebruikt die zich niet bewust van IoT Hub, moet u een Event Hub-compatibele eindpunt en een Event Hub-compatibele naam ophalen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw IoT-hub.

2. Klik op **ingebouwde eindpunten**.

3. De **gebeurtenissen** sectie bevat de volgende waarden: **Event Hub-compatibele eindpunt**, **Event Hub-compatibele naam**, **partities**, **Bewaartijd**, en **consumentengroepen**.

    ![Apparaat-naar-cloudinstellingen](./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png)

De SDK voor IoT Hub is de naam voor het eindpunt van IoT Hub, die is vereist **berichten/gebeurtenissen** zoals wordt weergegeven onder **eindpunten**.

Als de SDK die u moet een **hostnaam** of **Namespace** waarde, verwijdert u het schema van de **Event Hub-compatibele eindpunt**. Bijvoorbeeld, als uw Event Hub-compatibele eindpunt **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, wordt de **hostnaam** zou  **iothub-ns-myiothub-1234.servicebus.windows.net**. De **Namespace** zou **iothub-ns-myiothub-1234**.

Vervolgens kunt u een beleid voor gedeelde toegang waarvoor de **ServiceConnect** machtigingen voor het verbinding maken met de opgegeven Event Hub.

Als u moet een Event Hub-verbindingsreeks kunt maken met behulp van de vorige gegevens, gebruikt u het volgende patroon:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

De SDK's en integraties gebruikt die u kunt gebruiken met Event Hub-compatibele eindpunten die IoT-Hub toont bevat de items in de volgende lijst:

* [Java Event Hubs-client](https://github.com/Azure/azure-event-hubs-java).
* [Apache Storm spout](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). U vindt de [spout bron](https://github.com/apache/storm/tree/master/external/storm-eventhubs) op GitHub.
* [Integratie van Apache Spark](../hdinsight/spark/apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over IoT Hub-eindpunten, [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md).

* De [snelstartgidsen](quickstart-send-telemetry-node.md) ziet u hoe u apparaat-naar-cloud-berichten worden verzonden vanuit gesimuleerde apparaten en de berichten lezen van het ingebouwde eindpunt. 

Zie voor meer informatie de [Process IoT Hub apparaat-naar-cloud-berichten met behulp van routes](tutorial-routing.md) zelfstudie.

* Als u wilt voor het routeren van uw apparaat-naar-cloud-berichten met aangepaste eindpunten, Zie [berichtroutes en aangepaste eindpunten gebruiken voor apparaat-naar-cloud-berichten](iot-hub-devguide-messages-read-custom.md).