---
title: Azure Event raster abonnement gebeurtenis schema
description: Beschrijft de eigenschappen die beschikbaar zijn voor abonnement-gebeurtenissen met Azure Event raster
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 04bcc3018346e9cf162d26b0a23147c88e902e1e
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="azure-event-grid-event-schema-for-subscriptions"></a>Azure Event raster gebeurtenis schema voor abonnementen

Dit artikel bevat de eigenschappen en het schema voor gebeurtenissen van de Azure-abonnement. Zie voor een inleiding tot gebeurtenis schema's, [Azure gebeurtenis raster gebeurtenis schema](event-schema.md).

Azure-abonnementen en resourcegroepen verzenden hetzelfde type als de gebeurtenis. De typen gebeurtenissen betrekking hebben op wijzigingen in de resources. Het belangrijkste verschil is dat resourcegroepen gebeurtenissen voor resources binnen de resourcegroep verzenden en Azure-abonnementen verzenden van gebeurtenissen voor bronnen in het abonnement.

## <a name="available-event-types"></a>Typen beschikbare gebeurtenissen

Azure-abonnementen verzenden Beheergebeurtenissen van Azure Resource Manager, zoals wanneer een virtuele machine wordt gemaakt of een opslagaccount is verwijderd.

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.Resources.ResourceWriteSuccess | Deze gebeurtenis treedt op wanneer een resource maken of bijwerken van de bewerking is geslaagd. |
| Microsoft.Resources.ResourceWriteFailure | Deze gebeurtenis treedt op wanneer een resource maken of update-bewerking is mislukt. |
| Microsoft.Resources.ResourceWriteCancel | Deze gebeurtenis treedt op is wanneer een resource maken of bijwerken van de bewerking geannuleerd. |
| Microsoft.Resources.ResourceDeleteSuccess | Deze gebeurtenis treedt op als een resource-delete-bewerking is geslaagd. |
| Microsoft.Resources.ResourceDeleteFailure | Deze gebeurtenis treedt op wanneer een resource-delete-bewerking is mislukt. |
| Microsoft.Resources.ResourceDeleteCancel | Deze gebeurtenis treedt op wanneer een resource-delete-bewerking wordt geannuleerd. Deze gebeurtenis treedt op als de sjabloonimplementatie van een wordt geannuleerd. |

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het volgende voorbeeld ziet u het schema van een resource gemaakt van gebeurtenis: 

```json
[
  {
    "topic":"/subscriptions/{subscription-id}",
    "subject":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "eventType":"Microsoft.Resources.ResourceWriteSuccess",
    "eventTime":"2017-08-16T03:54:38.2696833Z",
    "id":"25b3b0d0-d79b-44d5-9963-440d4e6a9bba",
    "data": {
        "authorization":"{azure_resource_manager_authorizations}",
        "claims":"{azure_resource_manager_claims}",
        "correlationId":"54ef1e39-6a82-44b3-abc1-bdeb6ce4d3c6",
        "httpRequest":"{request-operation}",
        "resourceProvider":"Microsoft.EventGrid",
        "resourceUri":"/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
        "operationName":"Microsoft.EventGrid/eventSubscriptions/write",
        "status":"Succeeded",
        "subscriptionId":"{subscription-id}",
        "tenantId":"72f988bf-86f1-41af-91ab-2d7cd011db47"
        },
      "dataVersion": "",
      "metadataVersion": "1"
  }
]
```

Het schema voor een gebeurtenis met resource verwijderd is vergelijkbaar:

```json
[{
  "topic":"/subscriptions/{subscription-id}",
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicApp0ecd6c02-2296-4d7c-9865-01532dc99c93",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2017-11-07T21:24:19.6959483Z",
  "id": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
  "data": {
    "authorization": "{azure_resource_manager_authorizations}",
    "claims": "{azure_resource_manager_claims}",
    "correlationId": "7995ecce-39d4-4851-b9d7-a7ef87a06bf5",
    "httpRequest": "{request-operation}",
    "resourceProvider": "Microsoft.EventGrid",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventGrid/eventSubscriptions/LogicAppdd584bdf-8347-49c9-b9a9-d1f980783501",
    "operationName": "Microsoft.EventGrid/eventSubscriptions/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "72f988bf-86f1-41af-91ab-2d7cd011db47"
  },
  "dataVersion": "",
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
| gegevens | object | Gebeurtenisgegevens van het abonnement. |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Gebeurtenis raster definieert het schema van de eigenschappen op het hoogste niveau. Gebeurtenis raster bevat deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| Autorisatie | tekenreeks | De aangevraagde autorisatie voor de bewerking. |
| Claims | tekenreeks | De eigenschappen van de claims. |
| correlationId | tekenreeks | Een bewerking-ID voor het oplossen van problemen. |
| httpRequest | tekenreeks | De details van de bewerking. |
| resourceProvider | tekenreeks | De resourceprovider voor de bewerking wordt uitgevoerd. |
| resourceUri | tekenreeks | De URI van de resource in de bewerking. |
| operationName | tekenreeks | De bewerking die werd uitgevoerd. |
| status | tekenreeks | De status van de bewerking. |
| subscriptionId | tekenreeks | De abonnements-ID van de resource. |
| TenantId | tekenreeks | De tenant-ID van de resource. |

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure gebeurtenis raster, [wat is er gebeurtenis raster?](overview.md).
* Zie voor meer informatie over het maken van een abonnement op Azure gebeurtenis raster [gebeurtenis raster abonnement schema](subscription-creation-schema.md).
