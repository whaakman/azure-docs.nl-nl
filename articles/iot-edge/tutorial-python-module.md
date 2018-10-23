---
title: Zelfstudie voor Azure IoT Edge Python| Microsoft Docs
description: In deze zelfstudie ziet u hoe u een IoT Edge-module met Python-code maakt en deze implementeert op een Edge-apparaat.
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 1316dcaf32b709dbc7c07f7d82388082d8d6e6a9
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/15/2018
ms.locfileid: "49319639"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device"></a>Zelfstudie: een Python IoT Edge-module maken en implementeren op een gesimuleerd apparaat

U kunt Azure IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. U gebruikt het gesimuleerde IoT Edge-apparaat dat u hebt gemaakt in de quickstarts over het implementeren van Azure IoT Edge op een gesimuleerd apparaat in [Windows][lnk-quickstart-win] of [Linux][lnk-quickstart-lin]. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge Python-module te maken.
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren naar het register.
> * De module implementeren op uw IoT Edge-apparaat.
> * Gegenereerde gegevens weergeven.


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheid gegevens te reduceren die worden gecommuniceerd naar en worden opgeslagen in de cloud. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux](quickstart-linux.md).
* Python-modules voor IoT Edge ondersteunen geen Windows-apparaten.

Cloudresources:

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 

Ontwikkelingsresources:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code.
* [Python-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python) (Engelstalig). 
* [Docker CE](https://docs.docker.com/engine/installation/). 
* [Python](https://www.python.org/downloads/).
* [Pip](https://pip.pypa.io/en/stable/installing/#installation) voor de installatie van Python-pakketten (over het algemeen opgenomen in uw Python-installatie).

>[!Note]
>Zorg ervoor dat uw map `bin` zich op het pad voor uw platform bevindt. Normaal gesproken is dat `~/.local/` voor UNIX en macOS of `%APPDATA%\Python` voor Windows.

## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie gebruikt u de Azure IoT Edge-extensie voor VS Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

Voor deze zelfstudie kunt u elk register gebruiken dat compatibel is met Docker. Twee populaire Docker-registerservices die beschikbaar zijn in de cloud zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Azure Container Registry**.
2. Geef een naam op voor het register, kies een abonnement, kies een resourcegroep en stel de SKU in op **Basis**. 
3. Selecteer **Maken**.
4. Nadat het containerregister is gemaakt, bladert u ernaartoe en selecteert u **Toegangssleutels**. 
5. Stel de **Gebruiker met beheerdersrechten** in op **Inschakelen**.
6. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in de zelfstudie. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT Edge-moduleproject creëren
Met de volgende stappen maakt u een IoT Edge Python-module met behulp van Visual Studio Code en de Azure IoT Edge-extensie.

### <a name="create-a-new-solution"></a>Een nieuwe oplossing maken

Gebruik het Python-pakket **cookiecutter** om een Python-oplossingssjabloon te maken waarop u verder kunt bouwen. 

1. Selecteer in Visual Studio Code **View** > **Integrated Terminal** om de met VS code geïntegreerde terminal te openen.

2. Voer in de geïntegreerde terminal de volgende opdracht in voor het installeren (of bijwerken) van **cookiecutter**, wat u gebruikt om de IoT Edge-oplossingssjabloon te maken in VS Code:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```
   >[!Note]
   >Zorg ervoor dat de map waar cookiecutter wordt geïnstalleerd zich in het `Path` van uw omgeving bevindt, zodat het mogelijk is deze vanaf een opdrachtprompt aan te roepen. In Windows voegt u `%APPDATA%\Python\PythonVersion\Scripts` gewoonlijk toe waar uw versie van Python zich bevindt in plaats van PythonVersion.

3. Selecteer **View** > **Command Palette** om het VS Code-opdrachtpalet te openen. 

4. Voer in het opdrachtpalet de opdracht **Azure: Sign in** in en voer deze uit. Volg vervolgens de instructies om u aan te melden bij uw Azure-account. Als u al bent aangemeld, kunt u deze stap overslaan.

5. Voer in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Geef in het opdrachtpalet de volgende informatie op om de oplossing te maken: 

   1. Selecteer de map waarin u de oplossing wilt maken. 
   2. Geef een naam op voor de oplossing of houd de standaardnaam **EdgeSolution** aan.
   3. Kies **Python-module** als de modulesjabloon. 
   4. Geef de module de naam **PythonModule**. 
   5. Geef het Azure-containerregister dat u in de vorige sectie hebt gemaakt, op als de opslagplaats voor installatiekopieën voor de eerste module. Vervang **localhost:5000** door de gekopieerde waarde voor de aanmeldingsserver. De uiteindelijke tekenreeks ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/pythonmodule.
 
   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/tutorial-python-module/repository.png)

In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen. De werkruimte voor de oplossing bevat vijf onderdelen op het hoogste niveau. In deze zelfstudie gaan we niet het bestand **\.gitignore** bewerken. De map **modules** bevat de Python-code voor uw module evenals Docker-bestanden voor het bouwen van uw module als een containerinstallatiekopie. In het bestand **\.env** worden uw referenties voor het containerregister opgeslagen. Het bestand **deployment.template.json** bevat de gegevens die de IoT Edge-runtime gebruikt om modules op een apparaat te implementeren. 

Als u geen containerregister hebt opgegeven bij het maken van uw oplossing, maar de standaardwaarde localhost:5000 hebt geaccepteerd, is er geen \.env-bestand gemaakt. 

   ![Werkruimte voor de Python-oplossing](./media/tutorial-python-module/workspace.png)

### <a name="add-your-registry-credentials"></a>Uw registerreferenties toevoegen

In het omgevingsbestand worden de referenties voor de containeropslagplaats opgeslagen. Deze referenties worden gedeeld met de IoT Edge-runtime. De runtime heeft deze referenties nodig om uw persoonlijke installatiekopieën naar het IoT Edge-apparaat te halen. 

1. Open in VS Code Explorer het .env-bestand. 
2. Werk de velden **gebruikersnaam** en **wachtwoord** bij met de waarden die u hebt gekopieerd uit het Azure-containerregister. 
3. Sla dit bestand op. 

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

6. Voeg in de **HubManager**-klasse een nieuwe regel aan de **__init__**-methode toe om de zojuist toegevoegde functie **module_twin_callback** te initialiseren:

    ```python
    # Sets the callback when a module twin's desired properties are updated.
    self.client.set_module_twin_callback(module_twin_callback, self)
    ```

7. Sla dit bestand op.

## <a name="build-your-iot-edge-solution"></a>Uw eigen IoT Edge-oplossing bouwen

In de vorige sectie hebt u een IoT Edge-oplossing gemaakt en code toegevoegd aan de **PythonModule** om berichten te filteren waarin de gemelde temperatuur van de machine onder de aanvaardbare drempelwaarde is. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen. 

1. Meld u aan bij Docker door de volgende opdracht in de geïntegreerde Visual Studio Code-terminal in te voeren. Vervolgens kunt u de module-installatiekopie naar het Azure-containerregister pushen: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Gebruik de gebruikersnaam, het wachtwoord en de aanmeldingsserver die u in de eerste sectie hebt gekopieerd uit het Azure-containerregister. U kunt deze waarden ook ophalen in de sectie **Toegangssleutels** van het register in de Azure-portal.

2. Open in VS Code Explorer het bestand deployment.template.json in de werkruimte van de IoT Edge-oplossing. 

   Dit bestand zorgt ervoor dat deze twee modules worden geïmplementeerd in **$edgeAgent**: **tempSensor** (waarmee apparaatgegevens worden gesimuleerd) en **PythonModule**. De waarde **PythonModule.image** is ingesteld op een installatiekopie met de versie Linux amd64. Zie [Informatie over het gebruiken, configureren en hergebruiken van IoT Edge-modules](module-composition.md) voor meer informatie over distributiemanifesten.

   Dit bestand bevat ook uw registerreferenties. Uw gebruikersnaam en wachtwoord in het sjabloonbestand zijn tijdelijke aanduidingen. Wanneer u het distributiemanifest genereert, worden de velden bijgewerkt met de waarden die u aan het .env-bestand hebt toegevoegd. 

3. Voeg de moduledubbel **PythonModule** toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onderaan de sectie **moduleContent** in, na de moduledubbel **$edgeHub**: 
    ```json
        "PythonModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Sla dit bestand op.

5. Klik in VS Code Explorer met de rechtermuisknop op het bestand deployment.template.json en selecteer **Build and Push IoT Edge solution**. 

Wanneer u Visual Studio Code de opdracht geeft om uw oplossing te bouwen, wordt eerst een bestand deployment.json gemaakt in een nieuwe map genaamd **config** op basis van de informatie in de distributiesjabloon. Vervolgens worden twee opdrachten uitgevoerd in de geïntegreerde terminal: `docker build` en `docker push`. Met deze twee opdrachten wordt uw Python-code gebouwd, in een container opgeslagen en vervolgens naar het containerregister gepusht dat u hebt opgegeven toen u de oplossing initialiseerde. 

U kunt het volledige adres van de containerinstallatiekopie, inclusief de tag, zien in de opdracht `docker build` die wordt uitgevoerd in de geïntegreerde terminal van VS Code. Het adres van de installatiekopie is opgebouwd uit informatie uit het bestand module.json, in de indeling \<opslagplaats\>:\<versie\>-\<platform\>. Voor deze zelfstudie ziet dit adres er ongeveer als volgt uit: registryname.azurecr.io/pythonmodule:0.0.1-amd64.

## <a name="deploy-and-run-the-solution"></a>De oplossing implementeren en uitvoeren

In dit snelstartartikel voor het instellen van uw IoT Edge-apparaat hebt u een module geïmplementeerd met behulp van de Azure-portal. U kunt modules ook implementeren via de Azure IoT Toolkit-extensie voor Visual Studio Code. U hebt al een implementatiemanifest voorbereid voor uw scenario, namelijk het bestand **deployment.json**. U hoeft nu alleen nog maar een apparaat te selecteren dat de implementatie moet ontvangen.

1. Voer in het opdrachtenpalet van VS Code de opdracht **Azure IoT Hub: Select IoT Hub** uit. 

2. Kies het abonnement en de IoT-hub met het IoT Edge-apparaat dat u wilt configureren. 

3. Vouw in VS Code Explorer de sectie **Azure IoT Hub Devices** uit. 

4. Klik met de rechtermuisknop op de naam van het IoT Edge-apparaat en selecteer **Implementatie voor één apparaat maken**. 

   ![Implementatie voor één apparaat maken](./media/tutorial-python-module/create-deployment.png)

5. Selecteer het bestand **deployment.json** in de **configuratiemap** en klik vervolgens op **Edge-distributiemanifest selecteren**. Gebruik niet het bestand deployment.template.json. 

6. Klik op de knop Vernieuwen. U ziet nu dat de nieuwe **PythonModule** wordt uitgevoerd, samen met de module **TempSensor** en de **$edgeAgent** en **$edgeHub**. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Als u het implementatiemanifest op uw IoT Edge-apparaat toepast, verzamelt de IoT Edge-runtime op het apparaat de informatie over de nieuwe implementatie en wordt deze uitgevoerd. Modules die worden uitgevoerd op het apparaat en die niet zijn opgenomen in het implementatiemanifest, worden gestopt. Alle modules die ontbreken op het apparaat worden gestart. 

U kunt de status van uw IoT Edge-apparaat bekijken via de sectie **Azure IoT Hub Devices** van de Visual Studio Code explorer. Vouw de details van uw apparaat uit voor een overzicht van de modules die worden geïmplementeerd en uitgevoerd. 

Op het IoT Edge-apparaat kunt u de status van uw implementatiemodules bekijken met behulp van de opdracht `iotedge list`. U moet vier modules zien: de twee modules van de IoT Edge-runtime, tempSensor en de aangepaste module die u in deze zelfstudie hebt gemaakt. Het kan een paar minuten duren voordat alle modules zijn gestart, dus voer de opdracht opnieuw uit als u ze in eerste instantie niet allemaal ziet. 

Als u de berichten die worden gegenereerd door een module wilt weergeven, gebruikt u de opdracht `iotedge logs <module name>`. 

U kunt de berichten weergeven wanneer ze binnenkomen op uw IoT-hub met behulp van Visual Studio Code. 

1. Klik op het weglatingsteken (**...**) en selecteer **Controle D2C-berichten starten** om de gegevens te controleren die binnenkomen bij de IoT-hub.
2. Als u de D2C-berichten voor een specifiek apparaat wilt controleren, klikt u met de rechtermuisknop op dit apparaat in de lijst en selecteert u **Controle D2C-berichten starten**.
3. Voer de opdracht **Azure IoT Hub: Stop monitoring D2C message** uit in het opdrachtpalet om het controleren van berichten te stoppen. 
4. Als u de moduledubbel wilt weergeven of bewerken, klikt u met de rechtermuisknop op deze moduledubbel in de lijst en selecteert u **Moduledubbel bewerken**. Als u de moduledubbel wilt bijwerken, slaat u het dubbele JSON-bestand op, klikt u met de rechtermuisknop in de editor en selecteert u **Moduledubbel bijwerken**.
5. Als u Docker-logboeken wilt bekijken, installeert u [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) voor VS Code. U kunt de actieve modules lokaal zoeken in Docker Explorer. Klik in het contextmenu op **Logboeken weergeven** om ze te bekijken in de geïntegreerde terminal. 

## <a name="clean-up-resources"></a>Resources opschonen 

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

### <a name="delete-local-resources"></a>Lokale resources verwijderen

Als u de IoT Edge-runtime en de bijbehorende resources van uw apparaat wilt verwijderen, moet u de volgende opdrachten gebruiken. 

Verwijder de IoT Edge-runtime.

   ```bash
   sudo apt-get remove --purge iotedge
   ```

Wanneer de IoT Edge-runtime is verwijderd, worden de containers die deze heeft gemaakt gestopt. Ze worden echter niet van uw apparaat verwijderd. Geef alle containers weer.

   ```bash
   sudo docker ps -a
   ```

Verwijder de runtime-containers die op uw apparaat zijn gemaakt.

   ```bash
   docker rm -f edgeHub
   docker rm -f edgeAgent
   ```

Verwijder alle aanvullende containers die zijn vermeld in de `docker ps` uitvoer door te verwijzen naar de namen van containers. 

Verwijder de container-runtime.

   ```bash
   sudo apt-get remove --purge moby
   ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. U kunt verdergaan met de volgende zelfstudies om te leren hoe Azure IoT Edge u nog meer kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Azure-functies als module implementeren](tutorial-deploy-function.md)
> [Azure Stream Analytics als module implementeren](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
