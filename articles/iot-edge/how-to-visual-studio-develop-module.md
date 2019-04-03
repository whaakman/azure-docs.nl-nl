---
title: Ontwikkelen en testen van modules in Visual Studio - Azure IoT Edge | Microsoft Docs
description: Visual Studio 2017 gebruiken om te ontwikkelen en fouten opsporen in modules voor Azure IoT Edge
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 04/03/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.openlocfilehash: f2228726d4edc25efe46a660d25d398959c3ea59
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851735"
---
# <a name="use-visual-studio-2017-to-develop-and-debug-modules-for-azure-iot-edge-preview"></a>Visual Studio 2017 gebruiken om te ontwikkelen en fouten opsporen in modules voor Azure IoT Edge (Preview)

U kunt uw bedrijfslogica in modules inschakelen voor Azure IoT Edge. Dit artikel ziet u hoe u Visual Studio 2017 gebruikt als het belangrijkste instrument voor het ontwikkelen en fouten opsporen in modules.

De Azure IoT Edge-Tools voor Visual Studio biedt de volgende voordelen:

- Maken, bewerken, samenstellen, uitvoeren en fouten opsporen in Azure IoT Edge-oplossingen en -modules op uw lokale ontwikkelcomputer.
- Uw oplossing Azure IoT Edge implementeren op Azure IoT Edge-apparaat via Azure IoT Hub.
- Uw Azure-IoT-modules in C-code of C# terwijl alle van de voordelen van Visual Studio-ontwikkeling.
- Azure IoT Edge-apparaten en modules met de gebruikersinterface beheren.

Dit artikel leest u hoe de Azure IoT Edge-Tools voor Visual Studio 2017 gebruiken voor het ontwikkelen van uw IoT Edge-modules. U leert ook hoe u uw project implementeren in uw Azure IoT Edge-apparaat.

> [!TIP]
> De structuur van de IoT Edge-project gemaakt met Visual Studio is niet hetzelfde als in Visual Studio Code.
  
## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u een computer of virtuele machine met Windows als uw ontwikkelcomputer. Uw IoT Edge-apparaat mag een andere fysieke apparaat.

Omdat in dit artikel wordt Visual Studio 2017 als de belangrijkste ontwikkeltool gebruikt, Visual Studio installeren. Zorg ervoor dat u de **Azure-ontwikkeling** en **bureaubladontwikkeling met C++** workloads in de installatie van Visual Studio 2017. U kunt [Visual Studio 2017 wijzigen](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) om toe te voegen van de vereiste werkbelastingen.

Nadat uw Visual Studio 2017 klaar is, moet u ook de volgende hulpprogramma's en onderdelen:

- Download en installeer [Azure IoT Edge-extensie (Preview)](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) van de Visual Studio marketplace om een IoT Edge-project in Visual Studio 2017.

- Download en installeer [Docker Community Edition](https://docs.docker.com/install/) op uw ontwikkelmachine wilt bouwen en uitvoeren van uw module-installatiekopieën. U moet Docker CE om uit te voeren in de modus voor Linux-container of Windows-container modus ingesteld.

- Instellen van uw lokale ontwikkelomgeving om op te sporen, uitvoeren en testen van uw IoT Edge-oplossing door het installeren van de [Azure IoT EdgeHub ontwikkeltool](https://pypi.org/project/iotedgehubdev/). Installeer [Python (2.7/3.6) en Pip](https://www.python.org/) en installeer vervolgens de **iotedgehubdev** pakket door het uitvoeren van de volgende opdracht in uw terminal. Zorg ervoor dat uw versie van het Azure IoT EdgeHub Dev hulpprogramma is groter dan 0.3.0.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- Kloon de opslagplaats en installeer de bibliotheek Vcpkg manager en installeert u de **azure-iot-sdk-c-pakket** voor Windows.

  ```cmd
  git clone https://github.com/Microsoft/vcpkg
  cd vcpkg
  bootstrap-vcpkg.bat
  ```

  ```cmd
  vcpkg.exe install azure-iot-sdk-c:x64-windows
  vcpkg.exe --triplet x64-windows integrate install
  ```

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags).

  > [!TIP]
  > U kunt een lokale Docker-register voor prototype en in plaats van een cloud-register voor testdoeleinden gebruiken.

- Als u wilt testen van uw module op een apparaat, moet u een actieve IoT-hub met ten minste één IoT Edge-apparaat. Voor het gebruik van de computer als een IoT Edge-apparaat, volg de stappen in de quickstart voor [Linux](quickstart-linux.md) of [Windows](quickstart.md). Als u een IoT Edge-daemon op uw ontwikkelcomputer uitvoert, moet u mogelijk EdgeHub en EdgeAgent stoppen voordat u beginnen met ontwikkelen in Visual Studio.

### <a name="check-your-tools-version"></a>Controleer uw versie van de hulpprogramma 's

1. Uit de **extra** in het menu **extensies en Updates**. Vouw **geïnstalleerde > Extra** en u vindt **hulpprogramma's voor Azure IoT Edge** en **Cloud Explorer voor Visual Studio**.

1. Houd er rekening mee de geïnstalleerde versie. U kunt deze versie vergelijken met de meest recente versie van Visual Studio Marketplace ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools))

1. Als uw versie ouder is dan wat er beschikbaar is op Visual Studio Marketplace, de hulpprogramma's in Visual Studio bijwerken zoals weergegeven in de volgende sectie.

### <a name="update-your-tools"></a>De hulpprogramma's bijwerken

1. In de **extensies en Updates** dialoogvenster Vouw **Updates > Visual Studio Marketplace**, selecteer **hulpprogramma's voor Azure IoT Edge** of **Cloud Explorer voor visuele element Studio** en selecteer **Update**.

1. Nadat de hulpprogramma's-update is gedownload, sluit u Visual Studio aan de hulpprogramma's bijwerken met behulp van het installatieprogramma VSIX-trigger.

1. Selecteer in het installatieprogramma **OK** te starten en vervolgens **wijzigen** de hulpprogramma's bijwerken.

1. Nadat de update voltooid is, selecteert u **sluiten** en Visual Studio opnieuw te starten.

### <a name="create-an-azure-iot-edge-project"></a>Een Azure IoT Edge-project maken

De Azure IoT Edge-projectsjabloon in Visual Studio maakt een project dat kan worden geïmplementeerd op Azure IoT Edge-apparaten in Azure IoT Hub. U maakt eerst een Azure IoT Edge-oplossing, en vervolgens het genereren van de eerste module in die oplossing. Elke IoT Edge-oplossing kan meer dan één module bevatten.

1. Selecteer **Nieuw** > **Project** in het menu **Bestand** in Visual Studio.

1. In de **nieuw Project** dialoogvenster, selecteer **geïnstalleerde**, selecteer **Azure IoT**, selecteer **Azure IoT Edge**, voer een naam in voor uw project en Geef de locatie en selecteer vervolgens **OK**. De standaardnaam van het project is **AzureIoTEdgeApp1**.

   ![Nieuw project](./media/how-to-visual-studio-develop-csharp-module/create-new.jpg)

1. In de **IoT Edge-toepassing toevoegen en Module** venster **Linux Amd64**, **Windows-Amd64**, of beide als het platform voor toepassingen. Als u beide selecteert, maakt u een oplossing waarbij twee projecten of elk verwijzen naar de standaard-codemodule.

   > [!TIP]
   > De Azure IoT Edge-extensie voor Visual Studio ondersteunt momenteel niet maken projecten voor de ARM-platform. Raadpleeg deze [IoT Developer-blogbericht](https://devblogs.microsoft.com/iotdev/easily-build-and-debug-iot-edge-modules-on-your-remote-device-with-azure-iot-edge-for-vs-code-1-9-0/) voor een voorbeeld van het gebruik van Visual Studio Code voor het ontwikkelen van een oplossing voor ARM32v7/armhf.

1. Selecteer een  **C# Module** of **C Module** en geef vervolgens de modulenaam en -module-opslagplaats voor installatiekopieën. Visual Studio autopopulates de module met naam **localhost:5000 / < modulenaam van uw\>**. Vervang deze door uw eigen gegevens. Als u een lokale Docker-register voor het testen, klikt u vervolgens **localhost** functioneert. Als u Azure Container Registry gebruikt, gebruikt u de aanmeldingsserver van de instellingen van uw register. De aanmeldingsserver ziet eruit als ***\<registernaam\>*. azurecr.io**. Het vervangen van slechts de **localhost:5000** deel uitmaakt van de tekenreeks, zodat het uiteindelijke resultaat ziet als eruit **\<* registernaam*\>.azurecr.io/* \<de modulenaam van uw\>***. De standaardnaam van de module is **IoTEdgeModule1**

1. Selecteer **OK** voor het maken van de Azure IoT Edge-oplossing met een module die gebruikmaakt van een C# of C.

Nu u hebt een **AzureIoTEdgeApp1.Linux.Amd64** project of een **AzureIoTEdgeApp1.Windows.Amd64** project, of beide, en ook een **IoTEdgeModule1** project in uw oplossing. Elke **AzureIoTEdgeApp1** project heeft een `deployment.template.json` -bestand, dat bepaalt de modules die u voor uw IoT Edge-oplossing wilt bouwen en implementeren, en ook bepaalt de routes tussen modules. De standaardoplossing heeft een **tempSensor** module en een **IoTEdgeModule1** module. De **tempSensor** module genereert gesimuleerde gegevens naar de **IoTEdgeModule1** -module, terwijl de standaard-code in de **IoTEdgeModule1** module rechtstreeks pipes ontvangen berichten naar Azure IoT Hub.

De **IoTEdgeModule1** project is een .NET Core 2.1-consoletoepassing. Het bevat de vereiste Docker-bestanden die u nodig hebt voor uw IoT Edge-apparaat met een Windows-container- of Linux-container uitgevoerd. De `module.json` bestand beschrijft de metagegevens van een module. De werkelijke module-code, waarbij Azure IoT Device SDK als een afhankelijkheid is gevonden in de `Program.cs` of `main.c` bestand.

## <a name="develop-your-module"></a>Uw-module ontwikkelen

De standaard-module-code die wordt geleverd met de oplossing bevindt zich in **IoTEdgeModule1** > **Program.cs** (voor C#) of **main.c** (C). De module en de `deployment.template.json` bestand zijn ingesteld, zodat u kunt de oplossing te bouwen, deze naar het containerregister pushen en implementeren op een apparaat om te testen zonder code aan te starten. De module is gebouwd om invoer van een bron (in dit geval de **tempSensor** module die gegevens simuleert) en doorgeven voor Azure IoT Hub.

Wanneer u klaar bent om de module-sjabloon met uw eigen code aanpassen, gebruikt u de [Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md) modules bouwen dat adres de sleutel die nodig zijn voor IoT-oplossingen, zoals beveiliging, beheer van apparaten en betrouwbaarheid.

## <a name="initialize-iotedgehubdev-with-iot-edge-device-connection-string"></a>Initialiseren van iotedgehubdev met IoT Edge-apparaat-verbindingsreeks

1. Kopieer de verbindingsreeks van elk IoT Edge-apparaat uit **Primary Connection String** in Visual Studio Cloud Explorer. Zorg ervoor dat u niet kopiëren van de verbindingsreeks van een niet-Microsoft Edge-apparaat, zoals het pictogram van een IoT Edge-apparaat af van het pictogram van een niet-Edge-apparaat wijkt.

   ![Edge-apparaat-verbindingsreeks kopiëren](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

1. Met de rechtermuisknop op de **AzureIoTEdgeApp1** project en klik vervolgens op **verbindingsreeks van de Edge-apparaat instellen** om de Azure IoT Edge-setup-venster te openen.

   ![Venster tekenreeks Edge-verbinding instellen openen](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

1. Geef de verbindingsreeks van de eerste stap en selecteer vervolgens **OK**.

> [!NOTE]
> U moet deze stappen wordt slechts eenmaal op uw ontwikkelingscomputer als de resultaten worden automatisch toegepast op alle volgende Azure IoT Edge-oplossingen. Deze procedure kan opnieuw worden uitgevoerd als u wilt wijzigen naar een andere verbinding-tekenreeks.

## <a name="build-and-debug-single-module"></a>Bouwen en fouten opsporen in één module

Normaal gesproken moet u om te testen en fouten opsporen in elke module voordat het wordt uitgevoerd binnen een complete oplossing met meerdere modules.

1. Met de rechtermuisknop op **IoTEdgeModule1** en selecteer **Set as StartUp Project** in het contextmenu.

   ![Opstartproject instellen](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

1. Druk op **F5** of klik op de knop hieronder om uit te voeren van de module; het duurt 10&ndash;20 seconden de eerste keer dat u dat doen.

   ![Module uitvoeren](./media/how-to-visual-studio-develop-csharp-module/run-module.png)

1. Hier ziet u een .NET Core-consoletoepassing die is gestart als de module is geïnitialiseerd.

   ![Module uitgevoerd](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

1. Als ontwikkelt C#, stel een onderbrekingspunt in de `PipeMessage()` werken in **Program.cs**; als met c#, stel een onderbrekingspunt in de `InputQueue1Callback()` werken in **main.c**. U kunt deze vervolgens testen door een bericht verzenden door de volgende opdracht uit te voeren een **Git Bash** of **WSL Bash** shell. (Kan niet worden uitgevoerd de `curl` opdracht uit vanaf een PowerShell of opdrachtprompt.)

    ```bash
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Fouten opsporen in één Module](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Het onderbrekingspunt moet worden geactiveerd. U kunt variabelen in de Visual Studio bekijken **Locals** venster.

   > [!TIP]
   > U kunt ook [PostMan](https://www.getpostman.com/) of andere API's voor het verzenden van berichten in plaats van `curl`.

1. Druk op **Ctrl + F5** of klik op de stopknop stop de foutopsporing.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Bouwen en fouten opsporen in IoT Edge-oplossing met meerdere modules

Nadat u bent met klaar het ontwikkelen van een één-module, u mogelijk wilt uitvoeren en fouten opsporen in een complete oplossing met meerdere modules.

1. Een tweede module toevoegen aan de oplossing met de rechtermuisknop op **AzureIoTEdgeApp1** en selecteren **toevoegen** > **nieuwe IoT Edge-Module**. De standaardnaam van de tweede module is **IoTEdgeModule2** en fungeert als een andere pipe-module.

1. Open het bestand `deployment.template.json` en ziet u **IoTEdgeModule2** is toegevoegd aan de **modules** sectie. Vervang de **routes** sectie met de volgende stappen. Als u de modulenamen van de hebt aangepast, zorg er dan voor dat u deze namen zodat deze overeenkomen met bijwerken.

    ```json
        "routes": {
          "IoTEdgeModule1ToIoTHub": "FROM /messages/modules/IoTEdgeModule1/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule1": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule1/inputs/input1\")",
          "IoTEdgeModule2ToIoTHub": "FROM /messages/modules/IoTEdgeModule2/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule2": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule2/inputs/input1\")"
        },
    ```

1. Met de rechtermuisknop op **AzureIoTEdgeApp1** en selecteer **Set as StartUp Project** in het contextmenu.

1. Maken van uw onderbrekingspunten en druk vervolgens op **F5** uitvoeren en fouten opsporen tegelijkertijd in meerdere modules. U ziet meerdere .NET Core console app windows, die elk venster dat een andere module vertegenwoordigt.

   ![Fouten opsporen in meerdere Modules](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

1. Druk op **Ctrl + F5** of Selecteer de knop Stoppen om te stoppen met het opsporen van fouten.

## <a name="build-and-push-images"></a>Bouw en installatiekopieën pushen

1. Zorg ervoor dat **AzureIoTEdgeApp1** is het opstartproject. Selecteer **Debug** of **Release** als de configuratie om te bouwen voor uw module-installatiekopieën.

    > [!NOTE]
    > Bij het kiezen van **Debug**, Visual Studio gebruikt `Dockerfile.(amd64|windows-amd64).debug` Docker-installatiekopieën maken. Dit omvat het opdrachtregelprogramma foutopsporingsprogramma van .NET Core VSDBG in uw containerinstallatiekopie tijdens het maken van deze. Voor de IoT Edge-modules gereed is voor productie, raden wij aan dat u de **Release** configuratie, die gebruikmaakt van `Dockerfile.(amd64|windows-amd64)` zonder VSDBG.

1. Als u een persoonlijk register, zoals Azure Container Registry gebruikt, gebruikt u de volgende Docker-opdracht te melden bij het. Als u lokale register gebruikt, kunt u [uitvoeren van een lokale register](https://docs.docker.com/registry/deploying/#run-a-local-registry).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server>
    ```

1. Als u een persoonlijk register, zoals Azure Container Registry gebruikt, moet u de aanmeldingsgegevens voor uw register toevoegen aan de runtime-instellingen vindt u in het bestand `deployment.template.json`. Vervang de tijdelijke aanduidingen door uw werkelijke ACR gebruikersnaam, wachtwoord en registry beheerdersnaam.

    ```json
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
              "registry1": {
                "username": "<username>",
                "password": "<password>",
                "address": "<registry name>.azurecr.io"
              }
            }
          }
    ```

1. Met de rechtermuisknop op **AzureIoTEdgeApp1** en selecteer **Build and Push Edge oplossing** te bouwen en push de Docker-installatiekopie voor elke module.

   ![Bouw en installatiekopieën pushen](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)

## <a name="deploy-the-solution"></a>De oplossing implementeren

In dit snelstartartikel voor het instellen van uw IoT Edge-apparaat hebt u een module geïmplementeerd met behulp van de Azure-portal. U kunt ook modules met behulp van de Cloud Explorer voor Visual Studio implementeren. U hebt al een implementatie-manifest voorbereid voor uw scenario, de `deployment.json` bestands- en hoeft u alleen maar is een apparaat voor het ontvangen van de implementatie selecteert.

1. Open **Cloud Explorer** door te klikken op **weergave** > **Cloud Explorer**. Zorg ervoor dat u zich hebt aangemeld bij Visual Studio 2017.

1. In **Cloud Explorer**uit, vouw uw abonnement, zoeken naar uw Azure-IoT-Hub en de Azure IoT Edge-apparaat dat u wilt implementeren.

1. Met de rechtermuisknop op het IoT Edge-apparaat een implementatie voor het maken, moet u het manifestbestand van de implementatie onder de `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json`.

   > [!NOTE]
   > U moet niet selecteren `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json`

1. Klik op de vernieuwknop om te zien van de nieuwe modules uitvoeren samen met de **TempSensor** module en **$edgeAgent** en **$edgeHub**.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

1. Voor het controleren van het bericht D2C voor een specifiek apparaat, selecteert u het apparaat in de lijst en klik vervolgens op **Start Monitoring D2C berichten** in de **actie** venster.

1. Als u wilt stoppen door gegevens te controleren, selecteert u het apparaat in de lijst en selecteer vervolgens **Stop Monitoring D2C berichten** in de **actie** venster.

## <a name="next-steps"></a>Volgende stappen

Voor het ontwikkelen van aangepaste modules voor uw IoT Edge-apparaten, [begrijpen en gebruiken Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md).
