---
title: Weer prognose met Azure Machine Learning met gegevens uit IoT Hub | Microsoft Docs
description: Gebruik Azure Machine Learning de kans op regen voorspellen op basis van de temperatuur en vochtigheid die uw IoT-hub uit een sensor verzamelt.
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: prognose weer machine learning
ms.assetid: 8ba7d9e7-699c-4448-b353-0f3e1429d198
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 50ae54b9476c49b80236e295c0bf244df8236cff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a>Weer voorspellen met behulp van de sensorgegevens uit uw IoT-hub in Azure Machine Learning

![End-to-end-diagram](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

Machine learning is een techniek voor gegevenswetenschap waarmee computers leren van bestaande gegevens te voorspellen van toekomstige gedrag, resultaten en trends. Azure Machine Learning is een cloudservice voor predictive analytics waarmee u snel voorspellende modellen kunt maken en implementeren als analytics-oplossingen.

## <a name="what-you-learn"></a>Wat u leert

U leert hoe u met Azure Machine Learning weerbericht prognose (kans regen) met behulp van de temperatuur en vochtigheid gegevens van uw Azure-IoT-hub. De kans op regen is de uitvoer van een model van de voorspelling voorbereide weer. Het model is gebaseerd op historische gegevens te voorspellen op basis van temperatuur en vochtigheid regen kans.

## <a name="what-you-do"></a>Wat u doet

- Het model van de voorspelling weer als een webservice implementeren.
- Bereid uw IoT-hub voor toegang tot gegevens door een consumergroep toe te voegen.
- Een Stream Analytics-taak maken en configureren van de taak is:
  - Temperatuur en vochtigheid gegevens lezen uit uw IoT-hub.
  - De webservice om de kans op regen aanroept.
  - Het resultaat opslaan naar een Azure blob storage.
- Microsoft Azure Storage Explorer gebruiken om weer te geven van de prognose weer.

## <a name="what-you-need"></a>Wat u nodig hebt

- Zelfstudie [instellen van uw apparaat](iot-hub-raspberry-pi-kit-node-get-started.md) voltooid die wordt ingegaan op de volgende vereisten:
  - Een actief Azure-abonnement.
  - Een Azure-IoT-hub in uw abonnement.
  - Een clienttoepassing dat berichten naar uw Azure-IoT-hub verzendt.
- Een Azure Machine Learning Studio-account. ([Machine Learning Studio gratis proberen](https://studio.azureml.net/)).

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a>Het model van de voorspelling weer als een webservice implementeren

1. Ga naar de [weer voorspelling modelpagina](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).
1. Klik op **openen in Studio** in Microsoft Azure Machine Learning Studio.
   ![Open de weer voorspelling modelpagina in Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)
1. Klik op **uitvoeren** valideren van de stappen in het model. Deze stap kan twee minuten duren.
   ![Open het model van de voorspelling weer in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)
1. Klik op **instellen van WEBSERVICE** > **voorspellende webservice**.
   ![Het model van de voorspelling weer in Azure Machine Learning Studio implementeren](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)
1. In het diagram, sleept u de **Web service invoer** module ergens in de buurt van de **Score Model** module.
1. Verbinding maken met de **Web service invoer** module die u wilt de **Score Model** module.
   ![Verbinding maken met twee modules in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)
1. Klik op **uitvoeren** valideren van de stappen in het model.
1. Klik op **WEBSERVICE implementeren** voor het implementeren van het model als een webservice.
1. Op het dashboard van het model, downloaden de **Excel 2010 of ouder werkmap** voor **aanvraag/antwoord**.

   > [!Note]
   > Zorg ervoor dat u downloadt de **Excel 2010 of ouder werkmap** zelfs als u een nieuwere versie van Excel worden uitgevoerd op uw computer.

   ![De Excel voor het eindpunt REQUEST RESPONSE downloaden](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. Open de Excel-werkmap, noteert u de **URL van WEBSERVICE** en **TOEGANGSSLEUTEL**.

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a>Maken, configureren en uitvoeren van een Stream Analytics-taak

### <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

1. In de [Azure-portal](https://ms.portal.azure.com/), klikt u op **nieuw** > **Internet der dingen** > **Stream Analytics-taak**.
1. Voer de volgende informatie voor de taak.

   **Taaknaam**: de naam van de taak. De naam moet wereldwijd uniek zijn.

   **Resourcegroep**: gebruik dezelfde resourcegroep die gebruikmaakt van uw IoT-hub.

   **Locatie**: gebruik dezelfde locatie als uw resourcegroep.

   **Vastmaken aan dashboard**: Schakel deze optie voor eenvoudige toegang naar uw IoT-hub vanuit het dashboard.

   ![Een Stream Analytics-taak maken in Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. Klik op **Create**.

### <a name="add-an-input-to-the-stream-analytics-job"></a>Invoer voor de Stream Analytics-taak toevoegen

1. Open de Stream Analytics-taak.
1. Onder **taak topologie**, klikt u op **invoer**.
1. In de **invoer** deelvenster, klikt u op **toevoegen**, en voer de volgende informatie:

   **Invoeralias**: de alias die uniek zijn voor de invoer.

   **Bron**: Selecteer **IoT-hub**.

   **Consumergroep**: Selecteer de consumergroep die u hebt gemaakt.

   ![Invoer voor de Stream Analytics-taak toevoegen in Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. Klik op **Create**.

### <a name="add-an-output-to-the-stream-analytics-job"></a>Uitvoer toevoegen aan Stream Analytics-taak

1. Onder **taak topologie**, klikt u op **uitvoer**.
1. In de **uitvoer** deelvenster, klikt u op **toevoegen**, en voer de volgende informatie:

   **Uitvoeraliassen**: de alias die uniek zijn voor de uitvoer.

   **Sink**: Selecteer **Blob Storage**.

   **Storage-account**: het storage-account voor de blob-opslag. U kunt een opslagaccount maken of een bestaande gebruiken.

   **Container**: de container waarin de blob is opgeslagen. U kunt een container maken of een bestaande gebruiken.

   **Gebeurtenis serialisatie-indeling**: Selecteer **CSV**.

   ![Uitvoer toevoegen aan Stream Analytics-taak in Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. Klik op **Create**.

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a>Een functie toevoegen aan de Stream Analytics-taak voor het aanroepen van de webservice die u hebt geïmplementeerd

1. Onder **taak topologie**, klikt u op **functies** > **toevoegen**.
1. Voer de volgende informatie in:

   **Functie Alias**: Voer `machinelearning`.

   **Type functie**: Selecteer **Azure ML**.

   **Optie importeren**: Selecteer **importeren uit een ander abonnement**.

   **URL**: Voer de URL van de WEBSERVICE die u hebt genoteerd omlaag uit de Excel-werkmap.

   **Sleutel**: Voer de TOEGANGSSLEUTEL die u hebt genoteerd af van de Excel-werkmap.

   ![Een functie toevoegen aan Stream Analytics-taak in Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. Klik op **Create**.

### <a name="configure-the-query-of-the-stream-analytics-job"></a>De query van de Stream Analytics-taak configureren

1. Onder **taak topologie**, klikt u op **Query**.
1. Vervang de bestaande code door de volgende code:

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   Vervang `[YourInputAlias]` met de ingevoerde alias van de taak.

   Vervang `[YourOutputAlias]` met de uitvoeralias van de taak.

1. Klik op **Opslaan**.

### <a name="run-the-stream-analytics-job"></a>De Stream Analytics-taak uitvoeren

Klik in de Stream Analytics-taak op **Start** > **nu** > **Start**. Zodra de taak kan worden gestart, wordt de taakstatus verandert van **gestopt** naar **met**.

![De Stream Analytics-taak uitvoeren](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a>Microsoft Azure Storage Explorer gebruiken om weer te geven van de prognose weer

Voer de clienttoepassing om te beginnen met het verzamelen en verzenden van temperatuur en vochtigheid gegevens naar uw IoT-hub. Voor elk bericht dat uw IoT-hub ontvangt, roept de Stream Analytics-taak de webservice voor de prognose weer om de kans op regen produceren. Het resultaat wordt vervolgens opgeslagen in de Azure blob-opslag. Azure Storage Explorer is een hulpprogramma dat u gebruiken kunt om het resultaat weer te geven.

1. [Download en installeer Microsoft Azure Storage Explorer](http://storageexplorer.com/).
1. Open Azure Opslagverkenner.
1. Aanmelden bij uw Azure-account.
1. Selecteer uw abonnement.
1. Klik op uw abonnement > **Opslagaccounts** > uw storage-account > **Blob-Containers** > uw container.
1. Open een CSV-bestand om het resultaat te bekijken. De laatste kolom registreert de kans op regen.

   ![Resultaat van de prognose weer met Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a>Samenvatting

U hebt Azure Machine Learning is gebruikt voor het produceren van de kans op regen op basis van de temperatuur en vochtigheid gegevens die uw IoT-hub ontvangt.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]