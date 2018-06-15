---
title: Azure Event raster Service Bus event schema
description: Beschrijft de eigenschappen die beschikbaar zijn voor Service Bus-gebeurtenissen met Azure Event raster
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: reference
ms.date: 02/21/2018
ms.author: babanisa
ms.openlocfilehash: 991679eeb0f7c98606133750b193a5895f39178f
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34303314"
---
# <a name="azure-event-grid-event-schema-for-service-bus"></a>Azure Event raster gebeurtenis-schema voor Service Bus

Dit artikel bevat de eigenschappen en het schema voor Service Bus-gebeurtenissen. Zie voor een inleiding tot gebeurtenis schema's, [Azure gebeurtenis raster gebeurtenis schema](event-schema.md).

## <a name="available-event-types"></a>Typen beschikbare gebeurtenissen

Service Bus verzendt de volgende typen gebeurtenissen:

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.ServiceBus.ActiveMessagesAvailableWithNoListeners | Deze gebeurtenis treedt op wanneer er actieve berichten in een wachtrij of abonnement en er zijn geen ontvangers luisteren. |
| Microsoft.ServiceBus.DeadletterMessagesAvailableWithNoListener | Deze gebeurtenis treedt op wanneer er actieve berichten in een wachtrij met onbestelde berichten en geen actieve listeners. |

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het volgende voorbeeld ziet u het schema van een actieve berichten met geen luisteraars gebeurtenis:

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

Het schema voor een gebeurtenis van de wachtrij voor onbestelbare lijkt:

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

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| Onderwerp | tekenreeks | Volledige resource-pad naar de gegevensbron. Dit veld is niet beschrijfbaar. Gebeurtenis raster bevat deze waarde. |
| Onderwerp | tekenreeks | Publisher gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| EventType | tekenreeks | Een van de typen van de geregistreerde gebeurtenis van de bron van deze gebeurtenis. |
| eventTime | tekenreeks | De tijd dat de gebeurtenis wordt gegenereerd, gebaseerd op de UTC-tijd van de provider. |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| gegevens | object | BLOB storage-gebeurtenisgegevens. |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Gebeurtenis raster definieert het schema van de eigenschappen op het hoogste niveau. Gebeurtenis raster bevat deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| nameSpaceName | tekenreeks | De Service Bus-naamruimte de resource bestaat in. |
| requestUri | tekenreeks | De URI naar de specifieke wachtrij of een abonnement dat de gebeurtenis. |
| EntityType | tekenreeks | Het type van Service Bus-entiteit die gebeurtenissen (wachtrij of abonnement). |
| queueName | tekenreeks | De wachtrij met de actieve berichten als u zich abonneert op een wachtrij. Waarde van null als onderwerpen over het gebruik / abonnementen. |
| TopicName | tekenreeks | Het onderwerp de Service Bus-abonnement met actieve berichten behoort. De waarde null zijn als u gebruikmaakt van een wachtrij. |
| SubscriptionName | tekenreeks | De Service Bus-abonnement met actieve berichten. De waarde null zijn als u gebruikmaakt van een wachtrij. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)
* Zie voor meer informatie over het maken van een abonnement op Azure gebeurtenis raster [gebeurtenis raster abonnement schema](subscription-creation-schema.md).
* Zie voor meer informatie over het gebruik van Azure Event raster met Service Bus de [Service Bus Event raster integratie overzicht](../service-bus-messaging/service-bus-to-event-grid-integration-concept.md).
* Probeer [ontvangen van Service Bus-gebeurtenissen met de functies of Logic Apps](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json).
