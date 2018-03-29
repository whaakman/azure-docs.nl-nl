---
title: Werken met meerdere IoT Edge-modules in Visual Studio Code | Microsoft Docs
description: Azure Machine Learning implementeren als een module die u wilt een edge-apparaat
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 0ea2dc723c674e7119b6ef38771a73ff4c11e98d
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code---preview"></a>Een rand van de IoT-oplossing met meerdere modules in Visual Studio Code opstellen - voorbeeld
U kunt Visual Studio Code gebruiken voor het ontwikkelen van uw IoT-Edge-oplossing met meerdere modules. Deze zelfstudie wordt u bij het maken, bijwerken en implementeren van een rand van de IoT-oplossing die pipe-sensorgegevens gewoon op het gesimuleerde apparaat IoT Edge in Visual Studio Code. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een Edge van de IoT-oplossing te maken
> * VS-Code gebruiken een nieuwe module toevoegen aan uw werkende rand van de IoT-oplossing. 
> * Implementeren van de rand van de IoT-oplossing (meerdere modules) naar uw IoT-Edge-apparaat
> * Gegevens weergeven die zijn gegenereerd

## <a name="prerequisites"></a>Vereisten
* Hieronder zelfstudies voltooien
  * [C#-module implementeren](tutorial-csharp-module.md)
  * [C#-functie implementeren](tutorial-deploy-function.md)
  * [Python-module implementeren](tutorial-python-module.md)
* [Docker voor VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) met explorer-integratie voor het beheren van installatiekopieën en Containers.


## <a name="prepare-your-first-iot-edge-solution"></a>Voorbereiden van uw eerste rand van de IoT-oplossing
1. Typ in de VS Code opdracht palet en voer de opdracht **rand: nieuwe IoT oplossing**. Selecteer vervolgens uw werkruimtemap, geeft u de oplossingsnaam (de standaardnaam is **EdgeSolution**), en maak een C#-Module (**SampleModule**) als de eerste gebruikersmodule in deze oplossing. U moet ook de Docker-opslagplaats voor installatiekopieën voor uw eerste module opgeven. De standaard-opslagplaats voor installatiekopieën is gebaseerd op een lokale Docker-register (`localhost:5000/<first module name>`). U kunt deze ook wijzigen naar Azure container register- of Docker-Hub.

> [!NOTE]
> Als u van een lokale Docker-register gebruikmaakt, controleert u of het register wordt uitgevoerd door de opdracht te typen `docker run -d -p 5000:5000 --restart=always --name registry registry:2` in het consolevenster.

2. Het venster tegenover Code laadt uw werkruimte rand van de IoT-oplossing. Er is een `modules` map, een `.vscode` map en een deployment manifest sjabloonbestand in de hoofdmap. U kunt zien configuraties in voor foutopsporing `.vscode` map. Alle gebruikers module codes zijn submappen onder de map `modules`. De `deployment.template.json` is de deployment manifest-sjabloon. Sommige parameters in dit bestand wordt geparseerd uit de `module.json`, die bestaat in de modulemap van elke.

3. Uw tweede module toevoegen aan dit project oplossing. Typ deze tijd en voer **rand: module toevoegen IoT rand** en selecteer het sjabloonbestand implementatie bij te werken. Selecteer vervolgens een **Azure-functie - C#** met de naam **SampleFunction** en de Docker-opslagplaats voor installatiekopieën om toe te voegen.

4. Nu is uw eerste rand van de IoT-oplossing met twee basic modules gereed. De standaard C#-module fungeert als een module pipe-bericht, terwijl de C#-Funtion als een functie pipe-bericht fungeert. In de `deployment.template.json`, ziet u deze oplossing bevat drie modules. Het bericht wordt gegenereerd op basis van de `tempSensor` -module, en wordt het rechtstreeks worden doorgegeven `SampleModule` en `SampleFunction`, vervolgens naar uw IoT-hub zijn verzonden. De routes voor deze modules met onderstaande inhoud bijwerken. 
   ```json
        "routes": {
          "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
          "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
          "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
        },
   ```

5. Sla dit bestand.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Bouwen en implementeren van de rand van de IoT-oplossing
1. Typ in de VS Code opdracht palet en voer de opdracht **rand: bouwen IoT oplossing**. Op basis van de `module.json` -bestand in elke modulemap met deze opdracht wordt controleren en te bouwen, containerize en elke module docker-installatiekopie push start. Vervolgens wordt deze de vereiste waarde om te parseren `deployment.template.json`, genereren de `deployment.json` met de werkelijke waarde onder `config` map. U kunt de voortgang van de build in VS-Code geïntegreerde terminal zien.

2. In Azure IoT Hub-apparaten explorer met de rechtermuisknop op een apparaat-ID van IoT-rand en selecteer vervolgens **implementatie creëert voor randapparaat**. Selecteer de `deployment.json` onder `config` map. Vervolgens ziet u dat de implementatie is gemaakt met een implementatie-ID in VS-Code wordt geïntegreerd terminal.

3. Als u [simuleren een IoT-randapparaat](tutorial-simulate-device-linux.md) op uw ontwikkelcomputer. U ziet dat de containers voor de installatiekopie module in een paar minuten wordt gestart.

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

1. Voor het bewaken van gegevens die binnenkomen in de IoT-hub, selecteer de **weergave** > **opdracht palet...**  en zoek naar **IoT: D2C bericht bewaking starten**. 
2. Als u wilt stoppen met het bewaken van gegevens, gebruikt u de **IoT: D2C bericht bewaking stopt** opdracht in het palet opdracht. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een Edge van de IoT-oplossing met een C#-module, later u een functiemodule toegevoegd, de routes voor de oplossing wordt bijgewerkt, gebouwd en geïmplementeerd op het gesimuleerde apparaat van de rand van de IoT gemaakt. U kunt door te gaan naar een van de volgende zelfstudies voor meer informatie over andere scenario's bij het ontwikkelen van Azure IoT rand in de VS-Code.

> [!div class="nextstepaction"]
> [Fouten opsporen in een C#-module in VS-Code](how-to-vscode-debug-csharp-module.md)
> [fouten opsporen in een C#-functie in VS-Code](how-to-vscode-debug-azure-function.md)