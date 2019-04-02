---
title: Partitionering en horizontaal schalen in Azure Cosmos DB
description: Meer informatie over hoe partitionering werkt in Azure Cosmos DB, over het configureren van de partitionering en partitioneren van sleutels en hoe u de juiste partitiesleutel voor uw toepassing kiezen.
ms.author: rimman
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.openlocfilehash: cf454d6504f0433d7ac7ca883982ae317b14f814
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762530"
---
# <a name="partitioning-and-horizontal-scaling-in-azure-cosmos-db"></a>Partitionering en horizontaal schalen in Azure Cosmos DB

In dit artikel wordt uitgelegd fysieke en logische partities in Azure Cosmos DB. Hierin worden ook aanbevolen procedures voor het schalen en te partitioneren. 

## <a name="logical-partitions"></a>Logische partities

Een logische partitie bestaat uit een verzameling van items die dezelfde partitiesleutel hebben. Bijvoorbeeld in een container waarin alle items bevatten een `City` eigenschap, kunt u `City` als de partitiesleutel voor de container. Groepen van items die de specifieke waarden voor `City`, zoals `London`, `Paris`, en `NYC`, afzonderlijke logische partities vormen. U hebt geen zorgen te hoeven maken over het verwijderen van een partitie als de onderliggende gegevens wordt verwijderd.

In Azure Cosmos DB is een container de fundamentele eenheid van de schaalbaarheid. Gegevens die wordt toegevoegd aan de container en de doorvoer die u in te op de container richten worden automatisch (horizontaal) gepartitioneerde in een set met logische partities. Gegevens en doorvoer worden gepartitioneerd op basis van de partitiesleutel die u opgeeft voor de Azure Cosmos-container. Zie voor meer informatie, [maken van een Azure Cosmos-container](how-to-create-container.md).

Een logische partitie definieert ook de omvang van transacties met databases. U kunt items in een logische partitie bijwerken met behulp van een [transactie met snapshot-isolatie](database-transactions-optimistic-concurrency.md). Wanneer nieuwe items worden toegevoegd aan een container, worden nieuwe logische partities worden transparant gemaakt door het systeem.

## <a name="physical-partitions"></a>Fysieke partities

Een Azure Cosmos-container wordt geschaald met het distribueren van gegevens en doorvoer over een groot aantal logische partities. Intern, een of meer logische partities zijn toegewezen aan een fysieke partitie die uit een verzameling van replica's bestaat, ook wel een [ *replicaset*](global-dist-under-the-hood.md). Elke replica instellen hosts een exemplaar van de Azure Cosmos DB-database-engine. Het instellen van een replica maakt de gegevens die zijn opgeslagen in de fysieke partitie duurzame, maximaal beschikbare en consistent. Een fysieke partitie ondersteuning biedt voor de maximale hoeveelheid opslag- en aanvraageenheden (ru's). Elke replica die de fysieke partitie vormt neemt over van de opslaglimiet van de partitie. Alle replica's van een fysieke partitie ondersteuning gezamenlijk voor de doorvoer die toegewezen aan de fysieke partitie. 

De volgende afbeelding ziet u hoe logische partities zijn toegewezen aan de fysieke partities die wereldwijd worden gedistribueerd:

![Een afbeelding die laat partitioneren van Azure Cosmos DB zien](./media/partition-data/logical-partitions.png)

Voor een container ingerichte doorvoer is gelijkmatig verdeeld over fysieke partities. 'Hot' partities mogelijk maken om een partitie belangrijke ontwerp dat de doorvoer van aanvragen niet gelijkmatig verdelen. Hot-partities kunnen resulteren in gelden enkele beperkingen en inefficiënt gebruik van de ingerichte doorvoer en hogere kosten.

In tegenstelling tot logische partities zijn fysieke partities een interne implementatie van het systeem. U kunt niet bepalen de grootte, positie of het aantal fysieke partities, en u de toewijzing tussen logische partities en fysieke partities kan niet bepalen. U kunt echter het aantal logische partities en de distributie van gegevens, de werkbelasting en de doorvoer per beheren [kiezen van de sleutel van de juiste logische partitie](partitioning-overview.md#choose-partitionkey).

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [een partitiesleutel kiezen](partitioning-overview.md#choose-partitionkey).
* Meer informatie over [ingerichte doorvoer in Azure Cosmos DB](request-units.md).
* Meer informatie over [wereldwijde distributie in Azure Cosmos DB](distribute-data-globally.md).
* Meer informatie over het [inrichten doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).
* Meer informatie over het [doorvoer voor een Azure Cosmos-database inrichten](how-to-provision-database-throughput.md).
