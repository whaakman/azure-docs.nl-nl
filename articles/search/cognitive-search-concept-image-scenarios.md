---
title: Verwerken en haal de tekst uit afbeeldingen in Azure Search | Microsoft Docs
description: Verwerken en extraheren tekst en andere informatie uit afbeeldingen in cognitive zoeken in pijplijnen in Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 5ee821f3d1e81e581f376d4b14b3e8393a8f190c
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52444629"
---
#  <a name="how-to-process-and-extract-information-from-images-in-cognitive-search-scenarios"></a>Hoe worden verwerkt en extraheer informatie uit afbeeldingen in scenario's met cognitief zoeken

Cognitieve zoekopdrachten heeft verschillende mogelijkheden voor het werken met afbeeldingen en afbeeldingsbestanden. Tijdens de documenten kraken, kunt u de *imageAction* parameter om tekst te extraheren van foto's of afbeeldingen met alfanumerieke tekst, zoals het woord "STOP" in een teken stoppen. Andere scenario's omvatten genereren van een tekstweergave van een afbeelding, zoals 'dandelion' voor een foto van een dandelion of de kleur "yellow". U kunt ook metagegevens over de installatiekopie, zoals de grootte ervan te extraheren.

In dit artikel bevat informatie over afbeeldingsverwerking in meer detail en biedt richtlijnen voor het werken met afbeeldingen in een pijplijn cognitief zoeken.

<a name="get-normalized-images"></a>

## <a name="get-normalized-images"></a>Genormaliseerde afbeeldingen ophalen

Als onderdeel van documenten kraken, moet u er een nieuwe set parameters voor de indexeerfunctie configuratie voor de verwerking van afbeeldingen of afbeeldingen die zijn ingesloten in bestanden zijn. Deze parameters worden gebruikt voor het normaliseren van installatiekopieën voor verdere downstream-verwerkingen. Installatiekopieën normaliseren zorgt ervoor dat ze meer uniform. Grote afbeeldingen worden uitgebreid tot een maximale hoogte en breedte zodat deze bedrijfsgegevens. Voor installatiekopieën van metagegevens op richting bieden wordt rotatie afbeelding aangepast voor verticale laden. Aanpassingen van de metagegevens worden vastgelegd in een complex type gemaakt voor elke afbeelding. 

U uitschakelen normalisering van de installatiekopie niet. Genormaliseerde installatiekopieën verwacht dat de vaardigheden die installatiekopieën herhalen.

| Configuratieparameter | Beschrijving |
|--------------------|-------------|
| imageAction   | Ingesteld op 'none' als er is geen actie moet worden uitgevoerd wanneer de ingesloten afbeeldingen of afbeeldingen zijn opgetreden. <br/>Ingesteld op 'generateNormalizedImages' voor het genereren van een matrix van genormaliseerde installatiekopieën als onderdeel van het document kraken. Deze installatiekopieën worden weergegeven in de *normalized_images* veld. <br/>De standaardwaarde is 'none'. Deze configuratie is alleen relevant zijn voor blob-gegevensbronnen, als 'dataToExtract' is ingesteld op "contentAndMetadata." |
|  normalizedImageMaxWidth | De maximale breedte (in pixels) voor genormaliseerde installatiekopieën die worden gegenereerd. De standaardwaarde is 2000.|
|  normalizedImageMaxHeight | De maximale hoogte (in pixels) voor genormaliseerde installatiekopieën die worden gegenereerd. De standaardwaarde is 2000.|

> [!NOTE]
> Als u de *imageAction* eigenschap iets anders dan 'none', u niet mogelijk om in te stellen de *parsingMode* eigenschap in op iets anders dan 'standaard'.  U mag een van deze twee eigenschappen alleen ingesteld op een niet-standaard-waarde in de configuratie van de indexeerfunctie.

Stel de **parsingMode** parameter `json` (indexeren om elke blob als één document) of `jsonArray` (als uw blobs JSON-matrices bevatten en moet u elk element van een matrix moet worden behandeld als een afzonderlijke document).

De standaardwaarde van 2000 pixels voor de installatiekopieën van genormaliseerde maximale breedte en hoogte is gebaseerd op de maximale grootte wordt ondersteund door de [OCR-kwalificatie](cognitive-search-skill-ocr.md) en de [analysis vaardigheid image](cognitive-search-skill-image-analysis.md). Als u de maximale limieten verhoogt, kan op de grotere afbeeldingen verwerken mislukken.


U geeft de imageAction in uw [definitie van indexeerfunctie](https://docs.microsoft.com/rest/api/searchservice/create-indexer) als volgt:

```json
{
  //...rest of your indexer definition goes here ...
  "parameters":
  {
    "configuration": 
    {
        "dataToExtract": "contentAndMetadata",
        "imageAction": "generateNormalizedImages"
    }
  }
}
```

Wanneer de *imageAction* is ingesteld op 'generateNormalizedImages', de nieuwe *normalized_images* veld bevat een matrix van afbeeldingen. Elke installatiekopie is een complex type met de volgende leden:

| Lid van de installatiekopie       | Beschrijving                             |
|--------------------|-----------------------------------------|
| gegevens               | Met base64 gecodeerde tekenreeks van de genormaliseerde afbeelding in JPEG-indeling.   |
| Breedte              | Breedte van de installatiekopie van het genormaliseerde in pixels. |
| Hoogte             | De hoogte van de installatiekopie van het genormaliseerde in pixels. |
| originalWidth      | De oorspronkelijke breedte van de afbeelding voordat normalisering. |
| originalHeight      | De oorspronkelijke hoogte van de afbeelding voordat normalisering. |
| rotationFromOriginal |  Linksom draaien in graden die heeft plaatsgevonden voor het maken van de genormaliseerde installatiekopie. Een waarde tussen 0 graden en 360 graden. Deze stap leest de metagegevens van de installatiekopie die wordt gegenereerd door een camera of de scanner. Meestal is dit een veelvoud van 90 graden. |
| contentOffset |De offset teken in het veld inhoud waar de installatiekopie is geëxtraheerd uit. Dit veld is alleen van toepassing voor bestanden met ingesloten afbeeldingen. |

 Voorbeeld van een waarde van *normalized_images*:
```json
[
  {
    "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
    "width": 500,
    "height": 300,
    "originalWidth": 5000,  
    "originalHeight": 3000,
    "rotationFromOriginal": 90,
    "contentOffset": 500  
  }
]
```

## <a name="image-related-skills"></a>Installatiekopieën gerelateerde vaardigheden

Er zijn twee ingebouwde cognitieve vaardigheden die installatiekopieën als invoer: [OCR](cognitive-search-skill-ocr.md) en [analyse van de afbeelding](cognitive-search-skill-image-analysis.md). 

Deze vaardigheden werken op dit moment alleen met de installatiekopieën die zijn gegenereerd op basis van de stap voor het kraken van document. Hierdoor de enige ondersteunde invoer is `"/document/normalized_images"`.

### <a name="image-analysis-skill"></a>Afbeelding Analysis-kwalificatie

De [analyse van de afbeelding vaardigheid](cognitive-search-skill-image-analysis.md) extraheert van een grote verscheidenheid aan visuele kenmerken op basis van de inhoud van de installatiekopie. U kunt bijvoorbeeld een bijschrift genereren van een afbeelding, codes genereren of beroemdheden en oriëntatiepunten herkennen.

### <a name="ocr-skill"></a>OCR-kwalificatie

De [OCR-kwalificatie](cognitive-search-skill-ocr.md) haalt tekst op uit bestanden zoals jpg-bestanden, PNG en bitmaps. Deze tekst kunt ophalen en lay outinformatie. De lay outinformatie biedt selectiekader vakken voor elk van de tekenreeksen die zijn geïdentificeerd.

De kwalificatie OCR kunt u de algoritme te gebruiken voor het detecteren van tekst in uw installatiekopie te selecteren. Op dit moment ondersteunt deze twee algoritmen, een voor gedrukte tekst en andere voor handgeschreven tekst.

## <a name="embedded-image-scenario"></a>Ingesloten afbeelding scenario

Een veelvoorkomend scenario is het maken van één tekenreeks alle bestandsinhoud, tekst- en image-origin-tekst, die door de volgende stappen uit:  

1. [Normalized_images ophalen](#get-normalized-images)
1. Uitvoeren de OCR-vaardigheden met `"/document/normalized_images"` als invoer
1. De tekstweergave van deze installatiekopieën worden samengevoegd met de onbewerkte tekst uit het bestand hebt uitgepakt. U kunt de [tekst samenvoegen](cognitive-search-skill-textmerger.md) vaardigheid beide tekstsegmenten consolideren in één grote tekenreeks.

De vaardigheden van het volgende voorbeeld maakt een *merged_text* veld met de tekstinhoud van het document. Dit omvat ook de tekst OCRed van elk van de ingesloten afbeeldingen. 

#### <a name="request-body-syntax"></a>Syntaxis van de aanvraag hoofdtekst
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "description": "Extract text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": "en",
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text"
          }
        ]
    },
    {
      "@odata.type": "#Microsoft.Skills.Text.MergeSkill",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " ",
      "inputs": [
        {
          "name":"text", "source": "/document/content"
        },
        {
          "name": "itemsToInsert", "source": "/document/normalized_images/*/text"
        },
        {
          "name":"offsets", "source": "/document/normalized_images/*/contentOffset" 
        }
      ],
      "outputs": [
        {
          "name": "mergedText", "targetName" : "merged_text"
        }
      ]
    }
  ]
}
```

Nu dat u een veld merged_text hebt, kan u deze toewijzen als een doorzoekbaar veld in de definitie van de indexeerfunctie. Alle van de inhoud van uw bestanden, met inbegrip van de tekst van de installatiekopieën worden doorzocht.

## <a name="visualize-bounding-boxes-of-extracted-text"></a>Visualiseer omsluitende kaders van geëxtraheerde tekst

Een ander gebruikelijk scenario is informatie over de opmaak van de search resultaten te visualiseren. Bijvoorbeeld, als u wilt markeren waarin een stuk tekst is gevonden in een afbeelding als onderdeel van uw zoekresultaten.

Omdat de OCR-stap wordt uitgevoerd op de genormaliseerde afbeeldingen, zijn de lay-out-coördinaten in de ruimte genormaliseerde installatiekopie. Bij het weergeven van de installatiekopie van het genormaliseerde, de aanwezigheid van coördinaten is meestal geen probleem, maar in sommige gevallen wilt u mogelijk de oorspronkelijke afbeelding weergeven. In dit geval wordt elk van de coördinaat punten in de lay-out converteren naar de oorspronkelijke installatiekopie van het coördinatensysteem. 

Als u wilt transformeren genormaliseerde coördinaten naar de oorspronkelijke coördinatenruimte, kunt u als een hulpprogramma, de volgende algoritme gebruiken:

```csharp
        /// <summary>
        ///  Converts a point in the normalized coordinate space to the original coordinate space.
        ///  This method assumes the rotation angles are multiples of 90 degrees.
        /// </summary>
        public static Point GetOriginalCoordinates(Point normalized,
                                    int originalWidth,
                                    int originalHeight,
                                    int width,
                                    int height,
                                    double rotationFromOriginal)
        {
            Point original = new Point();
            double angle = rotationFromOriginal % 360;

            if (angle == 0 )
            {
                original.X = normalized.X;
                original.Y = normalized.Y;
            } else if (angle == 90)
            {
                original.X = normalized.Y;
                original.Y = (width - normalized.X);
            } else if (angle == 180)
            {
                original.X = (width -  normalized.X);
                original.Y = (height - normalized.Y);
            } else if (angle == 270)
            {
                original.X = height - normalized.Y;
                original.Y = normalized.X;
            }

            double scalingFactor = (angle % 180 == 0) ? originalHeight / height : originalHeight / width;
            original.X = (int) (original.X * scalingFactor);
            original.Y = (int)(original.Y * scalingFactor);

            return original;
        }
```

## <a name="see-also"></a>Zie ook
+ [Indexeerfunctie (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
+ [Afbeelding vaardigheid analyseren](cognitive-search-skill-image-analysis.md)
+ [OCR-kwalificatie](cognitive-search-skill-ocr.md)
+ [Tekst samenvoegen vaardigheid](cognitive-search-skill-textmerger.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Verrijkt velden toewijzen](cognitive-search-output-field-mapping.md)
