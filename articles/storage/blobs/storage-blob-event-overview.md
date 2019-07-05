---
title: Reageren op gebeurtenissen van Azure Blob storage | Microsoft Docs
description: Gebruik Azure Event Grid om u te abonneren op gebeurtenissen van Blob Storage.
services: storage,event-grid
author: cbrooksmsft
ms.author: cbrooks
ms.date: 01/30/2018
ms.topic: article
ms.service: storage
ms.subservice: blobs
ms.openlocfilehash: c0655d02fd5d0d64c22db286236b2a26f9e70619
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444687"
---
# <a name="reacting-to-blob-storage-events"></a>Reageren op gebeurtenissen van Blob storage

Gebeurtenissen van Azure Storage kunnen toepassingen om te reageren op gebeurtenissen, zoals het maken en verwijderen van blobs, met behulp van moderne architecturen zonder servers. Dit gebeurt zonder de noodzaak voor complexe code of kostbaar en inefficiënt polling-services.

In plaats daarvan de gebeurtenissen worden gepusht via [Azure Event Grid](https://azure.microsoft.com/services/event-grid/) voor abonnees zoals Azure Functions, Azure Logic Apps, of zelfs naar uw eigen aangepaste http-listener en u betaalt alleen voor wat u gebruikt.

Gebeurtenissen van BLOB storage zijn betrouwbaar verzonden naar de Event Grid-service waarmee u betrouwbare van leveringsservices aan uw toepassingen via beleid voor uitgebreide opnieuw proberen en dead-letter uitvoeren voor levering.

Algemene Blob storage event-scenario's omvatten afbeelding of video verwerking, zoekindexen of elke werkstroom bestand gerichte. Asynchrone bestandsuploads zijn uitstek geschikt voor de gebeurtenissen. Wanneer wijzigingen incidentele zijn, maar uw scenario direct reactietijd is vereist, kan architectuur op basis van gebeurtenissen met name efficiënt zijn.

Als u deze nu uitproberen wilt, ziet u een van deze quickstart-artikelen:

|Als u dit hulpprogramma gebruiken wilt:    |Raadpleeg dit artikel: |
|--|-|
|Azure-portal    |[Snelstart: Gebeurtenissen van Blob storage naar eindpunt met de Azure-portal op het web routeren](https://docs.microsoft.com/azure/event-grid/blob-event-quickstart-portal?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|Azure-CLI    |[Snelstart: Opslaggebeurtenissen doorsturen naar eindpunt op het web met PowerShell](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart-powershell?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|
|PowerShell    |[Snelstart: Opslaggebeurtenissen doorsturen naar eindpunt op het web met Azure CLI](https://docs.microsoft.com/azure/storage/blobs/storage-blob-event-quickstart?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)|

## <a name="the-event-model"></a>Het gebeurtenismodel

Maakt gebruik van Event Grid [gebeurtenisabonnementen](../../event-grid/concepts.md#event-subscriptions) gebeurtenis om berichten te routeren voor abonnees. Deze afbeelding ziet u de relatie tussen gebeurtenisuitgevers gebeurtenisabonnementen en gebeurtenis-handlers.

![Event Grid-Model](./media/storage-blob-event-overview/event-grid-functional-model.png)

Abonneer u eerst een eindpunt op een gebeurtenis. Vervolgens, wanneer een gebeurtenis wordt geactiveerd, de Event Grid-service gegevens over die gebeurtenis naar het eindpunt doorsturen wordt.

Zie de [gebeurtenissenschema van Blob storage](../../event-grid/event-schema-blob-storage.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) artikel om weer te geven:

> [!div class="checklist"]
> * Een volledige lijst van gebeurtenissen van Blob storage en hoe elke gebeurtenis wordt geactiveerd.
> * Een voorbeeld van de gegevens van de Event Grid wilt voor elk van deze gebeurtenissen verzenden.
> * Het doel van elke sleutel-waardepaar dat wordt weergegeven in de gegevens.

## <a name="filtering-events"></a>Gebeurtenissen filteren

Gebeurtenisabonnementen voor BLOB kunnen worden gefilterd op basis van het gebeurtenistype en door de containernaam en blobnaam van het object dat is gemaakt of verwijderd.  Filters kunnen worden toegepast op gebeurtenisabonnementen ofwel tijdens de [maken](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest) van het gebeurtenisabonnement of [op een later tijdstip](/cli/azure/eventgrid/event-subscription?view=azure-cli-latest). Filters in Event Grid werken op basis van onderwerp 'begint met' en 'eindigt op"overeenkomsten zijn, zodat gebeurtenissen met een overeenkomende onderwerp worden geleverd op de abonnee.

Zie voor meer informatie over het toepassen van filters, [gebeurtenissen filteren op Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).

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
> * Als u wilt ervoor te zorgen dat de **Microsoft.Storage.BlobCreated** gebeurtenis wordt geactiveerd, alleen wanneer een blok-Blob is volledig toegewezen, het filteren van de gebeurtenis voor de `CopyBlob`, `PutBlob`, `PutBlockList` of `FlushWithClose` REST API-aanroepen. Deze API-aanroepen trigger de **Microsoft.Storage.BlobCreated** gebeurtenis alleen nadat gegevens volledig doorgevoerd naar een blok-Blob zijn. Zie voor meer informatie over het maken van een filter, [gebeurtenissen filteren op Event Grid](https://docs.microsoft.com/azure/event-grid/how-to-filter-events).


## <a name="next-steps"></a>Volgende stappen

Meer informatie over Event Grid en gebeurtenissen van Blob storage een probeer:

- [Over Event Grid](../../event-grid/overview.md)
- [Blob-opslag gebeurtenissen routeren naar een aangepaste web-eindpunt](storage-blob-event-quickstart.md)
