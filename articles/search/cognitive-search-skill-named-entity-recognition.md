---
title: Met de naam voor entiteit cognitieve zoeken kwalificatie (Azure Search) | Microsoft Docs
description: Benoemde entiteiten voor de persoon, locatie en organisatie extraheren uit tekst in een Azure Search-pijplijn cognitieve zoeken.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 73ffcf5e2ced63fddaf0f5ef2ca7e72a7d94b966
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
#    <a name="named-entity-recognition-cognitive-skill"></a>Benoemde entiteit erkenning cognitieve kwalificatie

De **met de naam entiteit erkenning** kwalificatie benoemde eenheden geëxtraheerd uit de tekst. Beschikbare entiteiten bevatten de typen `person`, `location`, en `organization`.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Text.NamedEntityRecognitionSkill

## <a name="skill-parameters"></a>Kwalificatie parameters

Parameters zijn hoofdlettergevoelig.

| Parameternaam     | Beschrijving |
|--------------------|-------------|
| categorieën    | Matrix van categorieën die moet worden opgehaald.  Mogelijke categorietypen: `"Person"`, `"Location"`, `"Organization"`. Als er geen categorie is opgegeven, worden alle typen geretourneerd.|
|defaultLanguageCode |  Taalcode van de ingevoerde tekst. De volgende talen worden ondersteund: `ar, cs, da, de, en, es, fi, fr, he, hu, it, ko, pt-br, pt`|
| minimumPrecision  | Een getal tussen 0 en 1. Als de precisie lager is dan deze waarde is, wordt de entiteit wordt niet geretourneerd. De standaardwaarde is 0.|

## <a name="skill-inputs"></a>Kwalificatie invoer

| Voer een naam      | Beschrijving                   |
|---------------|-------------------------------|
| languageCode  | Optioneel. De standaardwaarde is `"en"`.  |
| Tekst          | De tekst te analyseren.          |

## <a name="skill-outputs"></a>Kwalificatie uitvoer

| Naam voor uitvoer     | Beschrijving                   |
|---------------|-------------------------------|
| personen      | Een matrix met tekenreeksen waar elke tekenreeks de naam van een persoon vertegenwoordigt. |
| Locaties  | Een matrix met tekenreeksen, waarbij elke tekenreeks staat voor een locatie. |
| Organisaties  | Een matrix met tekenreeksen, waarbij elke tekenreeks staat voor een organisatie. |
| entiteiten | Een matrix van complexe typen. Elk complex type bevat de volgende velden: <ul><li>categorie (`"person"`, `"organization"`, of `"location"`)</li> <li>waarde (de entiteitsnaam van de werkelijke)</li><li>offset (de locatie waar het is gevonden in de tekst)</li><li>vertrouwen (een waarde tussen 0 en 1 die aangeeft dat vertrouwen dat de waarde een feitelijke entiteit is)</li></ul> |

##  <a name="sample-definition"></a>Voorbeeld-definitie

```json
  {
    "@odata.type": "#Microsoft.Skills.Text.NamedEntityRecognitionSkill",
    "categories": [ "Person"],
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
##  <a name="sample-input"></a>Voorbeeldinvoer

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
             "text": "This is a the loan application for Joe Romero, he is a Microsoft employee who was born in Chile and then moved to Australia… Ana Smith is provided as a reference.",
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
        "locations": ["Seattle"],
        "organizations":["Microsoft Corporation"],
        "entities":  
        [
          {
            "category":"person",
            "value": "Joe Romero",
            "offset": 45,
            "confidence": 0.87
          },
          {
            "category":"person",
            "value": "Ana Smith",
            "offset": 59,
            "confidence": 0.87
          },
          {
            "category":"location",
            "value": "Seattle",
            "offset": 5,
            "confidence": 0.99
          },
          {
            "category":"organization",
            "value": "Microsoft Corporation",
            "offset": 120,
            "confidence": 0.99
          }
        ]
      }
    }
  ]
}
```


## <a name="error-cases"></a>Foutgevallen
Als u een niet-ondersteunde taalcode opgeeft, of als de inhoud komt niet overeen met de taal die is opgegeven, een fout is geretourneerd en er zijn geen entiteiten worden opgehaald.

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)