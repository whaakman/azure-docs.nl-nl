---
title: De kosten voor implementaties in meerdere regio's in Azure Cosmos DB optimaliseren
description: In dit artikel wordt uitgelegd hoe u kosten van implementaties in meerdere regio's in Azure Cosmos DB beheren.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.openlocfilehash: d5e10e04ea9567e2656f592407855a53a70d7b0f
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53263900"
---
# <a name="optimize-the-cost-for-multi-region-deployments-in-azure-cosmos-db"></a>De kosten voor implementaties in meerdere regio's in Azure Cosmos DB optimaliseren

U kunt toevoegen en verwijderen van regio's aan uw Azure Cosmos-account op elk gewenst moment. De doorvoer die u voor verschillende Azure-Cosmos-databases en containers configureert is in elke regio die is gekoppeld aan uw account gereserveerd. Als de doorvoer per uur hebt ingericht, dat is de som van RU/s is geconfigureerd voor alle databases en containers voor uw Azure Cosmos-account is `T` en het aantal Azure-regio's die zijn gekoppeld aan uw databaseaccount is `N`, klikt u vervolgens het totaal ingerichte doorvoer voor uw Cosmos-account, voor een bepaald uur is gelijk aan:

1. ` T x N RU/s` Als uw Azure Cosmos-account is geconfigureerd met een enkele schrijfregio. 

1. `T x (N+1) RU/s` Als uw Azure Cosmos-account is geconfigureerd met alle regio's kunnen schrijfbewerkingen verwerken. 

Ingerichte doorvoer met één schrijfregio kost $0.008/ uur per 100 RU/s en ingerichte doorvoer met meerdere beschrijfbare regio's, kosten $0,016 / per uur per 100 RU/s. Zie voor meer informatie, Cosmos DB [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/).

## <a name="costs-for-multiple-write-regions"></a>Kosten voor meerdere schrijven regio 's

In een systeem met meerdere masters, schrijft de netto beschikbare ru's voor operations toeneemt `N` tijden waar `N` is het aantal regio's schrijven. In tegenstelling tot één regio schrijfbewerkingen, elke regio kan nu worden bewerkt en moet ondersteuning bieden voor conflictoplossing. Het bedrag van de werkbelasting voor schrijvers is toegenomen. Van de kosten voor het plannen van oogpunt om uit te voeren` M` RU/s-waarde van schrijfbewerkingen over de hele wereld, moet u voor het inrichten van M `RUs` op het niveau van een container of de database. Vervolgens kunt u zoveel regio's als u wilt en ze voor schrijfbewerkingen gebruiken om uit te voeren toevoegen `M` RU-waarde van schrijfbewerkingen over de hele wereld. 

### <a name="example"></a>Voorbeeld

U kunt u een container in West-Amerika hebt ingericht met doorvoer 10 K RU/s en 1 TB aan gegevens van deze maand worden opgeslagen. Stel dat u toevoegt drie regio's: VS-Oost, Noord-Europa en Oost-Azië, elk met dezelfde opslag en doorvoer en u wilt dat de mogelijkheid om te schrijven naar de containers in alle vier regio's van uw wereldwijd gedistribueerde app. Uw totale maandfactuur (uitgaande van 31 dagen) in een maand is als volgt:

|**Item**|**Gebruik (maandelijks)**|**Snelheid**|**Maandelijkse kosten**|
|----|----|----|----|
|Doorvoerfactuur voor container in West-Amerika (meerdere regio's de schrijven) |10 K RU/s * 24 uur per dag * 31 |$0,016 per 100 RU/s per uur |$1,190.40 |
|Doorvoerfactuur voor 3 extra regio's: VS-Oost, Noord-Europa en Oost-Azië (meerdere regio's de schrijven) |(3 + 1) * 10 K RU/s * 24 uur per dag * 31 |$0,016 per 100 RU/s per uur |$4,761.60 |
|Opslagfactuur voor container in US - west |100 GB |$ 0,25/GB |$25 |
|Opslagfactuur voor drie extra regio's: US - oost, Europa - noord en Azië - oost |3 * 1 TB |$ 0,25/GB |$75 |
|**Totaal**|||**$6,052** |

## <a name="improve-throughput-utilization-on-a-per-region-basis"></a>Doorvoergebruik verbeteren op een per regio-basis

Hebt u inefficiënt gebruik, bijvoorbeeld, een of meer benut of te veel regio's gebruikt, u kunt de volgende stappen voor het verbeteren van doorvoergebruik:  

1. Zorg ervoor dat u eerst ingerichte doorvoer (ru's) in de schrijfregio te optimaliseren en breng het maximaal gebruik van de me's in meer regio's met behulp van de feed van de leesregio enzovoort wijzigen. 

2. Meerdere schrijfregio's leest en schrijft kunnen worden geschaald in alle regio's die zijn gekoppeld aan Azure Cosmos-account. 

3. De activiteiten in uw regio's en u kunt toevoegen en verwijderen van regio's op aanvraag schalen van uw lezen en schrijven-doorvoer.

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u doorgaan naar meer informatie over kostenoptimalisatie in Azure Cosmos DB met de volgende artikelen:

* Meer informatie over [optimaliseren voor de ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over [informatie over uw factuur voor Azure Cosmos DB](understand-your-bill.md)
* Meer informatie over [doorvoer kosten optimaliseren](optimize-cost-throughput.md)
* Meer informatie over [opslagkosten te optimaliseren](optimize-cost-storage.md)
* Meer informatie over [optimaliseren van de kosten van lees- en schrijfbewerkingen](optimize-cost-reads-writes.md)
* Meer informatie over [de kosten van query's optimaliseren](optimize-cost-queries.md)

