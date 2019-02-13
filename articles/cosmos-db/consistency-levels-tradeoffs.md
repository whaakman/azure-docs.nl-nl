---
title: Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus in Azure Cosmos DB
description: Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: ee0dc1bec39bf95cbf4f3bf7ecea92b877a78b88
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113750"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Compromissen tussen consistentie, beschikbaarheid en prestaties 

Gedistribueerde databases die afhankelijk van de replicatie voor hoge beschikbaarheid, lage latentie, of beide zijn moeten ontwikkelt. De nadelen zijn tussen lezen van consistentie en beschikbaarheid, latentie en doorvoer.

Azure Cosmos DB nadert de consistentie van gegevens als een breed scala aan mogelijkheden. Deze benadering omvat meer opties dan de twee extreme van sterke en uiteindelijke consistentie. U kunt kiezen uit vijf goed gedefinieerde modellen op het spectrum consistentie. Van sterk naar zwak, de modellen zijn:

- Sterk
- Gebonden veroudering
- Sessie
- Consistent voorvoegsel
- Mogelijk

Elk model optimalisatie van de beschikbaarheid en prestaties biedt en wordt ondersteund door een uitgebreide SLA.

## <a name="consistency-levels-and-latency"></a>Consistentieniveaus en latentie

- De leeslatentie voor alle consistentieniveaus is altijd gegarandeerd minder dan 10 milliseconden in het 99e percentiel. Deze leeslatentie wordt ondersteund door de SLA. De gemiddelde latentie in het 50e percentiel, lezen, is doorgaans 2 milliseconden of minder. Azure Cosmos-accounts die meerdere regio's beslaan en zijn geconfigureerd met een sterke consistentie vormen een uitzondering op deze gegarandeerd.

- De latentie schrijven voor de resterende consistentieniveaus is altijd gegarandeerd minder dan 10 milliseconden in het 99e percentiel. Deze schrijflatentie wordt ondersteund door de SLA. De gemiddelde schrijflatentie, in het 50e percentiel, is doorgaans 5 milliseconden of minder.

Sommige Azure-Cosmos-accounts mogelijk meerdere regio's die zijn geconfigureerd met sterke consistentie. In dit geval is de schrijflatentie gegarandeerd minder dan twee keer retourtijd (RTT) plus 10 milliseconden in het 99e percentiel. De RTT tussen een van de twee verst regio's is gekoppeld aan uw Azure Cosmos-account. Dit is gelijk aan de RTT tussen een van de twee verst regio's die zijn gekoppeld aan uw Azure Cosmos-account. Deze optie is momenteel in preview.

De exacte RTT latentie is een functie van de snelheid van licht afstand en de Azure-netwerktopologie. Azure-netwerken biedt geen eventuele latentie Sla's voor de RTT tussen elke twee Azure-regio's. Voor uw Azure Cosmos-account, worden replicatielatentie weergegeven in de Azure-portal. U kunt de Azure-portal gebruiken voor het bewaken van de replicatielatentie tussen verschillende regio's die gekoppeld aan uw account zijn.

## <a name="consistency-levels-and-throughput"></a>Consistentieniveaus en doorvoer

- Voor hetzelfde aantal aanvraageenheden bieden de sessie, consistent voorvoegsel en uiteindelijke consistentieniveaus ongeveer twee keer de lezen doorvoer in vergelijking met sterke en gebonden veroudering.

- Voor een bepaald type schrijfbewerking, zoals invoegen, vervangen, upsert en verwijderen, is de doorvoer van schrijfbewerkingen voor aanvraageenheden gelijk voor alle consistentieniveaus.

## <a name="consistency-levels-and-data-durability"></a>Consistentie niveaus en gegevens duurzaamheid

Binnen de databaseomgeving van een wereldwijd gedistribueerde moet u er een directe relatie is tussen de consistentie van niveau en gegevens duurzaamheid met een regiobrede uitval. De tabel definieert de relatie tussen de relatie tussen consistentie-model en gegevens duurzaamheid bij grote storing regio. Het is belangrijk te weten dat in een gedistribueerd systeem, zelfs met sterke consistentie is het onmogelijk om een gedistribueerde database met en RPO en RTO gelijk is aan nul vanwege het CAP-Theorema hebben. Zie voor meer informatie over waarom, [consistentieniveaus in Azure Cosmos DB](consistency-levels.md).

|**Regio('s)**|**Replicatiemodus**|**Consistentieniveau**|**RPO**|**RTO**|
|---------|---------|---------|---------|---------|
|1|Enkele of meerdere masters|Een Consistentieniveau|< 240 minuten|<1 Week|
|>1|Single Master|Sessie, Consistent voorvoegsel, uiteindelijke|< 15 minuten|< 15 minuten|
|>1|Single Master|Gebonden veroudering|K & T*|< 15 minuten|
|>1|Meerdere masters|Sessie, Consistent voorvoegsel, uiteindelijke|< 15 minuten|0|
|>1|Meerdere masters|Gebonden veroudering|K & T*|0|
|>1|Enkele of meerdere masters|Sterk|0|< 15 minuten|

* K & T = het nummer van 'K' versies (updates) van een item. Of "T" tijdsinterval.



## <a name="next-steps"></a>Volgende stappen

Meer informatie over globale distributie- en nadelen van de algemene consistentie in gedistribueerde systemen. Zie de volgende artikelen:

- [Optimalisatie van de consistentie in het ontwerp van moderne, gedistribueerde database-systemen](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
- [Hoge beschikbaarheid](high-availability.md)
- [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
