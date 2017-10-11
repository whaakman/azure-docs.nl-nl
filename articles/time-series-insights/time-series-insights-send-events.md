---
title: Gebeurtenissen verzenden naar een Azure Time Series Insights-omgeving | Microsoft Docs
description: In deze zelfstudie worden de stappen besproken voor het verzenden van gebeurtenissen naar uw Time Series Insights-omgeving
keywords: 
services: tsi
documentationcenter: 
author: venkatgct
manager: jhubbard
editor: 
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/21/2017
ms.author: venkatja
ms.openlocfilehash: b4ef96a045393f28b3cd750068fe82a5a8411afa
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Gebeurtenissen verzenden naar een Time Series Insights-omgeving met Event Hub

In deze zelfstudie wordt uitgelegd hoe u een Event Hub maakt en configureert, en een voorbeeldtoepassing uitvoert om gebeurtenissen te verzenden. Als u een bestaande Event Hub hebt die al gebeurtenissen in JSON-indeling bevat, kunt u deze zelfstudie overslaan en uw omgeving bekijken in [Time Series Insights](https://insights.timeseries.azure.com).

## <a name="configure-an-event-hub"></a>Een Event Hub configureren
1. Voor het maken van uw Event Hub volgt u de instructies in de Event Hub-[documentatie](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).

2. Zorg ervoor dat u een consumergroep maakt die uitsluitend door uw Time Series Insights-gebeurtenisbron wordt gebruikt.

  > [!IMPORTANT]
  > Deze consumergroep mag niet worden gebruikt door een andere service (zoals een Stream Analytics-taak of een andere Time Series Insights-omgeving). Als de consumergroep wordt gebruikt door andere services, wordt de leesbewerking negatief beïnvloed voor deze omgeving en de andere services. Als u $Default als consumergroep selecteert, kan dit leiden tot mogelijk hergebruik door andere lezers.

  ![Event Hub-consumergroep selecteren](media/send-events/consumer-group.png)

3. Maak in de Event Hub 'MySendPolicy'. Dit wordt gebruikt voor het verzenden van gebeurtenissen in het csharp-voorbeeld.

  ![Een beleid voor gedeelde toegang selecteren en klikken op de knop Toevoegen](media/send-events/shared-access-policy.png)  

  ![Een nieuw beleid voor gedeelde toegang toevoegen](media/send-events/shared-access-policy-2.png)  

## <a name="create-time-series-insights-event-source"></a>Een Time Series Insights-gebeurtenisbron maken
1. Als u nog geen gebeurtenisbron hebt gemaakt, volgt u [deze instructies](time-series-insights-add-event-source.md) om een gebeurtenisbron te maken.

2. Geef 'deviceTimestamp' op als de naam van de tijdstempeleigenschap. Deze eigenschap wordt gebruikt als de werkelijke tijdstempel in het csharp-voorbeeld. De naam van de tijdstempeleigenschap is hoofdlettergevoelig en waarden moeten de indeling __jjjj-MM-ddTUU. FFFFFFFK__ volgen wanneer ze als JSON worden verzonden naar een Event Hub. Als de eigenschap niet aanwezig is in de gebeurtenis, wordt de tijd gebruikt waarop de gebeurtenis in de wachtrij van de Event Hub is geplaatst.

  ![Gebeurtenisbron maken](media/send-events/event-source-1.png)

## <a name="sample-code-to-push-events"></a>Voorbeeldcode uitvoeren om gebeurtenissen te pushen
1. Ga naar het Event Hub-beleid 'MySendPolicy' en kopieer de verbindingsreeks met de beleidssleutel.

  ![MySendPolicy-verbindingsreeks kopiëren](media/send-events/sample-code-connection-string.png)

2. Voer de volgende code uit, waarmee voor elk van de drie apparaten 600 gebeurtenissen worden verzonden. Werk `eventHubConnectionString` bij met uw verbindingsreeks.

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.Rdx.DataGenerator
{
    internal class Program
    {
        private static void Main(string[] args)
        {
            var from = new DateTime(2017, 4, 20, 15, 0, 0, DateTimeKind.Utc);
            Random r = new Random();
            const int numberOfEvents = 600;

            var deviceIds = new[] { "device1", "device2", "device3" };

            var events = new List<string>();
            for (int i = 0; i < numberOfEvents; ++i)
            {
                for (int device = 0; device < deviceIds.Length; ++device)
                {
                    // Generate event and serialize as JSON object:
                    // { "deviceTimestamp": "utc timestamp", "deviceId": "guid", "value": 123.456 }
                    events.Add(
                        String.Format(
                            CultureInfo.InvariantCulture,
                            @"{{ ""deviceTimestamp"": ""{0}"", ""deviceId"": ""{1}"", ""value"": {2} }}",
                            (from + TimeSpan.FromSeconds(i * 30)).ToString("o"),
                            deviceIds[device],
                            r.NextDouble()));
                }
            }

            // Create event hub connection.
            var eventHubConnectionString = @"Endpoint=sb://...";
            var eventHubClient = EventHubClient.CreateFromConnectionString(eventHubConnectionString);

            using (var ms = new MemoryStream())
            using (var sw = new StreamWriter(ms))
            {
                // Wrap events into JSON array:
                sw.Write("[");
                for (int i = 0; i < events.Count; ++i)
                {
                    if (i > 0)
                    {
                        sw.Write(',');
                    }
                    sw.Write(events[i]);
                }
                sw.Write("]");

                sw.Flush();
                ms.Position = 0;

                // Send JSON to event hub.
                EventData eventData = new EventData(ms);
                eventHubClient.Send(eventData);
            }
        }
    }
}

```
## <a name="supported-json-shapes"></a>Ondersteunde JSON-vormen
### <a name="sample-1"></a>Voorbeeld 1

#### <a name="input"></a>Invoer

Een eenvoudig JSON-object.

```json
{
    "id":"device1",
    "timestamp":"2016-01-08T01:08:00Z"
}
```
#### <a name="output---1-event"></a>Uitvoer - 1 gebeurtenis

|id|tijdstempel|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|

### <a name="sample-2"></a>Voorbeeld 2

#### <a name="input"></a>Invoer
Een JSON-matrix met twee JSON-objecten. Elk JSON-object wordt omgezet in een gebeurtenis.
```json
[
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    },
    {
        "id":"device2",
        "timestamp":"2016-01-17T01:17:00Z"
    }
]
```
#### <a name="output---2-events"></a>Uitvoer - 2 gebeurtenissen

|id|tijdstempel|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|
### <a name="sample-3"></a>Voorbeeld 3

#### <a name="input"></a>Invoer

Een JSON-object met een geneste JSON-matrix met twee JSON-objecten.
```json
{
    "location":"WestUs",
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z"
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z"
        }
    ]
}

```
#### <a name="output---2-events"></a>Uitvoer - 2 gebeurtenissen
Houd er rekening mee dat de eigenschap 'location' naar elk van de gebeurtenissen wordt gekopieerd.

|location|events.id|events.timestamp|
|--------|---------------|----------------------|
|WestUs|device1|2016-01-08T01:08:00Z|
|WestUs|device2|2016-01-08T01:17:00Z|

### <a name="sample-4"></a>Voorbeeld 4

#### <a name="input"></a>Invoer

Een JSON-object met een geneste JSON-matrix met twee JSON-objecten. Uit deze invoer blijkt dat de algemene eigenschappen kunnen worden vertegenwoordigd door het complexe JSON-object.

```json
{
    "location":"WestUs",
    "manufacturer":{
        "name":"manufacturer1",
        "location":"EastUs"
    },
    "events":[
        {
            "id":"device1",
            "timestamp":"2016-01-08T01:08:00Z",
            "data":{
                "type":"pressure",
                "units":"psi",
                "value":108.09
            }
        },
        {
            "id":"device2",
            "timestamp":"2016-01-17T01:17:00Z",
            "data":{
                "type":"vibration",
                "units":"abs G",
                "value":217.09
            }
        }
    ]
}
```
#### <a name="output---2-events"></a>Uitvoer - 2 gebeurtenissen

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Volgende stappen

* Uw omgeving bekijken in de [Time Series Insights-portal](https://insights.timeseries.azure.com)
