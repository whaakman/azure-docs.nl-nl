---
title: Maken en implementeren van aangepaste modules - Machine Learning in Azure IoT Edge | Microsoft Docs
description: Maken en implementeren van IoT Edge-modules die gegevens van het leaf-apparaten via een machine learning-model verwerken en verzend de inzichten vervolgens naar IoT Hub.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 571a72d396c003bab363682559464715b180458a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67081390"
---
# <a name="tutorial-create-and-deploy-custom-iot-edge-modules"></a>Zelfstudie: Maken en implementeren van aangepaste IoT Edge-modules

> [!NOTE]
> In dit artikel maakt deel uit van een reeks voor een zelfstudie over het gebruik van Azure Machine Learning op IoT Edge. Als u rechtstreeks in dit artikel zijn ontvangen, u wordt aangeraden te beginnen met de [eerste artikel](tutorial-machine-learning-edge-01-intro.md) in de reeks voor de beste resultaten.

In dit artikel maken we drie IoT Edge-modules die berichten ontvangen van het leaf-apparaten, de gegevens uitvoeren via uw machine learning-model en stuur deze vervolgens inzichten met IoT Hub.

IoT Edge hub vergemakkelijkt de communicatie van de module-module. Met behulp van de IoT Edge houdt hub als een berichtenbroker modules onafhankelijk van elkaar. Modules hoeft alleen te geven van de invoer waarop ze berichten en de uitvoer waaraan ze berichten schrijven accepteren.

We willen het IoT Edge-apparaat om uit te voeren van vier dingen voor ons:

* Gegevens ontvangen van het leaf-apparaten
* Resterende Levensduur van het apparaat dat de gegevens zijn verzonden voorspellen
* Een bericht met alleen de resterende Levensduur van het apparaat verzenden naar IoT Hub (deze functie kan worden gewijzigd om alleen gegevens worden verzonden als de resterende bruikbare Levensduur onder een bepaald komt)
* Sla de leaf-apparaatgegevens naar een lokaal bestand op het IoT Edge-apparaat. Dit bestand wordt periodiek geüpload naar IoT Hub via bestand uploaden om training van de machine learning-model te verfijnen. Uploaden van bestanden in plaats van constante bericht streaming gebruiken is goedkoper.

Als u wilt uitvoeren van deze taken, gebruiken we drie aangepaste modules:

* **Classificatie van de resterende Levensduur:** De turboFanRulClassifier-module die we hebben gemaakt in [trainen en implementeren van een Azure Machine Learning-model](tutorial-machine-learning-edge-04-train-model.md) is van een standard machine learning module waarmee wordt aangegeven met de naam 'amlInput' en de uitvoer met de naam 'amlOutput' invoer. De 'amlInput' wordt verwacht dat de invoer voor precies hetzelfde uitzien als de invoer die we hebben verzonden met de ACI gebaseerde webservice. "AmlOutput" retourneert ook dezelfde gegevens als de webservice.

* **Avro-schrijver:** Deze module ontvangt berichten van de invoer van de 'avroModuleInput' en het bericht in een Avro-indeling naar schijf voor latere uploaden naar IoT-Hub zich blijft voordoen.

* **Routermodule:** De routermodule ontvangt berichten van downstream leaf-apparaten, en vervolgens indelingen en de berichten worden verzonden naar de classificatie. De module wordt vervolgens de berichten ontvangt van de classificatie en stuurt het bericht naar de Avro-schrijfmodule. Ten slotte verzendt de module alleen de resterende Levensduur voorspelling naar de IoT-Hub.

  * Invoer:
    * **deviceInput**: berichten ontvangt van het leaf-apparaten
    * **rulInput:** ontvangt berichten van de 'amlOutput"

  * Uitvoer:
    * **classificeren:** 'amlInput'-berichten verzendt
    * **writeAvro:** 'avroModuleInput'-berichten verzendt
    * **toIotHub:** berichten worden verzonden naar $upstream, die de berichten naar de gekoppelde IoT-Hub doorstuurt

In het onderstaande diagram ziet u de modules, invoer, uitvoer en de IoT Edge Hub-routes voor de volledige oplossing:

![Architectuurdiagram van IoT Edge drie modules](media/tutorial-machine-learning-edge-06-custom-modules/modules-diagram.png)

De stappen in dit artikel worden doorgaans uitgevoerd door een ontwikkelaar van de cloud.

## <a name="create-a-new-iot-edge-solution"></a>Een nieuwe IoT Edge-oplossing maken

Tijdens het uitvoeren van de tweede van onze twee Azure-laptops, we hebben gemaakt en gepubliceerd, een containerinstallatiekopie met onze RUL-model. Azure Machine Learning, als onderdeel van het kopieerproces ingebouwd in de onderdelen waarmee de installatiekopie kan worden geïmplementeerd als een Azure IoT Edge-module. In deze stap, we gaan maken van een Azure IoT Edge-oplossing met behulp van de module 'Azure Machine Learning' en wijst u de module aan de installatiekopie die we hebben gepubliceerd met behulp van Azure-Notebooks.

1. Open een extern-bureaubladsessie naar uw ontwikkelmachine.

2. Map openen **C:\\bron\\IoTEdgeAndMlSample** in Visual Studio Code.

3. Klik met de rechtermuisknop in het deelvenster explorer (in de lege ruimte) en selecteer **nieuwe IoT Edge-oplossing**.

    ![Nieuwe IoT Edge-oplossing maken](media/tutorial-machine-learning-edge-06-custom-modules/new-edge-solution-command.png)

4. Accepteer de standaardnaam van de oplossing **EdgeSolution**.

5. Kies **Azure Machine Learning** als de module-sjabloon.

6. Naam van de module **turbofanRulClassifier**.

7. Kies uw machine learning-werkruimte.

8. Selecteer de installatiekopie die u hebt gemaakt tijdens het uitvoeren van de Azure-Notebook.

9. Bekijk de oplossing en ziet u de bestanden die zijn gemaakt:

   * **deployment.template.json:** Dit bestand bevat de definitie van elk van de modules in de oplossing. Er zijn drie secties aandacht te besteden aan in dit bestand:

     * **Registerreferenties:** Definieert de set met aangepaste containerregisters die u in uw oplossing gebruikt. Rechts, deze moet bevatten nu het register van uw machine learning-werkruimte, dit is waar uw Azure Machine Learning-installatiekopie is opgeslagen. U kunt een willekeurig aantal containerregisters hebben, maar voor het gemak gebruiken we dit één register voor alle modules

       ```json
       "registryCredentials": {
         "<your registry>": {
           "username": "$CONTAINER_REGISTRY_USERNAME_<your registry>",
           "password": "$CONTAINER_REGISTRY_PASSWORD_<your registry>",
           "address": "<your registry>.azurecr.io”
         }
       }
       ```

     * **Modules:** In deze sectie bevat de verzameling van de gebruiker gedefinieerde modules die bij deze oplossing. U ziet in deze sectie bevat momenteel twee modules: tempSensor en turbofanRulClassifier. De tempSensor is geïnstalleerd door de sjabloon voor Visual Studio Code, maar is niet nodig voor deze oplossing. U kunt de definitie van de module tempSensor verwijderen uit de sectie modules. Houd er rekening mee dat de definitie van de module turbofanRulClassifier naar de afbeelding in uw containerregister verwijst. Als we meer modules aan de oplossing toevoegen, worden ze weergegeven in deze sectie.

       ```json
       "modules": {
         "tempSensor": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0",
             "createOptions": {}
           }
         },
         "turbofanRulClassifier": {
           "version": "1.0",
           "type": "docker",
           "status": "running",
           "restartPolicy": "always",
           "settings": {
             "image": "<your registry>.azurecr.io/edgemlsample:1",
             "createOptions": {}
           }
         }
       }
       ```

     * **Routes:** werken we met routes nogal in deze zelfstudie. Routes definiëren hoe modules met elkaar communiceren. De twee routes zijn gedefinieerd door de sjabloon komen niet overeen met de routering die we nodig hebben. De eerste route verzendt alle gegevens uit de uitvoer van de classificatie naar de IoT-Hub (upstream$). Er is een andere route voor tempSensor, die we zojuist hebt verwijderd. Verwijder de twee standaardroutes.

       ```json
       "$edgeHub": {
         "properties.desired": {
           "schemaVersion": "1.0",
           "routes": {
             "turbofanRulClassifierToIoTHub": "FROM /messages/modules/turbofanRulClassifier/outputs/\* INTO $upstream",
             "sensorToturbofanRulClassifier": "FROM /messages/modules/tempSensor/outputs/temperatureOutput INTO BrokeredEndpoint(\\"/modules/turbofanRulClassifier/inputs/input1\\")"
           },
           "storeAndForwardConfiguration": {
             "timeToLiveSecs": 7200
           }
         }
       }
       ```

   * **Deployment.Debug.template.JSON:** dit bestand is de foutopsporingsversie van deployment.template.json. We moeten alle wijzigingen van de deployment.template.json spiegelen in dit bestand.

   * **.env:** dit bestand is waar u de gebruikersnaam en wachtwoord voor toegang tot het register moet opgeven.

      ```env
      CONTAINER_REGISTRY_USERNAME_<your registry name>=<ACR username>
      CONTAINER_REGISTRY_PASSWORD_<your registry name>=<ACR password>
      ```

10. Klik met de rechtermuisknop op het bestand deployment.template.json in Visual Studio Code explorer en selecteer **IoT Edge-oplossing bouwen**.

11. U ziet dat deze opdracht maakt u een map configuratie met een deployment.amd64.json-bestand. Dit bestand is de concrete implementatiesjabloon voor de oplossing.

## <a name="add-router-module"></a>Router-module toevoegen

Vervolgens wordt de Router-module toevoegen aan onze oplossing. De module Router verwerkt verschillende verantwoordelijkheden voor onze oplossing:

* **Berichten ontvangen van het leaf-apparaten:** als berichten op het IoT Edge-apparaat vanaf downstream apparaten binnenkomen, de Router-module ontvangt het bericht en begint met het organiseren van de routering van het bericht.
* **Berichten verzenden naar de resterende Levensduur classificatie-module:** wanneer een nieuw bericht wordt ontvangen van een downstream-apparaat, het bericht met de indeling die de resterende Levensduur-classificatie wordt verwacht dat door de Router-module worden getransformeerd. De Router wordt het bericht verzonden naar de resterende Levensduur classificatie voor een voorspelling van de resterende Levensduur. Zodra de classificatie kan een voorspelling heeft gemaakt, wordt het bericht verzonden naar de Router-module.
* **Resterende Levensduur berichten verzenden naar IoT Hub:** de Router ontvangt berichten van de classificatie, wordt het bericht bevat alleen de essentiële informatie, apparaat-ID en de resterende Levensduur, transformeert en wordt het verkorte bericht verzonden naar de IoT hub. Een verdere verfijning, die we hier niet gedaan hebt, zou berichten verzenden naar de IoT-Hub, alleen wanneer de resterende Levensduur voorspelling onder de drempelwaarde (bijvoorbeeld valt wanneer de resterende Levensduur minder dan 100 cycli). Filteren op deze manier wilt verkleinen van berichten en kosten van de IoT-hub.
* **Bericht verzenden naar de Avro-schrijfmodule:** wilt behouden van de gegevens die zijn verzonden door de downstream-apparaat, verzendt de Router-module het hele bericht ontvangen van de classificatie op de Avro-schrijfmodule, waardoor de behouden en de gegevens met behulp van IoT Hub-bestand uploaden uploaden.

> [!NOTE]
> De beschrijving van de verantwoordelijkheden van de module kan de verwerking van opeenvolgende lijken maken, maar de stroom is op basis van berichten/gebeurtenissen. Daarom moeten we een orchestration-module, zoals de module Router.

### <a name="create-module-and-copy-files"></a>Module en kopieer de bestanden maken

1. Klik met de rechtermuisknop op de map modules in Visual Studio Code en kies **IoT Edge-Module toevoegen**.

2. Kies  **C# module**.

3. Naam van de module **turbofanRouter**.

4. Wanneer u wordt gevraagd naar uw opslagplaats voor Docker-installatiekopieën, gebruikt u het register van de machine learning-werkruimte (u vindt het register in het knooppunt registryCredentials van uw *deployment.template.json* bestand). Deze waarde is de volledig gekwalificeerde adres in het register, zoals  **\<uw register\>.azurecr.io/turbofanrouter**.

    > [!NOTE]
    > In dit artikel gebruiken we Azure Container Registry gemaakt met de werkruimte van de service Azure Machine Learning, die wordt gebruikt om te trainen en implementeren van onze classificatie. Dit is alleen voor het gemak. Er kan een nieuwe container registry hebt gemaakt en gepubliceerd er onze modules.

5. Een nieuw Terminalvenster openen in Visual Studio Code (**weergave** > **Terminal**) en kopieer de bestanden uit de map modules.

    ```cmd
    copy c:\source\IoTEdgeAndMlSample\EdgeModules\modules\turbofanRouter\*.cs c:\source\IoTEdgeAndMlSample\EdgeSolution\modules\turbofanRouter\
    ```

6. Wanneer u hierom wordt gevraagd om te overschrijven program.cs, drukt u op `y` en druk vervolgens op `Enter`.

### <a name="build-router-module"></a>Routermodule maken

1. Selecteer in Visual Studio Code, **Terminal** > **bouwen taak configureren standaard**.

2. Klik op **tasks.json-bestand maken met sjabloon**.

3. Klik op **.NET Core**.

4. Vervang de inhoud met wanneer tasks.json wordt geopend:

    ```json
    {
      // See https://go.microsoft.com/fwlink/?LinkId=733558
      // for the documentation about the tasks.json format
      "version": "2.0.0",
      "tasks": [
        {
          "label": "build",
          "command": "dotnet",
          "type": "shell",
          "group": {
            "kind": "build",
            "isDefault": true
          },
          "args": [
            "build",
            "${workspaceFolder}/modules/turbofanRouter"
          ],
          "presentation": {
            "reveal": "always"
          },
          "problemMatcher": "$msCompile"
        }
      ]
    }
    ```

5. Opslaan en sluiten tasks.json.

6. Samenstellen met `Ctrl + Shift + B` of **Terminal** > **bouwen taak uitvoeren**.

### <a name="set-up-module-routes"></a>Module routes instellen

Zoals eerder vermeld, IoT Edge-runtime maakt gebruik van routes die zijn geconfigureerd in de *deployment.template.json* bestand voor het beheren van communicatie tussen los gekoppelde modules. In deze sectie wordt over het instellen van de routes voor de module turbofanRouter inzoomen. We eerst betrekking hebben op de invoer routes en ga vervolgens verder de uitvoer.

#### <a name="inputs"></a>Invoer

1. In de methode Init() van Program.cs registreert we twee retouraanroepen voor de module:

   ```csharp
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromLeafDevice, LeafDeviceInputMessageHandler, ioTHubModuleClient);
   await ioTHubModuleClient.SetInputMessageHandlerAsync(EndpointNames.FromClassifier, ClassifierCallbackMessageHandler, ioTHubModuleClient);
   ```

2. De eerste callback luistert naar berichten naar de **deviceInput** sink. In het bovenstaande diagram ziet u dat we om berichten te routeren vanaf elk apparaat leaf naar deze invoer willen. In de *deployment.template.json* bestand, een route waarin de edge hub voor het routeren van alle berichten ontvangen door de IoT Edge-apparaat dat niet is verzonden door een IoT Edge-module in de invoer met de naam 'deviceInput' in de module turbofanRouter toevoegen:

   ```json
   "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")"
   ```

3. Naast een route voor berichten van de module rulClassifier toevoegen in de module turbofanRouter:

   ```json
   "classifierToRouter": "FROM /messages/modules/classifier/outputs/amloutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")"
   ```

#### <a name="outputs"></a>Uitvoer

Vier aanvullende routes toevoegen aan de parameter $edgeHub route voor het afhandelen van uitvoer van de Router-module.

1. Program.cs definieert de methode SendMessageToClassifier(), die een bericht verzenden naar de resterende Levensduur classificatie met behulp van de route met behulp van de module-client:

   ```json
   "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/classifier/inputs/amlInput\")"
   ```

2. SendRulMessageToIotHub() maakt gebruik van de module-client alleen de resterende Levensduur gegevens voor het apparaat verzenden naar de IoT-Hub via de route:

   ```json
   "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream"
   ```

3. SendMessageToAvroWriter() maakt gebruik van de module-client voor het verzenden van het bericht met de resterende Levensduur gegevens toegevoegd aan de module avroFileWriter.

   ```json
   "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")"
   ```

4. HandleBadMessage() verzendt upstream-mislukt berichten de IoT-Hub wanneer deze voor later kunnen worden gerouteerd.

   ```json
   "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
   ```

Knooppunt moet met alle routes die samen het '$edgeHub' eruitzien als de volgende JSON:

```json
"$edgeHub": {
  "properties.desired": {
    "schemaVersion": "1.0",
    "routes": {
      "leafMessagesToRouter": "FROM /messages/* WHERE NOT IS_DEFINED($connectionModuleId) INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/deviceInput\")",
      "classifierToRouter": "FROM /messages/modules/turbofanRulClassifier/outputs/amlOutput INTO BrokeredEndpoint(\"/modules/turbofanRouter/inputs/rulInput\")",
      "routerToClassifier": "FROM /messages/modules/turbofanRouter/outputs/classOutput INTO BrokeredEndpoint(\"/modules/turbofanRulClassifier/inputs/amlInput\")",
      "routerToIoTHub": "FROM /messages/modules/turboFanRouter/outputs/hubOutput INTO $upstream",
      "routerToAvro": "FROM /messages/modules/turbofanRouter/outputs/avroOutput INTO BrokeredEndpoint(\"/modules/avroFileWriter/inputs/avroModuleInput\")",
      "deadLetter": "FROM /messages/modules/turboFanRouter/outputs/deadMessages INTO $upstream"
    },
    "storeAndForwardConfiguration": {
      "timeToLiveSecs": 7200
    }
  }
}
```

> [!NOTE]
> De volgende aanvullende route toevoegen van de module turbofanRouter gemaakt: `turbofanRouterToIoTHub": "FROM /messages/modules/turbofanRouter/outputs/* INTO $upstream`. Verwijderen van deze route, zodat alleen de routes in uw bestand deployment.template.json hierboven wordt vermeld.

#### <a name="copy-routes-to-deploymentdebugtemplatejson"></a>Routes naar deployment.debug.template.json kopiëren

Als laatste stap, om te voorkomen dat de bestanden gesynchroniseerd, mirror van de wijzigingen in deployment.template.json in deployment.debug.template.json.

## <a name="add-avro-writer-module"></a>Schrijver van de Avro-module toevoegen

De Avro-schrijfmodule heeft twee verantwoordelijkheden in onze oplossing voor het opslaan van berichten en bestanden uploaden.

* **Berichten Store**: wanneer de schrijver van de Avro-module een bericht ontvangt, het bericht worden geschreven naar het lokale bestandssysteem in Avro-indeling. We gebruiken een koppelpunt bind, Hiermee wordt een map (in dit geval /data/avrofiles) in een pad in de container van de module. Deze koppeling kan de module te schrijven naar een lokaal pad (/ avrofiles) en de bestanden die toegankelijk is rechtstreeks vanaf de IoT Edge-apparaat hebt.

* **Bestanden uploaden**: de Avro-schrijfmodule maakt gebruik van de Azure IoT Hub-functionaliteit voor het uploaden van bestand bestanden uploaden naar Azure storage-account. Wanneer een bestand is geüpload, wordt het bestand met de module verwijderd van schijf

### <a name="create-module-and-copy-files"></a>Module en kopieer de bestanden maken

1. Zoek in het opdrachtenpalet en selecteer vervolgens **Python: Interpreter selecteren** in.

1. Kies de interpreter gevonden in de C:\\Python37.

1. Open het opdrachtenpalet opnieuw en zoek en selecteer vervolgens **Terminal: Selecteer de standaardshell**.

1. Wanneer u hierom wordt gevraagd, kiest u **opdrachtprompt**.

1. Open een nieuwe terminal-shell, **Terminal** > **nieuwe Terminal**.

1. Klik met de rechtermuisknop op de map modules in Visual Studio Code en kies **IoT Edge-Module toevoegen**.

1. Kies **Python-module**.

1. Naam van de module 'avroFileWriter'.

1. Wanneer u wordt gevraagd naar uw opslagplaats voor Docker-installatiekopieën, moet u de dezelfde register gebruiken als u hebt gebruikt bij het toevoegen van de Router-module.

1. Kopieer bestanden van de voorbeeldmodule in de oplossing.

   ```cmd
   copy C:\source\IoTEdgeAndMlSample\EdgeModules\modules\avroFileWriter\*.py C:\source\IoTEdgeAndMlSample\EdgeSolution\modules\avroFileWriter\
   ```

1. Als u hierom wordt gevraagd om te overschrijven main.py, typt u `y` en druk vervolgens op `Enter`.

1. U ziet dat filemanager.py en schema.py zijn toegevoegd aan de oplossing en main.py is bijgewerkt.

> [!NOTE]
> Bij het openen van een Python-bestand wordt u mogelijk gevraagd pylint installeren. U hoeft niet te installeren de linter om deze zelfstudie te voltooien.

### <a name="bind-mount-for-data-files"></a>Koppelpunt voor gegevensbestanden binden

Zoals vermeld in de inleiding, is de module schrijver afhankelijk van de aanwezigheid van bind koppelpunt Avro-bestanden schrijven naar het bestandssysteem van het apparaat.

#### <a name="add-directory-to-device"></a>Map toevoegen aan apparaat

1. Verbinding maken met uw IoT Edge-apparaat VM met behulp van SSH.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. Maken van de map waarin de opgeslagen leaf apparaat-berichten.

   ```bash
   sudo mkdir -p /data/avrofiles
   ```

3. Bijwerken van directory-machtigingen voor deze beschrijfbaar maken door de container.

   ```bash
   sudo chmod ugo+rw /data/avrofiles
   ```

4. Valideren van de map heeft nu de machtiging schrijven (s) voor gebruikers, groepen en eigenaar.

   ```bash
   ls -la /data
   ```

   ![Directory-machtigingen voor avrofiles](media/tutorial-machine-learning-edge-06-custom-modules/avrofiles-directory-permissions.png)

#### <a name="add-directory-to-the-module"></a>Map toevoegen aan de module

Als de map wilt van de module container toevoegen, wijzigen we de docker-bestanden die zijn gekoppeld aan de module avroFileWriter. Er zijn drie docker-bestanden die zijn gekoppeld aan de module: Dockerfile.AMD64, Dockerfile.amd64.debug en Dockerfile.arm32v7. Deze bestanden moeten synchroon worden gehouden als we willen fouten opspoort of op een apparaat arm32 implementeren. In dit artikel richten op Dockerfile.amd64.

1. Open op uw ontwikkelcomputer, de **Dockerfile.amd64** bestand.

2. Het bestand te wijzigen zodat deze er ongeveer zo uitziet als in het volgende voorbeeld:

   ```dockerfile
   FROM ubuntu:xenial  

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

   De `mkdir` en `chown` opdrachten de opdracht geven de Docker-build-proces voor het maken van een op het hoogste niveau directory /avrofiles genoemd in de afbeelding en vervolgens om de moduleuser de eigenaar van die map. Het is belangrijk dat deze opdrachten worden geplaatst nadat de Modulegebruiker wordt toegevoegd aan de installatiekopie met de `useradd` opdracht en voordat u de context-switches op de moduleuser (gebruiker moduleuser).

3. De bijbehorende wijzigingen aanbrengen in Dockerfile.amd64.debug en Dockerfile.arm32v7.

#### <a name="update-the-module-configuration"></a>De moduleconfiguratie van de bijwerken

De laatste stap van het maken van de binding is de deployment.template.json (en deployment.debug.template.json) om bestanden te werken met de binding-informatie.

1. Open deployment.template.json.

2. De definitie van de module voor avroFileWriter wijzigen door toe te voegen de `Binds` parameter die de container directory /avrofiles naar de lokale map op het edge-apparaat verwijst. De definitie van de module moet overeenkomen met het volgende voorbeeld:

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles"
           ]
         }
       }
     }
   }
   ```

3. De bijbehorende wijzigingen aanbrengen in deployment.debug.template.json.

### <a name="bind-mount-for-access-to-configyaml"></a>Koppeling voor toegang tot binden aan config.yaml

Er moet één meer binding voor de schrijver-module toevoegen. Deze binding biedt de module-toegang tot het lezen van de verbindingsreeks van het bestand /etc/iotedge/config.yaml op het IoT Edge-apparaat. Moeten we de verbindingsreeks voor het maken van een IoTHubClient zodat we het uploaden kan aanroepen\_blob\_asynchrone methode voor het uploaden van bestanden naar de IoT hub. De stappen voor het toevoegen van deze binding zijn vergelijkbaar met die in de vorige sectie.

#### <a name="update-directory-permission"></a>Directory-machtigingen bijwerken

1. Verbinding maken met uw IoT Edge-apparaat met behulp van SSH.

   ```bash
   ssh -l <user>@IoTEdge-<extension>.<region>.cloudapp.azure.com
   ```

2. Leesmachtiging naar het bestand config.yaml toevoegen.

   ```bash
   sudo chmod +r /etc/iotedge/config.yaml
   ```

3. Valideer de machtigingen juist zijn ingesteld.

   ```bash
   ls -la /etc/iotedge/
   ```

4. Zorg ervoor dat de machtigingen voor config.yaml **- r--r--r--** .

#### <a name="add-directory-to-module"></a>Map toevoegen aan de module

1. Open op uw ontwikkelcomputer, de **Dockerfile.amd64** bestand.

2. Voeg een extra set `mkdir` en `chown` opdrachten naar het bestand dus dat lijkt op:

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && apt-get install -y --no-install-recommends libcurl4-openssl-dev
   python3-pip libboost-python1.58-dev libpython3-dev && rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig

   USER moduleuser

   CMD "python3", "-u", "./main.py"]
   ```

3. De bijbehorende wijzigingen aanbrengen in Dockerfile.amd64.debug en Dockerfile.arm32v7.

#### <a name="update-the-module-configuration"></a>De moduleconfiguratie van de bijwerken

1. Open de **deployment.template.json** bestand.

2. De definitie van de module voor avroFileWriter wijzigen door het toevoegen van een tweede regel naar de `Binds` parameter die de containermap (/ app/iotconfig) naar de lokale map op het apparaat (/ etc/iotedge verwijst).

   ```json
   "avroFileWriter": {
     "version": "1.0",
     "type": "docker",
     "status": "running",
     "restartPolicy": "always",
     "settings": {
       "image": "${MODULES.avroFileWriter}",
       "createOptions": {
         "HostConfig": {
           "Binds": [
             "/data/avrofiles:/avrofiles",
             "/etc/iotedge:/app/iotconfig"
           ]
         }
       }
     }
   }
   ```

3. De bijbehorende wijzigingen aanbrengen in deployment.debug.template.json.

## <a name="install-dependencies"></a>Afhankelijkheden installeren

De module writer wordt afhankelijk van twee Python-bibliotheken, fastavro en PyYAML. Moeten we de afhankelijkheden te installeren op de ontwikkelcomputer en het Docker-bouwproces ze te installeren in de afbeelding van de module de opdracht geven.

### <a name="pyyaml"></a>PyYAML

1. Open op uw ontwikkelcomputer, de **requirements.txt** bestand en voeg pyyaml toe.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   ```

2. Open de **Dockerfile.amd64** -bestand en voeg een `pip install` opdracht setuptools bijwerken.

   ```dockerfile
   FROM ubuntu:xenial

   WORKDIR /app

   RUN apt-get update && \
       apt-get install -y --no-install-recommends libcurl4-openssl-dev python3-pip libboost-python1.58-dev libpython3-dev && \
       rm -rf /var/lib/apt/lists/\*

   RUN pip3 install --upgrade pip
   RUN pip install -U pip setuptools
   COPY requirements.txt ./
   RUN pip install -r requirements.txt

   COPY . .

   RUN useradd -ms /bin/bash moduleuser
   RUN mkdir /avrofiles && chown moduleuser /avrofiles
   RUN mkdir -p /app/iotconfig && chown moduleuser /app/iotconfig
   USER moduleuser

   CMD [ "python3", "-u", "./main.py" ]
   ```

3. De bijbehorende wijzigingen aanbrengen in Dockerfile.amd64.debug. <!--may not be necessary. Add 'if needed'?-->

4. Pyyaml lokaal installeren op een terminal openen in Visual Studio Code en typen

   ```cmd
   pip install pyyaml
   ```

### <a name="fastavro"></a>Fastavro

1. Voeg fastavro na pyyaml in requirements.txt.

   ```txt
   azure-iothub-device-client~=1.4.3
   pyyaml
   fastavro
   ```

2. Installeer fastavro naar uw ontwikkelmachine met behulp van de Visual Studio Code-terminal.

   ```cmd
   pip install fastavro
   ```

## <a name="reconfigure-iot-hub"></a>IoT Hub configureren

Door de introductie van de IoT Edge-apparaat en -modules op het systeem, hebben we onze verwachtingen over welke gegevens worden verzonden naar de hub en voor welk doel gewijzigd. Er moet opnieuw configureren de routering in de hub te maken met onze nieuwe realiteit.

> [!NOTE]
> We configureren de hub voordat u implementeert modules omdat sommige van de instellingen van de hub, specifiek bestand uploadt, moet worden correct ingesteld voor de module avroFileWriter correct uit te voeren

### <a name="set-up-route-for-rul-messages-in-iot-hub"></a>Instellen van de route voor berichten van de resterende Levensduur in IoT Hub

We verwachten regelmatig berichten met alleen de apparaat-ID en de resterende Levensduur voorspelling voor het apparaat ontvangen met de router en de classificatie in plaats. We willen de gegevens van de resterende Levensduur doorsturen naar een eigen opslaglocatie waar we kunnen de status van de apparaten bewaken, rapporten en waarschuwingen worden geactiveerd behoefte. Op hetzelfde moment willen we apparaatgegevens die nog steeds rechtstreeks door een leaf-apparaten die nog niet is gekoppeld aan onze IoT Edge-apparaat om door te gaan om te routeren naar de huidige opslaglocatie worden verzonden.

#### <a name="create-a-rul-message-route"></a>Een bericht Verschijnt route maken

1. Navigeer naar uw IoT-Hub in de Azure-portal.

2. Kies in het linkernavigatievenster **berichtroutering**.

3. Selecteer **Toevoegen**.

4. Naam van de route **RulMessageRoute**.

5. Selecteer **toevoegen** naast de **eindpunt** selector en kies **Blob storage**.

6. In de **toevoegen van een opslageindpunt** vormen en de naam van het eindpunt **ruldata**.

7. Selecteer **een container kiezen**.

8. Kies het opslagaccount die wordt gebruikt in deze zelfstudie, met de naam, zoals **iotedgeandml\<uniek achtervoegsel\>** .

9. Kies de **ruldata** container en klik op **Selecteer**.

10. Klik op **maken** om de storage-eindpunt te maken.

11. Voor de **Routing query**, voer de volgende query:

    ```sql
    IS_DEFINED($body.PredictedRul) AND NOT IS_DEFINED($body.OperationalSetting1)
    ```

12. Vouw de **Test** sectie en vervolgens de **berichttekst** sectie. Vervang het bericht met dit voorbeeld van onze verwachte berichten:

    ```json
    {
      "ConnectionDeviceId": "aaLeafDevice_1",
      "CorrelationId": "b27e97bb-06c5-4553-a064-e9ad59c0fdd3",
      "PredictedRul": 132.62721409309165,
      "CycleTime": 64.0
    }
    ```

13. Selecteer **route testen**. Als de test geslaagd is, ziet u "het bericht komt overeen met de query."

14. Klik op **Opslaan**.

#### <a name="update-turbofandevicetostorage-route"></a>Bijwerken van de route turbofanDeviceToStorage

We willen niet naar de nieuwe voorspellingsgegevens route naar de opslaglocatie van onze oude, zodat de route te voorkomen dat het bijwerken.

1. In de IoT-Hub **berichtroutering** weergeeft, schakelt de **Routes** tabblad.

2. Selecteer **turbofanDeviceDataToStorage**, of de servernaam die u heeft gegeven tot uw initiële gegevens route.

3. De routering query voor het bijwerken

   ```sql
   IS_DEFINED($body.OperationalSetting1)
   ```

4. Vouw de **Test** sectie en vervolgens de **berichttekst** sectie. Vervang het bericht met dit voorbeeld van onze verwachte berichten:

   ```json
   {
     "Sensor13": 2387.96,
     "OperationalSetting1": -0.0008,
     "Sensor6": 21.61,
     "Sensor11": 47.2,
     "Sensor9": 9061.45,
     "Sensor4": 1397.86,
     "Sensor14": 8140.39,
     "Sensor18": 2388.0,
     "Sensor12": 522.87,
     "Sensor2": 642.42,
     "Sensor17": 391.0,
     "OperationalSetting3": 100.0,
     "Sensor1": 518.67,
     "OperationalSetting2": 0.0002,
     "Sensor20": 39.03,
     "DeviceId": 19.0,
     "Sensor5": 14.62,
     "PredictedRul": 212.00132402791962,
     "Sensor8": 2388.01,
     "Sensor16": 0.03,
     "CycleTime": 42.0,
     "Sensor21": 23.3188,
     "Sensor15": 8.3773,
     "Sensor3": 1580.09,
     "Sensor10": 1.3,
     "Sensor7": 554.57,
     "Sensor19": 100.0
   }
   ```

5. Selecteer **route testen**. Als de test geslaagd is, ziet u "het bericht komt overeen met de query."

6. Selecteer **Opslaan**.

### <a name="configure-file-upload"></a>Uploaden van bestanden configureren

De functie IoT Hub-bestand uploaden om in te schakelen van de module voor de schrijver van bestanden bestanden uploaden naar opslag configureren.

1. Kies in de linker navigator in uw IoT-Hub, **uploaden van het bestand**.

2. Selecteer **Azure-Opslagcontainer**.

3. Selecteer uw storage-account in de lijst.

4. Selecteer de **uploadturbofanfiles** container en klik op **Selecteer**.

5. Selecteer **Opslaan**. De portal waarschuwt u als het opslaan voltooid is.

> [!Note]
> We zijn niet inschakelen op de melding uploaden voor deze zelfstudie, maar zien [ontvangen van een bestand uploaden melding](../iot-hub/iot-hub-java-java-file-upload.md#receive-a-file-upload-notification) voor meer informatie over het verwerken van bestand melding uploaden.

## <a name="build-publish-and-deploy-modules"></a>Maken, publiceren en implementeren van modules

Nu dat we de wijzigingen hebt aangebracht, zijn we staan klaar om de installatiekopieën van het bouwen en deze publiceren naar onze Azure-containerregister. Het buildproces wordt het deployment.template.json-bestand gebruikt om te bepalen welke modules moeten worden gemaakt. De instellingen voor elke module, met inbegrip van versie, vindt u in het bestand module.json in de modulemap. Het bouwproces eerst wordt uitgevoerd een Docker-build op de docker-bestanden die overeenkomen met de huidige configuratie vindt u in het bestand module.json om een installatiekopie te maken. Het publiceert vervolgens de installatiekopie naar het register uit het bestand module.json waarbij een versietag die overeenkomt met het certificaat in het bestand module.json. Ten slotte is het resultaat een configuratie-specifieke implementatie-manifest (bijvoorbeeld deployment.amd64.json), die we op het IoT Edge-apparaat gaat implementeren. De IoT Edge-apparaat leest de gegevens van de implementatie manifest en op basis van de instructies wordt gedownload van de modules, configureren van de routes en gewenste eigenschappen niet instellen. Deze implementatiemethode heeft twee neveneffecten die u moet rekening houden met:

* **Implementatie lag:** omdat de IoT Edge-runtime de wijziging van de gewenste eigenschappen herkennen moet voordat deze wordt gestart om opnieuw te configureren, duurt het een zekere mate van tijd nadat u uw modules hebt geïmplementeerd, totdat de runtime worden ze opgepikt en begint met het bijwerken van de IoT Edge het apparaat.

* **Module versies zaak:** als u een nieuwe versie van de container van de module naar het containerregister met behulp van de afsluitcodes met dezelfde versie als de vorige module publiceert, de runtime de nieuwe versie van de module niet downloaden. Hiervoor wordt een vergelijking van de tag versie van de lokale installatiekopie en de gewenste installatiekopie van het manifest van de implementatie. Als deze versies overeenkomen, wordt de uitvoering geen actie ondernomen. Het is daarom belangrijk om te verhogen van de versie van uw module elke keer dat u wilt implementeren van nieuwe wijzigingen. De versie verhogen door het veranderen van de **versie** eigenschap onder het **tag** eigenschap in het bestand module.json voor de module die u wilt wijzigen. Vervolgens ontwikkelt en publiceren van de module.

    ```json
    {
      "$schema-version": "0.0.1",
      "description": "",
      "image": {
        "repository": "<your registry>.azurecr.io/avrofilewriter",
        "tag": {
          "version": "0.0.1",
          "platforms": {
            "amd64": "./Dockerfile.amd64",
            "amd64.debug": "./Dockerfile.amd64.debug",
            "arm32v7": "./Dockerfile.arm32v7"
          }
        },
        "buildOptions": []
      },
      "language": "python"
    }
    ```

### <a name="build-and-publish"></a>Bouwen en publiceren

1. Open in Visual Studio-Code op de ontwikkeling van uw virtuele machine, een terminalvenster van Visual Studio Code en meld u aan bij uw containerregister.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

1. Met de rechtermuisknop op deployment.template.json in Visual Studio Code, en kies **Build and Push IoT Edge-oplossing**.

### <a name="view-modules-in-the-registry"></a>Modules weergeven in het register

Zodra de build is voltooid, zijn wij kunnen de Azure-portal gebruiken om te controleren van onze gepubliceerde modules.

1. In de Azure-portal, gaat u naar de werkruimte van uw Azure Machine Learning-service en klik op de hyperlink voor **register**.

    ![Navigeer naar het register van machine learning-werkruimte-service](media/tutorial-machine-learning-edge-06-custom-modules/follow-registry-link.png)

2. Selecteer in de navigator register naast **opslagplaatsen**.

3. Houd er rekening mee dat beide modules u hebt gemaakt, **avrofilewriter** en **turbofanrouter**, worden weergegeven als opslagplaatsen.

4. Selecteer **turbofanrouter** en houd er rekening mee dat u één afbeelding gemarkeerd als 0.0.1-amd64 hebt gepubliceerd.

   ![Eerste label versie weergeven van turbofanrouter](media/tutorial-machine-learning-edge-06-custom-modules/tagged-image-turbofanrouter-repo.png)

### <a name="deploy-modules-to-iot-edge-device"></a>Modules op IoT Edge-apparaat implementeren

We hebben gebouwd en geconfigureerd van de modules in onze oplossing, nu we de modules op het IoT Edge-apparaat gaat implementeren.

1. In Visual Studio Code, klik met de rechtermuisknop op de **deployment.amd64.json** bestand in de config-map.

2. Kies **implementatie voor één apparaat maken**.

3. Uw IoT Edge-apparaat, kies **aaTurboFanEdgeDevice**.

4. Vernieuwen van het deelvenster van de Azure IoT Hub-apparaten in Visual Studio Code explorer. U ziet dat de drie nieuwe modules worden geïmplementeerd, maar nog niet is uitgevoerd.

5. Vernieuw opnieuw na een paar minuten en ziet u de modules die zijn uitgevoerd.

   ![Actieve modules in Visual Studio Code weergeven](media/tutorial-machine-learning-edge-06-custom-modules/view-running-modules-list.png)

> [!NOTE]
> Het kan enkele minuten voor de modules te starten en de juiste een constante status running doorbrengt duren. Gedurende die tijd ziet u modules starten en stoppen als ze proberen om een verbinding met de IoT Edge hub-module te maken.

## <a name="diagnosing-failures"></a>Diagnose van fouten

In deze sectie delen we een aantal technieken om te begrijpen wat is een fout opgetreden in een module of modules. Een storing kan vaak eerst waargenomen van de status in de Visual Studio-Code.

### <a name="identify-failed-modules"></a>Mislukte modules identificeren

* **Visual Studio Code:** Kijken naar het deelvenster Azure IoT Hub-apparaten. Als de meeste modules in een status running doorbrengt zijn, maar een is gestopt, moet u die gestopt module verder onderzoeken. Als alle modules in een gestopte staat voor een lange periode, kan dit wijzen op mislukte ook.

* **Azure-portal:** Door te navigeren naar uw IoT-hub in de portal en vervolgens zoeken naar de pagina met details van het apparaat (in IoT Edge, Inzoomen op het apparaat) u wellicht dat een module heeft een fout gerapporteerd of heeft niets nooit gerapporteerd aan de IoT-hub.

### <a name="diagnosing-from-the-device"></a>Diagnose van het apparaat

Door aan te melden bij de IoT Edge-apparaat krijgt u toegang tot een groot deel van de informatie over de status van uw modules. Het belangrijkste mechanisme gebruiken we de Docker-opdrachten die wij controleren de containers en de afbeeldingen op het apparaat zijn.

1. Lijst met alle actieve containers. We verwachten dat om te zien van een container voor elke module met een naam die overeenkomt met de module. Met deze opdracht worden ook de exacte afbeelding voor de container met inbegrip van versie, zodat u aan uw verwachting koppelen kunt. U kunt ook afbeeldingen weergeven door te vervangen door 'afbeelding' voor 'container' in de opdracht.

   ```bash
   sudo docker container ls
   ```

2. De logboeken voor een container ophalen. Met deze opdracht voert wat is geschreven naar het StdErr en StdOut in de container. Met deze opdracht werkt voor containers die zijn gestart en vervolgens om een bepaalde reden is verbroken. Het is ook handig om te begrijpen wat met de containers edgeAgent of edgeHub heeft is gebeurt.

   ```bash
   sudo docker container logs <container name>
   ```

3. Een container inspecteren. Met deze opdracht geeft je verschrikkelijk veel informatie over de installatiekopie. De gegevens kunnen worden gefilterd, afhankelijk van wat u zoekt. Als u zien wilt als de bindingen op de avroFileWriter juist zijn kunt u bijvoorbeeld de opdracht gebruiken:

   ```bash
   sudo docker container inspect -f "{{ json .Mounts }}" avroFileWriter | python -m json.tool
   ```

4. Verbinding maken met een container die wordt uitgevoerd. Met deze opdracht is handig als u controleren van de container wilt, terwijl deze wordt uitgevoerd:

   ```bash
   sudo docker exec -it avroFileWriter bash
   ```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt gemaakt we een IoT Edge-oplossing in Visual Studio Code met drie modules, een classificatie van een router en een bestand van schrijver/uploader. We de routes instellen waarmee de modules om te communiceren met elkaar op het edge-apparaat, de configuratie van het edge-apparaat is gewijzigd en de docker-bestanden voor het installeren van afhankelijkheden en afhankelijk van de koppeling toevoegen aan de modules containers bijgewerkt. Vervolgens wordt de configuratie van de IoT-Hub voor het routeren van onze berichten op basis van het type en voor het afhandelen van het uploaden van bestanden bijgewerkt. Alles wat erin, we de modules geïmplementeerd op het IoT Edge-apparaat en ervoor gezorgd dat de modules correct zijn uitgevoerd.

Meer informatie vindt u op de volgende pagina's:

* [Meer informatie over het implementeren van modules en routes instellen in IoT Edge](module-composition.md)
* [IoT Hub-berichtroutering query-syntaxis](../iot-hub/iot-hub-devguide-routing-query-syntax.md)
* [IoT Hub-berichtroutering: nu met routering in de hoofdtekst van bericht](https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/)
* [Bestanden uploaden met IoT Hub](../iot-hub/iot-hub-devguide-file-upload.md)
* [Uploaden van bestanden van uw apparaat naar de cloud met IoT Hub](../iot-hub/iot-hub-python-python-file-upload.md)

Doorgaan met het volgende artikel om te beginnen met het verzenden van gegevens en uw oplossing in actie zien.

> [!div class="nextstepaction"]
> [Verzenden van gegevens via een transparante gateway](tutorial-machine-learning-edge-07-send-data-to-hub.md)
