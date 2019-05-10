---
title: Gebeurtenissen verzenden naar een Azure Time Series Insights-omgeving | Microsoft Docs
description: Informatie over het configureren van een event hub en een voorbeeldtoepassing om gebeurtenissen te verzenden u in Azure Time Series Insights bekijken kunt uitvoeren.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/06/2019
ms.custom: seodec18
ms.openlocfilehash: 2842a365cdf25a6b19f655f6397d62ecb9a723b0
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406877"
---
# <a name="send-events-to-a-time-series-insights-environment-by-using-an-event-hub"></a>Gebeurtenissen verzenden naar een Time Series Insights-omgeving met behulp van een event hub

In dit artikel wordt uitgelegd hoe u maken en configureren van een event hub in Azure Event Hubs. Ook wordt beschreven hoe u een voorbeeld van toepassing om gebeurtenissen te verzenden naar Azure Time Series Insights uitvoert vanuit Event Hubs. Als u een bestaande event hub met gebeurtenissen in JSON-indeling hebt, deze zelfstudie overslaan en uw omgeving bekijken in [Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Een Event Hub configureren

1. Zie voor meer informatie over het maken van een event hub, de [documentatie voor Event Hubs](https://docs.microsoft.com/azure/event-hubs/).
1. Zoek in het zoekvak **Event Hubs**. Selecteer in de geretourneerde lijst **Event Hubs**.
1. Selecteer uw event hub.
1. Wanneer u een event hub maakt, maakt u echt een event hub-naamruimte. Als u nog hebt gemaakt een event hub in de naamruimte in het menu onder **entiteiten**, een event hub maken.  

    [![Lijst met eventhubs](media/send-events/updated.png)](media/send-events/updated.png#lightbox)

1. Nadat u een event hub maakt, selecteert u dit in de lijst van eventhubs.
1. In het menu onder **entiteiten**, selecteer **Event Hubs**.
1. Selecteer de naam van de event hub te configureren.
1. Onder **entiteiten**, selecteer **consumentengroepen**, en selecteer vervolgens **Consumergroep**.

    [![Een consumentengroep maken](media/send-events/consumer-group.png)](media/send-events/consumer-group.png#lightbox)

1. Zorg ervoor dat u een consumergroep maakt die uitsluitend door uw Time Series Insights-gebeurtenisbron wordt gebruikt.

    > [!IMPORTANT]
    > Zorg ervoor dat deze consumergroep wordt niet gebruikt door een andere service (zoals een Azure Stream Analytics-taak of een andere Time Series Insights-omgeving). Als de consumergroep wordt gebruikt door de andere worden services, leesbewerkingen negatief beïnvloed voor deze omgeving en voor andere services. Als u **$Default** als de consumergroep andere lezers mogelijk uw consumentengroep kunnen gebruiken.

1. In het menu onder **instellingen**, selecteer **beleid voor gedeelde toegang**, en selecteer vervolgens **toevoegen**.

    [![Beleid voor gedeelde toegang selecteren en selecteer vervolgens de knop toevoegen](media/send-events/shared-access-policy.png)](media/send-events/shared-access-policy.png#lightbox)

1. In de **nieuw beleid voor gedeelde toegang toevoegen** deelvenster maken van een gedeelde toegang met de naam **MySendPolicy**. U kunt dit beleid voor gedeelde toegang gebruiken om te verzenden van gebeurtenissen in de C# voorbeelden verderop in dit artikel.

    [![Voer in het naamvak beleid MySendPolicy](media/send-events/shared-access-policy-2.png)](media/send-events/shared-access-policy-2.png#lightbox)

1. Onder **Claim**, selecteer de **verzenden** selectievakje.

## <a name="add-a-time-series-insights-instance"></a>Een Time Series Insights-exemplaar toevoegen

De update Time Series Insights maakt gebruik van exemplaren contextuele gegevens toevoegen aan binnenkomende telemetriegegevens. De gegevens op het moment dat de query is gekoppeld met behulp van een **Time Series-ID**. De **Time Series-ID** project dat we verderop in dit artikel gebruiken is voor de voorbeeld-windmills `id`. Voor meer informatie over Time Series Insight-exemplaren en **Time Series-ID**, Zie [Time Series modellen](./time-series-insights-update-tsm.md).

### <a name="create-a-time-series-insights-event-source"></a>Een Time Series Insights-gebeurtenisbron maken

1. Als u een gebeurtenisbron hebt gemaakt, de stappen voor voltooid [een gebeurtenisbron maken](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-eventhub).

1. Een waarde instellen voor `timeSeriesId`. Voor meer informatie over **Time Series-ID**, Zie [Time Series modellen](./time-series-insights-update-tsm.md).

### <a name="push-events"></a>Push-gebeurtenissen (windmills voorbeeld)

1. Zoek in de zoekbalk typt, **Event Hubs**. Selecteer in de geretourneerde lijst **Event Hubs**.

1. Selecteer uw event hub.

1. Ga naar **gedeeld toegangsbeleid** > **RootManageSharedAccessKey**. Kopieer de waarde voor **Connection String-primaire sleutel**.

    [![Kopieer de waarde voor de verbindingsreeks voor de primaire sleutel](media/send-events/sample-code-connection-string.png)](media/send-events/sample-code-connection-string.png#lightbox)

1. Ga naar https://tsiclientsample.azurewebsites.net/windFarmGen.html. De URL wordt gesimuleerd Zwart gat apparaten uitgevoerd.
1. In de **Event Hub-verbindingsreeks** vak op de webpagina, plak de verbindingsreeks die u hebt gekopieerd in [gebeurtenissen pusht](#push-events).
  
    [![Plak de verbindingsreeks voor de primaire sleutel in het vak Event Hub-verbindingsreeks](media/send-events/updated_two.png)](media/send-events/updated_two.png#lightbox)

1. Selecteer **Klik hier om te beginnen**. De simulator genereert exemplaar JSON die u rechtstreeks kunt gebruiken.

1. Ga terug naar uw event hub in Azure portal. Op de **overzicht** pagina, ziet u de nieuwe gebeurtenissen worden ontvangen door de event hub.

    [![Een event hub-overzichtspagina met metrische gegevens voor de event hub](media/send-events/telemetry.png)](media/send-events/telemetry.png#lightbox)

## <a name="json"></a>Ondersteunde JSON-vormen

### <a name="example-one"></a>Voorbeeld van een

* **Invoer**: Een eenvoudig JSON-object.

    ```JSON
    {
        "id":"device1",
        "timestamp":"2016-01-08T01:08:00Z"
    }
    ```

* **Uitvoer**: Een gebeurtenis.

    |id|tijdstempel|
    |--------|---------------|
    |device1|2016-01-08T01:08:00Z|

### <a name="example-two"></a>Voorbeeld 2

* **Invoer**: Een JSON-matrix met twee JSON-objecten. Elk JSON-object wordt geconverteerd naar een gebeurtenis.

    ```JSON
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

* **Uitvoer**: Twee gebeurtenissen.

    |id|tijdstempel|
    |--------|---------------|
    |device1|2016-01-08T01:08:00Z|
    |device2|2016-01-08T01:17:00Z|

### <a name="example-three"></a>Voorbeeld 3

* **Invoer**: Een JSON-object met een geneste JSON-matrix met twee JSON-objecten.

    ```JSON
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

* **Uitvoer**: Twee gebeurtenissen. De eigenschap **locatie** is gekopieerd naar elke gebeurtenis.

    |location|events.id|events.timestamp|
    |--------|---------------|----------------------|
    |WestUs|device1|2016-01-08T01:08:00Z|
    |WestUs|device2|2016-01-08T01:17:00Z|

### <a name="example-four"></a>Voorbeeld 4

* **Invoer**: Een JSON-object met een geneste JSON-matrix met twee JSON-objecten. Deze invoer blijkt dat de algemene eigenschappen kunnen worden gerepresenteerd door het complexe JSON-object.

    ```JSON
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

* **Uitvoer**: Twee gebeurtenissen.

    |location|manufacturer.name|manufacturer.location|events.id|events.timestamp|events.data.type|events.data.units|events.data.value|
    |---|---|---|---|---|---|---|---|
    |WestUs|manufacturer1|EastUs|device1|2016-01-08T01:08:00Z|pressure|psi|108.09|
    |WestUs|manufacturer1|EastUs|device2|2016-01-08T01:17:00Z|vibration|abs G|217.09|

## <a name="next-steps"></a>Volgende stappen

- [Uw omgeving bekijken](https://insights.timeseries.azure.com) in de Verkenner van Time Series Insights.
