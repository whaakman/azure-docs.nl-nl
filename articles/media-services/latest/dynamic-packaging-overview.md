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
ms.date: 02/26/2019
ms.author: juliako
ms.openlocfilehash: cd4eacc918acdf50bb256077030b86e121f663f0
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56985798"
---
# <a name="dynamic-packaging"></a>Dynamische verpakking

Microsoft Azure Media Services kunnen worden gebruikt om veel media bron-bestandsindelingen, streaming-indelingen, media en beveiliging van inhoud die is geformatteerd met tal van client-technologieën (bijvoorbeeld iOS- en XBOX genoemd). Deze clients verschillende protocollen te begrijpen, bijvoorbeeld iOS heeft dat een HTTP Live Streaming (HLS)-indeling en Xbox vereisen Smooth Streaming. Als u een set adaptive bitrate (multi-bitrate) hebt MP4-bestanden (ISO Base Media 14496-12) of een set adaptive bitrate Smooth Streaming-bestanden die u wilt gebruiken op clients die HLS, MPEG-DASH of Smooth Streaming begrijpen, kunt u profiteren van dynamische Verpakking. De pakketten is agnostisch ten opzichte van de video oplossing, SD/HD/UHD - 4K worden ondersteund.

[Streaming-eindpunten](streaming-endpoint-concept.md) is de service voor dynamische pakketten in Media Services gebruikt voor het leveren van media-inhoud naar client spelers. Dynamische pakketten is een functie die wordt standaard geleverd op alle **Streaming-eindpunten** (Standard of Premium). Er zijn geen extra kosten die gepaard gaan met deze functie in Media Services v3. 

Om te profiteren van **dynamische pakketten**, moet u beschikken over een **Asset** met een set adaptive bitrate MP4-bestanden en manifestbestanden. Er is één manier om de bestanden naar Codeer uw tussentijds (bron) bestand met Media Services. Als u video's in de Asset (met de server en client manifesten en gecodeerde MP4s) beschikbaar is voor clients om te worden afgespeeld, u moet maken een **Streaming-Locator gemaakt** en bouw vervolgens de streaming-URL's. Klik, op basis van de indeling die is opgegeven in het manifest van de client, u de stream ontvangt in het protocol dat u hebt gekozen.

Hierdoor hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. De Media Services-service bouwt en levert de juiste reactie op basis van aanvragen van een client. 

In Media Services dynamische pakketten gebruikt of u live of on-demand streaming. Het volgende diagram toont de streaming on demand met dynamische verpakking werkstroom.

![Dynamische codering](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

## <a name="common-video-on-demand-workflow"></a>Algemene werkstroom voor video op aanvraag

Hier volgt een algemene Media Services streaming-werkstroom waarbij dynamische pakketten wordt gebruikt.

1. Upload een bestand voor invoer (een tussentijds bestand genoemd). Bijvoorbeeld, H.264 MP4 of WMV (Zie voor de lijst met ondersteunde indelingen [indelingen ondersteund door de Media Encoder Standard](media-encoder-standard-formats.md).
2. Codeer uw tussentijds bestand op afspelen van H.264 MP4 adaptieve bitrate sets.
3. Publiceer de asset met de adaptive bitrate die MP4-set. U publiceert door het maken van een **Streaming-Locator gemaakt**.
4. URL's die zijn gericht op verschillende indelingen (HLS, Dash en Smooth Streaming) maken. De **Streaming-eindpunt** zou zorgen voor de juiste manifest en aanvragen voor deze verschillende indelingen.

## <a name="encode-to-adaptive-bitrate-mp4s"></a>Coderen naar adaptive bitrate MP4s

Voor informatie over [het coderen van een video met Media Services](encoding-concept.md), Zie de volgende voorbeelden:

* [Coderen in een HTTPS-URL met behulp van ingebouwde voorinstellingen](job-input-from-http-how-to.md)
* [Een lokaal bestand met behulp van ingebouwde voorinstellingen coderen](job-input-from-local-file-how-to.md)
* [Bouw een aangepaste voorinstelling wilt richten op uw specifieke vereisten voor scenario of het apparaat](customize-encoder-presets-how-to.md)

Zie voor een lijst van Media Encoder Standard indelingen en codecs voor [indelingen en codecs voor](media-encoder-standard-formats.md)

## <a name="delivery-protocols"></a>Levering protocollen

|Protocol|Voorbeeld|
|---|---|
|HLS V4-PROCESSORS |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl)`|
|HLS V3 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl-v3)`|
|HLS CMAF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-cmaf)`|
|CSF VOOR MPEG-DASH| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-csf)` |
|CMAF VOOR MPEG-DASH|`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-cmaf)` |
|Smooth Streaming| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest`|

## <a name="video-codecs-supported-by-dynamic-packaging"></a>Codecs invoervideo ondersteund door dynamische verpakking

MP4-bestanden, waarin video gecodeerd met biedt ondersteuning voor dynamische pakketten [H.264](https://en.m.wikipedia.org/wiki/H.264/MPEG-4_AVC) (MPEG-4 AVC of AVC1), [H.265](https://en.m.wikipedia.org/wiki/High_Efficiency_Video_Coding) (HEVC, hev1 of hvc1).

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Codecs audio wordt ondersteund door dynamische verpakking

MP4-bestanden, waarin de audio die is gecodeerd met biedt ondersteuning voor dynamische pakketten [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, hij AAC v1, v2 HE-AAC), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Enhanced AC-3 of E-AC3), of [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) DTS ( Snelle, DTS LBR, HD DTS, DTS HD zonder verlies).

> [!NOTE]
> Bestanden met biedt geen ondersteuning voor dynamische pakketten [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) audio (dit is een verouderde codec).

## <a name="manifest-files-overview"></a>Overzicht van de manifest-bestanden

Een **manifest** ()-afspeellijstbestand (op basis van tekst of XML-indeling) bevat metagegevens zoals streaming: type (audio, video of tekst) bijhouden, bijhouden van de naam, begintijd en eindtijd, bitrate (kenmerken), talen, bijhouden, presentatievenster (schuiven venster van vaste duur), video-codec (code). Hiermee geeft u ook de speler om op te halen van het volgende fragment door te geven informatie over het volgende kan worden afgespeeld video fragmenten beschikbaar en hun locatie. Fragmenten (of segmenten) zijn de werkelijke 'segmenten' van een video-inhoud.

Hier volgt een voorbeeld van een manifestbestand HLS: 

```
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(380658)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(380658)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721426)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721426)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1155246)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1155246)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2218559)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2218559)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579378)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579378)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

## <a name="next-steps"></a>Volgende stappen

[Uploaden, coderen, stream-video 's](stream-files-tutorial-with-api.md)

