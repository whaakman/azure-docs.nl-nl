---
title: 'Inleiding tot Azure Cosmos DBL: Table-API | Microsoft Docs'
description: Meer informatie over hoe u Azure Cosmos DB kunt gebruiken om grote hoeveelheden sleutelwaardegegevens met lage latentie op te slaan en op te vragen met behulp van de populaire OSS MongoDB-API's.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: ''
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/20/2017
ms.author: sngun
ms.openlocfilehash: 791529dc054c8007647ba64c4bceb3e3fdb1a194
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="introduction-to-azure-cosmos-db-table-api"></a>Inleiding tot Azure Cosmos DB: tabel-API

[Azure Cosmos DB](introduction.md) biedt de Table-API voor toepassingen die zijn geschreven voor Azure Table-opslag en die premium-mogelijkheden nodig hebben, zoals:

* [Kant-en-klare wereldwijde distributie](distribute-data-globally.md).
* [Speciale doorvoer](partition-data.md) overal ter wereld.
* Latentie van slechts enkele milliseconden op het 99e percentiel.
* Gegarandeerde hoge beschikbaarheid.
* [Automatische secundaire indexering](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf).

Toepassingen die zijn geschreven voor Azure Table-opslag kunnen met behulp van de Table-API zonder codeaanpassingen worden gemigreerd naar Azure Cosmos DB en zo gebruikmaken van premium-mogelijkheden. In Table-API zijn client-SDK's beschikbaar voor .NET, Java, Python en Node.js.

Het is raadzaam om de volgende video te bekijken, waarin Aravind Krishna uitlegt hoe u aan de slag gaat met de Table-API voor Azure Cosmos DB:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Table-API-for-Azure-Cosmos-DB/player]
> 
> 

## <a name="table-offerings"></a>Aanbiedingen voor Table
Als u momenteel gebruikmaakt van Azure Table-opslag, levert overstappen naar de Azure Cosmos DB Table-API de volgende voordelen op:

| | Azure Table Storage | Azure Cosmos DB Table-API |
| --- | --- | --- |
| Latentie | Snel, maar geen bovengrens voor latentie. | Latentie van slechts enkele milliseconden voor lees- en schrijfbewerkingen, ondersteund door <10 ms latentie voor leesbewerkingen en <15 ms latentie voor schrijfbewerkingen in het 99e percentiel, op elke schaal, overal ter wereld. |
| Doorvoer | Model voor variabele doorvoersnelheid. Tabellen hebben een schaalbaarheidslimiet van 20.000 bewerkingen/sec. | Zeer schaalbaar met [toegewezen gereserveerde doorvoer per tabel](request-units.md), op basis van serviceovereenkomsten. Accounts hebben geen bovengrens voor doorvoer en bieden ondersteuning voor > 10 miljoen bewerkingen/sec per tabel. |
| Wereldwijde distributie | Eén regio met één optioneel leesbaar secundair leesgebied voor hoge beschikbaarheid. U kunt geen failover starten. | [Kant en klare wereldwijde distributie](distribute-data-globally.md) tussen 1 tot 30+ regio's. Ondersteuning voor [kant en klare wereldwijde distributie](regional-failover.md), op elk moment en overal ter wereld. |
| Indexeren | Alleen primaire index op PartitionKey en RowKey. Geen secundaire indexen. | Automatische en volledige indexering voor alle eigenschappen, geen indexbeheer. |
| Query’s uitvoeren | Voor de queryuitvoering wordt een index gebruikt als primaire sleutel. In andere gevallen wordt er gescand. | Query's kunnen profiteren van de automatische indexering van eigenschappen voor een snelle uitvoertijden van query's. |
| Consistentie | Sterke in primaire regio. Mogelijk in secundaire regio. | [Vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) voor een wisselwerking tussen beschikbaarheid, latentie, doorvoer en consistentie op basis van uw toepassingsvereisten. |
| Prijzen | Geoptimaliseerd voor opslag. | Geoptimaliseerd voor doorvoer. |
| SLA's | 99,99% beschikbaarheid. | SLA voor een beschikbaarheid van 99,99% voor alle accounts voor één regio en alle accounts voor meerdere regio's met soepele consistentie en leesbeschikbaarheid van 99,999% voor alle databaseaccounts voor meerdere regio's [Toonaangevende uitgebreide serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/) voor algemene beschikbaarheid. |

## <a name="get-started"></a>Aan de slag

Maak een Azure Cosmos DB-account in de [Azure-portal](https://portal.azure.com). Ga dan aan de slag met onze [Snelstartgids voor Table-API met behulp van .NET](create-table-dotnet.md). 

> [!IMPORTANT]
> Als u tijdens de preview een tabel-API-account hebt gemaakt, moet u een [nieuw tabel-API-account](create-table-dotnet.md#create-a-database-account) maken om te kunnen werken met de algemeen beschikbare SDK’s voor tabel-API's.
>

## <a name="next-steps"></a>Volgende stappen

Hier volgen enkele aanwijzingen om aan de slag te gaan:
* [Een .NET-toepassing ontwikkelen met de Table-API](create-table-dotnet.md)
* [Ontwikkelen met de Table-API in .NET](tutorial-develop-table-dotnet.md)
* [Tabelgegevens opvragen met de Table-API](tutorial-query-table.md)
* [Meer informatie over het instellen van wereldwijde distributie met Azure Cosmos DB met behulp van de Table-API](tutorial-global-distribution-table.md)
* [Azure Cosmos DB Table .NET API](table-sdk-dotnet.md)
* [Azure Cosmos DB Table Java-API](table-sdk-java.md)
* [Azure Cosmos DB Table Node.js-API](table-sdk-nodejs.md)
* [Inleiding tot Azure Cosmos DB Table SDK voor Python](table-sdk-python.md)

