---
title: Azure DB Cosmos schaal en prestaties testen | Microsoft Docs
description: Meer informatie over het uitvoeren van de schaal en prestaties testen met Azure Cosmos-DB
keywords: Prestaties testen
services: cosmos-db
author: arramac
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: f4c96ebd-f53c-427d-a500-3f28fe7b11d0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: arramac
ms.openlocfilehash: f72a75f2750d4838566815bece5c2221359ece29
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="performance-and-scale-testing-with-azure-cosmos-db"></a>Prestaties en schaal testen met Azure Cosmos-DB

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Prestaties en schaal testen is een belangrijke stap in de ontwikkeling van toepassingen. Voor veel toepassingen heeft de databaselaag een aanzienlijke gevolgen voor de algehele prestaties en schaalbaarheid. Het is daarom een essentieel onderdeel van de prestatietests. [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) is speciaal hiervoor gemaakte voor elastisch schalen en voorspelbare prestaties. Deze mogelijkheden kunnen u een goede geschikt voor toepassingen die een krachtige databaselaag moeten. 

In dit artikel is een verwijzing voor ontwikkelaars prestaties testen van suites voor hun Azure DB die Cosmos-werkbelastingen te implementeren. Het kan ook worden gebruikt om te evalueren Azure Cosmos DB voor hoge prestaties toepassingsscenario's. Richt zich voornamelijk op de geïsoleerde prestaties testen van de database, maar bevat ook aanbevolen procedures voor productietoepassingen.

Na het lezen van dit artikel, hebt u mogelijk de volgende vragen beantwoorden: 

* Waar vind ik een voorbeeld .NET client-toepassing voor het testen van de prestaties van Azure DB die Cosmos 
* Hoe ik hoge doorvoersnelheid niveaus met Azure Cosmos DB van mijn clienttoepassing bereikt?

Download het project uit om te beginnen met code [Azure DB die Cosmos-prestaties testen voorbeeld](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark). 

> [!NOTE]
> Het doel van deze toepassing is te laten zien hoe u de beste prestaties vanuit Azure Cosmos DB met een beperkt aantal clientcomputers. Het doel van het voorbeeld is niet als u de maximale doorvoercapaciteit van Azure Cosmos-DB (die kunnen worden geschaald zonder beperkingen).
> 
> 

Als u op zoek bent voor clientzijde configuratieopties Azure Cosmos DB prestaties te verbeteren, Zie [tips voor betere prestaties van Azure DB die Cosmos](performance-tips.md).

## <a name="run-the-performance-testing-application"></a>De toepassing testen uitvoeren
De snelste manier om te beginnen is compileren en uitvoeren van het .NET-voorbeeld, zoals beschreven in de volgende stappen. U kunt ook de broncode controleren en soortgelijke configuraties implementeren op uw eigen clienttoepassingen.

**Stap 1:** downloaden van het project uit [Azure DB die Cosmos-prestaties testen voorbeeld](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark), of de GitHub-opslagplaats vertakken.

**Stap 2:** Wijzig de instellingen voor EndpointUrl, AuthorizationKey CollectionThroughput en DocumentTemplate App.config (optioneel).

> [!NOTE]
> Voordat u verzamelingen met hoge doorvoer inricht, verwijzen naar de [prijzen pagina](https://azure.microsoft.com/pricing/details/cosmos-db/) als indicatie van de kosten per verzameling. Azure DB Cosmos facturen opslag en doorvoer onafhankelijk op uurbasis. Door te verwijderen of te verlagen van de doorvoer van uw Azure Cosmos DB verzamelingen nadat u hebt getest, kunt u kosten besparen.
> 
> 

**Stap 3:** compileren en de console-app uitvoeren vanaf de opdrachtregel. Hier ziet u de volgende uitvoer:

    C:\Users\cosmosdb\Desktop\Benchmark>DocumentDBBenchmark.exe
    Summary:
    ---------------------------------------------------------------------
    Endpoint: https://arramacquerymetrics.documents.azure.com:443/
    Collection : db.data at 100000 request units per second
    Document Template*: Player.json
    Degree of parallelism*: -1
    ---------------------------------------------------------------------

    DocumentDBBenchmark starting...
    Found collection data with 100000 RU/s
    Starting Inserts with 100 tasks
    Inserted 4503 docs @ 4491 writes/s, 47070 RU/s (122B max monthly 1KB reads)
    Inserted 17910 docs @ 8862 writes/s, 92878 RU/s (241B max monthly 1KB reads)
    Inserted 32339 docs @ 10531 writes/s, 110366 RU/s (286B max monthly 1KB reads)
    Inserted 47848 docs @ 11675 writes/s, 122357 RU/s (317B max monthly 1KB reads)
    Inserted 58857 docs @ 11545 writes/s, 120992 RU/s (314B max monthly 1KB reads)
    Inserted 69547 docs @ 11378 writes/s, 119237 RU/s (309B max monthly 1KB reads)
    Inserted 80687 docs @ 11345 writes/s, 118896 RU/s (308B max monthly 1KB reads)
    Inserted 91455 docs @ 11272 writes/s, 118131 RU/s (306B max monthly 1KB reads)
    Inserted 102129 docs @ 11208 writes/s, 117461 RU/s (304B max monthly 1KB reads)
    Inserted 112444 docs @ 11120 writes/s, 116538 RU/s (302B max monthly 1KB reads)
    Inserted 122927 docs @ 11063 writes/s, 115936 RU/s (301B max monthly 1KB reads)
    Inserted 133157 docs @ 10993 writes/s, 115208 RU/s (299B max monthly 1KB reads)
    Inserted 144078 docs @ 10988 writes/s, 115159 RU/s (298B max monthly 1KB reads)
    Inserted 155415 docs @ 11013 writes/s, 115415 RU/s (299B max monthly 1KB reads)
    Inserted 166126 docs @ 10992 writes/s, 115198 RU/s (299B max monthly 1KB reads)
    Inserted 173051 docs @ 10739 writes/s, 112544 RU/s (292B max monthly 1KB reads)
    Inserted 180169 docs @ 10527 writes/s, 110324 RU/s (286B max monthly 1KB reads)
    Inserted 192469 docs @ 10616 writes/s, 111256 RU/s (288B max monthly 1KB reads)
    Inserted 199107 docs @ 10406 writes/s, 109054 RU/s (283B max monthly 1KB reads)
    Inserted 200000 docs @ 9930 writes/s, 104065 RU/s (270B max monthly 1KB reads)

    Summary:
    ---------------------------------------------------------------------
    Inserted 200000 docs @ 9928 writes/s, 104063 RU/s (270B max monthly 1KB reads)
    ---------------------------------------------------------------------
    DocumentDBBenchmark completed successfully.
    Press any key to exit...


**Stap 4 (indien nodig):** de doorvoer gerapporteerd (RU/s) uit het hulpprogramma moet gelijk zijn of hoger is dan de ingerichte doorvoer van de verzameling. Als dit niet het geval is, kan vergroot de DegreeOfParallelism in kleine stappen u helpen de limiet is bereikt. Als de doorvoer van uw clientapp plateaus, start u meerdere exemplaren van de app op extra computers. Als u hulp nodig hebt bij deze stap, e- askcosmosdb@microsoft.com of het bestand een ondersteuningsticket vanuit de [Azure-portal](https://portal.azure.com).

Nadat u de app actief hebt, kunt u proberen andere [beleid indexeren](indexing-policies.md) en [consistentieniveaus](consistency-levels.md) om te begrijpen van de impact op de doorvoer en latentie. Ook kunt u de broncode controleren en soortgelijke configuraties voor uw eigen tests suites of productietoepassingen implementeren.

## <a name="next-steps"></a>Volgende stappen
In dit artikel, die we kijken hoe u de prestaties en schaal testen met Azure Cosmos DB met behulp van een .NET-console-app kunt uitvoeren. Raadpleeg voor meer informatie de volgende artikelen:

* [Azure DB Cosmos-prestaties testen voorbeeld](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/documentdb-benchmark)
* [Client-configuratieopties voor het verbeteren van Azure DB die Cosmos-prestaties](performance-tips.md)
* [Serverzijde partitioneren in Azure Cosmos-DB](partition-data.md)


