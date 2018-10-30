---
title: Het bijsnijden van video's met Media Encoder Standard - Azure | Microsoft Docs
description: In dit artikel laat zien hoe bijsnijden video's met Media Encoder Standard.
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 29/10/2018
ms.author: anilmur;juliako;
ms.openlocfilehash: f6c853648b138763675e016d8de6eaff6377f166
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50229597"
---
# <a name="crop-videos-with-media-encoder-standard"></a>Video’s bijsnijden met Media Encoder Standard
Media Encoder Standard (MES) kunt u uw invoer video bijsnijden. Bijsnijden, is het proces van het selecteren van een rechthoekig venster in het kader van de video en codering van alleen de pixels binnen dit venster. Het volgende diagram kunt u het proces wordt verduidelijkt.

![Een video bijsnijden](./media/media-services-crop-video/media-services-crop-video01.png)

Stel dat u hebt als invoer een video die een resolutie van 1920 x 1080 pixels (hoogte-breedteverhouding 16:9), maar heeft een zwarte balken (pijler vakken) op de links en rechts, zodat alleen een venster 4:3 of 1440 x 1080 pixels bevat actieve video. U kunt MES gebruiken om bijsnijden of bewerken van de balken en de regio 1440 x 1080 coderen.

Bijsnijden in MES is een vooraf verwerken fase, zodat de bijsnijden parameters in de vooraf ingestelde standaardcodering van toepassing op de oorspronkelijke invoervideo. Codering is een latere fase en de breedte en hoogte-instellingen toepassen op de *vooraf verwerkte* video, en niet naar de oorspronkelijke video. Bij het ontwerpen van uw vooraf ingestelde moet u het volgende doen: (a) de parameters van de bijsnijden op basis van de oorspronkelijke invoervideo selecteert en (b) selecteert u uw instellingen op basis van de bijgesneden video coderen. Als u niet overeenkomt met uw instellingen op de bijgesneden video coderen, de uitvoer worden niet zoals verwacht.

De [volgende](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) onderwerp wordt beschreven hoe u een coderingstaak met MES maakt en hoe u om op te geven van een aangepaste voorinstelling voor de coderingstaak. 

## <a name="creating-a-custom-preset"></a>Het maken van een aangepaste voorinstelling
In het voorbeeld dat wordt weergegeven in het diagram:

1. Oorspronkelijke invoer is 1920 x 1080
2. Deze moeten worden bijgesneden tot een uitvoer van 1440 x 1080, die in het kader van de invoer wordt gecentreerd
3. Dit betekent dat een X-verschuiving van (1920-1440) / 2 = 240 en een Y-verschuiving van nul
4. De breedte en hoogte van de rechthoek bijsnijden zijn 1440 1080, respectievelijk en
5. In de fase coderen wordt de vraag is voor het produceren van drie lagen, resoluties 1440 x 1080, 960 x 720 en 480 x 360, respectievelijk zijn

### <a name="json-preset"></a>JSON-definitie
    {
      "Version": 1.0,
      "Sources": [
        {
          "Streams": [],
          "Filters": {
            "Crop": {
                "X": 240,
                "Y": 0,
                "Width": 1440,
                "Height": 1080
            }
          },
          "Pad": true
        }
      ],
      "Codecs": [
        {
          "KeyFrameInterval": "00:00:02",
          "H264Layers": [
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 3400,
              "MaxBitrate": 3400,
              "BufferWindow": "00:00:05",
              "Width": 1440,
              "Height": 1080,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 2250,
              "MaxBitrate": 2250,
              "BufferWindow": "00:00:05",
              "Width": 960,
              "Height": 720,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            },
            {
              "Profile": "Auto",
              "Level": "auto",
              "Bitrate": 1250,
              "MaxBitrate": 1250,
              "BufferWindow": "00:00:05",
              "Width": 480,
              "Height": 360,
              "BFrames": 3,
              "ReferenceFrames": 3,
              "AdaptiveBFrame": true,
              "Type": "H264Layer",
              "FrameRate": "0/1"
            }
          ],
          "Type": "H264Video"
        },
        {
          "Profile": "AACLC",
          "Channels": 2,
          "SamplingRate": 48000,
          "Bitrate": 128,
          "Type": "AACAudio"
        }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
          "Format": {
            "Type": "MP4Format"
          }
        }
      ]
    }


## <a name="restrictions-on-cropping"></a>Beperkingen voor bijsnijden
De functie voor bijsnijden is bedoeld om te worden handmatig. U moet uw invoervideo laden in een geschikte bewerken hulpprogramma waarmee u Selecteer frames van belang zijn, plaats de cursor om te bepalen verschuivingen voor de bijsnijden rechthoek, om te bepalen van de vooraf ingestelde standaardcodering die is afgestemd op die bepaalde video, enzovoort. Deze functie is niet bedoeld om in te schakelen, bijvoorbeeld: automatische detectie en het verwijderen van de zwarte letterbox/pillarbox randen in uw video-invoer.

De volgende beperkingen gelden voor de functie voor bijsnijden. Als deze niet wordt voldaan, kan de coderen taak mislukt of er onverwachte uitvoer gegenereerd.

1. De coördinaten en de grootte van de rechthoek bijsnijden hebben om te passen binnen de invoervideo
2. Zoals eerder vermeld, moet de breedte en hoogte in de instellingen voor coderen overeenkomen met de bijgesneden video
3. Bijsnijden is van toepassing op video's die zijn vastgelegd in de liggende modus (dat wil zeggen niet van toepassing op video's die zijn geregistreerd met een smartphone, die zijn ondergebracht in de staande modus of verticaal)
4. Werkt het beste met progressieve video vastgelegd met vierkante pixels

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Volgende stap
Azure Media Services-leertrajecten vindt u informatie over de handige functies die worden aangeboden door AMS zien.  

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
