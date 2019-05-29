---
title: Zelfstudie voor Node.js-module ontwikkelen voor Linux - Azure IoT Edge | Microsoft Docs
description: In deze zelfstudie ziet u hoe u een IoT Edge-module met Node.js-code maakt en deze implementeert op een Edge-apparaat
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/04/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 6c94ca3a82095736ef7d242987d1fbf66a825950
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306505"
---
# <a name="tutorial-develop-and-deploy-a-nodejs-iot-edge-module-for-linux-devices"></a>Zelfstudie: Ontwikkel en implementeer een Node.js IoT Edge-module voor Linux-apparaten

Gebruik Visual Studio Code ontwikkelen van Node.js-code en deze implementeren naar een Linux-apparaat met Azure IoT Edge. 

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. U gebruikt het gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de snelstarts. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge Node.js-module te maken
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar het register 
> * De module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheidgegevens die worden gecommuniceerd naar en opgeslagen in de cloud te reduceren. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Bereik van de oplossing

In deze zelfstudie laat zien hoe u voor het ontwikkelen van een module in **Node.js** met behulp van **Visual Studio Code**, en hoe u implementeert een **Linux apparaat**. IoT Edge biedt geen ondersteuning voor Node.js-modules voor Windows-apparaten.

Gebruik de volgende tabel om te begrijpen van de opties voor het ontwikkelen en implementeren van Node.js-modules: 

| Node.js | Visual Studio Code | Visual Studio 2017/2019 | 
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![VS Code gebruiken voor Node.js-modules op Linux AMD64](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![VS Code gebruiken voor Node.js-modules op Linux ARM32](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u zijn gebleven door middel van de vorige zelfstudie over het instellen van uw ontwikkelomgeving voor ontwikkeling van Linux-containers: [Ontwikkelen van IoT Edge-modules voor Linux-apparaten](tutorial-develop-for-linux.md). Door een van deze zelfstudie is voltooid, hebt u de volgende vereisten voldaan: 

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure.
* Een [Linux-apparaat met Azure IoT Edge](quickstart-linux.md)
* Een containerregister, bijvoorbeeld [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) geconfigureerd met de [hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) geconfigureerd voor het uitvoeren van Linux-containers.

Voor het ontwikkelen van een IoT Edge-module in Node.js, installeert u de volgende aanvullende vereisten op uw ontwikkelcomputer: 

* [Node.js en NPM](https://nodejs.org). Het NPM-pakket wordt gedistribueerd met Node.js. Dit betekent dat NMP automatisch wordt geïnstalleerd op de computer wanneer u Node.js downloadt.

## <a name="create-a-module-project"></a>Een module-project maken
De volgende stappen laten zien hoe u een IoT-Edge Node.js-module maakt met behulp van Visual Studio Code en de Azure IoT-hulpprogramma's.

### <a name="create-a-new-project"></a>Een nieuw project maken

Gebruik **NPM** om een Node.js-oplossingssjabloon te maken waarop u verder kunt bouwen. 

1. Selecteer in Visual Studio Code **View** > **Integrated Terminal** om de met VS code geïntegreerde terminal te openen.

2. Voer in de geïntegreerde terminal de volgende opdracht in om **yeoman** en de generator voor de Node.js Azure IoT Edge-module te installeren: 

    ```cmd/sh
    npm install -g yo generator-azure-iot-edge-module
    ```

3. Selecteer **View** > **Command Palette** om het VS Code-opdrachtpalet te openen. 

3. Typ in het opdrachtenpalet de opdracht **Azure: Aanmelden** in, voer deze uit en volg de instructies om u aan te melden bij uw Azure-account. Als u zich al hebt aangemeld, kunt u deze stap overslaan.

4. Typ in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Volg de aanwijzingen in het opdrachtpalet om uw oplossing te maken.

   | Veld | Value |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer waar VS Code de oplossingsbestanden moet maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in of accepteer de standaardnaam **EdgeSolution**. |
   | Modulesjabloon selecteren | Kies **Node.js Module**. |
   | Een modulenaam opgeven | Geef de module de naam **NodeModule**. |
   | Opslagplaats voor Docker-afbeeldingen voor de module opgeven | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. De containerafbeelding wordt vooraf gevuld vanuit de naam die u in de laatste stap hebt opgegeven. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br>De uiteindelijke opslagplaats voor de installatiekopie ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/nodemodule. |
 
   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/tutorial-node-module/repository.png)


### <a name="add-your-registry-credentials"></a>Uw registerreferenties toevoegen

In het omgevingsbestand worden de referenties voor de containeropslagplaats opgeslagen. Deze referenties worden gedeeld met de IoT Edge-runtime. De runtime heeft deze referenties nodig om uw persoonlijke installatiekopieën naar het IoT Edge-apparaat te halen. 

1. Open in VS Code Explorer het bestand **.env**. 
2. Werk de velden **gebruikersnaam** en **wachtwoord** bij met de waarden die u hebt gekopieerd uit het Azure-containerregister. 
3. Sla dit bestand op. 

### <a name="select-your-target-architecture"></a>Selecteer uw doel-architectuur

Visual Studio Code kunt op dit moment ontwikkelen met Node.js-modules voor Linux AMD64- en Linux ARM32v7 apparaten. U moet selecteren welke architectuur die u hebt geconfigureerd met elke oplossing, omdat de container is gemaakt en uitgevoerd anders voor elk architectuurtype. De standaardwaarde is Linux AMD64. 

1. Open het opdrachtenpalet en zoek **Azure IoT Edge: Doelplatform standaard ingesteld voor Edge-oplossing**, of Selecteer het pictogram van de snelkoppeling in de zijbalk aan de onderkant van het venster. 

2. Selecteer de doel-architectuur in de lijst met opties in het opdrachtenpalet. Voor deze zelfstudie gebruiken we een virtuele Ubuntu-machine als de IoT Edge-apparaat, zodat de standaardwaarde wordt **amd64**.

### <a name="update-the-module-with-custom-code"></a>De module bijwerken met aangepaste code

Elke sjabloon bevat voorbeeldcode. Met deze code worden gesimuleerde sensorgegevens uit de **tempSensor**-module gerouteerd naar IoT Hub. Voeg in deze sectie code toe om de berichten te analyseren met NodeModule voordat u ze verzendt. 

1. Open in VS Code Explorer **modules** > **NodeModule** > **app.js**.

2. Voeg een variabele voor de temperatuurdrempel toe onder de vereiste knooppuntmodules. De temperatuurdrempel bepaalt de waarde die de gemeten temperatuur moet overschrijden voordat de gegevens naar IoT Hub worden verzonden.

    ```javascript
    var temperatureThreshold = 25;
    ```

3. Vervang de hele functie `PipeMessage` door de functie `FilterMessage`.
    
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

4. Vervang in functie `client.on()` de functienaam `pipeMessage` door `filterMessage`.

    ```javascript
    client.on('inputMessage', function (inputName, msg) {
        filterMessage(client, inputName, msg);
        });
    ```

5. Kopieer het volgende codefragment in de functieaanroep `client.open()`, na `client.on()` binnen de `else`-instructie. Deze functie wordt aangeroepen wanneer de gewenste eigenschappen zijn bijgewerkt.

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

6. Sla het bestand app.js op.

7. Open in VS Code Explorer het bestand **deployment.template.json** in de werkruimte van de IoT Edge-oplossing.

8. Voeg de moduledubbel NodeModule toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onder aan de sectie `moduleContent` in, na de moduledubbel `$edgeHub`: 

   ```json
     "NodeModule": {
         "properties.desired":{
             "TemperatureThreshold":25
         }
     }
   ```

   ![Moduledubbel toevoegen aan implementatiesjabloon](./media/tutorial-node-module/module-twin.png)

9. Sla het bestand deployment.template.json op.


## <a name="build-and-push-your-module"></a>Bouw en stuur uw module

In de vorige sectie hebt u een oplossing die het IoT Edge en code toegevoegd aan de NodeModule die filtert berichten waarin de gerapporteerde machine temperatuur hoger is binnen de aanvaardbare grenzen. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen.

1. Open de met VS Code geïntegreerde terminal door **View** > **Terminal** te selecteren.

1. Meld u aan Docker met de volgende opdracht in de terminal. Meld u aan met de gebruikersnaam, wachtwoord en aanmeldingsserver van het Azure container registry. U vindt deze waarden uit de **toegangssleutels** gedeelte van uw register in Azure portal.
     
   ```bash
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   U ontvangt mogelijk een beveiligingswaarschuwing voor het gebruik van aanbevelen `--password-stdin`. Hoewel deze aanbevolen procedure wordt aanbevolen voor productiescenario's, valt buiten het bereik van deze zelfstudie. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) verwijzing.

2. Klik in VS Code Explorer met de rechtermuisknop op het bestand **deployment.template.json** en selecteer **Build and Push IoT Edge solution**.

   De opdracht build- en push start drie bewerkingen. Eerst een nieuwe map wordt gemaakt in de oplossing met de naam **config** die de volledige implementatie oplossing voor het manifest, gebouwd om van gegevens in de sjabloon voor de implementatie en andere bestanden bevat. Ten tweede, deze wordt uitgevoerd `docker build` om de containerinstallatiekopie op basis van het dockerfile dat geschikt is voor uw doel-architectuur te bouwen. Vervolgens wordt deze uitgevoerd `docker push` naar de opslagplaats voor installatiekopieën pushen naar uw containerregister.

## <a name="deploy-modules-to-device"></a>Modules op apparaat implementeren

Gebruik de Verkenner van Visual Studio Code en de hulpprogramma's voor Azure IoT-extensie aan het project module implementeren in uw IoT Edge-apparaat. U hebt al een implementatie-manifest voorbereid voor uw scenario, de **deployment.json** bestand in de config-map. U hoeft nu alleen nog maar een apparaat te selecteren dat de implementatie moet ontvangen.

Zorg ervoor dat uw IoT Edge-apparaat actief en werkend is.

1. Vouw in de Visual Studio Code explorer de **Azure IoT Hub-apparaten** sectie voor uw overzicht van IoT-apparaten.

2. Klik met de rechtermuisknop op de naam van het IoT Edge-apparaat en selecteer **Implementatie voor één apparaat maken**.

3. Selecteer het bestand **deployment.json** in de **configuratiemap** en klik vervolgens op **Edge-distributiemanifest selecteren**. Gebruik niet het bestand deployment.template.json.

4. Klik op de knop Vernieuwen. U ziet nu dat de nieuwe **NodeModule** wordt uitgevoerd, samen met de module **TempSensor** en de **$edgeAgent** en **$edgeHub**.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Als u het implementatiemanifest op uw IoT Edge-apparaat toepast, verzamelt de IoT Edge-runtime op het apparaat de informatie over de nieuwe implementatie en wordt deze uitgevoerd. Modules die worden uitgevoerd op het apparaat en die niet zijn opgenomen in het implementatiemanifest, worden gestopt. Alle modules die ontbreken op het apparaat worden gestart.

U kunt de status van uw IoT Edge-apparaat bekijken via de sectie **Azure IoT Hub Devices** van de Visual Studio Code explorer. Vouw de details van uw apparaat uit voor een overzicht van de modules die worden geïmplementeerd en uitgevoerd.

1. In de Visual Studio Code-Verkenner met de rechtermuisknop op de naam van uw IoT Edge-apparaat en selecteer **Start Monitoring ingebouwde gebeurtenis eindpunt**.

2. Bekijk de berichten die binnenkomen in uw IoT-Hub. Het duurt even voor de berichten binnenkomen, omdat het IoT Edge-apparaat moet de nieuwe implementatie ontvangen en alle modules te starten. We de wijzigingen in de code NodeModule wacht totdat de temperatuur machine 25 graden is bereikt voordat het verzenden van berichten. Het berichttype ook toegevoegd **waarschuwing** in de berichten die deze temperatuur drempelwaarde bereikt. 

## <a name="edit-the-module-twin"></a>De moduledubbel bewerken

We de moduledubbel NodeModule in het manifest van de implementatie hebt gebruikt voor het instellen van de drempelwaarde voor de temperatuur op 25 graden. De moduledubbel kunt u de functionaliteit wijzigen zonder de code van de module bijwerken.

1. Vouw de gegevens van uw IoT Edge-apparaat om te zien van de actieve modules in Visual Studio Code. 

2. Met de rechtermuisknop op **NodeModule** en selecteer **bewerken moduledubbel**. 

3. Zoek **TemperatureThreshold** in de gewenste eigenschappen. De waarde ervan wijzigen naar een nieuwe temperatuur 5 graden tot 10 graden hoger is dan de meest recente gemelde temperatuur. 

4. Sla het bestand van de dubbele module.

5. Met de rechtermuisknop op een willekeurige plaats in het deelvenster en selecteer Bewerken moduledubbel **Update moduledubbel**. 

6. De binnenkomende berichten voor apparaat-naar-cloud bewaken. U ziet de berichten die niet meer totdat de nieuwe temperatuur drempelwaarde is bereikt. 

## <a name="clean-up-resources"></a>Resources opschonen 

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Wanneer u gereed voor het bouwen van uw eigen modules bent, vindt u meer informatie over [ontwikkelen van uw eigen IoT Edge-modules](module-development.md) of hoe u [modules met Visual Studio Code ontwikkelen](how-to-vs-code-develop-module.md). U kunt doorgaan naar de volgende zelfstudies voor meer informatie over hoe Azure IoT Edge kunnen helpen bij het implementeren van Azure cloudservices om te verwerken en analyseren van gegevens aan de rand.

> [!div class="nextstepaction"]
> [Functies](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Machine Learning](tutorial-deploy-machine-learning.md)
> [Custom Vision Service](tutorial-deploy-custom-vision.md)
