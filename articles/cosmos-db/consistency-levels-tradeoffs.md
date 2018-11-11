---
title: Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus in Azure Cosmos DB | Microsoft Docs
description: Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus in Azure Cosmos DB.
keywords: consistentie, prestaties, azure cosmosdb, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 0e4105d6f56a8eb45a83e970c85319cf25041781
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514771"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels-in-azure-cosmos-db"></a>Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus in Azure Cosmos DB

Gedistribueerde databases die afhankelijk zijn van replicatie voor hoge beschikbaarheid, lage latentie of beide, moet het fundamentele verschil tussen het lezen van consistentie en beschikbaarheid, latentie en doorvoer. Azure Cosmos DB nadert de consistentie van gegevens als een spectrum van de opties in plaats van de twee extreme varianten strong en uiteindelijke consistentie. Cosmos DB kan ontwikkelaars kiezen tussen de vijf duidelijk gedefinieerde consistentiemodellen uit het spectrum consistentie (sterk naar zwak) – **sterke**, **gebonden veroudering**, **sessie** , **consistent voorvoegsel**, en **uiteindelijke**. Elk van de vijf consistentiemodellen en nadelen van de beschikbaarheid en prestaties biedt en worden ondersteund met uitgebreide Sla's.

## <a name="consistency-levels-and-latency"></a>Consistentieniveaus en latentie

- De **leeslatentie** voor alle consistentieniveaus is altijd gegarandeerd minder dan 10 milliseconden in het 99e percentiel, en wordt ondersteund door de SLA. Het gemiddelde (in het 50e percentiel) lezen latentie is doorgaans 2 milliseconden of minder.

- Met uitzondering van de Cosmos-accounts die meerdere regio's beslaan en zijn geconfigureerd met de sterke consistentie, de **schrijflatentie** voor de resterende consistentie niveaus is altijd gegarandeerd minder dan 10 milliseconden op de 99th percentiel. Deze schrijflatentie wordt ondersteund door de SLA. De latentie (in het 50e percentiel) gemiddelde schrijven is doorgaans 5 milliseconden of minder.

- Voor de Cosmos-accounts waarvoor meerdere regio's die zijn geconfigureerd met sterke consistentie (momenteel in preview), de **schrijflatentie** is gegarandeerd minder dan < (2 * Round-trip tijd/RTT) + 10 milliseconden in het 99e percentiel. De RTT tussen een van de twee verst regio's die zijn gekoppeld aan uw Cosmos-account. De exacte RTT latentie is een functie van de snelheid van licht afstand en de exacte Azure netwerktopologie. Azure-netwerken biedt geen eventuele latentie Sla's voor de RTT tussen elke twee Azure-regio's. Replicatielatentie van cosmos DB worden weergegeven in Azure portal voor uw Cosmos-account, zodat u kunt voor het bewaken van de replicatielatentie tussen verschillende regio's die zijn gekoppeld aan uw Cosmos-account.

## <a name="consistency-levels-and-throughput"></a>Consistentieniveaus en doorvoer

- Voor hetzelfde aantal aanvraageenheden bieden de sessie, consistent voorvoegsel en uiteindelijke consistentie-niveaus ongeveer 2 X doorvoer in vergelijking met sterke en gebonden veroudering lezen.

- Voor een bepaald type schrijfbewerking zoals invoegen, vervangen, upsert, verwijderen, enz. is de doorvoer van schrijfbewerkingen voor aanvraageenheden gelijk voor alle consistentieniveaus.

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u meer informatie over globale distributie- en nadelen van de algemene consistentie in gedistribueerde systemen met behulp van de volgende artikelen:

* [Optimalisatie van de consistentie in het ontwerp van moderne, gedistribueerde database-systemen](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Hoge beschikbaarheid](high-availability.md)
* [SLA voor cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
