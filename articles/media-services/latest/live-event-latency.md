---
title: LiveEvent latentie in Azure Media Services | Microsoft Docs
description: In dit onderwerp biedt een overzicht van LiveEvent latentie en laat zien hoe u om in te stellen met lage latentie.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 11/16/2018
ms.author: juliako
ms.openlocfilehash: b167d3424d520cf8be515eec9d495164dd9785ab
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52682092"
---
# <a name="liveevent-latency-in-media-services"></a>LiveEvent latentie in Media Services

Dit artikel wordt beschreven hoe u om in te stellen met lage latentie op een **LiveEvent**. Hierin worden ook typische resultaten die u ziet wanneer u de instellingen voor lage latentie in verschillende spelers. De resultaten variëren op basis van CDN en netwerklatentie. 

Gebruik van de nieuwe **LowLatency** functie, stelt u de **StreamOptionsFlag** naar **LowLatency** op de **LiveEvent**. Zodra de stroom actief en werkend is, kunt u de [Azure Media Player](http://ampdemo.azureedge.net/) (en) demo pagina toe en stel de opties voor afspelen 'Met lage latentie methodiek profiel' te gebruiken.

De volgende .NET-voorbeeld laat zien hoe om in te stellen **LowLatency** op de **LiveEvent**:

```csharp
LiveEvent liveEvent = new LiveEvent(
            location: mediaService.Location, 
            description: "Sample LiveEvent for testing",
            vanityUrl: false,
            encoding: new LiveEventEncoding(
                        // Set this to Basic to enable a transcoding LiveEvent, and None to enable a pass-through LiveEvent
                        encodingType:LiveEventEncodingType.None, 
                        presetName:null
                    ),
            input: new LiveEventInput(LiveEventInputProtocol.RTMP,liveEventInputAccess), 
            preview: liveEventPreview,
            streamOptions: new List<StreamOptionsFlag?>()
            {
                // Set this to Default or Low Latency
                // To use low latency optimally, you should tune your encoder settings down to 1 second "Group Of Pictures" (GOP) length instead of 2 seconds.
                StreamOptionsFlag.LowLatency
            }
        );
```                

Zie het volledige voorbeeld: [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

## <a name="liveevents-latency"></a>LiveEvents latentie

De volgende tabellen weergeven typische resultaten voor latentie (wanneer de LowLatency-vlag is ingeschakeld) in Media Services, gemeten vanaf het moment dat de service wanneer een viewer voor het afspelen op de speler ziet de bijdrage-feed is bereikt. Als u wilt gebruiken met lage latentie optimaal, moet u de instellingen van uw encoder omlaag naar 'Groep van foto's ' (GOP) de lengte van 1 seconde afstemmen. Wanneer u een hogere GOP lengte, kunt u gebruik van netwerkbandbreedte te minimaliseren en bitrate onder dezelfde framesnelheid verminderen. Het is vooral nuttig in video's met minder beweging.

### <a name="pass-through"></a>Pass-through 

||2s GOP lage latentie ingeschakeld|1s GOP lage latentie ingeschakeld|
|---|---|---|
|STREEPJES in AMP|per 10|8s versie|
|HLS op systeemeigen iOS-speler|14s|per 10|

### <a name="live-encoding"></a>Live Encoding

||2s GOP lage latentie ingeschakeld|1s GOP lage latentie ingeschakeld|
|---|---|---|
|STREEPJES in AMP|14s|per 10|
|HLS op systeemeigen iOS-speler|18s|13s|

> [!NOTE]
> De end-to-end-latentie kan variëren, afhankelijk van het lokale netwerk of door de introductie van een CDN caching-laag. De exacte configuraties, moet u testen.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van live streaming](live-streaming-overview.md)
- [Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)

