---
title: Standaardinstellingen voor Media Encoder Standard (MES) taken | Microsoft Docs
description: Het onderwerp biedt en overzicht van de service gedefinieerde voorbeeld voorinstellingen voor Media Encoder Standard (MES).
author: Juliako
manager: femila
editor: johndeu
services: media-services
documentationcenter: ''
ms.assetid: f243ed1c-ac9c-4300-a5f7-f092cf9853b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 50c52369a5a957a4dd6279cac5079e2dea023106
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260023"
---
# <a name="sample-presets-for-media-encoder-standard-mes"></a>Voorbeeld voorinstellingen voor Media Encoder Standard (MES)

**Media Encoder Standard** definieert een reeks vooraf gedefinieerde system voorinstellingen kunt u bij het maken van coderingstaken coderen. Het verdient aanbeveling gebruik van de 'adaptief streamen"vooraf ingesteld als u wilt coderen van een video streamen met Media Services. Wanneer u opgeeft in deze vooraf ingestelde, Media Encoder Standard wordt [automatisch een bitrateladder genereren](media-services-autogen-bitrate-ladder-with-mes.md). 

### <a name="creating-custom-presets-from-samples"></a>Het maken van aangepaste voorinstellingen van voorbeelden
Media Services biedt volledige ondersteuning voor het aanpassen van alle waarden in de standaardinstellingen om te voldoen aan uw specifieke behoeften voor codering en vereisten. Als u nodig hebt om aan te passen van een vooraf ingestelde standaardcodering, moet u beginnen met een van de onderstaande systeemwaarden die in deze sectie worden gegeven als een sjabloon voor uw aangepaste configuratie. Voor een uitleg van wat elk element in deze middelen voorinstellingen, en de geldige waarden voor elk element, Zie de [Media Encoder Standard schema](media-services-mes-schema.md) onderwerp.  
  
> [!NOTE]
>  Wanneer u een vooraf ingestelde voor 4k codeert, krijgt u de `S3` gereserveerde-eenheidstype. Zie voor meer informatie, [hoe schaal codering](https://azure.microsoft.com/documentation/articles/media-services-portal-encoding-units).  

#### <a name="video-rotation-default-setting-in-presets"></a>Standaardinstelling video te draaien in voorinstellingen:
Als u werkt met Media Encoder Standard, wordt video rotatie is standaard ingeschakeld. Als uw video is vastgelegd op een mobiel apparaat in de staande modus, klikt u vervolgens draait deze voorinstellingen ze naar de modus Liggend voordat ze worden gecodeerd.
 
## <a name="available-presets"></a>Voorinstellingen voor beschikbaar: 

 [H264 Multiple Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md) produceert een reeks van 8 GOP uitgelijnde MP4-bestanden, variërend van 6000 kbps tot 400 kbps en AAC-5.1 audio.  
  
 [H264 Multiple Bitrate 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md) produceert een reeks van 8 GOP uitgelijnde MP4-bestanden, variërend van 6000 kbps tot 400 kbps en stereo AAC-audio.  
  
 [H264 Multiple Bitrate 16 x 9 voor iOS](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md) produceert een reeks van 8 GOP uitgelijnde MP4-bestanden, variërend van 8500 kbps tot 200 kbps en stereo AAC-audio.  
  
 [H264 Multiple Bitrate 16 x 9 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md) produceert een reeks van 5 GOP uitgelijnde MP4-bestanden, variërend van 1900 kbps tot 400 kbps en AAC-5.1 audio.  
  
 [H264 Multiple Bitrate 16 x 9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md) produceert een reeks van 5 GOP uitgelijnde MP4-bestanden, variërend van 1900 kbps tot 400 kbps en stereo AAC-audio.  
  
 [H264 Multiple Bitrate 4K-Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md) produceert een reeks van 12 GOP uitgelijnde MP4-bestanden, variërend van 20000 kbps tot 1000 kbps en AAC-5.1 audio.  
  
 [H264 Multiple Bitrate 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md) produceert een reeks van 12 GOP uitgelijnde MP4-bestanden, variërend van 20000 kbps tot 1000 kbps en stereo AAC-audio.  
  
 [H264 Multiple Bitrate 4 x 3 voor iOS](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md) produceert een reeks van 8 GOP uitgelijnde MP4-bestanden, variërend van 8500 kbps tot 200 kbps en stereo AAC-audio.  
  
 [H264 Multiple Bitrate 4 x 3 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md) produceert een reeks van 5 GOP uitgelijnde MP4-bestanden, variërend van 1600 kbps tot 400 kbps en AAC-5.1 audio.  
  
 [H264 Multiple Bitrate 4 x 3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md) produceert een reeks van 5 GOP uitgelijnde MP4-bestanden, variërend van 1600 kbps tot 400 kbps en stereo AAC-audio.  
  
 [H264 Multiple Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md) produceert een set 6 GOP uitgelijnde MP4-bestanden, variërend van 3400 kbps tot 400 kbps en AAC-5.1 audio.  
  
 [H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) produceert een set 6 GOP uitgelijnde MP4-bestanden, variërend van 3400 kbps tot 400 kbps en stereo AAC-audio.  
  
 [H264 Single-Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 6750 kbps en AAC-5.1 audio.  
  
 [H264 Single-Bitrate 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md) produceert één MP4-bestand met een bitrate van 6750 kbps en stereo AAC-audio.  
  
 [H264 Single-Bitrate 4K-Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 18000 kbps en AAC-5.1 audio.  
  
 [H264 Single-Bitrate 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md) produceert één MP4-bestand met een bitrate van 18000 kbps en stereo AAC-audio.  
  
 [H264 Single-Bitrate 4 x 3 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 1800 kbps en AAC-5.1 audio.  
  
 [H264 Single-Bitrate 4 x 3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md) produceert één MP4-bestand met een bitrate van 1800 kbps en stereo AAC-audio.  
  
 [H264 Single-Bitrate 16 x 9 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 2200 kbps en AAC-5.1 audio.  
  
 [H264 Single-Bitrate 16 x 9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md) produceert één MP4-bestand met een bitrate van 2200 kbps en stereo AAC-audio.  
  
 [H264 Single-Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 4500 kbps en AAC-5.1 audio.  
  
 [H264 Single-Bitrate 720p voor Android](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md) vooraf ingestelde één MP4-bestand met een bitrate van 2000 kbps en stereo AAC produceert.  
  
 [H264 Single-Bitrate 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md) produceert één MP4-bestand met een bitrate van 4500 kbps en stereo AAC-audio.  
  
 [H264 één bitsnelheid van hoge kwaliteit SD voor Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md) produceert één MP4-bestand met een bitrate van 500 kbps en stereo AAC-audio...  
  
 [H264 Single Bitrate lage kwaliteit SD voor Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md) produceert één MP4-bestand met een bitrate van 56 kbps en stereo AAC-audio.  
  
 Zie voor meer informatie met betrekking tot Media Services-coderingsprogramma's [codering op aanvraag met Azure Media Services](https://azure.microsoft.com/documentation/articles/media-services-encode-asset/).
