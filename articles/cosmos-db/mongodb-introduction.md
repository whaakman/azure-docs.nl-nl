---
title: 'Inleiding tot Azure Cosmos DB: MongoDB-API | Microsoft Docs'
description: Leer hoe u Azure Cosmos DB kunt gebruiken om grote aantallen JSON-documenten met lage latentie op te slaan en te doorzoeken met behulp van populaire OSS MongoDB-API’s.
keywords: Wat is MongoDB?
services: cosmos-db
author: SnehaGunda
manager: slyons
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: overview
ms.date: 02/12/2018
ms.author: sclyon
ms.openlocfilehash: 3a5978603ae57a6975b76a0736dc4e2d1f133d30
ms.sourcegitcommit: 387d7edd387a478db181ca639db8a8e43d0d75f7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2018
ms.locfileid: "42022454"
---
# <a name="introduction-to-azure-cosmos-db-mongodb-api"></a>Inleiding tot Azure Cosmos DB: MongoDB-API

[Azure Cosmos DB](../cosmos-db/introduction.md) is de wereldwijd gedistribueerde databaseservice met meerdere modellen van Microsoft voor essentiële toepassingen. Azure Cosmos DB biedt [gebruiksklare wereldwijde distributie](distribute-data-globally.md), [elastisch schalen van doorvoer en opslag](partition-data.md), latentie van slechts enkele milliseconden op het 99e percentiel, en een gegarandeerd hoge beschikbaarheid, ondersteund met [toonaangevende SLA’s](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Met Azure Cosmos DB worden [gegevens automatisch geïndexeerd](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), zonder dat u te maken krijgt met schema- en indexbeheer. Azure Cosmos DB beschikt over meerdere modellen en ondersteunt modellen voor document-, sleutelwaarde-, grafiek- en kolomgegevens. 

![Azure Cosmos DB: MongoDB-API](./media/mongodb-introduction/cosmosdb-mongodb.png) 

Azure Cosmos DB-databases kunnen worden gebruikt als gegevensopslag voor apps die zijn geschreven voor [MongoDB](https://docs.mongodb.com/manual/introduction/). Deze functionaliteit zorgt ervoor dat de toepassing die is geschreven voor MongoDB nu, door gebruik te maken van de bestaande [stuurprogramma’s](https://docs.mongodb.org/ecosystem/drivers/), kan communiceren met Azure Cosmos DB, en Azure Cosmos DB-databases kan gebruiken in plaats van MongoDB-databases. In veel gevallen hoeft u alleen een verbindingsreeks te wijzigen als u wilt overschakelen van MongoDB naar Azure Cosmos DB. Met deze functionaliteit kunt u eenvoudig MongoDB-databasetoepassingen bouwen die wereldwijd zijn gedistribueerd, en deze uitvoeren in de Azure-cloud met behulp van Azure Cosmos DB en de bijbehorende [uitgebreide toonaangevende SLA’s](https://azure.microsoft.com/support/legal/sla/cosmos-db), terwijl u gebruik blijft maken van vertrouwde vaardigheden en hulpprogramma’s voor MongoDB.

**Compatibiliteit met MongoDB**: u kunt uw kennis van MongoDB aanwenden en gebruikmaken van de toepassingscode en hulpprogramma's van MongoDB, omdat het MongoDB-wire-protocol is geïmplementeerd in Azure Cosmos DB. U kunt toepassingen ontwikkelen met behulp van MongoDB en deze implementeren in productieomgevingen via de volledig beheerde en wereldwijd gedistribueerde Azure Cosmos DB-service. Zie [Protocolondersteuning voor MongoDB](mongodb-feature-support.md#mongodb-protocol-support) voor meer informatie over ondersteunde versies.

## <a name="what-is-the-benefit-of-using-azure-cosmos-db-for-mongodb-applications"></a>Wat is het voordeel van het gebruik van Azure Cosmos DB voor MongoDB-toepassingen?

**Elastisch schaalbare doorvoer en opslag**: U kunt de MongoDB-database eenvoudig omhoog of omlaag te schalen om te voldoen aan de behoeften van uw toepassingen. Uw gegevens worden opgeslagen op SSD-schijven (Solid State Disks) voor lage voorspelbare latenties. Azure Cosmos DB biedt ondersteuning voor MongoDB-verzamelingen waarvan de opslaggrootte en ingerichte doorvoer vrijwel onbeperkt kan worden geschaald. Naarmate uw toepassing groeit, kunt u Azure Cosmos DB probleemloos elastisch schalen met voorspelbare prestaties. 

**Replicatie voor meerdere regio's:** Azure Cosmos DB repliceert uw gegevens transparant naar alle regio's die u hebt gekoppeld aan uw MongoDB-account, zodat u toepassingen kunt ontwikkelen waarvoor wereldwijde toegang tot gegevens is vereist, en die tegelijkertijd zorgen voor een balans tussen consistentie, beschikbaarheid en prestaties, allemaal met overeenkomstige garanties. Azure Cosmos DB biedt transparante regionale failover met multihoming-API's en de mogelijkheid om doorvoer en opslag wereldwijd elastisch te schalen. Zie [Gegevens wereldwijd distribueren](distribute-data-globally.md) voor meer informatie.

**Geen serverbeheer**: u hoeft de MongoDB-databases niet te beheren en te schalen. Azure Cosmos DB is een volledig beheerde service. Dit betekent dat u de infrastructuur en virtuele machines niet zelf hoeft te beheren. Azure Cosmos DB is beschikbaar in meer dan 30 [Azure-regio’s](https://azure.microsoft.com/regions/services/).

**Instelbare consistentieniveaus:** omdat Azure Cosmos DB meerdere modellen API's heeft, zijn de consistentieniveaus van toepassing op het accountniveau, en het afdwingen van de consistentie wordt beheerd met elke API. Tot MongoDB 3.6 was er nog geen sprake van sessieconsistentie. Als u een MongoDB-API-account instelt voor gebruik van sessieconsistentie, wordt bij het gebruik van MongoDB-API’s een downgrade uitgevoerd naar het consistentieniveau: mogelijk. Als u een read-your-own-write-garantie nodig hebt voor een MongoDB-API-account, moet het standaardconsistentieniveau voor het account worden ingesteld op gebonden of gebonden-verouderd. Zie [Consistentieniveaus gebruiken om de beschikbaarheid en prestaties te maximaliseren](consistency-levels.md) voor meer informatie.

| Standaardconsistentieniveau voor Azure Cosmos DB |   Mongo API (3.4) |
|---|---|
|Mogelijk| Mogelijk |
|Consistent prefix| Mogelijk met consistente volgorde |
|Sessie| Mogelijk met consistente volgorde |
|Gebonden veroudering| Sterk |
| Sterk | Sterk |

**Automatisch indexeren:** alle eigenschappen in documenten in de MongoDB-database worden standaard automatisch geïndexeerd in Azure Cosmos DB. Er wordt geen schema verwacht of vereist en er hoeven geen secundaire indexen te worden gemaakt. Bovendien maakt de unieke index een uniekheidsbeperking mogelijk in elk documentveld dat al automatisch is geïndexeerd in Azure Cosmos DB.

**Enterprise-kwaliteit**: Azure Cosmos DB biedt ondersteuning voor meerdere lokale replica’s voor een beschikbaarheid van 99.99% en gegevensbeveiliging bij lokale en regionale fouten. Azure Cosmos DB beschikt over [nalevingscertificeringen](https://www.microsoft.com/trustcenter) en beveiligingsfuncties op zakelijk niveau. 

Senior Program Manager Aleksey Savateyev geeft u hier meer informatie over in deze video.

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T136/player]
> 

## <a name="how-to-get-started"></a>Hoe gaat u aan de slag

Volg de stappen in de MongoDB-quickstarts om een Azure Cosmos DB-account te maken en uw bestaande MongoDB-toepassing te migreren om Azure Cosmos DB te gebruiken, of bouw een nieuwe:

* [Een bestaande Node.js MongoDB-web-app migreren](create-mongodb-nodejs.md).
* [Een MongoDB-API-web-app ontwikkelen met .NET en Azure Portal](create-mongodb-dotnet.md)
* [Een MongoDB-API-console-app ontwikkelen met Java en Azure Portal](create-mongodb-java.md)

## <a name="next-steps"></a>Volgende stappen

Informatie over de Azure Cosmos DB MongoDB-API is geïntegreerd in de algemene documentatie voor Azure Cosmos DB, maar hier zijn enkele aanwijzingen om u op gang te helpen:

* Volg de zelfstudie [Verbinding maken met een MongoDB-account](connect-mongodb-account.md) voor informatie over het ophalen van verbindingsreeksinformatie voor uw account.
* Volg de zelfstudie [Studio 3T (MongoChef) gebruiken met Azure Cosmos DB](mongodb-mongochef.md) om te leren hoe u een verbinding maakt tussen een Azure Cosmos DB-database en de MongoDB-app in Studio 3T.
* Volg de zelfstudie [Gegevens migreren naar Azure Cosmos DB met protocolondersteuning voor MongoDB](mongodb-migrate.md) om uw gegevens te importeren naar een API voor een MongoDB-database.
* Maak verbinding met een API voor een MongoDB-account met behulp van [Robomongo](mongodb-robomongo.md).
* Ontdek hoeveel RU’s uw bewerkingen gebruiken met de [opdracht GetLastRequestStatistics en de metrische gegevens in Azure Portal](set-throughput.md#GetLastRequestStatistics).
* Meer informatie over het [configureren van leesvoorkeuren voor wereldwijd gedistribueerde apps](../cosmos-db/tutorial-global-distribution-mongodb.md).
