---
title: Over het verzenden van gebeurtenissen naar een Azure Time Series Insights-omgeving | Microsoft Docs
description: In deze zelfstudie wordt uitgelegd hoe u maken en event hub configureren en uitvoeren van een voorbeeldtoepassing om gebeurtenissen te verzenden in Azure Time Series Insights moet worden weergegeven.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 12/03/2018
ms.openlocfilehash: c583c2211297acd83f88d23b2b8cbd9f8207927f
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52867597"
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Gebeurtenissen verzenden naar een Time Series Insights-omgeving met Event Hub

Dit artikel wordt uitgelegd hoe u maken en configureren van de event hub en een voorbeeldtoepassing om gebeurtenissen te verzenden uitvoeren. Als u een bestaande event hub met gebeurtenissen in JSON-indeling hebt, deze zelfstudie overslaan en uw omgeving bekijken in [Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Een Event Hub configureren

1. Voor het maken van een Event Hub, volgt u de instructies uit de Event Hub [documentatie](https://docs.microsoft.com/azure/event-hubs/).
1. Zoeken naar `Event Hub` in de zoekbalk. Klik op **Event Hubs** in de lijst met resultaten.
1. Selecteer uw Event Hub door te klikken op de naam ervan.
1. Wanneer u een Event Hub maakt, maakt u een Event Hub-Namespace eigenlijk.  Als u nog hebt om te maken van een Event Hub in de Namespace, een onder entiteiten maken.  

    ![bijgewerkt][1]

1. Als u een Event Hub hebt gemaakt, klikt u op de naam ervan.
1. Onder **entiteiten** Klik in het Configuratievenster middelste **Event Hubs** opnieuw.
1. Selecteer de naam van de Event Hub te configureren.

    ![consumer-group][2]

1. Onder **entiteiten**, selecteer **consumentengroepen**.
1. Zorg ervoor dat u maakt u een consumentengroep die uitsluitend door uw TSI-gebeurtenisbron wordt gebruikt.

    > [!IMPORTANT]
    > Zorg ervoor dat deze consumergroep wordt niet gebruikt door een andere service (zoals Stream Analytics-taak of een andere TSI-omgeving). Als de consumergroep wordt gebruikt door andere wordt services, leesbewerking negatief beïnvloed voor deze omgeving en de andere services. Als u `$Default` als consumergroep, kan dit leiden tot mogelijk hergebruik door andere lezers.

1. Onder de **instellingen** kop, selecteer **Share toegangsbeleid**.
1. Maak op de gebeurtenishub **MySendPolicy** die wordt gebruikt voor het verzenden van gebeurtenissen in de C# voorbeeld.

    ![gedeelde toegang-een =][3]

    ![gedeelde toegang twee][4]

## <a name="add-time-series-insights-instances"></a>Time Series Insights-exemplaren toe te voegen

De update TSI maakt gebruik van exemplaren contextuele gegevens toevoegen aan binnenkomende telemetriegegevens. De gegevens is gekoppeld op query moment een **Time Series-ID**. De **Time Series-ID** project is voor de voorbeeld-windmills `Id`. Voor meer informatie over Time Series-instanties en **-id's van Time Series**Lees [Time Series modellen](./time-series-insights-update-tsm.md).

### <a name="create-time-series-insights-event-source"></a>Een Time Series Insights-gebeurtenisbron maken

1. Als u nog geen gebeurtenisbron hebt gemaakt, volgt u [deze instructies](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub) om een gebeurtenisbron te maken.
1. Geef de `timeSeriesId` – verwijzen naar [Time Series modellen](./time-series-insights-update-tsm.md) voor meer informatie over **-id's van Time Series**.

### <a name="push-events-sample-windmills"></a>Push-gebeurtenissen (voorbeeld windmills)

1. Zoeken naar event hub in de zoekbalk. Klik op **Event Hubs** in de lijst met resultaten.
1. Selecteer uw event hub door te klikken op de naam ervan.
1. Ga naar **gedeeld toegangsbeleid** en vervolgens **RootManageSharedAccessKey**. Kopieer de **Connection String-primaire sleutel**

   ![connection-string][5]

1. Ga naar https://tsiclientsample.azurewebsites.net/windFarmGen.html. Deze gesimuleerde Zwart gat apparaten wordt uitgevoerd.
1. Plak de verbindingsreeks die is opgehaald uit stap 3 de **Event Hub-verbindingsreeks**.

    ![connection-string][6]

1. Klik op **Klik hier om te starten**. De simulator wordt ook een JSON-exemplaar waarmee u rechtstreeks kunt genereren.
1. Ga terug naar uw Event Hub. Hier ziet u de nieuwe gebeurtenissen worden ontvangen door de hub:

   ![telemetrie][7]

<a id="json"></a>

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

#### <a name="output---one-event"></a>Uitvoer - één gebeurtenis

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

#### <a name="output---two-events"></a>Uitvoer - twee gebeurtenissen

|id|tijdstempel|
|--------|---------------|
|device1|2016-01-08T01:08:00Z|
|device2|2016-01-08T01:17:00Z|

### <a name="sample-3"></a>Voorbeeld 3

#### <a name="input"></a>Invoer

Een JSON-object met een geneste JSON-matrix met twee JSON-objecten:
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

#### <a name="output---two-events"></a>Uitvoer - twee gebeurtenissen

U ziet dat de eigenschap 'location' naar elk van de gebeurtenis is gekopieerd.

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

#### <a name="output---two-events"></a>Uitvoer - twee gebeurtenissen

|location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
|---|---|---|---|---|---|---|---|
|WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
|WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw omgeving bekijken in Time Series Insights Explorer](https://insights.timeseries.azure.com).

<!-- Images -->
[1]: media/send-events/updated.png
[2]: media/send-events/consumer-group.png
[3]: media/send-events/shared-access-policy.png
[4]: media/send-events/shared-access-policy-2.png
[5]: media/send-events/sample-code-connection-string.png
[6]: media/send-events/updated_two.png
[7]: media/send-events/telemetry.png