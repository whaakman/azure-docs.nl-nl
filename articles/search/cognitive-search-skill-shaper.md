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
ms.date: 05/02/2019
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 058b6c979346d9dcce36940432d0e222e919dba9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65540834"
---
#   <a name="shaper-cognitive-skill"></a>Shaper cognitieve vaardigheden

De **Shaper** vaardigheid consolideert verschillende invoer in een [complexe type](search-howto-complex-data-types.md) die later in de pijplijn verrijking kan worden verwezen. De **Shaper** vaardigheden kunt u in feite een structuur maken, het definiëren van de naam van de leden van die structuur en het waarden toewijzen aan elk lid. Voorbeelden van geconsolideerde velden nuttig bij het zoeken in scenario's zijn de naam van een eerste en laatste combineren in een enkele structuur, plaats en staat in een enkele structuur, of een naam en geboortedatum in een enkele structuur tot stand brengen van de unieke id.

De API-versie, bepaalt de diepte van het vormgeven van u kan bereiken. 

| API-versie | Gedrag vormgeven | 
|-------------|-------------------|
| 2019-05-06-preview-versie van de REST-API (.NET-SDK wordt niet ondersteund) | Complexe objecten, meerdere niveaus diep in een **Shaper** vaardigheid definitie. |
| 2019-05-06 ** (algemeen beschikbaar), 2017-11-11-Preview| Complexe objecten, één niveau diep. Een vorm van meerdere niveaus vereist verschillende shaper stappen samen-koppeling.|

Geleverd door `api-version=2019-05-06-Preview`, wordt de **Shaper** vaardigheid geïllustreerd in [scenario 3](#nested-complex-types) voegt een nieuwe optionele *sourceContext* eigenschap in op de invoer. De *bron* en *sourceContext* eigenschappen zijn sluiten elkaar wederzijds uit. Als de invoer in de context van de kwalificatie is, gebruik *bron*. Als de invoer naar een *verschillende* context dan de kwalificatie context, gebruik de *sourceContext*. De *sourceContext* , moet u voor het definiëren van een geneste invoer met de specifieke element als de bron wordt behandeld. 

In het antwoord, voor alle API-versies, is de naam van de uitvoer altijd 'uitvoeren'. De pijplijn kunt intern, een andere naam, zoals 'analyzedText' toewijzen, zoals wordt weergegeven in de onderstaande voorbeelden, maar de **Shaper** vaardigheid zelf wordt 'uitvoer' in het antwoord geretourneerd. Dit kan belangrijk zijn als u foutopsporing verrijkt documenten en u ziet de naamgevingsconventie afwijking, of als u een aangepaste vaardigheden ontwikkelen en zijn structureren van het antwoord zelf.

> [!NOTE]
> De **Shaper** kwalificatie is niet gebonden aan een Cognitive Services-API en u niet worden in rekening gebracht voor het gebruik ervan. U moet nog steeds [een Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md), echter overschrijven de **gratis** resource-optie waarmee u een klein aantal dagelijkse enrichments per dag wordt beperkt.

## <a name="odatatype"></a>@odata.type  
Microsoft.Skills.Util.ShaperSkill

## <a name="scenario-1-complex-types"></a>Scenario 1: complexe typen

U hebt een scenario waarin u wilt maken van een structuur genaamd *analyzedText* waarvoor twee leden: *tekst* en *sentiment*, respectievelijk. In een Azure Search-index, een meerdelige doorzoekbaar veld heet een *complexe type* en dit wordt vaak gemaakt wanneer de brongegevens heeft een bijbehorende complexe structuur die is toegewezen aan deze.

Een andere benadering voor het maken van complexe typen is echter via de **Shaper** kwalificatie. Door deze vaardigheid te nemen in een set vaardigheden, kunnen de bewerkingen in het geheugen tijdens de verwerking van de vaardigheden uitvoer gegevensshapes met geneste structuren, die vervolgens kunnen worden toegewezen aan een complex type in de index. 

De volgende definitie van de voorbeeld-kwalificatie bevat het lid namen als invoer. 


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
      "targetName": "analyzedText"
    }
  ]
}
```

### <a name="sample-index"></a>Index van voorbeeld

Een set vaardigheden wordt aangeroepen door een indexeerfunctie en een indexeerfunctie moet een index. Een weergave complexe veld in uw index uitzien als in het volgende voorbeeld. 

```json

    "name": "my-index",
    "fields": [
        {   "name": "myId", "type": "Edm.String", "key": true, "filterable": true   },
        {   "name": "analyzedText", "type": "Edm.ComplexType",
            "fields": [{
                    "name": "text",
                    "type": "Edm.String",
                    "filterable": false,
                    "sortable": false,
                    "facetable": false,
                    "searchable": true  },
          {
                    "name": "sentiment",
                    "type": "Edm.Double",
                    "searchable": true,
                    "filterable": true,
                    "sortable": true,
                    "facetable": true
                },
```

### <a name="skill-input"></a>Kwalificatie invoer

Een binnenkomende JSON-document bieden van bruikbare invoer voor deze **Shaper** vaardigheid kan zijn:

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


### <a name="skill-output"></a>Kwalificatie uitvoer

De **Shaper** vaardigheid genereert een nieuwe element met de naam *analyzedText* met de combinatie van elementen van *tekst* en *sentiment*. Deze uitvoer voldoet aan het indexschema. Deze worden geïmporteerd en geïndexeerd in een Azure Search-index.

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

## <a name="scenario-2-input-consolidation"></a>Scenario 2: invoer consolidatie

Een ander voorbeeld: Stel die in verschillende stadia van de pipeline-verwerking, u hebt uitgepakt de titel van een boek en hoofdstuktitels op verschillende pagina's van het rapport. U kunt nu een enkele structuur bestaat uit deze verschillende invoer maken.

De **Shaper** vaardigheid definitie voor dit scenario kan er uitzien als in het volgende voorbeeld:

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
            "source": "/document/content/pages/*/chapterTitles/*/title"
        }
    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "titlesAndChapters"
        }
    ]
}
```

### <a name="skill-output"></a>Kwalificatie uitvoer
In dit geval de **Shaper** worden alle hoofdstuktitels voor het maken van een matrix samengevoegd. 

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

<a name="nested-complex-types"></a>

## <a name="scenario-3-input-consolidation-from-nested-contexts"></a>Scenario 3: invoer consolidatie van geneste contexten

> [!NOTE]
> Geneste structuren die worden ondersteund in de [2019 in de REST-API-versie-05-06-Preview](search-api-preview.md) kan worden gebruikt in een [kennis store](knowledge-store-concept-intro.md) of in een Azure Search-index.

Stel dat u over de titel, de hoofdstukken en de inhoud van een boek en hebben op de inhoud op entiteit erkenning en de sleutel zinnen uitgevoerd en moet nu cumulatieve resultaten van de verschillende vaardigheden in een enkele vorm met de naam van de hoofdstuk, entiteiten en belangrijke zinnen.

De **Shaper** vaardigheid definitie voor dit scenario kan er uitzien als in het volgende voorbeeld:

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
            "sourceContext": "/document/content/pages/*/chapterTitles/*",
            "inputs": [
              {
                  "name": "title",
                  "source": "/document/content/pages/*/chapterTitles/*/title"
              },
              {
                  "name": "number",
                  "source": "/document/content/pages/*/chapterTitles/*/number"
              }
            ]
        }

    ],
    "outputs": [
        {
            "name": "output",
            "targetName": "titlesAndChapters"
        }
    ]
}
```

### <a name="skill-output"></a>Kwalificatie uitvoer
In dit geval de **Shaper** maakt u een complex type. Deze structuur bestaat in het geheugen. Als u opslaan in een winkel kennis wilt, moet u een projectie maken in uw vaardigheden uit waarin de kenmerken van opslag.

```json
{
    "values": [
        {
            "recordId": "1",
            "data": {
                "titlesAndChapters": {
                    "title": "How to be happy",
                    "chapterTitles": [
                      { "title": "Start young", "number": 1},
                      { "title": "Laugh often", "number": 2},
                      { "title": "Eat, sleep and exercise", "number: 3}
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
+ [Het gebruik van complexe typen](search-howto-complex-data-types.md)
+ [Kennis store overzicht](knowledge-store-concept-intro.md)
+ [Aan de slag met kennis store](knowledge-store-howto.md)