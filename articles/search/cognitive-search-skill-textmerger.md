---
title: Tekst samenvoegen cognitieve zoeken kwalificatie (Azure Search) | Microsoft Docs
description: Tekst van een verzameling van velden samenvoegen in één geconsolideerde veld. Gebruik deze cognitieve kwalificatie in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: ba779ebcbc791f9caa60948feeb38b88a23ef379
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640659"
---
#    <a name="text-merge-cognitive-skill"></a>Tekst samenvoegen cognitieve kwalificatie

De **tekst samenvoegen** kwalificatie consolideert tekst uit een verzameling van velden in één veld. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.TextMerger

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Beschrijving |
|--------------------|-------------|
| insertPreTag  | Tekenreeks die moet worden opgenomen voor elke invoegen. De standaardwaarde is `" "`. Als u wilt weglaten de ruimte, kunt u de waarde instelt op `""`.  |
| insertPostTag | Tekenreeks die moet worden opgenomen na elke invoegen. De standaardwaarde is `" "`. Als u wilt weglaten de ruimte, kunt u de waarde instelt op `""`.  |


##  <a name="sample-input"></a>Voorbeeldinvoer
Een JSON-document bieden van bruikbare invoer voor deze kwalificatie mogelijk:

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "The brown fox jumps over the dog" ,
             "itemsToInsert": ["quick", "lazy"],
             "offsets": [3, 28],
           }
      }
    ]
}
```

##  <a name="sample-output"></a>Voorbeelduitvoer
In dit voorbeeld ziet u de uitvoer van de vorige input, ervan uitgaande dat de *insertPreTag* is ingesteld op `" "`, en *insertPostTag* is ingesteld op `""`. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "mergedText": "The quick brown fox jumps over the lazy dog" 
           }
      }
    ]
}
```

## <a name="extended-sample-skillset-definition"></a>Uitgebreide voorbeeld vaardigheden definitie

Een gebruikelijk scenario voor het gebruik van tekst samenvoegen is het samenvoegen van de tekstweergave van installatiekopieën (tekst uit een kwalificatie OCR of het bijschrift van een afbeelding) in het veld inhoud van een document. 

Het volgende voorbeeld vaardigheden gebruikt de kwalificatie OCR voor tekst ophalen uit de afbeeldingen die zijn ingesloten in het document. Vervolgens wordt het maken van een *merged_text* veld bevat zowel oorspronkelijke als OCRed tekst van elke installatiekopie. 

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
      "@odata.type": "#Microsoft.Skills.Util.TextMerger",
      "description": "Create merged_text, which includes all the textual representation of each image inserted at the right location in the content field.",
      "context": "/document",
      "insertPreTag": " ",
      "insertPostTag": " "
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
Het bovenstaande voorbeeld wordt ervan uitgegaan dat een veld genormaliseerd installatiekopieën bestaat. Genormaliseerd installatiekopieën veld krijgen, stelt u de *imageAction* configuratie in de definitie van de indexeerfunctie *generateNormalizedImages* zoals hieronder wordt weergegeven:

```json
{  
   //...rest of your indexer definition goes here ... 
  "parameters":{  
      "configuration":{  
         "dataToExtract":"contentAndMetadata",
         "imageAction":"generateNormalizedImages"
      }
   }
}
```

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Maak indexeerfunctie (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)