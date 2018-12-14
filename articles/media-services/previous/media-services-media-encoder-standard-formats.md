---
title: Media Encoder Standard indelingen en codecs voor - Azure
description: In dit onderwerp biedt een overzicht van Media Encoder Standard indelingen en codecs voor.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2018
ms.author: juliako;anilmur
ms.openlocfilehash: fe1d681fa5f9ab49fec9112398ed03c87c975176
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384731"
---
# <a name="media-encoder-standard-formats-and-codecs"></a>Media Encoder Standard Formats and Codecs (Indelingen en codecs voor Media Encoder Standard)

> [!div class="op_single_selector" title1="Select the version of Media Services that you are using:"]
> * [Versie 2](media-services-media-encoder-standard-formats.md)
> * [Versie 3](../latest/media-encoder-standard-formats.md)

Dit document bevat een lijst van de meest voorkomende importeren en exporteren bestandsindelingen die u met Media Encoder Standard gebruiken kunt.

## <a name="input-containerfile-formats"></a>Invoer/bestandsindelingen
| Bestandsindelingen (bestandsextensies) | Ondersteund |
| --- | --- | --- | --- |
| FLV (met H.264- en AAC-codecs) (.flv) |Ja |
| MXF (.mxf) |Ja |
| GXF (.gxf) |Ja |
| MPEG2-PS, MPEG2-TS 3GP (TS, PS, .3gp, .3GP, mpg) |Ja |
| Windows mediavideo (WMV) / de AVP (WMV-indeling, .asf) |Ja |
| AVI (niet-gecomprimeerde 8-bits/10 bits) (.avi) |Ja |
| MP4 (.mp4, .m4a, .m4v) / ISMV (.isma, .ismv) |Ja |
| [Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (dvr-ms) |Ja |
| Matroska/WebM (.mkv) |Ja |
| WAVE/WAV (.wav) |Ja |
| QuickTime (MOV) |Ja |

> [!NOTE]
> Hiervoor is een lijst van de meer frequent bestandsextensies. Media Encoder Standard biedt ondersteuning voor vele andere (bijvoorbeeld: .m2ts, .mpeg2video, .qt). Als u probeert een bestand coderen en krijgt u een foutbericht over de indeling die niet wordt ondersteund, Geef uw feedback [hier](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).
> 
> 

### <a name="audio-formats-in-input-containers"></a>Audio-indelingen in invoer-containers
Media Encoder Standard ondersteunt de volgende audio-indelingen in invoer containers uitvoeren:

* MXF-, GXF- en QuickTime-bestanden, die met interleaved stereo of 5.1-voorbeelden audiosporen

of

* MXF-, GXF- en QuickTime-bestanden waar de audio wordt uitgevoerd als afzonderlijke PCM-sporen, maar de kanaaltoewijzing (in stereo of 5.1) kunnen worden afgeleid van de metagegevens van het bestand

## <a name="input-video-codecs"></a>Codecs invoervideo
| Codecs invoervideo | Ondersteund |
| --- | --- | --- | --- |
| AVC 8-bits/10 bits, maximaal 4:2:2, inclusief AVCIntra |8-bits 4:2:0 en 4:2:2 |
| Avid resolutie (in MXF) |Ja |
| DVCPro/DVCProHD (in MXF) |Ja |
| Digitale video (DV) (in AVI-bestanden) |Ja |
| JPEG 2000 |Ja |
| MPEG-2 (maximaal 422-profiel en hoog niveau, inclusief varianten zoals XDCAM, XDCAM HD XDCAM IMX, CableLabs® en D10) |Maximaal 422-profiel |
| MPEG-1 |Ja |
| VC-1/WMV9 |Ja |
| Canopus hoofdkantoor/HQX |Nee |
| MPEG-4-deel 2 |Ja |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Ja |
| YUV420 ongecomprimeerd of mezzanine |Ja |
| Apple ProRes 422 |Ja |
| Apple ProRes 422 LT |Ja |
| Apple ProRes 422 hoofdkantoor |Ja |
| Apple ProRes Proxy |Ja |
| Apple ProRes 4444 |Ja |
| Apple ProRes 4444 XQ |Ja |
| HEVC/H.265| Belangrijkste profiel|

## <a name="input-audio-codecs"></a>Codecs audio-invoer
| Codecs Audio-invoer | Ondersteund |
| --- | --- | --- | --- |
| AAC (AAC-LC, AAC-HE en AAC-HEv2; tot. 5.1) |Ja |
| MPEG-laag 2 |Ja |
| MP3 (laag 3 Audio MPEG-1) |Ja |
| Windows Media Audio |Ja |
| WAV/PCM |Ja |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Ja |
| [Opus](https://go.microsoft.com/fwlink/?LinkId=822667) |Ja |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Ja |
| AMR (adaptive multi-rate) |Ja |
| AES (SMPTE 331 M en 302 M, AES3-2003) |Nee |
| Dolby® E |Nee |
| Dolby® digitale (AC3) |Nee |
| Dolby® digitale Plus (E-AC3) |Nee |

## <a name="output-formats-and-codecs"></a>Output-indelingen en codecs voor
De volgende tabel bevat de codecs en bestandsindelingen die worden ondersteund voor exporteren.

| Bestandsindeling | Video-Codec | Audiocodec |
| --- | --- | --- |
| MP4 <br/><br/>(met inbegrip van multi-bitrate MP4-containers) |H.264 (hoog, Main en basislijn profielen) |AAC-LC, hij AAC v1, v2 HE-AAC |
| MPEG2-TS |H.264 (hoog, Main en basislijn profielen) |AAC-LC, hij AAC v1, v2 HE-AAC |

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Zie ook
[De inhoud op aanvraag met Azure mediaservices Encoding](media-services-encode-asset.md)

[Coderen met Media Encoder Standard](media-services-dotnet-encode-with-media-encoder-standard.md)

