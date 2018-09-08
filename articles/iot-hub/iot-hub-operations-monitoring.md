---
title: Azure IoT Hub-bewerkingen controleren | Microsoft Docs
description: Het gebruik van Azure IoT Hub-bewerkingen controleren voor het controleren van de status van bewerkingen op uw IoT-hub in realtime.
author: nberdy
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.author: nberdy
ms.openlocfilehash: 3aa452cd178bd0d064726c5be7dbdf65c6ef8d92
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44160045"
---
# <a name="iot-hub-operations-monitoring"></a>IoT Hub-bewerkingen controleren

IoT Hub-bewerkingen controleren kunt u de status van de bewerkingen op uw IoT-hub in realtime controleren. IoT Hub worden gebeurtenissen bijgehouden in meerdere categorieën met bewerkingen. U kunt kiezen voor het verzenden van gebeurtenissen uit een of meer categorieën naar een eindpunt van uw IoT-hub voor verwerking. U kunt de gegevens op fouten controleren of instellen van meer complexe verwerking op basis van, gegevenspatronen.

>[!NOTE]
>IoT Hub-bewerkingen controleren is afgeschaft en wordt verwijderd uit IoT Hub op 10 maart 2019. Zie voor het bewaken van de operations- en status van IoT Hub, [de status van Azure IoT Hub bewaken en problemen vast te stellen][lnk-monitor]. Zie voor meer informatie over de tijdlijn afschaffing [bewaken van uw Azure-IoT-oplossingen met Azure Monitor en Azure Resource Health][lnk-blog-announcement].

IoT Hub bewaakt zes categorieën van gebeurtenissen:

* Bewerkingen voor apparaat-id
* Telemetrie van apparaten
* Cloud-naar-apparaat-berichten
* Verbindingen
* Uploaden van bestanden
* Berichtroutering

> [!IMPORTANT]
> IoT Hub-bewerkingen controleren is geen garantie voor lijsten met en betrouwbare bezorging van gebeurtenissen. Afhankelijk van de onderliggende infrastructuur IoT-Hub, kunnen sommige gebeurtenissen worden verloren of die niet de juiste volgorde worden geleverd. Bewerkingen controleren voor het genereren van waarschuwingen op basis van de fout signalen, zoals mislukte verbindingspogingen of hoge frequentie verbroken verbindingen voor specifieke apparaten gebruiken. U niet afhankelijk zijn van bewerkingen controleren van gebeurtenissen voor het maken van een consistente archief voor de status van het apparaat, bijvoorbeeld een archief bijhouden verbonden of de status van een apparaat verbroken. 

## <a name="how-to-enable-operations-monitoring"></a>Bewerkingen controleren inschakelen

1. Een IoT-hub maken. U vindt instructies over het maken van een IoT-hub in de [aan de slag] [ lnk-get-started] handleiding.

1. Open de blade van uw IoT-hub. Van daaruit, klikt u op **bewerkingen controleren**.

    ![Bewerkingen voor gegevenstoegang configuratie in de portal bewaken][1]

1. Selecteer de bewaking categorieën die u wilt controleren, en klik vervolgens op **opslaan**. De gebeurtenissen zijn beschikbaar voor het lezen van de Event Hub-compatibele eindpunt die worden vermeld in **controle-instellingen**. De IoT Hub-eindpunt aangeroepen `messages/operationsmonitoringevents`.

    ![Bewerkingen controleren op uw IoT-hub configureren][2]

> [!NOTE]
> Selecteren **uitgebreid** bewaking voor de **verbindingen** categorie zorgt ervoor dat de IoT Hub kunt u extra diagnostische berichten genereren. Voor alle andere categorieën, de **uitgebreid** in elke foutbericht instellen van de hoeveelheid gegevens IoT Hub bevat.

## <a name="event-categories-and-how-to-use-them"></a>Gebeurteniscategorieën en hoe ze te gebruiken

Elke bewerkingen controleren categorie houdt een ander type interactie met IoT Hub, en elke controle categorie bevat een schema dat definieert hoe gebeurtenissen in die categorie worden opgebouwd.

### <a name="device-identity-operations"></a>Bewerkingen voor apparaat-id

De apparaatcategorie die identiteit operations houdt bij of fouten die optreden wanneer u probeert te maken, bijwerken of verwijderen van een vermelding in het identiteitenregister van uw IoT-hub. Het bijhouden van deze categorie is handig voor het inrichten van scenario's.

```json
{
    "time": "UTC timestamp",
    "operationName": "create",
    "category": "DeviceIdentityOperations",
    "level": "Error",
    "statusCode": 4XX,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "durationMs": 1234,
    "userAgent": "userAgent",
    "sharedAccessPolicy": "accessPolicy"
}
```

### <a name="device-telemetry"></a>Telemetrie van apparaten

De apparaatcategorie telemetrie houdt bij of fouten die optreden bij de IoT-hub en zijn gerelateerd aan de pijplijn telemetrie. Deze categorie bevat fouten die optreden bij het verzenden van telemetriegebeurtenissen (zoals beperking) en telemetriegebeurtenissen (zoals niet-geautoriseerde lezer) ontvangen. Deze categorie worden geen fouten veroorzaakt door de code die wordt uitgevoerd op het apparaat zelf gedetecteerd.

```json
{
    "messageSizeInBytes": 1234,
    "batching": 0,
    "protocol": "Amqp",
    "authType": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "DeviceTelemetry",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="cloud-to-device-commands"></a>Cloud-naar-apparaatopdrachten

De categorie cloud-naar-apparaatopdrachten houdt bij of fouten die optreden bij de IoT-hub en zijn gerelateerd aan de pijplijn cloud-naar-apparaat bericht. Deze categorie bevat fouten die optreden bij het verzenden van berichten van cloud-naar-apparaat (zoals niet-geautoriseerde afzender), het ontvangen van berichten van cloud-naar-apparaat (zoals aantal bezorgingen is overschreden) en het ontvangen van feedback van cloud-naar-apparaat bericht (zoals feedback verlopen). Deze categorie geen fouten van een apparaat dat een cloud-naar-apparaat-bericht niet goed verwerkt als de cloud-naar-apparaat-bericht met succes is afgeleverd af.

```json
{
    "messageSizeInBytes": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "deliveryAcknowledgement": 0,
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "C2DCommands",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "EventProcessedUtcTime": "UTC timestamp",
    "PartitionId": 1,
    "EventEnqueuedUtcTime": "UTC timestamp"
}
```

### <a name="connections"></a>Verbindingen

De categorie verbindingen houdt bij of fouten die optreden wanneer apparaten koppelen aan of van een IoT-hub loskoppelen. Het bijhouden van deze categorie is handig voor het identificeren van niet-geautoriseerde verbindingspogingen en voor het bijhouden van een verbinding is verbroken voor apparaten in de gebieden van slechte connectiviteit.

```json
{
    "durationMs": 1234,
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "Amqp",
    "time": " UTC timestamp",
    "operationName": "deviceConnect",
    "category": "Connections",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID"
}
```

### <a name="file-uploads"></a>Uploaden van bestanden

De categorie van bestand uploaden bijgehouden fouten die optreden bij de IoT-hub en zijn gerelateerd aan functionaliteit voor het uploaden van bestand. Deze categorie omvat:

* Fouten die met de SAS-URI optreden, zoals wanneer het verloopt voordat een apparaat aan de hub van het uploaden van een voltooide gecommuniceerd.
* Kan geen uploads gemeld door het apparaat.
* Fouten die optreden wanneer een bestand niet in de opslag tijdens het maken van IoT-Hub melding weergegeven gevonden is.

Deze categorie worden geen fouten die optreden wanneer het apparaat een bestand naar storage uploaden is gedetecteerd.

```json
{
    "authType": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\"}",
    "protocol": "HTTP",
    "time": " UTC timestamp",
    "operationName": "ingress",
    "category": "fileUpload",
    "level": "Error",
    "statusCode": 4XX,
    "statusType": 4XX001,
    "statusDescription": "MessageDescription",
    "deviceId": "device-ID",
    "blobUri": "http//bloburi.com",
    "durationMs": 1234
}
```

### <a name="message-routing"></a>Berichtroutering

De categorie voor het doorsturen van bericht houdt bij of fouten die optreden tijdens de evaluatie van bericht route en eindpunt status van de waargenomen door de IoT Hub. Deze categorie omvat gebeurtenissen zoals wanneer een regel resulteert in 'niet-gedefinieerde', wanneer IoT-Hub markeert een eindpunt als onbestelbaar en eventuele andere fouten die zijn ontvangen van een eindpunt. Deze categorie bevat geen specifieke fouten over berichten zelf (zoals apparaat beperkingsfouten), die worden gerapporteerd in de categorie 'apparaattelemetrie'.

```json
{
    "messageSizeInBytes": 1234,
    "time": "UTC timestamp",
    "operationName": "ingress",
    "category": "routes",
    "level": "Error",
    "deviceId": "device-ID",
    "messageId": "ID of message",
    "routeName": "myroute",
    "endpointName": "myendpoint",
    "details": "ExternalEndpointDisabled"
}
```

## <a name="connect-to-the-monitoring-endpoint"></a>Verbinding maken met de controle-eindpunt

De controle-eindpunt op uw IoT-hub is een Event Hub-compatibele eindpunt. U kunt een mechanisme dat werkt met Event Hubs controleren om berichten te lezen vanaf dit eindpunt. Het volgende voorbeeld maakt u een basislezer die niet geschikt voor een implementatie met hoge doorvoer. Zie voor meer informatie over het verwerken van Event Hubs-berichten de zelfstudie [Aan de slag met Event Hubs][lnk-eventhubs-tutorial].

Voor verbinding met de controle-eindpunt, moet u een verbindingsreeks en de naam van het eindpunt. De volgende stappen laten zien hoe u de vereiste waarden vinden in de portal:

1. Navigeer naar de resourceblade van uw IoT-Hub in de portal.

1. Kies **bewerkingen controleren**, en noteer de **Event Hub-compatibele naam** en **Event Hub-compatibele eindpunt** waarden:

    ![Event Hub-compatibele eindpunt waarden][img-endpoints]

1. Kies **beleid voor gedeelde toegang**, en kies vervolgens **service**. Noteer de **primaire sleutel** waarde:

    ![Primaire sleutel voor service gedeelde toegang beleid][img-service-key]

De volgende C#-codevoorbeeld is afkomstig uit een Visual Studio **Windows Classic Desktop** C#-consoletoepassing. Het project heeft de **WindowsAzure.ServiceBus** NuGet-pakket geïnstalleerd.

* Vervang de tijdelijke aanduiding voor tekenreeks met een verbindingsreeks die gebruikmaakt van de **Event Hub-compatibele eindpunt** en service **primaire sleutel** waarden die u hebt genoteerd eerder zoals wordt weergegeven in het volgende voorbeeld:

    ```cs
    "Endpoint={your Event Hub-compatible endpoint};SharedAccessKeyName=service;SharedAccessKey={your service primary key value}"
    ```

* Vervang de bewaking eindpunt de naam van de tijdelijke aanduiding met de **Event Hub-compatibele naam** waarde die u eerder hebt genoteerd.

```cs
class Program
{
    static string connectionString = "{your monitoring endpoint connection string}";
    static string monitoringEndpointName = "{your monitoring endpoint name}";
    static EventHubClient eventHubClient;

    static void Main(string[] args)
    {
        Console.WriteLine("Monitoring. Press Enter key to exit.\n");

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, monitoringEndpointName);
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
        CancellationTokenSource cts = new CancellationTokenSource();
        var tasks = new List<Task>();

        foreach (string partition in d2cPartitions)
        {
            tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }

        Console.ReadLine();
        Console.WriteLine("Exiting...");
        cts.Cancel();
        Task.WaitAll(tasks.ToArray());
    }

    private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
    {
        var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
        while (true)
        {
            if (ct.IsCancellationRequested)
            {
                await eventHubReceiver.CloseAsync();
                break;
            }

            EventData eventData = await eventHubReceiver.ReceiveAsync(new TimeSpan(0,0,10));

            if (eventData != null)
            {
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
    }
}
```

## <a name="next-steps"></a>Volgende stappen
Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

<!-- Links and images -->
[1]: media/iot-hub-operations-monitoring/enable-OM-1.png
[2]: media/iot-hub-operations-monitoring/enable-OM-2.png
[img-endpoints]: media/iot-hub-operations-monitoring/monitoring-endpoint.png
[img-service-key]: media/iot-hub-operations-monitoring/service-key.png

[lnk-blog-announcement]: https://azure.microsoft.com/blog/monitor-your-azure-iot-solutions-with-azure-monitor-and-azure-resource-health
[lnk-monitor]: iot-hub-monitor-resource-health.md
[lnk-get-started]: quickstart-send-telemetry-dotnet.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
