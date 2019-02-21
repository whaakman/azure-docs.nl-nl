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
ms.date: 02/19/2019
ms.author: juliako
ms.openlocfilehash: d1d07402bca5f01cf63d0b039c085e46bb0f0d62
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447919"
---
# <a name="dynamic-packaging"></a>Dynamische verpakking

Microsoft Azure Media Services kunnen worden gebruikt om veel media bron-bestandsindelingen, streaming-indelingen, media en beveiliging van inhoud die is geformatteerd met tal van client-technologieën (bijvoorbeeld iOS- en XBOX genoemd). Deze clients verschillende protocollen te begrijpen, bijvoorbeeld iOS heeft dat een HTTP Live Streaming (HLS)-indeling en Xbox vereisen Smooth Streaming. Als u een set adaptive bitrate (multi-bitrate) hebt MP4-bestanden (ISO Base Media 14496-12) of een set adaptive bitrate Smooth Streaming-bestanden die u wilt gebruiken op clients die HLS, MPEG-DASH of Smooth Streaming begrijpen, dient u te profiteren van Media Services dynamische pakketten.

Met dynamische verpakking is alles wat u hoeft te maken van een asset die een set adaptive bitrate MP4-bestanden bevat. Vervolgens, op basis van de opgegeven indeling in het manifest- of fragmentdeel, de On-Demand Streaming zorgt de server dat u de stream ontvangt in het protocol dat u hebt gekozen. Hierdoor hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. De Media Services-service bouwt en levert de juiste reactie op basis van aanvragen van een client.

Het volgende diagram toont de traditionele codering en statische pakketten werkstroom.

![Statische codering](./media/dynamic-packaging-overview/media-services-static-packaging.png)

Het volgende diagram toont de werkstroom dynamische pakketten.

![Dynamische codering](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="dynamic-packaging-workflow"></a>Werkstroom voor dynamische verpakking

1. Upload een bestand voor invoer (een tussentijds bestand genoemd). Bijvoorbeeld, H.264 MP4 of WMV (Zie voor de lijst met ondersteunde indelingen [indelingen ondersteund door de Media Encoder Standard](media-encoder-standard-formats.md).
2. Codeer uw tussentijds bestand op afspelen van H.264 MP4 adaptieve bitrate sets.
3. Publiceer de asset met de adaptive bitrate die MP4-set.
4. Bouw de streaming-URL's om te openen en de inhoud streamen.

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Codecs audio wordt ondersteund door dynamische verpakking

MP4-bestanden met audio die is gecodeerd met biedt ondersteuning voor dynamische pakketten [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, hij AAC v1, v2 HE-AAC), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Enhanced AC-3 of E-AC3), of [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) DTS ( Snelle, DTS LBR, HD DTS, DTS HD zonder verlies).

> [!NOTE]
> Bestanden met biedt geen ondersteuning voor dynamische pakketten [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) audio (dit is een verouderde codec).

## <a name="next-steps"></a>Volgende stappen

[Uploaden, coderen, stream-video 's](stream-files-tutorial-with-api.md)
