---
title: De juiste consistentieniveau voor uw toepassing kiezen | Microsoft Docs
description: Het kiezen van de juiste consistentieniveau voor uw toepassing in Azure Cosmos DB.
keywords: consistentie, prestaties, azure cosmosdb, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 4b438320ac75918d10880a4bcdccadca4eebec32
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244183"
---
# <a name="choose-the-right-consistency-level-for-your-application"></a>Kies de juiste consistentieniveau voor uw toepassing

Gedistribueerde databases die afhankelijk zijn van replicatie voor hoge beschikbaarheid, lage latentie of beide, moet het fundamentele verschil tussen het lezen van consistentie en beschikbaarheid, latentie en doorvoer. De meeste commercieel verkrijgbaar gedistribueerde databases stellen ontwikkelaars om te kiezen tussen de twee extreme consistentiemodellen: sterke consistentie en uiteindelijke consistentie. Azure Cosmos DB kunnen ontwikkelaars kiezen tussen vijf duidelijk gedefinieerde consistentiemodellen: sterk, gebonden veroudering, sessie, consistent voorvoegsel en mogelijk. Elk van deze consistentiemodellen is goed gedefinieerde, intuïtieve en kan worden gebruikt voor specifieke praktijkscenario's. Elk van de vijf consistentiemodellen wissen optimalisatie van de beschikbaarheid en prestaties biedt en worden ondersteund met uitgebreide Sla's. De volgende eenvoudige overwegingen krijgt u de juiste keuze maken in veel algemene scenario's.

## <a name="sql-api-or-table-api"></a>SQL-API of de tabel-API

- Sessieconsistentie is het optimale voor veel real-world scenario's, en is de aanbevolen optie. Zie voor meer informatie, [procedures voor sessietoken voor uw toepassing beheren](how-to-manage-consistency.md#utilize-session-tokens).
- Als uw toepassing vereist een sterke consistentie, verdient het aanbeveling gebruik van het consistentieniveau gebonden veroudering.
- Als u nodig dan degene die door sessieconsistentie en één cijfer milliseconde latentie voor schrijfbewerkingen strengere garanties voor consistentie hebt, verdient het aanbeveling gebruik van het consistentieniveau gebonden veroudering.  
- Als uw toepassing uiteindelijke consistentie vereist, verdient het aanbeveling dat u consistentieniveau consistent voorvoegsel gebruiken.
- Als u minder strikte consistentiegarantie dan wat wordt geleverd door sessieconsistentie, verdient het aanbeveling dat u consistentieniveau consistent voorvoegsel gebruiken.
- Als u nodig hebt voor de hoogst mogelijke beschikbaarheid en de laagste latentie, gebruikt u het niveau van uiteindelijke consistentie.

## <a name="cassandra-mongodb-or-gremlin-api"></a>Cassandra, MongoDB of Gremlin-API

- Zie voor meer informatie over de toewijzing tussen "Consistentieniveau lezen' van Apache Cassandra en Cosmos DB-consistentieniveaus, [consistentieniveaus en Cosmos DB-API's](consistency-levels-across-apis.md#cassandra-mapping).
- Zie voor meer informatie over de toewijzing tussen 'Lezen bezorgdheid' van de MongoDB- en Azure Cosmos DB-consistentieniveaus, [consistentieniveaus en Cosmos DB-API's](consistency-levels-across-apis.md#mongo-mapping).

## <a name="you-may-get-stronger-consistency-guarantees-in-practice"></a>Krijgt u mogelijk sterkere garanties voor consistentie in de praktijk

Garanties voor consistentie voor een leesbewerking komen overeen met de nieuwheid en volgorde van de status van de database wordt aangevraagd. Lezen van consistentie is gekoppeld aan de bestelling en de doorgifte van de schrijfbewerkingen (update).  

Wanneer het consistentieniveau is ingesteld op **gebonden veroudering**, garandeert Microsoft dat de clients altijd de waarde van een vorige schrijven met een vertraging begrensd door het venster veroudering lezen.

Wanneer het consistentieniveau is ingesteld op **sterke**, het venster veroudering is gelijk aan nul en de clients de meest recente toegezegde waarde van de schrijven lezen worden gegarandeerd.

Het venster veroudering is voor de resterende drie consistentieniveaus, grotendeels afhankelijk van uw workload. Bijvoorbeeld, als er geen schrijfbewerkingen plaatsvinden op de database, een leesbewerking met **uiteindelijke**, **sessie**, of **consistent voorvoegsel** consistentieniveaus waarschijnlijk te dragen aan hetzelfde resultaat als een leesbewerking met hoog consistentieniveau.

Als uw Cosmos DB-account is geconfigureerd met een consistentieniveau dan de sterke consistentie, vindt u de kans op uw clients om op te halen van sterk consistente (linearizable) leesbewerkingen voor uw workload(s) door te kijken Probabilistic Bounded Veroudering (PBS) metrische gegevens beschikbaar zijn in de Azure-portal [hier te zien voor het gebruik van de metriek PBS](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric). Probabilistic gebonden veroudering wordt aangegeven hoe de uiteindelijke is uw uiteindelijke consistentie. Met deze metriek biedt een beter inzicht in hoe vaak u een sterkere consistentie dan het consistentieniveau die u hebt geconfigureerd op uw Cosmos DB-account. Met andere woorden, ziet u de kans (beschreven in milliseconden) op sterk consistente leesbewerkingen voor een combinatie van een schrijfbewerking en leesregio's aan.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de consistentieniveaus in de volgende artikelen:

* [Toewijzing van op consistentie voor Cosmos DB-API 's](consistency-levels-across-apis.md)
* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Over het beheren van de sessietoken voor uw toepassing](how-to-manage-consistency.md#utilize-session-tokens)
* [De metriek Probabilistically gebonden veroudering (PBS) controleren](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)