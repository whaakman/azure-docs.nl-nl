---
title: Azure IoT rand Python-module | Microsoft Docs
description: Een IoT-Edge-module met Python-code maken en deze implementeren in een edge-apparaat
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: d5bad277e6a54b23f0e3ef7321e82d212ae885d3
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device---preview"></a>Opstellen en implementeren van een IoT-rand Python-module voor uw gesimuleerde apparaat - voorbeeld

Rand van de IoT-modules kunt u code waarmee u uw bedrijfslogica rechtstreeks aan de rand van de IoT-apparaten implementeren. Deze zelfstudie wordt u begeleid bij het maken en implementeren van een module IoT rand waarmee sensorgegevens wordt gefilterd. U gebruikt het gesimuleerde rand van de IoT-apparaat die u hebt gemaakt in de implementatie van Azure IoT-Edge van een gesimuleerd apparaat in [Windows] [ lnk-tutorial1-win] of [Linux] [ lnk-tutorial1-lin]zelfstudies. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om te maken van een IoT rand Python-module
> * Gebruik Visual Studio Code en Docker voor een docker-installatiekopie maken en deze publiceren naar het register 
> * De module implementeren voor uw IoT-randapparaat
> * Gegevens weergeven die zijn gegenereerd


De rand van de IoT-module die u in deze zelfstudie maakt filtert de temperatuur gegevens die zijn gegenereerd door uw apparaat. Alleen wordt verzonden berichten stroomopwaarts als de temperatuur hoger dan een opgegeven drempelwaarde is. Dit type analyse aan de rand is nuttig voor het beperken van de hoeveelheid gegevens wordt gecommuniceerd aan en opgeslagen in de cloud. 

> [!IMPORTANT]
> De Python-module kan momenteel alleen worden uitgevoerd in amd64 Linux containers. Kan niet worden uitgevoerd in Windows containers of op basis van ARM containers. 

## <a name="prerequisites"></a>Vereisten

* Azure IoT apparaat aan de rand die u hebt gemaakt in de Quick Start of de eerste zelfstudie.
* De verbindingsreeks van de primaire sleutel voor het IoT Edge-apparaat.  
* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge). 
* [Python-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python). 
* [Docker](https://docs.docker.com/engine/installation/) op dezelfde computer met Visual Studio Code. De Community Edition (CE) is voldoende voor deze zelfstudie. 
* [Python](https://www.python.org/downloads/).
* [PIP](https://pip.pypa.io/en/stable/installing/#installation) voor het installeren van Python-pakketten.

## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie maakt u de extensie Azure IoT rand voor VS-Code kunt gebruiken om te bouwen van een module en maak een **container installatiekopie** van de bestanden. En u deze installatiekopie push een **register** die worden opgeslagen en beheerd van uw afbeeldingen. U tot slot implementeren de installatiekopie uit het register uit te voeren op uw IoT-randapparaat.  

Voor deze zelfstudie kunt u een register Docker-compatibel. Twee populaire Docker Registerservices beschikbaar in de cloud zijn [Azure Container register](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Deze zelfstudie wordt Azure Container register. 

1. In de [Azure-portal](https://portal.azure.com), selecteer **maken van een resource** > **Containers** > **Azure Container register** .
2. Geef een naam op voor het register, kies een abonnement, kiest u een resourcegroep en de SKU ingesteld op **Basic**. 
3. Selecteer **Maken**.
4. Zodra het register van de container is gemaakt, gaat u naar het en selecteer **toegangssleutels**. 
5. Wisselknop **gebruiker met beheerdersrechten** naar **inschakelen**.
6. Kopieer de waarden voor **Login-server**, **gebruikersnaam**, en **wachtwoord**. Verderop in de zelfstudie gebruikt u deze waarden. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT-rand module-project maken
De volgende stappen ziet u het maken van een IoT rand Python-module met behulp van Visual Studio Code en de extensie Azure IoT rand.
1. Selecteer in Visual Studio Code **weergave** > **geïntegreerde Terminal** openen van de VS Code geïntegreerde terminal.
2. Voer de volgende opdracht om te installeren (of werk) in de geïntegreerde terminal de **cookiecutter**:

    ```cmd/sh
    pip install -U cookiecutter
    ```

3. Maak een project voor de nieuwe module. De volgende opdracht maakt u de projectmap **FilterModule**, met de container-opslagplaats. De parameter van `image_repository` moet in de vorm van `<your container registry name>.azurecr.io/filtermodule` als u van Azure container register gebruikmaakt. Voer de volgende opdracht in de huidige map:

    ```cmd/sh
    cookiecutter --no-input https://github.com/Azure/cookiecutter-azure-iot-edge-module module_name=FilterModule image_repository=<your container registry address>/filtermodule
    ```
 
4. Selecteer **bestand** > **Open map**.
5. Blader naar de **FilterModule** map en klik op **map selecteren** openen van het project in VS-Code.
6. Klik in Verkenner VS-Code op **main.py** om dit te openen.
7. Aan de bovenkant van de **FilterModule** naamruimte, importeer de `json` bibliotheek:

    ```python
    import json
    ```

8. Voeg de `TEMPERATURE_THRESHOLD` en `TWIN_CALLBACKS` onder de algemene items. De drempelwaarde temperatuur stelt u de waarde die de gemeten temperatuur moet overschrijden om de gegevens worden verzonden naar IoT Hub.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

9. Bijwerken van de functie `receive_message_callback` met onderstaande inhoud.

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

10. Voeg een nieuwe functie `device_twin_callback`. Deze functie wordt aangeroepen wanneer de gewenste eigenschappen zijn bijgewerkt.

    ```python
    # device_twin_callback is invoked when twin's desired properties are updated.
    def device_twin_callback(update_state, payload, user_context):
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

11. In de klasse `HubManager`, een nieuwe regel toe te voegen de `__init__` methode voor het initialiseren van de `device_twin_callback` functie die u zojuist hebt toegevoegd.

    ```python
    # sets the callback when a twin's desired properties are updated.
    self.client.set_device_twin_callback(device_twin_callback, self)
    ```


12. Sla dit bestand.

## <a name="create-a-docker-image-and-publish-it-to-your-registry"></a>Een Docker-installatiekopie maken en deze publiceren naar het register

1. Aanmelden bij Docker hiertoe de volgende opdracht in de geïntegreerde terminal VS-Code in: 
     
   ```csh/sh
   docker login -u <username> -p <password> <Login server>
   ```
        
   Gebruik de gebruikersnaam, wachtwoord en login-server die u hebt gekopieerd uit uw Azure-container register wanneer u het hebt gemaakt.

2. In VS-Code explorer met de rechtermuisknop op de **module.json** -bestand en klik op **Build en Push IoT rand module Docker installatiekopie**. Selecteer in het pop-vervolgkeuzelijst aan de bovenkant van het venster tegenover Code, uw platform container, bijvoorbeeld **amd64** voor Linux-container. VS-Code containerize de `main.py` en de vereiste afhankelijkheden vervolgens dit doorgeven aan de container-registersleutel die u hebt opgegeven. Het mogelijk enkele minuten duren voordat de eerste keer is het image.

3. U kunt het adres van de installatiekopie volledige container met het label krijgen in de VS Code geïntegreerd terminal. Voor meer informatie over de definitie van de build en push kunt u verwijzen naar de `module.json` bestand.

## <a name="add-registry-credentials-to-edge-runtime"></a>Register-referenties aan de rand runtime toevoegen
De referenties voor uw register toevoegen aan de rand runtime op de computer waarop u het apparaat aan de rand worden uitgevoerd. Deze referenties geven de runtime toegang tot het ophalen van de container. 

- Voor Windows, moet u de volgende opdracht uitvoeren:
    
    ```cmd/sh
    iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

- Voer de volgende opdracht voor Linux:
    
    ```cmd/sh
    sudo iotedgectl login --address <your container registry address> --username <username> --password <password> 
    ```

## <a name="run-the-solution"></a>De oplossing uitvoeren

1. In de [Azure-portal](https://portal.azure.com), gaat u naar uw IoT-hub.
2. Ga naar **IoT Edge (Preview)** en selecteer uw IoT Edge-apparaat.
3. Selecteer **Modules ingesteld**. 
2. Controleer of de **tempSensor** module wordt automatisch ingevuld. Als dit niet het geval is, gebruikt u de volgende stappen toe te voegen:
    1. Selecteer **IoT rand Module toevoegen**.
    2. In de **naam** veld `tempSensor`.
    3. In de **installatiekopie URI** veld `microsoft/azureiotedge-simulated-temperature-sensor:1.0-preview`.
    4. Laat de overige instellingen ongewijzigd en klik op **opslaan**.
9. Voeg de **filterModule** module die u hebt gemaakt in de vorige secties. 
    1. Selecteer **IoT rand Module toevoegen**.
    2. In de **naam** veld `filterModule`.
    3. In de **installatiekopie URI** en voer het adres klopt; bijvoorbeeld `<your container registry address>/filtermodule:0.0.1-amd64`. U vindt het adres van de volledige installatiekopie uit de vorige sectie.
    4. Controleer de **inschakelen** vak zodat u de twin module kunt bewerken. 
    5. Vervang de JSON in het tekstvak voor de module met de volgende JSON: 

        ```json
        {
           "properties.desired":{
              "TemperatureThreshold":25
           }
        }
        ```
 
    6. Klik op **Opslaan**.
10. Klik op **Volgende**.
11. In de **Routes opgeven** stap, kopieert u de JSON onder in het tekstvak. Alle berichten publiceren modules naar de Edge-runtime. Declaratieve regels in de runtime definiëren waar de berichten stromen. In deze zelfstudie moet u twee routes. De eerste route transporten berichten van de temperatuursensor naar de filtermodule via het eindpunt 'input1', dat het eindpunt dat u hebt geconfigureerd met de **FilterMessages** handler. De tweede route transporten berichten van de module expressiefilter IoT-hub. In deze route `upstream` is een speciale bestemming waarin wordt uitgelegd rand Hub berichten verzenden naar IoT Hub. 

    ```json
    {
       "routes":{
          "sensorToFilter":"FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\"/modules/filterModule/inputs/input1\")",
          "filterToIoTHub":"FROM /messages/modules/filterModule/outputs/output1 INTO $upstream"
       }
    }
    ```

4. Klik op **Volgende**.
5. In de **Template bekijken** stap, klikt u op **indienen**. 
6. Ga terug naar de detailpagina voor rand van de IoT-apparaat en klikt u op **vernieuwen**. U ziet nu de nieuwe **filtermodule** uitgevoerd samen met de **tempSensor** module en de **IoT rand runtime**. 

## <a name="view-generated-data"></a>Gegevens weergeven die zijn gegenereerd

Voor het bewaken van het apparaat verzonden van uw IoT-randapparaat naar uw IoT-hub voor cloud-berichten:
1. De Azure IoT Toolkit-uitbreiding configureren met de verbindingsreeks voor uw IoT-hub: 
    1. De Code van de VS explorer openen door te selecteren **weergave** > **Explorer**. 
    3. Klik in de Verkenner op **IOT HUB-apparaten** en klik vervolgens op **... **. Klik op **verbindingsreeks van de IoT-Hub ingesteld** en geef de verbindingsreeks voor de IoT-hub die uw IoT-Edge-apparaat met in het pop-upvenster verbindt. 

        Als u wilt zoeken in de verbindingsreeks, klik op de tegel voor uw IoT-hub in de Azure portal en klik vervolgens op **gedeeld toegangsbeleid**. In **gedeeld toegangsbeleid**, klikt u op de **iothubowner** beleid en kopieert u de verbinding met IoT Hub tekenreeks in de **iothubowner** venster.   

1. Selecteer voor het bewaken van gegevens die binnenkomen in de IoT-hub **weergave** > **opdracht palet** en zoek naar de **IoT: D2C bericht bewaking starten** menuopdracht. 
2. Als u wilt stoppen met het bewaken van gegevens, gebruikt u de **IoT: D2C bericht bewaking stopt** menuopdracht. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een IoT-rand module met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT-randapparaat gemaakt. U kunt door te gaan naar een van de volgende zelfstudies voor meer informatie over andere manieren Azure IoT rand kunt u de gegevens in zakelijke inzichten aan de rand.

> [!div class="nextstepaction"]
> [Azure-functie als een module implementeren](tutorial-deploy-function.md)
> [Azure Stream Analytics implementeren als een module](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-tutorial1-win]: tutorial-simulate-device-windows.md
[lnk-tutorial1-lin]: tutorial-simulate-device-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
