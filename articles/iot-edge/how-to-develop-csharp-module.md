---
title: Fouten opsporen in C#-modules voor Azure IoT Edge | Microsoft Docs
description: Gebruik Visual Studio Code ontwikkelen, bouwen en fouten opsporen in een C#-module voor Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/04/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a66a17a0f8daed5f61753dd7c20ed5d9987c0b15
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44053897"
---
# <a name="use-visual-studio-code-to-develop-and-debug-c-modules-for-azure-iot-edge"></a>Visual Studio Code gebruiken om te ontwikkelen en fouten opsporen in C#-modules voor Azure IoT Edge

U kunt uw bedrijfslogica in modules inschakelen voor Azure IoT Edge. Dit artikel ziet u hoe u Visual Studio Code (VS-Code) gebruikt als het belangrijkste instrument voor het ontwikkelen en fouten opsporen in C#-modules.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u een computer of virtuele machine met Windows, Mac OS- of Linux als uw ontwikkelcomputer. Uw IoT Edge-apparaat mag een andere fysieke apparaat.

> [!NOTE]
> In dit artikel foutopsporing ziet u twee gebruikelijke manieren fouten opsporen in uw C#-module in VS Code. Een manier is om te koppelen van een proces in een module-container, terwijl de andere het lanuch de module-code in de foutopsporingsmodus is. Als u niet bekend bent met de mogelijkheden voor foutopsporing van Visual Studio Code, meer informatie over [foutopsporing](https://code.visualstudio.com/Docs/editor/debugging).

Omdat in dit artikel Visual Studio Code als de belangrijkste ontwikkeltool wordt, Visual Studio Code installeren. Voeg vervolgens de nodige uitbreidingen toe:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [C#-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 
* [Docker-extensie](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Voor het maken van een module, moet u .NET voor het bouwen van de projectmap, Docker om de installatiekopie van de module en een containerregister voor de module-installatiekopie te bouwen:
* [.NET Core 2.1 SDK](https://www.microsoft.com/net/download).
* [Docker Community Edition](https://docs.docker.com/install/) op uw ontwikkelcomputer. 
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * U kunt een lokale Docker-register voor prototype en in plaats van een cloud-register voor testdoeleinden gebruiken. 

Het instellen van lokale ontwikkelomgeving om op te sporen, uitvoeren en testen van uw IoT Edge-oplossing, moet u [Azure IoT EdgeHub ontwikkeltool](https://pypi.org/project/iotedgehubdev/). Installeer [Python (2.7/3.6) en Pip](https://www.python.org/). Installeer vervolgens **iotedgehubdev** onderstaande opdracht in de terminal service wordt uitgevoerd.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Als u wilt testen van uw module op een apparaat, moet u een actieve IoT-hub met ten minste één IoT Edge-apparaat-ID gemaakt. Als u een IoT Edge-daemon op de ontwikkelcomputer uitvoert, moet u mogelijk EdgeHub en EdgeAgent stoppen voordat u met de volgende stap verdergaat. 

## <a name="create-a-new-solution-with-c-module"></a>Een nieuwe oplossing maken met C#-module

Volg deze stappen voor het maken van een IoT Edge-module op basis van .NET Core 2.0 met behulp van Visual Studio Code en de Azure IoT Edge-extensie. U maakt eerst een oplossing en vervolgens het genereren van de eerste module in die oplossing. Elke oplossing kan meer dan één module bevatten. 

1. Selecteer in Visual Studio Code, **weergave** > **geïntegreerde Terminal**.
3. Selecteer **weergave** > **Command Palette**. 
4. Voer in het opdrachtenpalet en voer de opdracht **Azure IoT Edge: nieuwe IoT Edge-oplossing**.

   ![Nieuwe IoT Edge-oplossing uitvoeren](./media/how-to-develop-csharp-module/new-solution.png)

5. Blader naar de map waar u de nieuwe oplossing maken. Kies **map selecteren**. 
6. Voer een naam voor uw oplossing. 
7. Selecteer **C#-Module** als de sjabloon voor de eerste module in de oplossing.
8. Voer een naam voor uw module. Kies een naam die uniek is binnen uw containerregister. 
9. Geef de naam van de opslagplaats voor installatiekopieën van de module. VS Code autopopulates de module met naam **localhost:5000**. Vervang deze door uw eigen gegevens. Als u een lokale Docker-register voor het testen, klikt u vervolgens **localhost** functioneert. Als u Azure Container Registry gebruikt, gebruikt u de aanmeldingsserver van de instellingen van uw register. De aanmeldingsserver ziet eruit als  **\<registernaam\>. azurecr.io**. Vervang alleen het onderdeel localhost van de tekenreeks, verwijder niet de naam van de module.

   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/how-to-develop-csharp-module/repository.png)

VS Code haalt de gegevens die u verstrekt, maakt u een IoT Edge-oplossing, en laadt deze in een nieuw venster.

   ![IoT Edge-oplossing weergeven](./media/how-to-develop-csharp-module/view-solution.png)

Er zijn vier items in de oplossing: 
* Een **.vscode** map bevat foutopsporing configuraties.
* Een **modules** map bevat submappen voor elke module. Op dit moment, hoeft u slechts één. Maar u kunt meer toevoegen in het opdrachtenpalet met de opdracht **Azure IoT Edge: IoT Edge-Module toevoegen**. 
* Een **.env** bestand geeft een lijst van de omgevingsvariabelen. Als u Azure Container Registry is uw register, hebt u een Azure Container Registry-gebruikersnaam en wachtwoord in het. 

   >[!NOTE]
   >Het omgevingsbestand wordt alleen gemaakt als u een opslagplaats voor installatiekopieën voor de module opgeven. Als u de standaardinstellingen localhost om te testen en fouten opsporen in lokaal hebt geaccepteerd, moet u geen omgevingsvariabelen declareren. 

* Een **deployment.template.json** bestand geeft een lijst van de nieuwe module samen met een voorbeeld van een **tempSensor** module die gegevens simuleert u voor testdoeleinden kunt gebruiken. Zie voor meer informatie over hoe implementatie werk manifesten [informatie over het gebruik van implementatie manifesten implementeren modules en routes tot stand brengen](module-composition.md). 

## <a name="develop-your-module"></a>Uw-module ontwikkelen

De standaard C#-module-code die wordt geleverd met de oplossing bevindt zich in **modules** > ** [Modulenaam van uw] ** > **Program.cs**. De module en het bestand deployment.template.json zijn ingesteld zodat u kunt de oplossing te bouwen, deze naar het containerregister pushen en implementeren op een apparaat om te testen zonder code aan te starten. De module is gebouwd om te gewoon nemen invoer van een bron (in dit geval de tempSensor-module die gegevens simuleert) en doorgeven naar IoT Hub. 

Wanneer u klaar bent om de sjabloon C# met uw eigen code aanpassen, gebruikt u de [Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md) modules bouwen dat adres de sleutel die nodig zijn voor IoT-oplossingen, zoals beveiliging, beheer van apparaten en betrouwbaarheid. 

De C#-ondersteuning in VS Code is geoptimaliseerd voor platformoverschrijdende .NET Core-ontwikkeling. Meer informatie over [over het werken met C# in VS Code](https://code.visualstudio.com/docs/languages/csharp).

## <a name="launch-and-debug-module-code-without-container"></a>Start en fouten opsporen in module code zonder container
De IoT Edge C#-module is een .net Core-toepassing. En dat hangt ervan af op Azure IoT C# Device SDK. In de code van de module standaard u initialiseert een **ModuleClient** met omgevingsinstellingen en voer een naam in, wat betekent dat de IoT Edge C#-module vereist dat de omgevingsinstellingen start en uitvoert en moet u ook om te verzenden of het routeren van berichten naar de invoer kanalen. Uw standaard C#-module bevat slechts één invoer kanaal en de naam is **input1**.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>IoT Edge-simulator voor één module app instellen

1. Als u wilt installeren en starten van de simulator, typ in het opdrachtenpalet van VS Code, en selecteer **Azure IoT Edge: Start IoT Edge Hub Simulator voor één Module**. U moet ook opgeven de namen van de invoer voor de module voor enkele toepassing, type **input1** en druk op Enter. De opdracht wordt geactiveerd **iotedgehubdev** CLI en IoT Edge-simulator en een test hulpprogramma module-container te starten. U ziet de uitvoer hieronder in de geïntegreerde terminal als de simulator is gestart in de modus voor één module. U ziet ook een `curl` opdracht om te verzenden via. U gebruikt dit later.

   ![IoT Edge-simulator voor één module app instellen](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   U kunt verplaatsen naar Docker Explorer en de module met de status zien.

   ![Status van de Simulator-module](media/how-to-develop-csharp-module/simulator-status.png)

   De **edgeHubDev** container is de kern van de lokale IoT Edge-simulator. Deze kunt uitvoeren op uw ontwikkelcomputer zonder IoT Edge security-daemon en omgevingsinstellingen opgeven voor uw systeemeigen module app of de module containers. De **invoer** container restAPIs voor de brug berichten moet worden toegepast op uw module-invoerkanaal weergegeven.

2. Typ in het opdrachtenpalet van VS Code, en selecteer **Azure IoT Edge: Module referenties instellen voor gebruikersinstellingen** om in te stellen van de module omgevingsinstellingen in `azure-iot-edge.EdgeHubConnectionString` en `azure-iot-edge.EdgeModuleCACertificateFile` in de gebruikersinstellingen. U vindt deze omgevingsinstellingen wordt verwezen in **.vscode** > **launch.json** en [gebruikersinstellingen van VS Code](https://code.visualstudio.com/docs/getstarted/settings).

### <a name="build-module-app-and-debug-in-launch-mode"></a>Module-app bouwen en fouten opsporen in de modus starten

1. In de geïntegreerde terminal, wijzig de map naar **CSharpModule** map, voer de volgende opdracht om te maken van .net Core-toepassing.

    ```cmd
    dotnet build
    ```

2. Navigeer naar `program.cs`. Voeg een onderbrekingspunt toe in dit bestand.

3. Navigeer naar de weergave voor probleemoplossing van VS Code. Selecteer de configuratie van de foutopsporing **ModuleName lokale foutopsporing (.NET Core)**. 

4. Klik op **Start Debugging** of druk op **F5**. U kunt de foutopsporingssessie wordt gestart.

5. In VS Code geïntegreerde terminal, voer de volgende opdracht voor het verzenden van een **Hello World** bericht naar uw module. Dit is de opdracht in de vorige stappen hebt u geleerd IoT Edge-simulator is wanneer de installatie.

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Als u Windows gebruikt, te zorgen dat de shell van uw VS Code geïntegreerde terminal is **Git Bash** of **WSL Bash**. Kan niet worden uitgevoerd `curl` opdracht in PowerShell of Command Prompt. 
   
   > [!TIP]
   > U kunt ook [PostMan](https://www.getpostman.com/) of andere API's voor het verzenden van berichten via in plaats van `curl`.

6. VS-Code opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

    ![Bekijk variabelen](media/how-to-develop-csharp-module/single-module-variables.png)

7. Als u wilt stoppen foutopsporingssessie, klikt u op de knop stoppen of druk op **Shift + F5**. Typ in het opdrachtenpalet van VS Code, en selecteer **Azure IoT Edge: IoT Edge-Simulator stoppen** stoppen en opschonen van de simulator.

## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Build van container van de module voor foutopsporing en fouten opsporen in koppelen modus

De standaardoplossing bevat twee modules, is een gesimuleerde temperatuur sensor-module en de andere is de C# pipe-module. De gesimuleerde temperatuursensor blijft verzenden van berichten naar de C# pipe-module en klikt u vervolgens de berichten worden doorgesluisd naar IoT Hub. In de modulemap die u hebt gemaakt, zijn er verschillende Docker-bestanden voor andere containertypen. Gebruik een van deze bestanden die met de extensie eindigen **.debug** uw-module voor het testen van maken. Op dit moment C# modules ondersteuning voor foutopsporing alleen in Linux amd64-containers in de modus koppelen. 

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>IoT Edge-simulator voor IoT Edge-oplossing instellen

Op uw ontwikkelcomputer kun u IoT Edge-simulator in plaats van installatie van de IoT Edge-security-daemon voor het uitvoeren van uw IoT Edge-oplossing. 

1. In de apparatenverkenner aan de linkerkant, met de rechtermuisknop op uw IoT Edge-apparaat-ID, selecteer **Setup IoT Edge Simulator** bij het starten van de simulator door de verbindingsreeks van het apparaat.

2. U ziet dat de IoT Edge-Simulator is ingesteld met succes in de geïntegreerde terminal.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Bouwen en uitvoeren van de container voor foutopsporing en fouten opsporen in koppelen modus

1. In VS Code, gaat u naar de `deployment.template.json` bestand. Bijwerken van uw C#-module afbeeldings-URL door toe te voegen **.debug** aan het einde.

   ![Voeg *** .debug op de installatiekopienaam van uw](./media/how-to-develop-csharp-module/image-debug.png)

2. Navigeer naar `program.cs`. Voeg een onderbrekingspunt toe in dit bestand.
3. Selecteer in het bestand VS Code explorer de `deployment.template.json` -bestand voor uw oplossing, in het contextmenu, klikt u op **bouwen en uitvoeren op IoT Edge-oplossing in de Simulator**. U kunt controleren of dat alle van de module-container in hetzelfde venster registreert. U kunt ook navigeren naar Docker Explorer om te bekijken van de containerstatus van de.

   ![Bekijk variabelen](media/how-to-develop-csharp-module/view-log.png)

5. Navigeer naar de weergave voor foutopsporing van VS Code. Selecteer het configuratiebestand voor de foutopsporing voor de module. De naam van de optie foutopsporing zijn vergelijkbaar met **ModuleName externe foutopsporing (.NET Core)**

   ![Configuratie selecteren](media/how-to-develop-csharp-module/debug-config.png)

6. Selecteer **Start Debugging** of selecteer **F5**. Selecteer het proces om aan te koppelen.

7. VS-Code opsporen in de weergave ziet u de variabelen in het linkerdeelvenster.

8. Als u wilt stoppen foutopsporingssessie, klikt u op de knop stoppen of druk op **Shift + F5**. Typ in het opdrachtenpalet van VS Code, en selecteer **Azure IoT Edge: IoT Edge-Simulator stoppen**.

> [!NOTE]
> In dit voorbeeld laat zien hoe fouten opsporen in .NET Core IoT Edge-modules voor containers. Deze gebaseerd op de foutopsporingsversie van `Dockerfile.debug`, waaronder het opdrachtregelprogramma foutopsporingsprogramma van .NET Core VSDBG in uw containerinstallatiekopie tijdens het maken van deze. Nadat u fouten opsporen in uw C#-modules, wordt aangeraden dat u rechtstreeks gebruiken of aanpassen `Dockerfile` zonder VSDBG voor IoT Edge-modules gereed is voor productie.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw module hebt gebouwd, informatie over hoe u [implementeren van Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md).

Voor het ontwikkelen van modules voor uw IoT Edge-apparaten, [begrijpen en gebruiken Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md).
