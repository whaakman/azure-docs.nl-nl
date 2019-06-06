---
title: Azure IoT Hub en Event Grid | Microsoft Docs
description: Gebruik Azure Event Grid voor het activeren van processen die op basis van acties die in IoT Hub plaatsvinden.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: kgremban
ms.openlocfilehash: f08845dbf4168627d0198e81d2092a1fe56c6c89
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66733874"
---
# <a name="react-to-iot-hub-events-by-using-event-grid-to-trigger-actions"></a>Reageren op gebeurtenissen van IoT-Hub met behulp van Event Grid om acties starten

Azure IoT Hub kan worden geïntegreerd met Azure Event Grid, zodat u kunt meldingen van gebeurtenissen naar andere services verzenden en downstream processen te activeren. Uw zakelijke toepassingen om te luisteren naar IoT Hub-gebeurtenissen, zodat u op kritieke gebeurtenissen op een betrouwbare, schaalbare en veilige manier reageren kunt configureren. Bijvoorbeeld, een toepassing bouwt die een database bijwerkt, maakt u een ticket werk en voorziet in een e-mailmelding telkens wanneer een nieuwe IoT-apparaat is geregistreerd met uw IoT hub.

[Azure Event Grid](../event-grid/overview.md) is een volledig beheerde service voor gebeurtenisroutering die gebruikmaakt van een publish-subscribe-model. Event Grid heeft ingebouwde ondersteuning voor Azure-services zoals [Azure Functions](../azure-functions/functions-overview.md) en [Azure Logic Apps](../logic-apps/logic-apps-what-are-logic-apps.md), en waarschuwingen kunt leveren aan niet-Azure-services met behulp van webhooks. Zie voor een volledige lijst van de gebeurtenis-handlers die ondersteuning biedt voor Event Grid, [een inleiding tot Azure Event Grid](../event-grid/overview.md).

![Azure Event Grid-architectuur](./media/iot-hub-event-grid/event-grid-functional-model.png)

## <a name="regional-availability"></a>Regionale beschikbaarheid

De Event Grid-integratie is beschikbaar voor IoT-hubs in de regio's waar Event Grid wordt ondersteund. Alle apparaatgebeurtenissen, met uitzondering van apparaat telemetriegebeurtenissen zijn algemeen beschikbaar. Apparaat telemetrie gebeurtenis is in openbare preview en is beschikbaar in alle regio's behalve VS-Oost, VS-West, West-Europa, [Azure Government](/azure/azure-government/documentation-government-welcome), [Azure China 21Vianet](/azure/china/china-welcome), en [Azure Duitsland](https://azure.microsoft.com/global-infrastructure/germany/). Zie voor de meest recente lijst met regio's, [een inleiding tot Azure Event Grid](../event-grid/overview.md).

## <a name="event-types"></a>Gebeurtenistypen

IoT Hub publiceert de volgende typen gebeurtenissen:

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.Devices.DeviceCreated | Gepubliceerd wanneer een apparaat is geregistreerd bij een IoT-hub. |
| Microsoft.Devices.DeviceDeleted | Wanneer een apparaat wordt verwijderd uit een IoT-hub gepubliceerd. |
| Microsoft.Devices.DeviceConnected | Wanneer een apparaat is verbonden met een IoT-hub gepubliceerd. |
| Microsoft.Devices.DeviceDisconnected | Gepubliceerd wanneer een apparaat niet is verbonden met een IoT-hub. |
| Microsoft.Devices.DeviceTelemetry | Wanneer een apparaat telemetrie-bericht wordt verzonden naar een IoT-hub gepubliceerd |

Gebruik de Azure portal of Azure CLI om te configureren welke gebeurtenissen om te publiceren vanaf elke IoT-hub. Raadpleeg de zelfstudie voor een voorbeeld [e-mailmeldingen verzenden over Azure IoT Hub-gebeurtenissen met Logic Apps](../event-grid/publish-iot-hub-events-to-logic-apps.md).

## <a name="event-schema"></a>Gebeurtenisschema

IoT Hub-gebeurtenissen bevatten alle informatie die u nodig hebt om te reageren op wijzigingen in de levenscyclus van uw apparaat. U kunt een gebeurtenis met IoT Hub door te controleren dat de eigenschap type gebeurtenis met begint identificeren **Microsoft.Devices**. Zie voor meer informatie over het gebruik van de eigenschappen van Event Grid-gebeurtenis, de [Event Grid-gebeurtenisschema](../event-grid/event-schema.md).

### <a name="device-connected-schema"></a>Verbonden apparaat-schema

Het volgende voorbeeld ziet u het schema van een verbonden apparaat-gebeurtenis:

```json
[{  
  "id": "f6bbf8f4-d365-520d-a878-17bf7238abd8",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceConnected",
  "eventTime": "2018-06-02T19:17:44.4383997Z",
  "data": {
      "deviceConnectionStateEventInfo": {
        "sequenceNumber":
          "000000000000000001D4132452F67CE200000002000000000000000000000001"
      },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice",
    "moduleId" : "DeviceModuleID",
  }, 
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="device-telemetry-schema"></a>Apparaat telemetrie schema

Apparaat telemetrie bericht moet een geldige JSON-indeling hebben met de contentType ingesteld op JSON en contentEncoding ingesteld op UTF-8 in het bericht [Systeemeigenschappen](iot-hub-devguide-routing-query-syntax.md#system-properties). Als dit niet is ingesteld, kunnen de IoT Hub wordt de berichten in de base 64 gecodeerde indeling schrijven.

U kunt apparaten telemetriegebeurtenissen voordat ze worden gepubliceerd naar Event Grid door te selecteren van het eindpunt als Event Grid verrijken. Zie voor meer informatie, [bericht Enrichments overzicht](iot-hub-message-enrichments-overview.md).

Het volgende voorbeeld ziet u het schema van een apparaat telemetrie-gebeurtenis:

```json
[{  
  "id": "9af86784-8d40-fe2g-8b2a-bab65e106785",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceTelemetry",
  "eventTime": "2019-01-07T20:58:30.48Z",
  "data": {
      "body": {
          "Weather": {
              "Temperature": 900
            },
            "Location": "USA"
        },
        "properties": {
            "Status": "Active"
        },
        "systemProperties": {
          "iothub-content-type": "application/json",
          "iothub-content-encoding": "utf-8",
          "iothub-connection-device-id": "d1",
          "iothub-connection-auth-method": "{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}",
          "iothub-connection-auth-generation-id": "123455432199234570",
          "iothub-enqueuedtime": "2019-01-07T20:58:30.48Z",
          "iothub-message-source": "Telemetry"
        }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="device-created-schema"></a>Schema van het apparaat is gemaakt

Het volgende voorbeeld ziet u het schema van een apparaat dat wordt gemaakt van gebeurtenis:

```json
[{
  "id": "56afc886-767b-d359-d59e-0da7877166b2",
  "topic": "/SUBSCRIPTIONS/<subscription ID>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.DEVICES/IOTHUBS/<hub name>",
  "subject": "devices/LogicAppTestDevice",
  "eventType": "Microsoft.Devices.DeviceCreated",
  "eventTime": "2018-01-02T19:17:44.4383997Z",
  "data": {
    "twin": {
      "deviceId": "LogicAppTestDevice",
      "etag": "AAAAAAAAAAE=",
      "deviceEtag":"null",
      "status": "enabled",
      "statusUpdateTime": "0001-01-01T00:00:00",
      "connectionState": "Disconnected",
      "lastActivityTime": "0001-01-01T00:00:00",
      "cloudToDeviceMessageCount": 0,
      "authenticationType": "sas",
      "x509Thumbprint": {
        "primaryThumbprint": null,
        "secondaryThumbprint": null
      },
      "version": 2,
      "properties": {
        "desired": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        },
        "reported": {
          "$metadata": {
            "$lastUpdated": "2018-01-02T19:17:44.4383997Z"
          },
          "$version": 1
        }
      }
    },
    "hubName": "egtesthub1",
    "deviceId": "LogicAppTestDevice"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Zie voor een gedetailleerde beschrijving van elke eigenschap [Azure Event Grid-gebeurtenisschema voor IoT-Hub](../event-grid/event-schema-iot-hub.md).

## <a name="filter-events"></a>Gebeurtenissen filteren

IoT Hub-gebeurtenisabonnementen kunnen filteren, gebeurtenissen op basis van het gebeurtenistype, gegevensinhoud en onderwerp, dit is de naam van het apparaat.

Met Event Grid kunt [filteren](../event-grid/event-filtering.md) op gebeurtenis, onderwerpen en -inhoud. Tijdens het maken van de Event Grid-abonnement, kunt u kiezen om u te abonneren op geselecteerde IoT-gebeurtenissen. Onderwerp filters in Event Grid werken op basis van **begint met** (voorvoegsel) en **eindigt met** (achtervoegsel) overeenkomt met. Het filter wordt gebruikt een `AND` -operator, zodat gebeurtenissen met een onderwerp die overeenkomen met het voorvoegsel en het achtervoegsel wordt geleverd aan de abonnee.

De indeling maakt gebruik van het onderwerp van IoT-gebeurtenissen:

```json
devices/{deviceId}
```

Event Grid kan ook filteren op kenmerken van elke gebeurtenis, inclusief de gegevensinhoud. Hiermee kunt u kiezen welke gebeurtenissen worden geleverd op basis van de inhoud van het bericht telemetrie. Raadpleeg [Geavanceerd filteren](../event-grid/event-filtering.md#advanced-filtering) om voorbeelden weer te geven.

Voor niet-telemetrische gebeurtenissen, zoals DeviceConnected, DeviceDisconnected, DeviceCreated en DeviceDeleted, kan de Event Grid filteren worden gebruikt bij het maken van het abonnement. Voor telemetriegebeurtenissen, naast het filter in Event Grid, kunnen gebruikers ook filteren op dubbele apparaten, eigenschappen van berichten en hoofdtekst met de routering bericht-query. We maken een standaard [route](iot-hub-devguide-messages-d2c.md) in IoT-Hub, op basis van uw Event Grid-abonnement op telemetrie van apparaten. Deze één route kan al uw Event Grid-abonnementen worden verwerkt. Als u wilt filteren berichten voordat telemetrische gegevens wordt verzonden, kunt u bijwerken uw [routering query](iot-hub-devguide-routing-query-syntax.md). Houd er rekening mee dat die routering query kan worden toegepast op de hoofdtekst van het bericht alleen als de hoofdtekst JSON is.

## <a name="limitations-for-device-connected-and-device-disconnected-events"></a>Beperkingen voor het apparaat is aangesloten en apparaat verbroken gebeurtenissen

Voor het ontvangen van verbonden apparaten en gebeurtenissen van apparaat is verbroken, moet u de koppeling D2C of C2D-koppeling openen voor uw apparaat. Als uw apparaat MQTT-protocol gebruikt, blijft de IoT Hub de C2D koppeling openen. Voor AMQP, kunt u de koppeling C2D openen door het aanroepen van de [Async-API ontvangen](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.deviceclient.receiveasync?view=azure-dotnet).

De koppeling D2C is geopend als u telemetrie verzendt. Als de apparaatverbinding is flikkerende, wat betekent dat het apparaat verbinding maakt en vaak de verbinding verbreekt we ontvangt niet de status van elke één verbinding, maar publiceert de status van de verbinding die een momentopname van elke minuut wordt gemaakt. In het geval van een storing van de IoT Hub publiceren we de status van het apparaat verbinding zodra de onderbreking is over. Als het apparaat wordt verbroken tijdens deze storing, wordt het apparaat niet-verbonden gebeurtenis binnen 10 minuten worden gepubliceerd.

## <a name="tips-for-consuming-events"></a>Tips voor het gebruik van gebeurtenissen

Toepassingen die werken met IoT Hub-gebeurtenissen moeten deze aanbevolen procedures te volgen:

* Meerdere abonnementen kunnen worden geconfigureerd om gebeurtenissen routeren naar de dezelfde gebeurtenis-handler, zo niet, wordt ervan uitgegaan dat er gebeurtenissen van een bepaalde bron zijn. Controleer altijd het onderwerp van het bericht om ervoor te zorgen dat het afkomstig is van de IoT-hub die u verwacht.

* Geen, wordt ervan uitgegaan dat alle gebeurtenissen die u ontvangt, zijn de typen die u verwacht. Controleer het type gebeurtenis altijd voordat het bericht wordt verwerkt.

* Berichten kunnen niet de juiste volgorde of na een vertraging. De etag-veld gebruiken om te begrijpen als uw gegevens over objecten bijgewerkt voor het apparaat is gemaakt of verwijderd apparaatgebeurtenissen is.

## <a name="next-steps"></a>Volgende stappen

* [Raadpleeg de zelfstudie IoT Hub-gebeurtenissen](../event-grid/publish-iot-hub-events-to-logic-apps.md)

* [Informatie over het rangschikken van gebeurtenissen ‘apparaat verbonden’ en ‘verbinding met apparaat verbroken’.](iot-hub-how-to-order-connection-state-events.md)

* [Meer informatie over Event Grid](../event-grid/overview.md)

* [Vergelijk de verschillen tussen routering IoT Hub-gebeurtenissen en -berichten](iot-hub-event-grid-routing-comparison.md)
