---
title: Regionale aanwezigheid met Azure Cosmos DB
description: Dit artikel wordt uitgelegd over de regionale aanwezigheid van Azure Cosmos DB en andere cloud-omgevingen.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.author: rimman
ms.custom: seodec18
ms.openlocfilehash: 851da72a308b164b9a01b7ccfa3d541fc06810ba
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762188"
---
# <a name="regional-presence-with-azure-cosmos-db"></a>Regionale aanwezigheid met Azure Cosmos DB

Azure Cosmos DB is een fundamentele service in Azure, en standaard is altijd beschikbaar in alle regio's waar Azure beschikbaar is. Azure is momenteel beschikbaar in [54 regio's](https://azure.microsoft.com/global-infrastructure/regions/) over de hele wereld. 

[![Regio's waar Azure Cosmos DB beschikbaar is](./media/regional-presence/regional-presence.png)](./media/regional-presence/regional-presence.png#lightbox)

Cosmos DB is beschikbaar in alle vijf afzonderlijke Azure-cloudomgevingen beschikbaar voor klanten:

* **Openbare Azure** cloud, die wereldwijd beschikbaar is.

* **Azure China 21Vianet** is beschikbaar via een uniek partnerschap tussen Microsoft en 21Vianet, een van de grootste internetproviders van het land in China.

* **Azure Duitsland** verzorgt services onder een gegevensbeheerdersmodel, die zorgt ervoor dat de klant gegevens blijven in Duitsland onder het beheer van T-Systems International GmbH, een dochteronderneming van Deutsche Telecom, die fungeert als de Duitse Gegevensbeheerder.

* **Azure Government** is beschikbaar in vier verschillende regio's in de Verenigde Staten voor Amerikaanse overheidsinstanties en hun partners. 

* **Azure Government voor Ministerie van defensie (DoD)** is beschikbaar in twee regio's in de Verenigde Staten naar het US Department of Defense.

## <a name="regional-presence-with-global-distribution"></a>Regionale aanwezigheid met wereldwijde distributie

Alle API's die worden weergegeven door Azure Cosmos DB (inclusief SQL, MongoDB, Cassandra, Gremlin en tabel) zijn beschikbaar in alle Azure-regio's standaard. Bijvoorbeeld, kunt u MongoDB hebben en Cassandra-API's beschikbaar worden gemaakt door Azure Cosmos DB niet alleen in alle van de globale Azure-regio's, maar ook in soevereine clouds, zoals China, Duitsland, overheid en ministerie van defensie (DoD) regio's.

Azure Cosmos DB is een [wereldwijd gedistribueerde](distribute-data-globally.md) database-service. U kunt een willekeurig aantal Azure-regio's koppelen aan uw Azure Cosmos-account en uw gegevens worden automatisch en transparant gerepliceerd. U kunt toevoegen of verwijderen van een regio voor uw Azure Cosmos-account op elk gewenst moment. Dankzij de mogelijkheid van kant en klare wereldwijde distributie en multi-onder de knie replicatie-protocol biedt Azure Cosmos DB minder dan 10 ms lezen en schrijven van latentie in het 99e percentiel, 99,999 lezen en schrijven van beschikbaarheid en de mogelijkheid om elastisch te schalen ingericht de doorvoer voor leest en schrijft in alle regio's die zijn gekoppeld aan uw Azure Cosmos-account. Azure Cosmos DB biedt ook vijf duidelijk gedefinieerde consistentiemodellen en u kunt een specifieke consistentiemodel toepassen op uw gegevens. Ten slotte Azure Cosmos DB is de enige databaseservice in de branche waarmee een uitgebreide [Service Level Agreement (SLA)](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/) allesomvattende ingerichte doorvoer, latentie in het 99e percentiel, hoge beschikbaarheid, en consistentie. De bovenstaande mogelijkheden zijn beschikbaar in alle Azure-clouds.

## <a name="next-steps"></a>Volgende stappen

U kunt nu meer informatie over de basisconcepten van Azure Cosmos DB met de volgende artikelen:

* [Verdeling van de globale gegevens](distribute-data-globally.md)
* [Een Azure Cosmos DB-account beheren](manage-account.md)
* [Doorvoer inrichten voor Azure Cosmos-containers en -databases](set-throughput.md)
* [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
