---
title: Maken van aangepaste Python-module - Azure IoT Edge | Microsoft Docs
description: In deze zelfstudie ziet u hoe u een IoT Edge-module met Python-code maakt en deze implementeert op een Edge-apparaat.
services: iot-edge
author: shizn
manager: philmea
ms.reviewer: kgremban
ms.author: xshi
ms.date: 03/24/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: d75e4c3e2b6f28dfbde670406854ac87790dd090
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850105"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-for-linux-devices"></a>Zelfstudie: Ontwikkel en implementeer een Python IoT Edge-module voor Linux-apparaten

Gebruik Visual Studio Code ontwikkelen van C-code en deze implementeren naar een Linux-apparaat met Azure IoT Edge. 

U kunt Azure IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie helpt u bij het maken en implementeren van een IoT Edge-module waarmee sensorgegevens op het IoT Edge-apparaat dat u hebt ingesteld in de Snelstartgids gefilterd. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge Python-module te maken.
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar het register.
> * De module implementeren op uw IoT Edge-apparaat.
> * Gegenereerde gegevens weergeven.


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheid gegevens te reduceren die worden gecommuniceerd naar en worden opgeslagen in de cloud. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Bereik van de oplossing

In deze zelfstudie laat zien hoe u voor het ontwikkelen van een module in **Python** met behulp van **Visual Studio Code**, en hoe u implementeert een **Linux apparaat**. IoT Edge biedt geen ondersteuning voor Python-modules voor Windows-apparaten. 

Gebruik de volgende tabel om te begrijpen van de opties voor het ontwikkelen en implementeren van Python-modules op Linux: 

| Python | Visual Studio Code | Visual Studio 2017/2019 | 
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![VS Code voor Python-modules op Linux AMD64 gebruiken](./media/tutorial-c-module/green-check.png) |  |
| **Linux ARM32** | ![VS Code voor Python-modules op Linux ARM32 gebruiken](./media/tutorial-c-module/green-check.png) |  |

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u zijn gebleven door middel van de vorige zelfstudie over het instellen van uw ontwikkelomgeving voor ontwikkeling van Linux-containers: [Ontwikkelen van IoT Edge-modules voor Linux-apparaten](tutorial-develop-for-linux.md). Door een van deze zelfstudie is voltooid, hebt u de volgende vereisten voldaan: 

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure.
* Een [Linux-apparaat met Azure IoT Edge](quickstart-linux.md)
* Een containerregister, bijvoorbeeld [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) geconfigureerd met de [hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) geconfigureerd voor het uitvoeren van Linux-containers.

Voor het ontwikkelen van een IoT Edge-module in Python, installeert u de volgende aanvullende vereisten op uw ontwikkelcomputer: 

* [Python-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python) (Engelstalig). 
* [Python](https://www.python.org/downloads/).
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) voor de installatie van Python-pakketten (over het algemeen opgenomen in uw Python-installatie).

>[!Note]
>Zorg ervoor dat uw map `bin` zich op het pad voor uw platform bevindt. Normaal gesproken is dat `~/.local/` voor UNIX en macOS of `%APPDATA%\Python` voor Windows.

## <a name="create-a-module-project"></a>Een module-project maken
Met de volgende stappen maakt u een IoT Edge Python-module met behulp van Visual Studio Code en de Azure IoT-hulpprogramma's.

### <a name="create-a-new-project"></a>Een nieuw project maken

De VS Code gebruiken om een Python-oplossingssjabloon die u kunt bouwen op te maken. 

1. Selecteer in Visual Studio Code **View** > **Terminal** om de met VS code geïntegreerde terminal te openen.

1. Selecteer **View** > **Command Palette** om het VS Code-opdrachtpalet te openen. 

1. Voer in het opdrachtpalet de opdracht **Azure: Aanmelden** in, voer deze uit en volg de instructies om u aan te melden bij uw Azure-account. Als u al bent aangemeld, kunt u deze stap overslaan.

1. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Volg de aanwijzingen en geeft u de volgende informatie voor het maken van uw oplossing:

   | Veld | Waarde |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer waar VS Code de oplossingsbestanden moet maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in of accepteer de standaardnaam **EdgeSolution**. |
   | Modulesjabloon selecteren | Kies **Python-module**. |
   | Een modulenaam opgeven | Geef de module de naam **PythonModule**. |
   | Opslagplaats voor Docker-afbeeldingen voor de module opgeven | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. De containerafbeelding wordt vooraf gevuld vanuit de naam die u in de laatste stap hebt opgegeven. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br>De uiteindelijke opslagplaats voor de installatiekopie ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/pythonmodule. |
 
   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/tutorial-python-module/repository.png)


### <a name="add-your-registry-credentials"></a>Uw registerreferenties toevoegen

In het omgevingsbestand worden de referenties voor de containeropslagplaats opgeslagen. Deze referenties worden gedeeld met de IoT Edge-runtime. De runtime heeft deze referenties nodig om uw persoonlijke installatiekopieën naar het IoT Edge-apparaat te halen. 

1. Open in VS Code Explorer het bestand **.env**. 
2. Werk de velden **gebruikersnaam** en **wachtwoord** bij met de waarden die u hebt gekopieerd uit het Azure-containerregister. 
3. Sla het .env-bestand op. 

### <a name="select-your-target-architecture"></a>Selecteer uw doel-architectuur

Op dit moment kunt C-modules voor Linux AMD64- en Linux ARM32v7 apparaten ontwikkelen met Visual Studio Code. U moet selecteren welke architectuur die u hebt geconfigureerd met elke oplossing, omdat de container is gemaakt en uitgevoerd anders voor elk architectuurtype. De standaardwaarde is Linux AMD64. 

1. Open het opdrachtenpalet en zoek **Azure IoT Edge: Doelplatform standaard ingesteld voor Edge-oplossing**, of Selecteer het pictogram van de snelkoppeling in de zijbalk aan de onderkant van het venster. 

2. Selecteer de doel-architectuur in de lijst met opties in het opdrachtenpalet. Voor deze zelfstudie gebruiken we een virtuele Ubuntu-machine als de IoT Edge-apparaat, zodat de standaardwaarde wordt **amd64**. 

### <a name="update-the-module-with-custom-code"></a>De module bijwerken met aangepaste code

Elke sjabloon bevat voorbeeldcode. Met deze code worden gesimuleerde sensorgegevens uit de **tempSensor**-module gerouteerd naar de IoT-hub. In deze sectie voegt u de code toe waarmee de **PythonModule** wordt uitgebreid om de berichten te analyseren voordat u ze verzendt. 

1. Open in VS Code Explorer **modules** > **PythonModule** > **main.py**.

2. Importeer bovenaan het **main.py**-bestand de **JSON**-bibliotheek:

    ```python
    import json
    ```

3. Voeg de variabelen **TEMPERATURE_THRESHOLD** en **TWIN_CALLBACKS** toe onder de globale tellers. De temperatuurdrempel bepaalt de waarde die de gemeten temperatuur van de machine moet overschrijden voordat de gegevens naar de IoT-hub worden verzonden.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

4. Vervang de functie **receive_message_callback** door de volgende code:

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we forward this message to the "output1" queue.
    def receive_message_callback(message, hubManager):
        global RECEIVE_CALLBACKS
        global TEMPERATURE_THRESHOLD
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        message_text = message_buffer[:size].decode('utf-8')
        print ( "    Data: <<<%s>>> & Size=%d" % (message_text, size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature %s exceeds threshold %s" % (data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

5. Voeg een nieuwe functie toe met de naam **module_twin_callback**. Deze functie wordt aangeroepen wanneer de gewenste eigenschappen zijn bijgewerkt.

    ```python
    # module_twin_callback is invoked when the module twin's desired properties are updated.
    def module_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print ( "\nTwin callback called with:\nupdateStatus = %s\npayload = %s\ncontext = %s" % (update_state, payload, user_context) )
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print ( "Total calls confirmed: %d\n" % TWIN_CALLBACKS )
    ```

6. Voeg in de **HubManager**-klasse een nieuwe regel aan de **__init__** -methode toe om de zojuist toegevoegde functie **module_twin_callback** te initialiseren:

    ```python
    # Sets the callback when a module twin's desired properties are updated.
    self.client.set_module_twin_callback(module_twin_callback, self)
    ```

7. Sla het bestand main.py op.

8. Open in VS Code Explorer het bestand **deployment.template.json** in de werkruimte van de IoT Edge-oplossing. 

9. Voeg de moduledubbel **PythonModule** toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onderaan de sectie **moduleContent** in, na de moduledubbel **$edgeHub**: 

   ```json
       "PythonModule": {
           "properties.desired":{
               "TemperatureThreshold":25
           }
       }
   ```

   ![Moduledubbel toevoegen aan implementatiesjabloon](./media/tutorial-python-module/module-twin.png)

10. Sla het bestand deployment.template.json op.

## <a name="build-and-push-your-module"></a>Bouw en stuur uw module

In de vorige sectie hebt u een oplossing die het IoT Edge en code toegevoegd aan de PythonModule die filtert berichten waarin de gerapporteerde machine temperatuur hoger is binnen de aanvaardbare grenzen. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen.

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

4. Klik op de knop Vernieuwen. U ziet nu dat de nieuwe **PythonModule** wordt uitgevoerd, samen met de module **TempSensor** en de **$edgeAgent** en **$edgeHub**. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Als u het implementatiemanifest op uw IoT Edge-apparaat toepast, verzamelt de IoT Edge-runtime op het apparaat de informatie over de nieuwe implementatie en wordt deze uitgevoerd. Modules die worden uitgevoerd op het apparaat en die niet zijn opgenomen in het implementatiemanifest, worden gestopt. Alle modules die ontbreken op het apparaat worden gestart.

U kunt de status van uw IoT Edge-apparaat bekijken via de sectie **Azure IoT Hub Devices** van de Visual Studio Code explorer. Vouw de details van uw apparaat uit voor een overzicht van de modules die worden geïmplementeerd en uitgevoerd.

1. In de Visual Studio Code-Verkenner met de rechtermuisknop op de naam van uw IoT Edge-apparaat en selecteer **Start Monitoring ingebouwde gebeurtenis eindpunt**.

2. Bekijk de berichten die binnenkomen in uw IoT-Hub. Het duurt even voor de berichten binnenkomen, omdat het IoT Edge-apparaat moet de nieuwe implementatie ontvangen en alle modules te starten. We de wijzigingen in de code PythonModule wacht totdat de temperatuur machine 25 graden is bereikt voordat het verzenden van berichten. Het berichttype ook toegevoegd **waarschuwing** in de berichten die deze temperatuur drempelwaarde bereikt. 

## <a name="edit-the-module-twin"></a>De moduledubbel bewerken

We de moduledubbel PythonModule in het manifest van de implementatie hebt gebruikt voor het instellen van de drempelwaarde voor de temperatuur op 25 graden. De moduledubbel kunt u de functionaliteit wijzigen zonder de code van de module bijwerken.

1. Vouw de gegevens van uw IoT Edge-apparaat om te zien van de actieve modules in Visual Studio Code. 

2. Met de rechtermuisknop op **PythonModule** en selecteer **bewerken moduledubbel**. 

3. Zoek **TemperatureThreshold** in de gewenste eigenschappen. De waarde ervan wijzigen naar een nieuwe temperatuur 5 graden tot 10 graden hoger is dan de meest recente gemelde temperatuur. 

4. Sla het bestand van de dubbele module.

5. Met de rechtermuisknop op een willekeurige plaats in het deelvenster en selecteer Bewerken moduledubbel **Update moduledubbel**. 

6. De binnenkomende berichten voor apparaat-naar-cloud bewaken. U ziet de berichten die niet meer totdat de nieuwe temperatuur drempelwaarde is bereikt. 

## <a name="clean-up-resources"></a>Resources opschonen 

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en de Azure-resources dat u in dit artikel gebruikt om kosten te vermijden verwijderen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Wanneer u gereed voor het bouwen van uw eigen modules bent, vindt u meer informatie over [ontwikkelen van uw eigen IoT Edge-modules](module-development.md) of hoe u [modules met Visual Studio Code ontwikkelen](how-to-vs-code-develop-module.md). U kunt doorgaan naar de volgende zelfstudies voor meer informatie over hoe Azure IoT Edge kunnen helpen bij het implementeren van Azure cloudservices om te verwerken en analyseren van gegevens aan de rand.

> [!div class="nextstepaction"]
> [Functies](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Machine Learning](tutorial-deploy-machine-learning.md)
> [Custom Vision Service](tutorial-deploy-custom-vision.md)