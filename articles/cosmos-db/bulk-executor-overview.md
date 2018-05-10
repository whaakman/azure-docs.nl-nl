---
title: Overzicht van Azure DB-BulkExecutor Cosmos-bibliotheek | Microsoft Docs
description: Meer informatie over Azure Cosmos DB BulkExecutor bibliotheek, de voordelen van het gebruik van de bibliotheek en de bijbehorende architectuur.
keywords: Java bulksgewijs executor
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.workload: data-services
ms.topic: article
ms.date: 05/07/2018
ms.author: ramkris
ms.openlocfilehash: d395376ad6cf191f8f355f6308f27e525da2911f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="azure-cosmos-db-bulkexecutor-library-overview"></a>Overzicht van Azure DB-BulkExecutor Cosmos-bibliotheek
 
Azure Cosmos-database is een snelle, flexibele en globaal gedistribueerde database-service die is ontworpen voor elastisch scale-out voor de ondersteuning van: 

* Grote lezen en schrijven doorvoercapaciteit (miljoenen bewerkingen per seconde).  
* Opslaan van grote hoeveelheden (honderden terabytes of zelfs meer) transactionele en operationele gegevens met voorspelbare milliseconde latentie.  

De bibliotheek BulkExecutor kunt u gebruikmaken van deze grote doorvoer en opslag, de BulkExecutor bibliotheek Hiermee kunt u Bulksgewijze bewerkingen in Azure Cosmos DB door middel van bulkimport en bulksgewijs bijwerken API's. U kunt meer lezen over de functies van de bibliotheek BulkExecutor in de volgende secties. 

> [!NOTE] 
> Op dit moment Bulkxecutor bibliotheek ondersteunt importeren en updatebewerkingen en deze bibliotheek wordt ondersteund door alleen accounts Azure Cosmos DB SQL-API. Zie [.NET](sql-api-sdk-bulk-executor-dot-net.md) en [Java](sql-api-sdk-bulk-executor-java.md) release-opmerkingen voor eventuele updates aan de bibliotheek.
 
## <a name="key-features-of-the-bulkexecutor-library"></a>Belangrijke functies van de bibliotheek BulkExecutor  
 
* Dit vermindert de clientzijde rekenresources die nodig zijn voor de doorvoer die is toegewezen aan een container verzadigen. Een enkele thread-toepassing die u schrijft gegevens met dat de bulk import API realiseert 10 keer groter schrijfbewerkingen in vergelijking tot een toepassing met meerdere threads gegevens parallel schrijft tijdens overbelasting van de client van de machine CPU.  

* Deze isoleert weg de omslachtig taken toepassingslogica moet worden aanvraag beperking, time-outs op aanvragen en andere tijdelijke uitzonderingen verwerkt door het efficiënt hanteren in de tapewisselaar om te schrijven.  

* Dit biedt een vereenvoudigde mechanisme voor toepassingen uitvoeren Bulksgewijze bewerkingen uit te schalen. Één BulkExecutor exemplaar uitgevoerd op een Azure VM kan verbruiken groter zijn dan 500 kB RU/s en u kunt een hogere doorvoersnelheid bereiken door toe te voegen extra exemplaren op individuele client virtuele machines.  
 
* Dit kunt importeren van meer dan een terabyte van gegevens binnen een uur bulksgewijs met behulp van een uitbreidbare architectuur.  

* Deze kan bestaande gegevens bijwerken in Azure DB die Cosmos-containers bulksgewijs als patches. 
 
## <a name="how-does-the-bulk-executor-operate"></a>Hoe werkt de Executor bulksgewijs? 

Wanneer een bulksgewijze bewerking om te importeren of documenten bijwerken met een batch entiteiten wordt geactiveerd, worden ze in eerste instantie verplaatste in buckets overeenkomt met de Azure DB die Cosmos-partitiesleutelbereik. Binnen elke bucket die overeenkomt met een partitiesleutelbereik, zijn ze onderverdeeld in mini-batches en elke act Mini batch als een nettolading die is toegewezen aan de serverzijde. De bibliotheek BulkExecutor is ingebouwd in optimalisaties voor gelijktijdige uitvoering van deze mini-batches zowel binnen en tussen partitie sleutelbereiken. Volgende afbeelding ziet u hoe BulkExecutory gegevens in verschillende partitiesleutels batches:  

![Bulksgewijs executor-architectuur](./media/bulk-executor-overview/bulk-executor-architecture.png)

De bibliotheek bulksgewijs Executor zorgt bewaartemperatuur gebruikmaken van de doorvoer die is toegewezen aan een verzameling. Gebruikt een [AIMD-stijl congestie het mechanisme voor toegangsbeheer](https://tools.ietf.org/html/rfc5681) partitie voor elke Azure DB die Cosmos-sleutel bereik efficiënte afhandeling van bandbreedtebeperking en time-outs. 

## <a name="next-steps"></a>Volgende stappen 
  
* Meer informatie door het uitproberen van de voorbeeldtoepassingen gebruiken van de bibliotheek bulksgewijs Executor in [.NET](bulk-executor-dot-net.md) en [Java](bulk-executor-java.md).  
* Bekijk de BulkExecutor SDK informatie en release-opmerkingen in [.NET](sql-api-sdk-bulk-executor-dot-net.md) en [Java](sql-api-sdk-bulk-executor-java.md).
* De bulksgewijs Executor-bibliotheek is geïntegreerd in de connector Cosmos DB Spark, voor meer informatie, Zie [Azure Cosmos DB Spark connector](spark-connector.md) artikel.  
* De bibliotheek BulkExecutor ook is geïntegreerd in een nieuwe versie van [Azure Cosmos DB connector](https://aka.ms/bulkexecutor-adf-v2) voor Azure Data Factory om gegevens te kopiëren.
