---
title: OCR cognitieve zoeken kwalificatie (Azure Search) | Microsoft Docs
description: Haal de tekst uit-bestanden in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 478afe81ed739b98487973eb092ee9cad0aa17fd
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37058963"
---
# <a name="ocr-cognitive-skill"></a>OCR cognitieve kwalificatie

De **OCR** kwalificatie extraheert tekst van de afbeeldingsbestanden. Ondersteunde bestandsindelingen zijn onder andere:

+ . JPEG
+ . JPG
+ . PNG
+ . BMP
+ . GIF


## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Beschrijving |
|--------------------|-------------|
| detectOrientation | Hiermee kunt automatische detectie van afdrukstand. <br/> Geldige waarden: true / false.|
|defaultLanguageCode | <p>  Taalcode van de ingevoerde tekst. Enkele ondersteunde talen: <br/> zh-Hans (ChineseSimplified) <br/> zh-Hant (ChineseTraditional) <br/>CS (Tsjechisch) <br/>da (Denemarken) <br/>NL (Nederlands) <br/>NL (Engels) <br/>Fi (Fins)  <br/>FR (Frans) <br/>  de (Duits) <br/>EL (Grieks) <br/> hu (Hongaars) <br/> Deze (Italiaans) <br/>  Japan (Japans) <br/> Ko (Koreaans) <br/> NB (Noors) <br/>   PL (Pools) <br/> PT (Portugees) <br/>  RU (Russisch) <br/>  ES (Spaans) <br/>  SV (Zweeds) <br/>  TR (Turks) <br/> ar (Arabisch) <br/> ro (Roemeens) <br/> SR-Cyrl (SerbianCyrillic) <br/> SR-Latn (SerbianLatin) <br/>  SK (Slowaaks). <br/>  UNK (onbekend) <br/><br/> Als de taalcode niet opgegeven of null is, is de taal autodetected. </p> |
| textExtractionAlgorithm | 'afgedrukt' of 'geschreven'. De tekst 'geschreven' erkenning OCR-algoritme is momenteel in preview en alleen ondersteund in het Engels. |

## <a name="skill-inputs"></a>Kwalificatie invoer

| Voer een naam      | Beschrijving                                          |
|---------------|------------------------------------------------------|
| installatiekopie         | Complex Type. Momenteel wordt alleen werkt met '/ document/normalized_images' veld, die wordt geproduceerd door de Azure Blob-indexeerfunctie wanneer ```imageAction``` is ingesteld op ```generateNormalizedImages```. Zie de [voorbeeld](#sample-output) voor meer informatie.|


## <a name="skill-outputs"></a>Kwalificatie uitvoer
| Naam voor uitvoer     | Beschrijving                   |
|---------------|-------------------------------|
| tekst          | De tekst zonder opmaak is geëxtraheerd uit de afbeelding.   |
| layoutText    | Complex type dat beschrijft de uitgepakte tekst, evenals de locatie waar de tekst is gevonden.|


## <a name="sample-definition"></a>Voorbeeld-definitie

```json
{
    "skills": [
      {
        "description": "Extracts text (plain and structured) from image.",
        "@odata.type": "#Microsoft.Skills.Vision.OcrSkill",
        "context": "/document/normalized_images/*",
        "defaultLanguageCode": null,
        "detectOrientation": true,
        "inputs": [
          {
            "name": "image",
            "source": "/document/normalized_images/*"
          }
        ],
        "outputs": [
          {
            "name": "text",
            "targetName": "myText"
          },
          {
            "name": "layoutText",
            "targetName": "myLayoutText"
          }
        ]
      }
    ]
 }
```
<a name="sample-output"></a>

## <a name="sample-text-and-layouttext-output"></a>Tekst- en layoutText voorbeelduitvoer

```json
{
  "text": "Hello World. -John",
  "layoutText":
  {
    "language" : "en",
    "text" : "Hello World. -John",
    "lines" : [
      {
        "boundingBox":
        [ {"x":10, "y":10}, {"x":50, "y":10}, {"x":50, "y":30},{"x":10, "y":30}],
        "text":"Hello World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ],
    "words": [
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"Hello"
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"World."
      },
      {
        "boundingBox": [ {"x":110, "y":10}, {"x":150, "y":10}, {"x":150, "y":30},{"x":110, "y":30}],
        "text":"-John"
      }
    ]
  }
}
```

## <a name="sample-merging-text-extracted-from-embedded-images-with-the-content-of-the-document"></a>Voorbeeld: Samenvoegen van tekst opgehaald uit ingesloten afbeeldingen met de inhoud van het document.

Een algemene gebruiksvoorbeeld voor tekst fusie is de mogelijkheid om het samenvoegen van de tekstweergave van installatiekopieën (tekst uit een kwalificatie OCR of het bijschrift van een afbeelding) in het veld inhoud van een document. 

Het volgende voorbeeld vaardigheden maakt een *merged_text* veld bevat de tekstinhoud van het document, evenals de tekst OCRed van elk van de installatiekopieën die zijn ingesloten in dat document. 

#### <a name="request-body-syntax"></a>Syntaxis aanvraagbody
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
Het bovenstaande vaardigheden voorbeeld wordt ervan uitgegaan dat een veld genormaliseerd installatiekopieën bestaat. Als dit veld worden gegenereerd, stel de *imageAction* configuratie in de definitie van de indexeerfunctie *generateNormalizedImages* zoals hieronder wordt weergegeven:

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
+ [TextMerger kwalificatie](cognitive-search-skill-textmerger.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Maak indexeerfunctie (REST)](https://docs.microsoft.com/rest/api/searchservice/create-indexer)
