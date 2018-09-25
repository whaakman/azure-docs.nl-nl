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
ms.openlocfilehash: 02624b4f3b0fceb1816f4f43b1f435356f8d5235
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46984038"
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Apparaat-naar-cloud-berichten lezen van het geïntegreerde eindpunt

Standaard berichten worden doorgestuurd naar het eindpunt van de ingebouwde service gerichte (**berichten/gebeurtenissen**) die compatibel is met [Event Hubs][lnk-event-hubs]. Dit eindpunt is momenteel alleen beschikbaar gemaakte met behulp van de [AMQP] [ lnk-amqp] -protocol op poort 5671. Een IoT-hub toont de volgende eigenschappen om te bepalen van het ingebouwde eindpunt van de Event Hub-compatibele-messaging **berichten/gebeurtenissen**.

| Eigenschap            | Beschrijving |
| ------------------- | ----------- |
| **Aantal partities** | Deze eigenschap instellen tijdens het maken van voor het definiëren van het aantal [partities] [ lnk-event-hub-partitions] voor gebeurtenisopname van apparaat-naar-cloud. |
| **Bewaartijd**  | Deze eigenschap geeft aan hoe lang in dagen berichten door de IoT Hub worden bewaard. De standaardwaarde is één dag, maar deze kan worden verhoogd tot zeven dagen. |

IoT Hub ook kunt u voor het beheren van consumentengroepen van de ingebouwde apparaat-naar-cloud eindpunt ontvangen.

Als u [berichtroutering](iot-hub-devguide-messages-d2c.md) en de [alternatieve route](iot-hub-devguide-messages-d2c.md#fallback-route) is ingeschakeld, worden alle berichten die niet overeenkomen met een query op een route geschreven naar het ingebouwde eindpunt. Als u deze alternatieve route uitschakelt, worden de berichten die niet overeenkomen met elke query verwijderd.

U kunt de bewaartijd wijzigen via een programma met behulp van de [IoT-Hub resourceprovider REST-API's][lnk-resource-provider-apis], of met de [Azure-portal] [ lnk-management-portal].

IoT-Hub toont de **berichten/gebeurtenissen** ingebouwd eindpunt voor uw back-end-services met de apparaat-naar-cloud-berichten ontvangen via uw hub lezen. Dit eindpunt is Event Hub-compatibele, waarmee u een van de mechanismen voor de Event Hubs-service biedt ondersteuning voor het lezen van berichten.

## <a name="read-from-the-built-in-endpoint"></a>Lezen van het ingebouwde eindpunt

Wanneer u gebruikt de [Azure Service Bus-SDK voor .NET] [ lnk-servicebus-sdk] of de [Event Hubs - Event Processor Host][lnk-eventprocessorhost], kunt u een IoT Hub-verbinding tekenreeksen met de juiste machtigingen. Gebruik vervolgens **berichten/gebeurtenissen** als de naam van de Event Hub.

Wanneer u SDK's (of productintegraties) gebruikt die zich niet bewust van IoT Hub, moet u een Event Hub-compatibele eindpunt en een Event Hub-compatibele naam ophalen:

1. Aanmelden bij de [Azure-portal] [ lnk-management-portal] en navigeer naar uw IoT-hub.
1. Klik op **ingebouwde eindpunten**.
1. De **gebeurtenissen** sectie bevat de volgende waarden: **Event Hub-compatibele eindpunt**, **Event Hub-compatibele naam**, **partities**, **Bewaartijd**, en **consumentengroepen**.

    ![Apparaat-naar-cloudinstellingen][img-eventhubcompatible]

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

* Zie voor meer informatie over IoT Hub-eindpunten, [IoT Hub-eindpunten][lnk-endpoints].
* De [snelstartgidsen] [ lnk-get-started] ziet u hoe u apparaat-naar-cloud-berichten worden verzonden vanuit gesimuleerde apparaten en de berichten lezen van het ingebouwde eindpunt. Zie voor meer informatie de [Process IoT Hub apparaat-naar-cloud-berichten met behulp van routes] [ lnk-d2c-tutorial] zelfstudie.
* Als u wilt voor het routeren van uw apparaat-naar-cloud-berichten met aangepaste eindpunten, Zie [berichtroutes en aangepaste eindpunten gebruiken voor apparaat-naar-cloud-berichten][lnk-custom].

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: quickstart-send-telemetry-node.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: tutorial-routing.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph
[lnk-amqp]: https://www.amqp.org/
