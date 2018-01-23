---
title: Azure IoT Hub-bewerkingen controleren | Microsoft Docs
description: Het gebruik van Azure IoT Hub-bewerkingen voor het controleren van de status van bewerkingen voor uw IoT-hub in real-time bewaking.
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: a299f3a5-b14d-4586-9c3b-44aea14ed013
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2017
ms.author: nberdy
ms.openlocfilehash: 94cbef9d01299547a48923876cf134d5f3dafa6b
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="iot-hub-operations-monitoring"></a>IoT Hub bewerkingen controleren

IoT Hub operations bewaking kunt u de status van bewerkingen voor uw IoT-hub in realtime controleren. IoT Hub houdt gebeurtenissen over verschillende categorieën van bewerkingen. U kunt kiezen voor het verzenden van gebeurtenissen uit een of meer categorieën voor een eindpunt van uw iothub voor verwerking. U kunt de gegevens op fouten controleren of complexe verwerking op basis van gegevenspatronen instellen.

>[!NOTE]
>IoT Hub operations bewaking is afgeschaft en wordt verwijderd uit IoT Hub voor 10 oktober 2018. Zie voor het bewaken van de bewerkingen en de status van de IoT Hub, [bewaken van de status van Azure IoT Hub en snel problemen onderzoeken][lnk-monitor]. Zie voor meer informatie over de tijdlijn afschaffing [bewaken van uw Azure-IoT-oplossingen met Azure Monitor en Azure resourcestatus][lnk-blog-announcement].

IoT Hub bewaakt zes soorten gebeurtenissen:

* Bewerkingen voor apparaat-id
* De apparaattelemetrie
* Cloud-naar-apparaat-berichten
* Verbindingen
* Uploaden van bestanden
* Berichtroutering

> [!IMPORTANT]
> IoT Hub operations bewaking garandeert niet betrouwbaar of geordende levering van gebeurtenissen. Afhankelijk van de onderliggende infrastructuur IoT Hub, sommige gebeurtenissen mogelijk verloren gegaan of volgorde geleverd. Bewerkingen controleren voor het genereren van waarschuwingen op basis van de fout signalen zoals mislukte verbindingspogingen of hoge frequentie worden verbroken voor specifieke apparaten gebruiken. U dient niet vertrouwen op de operations-controle van gebeurtenissen voor het maken van een consistente archief voor de status van het apparaat, bijvoorbeeld een archief bijhouden verbonden of losgekoppeld van de status van een apparaat. 

## <a name="how-to-enable-operations-monitoring"></a>Het inschakelen van bewaking bewerkingen

1. Een iothub maken. U vindt instructies over het maken van een IoT-hub in de [aan de slag] [ lnk-get-started] handleiding.

1. Open de blade van uw IoT-hub. Van daaruit, klikt u op **Operations bewaking**.

    ![Toegangsbewerkingen configuratie in de portal van de bewaking][1]

1. Selecteer de bewaking categorieën die u wilt bewaken, en klik vervolgens op **opslaan**. De gebeurtenissen beschikbaar zijn voor het lezen van het Event Hub-compatibele eindpunt die worden vermeld in **Bewakingsinstellingen**. Het eindpunt van de IoT Hub heet `messages/operationsmonitoringevents`.

    ![Bewerkingen bewaking op uw IoT-hub configureren][2]

> [!NOTE]
> Selecteren **uitgebreid** bewaking voor de **verbindingen** categorie zorgt ervoor dat de IoT Hub voor het genereren van aanvullende diagnostische berichten. Voor alle andere categorieën, de **uitgebreid** in elke foutbericht instellen van de hoeveelheid gegevens IoT Hub bevat.

## <a name="event-categories-and-how-to-use-them"></a>Gebeurteniscategorieën en hoe ze te gebruiken

Elke categorie houdt bewaking operations een ander type interactie met IoT Hub en elke categorie bewaking is een schema dat bepaalt hoe de structuur van gebeurtenissen in die categorie.

### <a name="device-identity-operations"></a>Bewerkingen voor apparaat-id

De apparaatcategorie identity-bewerkingen bijgehouden fouten die zich voordoen wanneer u probeert te maken, bijwerken of verwijderen van een vermelding in het identiteitenregister van uw IoT-hub. Het bijhouden van deze categorie is nuttig voor het inrichten van scenario's.

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

### <a name="device-telemetry"></a>De apparaattelemetrie

De telemetrie apparaatcategorie houdt fouten die optreden bij de IoT-hub en gerelateerd zijn aan de pijplijn telemetrie. Deze categorie bevat fouten die optreden bij het verzenden van telemetriegebeurtenissen (zoals beperking) en telemetrische gebeurtenissen (zoals onbevoegde lezer) ontvangen. Deze categorie kan geen catch-fouten worden veroorzaakt door de code die wordt uitgevoerd op het apparaat zelf.

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

De categorie cloud-naar-apparaatopdrachten houdt fouten die optreden bij de IoT-hub en gerelateerd zijn aan de pijplijn cloud-naar-apparaat-bericht. Deze categorie bevat fouten die optreden bij het verzenden van berichten van de cloud-naar-apparaat (zoals niet-geautoriseerde afzender), het ontvangen van berichten van de cloud-naar-apparaat (zoals levering aantal overschreden) en het cloud-naar-apparaat bericht feedback ontvangen (zoals feedback verlopen). Deze categorie onderschept fouten van een apparaat dat een bericht cloud-naar-apparaat niet goed verwerkt als de cloud-naar-apparaat-bericht is bezorgd niet.

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

De categorie verbindingen houdt fouten die optreden wanneer apparaten verbinding maken met of Verbreek de verbinding een IoT-hub tussen. Het bijhouden van deze categorie is handig voor het identificeren van niet-geautoriseerde verbindingspogingen en voor het bijhouden van wanneer een verbinding verbroken voor apparaten in de gebieden van slechte connectiviteit wordt.

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

Het bestand uploaden categorie houdt fouten die optreden bij de IoT-hub en gerelateerd zijn aan de functionaliteit van het uploaden van bestanden. Deze categorie omvat:

* Fouten die bij de SAS-URI optreden, zoals wanneer het voordat een apparaat een melding van de hub van het uploaden van een voltooide verloopt.
* Kan geen uploads gemeld door het apparaat.
* Fouten die optreden wanneer een bestand niet in de opslag tijdens het maken van message notification IoT Hub gevonden is.

Deze categorie kan geen catch-fouten die rechtstreeks zich tijdens het uploaden van het apparaat is een bestand naar de opslag.

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

De routering categorie bericht houdt fouten die optreden tijdens bericht route evaluatie- en eindpunt status waargenomen door de IoT Hub. Deze categorie omvat gebeurtenissen zoals wanneer een regel resulteert in 'niet-gedefinieerde', wanneer IoT Hub markeert een eindpunt als onbestelbare en eventuele andere fouten die zijn ontvangen van een eindpunt. Deze categorie omvat geen specifieke fouten over de berichten zelf (zoals apparaat beperking fouten), die worden gemeld onder de categorie 'apparaattelemetrie '.

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

## <a name="view-events"></a>Gebeurtenissen weergeven

U kunt de *iothub explorer* hulpprogramma voor het snel testen van uw IoT-hub genereren van gebeurtenissen voor bewaking. Zie de instructies in het installeren van het hulpprogramma de [iothub explorer] [ lnk-iothub-explorer] GitHub-opslagplaats.

1. Zorg ervoor dat de **verbindingen** categorie bewaking is ingesteld op **uitgebreid** in de portal.

1. Voer de volgende opdracht om te lezen van het controle-eindpunt bij een opdrachtprompt:

    ```
    iothub-explorer monitor-ops --login {your iothubowner connection string}
    ```

1. Voer de volgende opdracht om te simuleren van een apparaat verzenden van apparaat-naar-cloud-berichten in een andere opdrachtprompt:

    ```
    iothub-explorer simulate-device {your device name} --send "My test message" --login {your iothubowner connection string}
    ```

1. De eerste opdrachtprompt toont de bewakingsgebeurtenissen het gesimuleerde apparaat verbinding maakt met uw IoT-hub.

## <a name="connect-to-the-monitoring-endpoint"></a>Verbinding maken met het controle-eindpunt

Het eindpunt van de controle op uw IoT-hub is een Event Hub-compatibele eindpunt. U kunt een mechanisme dat werkt met Event Hubs bewaking om berichten te lezen van dit eindpunt. Het volgende voorbeeld maakt een basislezer die niet geschikt voor een implementatie met hoge doorvoer. Zie voor meer informatie over het verwerken van Event Hubs-berichten de zelfstudie [Aan de slag met Event Hubs][lnk-eventhubs-tutorial].

Voor verbinding met het controle-eindpunt, moet u een verbindingsreeks en de naam van het eindpunt. De volgende stappen ziet u hoe u de vereiste waarden vinden in de portal:

1. Navigeer naar de resourceblade van uw IoT-Hub in de portal.

1. Kies **Operations bewaking**, en noteer de **Event Hub-compatibele naam** en **Event Hub-compatibele eindpunt** waarden:

    ![Event Hub-compatibele eindpunt waarden][img-endpoints]

1. Kies **gedeeld toegangsbeleid**, en kies vervolgens **service**. Noteer de **primaire sleutel** waarde:

    ![Gedeelde beleid primaire toegangssleutel voor][img-service-key]

De volgende C#-codevoorbeeld is genomen van een Visual Studio **Classic Windows Desktop** C#-consoletoepassing. Het project heeft de **WindowsAzure.ServiceBus** NuGet-pakket geïnstalleerd.

* De tijdelijke aanduiding voor tekenreeks verbinding vervangen door een verbindingsreeks die gebruikmaakt van de **Event Hub-compatibele eindpunt** en service **primaire sleutel** waarden die u eerder weergegeven in het volgende voorbeeld is aangegeven:

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
[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[lnk-diagnostic-metrics]: iot-hub-metrics.md
[lnk-scaling]: iot-hub-scaling.md
[lnk-dr]: iot-hub-ha-dr.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-iothub-explorer]: https://github.com/azure/iothub-explorer
[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
