---
title: Werkstroom voor Media Encoder Premium indelingen en codecs | Microsoft Docs
description: In dit onderwerp overzicht een van Media Encoder Premium werkstroom indelingen indelingen en codecs
services: media-services
documentationcenter: 
author: juliako
manager: erik43
editor: 
ms.assetid: b197fce8-3b9b-4189-8d08-486810c0426f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: e18de2adc9aac585d6890dd7b43a54f1a0ca177e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Werkstroom voor Media Encoder Premium indelingen en codecs
> [!NOTE]
> Premium-encoder vragen, e-mepd op Microsoft.com.
> 
> Media Encoder Premium media werkstroomverwerking beschreven in dit onderwerp is niet beschikbaar in China. 
> 
> 

Dit document bevat een overzicht van de invoer en uitvoer bestandsindelingen en codecs die worden ondersteund door de openbare preview-versie van de **Media Encoder Premium werkstroom** encoder.

[Media Encoder Premium Worflow invoer indelingen en Codecs](#input_formats)

[Media Encoder Premium Worflow uitvoerindelingen en Codecs](#output_formats)

**Werkstroom voor Media Encoder Premium** ondersteunt ondertiteling beschreven in [dit](#closed_captioning) sectie. 

## <a id="input_formats"></a>Werkstroom voor Media Encoder Premium invoer indelingen en Codecs
De volgende sectie worden de codecs en bestandsindelingen die ondersteuning biedt voor deze Mediaprocessor als invoer.

### <a name="input-containerfile-formats"></a>Invoer Container/bestandsindelingen
* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* Streams MPEG-2-Transport
* Streams MPEG-2-programma
* MPEG-4/MP4
* Windows Media/AVP
* AVI (niet-gecomprimeerde 8-bits/10 bits)

### <a name="input-video-codecs"></a>Video-Codecs invoer
* AVC 8-bits/10-bits, maximaal 4:2:2, inclusief AVCIntra
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* JPEG2000
* MPEG-2 (maximaal 422 profiel en een hoog niveau, inclusief varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)
* MPEG-1
* Windows Media Video/VC-1

### <a name="input-audio-codecs"></a>Audio-invoer Codecs
* AES (SMPTE 331 M en 302 M, AES3 2003)
* Dolby® E
* Dolby® digitale (AC3)
* AAC (AAC Kredietbrief AAC HE en AAC-HEv2; maximaal 5.1)
* MPEG-laag 2
* MP3 (MPEG-1 Audio laag 3)
* Windows Media Audio
* WAV PCM /

## <a id="output_format"></a>Werkstroom uitvoerindelingen voor Media Encoder Premium en Codecs
Het volgende gedeelte bevat de codecs en bestandsindelingen die worden ondersteund als uitvoer van deze Mediaprocessor.

### <a name="output-containerfile-formats"></a>Container per bestand uitvoerindelingen
* Adobe® Flash® F4V
* MXF (OP1a, XDCAM en AS02)
* DPP (inclusief AS11)
* GXF
* MPEG-4/MP4
* Windows Media/AVP
* AVI (niet-gecomprimeerde 8-bits/10 bits)
* Smooth Streaming-bestandsindeling (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Video-Codecs uitvoer
* AVC (H.264; 8-bits; niveau tot hoge profiel 5.2; 4 K Ultra HD; AVC Intra)
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* MPEG-2 (maximaal 422 profiel en een hoog niveau, inclusief varianten zoals XDCAM, XDCAM HD, XDCAM IMX, CableLabs® en D10)
* MPEG-1
* Windows Media Video/VC-1
* Maken van JPEG-miniaturen

### <a name="output-audio-codecs"></a>Audio-Codecs uitvoer
* AES (SMPTE 331 M en 302 M, AES3 2003)
* Dolby® digitale (AC3)
* Dolby® Digital Plus (E-AC3) tot 7.1
* AAC (AAC Kredietbrief AAC HE en AAC-HEv2; maximaal 5.1)
* MPEG-laag 2
* MP3 (MPEG-1 Audio laag 3)
* Windows Media Audio

>[!NOTE]
>Als u coderen naar Dolby® Digital (AC3), kan alleen de uitvoer naar een ISO MP4-bestand worden geschreven.

## <a id="closed_captioning"></a>Ondersteuning voor ondertiteling
Op opnemen, **Media Encoder Premium werkstroom** ondersteunt:

1. SCC bestanden
2. Bestanden SMPTE TT
3. De voor- of ten als aanvullende gegevens in bestanden MXF/GXF uitgevoerd als gebruikersgegevens (SEI berichten H.264 elementaire streams, ATSC/53, SCTE20) CEA-608/CEA-708:
4. STL subtitel van de bestanden

Op output zijn de volgende opties beschikbaar:

1. CEA-608 CEA 708 vertaling
2. CEA 608/CEA 708 doorgeven (ingesloten in SEI berichten van H.264 elementaire streams of uitgevoerd als aanvullende gegevens in MXF-bestanden)
3. SCC
4. SMPTE getimede tekst (van bron CEA 608 per SMPTE RP2052, ook DFXP bestand maken)
5. SRT subtitel bestand
6. DVB subtitel stromen

Opmerking: niet alle van de bovenstaande uitvoerindelingen voor levering via streaming in Azure Media Services worden ondersteund.

## <a name="known-issues"></a>Bekende problemen
Als uw invoervideo geen bevat nog ondertiteling, de uitvoer van de Asset bevatten steeds een leeg TTML-bestand. 

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

