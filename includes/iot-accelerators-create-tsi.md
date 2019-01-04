---
title: bestand opnemen
description: bestand opnemen
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/20/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: ab3d4cbe8a200e91d02177e49446065cd9c228df
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53609069"
---
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

[Azure Time Series Insights](../articles/time-series-insights/time-series-insights-overview.md) is een volledig beheerde service voor analyse, opslag en visualisatie voor het beheren van IoT-schaal time series-gegevens in de cloud. Een nieuwe Time Series Insights-omgeving maken:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Selecteer **een resource maken** > **Internet of Things** > **Time Series Insights**:

    ![Nieuwe Time Series Insights](./media/iot-accelerators-create-tsi/new-time-series-insights.png)

1. Voor het maken van uw Time Series Insights-omgeving in dezelfde resourcegroep bevinden als uw oplossingenaccelerator, gebruikt u de waarden in de volgende tabel:

    | Instelling | Waarde |
    | ------- | ----- |
    | Naam van de omgeving | De volgende schermafbeelding wordt gebruikt voor de naam van de **Contoso-TSI**. Kies uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | Abonnement | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | Resourcegroep | **Contoso-simulatie**. Gebruik de naam van uw oplossingenaccelerator. |
    | Locatie | In dit voorbeeld wordt **VS-Oost**. In dezelfde regio als uw apparaat simulatie accelerator uw omgeving te maken. |
    | Sku |**S1** |
    | Capaciteit | **1** |

    ![Time Series Insights maken](./media/iot-accelerators-create-tsi/new-time-series-insights-create.png)

    > [!NOTE]
    > Toevoegen van de Time Series Insights betekent omgeving aan dezelfde resourcegroep bevinden als de solution accelerator wordt verwijderd wanneer u de oplossingsversneller verwijdert.

1. Klik op **Create**. Het kan duren voordat een paar minuten voor de omgeving moet worden gemaakt.

## <a name="create-event-source"></a>Gebeurtenisbron maken

Maak een nieuwe gebeurtenisbron verbinding maken met uw IoT-hub. Gebruik de consumergroep die u in de vorige stappen hebt gemaakt. Een Time Series Insights-gebeurtenisbron vereist een speciale klantengroep niet wordt gebruikt door een andere service.

1. Navigeer in de Azure-portal naar uw Time Series-omgeving.

1. Aan de linkerkant, klikt u op **gebeurtenisbronnen**:

    ![Bronnen van gebeurtenissen weergeven](./media/iot-accelerators-create-tsi/time-series-insights-event-sources.png)

1. Klik op **toevoegen**:

    ![De bron van gebeurtenis toevoegen](./media/iot-accelerators-create-tsi/time-series-insights-event-sources-add.png)

1. Voor het configureren van uw IoT-hub als een nieuwe gebeurtenisbron, gebruikt u de waarden in de volgende tabel:

    | Instelling | Waarde |
    | ------- | ----- |
    | De naam van de bron van gebeurtenis | De volgende schermafbeelding wordt gebruikt voor de naam van de **contoso-iot-hub**. Gebruik uw eigen unieke naam wanneer u deze stap hebt voltooid. |
    | Bron | **IoT Hub** |
    | Importoptie | **IoT-Hub uit de beschikbare abonnementen gebruiken** |
    | Abonnements-id | Selecteer uw Azure-abonnement in de vervolgkeuzelijst. |
    | De naam van de IOT-hub | **Contoso-simulation7d894**. Gebruik de naam van uw IoT-hub in uw oplossingenaccelerator Apparaatsimulatie. |
    | Naam van IOT hub-beleid | **iothubowner** |
    | Beleidssleutel voor IOT hub | Dit veld wordt automatisch gevuld. |
    | IOT hub-consumentengroep | **devicesimulationtsi** |
    | Serialisatie-indeling voor gebeurtenissen | **JSON** |
    | Naam van de eigenschap timestamp | Leeg laten |

    ![Gebeurtenisbron maken](./media/iot-accelerators-create-tsi/time-series-insights-event-source-create.png)

1. Klik op **Create**.

> [!NOTE]
> U kunt [extra gebruikers toegang verleent](../articles/time-series-insights/time-series-insights-data-access.md#grant-data-access) naar de Verkenner van Time Series Insights.