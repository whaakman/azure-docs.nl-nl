---
title: Azure Event Grid blob storage-gebeurtenisschema
description: Beschrijft de eigenschappen die beschikbaar zijn voor gebeurtenissen van blob storage met Azure Event Grid
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: reference
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: bed6c3f1efcb2d0ef34e827ddb2b521f8c038940
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445771"
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Azure Event Grid-gebeurtenisschema voor Blob-opslag

Dit artikel bevat de eigenschappen en het schema voor gebeurtenissen van blob storage. Zie voor een inleiding tot gebeurtenisschema's, [Azure Event Grid-gebeurtenisschema](event-schema.md).

Zie voor een lijst met zelfstudies en voorbeelden van scripts, [opslag gebeurtenisbron](event-sources.md#storage).

## <a name="list-of-events-for-blob-rest-apis"></a>Lijst van gebeurtenissen voor Blob REST-API 's

Deze gebeurtenissen worden geactiveerd wanneer een client wordt gemaakt, vervangt of een blob verwijderd door het aanroepen van de Blob REST-API's.

 |Gebeurtenisnaam |Description|
 |----------|-----------|
 |**Microsoft.Storage.BlobCreated** |Geactiveerd wanneer een blob wordt gemaakt of vervangen. <br>Om precies deze gebeurtenis wordt geactiveerd wanneer clients gebruiken de `PutBlob`, `PutBlockList`, of `CopyBlob` bewerkingen die beschikbaar in de Blob-REST-API zijn.   |
 |**Microsoft.Storage.BlobDeleted** |Geactiveerd wanneer een blob wordt verwijderd. <br>Om precies deze gebeurtenis wordt geactiveerd wanneer clients de `DeleteBlob` bewerking die beschikbaar is in de Blob-REST-API. |

> [!NOTE]
> Als u wilt ervoor te zorgen dat de **Microsoft.Storage.BlobCreated** gebeurtenis wordt geactiveerd, alleen wanneer een blok-Blob is volledig toegewezen, het filteren van de gebeurtenis voor de `CopyBlob`, `PutBlob`, en `PutBlockList` REST API-aanroepen. Deze API-aanroepen trigger de **Microsoft.Storage.BlobCreated** gebeurtenis alleen nadat gegevens volledig doorgevoerd naar een blok-Blob zijn. Zie voor meer informatie over het maken van een filter, [gebeurtenissen filteren op Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

## <a name="list-of-the-events-for-azure-data-lake-storage-gen-2-rest-apis"></a>Lijst van de gebeurtenissen voor Azure Data Lake Storage Gen 2 REST API 's

Deze gebeurtenissen worden geactiveerd als u een hiërarchische naamruimte in de storage-account inschakelen en de clients Azure Data Lake Storage Gen2 REST API's aanroepen.

> [!NOTE]
> Deze gebeurtenissen zijn in openbare preview en ze zijn alleen beschikbaar de **VS-West 2** en **West-Centraal VS** regio's.

 |Gebeurtenisnaam|Description|
 |----------|-----------|
 |**Microsoft.Storage.BlobCreated** | Geactiveerd wanneer een blob wordt gemaakt of vervangen. <br>Om precies deze gebeurtenis wordt geactiveerd wanneer clients gebruiken de `CreateFile` en `FlushWithClose` bewerkingen die beschikbaar in de REST-API voor Azure Data Lake Storage Gen2 zijn. |
 |**Microsoft.Storage.BlobDeleted** |Geactiveerd wanneer een blob wordt verwijderd. <br>Deze gebeurtenis wordt specifiek, ook geactiveerd wanneer clients aanroepen de `DeleteFile` bewerking die beschikbaar is in de REST-API voor Azure Data Lake Storage Gen2. |
 |**Microsoft.Storage.BlobRenamed**|Geactiveerd wanneer een blob wordt gewijzigd. <br>Om precies deze gebeurtenis wordt geactiveerd wanneer clients gebruiken de `RenameFile` bewerking die beschikbaar is in de REST-API voor Azure Data Lake Storage Gen2.|
 |**Microsoft.Storage.DirectoryCreated**|Geactiveerd wanneer een map wordt gemaakt. <br>Om precies deze gebeurtenis wordt geactiveerd wanneer clients gebruiken de `CreateDirectory` bewerking die beschikbaar is in de REST-API voor Azure Data Lake Storage Gen2.|
 |**Microsoft.Storage.DirectoryRenamed**|Geactiveerd wanneer een map wordt gewijzigd. <br>Om precies deze gebeurtenis wordt geactiveerd wanneer clients gebruiken de `RenameDirectory` bewerking die beschikbaar is in de REST-API voor Azure Data Lake Storage Gen2.|
 |**Microsoft.Storage.DirectoryDeleted**|Geactiveerd wanneer een directory wordt verwijderd. <br>Om precies deze gebeurtenis wordt geactiveerd wanneer clients gebruiken de `DeleteDirectory` bewerking die beschikbaar is in de REST-API voor Azure Data Lake Storage Gen2.|

> [!NOTE]
> Als u wilt ervoor te zorgen dat de **Microsoft.Storage.BlobCreated** gebeurtenis wordt geactiveerd, alleen wanneer een blok-Blob is volledig toegewezen, het filteren van de gebeurtenis voor de `FlushWithClose` REST API-aanroep. Deze API-aanroepen triggers de **Microsoft.Storage.BlobCreated** gebeurtenis alleen nadat gegevens volledig doorgevoerd naar een blok-Blob zijn. Zie voor meer informatie over het maken van een filter, [gebeurtenissen filteren op Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

<a id="example-event" />

## <a name="the-contents-of-an-event-response"></a>De inhoud van een reactie van de gebeurtenis

Wanneer een gebeurtenis wordt geactiveerd, verzendt de Event Grid-service gegevens over die gebeurtenis met geabonneerde eindpunt.

In deze sectie bevat een voorbeeld van hoe deze gegevens zien voor elke gebeurtenis van blob storage eruit zou.

### <a name="microsoftstorageblobcreated-event"></a>Microsoft.Storage.BlobCreated gebeurtenis

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/test-container/blobs/new-file.txt",
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
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/new-file.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobcreated-event-data-lake-storage-gen2"></a>Microsoft.Storage.BlobCreated gebeurtenis (Data Lake Storage Gen2)

Als de blob storage-account een hiërarchische naamruimte heeft, de gegevens er uitzien zoals in het vorige voorbeeld met uitzondering van de volgende wijzigingen:

* De `dataVersion` sleutel is ingesteld op een waarde van `2`.

* De `data.api` sleutel is ingesteld op de tekenreeks `CreateFile` of `FlushWithClose`.

* De `contentOffset` sleutel is opgenomen in de gegevensset.

> [!NOTE]
> Als toepassingen gebruikmaken van de `PutBlockList` bewerking een nieuwe blob uploaden naar de account, de gegevens niet in deze wijzigingen opgenomen.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/new-file.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 0,
    "contentOffset": 0,
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/new-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event"></a>Microsoft.Storage.BlobDeleted gebeurtenis

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.blob.core.windows.net/testcontainer/file-to-delete.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobdeleted-event-data-lake-storage-gen2"></a>Microsoft.Storage.BlobDeleted gebeurtenis (Data Lake Storage Gen2)

Als de blob storage-account een hiërarchische naamruimte heeft, de gegevens er uitzien zoals in het vorige voorbeeld met uitzondering van de volgende wijzigingen:

* De `dataVersion` sleutel is ingesteld op een waarde van `2`.

* De `data.api` sleutel is ingesteld op de tekenreeks `DeleteFile`.

* De `url` sleutel bevat het pad `dfs.core.windows.net`.

> [!NOTE]
> Als toepassingen gebruikmaken van de `DeleteBlob` bewerking om te verwijderen van een blob in het account de gegevens niet in deze wijzigingen opgenomen.

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/file-to-delete.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
    "api": "DeleteFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/file-to-delete.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "2",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstorageblobrenamed-event"></a>Microsoft.Storage.BlobRenamed gebeurtenis

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-file.txt",
  "eventType": "Microsoft.Storage.BlobRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameFile",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-file.txt",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-file.txt",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorycreated-event"></a>Microsoft.Storage.DirectoryCreated gebeurtenis

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-new-directory",
  "eventType": "Microsoft.Storage.DirectoryCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "CreateDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-new-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectoryrenamed-event"></a>Microsoft.Storage.DirectoryRenamed gebeurtenis

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/my-renamed-directory",
  "eventType": "Microsoft.Storage.DirectoryRenamed",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "RenameDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "destinationUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-renamed-directory",
    "sourceUrl": "https://my-storage-account.dfs.core.windows.net/my-file-system/my-original-directory",
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

### <a name="microsoftstoragedirectorydeleted-event"></a>Microsoft.Storage.DirectoryDeleted gebeurtenis

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/my-storage-account",
  "subject": "/blobServices/default/containers/my-file-system/blobs/directory-to-delete",
  "eventType": "Microsoft.Storage.DirectoryDeleted",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "DeleteDirectory",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "url": "https://my-storage-account.dfs.core.windows.net/my-file-system/directory-to-delete",
    "recursive": "true", 
    "sequencer": "00000000000004420000000000028963",  
    "storageDiagnostics": {
    "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

## <a name="event-properties"></a>Eigenschappen van gebeurtenis

Een gebeurtenis heeft de volgende gegevens op het hoogste niveau:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| topic | string | Volledige resource-pad naar de bron van de gebeurtenis. Dit veld is niet beschrijfbaar. Event Grid biedt deze waarde. |
| subject | string | Uitgever gedefinieerde pad naar het onderwerp van de gebeurtenis. |
| eventType | string | Een van de geregistreerde gebeurtenis-typen voor de bron van deze gebeurtenis. |
| eventTime | string | Het moment waarop dat de gebeurtenis is gegenereerd, is afhankelijk van de UTC-tijd van de provider. |
| id | string | De unieke id voor de gebeurtenis. |
| data | object | Gebeurtenisgegevens voor BLOB-opslag. |
| dataVersion | string | De schemaversie van het gegevensobject. De uitgever definieert de schemaversie. |
| metadataVersion | string | De schemaversie van de metagegevens van de gebeurtenis. Event Grid definieert het schema van de eigenschappen op het hoogste niveau. Event Grid biedt deze waarde. |

Het gegevensobject heeft de volgende eigenschappen:

| Eigenschap | Type | Description |
| -------- | ---- | ----------- |
| api | string | De bewerking waarmee de gebeurtenis geactiveerd. |
| clientRequestId | string | een aanvraag opgegeven client-id voor de opslag-API-bewerking. Deze id kan worden gebruikt voor het correleren van diagnostische logboeken van Azure Storage met behulp van het veld 'client-request-id' in de logboeken en kan worden opgegeven in aanvragen van clients met behulp van de header 'x-ms-client-request-id'. Zie [-indeling van logboekbestand](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format). |
| requestId | string | Service gegenereerde aanvraag-id voor de storage-API-bewerking. Kan worden gebruikt om te relateren aan Azure Storage diagnostische logboeken met het veld 'aanvraag-id-header' in de logboeken en wordt geretourneerd vanaf het initiëren van API-aanroepen in de header 'x-ms-request-id'. Zie [-indeling van logboekbestand](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format). |
| eTag | string | De waarde die u gebruiken kunt voor het uitvoeren van bewerkingen voorwaardelijk. |
| contentType | string | Het inhoudstype dat is opgegeven voor de blob. |
| contentLength | geheel getal | De grootte van de blob in bytes. |
| blobType | string | Het type van de blob. Geldige waarden zijn 'BlockBlob' of 'PageBlob'. |
| contentOffset | getal | De verschuiving van een schrijfbewerking genomen op het punt waar de toepassing activeren van de gebeurtenis voltooid voor schrijven naar het bestand in bytes. <br>Wordt alleen weergegeven voor gebeurtenissen die worden geactiveerd op blob storage-accounts waarvoor een hiërarchische naamruimte.|
| destinationUrl |string | De url van het bestand dat wordt aangelegd nadat de bewerking is voltooid. Als een bestand wordt gewijzigd, bijvoorbeeld de `destinationUrl` eigenschap bevat de url van de nieuwe bestandsnaam. <br>Wordt alleen weergegeven voor gebeurtenissen die worden geactiveerd op blob storage-accounts waarvoor een hiërarchische naamruimte.|
| sourceUrl |string | De url van het bestand dat voordat de bewerking bestaat. Als een bestand wordt gewijzigd, bijvoorbeeld de `sourceUrl` bevat de url van de naam van het oorspronkelijke bestand voorafgaand aan de naamswijziging. <br>Wordt alleen weergegeven voor gebeurtenissen die worden geactiveerd op blob storage-accounts waarvoor een hiërarchische naamruimte. |
| url | string | Het pad naar de blob. <br>Als de client maakt gebruik van een Blob REST-API, wordt de url deze structuur heeft:  *\<opslagaccountnaam\>.blob.core.windows.net/\<containernaam\>/\<-bestandsnaam \>* . <br>Als de client maakt gebruik van een Data Lake Storage REST API, wordt de url deze structuur heeft:  *\<opslagaccountnaam\>.dfs.core.windows.net/\<--naam van het bestandssysteem\> / \<-bestandsnaam\>* .
|
| recursive| string| `True` de bewerking op alle onderliggende mappen; uit te voeren anders `False`. <br>Wordt alleen weergegeven voor gebeurtenissen die worden geactiveerd op blob storage-accounts waarvoor een hiërarchische naamruimte. |
| sequencer | string | Een ondoorzichtige tekenreeks-waarde voor de logische volgorde van gebeurtenissen voor elke specifieke blobnaam.  Gebruikers kunnen standaard tekenreeksvergelijking gebruiken om te begrijpen van de relatieve volgorde van de twee gebeurtenissen op de naam van de dezelfde blob. |
| storageDiagnostics | object | Diagnostische gegevens van tijd tot tijd worden opgenomen door de Azure Storage-service. Indien aanwezig, moet worden genegeerd door consumenten van gebeurtenissen. |

|Eigenschap|Type|Description|
 |-------------------|------------------------|-----------------------------------------------------------------------|

## <a name="next-steps"></a>Volgende stappen

* Zie voor een inleiding tot Azure Event Grid, [wat is Event Grid?](overview.md)
* Zie voor meer informatie over het maken van een Azure Event Grid-abonnement [Event Grid-abonnementsschema](subscription-creation-schema.md).
* Zie voor een inleiding tot het werken met gebeurtenissen van blob storage, [Route Blob-opslaggebeurtenissen - Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
