---
title: Taal detecteren cognitief zoeken vaardigheid (Azure Search) | Microsoft Docs
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
ms.openlocfilehash: 2fd1c1ec0d2442afd6367e1d35af6f798dced2c7
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733275"
---
#   <a name="language-detection-cognitive-skill"></a>Taal detecteren cognitieve vaardigheden

Voor maximaal 120 talen, de **taaldetectie** vaardigheid detecteert de taal van de ingevoerde tekst en rapporten van een enkele taalcode voor elk document dat is ingediend bij de aanvraag. De taalcode die is gekoppeld aan een score die wijzen op de sterkte van de analyse.

Deze functie is vooral nuttig wanneer u nodig hebt voor de taal van de tekst als invoer voor andere vaardigheden (bijvoorbeeld de [Sentimentanalyse vaardigheid](cognitive-search-skill-sentiment.md) of [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md)).

> [!NOTE]
> Cognitief zoeken is een openbare preview. Uitvoering van vaardigheden en uitpakken van de installatiekopie en normalisering worden momenteel gratis aangeboden. Op een later tijdstip, worden de prijzen van deze mogelijkheden aangekondigd. 

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.LanguageDetectionSkill

## <a name="data-limits"></a>Limieten voor gegevens
De maximale grootte van een record moet tussen de 50.000 tekens wordt gemeten door `String.Length`. Als u moet het opsplitsen van uw gegevens voordat deze wordt verzonden naar de analyzer sentiment, mag u de [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md).

## <a name="skill-inputs"></a>Kwalificatie invoer

Parameters zijn hoofdlettergevoelig.

| Invoer     | Beschrijving |
|--------------------|-------------|
| tekst | De tekst die moet worden geanalyseerd.|

## <a name="skill-outputs"></a>Kwalificatie uitvoer

| Naam van de uitvoer    | Beschrijving |
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
