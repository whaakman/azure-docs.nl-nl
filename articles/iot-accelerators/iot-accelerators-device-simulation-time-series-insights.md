---
title: Visualiseer simulatie van apparaattelemetrie met Time Series Insights - Azure | Microsoft Docs
description: Informatie over het configureren van uw Time Series Insights-omgeving om te verkennen en analyseren van telemetrie die is gegenereerd door de oplossingsverbetering voor Apparaatsimulatie.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.date: 08/20/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 5d20adc11e0d679e12fd060e719593a50180db8e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65834920"
---
# <a name="use-time-series-insights-to-visualize-telemetry-sent-from-the-device-simulation-solution-accelerator"></a>Time Series Insights gebruiken om te visualiseren van telemetrie vanaf de oplossingsverbetering voor Apparaatsimulatie verzonden

De oplossingsversnellers Apparaatsimulatie kunt u telemetrie van gesimuleerde apparaten voor het testen van uw IoT-oplossingen te genereren. In deze gebruiksaanwijzing ziet u hoe visualiseren en analyseren van de gesimuleerde telemetrie met behulp van een Time Series Insights-omgeving.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen de stappen in deze handleiding, moet u een actief Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

De stappen in deze handleiding wordt ervan uitgegaan dat u hebt de oplossingsverbetering voor Apparaatsimulatie geïmplementeerd in uw Azure-abonnement. Als u de oplossingsversneller nog niet hebt geïmplementeerd, volgt u de stappen in de [implementeren en uitvoeren van een cloudservice voor Apparaatbeheer simulatie oplossing](quickstart-device-simulation-deploy.md) Quick Start.

In dit artikel wordt ervan uitgegaan dat de naam van uw oplossingenaccelerator is **contoso-simulatie**. Vervang **contoso-simulatie** met de naam van uw oplossingenaccelerator als u de volgende stappen uitvoeren.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-a-consumer-group"></a>Een consumentengroep maken

U moet maken van een speciale klantengroep in uw IoT-hub aan stream telemetrie naar de Time Series Insights. Een gebeurtenisbron in Time Series Insights moet het exclusieve gebruik van een IoT Hub-consumentengroep hebben.

De volgende stappen uit voor het gebruik van de Azure CLI in de Azure Cloud Shell om de consumentengroep te maken:

1. De IoT-hub is een van de verschillende bronnen die zijn gemaakt tijdens de implementatie van de oplossingsversneller voor Apparaatsimulatie. Voer de volgende opdracht Zoek de naam van uw IoT-hub - Vergeet niet dat u de naam van uw oplossingenaccelerator:

    ```azurecli-interactive
    az resource list --resource-group contoso-simulation -o table
    ```

    De IoT-hub is de resource van het type **Microsoft.Devices/IotHubs**.

1. Toevoegen van een consumentengroep met de naam **devicesimulationtsi** naar de hub. Gebruik de naam van uw hub en solution accelerator in de volgende opdracht:

    ```azurecli-interactive
    az iot hub consumer-group create --hub-name contoso-simulation7d894 --name devicesimulationtsi --resource-group contoso-simulation
    ```

    U kunt nu de Azure Cloud Shell sluiten.

## <a name="create-a-new-time-series-insights-environment"></a>Maak een nieuwe Time Series Insights-omgeving

[Azure Time Series Insights](../../articles/time-series-insights/time-series-insights-overview.md) is een volledig beheerde service voor analyse, opslag en visualisatie voor het beheren van IoT-schaal time series-gegevens in de cloud. Een nieuwe Time Series Insights-omgeving maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Selecteer **een resource maken** > **Internet of Things** > **Time Series Insights**:

    ![Nieuwe Time Series Insights](./media/iot-accelerators-device-simulation-time-series-insights/new-time-series-insights.png)

1. Voor het maken van uw Time Series Insights-omgeving in dezelfde resourcegroep bevinden als uw oplossingenaccelerator, gebruikt u de waarden in de volgende tabel:

    | Instelling | Value |
    | ------- | ----- |
    | Naam van de omgeving | De volgende schermafbeelding wordt gebruikt voor de naam van de **Contoso-TSI**. Kies uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | Abonnement | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | Resourcegroep | **contoso-simulation**. Gebruik de naam van uw oplossingenaccelerator. |
    | Locatie | In dit voorbeeld wordt **VS-Oost**. In dezelfde regio als uw apparaat simulatie accelerator uw omgeving te maken. |
    | Sku |**S1** |
    | Capaciteit | **1** |

    ![Time Series Insights maken](./media/iot-accelerators-device-simulation-time-series-insights/new-time-series-insights-create.png)

    > [!NOTE]
    > Toevoegen van de Time Series Insights betekent omgeving aan dezelfde resourcegroep bevinden als de solution accelerator wordt verwijderd wanneer u de oplossingsversneller verwijdert.

1. Klik op **Create**. Het kan duren voordat een paar minuten voor de omgeving moet worden gemaakt.

## <a name="create-event-source"></a>Gebeurtenisbron maken

Maak een nieuwe gebeurtenisbron verbinding maken met uw IoT-hub. Gebruik de consumergroep die u in de vorige stappen hebt gemaakt. Een Time Series Insights-gebeurtenisbron vereist een speciale klantengroep niet wordt gebruikt door een andere service.

1. Navigeer in de Azure-portal naar uw Time Series-omgeving.

1. Aan de linkerkant, klikt u op **gebeurtenisbronnen**:

    ![Bronnen van gebeurtenissen weergeven](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-event-sources.png)

1. Klik op **toevoegen**:

    ![De bron van gebeurtenis toevoegen](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-event-sources-add.png)

1. Voor het configureren van uw IoT-hub als een nieuwe gebeurtenisbron, gebruikt u de waarden in de volgende tabel:

    | Instelling | Value |
    | ------- | ----- |
    | De naam van de bron van gebeurtenis | De volgende schermafbeelding wordt gebruikt voor de naam van de **contoso-iot-hub**. Gebruik uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | source | **IoT Hub** |
    | Importoptie | **IoT-Hub uit de beschikbare abonnementen gebruiken** |
    | Abonnements-id | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | De naam van de IOT-hub | **contoso-simulation7d894**. Gebruik de naam van uw IoT-hub in uw oplossingenaccelerator Apparaatsimulatie. |
    | Naam van IOT hub-beleid | **iothubowner** |
    | Beleidssleutel voor IOT hub | Dit veld wordt automatisch gevuld. |
    | IOT hub-consumentengroep | **devicesimulationtsi** |
    | Serialisatie-indeling voor gebeurtenissen | **JSON** |
    | Naam van de eigenschap timestamp | Leeg laten |

    ![Gebeurtenisbron maken](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-event-source-create.png)

1. Klik op **Create**.

> [!NOTE]
> U kunt [extra gebruikers toegang verleent](../../articles/time-series-insights/time-series-insights-data-access.md#grant-data-access) naar de Verkenner van Time Series Insights.

## <a name="start-a-simulation"></a>Een simulatie starten

Voordat u de Verkenner van Time Series Insights gebruikt, configureert u de oplossingsversneller voor Apparaatsimulatie om telemetrie te genereren. De volgende schermafbeelding ziet u een actieve simulatie met 10 Koelunit apparaten:

![Apparaatsimulatie uitgevoerd](./media/iot-accelerators-device-simulation-time-series-insights/running-simulation.png)

## <a name="time-series-insights-explorer"></a>Verkenner van Time Series Insights

De Verkenner van Time Series Insights is een web-app die u gebruiken kunt voor het visualiseren van uw telemetrie.

1. Selecteer in de Azure-portal, de Time Series Insights **overzicht** tabblad.

1. Als u wilt de Time Series Insights explorer web-app openen, klikt u op **gaat u naar de omgeving**:

    ![Verkenner van Time Series Insights](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-environment.png)

1. Selecteer in het deelvenster tijd selecteren **afgelopen 30 minuten** van de snelle tijden en klik op **zoeken**:

    ![Time Series Insights explorer zoeken](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-search-time.png)

1. Selecteer in het deelvenster voorwaarden aan de linkerkant **temperatuur** als de **meting** en **iothub-verbinding-apparaat-id** als de **splitsen door** waarde:

    ![Time Series Insights explorer query](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-query1.png)

1. Met de rechtermuisknop op de grafiek en selecteer **gebeurtenissen onderzoeken**:

    ![Gebeurtenissen van Time Series Insights explorer](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-explore-events.png)

1. In een raster ziet u de gebeurtenisgegevens:

    ![Time Series Insights explorer tabel](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-table.png)

1. Klik op de knop perspectief weergeven:

    ![Time Series Insights explorer perspectief](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-explorer-perspective.png)

1. Klik op **+** een nieuwe query toevoegen aan het perspectief:

    ![Verkenner van Time Series Insights Query toevoegen](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-new-query.png)

1. Selecteer **afgelopen 30 minuten** als de tijdsspanne **vochtigheid** als de **meting**, en **iothub-verbinding-apparaat-id** als de **Splitsen door** waarde:

    ![Time Series Insights explorer query](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-query2.png)

1. Klik op de knop perspectief weergave om uw apparaat telemetrie-dashboard weer te geven:

    ![Time Series Insights explorer-dashboard](./media/iot-accelerators-device-simulation-time-series-insights/time-series-insights-dashboard.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent te verder te verkennen, laat u de oplossingsversneller geïmplementeerd.

Als u de oplossingsversneller niet meer nodig hebt, verwijderd uit de [ingericht oplossingen](https://www.azureiotsolutions.com/Accelerators#dashboard) pagina door te selecteren en vervolgens te klikken op **oplossing verwijderen**.

Als u de Time Series Insights-omgeving toegevoegd aan de oplossingsversnellers resourcegroep, wordt het automatisch verwijderd wanneer u de oplossingsversneller verwijderen. Anders moet u handmatig de Time Series Insights-omgeving verwijderen vanuit de Azure-portal.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het verkennen en query uitvoeren op gegevens in de Verkenner van Time Series Insights, [Azure Time Series Insights explorer](../time-series-insights/time-series-insights-explorer.md).
