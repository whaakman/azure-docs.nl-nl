---
title: Azure Machine Learning met Azure IoT-rand implementeren | Microsoft Docs
description: Azure Machine Learning implementeren als een module die u wilt een edge-apparaat
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/12/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: e4753cf0ffdedcc2ddc694fba67c560363789e3a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Implementeren van Azure Machine Learning als een module van de rand van de IoT - voorbeeld

Rand van de IoT-modules kunt u code waarmee u uw bedrijfslogica rechtstreeks aan de rand van de IoT-apparaten implementeren. Deze zelfstudie wordt u instructies voor het implementeren van een Azure Machine Learning-module die wanneer een apparaat is mislukt op basis van sensorgegevens op het gesimuleerde rand van de IoT-apparaat dat u hebt gemaakt voorspelt in de [Azure IoT-Edge implementeren van een gesimuleerd apparaat op Windows] [ lnk-tutorial1-win] of [Linux] [ lnk-tutorial1-lin] zelfstudies.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Machine Learning-module maken
> * Een module-container push naar een Azure container-register
> * Een Azure Machine Learning module implementeren voor uw IoT-randapparaat
> * Gegenereerde gegevens weergeven

De Azure Machine Learning-module die u in deze zelfstudie maakt het milieu gegevens die zijn gegenereerd door uw apparaat leest en labels van de berichten als afwijkend of niet.

## <a name="prerequisites"></a>Vereisten

* Azure IoT apparaat aan de rand die u hebt gemaakt in de Quick Start of de eerste zelfstudie.
* De verbindingsreeks voor de IoT Hub voor uw IoT-Edge-apparaat verbinding met maakt IoT-hub.
* Een Azure Machine Learning-account. Volg de instructies in voor het maken van een account [maken Azure Machine Learning-accounts en installeer Azure Machine Learning Workbench](../machine-learning/service/quickstart-installation.md#create-azure-machine-learning-services-accounts). U hoeft niet te installeren van de workbench-toepassing voor deze zelfstudie.
* Beheer van de module voor Azure ML op uw computer. Volg de instructies in voor het instellen van uw omgeving en maak een account, [Model beheerinstellingen](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration).

De Azure Machine Learning-module biedt geen ondersteuning voor ARM-processors.

## <a name="create-the-azure-ml-container"></a>De Azure ML-container maken
In deze sectie download de bestanden van het getrainde model en omzetten in een Azure ML-container.

Download op de machine uitvoeren van beheer van de Module voor Azure ML en sla [iot_score.py](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/iot_score.py) en [model.pkl](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/model.pkl) uit de werkset van Azure ML IoT op GitHub. Deze bestanden definiëren het getrainde machine learning-model dat u naar uw Iot-Edge-apparaat implementeren wilt.

Gebruik het getrainde model te maken van een container die kan worden geïmplementeerd op de IoT Edge-apparaten. Gebruik de volgende opdracht:

   * Registreer uw model.
   * Een manifest maken.
   * Maken van de installatiekopie van een Docker-container met de naam *machinelearningmodule*.
   * De installatiekopie implementeren op uw Azure Kubernetes Service (AKS)-cluster.

```cmd
az ml service create realtime --model-file model.pkl -f iot_score.py -n machinelearningmodule -r python
```

### <a name="view-the-container-repository"></a>De container-opslagplaats weergeven

Controleer of uw installatiekopie container met succes is gemaakt en opgeslagen in de repository Azure-container die is gekoppeld aan uw machine learning-omgeving.

1. Op de [Azure-portal](https://portal.azure.com), gaat u naar **alle Services** en selecteer **Container registers**.
2. Selecteer in het register. De naam moet beginnen met **mlcr** en behoort tot de resourcegroep, de locatie en het abonnement dat u gebruikt voor het beheer van de Module instellen.
3. Selecteer **toegangssleutels**
4. Kopieer de **Login-server**, **gebruikersnaam**, en **wachtwoord**.  U moet deze toegang tot het register van uw apparaten rand.
5. Selecteer **opslagplaatsen**
6. Selecteer **machinelearningmodule**
7. U hebt nu het pad van de volledige installatiekopie van de container. Noteer dit pad naar afbeelding voor de volgende sectie. Deze ziet er als volgt: **< registry_name >.azureacr.io/machinelearningmodule:1**

## <a name="add-registry-credentials-to-your-edge-device"></a>Registerreferenties toevoegen aan uw Edge-apparaat

Voeg de referenties voor uw register toe aan de Edge-runtime op de computer waarop u het Edge-apparaat uitvoert. Met deze opdracht geeft de runtime toegang voor het ophalen van de container.

Linux:
   ```cmd
   sudo iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password>
   ```

Windows:
   ```cmd
   iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password>
   ```

## <a name="run-the-solution"></a>De oplossing uitvoeren

1. Op de [Azure-portal](https://portal.azure.com), gaat u naar uw IoT-hub.
1. Ga naar **IoT Edge (Preview)** en selecteer uw IoT Edge-apparaat.
1. Selecteer **Modules instellen**.
1. Als u hebt de tempSensor-module eerder hebt geïmplementeerd naar uw IoT-Edge-apparaat, kunnen er automatisch invullen. Als deze nog niet in uw lijst met modules, toevoegen.
    1. Selecteer **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `tempSensor` in.
    3. Voer in het veld **URI installatiekopie** `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` in.
    4. Selecteer **Opslaan**.
1. Toevoegen van de machine learning-module die u hebt gemaakt.
    1. Selecteer **IoT Edge-module toevoegen**.
    1. In de **naam** veld `machinelearningmodule`
    1. In de **installatiekopie** en voer het adres klopt; bijvoorbeeld `<registry_name>.azurecr.io/machinelearningmodule:1`.
    1. Selecteer **Opslaan**.
1. Terug in de stap **Modules toevoegen** selecteert u **Volgende**.
1. Kopieer in de stap **Routes opgeven** de onderstaande JSON in het tekstvak. De eerste route transporten berichten van de temperatuursensor aan de machine learning-module via het eindpunt 'amlInput', is het eindpunt dat alle Azure Machine Learning-modules gebruiken. De tweede route transporten berichten van de machine learning module IoT-hub. In deze route '' amlOutput'' is het eindpunt dat alle Azure Machine Learning-modules met gegevens en '' stroomopwaarts$ '' geeft IoT Hub.

    ```json
    {
        "routes": {
            "sensorToMachineLearning":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/machinelearningmodule/inputs/amlInput\")",
            "machineLearningToIoTHub": "FROM /messages/modules/machinelearningmodule/outputs/amlOutput INTO $upstream"
        }
    }
    ```

1. Selecteer **Volgende**.
1. Selecteer in de stap **Sjabloon controleren** de optie **Indienen**.
1. Ga terug naar de detailpagina van het apparaat en selecteer **Vernieuwen**.  U ziet nu de nieuwe **machinelearningmodule** uitgevoerd samen met de **tempSensor** module en de rand van de IoT-runtime-modules.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

U vindt de apparaat-naar-cloud-berichten die uw IoT-randapparaat worden verzonden via de [IoT Hub explorer](https://github.com/azure/iothub-explorer) of de extensie Azure IoT Toolkit voor Visual Studio Code.

1. Selecteer in Visual Studio Code **IoT Hub-apparaten**.
2. Selecteer **...**  Selecteer **verbindingsreeks van de IoT-Hub ingesteld** in het menu.

   ![IoT Hub-apparaten meer menu](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Voer in het tekstvak dat wordt geopend aan de bovenkant van de pagina de verbindingsreeks iothubowner voor uw IoT Hub. Uw IoT-randapparaat moet worden weergegeven in de lijst met IoT Hub-apparaten.
4. Selecteer **...**  opnieuw selecteert **D2C bericht bewaking starten**.
5. De berichten afkomstig zijn van tempSensor elke vijf seconden bekijken. De berichttekst bevat een eigenschap genaamd **afwijkingsdetectie** waarmee de machinelearningmodule met de waarde true of false. De **AzureMLResponse** eigenschap bevat de waarde 'OK' als het model is uitgevoerd.

   ![Azure ML-respons in de hoofdtekst van bericht](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een IoT-rand module aangedreven door Azure Machine Learning geïmplementeerd. U kunt door te gaan naar een van de andere zelfstudies voor meer informatie over andere manieren Azure IoT rand kunt u de gegevens in zakelijke inzichten aan de rand.

> [!div class="nextstepaction"]
> [Een Azure-functie implementeren als een module](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
