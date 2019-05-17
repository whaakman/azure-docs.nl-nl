---
title: Met de naam entiteit erkenning cognitief zoeken vaardigheid - Azure Search
description: Benoemde entiteiten voor persoon, locatie en organisatie extraheren uit tekst in een Azure Search cognitief zoeken-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: e6a7d4bb7452ff8f6b3c2536a5aa100a15a6ec78
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65539268"
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Met de naam entiteit erkenning cognitieve vaardigheden

De **herkenning van entiteit met de naam** vaardigheid extraheert benoemde entiteiten uit tekst. Beschikbare entiteiten bevatten de typen `person`, `location` en `organization`.

> [!IMPORTANT]
> Herkenning van benoemde entiteiten kwalificatie is nu niet vervangen door [Microsoft.Skills.Text.EntityRecognitionSkill](cognitive-search-skill-entity-recognition.md). Ondersteuning op 15 februari 2019 gestopt en de API is verwijderd uit het product 2 mei 2019. Volg de aanbevelingen in [cognitief zoeken vaardigheden afgeschaft](cognitive-search-skill-deprecated.md) om te migreren naar een ondersteunde kwalificatie.

> [!NOTE]
> Als u bereik uitbreiden door het verhogen van de frequentie van de verwerking, meer documenten toe te voegen of toe te voegen meer AI-algoritmen, u moet [een factureerbare Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md). Kosten toenemen bij het aanroepen van API's in Cognitive Services en voor het ophalen van de afbeelding als onderdeel van de fase documenten kraken in Azure Search. Er zijn geen kosten voor het ophalen van de tekst van documenten.
>
> Uitvoering van de ingebouwde vaardigheden wordt in rekening gebracht op de bestaande [Cognitive Services betaalt u go prijs](https://azure.microsoft.com/pricing/details/cognitive-services/). Afbeelding extractie prijzen wordt beschreven op de [Azure Search-pagina met prijzen](https://go.microsoft.com/fwlink/?linkid=2042400).


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="data-limits"></a>Gegevenslimieten
De maximale grootte van een record moet tussen de 50.000 tekens wordt gemeten door `String.Length`. Als u moet het opsplitsen van uw gegevens voordat deze naar de extractor sleuteluitdrukkingen verzonden, kunt u overwegen de [tekst splitsen vaardigheid](cognitive-search-skill-textsplit.md).

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Description |
|--------------------|-------------|
| categorieën    | Matrix van categorieën die moeten worden geëxtraheerd.  Mogelijke categorietypen: `"Person"`, `"Location"`, `"Organization"`. Als er geen categorie is opgegeven, worden alle typen worden geretourneerd.|
|defaultLanguageCode |  De taalcode van de invoertekst. De volgende talen worden ondersteund: `de, en, es, fr, it`|
| minimumPrecision  | Een getal tussen 0 en 1. Als de precisie lager is dan deze waarde is, wordt de entiteit wordt niet geretourneerd. De standaardwaarde is 0.|

## <a name="skill-inputs"></a>Kwalificatie invoer

| Naam invoeren      | Description                   |
|---------------|-------------------------------|
| languageCode  | Optioneel. De standaardwaarde is `"en"`.  |
| tekst          | De tekst die moet worden geanalyseerd.          |

## <a name="skill-outputs"></a>Kwalificatie uitvoer

| Naam van de uitvoer     | Description                   |
|---------------|-------------------------------|
| personen      | Een matrix met tekenreeksen waarbij elke tekenreeks de naam van een persoon vertegenwoordigt. |
| locaties  | Een matrix met tekenreeksen waarbij elke tekenreeks een locatie vertegenwoordigt. |
| organizations  | Een matrix met tekenreeksen waarbij elke tekenreeks een organisatie vertegenwoordigt. |
| entiteiten | Een matrix met complexe typen. Elk complexe type bevat de volgende velden: <ul><li>categorie (`"person"`, `"organization"`, of `"location"`)</li> <li>waarde (de naam van de werkelijke entiteit)</li><li>offset (de locatie waar deze is gevonden in de tekst)</li><li>vertrouwen (een waarde tussen 0 en 1 die aangeeft dat vertrouwen dat de waarde een feitelijke entiteit is)</li></ul> |

##  <a name="sample-definition"></a>Van voorbeelddefinitie

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person", "Location", "Organization"],
    "defaultLanguageCode": "en",
    "inputs": [
      {
        "name": "text",
        "source": "/document/content"
      }
    ],
    "outputs": [
      {
        "name": "persons",
        "targetName": "people"
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
             "text": "This is the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia… Ana Smith is provided as a reference.",
             "languageCode": "en"
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
      "data" : 
      {
        "persons": [ "Joe Romero", "Ana Smith"],
        "locations": ["Chile", "Australia"],
        "organizations":["Microsoft"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 33,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 124,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Chile",
            "offset": 88,
            "confidence": 0.99
          },
          {
            "category":"location",
            "value": "Australia",
            "offset": 112,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft",
            "offset": 54,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Foutgevallen
Als de taal van het document niet ondersteund wordt, wordt een fout geretourneerd en er zijn geen entiteiten worden opgehaald.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Entiteit erkenning vaardigheid](cognitive-search-skill-entity-recognition.md)
