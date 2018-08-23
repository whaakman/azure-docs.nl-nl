---
title: Azure Event Grid blob storage-gebeurtenisschema
description: Beschrijft de eigenschappen die beschikbaar zijn voor gebeurtenissen van blob storage met Azure Event Grid
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: reference
ms.date: 08/17/2018
ms.author: tomfitz
ms.openlocfilehash: 11524f8868a0102e30b06f3385a26b1bd06aae6e
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42055320"
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Azure Event Grid-gebeurtenisschema voor Blob-opslag

Dit artikel bevat de eigenschappen en het schema voor gebeurtenissen van blob storage. Zie voor een inleiding tot gebeurtenisschema's, [Azure Event Grid-gebeurtenisschema](event-schema.md).

Zie voor een lijst met zelfstudies en voorbeelden van scripts, [opslag gebeurtenisbron](event-sources.md#storage).

## <a name="available-event-types"></a>Typen van de gebeurtenis berichten beschikbaar

BLOB-opslag, verzendt de volgende typen gebeurtenissen:

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.Storage.BlobCreated | Treedt op wanneer een blob wordt gemaakt. |
| Microsoft.Storage.BlobDeleted | Treedt op wanneer een blob wordt verwijderd. |

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het volgende voorbeeld ziet u het schema van een blob die wordt gemaakt van gebeurtenis: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Het schema voor een blob verwijderd gebeurtenis lijkt: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```
 
## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| onderwerp | tekenreeks | Volledige resource-pad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid biedt deze waarde. |
| Onderwerp | tekenreeks | Uitgever gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| type gebeurtenis | tekenreeks | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. |
| eventTime | tekenreeks | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| id | tekenreeks | De unieke id voor de gebeurtenis. |
| gegevens | object | Gebeurtenisgegevens voor BLOB-opslag. |
| dataVersion | tekenreeks | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | tekenreeks | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| api | tekenreeks | De bewerking waarmee de gebeurtenis geactiveerd. |
| clientRequestId | tekenreeks | Een client is gegenereerd, ondoorzichtig waarde met een tekenlimiet van 1 KB. Wanneer u opslaganalyse, logboekregistratie hebt ingeschakeld, wordt deze vastgelegd in de analytics-Logboeken. |
| aanvraag-id | tekenreeks | De unieke id voor de aanvraag. Deze gebruiken voor het oplossen van de aanvraag. |
| eTag | tekenreeks | De waarde die u gebruiken kunt voor het uitvoeren van bewerkingen voorwaardelijk. |
| contentType | tekenreeks | Het inhoudstype dat is opgegeven voor de blob. |
| contentLength | geheel getal | De grootte van de blob in bytes. |
| blobType | tekenreeks | Het type van de blob. Geldige waarden zijn 'BlockBlob' of 'PageBlob'. |
| url | tekenreeks | Het pad naar de blob. |
| sequencer | tekenreeks | Een door de gebruiker beheerde waarde die u gebruiken kunt om aanvragen te volgen. |
| storageDiagnostics | object | Informatie over de diagnostische gegevens. |
 
## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure Event Grid, [wat is Event Grid?](overview.md)
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).
* Zie voor een inleiding tot het werken met gebeurtenissen van blob storage, [Route Blob-opslaggebeurtenissen - Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
