---
title: Apparaatsimulatie in de oplossing voor externe controle - Azure | Microsoft Docs
description: Deze zelfstudie leert u hoe de apparaatsimulator gebruiken met de oplossingsverbetering voor externe controle.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/15/2018
ms.topic: conceptual
ms.openlocfilehash: 8d8835bd97b489a730a040e86748c668963c7196
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39187720"
---
# <a name="create-a-new-simulated-device"></a>Een nieuw gesimuleerd apparaat maken

Deze zelfstudie leert u over het aanpassen van het apparaat simulator microservice in de oplossingsverbetering voor externe controle. Om weer te geven de mogelijkheden van de apparaatsimulator in dat deze zelfstudie maakt gebruik van twee scenario's in de Contoso-IoT-toepassing.

De volgende video biedt een overzicht van de opties voor het aanpassen van het apparaat simulator microservice:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/How-to-customize-the-Remote-Monitoring-Preconfigured-Solution-for-Azure-IoT/Player]

In het eerste scenario wil Contoso voor het testen van een nieuwe slimme gloeilamp-apparaat. Als u wilt de uit te voeren, kunt u een nieuw gesimuleerd apparaat maken met de volgende kenmerken:

*Eigenschappen*

| Naam                     | Waarden                      |
| ------------------------ | --------------------------- |
| Kleur                    | Wit, rood, blauw            |
| Helderheid               | 0 tot 100                    |
| Geschatte resterende levensduur | Aftelling van 10.000 uren |

*Telemetry*

De volgende tabel ziet u de gegevens dat de gloeilamp rapporten naar de cloud als een gegevensstroom:

| Naam   | Waarden      |
| ------ | ----------- |
| Status | 'on', 'off' |
| Temperatuur | Graden F |
| Online | waar of ONWAAR |

> [!NOTE]
> De **online** telemetriewaarde is verplicht voor alle gesimuleerde typen.

*Methoden*

De volgende tabel ziet u de acties die ondersteuning biedt voor het nieuwe apparaat:

| Naam        |
| ----------- |
| Overschakelen op   |
| Uitschakelen  |

*Beginstatus*

De volgende tabel ziet u de oorspronkelijke status van het apparaat:

| Naam                     | Waarden |
| ------------------------ | -------|
| Eerste kleur            | Wit  |
| Eerste helderheid       | 75     |
| Eerste resterende levensduur   | 10.000 |
| Status van de eerste telemetrie | 'aan'   |
| Eerste telemetrie temperatuur | 200   |

In het tweede scenario, voegt u een nieuw telemetrietype naar Contoso de bestaande **Koelunit** apparaat.

Deze zelfstudie leert u hoe u kunt de apparaatsimulator gebruiken met de oplossingsverbetering voor externe controle:

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * Maak een nieuwe apparaattype
> * Aangepast apparaat gedrag simuleren
> * Een nieuwe apparaattype aan het dashboard toevoegen
> * Aangepaste telemetrie verzenden vanuit een bestaande apparaattype

De volgende video ziet u een overzicht van het gesimuleerde en echte apparaten verbinden met de oplossing voor externe controle:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Part-38-Customizing-Azure-IoT-Suite-solution-and-connect-a-real-device/Player]

## <a name="prerequisites"></a>Vereisten

Als u wilt in deze zelfstudie volgen, hebt u het volgende nodig:

* Een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement. Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [de oplossingsverbetering voor externe bewaking implementeren](../iot-accelerators/quickstart-remote-monitoring-deploy.md) zelfstudie.

* Visual Studio 2017. Als u geen Visual Studio 2017 is geïnstalleerd, kunt u downloaden de gratis [Visual Studio Community](https://www.visualstudio.com/free-developer-offers/) edition.

* [In de cloud Explorer voor Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVS15Preview) Visual Studio-uitbreiding.

* Een account op [Docker Hub](https://hub.docker.com/). U kunt zich registreren voor gratis aan de slag.

* [GIT](https://git-scm.com/downloads) op uw computer geïnstalleerd.

## <a name="prepare-your-development-environment"></a>Uw ontwikkelomgeving voorbereiden

Voer de volgende taken om voor te bereiden uw ontwikkelomgeving voor het toevoegen van een nieuw gesimuleerd apparaat aan uw oplossing voor externe controle:

### <a name="configure-ssh-access-to-the-solution-virtual-machine-in-azure"></a>SSH-toegang tot de oplossing voor virtuele machine in Azure configureren

Tijdens het maken van uw oplossing voor externe controle op [www.azureiotsolutions.com](https://www.azureiotsolutions.com), u de naam van een oplossing hebt gekozen. De oplossingsnaam van de wordt de naam van de Azure-resourcegroep die de verschillende geïmplementeerde resources die gebruikmaakt van de oplossing bevat. De volgende opdrachten gebruikt u een resourcegroep met de naam **Contoso-01**, moet u vervangen **Contoso-01** met de naam van de resourcegroep.

De volgende opdrachten gebruiken de `az` opdracht uit vanaf [Azure CLI 2.0](https://docs.microsoft.com/cli/azure?view=azure-cli-latest). U kunt de Azure CLI 2.0 installeren op uw ontwikkelcomputer of gebruik de [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) in de [Azure-portal](http://portal.azure.com). De Azure CLI 2.0 is vooraf geïnstalleerd in de Cloud Shell.

1. Als u wilt controleren of de naam van de resourcegroep die uw resources voor externe bewaking bevat, moet u de volgende opdracht uitvoeren:

    ```sh
    az group list | grep "name"
    ```

    Met deze opdracht worden alle resourcegroepen in uw abonnement. De lijst moet een resourcegroep met dezelfde naam als uw oplossing voor externe controle bevatten.

1. Als u wilt dat de resource groep voor de volgende opdrachten, voer de volgende opdracht uit met behulp van de naam van uw resource in plaats van **Contoso-01**:

    ```sh
    az configure --defaults group=Contoso-01
    ```

1. Als u de resources in uw resourcegroep, voer de volgende opdracht:

    ```sh
    az resource list -o table
    ```

    Noteer de namen van uw virtuele machine en de netwerkbeveiligingsgroep. U kunt deze waarden gebruiken in latere stappen.

1. Als u SSH-toegang tot uw virtuele machine, voert u de volgende opdracht uit met de naam van uw netwerkbeveiligingsgroep in de vorige stap:

    ```sh
    az network nsg rule create --name SSH --nsg-name YOUR-NETWORK-SECURITY-GROUP --priority 101 --destination-port-ranges 22 --access Allow --protocol TCP
    ```

    Als u wilt weergeven in de lijst met regels voor binnenkomende verbindingen voor uw netwerk, moet u de volgende opdracht uitvoeren:

    ```sh
    az network nsg rule list --nsg-name YOUR-NETWORK-SECURITY-GROUP -o table
    ```

1. Als u wilt wijzigen van het wachtwoord van de virtuele machine naar een wachtwoord weet u, voert u de volgende opdracht uit. Gebruik de naam van de virtuele machine die u eerder hebt genoteerd en een wachtwoord van uw keuze:

    ```sh
    az vm user update --name YOUR-VM-NAME --username azureuser --password YOUR-PASSWORD
    ```
1. Zoeken naar het IP-adres van uw virtuele machine, gebruikt u de volgende opdracht en noteer het openbare IP-adres:

    ```sh
    az vm list-ip-addresses --name YOUR-VM-NAME
    ```

1. U kunt nu SSH gebruiken om verbinding met uw virtuele machine te maken. De `ssh` opdracht is vooraf geïnstalleerd in de Cloud Shell. Gebruik het openbare IP-adres uit de vorige stap en wanneer u hierom wordt gevraagd, het wachtwoord dat u hebt geconfigureerd voor de virtuele machine:

    ```sh
    ssh azureuser@public-ip-address
    ```

    U hebt nu toegang tot de shell op de virtuele machine die de Docker-containers in de oplossing voor externe controle wordt uitgevoerd. Als u de actieve containers, gebruik de volgende opdracht:

    ```sh
    docker ps
    ```

### <a name="find-the-service-connection-strings"></a>De service-verbindingsreeksen zoeken

In de zelfstudie werkt u met Visual Studio-oplossing die is verbonden met het Cosmos DB en IoT Hub-services van de oplossing. De volgende stappen ziet u een manier om te vinden van de verbinding tekenreekswaarden die u nodig hebt:

1. Als u zoekt de Cosmos DB-verbindingsreeks, voer de volgende opdracht in de SSH-sessie die is verbonden met de virtuele machine:

    ```sh
    sudo grep STORAGEADAPTER_DOCUMENTDB /app/env-vars
    ```

    Noteer de verbindingsreeks. U gebruikt deze waarde verderop in de zelfstudie.

1. Als u zoekt de IoT Hub-verbindingsreeks, voer de volgende opdracht in de SSH-sessie die is verbonden met de virtuele machine:

    ```sh
    sudo grep IOTHUB_CONNSTRING /app/env-vars
    ```

    Noteer de verbindingsreeks. U gebruikt deze waarde verderop in de zelfstudie.

> [!NOTE]
> U kunt deze tekenreeksen voor databaseverbindingen ook vinden in Azure portal of met behulp van de `az` opdracht.

### <a name="stop-the-device-simulation-service-in-the-virtual-machine"></a>De apparaat-simulatie-service in de virtuele machine stoppen

Wanneer u de service van de simulatie apparaat wijzigt, kunt u lokaal te testen van uw wijzigingen kunt uitvoeren. Voordat u de simulatie device service lokaal uitvoert, moet u het exemplaar dat als volgt die worden uitgevoerd in de virtuele machine stoppen:

1. Om te zoeken de **CONTAINER-ID** van de **apparaat-simulatie-dotnet** -service, voer de volgende opdracht in de SSH-sessie die is verbonden met de virtuele machine:

    ```sh
    docker ps
    ```

    Maak een notitie van de container-ID van de **apparaat-simulatie-dotnet** service.

1. Om te stoppen de **apparaat-simulatie-dotnet** container, voer de volgende opdracht uit:

    ```sh
    docker stop container-id-from-previous-step
    ```

### <a name="clone-the-github-repositories"></a>Kloon de GitHub-opslagplaatsen

In deze zelfstudie, werkt u met de **apparaatsimulatie** en **opslagadapter** Visual Studio-projecten. U kunt de broncodeopslagplaatsen vanuit GitHub klonen. Deze stap uitvoeren op uw lokale ontwikkelcomputer waarop u Visual Studio geïnstalleerd hebt:

1. Open een opdrachtprompt en navigeer naar de map waar u uw exemplaar van de **apparaatsimulatie** en **opslagadapter** GitHub-opslagplaatsen.

1. Voor het klonen van de .NET-versie van de **apparaatsimulatie** opslagplaats, voer de volgende opdracht uit:

    ```cmd
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

    De apparaat-simulatie-service in de oplossing voor externe controle kunt u wijzigingen aanbrengen in de typen ingebouwde gesimuleerde apparaten en voor het maken van nieuwe gesimuleerde apparaattypen. Aangepaste apparaattypen kunt u het gedrag van de oplossing voor externe controle te testen voordat u verbinding maakt met uw fysieke apparaten.

1. Voor het klonen van de .NET-versie van de **opslagadapter** opslagplaats, voer de volgende opdracht uit:

    ```cmd
    git clone https://github.com/Azure/pcs-storage-adapter-dotnet.git
    ```

    De simulatie device service maakt gebruik van de opslagservice voor de adapter verbinding maken met de Cosmos DB-service in Azure. De oplossing voor externe controle slaat de configuratiegegevens van het gesimuleerde apparaat in een Cosmos DB-database.

### <a name="run-the-storage-adapter-service-locally"></a>De service storage-adapter lokaal uitvoeren

De simulatie device service maakt gebruik van de opslagservice voor de adapter verbinding maken met Cosmos DB-database van de oplossing. Als u de simulatie device service lokaal uitvoert, moet u de adapter opslagservice ook lokaal uitvoeren. De volgende stappen laten zien hoe u de adapter opslagservice uitvoeren vanuit Visual Studio:

1. Open in Visual Studio, de **pc's-opslag-adapter.sln** oplossingsbestand in de lokale kloon van de **opslagadapter** opslagplaats.

1. Klik in Solution Explorer met de rechtermuisknop op de **WebService** project, kies **eigenschappen**, en kies vervolgens **Debug**.

1. In de **omgevingsvariabelen** sectie, bewerkt u de waarde van de **pc's\_STORAGEADAPTER\_DOCUMENTDB\_CONNSTRING** variabele moet de Cosmos DB-verbinding tekenreeks die u eerder hebt genoteerd. Sla uw wijzigingen op.

1. Klik in Solution Explorer met de rechtermuisknop op de **WebService** project, kies **Debug**, en kies vervolgens **nieuwe instantie starten**.

1. De service wordt gestart die lokaal wordt uitgevoerd en wordt geopend `http://localhost:9022/v1/status` in de standaardbrowser. Controleer de **Status** waarde is ' OK: Alive en goed. "

1. Laat de opslagservice-adapter lokaal worden uitgevoerd voordat u de zelfstudie hebt voltooid.

U beschikt nu over alles wat erin, en u bent klaar om te beginnen een nieuw gesimuleerd apparaattype toe te voegen aan uw oplossing voor externe controle.

## <a name="create-a-simulated-device-type"></a>Een gesimuleerd apparaat maken

De eenvoudigste manier om een nieuwe apparaattype maken in de service van de simulatie apparaat is het kopiëren en wijzigen van een bestaand type. De volgende stappen laten zien over het kopiëren van de ingebouwde **Koelunit** apparaat om te maken van een nieuwe **gloeilamp** apparaat:

1. Klik in Solution Explorer met de rechtermuisknop op de **WebService** project, kies **eigenschappen**, en kies vervolgens **Debug**.

1. In de **omgevingsvariabelen** sectie, bewerkt u de waarde van de **pc's\_IOTHUB\_CONNSTRING** variabele moet de IoT Hub-verbindingsreeks u eerder hebt genoteerd. Sla uw wijzigingen op.

1. Klik in Solution Explorer met de rechtermuisknop op de **apparaatsimulatie** oplossing en kies **Opstartprojecten instellen**. Kies **Single startup project** en selecteer **WebService**. Klik vervolgens op **OK**.

1. Elk apparaattype heeft een JSON-bestand voor model en de bijbehorende scripts in de **Services/data/devicemodels** map. Kopieer in Solution Explorer de **Koelunit** bestanden maken de **gloeilamp** bestanden zoals wordt weergegeven in de volgende tabel:

    | Bron                      | Doel                   |
    | --------------------------- | ----------------------------- |
    | chiller-01.json             | lightbulb-01.json             |
    | scripts/Koelunit-01-state.js | scripts/gloeilamp-01-state.js |
    | scripts/opnieuw opstarten-method.js    | scripts/SwitchOn-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>De eigenschappen van het nieuwe apparaattype definiëren

De **gloeilamp 01.json** -bestand definieert de eigenschappen van het type, zoals de telemetrie wordt gegenereerd en de methoden die wordt ondersteund. De volgende stappen update de **gloeilamp 01.json** bestand voor het definiëren van de **gloeilamp** apparaat:

1. In de **gloeilamp 01.json** bestand, het bijwerken van de metagegevens van apparaten, zoals wordt weergegeven in het volgende codefragment:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. In de **gloeilamp 01.json** bestand, het bijwerken van de definitie van de simulatie, zoals wordt weergegeven in het volgende codefragment:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "temperature": 200.0,
        "temperature_unit": "F",
        "status": "on"
      },
      "Interval": "00:00:20",
      "Scripts": [
        {
          "Type": "javascript",
          "Path": "lightbulb-01-state.js"
        }
      ]
    },
    ```

1. In de **gloeilamp 01.json** bestand, het bijwerken van de eigenschappen van het apparaat, zoals wordt weergegeven in het volgende codefragment:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. In de **gloeilamp 01.json** bestand, het apparaat type telemetrie-definities bijwerken zoals weergegeven in het volgende codefragment:

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

1. In de **gloeilamp 01.json** bestand, bij te werken van methoden voor het apparaat, zoals wordt weergegeven in het volgende codefragment:

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

De **scripts/gloeilamp-01-state.js** bestand definieert het gedrag van de simulatie van de **gloeilamp** type. De volgende stappen update de **scripts/gloeilamp-01-state.js** bestand voor het definiëren van het gedrag van de **gloeilamp** apparaat:

1. Bewerken van de definitie van de status in de **scripts/gloeilamp-01-state.js** bestand zoals weergegeven in het volgende fragment:

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

1. Bewerk de **belangrijkste** functie voor het implementeren van het gedrag, zoals weergegeven in het volgende fragment:

    ```js
    function main(context, previousState, previousProperties) {

        // Restore the global device properties and the global state before
        // generating the new telemetry, so that the telemetry can apply changes
        // using the previous function state.
        restoreSimulation(previousState, previousProperties);

        state.temperature = vary(200, 5, 150, 250);

        // Make this flip every so often
        state.status = flip(state.status);

        updateState(state);

        return state;
    }
    ```

1. Sla de **scripts/gloeilamp-01-state.js** bestand.

De **scripts/SwitchOn-method.js** bestand implementeert de **Switch op** methode in een **gloeilamp** apparaat. De volgende stappen update de **scripts/SwitchOn-method.js** bestand:

1. Bewerken van de definitie van de status in de **scripts/SwitchOn-method.js** bestand zoals weergegeven in het volgende fragment:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Als u wilt overschakelen op de gloeilamp, bewerk de **belangrijkste** werkt als volgt:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Sla de **scripts/SwitchOn-method.js** bestand.

1. Een kopie maken de **scripts/SwitchOn-method.js** bestand met de naam **scripts/SwitchOff-method.js**.

1. Als u wilt de gloeilamp uitschakelen, bewerken de **belangrijkste** werken in de **scripts/SwitchOff-method.js** bestand als volgt:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Sla de **scripts/SwitchOff-method.js** bestand.

1. Klik in Solution Explorer, selecteert u elk van de vier nieuwe bestanden op zijn beurt. In de **eigenschappen** venster voor elk bestand, Controleer **naar uitvoermap kopiëren** is ingesteld op **kopiëren indien nieuwer**.

### <a name="configure-the-device-simulation-service"></a>De simulatie device service configureren

Als u wilt beperken het nummer van de gesimuleerde apparaten die verbinding met de oplossing tijdens het testen maken, moet u de service voor het uitvoeren van een enkele Koelunit en een apparaat één gloeilamp configureren. De configuratiegegevens is opgeslagen in de Cosmos DB-exemplaar in de resourcegroep van de oplossing. Als u wilt bewerken in de configuratiegegevens, gebruikt u de **Cloud Explorer** weergeven in Visual Studio:

1. Om te openen de **Cloud Explorer** bekijken in Visual Studio, kiest u **weergave** en vervolgens **Cloud Explorer**.

1. In het configuratiebestand voor simulatie vinden **zoeken naar resources** Voer **simualtions.1**.

1. Dubbelklik op de **simulations.1** document te bewerken.

1. In de waarde voor **gegevens**, zoek de **DeviceModels** matrix dat lijkt op het volgende codefragment:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"chiller-02\",\"Count\":1},{\"Id\":\"elevator-01\",\"Count\":1},{\"Id\":\"elevator-02\",\"Count\":1},{\"Id\":\"engine-01\",\"Count\":1},{\"Id\":\"engine-02\",\"Count\":1},{\"Id\":\"prototype-01\",\"Count\":1},{\"Id\":\"prototype-02\",\"Count\":1},{\"Id\":\"truck-01\",\"Count\":1},{\"Id\":\"truck-02\",\"Count\":1}]
    ```

1. Voor het definiëren van een enkele Koelunit en een gesimuleerd apparaat met één gloeilamp, vervangen de **DeviceModels** matrix met de volgende code:

    ```json
    [{\"Id\":\"chiller-01\",\"Count\":1},{\"Id\":\"lightbulb-01\",\"Count\":1}]
    ```

    Sla de wijziging naar de **simulations.1** document.

> [!NOTE]
> U kunt ook de Cosmos DB Data Explorer in Azure portal gebruiken om te bewerken de **simulations.1** document.

### <a name="test-the-lightbulb-device-type-locally"></a>De gloeilamp apparaattype lokaal testen

U bent nu klaar voor het testen van uw nieuwe gesimuleerde gloeilamp type door het apparaat simuleren-project lokaal worden uitgevoerd.

1. Klik in Solution Explorer met de rechtermuisknop op **WebService**, kiest u **Debug** en kies vervolgens **nieuwe instantie starten**.

1. Om te controleren dat de twee gesimuleerde apparaten zijn verbonden met uw IoT-Hub, de Azure-portal in uw browser te openen.

1. Navigeer naar de IoT-hub in de resourcegroep waarin uw oplossing voor externe controle.

1. In de **bewaking** sectie **metrische gegevens**. Controleer het aantal **verbonden apparaten** is twee:

    ![Aantal verbonden apparaten](./media/iot-accelerators-remote-monitoring-test/connecteddevices.png)

1. Navigeer in uw browser naar de **Dashboard** voor uw oplossing voor externe controle. In het deelvenster telemetrie op de **Dashboard**, selecteer **temperatuur**. De temperatuur waarmee uw gesimuleerde apparaten worden weergegeven in de grafiek:

    ![Temperatuurtelemetrie](./media/iot-accelerators-remote-monitoring-test/telemetry.png)

U hebt nu de gloeilamp apparaatsimulatie die lokaal wordt uitgevoerd. De volgende stap is het implementeren van uw bijgewerkte simulatorcode op de virtuele machine die wordt uitgevoerd van de bewaking op afstand microservices in Azure.

Voordat u doorgaat, kunt u stoppen met het opsporen van fouten in de apparaatsimulatie- en storage-adapter projecten in Visual Studio.

### <a name="deploy-the-updated-simulator-to-the-cloud"></a>De bijgewerkte simulator implementeren in de cloud

De microservices in de oplossing voor externe controle uitvoeren in docker-containers. De containers worden gehost in virtuele machine van de oplossing in Azure. In deze sectie doet u het volgende:

* Maak een nieuw apparaat simulatie docker-installatiekopie.
* De installatiekopie uploaden naar uw docker hub-opslagplaats.
* De afbeelding in uw oplossing voor de virtuele machine importeren.

De volgende stappen wordt ervan uitgegaan dat u een opslagplaats met de naam hebt **gloeilamp** in uw Docker Hub-account.

1. In Visual Studio in de **apparaatsimulatie** project, open het bestand **solution\scripts\docker\build.cmd**.

1. Bewerk de regel die bepaalt de **DOCKER_IMAGE** omgevingsvariabele op de naam van uw Docker Hub-opslagplaats:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Sla de wijziging.

1. In Visual Studio in de **apparaatsimulatie** project, open het bestand **solution\scripts\docker\publish.cmd**.

1. Bewerk de regel die bepaalt de **DOCKER_IMAGE** omgevingsvariabele op de naam van uw Docker Hub-opslagplaats:

    ```cmd
    SET DOCKER_IMAGE=your-docker-hub-acccount/lightbulb
    ```

    Sla de wijziging.

1. Open een opdrachtprompt als beheerder. Navigeer naar de map **scripts\docker** in de kloon van de **apparaatsimulatie** GitHub-opslagplaats.

1. Voor het bouwen van de docker-installatiekopie, moet u de volgende opdracht uitvoeren:

    ```cmd
    build.cmd
    ```

1. Als u wilt aanmelden bij uw Docker Hub-account, moet u de volgende opdracht uitvoeren:

    ```cmd
    docker login
    ```

1. Als u wilt uw nieuwe installatiekopie uploaden naar uw Docker Hub-account, moet u de volgende opdracht uitvoeren:

    ```cmd
    publish.cmd
    ```

1. Als u wilt controleren of het uploaden, gaat u naar [ https://hub.docker.com/ ](https://hub.docker.com/). Zoek uw **gloeilamp** opslagplaats en kies **Details**. Kies vervolgens **Tags**:

    ![Docker-hub](./media/iot-accelerators-remote-monitoring-test/dockerhub.png)

    De scripts die zijn toegevoegd de **testen** tag de installatiekopie.

1. SSH gebruiken om te verbinden met uw oplossing voor de virtuele machine in Azure. Navigeer naar de **App** map en bewerk de **docker-compose.yml** bestand:

    ```sh
    cd /app
    sudo nano docker-compose.yml
    ```

1. Hiermee bewerkt u de vermelding voor de service van de simulatie van apparaat naar uw docker-installatiekopie gebruiken:

    ```yaml
    devicesimulation:
      image: {your docker ID}/lightbulb:testing
    ```

    Sla uw wijzigingen op.

1. Als u wilt alle services met de nieuwe instellingen opnieuw hebt opgestart, moet u de volgende opdracht uitvoeren:

    ```sh
    sudo ./start.sh
    ```

1. Om te controleren of het logboekbestand van de nieuwe apparaat simuleren-container, voer de volgende opdracht te vinden van de container-ID:

    ```sh
    docker ps
    ```

    Voer vervolgens de volgende opdracht uit met behulp van de container-ID:

    ```sh
    docker logs {container ID}
    ```

U hebt nu de stappen voor het implementeren van een bijgewerkte versie van het apparaat simuleren-service voor uw oplossing voor externe controle.

Navigeer in uw browser naar de **Dashboard** voor uw oplossing voor externe controle. In het deelvenster telemetrie op de **Dashboard**, selecteer **temperatuur**. De temperatuur voor uw twee gesimuleerde apparaten worden weergegeven in de grafiek:

![Temperatuurtelemetrie](./media/iot-accelerators-remote-monitoring-test/telemetry.png)

Op de **apparaten** pagina, kunt u exemplaren van het nieuwe type inrichten:

![De lijst met beschikbare simulaties weergeven](./media/iot-accelerators-remote-monitoring-test/devicesmodellist.png)

U kunt de telemetrie van het gesimuleerde apparaat bekijken:

![Gloeilamp-telemetrie weergeven](./media/iot-accelerators-remote-monitoring-test/devicestelemetry.png)

U kunt aanroepen de **SwitchOn** en **SwitchOff** methoden op uw apparaat:

![Gloeilamp methoden aanroepen](./media/iot-accelerators-remote-monitoring-test/devicesmethods.png)

## <a name="add-a-new-telemetry-type"></a>Een nieuw telemetrietype toevoegen

In deze sectie wordt beschreven hoe u een bestaand type gesimuleerd apparaat ter ondersteuning van een nieuwe telemetrietype wijzigen.

### <a name="locate-the-chiller-device-type-files"></a>Zoek de Koelunit apparaat type bestanden

De volgende stappen laten zien over het vinden van de bestanden die definiëren van de ingebouwde **Koelunit** apparaat:

1. Als u hebt nog niet gedaan, gebruik de volgende opdracht voor het klonen van de **apparaat-simulatie-dotnet** GitHub-opslagplaats naar uw lokale computer:

    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git
    ```

1. Elk apparaattype heeft een JSON-bestand voor model en de bijbehorende scripts in de `data/devicemodels` map. De bestanden die worden gedefinieerd de gesimuleerde **Koelunit** apparaattype zijn:

    * **Data/devicemodels/chiller-01.JSON**
    * **Data/devicemodels/scripts/chiller-01-State.js**

### <a name="specify-the-new-telemetry-type"></a>Geef de nieuwe telemetrietype

De volgende stappen ziet u het toevoegen van een nieuwe **interne temperatuur** type de **Koelunit** apparaattype:

1. Open de **Koelunit 01.json** bestand.

1. Update de **SchemaVersion** waarde als volgt:

    ```json
    "SchemaVersion": "1.1.0",
    ```

1. In de **InitialState** sectie, voegt u de definities follwing twee:

    ```json
    "internal_temperature": 65.0,
    "internal_temperature_unit": "F",
    ```

1. In de **telemetrie** matrix, voegt u de definitie van de volgende:

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

1. Voeg de volgende regel aan de **belangrijkste** functie:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Sla de **scripts/Koelunit-01-state.js** bestand.

### <a name="test-the-chiller-device-type"></a>Het apparaattype Koelunit testen

Voor het testen van de bijgewerkte **Koelunit** apparaattype, voor het eerst uitvoert een lokale kopie van de **apparaat-simulatie-dotnet** service voor het testen van uw apparaattype werkt zoals verwacht. Wanneer u hebt getest en foutopsporing van uw bijgewerkte apparaattype lokaal, kunt u opnieuw maken van de container en implementeren het **apparaat-simulatie-dotnet** service naar Azure.

Bij het uitvoeren van de **apparaat-simulatie-dotnet** service lokaal deze telemetrie verzendt naar uw oplossing voor externe controle. Op de **apparaten** pagina, kunt u exemplaren van uw bijgewerkte type inrichten.

Als u wilt testen en foutopsporing van uw wijzigingen lokaal, Zie de vorige sectie [de gloeilamp apparaattype lokaal testen](#test-the-lightbulb-device-type-locally).

Voor het implementeren van uw bijgewerkte apparaat simuleren-service op virtuele machine van de oplossing in Azure, Zie de vorige sectie [de bijgewerkte simulator implementeren in de cloud](#deploy-the-updated-simulator-to-the-cloud).

Op de **apparaten** pagina, kunt u exemplaren van uw bijgewerkte type inrichten:

![Bijgewerkte Koelunit toevoegen](./media/iot-accelerators-remote-monitoring-test/devicesupdatedchiller.png)

U vindt de nieuwe **interne temperatuur** telemetrie van het gesimuleerde apparaat.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe aan:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Maak een nieuwe apparaattype
> * Aangepast apparaat gedrag simuleren
> * Een nieuwe apparaattype aan het dashboard toevoegen
> * Aangepaste telemetrie verzenden vanuit een bestaande apparaattype

U hebt nu geleerd over het aanpassen van het apparaat simuleren-service. De voorgestelde volgende stap is te leren hoe u [een fysiek apparaat verbinden met uw oplossing voor externe controle](iot-accelerators-connecting-devices-node.md).

Zie voor meer informatie voor ontwikkelaars over de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
