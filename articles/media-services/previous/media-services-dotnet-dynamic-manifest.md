---
title: Filters maken met Azure Media Services .NET SDK
description: In dit onderwerp wordt beschreven hoe u filters maken, zodat de client voor het specifieke secties van de stroom van een stroom gebruiken kunt. Media Services wordt gemaakt dynamische manifesten voor het bereiken van deze selectief streaming.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 02/09/2019
ms.author: juliako;cenkdin
ms.openlocfilehash: 2ee2e85188c4294060ef3effdc2d443f604aff61
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56003326"
---
# <a name="creating-filters-with-media-services-net-sdk-legacy"></a>Filters maken met Media Services .NET SDK (verouderde)
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

Media Services kunt vanaf 2,17 release, u filters voor uw assets definiëren. Deze filters zijn serverzijde regels waarmee uw klanten om te kiezen voor handelingen zoals: afspelen alleen een gedeelte van een video (in plaats van de hele video afspelen), of alleen een subset van audio en video voorinstelling die van uw klant apparaat (in plaats van verwerken kan opgeven alle de voorinstelling die gekoppeld aan de asset zijn). Deze filtering van uw activa wordt bereikt door **dynamische Manifest**s die zijn gemaakt op verzoek om te streamen van een video van uw klant op basis van opgegeven filter (s).

Zie voor meer informatie over filters en dynamische Manifest, [dynamische manifesten overzicht](media-services-dynamic-manifest-overview.md).

In dit artikel laat zien hoe Media Services .NET SDK gebruiken om te maken, bijwerken en verwijderen van filters. 

Houd er rekening mee dat als u een filter bijwerkt, duurt het tot twee minuten voor het streaming-eindpunt om te vernieuwen van de regels. Als de inhoud wordt aangeboden met dit filter (en caches in proxy's en CDN-cache), kan dit filter bijwerken resulteren in fouten player. Wis de cache altijd na het bijwerken van het filter. Als deze optie niet mogelijk is is, kunt u overwegen een ander filter. 

## <a name="types-used-to-create-filters"></a>Typen die worden gebruikt om filters te maken
De volgende typen worden gebruikt bij het maken van filters: 

* **IStreamingFilter**.  Dit type is gebaseerd op de volgende REST-API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. Dit type is gebaseerd op de volgende REST-API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. Dit type is gebaseerd op de volgende REST-API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** en **IFilterTrackPropertyCondition**. Deze typen zijn gebaseerd op de volgende REST-API's [FilterTrackSelect en FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Algemene filters maken/bijwerken/lezen/verwijderen
De volgende code toont hoe u .NET gebruiken om te maken, bijwerken, lezen en verwijderen van asset-filters.

```csharp
    string filterName = "GlobalFilter_" + Guid.NewGuid().ToString();

    List<FilterTrackSelectStatement> filterTrackSelectStatements = new List<FilterTrackSelectStatement>();

    FilterTrackSelectStatement filterTrackSelectStatement = new FilterTrackSelectStatement();
    filterTrackSelectStatement.PropertyConditions = new List<IFilterTrackPropertyCondition>();
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackNameCondition("Track Name", FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackBitrateRangeCondition(new FilterTrackBitrateRange(0, 1), FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatement.PropertyConditions.Add(new FilterTrackTypeCondition(FilterTrackType.Audio, FilterTrackCompareOperator.NotEqual));
    filterTrackSelectStatements.Add(filterTrackSelectStatement);

    // Create
    IStreamingFilter filter = _context.Filters.Create(filterName, new PresentationTimeRange(), filterTrackSelectStatements);

    // Update
    filter.PresentationTimeRange = new PresentationTimeRange(timescale: 500);
    filter.Update();

    // Read
    var filterUpdated = _context.Filters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filter.Delete();
```

## <a name="createupdatereaddelete-asset-filters"></a>Filters maken/bijwerken/lezen/verwijderen-asset
De volgende code toont hoe u .NET gebruiken om te maken, bijwerken, lezen en verwijderen van asset-filters.

```csharp
    string assetName = "AssetFilter_" + Guid.NewGuid().ToString();
    var asset = _context.Assets.Create(assetName, AssetCreationOptions.None);

    string filterName = "AssetFilter_" + Guid.NewGuid().ToString();


    // Create
    IStreamingAssetFilter filter = asset.AssetFilters.Create(filterName,
                                        new PresentationTimeRange(), 
                                        new List<FilterTrackSelectStatement>());

    // Update
    filter.PresentationTimeRange = 
            new PresentationTimeRange(start: 6000000000, end: 72000000000);

    filter.Update();

    // Read
    asset = _context.Assets.Where(c => c.Id == asset.Id).FirstOrDefault();
    var filterUpdated = asset.AssetFilters.FirstOrDefault();
    Console.WriteLine(filterUpdated.Name);

    // Delete
    filterUpdated.Delete();

```


## <a name="build-streaming-urls-that-use-filters"></a>Streaming-URL's die gebruikmaken van filters maken
Zie voor meer informatie over het publiceren en leveren van uw assets [inhoud leveren aan klanten overzicht](media-services-deliver-content-overview.md).

De volgende voorbeelden ziet hoe u filters toevoegen aan uw streaming-URL's.

**MPEG-DASH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Smooth Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Overzicht van dynamische manifesten](media-services-dynamic-manifest-overview.md)

