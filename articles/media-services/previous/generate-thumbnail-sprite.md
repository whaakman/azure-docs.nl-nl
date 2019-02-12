---
title: Genereer een miniatuur sprite met Azure Media Services | Microsoft Docs
description: In dit onderwerp laat zien hoe voor het genereren van een miniatuur sprite met Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/08/2019
ms.author: juliako
ms.openlocfilehash: b4d760d8520f43b223665f17c85d3932761ebe17
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56003105"
---
# <a name="generate-a-thumbnail-sprite"></a>Genereer een miniatuur sprite  

U kunt Media Encoder Standard gebruiken voor het genereren van een miniatuur sprite, dit is een JPEG-bestand met meerdere kleine resolutie miniaturen stitched samen in één (groot) installatiekopie, samen met een VTT-bestand. Dit bestand VTT Hiermee geeft u het tijdsbereik in de invoervideo die staat voor een miniatuur, samen met de grootte en de coördinaten van deze miniatuur binnen de grote JPEG-bestanden. De installatiekopie van het VTT-bestand en sprite videospelers gebruiken voor het weergeven van een 'visual' zoekbalk een viewer voorzien van visuele feedback wanneer het weer voor het reinigen en doorsturen in de tijdlijn van de video.

Als u wilt gebruiken voor het genereren van miniaturen Sprite, de vooraf ingestelde Media Encoder Standard:

1. JPG miniatuurafbeelding indeling moet gebruiken
2. Start/stap/bereik waarden als een van beide tijdstempels of % waarden (en niet frame aantallen) moet opgeven 
    
    1. Het is goed dat tijdstempels en % waarden combineren

3. De waarde SpriteColumn moet hebben als een niet-negatief getal groter dan of gelijk aan 1

    1. Als SpriteColumn is ingesteld op M > = 1, de installatiekopie van de uitvoer is een rechthoek met M-kolommen. Als het getal van miniatuurweergaven die is gegenereerd via #2 een veelvoud van M is, worden de laatste rij onvolledig en links met zwarte pixels.  

Hier volgt een voorbeeld:

```json
{
    "Version": 1.0,
    "Codecs": [
    {
      "Start": "00:00:01",
      "Type": "JpgImage",
      "Step": "5%",
      "Range": "100%",
      "JpgLayers": [
        {
          "Type": "JpgLayer",
          "Width": "10%",
          "Height": "10%",
          "Quality": 90
        }
      ],
      "SpriteColumn": 10
    }
      ],
      "Outputs": [
        {
          "FileName": "{Basename}_{Index}{Extension}",
          "Format": {
            "Type": "JpgFormat"
          }
        }
   ]
}
```

## <a name="known-issues"></a>Bekende problemen

1.  Het is niet mogelijk voor het genereren van een installatiekopie sprite met één rij van installatiekopieën (SpriteColumn = 1 resulteert in een afbeelding met één kolom).
2.  Logische groepen te verdelen van de installatiekopieën sprite in matig grootte JPEG-afbeeldingen is nog niet ondersteund. Dus wees voorzichtig om te beperken het nummer van miniaturen en de grootte, zodat de resulterende stitched miniatuur Sprite rond 8 miljoen pixels of minder.
3.  Azure Media Player ondersteunt sprites in Microsoft Edge, Chrome en Firefox browsers. Parseren van VTT wordt niet ondersteund in IE11.

## <a name="next-steps"></a>Volgende stappen

[Inhoud coderen](media-services-encode-asset.md)
