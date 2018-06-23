---
title: Grafiek Search-methode in de Academic Knowledge API | Microsoft Docs
description: De grafiek Search-methode in de Academic Knowledge API gebruiken om te retourneren van een reeks academic entiteiten op basis van specifieke grafiek patronen in cognitieve Microsoft-Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: d811db117c934c0d41fbfea1220d241cc022e4a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344410"
---
# <a name="graph-search-method"></a>Grafiek Search-methode

De **grafiek zoeken** REST-API wordt gebruikt om te retourneren van een reeks academic entiteiten op basis van de gegeven grafiek patronen.  Het antwoord is een set van grafiek paden die voldoet aan de gebruiker opgegeven beperkingen. Een grafiek pad is een interleaved reeks grafiekknooppunten en randen in de vorm van _v0, e0, v1, e1,..., vn_, waarbij _v0_ is het eerste knooppunt van het pad.
<br>

**REST-eindpunt:**  
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?
```   
<br>

## <a name="request-parameters"></a>Aanvraagparameters  
Naam     | Waarde | Vereist?  | Beschrijving
-----------|-----------|---------|--------
**Modus**       | Tekenreeks | Ja | Naam van de modus die u wilt gebruiken. De waarde is *json* of *lambda*.

De grafiek search-methode moet worden aangeroepen via een HTTP POST-aanvraag. De post-aanvraag moet de header content-type bevatten: **application/json**.

##### <a name="json-search"></a>JSON zoeken 

Voor de *json* zoeken, de hoofdtekst van bericht is een JSON-object. De JSON-object wordt een patroon pad met de gebruiker opgegeven beperkingen beschreven (Zie de [specificatie van JSON-object](JSONSearchSyntax.md) voor *json* zoeken).


##### <a name="lambda-search"></a>Lambda zoeken

Voor de *lambda* zoeken, de hoofdtekst van bericht is een tekenreeks met tekst zonder opmaak. De hoofdtekst van bericht is een queryreeks LIKQ lambda, die één C#-instructie (Zie de [specificatie van queryreeks](LambdaSearchSyntax.md) voor *lambda* zoeken). 

<br>
## <a name="response-json"></a>Antwoord (JSON)
Naam | Beschrijving
-------|-----   
**resultaten** | Een matrix met 0 of meer entiteiten die overeenkomen met de query-expressie. Elke entiteit bevat de waarden van de aangevraagde kenmerken. Dit veld is aanwezig als de aanvraag is verwerkt.
**Fout** | HTTP-statuscodes. Dit veld is aanwezig als de aanvraag is mislukt.
**Bericht** | Foutbericht. Dit veld is aanwezig als de aanvraag is mislukt.

Als een query kan niet worden verwerkt binnen _800 ms_, een _time-out_ fout wordt geretourneerd. 

<br>
#### <a name="example"></a>Voorbeeld:

##### <a name="json-search"></a>JSON zoeken
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=json
```
<br>
Voor de *json* zoeken, als we krijgen de papers waarvan de titels bevatten willen 'graph engine' en geschreven door 'bin shao', kunnen we als volgt de query opgeven.

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

De uitvoer van een query is een matrix van grafiek paden. Een grafiek pad is een matrix van knooppuntobjecten overeenkomt met de knooppunten in het pad van de query opgegeven. Deze knooppuntobjecten hebben ten minste één eigenschap *CellID*, die staat voor de entiteit-ID. Andere eigenschappen kunnen worden opgehaald door te geven of de eigenschapnamen via de operator select van een [ *Traversal actie-Object*](JSONSearchSyntax.md).

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

##### <a name="lambda-search"></a>Lambda zoeken 

```
https://westus.api.cognitive.microsoft.com/academic/v1.0/graph/search?mode=lambda
```
<br>
Voor de *lambda* zoeken, als we wilt krijgen van de auteur-id's van een bepaald artikel, wordt een query schrijven zoals de volgende.

```
MAG.StartFrom(@"{
    type  : ""Paper"",
    match : {
        NormalizedTitle : ""trinity: a distributed graph engine on a memory cloud""
    }
}").FollowEdge("AuthorIDs").VisitNode(Action.Return)
```

De uitvoer van een *lambda* zoekquery is ook een matrix van grafiek paden:

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
 
## <a name="graph-schema"></a>Grafiek-Schema

Schema van de grafiek is nuttig voor het schrijven van grafiek zoekquery's. Dit wordt weergegeven in de volgende afbeelding.

![Academic Microsoft Graph Schema](./Images/AcademicGraphSchema.png)
