---
title: Inleiding tot Azure Cosmos DB
description: Lees hier alles over Azure Cosmos DB. Deze wereldwijd gedistribueerde database met meerdere modellen is gebouwd voor lage latentie, elastische schaalbaarheid, hoge beschikbaarheid en voor het bieden van systeemeigen ondersteuning voor NoSQL-gegevens.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: overview
ms.date: 12/18/2018
ms.author: sngun
ms.openlocfilehash: 68ed21489cb97ff23a252ecc5287ad79aeeb210e
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54429509"
---
# <a name="welcome-to-azure-cosmos-db"></a>Welkom bij Azure Cosmos DB

Van toepassingen wordt tegenwoordig vereist dat ze zeer responsief en altijd online zijn. Voor het bereiken van lage latentie en hoge beschikbaarheid moeten de instanties van deze toepassingen worden geïmplementeerd in datacenters die zich dicht bij de gebruikers ervan bevinden. Toepassingen moeten in realtime reageren op grote fluctuaties in het gebruik tijdens piekuren, moeten steeds toenemende gegevensvolumes kunnen opslaan en deze gegevens in milliseconden beschikbaar kunnen maken voor gebruikers.

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. Cosmos DB maakt het mogelijk om met één muisklik doorvoer en opslag flexibel en onafhankelijk te schalen binnen een onbeperkt aantal geografische regio's van Azure. Met behulp van uw favoriete API zoals de SQL-, MongoDB-, Cassandra-, Table- of Gremlin-API kunt u de doorvoer en opslag elastisch schalen en zo profiteren van snelle gegevenstoegang met snelheden van niet meer dan enkele milliseconden. Cosmos DB biedt uitgebreide [serviceovereenkomsten](https://aka.ms/acdbsla) (SLA's) waarin gegarandeerde doorvoer, latentie, beschikbaarheid en consistentie zijn vastgelegd. Iets wat geen enkele andere databaseservice kan bieden.

U kunt [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/) zonder Azure-abonnement, zonder kosten en zonder verplichtingen.

> [!div class="nextstepaction"]
> [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/)

![Azure Cosmos DB is de service van Microsoft voor wereldwijd gedistribueerde databases met mogelijkheden voor elastisch uitschalen, een gegarandeerde lage latentie, vijf consistentiemodellen en uitgebreide, gegarandeerde SLA's](./media/introduction/azure-cosmos-db.png)

## <a name="key-benefits"></a>Belangrijkste voordelen

### <a name="turnkey-global-distribution"></a>Kant-en-klare wereldwijde distributie

Met Cosmos DB bent u in staat om wereldwijd zeer responsieve en maximaal beschikbare toepassingen te bouwen. Cosmos DB repliceert uw gegevens transparant naar ongeacht welke locatie waarop zich uw gebruikers bevinden, zodat zij kunnen werken met een replica van de gegevens die zich het dichtst bij hen in de buurt bevindt.

Met Cosmos DB kunt u op elk gewenst moment met een klik op een knop elke willekeurige Azure-regio aan uw Cosmos-account toevoegen of eruit verwijderen. Cosmos DB repliceert naadloos uw gegevens naar alle regio's die zijn gekoppeld aan uw Cosmos-account, terwijl uw toepassing maximaal beschikbaar blijft dankzij de mogelijkheden tot multihoming van de service. Zie het artikel [Wereldwijde distributie](distribute-data-globally.md) voor meer informatie.

### <a name="always-on"></a>AlwaysOn

Door de hechte integratie met de infrastructuur van Azure en de [transparante replicatie voor meerdere masters ](global-dist-under-the-hood.md) biedt Cosmos DB een [hoge beschikbaarheid](high-availability.md) van 99,999% voor zowel lees- als schrijfbewerkingen. Cosmos DB biedt u ook de mogelijkheid om via een programma (of via de portal) de regionale failover van uw Cosmos-account aan te roepen. Deze mogelijkheid helpt u om ervoor te zorgen dat in een situatie waarin een Cosmos-database automatisch een failover zou uitvoeren, de rest van uw toepassing ook is opgezet om een failover uit te voeren als er sprake is van een regionaal noodgeval.

### <a name="elastic-scalability-of-throughput-and-storage-worldwide"></a>Doorvoer en opslag wereldwijd elastisch schalen

Cosmos DB is ontwikkeld met transparante horizontale partitionering en replicatie voor meerdere masters en biedt nog nooit eerder vertoonde elastische schaalbaarheid voor uw schrijf- en leesbewerkingen, overal ter wereld. U kunt met een enkele API-aanroep elastisch opschalen van duizenden tot honderden miljoenen aanvragen per seconde over de hele wereld, en toch u betaalt u alleen voor de doorvoer (en opslag) die u nodig hebt. Deze mogelijkheid helpt u om onverwachte pieken in uw werkbelastingen te kunnen verwerken zonder dat u zich met excessieve inrichting hoeft bezig te houden om de piek aan te kunnen. Zie [Partitionering in Cosmos DB](partitioning-overview.md), [Ingerichte doorvoer op containers en databases](set-throughput.md) en [Doorvoer wereldwijd inrichten](scaling-throughput.md).

### <a name="guaranteed-low-latency-at-99th-percentile-worldwide"></a>Gegarandeerde lage latentie in het 99e percentiel, wereldwijd

Met Cosmos DB kunt u hoog-responsieve wereldwijde toepassingen bouwen. Dankzij het moderne replicatieprotocol voor meerdere masters en de vergrendelingsvrije en [voor schrijven geoptimaliseerde database-engine](index-policy.md) garandeert Cosmos DB een latentie van minder dan 10 ms voor leesbewerkingen en (geïndexeerde) schrijfbewerkingen in het 99e percentiel, wereldwijd. Op deze manier kunt u rekenen op een doorlopende opname van gegevens en uiterst snelle verwerking van query's voor zeer responsieve apps.

### <a name="precisely-defined-multiple-consistency-choices"></a>Nauwkeurig gedefinieerde, meerdere consistentiekeuzes

U hoeft niet langer verregaande [compromissen tussen consistentie, beschikbaarheid, latentie en doorvoer](consistency-levels-tradeoffs.md) te sluiten. Het replicatieprotocol voor meerdere masters van Cosmos DB biedt [vijf duidelijk gedefinieerde consistentieniveaus](consistency-levels.md) (sterk, gebonden veroudering, sessie, consistent voorvoegsel en uiteindelijk) voor een intuïtief programmeermodel met lage latentie en hoge beschikbaarheid voor uw wereldwijde toepassing.

### <a name="no-schema-or-index-management"></a>Geen schema- of indexbeheer

Vooral voor wereldwijd gedistribueerde apps is het uiterst moeilijk om databaseschema's en indexen synchroon te houden met het schema van een toepassing. Met Cosmos DB hoeft u zich echter niet te bekommeren om het beheer van schema's of indexen. De database-engine is volledig schemaneutraal.  Omdat er geen schema- en indexbeheer is vereist, hoeft u zich ook geen zorgen te maken over downtime van toepassingen tijdens het migreren van schema's. Cosmos DB [indexeert automatisch alle gegevens](index-policy.md) en zorgt dat query's snel worden uitgevoerd.

### <a name="battle-tested-database-service"></a>Intensief geteste databaseservice

Cosmos DB is een fundamentele service in Azure. Cosmos DB wordt al bijna een decennium lang gebruikt door veel van de Microsoft-producten voor essentiële toepassingen op wereldwijde schaal, zoals Skype, Xbox, Office 365, Azure en veel meer. Vandaag de dag is Cosmos DB een van de snelst groeiende services op Azure die wordt gebruikt door veel externe klanten en toepassingen die elastisch schalen en/of kant-en-klare replicatie voor meerdere datacenters/meerdere regio's en meerdere masters met een lage latentie en hoge beschikbaarheid voor zowel lees- als schrijfbewerkingen, vereisen.

### <a name="ubiquitous-regional-presence"></a>Alomtegenwoordige regionale aanwezigheid

Cosmos DB is beschikbaar in alle Azure-regio's wereldwijd, waaronder meer dan 54 regio's in de openbare cloud, Azure China 21Vianet, Azure Duitsland, Azure Government en Azure Government for Department of Defense (DoD). Zie [Regionale aanwezigheid van Cosmos DB](regional-presence.md).

### <a name="secure-by-default-and-enterprise-ready"></a>Standaard beveiligd en geschikt voor gebruik door bedrijven

Cosmos DB is gecertificeerd voor een [groot aantal verschillende nalevingsstandaarden](compliance.md). Bovendien worden alle gegevens in Cosmos DB tijdens opslag en verzending versleuteld. Cosmos DB biedt autorisatie op rijniveau en voldoet aan strenge beveiligingsstandaarden.

### <a name="significant-tco-savings"></a>Aanzienlijke TCO-besparingen

Aangezien Cosmos DB een volledig beheerde service is, hoeft u zich niet langer bezig te houden met het beheer of de werking van complexe implementaties op meerdere datacenters, met upgrades van uw databasesoftware, met betalingen voor ondersteuning, licenties of voor het uitvoeren van bewerkingen. Zie [Kosten optimaliseren met Cosmos DB](total-cost-ownership.md).

### <a name="industry-leading-comprehensive-slas"></a>Toonaangevende uitgebreide SLA's

Cosmos DB is de eerste en enige service die [toonaangevende SLA's](https://azure.microsoft.com/support/legal/sla/cosmos-db/) biedt voor 99,999% hoge beschikbaarheid, latentie voor lees- en schrijfbewerkingen in het 99e percentiel, gegarandeerde doorvoer en consistentie.

### <a name="apache-spark--cosmos-db--operational-analytics-at-global-scale"></a>Apache Spark + Cosmos DB = operationele analyse op wereldwijde schaal

U kunt [Spark](spark-connector.md) rechtstreeks uitvoeren op gegevens die zijn opgeslagen in Cosmos DB. Op deze manier kunt u op wereldwijde schaal operationele analyses met een lage latentie uitvoeren zonder dat dit gevolgen heeft voor transactionele workloads die rechtstreeks van Cosmos DB gebruikmaken.

### <a name="develop-applications-for-cosmos-db-using-popular-nosql-apis"></a>Toepassingen voor Cosmos DB ontwikkelen met populaire NoSQL-API's

Cosmos DB biedt een keuze uit API's voor het bijwerken van gegevens die zijn opgeslagen in uw Cosmos-database en het uitvoeren van query's hierop. Standaard [kunt u SQL gebruiken](how-to-sql-query.md) om uw gegevens in uw Cosmos-database bij te werken en er query's op uit te voeren.

Cosmos DB implementeert ook wire-protocollen voor[Cassandra](cassandra-introduction.md), [MongoDB](mongodb-introduction.md), [Gremlin](graph-introduction.md) en [Azure Table Storage](table-introduction.md) rechtstreeks op de service. Daardoor kunt u stuurprogramma's (en hulpprogramma's) van clients voor de meestgebruikte NoSQL-API's rechtstreeks naar uw Cosmos-database verwijzen. Door ondersteuning te bieden voor wire-protocollen van veelgebruikte NoSQL-API's kan Cosmos DB u het volgende bieden:

* U kunt uw toepassing eenvoudig migreren naar Cosmos DB met behoud van belangrijke onderdelen van uw toepassingslogica.
* Behoud de overdraagbaarheid van uw toepassing zonder dat u de vrijheid verliest om van cloudleverancier te veranderen.
* Voor de meestgebruikte NoSQL-API's krijgt u toonaangevende SLA's met een solide financiële basis. 
* U kunt de ingerichte doorvoer en opslag voor uw databases elastisch schalen op basis van uw behoeften, en u betaalt alleen voor de doorvoer en opslag die u nodig hebt. Dit leidt tot aanzienlijke kostenbesparingen.

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Oplossingen die profiteren van Azure Cosmos DB

Alle [toepassingen voor web, mobiel, games en IoT](use-cases.md) die op [wereldwijde](distribute-data-globally.md) schaal en met zeer snelle reactietijden zeer grote hoeveelheden gegevens en lees- en schrijfbewerkingen moeten verwerken voor een verscheidenheid aan gegevens, profiteren van de [gegarandeerde](https://azure.microsoft.com/support/legal/sla/cosmos-db/) hoge beschikbaarheid, hoge doorvoer, lage latentie en instelbare consistentie van Cosmos DB. Kom meer te weten over hoe Cosmos DB kan worden gebruikt om toepassingen voor [IoT en telematica](use-cases.md#iot-and-telematics), [detailhandel en marketing](use-cases.md#retail-and-marketing), [gaming](use-cases.md#gaming) en [web en mobiel](use-cases.md#web-and-mobile-applications) te maken.

## <a name="next-steps"></a>Volgende stappen

Kom meer te weten over de mogelijkheden van Cosmos DB op het gebied van [wereldwijde turnkeydistributie](distribute-data-globally.md) en [partitionering](partitioning-overview.md).

Lees onze snelstartgidsen om snel aan de slag te gaan met Azure Cosmos DB:

* [Aan de slag met de SQL-API van Azure Cosmos DB](create-sql-api-dotnet.md)
* [Aan de slag met de API van Azure Cosmos DB voor MongoDB](create-mongodb-nodejs.md)
* [Aan de slag met de Cassandra-API van Azure Cosmos DB](create-cassandra-dotnet.md)
* [Aan de slag met de Gremlin-API van Azure Cosmos DB](create-graph-dotnet.md)
* [Aan de slag met de Table-API van Azure Cosmos DB](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB gratis uitproberen](https://azure.microsoft.com/try/cosmosdb/)
