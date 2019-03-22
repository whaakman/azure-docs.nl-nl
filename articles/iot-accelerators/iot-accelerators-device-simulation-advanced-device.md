---
title: Maak een geavanceerde gesimuleerde apparaat-model - Azure | Microsoft Docs
description: In deze handleiding leert u hoe u een geavanceerde Apparaatmodel voor gebruik met de oplossingsverbetering voor Apparaatsimulatie maken.
author: troyhopwood
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.custom: mvc
ms.date: 03/18/2019
ms.author: troyhop
ms.openlocfilehash: 4401d4b93a27e76554368ce72d256b38de61df4c
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286122"
---
# <a name="create-an-advanced-device-model"></a>Maak een geavanceerde Apparaatmodel

In deze gebruiksaanwijzing wordt de JSON en JavaScript-bestanden die definiëren van een aangepast Apparaatmodel beschreven. Het artikel bevat enkele voorbeeldbestanden apparaat model definition en ziet u hoe u ze uploaden naar uw Apparaatsimulatie-exemplaar. U kunt geavanceerde modellen om te simuleren realistischer gedrag van apparaten voor het testen kunt maken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Als u wilt volgen de stappen in deze handleiding, moet u een geïmplementeerd exemplaar van Apparaatsimulatie in uw Azure-abonnement.

Als u Apparaatsimulatie nog niet hebt geïmplementeerd, moet u de snelstart [Een IoT-apparaatsimulatie in Azure implementeren en uitvoeren](quickstart-device-simulation-deploy.md) voltooien.

### <a name="open-device-simulation"></a>Apparaatsimulatie openen

Als u Apparaatsimulatie in uw browser wilt uitvoeren, gaat u eerst naar [Oplossingsverbeteringen van Microsoft Azure IoT](https://www.azureiotsolutions.com).

Mogelijk wordt u gevraagd u aan te melden met de referenties van uw Azure-abonnement.

Klik vervolgens op **starten** op de tegel voor Apparaatsimulatie die u hebt geïmplementeerd in de [implementeren en voer de simulatie van een IoT-apparaat in Azure](quickstart-device-simulation-deploy.md) Quick Start.

## <a name="device-models"></a>Apparaatmodellen

Elk gesimuleerd apparaat behoort tot een specifiek apparaat-model dat u het gedrag van de simulatie definieert. Dit gedrag bevat hoe vaak om telemetrie te verzenden, wat voor soort berichten te verzenden en de ondersteunde methoden.

U definieert een model van het apparaat met een definitiebestand van de JSON-apparaat en een scala van JavaScript-bestanden. Deze JavaScript-bestanden bepalen het gedrag van de simulatie, zoals de willekeurige Telemetrie en de methode logica.

Een typische Apparaatmodel heeft:

* Een JSON-bestand voor elk Apparaatmodel (bijvoorbeeld elevator.json).
* Een JavaScript gedrag scriptbestand voor elk Apparaatmodel (bijvoorbeeld, lift-state.js)
* Methode een JavaScript-script-bestand voor elke apparaatmethode (bijvoorbeeld, lift-go-down.js)

> [!NOTE]
> Niet alle apparaatmodellen definiëren methoden. Daarom een Apparaatmodel kan of kan geen methode scripts. Alle apparaatmodellen moeten echter een script gedrag hebben.

## <a name="device-definition-file"></a>Apparaat-definitiebestand

Elk apparaat definitie-bestand bevat de details van een gesimuleerde apparaat-model, met inbegrip van de volgende informatie:

* De naam van de apparaat-model: tekenreeks.
* Protocol: AMQP | MQTT | HTTP.
* De status van het eerste apparaat.
* Hoe vaak u wilt vernieuwen van de apparaatstatus.
* Welke JavaScript-bestand gebruiken om te vernieuwen van de apparaatstatus.
* Een lijst met berichten over telemetrie te verzenden, elk met een specifieke frequentie.
* Het schema van de berichten over telemetrie, gebruikt door de back-endtoepassing bij het parseren van de telemetrie die is ontvangen.
* Een lijst van ondersteunde methoden en de JavaScript-bestand moet worden gebruikt voor het simuleren van elke methode.

### <a name="file-schema"></a>Bestandsschema

De schemaversie is altijd '1.0.0' en is specifiek voor de indeling van dit bestand:

```json
"SchemaVersion": "1.0.0"
```

### <a name="device-model-description"></a>Beschrijving van het apparaat objectmodel

De volgende eigenschappen beschrijven het Apparaatmodel. Elk type heeft een unieke id, een semantische versie, een naam en een beschrijving:

```json
"Id": "chiller-01",
"Version": "0.0.1",
"Name": "Chiller",
"Description": "Chiller with external temperature and humidity sensors."
```

### <a name="iot-protocol"></a>IoT Protocol

IoT-apparaten kunnen verbinding maken met behulp van verschillende protocollen. De simulatie kunt u een **AMQP**, **MQTT**, of **HTTP**:

```json
"Protocol": "AMQP"
```

### <a name="simulated-device-state"></a>Status van het gesimuleerde apparaat

Elk gesimuleerd apparaat heeft een interne status, die moet worden gedefinieerd. De status bepaalt ook de eigenschappen die kunnen worden gerapporteerd in telemetrie. Bijvoorbeeld, een Koelunit beschikt mogelijk over een initiële status zoals:

```json
"InitialState": {
    "temperature": 50,
    "humidity": 40
},
```

Een bewegende apparaat met diverse sensoren mogelijk meer eigenschappen, bijvoorbeeld:

```json
"InitialState": {
    "latitude": 47.445301,
    "longitude": -122.296307,
    "speed": 30.0,
    "speed_unit": "mph",
    "cargotemperature": 38.0,
    "cargotemperature_unit": "F"
}
```

Status van het apparaat is opgeslagen in het geheugen door de service simulatie en opgegeven als invoer voor de JavaScript-functie. De JavaScript-functie kan bepalen:

* De status negeren en een willekeurige gegevens genereren.
* Bijwerken van de apparaatstatus in een realistische manier voor een bepaald scenario.

Er wordt ook de functie waarmee de status wordt gegenereerd als invoer:

* De apparaat-ID.
* Het Apparaatmodel.
* De huidige tijd. Deze waarde maakt het mogelijk is voor het genereren van verschillende gegevens door het apparaat en tijd.

### <a name="generating-telemetry-messages"></a>Berichten over telemetrie te genereren

De simulatie-service kan verschillende telemetrie die voor elk apparaat verzenden. Telemetrie bevat doorgaans gegevens uit de status van het apparaat. Een gesimuleerd lokaal kan bijvoorbeeld informatie over temperatuur en vochtigheid elke tien seconden te verzenden. Houd rekening met de tijdelijke aanduidingen in het volgende codefragment die automatisch worden vervangen door waarden van de Apparaatstatus:

```json
"Telemetry": [
    {
        "Interval": "00:00:10",
        "MessageTemplate":
            "{\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\",\"humidity\":\"${humidity}\"}",
        "MessageSchema": {
            "Name": "RoomComfort;v1",
            "Format": "JSON",
            "Fields": {
                "temperature": "double",
                "temperature_unit": "text",
                "humidity": "integer"
            }
        }
    }
],
```

De tijdelijke aanduidingen gebruikt u een speciale syntaxis **${NAME}** waar **naam** is een sleutel van het apparaat de status-object geretourneerd door de JavaScript **belangrijkste** functie. Tekenreeksen moeten worden vermeld, terwijl getallen mag niet.

#### <a name="message-schema"></a>Het schema

Elk berichttype moet een goed gedefinieerde schema hebben. Het berichtschema wordt ook gepubliceerd naar IoT-Hub, zodat back-end-toepassingen opnieuw de gegevens interpreteren van de binnenkomende telemetriegegevens kunnen gebruiken.

Het schema biedt ondersteuning voor JSON-indeling, kunt u eenvoudig parseren, transformatie en analytics, via verschillende systemen en services.

De velden die worden vermeld in het schema kunnen van de volgende typen zijn:

* Object - geserialiseerd met de JSON
* Binary - serialized using base64
* Tekst
* Booleaans
* Geheel getal
* Double
* DateTime

### <a name="supported-methods"></a>Ondersteunde methoden

Gesimuleerde apparaten kunnen ook reageren op methodeaanroepen, in welk geval ze uitvoeren van bepaalde logica en sommige antwoord geven. Op soortgelijke wijze aan de simulatie de logica van de methode wordt opgeslagen in een JavaScript-bestand, en kan communiceren met de status van het apparaat. Bijvoorbeeld:

```json
"CloudToDeviceMethods": {
    "Start": {
        "Type": "JavaScript",
        "Path": "release-pressure.js"
    }
}
```

## <a name="create-a-device-definition-file"></a>Maak een bestand van de definitie van apparaat

In deze procedure-naar-handleiding ziet u hoe u een Apparaatmodel voor een drone maken. De drone wordt willekeurig vliegen rond een eerste set met coördinaten wijzigen van de locatie en hoogte.

Kopieer de volgende JSON naar een teksteditor en sla het bestand als **drone.json**.

### <a name="device-definition-json-example"></a>Voorbeeld van JSON-definitie apparaat

```json
{
  "SchemaVersion": "1.0.0",
  "Id": "drone",
  "Version": "0.0.1",
  "Name": "Drone",
  "Description": "Simple drone.",
  "Protocol": "AMQP",
  "Simulation": {
    "InitialState": {
      "velocity": 0.0,
      "velocity_unit": "mm/sec",
      "acceleration": 0.0,
      "acceleration_unit": "mm/sec^2",
      "latitude": 47.476075,
      "longitude": -122.192026,
      "altitude": 0.0
    },
    "Interval": "00:00:05",
    "Scripts": [{
      "Type": "JavaScript",
      "Path": "drone-state.js"
    }]
  },
  "Properties": {
    "Type": "Drone",
    "Firmware": "1.0",
    "Model": "P-96"
  },
  "Tags": {
    "Owner": "Contoso"
  },
  "Telemetry": [{
      "Interval": "00:00:05",
      "MessageTemplate": "{\"velocity\":\"${velocity}\",\"acceleration\":\"${acceleration}\",\"position\":\"${latitude}|${longitude}|${altitude}\"}",
      "MessageSchema": {
        "Name": "drone-event-sensor;v1",
        "Format": "JSON",
        "Fields": {
          "velocity": "double",
          "velocity_unit": "text",
          "acceleration": "double",
          "acceleration_unit": "text",
          "latitude": "double",
          "longitude": "double",
          "altitude": "double"
        }
      }
    }
  ],
    "CloudToDeviceMethods": {
        "RecallDrone": {
            "Type": "JavaScript",
            "Path": "droneRecall-method.js"
        }
    }
}
```

## <a name="behavior-script-files"></a>Gedrag scriptbestanden

De code in het scriptbestand gedrag worden verplaatst van de drone zien. Het script wijzigt de uitbreiding en de locatie van de drone zien door te bewerken in de geheugenstatus van het apparaat.

De JavaScript-bestanden moeten hebben een **belangrijkste** -functie, die twee parameters worden geaccepteerd:

* Een **context** object dat drie eigenschappen bevat:
    * **currentTime** als een tekenreeks met de indeling **jjjj-MM-dd'T'HH:mm:sszzz**.
    * **deviceId**. Bijvoorbeeld, **Simulated.Elevator.123**.
    * **deviceModel**. Bijvoorbeeld, **lift**.
* Een **status** -object dat is de waarde die is geretourneerd door de functie in de vorige aanroep. Deze status van het apparaat wordt onderhouden door de service simulatie en gebruikt voor het genereren van berichten over telemetrie.

De **belangrijkste** functie geeft als resultaat de apparaatstatus van het nieuwe. Bijvoorbeeld:

```JavaScript
function main(context, state) {

    // Use context if the simulation depends on
    // time or device details.
    // Execute some logic, updating 'state'

    return state;
}
```

## <a name="create-a-behavior-script-file"></a>Een scriptbestand gedrag maken

Kopieer de volgende JavaScript in een teksteditor en sla deze als **drone state.js**.

### <a name="device-model-javascript-simulation-example"></a>Voorbeeld van apparaat model JavaScript-simulatie

```JavaScript
"use strict";

// Position control
const DefaultLatitude = 47.476075;
const DefaultLongitude = -122.192026;
const DistanceVariation = 10;

// Altitude control
const DefaultAltitude = 0.0;
const AverageAltitude = 499.99;
const AltitudeVariation = 5;

// Velocity control
const AverageVelocity = 60.00;
const MinVelocity = 20.00;
const MaxVelocity = 120.00;
const VelocityVariation = 5;

// Acceleration control
const AverageAcceleration = 2.50;
const MinAcceleration = 0.01;
const MaxAcceleration = 9.99;
const AccelerationVariation = 1;

// Display control for position and other attributes
const GeoSpatialPrecision = 6;
const DecimalPrecision = 2;

// Default state
var state = {
    velocity: 0.0,
    velocity_unit: "mm/sec",
    acceleration: 0.0,
    acceleration_unit: "mm/sec^2",
    latitude: DefaultLatitude,
    longitude: DefaultLongitude,
    altitude: DefaultAltitude
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
 */
function vary(avg, percentage, min, max) {
    var value = avg * (1 + ((percentage / 100) * (2 * Math.random() - 1)));
    value = Math.max(value, min);
    value = Math.min(value, max);
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
/*jslint unparam: true*/
function main(context, previousState, previousProperties) {

    // Restore the global state before generating the new telemetry, so that
    // the telemetry can apply changes using the previous function state.
    restoreSimulation(previousState, previousProperties);

    state.acceleration = vary(AverageAcceleration, AccelerationVariation, MinAcceleration, MaxAcceleration).toFixed(DecimalPrecision);
    state.velocity = vary(AverageVelocity, VelocityVariation, MinVelocity, MaxVelocity).toFixed(DecimalPrecision);

    // Use the last coordinates to calculate the next set with a given variation
    var coords = varylocation(Number(state.latitude), Number(state.longitude), DistanceVariation);
    state.latitude = Number(coords.latitude).toFixed(GeoSpatialPrecision);
    state.longitude = Number(coords.longitude).toFixed(GeoSpatialPrecision);

    // Fluctuate altitude between given variation constant by more or less
    state.altitude = vary(AverageAltitude, AltitudeVariation, AverageAltitude - AltitudeVariation, AverageAltitude + AltitudeVariation).toFixed(DecimalPrecision);

    return state;
}

/**
 * Generate a random geolocation at some distance (in miles)
 * from a given location
 */
function varylocation(latitude, longitude, distance) {
    // Convert to meters, use Earth radius, convert to radians
    var radians = (distance * 1609.344 / 6378137) * (180 / Math.PI);
    return {
        latitude: latitude + radians,
        longitude: longitude + radians / Math.cos(latitude * Math.PI / 180)
    };
}
```

## <a name="create-a-method-script-file"></a>Een methode-scriptbestand maken

Methode scripts zijn vergelijkbaar met het gedrag van scripts. Ze bepalen het gedrag van het apparaat wanneer een specifieke cloud naar apparaatmethode wordt aangeroepen.

Het script van de intrekken drone stelt coördinaten van de drone zien met een vaste punt voor het simuleren van de drone start retourneren.

Kopieer de volgende JavaScript in een teksteditor en sla deze als **droneRecall method.js**.

### <a name="device-model-javascript-simulation-example"></a>Voorbeeld van apparaat model JavaScript-simulatie

```JavaScript
"use strict";

// Default state
var state = {
    velocity: 0.0,
    velocity_unit: "mm/sec",
    acceleration: 0.0,
    acceleration_unit: "mm/sec^2",
    latitude: 0.0,
    longitude: 0.0,
    altitude: 0.0
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
 * Entry point function called by the simulation engine.
 *
 * @param context        The context contains current time, device model and id, not used
 * @param previousState  The device state since the last iteration, not used
 * @param previousProperties  The device properties since the last iteration
 */
/*jslint unparam: true*/
function main(context, previousState, previousProperties) {

    // Restore the global device properties and the global state before
    // generating the new telemetry, so that the telemetry can apply changes
    // using the previous function state.
    restoreSimulation(previousState, previousProperties);

    //simulate the behavior of a drone when recalled
  state.latitude = 47.476075;
  state.longitude = -122.192026;
  return state;
}
```

## <a name="debugging-script-files"></a>Foutopsporing scriptbestanden

Als u niet een foutopsporingsprogramma aan een actieve gedrag-bestand koppelen, is het mogelijk om te schrijven van gegevens in de service log met behulp de **log** functie. Syntaxisfouten, de interpreter mislukt en schrijft gegevens over de uitzondering naar het logboek.

Voorbeeld van logboekregistratie:

```JavaScript
function main(context, state) {

    log("This message will appear in the service logs.");

    log(context.deviceId);

    if (typeof(state) !== "undefined" && state !== null) {
        log("Previous value: " + state.temperature);
    }

    // ...

    return updateState;
}
```

## <a name="deploy-an-advanced-device-model"></a>Implementeren van een geavanceerde Apparaatmodel

Voor het implementeren van de geavanceerde Apparaatmodel, uploadt u de bestanden de Apparaatsimulatie-exemplaar:

Selecteer **Apparaatmodellen** in de menubalk. De **apparaatmodellen** pagina geeft een lijst van de apparaatmodellen die beschikbaar zijn in dit exemplaar van Apparaatsimulatie:

![Apparaatmodellen](media/iot-accelerators-device-simulation-advanced-device/devicemodelnav.png)

Klik op **+ Apparaatmodellen toevoegen** in de rechterbovenhoek van de pagina:

![Apparaatmodel toevoegen](media/iot-accelerators-device-simulation-advanced-device/devicemodels.png)

Klik op **Geavanceerd** om de geavanceerde apparaat model tabblad te openen:

![Tabblad Geavanceerd](media/iot-accelerators-device-simulation-advanced-device/advancedtab.png)

Klik op **Bladeren** en selecteert u de JSON en JavaScript-bestanden die u hebt gemaakt. Zorg ervoor dat alle drie bestanden selecteren. Als alle één bestand ontbreekt, wordt validatie is mislukt:

![Door bestanden bladeren](media/iot-accelerators-device-simulation-advanced-device/browse.png)

Als uw bestanden gevalideerd worden, klikt u op **opslaan** en het Apparaatmodel van uw is gereed om te worden gebruikt in een simulatie. Anders eventuele fouten te corrigeren en de bestanden Renderinstellingen:

![Opslaan](media/iot-accelerators-device-simulation-advanced-device/validated.png)

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd over de model-bestanden van het apparaat wordt gebruikt voor Apparaatsimulatie en over het maken van een geavanceerde Apparaatmodel. Vervolgens kunt u bekijken hoe u [gebruik Time Series Insights voor het visualiseren van telemetrie verzonden vanaf de oplossingsverbetering voor Apparaatsimulatie](https://docs.microsoft.com/azure/iot-accelerators/iot-accelerators-device-simulation-time-series-insights).
