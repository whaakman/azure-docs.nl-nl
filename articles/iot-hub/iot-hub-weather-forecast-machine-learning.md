---
title: Weersvoorspelling met behulp van Azure Machine Learning met gegevens uit IoT Hub | Microsoft Docs
description: Gebruik Azure Machine Learning om te voorspellen van de kans op regen op basis van de temperatuur en vochtigheid gegevens die uw IoT-hub worden verzameld van een sensor.
author: rangv
manager: ''
keywords: het weerbericht machine learning
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 04/11/2018
ms.author: rangv
ms.openlocfilehash: 7fdd2a96044acdae223243d751bfcffb7a99da78
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57534256"
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Weersvoorspelling met behulp van de sensorgegevens uit IoT hub in Azure Machine Learning

![Diagram voor end-to-end](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Machine learning is een techniek van gegevenswetenschap die computers leren uit bestaande gegevens om te voorspellen toekomstig gedrag, resultaten en trends helpt. Azure Machine Learning is een cloudservice voor predictive analytics waarmee u snel voorspellende modellen kunt maken en implementeren als analytics-oplossingen.

## <a name="what-you-learn"></a>Wat u leert

Leert u hoe u Azure Machine Learning gebruiken om u te weerbericht prognose (kans van regen) met behulp van de temperatuur en vochtigheid gegevens uit uw Azure-IoT-hub. De kans van regen is de uitvoer van een voorspellingsmodel voorbereide weer. Het model is gebaseerd op historische gegevens te voorspellen op basis van de temperatuur en vochtigheid regen kans.

## <a name="what-you-do"></a>Wat u allemaal doen

- Het model van de voorspelling weer als een webservice implementeren.
- Bereid u voor uw IoT-hub voor toegang tot gegevens door toe te voegen een consumergroep.
- Een Stream Analytics-taak maken en configureren van de taak is:
  - Temperatuur en vochtigheid gegevens lezen uit uw IoT-hub.
  - Roep de webservice voor de neerslagkans.
  - Sla het resultaat op in een Azure blob-opslag.
- Microsoft Azure Storage Explorer gebruiken om de weersvoorspelling weer te geven.

## <a name="what-you-need"></a>Wat u nodig hebt

- Zelfstudie [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) voltooid die voorziet in de volgende vereisten:
  - Een actief Azure-abonnement.
  - Een Azure IoT-hub in uw abonnement.
  - Een clienttoepassing die berichten naar uw Azure-IoT-hub verzendt.
- Een Azure Machine Learning Studio-account. ([Probeer Machine Learning Studio gratis](https://studio.azureml.net/)).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Het model van de voorspelling weer als een webservice implementeren

1. Ga naar de [weer voorspelling modelpagina](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).
1. Klik op **Open in Studio** in Microsoft Azure Machine Learning Studio.
   ![Open de weersomstandigheden voorspelling model-pagina in Cortana Intelligence-galerie](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)
1. Klik op **uitvoeren** voor het valideren van de stappen in het model. Deze stap kan 2 minuten duren.
   ![Weather voorspellingsmodel gemaakt in Azure Machine Learning Studio openen](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Klik op **WEBSERVICE instellen** > **voorspellende webservice**.
   ![Het weer voorspellingsmodel in Azure Machine Learning Studio implementeren](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Sleep in het diagram wordt de **Web service invoer** module ergens in de buurt van de **Score Model** module.
1. Verbinding maken met de **Web service invoer** module aan de **Score Model** module.
   ![Verbinding maken met twee modules in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)
1. Klik op **uitvoeren** voor het valideren van de stappen in het model.
1. Klik op **WEBSERVICE implementeren** het model als een webservice te implementeren.
1. Op het dashboard van het model, downloaden de **Excel 2010 of ouder werkmap** voor **aanvraag/antwoord**.

   > [!Note]
   > Zorg ervoor dat u downloadt de **Excel 2010 of ouder werkmap** , zelfs als u een nieuwere versie van Excel op uw computer worden uitgevoerd.

   ![Download het Excel voor de REQUEST RESPONSE-eindpunt](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. Open de Excel-werkmap, noteer de **WEBSERVICE-URL** en **TOEGANGSSLEUTEL**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Maken, configureren en uitvoeren van een Stream Analytics-taak

### <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

1. Klik [in de Azure Portal](https://portal.azure.com/) op **Een resource maken** > **Internet of Things** > **Stream Analytics-taak**.
1. Voer de volgende informatie in voor de taak.

   **Taaknaam**: De naam van de taak. De naam moet wereldwijd uniek zijn.

   **Resourcegroep**: Gebruik dezelfde resourcegroep bevinden die gebruikmaakt van uw IoT-hub.

   **Locatie**: Gebruik de dezelfde locatie als uw resourcegroep.

   **Vastmaken aan dashboard**: Schakel deze optie voor eenvoudige toegang voor uw IoT-hub vanuit het dashboard.

   ![Een Stream Analytics-taak maken in Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Klik op **Create**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Een invoer aan de Stream Analytics-taak toevoegen

1. Open de Stream Analytics-taak.
1. Klik onder **Taaktopologie** op **Invoer**.
1. In de **invoer** deelvenster, klikt u op **toevoegen**, en voer de volgende informatie:

   **Invoeralias**: De unieke alias voor de invoer.

   **Bron**: Selecteer **IoT-hub**.

   **Consumentengroep**: Selecteer de consumergroep die u hebt gemaakt.

   ![Invoer voor de Stream Analytics-taak toevoegen in Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Klik op **Create**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Een uitvoer aan de Stream Analytics-taak toevoegen

1. Klik onder **Taaktopologie** op **Uitvoer**.
1. In de **uitvoer** deelvenster, klikt u op **toevoegen**, en voer de volgende informatie:

   **Uitvoeralias**: De alias die uniek is voor de uitvoer.

   **Sink**: Selecteer **Blob Storage**.

   **Opslagaccount**: Het opslagaccount voor de blob-opslag. U kunt een storage-account maken of gebruik een bestaande resourcegroep.

   **Container**: De container waar de blob is opgeslagen. U kunt een container maken of gebruik een bestaande resourcegroep.

   **Serialisatie-indeling voor gebeurtenissen**: Selecteer **CSV**.

   ![Uitvoer toevoegen aan de Stream Analytics-taak in Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Klik op **Create**.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Een functie toevoegen aan de Stream Analytics-taak voor het aanroepen van de door u geïmplementeerde webservice

1. Onder **Taaktopologie**, klikt u op **functies** > **toevoegen**.
1. Voer de volgende informatie in:

   **Functie Alias**: Voer `machinelearning` in.

   **Functietype**: Selecteer **Azure ML**.

   **Importoptie**: Selecteer **importeren vanuit een ander abonnement**.

   **URL**: Voer de URL van de WEBSERVICE die u hebt genoteerd uit de Excel-werkmap.

   **sleutel**: Voer de TOEGANGSSLEUTEL die u hebt genoteerd uit de Excel-werkmap.

   ![Een functie toevoegen aan de Stream Analytics-taak in Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Klik op **Create**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>De query van de Stream Analytics-taak configureren

1. Klik onder **Taaktopologie** op **Query**.
1. Vervang de bestaande code door de volgende code:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Vervang `[YourInputAlias]` door de invoeralias van de taak.

   Vervang `[YourOutputAlias]` door de uitvoeralias van de taak.

1. Klik op **Opslaan**.

### <a name="run-the-stream-analytics-job"></a>De Stream Analytics-taak uitvoeren

Klik in de Stream Analytics-taak op **Start** > **Nu** > **Start**. Zodra de taak kan worden gestart, wordt de taakstatus veranderd van **Gestopt** naar **In uitvoering**.

![De Stream Analytics-taak uitvoeren](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Microsoft Azure Storage Explorer gebruiken om de weersvoorspelling weer te geven

Voer de clienttoepassing om te beginnen met het verzamelen van en temperatuur en vochtigheid gegevens te verzenden naar uw IoT-hub uit. Voor elk bericht dat uw IoT-hub ontvangt, wordt de Stream Analytics-taak de weersvoorspelling-webservice om de kans op regen produceren. Het resultaat wordt vervolgens opgeslagen in de Azure blob-opslag. Azure Storage Explorer is een hulpprogramma dat u gebruiken kunt om het resultaat weer te geven.

1. [Download en installeer Microsoft Azure Storage Explorer](https://storageexplorer.com/).
1. Open Azure Storage Explorer.
1. Meld u aan bij uw Azure-account.
1. Selecteer uw abonnement.
1. Klik op uw abonnement > **Opslagaccounts** > uw storage-account > **Blobcontainers** > uw container.
1. Open een CSV-bestand als het resultaat wilt weergeven. De laatste kolom registreert de kans op regen.

   ![Resultaat van de weersverwachting met Azure Machine Learning ophalen](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a>Samenvatting

U hebt Azure Machine Learning is gebruikt voor het produceren van de kans op regen op basis van de temperatuur en vochtigheid gegevens die uw IoT-hub ontvangt.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]