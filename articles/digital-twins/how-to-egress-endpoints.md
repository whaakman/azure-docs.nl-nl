---
title: Uitgaand verkeer en eindpunten in Azure, digitale dubbels | Microsoft Docs
description: Richtlijnen voor het maken van eindpunten met digitale dubbels van Azure.
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 12/31/2018
ms.author: alinast
ms.openlocfilehash: e93811a56f934a95dde45633c4fb64312b3696df
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994812"
---
# <a name="egress-and-endpoints"></a>Uitgaand verkeer en eindpunten

Azure van digitale dubbels *eindpunten* vertegenwoordigen een bericht of gebeurtenis-broker binnen Azure-abonnement van een gebruiker. Gebeurtenissen en -berichten kunnen worden verzonden naar Azure Event Hubs, Azure Event Grid en Azure Service Bus-onderwerpen.

Gebeurtenissen worden doorgestuurd naar eindpunten op basis van vooraf gedefinieerde routering voorkeuren. Gebruikers opgeven die *gebeurtenistypen* elk eindpunt kan ontvangen.

Voor meer informatie over gebeurtenissen, Routering en gebeurtenistypen, verwijzen naar [Routing gebeurtenissen en -berichten in Azure, digitale dubbels](./concepts-events-routing.md).

## <a name="events"></a>Gebeurtenissen

Gebeurtenissen worden verzonden door de IoT-objecten (zoals apparaten en sensoren) voor de verwerking van door Azure bericht en gebeurtenis-brokers. Gebeurtenissen worden gedefinieerd door de volgende [schemaverwijzing voor Azure Event Grid-gebeurtenis](../event-grid/event-schema.md).

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "subject": "ExtendedPropertyKey",
  "data": {
    "SpacesToNotify": [
      "3a16d146-ca39-49ee-b803-17a18a12ba36"
    ],
    "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
    "AccessType": "Create"
  },
  "eventType": "TopologyOperation",
  "eventTime": "2018-04-17T17:41:54.9400177Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Kenmerk | Type | Description |
| --- | --- | --- |
| id | string | De unieke id voor de gebeurtenis. |
| onderwerp | string | Uitgever gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| gegevens | object | De gegevens van de gebeurtenis is specifiek voor de resourceprovider. |
| type gebeurtenis | string | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. |
| eventTime | string | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| dataVersion | string | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | string | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |
| onderwerp | string | Volledige resource-pad naar de bron van de gebeurtenis. Dit veld is niet schrijfbaar. Event Grid biedt deze waarde. |

Voor meer informatie over de Event Grid-gebeurtenisschema:

- Controleer de [schemaverwijzing voor Azure Event Grid-gebeurtenis](../event-grid/event-schema.md).
- Lees de [Azure EventGrid Node.js SDK EventGridEvent verwijzing](https://docs.microsoft.com/javascript/api/azure-eventgrid/eventgridevent?view=azure-node-latest).

## <a name="event-types"></a>Gebeurtenistypen

Gebeurtenistypen classificeren van de aard van de gebeurtenis en zijn ingesteld de **type gebeurtenis** veld. Beschikbare gebeurtenistypen zijn gegeven door de volgende lijst:

- TopologyOperation
- UdfCustom
- SensorChange
- SpaceChange
- DeviceMessage

De indeling voor elk gebeurtenistype worden nader beschreven in de volgende subsecties.

### <a name="topologyoperation"></a>TopologyOperation

**TopologyOperation** is van toepassing op wijzigingen in de metagegevensgrafiek. De **onderwerp** eigenschap geeft u het type object dat is beïnvloed. De volgende typen objecten mogelijk deze gebeurtenis te activeren:

- Apparaat
- DeviceBlobMetadata
- DeviceExtendedProperty
- ExtendedPropertyKey
- ExtendedType
- KeyStore
- Rapport
- Roldefinitie
- Sensor
- SensorBlobMetadata
- SensorExtendedProperty
- Ruimte
- SpaceBlobMetadata
- SpaceExtendedProperty
- SpaceResource
- SpaceRoleAssignment
- Systeem
- Gebruiker
- UserBlobMetadata
- UserExtendedProperty

#### <a name="example"></a>Voorbeeld

```JSON
{
  "id": "00000000-0000-0000-0000-000000000000",
  "subject": "ExtendedPropertyKey",
  "data": {
    "SpacesToNotify": [
      "3a16d146-ca39-49ee-b803-17a18a12ba36"
    ],
    "Id": "00000000-0000-0000-0000-000000000000",
      "Type": "ExtendedPropertyKey",
    "AccessType": "Create"
  },
  "eventType": "TopologyOperation",
  "eventTime": "2018-04-17T17:41:54.9400177Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Waarde | Vervangen door |
| --- | --- |
| YOUR_TOPIC_NAME | De naam van uw aangepaste onderwerp |

### <a name="udfcustom"></a>UdfCustom

**UdfCustom** is een gebeurtenis die is verzonden door een gebruiker gedefinieerde functie (UDF's).
  
> [!IMPORTANT]  
> Deze gebeurtenis moet expliciet worden verzonden vanuit de UDF zelf.

#### <a name="example"></a>Voorbeeld

```JSON
{
  "id": "568fd394-380b-46fa-925a-ebb96f658cce",
  "subject": "UdfCustom",
  "data": {
    "TopologyObjectId": "7c799bfc-1bff-4b9e-b15a-669933969d20",
    "ResourceType": "Space",
    "Payload": "\"Room is not available or air quality is poor\"",
    "CorrelationId": "568fd394-380b-46fa-925a-ebb96f658cce"
  },
  "eventType": "UdfCustom",
  "eventTime": "2018-10-02T06:50:15.198Z",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Waarde | Vervangen door |
| --- | --- |
| YOUR_TOPIC_NAME | De naam van uw aangepaste onderwerp |

### <a name="sensorchange"></a>SensorChange

**SensorChange** is een update van een sensor status op basis van wijzigingen van telemetrie.

#### <a name="example"></a>Voorbeeld

```JSON
{
  "id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
  "subject": "SensorChange",
  "data": {
    "Type": "Classic",
    "DataType": "Motion",
    "Id": "60bf5336-2929-45b4-bb4c-b45699dfe95f",
    "Value": "False",
    "PreviousValue": "True",
    "EventTimestamp": "2018-04-17T17:46:15.4964262Z",
    "MessageType": "sensor",
    "Properties": {
      "ms-client-request-id": "c9e576b7-5eea-4f61-8617-92a57add5179",
      "ms-activity-id": "ct22YwXEGJ5u.605.0"
    }
  },
  "eventType": "SensorChange",
  "eventTime": "2018-04-17T17:46:18.5452993Z",
  "dataVersion": "1",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Waarde | Vervangen door |
| --- | --- |
| YOUR_TOPIC_NAME | De naam van uw aangepaste onderwerp |

### <a name="spacechange"></a>SpaceChange

**SpaceChange** is een update van een spatie staat op basis van wijzigingen van telemetrie.

#### <a name="example"></a>Voorbeeld

```JSON
{
  "id": "42522e10-b1aa-42ff-a5e7-7181788ffc4b",
  "subject": "SpaceChange",
  "data": {
    "Type": null,
    "DataType": "AvailableAndFresh",
    "Id": "7c799bfc-1bff-4b9e-b15a-669933969d20",
    "Value": "Room is not available or air quality is poor",
    "PreviousValue": null,
    "RawData": null,
    "transactionId": null,
    "EventTimestamp": null,
    "MessageType": null,
    "Properties": null,
    "CorrelationId": "42522e10-b1aa-42ff-a5e7-7181788ffc4b"
  },
  "eventType": "SpaceChange",
  "eventTime": "2018-10-02T06:50:20.128Z",
  "dataVersion": "1.0",
  "metadataVersion": "1",
  "topic": "/subscriptions/YOUR_TOPIC_NAME"
}
```

| Waarde | Vervangen door |
| --- | --- |
| YOUR_TOPIC_NAME | De naam van uw aangepaste onderwerp |

### <a name="devicemessage"></a>DeviceMessage

Met behulp van **DeviceMessage**, kunt u een **EventHub** verbinding waarmee onbewerkte telemetriegebeurtenissen ook kunnen worden gerouteerd van digitale dubbels van Azure.

> [!NOTE]
> - **DeviceMessage** is alleen met combineerbare **EventHub**. Kan niet worden gecombineerd **DeviceMessage** met een van de andere typen gebeurtenissen.
> - U kunt slechts één eindpunt van de combinatie van het type opgeven **EventHub** of **DeviceMessage**.

## <a name="configure-endpoints"></a>Eindpunten configureren

Eindpunt management wordt uitgevoerd via de API-eindpunten.

[!INCLUDE [Digital Twins Management API](../../includes/digital-twins-management-api.md)]

De volgende voorbeelden laten zien hoe u de ondersteunde eindpunten configureren.

>[!IMPORTANT]
> Let op de **eigenschap eventTypes** kenmerk. Hiermee definieert u welke gebeurtenis typen worden verwerkt door het eindpunt en dus bepalen de routering.

Op basis van een geverifieerde HTTP POST-aanvraag

```plaintext
YOUR_MANAGEMENT_API_URL/endpoints
```

- Route naar Service Bus-gebeurtenistypen **SensorChange**, **SpaceChange**, en **TopologyOperation**:

  ```JSON
  {
    "type": "ServiceBus",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME"
  }
  ```

    | Waarde | Vervangen door |
    | --- | --- |
    | YOUR_NAMESPACE | De naamruimte van uw eindpunt |
    | YOUR_PRIMARY_KEY | De primaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | YOUR_SECONDARY_KEY | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | YOUR_TOPIC_NAME | De naam van uw aangepaste onderwerp |

- Route naar Event Grid gebeurtenistypen **SensorChange**, **SpaceChange**, en **TopologyOperation**:

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "YOUR_SECONDARY_KEY",
    "path": "YOUR_TOPIC_NAME.westus-1.eventgrid.azure.net"
  }
  ```

    | Waarde | Vervangen door |
    | --- | --- |
    | YOUR_PRIMARY_KEY | De primaire verbindingsreeks die wordt gebruikt om te verifiëren|
    | YOUR_SECONDARY_KEY | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | YOUR_TOPIC_NAME | De naam van uw aangepaste onderwerp |

- Route naar Event Hubs gebeurtenistypen **SensorChange**, **SpaceChange**, en **TopologyOperation**:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Waarde | Vervangen door |
    | --- | --- |
    | YOUR_NAMESPACE | De naamruimte van uw eindpunt |
    | YOUR_PRIMARY_KEY | De primaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | YOUR_SECONDARY_KEY | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | YOUR_EVENT_HUB_NAME | De naam van uw event hub |

- Route naar Event Hubs-gebeurtenistype **DeviceMessage**. De opname van `EntityPath` in de **connectionString** is verplicht:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_PRIMARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "secondaryConnectionString": "Endpoint=sb://YOUR_NAMESPACE.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=YOUR_SECONDARY_KEY;EntityPath=YOUR_EVENT_HUB_NAME",
    "path": "YOUR_EVENT_HUB_NAME"
  }
  ```

    | Waarde | Vervangen door |
    | --- | --- |
    | YOUR_NAMESPACE | De naamruimte van uw eindpunt |
    | YOUR_PRIMARY_KEY | De primaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | YOUR_SECONDARY_KEY | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | YOUR_EVENT_HUB_NAME | De naam van uw event hub |

> [!NOTE]  
> Bij het maken van een nieuw eindpunt duurt het maximaal 5 tot 10 minuten te ontvangen van gebeurtenissen op het eindpunt.

## <a name="primary-and-secondary-connection-keys"></a>Van primaire en secundaire verbindingssleutels

Wanneer een primaire verbinding-sleutel niet-geautoriseerde wordt, probeert het systeem automatisch de van secundaire verbindingssleutel. Dat biedt een back-up en kunt de mogelijkheid om zonder problemen worden geverifieerd en bijwerken van de primaire sleutel via de API-eindpunten.

Als de primaire en secundaire verbindingssleutels niet gemachtigd bent, krijgt het systeem een exponentieel uitstel wachttijd van 30 minuten. Gebeurtenissen worden verwijderd op elke geactiveerde uitstel wachttijd.

Wanneer het systeem zich in de wacht staat een back-off, bijwerken verbindingen sleutels via de API-eindpunten kunnen pas van kracht tot 30 minuten duren.

## <a name="unreachable-endpoints"></a>Niet bereikbaar-eindpunten

Wanneer u een eindpunt niet meer bereikbaar is, krijgt het systeem een exponentieel uitstel wachttijd van 30 minuten. Gebeurtenissen worden verwijderd op elke geactiveerde uitstel wachttijd.

## <a name="next-steps"></a>Volgende stappen

- Informatie over [het gebruik van Azure digitale dubbele Swagger](how-to-use-swagger.md).

- Meer informatie over [routering van gebeurtenissen en berichten](concepts-events-routing.md) in digitale dubbels van Azure.
