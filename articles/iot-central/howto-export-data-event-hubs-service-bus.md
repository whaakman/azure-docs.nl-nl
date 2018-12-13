---
title: Uw gegevens exporteren naar Azure Event Hubs en Azure Service Bus | Microsoft Docs
description: Het exporteren van gegevens uit uw Azure IoT Central-toepassing naar Azure Event Hubs en Azure Service Bus
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 12/07/2018
ms.topic: conceptual
ms.service: iot-central
manager: peterpr
ms.openlocfilehash: 14b51f109ca76661ac10c99d42002dda45bc0500
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318684"
---
# <a name="export-your-data-in-azure-iot-central"></a>Uw gegevens in Azure IoT Central exporteren

*In dit onderwerp is bedoeld voor beheerders.*

In dit artikel dives dieper in het gebruik van de continue export-functie in Azure IoT Central uw gegevens te exporteren naar uw eigen **Azure Event Hubs**, en **Azure Service Bus** exemplaren. U kunt exporteren **metingen**, **apparaten**, en **apparaatsjablonen** naar uw eigen bestemming voor warm-pad inzichten en analyses. Dit omvat het activeren van de aangepaste regels in Azure Stream Analytics, aangepaste werkstromen in Azure Logic Apps activeren of omzetten van de gegevens en deze door te geven via Azure Functions. 

> [!Note]
> Nogmaals, als u continue gegevensexport inschakelt, krijgt u alleen de gegevens vanaf dat moment standaardtarieven. Gegevens kunnen op dit moment niet worden hersteld gedurende een periode wanneer voortdurende gegevensexport uitgeschakeld is. Als u wilt meer historische gegevens behouden, moet u voortdurende gegevensexport vroeg inschakelen.


## <a name="prerequisites"></a>Vereisten

- U moet een beheerder in uw IoT Central-toepassing

## <a name="export-to-azure-event-hubs-and-azure-service-bus"></a>Exporteren naar Azure Eventhubs en Azure Servicebus

Metingen, apparaten en sjablonen apparaatgegevens worden geëxporteerd naar uw event hub of de Service Bus-wachtrij of onderwerp in bijna realtime. Geëxporteerde metingen gegevens bevatten het geheel van het bericht dat uw apparaten verzonden naar IoT Central, niet alleen de waarden van de metingen zelf. Geëxporteerde apparaten gegevens bevatten wijzigingen in de eigenschappen en instellingen van alle apparaten en geëxporteerde apparaatsjablonen bevat wijzigingen aan alle apparaatsjablonen. De geëxporteerde gegevens is in de eigenschap 'hoofdtekst' en wordt in JSON-indeling.

> [!NOTE]
> Bij het kiezen van een Service Bus als een export-doel, wachtrijen en onderwerpen **mag geen sessies of detectie van dubbele ingeschakeld**. Als een van deze opties zijn ingeschakeld, wordt niet in de wachtrij of onderwerp enkele berichten binnenkomen.

### <a name="measurements"></a>Metingen

Een nieuw bericht is snel geëxporteerd nadat IoT Central het bericht van een apparaat ontvangt. Elk bericht dat wordt geëxporteerd in Event Hubs en Service Bus bevat de volledige berichtgeschiedenis het apparaat verzonden in de eigenschap 'body' in JSON-indeling. 

> [!NOTE]
> De apparaten die het verzenden van de metingen worden vertegenwoordigd door de apparaat-id's (Zie de volgende gedeelten). Als u de namen van de apparaten, apparaatgegevens exporteren en correleren van elk bericht met behulp van de **connectionDeviceId** die overeenkomt met de **deviceId** van het bericht.

Het volgende voorbeeld wordt een bericht over metingen gegevens in de event hub of Service Bus-wachtrij of onderwerp ontvangen.

```json
{
  "body": {
    "humidity": 29.06963648666288,
    "temp": 8.4503795661685,
    "pressure": 1075.8334910110093,
    "magnetometerX": 408.6966458887116,
    "magnetometerY": -532.8809796603962,
    "magnetometerZ": 174.70653875528205,
    "accelerometerX": 1481.546749013788,
    "accelerometerY": -1139.4316656437406,
    "accelerometerZ": 811.6928695575307,
    "gyroscopeX": 442.19879163299856,
    "gyroscopeY": 123.23710975717177,
    "gyroscopeZ": 708.5397575786151,
    "deviceState": "DANGER"
  },
  "annotations": {
    "iothub-connection-device-id": "<connectionDeviceId>",
    "iothub-connection-auth-method": "{\"scope\":\"hub\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
    "iothub-connection-auth-generation-id": "<generationId>",
    "iothub-enqueuedtime": 1539381029965,
    "iothub-message-source": "Telemetry",
    "x-opt-sequence-number": 25325,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539381030200
  },
  "sequenceNumber": 25325,
  "enqueuedTimeUtc": "2018-10-12T21:50:30.200Z",
  "offset": "<offset>",
  "properties": {
    "content_type": "application/json",
    "content_encoding": "utf-8"
  }
}
```

### <a name="devices"></a>Apparaten

Berichten met apparaatgegevens worden verzonden naar uw event hub of Service Bus-wachtrij of onderwerp om de paar minuten. Dit betekent dat om de paar minuten een berichtenbatch ontvangt met gegevens over
- Nieuwe apparaten die zijn toegevoegd
- Apparaten met gewijzigde eigenschappen en waarden in te stellen

Elk bericht vertegenwoordigt een of meer wijzigingen aan een apparaat sinds de laatste geëxporteerde bericht. Informatie die in elk bericht wordt verzonden, omvat:
- `id` van het apparaat in IoT Central
- `name` van het apparaat
- `deviceId` van [Device Provisioning Service](https://aka.ms/iotcentraldocsdps)
- Sjabloon van apparaatgegevens
- Waarden van eigenschappen
- Waarden in te stellen

> [!NOTE]
> Apparaten die zijn verwijderd, omdat de laatste batch worden niet geëxporteerd. Er zijn momenteel geen indicatoren in geëxporteerde berichten voor verwijderde apparaten.
>
> De apparaat-sjabloon die elk apparaat hoort wordt vertegenwoordigd door een sjabloon voor apparaat-ID. Als u de naam van de sjabloon van het apparaat, moet apparaatgegevens sjabloon te exporteren.

Het volgende voorbeeld ziet een bericht over apparaatgegevens in de event hub of Service Bus-wachtrij of onderwerp:


```json
{
  "body": {
    "id": "<id>",
    "name": "<deviceName>",
    "simulated": true,
    "deviceId": "<deviceId>",
    "deviceTemplate": {
      "id": "<templateId>",
      "version": "1.0.0"
    },
    "properties": {
      "cloud": {
        "location": "Seattle"
      },
      "device": {
        "dieNumber": 5445.5862873026645
      }
    },
    "settings": {
      "device": {
        "fanSpeed": 0
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "devices",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 39740,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274959654
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 39740,
  "enqueuedTimeUtc": "2018-10-11T16:22:39.654Z",
  "offset": "<offset>",
}
```

### <a name="device-templates"></a>Apparaatsjablonen

Berichten met apparaatgegevens sjablonen worden verzonden naar uw event hub of Service Bus-wachtrij of onderwerp om de paar minuten. Dit betekent dat om de paar minuten een berichtenbatch ontvangt met gegevens over
- Nieuwe apparaatsjablonen die zijn toegevoegd
- Apparaatsjablonen met gewijzigde metingen, eigenschap en definities instellen

Elk bericht vertegenwoordigt een of meer wijzigingen aan de sjabloon van een apparaat sinds de laatste geëxporteerde bericht. Informatie die in elk bericht wordt verzonden, omvat:
- `id` van de sjabloon voor apparaat
- `name` van de sjabloon voor apparaat
- `version` van de sjabloon voor apparaat
- Gegevenstypen van de meting en min/max-waarden
- Typen eigenschappen voor gegevens en standaardwaarden
- Gegevenstypen en waarden in te stellen

> [!NOTE]
> Apparaatsjablonen verwijderd sinds de laatste batch worden niet geëxporteerd. Er zijn momenteel geen indicatoren in geëxporteerde berichten voor verwijderde sjablonen.

Het volgende voorbeeld ziet een bericht over sjablonen apparaatgegevens in de event hub of Service Bus-wachtrij of onderwerp:

```json
{
  "body": {
    "id": "<id>",
    "version": "1.0.0",
    "name": "<templateName>",
    "measurements": {
      "telemetry": {
        "humidity": {
          "dataType": "double",
          "name": "humidity"
        },
        "pressure": {
          "dataType": "double",
          "name": "pressure"
        },
        "temp": {
          "dataType": "double",
          "name": "temperature"
        }
      }
    },
    "properties": {
      "cloud": {
        "location": {
          "dataType": "string",
          "name": "Location"
        }
      },
      "device": {
        "dieNumber": {
          "dataType": "double",
          "name": "Die Number"
        }
      }
    },
    "settings": {
      "device": {
        "fanSpeed": {
          "dataType": "double",
          "name": "Fan Speed",
          "initialValue": 0
        }
      }
    }
  },
  "annotations": {
    "iotcentral-message-source": "deviceTemplates",
    "x-opt-partition-key": "<partitionKey>",
    "x-opt-sequence-number": 25315,
    "x-opt-offset": "<offset>",
    "x-opt-enqueued-time": 1539274985085
  },
  "partitionKey": "<partitionKey>",
  "sequenceNumber": 25315,
  "enqueuedTimeUtc": "2018-10-11T16:23:05.085Z",
  "offset": "<offset>",
}
```

## <a name="next-steps"></a>Volgende stappen

Nu u weet hoe u uw gegevens te exporteren naar Azure Event Hubs en Azure Service Bus, doorgaan met de volgende stap:

> [!div class="nextstepaction"]
> [Hoe u Azure Functions activeren](howto-trigger-azure-functions.md)
