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
ms.date: 11/26/2018
ms.openlocfilehash: 64fb9f72cfd7edef18d56f15cbcce726dd33b50d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847266"
---
# <a name="send-events-to-a-time-series-insights-environment-using-event-hub"></a>Gebeurtenissen verzenden naar een Time Series Insights-omgeving met Event Hub

Dit artikel wordt uitgelegd hoe u maken en configureren van de event hub en een voorbeeldtoepassing om gebeurtenissen te verzenden uitvoeren. Als u een bestaande event hub met gebeurtenissen in JSON-indeling hebt, deze zelfstudie overslaan en uw omgeving bekijken in [Azure Time Series Insights](./time-series-insights-update-create-environment.md).

## <a name="configure-an-event-hub"></a>Een Event Hub configureren

1. Voor het maken van een Event Hub, volgt u de instructies uit de Event Hub [documentatie](https://docs.microsoft.com/azure/event-hubs/).
1. Zoeken naar Event Hub in de zoekbalk. Klik op **Event Hubs** in de lijst met resultaten.
1. Selecteer uw Event Hub door te klikken op de naam ervan.
1. Onder **entiteiten** Klik in het Configuratievenster middelste **Event Hubs** opnieuw.
1. Selecteer de naam van de Event Hub te configureren.

    ![consumer-group][1]

1. Onder **entiteiten**, selecteer **consumentengroepen**.
1. Zorg ervoor dat u maakt u een consumentengroep die uitsluitend door uw TSI-gebeurtenisbron wordt gebruikt.

> [!IMPORTANT]
> Zorg ervoor dat deze consumergroep wordt niet gebruikt door een andere service (zoals Stream Analytics-taak of een andere TSI-omgeving). Als de consumergroep wordt gebruikt door andere wordt services, leesbewerking negatief beïnvloed voor deze omgeving en de andere services. Als u `$Default` als consumergroep, kan dit leiden tot mogelijk hergebruik door andere lezers.

1. Onder de **instellingen** kop, selecteer **Share toegangsbeleid**.
1. Maak op de gebeurtenishub **MySendPolicy** die wordt gebruikt voor het verzenden van gebeurtenissen in de C# voorbeeld.

    ![gedeelde toegang-een =][2]

    ![gedeelde toegang twee][3]

## <a name="add-time-series-insights-instances"></a>Time Series Insights-exemplaren toe te voegen

De update TSI maakt gebruik van exemplaren contextuele gegevens toevoegen aan binnenkomende telemetriegegevens. De gegevens is gekoppeld op query moment een **Time Series-ID**. De **Time Series-ID** project is voor de voorbeeld-windmills `Id`. Voor meer informatie over Time Series-instanties en **-id's van Time Series**Lees [Time Series modellen](./time-series-insights-update-tsm.md).

### <a name="create-time-series-insights-event-source"></a>Een Time Series Insights-gebeurtenisbron maken

1. Als u een gebeurtenisbron hebt gemaakt, volg deze instructies om een gebeurtenisbron te maken.
1. Geef de `timeSeriesId` – verwijzen naar [Time Series modellen](./time-series-insights-update-tsm.md) voor meer informatie over **-id's van Time Series**.

### <a name="push-events-sample-windmills"></a>Push-gebeurtenissen (voorbeeld windmills)

1. Zoeken naar event hub in de zoekbalk. Klik op **Event Hubs** in de lijst met resultaten.
1. Selecteer uw event hub door te klikken op de naam ervan.
1. Ga naar **gedeeld toegangsbeleid** en vervolgens **RootManageSharedAccessKey**. Kopieer de **Connection String-primaire sleutel**

   ![connection-string][4]

1. Ga naar https://tsiclientsample.azurewebsites.net/windFarmGen.html. Deze gesimuleerde Zwart gat apparaten wordt uitgevoerd.
1. Plak de verbindingsreeks die is opgehaald uit stap 3 de **Event Hub-verbindingsreeks**
1. Klik op **Klik hier om te starten**
1. Ga terug naar uw Event Hub. Hier ziet u de nieuwe gebeurtenissen worden ontvangen door de hub:

   ![telemetrie][5]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw omgeving bekijken in Time Series Insights Explorer](https://insights.timeseries.azure.com).

<!-- Images -->
[1]: media/send-events/consumer-group.png
[2]: media/send-events/shared-access-policy.png
[3]: media/send-events/shared-access-policy-2.png
[4]: media/send-events/sample-code-connection-string.png
[5]: media/send-events/telemetry.png