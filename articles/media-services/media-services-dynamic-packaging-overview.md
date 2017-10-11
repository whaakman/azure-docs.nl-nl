---
title: Overzicht van Azure Media Services dynamische pakketten | Microsoft Docs
description: Het onderwerp biedt en overzicht van dynamische pakketten.
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0d9e4f54-5daa-45c1-bfaa-cf09ca89b812
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 2d212599302fced3f60085ab30cdeaefc1ee2e6a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="dynamic-packaging"></a>Dynamische verpakking
## <a name="overview"></a>Overzicht
Microsoft Azure Media Services kunnen worden gebruikt voor het leveren van veel bron bestandsindelingen mediastreaming indelingen, en beveiliging van inhoud bestandsindelingen naar verschillende technologieën van de client (bijvoorbeeld iOS-, XBOX, Silverlight, Windows 8). Deze clients begrijpen verschillende protocollen, bijvoorbeeld iOS een HTTP Live Streaming (HLS)-V4-indeling heeft en Silverlight en Xbox vereisen Smooth Streaming. Als u een set adaptive bitrate (multi-bitrate) hebt MP4-bestanden (ISO Base Media 14496-12) of een set adaptive bitrate Smooth Streaming-bestanden die u wilt leveren aan clients die MPEG DASH, HLS of Smooth Streaming begrijpen, moet u ook te profiteren van Media Services dynamische pakketten.

Dynamische pakketten hoeft u een asset die bestaat uit een set adaptive bitrate MP4-bestanden of adaptive bitrate Smooth Streaming-bestanden te maken. Klik, op basis van de opgegeven indeling de manifest- of fragmentdeel aanvraag, de On-Demand Streaming server zorgt ervoor dat u de stream ontvangt in het protocol dat u hebt gekozen. Hierdoor hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. De Media Services-service bouwt en levert de juiste reactie op basis van aanvragen van een client.

Het volgende diagram ziet u de traditionele codering en de werkstroom voor statische pakketten.

![Statische codering](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Het volgende diagram toont de werkstroom dynamische pakketten.

![Dynamische codering](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)


## <a name="common-scenario"></a>Gangbare scenario
1. Upload een invoerbestand (een tussentijds bestand genoemd). Bijvoorbeeld, H.264 MP4 of WMV (voor een lijst met ondersteunde indelingen raadpleegt [indelingen ondersteund door de Media Encoder Standard](media-services-media-encoder-standard-formats.md).
2. Codeer uw tussentijds bestand op H.264 MP4 adaptive bitrate sets.
3. Publiceer de asset met de adaptive bitrate MP4-set door de On-Demand-Locator te maken.
4. Bouw de streaming-URL's voor toegang tot en de inhoud streamen.

## <a name="preparing-assets-for-dynamic-streaming"></a>Activa voorbereiden voor dynamische streaming
Als u wilt uw asset voorbereiden voor het streamen van dynamische hebt u twee opties:

1. [Een master-bestand uploaden](media-services-dotnet-upload-files.md).
2. [De Media Encoder Standard encoder gebruiken voor het produceren MP4 H.264 adaptive bitrate sets](media-services-dotnet-encode-with-media-encoder-standard.md).
3. [De inhoud streamen](media-services-deliver-content-overview.md).

## <a id="unsupported_formats"></a>De opmaak die niet worden ondersteund door dynamische pakketten
De volgende indelingen voor bron worden niet ondersteund voor dynamische pakketten.

* Dolby digitale mp4-bestanden.
* Dolby digitale smooth bestanden.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

