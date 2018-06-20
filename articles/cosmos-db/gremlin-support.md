---
title: Ondersteuning voor Gremlin in Azure Cosmos DB | Microsoft Docs
description: Meer informatie over de Gremlin-taal van Apache TinkerPop. Leer welke functies en stappen beschikbaar zijn in Azure Cosmos DB
services: cosmos-db
author: LuisBosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: overview
ms.date: 01/02/2018
ms.author: lbosq
ms.openlocfilehash: c675f37e50f5b8a259048d9a92fcdbe5b947068c
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34797614"
---
# <a name="azure-cosmos-db-gremlin-graph-support"></a>Ondersteuning voor Gremlin-grafieken in Azure Cosmos DB
Azure Cosmos DB biedt ondersteuning voor de traversaltaal van [Apache Tinkerpop](http://tinkerpop.apache.org): [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps). Dit is de Graph API voor het maken van grafiekentiteiten en het uitvoeren van querybewerkingen. U kunt de Gremlin-taal gebruiken om grafiekentiteiten (hoekpunten en randen) te maken, eigenschappen binnen deze entiteiten te wijzigen, query’s en traversals uit te voeren, en entiteiten te verwijderen. 

Azure Cosmos DB brengt functies van zakelijk niveau naar grafiekdatabases. Dit omvat wereldwijde distributie, onafhankelijk schalen van opslag en doorvoer, voorspelbare latenties van slechts enkele milliseconden, automatisch indexeren, SLA’s, leesbeschikbaarheid voor databaseaccounts in twee of meer Azure-regio’s. Omdat Azure Cosmos DB ondersteuning biedt voor TinkerPop/Gremlin, kunt u eenvoudig toepassingen migreren die zijn geschreven met een andere grafiekdatabase, zonder dat u codewijzigingen hoeft aan te brengen. Daarnaast integreert Azure Cosmos DB, vanwege de ondersteuning voor Gremlin, naadloos met analyseframeworks waarvoor TinkerPop is ingeschakeld, zoals [Apache Spark GraphX](http://spark.apache.org/graphx/). 

In dit artikel bieden we een overzicht van Gremlin en een opsomming van de functies en stappen in Gremlin die worden ondersteund met de Graph API.

## <a name="gremlin-by-example"></a>Gremlin via een voorbeeld
Laten we een voorbeeldgrafiek gebruiken om te zien hoe query’s kunnen worden uitgedrukt in Gremlin. In de volgende afbeelding ziet u een zakelijke toepassing waarmee gegevens over gebruikers, interesses en apparaten worden beheerd, in de vorm van een grafiek.  

![Voorbeelddatabase met personen, apparaten en interesses](./media/gremlin-support/sample-graph.png) 

Deze grafiek heeft de volgende hoekpunttypen (in Gremlin ‘label’ genaamd):

- Personen: de grafiek heeft drie personen, Robin, Thomas, Ben
- Interesses: in dit voorbeeld is hun interesse football
- Apparaten: de apparaten die de personen gebruiken
- Besturingssystemen: de besturingssystemen op de apparaten

We laten de relaties tussen deze entiteiten zien via de volgende randtypen/labels:

- Kent: bijvoorbeeld ‘Thomas kent Robin’
- Is geïnteresseerd in: om de interesses weer te geven van de personen in de grafiek, bijvoorbeeld ‘Ben is geïnteresseerd in football’
- Voert besturingssysteem uit: op de laptop wordt het Windows-besturingssysteem uitgevoerd
- Gebruikt: om te vertegenwoordigen welk apparaat een persoon gebruikt. Robin gebruikt bijvoorbeeld een Motorola-telefoon met serienummer 77

We gaan enkele bewerkingen uitvoeren in deze grafiek met behulp van de [Gremlin-console](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console). U kunt deze bewerkingen ook uitvoeren met behulp van Gremlin-stuurprogramma’s op een platform naar keuze (Java, Node.js, Python of NET).  Voordat we kijken wat wordt ondersteund in Azure Cosmos DB, bekijken we een aantal voorbeelden om vertrouwd te raken met de syntaxis.

Eerst kijken we naar CRUD. Met de volgende Gremlin-instructie wordt het hoekpunt Thomas ingevoegd in de grafiek:

```
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Vervolgens wordt met de volgende Gremlin-instructie de rand Kent ingevoegd tussen Thomas en Robin.

```
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Met de volgende query worden de hoekpunten voor Persoon in aflopende volgorde van de voornamen geretourneerd:
```
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Waar grafieken in uitblinken is in het beantwoorden van vragen zoals: Welke besturingssystemen gebruiken de vrienden van Thomas? U kunt deze eenvoudige Gremlin-traversal uitvoeren om deze gegevens op te halen uit de grafiek:

```
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Laten we nu kijken wat Azure Cosmos DB te bieden heeft voor Gremlin-ontwikkelaars.

## <a name="gremlin-features"></a>Gremlin-functies
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

Azure Cosmos DB gebruikt de [GraphSON-indeling](https://github.com/thinkaurelius/faunus/wiki/GraphSON-Format) bij het retourneren van resultaten uit Gremlin-bewerkingen. GraphSON is de standaardindeling in Gremlin voor het weergeven van hoekpunten, randen en eigenschappen (eigenschappen met een of meerdere waarden) met behulp van JSON. 

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

In GraphSON worden de volgende eigenschappen gebruikt voor hoekpunten:

| Eigenschap | Beschrijving |
| --- | --- |
| id | De id voor het hoekpunt. Moet uniek zijn (in combinatie met de waarde _partition, indien van toepassing) |
| label | Het label van het hoekpunt. Dit is optioneel en wordt gebruikt om het entiteitstype te beschrijven. |
| type | Wordt gebruikt om hoekpunten te onderscheiden van niet-Graph-documenten |
| properties | Groep door de gebruiker gedefinieerde eigenschappen die zijn gekoppeld aan het hoekpunt. Elke eigenschap kan meerdere waarden hebben. |
| _partition (te configureren) | De partitiesleutel van het hoekpunt. Kan worden gebruikt om grafieken uit te schalen naar meerdere servers |
| outE | Dit bevat een lijst met uitlopende randen vanaf een hoekpunt. Het opslaan van informatie over aangrenzing met het hoekpunt zorgt voor een snelle uitvoering van traversals. Randen zijn gegroepeerd op basis van de bijbehorende labels. |

En de rand bevat de volgende informatie voor hulp bij het navigeren naar andere delen van de grafiek.

| Eigenschap | Beschrijving |
| --- | --- |
| id | De id voor de rand. Moet uniek zijn (in combinatie met de waarde _partition, indien van toepassing) |
| label | Het label van de rand. Deze eigenschap is optioneel en wordt gebruikt om het relatietype te beschrijven. |
| inV | Dit bevat een lijst met inkomende hoekpunten voor een rand. Het opslaan van informatie over aangrenzing aan de rand zorgt voor een snelle uitvoering van traversals. Hoekpunten zijn gegroepeerd op basis van de bijbehorende labels. |
| properties | Groep door de gebruiker gedefinieerde eigenschappen die zijn gekoppeld aan de rand. Elke eigenschap kan meerdere waarden hebben. |

Met elke eigenschap kunnen meerdere waarden worden opgeslagen binnen een matrix. 

| Eigenschap | Beschrijving |
| --- | --- |
| waarde | De waarde van de eigenschap

## <a name="gremlin-steps"></a>Gremlin-stappen
We gaan nu de Gremlin-stappen bekijken die worden ondersteund in Azure Cosmos DB. Zie [TinkerPop-naslagwerken](http://tinkerpop.apache.org/docs/current/reference) voor een compleet overzicht van Gremlin.

| stap | Beschrijving | TinkerPop 3.2-documentatie |
| --- | --- | --- |
| `addE` | Voegt een rand toe tussen twee hoekpunten | [stap: addE](http://tinkerpop.apache.org/docs/current/reference/#addedge-step) |
| `addV` | Voegt een hoekpunt toe aan de grafiek | [stap: addV](http://tinkerpop.apache.org/docs/current/reference/#addvertex-step) |
| `and` | Zorgt ervoor dat alle traversals een waarde retourneren | [stap: and](http://tinkerpop.apache.org/docs/current/reference/#and-step) |
| `as` | Een stapmodulator om een variabele toe te wijzen aan de uitvoer van een stap | [stap: as](http://tinkerpop.apache.org/docs/current/reference/#as-step) |
| `by` | Een stapmodulator die wordt gebruikt met `group` en `order` | [stap: by](http://tinkerpop.apache.org/docs/current/reference/#by-step) |
| `coalesce` | Retourneert de eerste traversal die wordt geretourneerd als resultaat | [stap: coalesce](http://tinkerpop.apache.org/docs/current/reference/#coalesce-step) |
| `constant` | Retourneert een constante waarde. Wordt gebruikt met `coalesce`| [stap: constant](http://tinkerpop.apache.org/docs/current/reference/#constant-step) |
| `count` | Retourneert het aantal van de traversal | [stap: count](http://tinkerpop.apache.org/docs/current/reference/#count-step) |
| `dedup` | Retourneert de waarden, zonder de dubbele waarden | [stap: dedup](http://tinkerpop.apache.org/docs/current/reference/#dedup-step) |
| `drop` | Verwijdert de waarden (hoekpunt/rand) | [stap: drop](http://tinkerpop.apache.org/docs/current/reference/#drop-step) |
| `fold` | Fungeert als barrière waarmee de combinatie van resultaten wordt berekend| [stap: fold](http://tinkerpop.apache.org/docs/current/reference/#fold-step) |
| `group` | Groepeert de waarden op basis van de opgegeven labels| [stap: group](http://tinkerpop.apache.org/docs/current/reference/#group-step) |
| `has` | Wordt gebruikt om eigenschappen, hoekpunten en randen te filteren. Biedt ondersteuning voor de varianten `hasLabel`, `hasId`, `hasNot` en `has`. | [stap: has](http://tinkerpop.apache.org/docs/current/reference/#has-step) |
| `inject` | Waarden invoeren in een stroom| [stap: inject](http://tinkerpop.apache.org/docs/current/reference/#inject-step) |
| `is` | Wordt gebruikt om een filter uit te voeren met behulp van een booleaanse expressie | [stap: is](http://tinkerpop.apache.org/docs/current/reference/#is-step) |
| `limit` | Wordt gebruikt om het aantal items in de traversal te beperken| [stap: limit](http://tinkerpop.apache.org/docs/current/reference/#limit-step) |
| `local` | Verpakt een gedeelte van een traversal lokaal in, vergelijkbaar met een subquery | [stap: local](http://tinkerpop.apache.org/docs/current/reference/#local-step) |
| `not` | Wordt gebruikt om de ontkenning van een filter te produceren | [stap: not](http://tinkerpop.apache.org/docs/current/reference/#not-step) |
| `optional` | Retourneert het resultaat van de opgegeven traversal, indien er een resultaat is, anders wordt het aanroepende element geretourneerd | [stap: optional](http://tinkerpop.apache.org/docs/current/reference/#optional-step) |
| `or` | Zorgt ervoor dat minstens een van de traversals een waarde retourneert | [stap: or](http://tinkerpop.apache.org/docs/current/reference/#or-step) |
| `order` | Retourneert resultaten in de opgegeven sorteervolgorde | [stap: order](http://tinkerpop.apache.org/docs/current/reference/#order-step) |
| `path` | Retourneert het volledige pad van de traversal | [stap: path](http://tinkerpop.apache.org/docs/current/reference/#path-step) |
| `project` | Projecteert de eigenschappen als een kaart | [stap: project](http://tinkerpop.apache.org/docs/current/reference/#project-step) |
| `properties` | Retourneert de eigenschappen voor de opgegeven labels | [stap: properties](http://tinkerpop.apache.org/docs/current/reference/#properties-step) |
| `range` | Filtert op het opgegeven waardenbereik| [stap: range](http://tinkerpop.apache.org/docs/current/reference/#range-step) |
| `repeat` | Herhaalt de stap zo vaak als is opgegeven. Wordt gebruikt om een lus te maken | [stap: repeat](http://tinkerpop.apache.org/docs/current/reference/#repeat-step) |
| `sample` | Wordt gebruikt voor voorbeeldresultaten van de traversal | [stap: sample](http://tinkerpop.apache.org/docs/current/reference/#sample-step) |
| `select` | Wordt gebruikt voor projectresultaten van de traversal |  [stap: select](http://tinkerpop.apache.org/docs/current/reference/#select-step) | |
| `store` | Wordt gebruikt voor niet-blokkerende combinaties van de traversal | [stap: store](http://tinkerpop.apache.org/docs/current/reference/#store-step) |
| `tree` | Paden van een hoekpunt combineren in een boomstructuur | [stap: tree](http://tinkerpop.apache.org/docs/current/reference/#tree-step) |
| `unfold` | Een iterator uitvoeren als stap| [stap: unfold](http://tinkerpop.apache.org/docs/current/reference/#unfold-step) |
| `union` | Resultaten van meerdere traversals samenvoegen| [stap: union](http://tinkerpop.apache.org/docs/current/reference/#union-step) |
| `V` | Bevat de stappen die nodig zijn voor traversals tussen hoekpunten en randen `V`, `E`, `out`, `in`, `both`, `outE`, `inE`, `bothE`, `outV`, `inV`, `bothV` en `otherV` voor | [stap: vertex](http://tinkerpop.apache.org/docs/current/reference/#vertex-steps) |
| `where` | Wordt gebruikt om resultaten van de traversal te filteren. Biedt ondersteuning voor de operators `eq`, `neq`, `lt`, `lte`, `gt`, `gte` en `between`  | [stap: where](http://tinkerpop.apache.org/docs/current/reference/#where-step) |

De voor schrijven geoptimaliseerde engine van Azure Cosmos DB biedt standaard ondersteuning voor het automatisch indexeren van alle eigenschappen binnen hoekpunten en randen. Daarom worden query’s met filters, bereikquery’s, sorteringen of combinaties van elke willekeurige eigenschap verwerkt vanaf de index en efficiënt geleverd. Bekijk onze paper over [schema-agnostisch indexeren](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) voor meer informatie over hoe indexeren werkt in Azure Cosmos DB.

## <a name="next-steps"></a>Volgende stappen
* Aan de slag met het bouwen van een grafiektoepassing [met behulp van onze SDK’s](create-graph-dotnet.md) 
* Meer informatie over [ondersteuning voor grafieken](graph-introduction.md) in Azure Cosmos DB
