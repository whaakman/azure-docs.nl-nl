---
title: Ontwikkelen en fouten opsporen in modules voor Azure IoT Edge | Microsoft Docs
description: Gebruik Visual Studio Code ontwikkelen, bouwen en fouten opsporen in een module voor het gebruik van Azure IoT Edge C#, Python, Node.js, Java of C
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/12/2019
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5abc13a39db3f1061e3df76857645d8075feade5
ms.sourcegitcommit: f4b78e2c9962d3139a910a4d222d02cda1474440
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2019
ms.locfileid: "54245749"
---
# <a name="use-visual-studio-code-to-develop-and-debug-modules-for-azure-iot-edge"></a>Visual Studio Code gebruiken om te ontwikkelen en fouten opsporen in modules voor Azure IoT Edge

U kunt uw bedrijfslogica in modules inschakelen voor Azure IoT Edge. Dit artikel ziet u hoe u Visual Studio Code gebruikt als het belangrijkste instrument voor het ontwikkelen en fouten opsporen in modules.

## <a name="prerequisites"></a>Vereisten

U kunt een computer of een virtuele machine met Windows, macOS of Linux als uw ontwikkelcomputer. Een IoT Edge-apparaat mag een andere fysieke apparaat.

Voor modules die zijn geschreven C#, Node.js of Java, er zijn twee manieren om fouten opsporen in uw module in Visual Studio Code: U kunt een proces in een container module toevoegen of start de module-code in de foutopsporingsmodus. Voor modules die zijn geschreven in Python of C, kunnen ze alleen worden opgespoord door te koppelen aan een proces in Linux amd64-containers.

> [!TIP]
> Als u niet bekend bent met de mogelijkheden voor foutopsporing van Visual Studio Code, meer informatie over [foutopsporing](https://code.visualstudio.com/Docs/editor/debugging).

Installeer [Visual Studio Code](https://code.visualstudio.com/) eerste en voeg de volgende extensies:

- [Hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
- [Docker-extensie](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
- Visual Studio extensie (s) specifiek zijn voor de taal die u ontwikkelt in:
  - C#, met inbegrip van Azure Functions: [C#-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  - Python: [Python-extensie](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  - Java: [Java-uitbreidingspakket voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
  - C: [C/C++-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)

U moet ook enkele aanvullende taalspecifieke-programma's installeren om het ontwikkelen van uw module:

- C#, met inbegrip van Azure Functions: [.NET Core-SDK 2.1](https://www.microsoft.com/net/download)

- Python: [Python](https://www.python.org/downloads/) en [Pip](https://pip.pypa.io/en/stable/installing/#installation) voor het installeren van Python-pakketten (gewoonlijk opgenomen met uw Python-installatie). Als Pip is geïnstalleerd, installeert de **Cookiecutteru** pakket met de volgende opdracht:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

- Node.js: [Node.js](https://nodejs.org). U zult ook wilt installeren [Yeoman](https://www.npmjs.com/package/yo) en de [Azure IoT Edge Node.js-Module Generator](https://www.npmjs.com/package/generator-azure-iot-edge-module).

- Java: [Java SE Development Kit 10](https://aka.ms/azure-jdks) en [Maven](https://maven.apache.org/). U moet [instellen de `JAVA_HOME` omgevingsvariabele](https://docs.oracle.com/cd/E19182-01/820-7851/inst_cli_jdk_javahome_t/) om te verwijzen naar de JDK-installatie.

Om te bouwen en implementeren van uw module-installatiekopie, moet u Docker om de installatiekopie van de module en een containerregister voor de module-installatiekopie te bouwen:

- [Docker Community Edition](https://docs.docker.com/install/) op uw ontwikkelcomputer.

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)

    > [!TIP]
    > U kunt een lokale Docker-register voor prototype en in plaats van een cloud-register voor testdoeleinden gebruiken.

Tenzij u uw module in C ontwikkelt, moet u ook de op Python gebaseerde [Azure IoT EdgeHub ontwikkeltool](https://pypi.org/project/iotedgehubdev/) om in te stellen van uw lokale ontwikkelomgeving om op te sporen, uitvoeren en testen van uw IoT Edge-oplossing. Als u dit nog niet hebt gedaan, installeert u [Python (2.7/3.6) en Pip](https://www.python.org/) en installeer vervolgens **iotedgehubdev** door het uitvoeren van deze opdracht in uw terminal.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

> [!NOTE]
> Als u wilt testen van uw module op een apparaat, moet u een actieve IoT-hub met ten minste één IoT Edge-apparaat. Voor het gebruik van de computer als een IoT Edge-apparaat, volg de stappen in de quickstart voor [Linux](quickstart-linux.md) of [Windows](quickstart.md). Als u een IoT Edge-daemon op uw ontwikkelcomputer uitvoert, moet u mogelijk EdgeHub en EdgeAgent stoppen voordat u met de volgende stap verdergaat.

## <a name="create-a-new-solution-template"></a>Een nieuwe oplossingssjabloon maken

De volgende stappen laten zien over het maken van een IoT Edge-module in uw favoriete talen (met inbegrip van Azure Functions, die zijn geschreven in C#) met behulp van Visual Studio Code en de Azure IoT-hulpprogramma's. Begint u met het maken van een oplossing en genereert vervolgens de eerste module in die oplossing. Elke oplossing kan meerdere modules bevatten.

1. Selecteer **weergave** > **Command Palette**.

1. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: Nieuwe IoT Edge-oplossing**.

   ![Nieuwe IoT Edge-oplossing uitvoeren](./media/how-to-develop-csharp-module/new-solution.png)

1. Blader naar de map waar u de nieuwe oplossing maken en selecteer vervolgens **map selecteren**.

1. Voer een naam voor uw oplossing.

1. Selecteer een sjabloon van de module voor de meest geschikte ontwikkelings-taal moet de eerste module in de oplossing.

1. Voer een naam voor uw module. Kies een naam die uniek is binnen uw containerregister.

1. Geef de naam van de opslagplaats voor installatiekopieën van de module. Visual Studio Code autopopulates de module met naam **localhost:5000 / < modulenaam van uw\>**. Vervang deze door uw eigen gegevens. Als u een lokale Docker-register voor het testen, klikt u vervolgens **localhost** functioneert. Als u Azure Container Registry gebruikt, gebruikt u de aanmeldingsserver van de instellingen van uw register. De aanmeldingsserver ziet eruit als ***\<registernaam\>*. azurecr.io**. Het vervangen van slechts de **localhost:5000** deel uitmaakt van de tekenreeks, zodat het uiteindelijke resultaat ziet als eruit **\<* registernaam*\>.azurecr.io/* \<de modulenaam van uw\>***.

   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/how-to-develop-csharp-module/repository.png)

Visual Studio Code wordt de informatie die u verstrekt, maakt u een IoT Edge-oplossing, en laadt deze in een nieuw venster.

Er zijn vier items in de oplossing:

- Een **.vscode** map bevat foutopsporing configuraties.

- Een **modules** map bevat submappen voor elke module. Op dit moment, hoeft u slechts één. Maar u kunt meer toevoegen in het opdrachtenpalet met de opdracht **Azure IoT Edge: Add IoT Edge Module** uit te voeren.

- Een **.env** bestand geeft een lijst van de omgevingsvariabelen. Als u Azure Container Registry is uw register, hebt u een Azure Container Registry-gebruikersnaam en wachtwoord in het.

  > [!NOTE]
  > Het omgevingsbestand wordt alleen gemaakt als u een opslagplaats voor installatiekopieën voor de module opgeven. Als u de standaardinstellingen localhost om te testen en fouten opsporen in lokaal hebt geaccepteerd, moet u geen omgevingsvariabelen declareren.

- Een **deployment.template.json** bestand geeft een lijst van de nieuwe module samen met een voorbeeld van een **tempSensor** module die gegevens simuleert u voor testdoeleinden kunt gebruiken. Zie voor meer informatie over hoe implementatie werk manifesten [informatie over het gebruik van implementatie manifesten implementeren modules en routes tot stand brengen](module-composition.md).

## <a name="add-additional-modules"></a>Aanvullende modules toevoegen

Aanvullende modules toevoegen aan uw oplossing, voer de opdracht **Azure IoT Edge: IoT Edge-Module toevoegen** vanuit het opdrachtenpalet. U kunt ook met de rechtermuisknop op de **modules** map of het `deployment.template.json` -bestand in de weergave van Visual Studio Code Explorer en selecteer vervolgens **IoT Edge-Module toevoegen**.

## <a name="develop-your-module"></a>Uw-module ontwikkelen

De standaard-module-code die wordt geleverd met de oplossing bevindt zich in de volgende locatie:

- Azure-functie (C#): **modules >  *&lt;de modulenaam van uw&gt;* > *&lt;de modulenaam van uw&gt;*.cs**
- C#: **modules > *&lt;de modulenaam van uw&gt;* > Program.cs**
- Python: **modules > *&lt;de modulenaam van uw&gt;* > main.py**
- Node.js: **modules > *&lt;de modulenaam van uw&gt;* > app.js**
- Java: **modules > *&lt;de modulenaam van uw&gt;* > src > belangrijkste > java > com > edgemodulemodules > App.java**
- C: **modules > *&lt;de modulenaam van uw&gt;* > main.c**

De module en het bestand deployment.template.json zijn ingesteld zodat u kunt de oplossing te bouwen, deze naar het containerregister pushen en implementeren op een apparaat om te testen zonder code aan te starten. De module is gebouwd om te gewoon nemen invoer van een bron (in dit geval de tempSensor-module die gegevens simuleert) en doorgeven naar IoT Hub.

Wanneer u klaar bent om de sjabloon aanpassen met uw eigen code, gebruikt u de [Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md) modules bouwen dat adres de sleutel die nodig zijn voor IoT-oplossingen, zoals beveiliging, beheer van apparaten en betrouwbaarheid.

## <a name="debug-a-module-without-a-container-c-nodejs-java"></a>Fouten opsporen in een module zonder een container (C#, Node.js, Java)

Als u ontwikkelt C#, Node.js of Java, gebruik van uw module vereist een **ModuleClient** van het object in de code voor de module zodat deze kan starten, uitvoeren en routeren van berichten. U gaat ook gebruikmaken invoerkanaal voor de standaard **input1** actie ondernemen wanneer de module berichten ontvangt.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>IoT Edge-simulator voor IoT Edge-oplossing instellen

Op uw ontwikkelcomputer kun u een IoT Edge-simulator in plaats van installatie van de IoT Edge-daemon voor beveiliging, zodat u uw IoT Edge-oplossing kunt uitvoeren.

1. In de apparatenverkenner aan de linkerkant, met de rechtermuisknop op uw IoT Edge-apparaat-ID en selecteer vervolgens **Setup IoT Edge Simulator** bij het starten van de simulator door de verbindingsreeks van het apparaat.
1. U ziet dat de IoT Edge-Simulator is is ingesteld door te lezen van de details van de voortgang in de geïntegreerde terminal.

### <a name="set-up-iot-edge-simulator-for-single-module-app"></a>IoT Edge-simulator voor één module app instellen

Als u wilt instellen en de simulator start, voert u de opdracht **Azure IoT Edge: IoT Edge Hub Simulator start voor één Module** vanuit het opdrachtenpalet van Visual Studio Code. Wanneer u hierom wordt gevraagd, gebruikt u de waarde **input1** van de code van de module standaard (of de equivalente waarde vanuit uw code) als de ingevoerde naam voor uw toepassing. De opdracht triggers de **iotedgehubdev** CLI en start vervolgens de IoT Edge-simulator en een test hulpprogramma module-container. U ziet de uitvoer hieronder in de geïntegreerde terminal als de simulator is gestart in de modus voor één module. U ziet ook een `curl` opdracht om te verzenden via. U gebruikt dit later.

   ![IoT Edge-simulator voor één module app instellen](media/how-to-develop-csharp-module/start-simulator-for-single-module.png)

   U kunt weergave van de Docker-Verkenner in Visual Studio Code gebruiken om de actieve status van de module te bekijken.

   ![Status van de Simulator-module](media/how-to-develop-csharp-module/simulator-status.png)

   De **edgeHubDev** container is de kern van de lokale IoT Edge-simulator. Het kan worden uitgevoerd op uw ontwikkelcomputer zonder de IoT Edge security-daemon en omgevingsinstellingen biedt voor uw systeemeigen module app of de module containers. De **invoer** container REST-API's om te helpen bridge-berichten naar de doel-invoerkanaal op uw module gebruikt.

### <a name="debug-module-in-launch-mode"></a>Fouten opsporen in de module in de modus starten

1. Uw omgeving voorbereidt voor foutopsporing op basis van de vereisten van uw programmeertaal, stel een onderbrekingspunt in de module en selecteer de configuratie van de foutopsporing te gebruiken:
   - **C#**
     - In de Visual Studio Code geïntegreerde terminal, wijzig de map in de ***&lt;de modulenaam van uw&gt;*** map en voer de volgende opdracht uit om te maken van .net Core-toepassing.

       ```cmd
       dotnet build
       ```

     - Open het bestand `Program.cs` en voeg een onderbrekingspunt toe.

     - Navigeer naar de weergave foutopsporing van Visual Studio Code door te selecteren **weergave > fouten opsporen in**. Selecteer de configuratie van de foutopsporing  ***&lt;de modulenaam van uw&gt;* lokale foutopsporing (.NET Core)** in de vervolgkeuzelijst.

        > [!NOTE]
        > Als uw .net Core `TargetFramework` is niet consistent met het programmapad in `launch.json`, moet u handmatig bijwerken van het programmapad in `launch.json` zodat deze overeenkomen met de `TargetFramework` in uw .csproj-bestand zodat die Visual Studio-Code kunt is dit starten programma.

   - **Node.js**
     - In de Visual Studio Code geïntegreerde terminal, wijzig de map in de ***&lt;de modulenaam van uw&gt;*** map en voer de volgende opdracht uit om knooppunt pakketten te installeren

       ```cmd
       npm install
       ```

     - Open het bestand `app.js` en voeg een onderbrekingspunt toe.

     - Navigeer naar de weergave foutopsporing van Visual Studio Code door te selecteren **weergave > fouten opsporen in**. Selecteer de configuratie van de foutopsporing  ***&lt;de modulenaam van uw&gt;* lokale foutopsporing (Node.js)** in de vervolgkeuzelijst.
   - **Java**
     - Open het bestand `App.java` en voeg een onderbrekingspunt toe.

     - Navigeer naar de weergave foutopsporing van Visual Studio Code door te selecteren **weergave > fouten opsporen in**. Selecteer de configuratie van de foutopsporing  ***&lt;de modulenaam van uw&gt;* lokale foutopsporing (Java)** in de vervolgkeuzelijst.

1. Klik op **Start Debugging** of druk op **F5** om de foutopsporingssessie te starten.

1. In de Visual Studio Code geïntegreerde terminal en voer de volgende opdracht voor het verzenden van een **Hello World** bericht naar uw module. Dit is de opdracht die wordt weergegeven in de vorige stappen bij het instellen van IoT Edge-simulator.

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   > [!NOTE]
   > Als u Windows gebruikt, te zorgen dat de shell van uw Visual Studio Code geïntegreerde terminal is **Git Bash** of **WSL Bash**. Kan niet worden uitgevoerd de `curl` opdracht uit vanaf een PowerShell of command prompt.
   > [!TIP]
   > U kunt ook [PostMan](https://www.getpostman.com/) of andere API's voor het verzenden van berichten via in plaats van `curl`.

1. In de weergave foutopsporing van Visual Studio Code ziet u de variabelen in het linkerdeelvenster.

1. Als u wilt uw Foutopsporingssessie stoppen, selecteert u de knop stoppen of druk op **Shift + F5**, en voer **Azure IoT Edge: IoT Edge Simulator stoppen** in het opdrachtenpalet de simulator stoppen en opschonen.

## <a name="debug-in-attach-mode-with-iot-edge-simulator-c-nodejs-java-azure-functions"></a>Fouten opsporen in de modus met IoT Edge Simulator koppelen (C#, Node.js, Java, Azure Functions)

Uw standaardoplossing bevat twee modules, is een gesimuleerde temperatuur sensor-module en de andere is het pipe-module. De gesimuleerde temperatuursensor berichten worden verzonden naar de pipe-module en klikt u vervolgens de berichten worden doorgesluisd naar de IoT-Hub. In de modulemap die u hebt gemaakt, zijn er verschillende Docker-bestanden voor andere containertypen. Gebruik een van de bestanden die met de extensie eindigen **.debug** uw-module voor het testen van maken.

Op dit moment foutopsporing in koppelen modus wordt ondersteund alleen als volgt te werk:

- C#modules, zoals die voor Azure Functions, ondersteuning voor foutopsporing in Linux amd64-containers
- Node.js-modules ondersteuning van foutopsporing in Linux amd64 arm32v7 containers en Windows-amd64-containers
- Java-modules ondersteuning voor foutopsporing in amd64 en arm32v7 voor Linux-containers

> [!TIP]
> U kunt wisselen tussen de opties voor het standaard-platform voor uw IoT Edge-oplossing door te klikken op het item in de Visual Studio Code-statusbalk.

### <a name="set-up-iot-edge-simulator-for-iot-edge-solution"></a>IoT Edge-simulator voor IoT Edge-oplossing instellen

Op uw ontwikkelcomputer kun u een IoT Edge-simulator in plaats van installatie van de IoT Edge-daemon voor beveiliging, zodat u uw IoT Edge-oplossing kunt uitvoeren.

1. In de apparatenverkenner aan de linkerkant, met de rechtermuisknop op uw IoT Edge-apparaat-ID en selecteer vervolgens **Setup IoT Edge Simulator** bij het starten van de simulator door de verbindingsreeks van het apparaat.

1. U ziet dat de IoT Edge-Simulator is is ingesteld door te lezen van de details van de voortgang in de geïntegreerde terminal.

### <a name="build-and-run-container-for-debugging-and-debug-in-attach-mode"></a>Bouwen en uitvoeren van de container voor foutopsporing en fouten opsporen in koppelen modus

1. Open het modulebestand (`Program.cs`, `app.js`, `App.java`, of `<your module name>.cs`) en voeg een onderbrekingspunt toe.

1. In de weergave Visual Studio Code Explorer met de rechtermuisknop op de `deployment.debug.template.json` -bestand voor uw oplossing en selecteer vervolgens **bouwen en uitvoeren op IoT Edge-oplossing in de Simulator**. U kunt controleren of dat alle van de module-container in hetzelfde venster registreert. U kunt ook navigeren naar de Docker-weergave bekijken van de containerstatus van de.

   ![Bekijk variabelen](media/how-to-develop-csharp-module/view-log.png)

1. Navigeer naar de weergave voor foutopsporing van Visual Studio Code en selecteer het configuratiebestand voor de foutopsporing voor de module. De naam van de optie foutopsporing zijn vergelijkbaar met  ***&lt;de modulenaam van uw&gt;* externe foutopsporing**

1. Selecteer **Start Debugging** of druk op **F5**. Selecteer het proces om aan te koppelen.

1. Visual Studio-Code opsporen in de weergave ziet u de variabelen in het linkerdeelvenster.

1. Als u wilt de Foutopsporingssessie stoppen, selecteert u eerst de knop stoppen of druk op **Shift + F5**, en selecteer vervolgens **Azure IoT Edge: IoT Edge Simulator stoppen** vanuit het opdrachtenpalet.

> [!NOTE]
> Het vorige voorbeeld laat zien hoe fouten opsporen in IoT Edge-modules voor containers. Deze blootgestelde poorten toegevoegd aan de container van de module `createOptions` instellingen. Nadat u klaar bent met het opsporen van fouten in uw modules, wordt u aangeraden verwijderen van deze blootgestelde poorten voor de IoT Edge-modules gereed is voor productie.
>
> Voor modules die zijn geschreven C#, met inbegrip van Azure Functions, in dit voorbeeld is gebaseerd op de foutopsporingsversie van `Dockerfile.amd64.debug`, waaronder het opdrachtregelprogramma foutopsporingsprogramma van .NET Core (VSDBG) in uw containerinstallatiekopie tijdens het maken van deze. Nadat u de fouten opsporen in uw C# modules, raden wij aan dat u direct het Dockerfile zonder VSDBG voor IoT Edge-modules gereed is voor productie.

## <a name="debug-a-module-with-the-iot-edge-runtime"></a>Fouten opsporen in een module met de IoT Edge-runtime

In elke modulemap zijn er verschillende Docker-bestanden voor andere containertypen. Gebruik een van de bestanden die met de extensie eindigen **.debug** uw-module voor het testen van maken.

Wanneer foutopsporing modules met behulp van deze methode, uw modules worden uitgevoerd boven op IoT Edge-runtime. De IoT Edge-apparaat en uw Visual Studio Code kunnen zich op dezelfde computer, of meer normaal gesproken Visual Studio Code is op de ontwikkelcomputer en de IoT Edge-runtime en -modules worden uitgevoerd op een andere fysieke machine. Om te kunnen opsporen in Visual Studio Code, moet u:

- Instellen van uw IoT Edge-apparaat, het bouwen van uw IoT Edge-modules met de **.debug** Dockerfile, en vervolgens implementeert op het IoT Edge-apparaat.
- De IP en poort van de module beschikbaar maakt zodat het foutopsporingsprogramma kan worden gekoppeld.
- Update de `launch.json` zodat Visual Studio Code aan het proces in de container op de externe computer koppelen kunt. Dit bestand bevindt zich de `.vscode` map in uw werkruimte en updates telkens wanneer u een nieuwe module die ondersteuning biedt voor foutopsporing toevoegen.

### <a name="build-and-deploy-your-module-to-the-iot-edge-device"></a>Ontwikkel en implementeer uw module op het IoT Edge-apparaat

1. Open in Visual Studio Code, de `deployment.debug.template.json` -bestand waarin de foutopsporingsversie van de module afbeeldingen met de juiste `createOptions` set waarden.

1. Als u uw module in Python ontwikkelt, volgt u deze stappen voordat u doorgaat:
   - Open het bestand `main.py` en voeg deze code toe na de sectie importeren:

      ```python
      import ptvsd
      ptvsd.enable_attach(('0.0.0.0',  5678))
      ```
   - De volgende één regel code toevoegen aan de callback die u fouten wilt opsporen:

      ```python
      ptvsd.break_into_debugger()
      ```

     Bijvoorbeeld, als u fouten wilt opsporen de `receive_message_callback` methode, zou u dat line-of-code, zoals hieronder weergegeven:

      ```python
      def receive_message_callback(message, hubManager):
          ptvsd.break_into_debugger()
          global RECEIVE_CALLBACKS
          message_buffer = message.get_bytearray()
          size = len(message_buffer)
          print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode ('utf-8'), size) )
          map_properties = message.properties()
          key_value_pair = map_properties.get_internals()
          print ( "    Properties: %s" % key_value_pair )
          RECEIVE_CALLBACKS += 1
          print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
          hubManager.forward_event_to_output("output1", message, 0)
          return IoTHubMessageDispositionResult.ACCEPTED
      ```

1. In het opdrachtenpalet van Visual Studio Code:
   1. Voer de opdracht **Azure IoT Edge: Build en Push IoT Edge-oplossing**.

   1. Selecteer de `deployment.debug.template.json` -bestand voor uw oplossing.

1. In de **Azure IoT Hub-apparaten** sectie van de Visual Studio Code Explorer-weergave:
   1. Met de rechtermuisknop op een IoT Edge-apparaat-ID en selecteer vervolgens **implementatie maken voor één apparaat**.

      > [!TIP]
      > Om te bevestigen dat het apparaat die u hebt ervoor gekozen een IoT Edge-apparaat is, selecteert u deze aan de lijst met modules uitvouwen en controleer of de aanwezigheid van **$edgeHub** en **$edgeAgent**. Elke IoT Edge-apparaat omvat deze twee modules.

   1. Navigeer naar van uw oplossing **config** map, selecteer de `deployment.debug.amd64.json` bestand en selecteer vervolgens **Selecteer Edge-implementatie Manifest**.

Hier ziet u de implementatie is gemaakt met een implementatie-ID in de geïntegreerde terminal.

U kunt de containerstatus van uw controleren door het uitvoeren van de `docker ps` opdracht in de terminal. Als uw Visual Studio Code en IoT Edge-runtime op dezelfde computer uitvoert, kunt u ook de status in de weergave van Visual Studio Code Docker controleren.

### <a name="expose-the-ip-and-port-of-the-module-for-the-debugger"></a>De IP en poort van de module voor het foutopsporingsprogramma beschikbaar maakt

U kunt in deze sectie overslaan als de modules worden uitgevoerd op dezelfde computer als Visual Studio Code, als u ' localhost ' koppelen aan de container en al beschikt over de juiste poortinstellingen in de **.debug** Dockerfile, module van container `createOptions` -instellingen en `launch.json` bestand. Als uw modules en Visual Studio Code worden uitgevoerd op afzonderlijke computers, volg de stappen voor de ontwikkeling van taal.

- **C#, met inbegrip van Azure Functions**

  [De SSH-kanaal op uw ontwikkelcomputer en IoT Edge-apparaat configureert](https://github.com/OmniSharp/omnisharp-vscode/wiki/Attaching-to-remote-processes) en bewerk vervolgens `launch.json` bestand om te koppelen.

- **Node.js**

  - Zorg ervoor dat de module op de machine u fouten wilt opsporen wordt uitgevoerd en gereed voor foutopsporing te koppelen, en dat poort 9229 extern toegankelijk is. U kunt dit controleren door het openen van `http://<target-machine-IP>:9229/json` op de machine foutopsporingsprogramma. Deze URL moet informatie over de Node.js-module u fouten wilt opsporen worden weergegeven.
  
  - Open Visual Studio Code op uw ontwikkelcomputer en bewerk vervolgens `launch.json` zodat de adreswaarde van de  ***&lt;de modulenaam van uw&gt;* externe foutopsporing (Node.js)** profiel (of  ***&lt;de modulenaam van uw&gt;* externe foutopsporing (Node.js in Windows-Container)** profileren als de module wordt uitgevoerd als een Windows-container) het IP-adres van de machine foutopsporing wordt uitgevoerd.

- **Java**

  - Bouw een SSH-tunnel naar de machine u fouten wilt opsporen door uit te voeren `ssh -f <username>@<target-machine> -L 5005:127.0.0.1:5005 -N`.
  
  - Op uw ontwikkelcomputer, Visual Studio Code open en bewerk de  ***&lt;de modulenaam van uw&gt;* externe foutopsporing (Java)** profiel `launch.json` zodat u kunt koppelen aan de doel-VM. Voor meer informatie over het bewerken van `launch.json` en opsporen van fouten in Java met Visual Studio Code, Zie de sectie over [configureren van het foutopsporingsprogramma](https://code.visualstudio.com/docs/java/java-debugging#_configuration).

- **Python**

  - Zorg ervoor dat poort 5678 op de machine u fouten wilt opsporen open en toegankelijk is.

  - In de code `ptvsd.enable_attach(('0.0.0.0', 5678))` die u eerder hebt ingevoegd in `main.py`, wijzigen **0.0.0.0** naar het IP-adres van de machine op te geven. Bouwen, push en opnieuw implementeren van uw IoT Edge-module.

  - Open Visual Studio Code op uw ontwikkelcomputer en bewerk vervolgens `launch.json` zodat de `host` waarde van de  ***&lt;de modulenaam van uw&gt;* externe foutopsporing (Python)** profiel gebruikt het IP-adres van de doel-VM in plaats van `localhost`.

### <a name="debug-your-module"></a>Fouten opsporen in uw module

1. Selecteer het configuratiebestand voor de foutopsporing voor de module in de weergave foutopsporing van Visual Studio Code. De naam van de optie foutopsporing zijn vergelijkbaar met  ***&lt;de modulenaam van uw&gt;* externe foutopsporing**

1. Open de module-bestand voor de taal van de ontwikkeling en voeg een onderbrekingspunt toe:

   - **Azure-functie (C#)**: Uw onderbrekingspunt toevoegen aan het bestand `<your module name>.cs`.
   - **C#**: Uw onderbrekingspunt toevoegen aan het bestand `Program.cs`.
   - **Node.js**: Uw onderbrekingspunt toevoegen aan het bestand `app.js`.
   - **Java**: Uw onderbrekingspunt toevoegen aan het bestand `App.java`.
   - **Python**: Uw onderbrekingspunt toevoegen aan het bestand `main.py`in de terugbelmethode waaraan u hebt toegevoegd de `ptvsd.break_into_debugger()` regel.
   - **C**: Uw onderbrekingspunt toevoegen aan het bestand `main.c`.

1. Selecteer **Start Debugging** of selecteer **F5**. Selecteer het proces om aan te koppelen.

1. In de weergave foutopsporing van Visual Studio Code ziet u de variabelen in het linkerdeelvenster.

> [!NOTE]
> Het vorige voorbeeld laat zien hoe fouten opsporen in IoT Edge-modules voor containers. Deze blootgestelde poorten toegevoegd aan de container van de module `createOptions` instellingen. Nadat u klaar bent met het opsporen van fouten in uw modules, wordt u aangeraden verwijderen van deze blootgestelde poorten voor de IoT Edge-modules gereed is voor productie.

## <a name="next-steps"></a>Volgende stappen

Nadat u uw module hebt gebouwd, informatie over hoe u [implementeren van Azure IoT Edge-modules van Visual Studio Code](how-to-deploy-modules-vscode.md).

Voor het ontwikkelen van modules voor uw IoT Edge-apparaten, [begrijpen en gebruiken Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md).
