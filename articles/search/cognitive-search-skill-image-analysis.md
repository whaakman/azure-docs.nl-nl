---
title: Afbeelding van de analyse cognitieve zoeken kwalificatie (Azure Search) | Microsoft Docs
description: Haal de semantische tekst door de installatiekopie-analyse met behulp van de ImageAnalysis cognitieve kwalificatie in een Azure Search verrijking pijplijn.
services: search
manager: pablocas
author: luiscabrer
documentationcenter: ''
ms.assetid: ''
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: b426fd253b436c71235f006cc41881f0c0c67703
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
#   <a name="image-analysis-cognitive-skill"></a>Afbeelding Analysis cognitieve kwalificatie

De **installatiekopie Analysis** kwalificatie extraheert een groot aantal visual functies op basis van de inhoud van de afbeelding. U kunt bijvoorbeeld een bijschrift genereren van een installatiekopie, codes genereren of beroemdheden en monumenten identificeren.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Beschrijving |
|--------------------|-------------|
| defaultLanguageCode   |  Een tekenreeks dat aangeeft welke taal om terug te keren. De service retourneert resultaten in een bepaalde taal. Als deze parameter niet is opgegeven, is de standaardwaarde 'en'. <br/><br/>Ondersteunde talen zijn: <br/>*nl* -Engels (standaard) <br/> *zh* -vereenvoudigd Chinees|
|visualFeatures |   Een matrix met tekenreeksen die aangeven welke typen visuele functie om terug te keren. Visuele functie geldige typen zijn:  <ul><li> *categorieën* -afbeeldingsinhoud op basis van een taxonomie gedefinieerd in de cognitieve Services categoriseert [documentatie](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).</li><li> *labels* -tags van de installatiekopie met een gedetailleerde lijst met woorden die betrekking hebben op de inhoud van de afbeelding.</li><li>*Beschrijving* -installatiekopie van de inhoud met een volledige Engelse zin.</li><li>*Vlakken* -detecteert als vlakken aanwezig zijn. Indien aanwezig, genereert coördinaten, geslacht en leeftijd.</li><li> *ImageType* -detecteert of de installatiekopie illustraties of de tekening van een regel is.</li><li>   *Kleur* -bepaalt de accentkleur, verwerkingsflexibiliteit kleur en of een installatiekopie van een zwarte is wit.</li><li>*Volwassenen* -detecteert of de afbeelding pornografische aard is (beschrijft bloot of een handeling geslacht). Seksuele suggestieve inhoud wordt ook gedetecteerd.</li></ul> Namen van visuele functies zijn hoofdlettergevoelig.|
| details   | Een matrix met tekenreeksen die aangeven welke domeinspecifieke gegevens te retourneren. Visuele functie geldige typen zijn: <ul><li>*Beroemdheden* -beroemdheden identificeert als gedetecteerd in de installatiekopie.</li><li>*Monumenten* -monumenten identificeert als gedetecteerd in de installatiekopie.</li></ul>
 |

## <a name="skill-inputs"></a>Kwalificatie invoer

| Voer een naam      | Beschrijving                                          |
|---------------|------------------------------------------------------|
| Afbeelding         | Complex Type. Momenteel wordt alleen werkt met '/ document/normalized_images' veld, die wordt geproduceerd door de Azure Blob-indexeerfunctie wanneer ```imageAction``` is ingesteld op ```generateNormalizedImages```. Zie de [voorbeeld](#sample-output) voor meer informatie.|



##  <a name="sample-definition"></a>Voorbeeld-definitie

```json
{
    "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
    "visualFeatures": [
        "Tags",
        "Faces",
        "Categories",
        "Adult",
        "Description",
        "ImageType",
        "Color"
    ],
    "defaultLanguageCode": "en",
    "inputs": [
        {
            "name": "image",
            "source": "/document/normalized_images/*"
        }
    ],
    "outputs": [
        {
            "name": "categories",
            "targetName": "myCategories"
        },
        {
            "name": "tags",
            "targetName": "myTags"
        },
        {
            "name": "description",
            "targetName": "myDescription"
        },
        {
            "name": "faces",
            "targetName": "myFaces"
        },
        {
            "name": "imageType",
            "targetName": "myImageType"
        },
        {
            "name": "color",
            "targetName": "myColor"
        },
        {
            "name": "adult",
            "targetName": "myAdultCategory"
        }
    ]
}
```

##  <a name="sample-input"></a>Voorbeeldinvoer

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "url": "https://storagesample.blob.core.windows.net/sample-container/image.jpg"
            }
        }
    ]
}
```


##  <a name="sample-output"></a>Voorbeelduitvoer

```json
{
    "values": [
      {
        "recordId": "1",
            "data": {
                "categories": [
           {
                        "name": "abstract_",
                        "score": 0.00390625
                    },
                    {
                "name": "people_",
                        "score": 0.83984375,
                "detail": {
                            "celebrities": [
                                {
                                    "name": "Satya Nadella",
                                    "faceRectangle": {
                                        "left": 597,
                                        "top": 162,
                                        "width": 248,
                                        "height": 248
                                    },
                                    "confidence": 0.999028444
                                }
                            ],
                            "landmarks": [
                                {
                                    "name": "Forbidden City",
                                    "confidence": 0.9978346
                                }
                            ]
                        }
                    }
                ],
                "adult": {
                    "isAdultContent": false,
                    "isRacyContent": false,
                    "adultScore": 0.0934349000453949,
                    "racyScore": 0.068613491952419281
                },
                "tags": [
                    {
                        "name": "person",
                        "confidence": 0.98979085683822632
                    },
                    {
                        "name": "man",
                        "confidence": 0.94493889808654785
                    },
                    {
                        "name": "outdoor",
                        "confidence": 0.938492476940155
                    },
                    {
                        "name": "window",
                        "confidence": 0.89513939619064331
                    }
                ],
                "description": {
                    "tags": [
                        "person",
                        "man",
                        "outdoor",
                        "window",
                        "glasses"
                    ],
                    "captions": [
                        {
                            "text": "Satya Nadella sitting on a bench",
                            "confidence": 0.48293603002174407
                        }
                    ]
                },
                "requestId": "0dbec5ad-a3d3-4f7e-96b4-dfd57efe967d",
                "metadata": {
                    "width": 1500,
                    "height": 1000,
                    "format": "Jpeg"
                },
                "faces": [
                    {
                        "age": 44,
                        "gender": "Male",
                    "faceRectangle": {
                            "left": 593,
                            "top": 160,
                            "width": 250,
                            "height": 250
                        }
                    }
                ],
                "color": {
                    "dominantColorForeground": "Brown",
                    "dominantColorBackground": "Brown",
                    "dominantColors": [
                        "Brown",
                        "Black"
                    ],
                    "accentColor": "873B59",
                    "isBWImg": false
                    },
                "imageType": {
                    "clipArtType": 0,
                    "lineDrawingType": 0
                }
           }
      }
    ]
}
```


## <a name="error-cases"></a>Foutgevallen
In de volgende foutgevallen zijn geen elementen worden geëxtraheerd.

| Foutcode | Beschrijving |
|------------|-------------|
| NotSupportedLanguage | De opgegeven taal wordt niet ondersteund. |
| InvalidImageUrl | Afbeeldings-URL is onjuist ingedeeld of niet toegankelijk.|
| InvalidImageFormat | Invoergegevens is geen geldige installatiekopie. |
| InvalidImageSize | De installatiekopie van de invoer is te groot. |
| NotSupportedVisualFeature  | Functietype van de opgegeven is niet geldig. |
| NotSupportedImage | Niet-ondersteunde afbeelding, bijvoorbeeld kinderpornografie. |
| InvalidDetails | Niet-ondersteunde domeinspecifieke model. |

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
