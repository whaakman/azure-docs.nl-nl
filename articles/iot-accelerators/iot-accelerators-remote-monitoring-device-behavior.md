---
title: Gesimuleerd apparaat gedrag in de oplossing voor externe controle - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u JavaScript gebruiken om te bepalen het gedrag van een gesimuleerd apparaat in de oplossing voor externe controle.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 01/29/2018
ms.topic: conceptual
ms.openlocfilehash: 04d2ad2f0e86ee977600af86a2ffd1e9d7680375
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65823421"
---
# <a name="implement-the-device-model-behavior"></a>Gedrag van het Apparaatmodel implementeren

Het artikel [schema van het Apparaatmodel begrijpen](iot-accelerators-remote-monitoring-device-schema.md) beschreven van het schema dat u een gesimuleerde apparaat-model definieert. Dit artikel waarnaar wordt verwezen naar twee typen van JavaScript-bestand die het gedrag van een gesimuleerd apparaat implementeren:

- **Status** JavaScript-bestanden die worden uitgevoerd met vaste intervallen om bij te werken van de interne status van het apparaat.
- **Methode** JavaScript-bestanden die worden uitgevoerd wanneer de oplossing een methode op het apparaat roept.

> [!NOTE]
> Model apparaatgedrag zijn alleen voor de gesimuleerde apparaten die worden gehost in de device simulatie-service. Als u maken van een echt apparaat wilt, Zie [uw apparaat aansluiten op de oplossingsverbetering voor externe controle](iot-accelerators-connecting-devices.md).

In dit artikel leert u het volgende:

>[!div class="checklist"]
> * Beheren van de status van een gesimuleerd apparaat
> * Definiëren hoe een gesimuleerd apparaat reageert op een methodeaanroep van de oplossing voor externe controle
> * Fouten opsporen in uw scripts

## <a name="state-behavior"></a>Status gedrag

De [simulatie](../../articles/iot-accelerators/iot-accelerators-remote-monitoring-device-schema.md#simulation) gedeelte van het modelschema apparaat definieert de interne status van een gesimuleerd apparaat:

- `InitialState` initiële waarden voor alle eigenschappen van het apparaat staat object definieert.
- `Script` een JavaScript-bestand dat wordt uitgevoerd volgens een schema om bij te werken van de apparaatstatus wordt geïdentificeerd.

Het volgende voorbeeld bevat de definitie van het apparaat staat-object voor een gesimuleerde Koelunit-apparaat:

```json
"Simulation": {
  "InitialState": {
    "online": true,
    "temperature": 75.0,
    "temperature_unit": "F",
    "humidity": 70.0,
    "humidity_unit": "%",
    "pressure": 150.0,
    "pressure_unit": "psig",
    "simulation_state": "normal_pressure"
  },
  "Interval": "00:00:05",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

De status van het gesimuleerde apparaat, zoals gedefinieerd in de `InitialState` sectie, door de service simulatie in het geheugen worden bewaard. De statusinformatie wordt doorgegeven als invoer voor de `main` gedefinieerd in de functie **Koelunit-01-state.js**. In dit voorbeeld wordt de simulatie-service wordt uitgevoerd de **Koelunit-01-state.js** bestand om de vijf seconden. Het script kunt de status van het gesimuleerde apparaat te wijzigen.

Hieronder ziet u de omtrek van een typische `main` functie:

```javascript
function main(context, previousState, previousProperties) {

  // Use the previous device state to
  // generate the new device state
  // returned by the function.

  return state;
}
```

De `context` parameter heeft de volgende eigenschappen:

- `currentTime` Als een tekenreeks met de indeling `yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, bijvoorbeeld `Simulated.Chiller.123`
- `deviceModel`, bijvoorbeeld `Chiller`

De `state` parameter bevat de status van het apparaat beheerd door het apparaat simuleren-service. Deze waarde is de `state` object dat wordt geretourneerd door de vorige aanroep aan `main`.

Het volgende voorbeeld toont een typische implementatie van de `main` methode voor het afhandelen van de status van het apparaat beheerd door de service simulatie:

```javascript
// Default state
var state = {
  online: true,
  temperature: 75.0,
  temperature_unit: "F",
  humidity: 70.0,
  humidity_unit: "%",
  pressure: 150.0,
  pressure_unit: "psig",
  simulation_state: "normal_pressure"
};

function restoreState(previousState) {
  // If the previous state is null, force a default state
  if (previousState !== undefined && previousState !== null) {
      state = previousState;
  } else {
      log("Using default state");
  }
}

function main(context, previousState, previousProperties) {

  restoreState(previousState);

  // Update state

  return state;
}
```

Het volgende voorbeeld ziet u hoe de `main` methode telemetriewaarden die na verloop van tijd variëren kunt simuleren:

```javascript
/**
 * Simple formula generating a random value
 * around the average and between min and max
 */
function vary(avg, percentage, min, max) {
  var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
  value = Math.max(value, min);
  value = Math.min(value, max);
  return value;
}


function main(context, previousState, previousProperties) {

    restoreState(previousState);

    // 75F +/- 5%,  Min 25F, Max 100F
    state.temperature = vary(75, 5, 25, 100);

    // 70% +/- 5%,  Min 2%, Max 99%
    state.humidity = vary(70, 5, 2, 99);

    log("Simulation state: " + state.simulation_state);
    if (state.simulation_state === "high_pressure") {
        // 250 psig +/- 25%,  Min 50 psig, Max 300 psig
        state.pressure = vary(250, 25, 50, 300);
    } else {
        // 150 psig +/- 10%,  Min 50 psig, Max 300 psig
        state.pressure = vary(150, 10, 50, 300);
    }

    return state;
}
```

U vindt de volledige [Koelunit-01-state.js](https://github.com/Azure/device-simulation-dotnet/blob/master/Services/data/devicemodels/scripts/chiller-01-state.js) op GitHub.

## <a name="method-behavior"></a>Werking van de methode

De [CloudToDeviceMethods](../../articles/iot-accelerators/iot-accelerators-remote-monitoring-device-schema.md#cloudtodevicemethods) gedeelte van het modelschema apparaat definieert de methoden die een gesimuleerd apparaat reageert op.

Het volgende voorbeeld ziet u de lijst met methoden die worden ondersteund door een gesimuleerde Koelunit-apparaat:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
  },
  "FirmwareUpdate": {
    "Type": "javascript",
    "Path": "FirmwareUpdate-method.js"
  },
  "EmergencyValveRelease": {
    "Type": "javascript",
    "Path": "EmergencyValveRelease-method.js"
  },
  "IncreasePressure": {
    "Type": "javascript",
    "Path": "IncreasePressure-method.js"
  }
}
```

Elke methode heeft een bijbehorende JavaScript-bestand waarmee het gedrag van de methode.

De status van het gesimuleerde apparaat, zoals gedefinieerd in de `InitialState` sectie van het schema wordt bewaard in het geheugen door de service simulatie. De statusinformatie wordt doorgegeven als invoer voor de `main` functie gedefinieerd in het JavaScript-bestand als de methode wordt aangeroepen. Het script kunt de status van het gesimuleerde apparaat te wijzigen.

Hieronder ziet u de omtrek van een typische `main` functie:

```javascript
function main(context, previousState, previousProperties) {

}
```

De `context` parameter heeft de volgende eigenschappen:

- `currentTime` Als een tekenreeks met de indeling `yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, bijvoorbeeld `Simulated.Chiller.123`
- `deviceModel`, bijvoorbeeld `Chiller`

De `state` parameter bevat de status van het apparaat beheerd door het apparaat simuleren-service.

De `properties` parameter bevat de eigenschappen van het apparaat die is geschreven als gerapporteerde eigenschappen naar de apparaatdubbel IoT-Hub.

Er zijn drie algemene functies die u gebruiken kunt bij het implementeren van het gedrag van de methode:

- `updateState` bijwerken van de status van de simulatie-service.
- `updateProperty` bijwerken van één apparaateigenschap.
- `sleep` om te worden uitgevoerd om te simuleren van een langlopende taak onderbreken.

Het volgende voorbeeld ziet u een verkorte versie van de **IncreasePressure method.js** script dat wordt gebruikt door de gesimuleerde Koelunit-apparaten:

```javascript
function main(context, previousState, previousProperties) {

    log("Starting 'Increase Pressure' method simulation (5 seconds)");

    // Pause the simulation and change the simulation mode so that the
    // temperature will fluctuate at ~250 when it resumes
    var state = {
        simulation_state: "high_pressure",
        CalculateRandomizedTelemetry: false
    };
    updateState(state);

    // Increase
    state.pressure = 210;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Increase
    state.pressure = 250;
    updateState(state);
    log("Pressure increased to " + state.pressure);
    sleep(1000);

    // Resume the simulation
    state.CalculateRandomizedTelemetry = true;
    updateState(state);

    log("'Increase Pressure' method simulation completed");
}
```

## <a name="debugging-script-files"></a>Foutopsporing scriptbestanden

Het is niet mogelijk een foutopsporingsprogramma koppelen aan de Javascript-interpreter gebruikt door de service van de simulatie apparaat staat en de methode scripts uit te voeren. U kunt echter gegevens vastleggen in logboek voor de service. De ingebouwde `log()` functie kunt u gegevens wilt bijhouden en fouten opsporen in een functie wordt uitgevoerd op te slaan.

Als er een syntaxisfout de interpreter mislukt is, en schrijft een `Jint.Runtime.JavaScriptException` vermelding in het servicelogboek.

De [waarop de service lokaal](https://github.com/Azure/device-simulation-dotnet#running-the-service-locally-eg-for-development-tasks) artikel op GitHub leest u hoe de simulatie device service lokaal uitvoeren. De service lokaal uitgevoerde gemakkelijker fouten opsporen in uw gesimuleerde apparaten voordat u deze naar de cloud implementeren.

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt beschreven hoe u definieert het gedrag van uw eigen aangepaste gesimuleerde apparaat-model. In dit artikel hebt u geleerd hoe aan:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Beheren van de status van een gesimuleerd apparaat
> * Definiëren hoe een gesimuleerd apparaat reageert op een methodeaanroep van de oplossing voor externe controle
> * Fouten opsporen in uw scripts

Nu dat u hebt geleerd om op te geven van het gedrag van een gesimuleerd apparaat, de voorgestelde volgende stap is te leren hoe u [een gesimuleerd apparaat maakt](iot-accelerators-remote-monitoring-create-simulated-device.md).

Zie voor meer informatie voor ontwikkelaars over de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->
