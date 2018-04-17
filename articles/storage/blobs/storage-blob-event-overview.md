---
title: Reageren op gebeurtenissen van Azure Blob storage | Microsoft Docs
description: Gebruik Azure Event Grid om u te abonneren op gebeurtenissen van Blob Storage.
services: storage,event-grid
keywords: ''
author: cbrooksmsft
ms.author: cbrooks
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.openlocfilehash: 2762466c0130ead36372a93f4c3b852cb378a02a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="reacting-to-blob-storage-events"></a>Reageren op gebeurtenissen voor Blob-opslag

Gebeurtenissen van Azure Storage kunnen toepassingen om te reageren op het maken en verwijderen van blobs met moderne zonder server architecturen. Zonder de noodzaak van complexe code of goedkoper en inefficiënt polling-services.  In plaats daarvan gebeurtenissen worden gepusht via [Azure gebeurtenis raster](https://azure.microsoft.com/services/event-grid/) naar abonnees zoals [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), of deze zelfs uw eigen aangepaste http-listener en u alleen betalen voor wat u gebruikt. 

Algemene Blob storage gebeurtenis scenario's omvatten afbeeldings- of verwerking, zoekindexering of een bestand gebaseerde werkstroom.  Asynchrone bestandsuploads zijn een goede geschikt is voor gebeurtenissen.  Wanneer wijzigingen incidentele zijn, maar uw scenario onmiddellijke reactiesnelheid vereist, kan architectuur op basis van gebeurtenissen met name efficiënt zijn.

De beschikbaarheid van gebeurtenissen voor opslag is gekoppeld aan de gebeurtenis raster [beschikbaarheid](../../event-grid/overview.md) en zijn binnenkort beschikbaar in andere regio's zoals het raster gebeurtenis. Kijk eens naar [Route Blob storage gebeurtenissen naar een aangepaste web-eindpunt - CLI](storage-blob-event-quickstart.md) of [Route Blob storage gebeurtenissen naar een aangepaste web-eindpunt - PowerShell](storage-blob-event-quickstart-powershell.md) voor een kort voorbeeld. 

![Gebeurtenis raster Model](./media/storage-blob-event-overview/event-grid-functional-model.png)

## <a name="blob-storage-accounts"></a>Blob Storage-accounts
BLOB storage gebeurtenissen zijn beschikbaar in [Blob storage-accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) en in [opslagaccounts voor algemeen gebruik v2](../common/storage-account-options.md#general-purpose-v2). **Algemene doel v2 (GPv2)** storage-accounts die ondersteuning bieden voor alle functies voor storage-services, met inbegrip van Blobs, bestanden, wachtrijen en tabellen zijn. Een **Blob-opslagaccount** is een gespecialiseerd opslagaccount voor het opslaan van ongestructureerde gegevens als blobs (objecten) in Azure Storage. BLOB storage-accounts zijn vergelijkbaar met opslagaccounts voor algemeen gebruik en delen van de meeste duurzaamheid, beschikbaarheid, schaalbaarheid en prestaties van functies dat u vandaag inclusief 100 procent API-consistentie voor blok-blobs gebruikt en toevoeg-blobs. Voor toepassingen die alleen blok- of toevoeg-blob-opslag nodig hebben, wordt het gebruik van Blob-opslagaccounts aangeraden. 

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
> |Onderwerp|tekenreeks|Volledige Azure Resource Manager-id van het opslagaccount dat de gebeurtenis verzendt.|
> |Onderwerp|tekenreeks|De relatieve bronpad naar het object dat het onderwerp van de gebeurtenis met behulp van dezelfde uitgebreid Azure Resource Manager-indeling die we gebruiken om te beschrijven van storage-accounts, services en containers voor Azure RBAC.  Deze indeling bevat een blob-naam van letters behouden blijven.|
> |eventTime|tekenreeks|De gebeurtenis is gegenereerd, in de ISO 8601-notatie van datum/tijd|
> |EventType|tekenreeks|'Microsoft.Storage.BlobCreated' of 'Microsoft.Storage.BlobDeleted'|
> |Id|tekenreeks|De unieke id als deze gebeurtenis|
> |dataVersion|tekenreeks|De versie van het schema van het gegevensobject.|
> |metadataVersion|tekenreeks|De schemaversie van de eigenschappen van het hoogste niveau.|
> |gegevens|object|Verzameling van blob storage-specifieke gebeurtenisgegevens|
> |data.contentType|tekenreeks|Het inhoudstype van de blob, zoals in de header Content-Type van de blob wordt geretourneerd|
> |data.contentLength|getal|De grootte van de blob als geheel getal dat een aantal bytes in de header Content-Length zou worden geretourneerd van de blob in.  Met BlobCreated gebeurtenis, maar niet met BlobDeleted verzonden.|
> |data.url|tekenreeks|De url van het object dat het onderwerp van de gebeurtenis is|
> |data.eTag|tekenreeks|De etag van het object wanneer deze gebeurtenis wordt gestart.  Niet beschikbaar voor de gebeurtenis BlobDeleted.|
> |data.api|tekenreeks|De naam van de api-bewerking die deze gebeurtenis wordt geactiveerd.  Gebeurtenissen die BlobCreated, wordt deze waarde is 'PutBlob', 'PutBlockList' of 'CopyBlob'.  Deze waarde is BlobDeleted gebeurtenissen, 'DeleteBlob'.  Deze waarden zijn dezelfde api-namen die aanwezig in de Azure Storage diagnostische logboeken zijn.  Zie [geregistreerde bewerkingen en statusberichten](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).|
> |Data.sequencer|tekenreeks|Een ondoorzichtige string-waarde die aangeeft van de logische volgorde van gebeurtenissen voor een bepaalde blob-naam.  Gebruikers kunnen standaard tekenreeksvergelijking gebruiken om te begrijpen van de relatieve volgorde van de twee gebeurtenissen op dezelfde blobnaam.|
> |data.requestId|tekenreeks|Service gegenereerde aanvraag-id voor de opslagbewerking API.  Kan worden gebruikt om te relateren aan Azure Storage diagnostische logboeken met het veld 'aanvraag-id-header' in de logboeken en wordt geretourneerd vanaf het initiëren van API-aanroep in de header 'x-ms-aanvraag-id'. Zie [-indeling van logboekbestand](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.clientRequestId|tekenreeks|Client-voorwaarde aanvraag-id voor de opslag API-bewerking.  Kan worden gebruikt voor correleert met Azure Storage logboeken met diagnostische gegevens met het veld 'client-request-id' in de logboeken en kan worden opgegeven in de aanvragen van clients met behulp van de header 'x-ms-client-request-id'. Zie [-indeling van logboekbestand](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.storageDiagnostics|object|Diagnostische gegevens van tijd tot tijd worden opgenomen door de Azure Storage-service.  Indien aanwezig, moet worden genegeerd door gebeurtenisconsumers.|
|data.blobType|tekenreeks|Het type van de blob. Geldige waarden zijn 'BlockBlob' of 'PageBlob'.| 

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
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]

```

Zie voor meer informatie [Blob storage gebeurtenissen schema](../../event-grid/event-schema-blob-storage.md).

## <a name="filtering-events"></a>Gebeurtenissen filteren
BLOB gebeurtenisabonnementen kunnen worden gefilterd op basis van het gebeurtenistype en door de naam van de container en de blob-naam van het object dat is gemaakt of verwijderd.  Filters kunnen worden toegepast op de gebeurtenisabonnementen ofwel tijdens de [maken](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create) van het gebeurtenisabonnement of [op een later tijdstip](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update). Onderwerp-filters in gebeurtenis raster werk op basis van "begint met" en "eindigt op" overeenkomt met, zodat de gebeurtenissen met een overeenkomende onderwerp worden geleverd op de abonnee. 

De houder van gebeurtenissen voor Blob-opslag maakt gebruik van de indeling:

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

Zodat deze overeenkomen met alle gebeurtenissen voor een opslagaccount, kunt u de filters onderwerp leeg laten.

Gebruik zodat deze overeenkomen met gebeurtenissen van blobs die zijn gemaakt in een set van containers delen een voorvoegsel, een `subjectBeginsWith` zoals filteren:

```
/blobServices/default/containers/containerprefix
```

Zodat deze overeenkomen met gebeurtenissen van de blobs in een specifieke container gemaakt, gebruikt u een `subjectBeginsWith` zoals filteren:

```
/blobServices/default/containers/containername/
```

Zodat deze overeenkomen met gebeurtenissen van de blobs in specifieke delen van een voorvoegsel van blob-container gemaakt, gebruikt u een `subjectBeginsWith` zoals filteren:

```
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
> * Het veld blobType gebruiken om te begrijpen wat voor soort bewerkingen zijn toegestaan in de blob en welke clientbibliotheek typt u moet gebruiken voor toegang tot de blob. Geldige waarden zijn `BlockBlob` of `PageBlob`. 
> * Gebruik het url-veld met de `CloudBlockBlob` en `CloudAppendBlob` constructors voor toegang tot de blob.
> * Velden die u niet begrijpt negeren.  Deze oefening kunt zorgen ervoor dat u robuuste op nieuwe functies die in de toekomst kunnen worden toegevoegd.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over de gebeurtenis raster en probeer Blob storage-gebeurtenissen:

- [Over Event Grid](../../event-grid/overview.md)
- [Blob storage gebeurtenissen routeren naar een aangepaste website-eindpunt](storage-blob-event-quickstart.md)
