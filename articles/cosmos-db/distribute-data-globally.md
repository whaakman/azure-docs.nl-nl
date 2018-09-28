---
title: Gegevens globaal distribueren met Azure Cosmos DB | Microsoft Docs
description: Meer informatie over wereldwijde geo-replicatie, meerdere masters, failover en gegevens herstellen met behulp van de globale databases van Azure Cosmos DB, een wereldwijd gedistribueerde, multi-model databaseservice.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mjbrown
ms.openlocfilehash: 227c243d82665dc533e3bfa6a1fe3e9bb775a262
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47408892"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>Globale gegevensdistributie met Azure Cosmos DB

Azure is overal - heeft een wereldwijde footprint in meer dan 50 geografische regio's en continu wordt uitgebreid. Met de wereldwijde aanwezigheid en ondersteuning voor meerdere masters is een van de unieke mogelijkheden die Azure voor de ontwikkelaars biedt de mogelijkheid om te bouwen, implementeren en beheren van wereldwijd gedistribueerde toepassingen eenvoudig.

[Azure Cosmos DB](../cosmos-db/introduction.md) is de wereldwijd gedistribueerde databaseservice met meerdere modellen van Microsoft voor essentiële toepassingen. Azure Cosmos DB biedt kant en klare wereldwijde distributie, [elastisch schalen van doorvoer en opslag](../cosmos-db/partition-data.md) lezen over de hele wereld, één cijfer en een latentie van schrijfbewerkingen in het 99e percentiel, [goed gedefinieerde consistentie modellen](consistency-levels.md), gegarandeerd hoge beschikbaarheid, ondersteund door [toonaangevende uitgebreide Sla's](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Azure Cosmos DB [al uw gegevens worden automatisch geïndexeerd](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) zonder u te maken krijgt met schema's of indexbeheer.

## <a name="global-distribution-with-multi-master"></a>Globale distributie met meerdere masters

Als een cloudservice is Azure Cosmos DB zorgvuldig ontworpen ter ondersteuning van multitenancy, wereldwijde distributie en meerdere masters voor document, sleutel / waarde-, grafiek en kolom-familie gegevensmodellen.

![Azure Cosmos DB-container gepartitioneerd en verdeeld over drie regio 's](./media/distribute-data-globally/global-apps.png)

**Een enkele Azure Cosmos DB-container gepartitioneerd en verdeeld over meerdere Azure-regio 's**

Zoals we hebben geleerd tijdens het maken van Azure Cosmos DB, mag niet toe te voegen wereldwijde distributie niet hoeft bezig. Kan niet 'bout-in' op een systeem van de database 'één site'. De mogelijkheden die door een wereldwijd gedistribueerde database omvatten naast de functies die worden geboden door een traditionele geografische disaster recovery (Geo-DR) die worden aangeboden door 'single-site' databases. Geo-DR-mogelijkheid bieden één site-databases zijn een strikte subset van wereldwijd gedistribueerde databases.

Met Azure Cosmos DB kant en klare wereldwijde distributie hebt het ontwikkelaars niet voor het bouwen van hun eigen hulpprogramma replicatie met behulp van het patroon Lambda via het databaselogboek of door het uitvoeren van "dubbele schrijfbewerkingen" in meerdere regio's. We doen *niet* raden deze methoden, omdat het onmogelijk is om de juistheid van deze methoden en leveren geluidskaart Sla's.

## <a id="Next Steps"></a>Volgende stappen

* [Hoe Azure Cosmos DB kunt kant en klare wereldwijde distributie](distribute-data-globally-turnkey.md)

* [Azure Cosmos DB globale distributie van de belangrijkste voordelen](distribute-data-globally-benefits.md)

* [Azure Cosmos DB globale databasereplicatie configureren](tutorial-global-distribution-sql-api.md)

* [Het inschakelen van meerdere masters voor Azure Cosmos DB-accounts](enable-multi-master.md)

* [Hoe automatische en handmatige failover in Azure Cosmos DB werkt](regional-failover.md)

* [Informatie over conflictoplossing in Azure Cosmos DB](multi-master-conflict-resolution.md)

* [Met behulp van meerdere masters met open-source NoSQL-databases](multi-master-oss-nosql.md)
