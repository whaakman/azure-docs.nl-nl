---
title: Azure IoT Edge Python-module| Microsoft Docs
description: Een IoT Edge-module met Python-code make en implementeren op een Edge-apparaat
author: shizn
manager: ''
ms.author: xshi
ms.date: 03/18/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 88d772306cb9e67216b380aa885284ebedc77b5f
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632105"
---
# <a name="develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device---preview"></a>Een Python IoT Edge-module maken en implementeren op uw gesimuleerd apparaat - preview

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. U gebruikt het gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de zelfstudie Azure IoT Edge implementeren op een gesimuleerd apparaat in [Windows][lnk-tutorial1-win] of [Linux][lnk-tutorial1-lin]. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge Python-module te maken
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar het register 
> * De module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheidgegevens die worden gecommuniceerd naar en opgeslagen in de cloud te reduceren. 

> [!IMPORTANT]
> Momenteel kan de Python-module alleen worden uitgevoerd in amd64 Linux-containers, niet in Windows- of ARM-based containers. 

## <a name="prerequisites"></a>Vereisten

* Het Azure IoT Edge-apparaat dat u hebt gemaakt in de snelstartgids of de vorige zelfstudie.
* De verbindingsreeks van de primaire sleutel voor het IoT Edge-apparaat.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Python-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python) (Engelstalig). 
* [Docker](https://docs.docker.com/engine/installation/) op dezelfde computer met Visual Studio Code. De Community Edition (CE) is voldoende voor deze zelfstudie. 
* [Python](https://www.python.org/downloads/).
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) voor de installatie van Python-pakketten (over het algemeen opgenomen in uw Python-installatie).

## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie gebruikt u de Azure IoT Edge-extensie voor VS Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

Voor deze zelfstudie kunt u elk register gebruiken dat compatibel is met Docker. Twee populaire Docker-registerservices die beschikbaar zijn in de cloud zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Azure Container Registry**.
2. Geef een naam op voor het register, kies een abonnement, kies een resourcegroep en stel de SKU in op **Basis**. 
3. Selecteer **Maken**.
4. Zodra het containerregister is gemaakt, gaat u er naartoe en selecteert u **Toegangssleutels**. 
5. Stel de **Gebruiker met beheerdersrechten** in op **Inschakelen**.
6. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in de zelfstudie. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT Edge-moduleproject creëren
De volgende stappen laten zien hoe u een IoT-Edge Python-module maakt met behulp van Visual Studio Code en de Azure IoT Edge-extensie.
1. Selecteer in Visual Studio Code **View** > **Integrated Terminal** om de met VS code geïntegreerde terminal te openen.
2. Voer in de geïntegreerde terminal de volgende opdracht in om **cookiecutter** te installeren (of te updaten) (we stellen voor dit in een virtuele omgeving te doen of als gebruikersinstallatie zoals hieronder weergegeven):

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

3. Maak een project voor de nieuwe module. Met de volgende opdracht maakt u de projectmap, **FilterModule**, met de containeropslagplaats. De parameter van `image_repository` moet een indeling vergelijkbaar met `<your container registry name>.azurecr.io/filtermodule` hebben als u Azure Container Registry gebruikt. Voer de volgende opdracht in de huidige werkmap in:

    ```cmd/sh
    cookiecutter --no-input https://github.com/Azure/cookiecutter-azure-iot-edge-module module_name=FilterModule image_repository=<your container registry address>/filtermodule
    ```
 
4. Selecteer **Bestand** > **Map openen**.
5. Blader naar de map **FilterModule** en klik op **Map selecteren** om het project in VS Code te openen.
6. Klik in VS Code-verkenner op **main.py** om het te openen.
7. Importeer bovenaan de naamruimte **FilterModule** de`json`-bibliotheek:

    ```python
    import json
    ```

8. Voeg de `TEMPERATURE_THRESHOLD`, `RECEIVE_CALLBACKS` en `TWIN_CALLBACKS` toe onder de globale tellers. De temperatuurdrempel bepaalt de waarde die de gemeten temperatuur moet overschrijden voordat de gegevens naar IoT Hub worden verzonden.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = RECEIVE_CALLBACKS = 0
    ```

9. Update functie `receive_message_callback` met onderstaande inhoud.

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we will forward this message onto the "output1" queue.
    def receive_message_callback(message, hubManager):
        global RECEIVE_CALLBACKS
        global TEMPERATURE_THRESHOLD
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        message_text = message_buffer[:size].decode('utf-8')
        print("    Data: <<<{}>>> & Size={:d}".format(message_text, size))
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print("    Properties: {}".format(key_value_pair))
        RECEIVE_CALLBACKS += 1
        print("    Total calls received: {:d}".format(RECEIVE_CALLBACKS))
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature {} exceeds threshold {}".format(data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

10. Voeg een nieuwe functie `device_twin_callback` toe. Deze functie wordt aangeroepen wanneer de gewenste eigenschappen zijn geüpdatet.

    ```python
    # device_twin_callback is invoked when twin's desired properties are updated.
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print("\nTwin callback called with:\nupdateStatus = {}\npayload = {}\ncontext = {}".format(update_state, payload, user_context))
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print("Total calls confirmed: {:d}\n".format(TWIN_CALLBACKS))
    ```

11. Voeg in klasse `HubManager` een nieuwe regel toe aan methode `__init__` om de functie `device_twin_callback` die u net hebt toegevoegd te initialiseren.

    ```python
    # sets the callback when a twin's desired properties are updated.
    self.client.set_device_twin_callback(device_twin_callback, self)
    ```


12. Dit bestand opslaan.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. Meld u aan bij Docker door de volgende opdracht in de geïntegreerde VS Code-terminal in te voeren: 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Gebruik de gebruikersnaam, het wachtwoord en de aanmeldserver die u hebt gekopieerd uit uw Azure-containerregister toen u die hebt gemaakt.

2. Klik in VS Code explorer met de rechtermuisknop op het bestand **module.json** en klik op **Build and Push IoT Edge module Docker image** (Docker-installatiekopie voor IoT Edge-module maken en pushen). Selecteer in de vervolgkeuzelijst bovenaan het venster VS Code uw containerplatform, bijvoorbeeld **amd64** voor een Linux-container. VS Code plaatst de `main.py` en vereiste afhankelijkheden in een container en pusht deze naar het containerregister dat u hebt opgegeven. De eerste keer dat u de installatiekopie maakt, kan het enkele minuten duren.

3. U kunt het volledige adres van de containerinstallatiekopie, inclusief tag, in de geïntegreerde terminal van VS Code krijgen. Bekijk het `module.json`-bestand als u meer wilt weten over de build- en push-definitie.

## <a name="add-registry-credentials-to-edge-runtime"></a>Registerreferenties toevoegen aan uw Edge-runtime
Voeg de referenties voor uw register toe aan de Edge-runtime op de computer waarop u het Edge-apparaat uitvoert. Deze referenties geven de runtime toegang voor het ophalen van de container. 

- Voer voor Windows de volgende opdracht uit:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Voer voor Linux de volgende opdracht uit:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>De oplossing uitvoeren

1. Navigeer in [Azure Portal](https://portal.azure.com) naar uw IoT-hub.
2. Ga naar **IoT Edge (Preview)** en selecteer uw IoT Edge-apparaat.
3. Selecteer **Modules instellen**. 
2. Controleer of de **tempSensor**-module automatisch wordt gevuld. Zo niet, gebruik dan de volgende stappen om die toe te voegen:
    1. Selecteer **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `tempSensor` in.
    3. Voer in het veld **URI installatiekopie** `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview` in.
    4. Laat de overige instellingen ongewijzigd en klik op **Opslaan**.
9. Voeg de module **filterModule** toe die u hebt gemaakt in de vorige sectie. 
    1. Selecteer **IoT Edge-module toevoegen**.
    2. Voer in het veld **Naam** `filterModule` in.
    3. Voer in het veld **URI installatiekopie** het adres van uw installatiekopie in, bijvoorbeeld `<your container registry address>/filtermodule:0.0.1-amd64`. Het volledige adres van de installatiekopie vindt u in de vorige sectie.
    4. Schakel het vak **Inschakelen** in, zodat u de moduledubbel kunt bewerken. 
    5. Vervang de JSON in het tekstvak voor de moduledubbel met de volgende JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Klik op **Opslaan**.
10. Klik op **Volgende**.
11. Kopieer in de stap **Routes opgeven** de onderstaande JSON in het tekstvak. Modules publiceren alle berichten naar de Edge-runtime. Declaratieve regels in de runtime bepalen waar de berichten naartoe stromen. In deze zelfstudie heb je twee routes nodig. De eerste route vervoert berichten van de temperatuursensor naar de filtermodule via het eindpunt ‘input1’, het eindpunt dat u hebt geconfigureerd met de handler **FilterMessages**. Met de tweede route worden berichten van de filtermodule naar IoT Hub getransporteerd. In deze route is `upstream` een speciale bestemming die Edge Hub opdracht geeft berichten te verzenden naar IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Klik op **Volgende**.
5. Klik in de stap **Sjabloon controleren** op **Indienen**. 
6. Ga terug naar de detailpagina van het IoT Edge-apparaat en klik op **Vernieuwen**. U ziet nu dat de nieuwe **filtermodule** samen met de **tempSensor**-module en de **IoT Edge-runtime** wordt uitgevoerd. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Ga als volgt te werk om het apparaat te controleren op cloudberichten die vanaf uw IoT Edge-apparaat worden verzonden naar uw IoT Hub:
1. Configureer de Azure IoT Toolkit-extensie met de verbindingsreeks voor uw IoT Hub: 
    1. Open de VS Code Explorer door **Verkenner** > **bekijken** te selecteren. 
    3. Klik in de verkenner op **IOT HUB DEVICES** en klik vervolgens op **...** . Klik op **IoT Hub-verbindingsreeks instellen** en voer in het pop-upvenster de verbindingsreeks in voor de IoT Hub waarnaar uw IoT Edge-apparaat verbindt. 

        Om de verbindingsreeks te vinden, klikt u op de tegel voor uw IoT Hub in Azure Portal en vervolgens op **Gedeeld toegangsbeleid**. Klik in **Gedeeld toegangsbeleid** op het beleid **iothubowner** en kopieer de IoT Hub-verbindingsreeks in het venster **iothubowner**.   

1. Om gegevens die binnenkomen in de IoT Hub te controleren, selecteert u **View** > **Command Palette** en zoekt u naar de menuopdracht **IoT: Start monitoring D2C message**. 
2. Als u wilt stoppen met het controleren van gegevens, gebruikt u de menuopdracht **IoT: Stop monitoring D2C message**. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. U kunt doorgaan met en van de volgende zelfstudies om te leren waarmee Azure IoT Edge u nog meer kan helpen uw gegevens om te zetten in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Azure-functies als module implementeren](tutorial-deploy-function.md)
> [Azure Stream Analytics als module implementeren](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
