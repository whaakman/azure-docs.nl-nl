---
title: Shaper cognitieve zoeken kwalificatie (Azure Search) | Microsoft Docs
description: Ophalen van metagegevens en gestructureerde gegevens uit niet-gestructureerde gegevens en deze als een complex type in een Azure Search verrijking pijplijn vorm.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 311f4bd67081de567763783a9d86540eda36d9f8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
#   <a name="shaper-cognitive-skill"></a>Shaper cognitieve kwalificatie

De **Shaper** kwalificatie maakt een complex type ter ondersteuning van samengestelde velden (ook wel bekend als meerdelige velden). Een complex typeveld heeft meerdere onderdelen, maar wordt behandeld als één item in een Azure Search-index. Voorbeelden van geconsolideerde nuttig in scenario's zoeken velden zijn de naam van een eerste en laatste combineren in een enkel veld, plaats en status in een enkel veld of de naam en geboortedatum in één veld tot stand brengen van unieke id van de.

De kwalificatie Shaper kunt u in feite een structuur maken, de naam van de leden van die structuur definiëren en waarden toewijzen aan elk lid.

Standaard ondersteunt deze techniek objecten die zich één niveau diep. U kunt verschillende Shaper stappen koppelen voor complexe objecten.

In het antwoord is de uitvoernaam altijd 'uitvoeren'. Intern maakt de pipeline een andere naam kunt toewijzen, zoals 'analyzedText' in de onderstaande voorbeelden 'uitvoer', maar de Shaper kwalificatie zelf als resultaat 'uitvoer' in het antwoord. Dit kan belangrijk zijn als u fouten opspoort verrijkt documenten en u ziet de naamgevingsconventie discrepantie, of als u een aangepaste kwalificatie bouwen en zijn structureren van het antwoord zelf.


## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="sample-1-complex-types"></a>Voorbeeld 1: complexe typen

Neem bijvoorbeeld een scenario waarin u wilt maken van een structuur aangeroepen *analyzedText* die heeft twee leden: *tekst* en *gevoel*respectievelijk. In Azure Search een meerdelig doorzoekbaar veld heet een *complex type*, en gebruiksklaar nog niet wordt ondersteund. In dit voorbeeld, kan een kwalificatie Shaper worden gebruikt voor het genereren van de velden van een complex type in de index. 

Het volgende voorbeeld bevat het lid namen als invoer. De uitvoer-structuur (uw complexe veld in Azure Search) is opgegeven via *targetName*. 


```json
{
  "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
  "context": "/document/content/phrases/*",
  "inputs": [
    {
      "name": "text",
      "source": "/document/content/phrases/*"
    },
    {
      "name": "sentiment",
      "source": "/document/content/phrases/*/sentiment"
    }
  ],
  "outputs": [
    {
      "name": "output",
      "targetName": analyzedText"
    }
  ]
}
```

### <a name="sample-input"></a>Voorbeeldinvoer
Een JSON-document bieden van bruikbare invoer voor deze kwalificatie Shaper mogelijk:

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "text": "this movie is awesome",
                "sentiment": 0.9
            }
        }
    ]
}
```


### <a name="sample-output"></a>Voorbeelduitvoer
De kwalificatie Shaper genereert een nieuw element met de naam *analyzedText* met de gecombineerde elementen van *tekst* en *gevoel*. 

```json
{
    "values": [
      {
        "recordId": "1",
        "data":
           {
            "analyzedText": 
              {
                "text": "this movie is awesome" ,
                "sentiment": 0.9
              }
           }
      }
    ]
}
```

## <a name="sample-2-input-consolidation"></a>Voorbeeld 2: invoer consolidatie

Een ander voorbeeld: Stel dat in verschillende stadia van de pipeline-verwerking, u hebt uitgepakt de titel van een boek en hoofdstuktitels op verschillende pagina's van het rapport. U kunt nu een enkele structuur bestaat uit deze verschillende invoer maken.

De definitie van de kwalificatie Shaper voor dit scenario lijkt op het volgende voorbeeld:

```json
{
    "@odata.type": "#Microsoft.Skills.Util.ShaperSkill",
    "context": "/document",
    "inputs": [
        {
            "name": "title",
            "source": "/document/content/title"
        },
        {
            "name": "chapterTitles",
            "source": "/document/content/pages/*/chapterTitles/*"
        }
    ],
    "outputs": [
        {
            "output": "titlesAndChapters",
            "targetName": "analyzedText"
        }
    ]
}
```

### <a name="sample-output"></a>Voorbeelduitvoer
In dit geval wordt de Shaper samengevoegd voor alle hoofdstuktitels voor het maken van een matrix. 

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "titlesAndChapters": {
                    "title": "How to be happy",
                    "chapterTitles": [
                        "Start young",
                        "Laugh often",
                        "Eat, sleep and exercise"
                    ]
                }
            }
        }
    ]
}
```

## <a name="see-also"></a>Zie ook

+ [Vooraf gedefinieerde vaardigheden](cognitive-search-predefined-skills.md)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)

