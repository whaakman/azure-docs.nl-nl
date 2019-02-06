---
title: Assets in mediaservices - Azure | Microsoft Docs
description: Dit artikel bevat een uitleg over wat activa zijn en hoe ze worden gebruikt door Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/03/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 72229a723247d6f0d68341771b073d0626ab2edb
ms.sourcegitcommit: 947b331c4d03f79adcb45f74d275ac160c4a2e83
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55745993"
---
# <a name="assets"></a>Assets

In Azure Media Services, een [Asset](https://docs.microsoft.com/rest/api/media/assets) digitale bestanden (met inbegrip van de video, audio, afbeeldingen, verzamelingen van miniaturen, tekstsporen en ondertitelingsbestanden-bestanden) en de metagegevens over deze bestanden bevat. Nadat de digitale bestanden zijn geüpload naar een Asset, kunnen ze worden gebruikt in de Media Services, codering, streaming, inhoud werkstromen analyseren. Zie voor meer informatie de [digitale bestanden uploaden naar activa](#upload-digital-files-into-assets) onderstaande sectie.

Een Asset is toegewezen aan een blobcontainer in de [Azure Storage-account](storage-account-concept.md) en de bestanden in de Asset worden opgeslagen als blok-blobs in die container. Media Services biedt ondersteuning voor Blob-lagen wanneer het account wordt gebruikt voor algemeen gebruik v2 (GPv2)-opslag. U kunt bestanden te verplaatsen met gpv2-Opslagaccounts [koude of Archiefopslag](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers). **Archief** opslag is geschikt voor het archiveren van bronbestanden wanneer u niet meer nodig hebt (bijvoorbeeld, nadat ze zijn gecodeerd).

De **archief** storage-laag wordt alleen aanbevolen voor zeer grote bronbestanden die al zijn gecodeerd en de uitvoer van de codering is geplaatst in een uitvoer-blob-container. De blobs in de uitvoercontainer die u wilt koppelen aan een Asset en om te streamen of te analyseren van uw inhoud moeten aanwezig zijn in een **warm** of **Cool** storage-laag.

> [!NOTE]
> Eigenschappen van het type datum/tijd van Asset zich altijd in UTC-notatie.

## <a name="upload-digital-files-into-assets"></a>Digitale bestanden uploaden naar activa

Een van de meestvoorkomende werkstromen zijn Media Services is om te uploaden, coderen en streamen van een bestand. In deze sectie geeft een overzicht van de algemene stappen.

1. De Media Services v3-API gebruiken om een nieuwe "tekstinvoer" Asset maken. Deze bewerking wordt een container gemaakt in het opslagaccount dat is gekoppeld aan uw Media Services-account. De API retourneert de containernaam van de (bijvoorbeeld `"container": "asset-b8d8b68a-2d7f-4d8c-81bb-8c7bbbe67ee4"`).
   
    Als u een blob-container die u wilt koppelen aan een Asset al hebt, kunt u de containernaam van de opgeven bij het maken van de Asset. Media Services ondersteunt momenteel alleen blobs in de hoofdmap van de container en niet met paden in de bestandsnaam. Dus werkt een container met de naam 'input.mp4'. Een container met de naam van het bestand "videos/inputs/input.mp4" werkt echter niet.

    De Azure CLI kunt u rechtstreeks naar een opslagaccount en container die u in uw abonnement rechten voor het uploaden. <br/>De containernaam moet uniek zijn en volgen richtlijnen voor de naamgeving opslag. De naam van de beschikt niet over de Media Services-Asset-containernaam (Asset-GUID) opmaak volgen. 
    
    ```azurecli
    az storage blob upload -f /path/to/file -c MyContainer -n MyBlob
    ```
2. Een SAS-URL met machtigingen voor lezen / schrijven die wordt gebruikt voor digitale bestanden uploaden naar de container Asset ophalen. U kunt de API van Media Services naar [de URL van de container asset](https://docs.microsoft.com/rest/api/media/assets/listcontainersas).
3. Gebruik de Azure Storage-API's of de SDK's (bijvoorbeeld de [REST API voor Storage](../../storage/common/storage-rest-api-auth.md), [JAVA SDK](../../storage/blobs/storage-quickstart-blobs-java-v10.md), of [.NET SDK](../../storage/blobs/storage-quickstart-blobs-dotnet.md)) bestanden uploaden naar de container Asset. 
4. Media Services v3 API's gebruiken om een transformatie en een taak voor het verwerken van uw "tekstinvoer" Asset te maken. Zie [Transformaties en taken](transform-concept.md) voor meer informatie.
5. De inhoud van de ' uitvoerasset ' Stream.

> [!TIP]
> Voor een volledig .NET-voorbeeld laat zien hoe u: maken van de Asset, een beschrijfbare SAS-URL ophalen van de Asset-container in de opslag, het bestand uploaden naar de container in storage met behulp van de SAS-URL, Zie [de Taakinvoer van een maken vanuit een lokaal bestand](job-input-from-local-file-how-to.md).

### <a name="create-a-new-asset"></a>Een nieuwe asset maken

#### <a name="rest"></a>REST

```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Media/mediaServices/{amsAccountName}/assets/{assetName}?api-version=2018-07-01
```

Zie voor een voorbeeld van de REST de [een Asset te maken met REST](https://docs.microsoft.com/rest/api/media/assets/createorupdate#examples) voorbeeld.

Het voorbeeld laat zien hoe maken de **aanvraagtekst** waarin u nuttige informatie, zoals beschrijving, de containernaam van de, storage-account en andere informatie kunt opgeven.

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

## <a name="filtering-ordering-paging"></a>Filters, bestellen, wisselbestand

Zie [filteren, bestellen, voor het wisselbestand van Media Services-entiteiten](entities-overview.md).

## <a name="storage-side-encryption"></a>Versleuteling van opslag aan de serverzijde

Ter bescherming van uw activa in rust, moeten de activa van de versleuteling van opslag aan de serverzijde worden versleuteld. De volgende tabel ziet u de werking van de versleuteling van opslag aan de serverzijde in Media Services:

|Optie voor opslagversleuteling|Description|Media Services v2|Media Services v3|
|---|---|---|---|
|Media Services-Storage-versleuteling|AES-256-codering, sleutel beheerd door Media Services|Ondersteund<sup>(1)</sup>|Niet ondersteund<sup>(2)</sup>|
|[Storage Service Encryption voor Data-at-Rest](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Versleuteling op de server die worden aangeboden door Azure Storage, sleutel beheerd door Azure of door de klant|Ondersteund|Ondersteund|
|[Client-Side-versleuteling van opslag](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Clientversleuteling die worden aangeboden door Azure-opslag, beheerd door de klant in Key Vault sleutel|Niet ondersteund|Niet ondersteund|

<sup>1</sup> terwijl Media Services biedt ondersteuning voor de verwerking van inhoud in de wissen/zonder enige vorm van versleuteling, als dit dus niet aanbevolen.

<sup>2</sup> in Media Services v3, versleuteling van opslag (AES-256-codering) wordt alleen ondersteund voor achterwaartse compatibiliteit bij uw activa zijn gemaakt met Media Services v2. Dit betekent dat v3 werkt met bestaande opslag versleuteld activa, maar staat niet toe dat het maken van nieuwe labels.

## <a name="next-steps"></a>Volgende stappen

* [Een bestand streamen](stream-files-dotnet-quickstart.md)
* [Met behulp van een cloud-DVR](live-event-cloud-dvr.md)
* [Verschillen tussen de Media Services v2 en v3](migrate-from-v2-to-v3.md)
