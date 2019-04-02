---
title: Aanvraageenheden en doorvoer in Azure Cosmos DB
description: Meer informatie over het opgeven en een schatting maken van Aanvraageenheid vereisten in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/31/2019
ms.author: rimman
ms.openlocfilehash: 850c8af3e3dd696ce0e84b9c553d6f1617deb3de
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762970"
---
# <a name="request-units-in-azure-cosmos-db"></a>Aanvraageenheden in Azure Cosmos DB

Met Azure Cosmos DB betaalt u voor de doorvoer u inrichten en de opslag die u op uurbasis verbruikt. Doorvoer moet worden ingericht om ervoor te zorgen dat voldoende systeembronnen beschikbaar voor uw Azure Cosmos-database te allen tijde zijn. U moet voldoende resources om te voldoen aan of groter zijn dan de [Sla's voor Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/).

Azure Cosmos DB biedt ondersteuning voor allerlei API's, zoals SQL, MongoDB, Cassandra, Gremlin en tabel. Elke API heeft een eigen set databasebewerkingen. Het bereik van deze bewerkingen in eenvoudige punt leest en schrijft naar complexe query's. Elke databasebewerking verbruikt systeembronnen op basis van de complexiteit van de bewerking. 

De kosten van alle databasebewerkingen is genormaliseerd door Azure Cosmos DB en wordt uitgedrukt door *Aanvraageenheden* (of ru's, kortweg). U kunt zien van ru's per seconde als de valuta die voor doorvoer. RU's per seconde is een valuta tarief is gebaseerd. Deze isoleert de systeembronnen, zoals CPU, IOP's en geheugen die nodig zijn voor het uitvoeren van de databasebewerkingen die worden ondersteund door Azure Cosmos DB. 

De kosten voor het lezen van een 1 KB is item 1 Aanvraageenheid (of 1 RU). Alle andere databasebewerkingen worden op dezelfde manier kosten met behulp van ru's toegewezen. Ongeacht welke API die u gebruiken om te communiceren met uw Azure Cosmos-container, worden kosten altijd wordt gemeten door ru's. Of de databasebewerking een schrijfbewerking is, kan lezen of query's en kosten altijd in ru's worden gemeten.

De volgende afbeelding ziet u het idee op hoog niveau van ru's:

![Databasebewerkingen Aanvraageenheden gebruiken](./media/request-units/request-units.png)

Als u wilt beheren en plannen van capaciteit, Azure Cosmos DB zorgt ervoor dat het aantal ru's voor een bepaalde database gedurende een bepaalde gegevensset deterministisch. U kunt de reactieheader voor het volgen van het aantal ru's die worden verbruikt door elke databasebewerking die controleren. Wanneer u begrijpt de [factoren die invloed hebben op RU kosten](request-units.md#request-unit-considerations) en vereisten van de doorvoer van uw toepassing, kunt u uw toepassing kosteneffectief uitvoeren.

U kunt het aantal ru's inrichten voor uw toepassing op basis van per seconde in stappen van 100 ru's per seconde. Als u wilt de ingerichte doorvoer voor uw toepassing schalen, kunt u vergroten of verkleinen van het aantal me's op elk gewenst moment. U kunt schalen in stappen van of afloopt in stappen van 100 ru's. U kunt uw wijzigingen aanbrengen via de programmacode of via de Azure-portal. U wordt gefactureerd op uurbasis.

U kunt de doorvoer op twee verschillende granulaties inrichten: 

* **Containers**: Zie voor meer informatie, [inrichten doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).
* **Databases**: Zie voor meer informatie, [doorvoer voor een Azure Cosmos-database inrichten](how-to-provision-database-throughput.md).

## <a name="request-unit-considerations"></a>Overwegingen voor aanvraag-eenheid

Terwijl u een schatting maken van het aantal ru's per seconde om in te richten, houd rekening met de volgende factoren:

* **Item grootte**: Als de grootte van een item wordt verhoogd, verhoogt ook het aantal om te lezen of schrijven van het item verbruikte ru's.

* **Item indexeren**: Elk item wordt standaard automatisch geïndexeerd. Minder ru's worden gebruikt als u ervoor kiest niet te indexeren aantal objecten in een container.

* **De eigenschap count-item**: Ervan uitgaande dat de standaard-indexering voor alle eigenschappen, het aantal ru's gebruikt voor het schrijven van dat een item wordt verhoogd wanneer het item eigenschap aantal toeneemt.

* **Geïndexeerde eigenschappen**: Een beleid index op elke container wordt bepaald welke eigenschappen standaard worden geïndexeerd. De RU-verbruik voor schrijfbewerkingen, verminderen het aantal geïndexeerde eigenschappen te beperken.

* **Gegevensconsistentie**: De sterke en gebonden veroudering consistentieniveaus verbruiken ongeveer twee keer meer ru's tijdens het uitvoeren van leesbewerkingen in vergelijking met die van andere soepele consistentieniveaus.

* **Query uitvoeren op patronen**: De complexiteit van een query is van invloed op het aantal ru's worden gebruikt voor een bewerking. Factoren die invloed hebben op de kosten van querybewerkingen zijn: 
    
    - Het nummer van de resultaten van query
    - Het aantal predicaten
    - De aard van de predicaten
    - Het nummer van de gebruiker gedefinieerde functies
    - De grootte van de brongegevens
    - De grootte van de resultatenset
    - Projecties

  Azure Cosmos DB garandeert dat dezelfde query op de dezelfde gegevens altijd hetzelfde aantal ru's op mislukte uitvoeringen kosten.

* **Gebruik een script**: Verbruiken ru's op basis van de complexiteit van de bewerkingen die worden uitgevoerd als met query's, opgeslagen procedures en triggers. Tijdens het ontwikkelen van uw toepassing controleren de [kosten in rekening gebracht aanvraagheader](optimize-cost-queries.md#evaluate-request-unit-charge-for-a-query) om beter te begrijpen hoeveel elke bewerking verbruikt RU-capaciteit.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [doorvoer inrichten op Azure Cosmos-containers en databases](set-throughput.md).
* Meer informatie over [logische partities](partition-data.md).
* Meer informatie over het [wereldwijd schalen ingerichte doorvoer](scaling-throughput.md).
* Meer informatie over het [inrichten doorvoer voor een Azure Cosmos-container](how-to-provision-container-throughput.md).
* Meer informatie over het [doorvoer voor een Azure Cosmos-database inrichten](how-to-provision-database-throughput.md).
* Meer informatie over het [optimaliseert u kosten van de ingerichte doorvoer in Azure Cosmos DB](optimize-cost-throughput.md).
* Meer informatie over het [lees- en schrijfbewerkingen in Azure Cosmos DB kosten optimaliseren](optimize-cost-reads-writes.md).
* Meer informatie over het [optimaliseert query kosten in Azure Cosmos DB](optimize-cost-queries.md).
