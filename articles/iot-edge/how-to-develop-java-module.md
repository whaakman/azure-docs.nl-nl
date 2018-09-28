---
title: Fouten opsporen in Java-modules voor Azure IoT Edge | Microsoft Docs
description: Visual Studio Code gebruiken om te ontwikkelen en fouten opsporen in Java-modules voor Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/21/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: a5e86b7c93344a6c61fecaa41b98bc1daa20f36f
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423218"
---
# <a name="use-visual-studio-code-to-develop-and-debug-java-modules-for-azure-iot-edge"></a>Visual Studio Code gebruiken om te ontwikkelen en fouten opsporen in Java-modules voor Azure IoT Edge

U kunt uw bedrijfslogica op de edge werken door te schakelen in modules voor Azure IoT Edge kunt verzenden. Dit artikel bevat gedetailleerde instructies voor het gebruik van Visual Studio Code (VS-Code) als de belangrijkste ontwikkeltool om te ontwikkelen en fouten opsporen in Java-modules.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u een computer of virtuele machine met Windows, Mac OS- of Linux als uw ontwikkelcomputer. Uw IoT Edge-apparaat mag een andere fysieke apparaat.

> [!NOTE]
> In dit artikel foutopsporing ziet u twee gebruikelijke manieren om op te sporen uw Java-module in VS Code. Een manier is om te koppelen van een proces in een module-container, terwijl de andere het lanuch de module-code in de foutopsporingsmodus is. Als u niet bekend bent met de mogelijkheden voor foutopsporing van Visual Studio Code, meer informatie over [foutopsporing](https://code.visualstudio.com/Docs/editor/debugging).

Omdat in dit artikel Visual Studio Code als de belangrijkste ontwikkeltool wordt, Visual Studio Code installeren en vervolgens de nodige uitbreidingen toevoegen:
* [Visual Studio Code](https://code.visualstudio.com/) 
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) 
* [Java-uitbreidingspakket voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
* [Docker-extensie](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)

Voor het maken van een module, moet u Java en Maven wilt bouwen en uitvoeren van code in de module, Docker om de installatiekopie van de module en een containerregister te maken voor de installatiekopie van de module:
* [Java SE Development Kit 10](http://www.oracle.com/technetwork/java/javase/downloads/index.html); en [stel de omgevingsvariabele `JAVA_HOME` in](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) om te verwijzen naar uw JDK-installatie.
* [Maven](https://maven.apache.org/)
* [Docker](https://docs.docker.com/engine/installation/)
* [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   * U kunt een lokale Docker-register voor prototype- en testdoeleinden, in plaats van een cloud-register. 

Het instellen van lokale ontwikkelomgeving om op te sporen, uitvoeren en testen van uw IoT Edge-oplossing, moet u [Azure IoT EdgeHub ontwikkeltool](https://pypi.org/project/iotedgehubdev/). Installeer [Python (2.7/3.6) en Pip](https://www.python.org/). Installeer vervolgens **iotedgehubdev** onderstaande opdracht in de terminal service wordt uitgevoerd.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

Als u wilt testen van uw module op een apparaat, moet u een actieve IoT-hub met ten minste één IoT Edge-apparaat-ID gemaakt. Als u een IoT Edge-daemon op de ontwikkelcomputer uitvoert, moet u mogelijk EdgeHub en EdgeAgent stoppen voordat u met de volgende stap verdergaat. 


## <a name="create-a-new-solution-template"></a>Een nieuwe oplossingssjabloon maken

De volgende stappen laten zien hoe u een IoT Edge-module op basis van Java maken met behulp van Visual Studio Code en de Azure IoT Edge-extensie. Begint u met het maken van een oplossing en genereert vervolgens de eerste module in die oplossing. Elke oplossing kan meerdere modules bevatten. 

1. Selecteer in Visual Studio Code, **weergave** > **geïntegreerde Terminal**.

3. Selecteer **View** > **Command Palette** in Visual Studio Code. 
4. Typ in het opdrachtenpalet en voer de opdracht **Azure IoT Edge: nieuwe IoT Edge-oplossing**.

   ![Nieuwe IoT Edge-oplossing uitvoeren](./media/how-to-develop-csharp-module/new-solution.png)

5. Blader naar de map waar u de nieuwe oplossing maken en klikt u op **map selecteren**. 
6. Geef een naam voor uw oplossing. 
7. Kies **Java Module** als de sjabloon voor de eerste module in de oplossing.
8. Geef een naam voor uw module. Kies een naam die uniek is binnen uw containerregister. 
8. Geef een waarde op voor groupId of accepteer de standaard **com.edgemodule**.
9. Geef de opslagplaats voor installatiekopieën voor de module. VS Code autopopulates de module-naam, dus u hoeft te vervangen **localhost:5000** met uw eigen gegevens. Als u een lokale Docker-register voor het testen, klikt u vervolgens is ' localhost ' geen probleem. Als u Azure Container Registry gebruikt, gebruikt u de aanmeldingsserver van de instellingen van uw register. De aanmeldingsserver ziet eruit als  **\<registernaam\>. azurecr.io**. Vervang alleen het onderdeel localhost van de tekenreeks, verwijder niet de naam van de module.

   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/how-to-develop-node-module/repository.png)

VS Code haalt de gegevens opgegeven, maakt u een IoT Edge-oplossing en laadt deze in een nieuw venster.

In de oplossing hebt u drie items: 
* Een **.vscode** map bevat foutopsporing configuraties.
* Een **modules** map bevat submappen voor elke module. Op dit moment u slechts één hebt, maar u kunt meer toevoegen in het opdrachtenpalet met de opdracht **Azure IoT Edge: IoT Edge-Module toevoegen**. 
* Een **.env** bestand geeft een lijst van de omgevingsvariabelen. Als u Azure Container Registry is uw register, hebt u een Azure Container Registry-gebruikersnaam en wachtwoord in het.

   >[!NOTE]
   >Het omgevingsbestand wordt alleen gemaakt als u een opslagplaats voor installatiekopieën voor de module opgeven. Als u de standaardinstellingen localhost om te testen en fouten opsporen in lokaal hebt geaccepteerd, moet u geen omgevingsvariabelen declareren. 

* Een **deployment.template.json** bestand geeft een lijst van de nieuwe module samen met een voorbeeld van een **tempSensor** module die gegevens die u gebruiken voor het testen van kunt simuleert. Zie voor meer informatie over hoe implementatie werk manifesten [te begrijpen hoe IoT Edge-modules kunnen worden gebruikt, geconfigureerd en opnieuw gebruikt](module-composition.md).

## <a name="develop-your-module"></a>Uw-module ontwikkelen

De standaard Java-code die wordt geleverd met de oplossing bevindt zich in **modules > [Modulenaam van uw] > src > belangrijkste > java > com > edgemodulemodules > App.java**. De module en het bestand deployment.template.json zijn ingesteld zodat u kunt de oplossing te bouwen, deze naar het containerregister pushen en implementeren op een apparaat om te testen zonder code aan te starten. De module is gebouwd om te gewoon nemen invoer van een bron (in dit geval de tempSensor-module die gegevens simuleert) en doorgeven naar IoT Hub. 

Wanneer u klaar bent om de Java-sjabloon met uw eigen code aanpassen, gebruikt u de [Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md) modules bouwen dat adres de sleutel die nodig zijn voor IoT-oplossingen, zoals beveiliging, beheer van apparaten en betrouwbaarheid. 

Visual Studio Code biedt ondersteuning voor Java. Meer informatie over [over het werken met Java in VS Code](https://code.visualstudio.com/docs/java/java-tutorial).

## <a name="launch-and-debug-module-code-without-container"></a>Start en fouten opsporen in module code zonder container
De IoT Edge-Java-module is afhankelijk van apparaat-SDK voor Azure IoT-Java. In de code van de module standaard u initialiseert een **ModuleClient** met omgevingsinstellingen en voer een naam in, wat betekent dat de IoT Edge-Java-module vereist dat de omgevingsinstellingen start en uitvoert en moet u ook om te verzenden of berichten te routeren de invoer-kanalen. Uw standaard Java-module bevat alleen één invoer kanaal en de naam is **input1**.

### <a name="setup-iot-edge-simulator-for-single-module-app"></a>IoT Edge-simulator voor één module app instellen

1. Als u wilt installeren en starten van de simulator, typ in het opdrachtenpalet van VS Code, en selecteer **Azure IoT Edge: Start IoT Edge Hub Simulator voor één Module**. U moet ook opgeven de namen van de invoer voor de module voor enkele toepassing, type **input1** en druk op Enter. De opdracht wordt geactiveerd **iotedgehubdev** CLI en IoT Edge-simulator en een test hulpprogramma module-container te starten. U ziet de uitvoer hieronder in de geïntegreerde terminal als de simulator is gestart in de modus voor één module. U ziet ook een `curl` opdracht om te verzenden via. U gebruikt dit later.

   ![IoT Edge-simulator voor één module app instellen](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   U kunt verplaatsen naar Docker Explorer en de module met de status zien.

   ![Status van de Simulator-module](media/how-to-develop-csharp-module/simulator-status.png)

   De **edgeHubDev** container is de kern van de lokale IoT Edge-simulator. Deze kunt uitvoeren op uw ontwikkelcomputer zonder IoT Edge security-daemon en omgevingsinstellingen opgeven voor uw systeemeigen module app of de module containers. De **invoer** container restAPIs voor de brug berichten moet worden toegepast op uw module-invoerkanaal weergegeven.

2. Typ in het opdrachtenpalet van VS Code, en selecteer **Azure IoT Edge: Module referenties instellen voor gebruikersinstellingen** om in te stellen van de module omgevingsinstellingen in `azure-iot-edge.EdgeHubConnectionString` en `azure-iot-edge.EdgeModuleCACertificateFile` in de gebruikersinstellingen. U vindt deze omgevingsinstellingen wordt verwezen in **.vscode** > **launch.json** en [gebruikersinstellingen van VS Code](https://code.visualstudio.com/docs/getstarted/settings).

### <a name="debug-java-module-in-launch-mode"></a>Fouten opsporen in Java-module in de modus starten

2. Navigeer naar `App.java`. Voeg een onderbrekingspunt toe in dit bestand.

3. Navigeer naar de weergave voor probleemoplossing van VS Code. Selecteer de configuratie van de foutopsporing **ModuleName lokale foutopsporing (java)**. 

4. Klik op **Start Debugging** of druk op **F5**. U kunt de foutopsporingssessie wordt gestart.

5. In VS Code geïntegreerde terminal, voer de volgende opdracht voor het verzenden van een **Hello World** bericht naar uw module. Dit is de opdracht in de vorige stappen hebt u geleerd IoT Edge-simulator is wanneer de installatie. U moet mogelijk te maken of schakel over naar een andere geïntegreerde terminal als de huidige bewerking is geblokkeerd.

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Als u Windows gebruikt, te zorgen dat de shell van uw VS Code geïntegreerde terminal is **Git Bash** of **WSL Bash**. Kan niet worden uitgevoerd `curl` opdracht in PowerShell of Command Prompt. 
   
   > [!TIP]
   > U kunt ook [PostMan](https://www.getpostman.com/) of andere API's voor het verzenden van berichten via in plaats van `curl`.

6. VS-Code opsporen in de weergave ziet u de variabelen in het linkerdeelvenster. 

7. Als u wilt stoppen foutopsporingssessie, klikt u op de knop stoppen of druk op **Shift + F5**. Typ in het opdrachtenpalet van VS Code, en selecteer **Azure IoT Edge: IoT Edge-Simulator stoppen** om te stoppen en opschonen van de simulator.


## <a name="build-module-container-for-debugging-and-debug-in-attach-mode"></a>Build van container van de module voor foutopsporing en fouten opsporen in koppelen modus

Uw standaardoplossing bevat twee modules, is een gesimuleerde temperatuur sensor-module en de andere is de Java-pipe-module. De gesimuleerde temperatuursensor blijft verzenden van berichten naar Java pipe-module en klikt u vervolgens de berichten worden doorgesluisd naar IoT Hub. In de modulemap die u hebt gemaakt, zijn er verschillende Docker-bestanden voor andere containertypen. Gebruik een van deze bestanden die met de extensie eindigen **.debug** uw-module voor het testen van maken. Op dit moment ondersteuning Java modules alleen voor foutopsporing in linux-amd64- en linux-arm32v7-containers.

### <a name="setup-iot-edge-simulator-for-iot-edge-solution"></a>IoT Edge-simulator voor IoT Edge-oplossing instellen

Op uw ontwikkelcomputer kun u IoT Edge-simulator in plaats van installatie van de IoT Edge-security-daemon voor het uitvoeren van uw IoT Edge-oplossing. 

1. In de apparatenverkenner aan de linkerkant, met de rechtermuisknop op uw IoT Edge-apparaat-ID, selecteer **Setup IoT Edge Simulator** bij het starten van de simulator door de verbindingsreeks van het apparaat.

2. U ziet dat de IoT Edge-Simulator is ingesteld met succes in de geïntegreerde terminal.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Bouwen en uitvoeren van de container voor foutopsporing en fouten opsporen in koppelen modus

1. In VS Code, gaat u naar de `deployment.template.json` bestand. De afbeeldings-URL van uw module bijwerken door toe te voegen **.debug** aan het einde.

2. Vervang de Java-module createOptions in **deployment.template.json** met onderstaande inhoud en sla dit bestand: 
    ```json
    "createOptions":"{\"HostConfig\":{\"PortBindings\":{\"5005/tcp\":[{\"HostPort\":\"5005\"}]}}}"
    ```

5. Navigeer naar de weergave voor foutopsporing van VS Code. Selecteer het configuratiebestand voor de foutopsporing voor de module. De naam van de optie foutopsporing zijn vergelijkbaar met **ModuleName externe foutopsporing (Java)**.

6. Selecteer **Start Debugging** of selecteer **F5**. Selecteer het proces om aan te koppelen.

7. VS-Code opsporen in de weergave ziet u de variabelen in het linkerdeelvenster.

8. Als u wilt stoppen foutopsporingssessie, klikt u op de knop stoppen of druk op **Shift + F5**. Typ in het opdrachtenpalet van VS Code, en selecteer **Azure IoT Edge: IoT Edge-Simulator stoppen**.

> [!NOTE]
> Het vorige voorbeeld laat zien hoe fouten opsporen in Java IoT Edge-modules voor containers. Deze blootgestelde poorten in uw container module createOptions toegevoegd. Nadat u klaar bent met het opsporen van fouten in uw Java-modules, wordt u aangeraden verwijderen van deze blootgestelde poorten voor de IoT Edge-modules gereed is voor productie.

## <a name="next-steps"></a>Volgende stappen

Zodra u uw module die is gebouwd hebt, informatie over hoe u [implementeren Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md)

Voor het ontwikkelen van modules voor uw IoT Edge-apparaten, [begrijpen en gebruiken Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md).
