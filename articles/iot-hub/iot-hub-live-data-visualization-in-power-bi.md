---
title: Realtime gegevensvisualisatie van sensorgegevens uit Azure IoT Hub-Power BI | Microsoft Docs
description: Power BI gebruiken om temperatuur en vochtigheid gegevens die worden verzameld van de sensor en verzonden naar uw Azure-IoT-hub te visualiseren.
author: robinsh
keywords: realtime gegevensvisualisatie, live gegevensvisualisatie, gegevensvisualisatie sensor
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 6/06/2019
ms.author: robinsh
ms.openlocfilehash: 7deb1b501d30c8af0cb190f4722d46435afa9b8e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065802"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Real-time sensorgegevens uit Azure IoT Hub met behulp van Power BI visualiseren

![Diagram voor end-to-end](./media/iot-hub-live-data-visualization-in-power-bi/1_end-to-end-diagram.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Wat u leert

U informatie over het visualiseren van realtime-sensorgegevens die uw Azure-IoT-hub ontvangt via Power BI. Als u proberen wilt te visualiseren van de gegevens in uw IoT-hub met een web-app, raadpleegt u [een web-app gebruiken voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Wat u allemaal doen

* Bereid u voor uw IoT-hub voor toegang tot gegevens door toe te voegen een consumergroep.

* Maken, configureren en uitvoeren van een Stream Analytics-taak voor overdracht van gegevens van uw IoT-hub aan uw Power BI-account.

* Maken en publiceren van een Power BI-rapport om de gegevens te visualiseren.

## <a name="what-you-need"></a>Wat u nodig hebt

* Voltooi de [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md) zelfstudie of een van de apparaat-zelfstudies, bijvoorbeeld [Raspberry Pi met node.js](iot-hub-raspberry-pi-kit-node-get-started.md). Deze artikelen hebben betrekking op de volgende vereisten:
  
  * Een actief Azure-abonnement.
  * Een Azure IoT-hub in uw abonnement.
  * Een clienttoepassing die berichten naar uw Azure-IoT-hub verzendt.

* Een Power BI-account. ([Power BI gratis proberen](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Maken, configureren en uitvoeren van een Stream Analytics-taak

Laten we beginnen met het maken van een Stream Analytics-taak. Nadat u de taak hebt gemaakt, definieert u de invoer, uitvoer en de query die wordt gebruikt om de gegevens te halen.

### <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

1. In de [Azure-portal](https://portal.azure.com), selecteer **een resource maken** > **Internet of Things** > **Stream Analytics-taak**.

2. Voer de volgende informatie in voor de taak.

   **Taaknaam**: De naam van de taak. De naam moet wereldwijd uniek zijn.

   **Resourcegroep**: Gebruik dezelfde resourcegroep bevinden die gebruikmaakt van uw IoT-hub.

   **Locatie**: Gebruik de dezelfde locatie als uw resourcegroep.

   ![Een Stream Analytics-taak maken in Azure](./media/iot-hub-live-data-visualization-in-power-bi/create-stream-analytics-job-azure.png)

3. Selecteer **Maken**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Een invoer aan de Stream Analytics-taak toevoegen

1. Open de Stream Analytics-taak.

2. Onder **Taaktopologie**, selecteer **invoer**.

3. In de **invoer** venster **Stroominvoer toevoegen**en selecteer vervolgens **IoT-Hub** uit de vervolgkeuzelijst. Voer de volgende informatie op het nieuwe deelvenster van de invoer:

   **Invoeralias**: Voer een unieke alias voor de invoer.

   **IoT Hub bieden van uw abonnement**: Selecteer dit keuzerondje.

   **Abonnement**: Selecteer het Azure-abonnement u voor deze zelfstudie.

   **IoT Hub**: Selecteer de IoT-Hub die u voor deze zelfstudie.

   **Eindpunt**: Selecteer **Berichten**.

   **Naam van het gedeelde toegangsbeleid**: Selecteer de naam van het gedeelde toegangsbeleid dat u wilt dat de Stream Analytics-taak moet worden gebruikt voor uw IoT-hub. Voor deze zelfstudie, kunt u *service*. De *service* beleid standaard op de nieuwe IoT-hubs wordt gemaakt en een machtiging verleend voor het verzenden en ontvangen op cloud de eindpunten die worden weergegeven door de IoT-hub. Zie voor meer informatie, [toegangsbeheer en machtigingen](iot-hub-devguide-security.md#access-control-and-permissions).

   **Beleid voor gedeelde toegangssleutel**: Dit veld wordt automatisch gevuld op basis van uw selectie voor de naam van het beleid voor gedeelde toegang.

   **Consumentengroep**: Selecteer de consumergroep die u eerder hebt gemaakt.

   Laat andere velden op de standaardwaarden staan.

   ![Invoer voor een Stream Analytics-taak toevoegen in Azure](./media/iot-hub-live-data-visualization-in-power-bi/add-input-to-stream-analytics-job-azure.png)

4. Selecteer **Opslaan**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Een uitvoer aan de Stream Analytics-taak toevoegen

1. Onder **Taaktopologie**, selecteer **uitvoer**.

2. In de **uitvoer** venster **toevoegen** en **Power BI**.

3. Op de **Power BI - nieuwe uitvoer** venster **autoriseren** en volg de aanwijzingen voor het aanmelden bij uw Power BI-account.

4. Nadat u zich hebt aangemeld bij Power BI, voert u de volgende informatie:

   **Uitvoeralias**: Een unieke-alias voor de uitvoer.

   **Werkruimte groep**: Selecteer de groepswerkruimte van uw doel.

   **Naam van de gegevensset**: Voer een naam van de gegevensset.

   **Tabelnaam**: Voer een tabelnaam in.

   ![Uitvoer toevoegen aan een Stream Analytics-taak in Azure](./media/iot-hub-live-data-visualization-in-power-bi/add-output-to-stream-analytics-job-azure.png)

5. Selecteer **Opslaan**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>De query van de Stream Analytics-taak configureren

1. Selecteer **Query** onder **Taaktopologie**.

2. Vervang `[YourInputAlias]` door de invoeralias van de taak.

3. Vervang `[YourOutputAlias]` door de uitvoeralias van de taak.

   ![Een query toevoegen aan een Stream Analytics-taak in Azure](./media/iot-hub-live-data-visualization-in-power-bi/add-query-stream-analytics-job-azure.png)

4. Selecteer **Opslaan**.

### <a name="run-the-stream-analytics-job"></a>De Stream Analytics-taak uitvoeren

Selecteer in de Stream Analytics-taak, **overzicht**en selecteer vervolgens **Start** > **nu** > **Start**. Zodra de taak kan worden gestart, wordt de taakstatus veranderd van **Gestopt** naar **In uitvoering**.

![Een Stream Analytics-taak uitvoeren in Azure](./media/iot-hub-live-data-visualization-in-power-bi/run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Maken en publiceren van een Power BI-rapport om de gegevens te visualiseren

1. Controleer of dat de voorbeeldtoepassing wordt uitgevoerd op uw apparaat. Als u niet het geval is, kunt u verwijzen naar de zelfstudies onder [instellen van uw apparaat](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).

2. Meld u aan bij uw [Power BI](https://powerbi.microsoft.com/en-us/)-account.

3. Selecteer de werkruimte die u hebt gebruikt, **mijn werkruimte**.

4. Selecteer **gegevenssets**.

   U ziet de gegevensset die u hebt opgegeven tijdens het maken van de uitvoer voor de Stream Analytics-taak.

5. Voor de gegevensset die u hebt gemaakt, selecteert u **rapport toevoegen** (het eerste pictogram aan de rechterkant van de naam van de gegevensset).

   ![Een Microsoft Power BI-rapport maken](./media/iot-hub-live-data-visualization-in-power-bi/start-power-bi.png)

6. Maak een lijndiagram om in realtime de temperatuur gedurende een bepaalde periode weer te geven.

   1. Op de **visualisaties** deelvenster van de rapportpagina maken, selecteer het pictogram lijndiagram om toe te voegen een lijndiagram.

   2. Klap in het deelvenster **Velden** de tabel uit die u hebt opgegeven toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt.

   3. Sleep **EventEnqueuedUtcTime** naar **As** in het deelvenster **Visualisaties**.

   4. Sleep **Temperatuur** naar **Waarden**.

      Er wordt een lijndiagram gemaakt. De x-as geeft de datum en tijd in UTC-tijdzone aan. De y-as geeft de temperatuur van de sensor aan.

      ![Een lijndiagram voor temperatuur toevoegen aan een Microsoft Power BI-rapport](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-add-temp.png)

7. Maak een ander lijndiagram om in realtime de vochtigheid gedurende een bepaalde periode weer te geven. Volg de bovenstaande dezelfde stappen om dit te doen, en plaats **EventEnqueuedUtcTime** op de x-as en **vochtigheid** op de y-as.

   ![Een lijndiagram voor de vochtigheid toevoegen aan een Microsoft Power BI-rapport](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-add-humidity.png)

8. Selecteer **opslaan** het rapport wilt opslaan.

9. Selecteer **rapporten** in het linkerdeelvenster en selecteer vervolgens het rapport dat u zojuist hebt gemaakt.

10. Selecteer **bestand** > **publiceren op Internet**.

11. Selecteer **invoegcode maken**, en selecteer vervolgens **publiceren**.

U krijgt de koppeling naar het rapport dat u met iedereen voor toegang tot rapporten delen kunt en een stukje code dat u kunt het rapport integreren in uw blog of website.

![Een Microsoft Power BI-rapport publiceren](./media/iot-hub-live-data-visualization-in-power-bi/power-bi-publish.png)

Microsoft biedt ook de [mobiele Power BI-apps](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) voor het weergeven en interactie met uw Power BI-dashboards en rapporten op uw mobiele apparaat.

## <a name="next-steps"></a>Volgende stappen

U hebt Power BI is gebruikt voor het visualiseren van realtime-sensorgegevens uit uw Azure-IoT-hub.

Zie voor een andere manier om gegevens van Azure IoT Hub te visualiseren, [een web-app gebruiken voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
