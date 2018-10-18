---
title: Assets in Azure mediaservices | Microsoft Docs
description: Dit artikel bevat een uitleg over wat activa zijn en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 10/15/2018
ms.author: juliako
ms.openlocfilehash: fcb4500a1e4503d90b00528544ae98fa93e16191
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49379211"
---
# <a name="assets"></a>Assets

Een **Asset** digitale bestanden (met inbegrip van de video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden-bestanden) en de metagegevens over deze bestanden bevat. Nadat de digitale bestanden zijn geüpload naar een asset, kunnen ze worden gebruikt in de Media Services coderen en streamen van werkstromen.

Een asset is toegewezen aan een blobcontainer in de [Azure Storage-account](storage-account-concept.md) en de bestanden in de asset worden opgeslagen als blok-blobs in die container. U kunt werken met de Asset-bestanden in de containers met behulp van de Storage-SDK-clients.

Azure Media Services biedt ondersteuning voor Blob-lagen wanneer het account wordt gebruikt voor algemeen gebruik v2 (GPv2)-opslag. Met gpv2-Opslagaccounts kunt u bestanden naar statisch of koude opslag verplaatsen. Koude opslag is geschikt voor het archiveren van bronbestanden wanneer u niet meer nodig hebt (bijvoorbeeld, nadat ze zijn gecodeerd.)

In Media Services v3, kan de invoer van de taken kan worden gemaakt van de activa of via HTTP (s) URL's. Zie voor het maken van een asset die kan worden gebruikt als invoer voor uw taak, [de Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md).

Lees ook over [storage-accounts in Media Services](storage-account-concept.md) en [transformaties en taken](transform-concept.md).

## <a name="asset-definition"></a>Asset-definitie

De volgende tabel ziet u de eigenschappen van de Asset en biedt de definities.

|Naam|Type|Beschrijving|
|---|---|---|
|id|tekenreeks|Volledig gekwalificeerde resource-ID voor de resource.|
|naam|tekenreeks|De naam van de resource.|
|properties.alternateId |tekenreeks|De alternatieve ID van de Asset.|
|properties.assetId |tekenreeks|De Asset-ID.|
|Properties.container |tekenreeks|De naam van de asset blob-container.|
|Properties.created |tekenreeks|De aanmaakdatum van de Asset.|
|Properties.Description |tekenreeks|De beschrijving van de Asset.|
|properties.lastModified |tekenreeks|Datum van de activa van de laatste wijziging.|
|properties.storageAccountName |tekenreeks|De naam van het storage-account.|
|properties.storageEncryptionFormat |AssetStorageEncryptionFormat |De coderingsindeling Asset. Een op None of MediaStorageEncryption.|
|type|tekenreeks|Het type van de resource.|

Zie voor de definitie van de volledige [activa](https://docs.microsoft.com/rest/api/media/assets).

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Media Services ondersteunt de volgende OData-queryopties voor activa: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

Beschrijving van de operator:

* EQ = gelijk zijn aan
* Ne = niet gelijk zijn aan
* Ge = groter dan of gelijk aan
* Le = kleiner dan of gelijk aan
* Gt = groter dan
* Lt = minder dan

### <a name="filteringordering"></a>Filteren/bestellen

De volgende tabel ziet u hoe deze opties kunnen worden toegepast op de Asset-eigenschappen: 

|Naam|Filteren|Bestellen|
|---|---|---|
|id|||
|naam|Ondersteunt: Eq, Gt, Lt|Ondersteunt: oplopend of aflopend|
|properties.alternateId |Ondersteunt: Eq||
|properties.assetId |Ondersteunt: Eq||
|Properties.container |||
|Properties.created|Ondersteunt: Eq, Gt, Lt| Ondersteunt: Oplopend of aflopend|
|Properties.Description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|type|||

De volgende C#-voorbeeld kunt u filteren op de aanmaakdatum:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>Paginering

Paginering wordt voor elk van de vier ingeschakelde sorteervolgorde ondersteund. Op dit moment is de paginagrootte 1000.

> [!TIP]
> U moet de volgende koppeling altijd gebruiken om inventariseren van de verzameling en niet afhankelijk van het formaat van een bepaalde pagina.

Als een query-antwoord veel items bevat, retourneert de service een "\@odata.nextLink" eigenschap om de volgende pagina van de resultaten. Dit kan worden gebruikt door de volledige resultatenset. U kunt het formaat van de pagina niet configureren. 

Als Assets zijn gemaakt of verwijderd, wanneer de verzameling worden er pagina's, worden de wijzigingen doorgevoerd in de geretourneerde resultaten (als deze wijzigingen zijn in het gedeelte van de verzameling die niet zijn gedownload.) 

De volgende C#-voorbeeld laat zien hoe om te inventariseren alle activa in het account.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

Zie voor voorbeelden van REST [activa - lijst](https://docs.microsoft.com/rest/api/media/assets/assets_list)


## <a name="storage-side-encryption"></a>Versleuteling van opslag aan de serverzijde

Ter bescherming van uw activa in rust, moeten de activa van de versleuteling van opslag aan de serverzijde worden versleuteld. De volgende tabel ziet u de werking van de versleuteling van opslag aan de serverzijde in Media Services:

|Optie voor opslagversleuteling|Beschrijving|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services-Storage-versleuteling|AES-256-codering, sleutel beheerd door Media Services|Ondersteund<sup>(1)</sup>|Niet ondersteund<sup>(2)</sup>|
|[Storage Service Encryption voor Data-at-Rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Versleuteling op de server die worden aangeboden door Azure Storage, sleutel beheerd door Azure of door de klant|Ondersteund|Ondersteund|
|[Client-Side-versleuteling van opslag](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Clientversleuteling die worden aangeboden door Azure-opslag, beheerd door de klant in Key Vault sleutel|Niet ondersteund|Niet ondersteund|

<sup>1</sup> terwijl Media Services biedt ondersteuning voor de verwerking van inhoud in de wissen/zonder enige vorm van versleuteling, als dit dus niet aanbevolen.

<sup>2</sup> in Media Services v3, versleuteling van opslag (AES-256-codering) wordt alleen ondersteund voor achterwaartse compatibiliteit bij uw activa zijn gemaakt met Media Services v2. Dit betekent dat v3 werkt met bestaande opslag versleuteld activa, maar staat niet toe dat het maken van nieuwe labels.

## <a name="next-steps"></a>Volgende stappen

[Een bestand streamen](stream-files-dotnet-quickstart.md)
