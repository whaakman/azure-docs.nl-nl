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
ms.openlocfilehash: c16a3ff28eff18b1638fbdbac2282b690093783e
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2019
ms.locfileid: "57244924"
---
# <a name="dynamic-packaging"></a>Dynamische verpakking

Microsoft Azure Media Services kunnen worden gebruikt om veel media bron-bestandsindelingen, streaming-indelingen, media en beveiliging van inhoud die is geformatteerd met tal van client-technologieën (bijvoorbeeld iOS- en XBOX genoemd). Deze clients verschillende protocollen te begrijpen, bijvoorbeeld iOS heeft dat een HTTP Live Streaming (HLS)-indeling en Xbox vereisen Smooth Streaming. Als u een set adaptive bitrate (multi-bitrate) hebt MP4-bestanden (ISO Base Media 14496-12) of een set adaptive bitrate Smooth Streaming-bestanden die u wilt gebruiken op clients die HLS, MPEG-DASH of Smooth Streaming begrijpen, kunt u profiteren van dynamische Verpakking. De pakketten is agnostisch ten opzichte van de video oplossing, SD/HD/UHD - 4K worden ondersteund.

[Streaming-eindpunten](streaming-endpoint-concept.md) is de service voor dynamische pakketten in Media Services gebruikt voor het leveren van media-inhoud naar client spelers. Dynamische pakketten is een functie die wordt standaard geleverd op alle **Streaming-eindpunten** (Standard of Premium). Er zijn geen extra kosten die gepaard gaan met deze functie in Media Services v3. 

Om te profiteren van **dynamische pakketten**, moet u beschikken over een **Asset** met een set adaptive bitrate MP4-bestanden en streaming-configuratiebestanden die nodig zijn voor Media Services dynamische pakketten. Er is één manier om de bestanden naar Codeer uw tussentijds (bron) bestand met Media Services. Als u video's in de gecodeerde Asset beschikbaar voor clients om te worden afgespeeld, u moet maken een **Streaming-Locator gemaakt** en streaming-URL's te bouwen. Klik, op basis van de indeling die is opgegeven in de client streamingmanifest (HLS, DASH of Smooth), u de stream ontvangt in het protocol dat u hebt gekozen.

Hierdoor hoeft u voor slechts één opslagindeling de bestanden op te slaan en hiervoor te betalen. De Media Services-service bouwt en levert de juiste reactie op basis van aanvragen van een client. 

In Media Services dynamische pakketten gebruikt of u live of on-demand streaming. Het volgende diagram toont de streaming on demand met dynamische verpakking werkstroom.

![Dynamische verpakking](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

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

## <a name="manifests"></a>Manifesten 
 
Media Services biedt ondersteuning voor HLS, MPEG DASH, Smooth Streaming-protocollen. Als onderdeel van **dynamische verpakking**, de streaming clientmanifesten (Master HLS Playlist, DASH Media presentatie beschrijving (MPD) en Smooth Streaming) dynamisch worden gegenereerd op basis van de selectie van de indeling in de URL. Zie de protocollen voor levering in [in deze sectie](#delivery-protocols). 

Een manifestbestand bevat metagegevens zoals streaming: type (audio, video of tekst) bijhouden, bijhouden van de naam, begintijd en eindtijd, bitrate (kenmerken), talen, bijhouden, presentatievenster (sliding window van vaste duur), video-codec (code). Hiermee geeft u ook de speler om op te halen van het volgende fragment door te geven informatie over het volgende kan worden afgespeeld video fragmenten beschikbaar en hun locatie. Fragmenten (of segmenten) zijn de werkelijke 'segmenten' van een video-inhoud.

### <a name="hls-master-playlist"></a>HLS Master afspeellijst

Hier volgt een voorbeeld van een manifestbestand HLS: 

```
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(381048)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(381048)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721495)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721495)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1154816)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1154816)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2217354)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2217354)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579827)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579827)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

### <a name="dash-media-presentation-description-mpd"></a>Beschrijving van de presentatie DASH Media (MPD)

Hier volgt een voorbeeld van een DASH-manifest:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" profiles="urn:mpeg:dash:profile:isoff-live:2011" type="static" mediaPresentationDuration="PT1M10.315S" minBufferTime="PT7S">
   <Period>
      <AdaptationSet id="1" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.2" lang="en">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60160000" r="10" />
               <S d="41386666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="5_A_aac_eng_2_128041_2_1_1" bandwidth="128041" audioSamplingRate="48000" />
      </AdaptationSet>
      <AdaptationSet id="2" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.640020" maxWidth="1280" maxHeight="720" startWithSAP="1">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60060000" r="10" />
               <S d="42375666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="1_V_video_1" bandwidth="3579827" width="1280" height="720" />
         <Representation id="1_V_video_2" bandwidth="2217354" codecs="avc1.64001F" width="960" height="540" />
         <Representation id="1_V_video_3" bandwidth="1154816" codecs="avc1.64001E" width="640" height="360" />
         <Representation id="1_V_video_4" bandwidth="721495" codecs="avc1.640015" width="480" height="270" />
         <Representation id="1_V_video_5" bandwidth="381048" codecs="avc1.64000D" width="320" height="180" />
      </AdaptationSet>
   </Period>
</MPD>
```
### <a name="smooth-streaming"></a>Smooth Streaming

Hier volgt een voorbeeld van een manifest Smooth Streaming:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="703146666" TimeScale="10000000">
   <StreamIndex Chunks="12" Type="audio" Url="QualityLevels({bitrate})/Fragments(aac_eng_2_128041_2_1={start time})" QualityLevels="1" Language="eng" Name="aac_eng_2_128041_2_1">
      <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="128041" FourCC="AACL" CodecPrivateData="1190" Channels="2" PacketSize="4" SamplingRate="48000" />
      <c t="0" d="60160000" r="11" />
      <c d="41386666" />
   </StreamIndex>
   <StreamIndex Chunks="12" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="5">
      <QualityLevel Index="0" Bitrate="3579827" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="0000000167640020ACD9405005BB011000003E90000EA600F18319600000000168EBECB22C" />
      <QualityLevel Index="1" Bitrate="2217354" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FACD940F0117EF01100000303E90000EA600F1831960000000168EBECB22C" />
      <QualityLevel Index="2" Bitrate="1154816" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EACD940A02FF9701100000303E90000EA600F162D960000000168EBECB22C" />
      <QualityLevel Index="3" Bitrate="721495" FourCC="H264" MaxWidth="480" MaxHeight="270" CodecPrivateData="0000000167640015ACD941E08FEB011000003E90000EA600F162D9600000000168EBECB22C" />
      <QualityLevel Index="4" Bitrate="381048" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DACD941419F9F011000003E90000EA600F14299600000000168EBECB22C" />
      <c t="0" d="60060000" r="11" />
      <c d="42375666" />
   </StreamIndex>
</SmoothStreamingMedia>
```
## <a name="next-steps"></a>Volgende stappen

[Uploaden, coderen, stream-video 's](stream-files-tutorial-with-api.md)

