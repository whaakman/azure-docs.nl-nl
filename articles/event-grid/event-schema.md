---
title: Azure Event raster gebeurtenis schema
description: Beschrijft de eigenschappen die beschikbaar zijn voor gebeurtenissen met Azure Event raster
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 04/17/2018
ms.author: babanisa
ms.openlocfilehash: 3e0b7fd825b8e985cea2c32301986b3a7f8bb619
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34304059"
---
# <a name="azure-event-grid-event-schema"></a>Azure Event raster gebeurtenis schema

In dit artikel beschrijft de eigenschappen en het schema die aanwezig zijn voor alle gebeurtenissen. Gebeurtenissen bestaan uit een set van vijf vereiste tekenreekseigenschappen en een object van de vereiste gegevens. De eigenschappen gelden voor alle gebeurtenissen vanaf een willekeurige uitgever. Het gegevensobject bevat eigenschappen die specifiek voor elke uitgever zijn. Deze eigenschappen zijn specifiek voor de resourceprovider, zoals Azure Storage of Azure Event Hubs voor systeemonderwerpen.

Bronnen van gebeurtenissen voor het verzenden van gebeurtenissen naar Azure gebeurtenis raster in een matrix die meerdere gebeurtenisobjecten kan bevatten. Bij het publiceren van gebeurtenissen naar een onderwerp van het raster gebeurtenis kan een totale grootte van maximaal 1 MB hebben in de matrix. Elke gebeurtenis in de matrix is beperkt tot 64 KB. Als een gebeurtenis of de matrix de maximale grootte overschrijdt, ontvangt u het antwoord **413 nettolading te groot**.

Gebeurtenis raster verzendt gebeurtenissen naar abonnees in een matrix met één gebeurtenis. Dit gedrag kan in de toekomst wijzigen.

U vindt de JSON-schema voor de gebeurtenis gebeurtenis raster en elke Azure uitgever nettolading met gegevens in de [gebeurtenis Schema store](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/eventgrid/data-plane).

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
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```

## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Alle gebeurtenissen bevatten dezelfde volgende op het hoogste niveau gegevens:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| Onderwerp | tekenreeks | Volledige resource-pad naar de gegevensbron. Dit veld is niet beschrijfbaar. Gebeurtenis raster bevat deze waarde. |
| Onderwerp | tekenreeks | Publisher gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| EventType | tekenreeks | Een van de typen van de geregistreerde gebeurtenis van de bron van deze gebeurtenis. |
| eventTime | tekenreeks | De tijd dat de gebeurtenis wordt gegenereerd, gebaseerd op de UTC-tijd van de provider. |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| gegevens | object | Gebeurtenisgegevens die specifiek zijn voor de resourceprovider. |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Gebeurtenis raster definieert het schema van de eigenschappen op het hoogste niveau. Gebeurtenis raster bevat deze waarde. |

Zie voor meer informatie over de eigenschappen in het gegevensobject, de gebeurtenisbron:

* [Azure-abonnementen (beheerbewerkingen)](event-schema-subscriptions.md)
* [Blob Storage](event-schema-blob-storage.md)
* [Event Hubs](event-schema-event-hubs.md)
* [Service Bus](event-schema-service-bus.md)
* [IoT Hub](event-schema-iot-hub.md)
* [Resourcegroepen (beheerbewerkingen)](event-schema-resource-groups.md)

Voor aangepaste onderwerpen bepaalt de uitgever van gebeurtenissen voor het gegevensobject. De gegevens op het hoogste niveau moet dezelfde velden als resource gedefinieerd gebeurtenissen (standaard) bevatten.

Bij het publiceren van gebeurtenissen naar aangepaste onderwerpen, maken onderwerpen voor de gebeurtenissen die gemakkelijk voor abonnees te weten of ze geïnteresseerd in de gebeurtenis. Abonnees op basis van het onderwerp op gebeurtenissen filteren en route. Overweeg voorzien in het pad waar de gebeurtenis heeft plaatsgevonden, dus abonnees op segmenten in dat pad filteren kunnen. Het pad kan abonnees nauwkeurig of grote schaal om gebeurtenissen te filteren. Bijvoorbeeld, als u een pad drie segment zoals opgeven `/A/B/C` in het onderwerp abonnees kunnen filteren op het eerste segment `/A` ophalen van een uitgebreide reeks gebeurtenissen. Deze abonnees ophalen van gebeurtenissen met onderwerpen zoals `/A/B/C` of `/A/D/E`. Andere abonnees kunnen filteren op `/A/B` ophalen van een fijner reeks gebeurtenissen.

Soms moet uw onderwerp meer informatie over wat er gebeurd is. Bijvoorbeeld, de **Opslagaccounts** publisher voorziet in de `/blobServices/default/containers/<container-name>/blobs/<file>` wanneer een bestand wordt toegevoegd aan een container. Een abonnee kan filteren op het pad `/blobServices/default/containers/testcontainer` alle gebeurtenissen ophalen voor die container, maar geen andere containers in het opslagaccount. Een abonnee kan ook filteren of route door het achtervoegsel `.txt` alleen werkt met tekstbestanden.

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)
* Zie voor meer informatie over het maken van een abonnement op Azure gebeurtenis raster [gebeurtenis raster abonnement schema](subscription-creation-schema.md).
