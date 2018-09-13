---
title: bestand opnemen
description: bestand opnemen
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/16/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 9196648d7e3d2ea717b1a61cbca959805649ed2f
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44754435"
---
In het eerste scenario, voegt u een nieuw telemetrietype naar Contoso de bestaande **Koelunit** apparaattype.

In het tweede scenario wil Contoso voor het testen van een nieuwe slimme gloeilamp-apparaat. De tests uitvoeren, kunt u een nieuw gesimuleerd apparaat maken met de volgende kenmerken:

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

Voor de stappen in deze handleiding, hebt u een actief Azure-abonnement nodig.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Vereisten

Als u wilt in deze gebruiksaanwijzing volgen, hebt u het volgende nodig:

* Visual Studio Code. U kunt [Visual Studio Code downloaden voor Mac, Linux en Windows](https://code.visualstudio.com/download).
* .NET core. U kunt downloaden [.NET Core voor Mac, Linux en Windows](https://www.microsoft.com/net/download).
* [C# voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* Postman. U kunt downloaden [Postman voor Mac, Windows of Linux](https://www.getpostman.com/apps).
* Een [IoT-hub die zijn geïmplementeerd in uw Azure-abonnement](../articles/iot-hub/iot-hub-create-through-portal.md). U moet de IoT-hub-verbindingsreeks voor de stappen in deze handleiding. U kunt de verbindingsreeks ophalen vanuit de Azure-portal.
* Een Cosmos DB-database die gebruikmaakt van de SQL-API en die is geconfigureerd voor [sterke consistentie](../articles/cosmos-db/manage-account.md). U moet de verbindingsreeks van de Cosmos DB-database voor de stappen in deze handleiding. U kunt de verbindingsreeks ophalen vanuit de Azure-portal.

## <a name="prepare-your-development-environment"></a>Uw ontwikkelomgeving voorbereiden

Voer de volgende taken als u wilt uw ontwikkelomgeving voorbereiden:

* Download de bron voor het apparaat simuleren-microservice.
* Download de broncode voor de opslag-adapter-microservice.
* De opslag-adapter microservice lokaal uitvoeren.

De instructies in dit artikel wordt ervan uitgegaan dat u gebruikmaakt van Windows. Als u een ander besturingssysteem gebruikt, moet u mogelijk enkele van de bestandspaden en opdrachten op basis van uw omgeving aanpassen.

### <a name="download-the-microservices"></a>Download de microservices

Downloaden en pak deze uit de [voor externe controle van microservices](https://github.com/Azure/remote-monitoring-services-dotnet/archive/master.zip) vanuit GitHub naar een geschikte locatie op uw lokale computer.

### <a name="run-the-storage-adapter-microservice"></a>De opslag-adapter microservice uitvoeren

Open de **remote-monitoring-services-dotnet-master\storage-adapter** map in Visual Studio Code. Klik op een **herstellen** knoppen om op te lossen dat alle afhankelijkheden niet opgelost.

Open de **.vscode/launch.json** bestands- en toewijzen van uw Cosmos DB-verbindingsreeks voor de **PCS_STORAGEADAPTER_DOCUMENTDB_CONNSTRING** omgevingsvariabele.

<!-- Open the **WebService/appsettings.ini** file and assign your Cosmos DB connection string to the **documentdb_connstring** configuration setting.-->

Als u wilt de microservices lokaal uitvoeren, klikt u op **fouten opsporen > Foutopsporing starten**.

De **Terminal** venster in Visual Studio Code ziet u uitvoer van de actieve microservice, met inbegrip van een URL voor de web service-statuscontrole: [ http://127.0.0.1:9022/v1/status ](http://127.0.0.1:9022/v1/status). Wanneer u naar dit adres navigeert, de status moet ' OK: Alive en goed '.

Laat u de opslag-adapter microservice in dit exemplaar van Visual Studio Code wordt uitgevoerd terwijl u de volgende stappen uitvoeren.

## <a name="modify-the-chiller"></a>De Koelunit wijzigen

In deze sectie maakt u een nieuwe toevoegen **interne temperatuur** telemetrietype aan de bestaande **Koelunit** apparaattype:

1. Maak een nieuwe map **C:\temp\devicemodels** op uw lokale computer.

1. Kopieer de volgende bestanden naar uw nieuwe map van de gedownloade kopie van het apparaat simulatie microservice:

    | Bron | Doel |
    | ------ | ----------- |
    | Services\Data\devicemodels\chiller-01.JSON | C:\temp\devicemodels\chiller-01.JSON |
    | Services\Data\devicemodels\scripts\chiller-01-State.js | C:\temp\devicemodels\scripts\chiller-01-State.js |
    | Services\Data\devicemodels\scripts\Reboot-Method.js | C:\temp\devicemodels\scripts\Reboot-Method.js |
    | Services\Data\devicemodels\scripts\FirmwareUpdate-Method.js | C:\temp\devicemodels\scripts\FirmwareUpdate-Method.js |
    | Services\Data\devicemodels\scripts\EmergencyValveRelease-Method.js | C:\temp\devicemodels\scripts\EmergencyValveRelease-Method.js |
    | Services\Data\devicemodels\scripts\IncreasePressure-Method.js | C:\temp\devicemodels\scripts\IncreasePressure-Method.js |

1. Open de **C:\temp\devicemodels\chiller-01.json** bestand.

1. Update de **SchemaVersion** waarde als volgt:

    ```json
    "SchemaVersion": "1.0.0",
    ```

1. In de **InitialState** sectie, voegt u de volgende twee definities:

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

1. Sla de **C:\temp\devicemodels\chiller-01.json** bestand.

1. Open de **C:\temp\devicemodels\scripts\chiller-01-state.js** bestand.

1. De volgende velden toevoegen aan de **status** variabele:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Update de **belangrijkste** werkt als volgt:

    ```js
    function main(context, previousState, previousProperties) {

        // Restore the global state before generating the new telemetry, so that
        // the telemetry can apply changes using the previous function state.
        restoreSimulation(previousState, previousProperties);

        // 75F +/- 5%,  Min 25F, Max 100F
        state.temperature = vary(75, 5, 25, 100);

        // 70% +/- 5%,  Min 2%, Max 99%
        state.humidity = vary(70, 5, 2, 99);

        // 65F +/- 2%,  Min 15F, Max 125F
        state.internal_temperature = vary(65, 2, 15, 125);

        log("Simulation state: " + state.simulation_state);
        if (state.simulation_state === "high_pressure") {
            // 250 psig +/- 25%,  Min 50 psig, Max 300 psig
            state.pressure = vary(250, 25, 50, 300);
        } else {
            // 150 psig +/- 10%,  Min 50 psig, Max 300 psig
            state.pressure = vary(150, 10, 50, 300);
        }

        updateState(state);
        return state;
    }
    ```

1. Sla de **C:\temp\devicemodels\scripts\chiller-01-state.js** bestand.

## <a name="create-the-lightbulb"></a>De gloeilamp maken

In deze sectie definieert u een nieuwe **gloeilamp** apparaattype:

1. Maak een bestand **C:\temp\devicemodels\lightbulb-01.json** en voeg de volgende inhoud:

    ```json
    {
      "SchemaVersion": "1.0.0",
      "Id": "lightbulb-01",
      "Version": "0.0.1",
      "Name": "Lightbulb",
      "Description": "Smart lightbulb device.",
      "Protocol": "MQTT",
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
      "Properties": {
        "Type": "Lightbulb",
        "Color": "White",
        "Brightness": 75,
        "EstimatedRemainingLife": 10000
      },
      "Tags": {
        "Location": "Building 2",
        "Floor": "2",
        "Campus": "Redmond"
      },
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
    }
    ```

    Sla de wijzigingen aan **C:\temp\devicemodels\lightbulb-01.json**.

1. Maak een bestand **C:\temp\devicemodels\scripts\lightbulb-01-state.js** en voeg de volgende inhoud:

    ```javascript
    "use strict";

    // Default state
    var state = {
      online: true,
      temperature: 200.0,
      temperature_unit: "F",
      status: "on"
    };

    // Default device properties
    var properties = {};

    /**
     * Restore the global state using data from the previous iteration.
     *
     * @param previousState device state from the previous iteration
     * @param previousProperties device properties from the previous iteration
     */
    function restoreSimulation(previousState, previousProperties) {
      // If the previous state is null, force a default state
      if (previousState) {
        state = previousState;
      } else {
        log("Using default state");
      }

      if (previousProperties) {
        properties = previousProperties;
      } else {
        log("Using default properties");
      }
    }

    /**
     * Simple formula generating a random value around the average
     * in between min and max
     *
     * @returns random value with given parameters
     */
    function vary(avg, percentage, min, max) {
      var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
      value = Math.max(value, min);
      value = Math.min(value, max);
      return value;
    }

    /**
     * Simple formula that sometimes flips the status of the lightbulb
     */
    function flip(value) {
      if (Math.random() < 0.2) {
        return (value == "on") ? "off" : "on"
      }
      return value;
    }

    /**
     * Entry point function called by the simulation engine.
     * Returns updated simulation state.
     * Device property updates must call updateProperties() to persist.
     *
     * @param context             The context contains current time, device model and id
     * @param previousState       The device state since the last iteration
     * @param previousProperties  The device properties since the last iteration
     */
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

    Sla de wijzigingen aan **C:\temp\devicemodels\scripts\lightbulb-01-state.js**.

1. Maak een bestand **C:\temp\devicemodels\scripts\SwitchOn-method.js** en voeg de volgende inhoud:

    ```javascript
    "use strict";

    // Default state
    var state = {
      status: "on"
    };

    /**
     * Entry point function called by the method.
     *
     * @param context        The context contains current time, device model and id
     * @param previousState  The device state since the last iteration
     * @param previousProperties  The device properties since the last iteration
     */
    function main(context, previousState) {
      log("Executing lightbulb Switch On method.");
      state.status = "on";
      updateState(state);
    }
    ```

    Sla de wijzigingen aan **C:\temp\devicemodels\scripts\SwitchOn-method.js**.

1. Maak een bestand **C:\temp\devicemodels\scripts\SwitchOff-method.js** en voeg de volgende inhoud:

    ```javascript
    "use strict";

    // Default state
    var state = {
      status: "on"
    };

    /**
     * Entry point function called by the method.
     *
     * @param context        The context contains current time, device model and id
     * @param previousState  The device state since the last iteration
     * @param previousProperties  The device properties since the last iteration
     */
    function main(context, previousState) {
      log("Executing lightbulb Switch Off method.");
      state.status = "off";
      updateState(state);
    }
    ```

    Sla de wijzigingen aan **C:\temp\devicemodels\scripts\SwitchOff-method.js**.

U hebt nu een aangepaste versie van de **Koelunit** apparaattype en een nieuw gemaakt **gloeilamp** apparaattype.

## <a name="test-the-devices"></a>Apparaten testen

In deze sectie maakt testen u de typen apparaten die u hebt gemaakt in de vorige secties lokaal.

### <a name="run-the-device-simulation-microservice"></a>De simulatie apparaat microservice uitvoeren

Open de **remote-monitoring-services-dotnet-master\device-simulation** map die u hebt gedownload van GitHub in een nieuw exemplaar van Visual Studio Code. Klik op een **herstellen** knoppen om op te lossen dat alle afhankelijkheden niet opgelost.

Open de **.vscode/launch.json** bestands- en toewijzen van uw IoT Hub-verbindingsreeks voor de **PCS_IOTHUB_CONNSTRING** omgevingsvariabele.

Open de **WebService/Properties/launchSettings.json** bestands- en toewijzen van uw IoT Hub-verbindingsreeks voor de **PCS_IOTHUB_CONNSTRING** omgevingsvariabele.

Open de **WebService/appsettings.ini** -bestand en wijzig de instellingen als volgt te werk:

```ini
device_models_folder = C:\temp\devicemodels\

device_models_scripts_folder = C:\temp\devicemodels\scripts\
```

Als u wilt de microservices lokaal uitvoeren, klikt u op **fouten opsporen > Foutopsporing starten**.

De **Terminal** venster in Visual Studio Code ziet u uitvoer van de actieve microservice.

Laat u het apparaat simulatie microservice in dit exemplaar van Visual Studio Code wordt uitgevoerd terwijl u de volgende stappen uitvoeren.

### <a name="set-up-a-monitor-for-device-events"></a>Instellen van een monitor voor apparaatgebeurtenissen

In deze sectie maakt u de Azure CLI gebruiken voor het instellen van een van de monitor de telemetrie die is verzonden vanaf de apparaten die zijn verbonden met uw IoT-hub bekijken.

Het volgende script wordt ervan uitgegaan dat de naam van uw IoT-hub **apparaat-simulatie-test**.

```azurecli-interactive
# Install the IoT extension if it's not already installed
az extension add --name azure-cli-iot-ext

# Monitor telemetry sent to your hub
az iot hub monitor-events --hub-name device-simulation-test
```

Laat het controleprogramma voor gebeurtenissen uitgevoerd terwijl u de gesimuleerde apparaten testen.

### <a name="create-a-simulation-with-the-updated-chiller-device-type"></a>Een simulatie maken met het bijgewerkte Koelunit apparaattype

In deze sectie kunt u het hulpprogramma Postman gebruiken om aan te vragen van het apparaat simulatie microservice om uit te voeren een simulatie met behulp van de bijgewerkte Koelunit apparaattype. Postman is een hulpprogramma waarmee u de REST-aanvragen verzenden naar een webservice. De Postman-configuratiebestanden die u moet zich in de lokale kopie van de **apparaat-simulatie-dotnet** opslagplaats.

Postman instellen:

1. Postman op uw lokale computer openen.

1. Klik op **bestand > importeren**. Klik vervolgens op **bestanden kiezen**.

1. Navigeer naar de **apparaat-simulatie-dotnet/docs/postman** map. Selecteer **Apparaatsimulatie voor Azure IoT-oplossing accelerator.postman_collection** en **Apparaatsimulatie voor Azure IoT-oplossing accelerator.postman_environment** en klikt u op **openen**.

1. Vouw de **Apparaatsimulatie voor Azure IoT-oplossingsversnellers** op de aanvragen die u kunt verzenden.

1. Klik op **geen omgeving** en selecteer **Apparaatsimulatie voor Azure IoT-oplossingsversnellers**.

U hebt nu een verzameling en geladen in de Postman-werkruimte die u gebruiken kunt om te communiceren met de apparaat-simulatie microservice-omgeving.

Configureren en de simulatie uitvoeren:

1. Selecteer in de Postman-verzameling **maken gewijzigd Koelunit simulatie** en klikt u op **verzenden**. Deze aanvraag maakt vier instanties van het type Koelunit gesimuleerde apparaat.

1. De uitvoer van de monitor gebeurtenis in de Azure CLI-venster ziet u de telemetrie van de gesimuleerde apparaten, met inbegrip van de nieuwe **internal_temperature** waarden.

Als u wilt de simulatie stoppen, selecteert u de **simulatie stoppen** aanvraag in Postman en klik op **verzenden**.

### <a name="create-a-simulation-with-the-lightbulb-device-type"></a>Maak een simulatie met het apparaattype gloeilamp

In deze sectie kunt u het hulpprogramma Postman gebruiken om aan te vragen van het apparaat simulatie microservice om uit te voeren een simulatie met behulp van het apparaattype gloeilamp. Postman is een hulpprogramma waarmee u de REST-aanvragen verzenden naar een webservice.

Configureren en de simulatie uitvoeren:

1. Selecteer in de Postman-verzameling **gloeilamp simulatie** en klikt u op **verzenden**. Deze aanvraag maakt twee exemplaren van het type gloeilamp gesimuleerde apparaat.

1. De uitvoer van de monitor gebeurtenis in de Azure CLI-venster ziet u de telemetrie van de gesimuleerde lightbulbs.

Als u wilt de simulatie stoppen, selecteert u de **simulatie stoppen** aanvraag in Postman en klik op **verzenden**.

## <a name="clean-up-resources"></a>Resources opschonen

Kunt u voorkomen dat de twee lokaal uitgevoerde microservices in hun Visual Studio Code-exemplaren (**fouten opsporen > foutopsporing stoppen**).

Als u de IoT Hub en Cosmos DB-instanties niet meer nodig hebt, verwijdert u deze uit uw Azure-abonnement om alle onnodige kosten te voorkomen.