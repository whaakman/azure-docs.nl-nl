---
title: Filters maken met Azure Media Services .NET SDK
description: Dit onderwerp beschrijft het maken van filters zodat de client naar specifieke secties van de stroom van een stroom gebruiken kan. Media Services maakt dynamische manifesten zodat deze selectief streaming.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 2f6894ca-fb43-43c0-9151-ddbb2833cafd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 12/07/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: 8ffd310573d0800593bd9d93d74da4bcece61fa4
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="creating-filters-with-azure-media-services-net-sdk"></a>Filters maken met Azure Media Services .NET SDK
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

Media Services kunt vanaf 2,17 versie, u filters definiëren voor de activa. Deze filters zijn serverzijde regels waarmee uw klanten te kiezen voor handelingen zoals: afspelen alleen een gedeelte van een video (in plaats van de hele video afspelen), of geef alleen een subset van audio en video vertoningen dat uw klant apparaat (in plaats van kan verwerken alle de vertoningen die gekoppeld aan de asset zijn). Dit filteren van uw assets wordt bereikt door **dynamische Manifest**s die zijn gemaakt op verzoek om te streamen van een video van uw klant op basis van opgegeven filter.

Zie voor meer informatie over filters en dynamische Manifest, [dynamische manifesten overzicht](media-services-dynamic-manifest-overview.md).

Dit artikel laat zien hoe u met Media Services .NET SDK maken, bijwerken en verwijderen van de filters. 

Houd er rekening mee dat als u een filter bijwerkt, het kan maximaal twee minuten duren voordat streaming-eindpunt om te vernieuwen van de regels. Als de inhoud is uitgevoerd met dit filter (en in het cachegeheugen van proxy's en CDN caches), kan het bijwerken van dit filter leiden tot player-fouten. Wis de cache altijd na het bijwerken van het filter. Als deze optie niet mogelijk is, kunt u overwegen een ander filter. 

## <a name="types-used-to-create-filters"></a>Typen die worden gebruikt om filters te maken
De volgende typen worden gebruikt wanneer u filters maken: 

* **IStreamingFilter**.  Dit type is gebaseerd op de volgende REST-API [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)
* **IStreamingAssetFilter**. Dit type is gebaseerd op de volgende REST-API [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* **PresentationTimeRange**. Dit type is gebaseerd op de volgende REST-API [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* **FilterTrackSelectStatement** en **IFilterTrackPropertyCondition**. Deze typen zijn gebaseerd op de volgende REST-API's [FilterTrackSelect en FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

## <a name="createupdatereaddelete-global-filters"></a>Globale filters maken, bijwerken, lezen/verwijderen
De volgende code laat zien hoe met .NET maken, bijwerken, lezen en verwijderen van asset-filters.

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


## <a name="createupdatereaddelete-asset-filters"></a>Filters asset maken, bijwerken, lezen/verwijderen
De volgende code laat zien hoe met .NET maken, bijwerken, lezen en verwijderen van asset-filters.

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




## <a name="build-streaming-urls-that-use-filters"></a>Streaming-URL's die gebruikmaken van filters maken
Zie voor meer informatie over het publiceren en leveren van uw assets [leveren van inhoud naar klanten overzicht](media-services-deliver-content-overview.md).

De volgende voorbeelden laten zien hoe filters toevoegen aan uw streaming-URL's.

**MPEG DASH** 

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V4**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl, filter=MyFilter)

**Apple HTTP Live Streaming (HLS) V3**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3, filter=MyFilter)

**Smooth Streaming**

    http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=MyFilter)


## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Overzicht van dynamische manifesten](media-services-dynamic-manifest-overview.md)

