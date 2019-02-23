---
title: Overzicht van Azure Media Services dynamische pakketten | Microsoft Docs
description: Het onderwerp biedt een overzicht van dynamische pakketten in Media Services.
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
ms.date: 02/22/2019
ms.author: juliako
ms.openlocfilehash: 02af95de3793f1d56204b17b0a3d91efbb285e55
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726411"
---
# <a name="dynamic-packaging"></a>Dynamische verpakking

Microsoft Azure Media Services kunnen worden gebruikt om veel media bron-bestandsindelingen, streaming-indelingen, media en beveiliging van inhoud die is geformatteerd met tal van client-technologieën (bijvoorbeeld iOS- en XBOX genoemd). Deze clients verschillende protocollen te begrijpen, bijvoorbeeld iOS heeft dat een HTTP Live Streaming (HLS)-indeling en Xbox vereisen Smooth Streaming. Als u een set adaptive bitrate (multi-bitrate) hebt MP4-bestanden (ISO Base Media 14496-12) of een set adaptive bitrate Smooth Streaming-bestanden die u wilt gebruiken op clients die HLS, MPEG-DASH of Smooth Streaming begrijpen, kunt u profiteren van dynamische Verpakking. De pakketten is agnostisch ten opzichte van de video oplossing, SD/HD/UHD - 4K worden ondersteund.

[Streaming-eindpunten](streaming-endpoint-concept.md) is de service voor dynamische pakketten in Media Services gebruikt voor het leveren van media-inhoud naar client spelers. Dynamische pakketten is een functie die wordt standaard geleverd op alle Streaming-eindpunten (Standard of Premium). Er zijn geen extra kosten die gepaard gaan met deze functie in Media Services v3. Met dynamische verpakking wordt alles wat u hoeft een asset die een set adaptive bitrate MP4-bestanden met manifestbestanden bevat. Klik, op basis van de opgegeven indeling in het manifest- of fragmentdeel, u de stream ontvangt in het protocol dat u hebt gekozen. Hierdoor hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. De Media Services-service bouwt en levert de juiste reactie op basis van aanvragen van een client.

In Media Services dynamische pakketten gebruikt of u On-Demand of Live streaming.

Het volgende diagram toont de werkstroom dynamische pakketten.

![Dynamische codering](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

## <a name="common-video-on-demand-workflow"></a>Algemene werkstroom voor video op aanvraag

Hier volgt een algemene Media Services streaming-werkstroom waarbij dynamische pakketten wordt gebruikt.

1. Upload een bestand voor invoer (een tussentijds bestand genoemd). Bijvoorbeeld, H.264 MP4 of WMV (Zie voor de lijst met ondersteunde indelingen [indelingen ondersteund door de Media Encoder Standard](media-encoder-standard-formats.md).
2. Codeer uw tussentijds bestand op afspelen van H.264 MP4 adaptieve bitrate sets.
3. Publiceer de asset met de adaptive bitrate die MP4-set.
4. URL's die zijn gericht op verschillende indelingen (HLS, Dash en Smooth Streaming) maken. Het Streaming-eindpunt zou zorgen voor de juiste manifest en aanvragen voor deze verschillende indelingen.
 
## <a name="video-codecs-supported-by-dynamic-packaging"></a>Codecs invoervideo ondersteund door dynamische verpakking

MP4-bestanden, waarin video gecodeerd met biedt ondersteuning voor dynamische pakketten [H.264](https://en.m.wikipedia.org/wiki/H.264/MPEG-4_AVC) (MPEG-4 AVC of AVC1), [H.265](https://en.m.wikipedia.org/wiki/High_Efficiency_Video_Coding) (HEVC, hev1 of hvc1).

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Codecs audio wordt ondersteund door dynamische verpakking

MP4-bestanden, waarin de audio die is gecodeerd met biedt ondersteuning voor dynamische pakketten [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, hij AAC v1, v2 HE-AAC), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Enhanced AC-3 of E-AC3), of [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) DTS ( Snelle, DTS LBR, HD DTS, DTS HD zonder verlies).

> [!NOTE]
> Bestanden met biedt geen ondersteuning voor dynamische pakketten [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) audio (dit is een verouderde codec).

## <a name="next-steps"></a>Volgende stappen

[Uploaden, coderen, stream-video 's](stream-files-tutorial-with-api.md)

