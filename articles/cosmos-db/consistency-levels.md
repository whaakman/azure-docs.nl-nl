---
title: Consistentieniveaus in Azure Cosmos DB
description: Azure Cosmos DB biedt vijf consistentieniveaus te verdelen over uiteindelijke consistentie, beschikbaarheid en latentie-en nadelen.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: 395b7bc31377fd771549a399032bad9d951ec804
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68384919"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Consistentieniveaus in Azure Cosmos DB

Gedistribueerde databases die afhankelijk van de replicatie voor hoge beschikbaarheid, lage latentie, of beide zijn, moeten het fundamentele verschil tussen het lezen van consistentie en beschikbaarheid, latentie en doorvoer. De meeste commerciële beschik bare gedistribueerde data bases vragen ontwikkel aars om te kiezen tussen de twee uiterst consistente consistentie modellen: *sterke* consistentie en *uiteindelijke* consistentie. Het linearizability of het sterke consistentie model is de Gold Standard van data Programmable. Maar Hiermee wordt een prijs van hogere latentie (in stabiele toestand) en een lagere Beschik baarheid (tijdens fouten) toegevoegd. Aan de andere kant bieden uiteindelijke consistentie een hogere Beschik baarheid en betere prestaties, maar maakt het Program meren van toepassingen lastig. 

Azure Cosmos DB nadert de consistentie van gegevens als een spectrum van de opties in plaats van twee extreme. Sterke consistentie en uiteindelijke consistentie bevinden zich aan de uiteinden van het spectrum, maar er zijn veel consistentie mogelijkheden in het spectrum. Ontwikkel aars kunnen deze opties gebruiken om nauw keurige keuzes en gedetailleerde afwegingen te maken met betrekking tot hoge Beschik baarheid en prestaties. 

Met Azure Cosmos DB kunnen ontwikkelaars kiezen uit vijf duidelijk gedefinieerde consistentiemodellen op het spectrum consistentie. Uit het oogpunt van het sterkst om minder goed te zijn, bevatten de modellen *sterke*, *gebonden veroudering*, *sessie*, *consistent voor voegsel*en *uiteindelijke* consistentie. De modellen zijn goed gedefinieerd en intuïtief en kunnen worden gebruikt voor specifieke praktijk scenario's. Elk model biedt [Beschik baarheid en prestatie](consistency-levels-tradeoffs.md) afwegingen en wordt ondersteund door de sla's. In de volgende afbeelding ziet u de verschillende consistentie niveaus als een spectrum.

![Consistentie als een breed spectrum aan mogelijkheden](./media/consistency-levels/five-consistency-levels.png)

De consistentie niveaus zijn neutraal en worden gegarandeerd voor alle bewerkingen, ongeacht de regio van waaruit de lees-en schrijf bewerkingen worden uitgevoerd, het aantal regio's dat aan uw Azure Cosmos-account is gekoppeld, of of uw account is geconfigureerd met één of meerdere schrijf regio's.

## <a name="scope-of-the-read-consistency"></a>Bereik van het lezen van consistentie

Lezen van consistentie van toepassing op een enkele leesbewerking binnen het bereik van binnen het bereik van een partitiesleutel of een logische partitie. De leesbewerking kan worden uitgegeven door een externe client of een opgeslagen procedure.

## <a name="configure-the-default-consistency-level"></a>Het standaardconsistentieniveau configureren

U kunt het standaardconsistentieniveau configureren op uw Azure Cosmos-account op elk gewenst moment. Het standaard consistentie niveau dat voor uw account is geconfigureerd, is van toepassing op alle Azure Cosmos-data bases en containers onder dat account. Alle leesbewerkingen en query's die zijn uitgegeven voor een container of een database wordt de opgegeven consistentieniveau standaard gebruikt. Zie voor meer informatie over het [het standaardconsistentieniveau configureren](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Garanties die zijn gekoppeld aan consistentieniveaus

De uitgebreide Sla's geleverd door Azure Cosmos DB gegarandeerd dat 100 procent van de leesaanvragen voldoet aan de consistentiegarantie voor elk consistentieniveau die u kiest. Een leesaanvraag voldoet aan de SLA van de consistentie als alle de consistentiegarantie die zijn gekoppeld aan het consistentieniveau is voldaan. De exacte definities van de vijf consistentie niveaus in Azure Cosmos DB met behulp van de TLA + specificatie taal zijn opgenomen in de [Azure-Cosmos-TLA](https://github.com/Azure/azure-cosmos-tla) github opslag plaats.

De semantiek van de vijf consistentieniveaus worden hier beschreven:

- **Sterk**: Sterke consistentie biedt een linearizability-garantie. Linearizability verwijst naar de gelijktijdigheid van aanvragen. De leesbewerkingen gegarandeerd de meest recente doorgevoerde versie van een item geretourneerd. Een client ziet nooit het terugschrijven van een niet-doorgevoerde of gedeeltelijke. Gebruikers zijn altijd gegarandeerd de meest recente toegezegde schrijven.

- **Gebonden veroudering**: De Lees bewerkingen worden gegarandeerd om te voldoen aan de consistentie van de voor voegsels. De Lees bewerkingen kunnen vertraging oplopen bij schrijf bewerkingen door de meeste *"K"* -versies (bijvoorbeeld ' updates ') van een item of op een *T* -interval. Met andere woorden, wanneer u de gebonden veroudering kiest, kan de ' verouderd ' op twee manieren worden geconfigureerd: 

  * Het aantal versies (*K*) van het item
  * Het tijds interval (*T*) waarmee de Lees bewerkingen kunnen worden vertraagd achter de schrijf bewerkingen 

  Gebonden veroudering aanbiedingen totale globale volgorde, behalve binnen de "veroudering venster." De monotone lezen garanties bestaan binnen een regio, zowel binnen als buiten het venster veroudering. Sterke consistentie heeft dezelfde semantiek als die van de gebonden veroudering. Het venster veroudering is gelijk aan nul. Gebonden veroudering wordt ook wel tijd uitgesteld verwerkingen genoemd. Wanneer een client lees bewerkingen uitvoert binnen een regio die schrijf acties accepteert, zijn de garanties die worden geboden door de gebonden verouderde consistentie, identiek aan die garanties van de sterke consistentie.

- **Sessie**:  Binnen één client sessie Lees bewerkingen wordt gegarandeerd het consistente voor voegsel (uitgaande van één ' schrijver ' sessie), monotone Lees bewerkingen, monotone schrijf bewerkingen, lees-uw-schrijf bewerkingen en lees-en schrijf bewerkingen. Clients buiten de sessie die schrijf bewerkingen uitvoeren, zien uiteindelijke consistentie.

- **Consistent voor voegsel**: Updates die worden geretourneerd, bevatten een voor voegsel van alle updates, zonder onderbrekingen. Consistent consistentie niveau van het voor voegsel zorgt ervoor dat lees bewerkingen die nooit worden uitgevoerd, niet worden weer gegeven.

- **Uiteindelijk**: Er is geen garantie voor lees bewerkingen. De replica's worden in de afwezigheid van geen schrijfbewerkingen meer kunnen uiteindelijk geconvergeerd.

## <a name="consistency-levels-explained-through-baseball"></a>Consistentieniveaus uitgelegd honkbal

We gaan een baseball game scenario als voorbeeld. Stel dat een reeks schrijfbewerkingen die staan voor de score van een game baseball. De regel inning door inning score wordt beschreven in de [gerepliceerd gegevensconsistentie honkbal](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) papier. Dit spel hypothetische baseball is momenteel in het midden van de zevende inning. Het is de zevende--inning stretch. De bezoekers bevinden zich achter met een Score van 2 tot 5, zoals hieronder wordt weer gegeven:

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Wordt uitgevoerd** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Bezoekers** | 0 | 0 | 1 | 0 | 1 | 0 | 0 |  |  | 2 |
| **startpagina** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5 |

Een Azure Cosmos-container bevat de uitvoerings totalen voor de bezoekers en de thuis teams. Terwijl het spel uitgevoerd wordt, lezen verschillende garanties kunnen leiden tot clients verschillende scores lezen. De volgende tabel bevat de volledige set van scores die door het lezen van de bezoekers en thuis scores met elk van de vijf consistentiegarantie kan worden geretourneerd. De bezoekers score wordt eerst weergegeven. Verschillende mogelijke geretourneerde waarden worden gescheiden door komma's.

| **Consistentieniveau** | **Scores (bezoekers, Home)** |
| - | - |
| **Sterke** | 2-5 |
| **Gebonden veroudering** | Scores die Maxi maal één inning verouderd zijn: 2-3, 2-4, 2-5 |
| **Sessie** | <ul><li>Voor de schrijver: 2-5</li><li> Voor iemand anders dan de schrijver: 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2 en 3, 2-4, 2-5</li><li>Na het lezen van 1-3: 1-3, 1-4, 1-5, 2-3, 2-4, 2-5</li> |
| **Consistent voorvoegsel** | 0-0, 0-1, 1-1, 1-2, 1-3, 2 en 3, 2-4, 2-5 |
| **Uiteindelijke** | 0-0, 0-1, 0-2, 0-3, 0-4, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2 en 3, 2-4, 2-5 |

## <a name="additional-reading"></a>Meer lezen

Lees voor meer informatie over concepten van de consistentie van de volgende artikelen:

- [Op hoog niveau TLA + specificaties voor de vijf consistentieniveaus die worden aangeboden door Azure Cosmos DB](https://github.com/Azure/azure-cosmos-tla)
- [Gerepliceerde gegevens consistentie uitgelegd via Baseball (video) door Doug Terry](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Gerepliceerde gegevens consistentie uitgelegd via Baseball (technisch document) door Doug Terry](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Sessie-garanties voor zwak consistente gerepliceerde gegevens](https://dl.acm.org/citation.cfm?id=383631)
- [Consistentie-afweging in het moderne ontwerp van gedistribueerde data base systemen: CAP maakt alleen deel uit van het verhaal](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Probabilistic gebonden veroudering (PBS) voor praktische gedeeltelijke quorum](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Uiteindelijk Consistent - herzien](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Volgende stappen

Lees de volgende artikelen voor meer informatie over consistentieniveaus in Azure Cosmos DB:

* [Kies de juiste consistentieniveau voor uw toepassing](consistency-levels-choosing.md)
* [Consistentieniveaus in Azure Cosmos DB-API 's](consistency-levels-across-apis.md)
* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Het standaardconsistentieniveau configureren](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Het standaardconsistentieniveau overschrijven](how-to-manage-consistency.md#override-the-default-consistency-level)

