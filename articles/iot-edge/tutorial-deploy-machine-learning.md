---
title: Azure Machine Learning implementeren met Azure IoT Edge | Microsoft Docs
description: Azure Machine Learning implementeren als module op een Edge-apparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/12/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 248bc97c214c013d10f1839201ce2f572cb854ed
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34631170"
---
# <a name="deploy-azure-machine-learning-as-an-iot-edge-module---preview"></a>Azure Machine Learning als een IoT Edge-module implementeren - preview

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie doorloopt u het implementeren van een Azure Machine Learning-module waarmee wordt voorspeld wanneer een apparaat niet meer werkt op basis van sensorgegevens op het gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de zelfstudie [Azure IoT-Edge implementeren op een gesimuleerd apparaat in Windows][lnk-tutorial1-win] of [Linux][lnk-tutorial1-lin].

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Machine Learning-module maken
> * Een module-container naar een Azure-containerregister zenden
> * Een Azure Machine Learning-module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven

De Azure Machine Learning-module die u in deze zelfstudie maakt, leest de omgevingsgegevens die door uw apparaat zijn gegenereerd en markeert de berichten als afwijkend of niet.

## <a name="prerequisites"></a>Vereisten

* Het Azure IoT Edge-apparaat dat u hebt gemaakt in de snelstartgids of de vorige zelfstudie.
* De IoT Hub-verbindingsreeks voor de IoT Hub waarmee uw IoT Edge-apparaat wordt verbonden.
* Een Azure Machine Learning-account. Volg de instructies in [Preview-accounts maken voor Azure Machine Learning en Azure Machine Learning Workbench installeren](../machine-learning/service/quickstart-installation.md#create-azure-machine-learning-services-accounts) om een account te maken. U hoeft de workbench-toepassing niet te installeren voor deze zelfstudie. 
* Module-beheer voor Azure ML op uw apparaat. Volg de instructies in [Model management setup](../machine-learning/desktop-workbench/deployment-setup-configuration.md) om uw omgeving in te stellen en een account te maken.

De Azure Machine Learning-module ondersteunt geen ARM-processoren.

## <a name="create-the-azure-ml-container"></a>De Azure ML-container maken
In deze sectie downloadt u de bestanden van het getrainde model en converteert die in een Azure ML-container.

Download [iot_score.py](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/iot_score.py) en [model.pkl](https://github.com/Azure/ai-toolkit-iot-edge/blob/master/IoT%20Edge%20anomaly%20detection%20tutorial/model.pkl) vanuit de Azure ML IoT Toolkit op GitHub op het apparaat waar Module Management voor Azure ML wordt uitgevoerd en sla ze daar op. Deze bestanden definiëren het getrainde machine learning-model dat u zult implementeren naar uw IoT Edge-apparaat.

Gebruik het getrainde model om een container te maken die kan worden geïmplementeerd op de IoT Edge-apparaten. Gebruik de volgende opdracht om:

   * Uw model te registreren.
   * Een manifest maken.
   * Maak een Docker-containerinstallatiekopie met de naam *machinelearningmodule*.
   * Implementeer de installatiekopie op uw Azure Kubernetes Service (AKS)-cluster.

```cmd
az ml service create realtime --model-file model.pkl -f iot_score.py -n machinelearningmodule -r python
```

### <a name="view-the-container-repository"></a>De containeropslagplaats bekijken

Controleer of de containerinstallatiekopie is gemaakt en opgeslagen in de Azure-containeropslagplaats die is gekoppeld aan uw machine learning-omgeving.

1. Ga op het [Azure Portal](https://portal.azure.com) naar **Alle services** en selecteer **Containerregisters**.
2. Selecteer uw register. De naam moet beginnen met **mlcr** en behoren tot de resourcegroep en locatie die, en het abonnement dat u hebt gebruikt om Module-beheer in te stellen.
3. Selecteer **Toegangssleutels**
4. Kopieer **Aanmeldingsserver**, **Gebruikersnaam** en **Wachtwoord**.  Deze hebt u nodig om het register van uw Edge-apparaten te openen.
5. Selecteer **Opslagplaatsen**
6. Selecteer **machinelearningmodule**
7. U hebt nu het volledige pad van de installatiekopie van de container. Noteer dit pad voor de volgende sectie. Het zou er als volgt uit moeten zien: **<registry_name>.azureacr.io/machinelearningmodule:1**

## <a name="add-registry-credentials-to-your-edge-device"></a>Registerreferenties toevoegen aan uw Edge-apparaat

Voeg de referenties voor uw register toe aan de Edge-runtime op de computer waarop u het Edge-apparaat uitvoert. Deze opdracht geeft de runtime toegang voor het ophalen van de container.

Linux:
   ```cmd
   sudo iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password>
   ```

Windows:
   ```cmd
   iotedgectl login --address <registry-login-server> --username <registry-username> --password <registry-password>
   ```

## <a name="run-the-solution"></a>De oplossing uitvoeren

1. Ga in [Azure Portal](https://portal.azure.com) naar uw IoT-hub.
1. Ga naar **IoT Edge (Preview)** en selecteer uw IoT Edge-apparaat.
1. Selecteer **Modules instellen**.
1. Als u de tempSensor-module eerder op uw IoT Edge-apparaat hebt geïmplementeerd, wordt het mogelijk automatisch ingevuld. Als die nog niet in uw lijst met modules staat, voeg deze dan toe.
    1. Selecteer **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `tempSensor` in.
    3. Voer in het veld **URI installatiekopie** `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` in.
    4. Selecteer **Opslaan**.
1. Voeg de machine learning-module toe die u hebt gemaakt.
    1. Selecteer **IoT Edge-module toevoegen**.
    1. Voer in het veld **Naam** `machinelearningmodule` in
    1. Voer in het veld **Installatiekopie** het adres van uw installatiekopie in, bijvoorbeeld `<registry_name>.azurecr.io/machinelearningmodule:1`.
    1. Selecteer **Opslaan**.
1. Terug in de stap **Modules toevoegen** selecteert u **Volgende**.
1. Kopieer in de stap **Routes opgeven** de onderstaande JSON in het tekstvak. De eerste route transporteert berichten van de temperatuursensor naar de machine learning-module via het ‘amlInput’-eindpunt. Dit is het eindpunt dat alle Azure Machine Learning-modules gebruiken. De tweede route transporteert berichten van de filtermodule naar IoT Hub. In deze route is ‘amlOutput’ het eindpunt dat alle Azure Machine Learning-modules gebruiken voor gegevens en ‘$upstream’ beschrijft IoT Hub.

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
1. Ga terug naar de detailpagina van het apparaat en selecteer **Vernieuwen**.  U ziet nu dat de nieuwe **machinelearningmodule** samen met de **tempSensor**-module en de IoT Edge-runtime-modules wordt uitgevoerd.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

U kunt de apparaat-naar-cloud-berichten zien die uw IoT Edge-apparaat verzendt door [IoT Hub-explorer](https://github.com/azure/iothub-explorer) of de Azure IoT Toolkit-extensie voor Visual Studio Code te gebruiken.

1. Selecteer in Visual Studio Code **IoT Hub-apparaten**.
2. Selecteer in het menu **...** en vervolgens **Set IoT Hub-verbindingsreeksen**.

   ![Menu Meer IoT Hub-apparaten](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Voer in het tekstvak dat bovenaan de pagina wordt geopend de iothubowner-verbindingsreeks in voor uw IoT Hub. Uw IoT Edge-apparaat moet worden weergegeven in de lijst met IoT Hub-apparaten.
4. Selecteer **...** opnieuw en vervolgens **Controle D2C-bericht starten**.
5. Bekijk de berichten die de tempSensor elke vijf seconden verzendt. De berichttekst bevat een eigenschap genaamd **afwijking** die de machinelearningmodule voorziet van een waarde ‘waar’ of ‘onwaar’. De eigenschap **AzureMLResponse** bevat de waarde ‘OK’ als het model is uitgevoerd.

   ![Azure ML-antwoord in berichttekst](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module geïmplementeerd die werd aangedreven door Azure Machine Learning. U kunt doorgaan met elke andere zelfstudie om te leren waarmee Azure IoT Edge u nog meer kan helpen uw gegevens om te zetten in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Een Azure Function implementeren als module](tutorial-deploy-function.md)

<!--Links-->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md
