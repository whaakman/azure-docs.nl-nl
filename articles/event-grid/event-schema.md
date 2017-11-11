---
title: Azure Event raster gebeurtenis schema
description: Beschrijft de eigenschappen die beschikbaar zijn voor gebeurtenissen met Azure Event raster
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/07/2017
ms.author: babanisa
ms.openlocfilehash: caa709fdc2a59472ee812bde91f7300396aa5755
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema"></a>Azure Event raster gebeurtenis schema

In dit artikel beschrijft de eigenschappen en het schema die aanwezig zijn voor alle gebeurtenissen. Gebeurtenissen bestaan uit een set van vijf vereiste tekenreekseigenschappen en een object van de vereiste gegevens. De eigenschappen gelden voor alle gebeurtenissen vanaf een willekeurige uitgever. Het gegevensobject bevat eigenschappen die specifiek voor elke uitgever zijn. Deze eigenschappen zijn specifiek voor de resourceprovider, zoals Azure Storage of Azure Event Hubs voor systeemonderwerpen.

Gebeurtenissen worden verzonden naar Azure gebeurtenis raster in een matrix die meerdere gebeurtenisobjecten kan bevatten. Als er slechts één gebeurtenis, heeft de matrix een lengte van 1. De matrix kan een totale grootte van maximaal 1 MB hebben. Elke gebeurtenis in de matrix is beperkt tot 64 KB.

## <a name="event-schema"></a>Gebeurtenisschema

Het volgende voorbeeld ziet u de eigenschappen die worden gebruikt door alle gebeurtenisuitgevers:

```json
[
  {
    "topic": string,
    "subject": string,    
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    }
  }
]
```

Het schema dat is gepubliceerd voor een Azure Blob storage-gebeurtenis is bijvoorbeeld:

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
  }
]
```
 
## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Alle gebeurtenissen bevatten dezelfde volgende op het hoogste niveau gegevens:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| Onderwerp | Tekenreeks | Volledige resource-pad naar de gegevensbron. Dit veld is niet beschrijfbaar. |
| Onderwerp | Tekenreeks | Publisher gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| EventType | Tekenreeks | Een van de typen van de geregistreerde gebeurtenis van de bron van deze gebeurtenis. |
| eventTime | Tekenreeks | De tijd dat de gebeurtenis wordt gegenereerd, gebaseerd op de UTC-tijd van de provider. |
| id | Tekenreeks | De unieke id voor de gebeurtenis. |
| Gegevens | object | Gebeurtenisgegevens die specifiek zijn voor de resourceprovider. |

Zie voor meer informatie over de eigenschappen in het gegevensobject, de gebeurtenisbron:

* [Azure-abonnementen (beheerbewerkingen)](event-schema-subscriptions.md)
* [Blob Storage](event-schema-blob-storage.md)
* [Event hubs](event-schema-event-hubs.md)
* [Resourcegroepen (beheerbewerkingen)](event-schema-resource-groups.md)

Voor aangepaste onderwerpen bepaalt de uitgever van gebeurtenissen voor het gegevensobject. De gegevens op het hoogste niveau moet dezelfde velden als resource gedefinieerd gebeurtenissen (standaard) bevatten. Bij het publiceren van gebeurtenissen naar aangepaste onderwerpen, kunt u overwegen modelleren van de houder van uw gebeurtenissen voor Routering en filteren.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)
* Zie voor meer informatie over het maken van een abonnement op Azure gebeurtenis raster [gebeurtenis raster abonnement schema](subscription-creation-schema.md).
