---
title: Apparaat-schema in de oplossing voor externe controle - Azure | Microsoft Docs
description: In dit artikel beschrijft de JSON-schema dat een gesimuleerd apparaat in de oplossing voor externe controle definieert.
services: iot-suite
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
ms.openlocfilehash: 24aeb9c3f73d04a3d05f09ebd2ba0859a38e7ad8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="understand-the-device-model-schema"></a>Het modelschema apparaat begrijpen

U kunt gesimuleerde apparaten in de oplossing voor externe controle gebruiken om het gedrag te testen. Wanneer u de oplossing voor externe controle implementeert, wordt automatisch een verzameling gesimuleerde apparaten ingericht. U kunt de bestaande gesimuleerde apparaten aanpassen of uw eigen maken.

In dit artikel beschrijft het modelschema apparaat waarmee de mogelijkheden en het gedrag van een gesimuleerd apparaat. Het model is opgeslagen in een JSON-bestand.

De volgende artikelen hebben betrekking op het huidige artikel:

* [De model-gedrag van het apparaat implementeren](iot-suite-remote-monitoring-device-behavior.md) beschrijft de JavaScript-bestanden die u gebruikt voor het implementeren van het gedrag van een gesimuleerd apparaat.
* [Maak een nieuw gesimuleerd apparaat](iot-suite-remote-monitoring-test.md) helemaal plaatst en ziet u hoe u een nieuw gesimuleerd apparaattype implementeert in uw oplossing.

In dit artikel leert u het volgende:

>[!div class="checklist"]
> * Een JSON-bestand voor het definiëren van een model gesimuleerde apparaat gebruiken
> * De eigenschappen van het gesimuleerde apparaat opgeven
> * Geef de telemetrie die het gesimuleerde apparaat verzendt
> * Specificeert de methoden die het apparaat reageert op voor het cloud-naar-apparaat

## <a name="the-parts-of-the-device-model-schema"></a>De onderdelen van het modelschema apparaat

Elk Apparaatmodel zoals een Koelunit of vrachtwagen, definieert een type van het gesimuleerde apparaat verbinding maakt met de oplossing voor externe controle. Elk Apparaatmodel is opgeslagen in een JSON-bestand met het volgende op het hoogste niveau schema:

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
| `Version` | Identificeert de versie van het model. |
| `Name` | Een beschrijvende naam voor het model. |
| `Description` | Een beschrijving van het model. |
| `Protocol` | Het verbindingsprotocol het apparaat gebruikt. Een van `AMQP`, `MQTT`, en `HTTP`. |

De volgende secties worden de andere secties in het JSON-schema:

## <a name="simulation"></a>Simulatie

In de `Simulation` sectie, definieert u de interne status van het gesimuleerde apparaat. Telemetrie-waarden die zijn verzonden door het apparaat moet deel uitmaken van de apparaatstatus van dit.

De definitie van de apparaatstatus heeft twee elementen:

* `InitialState` initiële waarden voor alle eigenschappen van het apparaat statusobject definieert.
* `Script` identificeert een JavaScript-bestand dat wordt uitgevoerd op een planning voor het bijwerken van de apparaatstatus. U kunt dit scriptbestand willekeurige de telemetrie-waarden die door het apparaat verzonden.

Zie voor meer informatie over het JavaScript-bestand dat het apparaat statusobject wordt bijgewerkt, [inzicht in het gedrag van het apparaat model](iot-suite-remote-monitoring-device-behavior.md).

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
  "Interval": "00:00:10",
  "Scripts": {
    "Type": "javascript",
    "Path": "chiller-01-state.js"
  }
}
```

De service wordt uitgevoerd van de simulatie de **Koelunit-01-state.js** bestand elke vijf seconden voor het bijwerken van de apparaatstatus. U ziet de JavaScript-bestanden voor de gesimuleerde standaardapparaten in de [scriptmap](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) op GitHub. Volgens de conventies deze JavaScript-bestanden het achtervoegsel hebben **-status** ze te onderscheiden van de bestanden die het gedrag van de methode implementeren.

## <a name="properties"></a>Eigenschappen

De `Properties` sectie van het schema definieert de eigenschapswaarden die het apparaat aan de oplossing rapporteert. Bijvoorbeeld:

```json
"Properties": {
  "Type": "Elevator",
  "Location": "Building 2",
  "Latitude": 47.640792,
  "Longitude": -122.126258
}
```

Als de oplossing wordt gestart, wordt de gesimuleerde apparaten als u wilt maken van een lijst met opgevraagd `Type` waarden worden gebruikt in de gebruikersinterface. De oplossing maakt gebruik van de `Latitiude` en `Longitude` eigenschappen van de locatie van het apparaat toevoegen aan de kaart op het dashboard.

## <a name="telemetry"></a>Telemetrie

De `Telemetry` matrix geeft een lijst van alle telemetrie-typen die het gesimuleerde apparaat naar de oplossing verzendt.

Het volgende voorbeeld wordt een JSON telemetrie-bericht verzonden om de tien seconden met `floor`, `vibration`, en `temperature` gegevens van de lift sensoren:

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

`MessageTemplate` Hiermee definieert u de structuur van de JSON-bericht verzonden door het gesimuleerde apparaat. De tijdelijke aanduidingen in `MessageTemplate` gebruikt u de syntaxis `${NAME}` waar `NAME` is een sleutel van de [status apparaatobject](#simulation). Tekenreeksen moeten worden aangehaald, getallen beter niet.

`MessageSchema` Hiermee definieert u het schema van het bericht is verzonden door het gesimuleerde apparaat. Het berichtschema is ook gepubliceerd naar IoT Hub voor back-end-toepassingen om de informatie voor het interpreteren van de binnenkomende telemetrie opnieuw te gebruiken.

U kunt op dit moment alleen JSON bericht schema's gebruiken. De velden die worden vermeld in het schema kunnen zijn van de volgende typen:

* Object - geserialiseerd met de JSON
* Binair - geserialiseerd met base64
* Tekst
* Boole-waarde
* Geheel getal
* Double
* DateTime

Toevoegen voor het verzenden van telemetrieberichten met verschillende intervallen, meerdere telemetrie-typen voor de `Telemetry` matrix. Het volgende voorbeeld temperatuur en vochtigheid gegevens verzendt elke 10 seconden en de status van het lichte elke minuut:

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

Een gesimuleerd apparaat kan reageren op cloud-naar-apparaat methoden aangeroepen vanuit de oplossing voor externe controle. De `CloudToDeviceMethods` sectie in het schema van apparaat modelbestand:

* Definieert de methoden die het gesimuleerde apparaat kan reageren.
* Geeft het JavaScript-bestand dat de logica bevat worden uitgevoerd.

De lijst met methoden die deze ondersteuning biedt voor verzendt het gesimuleerde apparaat naar de oplossing voor externe controle.

Zie voor meer informatie over het JavaScript-bestand dat het gedrag van het apparaat implementeert, [inzicht in het gedrag van het apparaat model](iot-suite-remote-monitoring-device-behavior.md).

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

U ziet de JavaScript-bestanden voor de gesimuleerde standaardapparaten in de [scriptmap](https://github.com/Azure/device-simulation-dotnet/tree/master/Services/data/devicemodels/scripts) op GitHub. Volgens de conventies deze JavaScript-bestanden het achtervoegsel hebben **-methode** ze te onderscheiden van de bestanden die het gedrag van de status te implementeren.

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt beschreven hoe u uw eigen aangepaste gesimuleerd Apparaatmodel maken. In dit artikel hebt u geleerd hoe naar:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Een JSON-bestand voor het definiëren van een model gesimuleerde apparaat gebruiken
> * De eigenschappen van het gesimuleerde apparaat opgeven
> * Geef de telemetrie die het gesimuleerde apparaat verzendt
> * Specificeert de methoden die het apparaat reageert op voor het cloud-naar-apparaat

Nu u over de JSON-schema hebt geleerd, de voorgestelde volgende stap is te leren hoe u [implementeren van het gedrag van uw gesimuleerde apparaat](iot-suite-remote-monitoring-device-behavior.md).

Zie voor meer informatie voor ontwikkelaars over de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->