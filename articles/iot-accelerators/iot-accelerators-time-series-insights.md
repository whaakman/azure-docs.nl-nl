---
title: Remote Monitoring gegevens met Azure Time Series Insights visualiseren | Microsoft Docs
description: Informatie over het configureren van uw omgeving Time Series Insights om te verkennen en analyseren van het gegevenstype van de reeks tijd van uw oplossing voor externe controle.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 7a0a5d4f1fbba5d7bd2813e8b9c300a37853e06c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111473"
---
# <a name="visualize-remote-monitoring-data-with-time-series-insights"></a>Externe controle-gegevens visualiseren met Time Series Insights

Een operator kunt verder uitbreiden buiten van de vakgegevens visualisatie geleverd door de externe controle vooraf configureren oplossing. Onze oplossingsverbetering biedt buiten de box-integratie met TSI. In deze instructies leert u het configureren van tijd reeks inzicht te analyseren telemetriegegevens en afwijkingen gedetecteerd.

## <a name="prerequisites"></a>Vereisten

Als u deze instructies, moet u het volgende:

* [De externe controle vooraf geconfigureerde oplossing implementeren](iot-accelerators-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Een consumergroep maken

U moet een speciale klantengroep maken in uw IoT-hub moet worden gebruikt voor het streamen van gegevens naar Time Series Insights.

> [!NOTE]
> Consumer-groepen worden gebruikt door toepassingen voor het ophalen van gegevens uit Azure IoT Hub. Elke consumergroep kan maximaal vijf uitvoer consumenten. Voor elke vijf put uitvoer en u kunt maximaal 32 consumergroepen maken, moet u een nieuwe consumergroep maken.

1. Klik op de knop Cloud Shell in de Azure portal.

1. Voer de volgende opdracht om een nieuwe consumergroep te maken:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="create-a-new-time-series-insights-environment"></a>Een nieuwe Time Series Insights-omgeving maken

Azure Time Series Insights is een volledig beheerde analyse-, opslag- en visualisatieservice waarmee u tijdseriegegevens op IoT-schaal kunt beheren in de cloud. De service voorziet in schaalbare opslag voor tijdseriegegevens en biedt u de mogelijkheid om binnen enkele seconden miljarden binnenkomende streaminggebeurtenissen te verkennen en analyseren. Time Series Insights gebruiken om op te slaan en beheren van terabytes aan timeseries gegevens, verkennen, en tegelijkertijd miljarden gebeurtenissen visualiseren hoofdoorzaak analyse uitvoeren en om meerdere sites en activa te vergelijken.

1. Meld u aan bij [Azure Portal](http://portal.azure.com/).

1. Selecteer **maken van een resource** > **Internet der dingen** > **Time Series Insights**.

    ![Time Series inzichten](./media/iot-accelerators-time-series-insights/new-time-series-insights.png)

1. Voor het maken van uw omgeving Time Series Insights gebruikt u de waarden in de volgende tabel:

    | Instelling | Waarde |
    | ------- | ----- |
    | De Omgevingsnaam van de | Gebruikt de naam van de volgende schermafbeelding **contorosrmtsi**. Kies uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | Abonnement | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | Resourcegroep | **Maken van nieuwe**. We gebruiken de naam van de **ContosoRM**. |
    | Locatie | We gebruiken **VS-Oost**. Uw omgeving maken in dezelfde regio bevinden als uw oplossing voor externe controle. |
    | Sku |**S1** |
    | Capaciteit | **1** |
    | Vastmaken aan dashboard | **Ja** |

    ![Time Series Insights maken](./media/iot-accelerators-time-series-insights/new-time-series-insights-create.png)

1. Klik op **Create**. Het kan even voor de omgeving moet worden gemaakt.

## <a name="create-event-source"></a>Maken van de bron van gebeurtenis

Maak een nieuwe gebeurtenisbron verbinding maken met uw IoT-hub. Zorg ervoor dat u de consumergroep in de vorige stappen hebt gemaakt. Time Series Insights moet elke service hebben een speciale klantengroep niet in gebruik door een andere service.

1. Navigeer naar uw nieuwe Time Series-omgeving.

1. Selecteer aan de linkerkant **gebeurtenisbronnen**.

    ![Bronnen van gebeurtenissen weergeven](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources.png)

1. Klik op **Add**.

    ![Gebeurtenisbron toevoegen](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources-add.png)

1. Uw IoT-hub configureren als een nieuwe gebeurtenisbron, gebruik de waarden in de volgende tabel:

    | Instelling | Waarde |
    | ------- | ----- |
    | Gebeurtenisbron naam | Gebruikt de naam van de volgende schermafbeelding **contosorm-iot-hub**. Gebruik uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | Bron | **IoT Hub** |
    | Importoptie | **Gebruik IoT Hub uit de beschikbare abonnementen** |
    | Abonnements-id | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | De naam van de IOT-hub | **contosorma57a6**. Gebruik de naam van uw IoT-hub van uw oplossing voor externe controle. |
    | Naam voor het IOT-hub | **iothubowner** Zorg ervoor dat het beleid op waarmee een beleid voor de eigenaar van is. |
    | IOT hub beleidssleutel | Dit veld wordt automatisch gevuld. |
    | IOT hub klantengroep | **timeseriesinsights** |
    | Serialisatie-indeling voor gebeurtenissen | **JSON**     | De naam van de timestamp-eigenschap | Leeg laten |

    ![Maken van de bron van gebeurtenis](./media/iot-accelerators-time-series-insights/time-series-insights-event-source-create.png)

1. Klik op **Create**.

> [!NOTE]
> Als u extra gebruikers toegang verlenen aan de Time Series Insights explorer moet, kunt u deze stappen voor het [verlenen van toegang tot gegevens](https://docs.microsoft.com/en-us/azure/time-series-insights/time-series-insights-data-access#grant-data-access).

## <a name="time-series-insights-explorer"></a>Time Series Insights Explorer

De Time Series Insights explorer is een web-app waarmee u kunt visualisaties van uw gegevens maken.

1. Selecteer de **overzicht** tabblad.

1. Klik op **omgeving gaat**, die de Time Series Insights explorer web-app wordt geopend.

    ![Time Series Insights Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-environment.png)

1. Selecteer in het deelvenster van de selectie tijd **afgelopen 12 uur** van de snelle tijden en klik op **Search**.

    ![Zoeken in tijd reeks Insights Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-search-time.png)

1. Selecteer in het deelvenster voorwaarden aan de linkerkant een waarde van de meting van **temperatuur** en een waarde splitsen door van **iothub-verbinding-apparaat-id**.

    ![Tijd reeks Insights Explorer Query](./media/iot-accelerators-time-series-insights/time-series-insights-query1.png)

1. Met de rechtermuisknop op de grafiek en selecteer **verkennen gebeurtenissen**.

    ![Time Series Insights Explorer-gebeurtenissen](./media/iot-accelerators-time-series-insights/time-series-insights-explore-events.png)

1. De gebeurtenissen worden weergegeven in het raster in tabelvorm.

    ![Tabel met Insights Explorer reeks](./media/iot-accelerators-time-series-insights/time-series-insights-table.png)

1. Klik op de knop perspectief weergeven.

    ![Time Series Insights Explorer perspectief](./media/iot-accelerators-time-series-insights/time-series-insights-explorer-perspective.png)

1. Klik op **toevoegen** een nieuwe query maken in het perspectief.

    ![Time Series Insights Explorer Query toevoegen](./media/iot-accelerators-time-series-insights/time-series-insights-new-query.png)

1. Selecteer een snelle-tijd van **afgelopen 12 uur**, een meting van **vochtigheid** en een gesplitste door van **iothub-verbinding-apparaat-id**.

    ![Tijd reeks Insights Explorer Query](./media/iot-accelerators-time-series-insights/time-series-insights-query2.png)

1. Klik op de knop perspectief weergave om het dashboard van apparaat metrische gegevens weer te geven.

    ![Time Series Insights Explorer Dashboard](./media/iot-accelerators-time-series-insights/time-series-insights-dashboard.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het verkennen en opvragen van gegevens in het Time Series Insights explorer, [Azure Time Series Insights explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
