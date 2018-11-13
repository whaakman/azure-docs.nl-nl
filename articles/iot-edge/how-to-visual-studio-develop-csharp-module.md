---
title: Ontwikkelen en fouten opsporen in C#-modules voor Azure IoT Edge in Visual Studio 2017 | Microsoft Docs
description: Visual Studio 2017 gebruiken om te ontwikkelen en fouten opsporen in C#-module voor Azure IoT Edge
services: iot-edge
keywords: ''
author: shizn
manager: philmea
ms.author: xshi
ms.date: 09/24/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 091eb6af871cc53dff3459c55c668e8ffb227984
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567465"
---
# <a name="use-visual-studio-2017-to-develop-and-debug-c-modules-for-azure-iot-edge-preview"></a>Visual Studio 2017 gebruiken om te ontwikkelen en fouten opsporen in C#-modules voor Azure IoT Edge (Preview)

U kunt uw bedrijfslogica in modules inschakelen voor Azure IoT Edge. Dit artikel ziet u hoe u Visual Studio 2017 gebruikt als het belangrijkste instrument voor het ontwikkelen en fouten opsporen in C#-modules.

De Azure IoT Edge-Tools voor Visual Studio biedt de volgende voordelen:

- Maken, bewerken, samenstellen, uitvoeren en fouten opsporen in Azure IoT Edge-oplossingen en -modules op uw lokale ontwikkelcomputer.
- Uw oplossing Azure IoT Edge implementeren op Azure IoT Edge-apparaat via Azure IoT Hub.
- Uw Azure-IoT-modules in C#-code terwijl alle van de voordelen van Visual Studio-ontwikkeling.
- Azure IoT Edge-apparaten en modules met de gebruikersinterface beheren. 

Dit artikel leest u hoe de Azure IoT Edge-Tools voor Visual Studio 2017 gebruiken voor het ontwikkelen van uw IoT Edge-modules in C#. U leert ook hoe u uw project implementeren in uw Azure IoT Edge-apparaat.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u een computer of virtuele machine met Windows als uw ontwikkelcomputer. Uw IoT Edge-apparaat mag een andere fysieke apparaat.

Omdat in dit artikel wordt Visual Studio 2017 als de belangrijkste ontwikkeltool gebruikt, Visual Studio installeren. En zorg ervoor dat u **Azure-ontwikkelworkload** in de installatie van Visual Studio 2017. U kunt [Visual Studio 2017 wijzigen](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) en Azure-ontwikkelworkload toe te voegen.

Nadat uw Visual Studio 2017 klaar is, moet u ook:

- Download en installeer [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) van Visual Studio marketplace om IoT Edge-project in Visual Studio 2017.
- [Docker Community Edition](https://docs.docker.com/install/) op uw ontwikkelmachine wilt bouwen en uitvoeren van uw module-installatiekopieën. U moet de Docker CE in Linux-container of Windows-container modus uitgevoerd correct ingesteld.
- Als u lokale ontwikkelomgeving, zodat fouten opsporen, uitvoeren en testen van uw IoT Edge-oplossing instelt, moet u [Azure IoT EdgeHub ontwikkeltool](https://pypi.org/project/iotedgehubdev/). Installeer [Python (2.7/3.6) en Pip](https://www.python.org/). Installeer vervolgens **iotedgehubdev** onderstaande opdracht in de terminal service wordt uitgevoerd. Zorg ervoor dat uw versie van het Azure IoT EdgeHub Dev hulpprogramma is groter dan 0.3.0.

   ```cmd
   pip install --upgrade iotedgehubdev
   ```

- [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) of [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags)
   - U kunt een lokale Docker-register voor prototype en in plaats van een cloud-register voor testdoeleinden gebruiken. 

- Als u wilt uw module testen, moet u een actieve IoT-hub met ten minste één IoT Edge-apparaat-ID gemaakt. Als u een IoT Edge security-daemon op de ontwikkelcomputer uitvoert, moet u mogelijk EdgeHub en EdgeAgent stoppen voordat u beginnen met ontwikkelen in Visual Studio. 

### <a name="check-your-tools-version"></a>Controleer uw versie van de hulpprogramma 's

1. Uit de **extra** menu, kiest u **extensies en Updates**. Vouw **geïnstalleerde > Extra** en u vindt **Azure IoT Edge** en **Cloud Explorer**.

2. Houd er rekening mee de geïnstalleerde versie. U kunt deze versie vergelijken met de meest recente versie van Visual Studio Marketplace ([Cloud Explorer](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.CloudExplorerForVS), [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.azureiotedgetools))

3. Als uw versie ouder is, werkt u uw hulpprogramma's in Visual Studio zoals wordt weergegeven in de volgende sectie.

### <a name="update-your-tools"></a>De hulpprogramma's bijwerken

1. In de **extensies en Updates** dialoogvenster Vouw **Updates > Visual Studio Marketplace**, kiest u **Azure IoT Edge** of **Cloud Explorer**en selecteer **Update**.

2. Nadat de hulpprogramma's-update is gedownload, sluit u Visual Studio aan de hulpprogramma's bijwerken met behulp van het installatieprogramma VSIX-trigger.

3. Kies in het installatieprogramma **OK** om te starten en vervolgens wijzigen om de hulpprogramma's bijwerken.

4. Nadat de update voltooid is, kiest u sluiten en opnieuw opstarten van Visual Studio.

### <a name="create-an-azure-iot-edge-project"></a>Een Azure IoT Edge-project maken

De Azure IoT Edge-projectsjabloon in Visual Studio maakt een project dat kan worden geïmplementeerd op Azure IoT Edge-apparaten in Azure IoT Hub. U maakt eerst een Azure IoT Edge-oplossing, en vervolgens het genereren van de eerste C#-module in die oplossing. Elke IoT Edge-oplossing kan meer dan één module bevatten. 

1. Selecteer **Nieuw** > **Project** in het menu **Bestand** in Visual Studio.

2. In de **nieuw Project** dialoogvenster, selecteer **geïnstalleerde**, vouw **Visual C# > Cloud**, selecteer **Azure IoT Edge**, typ een  **Naam** voor uw project en geef de locatie en op **OK**. De standaardnaam van het project is **AzureIoTEdgeApp1**. 

   ![Nieuw project](./media/how-to-visual-studio-develop-csharp-module/create-new.jpg)

3. In de **configuratie van de IoT Edge Module** venster **C#-Module** typt en geef uw modulenaam en de module-opslagplaats voor installatiekopieën.  VS autopopulates de module met naam **localhost:5000**. Vervang deze door uw eigen gegevens. Als u een lokale Docker-register voor het testen, klikt u vervolgens is ' localhost ' geen probleem. Als u Azure Container Registry gebruikt, gebruikt u de aanmeldingsserver van de instellingen van uw register. De aanmeldingsserver ziet eruit als  **<registry name>. azurecr.io**. Vervang alleen het onderdeel localhost van de tekenreeks, verwijder niet de naam van de module. De standaardnaam van de module is **IoTEdgeModule1**

4. Klik op **OK** de Azure IoT Edge-project maken met een C#-module.

Nu u hebt een **AzureIoTEdgeApp1** project en een **IoTEdgeModule1** project in onze oplossing. De **AzureIoTEdgeApp1** project heeft een **deployment.template.json** bestand. Dit bestand definieert de modules die u voor uw IoT Edge-oplossing wilt bouwen en implementeren, en definieert de routes tussen modules. De standaardoplossing heeft een **tempSensor** module en een **IoTEdgeModule1** module. De **tempSensor** module genereert gesimuleerde gegevens **IoTEdgeModule1** -module, terwijl de standaard-code in **IoTEdgeModule1** is een module van het bericht pipe die rechtstreeks pipe ontvangen berichten naar IoT Hub.

De **IoTEdgeModule1** project is een .net Core-consoletoepassing 2.1. Deze bevat vereist **docker-bestanden** u nodig hebt voor uw IoT Edge-apparaat met Windows-container of Linux-container uitgevoerd. De **module.json** bestand beschrijft de metagegevens van een module. De **program.cs** is de daadwerkelijke module-code, waarbij Azure IoT Device SDK als een afhankelijkheid.

## <a name="develop-your-module"></a>Uw-module ontwikkelen

De standaard C#-module-code die wordt geleverd met de oplossing bevindt zich in **IoTEdgeModule1** > **Program.cs**. De module en het bestand deployment.template.json zijn ingesteld zodat u kunt de oplossing te bouwen, deze naar het containerregister pushen en implementeren op een apparaat om te testen zonder code aan te starten. De module is gebouwd om te gewoon nemen invoer van een bron (in dit geval de tempSensor-module die gegevens simuleert) en doorgeven naar IoT Hub. 

Wanneer u klaar bent om de sjabloon C# met uw eigen code aanpassen, gebruikt u de [Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md) modules bouwen dat adres de sleutel die nodig zijn voor IoT-oplossingen, zoals beveiliging, beheer van apparaten en betrouwbaarheid. 

## <a name="initialize-iotegehubdev-with-iot-edge-device-connection-string"></a>Initialiseren **iotegehubdev** met IoT Edge-apparaat-verbindingsreeks

1. U moet ophalen van de verbindingsreeks van elk IoT Edge-apparaat, mag u de waarde van "Primary Connection String" kopiëren vanuit Cloud Explorer in Visual Studio 2017 als volgt. Controleer de verbindingsreeks van niet-Edge-apparaat niet kopiëren, het pictogram van IoT Edge-apparaat wijkt af van een van de niet-Edge-apparaat.

   ![Edge-apparaat-verbindingsreeks kopiëren](./media/how-to-visual-studio-develop-csharp-module/copy-edge-conn-string.png)

2. U moet Klik met de rechtermuisknop op **AzureIoTEdgeApp1** project openen in het contextmenu en klik vervolgens op **verbindingsreeks van de Edge-apparaat instellen**, venster van de installatie van Azure IoT Edge wordt weergegeven.

   ![Venster tekenreeks Edge-verbinding instellen openen](./media/how-to-visual-studio-develop-csharp-module/set-edge-conn-string.png)

3. In het instellingenvenster u hebt verkregen in de eerste stap verbindingsreeks invoeren, en klikt u op **OK** knop.

>[!NOTE]
>Dit is eenmalig werken, u alleen moet worden uitgevoerd in deze stap één keer op één computer, alle volgende Azure IoT Edge oplossingen krijgt als gratis. Natuurlijk kan u deze stap opnieuw uitvoeren als de verbindingsreeks ongeldig is of u wilt wijzigen in een andere verbindingstekenreeks.

## <a name="build-and-debug-single-c-module"></a>Bouwen en fouten opsporen in één C#-module

Normaal gesproken gaan we testen/debug elke module voordat we maken het die binnen een complete oplossing met meerdere modules worden uitgevoerd.

1. Selecteer **IoTEdgeModule1** als het opstartproject in het contextmenu.

   ![Opstartproject instellen](./media/how-to-visual-studio-develop-csharp-module/module-start-up-project.png)

2. Druk op **F5** of klik op de knop hieronder om de module worden uitgevoerd, duurt het 10 ~ 20 seconden voor de eerste keer.

   ![Module uitvoeren](./media/how-to-visual-studio-develop-csharp-module/run-module.png)


3. Er is een .net Core-console-app wordt gestart als de module is geïnitialiseerd.

   ![Module uitgevoerd](./media/how-to-visual-studio-develop-csharp-module/single-module-run.png)

4. Nu kunt u een onderbrekingspunt instellen in **PipeMessage** in **Program.cs**, vervolgens bericht te verzenden met de volgende opdracht uw **Git Bash** of **WSL Bash**  (Voer het niet in CMD of Powershell) (u vindt hier ook deze opdracht in het uitvoervenster van Visual Studio):

    ```cmd
    curl --header "Content-Type: application/json" --request POST --data '{"inputName": "input1","data":"hello world"}' http://localhost:53000/api/v1/messages
    ```

   ![Fouten opsporen in één Module](./media/how-to-visual-studio-develop-csharp-module/debug-single-module.png)

    Het onderbrekingspunt moet worden geactiveerd. U kunt variabelen in Visual Studio-variabelen kunt bekijken.

   > [!TIP]
   > U kunt ook [PostMan](https://www.getpostman.com/) of andere API's voor het verzenden van berichten via in plaats van `curl`.

5. Druk op **Ctrl + F5** of klik op de stopknop stop de foutopsporing.

## <a name="build-and-debug-iot-edge-solution-with-multiple-modules"></a>Bouwen en fouten opsporen in IoT Edge-oplossing met meerdere modules

Als we klaar bent met het ontwikkelen van één module vervolgens gaan we uitvoeren en fouten opsporen in de gehele oplossing met meerdere modules.

1. Uw tweede C#-module toevoegen aan de oplossing. Met de rechtermuisknop op **AzureIoTEdgeApp1** en selecteer **toevoegen** -> **nieuwe IoT Edge-Module**. De standaardnaam van de tweede module is **IoTEdgeModule2** en het is nog steeds een pipe-module.

2. Navigeer naar **deployment.template.json**. U ziet **IoTEdgeModule2** is toegevoegd aan **modules** sectie. Vervang de **routes** sectie met de volgende stappen. Als u de modulenamen van de hebt aangepast, zorg er dan voor dat u de namen in het volgende na vervanging bijwerken.

    ```json
        "routes": {
          "IoTEdgeModule1ToIoTHub": "FROM /messages/modules/IoTEdgeModule1/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule1": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule1/inputs/input1\")",
          "IoTEdgeModule2ToIoTHub": "FROM /messages/modules/IoTEdgeModule2/outputs/* INTO $upstream",
          "sensorToIoTEdgeModule2": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/IoTEdgeModule2/inputs/input1\")"
        },
    ```

3. Stel **AzureIoTEdgeApp1** project als opstartproject in het contextmenu.

4. Onderbrekingspunten instellen en druk op F5, kunt u uitvoeren en fouten opsporen tegelijkertijd in meerdere modules. U ziet meerdere .net Core console app windows, elk venster geeft aan dat uw C#-module. 

   ![Fouten opsporen in meerdere Modules](./media/how-to-visual-studio-develop-csharp-module/debug-multiple-modules.png)

5. Druk op **Ctrl + F5** of klik op de stopknop stop de foutopsporing.

## <a name="build-and-push-images"></a>Bouw en installatiekopieën pushen

1. Zorg ervoor dat **AzureIoTEdgeApp1** uw opstartproject is. Selecteer **Debug** of **Release** configuratie voor uw module-installatiekopieën te bouwen.

    > [!NOTE]
    > Bij het kiezen van **Debug**, Visual Studio gebruikt `Dockerfile.debug` Docker-installatiekopieën maken. Dit omvat het opdrachtregelprogramma foutopsporingsprogramma van .NET Core VSDBG in uw containerinstallatiekopie tijdens het maken van deze. Het is raadzaam dat u **Release** -configuratie die gebruikmaakt van `Dockerfile` zonder VSDBG voor IoT Edge-modules gereed is voor productie.

2. Als u privéregister zoals Azure Container Registry gebruikt, voert u Docker-Meld u aan met de volgende opdracht uit in de terminal. Als u lokale register gebruikt, kunt u [uitvoeren van een lokale register](https://docs.docker.com/registry/deploying/#run-a-local-registry).

    ```cmd
    docker login -u <ACR username> -p <ACR password> <ACR login server> 
    ```

3. Als u een privéregister zoals Azure Container Registry gebruikt, moet u plaatsen van register-gebruikersnaam en wachtwoord in `deployment.template.json` onder runtime-instellingen met de volgende inhoud. Vergeet niet dat u de tijdelijke aanduiding vervangt door uw werkelijke admin-gebruikersnaam en wachtwoord.

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

4. Klik met de rechtermuisknop **AzureIoTEdgeApp1** en kies context menu-item **Build and Push Edge oplossing**, het bouwen en push docker-installatiekopie voor elke module.

   ![Bouw en installatiekopieën pushen](./media/how-to-visual-studio-develop-csharp-module/build-and-push.png)


## <a name="deploy-the-solution"></a>De oplossing implementeren

In dit snelstartartikel voor het instellen van uw IoT Edge-apparaat hebt u een module geïmplementeerd met behulp van de Azure-portal. U kunt ook modules met behulp van de Cloud Explorer voor Visual Studio implementeren. U hebt al een implementatie-manifest voorbereid voor uw scenario, de `deployment.json` bestand. U hoeft nu alleen nog maar een apparaat te selecteren dat de implementatie moet ontvangen.

1. Open **Cloud Explorer** door te klikken op **weergave** > **Cloud Explorer**. Zorg ervoor dat u bent aangemeld in Visual Studio 2017 met uw account.

2. In **Cloud Explorer**uit, vouw uw abonnement, zoeken naar uw Azure-IoT-Hub en de Azure IoT Edge-apparaat dat u wilt implementeren.

3. Met de rechtermuisknop op IoT Edge-apparaat aan de implementatie voor het maken, moet u ervoor kiezen het manifestbestand van de implementatie onder de `$AzureIoTEdgeAppSolutionDir\config\deployment.(amd64|amd64.debug|windows-amd64).json`.

>>[!NOTE]
>>U moet niet selecteren `$AzureIoTEdgeAppSolutionDir\config\deployment_for_local_debug.json`

4. Klik op de knop Vernieuwen. U ziet nu de nieuwe modules uitvoeren samen met de **TempSensor** module en de **$edgeAgent** en **$edgeHub**.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

1. Voor het controleren van het bericht D2C voor een specifiek apparaat, klikt u op het apparaat in de lijst en klikt u op **Start Monitoring D2C berichten** in **actie** venster.

2. Als u wilt stoppen door gegevens te controleren, klikt u op het apparaat in de lijst en selecteer **Stop Monitoring D2C berichten** in **actie** venster.

## <a name="next-steps"></a>Volgende stappen

Voor het ontwikkelen van modules voor uw IoT Edge-apparaten, [begrijpen en gebruiken Azure IoT Hub SDK's](../iot-hub/iot-hub-devguide-sdks.md).
