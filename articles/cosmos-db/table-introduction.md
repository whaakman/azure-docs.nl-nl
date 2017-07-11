---
title: Inleiding tot de tabel-API van Azure Cosmos DB | Microsoft Docs
description: Leer hoe u Azure Cosmos DB kunt gebruiken om grote hoeveelheden sleutelwaardegegevens met lage latentie op te slaan met behulp van de populaire OSS MongoDB-API&quot;s.
services: cosmos-db
author: bhanupr
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/09/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 245ce9261332a3d36a36968f7c9dbc4611a019b2
ms.openlocfilehash: 07dc5e1520cf60e19a1961ad85d2aea0cc7bd15f
ms.contentlocale: nl-nl
ms.lasthandoff: 06/09/2017


---
<a id="introduction-to-azure-cosmos-db-table-api" class="xliff"></a>

# Inleiding tot Azure Cosmos DB: tabel-API

[Azure Cosmos DB](introduction.md) is de globaal gedistribueerde databaseservice met meerdere modellen van Microsoft voor essentiële toepassingen. Azure Cosmos DB biedt [gebruiksklare wereldwijde distributie](distribute-data-globally.md), [elastisch schalen van doorvoer en opslag](partition-data.md), latentie die wereldwijd in het 99e percentiel onder de 10 milliseconden blijft, [vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) en een gegarandeerd hoge beschikbaarheid, allemaal op basis van [toonaangevende serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Met Azure Cosmos DB worden [gegevens automatisch geïndexeerd](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf), zonder dat u te maken krijgt met schema- en indexbeheer. Het beschikt over meerdere modellen en ondersteunt modellen voor document-, sleutelwaarde-, grafiek- en kolomgegevens. 

![Azure-tabelopslag-API en Azure Cosmos DB](./media/table-introduction/premium-tables.png) 

Azure Cosmos DB bevat de tabel-API (voorbeeld) voor toepassingen die een sleutelwaardearchief nodig hebben met een flexibel schema, voorspelbare prestaties, wereldwijde distributie en hoge doorvoer. De tabel-API biedt dezelfde functionaliteit als Azure Table Storage, maar dan met de voordelen van de Azure Cosmos DB-engine. 

U kunt Azure-tabelopslag blijven gebruiken voor tabellen met hoge vereisten voor opslag en lagere vereisten voor doorvoer. In een toekomstige upgrade wordt in Azure Cosmos DB ondersteuning geïntroduceerd voor tabellen die zijn geoptimaliseerd voor opslag, en worden bestaande en nieuwe Azure-tabelopslagaccounts bijgewerkt naar Azure Cosmos DB.

<a id="premium-and-standard-table-apis" class="xliff"></a>

## Premium- en Standard-table-API's
Als u momenteel gebruikmaakt van Azure-tabelopslag, levert overstappen naar de preview-versie van Azure Cosmos DB (premium-tabel) de volgende voordelen op:

|  | Azure-tabelopslag | Azure Cosmos DB: tabelopslag (preview-versie) |
| --- | --- | --- |
| Latentie | Snel, maar geen bovengrens voor latentie | Latentie van minder dan 10 milliseconden voor lees- en schrijfbewerkingen, op basis van < 10 ms latentie voor leesbewerkingen en < 15 ms latentie voor schrijfbewerkingen in het 99e percentiel, op elke schaal, overal ter wereld |
| Doorvoer | Zeer schaalbaar, maar zonder model op basis van toegewezen doorvoer. Tabellen hebben een schaalbaarheidslimiet van 20.000 bewerkingen/sec | Zeer schaalbaar met [toegewezen gereserveerde doorvoer per tabel](request-units.md), op basis van serviceovereenkomsten. Accounts hebben geen bovengrens voor doorvoer en bieden ondersteuning voor > 10 miljoen bewerkingen/sec per tabel |
| Wereldwijde distributie | Eén regio met één optioneel leesbaar secundair leesgebied voor hoge beschikbaarheid. U kunt geen failover starten | [Gebruiksklare globale distributie](distribute-data-globally.md) van één tot 30+ regio's. Ondersteuning voor [automatische en handmatige failovers](regional-failover.md) op elk moment, overal ter wereld |
| Indexeren | Alleen primaire index op PartitionKey en RowKey. Geen secundaire indexen | Automatische en volledige indexering voor alle eigenschappen, geen indexbeheer |
| Query’s uitvoeren | Voor de queryuitvoering wordt een index gebruikt als primaire sleutel. In andere gevallen wordt er gescand. | Query's kunnen profiteren van de automatische indexering van eigenschappen voor een snelle uitvoertijden van query's. De database-engine van Azure Cosmos DB ondersteunt samenvoegingen, georuimtelijke bewerkingen en sorteren. |
| Consistentie | Sterk binnen primaire regio. Uiteindelijk met secundaire regio | [vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) voor beschikbaarheid, latentie, doorvoer en consistentie op basis van uw vereisten voor toepassingen |
| Prijzen | Geoptimaliseerd voor opslag  | Geoptimaliseerd voor doorvoer |
| SLA's | 99,9 % beschikbaarheid | 99,99% beschikbaarheid binnen een enkele regio en de mogelijkheid om meer regio's toe te voegen voor een hogere beschikbaarheid. [Toonaangevende uitgebreide serviceovereenkomsten ](https://azure.microsoft.com/support/legal/sla/cosmos-db/) op algemene beschikbaarheid |

<a id="how-to-get-started" class="xliff"></a>

## Hoe gaat u aan de slag

Maak een Azure Cosmos DB-account in [Azure Portal](https://portal.azure.com) en ga aan de slag met onze [Quick Start voor tabel-API met behulp van .NET](create-table-dotnet.md). 

<a id="next-steps" class="xliff"></a>

## Volgende stappen

Hier volgen enkele aanwijzingen om aan de slag te gaan:
* Aan de slag met de [tabel-API van Azure Cosmos DB](create-table-dotnet.md) met behulp van bestaande NET-tabel-SDK.
* Meer informatie over [Globale distributie met Azure Cosmos DB](distribute-data-globally.md).
* Meer informatie over [Ingerichte doorvoer in Azure Cosmos DB](request-units.md).

