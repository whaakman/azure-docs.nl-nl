---
title: Regionale aanwezigheid met Azure Cosmos DB
description: Dit artikel wordt uitgelegd over de regionale aanwezigheid van Azure Cosmos DB en andere cloud-omgevingen.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.custom: seodec18
ms.openlocfilehash: 32e69be95226d06f55d79ff5e245c76c3087efd9
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55461284"
---
# <a name="regional-presence-with-azure-cosmos-db"></a>Regionale aanwezigheid met Azure Cosmos DB

Azure is momenteel beschikbaar in [54 regio's](https://azure.microsoft.com/global-infrastructure/regions/) over de hele wereld. Azure Cosmos DB is een fundamentele service in Azure en is beschikbaar in alle regio's waar Azure beschikbaar is.

[![Regio's waar Azure Cosmos DB beschikbaar is](./media/regional-presence/regional-presence.png)](./media/regional-presence/regional-presence.png#lightbox)

Cosmos DB is beschikbaar in alle vijf afzonderlijke Azure-cloudomgevingen beschikbaar voor klanten:

* **Openbare Azure** cloud, die wereldwijd beschikbaar is.

* **Azure China** is beschikbaar via een uniek partnerschap tussen Microsoft en 21Vianet, een van de grootste internetproviders van het land.

* **Azure Duitsland** verzorgt services onder een gegevensbeheerdersmodel, die zorgt ervoor dat de klant gegevens blijven in Duitsland onder het beheer van T-Systems International GmbH, een dochteronderneming van Deutsche Telecom, die fungeert als de Duitse Gegevensbeheerder.

* **Azure Government** is beschikbaar in vier verschillende regio's in de Verenigde Staten voor Amerikaanse overheidsinstanties en hun partners. 

* **Azure Government voor afdeling van of** is beschikbaar in twee regio's in de Verenigde Staten naar het US Department of Defense.

## <a name="regional-presence-with-global-distribution"></a>Regionale aanwezigheid met wereldwijde distributie

Verschillende API's die worden weergegeven door Azure Cosmos DB (inclusief SQL, MongoDB, Cassandra, Gremlin en Azure Table-opslag) zijn beschikbaar in alle Azure-regio's. Bijvoorbeeld, kunt u MongoDB hebben en Cassandra-API's beschikbaar worden gemaakt door Azure Cosmos DB niet alleen in alle van de globale Azure-regio's, maar ook in Azure-soevereine regio's, zoals China, Duitsland, overheid en ministerie van defensie (DoD) regio's.

Azure Cosmos DB is een [wereldwijd gedistribueerde](distribute-data-globally.md) database. U kunt een willekeurig aantal Azure-regio's koppelen aan uw Azure Cosmos-account en uw gegevens worden automatisch en transparant gerepliceerd. U kunt toevoegen of verwijderen van een regio voor uw Azure Cosmos-account op elk gewenst moment. Dankzij de mogelijkheid van kant en klare wereldwijde distributie en multi-onder de knie replicatie-protocol biedt Azure Cosmos DB minder dan 10 ms lezen en schrijven van latentie in het 99e percentiel, 99,999 lezen en schrijven van beschikbaarheid en de mogelijkheid om elastisch te schalen ingericht de doorvoer voor leest en schrijft in alle regio's die zijn gekoppeld aan uw Azure Cosmos-account. Azure Cosmos DB biedt ook vijf duidelijk gedefinieerde consistentiemodellen en u kunt een specifieke consistentiemodel toepassen op uw gegevens. Ten slotte is Azure Cosmos DB de enige database-service in de branche waarmee een uitgebreide Service Level Agreement (SLA) dat bestaat uit ingerichte doorvoer en latentie in het 99e percentiel, hoge beschikbaarheid en consistentie.

## <a name="next-steps"></a>Volgende stappen

U kunt nu meer informatie over andere concepten van Azure Cosmos DB met de volgende artikelen:

* [Verdeling van de globale gegevens](distribute-data-globally.md)
* [Een Azure Cosmos DB-account beheren](manage-account.md)
* [Doorvoer inrichten voor Azure Cosmos-containers en -databases](set-throughput.md)
* [SLA voor cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
