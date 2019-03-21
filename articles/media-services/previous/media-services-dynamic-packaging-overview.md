---
title: Overzicht van Azure Media Services dynamische pakketten | Microsoft Docs
description: Het onderwerp biedt en overzicht van dynamische pakketten.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 15599beb47b7f6e72b89e7776196de8e6b94844f
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58189169"
---
# <a name="dynamic-packaging"></a>Dynamische verpakking

Microsoft Azure Media Services kunnen worden gebruikt om veel media bron-bestandsindelingen, streaming-indelingen, media en beveiliging van inhoud die is geformatteerd met tal van client-technologieën (bijvoorbeeld iOS-, XBOX, Silverlight, Windows 8). Deze clients begrijpen verschillende protocollen, bijvoorbeeld iOS heeft een HTTP Live Streaming (HLS)-V4-indeling en Silverlight en Xbox vereisen Smooth Streaming. Als u een set adaptive bitrate (multi-bitrate) hebt MP4-bestanden (ISO Base Media 14496-12) of een set adaptive bitrate Smooth Streaming-bestanden die u wilt gebruiken op clients die inzicht in MPEG-DASH, HLS of Smooth Streaming, moet u ook te profiteren van Media Services dynamische pakketten.

Met dynamische verpakking is alles wat u hoeft te maken van een asset die een set adaptive bitrate MP4-bestanden of adaptive bitrate Smooth Streaming-bestanden bevat. Vervolgens, op basis van de opgegeven indeling in het manifest- of fragmentdeel, de On-Demand Streaming zorgt de server dat u de stream ontvangt in het protocol dat u hebt gekozen. Hierdoor hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. De Media Services-service bouwt en levert de juiste reactie op basis van aanvragen van een client.

Het volgende diagram toont de traditionele codering en statische pakketten werkstroom.

![Statische codering](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

Het volgende diagram toont de werkstroom dynamische pakketten.

![Dynamische codering](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="common-scenario"></a>Algemeen scenario
1. Upload een bestand voor invoer (een tussentijds bestand genoemd). Bijvoorbeeld, H.264 MP4 of WMV (Zie voor de lijst met ondersteunde indelingen [indelingen ondersteund door de Media Encoder Standard](media-services-media-encoder-standard-formats.md).
2. Codeer uw tussentijds bestand op afspelen van H.264 MP4 adaptieve bitrate sets.
3. Publiceer de asset met de adaptive bitrate MP4-set door de On-Demand-Locator te maken.
4. Bouw de streaming-URL's om te openen en de inhoud streamen.

## <a name="preparing-assets-for-dynamic-streaming"></a>Activa voorbereiden voor het streamen van dynamische
Als u wilt uw asset voorbereiden voor het streamen van dynamische hebt u de volgende opties:

- [Uploaden van een hoofdbestand](media-services-dotnet-upload-files.md).
- [De Media Encoder Standard encoder gebruiken voor het produceren van afspelen van H.264 MP4 adaptieve bitrate sets](media-services-dotnet-encode-with-media-encoder-standard.md).
- [De inhoud van uw Stream](media-services-deliver-content-overview.md).

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Codecs audio wordt ondersteund door dynamische verpakking

Dynamische pakketten ondersteunt MP4-bestanden (of Smooth Streaming-bestanden) met audio die is gecodeerd met [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, hij AAC v1, v2 HE-AAC), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Enhanced AC-3 of E-AC3), of [ DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, DTS LBR, HD DTS, DTS HD zonder verlies).

> [!Note]
> Bestanden met biedt geen ondersteuning voor dynamische pakketten [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) audio (dit is een verouderde codec).

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

