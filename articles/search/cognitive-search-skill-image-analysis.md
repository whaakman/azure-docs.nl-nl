---
title: Afbeelding Analysis cognitief zoeken vaardigheid - Azure Search
description: Haal de semantische tekst via analyse van de afbeelding met behulp van de ImageAnalysis cognitieve vaardigheden in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: fc8780c5b99ce98a55a6cb08cfaa6585e5a4e89a
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53313306"
---
#   <a name="image-analysis-cognitive-skill"></a>Afbeelding Analysis cognitieve vaardigheden

De **analyse van de afbeelding** vaardigheid extraheert een grote verscheidenheid aan visuele kenmerken op basis van de inhoud van de installatiekopie. U kunt bijvoorbeeld een bijschrift genereren van een installatiekopie, codes genereren of beroemdheden en oriëntatiepunten herkennen.

> [!NOTE]
> Vanaf December 21 mei 2018, kunt u zich een Cognitive Services-resource koppelen aan een Azure Search-vaardigheden. Hierdoor kunnen we beginnen kosten te bereken voor uitvoering van vaardigheden. Op deze datum ook in rekening voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken. Tekst extractie van documenten blijven worden aangeboden zonder extra kosten.
>
> De uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/) . Afbeelding extractie prijsstelling wordt in rekening gebracht op de preview-prijzen en wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400). Informatie over [meer](cognitive-search-attach-cognitive-services.md).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Vision.ImageAnalysisSkill 

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Description |
|--------------------|-------------|
| defaultLanguageCode   |  Een tekenreeks die aangeeft welke taal moet worden geretourneerd. De service retourneert resultaten in een opgegeven taal. Als deze parameter niet is opgegeven, is de standaardwaarde 'en'. <br/><br/>Ondersteunde talen zijn: <br/>*nl* -Engels (standaard) <br/> *zh* -vereenvoudigd Chinees|
|visualFeatures |   Een matrix met tekenreeksen die aangeeft welke visuele functie om terug te keren. Geldige visualfunctie typen zijn onder andere:  <ul><li> *categorieën* -categoriseert afbeeldingsinhoud op basis van een gedefinieerd in de Cognitive Services taxonomie [documentatie](https://docs.microsoft.com/azure/cognitive-services/computer-vision/category-taxonomy).</li><li> *tags* -tags van de installatiekopie met een gedetailleerde lijst met betrekking tot de inhoud van de woorden.</li><li>*Beschrijving* -beschrijving van de afbeelding die inhoud met een volledige Engelse zin.</li><li>*Gezichten* -detecteert of gezichten aanwezig zijn. Als deze aanwezig is, genereert coördinaten, geslacht en leeftijd.</li><li> *ImageType* -detecteert of de afbeelding clipart of een lijntekening is.</li><li>   *Kleur* -bepaalt de accentkleur, de overheersende kleur, en of een afbeelding is zwart- wit.</li><li>*Volwassenen* -detecteert of de installatiekopie is in de natuur (ziet u bloot of een geregistreerde seksuele act) is. Seksueel suggestieve inhoud wordt ook gedetecteerd.</li></ul> Namen van visuele kenmerken zijn hoofdlettergevoelig.|
| details   | Een matrix met tekenreeksen die aangeeft welke domeinspecifieke gegevens moeten worden geretourneerd. Geldige visualfunctie typen zijn onder andere: <ul><li>*Beroemdheden* -beroemdheden wordt geïdentificeerd als gedetecteerd in de afbeelding.</li><li>*Oriëntatiepunten* -oriëntatiepunten wordt geïdentificeerd als gedetecteerd in de afbeelding.</li></ul>
 |

## <a name="skill-inputs"></a>Kwalificatie invoer

| Voer een naam in      | Description                                          |
|---------------|------------------------------------------------------|
| image         | Complexe Type. Momenteel wordt alleen werkt met "/ document/normalized_images"-veld, die worden geproduceerd door de indexeerfunctie Azure Blob als ```imageAction``` is ingesteld op ```generateNormalizedImages```. Zie de [voorbeeld](#sample-output) voor meer informatie.|



##  <a name="sample-definition"></a>Van voorbeelddefinitie
```json
{
    "@odata.type": "#Microsoft.Skills.Vision.ImageAnalysisSkill",
    "context": "/document/normalized_images/*",
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

##  <a name="sample-input"></a>Van Voorbeeldinvoer

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {                
                "image":  {
                               "data": "BASE64 ENCODED STRING OF A JPEG IMAGE",
                               "width": 500,
                               "height": 300,
                               "originalWidth": 5000,  
                               "originalHeight": 3000,
                               "rotationFromOriginal": 90,
                               "contentOffset": 500  
                           }
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
                                    "faceBoundingBox": {
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
                    "faceBoundingBox": {
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
                    "isBwImg": false
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
In de volgende foutgevallen worden geen elementen geëxtraheerd.

| Foutcode | Description |
|------------|-------------|
| NotSupportedLanguage | De opgegeven taal wordt niet ondersteund. |
| InvalidImageUrl | Afbeeldings-URL is onjuist ingedeeld of niet toegankelijk.|
| InvalidImageFormat | Invoergegevens is geen geldige afbeelding. |
| InvalidImageSize | Afbeelding is te groot. |
| NotSupportedVisualFeature  | Functietype van de opgegeven is niet geldig. |
| NotSupportedImage | Niet-ondersteunde afbeelding, bijvoorbeeld, kinderporno. |
| InvalidDetails | Niet-ondersteunde domeinspecifieke model. |

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Indexeerfunctie (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
