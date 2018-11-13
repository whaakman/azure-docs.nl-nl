---
title: Werken met meerdere Azure IoT Edge-modules in VS Code | Microsoft Docs
description: De IoT-extensie voor Visual Studio Code gebruiken voor het ontwikkelen van meerdere modules in één keer voor Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
ms.openlocfilehash: d895e10c6b667a05156b2ce0f83f7776b9ba4f0a
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568757"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code"></a>Een IoT Edge-oplossing met meerdere modules in Visual Studio Code ontwikkelen

U kunt Visual Studio Code gebruiken om een Azure IoT Edge-oplossing met meerdere modules te ontwikkelen. In dit artikel leest u hoe u een IoT Edge-oplossing kunt maken, bijwerken en implementeren waarmee sensorgegevens worden verzonden op een gesimuleerd IoT Edge-apparaat in VS Code. 

## <a name="prerequisites"></a>Vereisten

U moet over de volgende vereiste items beschikken om de stappen in dit artikel uit te voeren:

- [Visual Studio Code](https://code.visualstudio.com/)
- De [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- De [extensie C# voor Visual Studio Code (van OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- De [.NET Core SDK 2.1](https://www.microsoft.com/net/download)
- Een actieve IoT-hub met minimaal één IoT Edge-apparaat

U hebt ook [Docker voor VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) met Azure IoT Hub Device Explorer-integratie nodig om installatiekopieën en containers te beheren.

## <a name="create-your-iot-edge-solution"></a>Uw IoT Edge-oplossing maken

1. Open in Visual Studio code geïntegreerde terminal door te selecteren **weergave** > **geïntegreerde terminal**. 

1. In de VS Code **Command Palette**, voer en voer de opdracht **Azure IoT Edge: nieuwe IoT-Edge-oplossing**. Selecteer uw werkruimtemap en geef de oplossingsnaam op (de standaardwaarde is EdgeSolution). Maak een C#-module (met de naam van de **PipeModule**) als de eerste gebruikersmodule in deze oplossing. De standaardsjabloon van C#-module is een pipe-module, die rechtstreeks downstream pipes berichten van upstream aan. U moet ook de opslagplaats voor Dockerinstallatiekopieën opgeven voor de eerste module. De standaardopslagplaats voor installatiekopieën is gebaseerd op een lokaal Docker-register (**localhost:5000/<first module name>**). U kunt dit wijzigen in Azure Container Registry of Docker Hub. 

2. In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen. De hoofdmap bevat de map **modules**, de map **.vscode** en een sjabloonbestand voor het distributiemanifest. Foutopsporingsconfiguraties bevinden zich in de map .vscode. Alle gebruikersmodulecodes zijn submappen van de map modules. Het bestand deployment.template.json is de distributiemanifestsjabloon. Sommige parameters in dit bestand worden geparseerd van het bestand module.json, dat zich in elke modulemap bevindt.

3. Voeg de tweede module toe aan dit oplossingsproject. Er zijn verschillende manieren om toe te voegen een nieuwe module aan de huidige oplossing. Voer en voer de opdracht **Azure IoT Edge: toevoegen aan IoT Edge module**. Selecteer het implementatiesjabloonbestand dat u wilt bijwerken. Of met de rechtermuisknop op de map modules of met de rechtermuisknop op het bestand deployment.template.json en selecteer **IoT Edge-Module toevoegen**. Vervolgens wordt er een vervolgkeuzelijst module-type selecteren. Selecteer een **Azure Functions - C#** module met de naam van de **PipeFunction** en de opslagplaats voor Docker-installatiekopie. De standaardsjabloon van C#-functies-module is een pipe-module, die rechtstreeks downstream pipes berichten van upstream aan.

4. Open het bestand deployment.template.json. Controleer of met het bestand drie modules en de runtime worden gedeclareerd. Het bericht wordt gegenereerd vanuit de module tempSensor. Het bericht wordt rechtstreeks doorgegeven via de modules SampleModule en SampleFunction en vervolgens naar uw IoT-hub verzonden. 

5. Werk de routes voor deze modules bij met de volgende inhoud:

   ```json
        "routes": {
          "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/PipeModule/inputs/input1\")",
          "PipeModuleToPipeFunction": "FROM /messages/modules/PipeModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/PipeFunction/inputs/input1\")",
          "PipeFunctionToIoTHub": "FROM /messages/modules/PipeFunction/outputs/output1 INTO $upstream"
        },
   ```

5. Sla dit bestand op.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Uw IoT Edge-oplossing maken en implementeren

1. In de VS Code **Command Palette**, voer en voer de opdracht **Azure IoT Edge: Build IoT Edge-oplossing**. Op basis van het bestand module.json in elke modulemap wordt met de opdracht elke Docker-installatiekopie van de module gemaakt, in containers geplaatst en doorgegeven. Met de opdracht wordt vervolgens de vereiste waarde doorgevoerd naar het bestand deployment.template.json en wordt het bestand deployment.json gegenereerd met informatie uit de configuratiemap. In de geïntegreerde terminal in VS Code ziet u de voortgang van de build. 

2. Klik in de Azure IoT Hub **Device Explorer** met de rechtermuisknop op de id van een IoT Edge-apparaat en selecteer de opdracht **Create deployment for Edge device**. Selecteer het bestand deployment.json in de configuratiemap. In de geïntegreerde terminal in VS Code ziet u dat de implementatie is gemaakt met een implementatie-id.

3. Als u een IoT Edge-apparaat op uw ontwikkelcomputer simuleren bent, kunt u bekijken om te zien dat alle van de module installatiekopie containers binnen enkele minuten starten.

## <a name="view-the-generated-data"></a>De gegenereerde gegevens weergeven

1. Selecteer **View** > **Command Palette** om de gegevens te controleren die binnenkomen bij de IoT-hub. Selecteer vervolgens de opdracht **IoT: Start monitoring D2C message**. 
2. Als u wilt stoppen met het controleren van de gegevens, gebruikt u de opdracht **IoT: Stop monitoring D2C message** in het **opdrachtenpalet**. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere scenario's voor het ontwikkelen met Azure IoT Edge in Visual Studio Code:

* Ontwikkelen van modules in VS Code met [C#](how-to-develop-csharp-module.md) of [Node.js](how-to-develop-node-module.md).
* Ontwikkel Azure Functions in VS Code met [C#](how-to-develop-csharp-function.md).

Voor het ontwikkelen van modules voor uw IoT Edge-apparaten, [begrijpen en gebruiken Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md).