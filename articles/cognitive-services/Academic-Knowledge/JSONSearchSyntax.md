---
title: JSON-zoeksyntaxis - Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Meer informatie over de JSON-zoeksyntaxis die kunt u in de Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: b3f0c5c22775a6eb0ab7a34592d816ed174539c0
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55196562"
---
# <a name="json-search-syntax"></a>JSON-zoeksyntaxis

```javascript
/* Query Object:
   Suppose we have a query path /v0/e0/v1/e1/...
   A query object contains constraints to be applied to graph nodes in a path.
   Constraints are given in <"node identifier", {constraint object}> key-value pairs: 
*/
{
    /* query path can also be set in the query object */
    path: "/v0/e0/v1/e1/.../vn/",
    v0: { /* A Constraint Object */ },
    v1: { /* A Constraint Object */ },
}
```

De knooppuntnamen van de in een query-pad (_v0, v1,..._ ) fungeren als knooppunt-id's die kunnen worden verwezen in de queryobject. de namen van de rand (_e0, e1,..._ ) vertegenwoordigen de typen van de bijbehorende randen in het pad. We kunnen een sterretje gebruiken _*_ als de naam van een knooppunt of randtabel (met uitzondering van het eerste knooppunt, die moet worden gegeven) om aan te geven dat er zijn geen beperkingen met betrekking tot deze een element. Bijvoorbeeld, een query pad `/v0/*/v1/e1/*/` paden opgehaald uit de grafiek zonder het beperken van het type van de rand _(v0, v1)_. In de tussentijd zorgen, de query heeft geen beperkingen met betrekking tot de bestemming (het laatste knooppunt) van het pad naar een.

Wanneer een pad slechts één knooppunt bevat, zeg _v0_, de query retourneert alle entiteiten die voldoen aan de beperkingen. Een beperking-object dat is toegepast op het eerste knooppunt heet een *queryobject vanaf*, waarvan is niet opgegeven als volgt.

```javascript
/* Starting Query Object:
   Specifies constraints on the starting node
*/
{
    /* "match" operator searches for entities with the specified properties. 
       All properties defined in the graph schema be queried such as "Name" and "NormalizedTitle".
     */
    "match": { 
        "Name" : "some name",
        ...
    },
    /* "id" operator directly specifies the IDs of the starting nodes. 
       When it is present, the "match" operator is ignored. 
     */
    "id": [ id1, id2, id3... ],
    /* "type" operator limits results to a certain type of entities,
       for example, "Author" or "Paper".
     */
    "type": "Author",
    /* "select" operator pulls properties from the database and 
       returns them to the client.
     */
    "select": ["PaperRank", ...]
}
```

Wanneer een pad meer dan alleen een beginknooppunt bevat, zal de queryprocessor uitvoeren met een grafiek-traversal volgen het patroon van het opgegeven pad. Wanneer ze worden ontvangen op een knooppunt de acties van de gebruiker opgegeven traversal wordt geactiveerd, dat wil zeggen, of om te stoppen op het huidige knooppunt en retourneren, of om door te gaan naar het verkennen van de grafiek. Wanneer er is geen actie transport is opgegeven, worden standaardacties worden uitgevoerd. De standaardactie is voor een tussenliggende knooppunt om door te gaan naar het verkennen van de grafiek. De standaardactie is voor het laatste knooppunt van een pad, om te stoppen en te retourneren. Een beperking-object dat traversal acties heet een *Traversal actie voor het Object*. Het is niet opgegeven als volgt:

```javascript
/* Traversal Action Object:
   Specifies graph traversal actions on a node (except for the starting node).
 */
{
    /* Set the continue condition here. */
    continue: { 
        /* simple property exact match */
        "property_key" : "property_value", 
        /* Advanced query operators */
        /* -- Numerical comparisons */
        "property_key_2" : { "gt" /* or simply ">" */ : 123 },
        /* -- Substring query */
        "property_key_3" : { "substring" : "456" },
        /* -- CellID query */
        "id": [ 111, 222, 333... ],
        /* -- Entity type query */
        "type": "cell_type",
        /* -- Property existance query */
        "has" : "property_key_4",
        /* -- Logical operators */
        /* ---- Note that, by default the operators are combined with AND semantics */
        
        /* -- OR operator */
        "or": {
          /* Query operators... */
        },
        /* -- NOT operator */
        "not": {
            /* Query operators... */
        }
    },
    /* Set the return condition here. */
    return: {
        /* Same operators as the continue object */
    },
    /* The selected properties to return. */
    select: ["property_key_1", ...]
}
```

De hoofdtekst van bericht van een *json* zoekquery mag ten minste een *pad* patroon. Bladeren actie objecten zijn optioneel. Hier volgen twee voorbeelden.

```JSON
{
  "path": "/series/ConferenceInstanceIDs/conference/FieldOfStudyIDs/field",
  "series": {
    "type": "ConferenceSeries",
    "FullName": "graph",
    "select": [ "FullName", "ShortName" ]
  },
  "conference": {
    "type": "ConferenceInstance",
    "select": [ "FullName" ]
  },
  "field": {
    "type": "FieldOfStudy",
    "select": [ "Name" ],
    "return": { "Name": { "substring" : "World Wide Web" } }
  }
}
```

```JSON
{
  "path": "/author/PaperIDs/paper",
  "author": {
    "type": "Author",
    "select": [ "DisplayAuthorName" ],
    "match": { "Name": "leslie lamport" }
  },
  "paper": {
    "type": "Paper",
    "select": [ "OriginalTitle", "CitationCount" ],
    "return": { "CitationCount": { "gt": 100 } }
  }
}
```

