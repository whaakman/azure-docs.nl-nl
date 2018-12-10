---
title: Partitioneren van gegevens in Azure Cosmos DB Gremlin-API
description: Lees hoe u een gepartitioneerde grafiek kunt gebruiken in Azure Cosmos DB. Dit artikel beschrijft ook de vereisten en best practices voor een gepartitioneerde grafiek.
author: luisbosquez
ms.author: lbosq
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 7fc6068ccdc0c089b222fc8282063d526e862a38
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139432"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Met behulp van een gepartitioneerde grafiek in Azure Cosmos DB

Een van de belangrijkste functies van de Gremlin-API in Azure Cosmos DB is de mogelijkheid om grootschalige grafieken via horizontaal schalen. Horizontaal schalen wordt bereikt door de [mogelijkheden in Azure Cosmos DB partitioneren](partition-data.md). De containers kunnen onafhankelijk worden geschaald in termen van opslag en doorvoer. U kunt containers maken in Azure Cosmos DB die automatisch kunnen worden geschaald voor het opslaan van een graph-gegevens. De gegevens wordt automatisch verdeeld op basis van de opgegeven **partitiesleutel**.

De details op hoe de graph-databases worden gepartitioneerd in dit document worden beschreven, samen met de gevolgen voor zowel hoekpunten (of knooppunten) en randen.

## <a name="requirements-for-partitioned-graph"></a>Vereisten voor gepartitioneerde graph

Hieronder vindt u informatie die u bij het maken van een gepartitioneerde grafiekcontainer deze begrijpt:

- **Partitioneren is vereist** als wordt verwacht dat de container voor het opslaan van meer dan 10 GB groot is of als u wilt toewijzen van meer dan 10.000 aanvraageenheden per seconde (ru's).

- **Zowel hoekpunten en randen worden opgeslagen als JSON-documenten**.

- **Hoekpunten vereisen een partitiesleutel**. Deze sleutel wordt bepaald welke partitie het hoekpunt worden opgeslagen via een hash-algoritme. De naam van de partitiesleutel van deze is een tekenreeks één woord zonder spaties of speciale tekens. De partitiesleutel is gedefinieerd bij het maken van een nieuwe container en deze heeft een indeling: `/partitioning-key-name`.

- **Randen worden opgeslagen met hun bronhoekpunt**. Met andere woorden, voor elk hoekpunt dat de partitiesleutel wordt gedefinieerd waarin ze zijn opgeslagen, samen met de uitgaande randen. Dit wordt gedaan om te voorkomen dat partitieoverkoepelende query's bij het gebruik van de `out()` kardinaliteit in graph-query's.

- **Graph-query's hoeft op te geven van een partitiesleutel**. Als u wilt profiteren van de horizontale partitionering in Azure Cosmos DB, moet de partitiesleutel worden opgegeven wanneer een hoekpunt is geselecteerd, indien mogelijk. De volgende zijn query's voor het selecteren van een of meerdere hoekpunten in een gepartitioneerde grafiek:

    - `/id` en `/label` worden niet ondersteund als de partitiesleutels weergeven voor een container in Gremlin-API.


    - Selecteren en vervolgens een hoekpunt door-ID, **met behulp van de `.has()` stap om op te geven van de partitie-sleuteleigenschap**: 
    
        ```
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Selecteren van een hoekpunt door **op te geven een tuple met inbegrip van de waarde voor de partitiesleutel en -ID**: 
    
        ```
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Opgeven van een **matrix met tuples van partitie sleutelwaarden en id's**:
    
        ```
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Selecteren van een set met hoekpunten en **op te geven een lijst met partitie-sleutelwaarden**: 
    
        ```
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Aanbevolen procedures bij het gebruik van een gepartitioneerde grafiek

Gebruik de volgende richtlijnen voor optimale prestaties en schaalbaarheid bij het gebruik van gepartitioneerde grafieken met onbeperkte containers:

- **Altijd de waarde voor de partitiesleutel opgeven bij het opvragen van een hoekpunt**. Hoekpunt ophalen uit een bekende partitie is een manier prestaties bereiken.

- **De uitgaande richting gebruiken bij het opvragen van randen indien mogelijk**. Zoals eerder vermeld, worden de randen opgeslagen met hun bron hoekpunten in de uitgaande richting. De kans op sorteren op query's over meerdere partities zijn dus geminimaliseerd wanneer de gegevens en query's met dit patroon waarmee u rekening moet worden ontworpen.

- **Kies een partitiesleutel waarmee gegevens worden gelijkmatig wordt verdeeld over meerdere partities**. Dit besluit is sterk afhankelijk van het gegevensmodel van de oplossing. Meer informatie over het maken van een juiste partitiesleutel in [partitioneren en schalen in Azure Cosmos DB](partition-data.md).

- **Als u wilt ophalen van gegevens binnen de grenzen van een partitie-query's optimaliseren**. Een strategie voor het partitioneren van optimale zou worden uitgelijnd met de patronen in een query uitvoeren op. Query's die gegevens worden verkregen uit één partitie bevatten de best mogelijke prestaties.

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u doorgaan met het lezen van de volgende artikelen:

* Meer informatie over [partitioneren en schalen in Azure Cosmos DB](partition-data.md).
* Meer informatie over de [Gremlin-ondersteuning in Gremlin-API](gremlin-support.md).
* Meer informatie over [Inleiding tot Gremlin API](graph-introduction.md).
