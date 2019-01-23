---
title: Azure Service Bus van Event Grid-gebeurtenisschema
description: Beschrijft de eigenschappen die beschikbaar zijn voor Service Bus-gebeurtenissen met Azure Event Grid
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: reference
ms.date: 01/17/2019
ms.author: babanisa
ms.openlocfilehash: f44d2c1c5be6ac895b6f5ea9feca29c0f8ed09f3
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467737"
---
# <a name="azure-event-grid-event-schema-for-service-bus"></a>Azure Event Grid-gebeurtenisschema voor Service Bus

Dit artikel bevat de eigenschappen en het schema voor Service Bus-gebeurtenissen. Zie voor een inleiding tot gebeurtenisschema's, [Azure Event Grid-gebeurtenisschema](event-schema.md).

Zie voor een lijst met zelfstudies en voorbeelden van scripts, [Service Bus-gebeurtenisbron](event-sources.md#service-bus).

## <a name="available-event-types"></a>Typen van de gebeurtenis berichten beschikbaar

Service Bus verzendt de volgende typen gebeurtenissen:

| Gebeurtenistype | Description |
| ---------- | ----------- |
| Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners | Treedt op wanneer er zich actieve berichten in een wachtrij of abonnement en er geen ontvangers luisteren. |
| Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener | Treedt op wanneer er actieve berichten in een wachtrij voor onbestelbare berichten en er zijn geen actieve listeners zijn. |

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het volgende voorbeeld ziet u het schema van actieve berichten met geen listeners-gebeurtenis:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Het schema voor een gebeurtenis dead-letter-wachtrij is vergelijkbaar:

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourcegroups/{your-rg}/providers/Microsoft.ServiceBus/namespaces/{your-service-bus-namespace}",
  "subject": "topics/{your-service-bus-topic}/subscriptions/{your-service-bus-subscription}",
  "eventType": "Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener",
  "eventTime": "2018-02-14T05:12:53.4133526Z",
  "id": "dede87b0-3656-419c-acaf-70c95ddc60f5",
  "data": {
    "namespaceName": "YOUR SERVICE BUS NAMESPACE WILL SHOW HERE",
    "requestUri": "https://{your-service-bus-namespace}.servicebus.windows.net/{your-topic}/subscriptions/{your-service-bus-subscription}/$deadletterqueue/messages/head",
    "entityType": "subscriber",
    "queueName": "QUEUE NAME IF QUEUE",
    "topicName": "TOPIC NAME IF TOPIC",
    "subscriptionName": "SUBSCRIPTION NAME"
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| onderwerp | string | Volledige resource-pad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid biedt deze waarde. |
| onderwerp | string | Uitgever gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| eventType | string | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. |
| eventTime | string | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| id | string | De unieke id voor de gebeurtenis. |
| gegevens | object | Gebeurtenisgegevens voor BLOB-opslag. |
| dataVersion | string | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | string | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| namespaceName | string | De Service Bus-naamruimte de resource bestaat in. |
| requestUri | string | De URI naar de specifieke wachtrij of abonnement dat verzendt de gebeurtenis. |
| entityType | string | Het type van Service Bus-entiteit voor het verzenden van gebeurtenissen (wachtrij of abonnement). |
| queueName | string | De wachtrij met de actieve berichten als u zich abonneert op een wachtrij. Null-waarde als onderwerpen / abonnementen. |
| topicName | string | Het onderwerp het Service Bus-abonnement met de actieve berichten behoort tot. De waarde null als het gebruik van een wachtrij. |
| subscriptionName | string | Het Service Bus-abonnement met de actieve berichten. De waarde null als het gebruik van een wachtrij. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure Event Grid, [wat is Event Grid?](overview.md)
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).
* Zie voor meer informatie over het gebruik van Azure Event Grid met Service Bus de [Service Bus en Event Grid-integratie overzicht](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md).
* Probeer [ontvangen van Service Bus-gebeurtenissen met Functions of Logic Apps](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json).
