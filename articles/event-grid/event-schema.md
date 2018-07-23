---
title: Azure Event Grid-gebeurtenisschema
description: Beschrijft de eigenschappen die beschikbaar zijn voor gebeurtenissen met Azure Event Grid
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 07/20/2018
ms.author: babanisa
ms.openlocfilehash: f7be7e5f5e51a47b95d39047af9bcf08e463ca34
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186108"
---
# <a name="azure-event-grid-event-schema"></a>Azure Event Grid-gebeurtenisschema

Dit artikel beschrijft de eigenschappen en het schema die aanwezig zijn voor alle gebeurtenissen. Gebeurtenissen bestaan uit een set eigenschappen van vijf vereiste tekenreeks en een vereiste gegevens-object. De eigenschappen zijn algemene aan alle gebeurtenissen van een uitgever. Het gegevensobject heeft eigenschappen die specifiek voor elke uitgever zijn. Deze eigenschappen zijn specifiek voor de resourceprovider, zoals Azure Storage of Azure Event Hubs voor systeemonderwerpen.

Bronnen van gebeurtenissen voor het verzenden van gebeurtenissen naar Azure Event Grid in een matrix, maar dit kan verschillende event-objecten hebben. Als u gebeurtenissen naar een event grid-onderwerp boeken, kan de matrix een totale grootte van maximaal 1 MB hebben. Elke gebeurtenis in de matrix is beperkt tot 64 KB. Als een gebeurtenis of de matrix groter dan de maximale grootte is, ontvangt u het antwoord **413 Payload te groot**.

Event Grid verzonden de gebeurtenissen naar abonnees in een matrix die één gebeurtenis heeft. Dit gedrag veranderen in de toekomst.

U vindt het JSON-schema voor de Event Grid-gebeurtenis en de nettolading van de gegevens van elke Azure-uitgever van de [gebeurtenisschema in het archief](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/eventgrid/data-plane).

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
    },
    "dataVersion": string,
    "metadataVersion": string
  }
]
```

Het schema is gepubliceerd voor een Azure Blob storage-gebeurtenis is bijvoorbeeld:

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
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```

## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Alle gebeurtenissen hebben de dezelfde gegevens van de volgende op het hoogste niveau:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| onderwerp | tekenreeks | Volledige resource-pad naar de bron van de gebeurtenis. Dit veld is niet schrijfbaar. Event Grid biedt deze waarde. |
| Onderwerp | tekenreeks | Uitgever gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| type gebeurtenis | tekenreeks | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. |
| eventTime | tekenreeks | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| gegevens | object | De gegevens van de gebeurtenis is specifiek voor de resourceprovider. |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Zie voor meer informatie over de eigenschappen in het gegevensobject, de bron van gebeurtenis:

* [Azure-abonnementen (bewerkingen)](event-schema-subscriptions.md)
* [Container Registry](event-schema-container-registry.md)
* [Blob Storage](event-schema-blob-storage.md)
* [Event Hubs](event-schema-event-hubs.md)
* [IoT Hub](event-schema-iot-hub.md)
* [Media Services](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json)
* [Resourcegroepen (bewerkingen)](event-schema-resource-groups.md)
* [Service Bus](event-schema-service-bus.md)

Aangepaste onderwerpen bepaalt de gebeurtenisuitgever het gegevensobject. De gegevens op het hoogste niveau moet dezelfde velden als resource gedefinieerd standaardgebeurtenissen hebben.

Bij het publiceren van gebeurtenissen naar aangepaste onderwerpen, maken van onderwerpen voor de gebeurtenissen die het gemakkelijk voor abonnees om te weten of ze geïnteresseerd zijn in de gebeurtenis. Abonnees op basis van het onderwerp te filteren en route-gebeurtenissen. Houd rekening met voorziet in het pad waar de gebeurtenis zich voordeed, zodat abonnees op segmenten van het opgegeven pad filteren kunnen. Het pad kan abonnees nauwkeurig of ruim om gebeurtenissen te filteren. Bijvoorbeeld, als u een pad op drie segment zoals bieden `/A/B/C` in het onderwerp, abonnees kunnen filteren op het eerste segment `/A` om op te halen van een breed scala aan gebeurtenissen. Abonnees die aan de gebeurtenissen met onderwerpen zoals `/A/B/C` of `/A/D/E`. Andere abonnees kunnen filteren op `/A/B` om op te halen van een smaller reeks gebeurtenissen.

Soms moet uw onderwerp meer informatie over wat is er gebeurd. Bijvoorbeeld, de **Opslagaccounts** publisher bevat het onderwerp `/blobServices/default/containers/<container-name>/blobs/<file>` wanneer een bestand wordt toegevoegd aan een container. Een abonnee kan filteren op het pad `/blobServices/default/containers/testcontainer` om op te halen van alle gebeurtenissen voor die container, maar geen andere containers in het opslagaccount. Een abonnee kan ook filteren of de route met het achtervoegsel `.txt` om te werken alleen met tekstbestanden.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure Event Grid, [wat is Event Grid?](overview.md)
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).
