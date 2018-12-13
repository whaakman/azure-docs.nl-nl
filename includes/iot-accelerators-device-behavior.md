---
title: bestand opnemen
description: bestand opnemen
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 07/26/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: b0dd0c0c0c300b4db94c1ab22205f9e808556f0b
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53166824"
---
## <a name="state-behavior"></a>Status gedrag

De [simulatie](../articles/iot-accelerators/iot-accelerators-remote-monitoring-device-schema.md#simulation) gedeelte van het modelschema apparaat definieert de interne status van een gesimuleerd apparaat:

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

U vindt de volledige [Koelunit-01-state.js](https://github.com/Azure/device-simulation-dotnet/blob/master/Services/data/devicemodels/scripts/chiller-01-state.js) op Github.

## <a name="method-behavior"></a>Werking van de methode

De [CloudToDeviceMethods](../articles/iot-accelerators/iot-accelerators-remote-monitoring-device-schema.md#cloudtodevicemethods) gedeelte van het modelschema apparaat definieert de methoden die een gesimuleerd apparaat reageert op.

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