---
title: Uitgaand verkeer en eindpunten in Azure, digitale dubbels | Microsoft Docs
description: Richtlijn voor het maken van eindpunten met Azure digitale dubbels
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: alinast
ms.openlocfilehash: c09ee84cda5f0a9747d3ee1f8f1b37d1323f2cc2
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2018
ms.locfileid: "50212247"
---
# <a name="egress-and-endpoints"></a>Uitgaand verkeer en eindpunten

Het concept van biedt ondersteuning voor Azure van digitale dubbels _eindpunten_ waarbij elk eindpunt vertegenwoordigt een bericht/gebeurtenis-broker in Azure-abonnement van de gebruiker. Gebeurtenissen en -berichten kunnen worden verzonden naar **Event Hub**, **Event Grid**, en **Service Bus-onderwerpen**.

Gebeurtenissen worden verzonden naar de eindpunten op basis van vooraf gedefinieerde routering voorkeuren: de gebruiker kan opgeven welk eindpunt moet een van de volgende gebeurtenissen ontvangen: **TopologyOperation**, **UdfCustom**, **SensorChange**, **SpaceChange**, of **DeviceMessage**.

Raadpleeg voor een basiskennis hebt van gebeurtenissen Routering en gebeurtenistypen [Routing gebeurtenissen en -berichten](concepts-events-routing.md).

## <a name="event-types-description"></a>Beschrijving van gebeurtenis-typen

Hier volgen de indeling voor elk van de typen gebeurtenissen:

- **TopologyOperation**

  Van toepassing op wijzigingen in de metagegevensgrafiek. De *onderwerp* eigenschap geeft u het type object dat is beïnvloed. Typen van objecten die deze gebeurtenis kunnen zijn: **apparaat**, **DeviceBlobMetadata**, **DeviceExtendedProperty**, **ExtendedPropertyKey**, **ExtendedType**, **KeyStore**, **rapport**, **RoleDefinition**, **Sensor**, **SensorBlobMetadata**, **SensorExtendedProperty**, **ruimte**, **SpaceBlobMetadata**,  **SpaceExtendedProperty**, **SpaceResource**, **SpaceRoleAssignment**, **System**, **gebruiker**, **UserBlobMetadata**, **UserExtendedProperty**.

  Voorbeeld:

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

    | Naam van aangepast kenmerk | Vervangen |
    | --- | --- |
    | *yourTopicName* | De naam van uw aangepaste onderwerp |

- **UdfCustom**

  Een gebeurtenis die is verzonden door een gebruiker gedefinieerde functie (UDF's). 
  
  > [!IMPORTANT]
  > Deze gebeurtenis is expliciet worden verzonden vanuit de UDF zelf.

  Voorbeeld:

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

    | Naam van aangepast kenmerk | Vervangen |
    | --- | --- |
    | *yourTopicName* | De naam van uw aangepaste onderwerp |

- **SensorChange**

  Een update van een sensor status op basis van wijzigingen van telemetrie.

  Voorbeeld:

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

    | Naam van aangepast kenmerk | Vervangen |
    | --- | --- |
    | *yourTopicName* | De naam van uw aangepaste onderwerp |

- **SpaceChange**

  Een update van een spatie staat op basis van wijzigingen van telemetrie.

  Voorbeeld:

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

    | Naam van aangepast kenmerk | Vervangen |
    | --- | --- |
    | *yourTopicName* | De naam van uw aangepaste onderwerp |

- **DeviceMessage**

  Hiermee kunt u opgeven een **EventHub** verbinding waarmee onbewerkte telemetriegebeurtenissen ook kunnen worden gerouteerd van digitale dubbels van Azure.

> [!NOTE]
> - **DeviceMessage** is alleen met combineerbare **EventHub**; zich niet combineren **DeviceMessage** met een van de andere typen gebeurtenissen.
> - Kunt u zich kunt u slechts één eindpunt van de combinatie van het type **EventHub** of **DeviceMessage**.

## <a name="configuring-endpoints"></a>Eindpunten configureren

Eindpunt management wordt uitgevoerd via de API-eindpunten. Hier volgen enkele voorbeelden over het configureren van de verschillende ondersteunde eindpunten. Speciale aandacht besteden aan de matrix van de typen gebeurtenis omdat ze de routering voor het eindpunt:

```plaintext
POST https://endpoints-demo.azuresmartspaces.net/management/api/v1.0/endpoints
```

- Doorsturen naar **Service Bus** gebeurtenistypen: **SensorChange**, **SpaceChange**, **TopologyOperation**

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

    | Naam van aangepast kenmerk | Vervangen |
    | --- | --- |
    | *yourNamespace* | De naamruimte van uw eindpunt |
    | *yourPrimaryKey* | De primaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | *yourSecondaryKey* | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | *yourTopicName* | De naam van uw aangepaste onderwerp |

- Doorsturen naar **Event Grid** gebeurtenistypen: **SensorChange**, **SpaceChange**, **TopologyOperation**

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

    | Naam van aangepast kenmerk | Vervangen |
    | --- | --- |
    | *yourPrimaryKey* | De primaire verbindingsreeks die wordt gebruikt om te verifiëren|
    | *yourSecondaryKey* | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | *yourTopicName* | De naam van uw aangepaste onderwerp |

- Doorsturen naar **Event Hub** gebeurtenistypen: **SensorChange**, **SpaceChange**, **TopologyOperation**

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

    | Naam van aangepast kenmerk | Vervangen |
    | --- | --- |
    | *yourNamespace* | De naamruimte van uw eindpunt |
    | *yourPrimaryKey* | De primaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | *yourSecondaryKey* | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | *yourEventHubName* | De naam van uw **Event Hub** |

- Doorsturen naar **Event Hub** gebeurtenistype: **DeviceMessage**. De opname van `EntityPath` in de **connectionString** is verplicht.

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

    | Naam van aangepast kenmerk | Vervangen |
    | --- | --- |
    | *yourNamespace* | De naamruimte van uw eindpunt |
    | *yourPrimaryKey* | De primaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | *yourSecondaryKey* | De secundaire verbindingsreeks die wordt gebruikt om te verifiëren |
    | *yourEventHubName* | De naam van uw **Event Hub** |

> [!NOTE]
> Bij het maken van een nieuw eindpunt duurt het maximaal 5 tot 10 minuten te ontvangen van gebeurtenissen op het eindpunt.

## <a name="primary-and-secondary-connection-keys"></a>Van primaire en secundaire verbindingssleutels

Wanneer een primaire verbinding-sleutel niet-geautoriseerde wordt, probeert het systeem automatisch de van secundaire verbindingssleutel. Dat biedt een back-up en kunt de mogelijkheid om zonder problemen worden geverifieerd en bijwerken van de primaire sleutel via de API-eindpunten.

Als de primaire en secundaire verbindingssleutels niet gemachtigd bent, voert het systeem een exponentieel uitstel wachttijd van 30 minuten. Gebeurtenissen op elke geactiveerde uitstel wachttijd verwijderd.

Wanneer het systeem zich in de wacht staat een back-off, bijwerken verbindingen sleutels via de API-eindpunten kunnen pas van kracht tot 30 minuten duren.

## <a name="unreachable-endpoints"></a>Niet bereikbaar-eindpunten

Wanneer u een eindpunt niet meer bereikbaar is, voert het systeem een exponentieel uitstel wachttijd van 30 minuten. Gebeurtenissen op elke geactiveerde uitstel wachttijd verwijderd.

## <a name="next-steps"></a>Volgende stappen

Lees voor meer het gebruik van Azure digitale dubbele Swagger [Azure digitale dubbele Swagger](how-to-use-swagger.md).

Lees voor meer informatie over routering gebeurtenissen en berichten in Azure, digitale dubbels [Routing gebeurtenissen en -berichten](concepts-events-routing.md).
