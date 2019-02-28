---
title: Partitionering in Azure Cosmos DB
description: Overzicht van partitionering in Azure Cosmos DB.
ms.author: mjbrown
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: a8db510bea57fa3d6ee873571e586bcef7508b26
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961632"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Partitionering in Azure Cosmos DB

Azure Cosmos DB maakt gebruik van partitionering van afzonderlijke containers schalen in een database om te voldoen aan de prestatiebehoeften van uw toepassing. In het partitioneren, de items in een container zijn onderverdeeld in afzonderlijke subsets met de naam *logische partities*. Logische partities zijn samengesteld op basis van de waarde van een *partitiesleutel* die is gekoppeld aan elk item in een container. Alle items in een logische partitie hebben de dezelfde waarde voor de partitiesleutel.

Een container bevat bijvoorbeeld documenten. Elk document heeft een unieke waarde voor de `UserID` eigenschap. Als `UserID` fungeert als de partitie-sleutel voor de items in de container en er zijn 1.000 unieke `UserID` waarden, 1000 logische partities worden gemaakt voor de container.

Naast een partitiesleutel waarmee wordt bepaald van het item logische partitie, elk item in een container is een *item ID* (uniek zijn binnen een logische partitie). De partitiesleutel en de ID van het combineren van het item maakt *index*, die unieke identificatie van het item.

[Een partitiesleutel kiezen](partitioning-overview.md#choose-partitionkey) is een belangrijke beslissing die gevolgen voor de prestaties van uw toepassing hebben.

## <a name="managing-logical-partitions"></a>Beheer van logische partities

Azure Cosmos DB beheert transparant en automatisch de plaatsing van logische partities op fysieke partities (uw serverinfrastructuur) efficiënt te voldoen aan de behoeften van de schaalbaarheid en prestaties van de container. De vereisten voor doorvoer en opslag van een toepassing verhogen, verplaatst Azure Cosmos DB logische partities automatisch de belasting te verdelen over een groter aantal servers. 

Azure Cosmos DB maakt gebruik van hash-gebaseerde partitioneren om logische partities verdeeld over fysieke partities. Azure Cosmos DB-hashes de partitiesleutelwaarde van een item. Het hash-resultaat bepaalt de fysieke partitie. Vervolgens, Azure Cosmos DB ruimte wordt toegewezen de sleutel van de partitie sleutel hashes gelijkmatig over de fysieke partities.

Query's die toegang hebben tot gegevens in één partitie zijn kosteneffectiever dan query's die toegang hebben tot meerdere partities. Transacties (in opgeslagen procedures of triggers) mogen alleen voor items in een enkele logische partitie.

Zie voor meer informatie over hoe Azure Cosmos DB partities beheert, [logische partities](partition-data.md). (Dit is niet nodig om deze gegevens om te bouwen of Voer uw toepassingen te begrijpen.)

## <a id="choose-partitionkey"></a>Een partitiesleutel kiezen

Houd rekening met de volgende informatie wanneer u een partitiesleutel kiezen:

* Een enkele logische partitie heeft een limiet van 10 GB aan opslagruimte.  

* Gepartitioneerde containers hebben een minimale doorvoercapaciteit van 400 aanvraageenheden per seconde (RU/s). Aanvragen voor dezelfde partitiesleutel kunnen niet groter zijn dan de doorvoer die toegewezen aan een partitie. Als aanvragen groter is dan de toegewezen doorvoer, worden aanvragen tarief beperkt. Het is dus belangrijk om te kiezen van een partitiesleutel die niet in 'hotspots"in uw toepassing resulteren.

* Kies een partitiesleutel waarmee de belasting gelijkmatig over alle partities en gelijkmatig gedurende een periode verspreidt. Uw keuze van de partitiesleutel moet balans tussen de partitie efficiënt query's en transacties tegenover het doel van de items te verdelen over meerdere partities voor schaalbaarheid.

* Kies een partitiesleutel waarmee een breed scala aan waarden en patronen voor databasetoegang die evenredig zijn verdeeld over logische partities heeft. Dit helpt de gegevens en de activiteit in de container verdeeld over de set met logische partities, zodat de resources voor gegevensopslag en doorvoer kunnen worden verdeeld over de logische partities.

* Kandidaten voor partitiesleutels advies inwinnen eigenschappen die vaak als een filter in uw query's worden weergegeven. Query's kunnen door de partitiesleutel te nemen in het filterpredicaat efficiënt worden gerouteerd.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [partities](partition-data.md).
* Meer informatie over [ingerichte doorvoer in Azure Cosmos DB](request-units.md).
* Meer informatie over [wereldwijde distributie in Azure Cosmos DB](distribute-data-globally.md).
