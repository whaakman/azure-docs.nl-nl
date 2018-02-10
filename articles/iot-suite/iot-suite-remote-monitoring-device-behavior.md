---
title: Gedrag van het apparaat in de oplossing voor externe controle - Azure gesimuleerde | Microsoft Docs
description: "Dit artikel wordt beschreven hoe u JavaScript gebruikt voor het definiëren van het gedrag van een gesimuleerd apparaat in de oplossing voor externe controle."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/29/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: e5846893166c3e65b75e84d02849c2b8ab78e079
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="implement-the-device-model-behavior"></a>De model-gedrag van het apparaat implementeren

Het artikel [inzicht in het modelschema apparaat](iot-suite-remote-monitoring-device-schema.md) beschreven van het schema dat u een model gesimuleerde apparaat definieert. Dit artikel bedoelde twee soorten JavaScript-bestand die het gedrag van een gesimuleerd apparaat implementeren:

- **Status** JavaScript-bestanden die worden uitgevoerd met vaste intervallen bijwerken van de interne status van het apparaat.
- **Methode** JavaScript-bestanden die worden uitgevoerd wanneer de oplossing wordt aangeroepen voor een methode op het apparaat.

In dit artikel leert u hoe:

>[!div class="checklist"]
> * De status van een gesimuleerd apparaat beheren
> * Definieer hoe een gesimuleerd apparaat Nee klikt op een methode aanroepen vanuit de oplossing voor externe controle
> * Fouten opsporen in uw scripts

## <a name="state-behavior"></a>Status gedrag

De [simulatie](iot-suite-remote-monitoring-device-schema.md#simulation) sectie van het modelschema apparaat definieert de interne status van een gesimuleerd apparaat:

- `InitialState`initiële waarden voor alle eigenschappen van het apparaat statusobject definieert.
- `Script`identificeert een JavaScript-bestand dat wordt uitgevoerd op een planning voor het bijwerken van de apparaatstatus.

Het volgende voorbeeld toont de definitie van het apparaat statusobject voor een gesimuleerde Koelunit apparaat:

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
  "Script": {
    "Type": "javascript",
    "Path": "chiller-01-state.js",
    "Interval": "00:00:05"
  }
}
```

De status van het gesimuleerde apparaat, zoals gedefinieerd in de `InitialState` sectie, in het geheugen wordt vastgehouden door de simulatie-service. De statusinformatie wordt als invoer doorgegeven aan de `main` gedefinieerd in de functie **Koelunit-01-state.js**. In dit voorbeeld wordt de simulatie-service wordt uitgevoerd de **Koelunit-01-state.js** bestand elke vijf seconden. Het script kunt wijzigen van de status van het gesimuleerde apparaat.

Hieronder vindt u de omtrek van een typische `main` functie:

```javascript
function main(context, previousState) {

  // Use the previous device state to
  // generate the new device state
  // returned by the function.

  return state;
}
```

De `context` parameter heeft de volgende eigenschappen:

- `currentTime`Als een tekenreeks met de indeling`yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, bijvoorbeeld`Simulated.Chiller.123`
- `deviceModel`, bijvoorbeeld`Chiller`

De `state` parameter bevat de status van het apparaat beheerd door de simulatie-service van het apparaat. Deze waarde is de `state` object dat wordt geretourneerd door de vorige aanroep aan `main`.

Het volgende voorbeeld toont een typische implementatie van de `main` methode voor het afhandelen van de status van het apparaat beheerd door de simulatie-service:

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

function main(context, previousState) {

  restoreState(previousState);

  // Update state

  return state;
}
```

Het volgende voorbeeld ziet u hoe de `main` methode kunt simuleren telemetrie-waarden die gedurende een bepaalde periode variëren:

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


function main(context, previousState) {

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

De [CloudToDeviceMethods](iot-suite-remote-monitoring-device-schema.md#cloudtodevicemethods) sectie van het modelschema apparaat definieert de methoden die een gesimuleerd apparaat reageert op.

Het volgende voorbeeld ziet u de lijst met methoden die worden ondersteund door een gesimuleerde Koelunit apparaat:

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

Elke methode heeft een bijbehorende JavaScript-bestand dat het gedrag van de methode implementeert.

De status van het gesimuleerde apparaat, zoals gedefinieerd in de `InitialState` sectie van het schema wordt bewaard in het geheugen door de simulatie-service. De statusinformatie wordt als invoer doorgegeven aan de `main` functie gedefinieerd in het JavaScript-bestand wanneer de methode wordt aangeroepen. Het script kunt wijzigen van de status van het gesimuleerde apparaat.

Hieronder vindt u de omtrek van een typische `main` functie:

```javascript
function main(context, previousState) {

}
```

De `context` parameter heeft de volgende eigenschappen:

- `currentTime`Als een tekenreeks met de indeling`yyyy-MM-dd'T'HH:mm:sszzz`
- `deviceId`, bijvoorbeeld`Simulated.Chiller.123`
- `deviceModel`, bijvoorbeeld`Chiller`

De `state` parameter bevat de status van het apparaat beheerd door de simulatie-service van het apparaat.

Er zijn twee algemene functies die kunt u het gedrag van de methode implementeren:

- `updateState`de status waarover de simulatie-service bijgewerkt.
- `sleep`kan worden uitgevoerd om te simuleren een langlopende taak onderbreken.

Het volgende voorbeeld ziet u een verkorte versie van de **IncreasePressure method.js** script gebruikt door de Koelunit gesimuleerde apparaten:

```javascript
function main(context, previousState) {

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

Het is niet mogelijk een foutopsporingsprogramma koppelen aan de Javascript-interpreter gebruikt door de service van de simulatie apparaat status en de methode scripts uitvoeren. U kunt echter gegevens vastleggen in logboek voor de service. De ingebouwde `log()` functie kunt u gegevens wilt bijhouden en fouten opsporen in een functie wordt uitgevoerd op te slaan.

Als er een syntaxisfout de interpreter mislukt, en schrijft een `Jint.Runtime.JavaScriptException` item naar logboek voor de service.

De [een gesimuleerd apparaat maakt](iot-suite-remote-monitoring-test.md) artikel ziet u hoe de simulatie device service lokaal uitvoeren. De service lokaal wordt uitgevoerd, kunt gemakkelijker fouten opsporen in uw gesimuleerde apparaten voordat u ze naar de cloud implementeren.

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt beschreven hoe u definieert het gedrag van het model van uw eigen aangepaste gesimuleerde apparaat. In dit artikel hebt u geleerd hoe naar:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * De status van een gesimuleerd apparaat beheren
> * Definieer hoe een gesimuleerd apparaat Nee klikt op een methode aanroepen vanuit de oplossing voor externe controle
> * Fouten opsporen in uw scripts

Nu u hebt geleerd hoe u het gedrag van een gesimuleerd apparaat opgeeft, wordt de voorgestelde volgende stap is te leren hoe u [een gesimuleerd apparaat maakt](iot-suite-remote-monitoring-test.md).

Zie voor meer informatie voor ontwikkelaars over de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->