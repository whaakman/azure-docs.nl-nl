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
ms.openlocfilehash: 68d722338562d21d59dd720250a62b8603c8af43
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39076144"
---
# <a name="reacting-to-blob-storage-events"></a>Reageren op gebeurtenissen van Blob storage

Gebeurtenissen van Azure Storage kunnen toepassingen om te reageren op het maken en verwijderen van blobs met moderne architecturen zonder servers. Dit gebeurt zonder de noodzaak voor complexe code of kostbaar en inefficiënt polling-services.  In plaats daarvan de gebeurtenissen worden gepusht via [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) voor abonnees zoals [Azure Functions](https://azure.microsoft.com/services/functions/), [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/), of zelfs naar uw eigen aangepaste http-listener en u alleen Betaal voor wat u gebruikt. 

Algemene Blob storage event-scenario's omvatten afbeelding of video verwerking, zoekindexen of elke werkstroom bestand gerichte.  Asynchrone bestandsuploads zijn uitstek geschikt voor de gebeurtenissen.  Wanneer wijzigingen incidentele zijn, maar uw scenario direct reactietijd is vereist, kan architectuur op basis van gebeurtenissen met name efficiënt zijn.

De beschikbaarheid van gebeurtenissen van Storage is gekoppeld aan Event Grid [beschikbaarheid](../../event-grid/overview.md) en worden pas beschikbaar in andere regio's als Event Grid. Kijk eens [Route Blob-opslaggebeurtenissen aan een aangepaste web-eindpunt - CLI](storage-blob-event-quickstart.md) of [Route Blob-opslaggebeurtenissen aan een aangepaste web-eindpunt - PowerShell](storage-blob-event-quickstart-powershell.md) voor een kort voorbeeld. 

![Event Grid-Model](./media/storage-blob-event-overview/event-grid-functional-model.png)

## <a name="blob-storage-accounts"></a>Blob Storage-accounts
Gebeurtenissen van BLOB storage zijn beschikbaar in [Blob storage-accounts](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#blob-storage-accounts) en [opslagaccounts voor algemeen gebruik v2](../common/storage-account-options.md#general-purpose-v2-accounts). **GPv2-accounts (v2 voor algemeen gebruik)** zijn opslagaccounts die alle functies voor alle opslagservices ondersteunen, waaronder Blobs, Files, Queues en Tables. Een **Blob-opslagaccount** is een opslagaccount dat speciaal is bedoeld om ongestructureerde gegevens als blobs (objecten) op te slaan in Azure Storage. Blob-opslagaccounts zijn vergelijkbaar met de opslagaccounts voor algemeen gebruik en bieden dezelfde hoogwaardige kenmerken op het gebied van duurzaamheid, beschikbaarheid, schaalbaarheid en prestaties waarover u nu al beschikt, inclusief 100 procent API-consistentie voor blok-blobs en toevoeg-blobs. Voor toepassingen die alleen blok- of toevoeg-blob-opslag nodig hebben, wordt het gebruik van Blob-opslagaccounts aangeraden. 

## <a name="available-blob-storage-events"></a>Beschikbare gebeurtenissen van Blob storage
Maakt gebruik van Event grid [gebeurtenisabonnementen](../../event-grid/concepts.md#event-subscriptions) gebeurtenis om berichten te routeren voor abonnees.  Gebeurtenisabonnementen voor BLOB-opslag kunnen twee soorten gebeurtenissen omvatten:  

> |Naam van de gebeurtenis|Beschrijving|
> |----------|-----------|
> |`Microsoft.Storage.BlobCreated`|Geactiveerd wanneer een blob wordt gemaakt of vervangen door de `PutBlob`, `PutBlockList`, of `CopyBlob` bewerkingen|
> |`Microsoft.Storage.BlobDeleted`|Geactiveerd wanneer een blob wordt verwijderd via een `DeleteBlob` bewerking|

## <a name="event-schema"></a>Gebeurtenisschema
Gebeurtenissen van BLOB storage bevatten alle informatie die u nodig hebt om te reageren op wijzigingen in uw gegevens.  Omdat de eigenschap type gebeurtenis met "Microsoft.Storage begint", kunt u een Blob storage-gebeurtenis identificeren. Meer informatie over het gebruik van de eigenschappen van Event Grid-gebeurtenis wordt gedocumenteerd in [Event Grid-gebeurtenisschema](../../event-grid/event-schema.md).  

> |Eigenschap|Type|Beschrijving|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |onderwerp|tekenreeks|Volledige Azure Resource Manager-id van het opslagaccount dat u de gebeurtenis verzendt.|
> |Onderwerp|tekenreeks|De relatieve resourcepad naar het object dat het onderwerp van de gebeurtenis is met behulp van dezelfde uitgebreid Azure Resource Manager-indeling die we gebruiken om te beschrijven van storage-accounts, services en containers voor Azure RBAC.  Deze indeling bevat een blobnaam letters behouden blijven.|
> |eventTime|tekenreeks|Datum/tijd die de gebeurtenis is gegenereerd, in ISO 8601-notatie|
> |type gebeurtenis|tekenreeks|"Microsoft.Storage.BlobCreated" of "Microsoft.Storage.BlobDeleted"|
> |Id|tekenreeks|De unieke id als deze gebeurtenis|
> |dataVersion|tekenreeks|De schemaversie van het gegevensobject.|
> |metadataVersion|tekenreeks|De schemaversie van de eigenschappen van het hoogste niveau.|
> |gegevens|object|Verzamelen van gegevens voor blob storage-specifieke gebeurtenis|
> |data.contentType|tekenreeks|Het inhoudstype van de blob, zoals in de header Content-Type worden geretourneerd met de blob|
> |data.contentLength|getal|De grootte van de blob zoals integer voor een aantal bytes, zoals in de Content-Length-header worden geretourneerd met de blob in.  Met BlobCreated gebeurtenis, maar niet met BlobDeleted verzonden.|
> |data.url|tekenreeks|De url van het object dat het onderwerp van de gebeurtenis|
> |data.eTag|tekenreeks|De etag van het object wanneer deze gebeurtenis wordt gestart.  Niet beschikbaar voor de gebeurtenis BlobDeleted.|
> |data.api|tekenreeks|De naam van de api-bewerking die deze gebeurtenis wordt geactiveerd. Deze waarde is voor BlobCreated gebeurtenissen, 'PutBlob', 'PutBlockList' of 'CopyBlob'. Deze waarde is voor BlobDeleted gebeurtenissen, 'DeleteBlob'. Deze waarden zijn de api-namen die aanwezig in de diagnostische logboeken van Azure Storage zijn. Zie [vastgelegd Operations en statusberichten](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).|
> |Data.sequencer|tekenreeks|Een ondoorzichtige tekenreeks-waarde voor de logische volgorde van gebeurtenissen voor elke specifieke blobnaam.  Gebruikers kunnen standaard tekenreeksvergelijking gebruiken om te begrijpen van de relatieve volgorde van de twee gebeurtenissen op de naam van de dezelfde blob.|
> |data.requestId|tekenreeks|Service gegenereerde aanvraag-id voor de storage-API-bewerking. Kan worden gebruikt om te relateren aan Azure Storage diagnostische logboeken met het veld 'aanvraag-id-header' in de logboeken en wordt geretourneerd vanaf het initiëren van API-aanroepen in de header 'x-ms-request-id'. Zie [-indeling van logboekbestand](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format).|
> |data.clientRequestId|tekenreeks|De opgegeven client aanvraag-id voor de opslag-API-bewerking. Kan worden gebruikt voor het correleren van diagnostische logboeken van Azure Storage met behulp van het veld 'client-request-id' in de logboeken en kan worden opgegeven in aanvragen van clients met behulp van de header 'x-ms-client-request-id'. Zie [-indeling van logboekbestand](https://docs.microsoft.com/rest/api/storageservices/storage-analytics-log-format). |
> |data.storageDiagnostics|object|Diagnostische gegevens van tijd tot tijd worden opgenomen door de Azure Storage-service. Indien aanwezig, moet worden genegeerd door consumenten van gebeurtenissen.|
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

Zie voor meer informatie, [gebeurtenissenschema van Blob storage](../../event-grid/event-schema-blob-storage.md).

## <a name="filtering-events"></a>Gebeurtenissen filteren
Gebeurtenisabonnementen voor BLOB kunnen worden gefilterd op basis van het gebeurtenistype en door de containernaam en blobnaam van het object dat is gemaakt of verwijderd.  Filters kunnen worden toegepast op gebeurtenisabonnementen ofwel tijdens de [maken](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create) van het gebeurtenisabonnement of [op een later tijdstip](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update). Filters in Event Grid werken op basis van onderwerp 'begint met' en 'eindigt op"overeenkomsten zijn, zodat gebeurtenissen met een overeenkomende onderwerp worden geleverd op de abonnee. 

Het onderwerp van gebeurtenissen van Blob storage maakt gebruik van de indeling:

```
/blobServices/default/containers/<containername>/blobs/<blobname>
```

Om alle gebeurtenissen voor een storage-account, kunt u de onderwerp-filters leeg laten.

Gebruiken om gebeurtenissen van de blobs die zijn gemaakt in een set van containers voor het delen van een voorvoegsel, een `subjectBeginsWith` filteren, zoals:

```
/blobServices/default/containers/containerprefix
```

Gebruiken om gebeurtenissen van BLOB's in een specifieke container gemaakt, een `subjectBeginsWith` filteren, zoals:

```
/blobServices/default/containers/containername/
```

Gebruiken om gebeurtenissen van BLOB's in specifieke delen van het voorvoegsel van een blob-container gemaakt, een `subjectBeginsWith` filteren, zoals:

```
/blobServices/default/containers/containername/blobs/blobprefix
```

Gebruiken om gebeurtenissen van BLOB's in specifieke delen van een achtervoegsel van de blob-container gemaakt, een `subjectEndsWith` filter, zoals 'Log' of '.jpg'. Zie voor meer informatie, [concepten binnen Event Grid](../../event-grid/concepts.md#event-subscriptions).

## <a name="practices-for-consuming-events"></a>Procedures voor het gebruik van gebeurtenissen
Toepassingen die werken met Blob-opslaggebeurtenissen moeten volgen enkele aanbevolen procedures:
> [!div class="checklist"]
> * Als u meerdere abonnementen kunnen worden geconfigureerd om gebeurtenissen routeren naar de dezelfde gebeurtenis-handler, is het belangrijk niet wordt ervan uitgegaan dat gebeurtenissen worden van een specifieke bron, maar om te controleren of het onderwerp van het bericht om ervoor te zorgen dat het afkomstig is van het opslagaccount dat u verwacht.
> * Op dezelfde manier, Controleer of het type gebeurtenis een u bent voorbereid om te verwerken en kan niet vanuit gegaan is dat alle gebeurtenissen die u ontvangt zal de typen die u verwacht.
> * Als berichten kunnen niet de juiste volgorde binnenkomen, en na enige tijd duren, gebruikt u de etag-velden om te begrijpen of uw gegevens over objecten nog steeds up-to-date is.  De velden van de sequencer ook gebruiken om te begrijpen van de volgorde van gebeurtenissen op een bepaald object.
> * Het veld blobType gebruiken om te begrijpen wat voor soort bewerkingen zijn toegestaan voor de blob en welke client-bibliotheek van het type u moet gebruiken voor toegang tot de blob. Geldige waarden zijn `BlockBlob` of `PageBlob`. 
> * Gebruik de url-veld met de `CloudBlockBlob` en `CloudAppendBlob` constructors voor toegang tot de blob.
> * Velden die u niet machtig bent negeren. Met deze procedure krijgt u flexibele houden bij de nieuwe functies die in de toekomst kunnen worden toegevoegd.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over Event Grid en gebeurtenissen van Blob storage een probeer:

- [Over Event Grid](../../event-grid/overview.md)
- [Blob-opslag gebeurtenissen routeren naar een aangepaste web-eindpunt](storage-blob-event-quickstart.md)
