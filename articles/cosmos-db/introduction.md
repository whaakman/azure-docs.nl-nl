---
title: Inleiding tot Azure Cosmos DB
description: Lees hier alles over Azure Cosmos DB. Deze wereldwijd gedistribueerde database met meerdere modellen is gebouwd voor lage latentie, elastische schaalbaarheid, hoge beschikbaarheid en voor het bieden van systeemeigen ondersteuning voor NoSQL-gegevens.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: overview
ms.date: 03/18/2019
ms.author: sngun
ms.openlocfilehash: eec94217cc62f74a5075412aafcf85bf67346bf4
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58294362"
---
# <a name="welcome-to-azure-cosmos-db"></a>Welkom bij Azure Cosmos DB

Van toepassingen wordt tegenwoordig vereist dat ze zeer responsief en altijd online zijn. Voor het bereiken van lage latentie en hoge beschikbaarheid moeten de instanties van deze toepassingen worden geïmplementeerd in datacenters die zich dicht bij de gebruikers ervan bevinden. Toepassingen moeten in realtime reageren op grote fluctuaties in het gebruik tijdens piekuren, moeten steeds toenemende gegevensvolumes kunnen opslaan en deze gegevens in milliseconden beschikbaar kunnen maken voor gebruikers.

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. Met één klik, Cosmos DB kunt u elastisch en onafhankelijk van elkaar schalen doorvoer en opslag naar een willekeurig aantal Azure-regio's over de hele wereld. U kunt flexibel doorvoer en opslag schalen en profiteer van snelle, één cijfer milliseconde gegevenstoegang met behulp van uw favoriete API, met inbegrip van SQL, MongoDB, Cassandra, tabellen of Gremlin. Cosmos DB biedt uitgebreide [serviceovereenkomsten](https://aka.ms/acdbsla) (Sla's) voor doorvoer, latentie, beschikbaarheid en garanties voor consistentie, iets dat geen andere database-service biedt.

U kunt [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/) zonder Azure-abonnement, zonder kosten en zonder verplichtingen.

> [!div class="nextstepaction"]
> [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB is de service van Microsoft voor wereldwijd gedistribueerde databases met mogelijkheden voor elastisch uitschalen, een gegarandeerde lage latentie, vijf consistentiemodellen en uitgebreide, gegarandeerde SLA's](./media/introduction/azure-cosmos-db.png)

## <a name="key-benefits"></a>Belangrijkste voordelen

### <a name="turnkey-global-distribution"></a>Kant-en-klare wereldwijde distributie

Met Cosmos DB bent u in staat om wereldwijd zeer responsieve en maximaal beschikbare toepassingen te bouwen. Cosmos DB repliceert uw gegevens transparant waar uw gebruikers zijn, zodat uw gebruikers kunnen werken met een replica van de gegevens die het dichtst bij hen ligt.

Met Cosmos DB kunt u op elk gewenst moment met een klik op een knop elke willekeurige Azure-regio aan uw Cosmos-account toevoegen of eruit verwijderen. Cosmos DB probleemloos uw gegevens worden gerepliceerd naar alle regio's die zijn gekoppeld aan uw Cosmos-account terwijl u uw toepassing nog steeds maximaal beschikbaar, dank aan de *multihoming* mogelijkheden van de service. Zie het artikel [Wereldwijde distributie](distribute-data-globally.md) voor meer informatie.

### <a name="always-on"></a>AlwaysOn

Door de hechte integratie met Azure-infrastructuur en [transparante meerdere masters replicatie](global-dist-under-the-hood.md), Cosmos DB biedt [hoge beschikbaarheid van 99,999%](high-availability.md) voor zowel lees- en schrijfbewerkingen. Cosmos DB biedt u ook de mogelijkheid om via een programma (of via de portal) de regionale failover van uw Cosmos-account aan te roepen. Deze functionaliteit zorgt ervoor dat uw toepassing is ontworpen voor failover in het geval van regionale na noodgevallen.

### <a name="elastic-scalability-of-throughput-and-storage-worldwide"></a>Doorvoer en opslag wereldwijd elastisch schalen

Cosmos DB is ontwikkeld met transparante horizontale partitionering en replicatie voor meerdere masters en biedt nog nooit eerder vertoonde elastische schaalbaarheid voor uw schrijf- en leesbewerkingen, overal ter wereld. U kunt met een enkele API-aanroep elastisch opschalen van duizenden tot honderden miljoenen aanvragen per seconde over de hele wereld, en toch u betaalt u alleen voor de doorvoer (en opslag) die u nodig hebt. Deze mogelijkheid helpt u om onverwachte pieken in uw werkbelastingen te kunnen verwerken zonder dat u zich met excessieve inrichting hoeft bezig te houden om de piek aan te kunnen. Zie voor meer informatie, [partitionering in Cosmos DB](partitioning-overview.md), [ingerichte doorvoer voor containers en databases](set-throughput.md), en [ingerichte doorvoer wereldwijd schalen](scaling-throughput.md).

### <a name="guaranteed-low-latency-at-99th-percentile-worldwide"></a>Gegarandeerde lage latentie in het 99e percentiel, wereldwijd

Met Cosmos DB kunt u hoog-responsieve wereldwijde toepassingen bouwen. Dankzij het moderne replicatieprotocol voor meerdere masters en de vergrendelingsvrije en [voor schrijven geoptimaliseerde database-engine](index-policy.md) garandeert Cosmos DB een latentie van minder dan 10 ms voor leesbewerkingen en (geïndexeerde) schrijfbewerkingen in het 99e percentiel, wereldwijd. Op deze manier kunt u rekenen op een doorlopende opname van gegevens en uiterst snelle verwerking van query's voor zeer responsieve apps.

### <a name="precisely-defined-multiple-consistency-choices"></a>Nauwkeurig gedefinieerde, meerdere consistentiekeuzes

Wanneer het bouwen van wereldwijd toepassingen in Cosmos DB gedistribueerde, hebt u niet langer maken extreme [zorgen voor een balans tussen consistentie, beschikbaarheid, latentie en doorvoer](consistency-levels-tradeoffs.md). Cosmos DB de replicatie van meerdere masters protocol zorgvuldig is ontworpen om u te bieden [vijf duidelijk gedefinieerde consistentiekeuzen](consistency-levels.md) - *sterke*, *gebonden veroudering*, *sessie*, *consistent voorvoegsel*, en *uiteindelijke* — voor een intuïtief programmeermodel met lage latentie en hoge beschikbaarheid voor uw wereldwijd gedistribueerde toepassing.

### <a name="no-schema-or-index-management"></a>Geen schema- of indexbeheer

Vooral voor wereldwijd gedistribueerde apps is het uiterst moeilijk om databaseschema's en indexen synchroon te houden met het schema van een toepassing. Met Cosmos DB hoeft u niet te bekommeren om schema's of indexbeheer. De database-engine is volledig schemaneutraal.  Omdat er geen schema- en indexbeheer is vereist, hoeft u zich ook geen zorgen te maken over downtime van toepassingen tijdens het migreren van schema's. Cosmos DB [indexeert automatisch alle gegevens](index-policy.md) en zorgt dat query's snel worden uitgevoerd.

### <a name="battle-tested-database-service"></a>Intensief geteste databaseservice

Cosmos DB is een fundamentele service in Azure. Cosmos DB is voor bijna een decennium gebruikt door veel van de producten van Microsoft voor essentiële toepassingen op wereldwijde schaal, met inbegrip van Skype, Xbox, Office 365, Azure en nog veel meer. Vandaag de dag is Cosmos DB een van de snelst groeiende services op Azure, die wordt gebruikt door veel externe klanten en uw essentiële toepassingen waarvoor elastisch schalen, kant en klare wereldwijde distributie, replicatie van meerdere masters voor lage latentie en hoge beschikbaarheid van beide Lees- en schrijfbewerkingen.

### <a name="ubiquitous-regional-presence"></a>Alomtegenwoordige regionale aanwezigheid

Cosmos DB is beschikbaar in alle Azure-regio's wereldwijd, inclusief 54 + regio's in de openbare cloud, [Azure China 21Vianet](https://www.azure.cn/en-us/), Azure Duitsland, Azure Government en Azure Government voor Ministerie van defensie (DoD). Zie [Regionale aanwezigheid van Cosmos DB](regional-presence.md).

### <a name="secure-by-default-and-enterprise-ready"></a>Standaard beveiligd en geschikt voor gebruik door bedrijven

Cosmos DB is gecertificeerd voor een [groot aantal verschillende nalevingsstandaarden](compliance.md). Bovendien worden alle gegevens in Cosmos DB tijdens opslag en verzending versleuteld. Cosmos DB biedt autorisatie op rijniveau en voldoet aan strenge beveiligingsstandaarden.

### <a name="significant-tco-savings"></a>Aanzienlijke TCO-besparingen

Aangezien Cosmos DB een volledig beheerde service is, u niet meer nodig hebt om te beheren en werken met complexe multi-datacenter-implementaties en upgrades van uw database-software, betaalt voor de ondersteuning, licenties of bewerkingen of hebben voor het inrichten van uw database voor de piekworkload. Zie voor meer informatie, [optimaliseert u kosten met Cosmos DB](total-cost-ownership.md).

### <a name="industry-leading-comprehensive-slas"></a>Toonaangevende uitgebreide SLA's

Cosmos DB is de eerste en enige service die [toonaangevende SLA's](https://azure.microsoft.com/support/legal/sla/cosmos-db/) biedt voor 99,999% hoge beschikbaarheid, latentie voor lees- en schrijfbewerkingen in het 99e percentiel, gegarandeerde doorvoer en consistentie.

### <a name="globally-distributed-operational-analytics-with-spark"></a>Wereldwijd gedistribueerde operationele analyses met Spark

U kunt [Spark](spark-connector.md) rechtstreeks uitvoeren op gegevens die zijn opgeslagen in Cosmos DB. Op deze manier kunt u op wereldwijde schaal operationele analyses met een lage latentie uitvoeren zonder dat dit gevolgen heeft voor transactionele workloads die rechtstreeks van Cosmos DB gebruikmaken. Zie voor meer informatie, [operational analytics wereldwijd worden gedistribueerd](lambda-architecture.md).

### <a name="develop-applications-on-cosmos-db-using-popular-nosql-apis"></a>Toepassingen ontwikkelen op Cosmos DB met populaire NoSQL-APIs

Cosmos DB biedt een keuze uit de API's om te werken met uw gegevens opgeslagen in uw Cosmos-database. Standaard [kunt u SQL](how-to-sql-query.md) (een core API) voor query's in uw Cosmos-database. Cosmos DB implementeert ook API's voor [Cassandra](cassandra-introduction.md), [MongoDB](mongodb-introduction.md), [Gremlin](graph-introduction.md) en [Azure Table Storage](table-introduction.md). U kunt client stuurprogramma's (en hulpprogramma's) voor de meest gebruikte NoSQL (bijvoorbeeld, MongoDB, Cassandra, Gremlin) rechtstreeks naar uw Cosmos-database verwijzen. Door de wire-protocollen van veelgebruikte NoSQL APIs ondersteunen, kunt u Cosmos DB:

* U kunt uw toepassing eenvoudig migreren naar Cosmos DB met behoud van belangrijke onderdelen van uw toepassingslogica.
* Behoud de overdraagbaarheid van uw toepassing zonder dat u de vrijheid verliest om van cloudleverancier te veranderen.
* Een volledig beheerde cloudservice met de bedrijfstak toonaangevende, financieel ondersteunde Sla's voor de algemene NoSQL APIs ophalen. 
* U kunt de ingerichte doorvoer en opslag voor uw databases elastisch schalen op basis van uw behoeften, en u betaalt alleen voor de doorvoer en opslag die u nodig hebt. Dit leidt tot aanzienlijke kostenbesparingen.

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Oplossingen die profiteren van Azure Cosmos DB

Alle [web, mobiel, games en IoT-toepassing](use-cases.md) die nodig zijn voor het afhandelen van enorme hoeveelheden gegevens, lezen, en voor schrijfbewerkingen in een [wereldwijde schaal](distribute-data-globally.md) met bijna real-time antwoord tijden voor een verscheidenheid aan gegevens profiteren van Cosmos DB [gegarandeerd hoge beschikbaarheid](https://azure.microsoft.com/support/legal/sla/cosmos-db/), hoge doorvoer, lage latentie en instelbare consistentie. Kom meer te weten over hoe Cosmos DB kan worden gebruikt om toepassingen voor [IoT en telematica](use-cases.md#iot-and-telematics), [detailhandel en marketing](use-cases.md#retail-and-marketing), [gaming](use-cases.md#gaming) en [web en mobiel](use-cases.md#web-and-mobile-applications) te maken.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Cosmos-DB-core, concepten [kant en klare wereldwijde distributie](distribute-data-globally.md) en [partitioneren](partitioning-overview.md) en [ingerichte doorvoer](request-units.md).

Lees onze snelstartgidsen om snel aan de slag te gaan met Azure Cosmos DB:

* [Aan de slag met de SQL-API van Azure Cosmos DB](create-sql-api-dotnet.md)
* [Aan de slag met de API van Azure Cosmos DB voor MongoDB](create-mongodb-nodejs.md)
* [Aan de slag met de Cassandra-API van Azure Cosmos DB](create-cassandra-dotnet.md)
* [Aan de slag met de Gremlin-API van Azure Cosmos DB](create-graph-dotnet.md)
* [Aan de slag met de Table-API van Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/)
