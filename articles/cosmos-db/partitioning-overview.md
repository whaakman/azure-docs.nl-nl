---
title: Partitionering in Azure Cosmos DB
description: Overzicht van partitionering in Azure Cosmos DB
ms.author: mjbrown
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: 20e869d30f9e7b8102b723870f0102e041e064d7
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54042271"
---
# <a name="partitioning-in-azure-cosmos-db"></a>Partitionering in Azure Cosmos DB

Partitioneren is de techniek die door Cosmos DB gebruikt voor het schalen van afzonderlijke containers in een database om te voldoen aan de prestatiebehoeften van uw toepassing. Met behulp van partitionering, worden de items in een container worden onderverdeeld in afzonderlijke subsets, logische partities met de naam. De logische partities worden gemaakt op basis van de waarde van een partitie-sleuteleigenschap die zijn gekoppeld aan elk item.

Een logische partitie is een unieke subset van items in een container. De items in een logische partitie worden aangeduid met de waarde voor de partitiesleutel die wordt gedeeld door alle items in de logische partitie.  Neem bijvoorbeeld een container met documenten en elk document heeft een `UserID` eigenschap.  Als `UserID` fungeert als de partitie-sleutel voor de items in een container en er unieke 1000 `UserID` waarden, 1000 logische partities wordt gemaakt voor de container.

Elk item in een container is een **partitiesleutel** waarmee wordt bepaald van het item **logische partitie**, en elk item heeft ook een **artikel-id** (die uniek is binnen een logische partitie).  De **index** van een item deze uniek wordt aangeduid en deze wordt gevormd door een combinatie van de partitiesleutel en de artikel-id.

Het kiezen van een partitiesleutel is een belangrijke beslissing die gevolgen voor de prestaties van uw toepassing hebben.  Zie voor meer informatie, [een partitiesleutel kiezen](partitioning-overview.md#choose-partitionkey) artikel voor gedetailleerde instructies.

## <a name="logical-partition-management"></a>Beheer van logische partitie

Cosmos DB beheert transparant en automatisch de plaatsing van logische partities op fysieke partities (serverinfrastructuur) efficiënt te voldoen aan de behoeften van de schaalbaarheid en prestaties van de container. De vereisten voor doorvoer en opslag van de toepassing verhogen, verplaatst Cosmos DB logische partities automatisch de belasting te verdelen over een groter aantal servers. Zie voor meer informatie over hoe partities worden beheerd met Cosmos DB, [logische partities](partition-data.md) artikel. Het is niet nodig om deze gegevens om te bouwen of Voer uw toepassingen te begrijpen.

Cosmos DB maakt gebruik van hash-gebaseerde partitioneren om logische partities verdeeld over fysieke partities.  De waarde voor de partitiesleutel van een item wordt gehasht door Cosmos DB, en het hash-resultaat bepaalt de fysieke partitie. Cosmos DB ruimte wordt toegewezen de sleutel van de partitie sleutel hashes gelijkmatig over de fysieke partities "n".

Query's die toegang hebben tot gegevens in één partitie zijn kosteneffectiever dan query's die toegang hebben tot meerdere partities. Transacties (in opgeslagen procedures of triggers) zijn alleen toegestaan op basis van items in een enkele logische partitie.  

## <a id="choose-partitionkey"></a>Een partitiesleutel kiezen

Houd rekening met de volgende informatie op bij het kiezen van een partitiesleutel:

* Een enkele logische partitie is een limiet van 10 GB aan opslagruimte toegestaan.  

* Gepartitioneerde containers zijn geconfigureerd met minimale doorvoercapaciteit van 400 RU/s. Aanvragen voor dezelfde partitiesleutel kunnen niet groter zijn dan de doorvoer die is toegewezen aan een partitie. Als ze langer zijn dan de toegewezen doorvoer, worden de aanvragen tarief beperkt. Het is dus belangrijk om te kiezen van een partitiesleutel die niet in 'hotspots"in uw toepassing resulteren.

* Kies een partitiesleutel waarmee werkbelasting gelijkmatig over alle partities en gelijkmatig gedurende een periode verspreidt.  Uw keuze van de partitiesleutel moet balans tussen de partitie efficiënt query's en/of transacties tegenover het doel van de items te verdelen over meerdere partities voor schaalbaarheid.

* Kies een partitiesleutel waarmee een breed scala aan waarden en patronen voor databasetoegang die evenredig zijn verdeeld over logische partities heeft. Het uitgangspunt is op de gegevens en de activiteit in de container verdeeld over de set met logische partities, zodat de resources voor gegevensopslag en doorvoer kunnen worden verdeeld over de logische partities.

* Kandidaten voor partitiesleutels, omvat mogelijk de eigenschappen die vaak als een filter in uw query's worden weergegeven. Query's kunnen door de partitiesleutel te nemen in het filterpredicaat efficiënt worden gerouteerd.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [partities](partition-data.md)
* Meer informatie over [ingerichte doorvoer in Azure Cosmos DB](request-units.md)
* Meer informatie over [wereldwijde distributie in Azure Cosmos DB](distribute-data-globally.md)
