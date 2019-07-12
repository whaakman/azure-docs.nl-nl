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
ms.date: 07/02/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: d0a81d5d7ce8e7569b77007b6ad9c322cf626f16
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67670706"
---
# <a name="assets"></a>Assets

In Azure Media Services, een [Asset](https://docs.microsoft.com/rest/api/media/assets) bevat informatie over de digitale bestanden die zijn opgeslagen in Azure Storage (met inbegrip van de video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden-bestanden). 

Een Asset is toegewezen aan een blobcontainer in de [Azure Storage-account](storage-account-concept.md) en de bestanden in de Asset worden opgeslagen als blok-blobs in die container. Media Services biedt ondersteuning voor Blob-lagen wanneer het account wordt gebruikt voor algemeen gebruik v2 (GPv2)-opslag. U kunt bestanden te verplaatsen met gpv2-Opslagaccounts [koude of Archiefopslag](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). **Archief** opslag is geschikt voor het archiveren van bronbestanden wanneer u niet meer nodig hebt (bijvoorbeeld, nadat ze zijn gecodeerd).

De **archief** storage-laag wordt alleen aanbevolen voor zeer grote bronbestanden die al zijn gecodeerd en de uitvoer van de codering is geplaatst in een uitvoer-blob-container. De blobs in de uitvoercontainer die u wilt koppelen aan een Asset en om te streamen of te analyseren van uw inhoud moeten aanwezig zijn in een **warm** of **Cool** storage-laag.

## <a name="upload-digital-files-into-assets"></a>Digitale bestanden uploaden naar activa

Nadat de digitale bestanden zijn geüpload naar storage en die zijn gekoppeld aan een Asset, kunnen ze worden gebruikt in de Media Services, codering, streaming, inhoud werkstromen analyseren. Een van de meestvoorkomende werkstromen zijn Media Services is om te uploaden, coderen en streamen van een bestand. In deze sectie geeft een overzicht van de algemene stappen.

> [!TIP]
> Controleer voordat u begint met het ontwikkelen, [ontwikkelen met Media Services v3 API's](media-services-apis-overview.md) (bevat informatie over het verkrijgen van toegang tot API's voor naamgeving, enz.)

1. Gebruik de API van Media Services v3 om een asset voor nieuwe 'invoer' te maken. Met deze bewerking wordt een container gemaakt in het opslagaccount dat aan uw Media Services-account is gekoppeld. De API retourneert de containernaam van de (bijvoorbeeld `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Als u al een blobcontainer hebt die u aan een asset wilt koppelen, kunt u de containernaam opgeven tijdens het maken van de asset. Media Services ondersteunt momenteel alleen blobs in de hoofdmap van de container en geen mappen met paden in de bestandsnaam. Dus het werkt voor een container met de bestandsnaam 'input.mp4'. Het werkt echter niet voor de containernaam 'videos/inputs/input.mp4'.

    U kunt de Azure CLI gebruiken om bestanden rechtstreeks in een opslagaccount en container te uploaden waarvoor u rechten hebt in uw abonnement. <br/>De containernaam moet uniek zijn en de richtlijnen volgen voor de naamgeving van de opslag. De naam hoeft niet de indeling te volgen voor de containernaam van de Media Services-asset (Asset-GUID). 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Haal een SAS-URL op met lees-/schrijfmachtigingen die wordt gebruikt voor het uploaden van digitale bestanden in de asset-container. U kunt de Media Services-API gebruiken voor het [vermelden van de URL's voor de asset-container](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Gebruik de Azure Storage-API's of SDK's (bijvoorbeeld de [Storage REST API](../../storage/common/storage-rest-api-auth.md), [JAVA SDK](../../storage/blobs/storage-quickstart-blobs-java-v10.md) of [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) voor het uploaden van bestanden in de asset-container. 
4. Gebruik Media Services v3-API's om een transformatie en een taak te maken om uw 'invoer'-asset te verwerken. Zie [Transformaties en taken](transform-concept.md) voor meer informatie.
5. De inhoud van de ' uitvoerasset ' Stream.

Voor een volledig .NET-voorbeeld laat zien hoe u: maken van de Asset, een beschrijfbare SAS-URL ophalen van de Asset-container in de opslag, het bestand uploaden naar de container in storage met behulp van de SAS-URL, Zie [de Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Een nieuwe asset maken

> [!NOTE]
> Eigenschappen van het type datum/tijd van Asset zich altijd in UTC-notatie.

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

Zie voor een voorbeeld van de REST de [een Asset te maken met REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) voorbeeld.

Het voorbeeld laat zien hoe de **aanvraagbody** wordt gemaakt. U kunt nuttige informatie opgeven, zoals een beschrijving, containernaam, opslagaccount en andere gegevens.

#### <a name="curl"></a>cURL

```cURL
curl -X PUT \
  'https://management.azure.com/subscriptions/00000000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/Microsoft.Media/mediaServices/amsAccountName/assets/myOutputAsset?api-version=2018-07-01' \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "properties": {
    "description": "",
  }
}'
```

#### <a name="net"></a>.NET

```csharp
 Asset asset = await client.Assets.CreateOrUpdateAsync(resourceGroupName, accountName, assetName, new Asset());
```

Zie voor een compleet voorbeeld [de Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md). In Media Services v3, kan ook invoer van een taak van HTTPS-URL's worden gemaakt (Zie [de Taakinvoer van een maken van een HTTPS-URL](job-input-from-http-how-to.md)).

## <a name="map-v3-asset-properties-to-v2"></a>V3 asset eigenschappen toewijzen aan v2

De volgende tabel toont hoe de [Asset](https://docs.microsoft.com/rest/api/media/assets/createorupdate#asset)van eigenschappen in v3 toewijzen aan de eigenschappen van actief in v2.

|V3-eigenschappen|v2-eigenschappen|
|---|---|
|id - (unieke) het volledige pad in de Azure Resource Manager, Zie de voorbeelden in [Asset](https://docs.microsoft.com/rest/api/media/assets/createorupdate)||
|naam: (unieke) Zie [naamconventies](media-services-apis-overview.md#naming-conventions) ||
|alternateId|AlternateId|
|assetId|ID - waarde (unieke) begint met de `nb:cid:UUID:` voorvoegsel.|
|Gemaakt|Gemaakt|
|description|Name|
|lastModified|lastModified|
|storageAccountName|StorageAccountName|
|storageEncryptionFormat| Opties (opties voor het maken)|
|type||

## <a name="storage-side-encryption"></a>Versleuteling van opslag aan de serverzijde

Ter bescherming van uw activa in rust, moeten de activa van de versleuteling van opslag aan de serverzijde worden versleuteld. De volgende tabel ziet u de werking van de versleuteling van opslag aan de serverzijde in Media Services:

|Optie voor opslagversleuteling|Description|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services-Storage-versleuteling|AES-256-codering, sleutel beheerd door Media Services|Ondersteund<sup>(1)</sup>|Niet ondersteund<sup>(2)</sup>|
|[Storage Service Encryption voor Data-at-Rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Versleuteling op de server die worden aangeboden door Azure Storage, sleutel beheerd door Azure of door de klant|Ondersteund|Ondersteund|
|[Client-Side-versleuteling van opslag](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Clientversleuteling die worden aangeboden door Azure-opslag, beheerd door de klant in Key Vault sleutel|Niet ondersteund|Niet ondersteund|

<sup>1</sup> terwijl Media Services biedt ondersteuning voor de verwerking van inhoud in de wissen/zonder enige vorm van versleuteling, als dit dus niet aanbevolen.

<sup>2</sup> in Media Services v3, versleuteling van opslag (AES-256-codering) wordt alleen ondersteund voor achterwaartse compatibiliteit bij uw activa zijn gemaakt met Media Services v2. Dit betekent dat v3 werkt met bestaande opslag versleuteld activa, maar staat niet toe dat het maken van nieuwe labels.

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Zie [filteren, bestellen, voor het wisselbestand van Media Services-entiteiten](entities-overview.md).

## <a name="next-steps"></a>Volgende stappen

* [Een bestand streamen](stream-files-dotnet-quickstart.md)
* [Een cloud-DVR gebruiken](live-event-cloud-dvr.md)
* [Verschillen tussen de Media Services v2 en v3](migrate-from-v2-to-v3.md)
