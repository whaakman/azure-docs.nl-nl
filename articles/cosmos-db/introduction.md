---
title: Inleiding tot Azure Cosmos DB | Microsoft Docs
description: Lees hier alles over Azure Cosmos DB. Deze wereldwijd gedistribueerde database met meerdere modellen is gebouwd voor lage latentie, elastische schaalbaarheid en hoge beschikbaarheid.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: a855183f-34d4-49cc-9609-1478e465c3b7
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 11/15/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: f34790ad670b488159e945be9bf4ba378cc5e94a
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="welcome-to-azure-cosmos-db"></a>Welkom bij Azure Cosmos DB

Azure Cosmos DB is de wereldwijd gedistribueerde database van Microsoft met meerdere modellen. Azure Cosmos DB maakt het mogelijk om met één muisklik doorvoer en opslag elastisch en onafhankelijk te schalen binnen een onbeperkt aantal geografische regio's van Azure. De oplossing biedt gegarandeerde doorvoer, latentie, beschikbaarheid en consistentie, vastgelegd in uitgebreide [serviceovereenkomsten](https://aka.ms/acdbsla) (SLA's), iets wat geen andere databaseservice kan bieden. [Probeer Azure Cosmos DB gratis uit](https://azure.microsoft.com/try/cosmosdb/) zonder Azure-abonnement, zonder kosten en zonder verplichtingen.

![Azure Cosmos DB is de service van Microsoft voor wereldwijd gedistribueerde databases met mogelijkheden voor elastisch uitschalen, een gegarandeerde lage latentie, vijf consistentiemodellen en uitgebreide, gegarandeerde SLA's](./media/introduction/azure-cosmos-db.png)

> [!div class="nextstepaction"]
> [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/)

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden
Als een wereldwijd gedistribueerde databaseservice biedt Azure Cosmos DB de volgende mogelijkheden om u te helpen met het bouwen van schaalbare en zeer responsieve toepassingen:

* **Kant-en-klare wereldwijde distributie**
    * U kunt [uw gegevens distribueren](distribute-data-globally.md) naar een willekeurig aantal [Azure-regio's](https://azure.microsoft.com/regions/) met [slechts één druk op de knop](tutorial-global-distribution-documentdb.md). Op deze manier kunt u uw gegevens daar opslaan waar uw gebruikers zijn, waardoor uw klanten profiteren van de laagst mogelijke latentie. 
    * Dankzij de multihoming-API's van Azure Cosmos DB weet de app altijd wat de dichtstbijzijnde regio is en worden aanvragen naar het dichtstbijzijnde datacenter verzonden. Dit alles is mogelijk zonder wijzigingen in de configuratie. U stelt uw schrijfregio in en net zo veel leesregio's als u wilt, en de rest wordt automatisch afgehandeld.

* **Meerdere gegevensmodellen en populaire API's voor het raadplegen en opvragen van gegevens**
    * Het op 'atom-record-sequence' (ARS) gebaseerde gegevensmodel waarop Azure Cosmos DB is gebouwd, biedt systeemeigen ondersteuning voor meerdere gegevensmodellen, waaronder modellen op basis van documenten, grafieken, sleutels en waarden, tabellen en kolommen.
    * API's voor de volgende gegevensmodellen worden ondersteund, waarbij er SDK's beschikbaar zijn in meerdere talen:
        * [DocumentDB-API](documentdb-introduction.md): een JSON-database-engine zonder schema met SQL querymogelijkheden.
        * [MongoDB API](mongodb-introduction.md): een MongoDB-databaseservice die is gebouwd op Cosmos DB. Compatibel met bestaande bibliotheken, stuurprogramma's, hulpprogramma's en toepassingen van MongoDB.
        * [Tabel-API](table-introduction.md): een databaseservice op basis van sleutel/waardeparen die is ontwikkeld om premium-mogelijkheden te bieden voor Azure-tabelopslagtoepassingen.
        * [Grafiek-API (Gremlin)](graph-introduction.md): een databaseservice voor grafieken die is gebouwd volgens de [Apache TinkerPop-specificatie](http://tinkerpop.apache.org/).
        * [Cassandra-API](cassandra-introduction.md): een archief met sleutel/waardeparen dat is gebouwd op de [Apache Cassandra](https://cassandra.apache.org/)-implementatie. 
        * Er komen binnenkort nog meer gegevensmodellen beschikbaar.

* **Doorvoer en opslag op aanvraag elastisch schalen, waar ook ter wereld**
    * U kunt databasedoorvoer op eenvoudige wijze schalen met de granulatie [per seconde](request-units.md) en de instelling op elk gewenst moment wijzigen. 
    * U kunt opslag [transparant en automatisch](partition-data.md) schalen om te allen tijde aan uw opslagbehoeften te voldoen, nu en in de toekomst.

* **Zeer responsieve en bedrijfskritische toepassingen bouwen**
    * Azure Cosmos DB garandeert voor klanten een lage end-to-end latentie in het 99e percentiel. 
    * Voor een gemiddeld item van 1 kB betekent dit dat in 99% van de gevallen een end-to-end latentie van minder dan 10 ms wordt gegarandeerd voor leesbewerkingen en van minder dan 15 ms voor geïndexeerde schrijfbewerkingen, binnen dezelfde Azure-regio. De gemiddelde latenties zijn aanzienlijk lager (minder dan 5 ms).

* **Zorgen voor vrijwel volledige beschikbaarheid**
    * SLA voor een beschikbaarheid van 99,99% voor alle accounts voor één regio en alle accounts voor meerdere regio's met soepele consistentie en leesbeschikbaarheid van 99,999% voor alle databaseaccounts voor meerdere regio's.
    * U kunt voor een hogere beschikbaarheid implementeren naar een willekeurig aantal [Azure-regio's](https://azure.microsoft.com/regions).
    * U kunt zonder gegevensverlies [een storing simuleren](regional-failover.md) in een of meer regio's. 

* **Op correcte wijze wereldwijd gedistribueerde toepassingen schrijven**
    * De [vijf consistentiemodellen](consistency-levels.md) bieden een breed spectrum aan mogelijkheden: van sterke SQL-achtige consistentie tot NoSQL-achtige eindconsistentie en alles daartussenin. 
  
* **Niet-goed-geld-teruggarantie**
    * Uw gegevens komen razendsnel op de plaats van bestemming of u krijgt uw geld terug. 
    * [Serviceovereenkomsten](https://aka.ms/acdbsla) voor beschikbaarheid, latentie, doorvoer en consistentie. 

* **Geen databaseschema/indexbeheer**
    * U hoeft zich geen zorgen meer te maken of uw databaseschema en indexen nog synchroon zijn met uw toepassingsschema. We maken namelijk geen gebruik van schema's. 
    * De database-engine van Azure Cosmos DB is volledig schemaneutraal. Alle ontvangen gegevens worden automatisch geïndexeerd zonder dat hiervoor schema's of indexen nodig zijn en query's worden razendsnel uitgevoerd. 

* **Lage eigendomskosten**
    * Vijf tot tien keer [kosteneffectiever](https://aka.ms/cosmos-db-tco-paper) dan een niet-beheerde oplossing.
    * Drie keer goedkoper dan DynamoDB.

## <a name="capability-comparison"></a>Vergelijking van functionaliteit

Azure Cosmos DB biedt de beste mogelijkheden van relationele en niet-relationele databases.

| Functionaliteit | Relationele databases   | Niet-relationele databases (NoSQL) |    Azure Cosmos DB |
| --- | --- | --- | --- |
| Wereldwijde distributie | Nee | Nee | Ja, kant-en-klare distributie in meer dan 30 regio's met multihoming-API's|
| Horizontaal schalen | Nee | Ja | Ja, u kunt doorvoer en opslag onafhankelijk van elkaar schalen | 
| Gegarandeerde latentie | Nee | Ja | Ja, 99% van de leesbewerkingen in < 10 ms en van de schrijfbewerkingen in < 15 ms | 
| Hoge beschikbaarheid | Nee | Ja | Ja, Cosmos DB is altijd ingeschakeld, beschikt over PACELC-tradeoffs, en biedt opties voor automatische en handmatige failover|
| Gegevensmodel + API | Relationeel + SQL | Meerdere modellen + OSS-API | Meerdere modellen + SQL + OSS-API (binnenkort meer) |
| SLA's | Ja | Nee | Ja, uitgebreide SLA's voor latentie, doorvoer, consistentie en beschikbaarheid |

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Oplossingen die profiteren van Azure Cosmos DB

Alle [webtoepassingen, mobiele toepassingen, games en IoT-toepassingen](use-cases.md) die op [wereldwijde](distribute-data-globally.md) schaal en met snelle reactietijden zeer grote hoeveelheden lees- en schrijfbewerkingen moeten verwerken voor een verscheidenheid aan gegevens, profiteren van de [gegarandeerde](https://azure.microsoft.com/support/legal/sla/cosmos-db/) beschikbaarheid, hoge doorvoer, lage latentie en instelbare consistentie van Azure Cosmos DB. Lees meer over hoe CosmosDB kan worden toegepast op [IoT en telematica](use-cases.md#iot-and-telematics), [detailhandel en marketing](use-cases.md#retail-and-marketing), [gaming](use-cases.md#gaming) en [web- en mobiele toepassingen](use-cases.md#web-and-mobile-applications).

## <a name="next-steps"></a>Volgende stappen
Lees onze quickstarts om snel aan de slag te gaan met Azure Cosmos DB:

* [Aan de slag met de DocumentDB-API van Azure Cosmos DB](create-documentdb-dotnet.md)
* [Aan de slag met de MongoDB-API van Azure Cosmos DB](create-mongodb-nodejs.md)
* [Aan de slag met de grafiek-API van Azure Cosmos DB](create-graph-dotnet.md)
* [Aan de slag met de tabel-API van Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/)
