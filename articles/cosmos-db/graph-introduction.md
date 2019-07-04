---
title: Inleiding tot de Gremlin-API van Azure Cosmos DB
description: Leer hoe u Azure Cosmos DB kunt gebruiken om zeer grote grafieken met een lage latentie op te slaan, te doorzoeken en erin te bladeren, met behulp van de querytaal voor Gremlin-grafieken van Apache TinkerPop.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 06/25/2019
ms.author: lbosq
ms.openlocfilehash: 126c825106b7844a5fc8a5a3cdbcc7aa6c273b5b
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67502805"
---
# <a name="introduction-to-azure-cosmos-db-gremlin-api"></a>Inleiding tot Azure Cosmos DB: Gremlin-API

[Azure Cosmos DB](introduction.md) is de wereldwijd gedistribueerde, multi-model databaseservice van Microsoft voor essentiële toepassingen. Het is een database met meerdere modellen en biedt ondersteuning voor document, sleutel / waarde-, grafiek en kolommen. De Gremlin-API van Azure Cosmos DB wordt gebruikt voor het opslaan en werken met de graph-gegevens op een volledig beheerde database-omgeving op elke schaal.  

![Azure Cosmos DB-grafiekarchitectuur](./media/graph-introduction/cosmosdb-graph-architecture.png)

Dit artikel biedt een overzicht van de Azure Cosmos DB Gremlin API en legt uit hoe u deze kunt gebruiken om zeer grote grafieken met miljarden hoekpunten en randen op te slaan. U kunt query's uitvoeren in de grafieken met milliseconde en de structuur van de grafiek eenvoudig kunt veranderen. Azure Cosmos DB van Gremlin-API is gebaseerd op de [Apache TinkerPop](https://tinkerpop.apache.org) grafiekdatabase standard en maakt gebruik van de Gremlin-querytaal. 

Gremlin-API van Azure Cosmos-DB combineert de kracht van de graph-database algoritmen met uiterst schaalbare en beheerde infrastructuur om een unieke, flexibele oplossing naar de meest voorkomende problemen die zijn gekoppeld aan het gebrek aan flexibiliteit en relationele benaderingen te bieden. 

## <a name="features-of-azure-cosmos-db-graph-database"></a>Functies van Azure Cosmos DB-graafdatabase
 
Azure Cosmos DB is een volledig beheerde grafiekdatabase met wereldwijde distributie, elastisch schalen van opslag en doorvoer, automatisch indexeren en doorzoeken, instelbare consistentieniveaus, en ondersteuning voor de standaardversie van TinkerPop. 

Hier volgen de verschillende functies die Gremlin-API van Azure Cosmos DB biedt:

* **Elastisch schaalbare doorvoer en opslag**

  Grafieken moeten in de praktijk hoger worden geschaald dan de capaciteit van een enkele server. Azure Cosmos DB ondersteunt horizontaal schaalbare graafdatabase-databases die een vrijwel onbeperkt grootte in termen van en de ingerichte doorvoer kunnen hebben. Als de database grafiekschaal groeit, de gegevens worden automatisch gedistribueerd met behulp van [graph-partitionering](https://docs.microsoft.com/azure/cosmos-db/graph-partitioning).

* **Replicatie voor meerdere regio 's**

  Azure Cosmos DB kunt uw gegevens automatisch repliceren naar een Azure-regio. Replicatie vereenvoudigt de ontwikkeling van toepassingen waarvoor globale toegang tot gegevens. Naast het minimaliseren van leeslatentie, biedt Azure Cosmos DB een regionale failover-mechanisme dat de continuïteit van uw toepassing in het zeldzame geval van een service wordt onderbroken in een regio waarborgen kunt. 

* **Snelle query's en doorkruisingen met de meest algemeen aanvaarde graph-query-standaard**

  Sla heterogene hoekpunten en randen op, en doorzoek deze documenten met behulp van de vertrouwde Gremlin-syntaxis. Gremlin is een imperatieve, functionele querytaal waarmee een rijke interface voor het implementeren van algemene graph-algoritmen. 
  
  Azure Cosmos DB kunt u uitgebreide realtime query's en doorkruisingen zonder de noodzaak om op te geven schemahints, secundaire indexen of weergaven. Meer informatie vindt u in [Querygrafieken met behulp van Gremlin](gremlin-support.md).

* **Volledig beheerde grafiekdatabase**

  Dankzij Azure Cosmos DB is het niet meer nodig om database- en machineresources te beheren. De meeste bestaande graph database-platforms zijn gebonden aan de beperkingen van de infrastructuur en vereisen vaak een hoge mate van onderhoud om te controleren of de werking ervan. 
  
  Als een volledig beheerde Microsoft Azure-service is er hoeft te beheren van virtuele machines, runtime software bijwerken, sharding of replicatie beheren of complexe gegevenslaag upgrades zijn getroffen. Er wordt automatisch een back-up van elke grafiek gemaakt en de databases worden automatisch beveiligd tegen regionale fouten. Deze garanties kunnen ontwikkelaars zich richten op het leveren van de waarde van de toepassing in plaats van het besturingssysteem en het beheren van hun databases. 

* **Automatisch indexeren**

  In Azure Cosmos DB worden standaard alle eigenschappen in de knooppunten en randen van de grafiek geïndexeerd. Er wordt geen schema verwacht of vereist en er hoeven geen secundaire indexen te worden gemaakt. Meer informatie over [indexeren in Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/index-overview). 

* **Compatibiliteit met Apache TinkerPop**

  Azure Cosmos DB ondersteunt het [open-source voor Apache TinkerPop standard](http://tinkerpop.apache.org/). De standaard Tinkerpop heeft een ruime hoeveelheid ecosysteem van toepassingen en -bibliotheken die gemakkelijk kunnen worden geïntegreerd met Gremlin-API van Azure Cosmos DB. 

* **Instelbare consistentieniveaus**

  Selecteer een van de vijf goed gedefinieerde consistentieniveaus voor een optimale balans tussen consistentie en prestaties. Voor query's en leesbewerkingen biedt Azure Cosmos DB vijf verschillende consistentieniveaus: sterk, gebonden-verouderd, sessie, consistent voorvoegsel en mogelijk. Op basis van deze gedetailleerde, goed gedefinieerde consistentieniveaus kunt u een goede balans vinden tussen de consistentie, beschikbaarheid en latentie. Meer informatie over [Instelbare gegevensconsistentieniveaus in Azure Cosmos DB](consistency-levels.md).

## <a name="scenarios-that-can-use-gremlin-api"></a>Scenario's waarin de Gremlin-API kan worden gebruikt
Hier zijn enkele scenario’s waarin ondersteuning voor grafieken in Azure Cosmos DB kan worden gebruikt:

* Sociale netwerken

  Door gegevens over uw klanten en hun interacties met andere personen te combineren kunt u een persoonlijke ervaring ontwikkelen, het gedrag van klanten voorspellen of personen in contact brengen met anderen die gelijksoortige interesses hebben. Azure Cosmos DB kan worden gebruikt om sociale netwerken te beheren en klantvoorkeuren en -gegevens bij te houden.

* Aanbevelingsengines

  Dit scenario wordt doorgaans gebruikt in de detailhandel. Door informatie over producten, gebruikers en interactie tussen gebruikers, zoals het kopen, zoeken of beoordelen van een item, te combineren, kunt u aangepaste aanbevelingen genereren. De lage latentie, elastische schaal en systeemeigen ondersteuning voor grafieken van Azure Cosmos DB is ideaal voor het modelleren van deze interacties.

* Georuimtelijk

  In veel toepassingen voor telecommunicatie, logistiek en reisplanning moet vaak worden gezocht naar belangrijke locaties binnen een gebied, of moet de kortste/snelste route tussen twee locaties worden berekend. Azure Cosmos DB is zeer geschikt voor dit soort taken.

* Internet of Things

  Omdat het netwerk en de verbindingen tussen IoT-apparaten zijn gemodelleerd als een grafiek krijgt u een beter beeld van de status van uw apparaten en activa. U krijgt ook inzicht in hoe wijzigingen in één deel van het netwerk van invloed kunnen zijn op een ander deel.

## <a name="introduction-to-graph-databases"></a>Inleiding tot graph-databases
Gegevens zoals deze in werkelijkheid voorkomen, zijn op natuurlijke wijze verbonden. Traditionele gegevensmodellering richt zich op een afzonderlijk entiteiten definiëren en hun relaties tijdens runtime computing. Hoewel dit model zijn voordelen heeft, kan maximaal verbonden gegevens lastig zijn voor het beheren van onder de beperkingen.  

Een grafiek database benadering is afhankelijk van permanente relaties in de opslaglaag in plaats daarvan die leidt tot bewerkingen voor het ophalen van zeer efficiënt graph. Azure Cosmos DB van Gremlin-API biedt ondersteuning voor de [eigenschap graph model](https://tinkerpop.apache.org/docs/current/reference/#intro).

### <a name="property-graph-objects"></a>De eigenschap graph-objecten

Een eigenschap [graph](http://mathworld.wolfram.com/Graph.html) is een structuur die samengesteld uit [hoekpunten](http://mathworld.wolfram.com/GraphVertex.html) en [randen](http://mathworld.wolfram.com/GraphEdge.html). Beide objecten kunnen een willekeurig aantal sleutel-waardeparen hebben als eigenschappen. 

* **Hoekpunten** -hoekpunten geven discrete entiteiten, zoals een persoon, een plaats of een gebeurtenis.

* **Kanten**: Kanten duiden relaties tussen hoekpunten aan. Bijvoorbeeld, een persoon kent mogelijk een andere persoon, is betrokken bij een gebeurtenis, en is onlangs op een bepaalde locatie geweest. 

* **Eigenschappen**: Eigenschappen geven informatie over de hoekpunten en kanten. Er mag een willekeurig aantal eigenschappen in hoekpunten of randen en ze kunnen worden gebruikt om te beschrijven en de objecten in een query filteren. Van de Voorbeeldeigenschappen zijn een hoekpunt met de naam en de leeftijd of een rand, die een tijdstempel en/of een gewicht kan hebben. 

Graph-databases zijn vaak opgenomen in de NoSQL- of niet-relationele databasecategorie omdat er geen afhankelijkheid van een schema of een beperkte gegevensmodel. Dit gebrek aan schema kunt u modellen en verbonden structuren opslaan op een natuurlijke manier en efficiënt. 

### <a name="gremlin-by-example"></a>Gremlin via een voorbeeld
Laten we een voorbeeldgrafiek gebruiken om te zien hoe query’s kunnen worden uitgedrukt in Gremlin. In de volgende afbeelding ziet u een zakelijke toepassing waarmee gegevens over gebruikers, interesses en apparaten worden beheerd, in de vorm van een grafiek.  

![Voorbeelddatabase met personen, apparaten en interesses](./media/gremlin-support/sample-graph.png) 

Deze grafiek heeft de volgende hoekpunttypen (in Gremlin ‘label’ genaamd):

- Personen: In de grafiek komen drie personen voor: Robin, Thomas en Ben
- Interesses: In dit voorbeeld is hun interesse voetbal
- Apparaten: De apparaten die de personen gebruiken
- Besturingssystemen: De besturingssystemen op de apparaten

We laten de relaties tussen deze entiteiten zien via de volgende randtypen/labels:

- Kent: Bijvoorbeeld 'Thomas kent Robin'
- Is geïnteresseerd in: Geeft de interesses weer van de personen in de grafiek, bijvoorbeeld 'Ben is geïnteresseerd in voetbal'
- RunsOS: Op de laptop wordt het Windows-besturingssysteem uitgevoerd
- Gebruikt: Geeft aan welk apparaat een persoon gebruikt. Robin gebruikt bijvoorbeeld een Motorola-telefoon met serienummer 77

We gaan enkele bewerkingen uitvoeren in deze grafiek met behulp van de [Gremlin-console](https://tinkerpop.apache.org/docs/3.3.2/reference/#gremlin-console). U kunt deze bewerkingen ook uitvoeren met behulp van Gremlin-stuurprogramma’s op een platform naar keuze (Java, Node.js, Python of NET).  Voordat we kijken wat wordt ondersteund in Azure Cosmos DB, bekijken we een aantal voorbeelden om vertrouwd te raken met de syntaxis.

Eerst kijken we naar CRUD. Met de volgende Gremlin-instructie wordt het hoekpunt Thomas ingevoegd in de grafiek:

```java
:> g.addV('person').property('id', 'thomas.1').property('firstName', 'Thomas').property('lastName', 'Andersen').property('age', 44)
```

Vervolgens wordt met de volgende Gremlin-instructie de rand Kent ingevoegd tussen Thomas en Robin.

```java
:> g.V('thomas.1').addE('knows').to(g.V('robin.1'))
```

Met de volgende query worden de hoekpunten voor Persoon in aflopende volgorde van de voornamen geretourneerd:
```java
:> g.V().hasLabel('person').order().by('firstName', decr)
```

Waar grafieken in uitblinken is in het beantwoorden van vragen zoals: Welke besturingssystemen gebruiken de vrienden van Thomas? U kunt deze Gremlin-traversal om op te halen die informatie in de grafiek kunt uitvoeren:

```java
:> g.V('thomas.1').out('knows').out('uses').out('runsos').group().by('name').by(count())
```
Laten we nu kijken wat Azure Cosmos DB te bieden heeft voor Gremlin-ontwikkelaars.

## <a name="next-steps"></a>Volgende stappen
Voor meer informatie over ondersteuning voor grafieken in Azure Cosmos DB raadpleegt u:

* Aan de slag met de [zelfstudie voor grafieken in Azure Cosmos DB](create-graph-dotnet.md).
* Meer informatie over het [doorzoeken van grafieken in Azure Cosmos DB met behulp van Gremlin](gremlin-support.md).
