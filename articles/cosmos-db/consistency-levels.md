---
title: Consistentieniveaus in Azure Cosmos DB
description: Azure Cosmos DB biedt vijf consistentieniveaus te verdelen over uiteindelijke consistentie, beschikbaarheid en latentie-en nadelen.
keywords: uiteindelijke consistentie, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: aliuy
ms.author: andrl
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/27/2018
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b509c7eceb3c2e2fb2e53f20791976b0322ad744
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53089731"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Consistentieniveaus in Azure Cosmos DB

Gedistribueerde databases die afhankelijk van de replicatie voor hoge beschikbaarheid, lage latentie, of beide zijn, moeten het fundamentele verschil tussen het lezen van consistentie en beschikbaarheid, latentie en doorvoer. De meeste commercieel verkrijgbaar gedistribueerde databases stellen ontwikkelaars om te kiezen tussen de twee extreme consistentiemodellen: sterke consistentie en uiteindelijke consistentie. De [verwerkingen](https://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) of het model sterke consistentie is de standaard gold van gegevens programmeren. Maar een steile prijs van hogere latentie (in onveranderlijke) toegevoegd en verminderde beschikbaarheid (bij storingen). Aan de andere kant uiteindelijke consistentie biedt een hogere beschikbaarheid en betere prestaties, maar is moeilijk te programmeren van toepassingen. 

Azure Cosmos DB nadert de consistentie van gegevens als een spectrum van de opties in plaats van twee extreme. Sterke consistentie en uiteindelijke consistentie zijn aan het einde, maar er zijn veel keuzes in consistentie langs het spectrum krijgen. Ontwikkelaars kunnen deze opties gebruiken om te maken van nauwkeurige keuzes en gedetailleerde compromissen met betrekking tot hoge beschikbaarheid of prestaties. 

Met Azure Cosmos DB kunnen ontwikkelaars kiezen uit vijf duidelijk gedefinieerde consistentiemodellen op het spectrum consistentie. Van sterk naar zwak, de modellen zijn sterk, gebonden veroudering, sessie, consistent voorvoegsel en mogelijk. De modellen zijn goed gedefinieerde en intuïtieve. Ze kunnen worden gebruikt voor specifieke echte scenario's. Elk model biedt [zorgen voor beschikbaarheid en prestaties van een balans](consistency-levels-tradeoffs.md) en wordt ondersteund door uitgebreide Sla's. De volgende afbeelding ziet u verschillende consistentieniveaus als een breed spectrum aan mogelijkheden.

![Consistentie als een breed spectrum aan mogelijkheden](./media/consistency-levels/five-consistency-levels.png)

De consistentieniveaus zijn regioneutrale. Het consistentieniveau van de van uw Azure Cosmos-account wordt gegarandeerd voor alle leesbewerkingen, ongeacht de regio van waaruit de lees- en schrijfbewerkingen plaatsvindt, het aantal regio's die zijn gekoppeld aan uw Azure Cosmos-account, of dat uw account is geconfigureerd met een één of meerdere regio's voor schrijven.

## <a name="scope-of-the-read-consistency"></a>Bereik van het lezen van consistentie

Lezen van consistentie van toepassing op een enkele leesbewerking binnen het bereik van binnen het bereik van een partitiesleutel of een logische partitie. De leesbewerking kan worden uitgegeven door een externe client of een opgeslagen procedure.

## <a name="configure-the-default-consistency-level"></a>Het standaardconsistentieniveau configureren

U kunt het standaardconsistentieniveau configureren op uw Azure Cosmos-account op elk gewenst moment. Het standaardconsistentieniveau geconfigureerd in uw account is van toepassing op alle Azure Cosmos DB-databases en containers onder dat account. Alle leesbewerkingen en query's die zijn uitgegeven voor een container of een database wordt de opgegeven consistentieniveau standaard gebruikt. Zie voor meer informatie over het [het standaardconsistentieniveau configureren](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garanties die zijn gekoppeld aan consistentieniveaus

De uitgebreide Sla's geleverd door Azure Cosmos DB gegarandeerd dat 100 procent van de leesaanvragen voldoet aan de consistentiegarantie voor elk consistentieniveau die u kiest. Een leesaanvraag voldoet aan de SLA van de consistentie als alle de consistentiegarantie die zijn gekoppeld aan het consistentieniveau is voldaan. De precieze definities van de vijf consistentieniveaus in Azure Cosmos DB met behulp van de [TLA + specificatietaal](https://lamport.azurewebsites.net/tla/tla.html) vindt u in de [azure-cosmos-tla](https://github.com/Azure/azure-cosmos-tla) GitHub-opslagplaats. 

De semantiek van de vijf consistentieniveaus worden hier beschreven:

- **Sterke**: sterke consistentie biedt een [verwerkingen](https://aphyr.com/posts/313-strong-consistency-models) garanderen. De leesbewerkingen gegarandeerd de meest recente doorgevoerde versie van een item geretourneerd. Een client ziet nooit het terugschrijven van een niet-doorgevoerde of gedeeltelijke. Gebruikers zijn altijd gegarandeerd de meest recente toegezegde schrijven.

- **Gebonden veroudering**: de leesbewerkingen gegarandeerd de garantie consistent voorvoegsel in acht neemt. De leesbewerkingen kunnen volgen op schrijfbewerkingen door maximaal 'K'-versies (dat wil zeggen "updates") van een artikel of door te tijdsinterval "t". Als u gebonden veroudering kiest, kan de "veroudering' op twee manieren worden geconfigureerd: 

  * Het aantal versies (kB) van het item
  * Het tijdsinterval (t) waarmee de leesbewerkingen op schrijfbewerkingen volgen kunnen 

  Gebonden veroudering aanbiedingen totale globale volgorde, behalve binnen de "veroudering venster." De monotone lezen garanties bestaan binnen een regio, zowel binnen als buiten het venster veroudering. Sterke consistentie is dezelfde semantiek als degene die worden aangeboden door gebonden veroudering. Het venster veroudering is gelijk aan nul. Gebonden veroudering wordt ook wel tijd uitgesteld verwerkingen genoemd. Wanneer een client leesbewerkingen binnen een regio die schrijfbewerkingen accepteert uitvoert, zijn de garanties geboden door consistentie voor gebonden veroudering identiek zijn aan de garanties met de krachtige consistentie.

- **Sessie**: de leesbewerkingen gegarandeerd te voldoen aan de consistent voorvoegsel (uitgaande van een sessie voor één 'auteur'), monotone leesbewerkingen, monotone schrijfbewerkingen, read-your-writes en write-follows-reads garandeert. Sessieconsistentie is afgestemd op een clientsessie.

- **Consistent voorvoegsel**: Updates die worden geretourneerd, een prefix van alle updates, zonder hiaten bevatten. Consistent voorvoegsel wordt gegarandeerd dat leesbewerkingen nooit out volgorde schrijfbewerkingen te zien.

- **Uiteindelijke**: Er is geen garantie volgorde voor leesbewerkingen. De replica's worden in de afwezigheid van geen schrijfbewerkingen meer kunnen uiteindelijk geconvergeerd.

## <a name="consistency-levels-explained-through-baseball"></a>Consistentieniveaus uitgelegd honkbal

We gaan een baseball game scenario als voorbeeld. Stel dat een reeks schrijfbewerkingen die staan voor de score van een game baseball. De regel inning door inning score wordt beschreven in de [gerepliceerd gegevensconsistentie honkbal](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) papier. Dit spel hypothetische baseball is momenteel in het midden van de zevende inning. Het is de zevende--inning stretch. De bezoekers zich achter met een score van 2 tot en met 5.

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Wordt uitgevoerd** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Bezoekers** | 0 | 0 | 1 | 0 | 1 | 0 | 0 |  |  | 2 |
| **startpagina** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5 |

Een Azure Cosmos DB-container bevat de bezoekers en thuis team totalen worden uitgevoerd. Terwijl het spel uitgevoerd wordt, lezen verschillende garanties kunnen leiden tot clients verschillende scores lezen. De volgende tabel bevat de volledige set van scores die door het lezen van de bezoekers en thuis scores met elk van de vijf consistentiegarantie kan worden geretourneerd. De bezoekers score wordt eerst weergegeven. Verschillende mogelijke geretourneerde waarden worden gescheiden door komma's.

| **Consistentieniveau** | **Scores** |
| - | - |
| **Sterke** | 2-5 |
| **Gebonden veroudering** | Scores die maximaal één inning verouderd: 2-3, 2-4, 2-5 |
| **Sessie** | <ul><li>Voor de schrijver: 2-5</li><li> voor iedereen behalve de writer: 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2 en 3, 2-4, 2-5</li><li>Na het lezen van 1-3: 1-3, 1-4, 1-5, 2 en 3, 2-4, 2-5</li> |
| **Consistent voorvoegsel** | 0-0, 0-1, 1-1, 1-2, 1-3, 2 en 3, 2-4, 2-5 |
| **Uiteindelijke** | 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2 en 3, 2-4, 2-5 |

## <a name="additional-reading"></a>Meer lezen

Lees voor meer informatie over concepten van de consistentie van de volgende artikelen:

- [Op hoog niveau TLA + specificaties voor de vijf consistentieniveaus die worden aangeboden door Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla)
- [Gerepliceerde gegevens consistentie uitgelegd via Baseball (video) door Doug Terry](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Gerepliceerde gegevens consistentie uitgelegd via Baseball (technisch document) door Doug Terry](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Sessie-garanties voor zwak consistente gerepliceerde gegevens](https://dl.acm.org/citation.cfm?id=383631)
- [Optimalisatie van de consistentie in het ontwerp van moderne systemen voor gedistribueerde Database: LIMIET is slechts een deel van het artikel](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
- [Probabilistic gebonden veroudering (PBS) voor praktische gedeeltelijke quorum](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Uiteindelijk Consistent - herzien](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Volgende stappen

Lees de volgende artikelen voor meer informatie over consistentieniveaus in Azure Cosmos DB:

* [Kies de juiste consistentieniveau voor uw toepassing](consistency-levels-choosing.md)
* [Consistentieniveaus in Azure Cosmos DB-API 's](consistency-levels-across-apis.md)
* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Het standaardconsistentieniveau configureren](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Het standaardconsistentieniveau overschrijven](how-to-manage-consistency.md#override-the-default-consistency-level)

