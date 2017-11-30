---
title: Azure Machine Learning met Azure IoT-rand implementeren | Microsoft Docs
description: Azure Machine Learning implementeren als een module die u wilt een edge-apparaat
services: iot-edge
keywords: 
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 11/15/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: e061e599f365bf3d343cb59b8dc6a61e06627517
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2017
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Implementeren van Azure Machine Learning als een module van de rand van de IoT - voorbeeld

Rand van de IoT-modules kunt u code waarmee u uw bedrijfslogica rechtstreeks aan de rand van de IoT-apparaten implementeren. Deze zelfstudie wordt u instructies voor het implementeren van een Azure Machine Learning-module die als een apparaat is mislukt op basis van sensorgegevens op het gesimuleerde rand van de IoT-apparaat dat u hebt gemaakt in de implementatie van Azure IoT-Edge van een gesimuleerd apparaat op voorspelt [Windows] [ lnk-tutorial1-win] of [Linux] [ lnk-tutorial1-lin] zelfstudies. Procedures voor: 

> [!div class="checklist"]
> * Een Azure Machine Learning module implementeren voor uw IoT-randapparaat
> * Gegevens weergeven die zijn gegenereerd

Als u wilt gebruiken uw eigen [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/) model is in uw oplossing die u gaat [implementeert een model](https://aka.ms/aml-iot-edge-doc) voor IoT-rand en host zoals deze in een container register [Azure Container register](../container-registry/index.yml) of Docker.

## <a name="prerequisites"></a>Vereisten

* Azure IoT apparaat aan de rand die u hebt gemaakt in de Quick Start of de eerste zelfstudie.
* De verbindingsreeks voor de IoT Hub voor uw IoT-Edge-apparaat verbinding met maakt IoT-hub.
* De Azure ML-container

## <a name="create-the-azure-ml-container"></a>De Azure ML-container maken
Voor het maken van uw Azure ML-container, volg de instructies in de [AI toolkit voor Azure IoT rand](https://aka.ms/aml-iot-edge-anomaly-detection).

## <a name="run-the-solution"></a>Uitvoeren van de oplossing

1. Op de [Azure-portal](https://portal.azure.com), gaat u naar uw IoT-hub.
1. Ga naar **IoT rand (preview)** en selecteer het apparaat IoT rand.
1. Selecteer **modules ingesteld**.
1. Selecteer **toevoegen IoT rand module**.
1. In de **naam** veld `tempSensor`.
1. In de **installatiekopie URI** veld `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
1. De andere instellingen ongewijzigd laat en selecteer **opslaan**.
1. Voeg nog steeds op de **Modules toevoegen** stap, selecteer **toevoegen IoT rand module** opnieuw.
1. In de **naam** en voer de naam van de container die u hebt aangebracht in de vorige sectie. Raadpleeg de [AI toolkit voor Azure IoT rand](https://aka.ms/aml-iot-edge-anomaly-detection) voor ondersteuning bij het vinden van de naam.
1. In de **installatiekopie** en voer de URI van de container die u hebt aangebracht in de vorige sectie van de installatiekopie. Raadpleeg de [AI toolkit voor Azure IoT rand](https://aka.ms/aml-iot-edge-anomaly-detection) voor ondersteuning bij het vinden van de installatiekopie.
1. Klik op **Opslaan**.
1. Terug in de **Modules toevoegen** stap, klikt u op **volgende**.
1. Routes voor de module bijwerken:
1. In de **Routes opgeven** stap, kopieert u de JSON onder in het tekstvak. Alle berichten publiceren modules naar de Edge-runtime. Declaratieve regels in de runtime definiëren waar deze berichten stromen. In deze zelfstudie moet u twee routes. De eerste route transporten berichten van de temperatuursensor aan de machine learning-module via het eindpunt 'amlInput', is het eindpunt dat alle Azure Machine Learning-modules gebruiken. De tweede route transporten berichten van de machine learning module IoT-hub. In deze route '' amlOutput'' is het eindpunt dat alle Azure Machine Learning-modules met gegevens en '' stroomopwaarts$ '' is een speciale bestemming waarin wordt uitgelegd rand Hub berichten verzenden naar IoT Hub. 

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ``` 

1. Klik op **Volgende**. 
1. Klik in de stap '' Template bekijken '' '' indienen ''. 
1. Terug naar de detailpagina voor het apparaat en klik '' vernieuwen ''.  Hier ziet u de nieuwe '' machinelearningmodule'' samen met de module '' tempSensor'' en '' IoT rand runtime'' uitgevoerd.

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

 Gebruik in VS-Code, de **weergave | Opdracht palet... | IoT: Bewaking D2C startberichten** menuopdracht voor het bewaken van gegevens die binnenkomen in de IoT-Hub. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een IoT-rand module aangedreven door Azure Machine Learning geïmplementeerd. U kunt door te gaan naar een van de andere zelfstudies voor meer informatie over andere manieren Azure IoT rand kunt u de gegevens in zakelijke inzichten aan de rand.

> [!div class="nextstepaction"]
> [Een Azure-functie implementeren als een module](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
