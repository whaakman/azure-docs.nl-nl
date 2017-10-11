---
title: Het bijsnijden video's met Media Encoder Standard - Azure | Microsoft Docs
description: In dit artikel laat zien hoe bijsnijden video's met Media Encoder Standard.
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 7628f674-2005-4531-8b61-d7a4f53e46ba
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/09/2017
ms.author: anilmur;juliako;
ms.openlocfilehash: 60d0ce14a271fcbe698559da95ca011cb888b221
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="crop-videos-with-media-encoder-standard"></a>Video’s bijsnijden met Media Encoder Standard
Media Encoder Standard (MES) kunt u uw invoer video bijsnijden. Bijsnijden is het proces van een rechthoekig venster in het kader van de video te selecteren en alleen de pixels in dit venster codering. Het volgende diagram kunt u het proces wordt verduidelijkt.

![Een video bijsnijden](./media/media-services-crop-video/media-services-crop-video01.png)

Stel dat u hebt als invoer een video die een resolutie van 1920 x 1080 pixels (hoogte-breedteverhouding 16:9), maar wel met balken (pillar vakken) op de links, rechts, zodat alleen een venster 4:3 of 1440 x 1080 pixels active video bevat. U kunt MES bijsnijden of bewerken van de zwarte balken gebruiken en de regio 1440 x 1080 coderen.

Bijsnijden in MES is een vooraf verwerken fase, zodat de bijsnijden parameters in de codering voorinstelling op de oorspronkelijke invoer video toepassen. Codering is een latere fase en de breedte en hoogte-instellingen toepassen op de *vooraf verwerkt* video en niet naar de oorspronkelijke video. Bij het ontwerpen van uw vooraf ingestelde moet u de volgende handelingen uit: (a) Selecteer de bijsnijden parameters op basis van de oorspronkelijke invoervideo en (b) Selecteer de instellingen op basis van de bijgesneden video coderen. Als u niet overeenkomt met de instellingen op de bijgesneden video coderen, de uitvoer niet zoals verwacht.

De [volgende](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) onderwerp wordt beschreven hoe u een codeertaak maakt met MES en het opgeven van een aangepaste voorinstelling voor de codering taak. 

## <a name="creating-a-custom-preset"></a>Maken van een aangepaste voorinstelling
In het voorbeeld weergegeven in het diagram:

1. Oorspronkelijke invoer is 1920 x 1080
2. Deze moeten worden bijgesneden voor uitvoer van 1440 x 1080, die in de invoer frame wordt gecentreerd
3. Dit betekent dat een X-verschuiving van (1920 – 1440) / 2 = 240 en een Y-verschuiving van nul
4. De breedte en hoogte van de rechthoek bijsnijden zijn 1440 1080, respectievelijk en
5. In de fase coderen de vraag is voor het produceren van drie lagen, respectievelijk resoluties 1440 x 1080 960 x 720 en 480 x 360 zijn

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
De functie bijsnijden is bedoeld om handmatig zijn. U moet uw invoervideo laden in een geschikte bewerken hulpprogramma waarmee u frames van belang selecteren, plaatst u de cursor om te bepalen verschuivingen voor de rechthoek bijsnijden om te bepalen van de codering definitie die is afgestemd op die bepaalde video's, enzovoort. Deze functie is niet bedoeld om in te schakelen, bijvoorbeeld: automatische detectie en verwijderen van de zwarte letterbox/pillarbox randen in uw invoervideo.

Volgende beperkingen zijn van toepassing op de bijsnijden functie. Als deze niet wordt voldaan, kan de coderen taak mislukt of er een onverwachte uitvoer geproduceerd.

1. De coördinaten en de grootte van de rechthoek bijsnijden hebben om te passen binnen de invoer video
2. Zoals eerder vermeld, moet de breedte en hoogte in de instellingen voor het coderen overeenkomen met de bijgesneden video
3. Bijsnijden geldt voor video's die zijn vastgelegd in de liggende modus (d.w.z. niet van toepassing op video's die zijn geregistreerd met een smartphone ondergebracht verticaal of horizontaal of verticaal gebruik)
4. Het meest geschikt voor progressieve video vastgelegd met vierkante pixels

## <a name="provide-feedback"></a>Feedback geven
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Volgende stap
Azure Media Services-leertrajecten voor meer informatie over de handige functies die worden aangeboden door AMS zien.  

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]
