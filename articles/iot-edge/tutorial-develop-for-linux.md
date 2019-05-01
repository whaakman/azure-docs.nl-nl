---
title: 'Voor Linux-apparaten: Azure IoT Edge-module ontwikkelen | Microsoft Docs'
description: Deze zelfstudie leidt u door het instellen van uw machine en cloud resources voor het ontwikkelen voor het ontwikkelen van IoT Edge-modules met behulp van Linux-containers voor Linux-apparaten
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 04/26/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: e5b34171af53a72ef231bb8beb758b7ff97cf5d9
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576780"
---
# <a name="tutorial-develop-iot-edge-modules-for-linux-devices"></a>Zelfstudie: Ontwikkelen van IoT Edge-modules voor Linux-apparaten

Gebruik Visual Studio Code ontwikkelen en implementeren van code naar Linux-apparaten met IoT Edge. 

In de quickstart-artikelen, moet u een IoT Edge-apparaat met een Linux-machine gemaakt en geïmplementeerd een vooraf samengestelde module op basis van de Azure Marketplace. Deze zelfstudie leert wat het duurt om te ontwikkelen en implementeren van uw eigen code in een IoT Edge-apparaat. Deze zelfstudie is een nuttig vereiste voor alle de andere zelfstudies die u meer informatie over specifieke programmeertalen of Azure-services krijgt. 

Deze zelfstudie wordt gebruikgemaakt van het voorbeeld van de implementatie van een **C-module op een Linux-apparaat**. In dit voorbeeld is gekozen omdat de vereisten zo weinig mogelijk, zodat u kunt meer informatie over de ontwikkelhulpprogramma's zonder dat u of u de juiste bibliotheken geïnstalleerd hebt. Als u inzicht in de ontwikkeling van concepten, kunt klikt u vervolgens u uw voorkeurstaal of Duik in de details van de Azure-service. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van uw ontwikkelcomputer.
> * De IoT Edge-hulpprogramma's voor Visual Studio Code gebruiken om te maken van een nieuw project.
> * Bouw uw project als een container en op te slaan in een Azure container registry.
> * Uw code implementeren in een IoT Edge-apparaat. 


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="key-concepts"></a>Belangrijkste concepten

Deze zelfstudie leidt u door de ontwikkeling van een IoT Edge-module. Een *IoT Edge-module*, of gewoon soms *module* voor korte, is een container waarin uitvoerbare code bevat. U kunt een of meer modules voor een IoT Edge-apparaat implementeren. Modules specifieke taken uitvoeren, zoals gegevens opnemen van sensors, gegevensanalyse of gegevens reinigen of het verzenden van berichten naar een IoT-hub. Zie voor meer informatie, [inzicht in Azure IoT Edge-modules](iot-edge-modules.md).

Bij het ontwikkelen van IoT Edge-modules, is het belangrijk om te begrijpen van het verschil tussen de ontwikkelcomputer en het doel IoT Edge-apparaat waarop de module uiteindelijk zal worden geïmplementeerd. De container die u bouwt voor het opslaan van uw code van de module moet overeenkomen met het besturingssysteem (OS) van de *doelapparaat*. De meest voorkomende scenario is bijvoorbeeld iemand het ontwikkelen van een module op een Windows-computer wilde maken op een Linux-apparaat met IoT Edge. In dat geval is het besturingssysteem van de container Linux. Als u deze zelfstudie doorloopt, houd rekening met het verschil tussen de *ontwikkelcomputer OS* en de *container OS*.

In deze zelfstudie is bedoeld voor Linux-apparaten met IoT Edge. U kunt uw besturingssysteem, voor de meest geschikte ontwikkelings-machine gebruiken, zolang uw ontwikkelcomputer Linux-containers kunt uitvoeren. U wordt aangeraden gebruik van Visual Studio Code voor het ontwikkelen voor Linux-apparaten, dit is wat deze zelfstudie wordt gebruikt. Kunt u Visual Studio 2017, maar er verschillen in de ondersteuning tussen de twee hulpprogramma's zijn.

De volgende tabel bevat de ondersteunde scenario's voor **Linux-containers** in Visual Studio Code en Visual Studio 2017.

|   | Visual Studio Code | Visual Studio 2017 |
| - | ------------------ | ------------------ |
| **Architectuur van de Linux-apparaat** | Linux AMD64 <br> Linux ARM32 | Linux AMD64 <br> Linux ARM32 |
| **Azure-services** | Azure Functions <br> Azure Stream Analytics <br> Azure Machine Learning |   |
| **Talen** | C <br> C# <br> Java <br> Node.js <br> Python | C <br> C# |
| **Meer informatie** | [Azure IoT Edge voor Visual Studio Code](https://marketplace.visualstudio.com/itemdetails?itemName=vsciot-vscode.azure-iot-edge) | [Azure IoT Edge-hulpprogramma's voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) |

In deze zelfstudie leert de stappen ontwikkeling voor Visual Studio Code. Als u echter liever Visual Studio 2017, raadpleegt u de instructies in [gebruik van Visual Studio 2017 om te ontwikkelen en fouten opsporen in modules voor Azure IoT Edge](how-to-visual-studio-develop-module.md).

## <a name="prerequisites"></a>Vereisten

Een ontwikkelcomputer:

* U kunt uw eigen computer of op een virtuele machine, afhankelijk van uw voorkeuren voor ontwikkeling.
* De meeste besturingssystemen die kunnen worden uitgevoerd een container-engine kan worden gebruikt voor het ontwikkelen van IoT Edge-modules voor Linux-apparaten. In deze zelfstudie maakt gebruik van een Windows-computer, maar wijst bekende verschillen in MacOS of Linux. 
* Installeer [Git](https://git-scm.com/), voor het ophalen van module sjabloon pakketten verderop in deze zelfstudie.  

Een Azure IoT Edge-apparaat op Linux:

* U wordt aangeraden dat u niet IoT Edge worden uitgevoerd op uw ontwikkelcomputer, maar in plaats daarvan een afzonderlijk apparaat gebruiken. Dit onderscheid tussen ontwikkelcomputer en IoT Edge-apparaat nauwkeurig komt overeen met een waarde true implementatiescenario en de andere concepten rechte blijft.
* Als u niet een tweede apparaat beschikbaar hebt, gebruikt u artikel te maken van een IoT Edge-apparaat in Azure met een [virtuele Linux-machine](quickstart-linux.md).

Cloudresources:

* Een gratis of standard-laag [IoT-hub](../iot-hub/iot-hub-create-through-portal.md) in Azure. 

## <a name="install-container-engine"></a>Container-engine installeren

IoT Edge-modules worden geleverd als containers, dus moet u een container-engine op uw ontwikkelcomputer bouwen en beheren van de containers. Het is raadzaam om met behulp van Docker-Desktop voor de ontwikkeling van vanwege de vele functies en de populariteit als een container-engine. Met Docker-bureaublad op een Windows-apparaat, kunt u schakelen tussen de Linux-containers en Windows-containers zodat u eenvoudig modules voor verschillende soorten IoT Edge-apparaten kunt ontwikkelen. 

Gebruik de Docker-documentatie om te installeren op uw ontwikkelcomputer: 

* [Docker-Desktop voor Windows installeren](https://docs.docker.com/docker-for-windows/install/)

  * Wanneer u Desktop Docker voor Windows installeert, wordt u gevraagd of u wilt gebruiken, Linux of Windows-containers. Deze beslissing kan worden gewijzigd op elk gewenst moment met behulp van een eenvoudig switch. Voor deze zelfstudie gebruiken we Linux-containers omdat onze modules voor Linux-apparaten ontwikkelt. Zie voor meer informatie, [schakelen tussen Windows en Linux-containers](https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers).

* [Installeren van Docker-bureaublad voor Mac](https://docs.docker.com/docker-for-mac/install/)

* Lezen [over Docker CE](https://docs.docker.com/install/) voor installatie-informatie over verschillende Linux-platforms.

## <a name="set-up-vs-code-and-tools"></a>Instellen van VS Code en hulpprogramma 's

De IoT-extensies voor Visual Studio Code gebruiken voor het ontwikkelen van IoT Edge-modules. Deze uitbreidingen bevatten projectsjablonen, het maken van het manifest implementatie automatiseren en kunnen u controleren en beheren van IoT Edge-apparaten. In deze sectie kunt u Visual Studio Code en de IoT-extensie installeren en vervolgens uw Azure-account instellen voor het beheren van IoT Hub-resources vanuit Visual Studio Code. 

1. Installeer [Visual Studio Code](https://code.visualstudio.com/) op uw ontwikkelcomputer. 

2. Zodra de installatie is voltooid, selecteert u **weergave** > **extensies**. 

3. Zoeken naar **hulpprogramma's voor Azure IoT**, dat is eigenlijk een verzameling van extensies die u helpen bij communiceren met IoT Hub en IoT-apparaten, evenals het ontwikkelen van IoT Edge-modules. 

4. Selecteer **Installeren**. Elke uitbreiding opgenomen afzonderlijk worden geïnstalleerd. 

5. Wanneer de extensies zijn klaar installeert, open het opdrachtenpalet hiervoor **weergave** > **Command Palette**. 

6. Zoek in het opdrachtenpalet en selecteer **Azure: Aanmelden** en voer deze uit. Volg de aanwijzingen om u aan te melden bij uw Azure-account. 

7. In het opdrachtenpalet opnieuw uit, zoek en selecteer **Azure IoT Hub: Select IoT Hub**. Volg de aanwijzingen om uw Azure-abonnement en de IoT-hub te selecteren. 

7. Open de sectie explorer van Visual Studio Code door Selecteer het pictogram in de activiteitenbalk aan de linkerkant, of door het selecteren van **weergave** > **Explorer**. 

8. Vouw de samengevouwen aan de onderkant van de sectie explorer **Azure IoT Hub-apparaten** menu. U ziet de apparaten en de IoT Edge-apparaten die zijn gekoppeld aan de IoT-hub die u hebt geselecteerd via de command palette. 

   ![Apparaten weergeven in uw IoT-hub](./media/tutorial-develop-for-linux/view-iot-hub-devices.png)

[!INCLUDE [iot-edge-create-container-registry](../../includes/iot-edge-create-container-registry.md)]

## <a name="create-a-new-module-project"></a>Maak een nieuw project in de module

De hulpprogramma's voor Azure IoT-extensie biedt projectsjablonen voor alle ondersteunde IoT Edge module talen in Visual Studio Code. Deze sjablonen zijn alle bestanden en code die u nodig hebt voor het implementeren van een module werken als u wilt testen van IoT Edge of geeft u een beginpunt voor het aanpassen van de sjabloon met uw eigen bedrijfslogica. 

Voor deze zelfstudie gebruiken we de sjabloon van de module C omdat deze de minst vereisten voor het installeren. 

### <a name="create-a-project-template"></a>Een projectsjabloon maken

Zoek in het opdrachtenpalet van Visual Studio Code, en selecteer **Azure IoT Edge: Nieuwe IoT Edge-oplossing**. Volg de aanwijzingen en de volgende waarden gebruiken om uw oplossing te maken: 

   | Veld | Value |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer waar VS Code de oplossingsbestanden moet maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in of accepteer de standaardnaam **EdgeSolution**. |
   | Modulesjabloon selecteren | Kies **C Module**. |
   | Een modulenaam opgeven | Accepteer de standaardwaarde **SampleModule**. |
   | Opslagplaats voor Docker-afbeeldingen voor de module opgeven | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. De containerafbeelding wordt vooraf gevuld vanuit de naam die u in de laatste stap hebt opgegeven. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br> De opslagplaats voor de uiteindelijke installatiekopie ziet eruit als \<registernaam\>.azurecr.io/samplemodule. |
 
   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/tutorial-develop-for-linux/image-repository.png)

Zodra de nieuwe oplossing wordt geladen in het venster Visual Studio Code, even de tijd om vertrouwd te raken met de bestanden dat is gemaakt: 

* De **.vscode** map bevat een bestand met de naam **launch.json**, die wordt gebruikt voor het opsporen van fouten in modules.
* De **modules** map bevat een map voor elke module in uw oplossing. Nu alleen **SampleModule**, of de servernaam die u heeft gegeven tot de module. De map SampleModule bevat de programmacode van de belangrijkste, de metagegevens van de module en verschillende Docker-bestanden. 
* De **.env** -bestand bevat de referenties bij uw containerregister. Deze referenties worden gedeeld met uw IoT Edge-apparaat, zodat deze toegang voor het ophalen van de containerinstallatiekopieën. 
* De **deployment.debug.template.json** bestand en **deployment.template.json** bestand zijn sjablonen die u helpen bij het maken van een manifest van de implementatie. Een *implementatie manifest* is een bestand dat bepaalt precies welke modules gewenste geïmplementeerd op een apparaat, hoe deze moeten worden geconfigureerd en hoe ze kunnen communiceren met elkaar en in de cloud. De sjabloonbestanden aanwijzers voor sommige waarden gebruiken. Wanneer u de sjabloon in een implementatie van true-manifest transformeert, worden de aanwijzers vervangen door waarden die zijn overgenomen uit andere oplossingsbestanden. Zoek de twee algemene tijdelijke aanduidingen in de sjabloon voor de implementatie: 

  * Het adres is in het gedeelte van de referenties register autofilled van de informatie die u hebt opgegeven tijdens het maken van de oplossing. Echter, de gebruikersnaam en het wachtwoord verwijzen naar de variabelen die zijn opgeslagen in het .env-bestand. Dit is voor beveiliging, zoals het .env-bestand git genegeerd is, maar de sjabloon voor de implementatie niet is. 
  * De containerinstallatiekopie is niet in de sectie SampleModule gevuld, zelfs als u de opslagplaats voor installatiekopieën opgegeven tijdens het maken van de oplossing. Deze tijdelijke aanduiding verwijst naar de **module.json** bestand in de map SampleModule. Als u naar dat bestand gaat, ziet u dat het afbeeldingsveld bevat de opslagplaats, maar ook een tagwaarde die bestaat uit de versie en het platform van de container. U kunt de versie herhalen als onderdeel van de ontwikkelingscyclus en u de containerplatform met behulp van een schakelbaar die we introduceren later in deze sectie selecteren. 

### <a name="provide-your-registry-credentials-to-the-iot-edge-agent"></a>Referenties voor uw register met de IoT Edge-agent

In het omgevingsbestand worden de referenties voor het containerregister opgeslagen. Deze referenties worden gedeeld met de IoT Edge-runtime. De runtime moet deze referenties voor het ophalen van uw containerinstallatiekopieën op het IoT Edge-apparaat. 

De IoT Edge-extensie probeert voor het ophalen van de container registerreferenties van Azure en deze in het omgevingsbestand te vullen. Controleer of uw referenties al zijn opgenomen. Als dat niet het geval is, wordt deze nu toevoegen:

1. Open de **.env** bestand in de module-oplossing. 
2. Voeg de **gebruikersnaam** en **wachtwoord** waarden die u hebt gekopieerd uit uw Azure container registry.
3. Sla uw wijzigingen in de .env-bestand. 

### <a name="select-your-target-architecture"></a>Selecteer uw doel-architectuur

Op dit moment kunt C-modules voor Linux AMD64- en Linux ARM32v7 apparaten ontwikkelen met Visual Studio Code. U moet selecteren welke architectuur die u hebt geconfigureerd met elke oplossing, omdat die van invloed op hoe de container is gemaakt en wordt uitgevoerd. De standaardwaarde is Linux AMD64. 

1. Open het opdrachtenpalet en zoek **Azure IoT Edge: Doelplatform standaard ingesteld voor Edge-oplossing**, of Selecteer het pictogram van de snelkoppeling in de zijbalk aan de onderkant van het venster. 

   ![Selecteer architectuur-pictogram in de zijbalk](./media/tutorial-develop-for-linux/select-architecture.png)

2. Selecteer de doel-architectuur in de lijst met opties in het opdrachtenpalet. Voor deze zelfstudie gebruiken we een virtuele Ubuntu-machine als de IoT Edge-apparaat, zodat de standaardwaarde wordt **amd64**. 

### <a name="review-the-sample-code"></a>De voorbeeldcode controleren

De sjabloon van de oplossing die u hebt gemaakt, bevat voorbeelden van code voor een IoT Edge-module. Deze voorbeeldmodule ontvangt gewoon berichten en geeft deze op. De functionaliteit van de pijplijn ziet u een belangrijk concept in IoT Edge, dit is hoe u modules met elkaar communiceren.

Elke module kunt hebben meerdere *invoer* en *uitvoer* wachtrijen gedeclareerd in hun code. De IoT Edge hub die wordt uitgevoerd op het apparaat routeert berichten uit de uitvoer van een module in de invoer van een of meer modules. De specifieke taal voor de invoer en uitvoer declareren varieert tussen talen, maar het concept is hetzelfde voor alle modules. Zie voor meer informatie over routering tussen modules [declareren routes](module-composition.md#declare-routes).

1. Open de **main.c** bestand, dat zich bevindt in de **modules/SampleModules/** map. 

2. De IoT Hub C-SDK gebruikt de functie [SetInputMessageCallback](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-setinputmessagecallback) module-invoer wachtrijen worden geïnitialiseerd. Zoeken in het bestand main.c voor die functie.

3. Bekijk de constructor van de functie SetInputMessageCallback en dat er een wachtrij met de naam **input1** in de code is geïnitialiseerd. 

   ![De ingevoerde naam niet vinden in SetInputMessageCallback-constructor](./media/tutorial-develop-for-linux/declare-input-queue.png)

4. Module uitvoer wachtrijen worden op een soortgelijke manier geïnitialiseerd. Zoek de [SendEventToOutputAsync](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/iothub-module-client-ll-h/iothubmoduleclient-ll-sendeventtooutputasync) functie in het bestand main.c. 

5. Bekijk de constructor van de functie SendEventToOutputAsync en dat wordt aangeroepen door een uitvoerwachtrij **output1** in de code is geïnitialiseerd. 

   ![De naam van de uitvoer niet vinden in SendEventToOutputAsync](./media/tutorial-develop-for-linux/declare-output-queue.png)

6. Open de **deployment.template.json** bestand.

7. Zoek de **modules** eigenschap van de $edgeAgent gewenste eigenschappen. 

   Er moet twee modules die hier worden vermeld. De eerste is **tempSensor**, die is opgenomen in alle sjablonen standaard voor de gesimuleerde temperatuurgegevens die u gebruiken kunt voor het testen van uw modules. De tweede is het **SampleModule** -module die u hebt gemaakt als onderdeel van deze oplossing.

7. Aan de onderkant van het bestand, zoek de gewenste eigenschappen voor de **$edgeHub** module. 

   Een van de functies van de IoT Edge hub-module is het routeren van berichten tussen de modules in een implementatie. Bekijk de waarden in de **routes** eigenschap. De eerste route **SampleModuleToIoTHub**, gebruikt u een jokerteken (**\***) om aan te geven die afkomstig zijn van een uitvoer-wachtrijen in de module SampleModule berichten. Deze berichten met ingang van *$upstream*, dit is een gereserveerde naam die aangeeft van IoT-Hub. De tweede route, sensorToSampleModule, gebruikt die afkomstig zijn van de module tempSensor berichten ontvangen en doorgestuurd naar de *input1* invoerwachtrij die u hebt gezien in de code SampleModule geïnitialiseerd. 

   ![Routes in deployment.template.json bekijken](./media/tutorial-develop-for-linux/deployment-routes.png)

## <a name="build-and-push-your-solution"></a>De oplossing bouwen en pushen

U hebt gecontroleerd met de module-code en de sjabloon voor de implementatie enkele implementatie van de belangrijkste concepten begrijpen. U bent nu klaar voor het bouwen van de containerinstallatiekopie SampleModule en deze naar het containerregister pushen. Met de hulpprogramma's voor IoT-extensie voor Visual Studio Code genereert deze stap ook het manifest van de implementatie op basis van de informatie in het sjabloonbestand en de modulegegevens van de oplossingsbestanden. 

### <a name="sign-in-to-docker"></a>Aanmelden bij Docker

Referenties van uw container registry Docker zodat deze de containerinstallatiekopie moet worden opgeslagen in het register kunt pushen. 

1. Open de Visual Studio Code geïntegreerde terminal door te selecteren **weergave** > **Terminal**.

2. Meld u aan Docker met de Azure container registry-referenties die u na het maken van het register hebt opgeslagen. 

   ```cmd/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   U ontvangt mogelijk een beveiligingswaarschuwing voor het gebruik van aanbevelen `--password-stdin`. Hoewel deze aanbevolen procedure wordt aanbevolen voor productiescenario's, valt buiten het bereik van deze zelfstudie. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) verwijzing.

### <a name="build-and-push"></a>Maken en pushen 

Visual Studio-Code heeft nu toegang tot uw containerregister, dus is het tijd om in te schakelen van de code van de oplossing in een containerinstallatiekopie. 

1. Klik in de Visual Studio Code explorer met de rechtermuisknop op de **deployment.template.json** bestand en selecteer **Build and Push IoT Edge-oplossing**. 

   ![Bouw en push IoT Edge-modules](./media/tutorial-develop-for-linux/build-and-push-modules.png)

   De opdracht build- en push start drie bewerkingen. Eerst een nieuwe map wordt gemaakt in de oplossing met de naam **config** die de volledige implementatie oplossing voor het manifest, gebouwd om van gegevens in de sjabloon voor de implementatie en andere bestanden bevat. Ten tweede, deze wordt uitgevoerd `docker build` om de containerinstallatiekopie op basis van het dockerfile dat geschikt is voor uw doel-architectuur te bouwen. Vervolgens wordt deze uitgevoerd `docker push` naar de opslagplaats voor installatiekopieën pushen naar uw containerregister. 

   Dit proces kan enkele minuten voor het eerst duren, maar is sneller de volgende keer dat u de opdrachten uitvoeren. 

2. Open de **deployment.amd64.json** bestand in de configuratie van de zojuist gemaakte map. De bestandsnaam weerspiegelt de doel-architectuur, zodat verschillende als u een andere architectuur hebt gekozen.

3. U ziet dat de twee parameters die al enige tijd tijdelijke aanduidingen door de juiste waarden zijn ingevuld. De **registryCredentials** sectie heeft uw register gebruikersnaam en wachtwoord die zijn opgehaald uit het .env-bestand. De **SampleModule** heeft de volledige installatiekopie-opslagplaats met de naam, versie en architectuur-code uit het bestand module.json. 

4. Open de **module.json** bestand in de map SampleModule. 

5. Wijzig het versienummer voor de installatiekopie van de module. (De versie, niet de $schema-versie.) Bijvoorbeeld: verhoog het versienummer van de patch op **0.0.2** alsof we een kleine oplossing in de module-code heeft gemaakt. 

   >[!TIP]
   >Moduleversies versiebeheer inschakelen en kunnen u wijzigingen op een klein aantal apparaten te testen voordat u updates implementeert naar productie. Als u niet de versie van de module verhogen voor het bouwen en te pushen, overschrijft u de opslagplaats in uw containerregister. 

6. Sla uw wijzigingen in het bestand module.json.

7. Met de rechtermuisknop op de **deployment.template.json** bestand opnieuw uit en selecteer opnieuw **Build and Push IoT Edge-oplossing**.

8. Open de **deployment.amd64.json** bestand opnieuw uit. U ziet dat er een nieuw bestand is niet gemaakt wanneer u de opdracht build- en push opnieuw uitgevoerd. In plaats daarvan is hetzelfde bestand bijgewerkt om de wijzigingen weer te geven. De installatiekopie van het SampleModule verwijst nu naar de 0.0.2 versie van de container. 

9. Verder controleren wat de build- en push-opdracht hebt gedaan, gaat u naar de [Azure-portal](https://portal.azure.com) en navigeer naar het containerregister. 

10. Selecteer in het containerregister **opslagplaatsen** vervolgens **samplemodule**. Controleer of dat beide versies van de installatiekopie naar het register zijn gepusht.

   ![Beide versies van een installatiekopie in containerregister weergeven](./media/tutorial-develop-for-linux/view-repository-versions.png)

<!--Alternative steps: Use VS Code Docker tools to view ACR images with tags-->

### <a name="troubleshoot"></a>Problemen oplossen

Als u fouten bij het maken en pushen van de installatiekopie van de module optreden, heeft dit vaak doen met de configuratie van Docker op uw ontwikkelcomputer. Gebruik de volgende controles voor de configuratie controleren: 

* Hebt u uitvoeren de `docker login` opdracht met de referenties die u hebt gekopieerd uit uw container registry? Deze referenties zijn anders dan degene die u gebruiken om aan te melden bij Azure. 
* Hebt u de juiste containeropslagplaats? Beschikt het over de juiste naam van het containerregister en de naam van de juiste module? Open de **module.json** bestand in de map SampleModule om te controleren. De waarde van de opslagplaats moet eruitzien als  **\<registernaam\>.azurecr.io/samplemodule**. 
* Als u een andere naam dan gebruikt **SampleModule** voor uw module, wordt u die naam consistent wordt gebruikt in de oplossing?
* Wordt op uw machine hetzelfde type containers die u bouwt uitgevoerd? Deze zelfstudie is voor Linux IoT Edge-apparaten, zodat Visual Studio Code melding **amd64** of **arm32v7** in de zijbalk en Linux-containers, Docker Desktop moet worden uitgevoerd. C-modules in Visual Studio Code bieden geen ondersteuning voor Windows-containers. 

## <a name="deploy-modules-to-device"></a>Modules op apparaat implementeren

U verifiëren dat de gemaakte containerinstallatiekopieën worden opgeslagen in uw container registry, dus is het tijd om ze te implementeren op een apparaat. Zorg ervoor dat uw IoT Edge-apparaat actief en werkend is. 

1. Vouw de sectie Azure IoT Hub-apparaten in de Visual Studio Code-Verkenner. 

2. Met de rechtermuisknop op het IoT Edge-apparaat dat u wilt implementeren op en selecteer vervolgens **implementatie maken voor één apparaat**. 

   ![Implementatie voor één apparaat maken](./media/tutorial-develop-for-linux/create-deployment.png)

3. Navigeer in de Verkenner naar de **config** map Selecteer vervolgens de **deployment.amd64.json** bestand. 

   Gebruik niet de deployment.template.json-bestand, dat niet beschikt over de container registerreferenties of module image-waarden in deze. Als u een Linux ARM32-apparaat hebt geconfigureerd, worden het manifest van de implementatie de naam deployment.arm32v7.json. 

4. Vouw de details voor uw IoT Edge-apparaat, en vouw vervolgens de **Modules** lijst voor uw apparaat. 

5. Gebruik de vernieuwknop om bij te werken van de apparaatweergave tot u ziet de tempSensor en SampleModule modules die worden uitgevoerd op uw apparaat. 

   Het duurt een paar minuten voor beide modules om te starten. IoT Edge-runtime moet voor het ontvangen van de nieuwe implementatie van het manifest, haal de module-installatiekopieën van de container-runtime en start u elke nieuwe module. 

   ![Weergave-modules die worden uitgevoerd op uw IoT Edge-apparaat](./media/tutorial-develop-for-linux/view-running-modules.png)

## <a name="view-messages-from-device"></a>Berichten van apparaat weergeven

De code SampleModule ontvangt berichten via de invoerwachtrij en doorgegeven aan via de uitvoerwachtrij. Het manifest implementatie gedeclareerd routes die berichten doorgegeven aan SampleModule van tempSensor en die berichten vanaf SampleModule doorgestuurd naar IoT Hub. De Azure IoT-hulpprogramma's voor Visual Studio Code kunnen u berichten te zien wanneer ze op IoT-Hub van uw afzonderlijke apparaten binnenkomen. 

1. Klik in de Visual Studio Code explorer met de rechtermuisknop op het IoT Edge-apparaat dat u wilt controleren, en selecteer vervolgens **Start Monitoring D2C Message**. 

2. Bekijk het uitvoervenster van Visual Studio Code om te zien van berichten die binnenkomen in uw IoT-hub. 

   ![Binnenkomende apparaat naar cloud-berichten bekijken](./media/tutorial-develop-for-linux/view-d2c-messages.png)

## <a name="view-changes-on-device"></a>Wijzigingen weergeven op apparaat

Als u zien wat er gebeurt op het apparaat zelf wilt, gebruikt u de opdrachten in deze sectie om te controleren van de IoT Edge-runtime en -modules die worden uitgevoerd op uw apparaat. 

De opdrachten in deze sectie zijn voor uw IoT Edge-apparaat, niet op uw ontwikkelcomputer. Als u een virtuele machine voor uw IoT Edge-apparaat, verbinding maken met het nu. Ga naar de pagina overzicht van de virtuele machine in Azure, en selecteer **Connect** voor toegang tot de secure shell-verbinding. 

* Weergeven van alle modules die zijn geïmplementeerd op het apparaat en hun status controleren:

   ```bash
   iotedge list
   ```

   Ziet u vier modules: de twee IoT Edge-runtime-modules, tempSensor en SampleModule. Alle vier moeten worden vermeld als die wordt uitgevoerd.

* Controleer de logboeken voor een specifieke module:

   ```bash
   iotedge logs <module name>
   ```

   IoT Edge-modules zijn hoofdlettergevoelig. 

   Ze zijn verwerking van berichten moeten worden weergegeven door de tempSensor en SamplModule Logboeken. De module edgeAgent is verantwoordelijk voor het starten van de andere modules, zodat de logboeken wordt informatie over het implementeren van het manifest van de implementatie. Als een module wordt niet weergegeven of niet wordt uitgevoerd, wordt de logboeken edgeAgent waarschijnlijk de fouten hebben. De module edgeHub is verantwoordelijk voor de communicatie tussen de modules en IoT-Hub. Als de modules zijn en die wordt uitgevoerd, maar de berichten die niet zijn binnenkomen in uw IoT-hub, wordt de fouten waarschijnlijk in de logboeken edgeHub hebben. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, instellen van Visual Studio Code op uw ontwikkelcomputer en uw eerste IoT Edge module op basis van deze geïmplementeerd. Nu u de basisbegrippen kent, kunt u proberen functionaliteit toevoegen aan een module zodat deze de passeren gegevens kunt analyseren. Kies de taal van uw voorkeur: 

> [!div class="nextstepaction"] 
> [C](tutorial-c-module.md)
> [C#](tutorial-csharp-module.md)
> [Java](tutorial-java-module.md)
> [Node.js](tutorial-node-module.md)
> [Python](tutorial-python-module.md)