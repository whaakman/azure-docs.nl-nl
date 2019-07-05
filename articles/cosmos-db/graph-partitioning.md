---
title: Partitioneren van gegevens in Azure Cosmos DB Gremlin-API
description: Lees hoe u een gepartitioneerde grafiek kunt gebruiken in Azure Cosmos DB. Dit artikel beschrijft ook de vereisten en best practices voor een gepartitioneerde grafiek.
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: conceptual
ms.date: 06/24/2019
ms.custom: seodec18
ms.openlocfilehash: 4c8761d82c8a735ac9c4bff2e5ac0107b2a57fe0
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537546"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Met behulp van een gepartitioneerde grafiek in Azure Cosmos DB

Een van de belangrijkste functies van de Gremlin-API in Azure Cosmos DB is de mogelijkheid om grootschalige grafieken via horizontaal schalen. De containers kunnen onafhankelijk worden geschaald in termen van opslag en doorvoer. U kunt containers maken in Azure Cosmos DB die automatisch kunnen worden geschaald voor het opslaan van een graph-gegevens. De gegevens wordt automatisch verdeeld op basis van de opgegeven **partitiesleutel**.

**Partitioneren is vereist** als wordt verwacht dat de container voor het opslaan van meer dan 10 GB groot is of als u wilt toewijzen van meer dan 10.000 aanvraageenheden per seconde (ru's). Dezelfde algemene principes van de [mechanisme voor het partitioneren van Azure Cosmos DB](partition-data.md) betrekking hebben enkele graph-specifieke optimalisaties die hieronder worden beschreven.

![Graph-partitionering.](./media/graph-partitioning/graph-partitioning.png)

## <a name="graph-partitioning-mechanism"></a>Graph-mechanisme voor partitionering

De volgende richtlijnen beschreven werking van de strategie voor partitionering in Azure Cosmos DB:

- **Zowel hoekpunten en randen worden opgeslagen als JSON-documenten**.

- **Hoekpunten vereisen een partitiesleutel**. Deze sleutel wordt bepaald welke partitie het hoekpunt worden opgeslagen via een hash-algoritme. De partitienaam van de sleuteleigenschap is gedefinieerd bij het maken van een nieuwe container en deze heeft een indeling: `/partitioning-key-name`.

- **Randen worden opgeslagen met hun bronhoekpunt**. Met andere woorden, voor elk hoekpunt dat de partitiesleutel wordt gedefinieerd waarin ze zijn opgeslagen, samen met de uitgaande randen. Deze optimalisatie wordt gedaan om te voorkomen dat partitieoverkoepelende query's bij het gebruik van de `out()` kardinaliteit in graph-query's.

- **Randen bevatten verwijzingen naar de hoekpunten die ze naar verwijzen**. Alle randen worden opgeslagen met de partitiesleutels en id's van de hoekpunten die ze aan te wijzen zijn. Deze berekening maakt alle `out()` richting query's worden altijd een scoped gepartitioneerde query en niet een blind partitie-overkoepelende query. 

- **Graph-query's hoeft op te geven van een partitiesleutel**. Als u wilt profiteren van de horizontale partitionering in Azure Cosmos DB, moet de partitiesleutel worden opgegeven wanneer een hoekpunt is geselecteerd, indien mogelijk. De volgende zijn query's voor het selecteren van een of meerdere hoekpunten in een gepartitioneerde grafiek:

    - `/id` en `/label` worden niet ondersteund als de partitiesleutels weergeven voor een container in Gremlin-API.


    - Selecteren en vervolgens een hoekpunt door-ID, **met behulp van de `.has()` stap om op te geven van de partitie-sleuteleigenschap**: 
    
        ```java
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Selecteren van een hoekpunt door **op te geven een tuple met inbegrip van de waarde voor de partitiesleutel en -ID**: 
    
        ```java
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Opgeven van een **matrix met tuples van partitie sleutelwaarden en id's**:
    
        ```java
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Selecteren van een set met hoekpunten met de id's en **op te geven een lijst met partitie-sleutelwaarden**: 
    
        ```java
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

    - Met behulp van de **strategie voor partitioneren** aan het begin van een query en een partitie van het bereik van de rest van de Gremlin-query op te geven: 
    
        ```java
        g.withStrategies(PartitionStrategy.build().partitionKey('partitionKey').readPartitions('partitionKey_value').create()).V()
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Aanbevolen procedures bij het gebruik van een gepartitioneerde grafiek

Gebruik de volgende richtlijnen voor optimale prestaties en schaalbaarheid bij het gebruik van gepartitioneerde grafieken met onbeperkte containers:

- **Altijd de waarde voor de partitiesleutel opgeven bij het opvragen van een hoekpunt**. Hoekpunt ophalen uit een bekende partitie is een manier prestaties bereiken. Alle volgende Aangrenzing bewerkingen wordt altijd toegewezen aan een partitie omdat randen verwijzing-ID en de partitie-sleutel op de hoekpunten van hun doel bevatten.

- **De uitgaande richting gebruiken bij het opvragen van randen indien mogelijk**. Zoals eerder vermeld, worden de randen opgeslagen met hun bron hoekpunten in de uitgaande richting. De kans op sorteren op query's over meerdere partities zijn dus geminimaliseerd wanneer de gegevens en query's met dit patroon waarmee u rekening moet worden ontworpen. Daarentegen de `in()` query is altijd een dure fanout-query.

- **Kies een partitiesleutel waarmee gegevens worden gelijkmatig wordt verdeeld over meerdere partities**. Dit besluit is sterk afhankelijk van het gegevensmodel van de oplossing. Meer informatie over het maken van een juiste partitiesleutel in [partitioneren en schalen in Azure Cosmos DB](partition-data.md).

- **Als u wilt ophalen van gegevens binnen de grenzen van een partitie-query's optimaliseren**. Een strategie voor het partitioneren van optimale zou worden uitgelijnd met de patronen in een query uitvoeren op. Query's die gegevens worden verkregen uit één partitie bevatten de best mogelijke prestaties.

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u doorgaan met het lezen van de volgende artikelen:

* Meer informatie over [partitioneren en schalen in Azure Cosmos DB](partition-data.md).
* Meer informatie over de [Gremlin-ondersteuning in Gremlin-API](gremlin-support.md).
* Meer informatie over [Inleiding tot Gremlin API](graph-introduction.md).
