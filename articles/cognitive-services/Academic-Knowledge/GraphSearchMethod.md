---
title: Graph-methode van de Search - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Gebruik de methode graaf zoeken in de Academic Knowledge API op een set academische entiteiten op basis van specifieke graafpatronen retourneren.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: f6c2fbe5daeb114d6a5ea77c9823f1fa5bfe8425
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55864465"
---
# <a name="graph-search-method"></a>Zoekmethode Graph

De **graaf zoeken** REST-API wordt gebruikt om te retourneren van een set academische entiteiten op basis van de gegeven grafiek patronen.  Het antwoord is een set met graph-paden die voldoen aan de gebruiker opgegeven beperkingen. Het pad van een grafiek is een interleaved reeks graph-knooppunten en randen in de vorm van _v0, e0, v1, e1,..., vn_, waarbij _v0_ is het eerste knooppunt van het pad.
<br>

**REST-eindpunt:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?
```   
<br>

## <a name="request-parameters"></a>Aanvraagparameters  
Name     | Value | Vereist?  | Description
-----------|-----------|---------|--------
**mode**       | Tekenreeks met tekst | Ja | De naam van de modus die u wilt gebruiken. De waarde is een *json* of *lambda*.

De zoekmethode die grafiek moet worden aangeroepen via een HTTP POST-aanvraag. De post-aanvraag moet de inhoudstype-header bevatten: **application/json**.

##### <a name="json-search"></a>JSON-zoeken 

Voor de *json* zoeken, in de hoofdtekst van bericht is een JSON-object. Het JSON-object beschrijft een padpatroon met de gebruiker opgegeven beperkingen (Zie de [specificatie van de JSON-object](JSONSearchSyntax.md) voor *json* zoeken).


##### <a name="lambda-search"></a>Lambda-zoeken

Voor de *lambda* zoeken, in de hoofdtekst van bericht is een tekenreeks met tekst zonder opmaak. De hoofdtekst van bericht is een queryreeks LIKQ lambda, dit één C#-instructie is (Zie de [specificatie van de query-tekenreeks](LambdaSearchSyntax.md) voor *lambda* zoeken). 

<br>
## <a name="response-json"></a>Antwoord (JSON)
Name | Description
-------|-----   
**Resultaten** | Een matrix met 0 of meer entiteiten die overeenkomen met de query-expressie. Elke entiteit bevat de waarden van de aangevraagde kenmerken. Dit veld is aanwezig als de aanvraag is verwerkt.
**error** | HTTP-statuscodes. Dit veld is aanwezig als de aanvraag is mislukt.
**message** | Foutbericht. Dit veld is aanwezig als de aanvraag is mislukt.

Als een query kan niet worden verwerkt binnen _800 ms_, een _time-out_ fout wordt geretourneerd. 

<br>
#### <a name="example"></a>Voorbeeld:

##### <a name="json-search"></a>JSON-zoeken
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=json
```
<br>
Voor de *json* zoeken, als we ophalen van de documenten waarvan de titels bevatten willen 'graph-engine' en die is geschreven door 'bin shao', kunnen we als volgt de query opgeven.

```JSON
{
  "path": "/paper/AuthorIDs/author",
  "paper": {
    "type": "Paper",
    "NormalizedTitle": "graph engine",
    "select": [
      "OriginalTitle"
    ]
  },
  "author": {
    "return": {
      "type": "Author",
      "Name": "bin shao"
    }
  }
}
```

De uitvoer van een query is een reeks paden van de grafiek. Een graph-pad is een matrix met knooppuntobjecten die overeenkomt met de knooppunten die zijn opgegeven in de query-pad. Deze knooppuntobjecten hebben ten minste één eigenschap *CellID*, die staat voor de entiteit-ID. Andere eigenschappen kunnen worden opgehaald door op te geven de namen van eigenschappen via de operator select van een [ *Traversal actie voor het Object*](JSONSearchSyntax.md).

```JSON
{
  "Results": [
    [
      {
        "CellID": 2160459668,
        "OriginalTitle": "Trinity: a distributed graph engine on a memory cloud"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 2171539317,
        "OriginalTitle": "A distributed graph engine for web scale RDF data"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 2411554868,
        "OriginalTitle": "A distributed graph engine for web scale RDF data"
      },
      {
        "CellID": 2093502026
      }
    ],
    [
      {
        "CellID": 73304046,
        "OriginalTitle": "The Trinity graph engine"
      },
      {
        "CellID": 2093502026
      }
    ]
  ]
}
 ```

##### <a name="lambda-search"></a>Lambda-zoeken 

```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=lambda
```
<br>
Voor de *lambda* zoeken, als we willen ophalen van de auteur van de id's van een bepaald artikel, we kunnen een query schrijven zoals de volgende.

```
MAG.StartFrom(@"{
    type  : ""Paper"",
    match : {
        NormalizedTitle : ""trinity: a distributed graph engine on a memory cloud""
    }
}").FollowEdge("AuthorIDs").VisitNode(Action.Return)
```

De uitvoer van een *lambda* zoekquery is ook een reeks paden van de grafiek:

```JSON
{
  "Results": [
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2142490828
      }
    ],
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2116756368
      }
    ],
    [
      {
        "CellID": 2160459668
      },
      {
        "CellID": 2093502026
      }
    ]
  ]
}
```
 
## <a name="graph-schema"></a>Graph-Schema

Graph-schema is handig voor het schrijven van graph zoekquery's. Deze wordt weergegeven in de volgende afbeelding.

![Microsoft Academic Graph-Schema](./Images/AcademicGraphSchema.png)
