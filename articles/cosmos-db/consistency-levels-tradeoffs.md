---
title: Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus | Microsoft Docs
description: Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus in Azure Cosmos DB.
keywords: consistentie, prestaties, azure cosmosdb, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 061a7c223d0e1c6fa7384d7defe9f84f470236ce
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244071"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels"></a>Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus

Gedistribueerde databases die afhankelijk zijn van replicatie voor hoge beschikbaarheid, lage latentie of beide, moet het fundamentele verschil tussen het lezen van consistentie versus availability1, latency2 en doorvoer. Azure Cosmos DB nadert de consistentie van gegevens als een spectrum van de opties in plaats van de twee extreme varianten strong en uiteindelijke consistentie. Cosmos DB kan ontwikkelaars kiezen tussen vijf duidelijk gedefinieerde consistentiemodellen uit het spectrum consistentie (sterk naar zwak) – **sterke**, **gebonden veroudering**, **sessie** , **consistent voorvoegsel**, en **uiteindelijke**. Elk van de vijf consistentiemodellen wissen optimalisatie van de beschikbaarheid en prestaties biedt en worden ondersteund met uitgebreide Sla's.

## <a name="consistency-levels-and-latency"></a>Consistentieniveaus en latentie

- De **leeslatentie** voor alle consistentieniveaus is altijd gegarandeerd minder dan 10 milliseconden in het 99e percentiel, en wordt ondersteund door de SLA. Het gemiddelde (in het 50e percentiel) lezen latentie is doorgaans 2 milliseconden of minder.
- Met uitzondering van Cosmos-accounts die verschillende regio's omvatten en geconfigureerd met de sterke consistentie, de **schrijflatentie** voor alle consistentieniveaus is altijd gegarandeerd minder dan 10 milliseconden in het 99e percentiel, en wordt ondersteund door de SLA. De latentie (in het 50e percentiel) gemiddelde schrijven is doorgaans 5 milliseconden of minder.
- Voor de Cosmos-accounts met meerdere regio's die zijn geconfigureerd met sterke consistentie (momenteel in Preview), de **schrijflatentie** is gegarandeerd minder dan < (2 * RTT) + 10 milliseconden in het 99e percentiel. De RTT tussen het verst van een van de twee regio's die zijn gekoppeld aan uw Cosmos-account. De exacte RTT latentie is een functie van de snelheid van licht afstand en de exacte Azure netwerktopologie. Azure-netwerken biedt geen eventuele latentie Sla's voor de RTT tussen elke twee Azure-regio's. Replicatielatentie van cosmos DB worden weergegeven in Azure portal voor uw Cosmos-account, zodat u kunt voor het bewaken van de replicatielatentie tussen verschillende regio's die zijn gekoppeld aan uw Cosmos-account.

## <a name="consistency-levels-and-throughput"></a>Consistentieniveaus en doorvoer

- Geef ongeveer 2 X leesdoorvoer in vergelijking met sterke en gebonden veroudering voor dezelfde hoeveelheid RU, sessie, consistent voorvoegsel en uiteindelijke consistentie.
- Voor een bepaald type schrijfbewerking (bijvoorbeeld Insert, Replace, Upsert, verwijderen, enzovoort) is de doorvoer van schrijfbewerkingen voor ru's gelijk voor alle consistentieniveaus.

## <a name="consistency-levels-and-durability"></a>Consistentieniveaus en duurzaamheid

Voordat een schrijfbewerking is bevestigd naar de client, de gegevens is blijvend zijn doorgevoerd op door een quorum van replicaties in de regio de schrijfbewerkingen accepteren. Bovendien, als de container is geconfigureerd met consistente indexeringsbeleid, de index is ook synchroon bijgewerkt, gerepliceerd en blijvend zijn doorgevoerd door het quorum van replica's voordat het schrijven naar de client is bevestigd. 

De volgende tabel geeft een overzicht van het venster van de mogelijke verlies van gegevens in het geval van een regionaal noodgeval voor de Cosmos-accounts die verschillende regio's omvatten.

| **Consistentieniveau** | **Venster van mogelijk gegevensverlies in het geval van een regionaal noodgeval** |
| - | - |
| Sterk | Nul |
| Gebonden veroudering | Beperkt tot de "veroudering venster" geconfigureerd door de ontwikkelaar van de Cosmos-account |
| Sessie | Maximaal 5 seconden |
| Consistent prefix | Maximaal 5 seconden |
| Mogelijk | Maximaal 5 seconden |

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u meer informatie over globale distributie- en nadelen van de algemene consistentie in gedistribueerde systemen met behulp van de volgende artikelen:

* [Optimalisatie van de consistentie in het ontwerp van moderne, gedistribueerde database-systemen](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Hoge beschikbaarheid](high-availability.md)
* [SLA voor cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
