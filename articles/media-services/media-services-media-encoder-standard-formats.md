---
title: Media Encoder Standard indelingen en codecs
description: In dit onderwerp geeft een overzicht van Media Encoder Standard indelingen en codecs.
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: f334b1ce-2f56-4968-a019-f0a2b0016d9f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako;anilmur
ms.openlocfilehash: 036c192c5f52a1943bc2799ad6c7e6db7bbffcc4
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder Standard Formats and Codecs (Indelingen en codecs voor Media Encoder Standard)
Dit document bevat een lijst met de meest voorkomende importeren en exporteren bestandsindelingen die u met Media Encoder Standard gebruiken kunt.

## <a name="input-containerfile-formats"></a>Invoer Container/bestandsindelingen
| Bestandsindelingen (bestandsextensies) | Ondersteund |
| --- | --- | --- | --- |
| FLV (met H.264 en AAC codecs) (.flv) |Ja |
| MXF (.mxf) |Ja |
| GXF (.gxf) |Ja |
| MPEG2-PS, MPEG2-TS 3GP (.ts, PS, .3gp, .3gpp, mpg) |Ja |
| Windows mediavideo (WMV) / de AVP (WMV, .asf) |Ja |
| AVI (niet-gecomprimeerde 8-bits/10 bits) (.avi) |Ja |
| MP4 (MP4, .m4a, .m4v) / ISMV (.isma, .ismv) |Ja |
| [Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (dvr-ms) |Ja |
| Matroska/WebM (.mkv) |Ja |
| WAVE/WAV (.wav) |Ja |
| QuickTime (MOV) |Ja |

> [!NOTE]
> Een lijst van de meer veelvoorkomende bestandsextensies is hoger. Media Encoder Standard ondersteunt vele andere (bijvoorbeeld: .m2ts, .mpeg2video, .qt). Als u probeert een bestand te coderen en u krijgt een foutbericht over de indeling die niet wordt ondersteund, uw feedback [hier](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).
> 
> 

### <a name="audio-formats-in-input-containers"></a>Audio-indelingen in invoer-containers
Media Encoder Standard biedt ondersteuning voor uitvoering van de volgende audio-indelingen in invoer-containers:

* MXF, GXF en QuickTime bestanden waarvoor audio houdt met interleaved stereo of 5.1-voorbeelden

of

* MXF, GXF en QuickTime bestanden waar de audio wordt uitgevoerd als afzonderlijke PCM-nummers, maar de kanaaltoewijzing (in stereo of 5.1) kunnen worden afgeleid van metagegevens van het bestand

Ondersteuning voor expliciete/een gebruiker opgegeven kanaaltoewijzing wordt in de nabije toekomst worden geleverd.

## <a name="input-video-codecs"></a>Video-Codecs invoer
| Video-Codecs invoer | Ondersteund |
| --- | --- | --- | --- |
| AVC 8-bits/10-bits, maximaal 4:2:2, inclusief AVCIntra |8-bits 4:2:0 en 4:2:2 |
| Avid DNxHD (in MXF) |Ja |
| DVCPro/DVCProHD (in MXF) |Ja |
| Digitale video (DV) (in AVI-bestanden) |Ja |
| JPEG 2000 |Ja |
| MPEG-2 (maximaal 422 profiel en een hoog niveau, inclusief varianten zoals XDCAM, XDCAM HD XDCAM IMX, CableLabs® en D10) |Maximaal 422 profiel |
| MPEG-1 |Ja |
| VC-1/WMV9 |Ja |
| Canopus hoofdkantoor/HQX |Nee |
| MPEG-4 Part 2 |Ja |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Ja |
| YUV420 ongecomprimeerde of tussentijds |Ja |
| Apple ProRes 422 |Ja |
| Apple ProRes 422 LT |Ja |
| Apple ProRes 422 hoofdkantoor |Ja |
| Apple ProRes Proxy |Ja |
| Apple ProRes 4444 |Ja |
| Apple ProRes 4444 XQ |Ja |

## <a name="input-audio-codecs"></a>Audio-invoer Codecs
| Audio-invoer Codecs | Ondersteund |
| --- | --- | --- | --- |
| AAC (AAC Kredietbrief AAC HE en AAC-HEv2; maximaal 5.1) |Ja |
| MPEG-laag 2 |Ja |
| MP3 (MPEG-1 Audio laag 3) |Ja |
| Windows Media Audio |Ja |
| WAV PCM / |Ja |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Ja |
| [Opus](http://go.microsoft.com/fwlink/?LinkId=822667) |Ja |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Ja |
| AMR (adaptieve meerdere tarief) |Ja |
| AES (SMPTE 331 M en 302 M, AES3 2003) |Nee |
| Dolby® E |Nee |
| Dolby® digitale (AC3) |Nee |
| Dolby® digitale Plus (E-AC3) |Nee |

## <a name="output-formats-and-codecs"></a>Uitvoerindelingen en codecs
De volgende tabel bevat de codecs en bestandsindelingen die worden ondersteund voor exporteren.

| Bestandsindeling | Video-Codec | Audio-Codec |
| --- | --- | --- |
| MP4 <br/><br/>(inclusief multi-bitrate MP4-containers) |H.264 (hoog, Main en basislijn profielen) |AAC-Kredietbrief HE-AAC v1, v2 HE-AAC |
| MPEG2 TS |H.264 (hoog, Main en basislijn profielen) |AAC-Kredietbrief HE-AAC v1, v2 HE-AAC |

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[Codering van inhoud op aanvraag met Azure mediaservices](media-services-encode-asset.md)

[Het coderen met Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

