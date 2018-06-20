---
title: Meerdere Azure IoT Edge-modules beheren in VS Code | Microsoft Docs
description: Gebruik Visual Studio Code om Azure IoT Edge-oplossingen te ontwikkelen die gebruikmaken van meerdere modules.
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/18/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 4a624994f93e7a3cbb04db2a0ec0b2b823a12ee7
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34763031"
---
# <a name="develop-an-iot-edge-solution-with-multiple-modules-in-visual-studio-code-preview"></a>Een IoT Edge-oplossing met meerdere modules ontwikkelen in Visual Studio Code (preview)

U kunt Visual Studio Code gebruiken om een Azure IoT Edge-oplossing met meerdere modules te ontwikkelen. In dit artikel leest u hoe u een IoT Edge-oplossing kunt maken, bijwerken en implementeren waarmee sensorgegevens worden verzonden op een gesimuleerd IoT Edge-apparaat in VS Code. 

## <a name="prerequisites"></a>Vereisten

U moet over de volgende vereiste items beschikken om de stappen in dit artikel uit te voeren:

- [Visual Studio Code](https://code.visualstudio.com/)
- De [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge)
- De [extensie C# voor Visual Studio Code (van OmniSharp)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- [Docker](https://docs.docker.com/engine/installation/)
- De [.NET Core 2.0 SDK](https://www.microsoft.com/net/core#windowscmd)
- De sjabloon AzureIoTEdgeModule (`dotnet new -i Microsoft.Azure.IoT.Edge.Module`)
- Een actieve IoT-hub met minimaal één IoT Edge-apparaat

U hebt ook [Docker voor VS Code](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) met Azure IoT Hub Device Explorer-integratie nodig om installatiekopieën en containers te beheren.

## <a name="prepare-your-first-iot-edge-solution"></a>Uw eerste IoT Edge-oplossing voorbereiden

1. Voer in het **opdrachtenpalet** van VS Code de opdracht **Edge: New IoT Edge solution** uit. Selecteer uw werkruimtemap en geef de oplossingsnaam op (de standaardwaarde is EdgeSolution). Maak een C#-module (met de naam **SampleModule**) als eerste gebruikersmodule in deze oplossing. U moet ook de opslagplaats voor Dockerinstallatiekopieën opgeven voor de eerste module. De standaardopslagplaats voor installatiekopieën is gebaseerd op een lokaal Docker-register (**localhost:5000/<first module name>**). U kunt dit wijzigen in Azure Container Registry of Docker Hub.

   > [!NOTE]
   > Als u een lokaal Docker-register gebruikt, controleert u of het register wordt uitgevoerd. Geef de volgende opdracht op in het consolevenster:
   > 
   > `docker run -d -p 5000:5000 --restart=always --name registry registry:2`

2. In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen. De hoofdmap bevat de map **modules**, de map **.vscode** en een sjabloonbestand voor het distributiemanifest. Foutopsporingsconfiguraties bevinden zich in de map .vscode. Alle gebruikersmodulecodes zijn submappen van de map modules. Het bestand deployment.template.json is de distributiemanifestsjabloon. Sommige parameters in dit bestand worden geparseerd van het bestand module.json, dat zich in elke modulemap bevindt.

3. Voeg de tweede module toe aan dit oplossingsproject. Geef de opdracht **Edge: Add IoT Edge module** op en voer deze uit. Selecteer het implementatiesjabloonbestand dat u wilt bijwerken. Selecteer een module **Azure Function - C#** met de naam **SampleFunction** en de bijbehorende Docker-opslagplaats voor installatiekopieën.

4. Open het bestand deployment.template.json. Controleer of met het bestand drie modules en de runtime worden gedeclareerd. Het bericht wordt gegenereerd vanuit de module tempSensor. Het bericht wordt rechtstreeks doorgegeven via de modules SampleModule en SampleFunction en vervolgens naar uw IoT-hub verzonden. 

5. Werk de routes voor deze modules bij met de volgende inhoud:

   ```json
   "routes": {
      "SensorToPipeModule": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/SampleModule/inputs/input1\")",
      "PipeModuleToPipeFunction": "FROM /messages/modules/SampleModule/outputs/output1 INTO BrokeredEndpoint(\"/modules/SampleFunction/inputs/input1\")",
      "PipeFunctionToIoTHub": "FROM /messages/modules/SampleFunction/outputs/output1 INTO $upstream"
   },
   ```

6. Sla dit bestand op.

## <a name="build-and-deploy-your-iot-edge-solution"></a>Uw IoT Edge-oplossing maken en implementeren

1. Geef in het **opdrachtenpalet** van VS Code de opdracht **Edge: Build IoT Edge solution** op en voer deze uit. Op basis van het bestand module.json in elke modulemap wordt met de opdracht elke Docker-installatiekopie van de module gemaakt, in containers geplaatst en doorgegeven. Met de opdracht wordt vervolgens de vereiste waarde doorgevoerd naar het bestand deployment.template.json en wordt het bestand deployment.json gegenereerd met informatie uit de configuratiemap. In de geïntegreerde terminal in VS Code ziet u de voortgang van de build.

2. Klik in de Azure IoT Hub **Device Explorer** met de rechtermuisknop op de id van een IoT Edge-apparaat en selecteer de opdracht **Create deployment for Edge device**. Selecteer het bestand deployment.json in de configuratiemap. In de geïntegreerde terminal in VS Code ziet u dat de implementatie is gemaakt met een implementatie-id.

3. Als u een IoT Edge-apparaat simuleert op uw ontwikkelcomputer, ziet u dat alle installatiekopiecontainers van de module binnen enkele minuten worden gestart.

## <a name="view-the-generated-data"></a>De gegenereerde gegevens weergeven

1. Selecteer **View** > **Command Palette** om de gegevens te controleren die binnenkomen bij de IoT-hub. Selecteer vervolgens de opdracht **IoT: Start monitoring D2C message**. 
2. Als u wilt stoppen met het controleren van de gegevens, gebruikt u de opdracht **IoT: Stop monitoring D2C message** in het **opdrachtenpalet**. 

## <a name="next-steps"></a>Volgende stappen

Meer informatie over andere scenario's voor het ontwikkelen met Azure IoT Edge in Visual Studio Code:

* [Fouten opsporen in een C#-module in VS Code](how-to-vscode-debug-csharp-module.md)
* [Fouten opsporen in een C#-functie in VS Code](how-to-vscode-debug-azure-function.md)
