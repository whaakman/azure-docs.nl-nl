---
title: Azure Media Services-inhoud met behulp van .NET publiceren | Microsoft Docs
description: Informatie over het maken van een locator die wordt gebruikt voor het bouwen van een streaming-URL. Codevoorbeelden zijn geschreven in C# en de Media Services SDK voor .NET gebruiken.
author: juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.assetid: c53b1f83-4cb1-4b09-840f-9c145b7d6f8d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2019
ms.author: juliako
ms.openlocfilehash: 8e34d8cfbcd655dbb49279a0cefd63818963652a
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55999178"
---
# <a name="publish-media-services-content-using-net"></a>Media Services-inhoud met behulp van .NET publiceren  
> [!div class="op_single_selector"]
> * [REST](media-services-rest-deliver-streaming-content.md)
> * [.NET](media-services-deliver-streaming-content.md)
> * [Portal](media-services-portal-publish.md)
> 
> 

## <a name="overview"></a>Overzicht
U kunt een adaptive bitrate MP4-set door het maken van een OnDemand-locator voor streaming en het bouwen van een streaming-URL streamen. De [een asset coderen](media-services-encode-asset.md) onderwerp wordt beschreven hoe moeten worden gecodeerd naar een adaptive bitrate MP4-set. 

> [!NOTE]
> Als uw inhoud is versleuteld, leveringsbeleid voor Assets configureren (zoals beschreven in [dit](media-services-dotnet-configure-asset-delivery-policy.md) onderwerp) voordat u een locator maakt. 
> 
> 

U kunt ook een OnDemand-streaminglocator maken van URL's die verwijzen naar een MP4-bestanden progressief kunnen worden gedownload.  

Dit onderwerp leest hoe u een OnDemand-streaminglocator voor uw asset publiceren en maken van een soepele, MPEG DASH en HLS streaming-URL's maken. U ziet ook ' hot ' naar de progressieve download-URL's maken. 

## <a name="create-an-ondemand-streaming-locator"></a>Een OnDemand-streaminglocator maken
Voor het maken van de streaming OnDemand-locator en URL's ophalen, moet u het volgende doen:

1. Als de inhoud is versleuteld, moet u een toegangsbeleid definiëren.
2. Een OnDemand-streaminglocator maken.
3. Als u van plan bent om te streamen, krijgt u de streaming manifestbestand (.ISM bevat) in de asset. 
   
   Als u van plan bent progressief te downloaden, krijgt u de namen van de MP4-bestanden in de asset.  
4. URL's naar de manifest-bestand of de MP4-bestanden maken. 


>[!NOTE]
>Er geldt een limiet van 1.000.000 beleidsregels voor verschillende AMS-beleidsitems (bijvoorbeeld voor Locator-beleid of ContentKeyAuthorizationPolicy). Gebruik dezelfde beleids-ID als u altijd dezelfde dagen / toegangsmachtigingen. Bijvoorbeeld, beleidsregels voor locators die zijn bedoeld om te blijven aanwezig gedurende een lange periode (niet-uploadbeleidsregels). Raadpleeg [dit](media-services-dotnet-manage-entities.md#limit-access-policies) onderwerp voor meer informatie.

### <a name="use-media-services-net-sdk"></a>Use Media Services .NET SDK
Streaming-URL's maken 

```csharp
    private static void BuildStreamingURLs(IAsset asset)
    {

        // Create a 30-day readonly access policy. 
          // You cannot create a streaming locator using an AccessPolicy that includes write or delete permissions.
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create a locator to the streaming content on an origin. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get a reference to the streaming manifest file from the  
        // collection of files in the asset. 
        var manifestFile = asset.AssetFiles.Where(f => f.Name.ToLower().
                                    EndsWith(".ism")).
                                    FirstOrDefault();

        // Create a full URL to the manifest file. Use this for playback
        // in streaming media clients. 
        string urlForClientStreaming = originLocator.Path + manifestFile.Name + "/manifest";
        Console.WriteLine("URL to manifest for client streaming using Smooth Streaming protocol: ");
        Console.WriteLine(urlForClientStreaming);
        Console.WriteLine("URL to manifest for client streaming using HLS protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=m3u8-aapl)");
        Console.WriteLine("URL to manifest for client streaming using MPEG DASH protocol: ");
        Console.WriteLine(urlForClientStreaming + "(format=mpd-time-csf)"); 
        Console.WriteLine();
    }
```

De uitvoer:

    URL to manifest for client streaming using Smooth Streaming protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest
    URL to manifest for client streaming using HLS protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=m3u8-aapl)
    URL to manifest for client streaming using MPEG DASH protocol:
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny.ism/manifest(format=mpd-time-csf)


> [!NOTE]
> U kunt ook uw inhoud streamen via een SSL-verbinding. Controleer hiervoor deze benadering, of uw streaming URL begint met HTTPS. Op dit moment biedt geen AMS ondersteuning voor SSL met aangepaste domeinen.
> 
> 

Progressieve download-URL's maken 

```csharp
    private static void BuildProgressiveDownloadURLs(IAsset asset)
    {
        // Create a 30-day readonly access policy. 
        IAccessPolicy policy = _context.AccessPolicies.Create("Streaming policy",
            TimeSpan.FromDays(30),
            AccessPermissions.Read);

        // Create an OnDemandOrigin locator to the asset. 
        ILocator originLocator = _context.Locators.CreateLocator(LocatorType.OnDemandOrigin, asset,
            policy,
            DateTime.UtcNow.AddMinutes(-5));

        // Display some useful values based on the locator.
        Console.WriteLine("Streaming asset base path on origin: ");
        Console.WriteLine(originLocator.Path);
        Console.WriteLine();

        // Get MP4 files.
        IEnumerable<IAssetFile> mp4AssetFiles = asset
            .AssetFiles
            .ToList()
            .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Create a full URL to the MP4 files. Use this to progressively download files.
        foreach (var pd in mp4AssetFiles)
            Console.WriteLine(originLocator.Path + pd.Name);
    }
```
De uitvoer:

    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4
    http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4

    . . . 

### <a name="use-media-services-net-sdk-extensions"></a>Gebruik Media Services .NET SDK Extensions
De volgende code wordt de .NET SDK extensions methoden die een locator te maken en genereren van de Smooth Streaming, HLS en MPEG-DASH-URL's voor adaptief streamen.
```csharp
    // Create a loctor.
    _context.Locators.Create(
        LocatorType.OnDemandOrigin,
        inputAsset,
        AccessPermissions.Read,
        TimeSpan.FromDays(30));

    // Get the streaming URLs.
    Uri smoothStreamingUri = inputAsset.GetSmoothStreamingUri();
    Uri hlsUri = inputAsset.GetHlsUri();
    Uri mpegDashUri = inputAsset.GetMpegDashUri();

    Console.WriteLine(smoothStreamingUri);
    Console.WriteLine(hlsUri);
    Console.WriteLine(mpegDashUri);
```

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Volgende stappen
* [Activa downloaden](media-services-deliver-asset-download.md)
* [Leveringsbeleid voor Assets configureren](media-services-dotnet-configure-asset-delivery-policy.md)

