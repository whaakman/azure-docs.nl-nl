---
title: Azure Event raster blob storage gebeurtenis schema
description: Beschrijft de eigenschappen die beschikbaar zijn voor blob storage-gebeurtenissen met Azure Event raster
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: bdc64733b75fd809cf0245986aa96370343c1a34
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Azure Event raster gebeurtenis schema voor Blob-opslag

Dit artikel bevat de eigenschappen en het schema voor blob storage-gebeurtenissen. Zie voor een inleiding tot gebeurtenis schema's, [Azure gebeurtenis raster gebeurtenis schema](event-schema.md).

## <a name="available-event-types"></a>Typen beschikbare gebeurtenissen

BLOB-opslag, verzendt de volgende typen gebeurtenissen:

| Gebeurtenistype | Beschrijving |
| ---------- | ----------- |
| Microsoft.Storage.BlobCreated | Deze gebeurtenis treedt op wanneer een blob is gemaakt. |
| Microsoft.Storage.BlobDeleted | Deze gebeurtenis treedt op wanneer een blob wordt verwijderd. |

## <a name="example-event"></a>Voorbeeld van de gebeurtenis

Het volgende voorbeeld ziet u het schema van een blob gemaakt van gebeurtenis: 

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
  }
}]
```

Het schema voor een gebeurtenis blob verwijderd is vergelijkbaar: 

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
  }
}]
```
 
## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| Onderwerp | Tekenreeks | Volledige resource-pad naar de gegevensbron. Dit veld is niet beschrijfbaar. |
| Onderwerp | Tekenreeks | Publisher gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| EventType | Tekenreeks | Een van de typen van de geregistreerde gebeurtenis van de bron van deze gebeurtenis. |
| eventTime | Tekenreeks | De tijd dat de gebeurtenis wordt gegenereerd, gebaseerd op de UTC-tijd van de provider. |
| id | Tekenreeks | De unieke id voor de gebeurtenis. |
| Gegevens | object | BLOB storage-gebeurtenisgegevens. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Beschrijving |
| -------- | ---- | ----------- |
| api | Tekenreeks | De bewerking waarmee de gebeurtenis is geactiveerd. |
| clientRequestId | Tekenreeks | Een client is gegenereerd, ondoorzichtige waarde met een maximaal 1 KB tekens. Wanneer u opslaganalyse logboekregistratie hebt ingeschakeld, is het vastgelegd in het analytics. |
| aanvraag-id | Tekenreeks | De unieke id voor de aanvraag. Deze gebruiken voor het oplossen van de aanvraag. |
| ETag | Tekenreeks | De waarde die u gebruiken kunt voor voorwaardelijk bewerkingen uitvoeren. |
| ContentType | Tekenreeks | Het inhoudstype dat is opgegeven voor de blob. |
| contentLength | geheel getal | De grootte van de blob in bytes. |
| BlobType | Tekenreeks | Het type van de blob. |
| URL | Tekenreeks | Het pad naar de blob. |
| sequencer | Tekenreeks | Een gebruiker gecontroleerde waarde die u gebruiken kunt om aanvragen te volgen. |
| storageDiagnostics | object | Informatie over diagnostische gegevens over de opslag. |
 
## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure gebeurtenis raster, [wat gebeurtenis raster is?](overview.md)
* Zie voor meer informatie over het maken van een abonnement op Azure gebeurtenis raster [gebeurtenis raster abonnement schema](subscription-creation-schema.md).
* Zie voor een inleiding in het werken met blob storage gebeurtenissen [gebeurtenissen in de Route Blob-opslag - Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
