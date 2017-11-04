---
title: Inzicht in de Azure IoT Hub ingebouwd eindpunt | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijft het gebruik van de ingebouwde Event Hub-compatibele eindpunt toread apparaat-naar-cloud-berichten.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: dobett
ms.openlocfilehash: c9e6aa03e3a1e0592223630c7b81634bcb09add6
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Apparaat-naar-cloud-berichten lezen van het ingebouwde eindpunt

Standaard berichten worden doorgestuurd naar het ingebouwde gerichte service-eindpunt (**berichten/gebeurtenissen**) die compatibel is met [Event Hubs][lnk-event-hubs]. Dit eindpunt is momenteel alleen zichtbare met behulp van de [AMQP] [ lnk-amqp] -protocol op poort 5671. Een IoT-hub toont de volgende eigenschappen om te bepalen van de ingebouwde Event Hub-compatibele messaging eindpunt **berichten/gebeurtenissen**.

| Eigenschap            | Beschrijving |
| ------------------- | ----------- |
| **Aantal partities** | Stel deze eigenschap bij het maken van het aantal definiëren [partities] [ lnk-event-hub-partitions] voor opname van apparaat-naar-cloud-gebeurtenis. |
| **Bewaartijd**  | Deze eigenschap specificeert hoe lang in dagen dat berichten door de IoT Hub worden bewaard. De standaardwaarde is één dag, maar deze kan worden verhoogd tot zeven dagen. |

IoT-Hub kunt u beheren consumer-groepen op de ingebouwde apparaat-naar-cloud eindpunt ontvangen.

Standaard worden alle berichten die niet expliciet overeen met een regel voor het doorsturen van een bericht geschreven naar het eindpunt van de ingebouwde. Als u deze route voor terugval uitschakelt, worden berichten die niet expliciet overeen met alle regels voor het doorsturen van bericht verwijderd.

U kunt ofwel de bewaartijd wijzigen programmatisch via de [resourceprovider IoT Hub REST-API's][lnk-resource-provider-apis], of met behulp van de [Azure-portal][lnk-management-portal].

IoT-Hub toont de **berichten/gebeurtenissen** ingebouwd eindpunt voor uw back-end-services om te lezen van de apparaat-naar-cloud-berichten dat is ontvangen door de hub. Dit eindpunt is Event Hub-compatibele, waarmee u een van de mechanismen gebruiken de Event Hubs-service ondersteunt voor het lezen van berichten.

## <a name="read-from-the-built-in-endpoint"></a>Lezen van het ingebouwde eindpunt

Wanneer u gebruikt de [Azure Service Bus-SDK voor .NET] [ lnk-servicebus-sdk] of de [Event Hubs - Gebeurtenisprocessorhost][lnk-eventprocessorhost], kunt u eventuele verbindingstekenreeksen IoT Hub met de juiste machtigingen. Gebruik vervolgens **berichten/gebeurtenissen** als de naam van de Event Hub.

Wanneer u SDK's (of product integraties) gebruikt die zich niet bewust zijn van IoT Hub, moet u een Event Hub-compatibele eindpunt en een Event Hub-compatibele naam ophalen van uw IoT hub-instellingen:

1. Aanmelden bij de [Azure-portal] [ lnk-management-portal] en navigeer naar uw IoT-hub.
1. Klik op **Eindpunten**.
1. In de **ingebouwde eindpunten** sectie, klikt u op **gebeurtenissen**. 
1. Een pagina met eigenschappen wordt geopend, waarin de volgende waarden: **Event Hub-compatibele eindpunt**, **Event Hub-compatibele naam**, **partities**,  **Bewaartijd**, en **consumergroepen**.

    ![Apparaat-naar-cloud-instellingen][img-eventhubcompatible]

De naam voor het eindpunt van IoT Hub, die is vereist dat de IoT Hub SDK **berichten/gebeurtenissen** zoals wordt weergegeven onder **eindpunten**.

Als de SDK die u moet een **hostnaam** of **Namespace** waarde, verwijdert u het schema van de **Event Hub-compatibele eindpunt**. Bijvoorbeeld, als uw Event Hub-compatibele eindpunt **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, wordt de **hostnaam** zou worden  **iothub ns-myiothub 1234.servicebus.windows.net**. De **Namespace** zou **iothub-ns-myiothub-1234**.

U kunt een beleid voor gedeelde toegang met de **ServiceConnect** machtigingen verbinding maken met de opgegeven Event Hub.

Als u een Event Hub-verbindingsreeks bouwen wilt met behulp van de vorige gegevens, gebruikt u het volgende patroon volgen:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

De SDK's en integraties gebruikt die u kunt gebruiken met Event Hub-compatibele eindpunten die IoT-Hub toont bevat de items in de volgende lijst:

* [Java Event Hubs-client](https://github.com/Azure/azure-event-hubs-java).
* [Apache Storm spout](../hdinsight/storm/apache-storm-develop-csharp-event-hub-topology.md). U vindt de [spout bron](https://github.com/apache/storm/tree/master/external/storm-eventhubs) op GitHub.
* [Apache Spark-integratie](../hdinsight/spark/apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over IoT-hubeindpunten [IoT-hubeindpunten][lnk-endpoints].

De [aan de slag] [ lnk-get-started] zelfstudies ziet u hoe u apparaat-naar-cloud-berichten verzenden van de gesimuleerde apparaten en de berichten lezen van het ingebouwde eindpunt. Zie voor meer details over de [proces IoT Hub apparaat-naar-cloud-berichten met behulp van routes] [ lnk-d2c-tutorial] zelfstudie.

Als u wilt voor het routeren van uw apparaat-naar-cloud-berichten met aangepaste eindpunten, Zie [berichtroutes en aangepaste eindpunten gebruikt voor apparaat-naar-cloud-berichten][lnk-custom].

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx
[lnk-amqp]: https://www.amqp.org/
