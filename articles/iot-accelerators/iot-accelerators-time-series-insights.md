---
title: Externe bewaking van gegevens met Azure Time Series Insights visualiseren | Microsoft Docs
description: Informatie over het configureren van uw Time Series Insights-omgeving om te verkennen en analyseren van de time series-gegevens van uw oplossing voor externe controle.
author: philmea
manager: timlt
ms.author: philmea
ms.date: 04/29/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 10617c129212d8196897af750c02647f0086c8e5
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39185887"
---
# <a name="visualize-remote-monitoring-data-with-time-series-insights"></a>Externe bewaking gegevens visualiseren met Time Series Insights

Een operator kan wilt verder uitbreiden van de kant van de data box visualisatie geleverd door de externe controle vooraf configureren oplossing. Onze oplossingsverbetering biedt buiten de box-integratie met TSI. In deze instructies leert u hoe het configureren van Time Series Insights voor het analyseren van telemetrie van apparaten en afwijkingen.

## <a name="prerequisites"></a>Vereisten

Als u wilt deze procedures hebt voltooid, moet u het volgende:

* [De externe controle vooraf geconfigureerde oplossing implementeren](quickstart-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Een consumentengroep maken

U moet maken van een speciale klantengroep in uw IoT-hub moet worden gebruikt voor het streamen van gegevens naar de Time Series Insights.

> [!NOTE]
> Consumer-groepen worden gebruikt door toepassingen voor het ophalen van gegevens van Azure IoT Hub. Elke consumentengroep kan maximaal vijf uitvoer consumenten. Voor elke vijf sinks uitvoer en kunt u maximaal 32 consumergroepen maken, moet u een nieuwe consumergroep maken.

1. Klik op de knop Cloud Shell in de Azure-portal.

1. Voer de volgende opdracht om een nieuwe consumergroep maakt:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="create-a-new-time-series-insights-environment"></a>Maak een nieuwe Time Series Insights-omgeving

Azure Time Series Insights is een volledig beheerde analyse-, opslag- en visualisatieservice waarmee u tijdseriegegevens op IoT-schaal kunt beheren in de cloud. De service voorziet in schaalbare opslag voor tijdseriegegevens en biedt u de mogelijkheid om binnen enkele seconden miljarden binnenkomende streaminggebeurtenissen te verkennen en analyseren. Gebruik Time Series Insights opslaan en beheren van terabytes aan tijdseriegegevens, verkennen, en miljarden gebeurtenissen tegelijkertijd te visualiseren en uitvoeren van de hoofdoorzaak wordt onderzocht, en om meerdere sites en assets te vergelijken.

1. Meld u aan bij [Azure Portal](http://portal.azure.com/).

1. Selecteer **een resource maken** > **Internet of Things** > **Time Series Insights**.

    ![Nieuwe Time Series Insights](./media/iot-accelerators-time-series-insights/new-time-series-insights.png)

1. Voor het maken van uw Time Series Insights-omgeving gebruikt u de waarden in de volgende tabel:

    | Instelling | Waarde |
    | ------- | ----- |
    | Naam van de omgeving | De volgende schermafbeelding wordt gebruikt voor de naam van de **contorosrmtsi**. Kies uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | Abonnement | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | Resourcegroep | **Maak een nieuwe**. We gebruiken de naam van de **ContosoRM**. |
    | Locatie | We gebruiken **VS-Oost**. Maak uw omgeving in dezelfde regio als uw oplossing voor externe controle. |
    | Sku |**S1** |
    | Capaciteit | **1** |
    | Vastmaken aan dashboard | **Ja** |

    ![Time Series Insights maken](./media/iot-accelerators-time-series-insights/new-time-series-insights-create.png)

1. Klik op **Create**. Duurt het even de tijd voor de omgeving moet worden gemaakt.

## <a name="create-event-source"></a>Gebeurtenisbron maken

Maak een nieuwe gebeurtenisbron verbinding maken met uw IoT-hub. Zorg ervoor dat u de consumergroep in de vorige stappen hebt gemaakt. Time Series Insights is vereist voor elke service hebben een speciale klantengroep niet wordt gebruikt door een andere service.

1. Navigeer naar uw nieuwe Time Series-omgeving.

1. Selecteer aan de linkerkant **gebeurtenisbronnen**.

    ![Bronnen van gebeurtenissen weergeven](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources.png)

1. Klik op **Add**.

    ![De bron van gebeurtenis toevoegen](./media/iot-accelerators-time-series-insights/time-series-insights-event-sources-add.png)

1. Voor het configureren van uw IoT-hub als een nieuwe gebeurtenisbron, gebruikt u de waarden in de volgende tabel:

    | Instelling | Waarde |
    | ------- | ----- |
    | De naam van de bron van gebeurtenis | De volgende schermafbeelding wordt gebruikt voor de naam van de **contosorm-iot-hub**. Gebruik uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | Bron | **IoT Hub** |
    | Importoptie | **IoT-Hub uit de beschikbare abonnementen gebruiken** |
    | Abonnements-id | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | De naam van de IOT-hub | **contosorma57a6**. Gebruik de naam van uw IoT-hub in uw oplossing voor externe controle. |
    | Naam van IOT hub-beleid | **iothubowner** Zorg ervoor dat het beleid op waarmee een beleid voor de eigenaar van is. |
    | Beleidssleutel voor IOT hub | Dit veld wordt automatisch gevuld. |
    | IOT hub-consumentengroep | **timeseriesinsights** |
    | Serialisatie-indeling voor gebeurtenissen | **JSON**     | Naam van de eigenschap timestamp | Leeg laten |

    ![Gebeurtenisbron maken](./media/iot-accelerators-time-series-insights/time-series-insights-event-source-create.png)

1. Klik op **Create**.

> [!NOTE]
> Als u moet extra gebruikers toegang verlenen tot de Verkenner van Time Series Insights, kunt u deze stappen voor het [gegevenstoegang verlenen](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access#grant-data-access).

## <a name="time-series-insights-explorer"></a>Time Series Insights Explorer

De Verkenner van Time Series Insights is een WebApp die helpt u bij het maken van visualisaties van uw gegevens.

1. Selecteer de **overzicht** tabblad.

1. Klik op **gaat u naar omgeving**, die de Time Series Insights explorer web-app wordt geopend.

    ![Time Series Insights Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-environment.png)

1. Selecteer in het deelvenster tijd selecteren **afgelopen 12 uur** van de snelle tijden en klik op **zoeken**.

    ![Zoeken in Time Series Insights Explorer](./media/iot-accelerators-time-series-insights/time-series-insights-search-time.png)

1. Selecteer in het deelvenster voorwaarden aan de linkerkant de waarde van een meting van **temperatuur** en een waarde gesplitst door van **iothub-verbinding-apparaat-id**.

    ![Time Series Insights Explorer-Query](./media/iot-accelerators-time-series-insights/time-series-insights-query1.png)

1. Met de rechtermuisknop op de grafiek en selecteer **gebeurtenissen onderzoeken**.

    ![Time Series Insights Explorer-gebeurtenissen](./media/iot-accelerators-time-series-insights/time-series-insights-explore-events.png)

1. De gebeurtenissen worden weergegeven in het raster in tabelvorm.

    ![Time Series Insights Explorer tabel](./media/iot-accelerators-time-series-insights/time-series-insights-table.png)

1. Klik op de knop perspectief weergeven.

    ![Time Series Insights Explorer perspectief](./media/iot-accelerators-time-series-insights/time-series-insights-explorer-perspective.png)

1. Klik op **toevoegen** te maken van een nieuwe query in het perspectief.

    ![Time Series Insights Explorer Query toevoegen](./media/iot-accelerators-time-series-insights/time-series-insights-new-query.png)

1. Selecteer een tijdstip snelle **afgelopen 12 uur**, een meting van **vochtigheid** en een gesplitst door van **iothub-verbinding-apparaat-id**.

    ![Time Series Insights Explorer-Query](./media/iot-accelerators-time-series-insights/time-series-insights-query2.png)

1. Klik op de knop perspectief weergave om het dashboard van apparaat metrische gegevens weer te geven.

    ![Time Series Insights Explorer-Dashboard](./media/iot-accelerators-time-series-insights/time-series-insights-dashboard.png)

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het verkennen en het opvragen van gegevens in de Verkenner van Time Series Insights, [Azure Time Series Insights explorer](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).
