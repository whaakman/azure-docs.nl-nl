---
title: bestand opnemen
description: bestand opnemen
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: include
ms.date: 04/06/2018
ms.author: mimig
ms.custom: include file
ms.openlocfilehash: 76e1a22e02ad6b8161ec18f3806c88ffc54fe123
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "34665375"
---
Als u momenteel gebruikmaakt van Azure Table-opslag, levert overstappen naar de Azure Cosmos DB Table-API de volgende voordelen op:

| | Azure Table Storage | Azure Cosmos DB Table-API |
| --- | --- | --- |
| Latentie | Snel, maar geen bovengrens voor latentie. | Latentie van slechts enkele milliseconden voor lees- en schrijfbewerkingen, ondersteund door <10 ms latentie voor leesbewerkingen en <15 ms latentie voor schrijfbewerkingen in het 99e percentiel, op elke schaal, overal ter wereld. |
| Doorvoer | Model voor variabele doorvoersnelheid. Tabellen hebben een schaalbaarheidslimiet van 20.000 bewerkingen/sec. | Zeer schaalbaar met [toegewezen gereserveerde doorvoer per tabel](../articles/cosmos-db/request-units.md), op basis van serviceovereenkomsten. Accounts hebben geen bovengrens voor doorvoer en bieden ondersteuning voor > 10 miljoen bewerkingen/sec per tabel. |
| Wereldwijde distributie | Eén regio met één optioneel leesbaar secundair leesgebied voor hoge beschikbaarheid. U kunt geen failover starten. | [Kant en klare wereldwijde distributie](../articles/cosmos-db/distribute-data-globally.md) tussen 1 tot 30+ regio's. Ondersteuning voor [kant en klare wereldwijde distributie](../articles/cosmos-db/regional-failover.md), op elk moment en overal ter wereld. |
| Indexeren | Alleen primaire index op PartitionKey en RowKey. Geen secundaire indexen. | Automatische en volledige indexering voor alle eigenschappen, geen indexbeheer. |
| Query’s uitvoeren | Voor de queryuitvoering wordt een index gebruikt als primaire sleutel. In andere gevallen wordt er gescand. | Query's kunnen profiteren van de automatische indexering van eigenschappen voor een snelle uitvoertijden van query's. |
| Consistentie | Sterke in primaire regio. Mogelijk in secundaire regio. | [Vijf goed gedefinieerde consistentieniveaus](../articles/cosmos-db/consistency-levels.md) voor een wisselwerking tussen beschikbaarheid, latentie, doorvoer en consistentie op basis van uw toepassingsvereisten. |
| Prijzen | Geoptimaliseerd voor opslag. | Geoptimaliseerd voor doorvoer. |
| SLA's | 99,99% beschikbaarheid. | SLA voor een beschikbaarheid van 99,99% voor alle accounts voor één regio en alle accounts voor meerdere regio's met soepele consistentie en leesbeschikbaarheid van 99,999% voor alle databaseaccounts voor meerdere regio's [Toonaangevende uitgebreide serviceovereenkomsten](https://azure.microsoft.com/support/legal/sla/cosmos-db/) voor algemene beschikbaarheid. |
