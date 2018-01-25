---
title: De simulatie apparaat in de oplossing voor externe controle - Azure | Microsoft Docs
description: Deze zelfstudie laat zien hoe de apparaatsimulator gebruiken met de vooraf geconfigureerde oplossing voor externe controle.
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/15/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 0bf1cff4058bfe46b54f3f0b6836ede3e04ed5dd
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="test-your-solution-with-simulated-devices"></a>Testen van uw oplossing met gesimuleerde apparaten

Deze zelfstudie ziet u het aanpassen van het apparaat simulator microservice in de vooraf geconfigureerde oplossing voor externe controle. Om weer te geven de mogelijkheden van de apparaatsimulator dat twee scenario's in de toepassing Contoso IoT maakt gebruik van deze zelfstudie.

In het eerste scenario wil Contoso voor het testen van een nieuw smart gloeilamp-apparaat. Als u wilt de tests uitvoert, kunt u een nieuw gesimuleerd apparaat maken met de volgende kenmerken:

*Eigenschappen*

| Naam                     | Waarden                      |
| ------------------------ | --------------------------- |
| Kleur                    | Wit, rood, blauw            |
| Helderheid               | 0 en 100                    |
| Geschatte resterende levensduur | Aftelling van 10.000 uur |

*Telemetry*

De volgende tabel ziet u de gegevens dat de gloeilamp rapporten naar de cloud als een gegevensstroom:

| Naam   | Waarden      |
| ------ | ----------- |
| Status | 'on', 'off' |
| Temperatuur | Graden F |
| online | True, false |

> [!NOTE]
> De **online** telemetriewaarde is verplicht voor alle gesimuleerde typen.

*Methoden*

De volgende tabel ziet u de acties die ondersteuning biedt voor het nieuwe apparaat:

| Naam        |
| ----------- |
| Overschakelen op   |
| Uitschakelen  |

*Initiële status*

De volgende tabel ziet u de oorspronkelijke status van het apparaat:

| Naam                     | Waarden |
| ------------------------ | -------|
| Eerste kleur            | Wit  |
| Initiële helderheid       | 75     |
| Initiële resterende levensduur   | 10.000 |
| Initiële telemetrie-status | "on"   |
| Initiële telemetrie temperatuur | 200   |

In het tweede scenario, voegt u toe een nieuw type telemetrie naar Contoso de bestaande **Koelunit** apparaat.

Deze zelfstudie laat zien hoe u de apparaatsimulator gebruiken met de vooraf geconfigureerde oplossing voor externe controle:

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * Maak een nieuwe apparaattype
> * Aangepast apparaat gedrag simuleren
> * Een nieuw apparaattype toevoegen aan het dashboard
> * Aangepaste telemetrie verzenden vanuit een bestaand apparaattype

## <a name="prerequisites"></a>Vereisten

Volg deze zelfstudie, hebt u het volgende nodig:

* Een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement. Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren van de vooraf geconfigureerde oplossing voor externe controle](iot-suite-remote-monitoring-deploy.md) zelfstudie.

* Visual Studio 2017. Als u Visual Studio 2017 geïnstalleerd hebt, kunt u downloaden de gratis [Visual Studio Community](https://www.visualstudio.com/free-developer-offers/) edition.

* [Cloud Explorer voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVS15Preview) Visual Studio-extensie.

* Een account op [Docker Hub](https://hub.docker.com/). U kunt zich aanmelden gratis aan de slag.

* [GIT](https://git-scm.com/downloads) op uw computer geïnstalleerd.

## <a name="prepare-your-development-environment"></a>Uw ontwikkelomgeving voorbereiden

De volgende taken uitvoeren om uw ontwikkelingsomgeving voor een nieuw gesimuleerd apparaat toe te voegen aan uw oplossing voor externe controle voorbereiden:

### <a name="configure-ssh-access-to-the-solution-virtual-machine-in-azure"></a>SSH-toegang tot de oplossing voor virtuele machine in Azure configureren

Tijdens het maken van uw oplossing voor externe controle op [www.azureiotsuite.com](https://www.azureiotsuite.com), u de naam van een oplossing hebt gekozen. Naam van de oplossing wordt de naam van de Azure-resourcegroep met de verschillende geïmplementeerde resources die gebruikmaakt van de oplossing. De volgende opdrachten gebruiken een resourcegroep met de naam **Contoso-01**, vervangt u **Contoso-01** met de naam van de resourcegroep.

De volgende opdrachten gebruiken de `az` opdracht [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest). U kunt Azure CLI 2.0 installeren op uw ontwikkelcomputer of gebruik de [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) in de [Azure-portal](http://portal.azure.com). De Azure CLI 2.0 is vooraf geïnstalleerd in de Cloud-Shell.

1. Om te controleren of de naam van de resourcegroep die uw externe controle resources bevat, voer de volgende opdracht:

    ```sh
    az group list | grep "name"
    ```

    Met deze opdracht worden alle resourcegroepen in uw abonnement. De lijst moet een resourcegroep met dezelfde naam als uw oplossing voor externe controle bevatten.

1. Als u uw resource groep de standaardgroep voor de volgende opdrachten, voer de volgende opdracht die met de naam van de resourcegroep in plaats van **Contoso-01**:

    ```sh
    az configure --defaults group=Contoso-01
    ```

1. Als de resources in de resourcegroep wilt weergeven, moet u de volgende opdracht uitvoeren:

    ```sh
    az resource list -o table
    ```

    Noteer de namen van de virtuele machine en de netwerkbeveiligingsgroep. U gebruikt deze waarden in latere stappen.

1. Als u SSH toegang tot uw virtuele machine, voert u de volgende opdracht met de naam van de beveiligingsgroep van uw netwerk uit de vorige stap:

    ```sh
    az network nsg rule create --name SSH --nsg-name your-network-security-group --priority 101 --destination-port-ranges 22 --access Allow --protocol TCP
    ```

    Als u wilt weergeven in de lijst met regels voor binnenkomende verbindingen voor uw netwerk, voer de volgende opdracht:

    ```sh
    az network nsg rule list --nsg-name Contoso-01-nsg -o table
    ```

1. De virtuele machine om wachtwoord te wijzigen in een wachtwoord dat u weet, kunt u de volgende opdracht uitvoeren. Gebruik de naam van de virtuele machine die u eerder hebt genoteerd en een wachtwoord van uw keuze:

    ```sh
    az vm user update --name your-vm-name --username azureuser --password your-password
    ```
1. Het IP-adres van uw virtuele machine vinden, gebruikt u de volgende opdracht en noteer het openbare IP-adres:

    ```sh
    az vm list-ip-addresses --name your-vm-name
    ```

1. U kunt nu SSH verbinding maken met uw virtuele machine. De `ssh` opdracht vooraf is geïnstalleerd in de Cloud-Shell. Het openbare IP-adres van de vorige stap en klik desgevraagd het wachtwoord dat u hebt geconfigureerd voor de virtuele machine:

    ```sh
    ssh azureuser@public-ip-address
    ```

    U hebt nu toegang tot de shell in de virtuele machine die de Docker-containers in de oplossing voor externe controle wordt uitgevoerd. Als u wilt weergeven in de actieve containers, gebruik de volgende opdracht:

    ```sh
    docker ps
    ```

### <a name="find-the-service-connection-strings"></a>De verbindingsreeksen service vinden

In de zelfstudie werkt u met Visual Studio-oplossing die verbinding met de oplossing Cosmos DB en IoT Hub-services maakt. De volgende stappen ziet u een manier om de verbinding u moet de waarden van verbindingsreeksen vinden:

1. Als u wilt zoeken in de verbindingsreeks van de Cosmos-DB, voer de volgende opdracht in de SSH-sessie die is verbonden met de virtuele machine:

    ```sh
    sudo grep STORAGEADAPTER_DOCUMENTDB /app/env-vars
    ```

    Noteer de verbindingsreeks. U gebruikt deze waarde verderop in de zelfstudie.

1. Als u wilt zoeken in de verbindingsreeks IoT Hub, voer de volgende opdracht in de SSH-sessie die is verbonden met de virtuele machine:

    ```sh
    sudo grep IOTHUB_CONNSTRING /app/env-vars
    ```

    Noteer de verbindingsreeks. U gebruikt deze waarde verderop in de zelfstudie.

> [!NOTE]
> U kunt deze verbindingsreeksen ook vinden in de Azure portal of met behulp van de `az` opdracht.

### <a name="stop-the-device-simulation-service-in-the-virtual-machine"></a>Stop de service van de simulatie apparaat in de virtuele machine

Wanneer u de simulatie device service wijzigt, kunt u lokaal te testen van uw wijzigingen kunt uitvoeren. Voordat u de simulatie device service lokaal uitvoeren, moet u het exemplaar dat wordt uitgevoerd in de virtuele machine als volgt stoppen:

1. Vinden de **CONTAINER-ID** van de **apparaat simulatie** service, voer de volgende opdracht in de SSH-sessie die is verbonden met de virtuele machine:

    ```sh
    docker ps
    ```

    Noteer de container-ID van de **apparaat simulatie** service.

1. Om te stoppen de **apparaat simulatie** container, voer de volgende opdracht:

    ```sh
    docker stop container-id-from-previous-step
    ```

### <a name="clone-the-github-repositories"></a>Kloon de GitHub-opslagplaatsen

In deze zelfstudie werkt u met de **apparaat simulatie** en **opslagadapter** Visual Studio-projecten. U kunt de broncodeopslagplaatsen vanuit GitHub klonen. Deze stap uitvoeren op uw lokale ontwikkelcomputer waarop u Visual Studio geïnstalleerd is:

1. Open een opdrachtprompt en navigeer naar de map waar u uw exemplaar van de **apparaat simulatie** en **opslagadapter** GitHub-opslagplaatsen.

1. Voor het klonen van de .NET-versie van de **apparaat simulatie** -opslagplaats, voer de volgende opdracht:

    ```cmd
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

    De simulatie-service van het apparaat in de oplossing voor externe controle kunt u wijzigingen aanbrengen in de typen ingebouwde gesimuleerde apparaten en voor het maken van nieuwe gesimuleerde apparaattypen. Aangepaste apparaattypen kunt u het gedrag van de oplossing voor externe controle te testen voordat u uw fysieke apparaten die verbinding maken.

1. Voor het klonen van de .NET-versie van de **opslagadapter** -opslagplaats, voer de volgende opdracht:

    ```cmd
    git clone https://github.com/Azure/storage-adapter.git
    ```

    De service van de simulatie apparaat gebruikt de opslagservice adapter verbinding maken met de service Cosmos-database in Azure. De oplossing voor externe controle slaat de configuratiegegevens van het gesimuleerde apparaat in een Cosmos-DB-database.

### <a name="run-the-storage-adapter-service-locally"></a>De opslagservice adapter lokaal uitvoeren

De service van de simulatie apparaat gebruikt de opslagservice adapter verbinding maken met de oplossing Cosmos-DB-database. Als u de simulatie device service lokaal uitvoert, moet u de adapter opslagservice ook lokaal uitvoeren. De volgende stappen ziet u het uitvoeren van de adapter opslagservice vanuit Visual Studio:

1. Open in Visual Studio de **pc's-opslag-adapter.sln** oplossingsbestand in uw lokale kloon van de **opslagadapter** opslagplaats.

1. Klik in Solution Explorer met de rechtermuisknop op de **WebService** project, kies **eigenschappen**, en kies vervolgens **Debug**.

1. In de **omgevingsvariabelen** sectie, het bewerken van de waarde van de **pc's\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** variabele moet de Cosmos-DB-verbinding tekenreeks die u eerder hebt genoteerd. Sla uw wijzigingen.

1. Klik in Solution Explorer met de rechtermuisknop op de **WebService** project, kies **Debug**, en kies vervolgens **nieuw exemplaar gestart**.

1. De service wordt gestart die lokaal wordt uitgevoerd en wordt geopend `http://localhost:9022/v1/status` in de browser. Controleer de **Status** waarde is ' OK: actief is en goed. "

1. Laat de opslagservice adapter lokaal wordt uitgevoerd totdat u de zelfstudie hebt voltooid.

U hebt nu alles in plaats en u bent klaar om te beginnen een nieuw gesimuleerd apparaattype toe te voegen aan uw oplossing voor externe controle.

## <a name="create-a-simulated-device-type"></a>Een type gesimuleerd apparaat maken

De eenvoudigste manier om een nieuwe apparaattype maken in de simulatie-service van het apparaat is kopiëren en wijzigen van een bestaand type. De volgende stappen ziet u hoe u de ingebouwde kopieert **Koelunit** apparaat Maak een nieuwe **gloeilamp** apparaat:

1. Open in Visual Studio de **apparaat simulation.sln** oplossingsbestand in uw lokale kloon van de **apparaat simulatie** opslagplaats.

1. Klik in Solution Explorer met de rechtermuisknop op de **SimulationAgent** project, kies **eigenschappen**, en kies vervolgens **Debug**.

1. In de **omgevingsvariabelen** sectie, het bewerken van de waarde van de **pc's\_IOTHUB\_CONNSTRING** variabele moet de verbindingsreeks van de IoT-Hub u eerder hebt genoteerd. Sla uw wijzigingen.

1. Klik in Solution Explorer met de rechtermuisknop op de **apparaat simulatie** oplossing en kies **Opstartprojecten instellen**. Kies **één opstartproject** en selecteer **SimulationAgent**. Klik vervolgens op **OK**.

1. Elk apparaattype heeft een JSON-modelbestand en de bijbehorende scripts in de **gegevens/Services/devicemodels** map. Kopieer in Solution Explorer de **Koelunit** bestanden maken de **gloeilamp** bestanden zoals weergegeven in de volgende tabel:

    | Bron                      | Doel                   |
    | --------------------------- | ----------------------------- |
    | chiller-01.json             | lightbulb-01.json             |
    | scripts/Koelunit-01-state.js | scripts/gloeilamp-01-state.js |
    | opnieuw opstarten-scripts-method.js    | SwitchOn-scripts-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>De eigenschappen van het nieuwe apparaattype definiëren

De **gloeilamp 01.json** -bestand definieert de kenmerken van het type, zoals de telemetrie wordt gegenereerd en de methoden ondersteunt. De volgende stappen uit update de **gloeilamp 01.json** bestand voor het definiëren van de **gloeilamp** apparaat:

1. In de **gloeilamp 01.json** bestand, het bijwerken van de metagegevens van apparaten zoals weergegeven in het volgende fragment:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. In de **gloeilamp 01.json** bestand, de definitie van de simulatie bijwerken zoals weergegeven in het volgende fragment:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "temperature": 200.0,
        "temperature_unit": "F",
        "status": "on"
      },
      "Script": {
        "Type": "javascript",
        "Path": "lightbulb-01-state.js",
        "Interval": "00:00:20"
      }
    },
    ```

1. In de **gloeilamp 01.json** bestand, de eigenschappen van het apparaat bijwerken zoals weergegeven in het volgende fragment:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. In de **gloeilamp 01.json** bestand, het apparaat type telemetrie-definities bijwerken zoals weergegeven in het volgende fragment:

    ```json
    "Telemetry": [
      {
        "Interval": "00:00:20",
        "MessageTemplate": "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"status\":\"${status}\"}",
        "MessageSchema": {
          "Name": "lightbulb-status;v1",
          "Format": "JSON",
          "Fields": {
            "temperature": "double",
            "temperature_unit": "text",
            "status": "text"
          }
        }
      }
    ],
    ```

1. In de **gloeilamp 01.json** bestand, het bijwerken van de methoden voor het apparaat, zoals wordt weergegeven in het volgende fragment:

    ```json
    "CloudToDeviceMethods": {
      "SwitchOn": {
        "Type": "javascript",
        "Path": "SwitchOn-method.js"
      },
      "SwitchOff": {
        "Type": "javascript",
        "Path": "SwitchOff-method.js"
      }
    }
    ```

1. Sla de **gloeilamp 01.json** bestand.

### <a name="simulate-custom-device-behavior"></a>Aangepast apparaat gedrag simuleren

De **scripts/gloeilamp-01-state.js** -bestand definieert het gedrag van de simulatie van de **gloeilamp** type. De volgende stappen uit update de **scripts/gloeilamp-01-state.js** bestand voor het definiëren van het gedrag van de **gloeilamp** apparaat:

1. Bewerk de definitie van de status in de **scripts/gloeilamp-01-state.js** bestand, zoals wordt weergegeven in het volgende fragment:

    ```js
    // Default state
    var state = {
      online: true,
      temperature: 200.0,
      temperature_unit: "F",
      status: "on"
    };
    ```

1. Voeg een **spiegelen** werken na de **variëren** functie met de definitie van de volgende:

    ```js
    /**
    * Simple formula that sometimes flips the status of the lightbulb
    */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
      return value;
    }
    ```

1. Bewerk de **belangrijkste** functie voor het implementeren van het gedrag, zoals wordt weergegeven in het volgende fragment:

    ```js
    function main(context, previousState) {

      // Restore the global state before generating the new telemetry, so that
      // the telemetry can apply changes using the previous function state.
      restoreState(previousState);

      state.temperature = vary(200, 5, 150, 250);

      // Make this flip every so often
      state.status = flip(state.status);

      return state;
    }
    ```

1. Sla de **scripts/gloeilamp-01-state.js** bestand.

De **SwitchOn-scripts-method.js** bestand implementeert de **Switch op** methode in een **gloeilamp** apparaat. De volgende stappen uit update de **SwitchOn-scripts-method.js** bestand:

1. Bewerk de definitie van de status in de **SwitchOn-scripts-method.js** bestand, zoals wordt weergegeven in het volgende fragment:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Als u wilt overschakelen op de gloeilamp, bewerk de **belangrijkste** werken als volgt:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Sla de **SwitchOn-scripts-method.js** bestand.

1. Een kopie maken de **SwitchOn-scripts-method.js** bestand met de naam **SwitchOff-scripts-method.js**.

1. Bewerken om de gloeilamp uitschakelen, de **belangrijkste** werken in de **SwitchOff-scripts-method.js** als volgt:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Sla de **SwitchOff-scripts-method.js** bestand.

1. Klik in Solution Explorer, selecteert u elk van uw vier nieuwe bestanden op zijn beurt. In de **eigenschappen** venster voor elk bestand Controleer **naar uitvoermap kopiëren** is ingesteld op **kopiëren indien nieuwer**.

### <a name="configure-the-device-simulation-service"></a>De service van de simulatie apparaat configureren

Beperk het aantal gesimuleerde apparaten die verbinding met de oplossing tijdens het testen maken, moet u de service voor uitvoering van een enkele Koelunit en één gloeilamp apparaat configureren. De configuratiegegevens wordt opgeslagen in het exemplaar van de Cosmos-database in de resourcegroep van de oplossing. Kunt u de configuratiegegevens bewerken de **Cloud Explorer** weergave in Visual Studio:

1. Openen van de **Cloud Explorer** bekijken in Visual Studio, kiest u **weergave** en vervolgens **Cloud Explorer**.

1. In het configuratiebestand voor de simulatie vinden **zoeken naar resources** Voer **simualtions.1**.

1. Dubbelklik op de **simulations.1** document te bewerken.

1. In de waarde voor **gegevens**, zoek de **DeviceModels** matrix dat op het volgende fragment lijkt:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"chiller-02\",\"Count\":1},{\"Id\":\"elevator-01\",\"Count\":1},{\"Id\":\"elevator-02\",\"Count\":1},{\"Id\":\"engine-01\",\"Count\":1},{\"Id\":\"engine-02\",\"Count\":1},{\"Id\":\"prototype-01\",\"Count\":1},{\"Id\":\"prototype-02\",\"Count\":1},{\"Id\":\"truck-01\",\"Count\":1},{\"Id\":\"truck-02\",\"Count\":1}]
    ```

1. Als u wilt definiëren een enkele Koelunit en een gesimuleerd apparaat één gloeilamp, vervang de **DeviceModels** matrix met de volgende code:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"lightbulb-01\",\"Count\":1}]
    ```

    Sla de wijziging in de **simulations.1** document.

> [!NOTE]
> U kunt ook de Cosmos DB Data Explorer gebruiken in de Azure portal bewerken de **simulations.1** document.

### <a name="test-the-lightbulb-device-type-locally"></a>Het gloeilamp apparaattype lokaal testen

U bent nu klaar voor het testen van het type van uw nieuwe gesimuleerde gloeilamp door het apparaat simulatie project lokaal uit te voeren.

1. Klik in Solution Explorer met de rechtermuisknop op **SimulationAgent**, kies **Debug** en kies vervolgens **nieuw exemplaar gestart**.

1. Om te controleren dat de twee gesimuleerde apparaten zijn verbonden met uw IoT-Hub, de Azure portal te openen in uw browser.

1. Navigeer naar de IoT-hub in de resourcegroep die uw oplossing voor externe controle bevat.

1. In de **bewaking** sectie **metrische gegevens**. Controleer vervolgens of het aantal **verbonden apparaten** twee:

    ![Aantal verbonden apparaten](media/iot-suite-remote-monitoring-test/connecteddevices.png)

1. Navigeer in uw browser naar de **Dashboard** voor uw oplossing voor externe controle. In het deelvenster telemetrie op de **Dashboard**, selecteer **temperatuur**. De temperatuur voor uw twee gesimuleerde apparaten wordt weergegeven in de grafiek:

    ![Temperatuur telemetrie](media/iot-suite-remote-monitoring-test/telemetry.png)

U hebt nu de gloeilamp apparaat simulatie die lokaal wordt uitgevoerd. De volgende stap is uw bijgewerkte simulator om code te implementeren voor de virtuele machine die wordt uitgevoerd van de externe controle microservices in Azure.

Voordat u doorgaat, kunt u stop de foutopsporing voor de simulatie van het apparaat en de opslag adapter projecten in Visual Studio.

### <a name="deploy-the-updated-simulator-to-the-cloud"></a>De bijgewerkte simulator implementeren in de cloud

De microservices in de oplossing voor externe controle in docker-containers worden uitgevoerd. De containers worden gehost in virtuele machine van de oplossing in Azure. In deze sectie doet u het volgende:

* Maak een nieuwe apparaat simulatie docker-installatiekopie.
* Upload de installatiekopie naar uw opslagplaats docker-hub.
* De afbeelding in uw oplossing virtuele machine importeren.

De volgende stappen wordt ervan uitgegaan dat u een opslagplaats aangeroepen hebt **gloeilamp** in uw account Docker-Hub.

1. In Visual Studio in de **apparaat simulatie** project, open het bestand **solution\scripts\docker\build.cmd**.

1. Bewerk de regel die de **DOCKER_IMAGE** omgevingsvariabele naar uw opslagplaats Docker-hubnaam:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Sla de wijziging.

1. In Visual Studio in de **apparaat simulatie** project, open het bestand **solution\scripts\docker\publish.cmd**.

1. Bewerk de regel die de **DOCKER_IMAGE** omgevingsvariabele naar uw opslagplaats Docker-hubnaam:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Sla de wijziging.

1. Open een opdrachtprompt als beheerder. Navigeer naar de map **scripts\docker** in de kloon van de **apparaat simulatie** GitHub-opslagplaats.

1. De docker-installatiekopie bouwen, kunt u de volgende opdracht uitvoeren:

    ```cmd
    build.cmd
    ```

1. Als u wilt aanmelden bij uw account Docker-Hub, voer de volgende opdracht:

    ```cmd
    docker login
    ```

1. Als u wilt uw nieuwe installatiekopie uploaden naar uw Hub Docker-account, moet u de volgende opdracht uitvoeren:

    ```cmd
    publish.cmd
    ```

1. Om te controleren of het uploaden, gaat u naar [https://hub.docker.com/](https://hub.docker.com/). Zoek uw **gloeilamp** opslagplaats en kies **Details**. Kies vervolgens **labels**:

    ![Docker-hub](media/iot-suite-remote-monitoring-test/dockerhub.png)

    De scripts die zijn toegevoegd de **testen** label op de installatiekopie.

1. SSH gebruiken voor verbinding met uw oplossing virtuele machine in Azure. Navigeer naar de **App** map en bewerk de **docker compose.yaml** bestand:

    ```sh
    cd /app
    sudo nano docker-compose.yaml
    ```

1. Bewerk de vermelding voor de service van de simulatie apparaat de docker-afbeelding te gebruiken:

    ```yaml
    devicesimulation:
      image: {your docker ID}/lightbulb:testing
    ```

    Sla uw wijzigingen op.

1. Als u wilt starten op alle services met de nieuwe instellingen, voer de volgende opdracht:

    ```sh
    sudo ./start.sh
    ```

1. Als u wilt controleren in het logboekbestand van uw nieuwe apparaat simulatie container, voer de volgende opdracht te vinden van de container-ID:

    ```sh
    docker ps
    ```

    Voer vervolgens de volgende opdracht met behulp van de container-ID:

    ```sh
    docker logs {container ID}
    ```

U hebt nu de stappen voor het implementeren van een bijgewerkte versie van de simulatie device service voor uw oplossing voor externe controle voltooid.

Navigeer in uw browser naar de **Dashboard** voor uw oplossing voor externe controle. In het deelvenster telemetrie op de **Dashboard**, selecteer **temperatuur**. De temperatuur voor uw twee gesimuleerde apparaten wordt weergegeven in de grafiek:

![Temperatuur telemetrie](media/iot-suite-remote-monitoring-test/telemetry.png)

Op de **apparaten** pagina kunt u exemplaren van het type van uw nieuwe inrichten:

![De lijst met beschikbare simulaties weergeven](media/iot-suite-remote-monitoring-test/devicesmodellist.png)

U kunt de telemetrie van het gesimuleerde apparaat bekijken:

![Telemetrie van paginaweergaven gloeilamp](media/iot-suite-remote-monitoring-test/devicestelemetry.png)

U kunt aanroepen de **SwitchOn** en **SwitchOff** methoden op uw apparaat:

![Gloeilamp methoden aanroepen](media/iot-suite-remote-monitoring-test/devicesmethods.png)

## <a name="add-a-new-telemetry-type"></a>Een nieuw telemetrie-type toevoegen

Deze sectie beschrijft het wijzigen van een bestaand type gesimuleerd apparaat ter ondersteuning van een nieuw telemetrie-type.

### <a name="locate-the-chiller-device-type-files"></a>Zoek de Koelunit apparaat type bestanden

De volgende stappen ziet u hoe u de bestanden die definiëren van de ingebouwde vinden **Koelunit** apparaat:

1. Als u dit nog niet hebt gedaan, gebruik de volgende opdracht voor het klonen van de **apparaat simulatie** GitHub-opslagplaats met uw lokale machine:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
    ```

1. Elk apparaattype heeft een JSON-modelbestand en de bijbehorende scripts in de `data/devicemodels` map. De bestanden die de gesimuleerde definiëren **Koelunit** apparaattype zijn:

    * **data/devicemodels/chiller-01.json**
    * **data/devicemodels/scripts/chiller-01-state.js**

### <a name="specify-the-new-telemetry-type"></a>Geef het nieuwe telemetrie-type

De volgende stappen ziet u het toevoegen van een nieuwe **interne temperatuur** type de **Koelunit** apparaattype:

1. Open de **Koelunit 01.json** bestand.

1. Update de **SchemaVersion** waarde als volgt:

    ```json
    "SchemaVersion": "1.1.0",
    ```

1. In de **InitialState** sectie, de definities dit twee toevoegen:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. In de **telemetrie** matrix kunnen de volgende definitie toevoegen:

    ```json
    {
      "Interval": "00:00:05",
      "MessageTemplate": "{\"internal_temperature\":${internal_temperature},\"internal_temperature_unit\":\"${internal_temperature_unit}\"}",
      "MessageSchema": {
        "Name": "chiller-internal-temperature;v1",
        "Format": "JSON",
        "Fields": {
          "temperature": "double",
          "temperature_unit": "text"
        }
      }
    },
    ```

1. Sla de **Koelunit 01.json** bestand.

1. Open de **scripts/Koelunit-01-state.js** bestand.

1. De volgende velden toevoegen aan de **status** variabele:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Voeg de volgende regel om de **belangrijkste** functie:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Sla de **scripts/Koelunit-01-state.js** bestand.

### <a name="test-the-chiller-device-type"></a>Het type van het apparaat Koelunit testen

Voor het testen van de bijgewerkte **Koelunit** apparaattype, de eerste uitvoering van een lokale kopie van de **apparaat simulatie** service voor het testen van uw apparaattype naar verwachting werkt. Wanneer u hebt getest en foutopsporing van uw bijgewerkte apparaattype lokaal, kunt u de container opnieuw maken en implementeren het **apparaat simulatie** service naar Azure.

Bij het uitvoeren van de **apparaat simulatie** service lokaal het telemetrie verzendt naar uw oplossing voor externe controle. Op de **apparaten** pagina kunt u exemplaren van het type van uw bijgewerkte inrichten.

Als u wilt testen en fouten opsporen in uw wijzigingen op een lokaal, Zie de vorige sectie [gloeilamp-apparaattype lokaal testen](#test-the-lightbulb-device-type-locally).

Als u wilt uw bijgewerkte apparaten simulatie-service op de virtuele machine van de oplossing in Azure implementeert, Zie de vorige sectie [de bijgewerkte simulator implementeren in de cloud](#deploy-the-updated-simulator-to-the-cloud).

Op de **apparaten** pagina kunt u exemplaren van het type van uw bijgewerkte inrichten:

![Bijgewerkte Koelunit toevoegen](media/iot-suite-remote-monitoring-test/devicesupdatedchiller.png)

U vindt de nieuwe **interne temperatuur** telemetrie van het gesimuleerde apparaat.

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie hebt u geleerd hoe naar:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Maak een nieuwe apparaattype
> * Aangepast apparaat gedrag simuleren
> * Een nieuw apparaattype toevoegen aan het dashboard
> * Aangepaste telemetrie verzenden vanuit een bestaand apparaattype

Nu hebt u het aanpassen van de simulatie device service geleerd. De voorgestelde volgende stap is te leren hoe u [een fysiek apparaat aansluit op uw oplossing voor externe controle](iot-suite-connecting-devices-node.md).

Zie voor meer informatie voor ontwikkelaars over de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->