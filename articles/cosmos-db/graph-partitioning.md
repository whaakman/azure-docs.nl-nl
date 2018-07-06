---
title: Partitioneren van de Graph API | Microsoft Docs
description: Lees hoe u een gepartitioneerde grafiek kunt gebruiken in Azure Cosmos DB.
services: cosmos-db
author: luisbosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: conceptual
ms.date: 02/28/2018
ms.author: lbosq
ms.openlocfilehash: 202c575a917cfb24436d86881e5368b61f216d42
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37861697"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Met behulp van een gepartitioneerde grafiek in Azure Cosmos DB

Een van de belangrijkste functies van de Graph API in Azure Cosmos DB is de mogelijkheid om grootschalige grafieken via horizontale schaalbaarheid. Dit proces wordt bereikt door de [mogelijkheden in Azure Cosmos DB partitioneren](partition-data.md#how-does-partitioning-work), waardoor het gebruik van containers, die onafhankelijk in de opslag en doorvoer schalen kunnen. Azure Cosmos DB ondersteunt de volgende typen containers voor alle API's:

- **Vaste container**: deze containers een grafiek kunnen opslaan in grootte met een maximum van 10.000 aanvraageenheden per seconde dat is toegewezen maximaal 10 GB-database. Voor het maken van een vaste container is het niet nodig om op te geven van een belangrijke eigenschap van de partitie in de gegevens.

- **Onbeperkte container**: deze containers kunnen automatisch worden geschaald voor het opslaan van een grafiek buiten de limiet van 10 GB tot en met horizontale partitionering. Elke partitie worden opgeslagen 10 GB en de gegevens zullen worden automatisch verdeeld op basis van de **opgegeven partitiesleutel**, dit is een vereiste parameter bij het gebruik van een onbeperkte container. Dit type container kan een vrijwel onbeperkt aantal gegevensgrootte opslaan en mogen maximaal 100.000 aanvraageenheden per seconde of meer [contact opnemen met ondersteuning voor](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request).

De details op hoe de graph-databases worden gepartitioneerd in dit document worden beschreven, samen met de gevolgen voor zowel hoekpunten (of knooppunten) en randen.

## <a name="requirements-for-partitioned-graph"></a>Vereisten voor gepartitioneerde graph

Hieronder vindt u informatie die u bij het maken van een gepartitioneerde grafiekcontainer deze begrijpt:
- **Instellen van het partitioneren is noodzakelijk** als de container wordt verwacht dat meer dan 10 GB groot zijn en/of als bij het toewijzen van meer dan 10.000 aanvraageenheden per seconde (RU/s) vereist is.
- **Zowel hoekpunten en randen worden opgeslagen als JSON-documenten** in de back-end van een Azure Cosmos DB Graph API-container.
- **Hoekpunten vereisen een partitiesleutel**. Deze sleutel wordt bepaald welke partitie het hoekpunt worden opgeslagen via een hash-algoritme. De naam van de partitiesleutel van deze is een tekenreeks van één woord zonder spaties of speciale tekens en deze is gedefinieerd bij het maken van een nieuwe container met de volgende indeling `/partitioning-key-name` in de portal.
- **Randen worden opgeslagen met hun bronhoekpunt**. Met andere woorden, voor elk hoekpunt dat definieert de partitiesleutel waar ze worden opgeslagen samen met de uitgaande randen. Dit wordt gedaan om te voorkomen dat partitieoverkoepelende query's bij het gebruik van de `out()` kardinaliteit in graph-query's.
- **Graph-query's hoeft op te geven van een partitiesleutel**. Als u wilt profiteren van de horizontale partitionering in Azure Cosmos DB, moet de partitiesleutel worden opgegeven wanneer een hoekpunt is geselecteerd, indien mogelijk. De volgende zijn query's voor het selecteren van een of meerdere hoekpunten in een gepartitioneerde grafiek:

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

Hier volgen richtlijnen die moeten worden gevolgd om te controleren of de meest efficiënte prestaties en schaalbaarheid bij het gebruik van gepartitioneerde grafieken in onbeperkte containers:
- **Altijd de waarde voor de partitiesleutel opgeven bij het opvragen van een hoekpunt**. Het verkrijgen van een hoekpunt van een bekende partitie is de meest efficiënte manier wat betreft prestaties.
- **De uitgaande richting gebruiken bij het opvragen van randen indien mogelijk**. Zoals eerder vermeld, worden de randen opgeslagen met hun bron hoekpunten in de uitgaande richting. Dit betekent dat de kans op sorteren op query's over meerdere partities wanneer de gegevens en query's zijn ontworpen met dit patroon waarmee u rekening moet worden beperkt.
- **Kies een partitiesleutel waarmee gegevens worden gelijkmatig wordt verdeeld over meerdere partities**. Dit besluit is sterk afhankelijk van het gegevensmodel van de oplossing. Meer informatie over het maken van een juiste partitiesleutel in [partitioneren en schalen in Azure Cosmos DB](partition-data.md).
- **Query's voor het verkrijgen van gegevens binnen de grenzen van een partitie indien mogelijk optimaliseren**. Een strategie voor het partitioneren van optimale zou worden uitgelijnd met de patronen in een query uitvoeren op. Query's die gegevens worden verkregen uit één partitie bevatten de best mogelijke prestaties.

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt is een overzicht van concepten en aanbevolen procedures voor het partitioneren van met een Azure Cosmos DB Graph API opgegeven. 

* Meer informatie over [partitioneren en schalen in Azure Cosmos DB](partition-data.md).
* Meer informatie over de [Gremlin-ondersteuning in Graph API](gremlin-support.md).
* Meer informatie over [Inleiding tot Graph API](graph-introduction.md).
