---
title: Media Encoder Premium Workflow-indelingen en codecs voor | Microsoft Docs
description: In dit onderwerp overzicht een van indelingen voor Media Encoder Premium Workflow-indelingen en codecs voor
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
ms.date: 01/15/2019
ms.author: juliako;anilmur
ms.openlocfilehash: 9c507bf1526bbee78d519362fc51453803820d57
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58108389"
---
# <a name="media-encoder-premium-workflow-formats-and-codecs"></a>Media Encoder Premium Workflow-indelingen en codecs voor

> [!NOTE]
> Voor premium encoder vragen, e- mepd@microsoft.com.
> 
> Media Encoder Premium Workflow Mediaprocessor besproken in dit onderwerp is niet beschikbaar in China. 

Dit document bevat een overzicht van de invoer- en indelingen en codecs voor die worden ondersteund door de openbare preview-versie van de **Media Encoder Premium Workflow** coderingsprogramma.

[De invoer-indelingen en codecs voor Media Encoder Premium Workflow](#input_formats)

Media Encoder Premium Workflow uitvoer-indelingen en codecs voor

**Media Encoder Premium Workflow** ondersteunt ondertiteling in beschreven [dit](#closed_captioning) sectie. 

## <a id="input_formats"></a>De invoer-indelingen en codecs voor Media Encoder Premium Workflow

De volgende sectie vindt u de codecs- en bestandsindelingen die ondersteuning biedt voor de Mediaprocessor als invoer.

### <a name="input-containerfile-formats"></a>Invoer/bestandsindelingen

* Adobe® Flash® F4V
* MXF/SMPTE 377M
* GXF
* MPEG-2-Transport Streams
* Programma voor MPEG-2-Streams
* MPEG-4/MP4
* Windows Media/ASF
* AVI (niet-gecomprimeerde 8-bits/10 bits)

### <a name="input-video-codecs"></a>Codecs Invoervideo

* AVC 8-bits/10 bits, maximaal 4:2:2, inclusief AVCIntra
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* HEVC/H.265, hoofd- en Main 10-profiel
* JPEG2000
* MPEG-2 (maximaal 422-profiel en hoog niveau, inclusief varianten zoals XDCAM, XDCAM HD XDCAM IMX, CableLabs® en D10)
* MPEG-1
* Windows Media Video/VC-1

### <a name="input-audio-codecs"></a>Codecs Audio-invoer

* AES (SMPTE 331 M en 302 M, AES3-2003)
* Dolby® E
* Dolby® Digital (AC3)
* AAC (AAC-LC, AAC-HE en AAC-HEv2; tot. 5.1)
* MPEG-laag 2
* MP3 (laag 3 Audio MPEG-1)
* Windows Media Audio
* WAV/PCM

## <a id="output_format"></a>Media Encoder Premium Workflow uitvoer-indelingen en codecs voor

Het volgende gedeelte bevat de codecs en bestandsindelingen die worden ondersteund als uitvoer van de Mediaprocessor.

### <a name="output-containerfile-formats"></a>Uitvoer/bestandsindelingen

* Adobe® Flash® F4V
* MXF (OP1a, XDCAM en AS02)
* DPP (inclusief AS11)
* GXF
* MPEG-4/MP4
* Windows Media/ASF
* AVI (niet-gecomprimeerde 8-bits/10 bits)
* Smooth Streaming-bestandsindeling (PIFF 1.3)
* MPEG-TS 

### <a name="output-video-codecs"></a>Codecs Invoervideo voor uitvoer

* AVC (H.264; 8-bits; tot hoge profiel, niveau 5.2; 4 K Ultra HD; AVC Intra)
* Avid DNxHD (in MXF)
* DVCPro/DVCProHD (in MXF)
* MPEG-2 (maximaal 422-profiel en hoog niveau, inclusief varianten zoals XDCAM, XDCAM HD XDCAM IMX, CableLabs® en D10)
* MPEG-1
* Windows Media Video/VC-1
* JPEG-miniatuur maken
* HEVC (H.265; 8-bits en 10 bits, hoofd- en Main-10-profiel)

  Ondersteuning voor Kopregel 10 is beschikbaar in bepaalde scenario's, neem contact op met mepd@microsoft.com voor meer informatie


### <a name="output-audio-codecs"></a>Codecs Audio-uitvoer

* AES (SMPTE 331 M en 302 M, AES3-2003)
* Dolby® Digital (AC3)
* Dolby® Digital Plus (E-AC3) tot 7.1
* AAC (AAC-LC, AAC-HE en AAC-HEv2; tot. 5.1)
* MPEG-laag 2
* MP3 (laag 3 Audio MPEG-1)
* Windows Media Audio

>[!NOTE]
>Als u codeert naar Dolby® Digital (AC3), kan de uitvoer alleen in een ISO-MP4-bestand worden geschreven.

## <a id="closed_captioning"></a>Ondersteuning voor ondertiteling

Op opnemen, **Media Encoder Premium Workflow** ondersteunt:

1. SCC bestanden
2. SMPTE TT-bestanden
3. CEA-608/CEA-708-uitgevoerd als gebruikersgegevens (SEI berichten van H.264 elementaire stromen, ATSC/53, SCTE20) of uitgevoerd als aanvullende gegevens in MXF/GXF-bestanden
4. STL subtitel bestanden

Op de uitvoer van zijn de volgende opties beschikbaar:

1. CEA-608 CEA-708 vertaling
2. CEA-608/CEA-708 doorgeven (ingesloten in SEI berichten van H.264 elementaire stromen of uitgevoerd als aanvullende gegevens in MXF-bestanden)
3. SCC
4. SMPTE is een time-out opgetreden tekst (van bron CEA-608 per SMPTE RP2052, inclusief het maken van het bestand DFXP)
5. Het ondertitelbestand is SRT
6. DVB subtitel streams

> [!NOTE]
> Niet alle van de bovenstaande uitvoerindelingen worden ondersteund voor levering via streaming in Azure Media Services.

## <a name="known-issues"></a>Bekende problemen

Als uw invoervideo geen bevat bevat ondertiteling, de uitvoer Asset nog steeds een leeg TTML-bestand. 

## <a name="media-services-learning-paths"></a>Media Services-leertrajecten

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geven

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

