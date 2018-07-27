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
ms.openlocfilehash: dc87079083b8f07ad18f5f871bff64de8d492ebd
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39285643"
---
## <a name="the-parts-of-the-device-model-schema"></a>De onderdelen van het schema van het Apparaatmodel

Elke Apparaatmodel, zoals een Koelunit of vrachtwagen, definieert een type apparaat dat de service simulatie kunt simuleren. Elk Apparaatmodel is opgeslagen in een JSON-bestand met het volgende op het hoogste niveau schema:

```json
{
  "SchemaVersion": "1.0.0",
  "Id": "elevator-01",
  "Version": "0.0.1",
  "Name": "Elevator",
  "Description": "Elevator with floor, vibration and temperature sensors.",
  "Protocol": "AMQP",
  "Simulation": {
    // Specify the simulation behavior
  },
  "Properties": {
    // Define properties
  },
  "Telemetry": [
    // Specify telemetry
  ],
  "CloudToDeviceMethods": {
    // Specify methods
  }
}
```

U vindt de schema-bestanden voor de gesimuleerde standaardapparaten in de [devicemodels map](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels) op GitHub.

De volgende tabel beschrijft de vermeldingen op het hoogste niveau schema:

| Schema-item | Beschrijving |
| -- | --- |
| `SchemaVersion` | De schemaversie is altijd `1.0.0` en specifiek is voor de indeling van dit bestand. |
| `Id` | Een unieke ID voor dit Apparaatmodel. |
| `Version` | Hiermee geeft u de versie van het Apparaatmodel. |
| `Name` | Een beschrijvende naam voor het model. |
| `Description` | Een beschrijving van het Apparaatmodel. |
| `Protocol` | Het protocol van het apparaat wordt gebruikt. Een van `AMQP`, `MQTT`, en `HTTP`. |

De volgende secties worden de andere secties in het JSON-schema:

## <a name="simulation"></a>Simulatie

In de `Simulation` gedeelte definieert u de interne status van het gesimuleerde apparaat. Geen telemetriewaarden is verzonden door het apparaat moeten deel uitmaken van deze status van het apparaat.

De definitie van de apparaatstatus bestaat uit twee elementen:

* `InitialState` initiële waarden voor alle eigenschappen van het apparaat staat object definieert.
* `Script` een JavaScript-bestand dat wordt uitgevoerd volgens een schema om bij te werken van de apparaatstatus wordt geïdentificeerd. Dit scriptbestand kunt u de telemetriewaarden die is verzonden door het apparaat een willekeurige kleur geven.

Zie voor meer informatie over de JavaScript-bestand dat het apparaat de status-object wordt bijgewerkt, [gedrag van het Apparaatmodel begrijpen](../articles/iot-accelerators/iot-accelerators-device-simulation-device-behavior.md).

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
  "Interval": "00:00:10",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

De simulatie van de service wordt uitgevoerd de **Koelunit-01-state.js** bestand om de vijf seconden voor het bijwerken van de apparaatstatus. U ziet de JavaScript-bestanden voor de gesimuleerde standaardapparaten in de [scriptmap](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) op GitHub. Volgens de conventies wordt deze JavaScript-bestanden hebben het achtervoegsel **-status** ze te onderscheiden van de bestanden die het gedrag van de methode worden geïmplementeerd.

## <a name="properties"></a>Eigenschappen

De `Properties` sectie van het schema worden gedefinieerd de eigenschapswaarden die het apparaat aan de oplossing rapporteert. Bijvoorbeeld:

```json
"Properties": {
  "Type": "Elevator",
  "Location": "Building 2",
  "Latitude": 47.640792,
  "Longitude": -122.126258
}
```

Wanneer de oplossing wordt gestart, vraagt de gesimuleerde apparaten voor het bouwen van een lijst met `Type` waarden worden gebruikt in de gebruikersinterface. De oplossing maakt gebruik van de `Latitiude` en `Longitude` eigenschappen van de locatie van het apparaat toevoegen aan de kaart op het dashboard.

## <a name="telemetry"></a>Telemetrie

De `Telemetry` matrix geeft een lijst van alle telemetrietypen het gesimuleerde apparaat naar de oplossing verzendt.

Het volgende voorbeeld wordt een JSON-telemetrie-bericht verzonden elke tien seconden met `floor`, `vibration`, en `temperature` gegevens van sensoren van de lift:

```json
"Telemetry": [
  {
    "Interval": "00:00:10",
    "MessageTemplate": "{\"floor\":${floor},\"vibration\":${vibration},\"vibration_unit\":\"${vibration_unit}\",\"temperature\":${temperature},\"temperature_unit\":\"${temperature_unit}\"}",
    "MessageSchema": {
      "Name": "elevator-sensors;v1",
      "Format": "JSON",
      "Fields": {
        "floor": "integer",
        "vibration": "double",
        "vibration_unit": "text",
        "temperature": "double",
        "temperature_unit": "text"
      }
    }
  }
]
```

`MessageTemplate` Hiermee definieert u de structuur van het JSON-bericht verzonden door het gesimuleerde apparaat. De tijdelijke aanduidingen `MessageTemplate` gebruikt u de syntaxis `${NAME}` waar `NAME` is een sleutel uit de [status apparaatobject](#simulation). Moeten tussen aanhalingstekens staan tekenreeksen, cijfers moet niet.

`MessageSchema` Hiermee definieert u het schema van het bericht is verzonden door het gesimuleerde apparaat. Het berichtschema wordt ook gepubliceerd naar IoT Hub kunt u back endtoepassingen opnieuw gebruiken van de gegevens interpreteren van de binnenkomende telemetrie inschakelen.

U kunt op dit moment alleen JSON berichtschema's gebruiken. De velden die worden vermeld in het schema kunnen van de volgende typen zijn:

* Object - geserialiseerd met de JSON
* Binair - geserialiseerd met de met base64
* Tekst
* Booleaans
* Geheel getal
* Double-waarde
* DateTime

Toevoegen voor het verzenden van berichten over telemetrie met verschillende intervallen, meerdere telemetrietypen voor de `Telemetry` matrix. Het volgende voorbeeld temperatuur en vochtigheid gegevens verzendt elke 10 seconden en de status van het licht elke minuut:

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
  },
  {
    "Interval": "00:01:00",
    "MessageTemplate": "{\"lights\":${lights_on}}",
    "MessageSchema": {
      "Name": "RoomLights;v1",
      "Format": "JSON",
      "Fields": {
        "lights": "boolean"
      }
    }
  }
],
```

## <a name="cloudtodevicemethods"></a>CloudToDeviceMethods

Een gesimuleerd apparaat kan reageren op cloud-naar-apparaat-methoden aangeroepen vanuit een IoT-hub. De `CloudToDeviceMethods` sectie in het bestand apparaat model schema:

* Definieert de methoden die het gesimuleerde apparaat kan reageren.
* Identificeert de JavaScript-bestand met de logica om uit te voeren.

De lijst met methoden die deze ondersteuning biedt voor verzendt het gesimuleerde apparaat naar de IoT-hub die verbonden met.

Zie voor meer informatie over de JavaScript-bestand dat het gedrag van het apparaat implementeert, [gedrag van het Apparaatmodel begrijpen](../articles/iot-accelerators/iot-accelerators-device-simulation-device-behavior.md).

Het volgende voorbeeld worden drie ondersteunde methoden en de JavaScript-bestanden die voor het implementeren van deze methoden:

```json
"CloudToDeviceMethods": {
  "Reboot": {
    "Type": "javascript",
    "Path": "Reboot-method.js"
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

U ziet de JavaScript-bestanden voor de gesimuleerde standaardapparaten in de [scriptmap](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) op GitHub. Volgens de conventies wordt deze JavaScript-bestanden hebben het achtervoegsel **-methode** ze te onderscheiden van de bestanden die status gedrag te implementeren.