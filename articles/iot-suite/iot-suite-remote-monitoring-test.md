---
title: De simulatie apparaat in de oplossing voor externe controle - Azure | Microsoft Docs
description: Deze zelfstudie laat zien hoe de apparaatsimulator gebruiken met de vooraf geconfigureerde oplossing voor externe controle.
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 0e6cc412fdb3ea7b9d8291b9f963e6412ae994a9
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="test-your-solution-with-simulated-devices"></a>Testen van uw oplossing met gesimuleerde apparaten

Deze zelfstudie leert u hoe u het apparaat simulator microservice met de vooraf geconfigureerde oplossing voor externe controle aanpassen. Om weer te geven de mogelijkheden van de apparaatsimulator dat twee scenario's in de toepassing Contoso IoT maakt gebruik van deze zelfstudie.

In het eerste scenario wil Contoso voor het testen van een nieuw smart gloeilamp-apparaat. Als u wilt de tests uitvoert, kunt u een nieuw gesimuleerd apparaat maken met de volgende kenmerken:

*Eigenschappen*

| Naam                     | Waarden                      |
| ------------------------ | --------------------------- |
| Kleur                    | Wit, rood, blauw            |
| Helderheid               | 0 en 100                    |
| Geschatte resterende levensduur | Aftelling van 10.000 uur |

*Telemetrie*

De volgende tabel ziet u de gegevens dat de ligthbulb rapporten naar de cloud als een gegevensstroom:

| Naam   | Waarden      |
| ------ | ----------- |
| Status | 'on', 'off' |
| Online | True, false |

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

In het tweede scenario, voegt u toe een nieuw type telemetrie naar Contoso de bestaande **Koelunit** apparaat.

Deze zelfstudie laat zien hoe u de apparaatsimulator gebruiken met de vooraf geconfigureerde oplossing voor externe controle:

In deze zelfstudie leert u het volgende:

>[!div class="checklist"]
> * Maak een nieuwe apparaattype
> * Aangepast apparaat gedrag simuleren
> * Een nieuw apparaattype toevoegen aan het dashboard
> * Aangepaste telemetrie verzenden vanuit een bestaand apparaattype

## <a name="prerequisites"></a>Vereisten

Volg deze zelfstudie, moet u een geïmplementeerd exemplaar van de oplossing voor externe controle in uw Azure-abonnement.

Als u de oplossing voor externe controle nog niet hebt geïmplementeerd, maar u moet voltooien de [implementeren van de vooraf geconfigureerde oplossing voor externe controle](iot-suite-remote-monitoring-deploy.md) zelfstudie.

<!-- Dominic please this use as your reference https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models -->

## <a name="the-device-simulation-service"></a>De simulatie device service

De simulatie-service van het apparaat in de vooraf geconfigureerde oplossing kunt u wijzigingen aanbrengen in de ingebouwde gesimuleerde apparaattypen en maak nieuwe typen van het gesimuleerde apparaat. Aangepaste apparaattypen kunt u het gedrag van de oplossing voor externe controle te testen voordat u uw fysieke apparaten op de oplossing aansluit.

## <a name="create-a-simulated-device-type"></a>Een type gesimuleerd apparaat maken

De eenvoudigste manier om een nieuwe apparaattype maken in de simulatie microservice is kopiëren en wijzigen van een bestaand type. De volgende stappen ziet u hoe u de ingebouwde kopieert **Koelunit** apparaat Maak een nieuwe **gloeilamp** apparaat:

1. Gebruik de volgende opdracht voor het klonen van de **apparaat simulatie** GitHub-opslagplaats met uw lokale machine:

    ```cmd/sh
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

1. Elk apparaattype heeft een JSON-modelbestand en de bijbehorende scripts in de `Services/data/devicemodels` map. Kopieer de **Koelunit** bestanden maken de **gloeilamp** bestanden zoals weergegeven in de volgende tabel:

    | Bron                      | Doel                   |
    | --------------------------- | ----------------------------- |
    | Koelunit 01.json             | gloeilamp 01.json             |
    | scripts/Koelunit-01-state.js | scripts/gloeilamp-01-state.js |
    | opnieuw opstarten-scripts-method.js    | SwitchOn-scripts-method.js    |

### <a name="define-the-characteristics-of-the-new-device-type"></a>De eigenschappen van het nieuwe apparaattype definiëren

De `lightbulb-01.json` -bestand definieert de kenmerken van het type, zoals de telemetrie wordt gegenereerd en de methoden ondersteunt. De volgende stappen uit update de `lightbulb-01.json` bestand voor het definiëren van de **gloeilamp** apparaat:

1. In de `lightbulb-01.json` bestand, het bijwerken van de metagegevens van apparaten zoals weergegeven in het volgende fragment:

    ```json
    "SchemaVersion": "1.0.0",
    "Id": "lightbulb-01",
    "Version": "0.0.1",
    "Name": "Lightbulb",
    "Description": "Smart lightbulb device.",
    "Protocol": "MQTT",
    ```

1. In de `lightbulb-01.json` bestand, de definitie van de simulatie bijwerken zoals weergegeven in het volgende fragment:

    ```json
    "Simulation": {
      "InitialState": {
        "online": true,
        "status": "on"
      },
      "Script": {
        "Type": "javascript",
        "Path": "lightbulb-01-state.js",
        "Interval": "00:00:20"
      }
    },
    ```

1. In de `lightbulb-01.json` bestand, de eigenschappen van het apparaat bijwerken zoals weergegeven in het volgende fragment:

    ```json
    "Properties": {
      "Type": "Lightbulb",
      "Color": "White",
      "Brightness": 75,
      "EstimatedRemainingLife": 10000
    },
    ```

1. In de `lightbulb-01.json` bestand, het apparaat type telemetrie-definities bijwerken zoals weergegeven in het volgende fragment:

    ```json
    "Telemetry": [
      {
        "Interval": "00:00:20",
        "MessageTemplate": "{\"status\":\"${status}\"}",
        "MessageSchema": {
          "Name": "lightbulb-status;v1",
          "Format": "JSON",
          "Fields": {
            "status": "text"
          }
        }
      }
    ],
    ```

1. In de `lightbulb-01.json` bestand, het bijwerken van de methoden voor het apparaat, zoals wordt weergegeven in het volgende fragment:

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

1. Sla de `lightbulb-01.json` bestand.

### <a name="simulate-custom-device-behavior"></a>Aangepast apparaat gedrag simuleren

De `scripts/lightbulb-01-state.js` -bestand definieert het gedrag van de simulatie van de **gloeilamp** type. De volgende stappen uit update de `scripts/lightbulb-01-state.js` bestand voor het definiëren van het gedrag van de **gloeilamp** apparaat:

1. Bewerk de definitie van de status in de `scripts/lightbulb-01-state.js` bestand, zoals wordt weergegeven in het volgende fragment:

    ```js
    // Default state
    var state = {
      online: true,
      status: "on"
    };
    ```

1. Vervang de **variëren** functie met de volgende **spiegelen** functie:

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

      // Make this flip every so often
      state.status = flip(state.status);

      return state;
    }
    ```

1. Sla de `scripts/lightbulb-01-state.js` bestand.

De `scripts/SwitchOn-method.js` bestand implementeert de **Switch op** methode in een **gloeilamp** apparaat. De volgende stappen uit update de `scripts/SwitchOn-method.js` bestand:

1. Bewerk de definitie van de status in de `scripts/SwitchOn-method.js` bestand, zoals wordt weergegeven in het volgende fragment:

    ```js
    var state = {
       status: "on"
    };
    ```

1. Als u wilt schakelen de gloeilamp, bewerken de **belangrijkste** werken als volgt:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch On method.");
        state.status = "on";
        updateState(state);
    }
    ```

1. Sla de `scripts/SwitchOn-method.js` bestand.

1. Een kopie maken de `scripts/SwitchOn-method.js` bestand met de naam `scripts/SwitchOff-method.js`.

1. Als u wilt de gloeilamp uitschakelen bewerken de **belangrijkste** werken in de `scripts/SwitchOff-method.js` als volgt:

    ```js
    function main(context, previousState) {
        log("Executing lightbulb Switch Off method.");
        state.status = "off";
        updateState(state);
    }
    ```

1. Sla de `scripts/SwitchOff-method.js` bestand.

### <a name="test-the-lightbulb-device-type"></a>Testen van het apparaattype gloeilamp

Voor het testen van de **gloeilamp** apparaattype, kunt u eerst testen uw apparaattype werkt zoals verwacht door het uitvoeren van een lokale kopie van de **apparaat simulatie** service. Wanneer u hebt getest en foutopsporing van uw nieuwe apparaattype lokaal, kunt u de container opnieuw maken en implementeren het **apparaat simulatie** service naar Azure.

Als u wilt testen en fouten opsporen in uw wijzigingen op een lokaal, Zie [apparaat simulatie overzicht](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md).

Configureren van het project voor het kopiëren van de nieuwe **gloeilamp** apparaatbestanden naar de uitvoermap:

* Als u Visual Studio gebruikt, controleert u of u de drie nieuwe gloeilamp bestanden u hebt gemaakt in de vorige sectie toevoegen de **Services** project in de oplossing. Gebruik vervolgens **Solution explorer** ze kunnen worden gekopieerd naar de uitvoermap markeren.

* Als u Visual Studio Code gebruikt, opent u de **Services.csproj** -bestand en voeg de drie nieuwe gloeilamp bestanden u in de vorige sectie hebt gemaakt. Zie het bestaande model bestand apparaatvermeldingen in de **Services.csproj** bestand als voorbeelden.

Test het nieuwe apparaat in een oplossing geïmplementeerd, ziet u een van:

* [Implementatie van de containers uit aangepaste docker-hub-account](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account)
* [Bijwerken van een geïmplementeerde container via handmatige kopiëren](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy)

Op de **apparaten** pagina kunt u exemplaren van het type van uw nieuwe inrichten:

![De lijst met beschikbare simulaties weergeven](media/iot-suite-remote-monitoring-test/devicesmodellist.png)

U kunt de telemetrie van het gesimuleerde apparaat bekijken:

![Telemetrie van paginaweergaven gloeilamp](media/iot-suite-remote-monitoring-test/devicestelemetry.png)

U kunt aanroepen de **SwitchOn** en **SwitchOff** methoden op uw apparaat:

![Gloeilamp methoden aanroepen](media/iot-suite-remote-monitoring-test/devicesmethods.png)

Een Docker-installatiekopie bouwen met het nieuwe apparaattype voor implementatie naar Azure, Zie [bouwen van een aangepaste installatiekopie met Docker](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image).

## <a name="add-a-new-telemetry-type"></a>Een nieuw telemetrie-type toevoegen

Deze sectie beschrijft het wijzigen van een bestaand type gesimuleerd apparaat ter ondersteuning van een nieuw telemetrie-type.

### <a name="locate-the-chiller-device-type-files"></a>Zoek de Koelunit apparaat type bestanden

De volgende stappen ziet u hoe u de bestanden die definiëren van de ingebouwde vinden **Koelunit** apparaat:

1. Als u dit nog niet hebt gedaan, gebruik de volgende opdracht voor het klonen van de **apparaat simulatie** GitHub-opslagplaats met uw lokale machine:

    ```cmd/sh
    git clone https://github.com/Azure/device-simulation-dotnet.git
    ```

1. Elk apparaattype heeft een JSON-modelbestand en de bijbehorende scripts in de `Services/data/devicemodels` map. De bestanden die de gesimuleerde definiëren **Koelunit** apparaattype zijn:
    * `Services/data/devicemodels/chiller-01.json`
    * `Services/data/devicemodels/scripts/chiller-01-state.js`

### <a name="specify-the-new-telemetry-type"></a>Geef het nieuwe telemetrie-type

De volgende stappen ziet u het toevoegen van een nieuwe **interne temperatuur** type de **Koelunit** apparaattype:

1. Open het `chiller-01.json`-bestand.

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

1. Sla de `chiller-01.json` bestand.

1. Open het `scripts/chiller-01-state.js`-bestand.

1. De volgende velden toevoegen aan de **status** variabele:

    ```js
    internal_temperature: 65.0,
    internal_temperature_unit: "F",
    ```

1. Voeg de volgende regel om de **belangrijkste** functie:

    ```js
    state.internal_temperature = vary(65, 2, 15, 125);
    ```

1. Sla de `scripts/chiller-01-state.js` bestand.

### <a name="test-the-chiller-device-type"></a>Het type van het apparaat Koelunit testen

Voor het testen van de bijgewerkte **Koelunit** apparaattype, kunt u eerst testen uw apparaattype werkt zoals verwacht door het uitvoeren van een lokale kopie van de **apparaat simulatie** service. Wanneer u hebt getest en foutopsporing van uw bijgewerkte apparaattype lokaal, kunt u de container opnieuw maken en implementeren het **apparaat simulatie** service naar Azure.

Bij het uitvoeren van de **apparaat simulatie** service lokaal het telemetrie verzendt naar uw oplossing voor externe controle. Op de **apparaten** pagina kunt u exemplaren van het type van uw bijgewerkte inrichten.

Als u wilt testen en fouten opsporen in uw wijzigingen op een lokaal, Zie [waarop de service wordt uitgevoerd met Visual Studio](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#running-the-service-with-visual-studio) of [bouwen en uitvoeren vanaf de opdrachtregel](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#build-and-run-from-the-command-line).

Test het nieuwe apparaat in een oplossing geïmplementeerd, ziet u een van:

* [Implementatie van de containers uit aangepaste docker-hub-account](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#deploying-containers-from-custom-docker-hub-account)
* [Bijwerken van een geïmplementeerde container via handmatige kopiëren](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#update-a-deployed-container-via-manual-copy)

Op de **apparaten** pagina kunt u exemplaren van het type van uw bijgewerkte inrichten:

![Bijgewerkte Koelunit toevoegen](media/iot-suite-remote-monitoring-test/devicesupdatedchiller.png)

U vindt de nieuwe **interne temperatuur** telemetrie van het gesimuleerde apparaat.

Een Docker-installatiekopie bouwen met het nieuwe apparaattype voor implementatie naar Azure, Zie [bouwen van een aangepaste installatiekopie met Docker](https://github.com/Azure/device-simulation-dotnet/blob/master/README.md#building-a-customized-docker-image).

## <a name="next-steps"></a>Volgende stappen

Deze zelfstudie hebt u geleerd hoe naar:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Maak een nieuwe apparaattype
> * Aangepast apparaat gedrag simuleren
> * Een nieuw apparaattype toevoegen aan het dashboard
> * Aangepaste telemetrie verzenden vanuit een bestaand apparaattype

Nu u hebt geleerd hoe u de simulatie apparaat service, de voorgestelde volgende stap is te leren hoe u [een fysiek apparaat aansluit op uw oplossing voor externe controle](iot-suite-connecting-devices-node.md).

Zie voor meer informatie voor ontwikkelaars over de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->