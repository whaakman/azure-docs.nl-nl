---
title: Evalueren van uw query's met de functie profiel kan worden uitgevoerd voor de Gremlin-API van Azure Cosmos DB
description: Informatie over het oplossen van problemen en het verbeteren van de Gremlin-query's met behulp van de uitvoering van profiel stap.
services: cosmos-db
author: luisbosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: lbosq
ms.openlocfilehash: 2f3967c64e79b2bc7b01b35eff26f5ac0d4e3db4
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288604"
---
# <a name="how-to-use-the-execution-profile-step-to-evaluate-your-gremlin-queries"></a>Het gebruik van de uitvoering van profiel stap om te evalueren van de Gremlin-query 's

Dit artikel bevat een overzicht van het gebruik van de uitvoering van profiel stap voor Gremlin-API van Azure Cosmos DB graph-databases. Deze stap voorziet in relevante informatie voor het oplossen van problemen en query-optimalisatie, en is compatibel met elke Gremlin-query die kan worden uitgevoerd voor een Gremlin-API voor Cosmos DB-account.

Voor het gebruik van deze stap toe te voegen de `executionProfile()` functieaanroep aan het einde van de Gremlin-query. **De Gremlin-query wordt uitgevoerd** en het resultaat van de bewerking retourneert een JSON-antwoord-object met het profiel van de uitvoering van query.

Bijvoorbeeld:

```java
    // Basic traversal
    g.V('mary').out()

    // Basic traversal with execution profile call
    g.V('mary').out().executionProfile()
```

Na het aanroepen de `executionProfile()` stap, het antwoord is een JSON-object dat de uitgevoerde Gremlin-stap bevat, de totale tijd die nodig was, en een matrix met de Cosmos DB-runtime-operators die de instructie heeft geresulteerd in.

> [!NOTE]
> Deze implementatie voor de uitvoering van profiel is niet gedefinieerd in de Apache Tinkerpop-specificatie. Het is specifiek voor Azure Cosmos DB Gremlin API's-implementatie.


## <a name="response-example"></a>Voorbeeld van de reactie

Hier volgt een voorbeeld van de uitvoer die wordt geretourneerd:

> [!NOTE]
> In dit voorbeeld van aantekeningen is voorzien met opmerkingen waarin wordt uitgelegd van de algemene structuur van het antwoord. Een reactie van de werkelijke executionProfile niet eventuele opmerkingen opgenomen.

```json
[
  {
    // The Gremlin statement that was executed.
    "gremlin": "g.V('mary').out().executionProfile()",

    // Amount of time in milliseconds that the entire operation took.
    "totalTime": 28,

    // An array containing metrics for each of the steps that were executed. Each Gremlin step will translate to one or more of these steps.
    // This list is sorted in order of execution.
    "metrics": [
      {
        // This operation obtains a set of Vertex objects.
        // The metrics include: time, percentTime of total execution time, resultCount, fanoutFactor, count, size (in bytes) and time.
        "name": "GetVertices",
        "time": 24,
        "annotations": {
          "percentTime": 85.71
        },
        "counts": {
          "resultCount": 2
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 2,
            "size": 696,
            "time": 0.4
          }
        ]
      },
      {
        // This operation obtains a set of Edge objects. Depending on the query, these might be directly adjacent to a set of vertices, or separate, in the case of an E() query.
        // The metrics include: time, percentTime of total execution time, resultCount, fanoutFactor, count, size (in bytes) and time.
        "name": "GetEdges",
        "time": 4,
        "annotations": {
          "percentTime": 14.29
        },
        "counts": {
          "resultCount": 1
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 1,
            "size": 419,
            "time": 0.67
          }
        ]
      },
      {
        // This operation obtains the vertices that a set of edges point at.
        // The metrics include: time, percentTime of total execution time and resultCount.
        "name": "GetNeighborVertices",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      },
      {
        // This operation represents the serialization and preparation for a result from the preceding graph operations.
        // The metrics include: time, percentTime of total execution time and resultCount.
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      }
    ]
  }
]
```

> [!NOTE]
> De stap executionProfile wordt de Gremlin-query uitvoeren. Dit omvat de `addV` of `addE`stappen, die zal leiden tot het maken en worden de wijzigingen in de query zijn opgegeven. Als gevolg hiervan de Aanvraageenheden die worden gegenereerd door de Gremlin-query wordt ook in rekening gebracht.

## <a name="execution-profile-response-objects"></a>Uitvoering van profiel antwoordobjecten

Het antwoord van een functie executionProfile() resulteert in een hiërarchie van JSON-objecten met de volgende structuur:
  - **Gremlin-bewerking object**: Hiermee geeft u de gehele Gremlin-bewerking die is uitgevoerd. Bevat de volgende eigenschappen.
    - `gremlin`: De expliciete Gremlin-instructie die is uitgevoerd.
    - `totalTime`: De tijd in milliseconden, die de uitvoering van de stap in rekening gebracht in. 
    - `metrics`: Een matrix met elk van de Cosmos DB-runtime-operators die zijn uitgevoerd om te voldoen aan de query. Deze lijst is gesorteerd in volgorde van de uitvoering.
    
  - **Cosmos DB runtime operators**: Hiermee geeft u elk van de onderdelen van de gehele Gremlin-bewerking. Deze lijst is gesorteerd in volgorde van de uitvoering. Elk object bevat de volgende eigenschappen:
    - `name`: De naam van de operator. Dit is het type van de stap die is geëvalueerd en uitgevoerd. Lees meer in de onderstaande tabel.
    - `time`: Hoeveelheid tijd in milliseconden, dat een bepaalde operator heeft geduurd.
    - `annotations`: Bevat aanvullende informatie die specifiek zijn voor de operator die is uitgevoerd.
    - `annotations.percentTime`: Percentage van de totale tijd die nodig was voor het uitvoeren van de specifieke operator.
    - `counts`: Het aantal objecten dat uit de storage-laag zijn geretourneerd door deze operator. Dit is opgenomen in de `counts.resultCount` scalaire waarde in.
    - `storeOps`: Hiermee geeft u een opslagbewerking die een of meerdere partities kan omvatten.
    - `storeOps.fanoutFactor`: Hiermee geeft u het aantal partities dat deze specifieke opslagbewerking geopend.
    - `storeOps.count`: Hiermee geeft u het aantal resultaten dat deze opslagbewerking geretourneerd.
    - `storeOps.size`: Hiermee geeft u de grootte in bytes van het resultaat van een bepaalde opslagbewerking.

Cosmos DB Gremlin Runtime-Operator|Description
---|---
`GetVertices`| Deze stap verkrijgt een predicated set van objecten van de laag voor gegevenspersistentie. 
`GetEdges`| Deze stap verkrijgt de randen die aan een set met hoekpunten grenzen. Deze stap kan resulteren in een of meer opslag-bewerkingen.
`GetNeighborVertices`| Deze stap verkrijgt de hoekpunten die zijn verbonden met een set met randen. De partitie die de randen met sleutels en id's van zowel de bron- en hoekpunten.
`Coalesce`| Deze stap is verantwoordelijk voor de evaluatie van twee bewerkingen wanneer de `coalesce()` Gremlin stap wordt uitgevoerd.
`CartesianProductOperator`| Deze stap een Cartesisch product tussen twee gegevenssets worden berekend. Gewoonlijk uitgevoerd wanneer de predicaten `to()` of `from()` worden gebruikt.
`ConstantSourceOperator`| Deze stap berekent een expressie voor het produceren van een constante waarde als gevolg hiervan.
`ProjectOperator`| Deze stap wordt voorbereid en serialiseert een antwoord met behulp van het resultaat van vorige activiteiten.
`ProjectAggregation`| Deze stap wordt voorbereid en een antwoord voor een statistische bewerking serialiseert.

> [!NOTE]
> Deze lijst blijft worden bijgewerkt wanneer nieuwe operators worden toegevoegd.

## <a name="examples-on-how-to-analyze-an-execution-profile-response"></a>Voorbeelden van het analyseren van een reactie op uitvoering van profiel

Hier volgen enkele voorbeelden van algemene optimalisaties die kunnen worden gevonden met behulp van het antwoord van de uitvoering van profiel:
  - Blind fanout-query.
  - Ongefilterde query.

### <a name="blind-fan-out-query-patterns"></a>Blind fanout-querypatronen

Wordt ervan uitgegaan dat de volgende uitvoering profiel reactie van een **gepartitioneerde graph**:

```json
[
  {
    "gremlin": "g.V('tt0093640').executionProfile()",
    "totalTime": 46,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 46,
        "annotations": {
          "percentTime": 100
        },
        "counts": {
          "resultCount": 1
        },
        "storeOps": [
          {
            "fanoutFactor": 5,
            "count": 1,
            "size": 589,
            "time": 75.61
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 1
        }
      }
    ]
  }
]
```

De volgende conclusies kunnen worden gemaakt van het:
- De query is een één-ID zoeken, omdat de instructie Gremlin het patroon volgt `g.V('id')`.
- Beoordeling van de `time` metrische gegevens, de latentie van deze query lijkt te hoog zijn omdat het [meer dan 10ms voor één punt lezen bewerking](https://docs.microsoft.com/azure/cosmos-db/introduction#guaranteed-low-latency-at-99th-percentile-worldwide).
- Als u in de `storeOps` -object, zien we dat de `fanoutFactor` is `5`, dit houdt in dat [5 partities](https://docs.microsoft.com/azure/cosmos-db/partition-data) zijn gebruikt door deze bewerking.

Als een sluiting van deze analyse, kunnen we bepalen dat de eerste query toegang heeft tot meer partities dan nodig is. Dit kan worden opgelost door de te nemen partitionerende sleutel op te geven in de query als een predicaat. Dit zal leiden tot minder latentie en minder kosten per query. Meer informatie over [graph-partitionering](graph-partitioning.md). Een meer optimale query zou worden `g.V('tt0093640').has('partitionKey', 't1001')`.

### <a name="unfiltered-query-patterns"></a>Ongefilterde querypatronen

De volgende twee uitvoering profiel antwoorden met elkaar vergelijken. Deze voorbeelden worden gebruikt voor het gemak één gepartitioneerde grafiek.

Deze eerste query haalt alle hoekpunten met het label `tweet` en vervolgens haalt de aangrenzende hoekpunten:

```json
[
  {
    "gremlin": "g.V().hasLabel('tweet').out().executionProfile()",
    "totalTime": 42,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 31,
        "annotations": {
          "percentTime": 73.81
        },
        "counts": {
          "resultCount": 30
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 13,
            "size": 6819,
            "time": 1.02
          }
        ]
      },
      {
        "name": "GetEdges",
        "time": 6,
        "annotations": {
          "percentTime": 14.29
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 20,
            "size": 7950,
            "time": 1.98
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 5,
        "annotations": {
          "percentTime": 11.9
        },
        "counts": {
          "resultCount": 20
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 4,
            "size": 1070,
            "time": 1.19
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 20
        }
      }
    ]
  }
]
```

U ziet het profiel van dezelfde query, maar nu met een extra filter `has('lang', 'en')`, daarna wordt ingegaan op de aaneengesloten hoekpunten:

```json
[
  {
    "gremlin": "g.V().hasLabel('tweet').has('lang', 'en').out().executionProfile()",
    "totalTime": 14,
    "metrics": [
      {
        "name": "GetVertices",
        "time": 14,
        "annotations": {
          "percentTime": 58.33
        },
        "counts": {
          "resultCount": 11
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 11,
            "size": 4807,
            "time": 1.27
          }
        ]
      },
      {
        "name": "GetEdges",
        "time": 5,
        "annotations": {
          "percentTime": 20.83
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 18,
            "size": 7159,
            "time": 1.7
          }
        ]
      },
      {
        "name": "GetNeighborVertices",
        "time": 5,
        "annotations": {
          "percentTime": 20.83
        },
        "counts": {
          "resultCount": 18
        },
        "storeOps": [
          {
            "fanoutFactor": 1,
            "count": 4,
            "size": 1070,
            "time": 1.01
          }
        ]
      },
      {
        "name": "ProjectOperator",
        "time": 0,
        "annotations": {
          "percentTime": 0
        },
        "counts": {
          "resultCount": 18
        }
      }
    ]
  }
]
```

Deze twee query's bereikt hetzelfde resultaat, maar het eerste item wordt vereisen meer Aanvraageenheden omdat nodig is voor het herhalen van een grotere initiële gegevensset voor het uitvoeren van query's de aangrenzende items. We kunnen indicatoren van dit gedrag zien bij het vergelijken van de volgende parameters uit beide antwoorden:
- De `metrics[0].time` waarde hoger is in de eerste antwoord krijgen, waarmee wordt aangegeven dat deze één stap duurde langer om op te lossen.
- De `metrics[0].counts.resultsCount` waarde hoger is ook in het eerste antwoord krijgen, waarmee wordt aangegeven dat de initiële gegevensset in de werkmap groter is.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de [ondersteunde functies Gremlin](gremlin-support.md) in Azure Cosmos DB. 
* Meer informatie over de [Gremlin-API in Azure Cosmos DB](graph-introduction.md).
