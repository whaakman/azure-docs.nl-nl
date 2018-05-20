---
title: Beheren van meerdere Azure IoT Edge-modules in VS-Code | Microsoft Docs
description: Gebruik Visual Studio Code voor het ontwikkelen van de rand van de IoT-oplossingen die gebruikmaken van meerdere modules.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4d9caa7aa95c99fa30e8ec76c5b6362615725622
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code---preview"></a>Een rand van de IoT-oplossing met meerdere modules in Visual Studio Code opstellen - voorbeeld
U kunt Visual Studio Code gebruiken voor het ontwikkelen van uw IoT-Edge-oplossing met meerdere modules. Dit artikel begeleidt bij het maken, bijwerken en implementeren van een rand van de IoT-oplossing die pipes sensorgegevens op het gesimuleerde apparaat IoT Edge in Visual Studio Code. 

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van de stappen in dit artikel hebt u de volgende vereisten voldaan:

- [Visual Studio Code](https://code.visualstudio.com/) 
- [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
- [C# voor uitbreiding van de Visual Studio Code (via OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
- [Docker](https://docs.docker.com/engine/installation/)
- [.NET core SDK 2.0](https://www.microsoft.com/net/core#windowscmd) 
- Sjabloon AzureIoTEdgeModule (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Een actieve iothub met ten minste één IoT randapparaat


* [Docker voor VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) met explorer-integratie voor het beheren van installatiekopieën en Containers.


## <a name="prepare-your-first-iot-edge-solution"></a>Voorbereiden van uw eerste rand van de IoT-oplossing
1. Typ in de VS Code opdracht palet en voer de opdracht **rand: nieuwe IoT oplossing**. Selecteer vervolgens uw werkruimtemap, geeft u de oplossingsnaam (de standaardnaam is **EdgeSolution**), en maak een C#-Module (**SampleModule**) als de eerste gebruikersmodule in deze oplossing. U moet ook de Docker-opslagplaats voor installatiekopieën voor uw eerste module opgeven. De standaard-opslagplaats voor installatiekopieën is gebaseerd op een lokale Docker-register (`localhost:5000/<first module name>`). U kunt deze ook wijzigen naar Azure container register- of Docker-Hub.

   > [!NOTE]
   > Als u van een lokale Docker-register gebruikmaakt, controleert u of het register wordt uitgevoerd door de opdracht te typen `docker run -d -p 5000:5000 --restart=always --name registry registry:2` in het consolevenster.

2. Het venster tegenover Code laadt uw werkruimte rand van de IoT-oplossing. De hoofdmap bevat een `modules` map, een `.vscode` map en een deployment manifest sjabloonbestand. U kunt zien configuraties in voor foutopsporing `.vscode` map. Alle gebruikers module codes zijn submappen onder de map `modules`. De `deployment.template.json` is de deployment manifest-sjabloon. Sommige parameters in dit bestand wordt geparseerd uit de `module.json`, die bestaat in de modulemap van elke.

3. Uw tweede module toevoegen aan dit project oplossing. Typ deze tijd en voer **rand: module toevoegen IoT rand** en selecteer het sjabloonbestand implementatie bij te werken. Selecteer vervolgens een **Azure-functie - C#** met de naam **SampleFunction** en de Docker-opslagplaats voor installatiekopieën.

4. Open de `deployment.template.json` bestand en controleer of er drie modules naast de runtime wordt mee aangegeven. Het bericht wordt gegenereerd op basis van de `tempSensor` -module, en wordt het rechtstreeks worden doorgegeven `SampleModule` en `SampleFunction`, vervolgens naar uw IoT-hub zijn verzonden. 
5. Werk de routes op voor deze modules met de volgende inhoud:
   ```json
   "routes": {
      "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
      "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
      "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
   },
   ```

6. Sla dit bestand.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Bouwen en implementeren van de rand van de IoT-oplossing
1. Typ in de VS Code opdracht palet en voer de opdracht **rand: bouwen IoT oplossing**. Op basis van de `module.json` in de modulemap van elke, deze opdracht start te bouwen, containerize en push van elke module docker-installatiekopie. Vervolgens deze de vereiste waarde geeft `deployment.template.json` en genereert de `deployment.json` bestand met informatie uit de `config` map. U kunt de voortgang van de build in VS-Code geïntegreerde terminal zien.

2. In Azure IoT Hub-apparaten explorer met de rechtermuisknop op een apparaat-ID van IoT-rand en selecteer vervolgens **implementatie creëert voor randapparaat**. Selecteer de `deployment.json` onder `config` map. Vervolgens ziet u dat de implementatie is gemaakt met een implementatie-ID in VS-Code wordt geïntegreerd terminal.

3. Als u een IoT-randapparaat zijn simuleren op uw ontwikkelcomputer, ziet u dat de containers voor de installatiekopie module in een paar minuten wordt gestart.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

1. Om gegevens die binnenkomen in de IoT Hub te controleren, selecteert u **View** > **Command Palette...** en zoekt u naar **IoT: Start monitoring D2C message**. 
2. Als u wilt stoppen met het controleren van gegevens, gebruikt u de opdracht **IoT: Stop monitoring D2C message** in de Command Palette. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere scenario's voor het ontwikkelen van Azure IoT rand in de Visual Studio Code:

* [Fouten opsporen in een C#-module in VS-Code](how-to-vscode-debug-csharp-module.md)
* [Fouten opsporen in een C#-functie in VS-Code](how-to-vscode-debug-azure-function.md)