---
title: Shaper cognitief zoeken vaardigheid - Azure Search
description: Metagegevens en gestructureerde informatie onttrekken aan niet-gestructureerde gegevens en vorm te geven als een complex type in een Azure Search verrijking-pijplijn.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 01/17/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: a1f5a698ee76ebd0561bd19ff1a23d0f04be0771
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2019
ms.locfileid: "54410112"
---
#   <a name="shaper-cognitive-skill"></a>Shaper cognitieve vaardigheden

De **Shaper** vaardigheid maakt een complex type ter ondersteuning van samengestelde velden (ook wel bekend als gedeeltelijk velden). Een veld van het complexe type heeft meerdere onderdelen, maar wordt beschouwd als één item in een Azure Search-index. Voorbeelden van geconsolideerde velden nuttig bij het zoeken in scenario's zijn de naam van een eerste en laatste combineren in een enkel veld, de plaats en de status in een enkel veld of de naam en geboortedatum in één veld tot stand brengen van de unieke id.

De **Shaper** vaardigheden kunt u in feite een structuur maken, het definiëren van de naam van de leden van die structuur en het waarden toewijzen aan elk lid.

Standaard ondersteunt deze techniek objecten die een niveau zijn. Voor meer complexe objecten, kunt u verschillende koppelen **Shaper** stappen.

In het antwoord, is de naam van de uitvoer altijd 'uitvoeren'. Intern, de pijplijn kunt toewijzen voor een andere naam, zoals 'analyzedText' in de voorbeelden hieronder om de "uitvoeren", maar de **Shaper** vaardigheid zelf wordt 'uitvoer' in het antwoord geretourneerd. Dit kan belangrijk zijn als u foutopsporing verrijkt documenten en u ziet de naamgevingsconventie afwijking, of als u een aangepaste vaardigheden ontwikkelen en zijn structureren van het antwoord zelf.

> [!NOTE]
> Deze kwalificatie is niet gebonden aan een Cognitive Services-API en u niet worden in rekening gebracht voor het gebruik ervan. U moet nog steeds [een Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md), echter overschrijven de **gratis** resource-optie waarmee u een klein aantal dagelijkse enrichments per dag wordt beperkt.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="sample-1-complex-types"></a>Voorbeeld 1: complexe typen

U hebt een scenario waarin u wilt maken van een structuur genaamd *analyzedText* waarvoor twee leden: *tekst* en *sentiment*, respectievelijk. In Azure Search, een meerdelige doorzoekbaar veld heet een *complexe type*, en hieraan is nog niet wordt ondersteund. In deze Preview-versie, een **Shaper** vaardigheid kan worden gebruikt voor het genereren van velden van een complex type in uw index. 

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

### <a name="sample-input"></a>Van Voorbeeldinvoer
Een JSON-document bieden van bruikbare invoer voor deze **Shaper** vaardigheid kan zijn:

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
De **Shaper** vaardigheid genereert een nieuwe element met de naam *analyzedText* met de combinatie van elementen van *tekst* en *sentiment*. 

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

Een ander voorbeeld: Stel die in verschillende stadia van de pipeline-verwerking, u hebt uitgepakt de titel van een boek en hoofdstuktitels op verschillende pagina's van het rapport. U kunt nu een enkele structuur bestaat uit deze verschillende invoer maken.

De definitie van de kwalificatie Shaper voor dit scenario kan er uitzien als in het volgende voorbeeld:

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
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)

