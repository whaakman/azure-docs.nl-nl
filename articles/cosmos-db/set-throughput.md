---
title: Doorvoer inrichten op Azure Cosmos-containers en databases
description: Informatie over het instellen van ingerichte doorvoer voor uw Azure Cosmos-containers en -databases.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: andrl
ms.openlocfilehash: 439b48c271260e9744bb9c9ca0e2b21e61cf4687
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005060"
---
# <a name="provision-throughput-on-containers-and-databases"></a>Doorvoer voor containers en databases inrichten

Een Azure Cosmos-database is een eenheid van het beheer voor een set met containers. Een database bestaat uit een verzameling van schema-agnostische containers. Een Azure Cosmos-container is de schaalbaarheid voor doorvoer en opslag. Een container is horizontaal gepartitioneerd in een set van machines binnen een Azure-regio en is verdeeld over alle Azure-regio's die zijn gekoppeld aan uw Azure Cosmos-account.

U kunt doorvoer op twee granulaties configureren met Azure Cosmos DB:
 
- Azure Cosmos-containers
- Azure Cosmos-databases

## <a name="set-throughput-on-a-container"></a>Doorvoer instellen voor een container  

De doorvoer die is ingericht op een Azure Cosmos-container is exclusief zijn gereserveerd voor de container. De container, ontvangt de ingerichte doorvoer voortdurend. De ingerichte doorvoer voor een container wordt met financiële ondersteund door Sla's. Zie voor meer informatie over het configureren van doorvoer voor een container [inrichten doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).

Instellen van de ingerichte doorvoer voor een container is de meest gebruikte optie. U kunt de doorvoer voor een container elastisch schalen door de inrichting van elke hoeveelheid doorvoer op basis van Aanvraageenheden (ru's). Maar u kunt de doorvoer voor logische partities selectief niet opgeven. 

Als de werkbelasting wordt uitgevoerd op een logische partitie meer dan de doorvoer die is toegewezen aan de specifieke logische partitie verbruikt, krijgt uw operations tarief beperkt. Wanneer er gelden enkele beperkingen optreedt, kunt u de doorvoer voor de volledige container verhogen of probeer het opnieuw. Zie voor meer informatie over het partitioneren van [logische partities](partition-data.md).

Het is raadzaam om de doorvoer op de granulatie van de container te configureren als u wilt dat gegarandeerde prestaties voor de container.

Op een Azure Cosmos-container ingerichte doorvoer is gelijkmatig verdeeld over alle logische partities van de container. Omdat een of meer logische partities van een container worden gehost door een fysieke partitie, wordt de fysieke partities uitsluitend deel uitmaken van de container en ondersteuning voor de doorvoer die is ingericht voor de container. 

De volgende afbeelding ziet u hoe een fysieke partitie als host fungeert voor een of meer logische partities van een container:

![Fysieke partitie](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>Doorvoer instellen voor een database

Wanneer u de doorvoer voor een Azure Cosmos-database inrichten, is de doorvoer verdeeld over alle containers in de database. Een uitzondering hierop is als u een ingerichte doorvoer op specifieke containers hebt opgegeven. Het delen van de database-doorvoer tussen de containers is vergelijkbaar met die als host fungeert voor een database in een computercluster. Omdat alle containers in een database de resources die beschikbaar is op een computer deelt, op een natuurlijke manier er voorspelbare prestaties op een specifieke container. Zie voor meer informatie over het configureren van de doorvoer van een database [ingerichte doorvoer voor een Azure Cosmos-database configureren](how-to-provision-database-throughput.md).

Doorvoer van de instelling voor een Azure Cosmos-database zorgt ervoor dat u de ingerichte doorvoer voortdurend ontvangt. Omdat alle containers in de database de ingerichte doorvoer deelt, biedt Azure Cosmos DB geen dat een voorspelbare doorvoer garanties voor een bepaalde container in de database. Het gedeelte van de doorvoer die een specifieke container kan ontvangen, is afhankelijk van:

* Het aantal containers.
* De keuze van de partitiesleutels weergeven voor de verschillende containers.
* De distributie van de werkbelasting over verschillende logische partities van de containers. 

Het is raadzaam om de doorvoer van een database te configureren wanneer u wilt delen van de doorvoer in meerdere containers, maar niet wilt reserveren van de doorvoer voor een bepaalde container. 

De volgende voorbeelden laten zien waar het om te worden ingericht met doorvoer op het databaseniveau van de heeft voorkeur:

* Een database de ingerichte doorvoer voor een set van containers te delen is handig voor een multitenant-toepassing. Elke gebruiker kan worden vertegenwoordigd door een afzonderlijke Azure-Cosmos-container.

* Een database de ingerichte doorvoer voor een set van containers te delen is nuttig wanneer u een NoSQL-database, zoals MongoDB of Cassandra, gehost op een cluster van virtuele machines of fysieke on-premises servers naar Azure Cosmos DB migreren. Denk aan de ingerichte doorvoer geconfigureerd in uw Azure Cosmos-database als een logische equivalent, maar meer rendabele en flexibele met die van de computercapaciteit van uw MongoDB- of Cassandra-cluster.  

Alle containers die zijn gemaakt in een database met een ingerichte doorvoer moeten worden gemaakt met een partitiesleutel. Op een willekeurig moment tijd wordt opgelost, wordt de doorvoer die is toegewezen aan een container in een database verdeeld over alle logische partities van die container. Wanneer u containers die delen van de ingerichte doorvoer voor een database hebt, kunt u de doorvoer niet selectief toepassen op een specifieke container of een logische partitie. 

Als de werkbelasting van een logische partitie meer dan de doorvoer die toegewezen aan een specifieke logische partitie verbruikt, worden uw bewerkingen tarief beperkt. Wanneer er gelden enkele beperkingen optreedt, kunt u de doorvoer voor de volledige container verhogen of probeer het opnieuw. Zie voor meer informatie over het partitioneren van [logische partities](partition-data.md).

Meerdere logische partities die delen van de doorvoer die is ingericht met een database kunnen worden gehost op één fysieke partitie. Als één logische partitie van een container heeft altijd een bereik binnen een fysieke partitie, kunnen 'L' logische partities 'C'-containers die delen van de ingerichte doorvoer van een database kunnen worden toegewezen en die worden gehost op 'R' fysieke partities. 

De volgende afbeelding ziet u hoe een of meer logische partities die deel uitmaken van verschillende containers in een database op een fysieke partitie kan hosten:

![Fysieke partitie](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>Doorvoer instellen op een database en een container

U kunt de twee modellen kunt combineren. Inrichting doorvoer voor de database en de container is toegestaan. Het volgende voorbeeld laat zien hoe om te worden ingericht met doorvoer op een Azure Cosmos-database en een container:

* U kunt een Azure Cosmos-database met de naam Z met ingerichte doorvoer van 'K' ru's maken. 
* Maak vervolgens vijf containers met de naam A, B, C, D en E in de database.
* U kunt expliciet "P" ru's de ingerichte doorvoer configureren op de container met de naam B.
* De 'K' RUs-doorvoer wordt gedeeld door de vier containers A, C, D en E. De exacte hoeveelheid doorvoer beschikbaar voor A, C, D of E-varieert. Er zijn geen Sla's voor elke afzonderlijke containerdoorvoer.
* De container met de naam B wordt om op te halen van de doorvoer "P" ru's voortdurend gegarandeerd. Wordt ondersteund door Sla's.

## <a name="comparison-of-models"></a>Vergelijking van modellen

|**Quotum**  |**Doorvoer die is ingericht op een database**  |**Doorvoer die is ingericht in een container**|
|---------|---------|---------|
|Minimale ru 's |400 (na de eerste vier containers, moet elke aanvullende container een minimum van 100 ru's per seconde.) |400|
|Minimale ru's per container|100|400|
|Minimale ru's nodig om te gebruiken van 1 GB aan opslagruimte|40|40|
|Maximum aantal ru 's|Onbeperkte, op de database.|Onbeperkte, voor de container.|
|RU's toegewezen of beschikbaar is voor een specifieke container|Er zijn geen garanties. RU's toegewezen aan een bepaalde container, is afhankelijk van de eigenschappen. Eigenschappen kunnen worden de keuze van de partitiesleutels van containers die delen van de doorvoer, de distributie van de werkbelasting en het aantal containers. |Alle ru's die zijn geconfigureerd op de container zijn exclusief zijn gereserveerd voor de container.|
|Maximale opslag voor een container|Onbeperkt.|Onbeperkt.|
|Maximale doorvoer per logische partitie van een container|10K ru 's|10K ru 's|
|Maximale opslag (gegevens en index) per logische partitie van een container|10 GB|10 GB|

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [logische partities](partition-data.md).
* Meer informatie over het [inrichten doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).
* Meer informatie over het [doorvoer voor een Azure Cosmos-database inrichten](how-to-provision-database-throughput.md).

