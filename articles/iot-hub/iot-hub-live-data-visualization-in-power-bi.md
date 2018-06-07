---
title: Realtime gegevensvisualisatie van sensorgegevens uit Azure IoT Hub – Power BI | Microsoft Docs
description: Gebruik Power BI om temperatuur en vochtigheid gegevens die worden verzameld van de sensor en verzonden naar uw Azure-IoT-hub te visualiseren.
author: rangv
manager: ''
keywords: realtime gegevensvisualisatie, live gegevensvisualisatie, sensor gegevensvisualisatie
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 4/11/2018
ms.author: rangv
ms.openlocfilehash: a3c54fe635fe0f8988c321684a815e9896922587
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34634342"
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a>Realtime-sensorgegevens uit Azure IoT Hub met Power BI visualiseren

![End-to-end-diagram](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a>Wat u leert

U leert hoe voor het visualiseren van realtime-sensorgegevens die uw Azure-IoT-hub ontvangt door Power BI. Als u proberen de gegevens in uw IoT-hub met Web-Apps wilt, raadpleegt u [gebruik Azure Web Apps voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

## <a name="what-you-do"></a>Wat u doet

- Bereid uw IoT-hub voor toegang tot gegevens door een consumergroep toe te voegen.
- Maken, configureren en uitvoeren van een Stream Analytics-taak voor overdracht van gegevens uit uw IoT-hub aan uw Power BI-account.
- Maken en publiceren van een Power BI-rapport om de gegevens te visualiseren.

## <a name="what-you-need"></a>Wat u nodig hebt

- Zelfstudie [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) voltooid die wordt ingegaan op de volgende vereisten:
  - Een actief Azure-abonnement.
  - Een Azure-IoT-hub in uw abonnement.
  - Een clienttoepassing dat berichten naar uw Azure-IoT-hub verzendt.
- Een Power BI-account. ([Probeer Power BI gratis](https://powerbi.microsoft.com/))

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Maken, configureren en uitvoeren van een Stream Analytics-taak

### <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

1. Klik [in de Azure Portal](https://portal.azure.com) op **Een resource maken** > **Internet of Things** > **Stream Analytics-taak**.
1. Voer de volgende informatie in voor de taak.

   **Taaknaam**: de naam van de taak. De naam moet wereldwijd uniek zijn.

   **Resourcegroep**: gebruik dezelfde resourcegroep die gebruikmaakt van uw IoT-hub.

   **Locatie**: gebruik dezelfde locatie als uw resourcegroep.

   **Vastmaken aan dashboard**: vink deze optie aan voor eenvoudige toegang tot uw IoT-hub vanuit het dashboard.

   ![Een Stream Analytics-taak maken in Azure](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. Klik op **Create**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Een invoer aan de Stream Analytics-taak toevoegen

1. Open de Stream Analytics-taak.
1. Klik onder **Taaktopologie** op **Invoer**.
1. In de **invoer** deelvenster, klikt u op **toevoegen**, en voer de volgende informatie:

   **Invoeralias**: de alias die uniek zijn voor de invoer.

   **Bron**: Selecteer **IoT-hub**.

   **Consumergroep**: Selecteer de consumergroep die u zojuist hebt gemaakt.
1. Klik op **Create**.

   ![Een invoer toevoegen aan Stream Analytics-taak in Azure](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a>Een uitvoer aan de Stream Analytics-taak toevoegen

1. Klik onder **Taaktopologie** op **Uitvoer**.
1. In de **uitvoer** deelvenster, klikt u op **toevoegen**, en voer de volgende informatie:

   **Uitvoeralias**: de alias die uniek is voor de uitvoer.

   **Sink**: Selecteer **Power BI**.
1. Klik op **autoriseren**, en vervolgens meldt u zich bij uw Power BI-account.
1. Na autorisatie, voer de volgende informatie:

   **Werkruimte groep**: Selecteer de groep doelwerkruimte.

   **Naam van DataSet**: Voer een gegevenssetnaam.

   **Tabelnaam**: Voer een tabelnaam.
1. Klik op **Create**.

   ![Uitvoer toevoegen aan een Stream Analytics-taak in Azure](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a>De query van de Stream Analytics-taak configureren

1. Klik onder **Taaktopologie** op **Query**.
1. Vervang `[YourInputAlias]` door de invoeralias van de taak.
1. Vervang `[YourOutputAlias]` door de uitvoeralias van de taak.
1. Klik op **Opslaan**.

   ![Een query toevoegen aan een Stream Analytics-taak in Azure](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a>Voer de Stream Analytics-taak uit

Klik in de Stream Analytics-taak op **Start** > **Nu** > **Start**. Zodra de taak kan worden gestart, wordt de taakstatus veranderd van **Gestopt** naar **In uitvoering**.

![Een Stream Analytics-taak uitgevoerd in Azure](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a>Maken en publiceren van een Power BI-rapport om de gegevens te visualiseren

1. Controleer dat de voorbeeldtoepassing op uw apparaat wordt uitgevoerd. Als u niet het geval is, raadpleegt u de zelfstudies onder [instellen van uw apparaat](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).
1. Aanmelden bij uw [Power BI](https://powerbi.microsoft.com/en-us/) account.
1. Ga naar de werkruimte voor groep die u hebt ingesteld toen u de uitvoer voor de Stream Analytics-taak gemaakt.
1. Klik op **Streaming gegevenssets**.

   U zou de vermelde gegevensset moeten zien die u hebt opgegeven toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt.
1. Klik onder **ACTION** op het eerste pictogram om een rapport te maken.

   ![Een Microsoft Power BI-rapport maken](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. Maak een lijndiagram om in realtime de temperatuur gedurende een bepaalde periode weer te geven.
   1. Voeg een lijndiagram op de pagina rapport maken.
   1. Klap in het deelvenster **Velden** de tabel uit die u hebt opgegeven toen u de uitvoer voor de Stream Analytics-taak hebt gemaakt.
   1. Sleep **EventEnqueuedUtcTime** naar **As** in het deelvenster **Visualisaties**.
   1. Sleep **Temperatuur** naar **Waarden**.

      Nu wordt een lijndiagram gemaakt. De x-as geeft de datum en tijd in UTC-tijdzone aan. De y-as geeft de temperatuur van de sensor aan.

      ![Een lijndiagram voor temperatuur toevoegen aan een Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. Maak een ander lijndiagram om in realtime de vochtigheid gedurende een bepaalde periode weer te geven. U doet dit door dezelfde stappen hierboven en plaats **EventEnqueuedUtcTime** op de x-as en **vochtigheid** op de y-as.

   ![Een lijndiagram voor vochtigheid toevoegen aan een Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. Klik op **Opslaan** om het rapport op te slaan.
1. Klik op **bestand** > **publiceren op web**.
1. Klik op **invoegcode maken**, en klik vervolgens op **publiceren**.

U hebt de rapportkoppeling hebt opgegeven dat u met iedereen voor toegang tot rapporten en een codefragment delen kunt aan het rapport integreren in uw blog of website.

![Publiceren van een Microsoft Power BI-rapport](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

Microsoft biedt ook de [mobiele apps van Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) voor het weergeven en interactie met uw Power BI-dashboards en rapporten op je mobiele apparaat.

## <a name="next-steps"></a>Volgende stappen

U hebt Power BI is gebruikt voor het visualiseren van realtime-sensorgegevens uit uw Azure-IoT-hub.
Er is een andere manier om gegevens uit Azure IoT Hub te visualiseren. Zie [gebruik Azure Web Apps voor het visualiseren van realtime-sensorgegevens uit Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
