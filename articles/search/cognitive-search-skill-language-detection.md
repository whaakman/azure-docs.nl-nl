---
title: Taal detecteren cognitief zoeken vaardigheid - Azure Search
description: Evalueert ongestructureerde tekst en voor elke record geeft als resultaat een taal-id met een score die wijzen op de sterkte van de analyse in een Azure Search verrijking-pijplijn.
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
ms.openlocfilehash: 741710a9f2a9e505681401183f5f41be0695633b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53308563"
---
#   <a name="language-detection-cognitive-skill"></a>Taal detecteren cognitieve vaardigheden

Voor maximaal 120 talen, de **taaldetectie** vaardigheid detecteert de taal van de ingevoerde tekst en rapporten van een enkele taalcode voor elk document dat is ingediend bij de aanvraag. De taalcode die is gekoppeld aan een score die wijzen op de sterkte van de analyse.

Deze functie is vooral nuttig wanneer u nodig hebt voor de taal van de tekst als invoer voor andere vaardigheden (bijvoorbeeld de [Sentimentanalyse vaardigheid](cognitive-search-skill-sentiment.md) of [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md)).

> [!NOTE]
> Vanaf December 21 mei 2018, kunt u zich een Cognitive Services-resource koppelen aan een Azure Search-vaardigheden. Hierdoor kunnen we beginnen kosten te bereken voor uitvoering van vaardigheden. Op deze datum ook in rekening voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken. Tekst extractie van documenten blijven worden aangeboden zonder extra kosten.
>
> De uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/) . Afbeelding extractie prijsstelling wordt in rekening gebracht op de preview-prijzen en wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400). Informatie over [meer](cognitive-search-attach-cognitive-services.md).

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Gegevenslimieten
De maximale grootte van een record moet tussen de 50.000 tekens wordt gemeten door `String.Length`. Als u moet het opsplitsen van uw gegevens voordat deze wordt verzonden naar de analyzer sentiment, mag u de [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Kwalificatie invoer

Parameters zijn hoofdlettergevoelig.

| Invoer     | Description |
|--------------------|-------------|
| tekst | De tekst die moet worden geanalyseerd.|

## <a name="skill-outputs"></a>Kwalificatie uitvoer

| Naam van de uitvoer    | Description |
|--------------------|-------------|
| languageCode | De ISO 6391 taalcode voor de taal die wordt geïdentificeerd. Bijvoorbeeld, "en". |
| LanguageName | De naam van de taal. Bijvoorbeeld "Engels". |
| score | Een waarde tussen 0 en 1. De kans dat taal correct wordt geïdentificeerd. De score is mogelijk lager is dan 1 als de zin heeft gemengde talen.  |

##  <a name="sample-definition"></a>Van voorbeelddefinitie

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

##  <a name="sample-input"></a>Van Voorbeeldinvoer

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
Als tekst wordt uitgedrukt in een niet-ondersteunde taal, een fout wordt gegenereerd en er is geen taal-id wordt geretourneerd.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
