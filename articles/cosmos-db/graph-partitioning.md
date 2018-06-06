---
title: Graph API partitioneren | Microsoft Docs
description: Meer informatie over hoe u kunt een gepartitioneerde grafiek in Azure Cosmos DB.
services: cosmos-db
author: luisbosquez
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-graph
ms.devlang: na
ms.topic: conceptual
ms.date: 02/28/2018
ms.author: lbosq
ms.openlocfilehash: e2658c01ea3a31efdcf2fef3bb9b56df536de295
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34795649"
---
# <a name="using-a-partitioned-graph-in-azure-cosmos-db"></a>Met behulp van een gepartitioneerde grafiek in Azure Cosmos-DB

Een van de belangrijkste functies van de Graph API in Azure Cosmos-database is de mogelijkheid voor het afhandelen van grootschalige grafieken via horizontale schaalbaarheid. Dit proces wordt bereikt door de [mogelijkheden in Azure Cosmos DB te partitioneren](partition-data.md#how-does-partitioning-work), waardoor het gebruik van verzamelingen, ook wel aangeduid als containers die onafhankelijk in termen van opslag en doorvoer schalen kunnen. Azure Cosmos DB ondersteunt de volgende soorten containers via alle API's:

- **Vaste collectie**: een grafiek kunnen worden opgeslagen in deze verzamelingen maximaal 10 GB aan de grootte van maximaal 10.000 aanvraageenheden per seconde dat is toegewezen aan deze database. Als u wilt maken van een vaste collectie is het niet nodig om op te geven van een sleuteleigenschap partitie in de gegevens.

- **Onbeperkte verzameling**: deze verzamelingen automatisch kunnen schalen voor het opslaan van een grafiek buiten de limiet van 10 GB via horizontaal partitioneren. Elke partitie 10 GB worden opgeslagen en de gegevens worden automatisch in evenwicht gebracht op basis van de **opgegeven partitiesleutel**, die is een vereiste parameter wanneer u een onbeperkt aantal collectie. Dit type van de container kan een vrijwel onbeperkte gegevens-grootte opslaan en kan tot 100.000 aanvraageenheden per seconde of meer [door contact op met ondersteuning](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request).

De details op hoe de databases van de grafiek worden gepartitioneerd in dit document wordt beschreven samen met de gevolgen voor zowel hoekpunten (of knooppunten) en randen.

## <a name="requirements-for-partitioned-graph"></a>Vereisten voor gepartitioneerde grafiek

Hier volgen de details die worden geïnterpreteerd moeten bij het maken van een container gepartitioneerde grafiek:
- **Instellen van het partitioneren van moeten** als de verzameling wordt verwacht dat meer dan 10 GB groot worden en/of als het toewijzen van meer dan 10.000 aanvraageenheden per seconde (RU/s) moet worden gestart.
- **Zowel hoekpunten en randen worden opgeslagen als JSON-documenten** in de back-end van een container Azure Cosmos DB Graph API.
- **Hoekpunten vereisen een partitiesleutel**. Deze sleutel, bepaalt op welke partitie het hoekpunt via een hash-algoritme worden opgeslagen. De naam van deze partitiesleutel is een tekenreeks van één woord zonder spaties of speciale tekens bevatten en deze is gedefinieerd bij het maken van een nieuwe verzameling met de indeling `/partitioning-key-name` op de portal.
- **Randen worden opgeslagen met hun bronhoekpunt**. Met andere woorden, voor elk hoekpunt definieert de partitiesleutel waar ze worden opgeslagen samen met de uitgaande randen. Dit wordt gedaan om te voorkomen dat cross-partitie query's bij gebruik van de `out()` kardinaliteit in grafiek query's.
- **Grafiek-query's hoeft op te geven van een partitiesleutel**. Als u wilt profiteren van de horizontale partitionering in Azure Cosmos DB, moet de partitiesleutel worden opgegeven wanneer een één hoekpunt is geselecteerd, indien mogelijk. Hieronder vindt u query's voor het selecteren van een of meerdere hoekpunten in een gepartitioneerde grafiek:

    - Als u een hoekpunt op ID, vervolgens selecteert **met behulp van de `.has()` stap om op te geven van de sleuteleigenschap partitie**: 
    
        ```
        g.V('vertex_id').has('partitionKey', 'partitionKey_value')
        ```
    
    - Als u een hoekpunt door **opgeven van een waarde voor de partitiesleutel en -ID waaronder tuple**: 
    
        ```
        g.V(['partitionKey_value', 'vertex_id'])
        ```
        
    - Geven een **matrix tuples van partitie sleutelwaarden en id's**:
    
        ```
        g.V(['partitionKey_value0', 'verted_id0'], ['partitionKey_value1', 'vertex_id1'], ...)
        ```
        
    - Als u een reeks hoekpunten en **geven een lijst met partitie-sleutelwaarden**: 
    
        ```
        g.V('vertex_id0', 'vertex_id1', 'vertex_id2', …).has('partitionKey', within('partitionKey_value0', 'partitionKey_value01', 'partitionKey_value02', …)
        ```

## <a name="best-practices-when-using-a-partitioned-graph"></a>Aanbevolen procedures bij het gebruik van een gepartitioneerde grafiek

Hieronder volgen richtlijnen die moeten worden gevolgd om te controleren of de meest efficiënte prestaties en schaalbaarheid bij gebruik van gepartitioneerde grafieken in onbeperkt verzamelingen:
- **De waarde voor de partitiesleutel altijd opgeven bij het opvragen van een hoekpunt**. Het verkrijgen van een hoekpunt van een bekende partitie is de meest efficiënte manier in termen van prestaties.
- **De uitgaande richting gebruiken bij het opvragen van randen indien mogelijk**. Zoals eerder vermeld, worden de randen opgeslagen met hun bron hoekpunten in de uitgaande richting. Dit betekent dat de kans op sorteren op cross-partitie query's zijn geminimaliseerd wanneer de gegevens en query's zijn ontworpen met dit patroon in gedachten.
- **Kies een partitiesleutel die gegevens gelijkmatig over partities verdelen wordt**. Deze beslissing is sterk afhankelijk van het gegevensmodel van de oplossing. Meer informatie over het maken van een juiste partitiesleutel in [Partitining en schalen in Azure Cosmos DB](partition-data.md).
- **Query's voor het verkrijgen van gegevens binnen de grenzen van een partitie indien mogelijk optimaliseren**. Een strategie voor een optimale partitionering zou worden afgestemd op de opvragen patronen. Query's waarmee gegevens uit één partitie bieden de best mogelijke prestaties.

## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt is een overzicht van de concepten en aanbevolen procedures voor het partitioneren van met een Azure Cosmos DB Graph API opgegeven. 

* Meer informatie over [partitie en schalen in Azure Cosmos DB](partition-data.md).
* Meer informatie over de [Gremlin ondersteuning in Graph API](gremlin-support.md).
* Meer informatie over [Inleiding tot Graph API](graph-introduction.md).
