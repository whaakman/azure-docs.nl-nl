---
title: Inleiding tot Azure Cosmos DB Graph API's | Microsoft Docs
description: Meer informatie over hoe u Azure Cosmos DB op te slaan, query, en bladeren door het grote grafieken met een lage latentie met behulp van de Gremlin graph-querytaal van Apache TinkerPop kunt gebruiken.
services: cosmos-db
author: luisbosquez
documentationcenter: 
ms.assetid: b916644c-4f28-4964-95fe-681faa6d6e08
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/05/2017
ms.author: lbosq
ms.openlocfilehash: 14921dbeb0b670e4ec29a224caca07da12bfb82b
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="introduction-to-azure-cosmos-db-graph-api"></a>Inleiding tot Azure Cosmos DB: Graph API

[Azure Cosmos DB](introduction.md) is de globaal gedistribueerd, multimodel databaseservice van Microsoft voor kritieke toepassingen. Azure Cosmos DB biedt de volgende functies die worden ondersteund door [toonaangevende Sla's](https://azure.microsoft.com/support/legal/sla/cosmos-db/):

* [Kant-en-klare wereldwijde distributie](distribute-data-globally.md)
* [Elastisch schalen van doorvoer en opslag](partition-data.md) overal ter wereld
* Één cijfer milliseconde latenties op het 99th percentiel
* [Vijf goed gedefinieerde consistentieniveaus](consistency-levels.md)
* Hoge beschikbaarheid gegarandeerd 

Met Azure Cosmos DB worden [gegevens automatisch geïndexeerd](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), zonder dat u te maken krijgt met schema- en indexbeheer. Het is multimodel en ondersteunt document, sleutel / waarde-grafiek en kolomgegevens modellen.

U wordt aangeraden dat u de volgende video bekijken, waarbij Kirill Gavrylyuk wordt uitgelegd hoe u aan de slag met grafieken voor Azure Cosmos DB:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Graphs-with-Azure-Cosmos-DB-Gremlin-API/player]
> 
> 

Azure Cosmos DB Graph API biedt:

- Een grafiek weergegeven modelleren.
- Verandering API's.
- Klare globale distributie.
- Elastisch schalen van opslag en doorvoer met minder dan 10 ms lezen latenties en minder dan 15 ms op het 99th percentiel.
- Automatisch met de beschikbaarheid van de directe query te indexeren.
- Instelbare consistentieniveaus.
- Uitgebreide Sla's, met inbegrip van 99,99% beschikbaarheid SLA voor alle één regio en alle meerdere landen/regio-accounts met beperkte consistentie en 99,999% beschikbaarheid voor alle accounts voor meerdere landen/regio-database te lezen.

Om te vragen Azure Cosmos DB, kunt u de [Apache TinkerPop](http://tinkerpop.apache.org) graph traversal taal, [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), of andere TinkerPop-compatibele grafiek systemen, zoals [Apache Spark GraphX](spark-connector-graph.md).

Dit artikel biedt een overzicht van Azure Cosmos DB Graph API en wordt uitgelegd hoe u het kunt gebruiken voor het opslaan van grote grafieken miljarden hoekpunten en randen. U kunt query's uitvoeren in de grafieken met milliseconde latentie en eenvoudig ontwikkelen van de structuur van de grafiek en het schema.

## <a name="graph-database"></a>Grafiek-database
Gegevens is zoals deze wordt weergegeven in de praktijk natuurlijk verbonden. Er is een traditionele gegevens modelleren gericht op entiteiten. Voor veel toepassingen hoeft ook een model of model is zowel entiteiten en relaties natuurlijk.

Een [grafiek](http://mathworld.wolfram.com/Graph.html) is een structuur die bestaat uit [hoekpunten](http://mathworld.wolfram.com/GraphVertex.html) en [randen](http://mathworld.wolfram.com/GraphEdge.html). Zowel hoekpunten en randen kunnen een willekeurig aantal eigenschappen hebben. Hoekpunten geven aan afzonderlijke objecten, zoals een persoon, een plaats of een gebeurtenis. Randen geven relaties tussen hoekpunten. Bijvoorbeeld, een persoon mogelijk weet iemand anders, worden betrokken bij een gebeurtenis en onlangs op een locatie. Eigenschappen van snelle informatie over de hoekpunten en randen. Van de Voorbeeldeigenschappen omvatten een hoekpunt met een naam, leeftijd en rand met een tijdstempel en/of een gewicht. Meer formeel dit model wordt ook wel een [eigenschappengrafiek](http://tinkerpop.apache.org/docs/current/reference/#intro). Het model van de grafiek eigenschap biedt ondersteuning voor Azure Cosmos-DB.

Het volgende voorbeeld-grafiek ziet u bijvoorbeeld relaties tussen gebruikers, mobiele apparaten, interesses en besturingssystemen:

![Voorbeelddatabase met personen, apparaten en interesses](./media/graph-introduction/sample-graph.png)

Grafieken zijn handig voor het begrijpen van een breed scala aan gegevenssets in de wetenschap, technologie en bedrijven. Grafiek-databases kunnen u model en opslaan van grafieken natuurlijk en efficiënt, waardoor ze handig voor veel scenario's. Grafiek-databases zijn meestal NoSQL-databases, omdat deze ook vaak nodig schemaflexibiliteit en snelle herhaling gevallen.

Grafieken bieden een nieuwe en krachtige gegevens techniek modelleren. Maar dit feit zelf is niet voldoende reden voor het gebruiken van een database van de grafiek. Voor veel gebruiksvoorbeelden en patronen die betrekking hebben op grafiek traversals grafieken leveren betere prestaties dan traditionele SQL en NoSQL-databases met orde van grootte. Dit verschil in prestaties is verder versterkt wanneer meer dan één relatie, zoals vriend van een vriend passeert.

U kunt de snelle traversals waarmee grafiek databases combineren met grafiek algoritmen, zoals Diepte eerste zoeken, brede eerste zoeken en van Dijkstra-algoritme voor het oplossen van problemen in verschillende domeinen zoals sociale netwerken, inhoudsbeheer georuimtelijke, en aanbevelingen.

## <a name="planet-scale-graphs-with-azure-cosmos-db"></a>Wereld scale grafieken met Azure Cosmos-DB
Azure Cosmos-database is een volledig beheerde grafiek database biedt wereldwijde distributie elastisch schalen van opslag en doorvoer, automatisch te indexeren en query instelbare consistentieniveaus en ondersteuning voor de standaard TinkerPop.

![Architectuur van Azure DB Cosmos-grafiek](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos DB biedt de volgende gedifferentieerde mogelijkheden in vergelijking met andere databases grafiek in de markt:

* Elastische schaalbare doorvoer en opslag

 De grafieken in de praktijk moeten worden uitgebreid dan de capaciteit van één server. Met Azure Cosmos DB, kunt u uw grafieken probleemloos schalen op meerdere servers. U kunt ook de doorvoer van uw grafiek onafhankelijk op basis van uw toegangspatronen schalen. Azure Cosmos DB ondersteunt graph-databases die kunnen worden geschaald naar vrijwel onbeperkte opslaggrootte en ingerichte doorvoer.

* Meerdere landen/regio-replicatie

 De grafiekgegevens Azure Cosmos DB transparant gerepliceerd naar alle regio's die je hebt gekoppeld aan uw account. Replicatie kunt u toepassingen ontwikkelen die globale toegang tot gegevens nodig. Er zijn voor-en nadelen op het gebied van de consistentie, beschikbaarheid, prestaties en en bijbehorende garanties. Azure Cosmos DB biedt transparante regionale failover met multihoming-API's. U kunt schalen doorvoer en opslag overal ter wereld.

* Snelle query's en traversals met bekende Gremlin syntaxis

 Heterogene hoekpunten en randen opslaan en deze documenten via de syntaxis van een bekend Gremlin opvragen. Azure Cosmos-database maakt gebruik van een uiterst gelijktijdige, vergrendeling vrij, logboek gestructureerde indexering technologie voor het automatisch alle inhoud te indexeren. Deze mogelijkheid kunt u uitgebreide realtime query's en traversals zonder te hoeven schemahints, secundaire indexen of weergaven opgeven. Meer informatie [grafieken opvragen met behulp van Gremlin](gremlin-support.md).

* Volledig beheerd

 Azure Cosmos DB elimineert de noodzaak om database- en resources te beheren. Als een volledig beheerde Microsoft Azure-service geen u moet virtuele machines beheren, implementeren en configureren van software, schalen beheren of te maken met complexe gegevenslaag upgrades. Elke grafiek wordt automatisch een back-up en beveiligd tegen regionale fouten. U kunt gemakkelijk een Cosmos-DB Azure-account toevoegen en capaciteit inrichten als u dit nodig zodat u zich kunt richten op uw toepassing in plaats van het besturingssysteem en het beheren van uw database.

* Automatische indexeren

 Standaard Azure Cosmos DB automatisch alle eigenschappen in de knooppunten en randen in de grafiek worden geïndexeerd en niet verwacht of vereisen een schema of het maken van secundaire indexen.

* Compatibiliteit met Apache TinkerPop

 Azure Cosmos DB systeemeigen ondersteuning biedt voor de open source Apache TinkerPop standaard en kan worden geïntegreerd met andere systemen grafiek TinkerPop ingeschakeld. Ja, kunt u eenvoudig migreren vanaf een andere grafiek database, zoals Titan of Neo4j, of Azure Cosmos DB met grafiek analytics frameworks zoals [Apache Spark GraphX](spark-connector-graph.md).

* Instelbare consistentieniveaus

 Selecteer in de vijf goed gedefinieerde consistentieniveaus voor optimale balans tussen de consistentie en prestaties. Voor query's en leesbewerkingen biedt Azure Cosmos DB vijf verschillende consistentieniveaus: sterk, gebonden-verouderd, sessie, consistent voorvoegsel en mogelijk. Deze gedetailleerde, goed gedefinieerde consistentieniveaus zodat u kunt geluid-en nadelen van de consistentie, beschikbaarheid en latentie. Meer informatie [gegevens instelbare consistentieniveaus in Azure Cosmos DB](consistency-levels.md).

Azure Cosmos DB kunt ook meerdere modellen, zoals documenten en grafiek binnen de dezelfde containers/databases gebruiken. U kunt een documentverzameling gebruiken voor het opslaan van grafiekgegevens naast documenten. U kunt SQL-query's via JSON en query's Gremlin query dezelfde gegevens als een grafiek gebruiken.

## <a name="get-started"></a>Aan de slag
U kunt de Azure-opdrachtregelinterface (CLI), Azure PowerShell of de Azure-portal met ondersteuning voor graph API Azure DB die Cosmos-accounts maken. Nadat u de accounts hebt gemaakt, de Azure portal een service-eindpunt biedt zoals `https://<youraccount>.gremlin.cosmosdb.azure.com`, biedt een WebSocket-front-end voor Gremlin. U kunt uw TinkerPop-compatibele hulpprogramma's, zoals configureren de [Gremlin Console](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console), zodat deze verbinding maken met dit eindpunt en ontwikkelen van toepassingen in Java, Node.js of eventuele Gremlin clientstuurprogramma.

De volgende tabel geeft populaire Gremlin-stuurprogramma's die u met Azure Cosmos DB gebruiken kunt:

| Downloaden | Documentatie |
| --- | --- |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) |[Gremlin JavaDoc](http://tinkerpop.apache.org/javadocs/current/full/) |
| [Node.js](https://www.npmjs.com/package/gremlin) |[Gremlin-JavaScript op Github](https://github.com/jbmusso/gremlin-javascript) |
| [Gremlin console](https://tinkerpop.apache.org/downloads.html) |[TinkerPop docs](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |

Azure Cosmos DB biedt ook een .NET-bibliotheek met Gremlin uitbreidingsmethoden boven de [Azure Cosmos DB SDK's](sql-api-sdk-dotnet.md) via NuGet. Deze bibliotheek biedt een 'in-process' Gremlin-server kunt u direct verbinding maken met gegevenspartities.

| Downloaden | Documentatie |
| --- | --- |
| [.NET](https://www.nuget.org/packages/Microsoft.Azure.Graphs/) |[Microsoft.Azure.Graphs](https://msdn.microsoft.com/library/azure/dn948556.aspx) |

Met behulp van de [Azure Cosmos DB Emulator](local-emulator.md), kunt u de bovenstaande .NET Graph API ontwikkelen en testen lokaal zonder te maken van een Azure-abonnement of mogelijke kosten. Wanneer u tevreden bent over hoe uw toepassing in de Emulator werkt, kunt u overschakelen naar het met een Azure DB die Cosmos-account in de cloud.

> [!NOTE]
> Ondersteuning voor het valideren van Gremlin een query uitgevoerd naar [Azure Cosmos DB Emulator](local-emulator.md) is alleen beschikbaar via .NET Graph API.

## <a name="scenarios-for-graph-support-of-azure-cosmos-db"></a>Scenario's voor ondersteuning van Azure DB die Cosmos grafiek
Hier volgen enkele scenario's waar de grafiek ondersteuning van Azure Cosmos-database kan worden gebruikt:

* Sociale netwerken

 Door het combineren van gegevens over uw klanten en hun interacties met andere mensen, kunt u gepersonaliseerde ervaring te ontwikkelen, klant gedrag te voorspellen of mensen communiceren met anderen interesses. Azure Cosmos DB kan sociale netwerken beheren en bijhouden van klantvoorkeuren en gegevens die worden gebruikt.

* Aanbeveling engines

 Dit scenario wordt doorgaans gebruikt in de detailhandel. U kunt aangepaste aanbevelingen opbouwen door een combinatie van informatie over producten, gebruikers en interactie van gebruikers, zoals aanschaffen, bladeren of een item. De lage latentie, elastisch schalen en systeemeigen grafiek ondersteuning van Azure DB die Cosmos is ideaal voor het modelleren van deze bewerkingen.

* Georuimtelijk

 Veel toepassingen in telecommunicatie, logistiek en reizen plannen moeten zoeken naar een locatie van belang zijn binnen een gebied of de kortste/optimale route tussen twee locaties vinden. Azure Cosmos-database is een natuurlijke geschikt is voor deze problemen.

* Internet of Things

 Met de netwerk- en verbindingen tussen gemodelleerd als een grafiek van IoT-apparaten, kunt u een beter inzicht te krijgen van de status van uw apparaten en activa bouwen. U kunt leert ook hoe wijzigingen in een deel van het netwerk mogelijk een ander deel kunnen beïnvloeden.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over ondersteuning van de grafiek in Azure Cosmos DB:

* Aan de slag met de [Azure Cosmos DB grafiek zelfstudie](create-graph-dotnet.md).
* Meer informatie over het [grafieken in Azure Cosmos DB een query met behulp van Gremlin](gremlin-support.md).
