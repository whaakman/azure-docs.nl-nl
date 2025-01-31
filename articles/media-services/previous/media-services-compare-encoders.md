---
title: Vergelijking van Azure op media coderingsprogramma | Microsoft Docs
description: Dit onderwerp worden de coderingsmogelijkheden van vergeleken **Media Encoder Standard** en **Media Encoder Premium Workflow**.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: a79437c0-4832-423a-bca8-82632b2c47cc
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako;anilmur
ms.openlocfilehash: bb827b80f79a53f30074b9230efe3e2049471051
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "61465708"
---
# <a name="comparison-of-azure-on-demand-media-encoders"></a>Vergelijking van Azure op de media-coderingsprogramma  

Dit onderwerp worden de coderingsmogelijkheden van vergeleken **Media Encoder Standard** en **Media Encoder Premium Workflow**.

## <a name="video-and-audio-processing-capabilities"></a>Mogelijkheden voor de verwerking van video en audio

De volgende tabel vergelijkt de functionaliteit tussen Media Encoder Standard (MES) en Media Encoder Premium Workflow (MEPW). 

|Mogelijkheid|Media Encoder Standard|Media Encoder Premium Workflow|
|---|---|---|
|Voorwaardelijke logica wordt toegepast tijdens het coderen<br/>(bijvoorbeeld, als de invoer HD is, klikt u vervolgens coderen 5.1 audio)|Nee|Ja|
|Ondertiteling|Nee|[Ja](media-services-premium-workflow-encoder-formats.md#closed_captioning)|
|[Correctie van Dolby® professionele volume](https://www.dolby.com/us/en/technologies/dolby-professional-loudness-solutions.pdf)<br/> met de dialoog Intelligence™|Nee|Ja|
|Ongedaan maken ineengestrengeld, inverse telecine|Basic|Broadcast-kwaliteit|
|Detecteren en verwijderen van zwarte randen <br/>(pillarboxes, postbussen)|Nee|Ja|
|Miniaturen genereren|[Ja](media-services-dotnet-generate-thumbnail-with-mes.md)|[Ja](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)|
|Knippen/bijsnijden en het samenvoegen van video 's|[Ja](media-services-advanced-encoding-with-mes.md#trim_video)|Ja|
|Overlays van audio of video|[Ja](media-services-advanced-encoding-with-mes.md#overlay)|[Ja](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-1--overlay-an-image-on-top-of-the-video)|
|Overlays van afbeeldingen|Van de installatiekopie van bronnen|Van afbeeldingen en tekst bronnen|
|Meerdere audio taal|Beperkt|[Ja](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-2--multiple-audio-language-encoding)|

## <a id="billing"></a>Facturering-meter die wordt gebruikt door elke coderingsprogramma
| Naam van Mediaprocessor | Van toepassing prijzen | Opmerkingen |
| --- | --- | --- |
| **Media Encoder Standard** |ENCODER |Codering taken wordt in rekening gebracht op basis van de totale duur, in minuten, alle mediabestanden die worden geproduceerd als uitvoer, tegen het tarief dat [hier][1], onder de kolom CODERINGSPROGRAMMA. |
| **Media Encoder Premium Workflow** |PREMIUM ENCODER |Codering taken wordt in rekening gebracht op basis van de totale duur, in minuten, alle mediabestanden die worden geproduceerd als uitvoer, tegen het tarief dat [hier][1], in de PREMIUM ENCODER-kolom. |

## <a name="input-containerfile-formats"></a>Invoer/bestandsindelingen
| Invoer/bestandsindelingen | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| Adobe® Flash® F4V |Ja |Ja |
| MXF/SMPTE 377M |Ja |Ja |
| GXF |Ja |Ja |
| MPEG-2-Transport Streams |Ja |Ja |
| Programma voor MPEG-2-Streams |Ja |Ja |
| MPEG-4/MP4 |Ja |Ja |
| Windows Media/ASF |Ja |Ja |
| AVI (niet-gecomprimeerde 8-bits/10 bits) |Ja |Ja |
| 3GPP/3GPP2 |Ja |Nee |
| Smooth Streaming-bestandsindeling (PIFF 1.3) |Ja |Nee |
| [Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) |Ja |Nee |
| Matroska/WebM |Ja |Nee |
| QuickTime (.mov) |Ja |Nee |

## <a name="input-video-codecs"></a>Codecs invoervideo
| Codecs Invoervideo | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| AVC 8-bits/10 bits, maximaal 4:2:2, inclusief AVCIntra |8-bits 4:2:0 en 4:2:2 |Ja |
| Avid DNxHD (in MXF) |Ja |Ja |
| DVCPro/DVCProHD (in MXF) |Ja |Ja |
| JPEG2000 |Ja |Ja |
| MPEG-2 (maximaal 422-profiel en hoog niveau, inclusief varianten zoals XDCAM, XDCAM HD XDCAM IMX, CableLabs® en D10) |Maximaal 422-profiel |Ja |
| MPEG-1 |Ja |Ja |
| Windows Media Video/VC-1 |Ja |Ja |
| Canopus hoofdkantoor/HQX |Nee |Nee |
| MPEG-4-deel 2 |Ja |Nee |
| [Theora](https://en.wikipedia.org/wiki/Theora) |Ja |Nee |
| Apple ProRes 422 |Ja |Nee |
| Apple ProRes 422 LT |Ja |Nee |
| Apple ProRes 422 hoofdkantoor |Ja |Nee |
| Apple ProRes Proxy |Ja |Nee |
| Apple ProRes 4444 |Ja |Nee |
| Apple ProRes 4444 XQ |Ja |Nee |
| HEVC/H.265|Belangrijkste profiel|Hoofd- en Main 10-profiel|

## <a name="input-audio-codecs"></a>Codecs audio-invoer
| Codecs Audio-invoer | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| AES (SMPTE 331 M en 302 M, AES3-2003) |Nee |Ja |
| Dolby® E |Nee |Ja |
| Dolby® Digital (AC3) |Nee |Ja |
| Dolby® Digital Plus (E-AC3) |Nee |Ja |
| AAC (AAC-LC, AAC-HE en AAC-HEv2; tot. 5.1) |Ja |Ja |
| MPEG-laag 2 |Ja |Ja |
| MP3 (laag 3 Audio MPEG-1) |Ja |Ja |
| Windows Media Audio |Ja |Ja |
| WAV/PCM |Ja |Ja |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |Ja |Nee |
| [Opus](https://en.wikipedia.org/wiki/Opus_\(audio_format\)) |Ja |Nee |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |Ja |Nee |

## <a name="output-containerfile-formats"></a>Uitvoer/bestandsindelingen
| Uitvoer/bestandsindelingen | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| Adobe® Flash® F4V |Nee |Ja |
| MXF (OP1a, XDCAM en AS02) |Nee |Ja |
| DPP (inclusief AS11) |Nee |Ja |
| GXF |Nee |Ja |
| MPEG-4/MP4 |Ja |Ja |
| MPEG-TS |Ja |Ja |
| Windows Media/ASF |Nee |Ja |
| AVI (niet-gecomprimeerde 8-bits/10 bits) |Nee |Ja |
| Smooth Streaming-bestandsindeling (PIFF 1.3) |Nee |Ja |

## <a name="output-video-codecs"></a>Codecs invoervideo voor uitvoer
| Codecs Invoervideo voor uitvoer | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| AVC (H.264; 8-bits; tot hoge profiel, niveau 5.2; 4 K Ultra HD; AVC Intra) |Alleen 8 bit 4:2:0 |Ja |
| HEVC (H.265; 8-bits en 10-bits;)  |Nee |Ja |
| Avid DNxHD (in MXF) |Nee |Ja |
| MPEG-2 (maximaal 422-profiel en hoog niveau, inclusief varianten zoals XDCAM, XDCAM HD XDCAM IMX, CableLabs® en D10) |Nee |Ja |
| MPEG-1 |Nee |Ja |
| Windows Media Video/VC-1 |Nee |Ja |
| JPEG-miniatuur maken |Ja |Ja |
| PNG-miniaturen maken |Ja |Ja |
| Maken van miniaturen BMP |Ja |Nee |

## <a name="output-audio-codecs"></a>Codecs audio-uitvoer
| Codecs Audio-uitvoer | Media Encoder Standard | Media Encoder Premium Workflow |
| --- | --- | --- |
| AES (SMPTE 331 M en 302 M, AES3-2003) |Nee |Ja |
| Dolby® Digital (AC3) |Nee |Ja |
| Dolby® Digital Plus (E-AC3) tot 7.1 |Nee |Ja |
| AAC (AAC-LC, AAC-HE en AAC-HEv2; tot. 5.1) |Ja |Ja |
| MPEG-laag 2 |Nee |Ja |
| MP3 (laag 3 Audio MPEG-1) |Nee |Ja |
| Windows Media Audio |Nee |Ja |

>[!NOTE]
>Als u codeert naar Dolby® Digital (AC3), kan de uitvoer alleen in een ISO-MP4-bestand worden geschreven.

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Verwante artikelen:
* [Geavanceerde codering taken uitvoeren door aan te passen voorinstellingen voor Media Encoder Standard](media-services-custom-mes-presets-with-dotnet.md)
* [Quota en beperkingen](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
