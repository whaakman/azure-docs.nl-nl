---
title: Ondersteuning voor Gremlin in Azure Cosmos DB
description: Meer informatie over de Gremlin-taal van Apache TinkerPop. Leer welke functies en stappen beschikbaar zijn in Azure Cosmos DB
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 06/24/2019
ms.author: lbosq
ms.openlocfilehash: db263c1c7f0a8b87b315c5aa6da31336229c9643
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502727"
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Ondersteuning voor Gremlin-grafieken in Azure Cosmos DB
Azure Cosmos DB ondersteunt [van Apache Tinkerpop](https://tinkerpop.apache.org) grafiek-traversal taal, ook wel [Gremlin](https://tinkerpop.apache.org/docs/3.3.2/reference/#graph-traversal-steps). U kunt de Gremlin-taal gebruiken om grafiekentiteiten (hoekpunten en randen) te maken, eigenschappen binnen deze entiteiten te wijzigen, query’s en traversals uit te voeren, en entiteiten te verwijderen. 

In dit artikel, we bieden een snel overzicht van Gremlin en inventariseren van de Gremlin-functies die worden ondersteund door de Gremlin-API.

## <a name="compatible-client-libraries"></a>Compatibel-clientbibliotheken

In de volgende tabel ziet u populaire Gremlin-stuurprogramma’s die u kunt gebruiken met Azure Cosmos DB:

| Downloaden | source | Aan de slag | Ondersteunde connectorversie |
| --- | --- | --- | --- |
| [.NET](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-DotNet) | [Gremlin.NET on GitHub](https://github.com/apache/tinkerpop/tree/master/gremlin-dotnet) | [Grafiek maken met behulp van .NET](create-graph-dotnet.md) | 3.4.0-RC2 |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) | [Gremlin JavaDoc](https://tinkerpop.apache.org/javadocs/current/full/) | [Grafiek maken met behulp van Java](create-graph-java.md) | 3.2.0+ |
| [Node.js](https://www.npmjs.com/package/gremlin) | [Gremlin-JavaScript in GitHub](https://github.com/jbmusso/gremlin-javascript) | [Grafiek maken met behulp van Node.js](create-graph-nodejs.md) | 3.3.4+ |
| [Python](https://tinkerpop.apache.org/docs/3.3.1/reference/#gremlin-python) | [Gremlin-Python in GitHub](https://github.com/apache/tinkerpop/tree/master/gremlin-python) | [Grafiek maken met behulp van Python](create-graph-python.md) | 3.2.7 |
| [PHP](https://packagist.org/packages/brightzone/gremlin-php) | [Gremlin-PHP in GitHub](https://github.com/PommeVerte/gremlin-php) | [Grafiek maken met behulp van PHP](create-graph-php.md) | 3.1.0 |
| [Gremlin-console](https://tinkerpop.apache.org/downloads.html) | [TinkerPop-documenten](https://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |  [Grafiek maken met behulp van de Gremlin-console](create-graph-gremlin-console.md) | 3.2.0 + |

## <a name="supported-graph-objects"></a>Graph-objecten ondersteund
TinkerPop is een standaard die een breed bereik aan grafiektechnologieën beslaat. Daarom beschikt TinkerPop over een standaardterminologie om te beschrijven welke functies worden aangeboden door een grafiekprovider. Azure Cosmos DB biedt een permanente, beschrijfbare grafiekdatabase met een hoge gelijktijdigheid, die kan worden gepartitioneerd op meerdere servers of clusters. 

In de volgende tabel worden de TinkerPop-functies vermeld die zijn geïmplementeerd met Azure Cosmos DB: 

| Category | Azure Cosmos DB-implementatie |  Opmerkingen | 
| --- | --- | --- |
| Graph-functies | Biedt persistentie en ConcurrentAccess. Ontworpen om transacties te ondersteunen | Computermethoden kunnen worden geïmplementeerd via de Spark-connector. |
| Variabele functies | Biedt ondersteuning voor Boolean, Integer, Byte, Double, Float, Integer, Long, String | Biedt ondersteuning voor primitieve typen, is compatibel met complexe typen via een gegevensmodel |
| Hoekpuntfuncties | Biedt ondersteuning voor RemoveVertices, MetaProperties, AddVertices, MultiProperties, StringIds, UserSuppliedIds, AddProperty, RemoveProperty  | Biedt ondersteuning voor het maken, wijzigen en verwijderen van hoekpunten |
| Functies voor hoekpunteigenschappen | StringIds, UserSuppliedIds, AddProperty, RemoveProperty, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Biedt ondersteuning voor het maken, wijzigen en verwijderen van hoekpunteigenschappen |
| Randfuncties | AddEdges, RemoveEdges, StringIds, UserSuppliedIds, AddProperty, RemoveProperty | Biedt ondersteuning voor het maken, wijzigen en verwijderen van randen |
| Functies voor randeigenschappen | Properties, BooleanValues, ByteValues, DoubleValues, FloatValues, IntegerValues, LongValues, StringValues | Biedt ondersteuning voor het maken, wijzigen en verwijderen van randeigenschappen |

## <a name="gremlin-wire-format-graphson"></a>Gremlin-draadindeling: GraphSON

Azure Cosmos DB gebruikt de [GraphSON-indeling](https://tinkerpop.apache.org/docs/3.3.2/reference/#graphson-reader-writer) bij het retourneren van resultaten uit Gremlin-bewerkingen. GraphSON is de standaardindeling in Gremlin voor het weergeven van hoekpunten, randen en eigenschappen (eigenschappen met een of meerdere waarden) met behulp van JSON. 

In het volgende fragment wordt bijvoorbeeld een GraphSON-representatie weergegeven van een hoekpunt *dat is geretourneerd naar de client* vanuit Azure Cosmos DB. 

```json
  {
    "id": "a7111ba7-0ea1-43c9-b6b2-efc5e3aea4c0",
    "label": "person",
    "type": "vertex",
    "outE": {
      "knows": [
        {
          "id": "3ee53a60-c561-4c5e-9a9f-9c7924bc9aef",
          "inV": "04779300-1c8e-489d-9493-50fd1325a658"
        },
        {
          "id": "21984248-ee9e-43a8-a7f6-30642bc14609",
          "inV": "a8e3e741-2ef7-4c01-b7c8-199f8e43e3bc"
        }
      ]
    },
    "properties": {
      "firstName": [
        {
          "value": "Thomas"
        }
      ],
      "lastName": [
        {
          "value": "Andersen"
        }
      ],
      "age": [
        {
          "value": 45
        }
      ]
    }
  }
```

De eigenschappen die worden gebruikt door GraphSON voor hoekpunten worden hieronder beschreven:

| Eigenschap | Description | 
| --- | --- | --- |
| `id` | De id voor het hoekpunt. Moet uniek zijn (in combinatie met de waarde van `_partition` indien van toepassing). Als er geen waarde is opgegeven, zal deze automatisch worden geleverd met een GUID | 
| `label` | Het label van het hoekpunt. Deze eigenschap wordt gebruikt om te beschrijven van het entiteitstype. |
| `type` | Wordt gebruikt om hoekpunten te onderscheiden van niet-Graph-documenten |
| `properties` | Groep door de gebruiker gedefinieerde eigenschappen die zijn gekoppeld aan het hoekpunt. Elke eigenschap kan meerdere waarden hebben. |
| `_partition` | De partitiesleutel van het hoekpunt. Gebruikt voor [graph-partitionering](graph-partitioning.md). |
| `outE` | Deze eigenschap bevat een lijst van de randen van een hoekpunt. Het opslaan van informatie over aangrenzing met het hoekpunt zorgt voor een snelle uitvoering van traversals. Randen zijn gegroepeerd op basis van de bijbehorende labels. |

En de rand bevat de volgende informatie voor hulp bij het navigeren naar andere delen van de grafiek.

| Eigenschap | Description |
| --- | --- |
| `id` | De id voor de rand. Moet uniek zijn (in combinatie met de waarde van `_partition` indien van toepassing) |
| `label` | Het label van de rand. Deze eigenschap is optioneel en wordt gebruikt om het relatietype te beschrijven. |
| `inV` | Deze eigenschap bevat een lijst in hoekpunten voor een edge. Het opslaan van informatie over aangrenzing aan de rand zorgt voor een snelle uitvoering van traversals. Hoekpunten zijn gegroepeerd op basis van de bijbehorende labels. |
| `properties` | Groep door de gebruiker gedefinieerde eigenschappen die zijn gekoppeld aan de rand. Elke eigenschap kan meerdere waarden hebben. |

Met elke eigenschap kunnen meerdere waarden worden opgeslagen binnen een matrix. 

| Eigenschap | Description |
| --- | --- |
| `value` | De waarde van de eigenschap

## <a name="gremlin-steps"></a>Gremlin-stappen
We gaan nu de Gremlin-stappen bekijken die worden ondersteund in Azure Cosmos DB. Zie [TinkerPop-naslagwerken](https://tinkerpop.apache.org/docs/3.3.2/reference) voor een compleet overzicht van Gremlin.

| stap | Description | TinkerPop 3.2-documentatie |
| --- | --- | --- |
| `addE` | Voegt een rand toe tussen twee hoekpunten | [stap: addE](https://tinkerpop.apache.org/docs/3.3.2/reference/#addedge-step) |
| `addV` | Voegt een hoekpunt toe aan de grafiek | [stap: addV](https://tinkerpop.apache.org/docs/3.3.2/reference/#addvertex-step) |
| `and` | Zorgt ervoor dat alle traversals een waarde retourneren | [stap: and](https://tinkerpop.apache.org/docs/3.3.2/reference/#and-step) |
| `as` | Een stapmodulator om een variabele toe te wijzen aan de uitvoer van een stap | [stap: as](https://tinkerpop.apache.org/docs/3.3.2/reference/#as-step) |
| `by` | Een stapmodulator die wordt gebruikt met `group` en `order` | [stap: by](https://tinkerpop.apache.org/docs/3.3.2/reference/#by-step) |
| `coalesce` | Retourneert de eerste traversal die wordt geretourneerd als resultaat | [stap: coalesce](https://tinkerpop.apache.org/docs/3.3.2/reference/#coalesce-step) |
| `constant` | Retourneert een constante waarde. Wordt gebruikt met `coalesce`| [stap: constant](https://tinkerpop.apache.org/docs/3.3.2/reference/#constant-step) |
| `count` | Retourneert het aantal van de traversal | [stap: count](https://tinkerpop.apache.org/docs/3.3.2/reference/#count-step) |
| `dedup` | Retourneert de waarden, zonder de dubbele waarden | [stap: dedup](https://tinkerpop.apache.org/docs/3.3.2/reference/#dedup-step) |
| `drop` | Verwijdert de waarden (hoekpunt/rand) | [stap: drop](https://tinkerpop.apache.org/docs/3.3.2/reference/#drop-step) |
| `executionProfile` | Hiermee maakt u een beschrijving van alle bewerkingen die worden gegenereerd door de uitgevoerde Gremlin-stap | [executionProfile stap](graph-execution-profile.md) |
| `fold` | Fungeert als barrière waarmee de combinatie van resultaten wordt berekend| [stap: fold](https://tinkerpop.apache.org/docs/3.3.2/reference/#fold-step) |
| `group` | Groepeert de waarden op basis van de opgegeven labels| [stap: group](https://tinkerpop.apache.org/docs/3.3.2/reference/#group-step) |
| `has` | Wordt gebruikt om eigenschappen, hoekpunten en randen te filteren. Biedt ondersteuning voor de varianten `hasLabel`, `hasId`, `hasNot` en `has`. | [stap: has](https://tinkerpop.apache.org/docs/3.3.2/reference/#has-step) |
| `inject` | Waarden invoeren in een stroom| [stap: inject](https://tinkerpop.apache.org/docs/3.3.2/reference/#inject-step) |
| `is` | Wordt gebruikt om een filter uit te voeren met behulp van een booleaanse expressie | [stap: is](https://tinkerpop.apache.org/docs/3.3.2/reference/#is-step) |
| `limit` | Wordt gebruikt om het aantal items in de traversal te beperken| [stap: limit](https://tinkerpop.apache.org/docs/3.3.2/reference/#limit-step) |
| `local` | Verpakt een gedeelte van een traversal lokaal in, vergelijkbaar met een subquery | [stap: local](https://tinkerpop.apache.org/docs/3.3.2/reference/#local-step) |
| `not` | Wordt gebruikt om de ontkenning van een filter te produceren | [stap: not](https://tinkerpop.apache.org/docs/3.3.2/reference/#not-step) |
| `optional` | Retourneert het resultaat van de opgegeven traversal, indien er een resultaat is, anders wordt het aanroepende element geretourneerd | [stap: optional](https://tinkerpop.apache.org/docs/3.3.2/reference/#optional-step) |
| `or` | Zorgt ervoor dat minstens een van de traversals een waarde retourneert | [stap: or](https://tinkerpop.apache.org/docs/3.3.2/reference/#or-step) |
| `order` | Retourneert resultaten in de opgegeven sorteervolgorde | [stap: order](https://tinkerpop.apache.org/docs/3.3.2/reference/#order-step) |
| `path` | Retourneert het volledige pad van de traversal | [stap: path](https://tinkerpop.apache.org/docs/3.3.2/reference/#path-step) |
| `project` | Projecteert de eigenschappen als een kaart | [stap: project](https://tinkerpop.apache.org/docs/3.3.2/reference/#project-step) |
| `properties` | Retourneert de eigenschappen voor de opgegeven labels | [stap: properties](https://tinkerpop.apache.org/docs/3.3.2/reference/#_properties_step) |
| `range` | Filtert op het opgegeven waardenbereik| [stap: range](https://tinkerpop.apache.org/docs/3.3.2/reference/#range-step) |
| `repeat` | Herhaalt de stap zo vaak als is opgegeven. Wordt gebruikt om een lus te maken | [stap: repeat](https://tinkerpop.apache.org/docs/3.3.2/reference/#repeat-step) |
| `sample` | Wordt gebruikt voor voorbeeldresultaten van de traversal | [stap: sample](https://tinkerpop.apache.org/docs/3.3.2/reference/#sample-step) |
| `select` | Wordt gebruikt voor projectresultaten van de traversal |  [stap: select](https://tinkerpop.apache.org/docs/3.3.2/reference/#select-step) |
| `store` | Wordt gebruikt voor niet-blokkerende combinaties van de traversal | [stap: store](https://tinkerpop.apache.org/docs/3.3.2/reference/#store-step) |
| `TextP.startingWith(string)` | Filters gebruiken om de functie een tekenreeks. Deze functie wordt gebruikt als een predicaat voor de `has()` stap zodat deze overeenkomt met een eigenschap met het begin van een opgegeven tekenreeks | [TextP predicaten](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.endingWith(string)` |  Filters gebruiken om de functie een tekenreeks. Deze functie wordt gebruikt als een predicaat voor de `has()` stap zodat deze overeenkomt met een eigenschap met de beëindiging van een opgegeven tekenreeks | [TextP predicaten](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.containing(string)` | Filters gebruiken om de functie een tekenreeks. Deze functie wordt gebruikt als een predicaat voor de `has()` stap zodat deze overeenkomt met een eigenschap met de inhoud van een opgegeven tekenreeks | [TextP predicaten](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.notStartingWith(string)` | Filters gebruiken om de functie een tekenreeks. Deze functie wordt gebruikt als een predicaat voor de `has()` stap zodat deze overeenkomt met een eigenschap die niet wordt gestart door een opgegeven tekenreeks | [TextP predicaten](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.notEndingWith(string)` | Filters gebruiken om de functie een tekenreeks. Deze functie wordt gebruikt als een predicaat voor de `has()` stap zodat deze overeenkomt met een eigenschap die niet met een opgegeven tekenreeks eindigt | [TextP predicaten](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `TextP.notContaining(string)` | Filters gebruiken om de functie een tekenreeks. Deze functie wordt gebruikt als een predicaat voor de `has()` stap zodat deze overeenkomt met een eigenschap die een opgegeven tekenreeks bevat geen | [TextP predicaten](https://tinkerpop.apache.org/docs/3.4.0/reference/#a-note-on-predicates) |
| `tree` | Paden van een hoekpunt combineren in een boomstructuur | [stap: tree](https://tinkerpop.apache.org/docs/3.3.2/reference/#tree-step) |
| `unfold` | Een iterator uitvoeren als stap| [stap: unfold](https://tinkerpop.apache.org/docs/3.3.2/reference/#unfold-step) |
| `union` | Resultaten van meerdere traversals samenvoegen| [stap: union](https://tinkerpop.apache.org/docs/3.3.2/reference/#union-step) |
| `V` | Bevat de stappen die nodig zijn voor traversals tussen hoekpunten en randen `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV` en `otherV` voor | [stap: vertex](https://tinkerpop.apache.org/docs/3.3.2/reference/#vertex-steps) |
| `where` | Wordt gebruikt om resultaten van de traversal te filteren. Biedt ondersteuning voor de operators `eq`, `neq`, `lt`, `lte`, `gt`, `gte` en `between`  | [stap: where](https://tinkerpop.apache.org/docs/3.3.2/reference/#where-step) |

De voor schrijven geoptimaliseerde engine van Azure Cosmos DB biedt standaard ondersteuning voor het automatisch indexeren van alle eigenschappen binnen hoekpunten en randen. Daarom worden query’s met filters, bereikquery’s, sorteringen of combinaties van elke willekeurige eigenschap verwerkt vanaf de index en efficiënt geleverd. Bekijk onze paper over [schema-agnostisch indexeren](https://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) voor meer informatie over hoe indexeren werkt in Azure Cosmos DB.

## <a name="next-steps"></a>Volgende stappen
* Aan de slag met het bouwen van een grafiektoepassing [met behulp van onze SDK’s](create-graph-dotnet.md) 
* Meer informatie over [ondersteuning voor grafieken](graph-introduction.md) in Azure Cosmos DB
