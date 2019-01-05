---
title: Doorvoer inrichten voor Azure Cosmos DB
description: Informatie over het instellen van ingerichte doorvoer voor uw Azure Cosmos DB-containers en -databases.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: andrl
ms.openlocfilehash: 3e992dd8ab24e4e60b81c6565ea4ec3971a9336b
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54036413"
---
# <a name="provision-throughput-on-azure-cosmos-containers-and-databases"></a>Doorvoer inrichten op Azure Cosmos-containers en databases

Een Azure Cosmos-database is een eenheid van het beheer voor een set met containers. Een database bestaat uit een verzameling van schema-agnostische containers. Een Azure Cosmos-container is de schaalbaarheid voor doorvoer en opslag. Een container is horizontaal gepartitioneerd in een set van machines binnen een Azure-regio en is verdeeld over alle Azure-regio's die zijn gekoppeld aan uw Azure Cosmos-account.

Azure Cosmos DB kunt u de doorvoer te configureren op twee granulaties - **containers van Azure Cosmos** en **databases van Azure Cosmos**.

## <a name="setting-throughput-on-a-container"></a>Doorvoer van de instelling voor een container  

De doorvoer die is ingericht op een Azure Cosmos-container is exclusief zijn gereserveerd voor de container. De container, ontvangt de ingerichte doorvoer voortdurend. De ingerichte doorvoer voor een container wordt met financiële ondersteund door Sla's. Zie voor meer informatie over het configureren van doorvoer voor een container [over het inrichten van doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).

Instellen van de ingerichte doorvoer voor een container is de meest gebruikte optie. Terwijl u kunt de doorvoer voor een container elastisch schalen door in te richten van elke hoeveelheid doorvoer (ru's), kunt u de doorvoer voor de logische partitie (s) niet selectief opgeven. Wanneer de werkbelasting wordt uitgevoerd op een logische partitie meer dan de doorvoer die is toegewezen aan de specifieke logische partitie verbruikt, krijgt uw bewerkingen tarief beperkt. Wanneer er gelden enkele beperkingen optreedt, kunt u verhoogt de doorvoer voor de volledige container of Voer de bewerking opnieuw uit. Zie voor meer informatie over het partitioneren van [logische partities](partition-data.md).

Het verdient aanbeveling om doorvoer te configureren op de granulatie van de container als u wilt dat gegarandeerde prestaties voor de container.

Voor een Azure Cosmos-container ingerichte doorvoer is gelijkmatig verdeeld over alle logische partities van de container. Omdat een of meer logische partities van een container worden gehost door een fysieke partitie, worden de fysieke partities uitsluitend deel uitmaken van de container en ondersteuning voor de doorvoer die is ingericht voor de container. De volgende afbeelding ziet u hoe een fysieke partitie als host fungeert voor een of meer logische partities van een container:

![Fysieke partitie](./media/set-throughput/resource-partition.png)

## <a name="setting-throughput-on-a-database"></a>Doorvoer van de instelling voor een database

Wanneer u de doorvoer voor een Azure Cosmos-database inrichten, wordt de doorvoer gedeeld tussen alle containers in de database, tenzij u een ingerichte doorvoer op specifieke containers hebt opgegeven. Het delen van de database-doorvoer tussen de containers is vergelijkbaar met die als host fungeert voor een database in een computercluster. Omdat alle containers in een database de resources die beschikbaar is op een computer deelt, op een natuurlijke manier er voorspelbare prestaties op een specifieke container. Zie voor meer informatie over het configureren van de doorvoer van een database [ingerichte doorvoer op een Azure Cosmos-database configureren](how-to-provision-database-throughput.md).

Doorvoer van de instelling voor een Azure Cosmos-database zorgt ervoor dat u de ingerichte doorvoer voortdurend ontvangt. Nadat alle containers in de database-share de ingerichte doorvoer biedt Azure Cosmos DB geen dat een voorspelbare doorvoer garanties voor een bepaalde container in de database. Het gedeelte van de doorvoer die een specifieke container kan ontvangen, is afhankelijk van:

* Het aantal containers
* De keuze van de partitiesleutels weergeven voor de verschillende containers en
* De distributie van de werkbelasting over verschillende logische partities van de containers. 

Het verdient aanbeveling om de doorvoer van een database te configureren wanneer u wilt delen van de doorvoer in meerdere containers, maar niet wilt reserveren van de doorvoer voor een bepaalde container. Hier volgen enkele voorbeelden waar het gewenste doorvoer voor het inrichten op het databaseniveau van de is:

* Een database de ingerichte doorvoer voor een set van containers te delen is handig voor een toepassing met meerdere tenants. Elke gebruiker kan worden vertegenwoordigd door een afzonderlijke Azure-Cosmos-container.

* Een database de ingerichte doorvoer voor een set van containers te delen is handig wanneer u van een NoSQL-database (zoals MongoDB, Cassandra migreert) die worden gehost vanuit een cluster met virtuele machines of vanuit on-premises fysieke servers naar Azure Cosmos DB. U kunt zien van de ingerichte doorvoer in uw Azure Cosmos-database geconfigureerd als een logische equivalent (maar meer rendabele en flexibele) met die van de computercapaciteit van uw MongoDB- of Cassandra-cluster.  

Alle containers die zijn gemaakt in een database met een ingerichte doorvoer moeten worden gemaakt met een partitiesleutel. Op een willekeurig moment tijd wordt opgelost, wordt de doorvoer die is toegewezen aan een container in een database verdeeld over alle logische partities van die container. Wanneer u containers delen ingerichte doorvoer voor een database hebt, kunt u de doorvoer niet selectief toepassen op een specifieke container of een logische partitie. Als de werkbelasting van een logische partitie meer dan de doorvoer die is toegewezen aan een specifieke logische partitie verbruikt, worden uw bewerkingen tarief beperkt. Wanneer er gelden enkele beperkingen optreedt, kunt u verhoogt de doorvoer voor de volledige container of Voer de bewerking opnieuw uit. Zie voor meer informatie over het partitioneren van [logische partities](partition-data.md).

Meerdere logische partities delen van de doorvoer die is ingericht met een database kunnen worden gehost op één fysieke partitie. Als één logische partitie van een container heeft altijd een bereik binnen een fysieke partitie, kunnen 'L' logische partities 'C' containers delen van de ingerichte doorvoer van een database kunnen worden toegewezen en die worden gehost op 'R' fysieke partities. De volgende afbeelding ziet u hoe een of meer logische partities die deel uitmaken van verschillende containers in een database op een fysieke partitie kan hosten:

![Fysieke partitie](./media/set-throughput/resource-partition2.png)

## <a name="setting-throughput-on-a-database-and-a-container"></a>Doorvoer instellen op een database en een container

U kunt de twee modellen combineren, is inrichting doorvoer op zowel de database en de container toegestaan. Het volgende voorbeeld laat zien hoe om te worden ingericht met doorvoer op een Azure Cosmos-database en een container:

* U kunt een Azure Cosmos-database met de naam 'Z' met een ingerichte doorvoer van 'K' ru's maken. 
* Maak vervolgens vijf containers met de naam A, B, C, D en E in de database.
* U kunt expliciet 'P' ru's van de ingerichte doorvoer voor de container "B" configureren.
* De 'K' RUs-doorvoer wordt gedeeld door de vier containers: A, C, D en E. De exacte hoeveelheid doorvoer beschikbaar voor A, C, D of E variëren en zijn er geen Sla's voor elke afzonderlijke containerdoorvoer.
* De Container 'B' om de doorvoer 'P' ru's voortdurend wordt gegarandeerd en wordt ondersteund door Sla's.

## <a name="comparison-of-models"></a>Vergelijking van modellen

|**Quotum**  |**Doorvoer die is ingericht op een database**  |**Doorvoer die is ingericht in een container**|
|---------|---------|---------|
|Minimale ru 's |400 (na de eerste vier containers, moet elke aanvullende container een minimum van 100 RU/s.) |400|
|Minimale ru's per container|100|400|
|Minimale ru's nodig om te gebruiken van 1 GB aan opslagruimte|40|40|
|Maximum aantal ru 's|Onbeperkte, op de database|Onbeperkt aantal voor de container|
|RU's toegewezen/beschikbaar voor een specifieke container|Er zijn geen garanties. RU's toegewezen aan een bepaalde container, is afhankelijk van de eigenschappen op, zoals - de keuze van de partitiesleutels van containers die de doorvoer, de verdeling van de werkbelasting, het aantal containers delen. |Alle ru's die zijn geconfigureerd op de container zijn exclusief zijn gereserveerd voor de container.|
|Maximale opslag voor een container|Onbeperkt|Onbeperkt|
|Maximale doorvoer per logische partitie van een container|10K ru 's|10K ru 's|
|Maximale opslag (gegevens en index) per logische partitie van een container|10 GB|10 GB|

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [logische partities](partition-data.md)
* Informatie over [over het inrichten van doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md)
* Informatie over [hoe u de doorvoer voor een Azure Cosmos-database inrichten](how-to-provision-database-throughput.md)

