---
title: Standaardinstellingen van de taak voor Media Encoder Standard (MES) | Microsoft Docs
description: Het onderwerp biedt en overzicht van de standaardinstellingen van de voorbeeld-service is gedefinieerd voor Media Encoder Standard (MES).
author: Juliako
manager: cfow
editor: johndeu
services: media-services
documentationcenter: 
ms.assetid: f243ed1c-ac9c-4300-a5f7-f092cf9853b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/01/2017
ms.author: juliako
ms.openlocfilehash: 5753b1dffe5a1a4ee069b83f58e9c2dac433b89d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sample-presets-for-media-encoder-standard-mes"></a>Voorbeeld standaardinstellingen voor Media Encoder Standard (MES)

**Media Encoder Standard** definieert een aantal vooraf gedefinieerde standaardcodering standaardinstellingen die u gebruiken kunt bij het maken van codering taken van het systeem. Het verdient aanbeveling gebruik van de 'adaptief streamen' voorinstelling als u coderen een video wilt voor streaming met Media Services. Wanneer u opgeeft in deze vooraf ingestelde, Media Encoder Standard wordt [automatisch genereren een ladder bitrate](media-services-autogen-bitrate-ladder-with-mes.md). 

### <a name="creating-custom-presets-from-samples"></a>Maken van aangepaste standaardinstellingen van voorbeelden
Media Services biedt volledige ondersteuning voor het aanpassen van alle waarden in de standaardinstellingen om te voldoen aan uw specifieke behoeften voor codering en vereisten. Als u een codering voorinstelling aanpassen wilt, moet u eerst met een van de onderstaande systeemwaarden die beschikbaar zijn in deze sectie als een sjabloon voor uw aangepaste configuratie. Zie voor uitleg van welke elk element in deze standaardinstellingen middelen en de geldige waarden voor elk element in de [Media Encoder Standard schema](media-services-mes-schema.md) onderwerp.  
  
> [!NOTE]
>  Wanneer u een definitie voor 4k codeert, krijgt u de `S3` eenheidstype gereserveerd. Zie voor meer informatie [How to Scale codering](https://azure.microsoft.com/documentation/articles/media-services-portal-encoding-units).  

#### <a name="video-rotation-default-setting-in-presets"></a>Standaardinstelling video te draaien in standaardinstellingen:
Als u werkt met Media Encoder Standard, wordt video te draaien is standaard ingeschakeld. Als uw video is vastgelegd op een mobiel apparaat in de modus Staand, klikt u vervolgens draait deze standaardinstellingen deze liggend voordat ze worden gecodeerd.
 
## <a name="available-presets"></a>Beschikbare standaardinstellingen: 

 [Geselecteerde instelling H264 Multiple Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-1080p-Audio-5.1.md) levert een set van 8 GOP uitgelijnde MP4-bestanden, variërend van 6000 kbps tot 400 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 1080p](media-services-mes-preset-H264-Multiple-Bitrate-1080p.md) levert een set van 8 GOP uitgelijnde MP4-bestanden, variërend van 6000 kbps tot 400 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 16 x 9 voor iOS](media-services-mes-preset-H264-Multiple-Bitrate-16x9-for-iOS.md) levert een set van 8 GOP uitgelijnde MP4-bestanden, variërend van 8500 kbps tot 200 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 16 x 9 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD-Audio-5.1.md) levert een set van 5 GOP uitgelijnde MP4-bestanden, variërend van 1900 kbps tot 400 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 16 x 9 SD](media-services-mes-preset-H264-Multiple-Bitrate-16x9-SD.md) levert een set van 5 GOP uitgelijnde MP4-bestanden, variërend van 1900 kbps tot 400 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4K-Audio-5.1.md) levert een set 12 GOP uitgelijnde MP4-bestanden, variërend van 20000 kbps tot 1000 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 4K](media-services-mes-preset-H264-Multiple-Bitrate-4K.md) levert een set 12 GOP uitgelijnde MP4-bestanden, variërend van 20000 kbps tot 1000 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 4 x 3 voor iOS](media-services-mes-preset-H264-Multiple-Bitrate-4x3-for-iOS.md) levert een set van 8 GOP uitgelijnde MP4-bestanden, variërend van 8500 kbps tot 200 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 4 x 3 SD Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD-Audio-5.1.md) levert een set van 5 GOP uitgelijnde MP4-bestanden, variërend van 1600 kbps tot 400 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 4 x 3 SD](media-services-mes-preset-H264-Multiple-Bitrate-4x3-SD.md) levert een set van 5 GOP uitgelijnde MP4-bestanden, variërend van 1600 kbps tot 400 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Multiple-Bitrate-720p-Audio-5.1.md) levert een set 6 GOP uitgelijnde MP4-bestanden, variërend van 3400 kbps tot 400 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Multiple Bitrate 720p](media-services-mes-preset-H264-Multiple-Bitrate-720p.md) levert een set 6 GOP uitgelijnde MP4-bestanden, variërend van 3400 kbps tot 400 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 1080p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-1080p-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 6750 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 1080p](media-services-mes-preset-H264-Single-Bitrate-1080p.md) produceert één MP4-bestand met een bitrate van 6750 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 4K Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4K-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 18000 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 4K](media-services-mes-preset-H264-Single-Bitrate-4K.md) produceert één MP4-bestand met een bitrate van 18000 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 4 x 3 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-4x3-SD-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 1800 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 4 x 3 SD](media-services-mes-preset-H264-Single-Bitrate-4x3-SD.md) produceert één MP4-bestand met een bitrate van 1800 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 16 x 9 SD Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-16x9-SD-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 2200 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 16 x 9 SD](media-services-mes-preset-H264-Single-Bitrate-16x9-SD.md) produceert één MP4-bestand met een bitrate van 2200 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 720p Audio 5.1](media-services-mes-preset-H264-Single-Bitrate-720p-Audio-5.1.md) produceert één MP4-bestand met een bitrate van 4500 kbps en AAC 5.1 audio.  
  
 [Geselecteerde instelling H264 Single-Bitrate 720p voor Android](media-services-mes-preset-H264-Single-Bitrate-720p-for-Android.md) voorinstelling produceert één MP4-bestand met een bitrate van 2000 kbps en aansluiting AAC.  
  
 [Geselecteerde instelling H264 Single-Bitrate 720p](media-services-mes-preset-H264-Single-Bitrate-720p.md) produceert één MP4-bestand met een bitrate van 4500 kbps en aansluiting AAC audio.  
  
 [Geselecteerde instelling H264 één bitsnelheid van hoge kwaliteit SD voor Android](media-services-mes-preset-H264-Single-Bitrate-High-Quality-SD-for-Android.md) produceert één MP4-bestand met een bitrate van 500 kbps en stereogeluid AAC...  
  
 [Geselecteerde instelling H264 Single Bitrate lage kwaliteit SD voor Android](media-services-mes-preset-H264-Single-Bitrate-Low-Quality-SD-for-Android.md) produceert één MP4-bestand met een bitrate van 56 kbps en aansluiting AAC audio.  
  
 Zie voor meer informatie over Media Services coderingsprogramma's, [codering op aanvraag met Azure Media Services](https://azure.microsoft.com/en-us/documentation/articles/media-services-encode-asset/).
