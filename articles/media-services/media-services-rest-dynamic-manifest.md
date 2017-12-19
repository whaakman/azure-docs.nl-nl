---
title: Filters maken met Azure Media Services-REST-API | Microsoft Docs
description: Dit onderwerp beschrijft het maken van filters zodat de client naar specifieke secties van de stroom van een stroom gebruiken kan. Media Services maakt dynamische manifesten zodat deze selectief streaming.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: f7d23daf-7cd2-49c7-a195-ab902912ab3c
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 12/07/2017
ms.author: juliako;cenkdin
ms.openlocfilehash: 98df3b6592ed865fc0eb4b942d298b26e930365f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="creating-filters-with-azure-media-services-rest-api"></a>Filters maken met Azure Media Services REST-API
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-dynamic-manifest.md)
> * [REST](media-services-rest-dynamic-manifest.md)
> 
> 

Media Services kunt vanaf 2,17 versie, u filters definiëren voor de activa. Deze filters zijn serverzijde regels waarmee uw klanten te kiezen voor handelingen zoals: afspelen alleen een gedeelte van een video (in plaats van de hele video afspelen), of geef alleen een subset van audio en video vertoningen dat uw klant apparaat (in plaats van kan verwerken alle de vertoningen die gekoppeld aan de asset zijn). Via dit filteren van uw assets wordt gearchiveerd **dynamische Manifest**s die zijn gemaakt op verzoek om te streamen van een video van uw klant op basis van opgegeven filter.

Zie voor meer informatie over filters en dynamische Manifest, [dynamische manifesten overzicht](media-services-dynamic-manifest-overview.md).

Dit artikel laat zien hoe u REST-API's maken, bijwerken en verwijderen van de filters. 

## <a name="types-used-to-create-filters"></a>Typen die worden gebruikt om filters te maken
De volgende typen worden gebruikt wanneer u filters maken:  

* [Filter](https://docs.microsoft.com/rest/api/media/operations/filter)
* [AssetFilter](https://docs.microsoft.com/rest/api/media/operations/assetfilter)
* [PresentationTimeRange](https://docs.microsoft.com/rest/api/media/operations/presentationtimerange)
* [FilterTrackSelect en FilterTrackPropertyCondition](https://docs.microsoft.com/rest/api/media/operations/filtertrackselect)

>[!NOTE]

>Bij het openen van entiteiten in Media Services, moet u specifieke header-velden en waarden instellen in uw HTTP-aanvragen. Zie voor meer informatie [Setup voor het ontwikkelen van Media Services REST API](media-services-rest-how-to-use.md).

## <a name="connect-to-media-services"></a>Verbinding met Media Services maken

Zie voor meer informatie over de verbinding maken met de AMS API [toegang tot de API van Azure Media Services met Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md). 

## <a name="create-filters"></a>Filters maken
### <a name="create-global-filters"></a>Globale Filters maken
Gebruik de volgende HTTP-aanvragen voor het maken van een globaal Filter:  

#### <a name="http-request"></a>HTTP-aanvraag
Aanvraagheaders

    POST https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host:media.windows.net 

Aanvraagtekst 

    {  
       "Name":"GlobalFilter",
       "PresentationTimeRange":{  
          "StartTimestamp":"0",
          "EndTimestamp":"9223372036854775807",
          "PresentationWindowDuration":"12000000000",
          "LiveBackoffDuration":"0",
          "Timescale":"10000000"
       },
       "Tracks":[  
          {  
             "PropertyConditions":
                  [  
                {  
                   "Property":"Type",
                   "Value":"audio",
                   "Operator":"Equal"
                },
                {  
                   "Property":"Bitrate",
                   "Value":"0-2147483647",
                   "Operator":"Equal"
                }
             ]
          }
       ]
    }




#### <a name="http-response"></a>HTTP-antwoord
    HTTP/1.1 201 Created 

### <a name="create-local-assetfilters"></a>Lokale AssetFilters maken
Gebruik de volgende HTTP-aanvragen voor het maken van een lokale AssetFilter:  

#### <a name="http-request"></a>HTTP-aanvraag
Aanvraagheaders

    POST https://media.windows.net/API/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net  

Aanvraagtekst 

    {   
       "Name":"AssetFilter", 
       "ParentAssetId":"nb:cid:UUID:536e555d-1500-80c3-92dc-f1e4fdc6c592", 
       "PresentationTimeRange":{   
          "StartTimestamp":"0", 
          "EndTimestamp":"9223372036854775807", 
          "PresentationWindowDuration":"12000000000", 
          "LiveBackoffDuration":"0", 
          "Timescale":"10000000" 
       }, 
       "Tracks":[   
          {   
             "PropertyConditions": 
                  [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

#### <a name="http-response"></a>HTTP-antwoord
    HTTP/1.1 201 Created 
    . . . 

## <a name="list-filters"></a>Lijst met filters
### <a name="get-all-global-filters-in-the-ams-account"></a>Ophalen van alle globale **Filter**s in de AMS-account
Filters weergeven door gebruik van de volgende HTTP-aanvragen: 

#### <a name="http-request"></a>HTTP-aanvraag
    GET https://media.windows.net/API/Filters HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.17 
    Host: media.windows.net 

### <a name="get-assetfilters-associated-with-an-asset"></a>Ophalen van **AssetFilter**s die is gekoppeld aan een asset
#### <a name="http-request"></a>HTTP-aanvraag
    GET https://media.windows.net/API/Assets('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592')/AssetFilters HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

### <a name="get-an-assetfilter-based-on-its-id"></a>Ophalen van een **AssetFilter** op basis van de bijbehorende Id
#### <a name="http-request"></a>HTTP-aanvraag
    GET https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000


## <a name="update-filters"></a>Filters bijwerken
Gebruik PATCH-, opslag- of MERGE bijwerken van een filter met de nieuwe eigenschapswaarden.  Zie voor meer informatie over deze bewerkingen [PATCH, PUT, MERGE](http://msdn.microsoft.com/library/dd541276.aspx).

Als u een filter bijwerkt, kan het maximaal twee minuten duren voordat een streaming-eindpunt om te vernieuwen van de regels. Als de inhoud is uitgevoerd met dit filter (en in het cachegeheugen van proxy's en CDN caches), kan het bijwerken van dit filter leiden tot player-fouten. Wis de cache na het bijwerken van het filter. Als deze optie niet mogelijk is, kunt u overwegen een ander filter.  

### <a name="update-global-filters"></a>Globale Filters bijwerken
Gebruik de volgende HTTP-aanvragen voor het bijwerken van een globaal filter: 

#### <a name="http-request"></a>HTTP-aanvraag
Headers voor aanvraag: 

    MERGE https://media.windows.net/API/Filters('filterName') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 
    Content-Length: 384

Hoofdtekst van de aanvraag: 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 

### <a name="update-local-assetfilters"></a>Lokale AssetFilters bijwerken
Gebruik de volgende HTTP-aanvragen voor het bijwerken van een lokale filter: 

#### <a name="http-request"></a>HTTP-aanvraag
Headers voor aanvraag: 

    MERGE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__TestFilter')  HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Content-Type: application/json 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.17 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000 
    Host: media.windows.net 

Hoofdtekst van de aanvraag: 

    { 
       "Tracks":[   
          {   
             "PropertyConditions": 
             [   
                {   
                   "Property":"Type", 
                   "Value":"audio", 
                   "Operator":"Equal" 
                }, 
                {   
                   "Property":"Bitrate", 
                   "Value":"0-2147483647", 
                   "Operator":"Equal" 
                } 
             ] 
          } 
       ] 
    } 


## <a name="delete-filters"></a>Filters verwijderen
### <a name="delete-global-filters"></a>Globale Filters verwijderen
Gebruik de volgende HTTP-aanvragen voor het verwijderen van een globaal Filter:

#### <a name="http-request"></a>HTTP-aanvraag
    DELETE https://media.windows.net/api/Filters('GlobalFilter') HTTP/1.1 
    DataServiceVersion:3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.17 
    Host: media.windows.net 


### <a name="delete-local-assetfilters"></a>Lokale AssetFilters verwijderen
Gebruik de volgende HTTP-aanvragen voor het verwijderen van een lokale AssetFilter:

#### <a name="http-request"></a>HTTP-aanvraag
    DELETE https://media.windows.net/API/AssetFilters('nb%3Acid%3AUUID%3A536e555d-1500-80c3-92dc-f1e4fdc6c592__%23%23%23__LocalFilter') HTTP/1.1 
    DataServiceVersion: 3.0 
    MaxDataServiceVersion: 3.0 
    Accept: application/json 
    Accept-Charset: UTF-8 
    Authorization: Bearer <token value> 
    x-ms-version: 2.17 
    Host: media.windows.net 

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

