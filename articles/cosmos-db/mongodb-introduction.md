---
title: 'Inleiding tot Azure Cosmos DB: API voor MongoDB | Microsoft Docs'
description: Meer informatie over hoe u Azure Cosmos DB kunt gebruiken om op te slaan en query grote hoeveelheden JSON-documenten met een lage latentie met behulp van de populaire OSS MongoDB APIs.
keywords: Wat is MongoDB
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 4afaf40d-c560-42e0-83b4-a64d94671f0a
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: anhoh
ms.openlocfilehash: 4dbf91a3c1d6a287d7337647f9e059566c7ddbe5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-cosmos-db-api-for-mongodb"></a>Inleiding tot Azure Cosmos DB: API voor MongoDB

[Azure Cosmos DB](../cosmos-db/introduction.md) is de wereldwijd gedistribueerde databaseservice met meerdere modellen van Microsoft voor essentiële toepassingen. Azure Cosmos DB biedt [gebruiksklare wereldwijde distributie](distribute-data-globally.md), [elastisch schalen van doorvoer en opslag](partition-data.md), latentie die wereldwijd in het 99e percentiel onder de 10 milliseconden blijft, [vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) en een gegarandeerd hoge beschikbaarheid, allemaal op basis van [toonaangevende serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Met Azure Cosmos DB worden [gegevens automatisch geïndexeerd](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), zonder dat u te maken krijgt met schema- en indexbeheer. Azure Cosmos DB beschikt over meerdere modellen en ondersteunt modellen voor document-, sleutelwaarde-, grafiek- en kolomgegevens. 

![Azure Cosmos DB: MongoDB API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Cosmos DB databases kunnen worden gebruikt als gegevensopslag voor apps die zijn geschreven voor [MongoDB](https://docs.mongodb.com/manual/introduction/). Dit betekent dat met behulp van de bestaande [stuurprogramma's](https://docs.mongodb.org/ecosystem/drivers/), uw toepassing geschreven voor MongoDB kan nu met Cosmos DB communiceren en Cosmos DB databases in plaats van MongoDB-databases gebruiken. In veel gevallen kunt u overschakelen van het gebruik van MongoDB aan de Cosmos-database door eenvoudigweg een verbindingsreeks. Met deze functionaliteit kunt u gemakkelijk maken en in de MongoDB-databasetoepassingen uitvoeren in Azure cloud met Azure Cosmos DB globale verdeling en [uitgebreide toonaangevende Sla's](https://azure.microsoft.com/support/legal/sla/cosmos-db), maar blijft gebruiken bekend vaardigheden en hulpprogramma's voor MongoDB.


## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Wat is het voordeel van het gebruik van Azure DB die Cosmos voor MongoDB toepassingen?

**Elastische schaalbare doorvoer en opslag:** eenvoudig omhoog of omlaag de MongoDB-database om te voldoen aan de behoeften van uw toepassing schalen. Uw gegevens worden opgeslagen op SSD-schijven (Solid State Disks) voor lage voorspelbare latenties. MongoDB-verzamelingen die kunnen worden geschaald naar vrijwel onbeperkte opslaggrootte en ingerichte doorvoer biedt ondersteuning voor cosmos DB. U kunt schalen Cosmos DB met voorspelbare prestaties naadloos wanneer uw toepassing groeit. 

**Meerdere landen/regio-replicatie:** Cosmos DB transparant uw gegevens worden gerepliceerd naar alle regio's die je hebt gekoppeld aan je account MongoDB, zodat u kunt toepassingen ontwikkelen die globale toegang tot gegevens en tegelijkertijd de wisselwerking tussen de consistentie, beschikbaarheid en prestaties, met bijbehorende garanties vereisen. Cosmos DB biedt transparante regionale failover met multihoming-API's en de mogelijkheid om te schalen doorvoer en opslag overal ter wereld. Meer informatie [Distribueer gegevens globaal](distribute-data-globally.md).

**Compatibiliteit met MongoDB**: U kunt uw bestaande MongoDB-expertise toepassingscode en tooling. U kunt ontwikkelen met behulp van MongoDB en deze implementeren in productieomgevingen met de volledig beheerde service voor globaal gedistribueerde Cosmos DB.

**Er is geen Serverbeheer**: U hoeft te beheren en schalen van uw MongoDB-databases. Cosmos DB is een volledig beheerde service, waardoor u niet beschikt over een infrastructuur of virtuele Machines om zelf te beheren. Cosmos DB is beschikbaar in 30 + [Azure-gebieden](https://azure.microsoft.com/regions/services/).

**Instelbare consistentieniveaus:** selecteer een van de vijf goed gedefinieerde consistentieniveaus voor een optimale balans tussen consistentie en prestaties. Voor query's en leesbewerkingen biedt Cosmos DB vijf verschillende consistentieniveaus: sterk, gebonden-verouderd, sessie, consistente voorvoegsel en uiteindelijk. Op basis van deze gedetailleerde, goed gedefinieerde consistentieniveaus kunt u een goede balans vinden tussen de consistentie, beschikbaarheid en latentie. Zie [Consistentieniveaus gebruiken om de beschikbaarheid en prestaties te maximaliseren](consistency-levels.md) voor meer informatie.

**Automatische indexering**: standaard Cosmos DB indexeert automatisch alle de documenten in uw MongoDB-eigenschappen van de database en niet verwacht of vereist een schema of het maken van secundaire indexen.

**Enterprise hoogwaardige** -Azure Cosmos DB ondersteunt meerdere lokale replica's voor het leveren van 99,99% beschikbaarheid en gegevensbescherming, met betrekking tot lokale en regionale fouten. Azure Cosmos DB heeft enterprise hoogwaardige [naleving certificeringen](https://www.microsoft.com/trustcenter) en beveiligingsfuncties. 

Meer informatie in deze Azure video met Scott Hanselman en Azure Cosmos DB Principal Engineering Manager, Kirill Gavrylyuk vrijdag.

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Introducing-Azure-Cosmos-DB/player]
> 

## <a name="how-to-get-started"></a>Hoe gaat u aan de slag

Ga als volgt de MongoDB-snelstartgidsen voor het maken van een Cosmos-DB-account en het migreren van uw bestaande toepassing Mongo DB Cosmos-database gebruiken of maken van een nieuwe:

* [Migreren van een bestaande Node.js MongoDB-web-app](create-mongodb-nodejs.md).
* [Een web-App voor MongoDB-API met .NET- en de Azure-portal](create-mongodb-dotnet.md)
* [Maken van een console-app van de MongoDB-API met Java en de Azure-portal](create-mongodb-java.md)

## <a name="next-steps"></a>Volgende stappen

Informatie over Azure Cosmos DB MongoDB-API is geïntegreerd in de algehele Azure DB die Cosmos-documentatie, maar hier zijn enkele verwijzingen naar u op weg:

* Ga als volgt de [verbinding maken met een account MongoDB](connect-mongodb-account.md) zelfstudie voor informatie over het ophalen van de verbindingsinformatie voor uw account.
* Ga als volgt de [MongoChef voor gebruik met Azure Cosmos DB](mongodb-mongochef.md) zelfstudie voor informatie over het maken van een verbinding tussen uw Azure DB die Cosmos-database en de MongoDB-app in MongoChef.
* Ga als volgt de [gegevens voor het migreren naar Azure Cosmos DB met het protocol ondersteunen voor MongoDB](mongodb-migrate.md) zelfstudie voor het importeren van uw gegevens naar een API voor MongoDB-database.
* Verbinding maken met een API voor het gebruik van MongoDB-account [Robomongo](mongodb-robomongo.md).
* Meer informatie over hoeveel RUs van uw bewerkingen met gebruikmaakt de [GetLastRequestStatistics opdracht en de Azure portal metrische gegevens](request-units.md#GetLastRequestStatistics).
* Meer informatie over hoe [lezen voorkeuren voor globaal gedistribueerde apps configureren](../cosmos-db/tutorial-global-distribution-mongodb.md).
