---
title: Kies de juiste consistentieniveau voor uw toepassing die gebruikmaakt van Azure Cosmos DB
description: Het kiezen van de juiste consistentieniveau voor uw toepassing in Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/24/2018
ms.reviewer: sngun
ms.openlocfilehash: 11eb849567079bfb1293c3c1e8ce97c43b66d493
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116841"
---
# <a name="choose-the-right-consistency-level"></a>Het juiste consistentieniveau kiezen 

Gedistribueerde databases die afhankelijk zijn van replicatie voor hoge beschikbaarheid, lage latentie of beide, moet het fundamentele verschil tussen het lezen van consistentie en beschikbaarheid, latentie en doorvoer. De meeste commercieel verkrijgbaar gedistribueerde databases stellen ontwikkelaars om te kiezen tussen de twee extreme consistentiemodellen: sterke consistentie en uiteindelijke consistentie. Azure Cosmos DB kunnen ontwikkelaars kiezen tussen de vijf duidelijk gedefinieerde consistentiemodellen: sterk, gebonden veroudering, sessie, consistent voorvoegsel en mogelijk. Elk van deze consistentiemodellen is goed gedefinieerde, intuïtieve en kan worden gebruikt voor specifieke praktijkscenario's. Elk van de vijf consistentiemodellen bieden [zorgen voor beschikbaarheid en prestaties van een balans](consistency-levels-tradeoffs.md) en worden ondersteund door uitgebreide Sla's. De volgende eenvoudige overwegingen krijgt u de juiste keuze maken in veel algemene scenario's.

## <a name="sql-api-and-table-api"></a>SQL-API en Table-API

Houd rekening met de volgende punten als uw toepassing is gemaakt met behulp van Cosmos DB SQL API of de tabel-API

- Sessieconsistentie is het optimale voor veel real-world scenario's, en is de aanbevolen optie. Voor meer informatie ziet, [procedures voor sessietoken voor uw toepassing beheren](how-to-manage-consistency.md#utilize-session-tokens).

- Als uw toepassing vereist een sterke consistentie, is het aanbevolen dat u het consistentieniveau gebonden veroudering.

- Als u nodig strengere hebt garandeert consistentie dan de opgegeven sessieconsistentie en één cijfer milliseconde latentie voor schrijfbewerkingen, is het raadzaam dat u veroudering consistentieniveau gebonden.  

- Als uw toepassing uiteindelijke consistentie vereist, is het aanbevolen dat u consistent voorvoegsel consistentieniveau.

- Als u minder strikte consistentiegarantie dan de opgegeven door sessieconsistentie nodig hebt, is het aanbevolen dat u consistent voorvoegsel consistentieniveau.

- Als u nodig hebt voor de hoogst mogelijke beschikbaarheid en de laagste latentie, gebruikt u het niveau van uiteindelijke consistentie.

- Als u nog hogere duurzaamheid van gegevens moet zonder verlies van prestaties, maakt u een aangepaste consistentieniveau op het niveau van de toepassing. Zie voor meer informatie, [instructies aangepaste synchronisatie implementeren in uw toepassingen](how-to-custom-synchronization.md).

## <a name="cassandra-mongodb-and-gremlin-api"></a>Cassandra, MongoDB en Gremlin-API

- Zie voor meer informatie over de toewijzing tussen 'Consistentieniveau lezen' aangeboden in Apache Cassandra en Cosmos DB-consistentieniveaus [consistentieniveaus en Cosmos DB-API's](consistency-levels-across-apis.md#cassandra-mapping).

- Zie voor meer informatie over de toewijzing tussen 'Lezen bezorgdheid' van de MongoDB- en Azure Cosmos DB-consistentieniveaus, [consistentieniveaus en Cosmos DB-API's](consistency-levels-across-apis.md#mongo-mapping).

## <a name="consistency-guarantees-in-practice"></a>Garanties voor consistentie in de praktijk

Krijgt u mogelijk sterkere garanties voor consistentie in de praktijk. Garanties voor consistentie voor een leesbewerking komen overeen met de nieuwheid en volgorde van de status van de database die u aanvraagt. Lezen van consistentie is gekoppeld aan de bestelling en de doorgifte van de bewerkingen voor schrijven/bijwerken.  

* Wanneer het consistentieniveau is ingesteld op **gebonden veroudering**, garandeert Microsoft dat de clients altijd de waarde van een vorige schrijven met een vertraging begrensd door het venster veroudering lezen.

* Wanneer het consistentieniveau is ingesteld op **sterke**, het venster veroudering is gelijk aan nul en de clients de meest recente toegezegde waarde van de schrijfbewerking lezen worden gegarandeerd.

* Het venster veroudering is voor de resterende drie consistentieniveaus, grotendeels afhankelijk van uw workload. Bijvoorbeeld, als er geen schrijfbewerkingen op de database, een leesbewerking met **uiteindelijke**, **sessie**, of **consistent voorvoegsel** consistentieniveaus waarschijnlijk opbrengst hetzelfde resultaat als een leesbewerking met hoog consistentieniveau.

Als uw Cosmos DB-account is geconfigureerd met een consistentieniveau dan de sterke consistentie, vindt u de kans dat uw clients kunnen er sterke en consistente leesbewerkingen voor uw workloads door te kijken op de Probabilistic Bounded Staleness (PBS) metrische gegevens. Met deze metriek wordt weergegeven in de Azure-portal, voor meer informatie, Zie [Monitor Probabilistically gebonden veroudering (PBS) metriek](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

Probabilistic gebonden veroudering wordt aangegeven hoe de uiteindelijke is uw uiteindelijke consistentie. Met deze metriek biedt een beter inzicht in hoe vaak u krijgt een sterkere consistentie dan het consistentieniveau die u momenteel hebt geconfigureerd op uw Cosmos DB-account. U kunt met andere woorden, Zie de kans (gemeten in milliseconden) aan sterk consistente leesbewerkingen voor een combinatie van schrijven en leesregio's.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de consistentieniveaus in de volgende artikelen:

* [Toewijzing van op consistentie voor Cosmos DB-API 's](consistency-levels-across-apis.md)
* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Over het beheren van de sessietoken voor uw toepassing](how-to-manage-consistency.md#utilize-session-tokens)
* [Probabilistically gebonden veroudering (PBS) metrische gegevens controleren](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)
