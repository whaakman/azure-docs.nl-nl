---
title: Taal detectie cognitieve zoeken kwalificatie (Azure Search) | Microsoft Docs
description: Niet-gestructureerde tekst geëvalueerd en voor elke record, retourneert een taal-id met een score die aangeeft van de sterkte van de analyse in een Azure Search verrijking pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 338d89b47ea451efcf8300d4ac016a6946a95259
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
#   <a name="language-detection-cognitive-skill"></a>Taal detectie cognitieve kwalificatie

Voor maximaal 120 talen de **taaldetectie** kwalificatie detecteert de taal van de ingevoerde tekst en rapporten van een enkele taalcode voor elk document die op de aanvraag heeft ingediend. De taalcode is gekoppeld aan een score die aangeeft van de sterkte van de analyse.

Deze mogelijkheid is vooral nuttig wanneer u moet de taal van de tekst opgeven als invoer voor andere vaardigheden (bijvoorbeeld de [gevoel Snalysis kwalificatie](cognitive-search-skill-sentiment.md) of [tekst Split-kwalificatie](cognitive-search-skill-textsplit.md)).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Gegevenslimieten
De maximale grootte van een record moet 50.000 tekens gemeten door `String.Length`. Als u uw gegevens wilt voordat deze wordt verzonden naar de analyzer gevoel onderverdelen, mag u de [tekst Split-kwalificatie](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Kwalificatie invoer

Parameters zijn hoofdlettergevoelig.

| Invoer     | Beschrijving |
|--------------------|-------------|
| Tekst | De tekst die moet worden geanalyseerd.|

## <a name="skill-outputs"></a>Kwalificatie uitvoer

| Naam voor uitvoer    | Beschrijving |
|--------------------|-------------|
| languageCode | De taalcode van de ISO-6391 voor de taal die wordt geïdentificeerd. Bijvoorbeeld 'en'. |
| LanguageName | De naam van de taal. Bijvoorbeeld 'Engels'. |
| Score | Een waarde tussen 0 en 1. De kans dat taal correct wordt geïdentificeerd. De score zijn mogelijk lager is dan 1 als de zin talen verschillende bevat.  |

##  <a name="sample-definition"></a>Voorbeeld-definitie

```json
 {
    "@odata.type": "#Microsoft.Skills.Text.LanguageDetectionSkill ",
    "inputs": [
      {
        "name": "text",
        "source": "/document/text"
      }
    ],
    "outputs": [
      {
        "name": "languageCode",
        "targetName": "myLanguageCode"
      },
      {
        "name": "languageName",
        "targetName": "myLanguageName"
      },
      {
        "name": "score",
        "targetName": "myLanguageScore"
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
        "data":
           {
             "text": "Glaciers are huge rivers of ice that ooze their way over land, powered by gravity and their own sheer weight. "
           }
      },
      {
        "recordId": "2",
        "data":
           {
             "text": "Estamos muy felices de estar con ustedes."
           }
      }
    ]
```


##  <a name="sample-output"></a>Voorbeelduitvoer

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
            {
              "languageCode": "en",
              "languageName": "English",
              "score": 1,
            }
      },
      {
        "recordId": "2",
        "data":
            {
              "languageCode": "es",
              "languageName": "Spanish",
              "score": 1,
            }
      }
    ]
}
```


## <a name="error-cases"></a>Foutgevallen
Als tekst wordt uitgedrukt in een niet-ondersteunde taal, een fout is gegenereerd en er is geen taal-id geretourneerd.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)