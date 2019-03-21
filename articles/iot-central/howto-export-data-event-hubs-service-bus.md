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
ms.openlocfilehash: 700e8e9fe0dac182d71df8ca66800fa03cf25a2e
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295790"
---
# <a name="export-your-data-in-azure-iot-central"></a>Uw gegevens in Azure IoT Central exporteren

*In dit onderwerp is bedoeld voor beheerders.*

In dit artikel wordt beschreven hoe u van de continue export-functie in Azure IoT Central uw gegevens te exporteren naar uw eigen **Azure Event Hubs**, en **Azure Service Bus** exemplaren. U kunt exporteren **metingen**, **apparaten**, en **apparaatsjablonen** naar uw eigen bestemming voor warm-pad inzichten en analyses. Dit omvat het activeren van de aangepaste regels in Azure Stream Analytics, aangepaste werkstromen in Azure Logic Apps activeren of omzetten van de gegevens en deze door te geven via Azure Functions. 

> [!Note]
> Nogmaals, als u continue gegevensexport inschakelt, krijgt u alleen de gegevens vanaf dat moment standaardtarieven. Gegevens kunnen op dit moment niet worden hersteld gedurende een periode wanneer voortdurende gegevensexport uitgeschakeld is. Als u wilt meer historische gegevens behouden, moet u voortdurende gegevensexport vroeg inschakelen.


## <a name="prerequisites"></a>Vereisten

- U moet een beheerder in uw IoT Central-toepassing

## <a name="set-up-export-destination"></a>Export doel instellen

Als u een bestaande Event Hubs-Service Bus om te exporteren naar niet hebt, volgt u deze stappen:

## <a name="create-event-hubs-namespace"></a>Event Hubs-naamruimte maken

1. Maak een [nieuwe Event Hubs-naamruimte in Azure portal](https://ms.portal.azure.com/#create/Microsoft.EventHub). U kunt meer informatie in [Azure Event Hubs docs](https://docs.microsoft.com/azure/event-hubs/event-hubs-create).
2. Kies een abonnement. 

    > [!Note] 
    > U kunt nu gegevens exporteren naar andere abonnementen die zijn **niet hetzelfde** als voor uw betalen per gebruik IoT Central-toepassing. U verbinding maakt met een verbindingsreeks in dit geval.
3. Maak een event hub in uw Event Hubs-naamruimte. Ga naar uw naamruimte en selecteer **+ Event Hub** boven aan het maken van een event hub-instantie.

## <a name="create-service-bus-namespace"></a>Service Bus-naamruimte maken

1. Maak een [nieuwe Service Bus-naamruimte in Azure portal](https://ms.portal.azure.com/#create/Microsoft.ServiceBus.1.0.5) . U kunt meer informatie in [Azure Service Bus-docs](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-create-namespace-portal).
2. Kies een abonnement. 

    > [!Note] 
    > U kunt nu gegevens exporteren naar andere abonnementen die zijn **niet hetzelfde** als voor uw betalen per gebruik IoT Central-toepassing. U verbinding maakt met een verbindingsreeks in dit geval.

3. Ga naar uw Service Bus-naamruimte en selecteer **+ wachtrij** of **+ onderwerp** boven aan het maken van een wachtrij of onderwerp om te exporteren naar.


## <a name="set-up-continuous-data-export"></a>Voortdurende gegevensexport instellen

Nu dat u een Event Hubs-Service Bus-doel hebt voor het exporteren van gegevens, volg deze stappen voor het instellen van continue gegevensexport. 

1. Meld u aan uw IoT Central-toepassing.

2. Selecteer in het menu links **continue gegevensexport**.

    > [!Note]
    > Als er geen continue Export van gegevens in het menu links, bent u niet een beheerder in uw app. Neem contact op met een beheerder voor het instellen van het exporteren van gegevens.

    ![Nieuwe cde Event Hub maken](media/howto-export-data/export_menu.PNG)

3. Selecteer de **+ nieuw** knop in de rechterbovenhoek. Kies een van de **Azure Event Hubs** of **Azure Service Bus** als de bestemming of het exporteren. 

    > [!NOTE] 
    > Het maximum aantal uitvoer per app is vijf. 

    ![Maken van nieuwe voortdurende gegevensexport](media/howto-export-data/export_new.PNG)

4. Selecteer in de vervolgkeuzelijst uw **Event Hubs-naamruimte/Service Bus-naamruimte**. U kunt ook de laatste optie kiezen in de lijst die is **een verbindingsreeks invoeren**. 

    > [!NOTE] 
    > U ziet alleen Storage Accounts/Event Hubs-naamruimten/Service Bus-naamruimten in de **hetzelfde abonnement als uw app IoT Central**. Als u exporteren naar een bestemming buiten dit abonnement wilt, kiest u **een verbindingsreeks invoeren** en raadpleegt u stap 5.

    > [!NOTE] 
    > Voor zeven dagen proefversie apps, de enige manier om het configureren van doorlopende gegevens exporteren, is via een verbindingsreeks. Dit komt doordat zeven dagen proefversie apps nog geen een gekoppelde Azure-abonnement.

    ![Nieuwe cde Event Hub maken](media/howto-export-data/export_create.PNG)

5. (Optioneel) Als u ervoor hebt gekozen **een verbindingsreeks invoeren**, een nieuwe verschijnt u plak de verbindingsreeks. Om op te halen van de verbindingsreeks voor uw:
    - Eventhubs of Service Bus, gaat u naar de naamruimte in Azure portal.
        - Onder **instellingen**, selecteer **beleid voor gedeelde toegang**
        - Kies de standaardwaarden voor **RootManageSharedAccessKey** of een nieuw wachtwoord maken
        - Kopieer de primaire of secundaire verbindingsreeks
 
6. Kies een Event hub/wachtrij of onderwerp in het vak vervolgkeuzelijst.

7. Onder **gegevens naar de export**, elk type gegevens wilt exporteren door het type in te stellen **op**.

6. Als u wilt inschakelen voortdurende gegevensexport, zorg ervoor dat **gegevensexport** is **op**. Selecteer **Opslaan**.

  ![Voortdurende gegevensexport configureren](media/howto-export-data/export_list.PNG)

7. Na een paar minuten verschijnt uw gegevens in uw gekozen bestemming.


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
