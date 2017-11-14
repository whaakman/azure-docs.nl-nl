---
title: Reageren op Azure Blob Storage-gebeurtenissen (preview) | Microsoft Docs
description: Gebruik Azure gebeurtenis raster om u te abonneren op gebeurtenissen van de Blob-opslag.
services: storage,event-grid
keywords: 
author: cbrooksmsft
ms.author: cbrooks
ms.date: 08/25/2017
ms.topic: article
ms.service: storage
ms.openlocfilehash: a56e6026ed0c2c873030625fa7a9b35b92faf930
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="reacting-to-blob-storage-events-preview"></a>Reageren op gebeurtenissen van Blob-opslag (preview)

Gebeurtenissen van Azure Blob storage kunnen toepassingen om te reageren op het maken en verwijderen van blobs moderne zonder server architecturen met en zonder de noodzaak van complexe code of goedkoper en inefficiënt polling-services.  In plaats daarvan gebeurtenissen worden gepusht via [Azure gebeurtenis raster](https://azure.microsoft.com/services/event-grid/) naar abonnees zoals [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), of deze zelfs uw eigen aangepaste http-listener en u alleen betalen voor wat u gebruikt.

Algemene scenario's voor Blob Storage-gebeurtenis bevatten afbeeldings- of verwerking, zoekindexering of een bestand gebaseerde werkstroom.  Asynchrone bestandsuploads zijn een goede geschikt is voor gebeurtenissen.  Wanneer wijzigingen incidentele zijn, maar uw scenario onmiddellijke reactiesnelheid vereist, kan architectuur op basis van gebeurtenissen met name efficiënt zijn.

Raster gebeurtenis is momenteel in preview en beschikbaar voor accounts in de ***West-Centraal VS*** of ***VS-West 2*** locaties.  Kijk eens naar [Route Blob storage-gebeurtenissen naar een aangepaste website-eindpunt](storage-blob-event-quickstart.md) voor een kort voorbeeld.

![Gebeurtenis raster Model](./media/storage-blob-event-overview/event-grid-functional-model.png)

## <a name="blob-storage-accounts"></a>Blob Storage-accounts
BLOB storage gebeurtenissen zijn beschikbaar in [Blob storage-accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) (en niet in opslagaccounts voor algemeen gebruik).  Een Blob Storage-account is een gespecialiseerd opslagaccount voor het opslaan van ongestructureerde gegevens als blobs (objecten) in Azure Storage. BLOB storage-accounts zijn vergelijkbaar met opslagaccounts voor algemeen gebruik en delen van de meeste duurzaamheid, beschikbaarheid, schaalbaarheid en prestaties van functies dat u vandaag inclusief 100 procent API-consistentie voor blok-blobs gebruikt en toevoeg-blobs. Voor toepassingen die alleen blok- of toevoeg-blob-opslag nodig hebben, wordt het gebruik van Blob-opslagaccounts aangeraden.

## <a name="available-blob-storage-events"></a>Beschikbare Blob storage-gebeurtenissen
Gebruikt voor gebeurtenis raster [gebeurtenisabonnementen](../../event-grid/concepts.md#event-subscriptions) gebeurtenis om berichten te routeren naar abonnees.  BLOB storage gebeurtenisabonnementen kunnen bevatten twee soorten gebeurtenissen:  

> |De naam van gebeurtenis|Beschrijving|
> |----------|-----------|
> |`Microsoft.Storage.BlobCreated`|Deze gebeurtenis wordt gestart wanneer een blob wordt gemaakt of vervangen door de `PutBlob`, `PutBlockList`, of `CopyBlob` bewerkingen|
> |`Microsoft.Storage.BlobDeleted`|Deze gebeurtenis wordt gestart wanneer een blob is verwijderd via een `DeleteBlob` bewerking|

## <a name="event-schema"></a>Gebeurtenis-Schema
BLOB storage-gebeurtenissen bevatten alle informatie die u nodig hebt om te reageren op wijzigingen in uw gegevens.  U kunt een Blob storage-gebeurtenis identificeren omdat de eigenschap eventType begint met "Microsoft.Storage."  
Meer informatie over het gebruik van eigenschappen van gebeurtenis raster gebeurtenis wordt beschreven in [gebeurtenis raster gebeurtenis schema](../../event-grid/event-schema.md).  

> |Eigenschap|Type|Beschrijving|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |Onderwerp|Tekenreeks|Volledige Azure Resource Manager-id van het opslagaccount dat de gebeurtenis verzendt.|
> |Onderwerp|Tekenreeks|De relatieve bronpad naar het object dat het onderwerp van de gebeurtenis met behulp van dezelfde uitgebreid Azure Resource Manager-indeling die we gebruiken om te beschrijven van storage-accounts, services en containers voor Azure RBAC.  Deze indeling bevat een blob-naam van letters behouden blijven.|
> |eventTime|Tekenreeks|De gebeurtenis is gegenereerd, in de ISO 8601-notatie van datum/tijd|
> |EventType|Tekenreeks|'Microsoft.Storage.BlobCreated' of 'Microsoft.Storage.BlobDeleted'|
> |Id|Tekenreeks|De unieke id als deze gebeurtenis|
> |Gegevens|object|Verzameling van blob storage-specifieke gebeurtenisgegevens|
> |data.contentType|Tekenreeks|Het inhoudstype van de blob, zoals in de header Content-Type van de blob wordt geretourneerd|
> |data.contentLength|Aantal|De grootte van de blob als geheel getal dat een aantal bytes in de header Content-Length zou worden geretourneerd van de blob in.  Met BlobCreated gebeurtenis, maar niet met BlobDeleted verzonden.|
> |Data.URL|Tekenreeks|De url van het object dat het onderwerp van de gebeurtenis is|
> |data.eTag|Tekenreeks|De etag van het object wanneer deze gebeurtenis wordt gestart.  Niet beschikbaar voor de gebeurtenis BlobDeleted.|
> |Data.API|Tekenreeks|De naam van de api-bewerking die deze gebeurtenis wordt geactiveerd.  Gebeurtenissen die BlobCreated, wordt deze waarde is 'PutBlob', 'PutBlockList' of 'CopyBlob'.  Deze waarde is BlobDeleted gebeurtenissen, 'DeleteBlob'.  Deze waarden zijn dezelfde api-namen die aanwezig in de Azure Storage diagnostische logboeken zijn.  Zie [geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).|
> |Data.sequencer|Tekenreeks|Een ondoorzichtige string-waarde die aangeeft van de logische volgorde van gebeurtenissen voor een bepaalde blob-naam.  Gebruikers kunnen standaard tekenreeksvergelijking gebruiken om te begrijpen van de relatieve volgorde van de twee gebeurtenissen op dezelfde blobnaam.|
> |data.requestId|Tekenreeks|Service gegenereerde aanvraag-id voor de opslagbewerking API.  Kan worden gebruikt om te relateren aan Azure Storage diagnostische logboeken met het veld 'aanvraag-id-header' in de logboeken en wordt geretourneerd vanaf het initiëren van API-aanroep in de header 'x-ms-aanvraag-id'. Zie [-indeling van logboekbestand](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.clientRequestId|Tekenreeks|Client-voorwaarde aanvraag-id voor de opslag API-bewerking.  Kan worden gebruikt voor correleert met Azure Storage logboeken met diagnostische gegevens met het veld 'client-request-id' in de logboeken en kan worden opgegeven in de aanvragen van clients met behulp van de header 'x-ms-client-request-id'. Zie [-indeling van logboekbestand](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.storageDiagnostics|object|Diagnostische gegevens van tijd tot tijd worden opgenomen door de Azure Storage-service.  Indien aanwezig, moet worden genegeerd door gebeurtenisconsumers.|

Hier volgt een voorbeeld van een gebeurtenis BlobCreated:
```json
[{
  "topic": "/subscriptions/319a9601-1ec0-0000-aebc-8fe82724c81e/resourceGroups/testrg/providers/Microsoft.Storage/storageAccounts/myaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/file1.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-08-16T01:57:26.005121Z",
  "id": "602a88ef-0001-00e6-1233-1646070610ea",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "799304a4-bbc5-45b6-9849-ec2c66be800a",
    "requestId": "602a88ef-0001-00e6-1233-164607000000",
    "eTag": "0x8D4E44A24ABE7F1",
    "contentType": "text/plain",
    "contentLength": 447,
    "blobType": "BlockBlob",
    "url": "https://myaccount.blob.core.windows.net/testcontainer/file1.txt",
    "sequencer": "00000000000000EB000000000000C65A",
  }
}]

```

Zie voor meer informatie [Blob storage gebeurtenissen schema](../../event-grid/event-schema-blob-storage.md).

## <a name="filtering-events"></a>Gebeurtenissen filteren
BLOB gebeurtenisabonnementen kunnen worden gefilterd op basis van het gebeurtenistype en door de naam van de container en de blob-naam van het object dat is gemaakt of verwijderd.  Onderwerp-filters in gebeurtenis raster werk op basis van "begint met" en "eindigt op" overeenkomt met, zodat de gebeurtenissen met een overeenkomende onderwerp worden geleverd op de abonnee.
De houder van gebeurtenissen voor Blob-opslag maakt gebruik van de indeling:
```json
/blobServices/default/containers/<containername>/blobs/<blobname>
```
Zodat deze overeenkomen met alle gebeurtenissen voor een opslagaccount, kunt u de filters onderwerp leeg laten.

Gebruik zodat deze overeenkomen met gebeurtenissen van blobs die zijn gemaakt in een set van containers delen een voorvoegsel, een `subjectBeginsWith` zoals filteren:
```json
/blobServices/default/containers/containerprefix
```
Zodat deze overeenkomen met gebeurtenissen van de blobs in een specifieke container gemaakt, gebruikt u een `subjectBeginsWith` zoals filteren:
```json
/blobServices/default/containers/containername/
```
Zodat deze overeenkomen met gebeurtenissen van de blobs in specifieke delen van een voorvoegsel van blob-container gemaakt, gebruikt u een `subjectBeginsWith` zoals filteren:
```json
/blobServices/default/containers/containername/blobs/blobprefix
```

Zodat deze overeenkomen met gebeurtenissen van de blobs in specifieke delen van een achtervoegsel van de blob-container gemaakt, gebruikt u een `subjectEndsWith` filter zoals 'Log' of 'jpg'

Zie voor meer informatie [gebeurtenis raster concepten](../../event-grid/concepts.md#filters).

## <a name="practices-for-consuming-events"></a>Procedures voor het gebruiken van gebeurtenissen
Toepassingen die Blob storage gebeurtenissen verwerken moeten volgen enkele aanbevolen procedures:
> [!div class="checklist"]
> * Als u meerdere abonnementen kunnen worden geconfigureerd op route-gebeurtenissen naar de dezelfde gebeurtenis-handler, is het belangrijk niet te mogen nemen gebeurtenissen zijn van een bepaalde bron, maar om te controleren van het onderwerp van het bericht om ervoor te zorgen dat ze afkomstig zijn van het opslagaccount dat u verwacht.
> * Op dezelfde manier, Controleer of de eventType één u klaarstaan om te verwerken en niet vanuit gegaan voor dat alle gebeurtenissen die u ontvangt zal de typen die u verwacht.
> * Als berichten kunnen volgorde binnenkomen, en na enige tijd duren, gebruikt u de etag-velden te begrijpen of uw gegevens over objecten nog steeds up-to-date.  De velden van de sequencer ook gebruiken om te begrijpen van de volgorde van gebeurtenissen van een bepaald object.
> * Het veld blobType gebruiken om te begrijpen wat voor soort bewerkingen zijn toegestaan in de blob en welke clientbibliotheek typt u moet gebruiken voor toegang tot de blob.
> * Gebruik het url-veld met de `CloudBlockBlob` en `CloudAppendBlob` constructors voor toegang tot de blob.
> * Velden die u niet begrijpt negeren.  Deze oefening kunt zorgen ervoor dat u robuuste op nieuwe functies die in de toekomst kunnen worden toegevoegd.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de gebeurtenis raster en probeer Blob storage-gebeurtenissen:

- [Over Event Grid](../../event-grid/overview.md)
- [Blob storage gebeurtenissen routeren naar een aangepaste website-eindpunt](storage-blob-event-quickstart.md)
