---
title: Verwerken en haal de tekst uit afbeeldingen in Azure Search | Microsoft Docs
description: Proces en extract tekst en andere gegevens van afbeeldingen in cognitieve zoeken pijplijnen in Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 566b1e731f137863e9d4bc284d8868d408c7a575
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640197"
---
#  <a name="how-to-process-and-extract-information-from-images-in-cognitive-search-scenarios"></a>Hoe worden verwerkt en gegevens ophalen van afbeeldingen in scenario's cognitieve zoeken

Cognitieve zoekopdracht heeft verschillende mogelijkheden voor het werken met installatiekopieën en afbeeldingsbestanden. Tijdens het document kraken, kunt u de *imageAction* -parameter voor tekst ophalen uit de foto's of afbeeldingen met alfanumerieke tekst, zoals het woord 'STOP' in een teken stoppen. Andere scenario's bevatten voor het genereren van een tekstweergave van een afbeelding, zoals 'dandelion' voor een foto van een dandelion of de kleur 'geel'. U kunt ook metagegevens over de installatiekopie, zoals de grootte extraheren.

Dit artikel worden behandeld in meer detail verwerking van afbeeldingen en biedt richtlijnen voor het werken met afbeeldingen in een pijplijn cognitieve zoeken.

<a name="get-normalized-images"></a>

## <a name="get-normalized-images"></a>Installatiekopieën van genormaliseerde ophalen

Als onderdeel van het document kraken, moet u er een nieuwe reeks configuratieparameters indexeerfunctie voor het verwerken van afbeeldingen of afbeeldingen die zijn ingesloten in bestanden zijn. Deze parameters worden gebruikt voor afbeeldingen voor verdere downstreamverwerking normaliseren. Normaliseren afbeeldingen, maakt u ze meer uniform. Grote afbeeldingen die zijn aangepast aan een maximale hoogte en breedte zodat ze worden gebruikt. Voor afbeeldingen met metagegevens over afdrukstand wordt rotatie afbeelding aangepast voor verticale laden. Aanpassingen aan de metagegevens worden vastgelegd in een complex type gemaakt voor elke installatiekopie. 

U uitschakelen installatiekopie normalisatie niet. Vaardigheden die afbeeldingen doorlopen verwachten genormaliseerde installatiekopieën.

| Configuratieparameter | Beschrijving |
|--------------------|-------------|
| imageAction   | Ingesteld op 'geen' als geen actie moet worden ondernomen wanneer ingesloten afbeeldingen of afbeeldingsbestanden zijn opgetreden. <br/>Ingesteld op 'generateNormalizedImages' voor het genereren van een matrix van genormaliseerde afbeeldingen als onderdeel van het document kraken. Deze installatiekopieën worden weergegeven in de *normalized_images* veld. <br/>De standaardwaarde is 'geen'. Deze configuratie is alleen relevant zijn voor blob-gegevensbronnen, als 'dataToExtract' is ingesteld op 'contentAndMetadata'. |
|  normalizedImageMaxWidth | De maximale breedte (in pixels) voor genormaliseerde afbeeldingen die zijn gegenereerd. De standaardwaarde is 2000.|
|  normalizedImageMaxHeight | De maximumhoogte (in pixels) voor genormaliseerde afbeeldingen die zijn gegenereerd. De standaardwaarde is 2000.|

> [!NOTE]
> Als u de *imageAction* eigenschap in op iets anders dan 'none', u zich niet in te stellen de *parsingMode* eigenschap in op iets anders dan 'standaard'.  U mag een van deze twee eigenschappen alleen instellen op een niet-standaard-waarde in de configuratie van de indexeerfunctie.

Stel de **parsingMode** -parameter voor `json` (om het indexeren van elke blob als één document) of `jsonArray` (als uw blobs JSON-matrices bevatten en moet u elk element van een matrix moet worden behandeld als een afzonderlijk document).

De standaardwaarde van 2000 pixels voor de installatiekopieën van genormaliseerde maximale breedte en hoogte is gebaseerd op de maximale grootte wordt ondersteund door de [OCR kwalificatie](cognitive-search-skill-ocr.md) en de [analysis kwalificatie installatiekopie](cognitive-search-skill-image-analysis.md). Als u de maximale limieten verhogen, kan verwerking op de grotere afbeeldingen mislukken.


U geeft de imageAction in uw [indexeerfunctie definitie](https://docs.microsoft.com/rest/api/searchservice/create-indexer) als volgt:

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

| Lid van de afbeelding       | Beschrijving                             |
|--------------------|-----------------------------------------|
| gegevens               | Base64-gecodeerde tekenreeks van de installatiekopie van het genormaliseerde in JPEG-indeling.   |
| Breedte              | Breedte van de installatiekopie van het genormaliseerde in pixels. |
| Hoogte             | Hoogte van de installatiekopie van het genormaliseerde in pixels. |
| originalWidth      | De oorspronkelijke breedte van de afbeelding voordat normalisatie. |
| originalHeight      | De oorspronkelijke hoogte van de afbeelding voordat normalisatie. |
| rotationFromOriginal |  Rechtsom draaien in graden die is opgetreden om de genormaliseerde afbeelding te maken. Een waarde tussen 0 graden en 360 graden. Deze stap leest de metagegevens van de afbeelding die wordt gegenereerd door een camera of scanner. Meestal is dit een veelvoud van 90 graden zijn. |
| contentOffset |De offset teken in het veld inhoud waar de installatiekopie is opgehaald uit. Dit veld is alleen van toepassing voor bestanden met ingesloten afbeeldingen. |

 Waarde van een steekproef nemen uit *normalized_images*:
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

Er zijn twee ingebouwde cognitieve vaardigheden die afbeeldingen als invoer nemen: [OCR](cognitive-search-skill-ocr.md) en [installatiekopie Analysis](cognitive-search-skill-image-analysis.md). 

Deze kwalificaties werken op dit moment alleen met afbeeldingen die zijn gegenereerd op basis van het document kraken stap. Hierdoor de enige ondersteunde invoer is `"/document/normalized_images"`.

### <a name="image-analysis-skill"></a>Afbeelding Analysis kwalificatie

De [installatiekopie Analysis kwalificatie](cognitive-search-skill-image-analysis.md) extraheert een groot aantal visual functies op basis van de inhoud van de afbeelding. U kunt bijvoorbeeld een bijschrift genereren van een installatiekopie, tags te genereren of beroemdheden en monumenten identificeren.

### <a name="ocr-skill"></a>OCR kwalificatie

De [OCR kwalificatie](cognitive-search-skill-ocr.md) extraheert tekst van afbeeldingsbestanden zoals jpg-bestanden, PNG en bitmaps. Tekst kan worden opgehaald en de lay-outinformatie. De lay-outinformatie biedt omsluitende vakken voor elk van de tekenreeksen die zijn geïdentificeerd.

De kwalificatie OCR kunt u de algoritme gebruiken voor het detecteren van tekst in uw afbeeldingen te selecteren. Momenteel ondersteunt deze twee algoritmen, één voor afgedrukte tekst en een andere voor de hand geschreven.

## <a name="embedded-image-scenario"></a>Ingesloten afbeelding scenario

Een veelvoorkomend scenario omvat het maken van één tekenreeks alle bestandsinhoud, tekst- en image-origin-tekst die door de volgende stappen uit te voeren:  

1. [Normalized_images uitpakken](#get-normalized-images)
1. Uitgevoerd OCR kwalificatie met `"/document/normalized_images"` als invoer
1. De tekstweergave van deze installatiekopieën worden samengevoegd met de onbewerkte tekst uit het bestand hebt uitgepakt. U kunt de [tekst samenvoegen](cognitive-search-skill-textmerger.md) kwalificatie beide tekstsegmenten consolideren in één grote tekenreeks.

Het volgende voorbeeld vaardigheden maakt een *merged_text* veld met de tekstinhoud van het document. Dit omvat ook de tekst OCRed van elk van de ingesloten afbeeldingen. 

#### <a name="request-body-syntax"></a>Syntaxis van de aanvraag hoofdtekst
```json
{
  "description": "Extract text from images and merge with content text to produce merged_text",
  "skills":
  [
    {
        "name": "OCR skill",
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
          "name": "mergedText", "targetname" : "merged_text"
        }
      ]
    }
  ]
}
```

Nu dat u een veld merged_text hebt, kan u deze in de definitie van de indexeerfunctie toewijzen als een doorzoekbaar veld. Alle inhoud van uw bestanden, inclusief de tekst van de afbeeldingen worden doorzocht.

## <a name="visualize-bounding-boxes-of-extracted-text"></a>Visualiseren begrenzingsvak vakken van uitgepakte tekst

Een ander gebruikelijk scenario is informatie over lay-out zoekresultaten te visualiseren. U wilt markeren waarin tekst als onderdeel van uw zoekresultaten in een installatiekopie is gevonden.

Omdat de stap OCR wordt uitgevoerd op de genormaliseerde afbeeldingen, zijn de coördinaten van de lay-out in de ruimte genormaliseerde installatiekopie. Bij het weergeven van de installatiekopie van het genormaliseerde de aanwezigheid van coördinaten is doorgaans geen probleem, maar in sommige situaties wilt u mogelijk de originele installatiekopie weergeven. In dit geval wordt elk coördinaat punten in de indeling converteren naar de oorspronkelijke installatiekopie van het coördinatensysteem. 

Als een helper als u nodig hebt voor het transformeren van genormaliseerde coördinaten naar de oorspronkelijke coördinaat ruimte kunt u het volgende algoritme:

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
+ [Maak indexeerfunctie (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
+ [Afbeelding kwalificatie analyseren](cognitive-search-skill-image-analysis.md)
+ [OCR kwalificatie](cognitive-search-skill-ocr.md)
+ [Tekst samenvoegen kwalificatie](cognitive-search-skill-textmerger.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Hoe verrijkt velden toe te wijzen](cognitive-search-output-field-mapping.md)
