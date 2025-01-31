---
title: 'Voor Windows-apparaten: Azure IoT Edge-module ontwikkelen | Microsoft Docs'
description: Deze zelfstudie leidt u door het instellen van uw machine en cloud resources voor het ontwikkelen voor het ontwikkelen van IoT Edge-modules met behulp van Windows-containers voor Windows-apparaten
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/06/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 94a287cd996bd18b757620254540f8dc0df499e8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051814"
---
# <a name="tutorial-develop-iot-edge-modules-for-windows-devices"></a>Zelfstudie: IoT Edge-modules ontwikkelen voor Windows-apparaten

Visual Studio gebruiken om te ontwikkelen en implementeren van code op Windows-apparaten met IoT Edge.

In de Quick Start, moet u een IoT Edge-apparaat met een Windows-machine gemaakt en geïmplementeerd een vooraf samengestelde module op basis van de Azure Marketplace. Deze zelfstudie leert wat het duurt om te ontwikkelen en implementeren van uw eigen code in een IoT Edge-apparaat. Deze zelfstudie is een nuttig vereiste voor alle de andere zelfstudies die u meer informatie over specifieke programmeertalen of Azure-services krijgt. 

Deze zelfstudie wordt gebruikgemaakt van het voorbeeld van de implementatie van een  **C# module aan een Windows-apparaat**. In dit voorbeeld is gekozen omdat dit de meest voorkomende scenario voor het ontwikkelen. Als u geïnteresseerd bent in het ontwikkelen in een andere taal of van plan bent over het implementeren van Azure-services als modules, wordt in deze zelfstudie nog steeds nuttig zijn voor meer informatie over de ontwikkelhulpprogramma's. Als u inzicht in de ontwikkeling van concepten, kunt klikt u vervolgens u uw voorkeurstaal of Duik in de details van de Azure-service. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van uw ontwikkelcomputer.
> * Gebruik het IoT Edge-tools voor Visual Studio een nieuw project te maken.
> * Bouw uw project als een container en op te slaan in een Azure container registry.
> * Uw code implementeren in een IoT Edge-apparaat. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="key-concepts"></a>Belangrijkste concepten

Deze zelfstudie leidt u door de ontwikkeling van een IoT Edge-module. Een *IoT Edge-module*, of gewoon soms *module* voor korte, is een container waarin uitvoerbare code bevat. U kunt een of meer modules voor een IoT Edge-apparaat implementeren. Modules specifieke taken uitvoeren, zoals gegevens opnemen van sensors, gegevensanalyse of gegevens reinigen of het verzenden van berichten naar een IoT-hub. Zie voor meer informatie, [inzicht in Azure IoT Edge-modules](iot-edge-modules.md).

Bij het ontwikkelen van IoT Edge-modules, is het belangrijk om te begrijpen van het verschil tussen de ontwikkelcomputer en het doel IoT Edge-apparaat waarop de module uiteindelijk zal worden geïmplementeerd. De container die u bouwt voor het opslaan van uw code van de module moet overeenkomen met het besturingssysteem (OS) van de *doelapparaat*. Voor de ontwikkeling van Windows-containers is dit concept eenvoudiger, omdat Windows-containers alleen op Windows-besturingssystemen worden uitgevoerd. Maar u kunt uw Windows-ontwikkelcomputer bijvoorbeeld gebruiken om te bouwen-modules voor Linux IoT Edge-apparaten. In dit scenario moet u om ervoor te zorgen dat uw ontwikkelcomputer Linux-containers werd uitgevoerd. Als u deze zelfstudie doorloopt, houd rekening met het verschil tussen *ontwikkelcomputer OS* en de *container OS*.

In deze zelfstudie is bedoeld voor Windows-apparaten met IoT Edge. Windows IoT Edge-apparaten Windows-containers gebruiken. U wordt aangeraden met Visual Studio ontwikkelen voor Windows-apparaten, dit is wat deze zelfstudie wordt gebruikt. U kunt ook Visual Studio Code gebruiken maar er verschillen in de ondersteuning tussen de twee hulpprogramma's zijn.

De volgende tabel bevat de ondersteunde scenario's voor **Windows containers** in Visual Studio Code en Visual Studio.

|   | Visual Studio Code | Visual Studio 2017/2019 |
| - | ------------------ | ------------------ |
| **Azure-services** | Azure Functions <br> Azure Stream Analytics |   |
| **Talen** | C#(foutopsporing niet ondersteund) | C <br> C# |
| **Meer informatie** | [Azure IoT Edge voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) | [Azure IoT Edge-hulpprogramma's voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools)<br>[Azure IoT Edge-hulpprogramma's voor Visual Studio 2019](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) |

## <a name="prerequisites"></a>Vereisten

Een ontwikkelcomputer:

* Windows 10 met 1809 update of hoger.
* U kunt uw eigen computer of op een virtuele machine, afhankelijk van uw voorkeuren voor ontwikkeling.
* Installeer [Git](https://git-scm.com/). 

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

De IoT-uitbreidingen voor Visual Studio kunnen u voor het ontwikkelen van IoT Edge-modules. Deze uitbreidingen bevatten projectsjablonen, het maken van het manifest implementatie automatiseren en kunnen u controleren en beheren van IoT Edge-apparaten. In deze sectie kunt u Visual Studio en de IoT Edge-extensie installeren en vervolgens uw Azure-account instellen voor het beheren van IoT Hub-resources vanuit Visual Studio. 

In deze zelfstudie leert de stappen ontwikkeling voor Visual Studio 2019. Als u Visual Studio 2017 (versie 15.7 of hoger) gebruikt, zijn de stappen zijn vergelijkbaar. Als u in plaats van Visual Studio Code gebruikt, raadpleegt u de instructies in [gebruik van Visual Studio Code te ontwikkelen en fouten opsporen in modules voor Azure IoT Edge](how-to-vs-code-develop-module.md). 

1. Visual Studio 2019 voorbereiden op uw ontwikkelcomputer. 

   * Als u geen Visual Studio al op uw ontwikkelcomputer hebt [2019 Visual Studio installeren](https://docs.microsoft.com/visualstudio/install/install-visual-studio) met de volgende workloads: 

      * Azure-ontwikkeling
      * Desktopontwikkeling metC++
      * Platformoverschrijdende ontwikkelmogelijkheden van .NET Core

   * Als u Visual Studio 2019 al op uw ontwikkelcomputer hebt, volgt u de stappen in [Visual Studio wijzigen](https://docs.microsoft.com/visualstudio/install/modify-visual-studio) om toe te voegen van de vereiste werkbelastingen.

2. Download en installeer de [hulpprogramma's voor Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vs16iotedgetools) -extensie voor Visual Studio 2019. 

   Als u gebruikmaakt van Visual Studio 2017 (versie 15.7 of hoger), downloadt en installeert de [Azure IoT Edge-Tools voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools).

3. Wanneer uw installaties voltooid zijn, opent u Visual Studio 2019 en selecteer **doorgaan zonder code**.

4. Selecteer **weergave** > **Cloud Explorer**. 

5. Selecteer het pictogram van het profiel in de cloud explorer en aanmelden bij uw Azure-account als u al zijn niet aangemeld. 

6. Als u zich aanmeldt, wordt uw Azure-abonnementen worden weergegeven. Vouw het abonnement waaraan uw IoT-hub. 

7. Vouw onder uw abonnement, **IoT-Hubs** vervolgens uw IoT-hub. U ziet een lijst met uw IoT-apparaten en kunt deze explorer gebruiken om deze te beheren. 

   ![Toegang tot IoT Hub-resources in de Cloud Explorer](./media/tutorial-develop-for-windows/cloud-explorer-view-hub.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>Maak een nieuw project in de module

De hulpprogramma's voor Azure IoT Edge-extensie biedt projectsjablonen voor alle ondersteunde IoT Edge module talen in Visual Studio. Deze sjablonen zijn alle bestanden en code die u nodig hebt voor het implementeren van een module werken als u wilt testen van IoT Edge of geeft u een beginpunt voor het aanpassen van de sjabloon met uw eigen bedrijfslogica. 

1. Selecteer **bestand** > **nieuwe** > **Project...**

2. Zoek in het venster Nieuw project **IoT Edge** en kies de **Azure IoT Edge (Windows-amd64)** project. Klik op **volgende**. 

   ![Een nieuw Azure IoT Edge-project maken](./media/tutorial-develop-for-windows/new-project.png)

3. In de configureren uw nieuw projectvenster, wijzig de naam van het project en de oplossing iets beschrijvende als volgt aan bij **CSharpTutorialApp**. Klik op **maken** om het project te maken.

   ![Een nieuw project voor Azure IoT Edge configureren](./media/tutorial-develop-for-windows/configure-project.png)
 

4. In de IoT Edge-toepassing en het modulevenster, configureert u uw project met de volgende waarden: 

   | Veld | Value |
   | ----- | ----- |
   | Selecteer een sjabloon | Selecteer  **C# Module**. | 
   | Naam van de module-project | Accepteer de standaardwaarde **IoTEdgeModule1**. | 
   | Opslagplaats voor docker-installatiekopieën | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. Uw containerinstallatiekopie wordt vooraf ingevuld vanuit de waarde van de naam van de module project. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br> De opslagplaats voor de uiteindelijke installatiekopie ziet eruit als \<registernaam\>.azurecr.io/iotedgemodule1. |

   ![Configureren van uw project voor het doelapparaat en module-type containerregister](./media/tutorial-develop-for-windows/add-module-to-solution.png)

5. Selecteer **Ja** de wijzigingen worden toegepast. 

Zodra het nieuwe project wordt geladen in het venster Visual Studio, even de tijd om vertrouwd te raken met de bestanden dat is gemaakt: 

* Een IoT Edge-project met de naam **CSharpTutorialApp**.
    * De **Modules** map bevat verwijzingen naar de modules die zijn opgenomen in het project. In dit geval moet het zojuist IoTEdgeModule1 zijn. 
    * De **deployment.template.json** bestand is een sjabloon voor het maken van een manifest van de implementatie. Een *implementatie manifest* is een bestand dat bepaalt precies welke modules gewenste geïmplementeerd op een apparaat, hoe deze moeten worden geconfigureerd en hoe ze kunnen communiceren met elkaar en in de cloud. 
* Een IoT Edge-module-project met de naam **IoTEdgeModule1**.
    * De **program.cs** bestand bevat de standaard C# module-code die wordt geleverd met de projectsjabloon, maken. De standaardmodule invoer van een bron en geeft deze samen met IoT Hub. 
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
   ```

4. Sla het bestand deployment.template.json op. 

### <a name="review-the-sample-code"></a>De voorbeeldcode controleren

De sjabloon van de oplossing die u hebt gemaakt, bevat voorbeelden van code voor een IoT Edge-module. Deze voorbeeldmodule ontvangt gewoon berichten en geeft deze op. De functionaliteit van de pijplijn ziet u een belangrijk concept in IoT Edge, dit is hoe u modules met elkaar communiceren.

Elke module kunt hebben meerdere *invoer* en *uitvoer* wachtrijen gedeclareerd in hun code. De IoT Edge hub die wordt uitgevoerd op het apparaat routeert berichten uit de uitvoer van een module in de invoer van een of meer modules. De specifieke taal voor de invoer en uitvoer declareren varieert tussen talen, maar het concept is hetzelfde voor alle modules. Zie voor meer informatie over routering tussen modules [declareren routes](module-composition.md#declare-routes).

Het voorbeeld C# maakt gebruik van code die wordt geleverd met de projectsjabloon, maken de [ModuleClient klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient?view=azure-dotnet) van de IoT Hub-SDK voor .NET. 

1. In de **program.cs** bestand, zoek de **SetInputMessageHandlerAsync** methode.

2. De [SetInputMessageHandlerAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.setinputmessagehandlerasync?view=azure-dotnet) methode stelt u een wachtrij om binnenkomende berichten te ontvangen. Deze methode controleren en bekijken hoe het initialiseren van een wachtrij met de naam **input1**. 

   ![De ingevoerde naam niet vinden in de constructor SetInputMessageHandlserAsync](./media/tutorial-develop-for-windows/declare-input-queue.png)

3. Zoek vervolgens de **SendEventAsync** methode.

4. De [SendEventAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.moduleclient.sendeventasync?view=azure-dotnet) methode ontvangen berichten worden verwerkt en een uitvoerwachtrij ingesteld om door te geven ze samen. Bekijk deze methode en dat het initialiseren van een uitvoerwachtrij met de naam **output1**. 

   ![De naam van de uitvoer niet vinden in de constructor SendEventAsync](./media/tutorial-develop-for-windows/declare-output-queue.png)

5. Open de **deployment.template.json** bestand.

6. Zoek de **modules** eigenschap van de $edgeAgent gewenste eigenschappen. 

   Er moet twee modules die hier worden vermeld. De eerste is **tempSensor**, die is opgenomen in alle sjablonen standaard voor de gesimuleerde temperatuurgegevens die u gebruiken kunt voor het testen van uw modules. De tweede is het **IotEdgeModule1** module die u hebt gemaakt als onderdeel van dit project.

   Deze eigenschap modules worden gedeclareerd welke modules moeten worden opgenomen in de implementatie voor uw apparaat of apparaten. 

7. Zoek de **routes** eigenschap van de $edgeHub gewenste eigenschappen. 

   Een van de functies als de IoT Edge hub-module het routeren van berichten tussen de modules in een implementatie is. Bekijk de waarden in de eigenschap routes. De eerste route **IotEdgeModule1ToIoTHub**, gebruikt u een jokerteken ( **\*** ) om op te nemen die afkomstig zijn van een uitvoerwachtrij in de module IoTEdgeModule1 bericht. Deze berichten met ingang van *$upstream*, dit is een gereserveerde naam die aangeeft van IoT-Hub. De tweede route **sensorToIotEdgeModule1**, gebruikt die afkomstig zijn van de module tempSensor berichten ontvangen en doorgestuurd naar de *input1* invoerwachtrij van de module IotEdgeModule1. 

   ![Routes in deployment.template.json bekijken](./media/tutorial-develop-for-windows/deployment-routes.png)


## <a name="build-and-push-your-solution"></a>De oplossing bouwen en pushen

U hebt gecontroleerd met de module-code en de sjabloon voor de implementatie enkele implementatie van de belangrijkste concepten begrijpen. U bent nu klaar voor het bouwen van de containerinstallatiekopie IotEdgeModule1 en deze naar het containerregister pushen. Met de hulpprogramma's voor IoT-extensie voor Visual Studio genereert deze stap ook het manifest van de implementatie op basis van de informatie in het sjabloonbestand en de modulegegevens van de oplossingsbestanden. 

### <a name="sign-in-to-docker"></a>Aanmelden bij Docker

Referenties van uw container registry Docker op uw ontwikkelcomputer zodat deze de containerinstallatiekopie moet worden opgeslagen in het register kunt pushen. 

1. Open PowerShell of een opdrachtprompt.

2. Meld u aan Docker met de Azure container registry-referenties die u na het maken van het register hebt opgeslagen. 

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   U ontvangt mogelijk een beveiligingswaarschuwing voor het gebruik van aanbevelen `--password-stdin`. Hoewel deze aanbevolen procedure wordt aanbevolen voor productiescenario's, valt buiten het bereik van deze zelfstudie. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) verwijzing.

### <a name="build-and-push"></a>Maken en pushen

Uw ontwikkelcomputer heeft nu toegang tot uw container registry en uw IoT Edge-apparaten te worden. Het is tijd om in te schakelen van de projectcode in een containerinstallatiekopie. 

1. Met de rechtermuisknop op de **CSharpTutorialApp** projectmap en selecteer **Build and Push IoT Edge-Modules**. 

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

7. Met de rechtermuisknop op de **CSharpTutorialApp** map opnieuw project en selecteer **Build and Push IoT Edge-modules** opnieuw. 

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


3. Navigeer in de Verkenner naar de map van de configuratie van uw project en selecteer de **deployment.windows amd64.json** bestand. Dit bestand bevindt zich vaak `C:\Users\<username>\source\repos\CSharpTutorialApp\CSharpTutorialApp\config\deployment.windows-amd64.json`

   Gebruik niet de deployment.template.json-bestand, dat niet beschikt over de volledige module image-waarden in deze. 

4. Vouw de details voor uw IoT Edge-apparaat in de Cloud Explorer om te zien van de modules op uw apparaat.

5. Gebruik de **vernieuwen** bijwerken van de status van het apparaat om te zien dat de tempSensor en IotEdgeModule1 modules worden geïmplementeerd om uw apparaat. 


   ![Weergave-modules die worden uitgevoerd op uw IoT Edge-apparaat](./media/tutorial-develop-for-windows/view-running-modules.png)

## <a name="view-messages-from-device"></a>Berichten van apparaat weergeven

De code IotEdgeModule1 ontvangt berichten via de invoerwachtrij en doorgegeven aan via de uitvoerwachtrij. Het manifest implementatie gedeclareerd routes die berichten van tempSensor doorgegeven aan IotEdgeModule1 en die berichten vanaf IotEdgeModule1 doorgestuurd naar IoT Hub. De Azure IoT Edge-tools voor Visual Studio kunnen u berichten te zien wanneer ze op IoT-Hub van uw afzonderlijke apparaten binnenkomen. 

1. In Visual Studio cloud explorer, selecteer de naam van het IoT Edge-apparaat dat u hebt geïmplementeerd op. 

2. In de **acties** in het menu **Start Monitoring ingebouwde gebeurtenis eindpunt**.

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

In deze zelfstudie, Visual Studio 2019 instellen op uw ontwikkelcomputer en uw eerste IoT Edge module op basis van deze geïmplementeerd. Nu u de basisbegrippen kent, kunt u proberen functionaliteit toevoegen aan een module zodat deze de passeren gegevens kunt analyseren. Kies de taal van uw voorkeur: 

> [!div class="nextstepaction"] 
> [C](tutorial-c-module-windows.md)
> [C#](tutorial-csharp-module-windows.md)
