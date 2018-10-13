---
title: Tekst samenvoegen cognitief zoeken vaardigheid (Azure Search) | Microsoft Docs
description: Tekst uit een verzameling van velden samenvoegen in één geconsolideerde veld. Gebruik deze cognitieve vaardigheden in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 5387eeacc78875ac0f38f96a6c83fb3f5791775e
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167611"
---
#    <a name="text-merge-cognitive-skill"></a>Tekst samenvoegen cognitieve vaardigheden

De **tekst samenvoegen** vaardigheid consolideert tekst uit een verzameling van velden in één veld. 

> [!NOTE]
> Cognitief zoeken is een openbare preview. Uitvoering van vaardigheden en uitpakken van de installatiekopie en normalisering worden momenteel gratis aangeboden. Op een later tijdstip, worden de prijzen van deze mogelijkheden aangekondigd. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.MergeSkill

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Beschrijving |
|--------------------|-------------|
| insertPreTag  | Tekenreeks die moet worden opgenomen voor elke invoegen. De standaardwaarde is `" "`. Als u wilt de ruimte weglaat, stelt u de waarde in op `""`.  |
| insertPostTag | Tekenreeks die moet worden opgenomen na elke invoegen. De standaardwaarde is `" "`. Als u wilt de ruimte weglaat, stelt u de waarde in op `""`.  |


##  <a name="sample-input"></a>Van Voorbeeldinvoer
Een JSON-document bieden van bruikbare invoer voor deze kwalificatie kan worden:

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
In dit voorbeeld ziet u de uitvoer van de vorige invoer, ervan uitgaande dat de *insertPreTag* is ingesteld op `" "`, en *insertPostTag* is ingesteld op `""`. 

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

## <a name="extended-sample-skillset-definition"></a>Uitgebreide voorbeelddefinitie voor vaardigheden

Een veelvoorkomend scenario voor het gebruik van tekst samenvoegen is om samen te voegen van de tekstweergave van installatiekopieën (tekst uit een OCR-vaardigheden of het bijschrift van een installatiekopie) in het veld inhoud van een document. 

De vaardigheden van het volgende voorbeeld maakt gebruik van de OCR-vaardigheden om tekst te extraheren uit in het document ingesloten afbeeldingen. Vervolgens maakt u er een *merged_text* veld naar het oorspronkelijke zowel OCRed tekst van elke afbeelding bevatten. 

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
          "name": "mergedText", "targetName" : "merged_text"
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
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Indexeerfunctie (REST) maken](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
