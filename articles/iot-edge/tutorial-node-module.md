---
title: Aangepaste Node.js-module maken - Azure IoT Edge | Microsoft Docs
description: In deze zelfstudie ziet u hoe u een IoT Edge-module met Node.js-code maakt en deze implementeert op een Edge-apparaat
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: f5cf14ae1dcbbb00d723a86213c2707ad91794b7
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564915"
---
# <a name="tutorial-develop-and-deploy-a-nodejs-iot-edge-module-to-your-simulated-device"></a>Zelfstudie: Een Node.js IoT Edge-module maken en implementeren op een gesimuleerd apparaat

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. U gebruikt het gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de snelstarts. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge Node.js-module te maken
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar het register 
> * De module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheidgegevens die worden gecommuniceerd naar en opgeslagen in de cloud te reduceren. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux-](quickstart-linux.md) of [Windows-apparaten](quickstart.md).
* Als u IoT Edge onder Windows uitvoert: Node.js-modules worden niet door IoT Edge-versie 1.0.5 ondersteund. Zie [1.0.5 release notes](https://github.com/Azure/azure-iotedge/releases/tag/1.0.5) (Opmerkingen bij de release 1.0.5) voor meer informatie. Zie [Update the IoT Edge security daemon and runtime](how-to-update-iot-edge.md) (De IoT Edge-beveiligings-daemon en runtime bijwerken) voor stappen voor het installeren van specifieke versies.

Cloudresources:

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 

Ontwikkelingsresources:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT-hulpprogramma's](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code. 
* [Docker CE](https://docs.docker.com/engine/installation/). 
* [Node.js en NPM](https://nodejs.org). Het NPM-pakket wordt gedistribueerd met Node.js. Dit betekent dat NMP automatisch wordt geïnstalleerd op de computer wanneer u Node.js downloadt.

## <a name="create-a-container-registry"></a>Een containerregister maken

In deze zelfstudie gebruikt u de Azure IoT-hulpprogramma's voor Visual Studio Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

U kunt een Docker-register gebruiken om de containerinstallatiekopieën op te slaan. Twee populaire Docker-registerservices zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

Als u nog geen containerregister hebt, volgt u deze stappen om een nieuw containerregister te maken in Azure:

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Container Registry**.

2. Geef de volgende waarden op om uw containerregister te maken:

   | Veld | Waarde | 
   | ----- | ----- |
   | Registernaam | Geef hier een unieke naam op. |
   | Abonnement | Selecteer een abonnement in de vervolgkeuzelijst. |
   | Resourcegroep | Het wordt aangeraden om dezelfde resourcegroep te gebruiken voor alle test-resources die u maakt met de snelstartgidsen en zelfstudies voor IoT Edge, zoals **IoTEdgeResources**. |
   | Locatie | Kies een locatie dicht bij u in de buurt. |
   | Beheerder | Stel deze optie in op **Inschakelen**. |
   | SKU | Selecteer **Basic**. | **Terminal**

5. Selecteer **Maken**.

6. Nadat het containerregister is gemaakt, bladert u ernaartoe en selecteert u vervolgens **Toegangssleutels**. 

7. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in de zelfstudie om toegang te verlenen tot het containerregister. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT Edge-moduleproject creëren
De volgende stappen laten zien hoe u een IoT-Edge Node.js-module maakt met behulp van Visual Studio Code en de Azure IoT-hulpprogramma's.

### <a name="create-a-new-solution"></a>Een nieuwe oplossing maken

Gebruik **NPM** om een Node.js-oplossingssjabloon te maken waarop u verder kunt bouwen. 

1. Selecteer in Visual Studio Code **View** > **Integrated Terminal** om de met VS code geïntegreerde terminal te openen.

2. Voer in de geïntegreerde terminal de volgende opdracht in om **yeoman** en de generator voor de Node.js Azure IoT Edge-module te installeren: 

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. Selecteer **View** > **Command Palette** om het VS Code-opdrachtpalet te openen. 

3. Typ in het opdrachtenpalet de opdracht **Azure: Aanmelden** in, voer deze uit en volg de instructies om u aan te melden bij uw Azure-account. Als u zich al hebt aangemeld, kunt u deze stap overslaan.

4. Typ in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Volg de aanwijzingen in het opdrachtpalet om uw oplossing te maken.

   | Veld | Waarde |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer waar VS Code de oplossingsbestanden moet maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in of accepteer de standaardnaam **EdgeSolution**. |
   | Modulesjabloon selecteren | Kies **Node.js Module**. |
   | Een modulenaam opgeven | Geef de module de naam **NodeModule**. |
   | Opslagplaats voor Docker-afbeeldingen voor de module opgeven | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. De containerafbeelding wordt vooraf gevuld vanuit de naam die u in de laatste stap hebt opgegeven. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br>De uiteindelijke opslagplaats voor de installatiekopie ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/nodemodule. |
 
   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/tutorial-node-module/repository.png)

In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen. De werkruimte voor de oplossing bevat vijf onderdelen op het hoogste niveau. De map **modules** bevat de Node.js-code voor de module evenals Docker-bestanden voor het bouwen van uw module als een containerinstallatiekopie. In het bestand **\.env** worden uw referenties voor het containerregister opgeslagen. Het bestand **deployment.template.json** bevat de gegevens die de IoT Edge-runtime gebruikt om modules op een apparaat te implementeren. En het bestand **deployment.debug.template.json** bevat de foutopsporingsversie van modules. In deze zelfstudie gaan we niet de map **\.vscode** of het bestand **\.gitignore** bewerken. 

Als u geen containerregister hebt opgegeven bij het maken van uw oplossing, maar de standaardwaarde localhost:5000 hebt geaccepteerd, is er geen \.env-bestand gemaakt. 

<!--
   ![Node.js solution workspace](./media/tutorial-node-module/workspace.png)
-->

### <a name="add-your-registry-credentials"></a>Uw registerreferenties toevoegen

In het omgevingsbestand worden de referenties voor de containeropslagplaats opgeslagen. Deze referenties worden gedeeld met de IoT Edge-runtime. De runtime heeft deze referenties nodig om uw persoonlijke installatiekopieën naar het IoT Edge-apparaat te halen. 

1. Open in VS Code Explorer het bestand **.env**. 
2. Werk de velden **gebruikersnaam** en **wachtwoord** bij met de waarden die u hebt gekopieerd uit het Azure-containerregister. 
3. Sla dit bestand op. 

### <a name="update-the-module-with-custom-code"></a>De module bijwerken met aangepaste code

Elke sjabloon bevat voorbeeldcode. Met deze code worden gesimuleerde sensorgegevens uit de **tempSensor**-module gerouteerd naar IoT Hub. Voeg in deze sectie code toe om de berichten te analyseren met NodeModule voordat u ze verzendt. 

1. Open in VS Code Explorer **modules** > **NodeModule** > **app.js**.

5. Voeg een variabele voor de temperatuurdrempel toe onder de vereiste knooppuntmodules. De temperatuurdrempel bepaalt de waarde die de gemeten temperatuur moet overschrijden voordat de gegevens naar IoT Hub worden verzonden.

    ```javascript
    var temperatureThreshold = 25;
    ```

6. Vervang de hele functie `PipeMessage` door de functie `FilterMessage`.
    
    ```javascript
    // This function filters out messages that report temperatures below the temperature threshold.
    // It also adds the MessageType property to the message with the value set to Alert.
    function filterMessage(client, inputName, msg) {
        client.complete(msg, printResultFor('Receiving message'));
        if (inputName === 'input1') {
            var message = msg.getBytes().toString('utf8');
            var messageBody = JSON.parse(message);
            if (messageBody && messageBody.machine && messageBody.machine.temperature && messageBody.machine.temperature > temperatureThreshold) {
                console.log(`Machine temperature ${messageBody.machine.temperature} exceeds threshold ${temperatureThreshold}`);
                var outputMsg = new Message(message);
                outputMsg.properties.add('MessageType', 'Alert');
                client.sendOutputEvent('output1', outputMsg, printResultFor('Sending received message'));
            }
        }
    }

    ```

7. Vervang in functie `client.on()` de functienaam `pipeMessage` door `filterMessage`.

    ```javascript
    client.on('inputMessage', function (inputName, msg) {
        filterMessage(client, inputName, msg);
        });
    ```

8. Kopieer het volgende codefragment in de functieaanroep `client.open()`, na `client.on()` binnen de `else`-instructie. Deze functie wordt aangeroepen wanneer de gewenste eigenschappen zijn bijgewerkt.

    ```javascript
    client.getTwin(function (err, twin) {
        if (err) {
            console.error('Error getting twin: ' + err.message);
        } else {
            twin.on('properties.desired', function(delta) {
                if (delta.TemperatureThreshold) {
                    temperatureThreshold = delta.TemperatureThreshold;
                }
            });
        }
    });
    ```

9. Sla het bestand app.js op.

10. Open in VS Code Explorer het bestand **deployment.template.json** in de werkruimte van de IoT Edge-oplossing. Dit bestand laat aan de IoT Edge-agent weten welke modules moeten worden geïmplementeerd. In dit geval gaat het om **tempSensor** en **NodeModule**. Het bestand laat de IoT Edge-hub ook weten hoe berichten tussen de modules moeten worden gerouteerd. De Visual Studio Code-extensie vult automatisch het overgrote deel van de informatie in die u nodig hebt in de implementatiesjabloon. Controleer echter wel of alles klopt voor uw oplossing: 

   1. Het standaardplatform van uw IoT Edge-apparaat is ingesteld op **amd64** in de VS Code-statusbalk. Dit betekent dat **NodeModule** is ingesteld op de Linux amd64-versie van de installatiekopie. Wijzig in de statusbalk het standaardplatform van **amd64** in **arm32v7** of **windows-amd64** als dit de architectuur van het IoT Edge-apparaat is. 

      ![Het installatiekopieplatform van de module bijwerken](./media/tutorial-node-module/image-platform.png)

   2. Controleer of de sjabloon de juiste modulenaam heeft, niet de **SampleModule**-standaardnaam die u hebt gewijzigd tijdens het maken van de IoT Edge-oplossing.

   3. In de sectie **registryCredentials** worden uw Docker-registerreferenties opgeslagen zodat de IoT Edge-agent uw module-installatiekopie kan ophalen. De actuele gebruikersnaam en het bijbehorende wachtwoord zijn opgeslagen in het .env-bestand, dat wordt genegeerd door Git. Voeg uw referenties toe aan het ENV-bestand als u dat nog niet hebt gedaan.  

   4. Als u meer informatie wilt over distributiemanifesten, gaat u naar [Meer informatie over het implementeren van modules en het instellen van routes in IoT Edge](module-composition.md).

11. Voeg de moduledubbel NodeModule toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onder aan de sectie `moduleContent` in, na de moduledubbel `$edgeHub`: 

   ```json
       "NodeModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![Moduledubbel toevoegen aan implementatiesjabloon](./media/tutorial-node-module/module-twin.png)

12. Sla het bestand deployment.template.json op.


## <a name="build-your-iot-edge-solution"></a>Uw eigen IoT Edge-oplossing bouwen

In de vorige sectie hebt u een IoT Edge-oplossing gemaakt en code toegevoegd aan de NodeModule waarmee berichten worden gefilterd waarin de gemelde temperatuur van de computer onder de aanvaardbare drempelwaarde komt. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen. 

1. Meld u aan bij Docker door de volgende opdracht in te voeren in de geïntegreerde terminal van Visual Studio Code, zodat u de module-installatiekopie naar het ACR kunt pushen: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Gebruik de gebruikersnaam, het wachtwoord en de aanmeldingsserver die u in de eerste sectie hebt gekopieerd uit het Azure-containerregister. Of haal deze opnieuw op in de sectie **Toegangssleutels** van het register in Azure Portal.

2. Klik in VS Code Explorer met de rechtermuisknop op het bestand **deployment.template.json** en selecteer **Build and Push IoT Edge solution**. 

Wanneer u Visual Studio Code de opdracht geeft om uw oplossing te bouwen, wordt eerst een `deployment.json`-bestand gemaakt in een nieuwe **configuratiemap** op basis van de informatie in het distributiesjabloon. Vervolgens worden twee opdrachten uitgevoerd in de geïntegreerde terminal: `docker build` en `docker push`. Met deze twee opdrachten wordt uw code gebouwd, de Node.js-code opgeslagen in een container, en vervolgens naar het containerregister gepusht dat u hebt opgegeven toen u de oplossing initialiseerde. 

U kunt het volledige adres van de containerinstallatiekopie, inclusief de tag, zien in de opdracht `docker build` die wordt uitgevoerd in de geïntegreerde terminal van VS Code. Het adres van de installatiekopie is opgebouwd uit informatie uit het `module.json`-bestand, in de indeling **\<opslagplaats\>:\<versie\>-\<platform\>**. Voor deze zelfstudie ziet dit adres er ongeveer als volgt uit: **registryname.azurecr.io/nodemodule:0.0.1-amd64**.

>[!TIP]
>Als u een foutmelding krijgt bij het bouwen en pushen van de module, controleert u het volgende:
>* Hebt u zich bij Docker in Visual Studio Code aangemeld met de referenties uit uw containerregister? Deze referenties zijn anders dan de referenties die u gebruikt om u aan te melden bij de Azure-portal.
>* Hebt u de juiste containeropslagplaats? Open **modules** > **cmodule** > **module.json** en zoek het veld **opslagplaats**. De opslagplaats voor de installatiekopie ziet er ongeveer uit als **\<registryname\>.azurecr.io/nodemodule**. 
>* Bouwt u hetzelfde type containers dat door uw ontwikkelcomputer wordt uitgevoerd? Visual Studio Code wordt teruggezet op de Linux amd64-standaardcontainers. Als op uw ontwikkelcomputer Windows-containers of Linux arm32v7-containers worden uitgevoerd, werkt u het platform bij op de blauwe statusbalk onder aan het Visual Studio Code-venster, zodat dit overeenkomt met uw containerplatform.

## <a name="deploy-and-run-the-solution"></a>De oplossing implementeren en uitvoeren

In dit snelstartartikel voor het instellen van uw IoT Edge-apparaat hebt u een module geïmplementeerd met behulp van de Azure-portal. U kunt modules ook implementeren via de Azure IoT Hub Toolkit-extensie (voorheen Azure IoT Toolkit-extensie) voor Visual Studio Code. U hebt al een implementatiemanifest voorbereid voor uw scenario, namelijk het bestand **deployment.json**. U hoeft nu alleen nog maar een apparaat te selecteren dat de implementatie moet ontvangen.

1. Voer in het opdrachtpalet van VS Code de opdracht **Azure IoT Hub: Select IoT Hub**. 

2. Kies het abonnement en de IoT-hub met het IoT Edge-apparaat dat u wilt configureren. 

3. Vouw in VS Code Explorer de sectie **Azure IoT Hub Devices** uit. 

4. Klik met de rechtermuisknop op de naam van het IoT Edge-apparaat en selecteer **Implementatie voor één apparaat maken**. 

   ![Implementatie voor één apparaat maken](./media/tutorial-node-module/create-deployment.png)

5. Selecteer het bestand **deployment.json** in de **configuratiemap** en klik vervolgens op **Edge-distributiemanifest selecteren**. Gebruik niet het bestand deployment.template.json. 

6. Klik op de knop Vernieuwen. U ziet nu dat de nieuwe **NodeModule** wordt uitgevoerd, samen met de module **TempSensor** en de **$edgeAgent** en **$edgeHub**. 


## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Als u het implementatiemanifest op uw IoT Edge-apparaat toepast, verzamelt de IoT Edge-runtime op het apparaat de informatie over de nieuwe implementatie en wordt deze uitgevoerd. Modules die worden uitgevoerd op het apparaat en die niet zijn opgenomen in het implementatiemanifest, worden gestopt. Alle modules die ontbreken op het apparaat worden gestart. 

U kunt de status van uw IoT Edge-apparaat bekijken via de sectie **Azure IoT Hub Devices** van de Visual Studio Code explorer. Vouw de details van uw apparaat uit voor een overzicht van de modules die worden geïmplementeerd en uitgevoerd. 

Op het IoT Edge-apparaat kunt u de status van uw implementatiemodules bekijken met behulp van de opdracht `iotedge list`. U ziet vier modules: de twee modules van de IoT Edge-runtime, tempSensor en de aangepaste module die u in deze zelfstudie hebt gemaakt. Het kan een paar minuten duren voordat alle modules zijn gestart, dus voer de opdracht opnieuw uit als u ze in eerste instantie niet allemaal ziet. 

Als u de berichten die worden gegenereerd door een module wilt weergeven, gebruikt u de opdracht `iotedge logs <module name>`. 

U kunt de berichten weergeven wanneer ze binnenkomen op uw IoT-hub met behulp van Visual Studio Code. 

1. Klik op **...** en selecteer **Controle D2C-berichten starten** om de gegevens te controleren die binnenkomen bij de IoT-hub.
2. Als u de D2C-berichten voor een specifiek apparaat wilt controleren, klikt u met de rechtermuisknop op dit apparaat in de lijst en selecteert u **Controle D2C-berichten starten**.
3. Als u wilt stoppen met het bewaken van gegevens, voert u de opdracht **Azure IoT Hub: Stop monitoring D2C message** (Controle D2C-bericht stoppen) en voer deze uit. 
4. Als u de moduledubbel wilt weergeven of bewerken, klikt u met de rechtermuisknop op deze moduledubbel in de lijst en selecteert u **Moduledubbel bewerken**. Als u de moduledubbel wilt bijwerken, slaat u het dubbele JSON-bestand op, klikt u met de rechtermuisknop in de editor en selecteert u **Moduledubbel bijwerken**.
5. Als u Docker-logboeken wilt bekijken, kunt u [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) voor VS Code installeren. U kunt de actieve modules lokaal zoeken in Docker Explorer. Klik in het contextmenu op **Logboeken weergeven** om ze te bekijken in de geïntegreerde terminal. 

## <a name="clean-up-resources"></a>Resources opschonen 

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. U kunt doorgaan met en van de volgende zelfstudies om te leren waarmee Azure IoT Edge u nog meer kan helpen uw gegevens om te zetten in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Azure-functies als module implementeren](tutorial-deploy-function.md)
> [Azure Stream Analytics als module implementeren](tutorial-deploy-stream-analytics.md)

