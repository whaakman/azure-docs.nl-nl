---
title: De kosten te optimaliseren leest en schrijft in Azure Cosmos DB
description: Dit artikel wordt uitgelegd wordt uitgelegd hoe u Azure Cosmos DB verlagen bij het uitvoeren van lees- en schrijfbewerkingen op de gegevens.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.openlocfilehash: c89374919087dc3bca435b8ce4af2dedd26ccb23
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041319"
---
# <a name="optimize-the-cost-required-to-read-and-write-data-from-azure-cosmos-db"></a>De kosten die nodig zijn om te lezen en schrijven van gegevens uit Azure Cosmos DB optimaliseren

Dit artikel wordt beschreven hoe de kosten die nodig zijn om te lezen en schrijven van gegevens uit Azure Cosmos DB wordt berekend. Lees-bewerkingen zijn get-bewerkingen op items en schrijfbewerkingen zijn insert, replace, verwijderen en upsert van items.  

## <a name="cost-of-reads-and-writes"></a>Kosten van lees- en schrijfbewerkingen

Azure Cosmos DB garandeert voorspelbare prestaties wat betreft de doorvoer en latentie met behulp van een model van de ingerichte doorvoer. De ingerichte doorvoer wordt weergegeven in termen van [Aanvraageenheden](request-units.md) per seconde of RU/s. Een Aanvraageenheid (RU) is een logische abstractie via rekenresources, zoals CPU, geheugen, i/o, enz. die nodig zijn voor het uitvoeren van een aanvraag. De ingerichte doorvoer (ru's) is gereserveerd en toegewezen aan de container of de database om een voorspelbare doorvoer en latentie. Ingerichte doorvoer kan Azure Cosmos DB voor voorspelbare en consistente prestaties en een gegarandeerde lage latentie en hoge beschikbaarheid op elke schaal. Aanvraageenheden vertegenwoordigen de genormaliseerde valuta die vereenvoudigt de redenering over hoeveel resources in een toepassing nodig heeft. 

U hebt geen om na te denken over het verschil tussen de lees- en schrijfbewerkingen basis van aanvraageenheden. Het model geïntegreerde valuta van aanvraageenheden maakt efficiëntie voor het gebruik van de dezelfde doorvoercapaciteit voor zowel lees- en schrijfbewerkingen worden door elkaar. In de volgende tabel worden de kosten van leest en schrijft in termen van RU/s voor artikelen die 1 KB en 100 KB groot zijn.

|**Grootte van item**  |**Kosten van één lezen** |**Kosten van één schrijven**|
|---------|---------|---------|
|1 KB |1 RU |5 ru 's |
|100 KB |Tien aanvraageenheden |50 ru 's |

Een item van 1 kB in grootte kosten één me lezen. Een item van 1 kB schrijven kost 5 ru's. De kosten voor lezen en schrijven van toepassing zijn wanneer u de standaard-sessie [consistentieniveau](consistency-levels.md).  De overwegingen omtrent het ru's omvatten: grootte, de eigenschap aantal, de consistentie van gegevens, geïndexeerde eigenschappen, indexering,-item en patronen op te vragen.

## <a name="normalized-cost-for-1-million-reads-and-writes"></a>Genormaliseerde kosten voor 1 miljoen lees- en schrijfbewerkingen

Inrichting van 1000 RU/s wordt omgezet in het 3.6 miljoen RU/uur en $0,08 kosten voor het uur (in de Verenigde Staten en Europa). Voor een item van 1 KB, kunt u 3.6 miljoen lees- of 0,72 miljoen schrijfbewerkingen, uitvoeren (deze waarde wordt berekend als: `3.6 million RU / 5`) per uur met deze ingerichte doorvoer. Genormaliseerd voor een miljoen lees- en schrijfbewerkingen, de kosten zijn $0,022 voor 1 miljoen leesbewerkingen (deze waarde wordt berekend als: $0.08/3.6 miljoen) en 0.111 $ 1 miljoen wordt het schrijven van (deze waarde wordt berekend als: $0.08/0.72 miljoen).

## <a name="number-of-regions-and-the-request-units-cost"></a>Aantal regio's en de basis van aanvraageenheden kosten

De kosten van schrijfbewerkingen zijn constante, ongeacht het aantal regio's die zijn gekoppeld aan het Azure Cosmos-account. Met andere woorden, kost een 1 KB schrijven 5 ru's onafhankelijk van het aantal regio's die gekoppeld aan het account zijn. Er is een essentieel hoeveelheid resources die is doorgebracht in repliceren, accepteren en verwerken van het replicatieverkeer op elke regio. Zie voor meer informatie over meerdere regio's kosten optimalisatie [optimaliseren van de kosten van de Cosmos-accounts voor meerdere regio's](optimize-cost-regions.md) artikel.

## <a name="optimize-the-cost-of-writes-and-reads"></a>De kosten van schrijfbewerkingen en leesbewerkingen optimaliseren

Als u schrijfbewerkingen uitvoert, moet u voldoende capaciteit voor de ondersteuning van het aantal schrijfbewerkingen per seconde nodig inrichten. U kunt de ingerichte doorvoer verhogen met behulp van SDK, portal, CLI voordat u de schrijfbewerkingen uitvoert en vervolgens de doorvoer te verminderen nadat de schrijfbewerkingen zijn voltooid. De doorvoer voor de schrijfperiode is de minimale doorvoer die nodig zijn voor de opgegeven gegevens, plus de doorvoer die is vereist voor invoegen werkbelasting mits dat er geen andere werkbelastingen worden uitgevoerd. 

Als u andere werkbelastingen gelijktijdig worden uitgevoerd, bijvoorbeeld query/lezen/bijwerken/verwijderen, moet u toevoegen de extra aanvraageenheden vereist voor deze bewerkingen te. Als de schrijfbewerkingen beperkt in de snelheid zijn, kunt u het beleid voor opnieuw proberen/uitstel aanpassen met behulp van Azure Cosmos DB SDK's. Bijvoorbeeld, kunt u de belasting verhogen tot een klein aantal aanvragen beperkt in de snelheid wordt. Als er rate-limit optreedt, de clienttoepassing moet terug op frequentiebeperkende aanvragen voor het opgegeven interval. Voordat u probeert schrijfbewerkingen, hebt u een minimale hoeveelheid ruimte tussen de tijd tussen nieuwe pogingen. Ondersteuning voor beleid voor nieuwe pogingen is opgenomen in SQL .NET, Java, Node.js, Python-SDK's en alle ondersteunde versies van de .NET Core SDK's. 

U kunt ook bulksgewijs invoegen gegevens in Azure Cosmos DB of gegevens kopiëren van een ondersteund brongegevensarchief naar een Azure Cosmos DB met behulp van [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md). Azure Data Factory is systeemeigen is geïntegreerd met de API van Azure Cosmos DB bulksgewijs voor de beste prestaties bij het schrijven van gegevens.

## <a name="next-steps"></a>Volgende stappen

Vervolgens kunt u doorgaan naar meer informatie over kostenoptimalisatie in Azure Cosmos DB met de volgende artikelen:

* Meer informatie over [optimaliseren voor de ontwikkeling en testen](optimize-dev-test.md)
* Meer informatie over [informatie over uw factuur voor Azure Cosmos DB](understand-your-bill.md)
* Meer informatie over [doorvoer kosten optimaliseren](optimize-cost-throughput.md)
* Meer informatie over [opslagkosten te optimaliseren](optimize-cost-storage.md)
* Meer informatie over [de kosten van query's optimaliseren](optimize-cost-queries.md)
* Meer informatie over [optimaliseren van de kosten van Azure Cosmos-accounts voor meerdere regio's](optimize-cost-regions.md)
