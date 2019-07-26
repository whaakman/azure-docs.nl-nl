---
title: Regionale aanwezigheid met Azure Cosmos DB
description: Dit artikel wordt uitgelegd over de regionale aanwezigheid van Azure Cosmos DB en andere cloud-omgevingen.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: rimman
ms.custom: seodec18
ms.openlocfilehash: 775ca8c57a9a444df2f7d0a4df4224b7eba9d677
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68384042"
---
# <a name="regional-presence-with-azure-cosmos-db"></a>Regionale aanwezigheid met Azure Cosmos DB

Azure Cosmos DB is een Foundational service in Azure en is standaard altijd beschikbaar in alle regio's, waar Azure beschikbaar is. Azure is momenteel beschikbaar in [54 regio's](https://azure.microsoft.com/global-infrastructure/regions/) over de hele wereld. 

[![Regio's waar Azure Cosmos DB beschikbaar is](./media/regional-presence/regional-presence.png)](./media/regional-presence/regional-presence.png#lightbox)

Cosmos DB is beschikbaar in alle vijf afzonderlijke Azure-cloudomgevingen beschikbaar voor klanten:

* **Openbare Azure** cloud, die wereldwijd beschikbaar is.

* **Azure China 21vianet** is beschikbaar via een unieke samen werking tussen micro soft en 21vianet, een van de grootste Internet providers van het land in China.

* **Azure Duitsland** verzorgt services onder een gegevensbeheerdersmodel, die zorgt ervoor dat de klant gegevens blijven in Duitsland onder het beheer van T-Systems International GmbH, een dochteronderneming van Deutsche Telecom, die fungeert als de Duitse Gegevensbeheerder.

* **Azure Government** is beschikbaar in vier verschillende regio's in de Verenigde Staten voor Amerikaanse overheidsinstanties en hun partners. 

* **Azure Government voor ministerie van defensie (DoD)** is beschikbaar in twee regio's in de Verenigde Staten van het Amerikaanse ministerie van defensie.

## <a name="regional-presence-with-global-distribution"></a>Regionale aanwezigheid met wereldwijde distributie

Alle Api's die worden weer gegeven door Azure Cosmos DB (inclusief SQL, MongoDB, Cassandra, Gremlin en Table) zijn standaard beschikbaar in alle Azure-regio's. U kunt bijvoorbeeld MongoDB-en Cassandra-Api's weer gegeven door Azure Cosmos DB niet alleen in alle wereld wijde Azure-regio's, maar ook in soevereine Clouds, zoals China, Duitsland, overheids instellingen en DoD-regio's (Ministerie van defensie).

Azure Cosmos DB is een [wereld wijd](distribute-data-globally.md) gedistribueerde database service. U kunt een willekeurig aantal Azure-regio's koppelen aan uw Azure Cosmos-account en uw gegevens worden automatisch en transparant gerepliceerd. U kunt toevoegen of verwijderen van een regio voor uw Azure Cosmos-account op elk gewenst moment. Dankzij de mogelijkheid van kant en klare wereldwijde distributie en multi-onder de knie replicatie-protocol biedt Azure Cosmos DB minder dan 10 ms lezen en schrijven van latentie in het 99e percentiel, 99,999 lezen en schrijven van beschikbaarheid en de mogelijkheid om elastisch te schalen ingericht de doorvoer voor leest en schrijft in alle regio's die zijn gekoppeld aan uw Azure Cosmos-account. Azure Cosmos DB biedt ook vijf duidelijk gedefinieerde consistentiemodellen en u kunt een specifieke consistentiemodel toepassen op uw gegevens. Ten slotte is Azure Cosmos DB de enige database service in de branche met een uitgebreide [Service Level Agreement (Sla)](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/) die voorziet in de ingerichte door Voer, latentie bij het 99e percentiel, hoge Beschik baarheid en consistentie. De bovenstaande mogelijkheden zijn beschikbaar in alle Azure-Clouds.

## <a name="next-steps"></a>Volgende stappen

U kunt nu meer informatie over de basis concepten van Azure Cosmos DB met de volgende artikelen:

* [Verdeling van de globale gegevens](distribute-data-globally.md)
* [Een Azure Cosmos DB-account beheren](manage-account.md)
* [Doorvoer inrichten voor Azure Cosmos-containers en -databases](set-throughput.md)
* [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
