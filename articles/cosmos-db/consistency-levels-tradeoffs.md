---
title: Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus in Azure Cosmos DB
description: Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus in Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/08/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: a76e277bf56861bcaefb5bf7f8b3b3bc03ad1164
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59266423"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Compromissen tussen consistentie, beschikbaarheid en prestaties 

Gedistribueerde databases die afhankelijk van de replicatie voor hoge beschikbaarheid, lage latentie, of beide zijn moeten ontwikkelt. De nadelen zijn tussen lezen van consistentie en beschikbaarheid, latentie en doorvoer.

Azure Cosmos DB nadert de consistentie van gegevens als een breed scala aan mogelijkheden. Deze benadering omvat meer opties dan de twee extreme van sterke en uiteindelijke consistentie. U kunt kiezen uit vijf goed gedefinieerde modellen op het spectrum consistentie. Van sterk naar zwak, de modellen zijn:

- *Sterk*
- *Gebonden veroudering*
- *Sessie*
- *Consistent voorvoegsel*
- *Mogelijk*

Elk model optimalisatie van de beschikbaarheid en prestaties biedt en wordt ondersteund door een uitgebreide Sla's.

## <a name="consistency-levels-and-latency"></a>Consistentieniveaus en latentie

De leeslatentie voor alle consistentieniveaus is altijd gegarandeerd minder dan 10 milliseconden in het 99e percentiel. Deze leeslatentie wordt ondersteund door de SLA. De gemiddelde latentie in het 50e percentiel, lezen, is doorgaans 2 milliseconden of minder. Azure Cosmos-accounts die meerdere regio's beslaan en zijn geconfigureerd met een sterke consistentie vormen een uitzondering op deze gegarandeerd.

De latentie schrijven voor alle consistentieniveaus is altijd gegarandeerd minder dan 10 milliseconden in het 99e percentiel. Deze schrijflatentie wordt ondersteund door de SLA. De gemiddelde schrijflatentie, in het 50e percentiel, is doorgaans 5 milliseconden of minder.

Voor Azure Cosmos-accounts die zijn geconfigureerd met sterke consistentie met meer dan één regio, is de schrijflatentie gegarandeerd minder dan twee keer retourtijd (RTT) tussen de twee verst regio's, plus 10 milliseconden in het 99e percentiel.

De exacte RTT latentie is een functie van de snelheid van licht afstand en de Azure-netwerktopologie. Azure-netwerken biedt geen eventuele latentie Sla's voor de RTT tussen elke twee Azure-regio's. Voor uw Azure Cosmos-account, worden replicatielatentie weergegeven in de Azure-portal. U kunt de Azure portal (Ga naar de blade met metrische gegevens) gebruiken voor het bewaken van de replicatielatentie tussen verschillende regio's die gekoppeld aan uw Azure Cosmos-account zijn.

## <a name="consistency-levels-and-throughput"></a>Consistentieniveaus en doorvoer

- Voor hetzelfde aantal aanvraageenheden bieden de sessie, consistent voorvoegsel en uiteindelijke consistentieniveaus ongeveer twee keer de lezen doorvoer in vergelijking met sterke en gebonden veroudering.

- Voor een bepaald type schrijfbewerking, zoals invoegen, vervangen, upsert en verwijderen, is de doorvoer van schrijfbewerkingen voor aanvraageenheden gelijk voor alle consistentieniveaus.

## <a id="rto"></a>Consistentie niveaus en gegevens duurzaamheid

Binnen de databaseomgeving van een wereldwijd gedistribueerde moet u er een directe relatie is tussen de consistentie van niveau en gegevens duurzaamheid met een regiobrede uitval. Tijdens het ontwikkelen van uw plan voor bedrijfscontinuïteit, moet u inzicht in de maximaal acceptabele tijd voordat de toepassing volledig is hersteld na een storing. De tijd die nodig is voor een toepassing om volledig te herstellen wordt ook wel **beoogde hersteltijd** (**RTO**). U moet ook weten wat de maximale periode van recente Gegevensupdates de toepassing kan tolereren verliezen tijdens het herstellen na een storing. De periode van updates die u in het ergste geval kan kwijtraken staat bekend als **beoogd herstelpunt** (**RPO**).

De onderstaande tabel definieert u de relatie tussen consistentie-model en gegevens duurzaamheid bij grote storing regio. Het is belangrijk te weten dat in een gedistribueerd systeem, zelfs met sterke consistentie is het onmogelijk om een gedistribueerde database met een RPO en de RTO gelijk is aan nul vanwege het CAP-Theorema hebt. Zie voor meer informatie over de reden waarom, [consistentieniveaus in Azure Cosmos DB](consistency-levels.md).

|**Regio's**|**Replicatiemodus**|**Consistentieniveau**|**RPO**|**RTO**|
|---------|---------|---------|---------|---------|
|1|Enkele of meerdere masters|Een Consistentieniveau|< 240 minuten|<1 Week|
|>1|Single Master|Sessie, Consistent voorvoegsel, uiteindelijke|< 15 minuten|< 15 minuten|
|>1|Single Master|Gebonden veroudering|*K* & *T*|< 15 minuten|
|>1|Meerdere masters|Sessie, Consistent voorvoegsel, uiteindelijke|< 15 minuten|0|
|>1|Meerdere masters|Gebonden veroudering|*K* & *T*|0|
|>1|Enkele of meerdere masters|Sterk|0|< 15 minuten|

*K* = het aantal *'K'* versies (dat wil zeggen, updates) van een item.

*T* = het tijdsinterval *"T"* sinds de laatste update.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over globale distributie- en nadelen van de algemene consistentie in gedistribueerde systemen. Zie de volgende artikelen:

- [Optimalisatie van de consistentie in het ontwerp van moderne, gedistribueerde database-systemen](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Hoge beschikbaarheid](high-availability.md)
- [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
