---
title: Activa in Azure mediaservices | Microsoft Docs
description: In dit artikel geeft een uitleg van wat activa zijn en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 76ed74f2df62d478b83e109a492977ec2d580198
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
---
# <a name="assets"></a>Assets

Een **Asset** digitale bestanden (inclusief video, audio, afbeeldingen, verzamelingen miniaturen, tekst tekstsporen en ondertitelingsbestanden bestanden) en de metagegevens over deze bestanden bevat. Nadat de digitale bestanden zijn geüpload naar een asset, kunnen ze in de Media Services-codering en streaming-werkstromen worden gebruikt.

Een asset is toegewezen aan een blobcontainer in de [Azure Storage-account](storage-account-concept.md) en de bestanden in de asset worden opgeslagen als blok-blobs in de container. U kunt werken met de Asset-bestanden in de containers met behulp van de opslag-SDK-clients.

Azure Media Services ondersteunt de Blob-lagen wanneer het account wordt gebruikt voor algemene doeleinden v2 (GPv2) opslag. Met GPv2, kunt u bestanden naar cool of koude opslag verplaatsen. Koude opslag is geschikt voor het archiveren van bronbestanden wanneer deze niet langer nodig is (bijvoorbeeld nadat ze zijn gecodeerd.)

In Media Services v3, kan de invoer van de taak van activa of van HTTP (s) URL's worden gemaakt. Zie het maken van een asset die kan worden gebruikt als invoer voor de taak [maken van een taak voor invoer van een lokaal bestand](job-input-from-local-file-how-to.md).

Lees ook over [storage-accounts in een Media Services](storage-account-concept.md) en [transformaties en taken](transform-concept.md).

## <a name="asset-definition"></a>Asset-definitie

De volgende tabel ziet u de eigenschappen van de Asset en geeft de definities.

|Naam|Type|Beschrijving|
|---|---|---|
|Id|tekenreeks|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|tekenreeks|De naam van de resource.|
|properties.alternateId |tekenreeks|De alternatieve ID van de Asset.|
|properties.assetId |tekenreeks|De id van de Asset.|
|Properties.container |tekenreeks|De naam van de asset blob-container.|
|Properties.created |tekenreeks|De aanmaakdatum van de Asset.|
|Properties.Description |tekenreeks|De beschrijving Asset.|
|properties.lastModified |tekenreeks|De laatste wijzigingsdatum van de activa.|
|properties.storageAccountName |tekenreeks|De naam van het opslagaccount.|
|properties.storageEncryptionFormat |AssetStorageEncryptionFormat |De coderingsindeling Asset. Een none of MediaStorageEncryption.|
|type|tekenreeks|Het type van de resource.|

Zie voor de volledige definitie [activa](https://docs.microsoft.com/rest/api/media/assets).

## <a name="filtering-ordering-and-paging-support"></a>Filteren, ordening en ondersteuning van het wisselbestand

Media Services ondersteunt de volgende opties voor OData-query voor de activa: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

### <a name="filteringordering"></a>Filteren/ordenen

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de Asset-eigenschappen: 

|Naam|Filteren|Volgorde|
|---|---|---|
|Id|Ondersteunt:<br/>Is gelijk aan<br/>Groter dan<br/>Kleiner dan|Ondersteunt:<br/>Oplopend<br/>Aflopend|
|naam|||
|properties.alternateId |Ondersteunt:<br/>Is gelijk aan||
|properties.assetId |Ondersteunt:<br/>Is gelijk aan||
|Properties.container |||
|Properties.created|Ondersteunt:<br/>Is gelijk aan<br/>Groter dan<br/>Kleiner dan|Ondersteunt:<br/>Oplopend<br/>Aflopend|
|Properties.Description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|type|||

De volgende C#-voorbeeld kunt u filteren op de datum gemaakt:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>Paginering

Paginering wordt ondersteund voor elk van de vier ingeschakelde sorteervolgorde. 

Als een queryantwoord bevat een groot aantal (momenteel meer dan 1000) items, de service retourneert een '@odata.nextLink' eigenschap ophalen van de volgende pagina van de resultaten. Dit kan worden gebruikt door de volledige resultatenset. De paginagrootte kan niet worden geconfigureerd door de gebruiker. 

Als activa zijn gemaakt of verwijderd tijdens het wisselbestand via de verzameling, worden de wijzigingen doorgevoerd in de geretourneerde resultaten (als deze wijzigingen zijn in het gedeelte van de verzameling die niet zijn gedownload.) 

De volgende C#-voorbeeld laat zien hoe opsommen via alle activa in het account.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

Zie voor voorbeelden van REST [activa - lijst](https://docs.microsoft.com/rest/api/media/assets/list)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een bestand streamen](stream-files-dotnet-quickstart.md)
