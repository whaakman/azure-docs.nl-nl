---
title: Uitgaand verkeer en eindpunten in Azure, digitale dubbels | Microsoft Docs
description: Richtlijnen voor het maken van eindpunten met Azure digitale dubbels
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: e140ca46a18fcab2194adb213d723ab67d40b0a8
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615157"
---
# <a name="egress-and-endpoints"></a>Uitgaand verkeer en eindpunten

Het concept van biedt ondersteuning voor Azure van digitale dubbels **eindpunten**. Elk eindpunt vertegenwoordigt een bericht of gebeurtenis-broker in de Azure-abonnement van de gebruiker. Gebeurtenissen en -berichten kunnen worden verzonden naar Azure Event Hubs, Azure Event Grid en Azure Service Bus-onderwerpen.

Gebeurtenissen worden verzonden naar eindpunten op basis van vooraf gedefinieerde routering voorkeuren. De gebruiker kan opgeven welk eindpunt moet een van de volgende gebeurtenissen ontvangen: 

- TopologyOperation
- UdfCustom
- SensorChange
- SpaceChange
- DeviceMessage

Raadpleeg voor een basiskennis hebt van gebeurtenissen Routering en gebeurtenistypen [Routing gebeurtenissen en -berichten](concepts-events-routing.md).

## <a name="event-types-description"></a>Beschrijving van gebeurtenis-typen

De indeling voor elk van de typen gebeurtenissen worden beschreven in de volgende secties.

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
  "topic": "/subscriptions/yourTopicName"
}
```

| Naam van aangepast kenmerk | Vervangen door |
| --- | --- |
| yourTopicName | De naam van uw aangepaste onderwerp |

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
  "topic": "/subscriptions/yourTopicName"
}
```

| Naam van aangepast kenmerk | Vervangen door |
| --- | --- |
| yourTopicName | De naam van uw aangepaste onderwerp |

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
  "topic": "/subscriptions/yourTopicName"
}
```

| Naam van aangepast kenmerk | Vervangen door |
| --- | --- |
| yourTopicName | De naam van uw aangepaste onderwerp |

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
  "topic": "/subscriptions/yourTopicName"
}
```

| Naam van aangepast kenmerk | Vervangen door |
| --- | --- |
| yourTopicName | De naam van uw aangepaste onderwerp |

### <a name="devicemessage"></a>DeviceMessage

Met behulp van **DeviceMessage**, kunt u een **EventHub** verbinding waarmee onbewerkte telemetriegebeurtenissen ook kunnen worden gerouteerd van digitale dubbels van Azure.

> [!NOTE]
> - **DeviceMessage** is alleen met combineerbare **EventHub**. Kan niet worden gecombineerd **DeviceMessage** met een van de andere typen gebeurtenissen.
> - U kunt slechts één eindpunt van de combinatie van het type opgeven **EventHub** of **DeviceMessage**.

## <a name="configure-endpoints"></a>Eindpunten configureren

Eindpunt management wordt uitgevoerd via de API-eindpunten. De volgende voorbeelden laten zien hoe u de verschillende ondersteunde eindpunten configureren. Speciale aandacht besteden aan de matrix van de typen gebeurtenis omdat ze de routering voor het eindpunt:

```plaintext
POST https://endpoints-demo.azuresmartspaces.net/management/api/v1.0/endpoints
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
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourTopicName"
  }
  ```

    | Naam van aangepast kenmerk | Vervangen door |
    | --- | --- |
    | yourNamespace | De naamruimte van uw eindpunt |
    | yourPrimaryKey | De primaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | yourSecondaryKey | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | yourTopicName | De naam van uw aangepaste onderwerp |

- Route naar Event Grid gebeurtenistypen **SensorChange**, **SpaceChange**, en **TopologyOperation**:

  ```JSON
  {
    "type": "EventGrid",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "yourPrimaryKey",
    "secondaryConnectionString": "yourSecondaryKey",
    "path": "yourTopicName.westus-1.eventgrid.azure.net"
  }
  ```

    | Naam van aangepast kenmerk | Vervangen door |
    | --- | --- |
    | yourPrimaryKey | De primaire verbindingsreeks die wordt gebruikt om te verifiëren|
    | yourSecondaryKey | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | yourTopicName | De naam van uw aangepaste onderwerp |

- Route naar Event Hubs gebeurtenistypen **SensorChange**, **SpaceChange**, en **TopologyOperation**:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "SensorChange",
      "SpaceChange",
      "TopologyOperation"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey",
    "path": "yourEventHubName"
  }
  ```

    | Naam van aangepast kenmerk | Vervangen door |
    | --- | --- |
    | yourNamespace | De naamruimte van uw eindpunt |
    | yourPrimaryKey | De primaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | yourSecondaryKey | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | yourEventHubName | De naam van uw event hub |

- Route naar Event Hubs-gebeurtenistype **DeviceMessage**. De opname van `EntityPath` in de **connectionString** is verplicht:

  ```JSON
  {
    "type": "EventHub",
    "eventTypes": [
      "DeviceMessage"
    ],
    "connectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourPrimaryKey;EntityPath=yourEventHubName",
    "secondaryConnectionString": "Endpoint=sb://yourNamespace.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=yourSecondaryKey;EntityPath=yourEventHubName",
    "path": "yourEventHubName"
  }
  ```

    | Naam van aangepast kenmerk | Vervangen door |
    | --- | --- |
    | yourNamespace | De naamruimte van uw eindpunt |
    | yourPrimaryKey | De primaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | yourSecondaryKey | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | yourEventHubName | De naam van uw event hub |

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
