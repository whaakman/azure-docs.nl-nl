---
title: JSON-zoeksyntaxis in de Academic Knowledge API | Microsoft Docs
description: Meer informatie over de syntaxis van de JSON-zoekopdracht die kunt u in de Academic Knowledge API in cognitieve Microsoft-Services.
services: cognitive-services
author: alch-msft
manager: kuansanw
ms.service: cognitive-services
ms.component: academic-knowledge
ms.topic: article
ms.date: 03/23/2017
ms.author: alch
ms.openlocfilehash: a4b9cf535dae60258d71c43bba6f9eec1444bd41
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344402"
---
# <a name="json-search-syntax"></a>JSON-Zoeksyntaxis

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

De knooppuntnamen in een query-pad (_v0, v1,..._ ) fungeren als knooppunt-id's die kunnen worden verwezen in de queryobject. de namen van de rand (_e0, e1,..._ ) vertegenwoordigen de typen van de bijbehorende randen in het pad. We kunnen ook een sterretje gebruiken _*_ als de naam van een knooppunt of rand (met uitzondering van het eerste knooppunt, die moet worden gegeven) om te declareren die er zijn geen beperkingen voor dergelijke een element. Bijvoorbeeld, een query pad `/v0/*/v1/e1/*/` paden opgehaald uit de grafiek zonder het beperken van het type van de rand _(v0 v1)_. Ondertussen heeft de query geen beperkingen op de doelhost (het laatste knooppunt) van het pad op.

Wanneer een pad slechts één knooppunt bevat, spreken _v0_, retourneert de query gewoon alle entiteiten die voldoet aan de beperkingen. Een beperking-object dat is toegepast op het eerste knooppunt heet een *queryobject starten*, waarvan is niet opgegeven als volgt.

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

Wanneer een pad meer dan alleen het beginknooppunt bevat, zal de queryprocessor een grafiek traversal na het opgegeven pad patroon uitvoeren. Wanneer het aankomt op een knooppunt, de acties van de gebruiker opgegeven traversal wordt geactiveerd, dat wil zeggen, of u wilt stoppen op het huidige knooppunt en retourneren of moet worden doorgegaan met het verkennen van de grafiek. Wanneer er geen actie traversal wordt opgegeven, wordt standaardacties worden genomen. De standaardactie is voor een tussenliggende knooppunt om door te gaan verkennen van de grafiek. De standaardactie is voor het laatste knooppunt van een pad om te stoppen en te retourneren. Wordt aangeroepen met een beperking-object die traversal acties opgeeft die een *Traversal actie-Object*. De specificatie krijgt als volgt:

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

De POST-hoofdtekst van een *json* zoekquery mag ten minste een *pad* patroon. Bladeren action-objecten zijn optioneel. Hier vindt u twee voorbeelden.

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

