---
title: 'Voor Windows-apparaten: Azure IoT Edge-module ontwikkelen | Microsoft Docs'
description: Deze zelfstudie leidt u door het instellen van uw machine en cloud resources voor het ontwikkelen voor het ontwikkelen van IoT Edge-modules met behulp van Windows-containers voor Windows-apparaten
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/20/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 631338c0217eb61f4f98cd06ffa16cb2500f246b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66146738"
---
# <a name="tutorial-develop-iot-edge-modules-for-windows-devices"></a>Zelfstudie: Ontwikkelen van IoT Edge-modules voor Windows-apparaten

Visual Studio 2017 gebruiken om te ontwikkelen en implementeren van code op Windows-apparaten met IoT Edge.

In de Quick Start, moet u een IoT Edge-apparaat met een Windows-machine gemaakt en geïmplementeerd een vooraf samengestelde module op basis van de Azure Marketplace. Deze zelfstudie leert wat het duurt om te ontwikkelen en implementeren van uw eigen code in een IoT Edge-apparaat. Deze zelfstudie is een nuttig vereiste voor alle de andere zelfstudies die u meer informatie over specifieke programmeertalen of Azure-services krijgt. 

Deze zelfstudie wordt gebruikgemaakt van het voorbeeld van de implementatie van een **C-module op een Windows-apparaat**. In dit voorbeeld is gekozen voor de eenvoudig te houden, zodat u kunt meer informatie over de ontwikkelhulpprogramma's zonder dat u of u de juiste bibliotheken geïnstalleerd hebt. Als u inzicht in de ontwikkeling van concepten, kunt klikt u vervolgens u uw voorkeurstaal of Duik in de details van de Azure-service. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van uw ontwikkelcomputer.
> * Gebruik de IoT Edge-hulpprogramma's voor Visual Studio 2017 een nieuw project te maken.
> * Bouw uw project als een container en op te slaan in een Azure container registry.
> * Uw code implementeren in een IoT Edge-apparaat. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="key-concepts"></a>Belangrijkste concepten

Deze zelfstudie leidt u door de ontwikkeling van een IoT Edge-module. Een *IoT Edge-module*, of gewoon soms *module* voor korte, is een container waarin uitvoerbare code bevat. U kunt een of meer modules voor een IoT Edge-apparaat implementeren. Modules specifieke taken uitvoeren, zoals gegevens opnemen van sensors, gegevensanalyse of gegevens reinigen of het verzenden van berichten naar een IoT-hub. Zie voor meer informatie, [inzicht in Azure IoT Edge-modules](iot-edge-modules.md).

Bij het ontwikkelen van IoT Edge-modules, is het belangrijk om te begrijpen van het verschil tussen de ontwikkelcomputer en het doel IoT Edge-apparaat waarop de module uiteindelijk zal worden geïmplementeerd. De container die u bouwt voor het opslaan van uw code van de module moet overeenkomen met het besturingssysteem (OS) van de *doelapparaat*. Voor de ontwikkeling van Windows-containers is dit concept eenvoudiger, omdat Windows-containers alleen op Windows-besturingssystemen worden uitgevoerd. Maar u kunt uw Windows-ontwikkelcomputer bijvoorbeeld gebruiken om te bouwen-modules voor Linux IoT Edge-apparaten. In dit scenario moet u om ervoor te zorgen dat uw ontwikkelcomputer Linux-containers werd uitgevoerd. Als u deze zelfstudie doorloopt, houd rekening met het verschil tussen *ontwikkelcomputer OS* en de *container OS*.

In deze zelfstudie is bedoeld voor Windows-apparaten met IoT Edge. Windows IoT Edge-apparaten Windows-containers gebruiken. U wordt aangeraden gebruik van Visual Studio 2017 voor het ontwikkelen voor Windows-apparaten, dit is wat deze zelfstudie wordt gebruikt. U kunt ook Visual Studio Code gebruiken maar er verschillen in de ondersteuning tussen de twee hulpprogramma's zijn.

De volgende tabel bevat de ondersteunde scenario's voor **Windows containers** in Visual Studio Code en Visual Studio 2017.

|   | Visual Studio Code | Visual Studio 2017 |
| - | ------------------ | ------------------ |
| **Azure-services** | Azure Functions <br> Azure Stream Analytics |   |
| **Talen** | C#(foutopsporing niet ondersteund) | C <br> C# |
| **Meer informatie** | [Azure IoT Edge voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [Azure IoT Edge-hulpprogramma's voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) |

In deze zelfstudie leert de stappen ontwikkeling voor Visual Studio 2017. Als u in plaats van Visual Studio Code gebruikt, raadpleegt u de instructies in [gebruik van Visual Studio Code te ontwikkelen en fouten opsporen in modules voor Azure IoT Edge](how-to-vs-code-develop-module.md).

## <a name="prerequisites"></a>Vereisten

Een ontwikkelcomputer:

* Windows 10 met 1809 update of hoger.
* U kunt uw eigen computer of op een virtuele machine, afhankelijk van uw voorkeuren voor ontwikkeling.
* Installeer [Git](https://git-scm.com/). 
* De Azure IoT C SDK voor Windows x64 via vcpkg installeren:

   ```powershell
   git clone https://github.com/Microsoft/vcpkg
   cd vcpkg
   .\bootstrap-vcpkg.bat
   .\vcpkg install azure-iot-sdk-c:x64-windows
   .\vcpkg --triplet x64-windows integrate install
   ```

<!--vcpkg only required for C development-->

Een Azure IoT Edge-apparaat op Windows:

* U wordt aangeraden dat u niet IoT Edge worden uitgevoerd op uw ontwikkelcomputer, maar in plaats daarvan een afzonderlijk apparaat gebruiken. Dit onderscheid tussen ontwikkelcomputer en IoT Edge-apparaat nauwkeurig komt overeen met een waarde true implementatiescenario en de andere concepten rechte blijft.
* Als u niet een tweede apparaat beschikbaar hebt, gebruikt u artikel te maken van een IoT Edge-apparaat in Azure met een [Windows virtuele machine](quickstart.md).

Cloudresources:

* Een gratis of standard-laag [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) in Azure. 

## <a name="install-container-engine"></a>Container-engine installeren

IoT Edge-modules worden geleverd als containers, dus moet u een container-engine op uw ontwikkelcomputer bouwen en beheren van de containers. Het is raadzaam om met behulp van Docker-Desktop voor de ontwikkeling van vanwege de vele functies en de populariteit als een container-engine. Met Docker-bureaublad op een Windows-computer, kunt u schakelen tussen de Linux-containers en Windows-containers zodat u eenvoudig modules voor verschillende soorten IoT Edge-apparaten kunt ontwikkelen. 

Gebruik de Docker-documentatie om te installeren op uw ontwikkelcomputer: 

* [Docker-Desktop voor Windows installeren](https://docs.docker.com/docker-for-windows/install/)

  * Wanneer u Desktop Docker voor Windows installeert, wordt u gevraagd of u wilt gebruiken, Linux of Windows-containers. Voor deze zelfstudie gebruiken we **Windows containers**. Zie voor meer informatie, [schakelen tussen Windows en Linux-containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).


## <a name="set-up-visual-studio-and-tools"></a>Instellen van Visual Studio en hulpprogramma 's

De IoT-extensies voor Visual Studio 2017 gebruiken voor het ontwikkelen van IoT Edge-modules. Deze uitbreidingen bevatten projectsjablonen, het maken van het manifest implementatie automatiseren en kunnen u controleren en beheren van IoT Edge-apparaten. In deze sectie kunt u Visual Studio en de IoT Edge-extensie installeren en vervolgens uw Azure-account instellen voor het beheren van IoT Hub-resources vanuit Visual Studio. 

1. Als u geen Visual Studio al op uw ontwikkelcomputer hebt [Visual Studio 2017 installeren](https://docs.microsoft.com/visualstudio/install/install-visual-studio?view=vs-2017) met de volgende workloads: 

   * Azure-ontwikkeling
   * Desktopontwikkeling metC++
   * Platformoverschrijdende ontwikkelmogelijkheden van .NET Core

1. Als u Visual Studio 2017 al op uw ontwikkelcomputer hebt, zorg ervoor dat de versie 15.7 of hoger. Volg de stappen in [Visual Studio wijzigen](https://docs.microsoft.com/visualstudio/install/modify-visual-studio?view=vs-2017) om toe te voegen van de vereiste werkbelastingen als u nog geen hebt.

2. Download en installeer de [hulpprogramma's voor Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) -extensie voor Visual Studio 2017. 

3. Wanneer uw installaties voltooid zijn, opent u Visual Studio.

4. Selecteer **weergave** > **Cloud Explorer**. 

5. Selecteer het pictogram van het profiel in de cloud explorer en aanmelden bij uw Azure-account als u al zijn niet aangemeld. 

6. Als u zich aanmeldt, wordt uw Azure-abonnementen worden weergegeven. Selecteer de abonnementen die u wilt openen via de cloud explorer en selecteer vervolgens **toepassen**. 

7. Vouw uw abonnement, klikt u vervolgens **IoT-Hubs**, klikt u vervolgens uw IoT-hub. U ziet een lijst met uw IoT-apparaten en kunt deze explorer gebruiken om deze te beheren. 

   ![Toegang tot IoT Hub-resources in de Cloud Explorer](./media/tutorial-develop-for-windows/cloud-explorer-view-hub.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>Maak een nieuw project in de module

De hulpprogramma's voor Azure IoT Edge-extensie biedt projectsjablonen voor alle ondersteunde IoT Edge module talen in Visual Studio 2017. Deze sjablonen zijn alle bestanden en code die u nodig hebt voor het implementeren van een module werken als u wilt testen van IoT Edge of geeft u een beginpunt voor het aanpassen van de sjabloon met uw eigen bedrijfslogica. 

1. Visual Studio als beheerder uitvoeren.

2. Selecteer **Bestand** > **Nieuw** > **Project**. 

3. Selecteer in het venster Nieuw project, de **Azure IoT** type toepassingsproject en kies de **Azure IoT Edge** project. Wijzig de naam van het project en de oplossing of accepteer de standaardwaarde **AzureIoTEdgeApp1**. Selecteer **OK** om het project te maken. 

   ![Een nieuw Azure IoT Edge-project maken](./media/tutorial-develop-for-windows/new-project.png)

4. In de IoT Edge-toepassing en het modulevenster, configureert u uw project met de volgende waarden: 

   | Veld | Value |
   | ----- | ----- |
   | Platform voor toepassingen | Schakel het selectievakje **Linux Amd64**, en Controleer **WindowsAmd64**. |
   | Selecteer een sjabloon | Selecteer **C Module**. | 
   | Naam van de module-project | Accepteer de standaardwaarde **IoTEdgeModule1**. | 
   | Opslagplaats voor docker-installatiekopieën | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. Uw containerinstallatiekopie wordt vooraf ingevuld vanuit de waarde van de naam van de module project. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br> De opslagplaats voor de uiteindelijke installatiekopie ziet eruit als \<registernaam\>.azurecr.io/iotedgemodule1. |

   ![Configureren van uw project voor het doelapparaat en module-type containerregister](./media/tutorial-develop-for-windows/add-application-and-module.png)

5. Selecteer **OK** de wijzigingen worden toegepast. 

Zodra het nieuwe project wordt geladen in het venster Visual Studio, even de tijd om vertrouwd te raken met de bestanden dat is gemaakt: 

* Een IoT Edge-project met de naam **AzureIoTEdgeApp1.Windows.Amd64**.
    * De **Modules** map bevat verwijzingen naar de modules die zijn opgenomen in het project. In dit geval moet het zojuist IoTEdgeModule1 zijn. 
    * De **deployment.template.json** bestand is een sjabloon voor het maken van een manifest van de implementatie. Een *implementatie manifest* is een bestand dat bepaalt precies welke modules gewenste geïmplementeerd op een apparaat, hoe deze moeten worden geconfigureerd en hoe ze kunnen communiceren met elkaar en in de cloud. 
* Een IoT Edge-module-project met de naam **IoTEdgeModule1**.
    * De **main.c** -bestand bevat de standaard C-module-code die wordt geleverd met de projectsjabloon, maken. De standaardmodule invoer van een bron en geeft deze samen met IoT Hub. 
    * De **module.json** bestand wachtstand details over de module, met inbegrip van de opslagplaats volledige installatiekopie installatiekopie versie, en welke docker-bestand moet worden gebruikt voor elk ondersteund platform.

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>Referenties voor uw register met de IoT Edge-agent

IoT Edge-runtime moet de registerreferenties van uw voor het ophalen van uw containerinstallatiekopieën op het IoT Edge-apparaat. Deze referenties toevoegen aan de sjabloon voor de implementatie. 

1. Open de **deployment.template.json** bestand.

2. Zoek de **registryCredentials** eigenschap in de $edgeAgent gewenste eigenschappen. 

3. Werk de eigenschap met de referenties, deze indeling te volgen: 

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "<username>",
       "password": "<password>",
       "address": "<registry name>.azurecr.io"
     }
   }

4. Save the deployment.template.json file. 

### Review the sample code

The solution template that you created includes sample code for an IoT Edge module. This sample module simply receives messages and then passes them on. The pipeline functionality demonstrates an important concept in IoT Edge, which is how modules communicate with each other.

Each module can have multiple *input* and *output* queues declared in their code. The IoT Edge hub running on the device routes messages from the output of one module into the input of one or more modules. The specific language for declaring inputs and outputs varies between languages, but the concept is the same across all modules. For more information about routing between modules, see [Declare routes](module-composition.md#declare-routes).

1. In the **main.c** file, find the **SetupCallbacksForModule** function.

2. This function sets up an input queue to receive incoming messages. It calls the C SDK module client function [SetInputMessageCallback](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-setinputmessagecallback). Review this function and see that it initializes an input queue called **input1**. 

   ![Find the input name in the SetInputMessageCallback constructor](./media/tutorial-develop-for-windows/declare-input-queue.png)

3. Next, find the **InputQueue1Callback** function.

4. This function processes received messages and sets up an output queue to pass them along. It calls the C SDK module client function [SendEventToOutputAsync](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-sendeventtooutputasync). Review this function and see that it initializes an output queue called **output1**. 

   ![Find the output name in the SendEventToOutputAsync constructor](./media/tutorial-develop-for-windows/declare-output-queue.png)

5. Open the **deployment.template.json** file.

6. Find the **modules** property of the $edgeAgent desired properties. 

   There should be two modules listed here. The first is **tempSensor**, which is included in all the templates by default to provide simulated temperature data that you can use to test your modules. The second is the **IotEdgeModule1** module that you created as part of this project.

   This modules property declares which modules should be included in the deployment to your device or devices. 

7. Find the **routes** property of the $edgeHub desired properties. 

   One of the functions if the IoT Edge hub module is to route messages between all the modules in a deployment. Review the values in the routes property. The first route, **IotEdgeModule1ToIoTHub**, uses a wildcard character (**\***) to include any message coming from any output queue in the IoTEdgeModule1 module. These messages go into *$upstream*, which is a reserved name that indicates IoT Hub. The second route, **sensorToIotEdgeModule1**, takes messages coming from the tempSensor module and routes them to the *input1* input queue of the IotEdgeModule1 module. 

   ![Review routes in deployment.template.json](./media/tutorial-develop-for-windows/deployment-routes.png)


## Build and push your solution

You've reviewed the module code and the deployment template to understand some key deployment concepts. Now, you're ready to build the IotEdgeModule1 container image and push it to your container registry. With the IoT tools extension for Visual Studio, this step also generates the deployment manifest based on the information in the template file and the module information from the solution files. 

### Sign in to Docker

Provide your container registry credentials to Docker on your development machine so that it can push your container image to be stored in the registry. 

1. Open PowerShell or a command prompt.

2. Sign in to Docker with the Azure container registry credentials that you saved after creating the registry. 

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   U ontvangt mogelijk een beveiligingswaarschuwing voor het gebruik van aanbevelen `--password-stdin`. Hoewel deze aanbevolen procedure wordt aanbevolen voor productiescenario's, valt buiten het bereik van deze zelfstudie. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) verwijzing.

### <a name="build-and-push"></a>Maken en pushen

Uw ontwikkelcomputer heeft nu toegang tot uw container registry en uw IoT Edge-apparaten te worden. Het is tijd om in te schakelen van de projectcode in een containerinstallatiekopie. 

1. Met de rechtermuisknop op de **AzureIotEdgeApp1.Windows.Amd64** projectmap en selecteer **Build and Push IoT Edge-Modules**. 

   ![Bouw en push IoT Edge-modules](./media/tutorial-develop-for-windows/build-and-push-modules.png)

   De opdracht build- en push start drie bewerkingen. Eerst een nieuwe map wordt gemaakt in de oplossing met de naam **config** die de volledige implementatie oplossing voor het manifest, gebouwd om van gegevens in de sjabloon voor de implementatie en andere bestanden bevat. Ten tweede, deze wordt uitgevoerd `docker build` om de containerinstallatiekopie op basis van het dockerfile dat geschikt is voor uw doel-architectuur te bouwen. Vervolgens wordt deze uitgevoerd `docker push` naar de opslagplaats voor installatiekopieën pushen naar uw containerregister. 

   Dit proces kan enkele minuten voor het eerst duren, maar is sneller de volgende keer dat u de opdrachten uitvoeren. 

2. Open de **deployment.windows amd64.json** bestand in de zojuist gemaakte config-map. (De map config mogelijk niet weergegeven in de Solution Explorer in Visual Studio. Als dit het geval is, selecteert u de **weergeven van alle bestanden** pictogram in het systeemvak Solution Explorer.)

3. Zoek de **installatiekopie** parameter van de sectie IotEdgeModule1. U ziet dat de installatiekopie van de opslagplaats voor de volledige installatiekopie met de naam, versie en architectuur-code uit het bestand module.json bevat.

4. Open de **module.json** bestand in de map IotEdgeModule1. 

5. Wijzig het versienummer voor de installatiekopie van de module. (De versie, niet de $schema-versie.) Bijvoorbeeld: verhoog het versienummer van de patch op **0.0.2** alsof we een kleine oplossing in de module-code heeft gemaakt. 

   >[!TIP]
   >Moduleversies versiebeheer inschakelen en kunnen u wijzigingen op een klein aantal apparaten te testen voordat u updates implementeert naar productie. Als u niet de versie van de module verhogen voor het bouwen en te pushen, overschrijft u de opslagplaats in uw containerregister. 

6. Sla uw wijzigingen in het bestand module.json.

7. Met de rechtermuisknop op de **AzureIotEdgeApp1.Windows.Amd64** map opnieuw project en selecteer opnieuw **Build and Push IoT Edge-modules**. 

8. Open de **deployment.windows amd64.json** bestand opnieuw uit. U ziet dat er een nieuw bestand is niet gemaakt wanneer u de opdracht build- en push opnieuw uitgevoerd. In plaats daarvan is hetzelfde bestand bijgewerkt om de wijzigingen weer te geven. De installatiekopie van het IotEdgeModule1 verwijst nu naar de 0.0.2 versie van de container. Deze wijziging in het manifest van de implementatie is hoe u de IoT Edge-apparaat dat er een nieuwe versie van een module om op te halen. 

9. Verder controleren wat de build- en push-opdracht hebt gedaan, gaat u naar de [Azure-portal](https://portal.azure.com) en navigeer naar het containerregister. 

10. Selecteer in het containerregister **opslagplaatsen** vervolgens **iotedgemodule1**. Controleer of dat beide versies van de installatiekopie naar het register zijn gepusht.

    ![Beide versies van een installatiekopie in containerregister weergeven](./media/tutorial-develop-for-windows/view-repository-versions.png)

### <a name="troubleshoot"></a>Problemen oplossen

Als u fouten bij het maken en pushen van de installatiekopie van de module optreden, heeft dit vaak doen met de configuratie van Docker op uw ontwikkelcomputer. Gebruik de volgende controles voor de configuratie controleren: 

* Hebt u uitvoeren de `docker login` opdracht met de referenties die u hebt gekopieerd uit uw container registry? Deze referenties zijn anders dan degene die u gebruiken om aan te melden bij Azure. 
* Hebt u de juiste containeropslagplaats? Beschikt het over de juiste naam van het containerregister en de naam van de juiste module? Open de **module.json** bestand in de map IotEdgeModule1 om te controleren. De waarde van de opslagplaats moet eruitzien als  **\<registernaam\>.azurecr.io/iotedgemodule1**. 
* Als u een andere naam dan gebruikt **IotEdgeModule1** voor uw module, wordt u die naam consistent wordt gebruikt in de oplossing?
* Wordt op uw machine hetzelfde type containers die u bouwt uitgevoerd? Deze zelfstudie is voor Windows IoT Edge-apparaten, zodat uw Visual Studio-bestanden moeten de **windows-amd64** -extensie en Docker Desktop het moet Windows-containers worden uitgevoerd. 

## <a name="deploy-modules-to-device"></a>Modules op apparaat implementeren

U verifiëren dat de gemaakte containerinstallatiekopieën worden opgeslagen in uw container registry, dus is het tijd om ze te implementeren op een apparaat. Zorg ervoor dat uw IoT Edge-apparaat actief en werkend is. 

1. Open de Cloud Explorer in Visual Studio en vouw de details voor uw IoT-hub. 

2. Selecteer de naam van het apparaat dat u implementeren wilt op. In de **acties** in de lijst met **implementatie maken**.

   ![Implementatie voor één apparaat maken](./media/tutorial-develop-for-windows/create-deployment.png)


3. Navigeer in de Verkenner naar de map van de configuratie van uw project en selecteer de **deployment.windows amd64.json** bestand. Dit bestand bevindt zich vaak `C:\Users\<username>\source\repos\AzureIotEdgeApp1\AzureIotEdgeApp1.Windows.Amd64\config\deployment.windows-amd64.json`

   Gebruik niet de deployment.template.json-bestand, dat niet beschikt over de volledige module image-waarden in deze. 

4. Vouw de details voor uw IoT Edge-apparaat in de Cloud Explorer om te zien van de modules op uw apparaat.

5. Gebruik de **vernieuwen** bijwerken van de status van het apparaat om te zien dat de tempSensor en IotEdgeModule1 modules worden geïmplementeerd om uw apparaat. 


   ![Weergave-modules die worden uitgevoerd op uw IoT Edge-apparaat](./media/tutorial-develop-for-windows/view-running-modules.png)

## <a name="view-messages-from-device"></a>Berichten van apparaat weergeven

De code IotEdgeModule1 ontvangt berichten via de invoerwachtrij en doorgegeven aan via de uitvoerwachtrij. Het manifest implementatie gedeclareerd routes die berichten van tempSensor doorgegeven aan IotEdgeModule1 en die berichten vanaf IotEdgeModule1 doorgestuurd naar IoT Hub. De Azure IoT Edge-tools voor Visual Studio kunnen u berichten te zien wanneer ze op IoT-Hub van uw afzonderlijke apparaten binnenkomen. 

1. In Visual Studio cloud explorer, selecteer de naam van het IoT Edge-apparaat dat u hebt geïmplementeerd op. 

2. In de **acties** in het menu **Start Monitoring D2C Message**.

3. Bekijk de **uitvoer** sectie in Visual Studio om berichten die binnenkomen in uw IoT-hub te bekijken. 

   Het duurt een paar minuten voor beide modules om te starten. IoT Edge-runtime moet voor het ontvangen van de nieuwe implementatie van het manifest, haal de module-installatiekopieën van de container-runtime en start u elke nieuwe module. Als u 

   ![Binnenkomende apparaat naar cloud-berichten bekijken](./media/tutorial-develop-for-windows/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>Wijzigingen weergeven op apparaat

Als u zien wat er gebeurt op het apparaat zelf wilt, gebruikt u de opdrachten in deze sectie om te controleren van de IoT Edge-runtime en -modules die worden uitgevoerd op uw apparaat. 

De opdrachten in deze sectie zijn voor uw IoT Edge-apparaat, niet op uw ontwikkelcomputer. Als u een virtuele machine voor uw IoT Edge-apparaat, verbinding maken met het nu. Ga naar de pagina overzicht van de virtuele machine in Azure, en selecteer **Connect** voor toegang tot de verbinding met extern bureaublad. Open op het apparaat een opdracht of een PowerShell-venster om uit te voeren de `iotedge` opdrachten.

* Weergeven van alle modules die zijn geïmplementeerd op het apparaat en hun status controleren:

   ```cmd
   iotedge list
   ```

   Ziet u vier modules: de twee IoT Edge-runtime-modules, tempSensor en IotEdgeModule1. Alle vier moeten worden vermeld als die wordt uitgevoerd.

* Controleer de logboeken voor een specifieke module:

   ```cmd
   iotedge logs <module name>
   ```

   IoT Edge-modules zijn hoofdlettergevoelig. 

   Ze zijn verwerking van berichten moeten worden weergegeven door de tempSensor en IotEdgeModule1 Logboeken. De module edgeAgent is verantwoordelijk voor het starten van de andere modules, zodat de logboeken wordt informatie over het implementeren van het manifest van de implementatie. Als een module wordt niet weergegeven of niet wordt uitgevoerd, wordt de logboeken edgeAgent waarschijnlijk de fouten hebben. De module edgeHub is verantwoordelijk voor de communicatie tussen de modules en IoT-Hub. Als de modules zijn en die wordt uitgevoerd, maar de berichten die niet zijn binnenkomen in uw IoT-hub, wordt de fouten waarschijnlijk in de logboeken edgeHub hebben. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, Visual Studio 2017 instellen op uw ontwikkelcomputer en uw eerste IoT Edge module op basis van deze geïmplementeerd. Nu u de basisbegrippen kent, kunt u proberen functionaliteit toevoegen aan een module zodat deze de passeren gegevens kunt analyseren. Kies de taal van uw voorkeur: 

> [!div class="nextstepaction"] 
> [C](tutorial-c-module-windows.md)
> [C#](tutorial-csharp-module-windows.md)