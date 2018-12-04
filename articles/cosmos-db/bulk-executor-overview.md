---
title: Bulksgewijs importeren en bijwerken van gegevens in Azure Cosmos DB met behulp van de bulksgewijs executor-bibliotheek
description: Bulksgewijs bewerkingen uitvoeren in Azure Cosmos DB via bulkimport en bulksgewijs bijwerken API's die worden aangeboden door de bulksgewijs executor-bibliotheek.
services: cosmos-db
author: tknandu
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: ramkris
ms.openlocfilehash: e3b77ce28e5c3a236abcd9a22e643428b53ed0a6
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52834805"
---
# <a name="azure-cosmos-db-bulk-executor-library-overview"></a>Azure Cosmos DB bulksgewijs executor-bibliotheek-overzicht
 
Azure Cosmos DB is een snelle, flexibele en wereldwijd gedistribueerde databaseservice die is ontworpen voor elastisch scale-out voor de ondersteuning van: 

* Grote lezen en schrijven van doorvoer (miljoenen-bewerkingen per seconde).  
* Opslaan van grote hoeveelheden (honderden terabytes of zelfs meer) transactionele en operationele gegevens met voorspelbare milliseconde.  

De bulksgewijs executor-bibliotheek kunt u gebruikmaken van deze enorme doorvoer en opslag. De bulksgewijs executor-bibliotheek kunt u uitvoeren bulksgewijs bewerkingen in Azure Cosmos DB door middel van bulkimport en bulksgewijs bijwerken API's. U kunt meer lezen over de functies van bulksgewijs executor-bibliotheek in de volgende secties. 

> [!NOTE] 
> Op dit moment bulksgewijs executor-bibliotheek biedt ondersteuning voor importeren en update-bewerkingen en deze bibliotheek wordt ondersteund door Azure Cosmos DB SQL API- en Gremlin-API-accounts.
 
## <a name="key-features-of-the-bulk-executor-library"></a>Belangrijke functies van de bulksgewijs executor-bibliotheek  
 
* Dit vermindert de client-side-compute-resources die nodig zijn voor de doorvoer die is toegewezen aan een container volledig in beslag nemen. Een enkele thread-toepassing die schrijft gegevens met dat de bulksgewijs importeren API behaalt 10 keer groter schrijven-doorvoer in vergelijking tot een dankzij de multi-threaded toepassing gegevens parallel schrijft tijdens overbelasting van de client de CPU van de machine.  

* Deze de tijdrovende taken van het schrijven van de logica van toepassingen voor het afhandelen van gelden enkele beperkingen van de aanvraag, aanvraag-outs en andere tijdelijke uitzonderingen door op efficiënte wijze ze in de tapewisselaar weggewerkt.  

* Het biedt een vereenvoudigde mechanisme voor toepassingen uitvoeren van bulksgewijs bewerkingen om uit te schalen. Een enkele bulksgewijs executor-instantie die wordt uitgevoerd op een Azure-VM kan verbruiken groter is dan 500 K RU/s en u een hogere doorvoersnelheid bereiken door het toevoegen van extra exemplaren op individuele client VM's.  
 
* Dit kunt importeren van meer dan een terabyte aan gegevens binnen een uur bulksgewijs met behulp van een uitbreidbare architectuur.  

* Het kunt bijwerken van bestaande gegevens in Azure Cosmos DB-containers bulksgewijs als patches. 
 
## <a name="how-does-the-bulk-executor-operate"></a>Hoe werkt de Executor bulksgewijs? 

Wanneer een bulkbewerking te importeren of bijwerken van documenten wordt geactiveerd met een batch entiteiten, worden ze in eerste instantie zij in buckets overeenkomt met de Azure Cosmos DB-partitiesleutelbereik. Binnen elke bucket die overeenkomt met een partitiesleutelbereik, zijn ze onderverdeeld in mini-batches en elke act Mini batch als een nettolading die is toegewezen aan de serverzijde. De bulksgewijs executor-bibliotheek met ingebouwde optimalisaties voor gelijktijdige uitvoering van deze mini-batches zowel binnen en tussen partitie sleutelbereiken. Volgende afbeelding ziet u hoe executor voor grote hoeveelheden gegevens in verschillende partitiesleutels batches:  

![Bulksgewijs executor-architectuur](./media/bulk-executor-overview/bulk-executor-architecture.png)

De bibliotheek bulksgewijs Executor zorgt ervoor dat er bewaartemperatuur gebruikmaken van de doorvoer die is toegewezen aan een verzameling. Hierbij een [mechanisme voor toegangsbeheer van AIMD-stijl congestie](https://tools.ietf.org/html/rfc5681) partitie voor elke Azure Cosmos DB-sleutelbereik om af te handelen efficiënt gelden enkele beperkingen en time-outs. 

## <a name="next-steps"></a>Volgende stappen 
  
* Voor meer informatie proberen van de voorbeeldtoepassingen verbruikt de bulksgewijs Executor-bibliotheek in [.NET](bulk-executor-dot-net.md) en [Java](bulk-executor-java.md).  
* Bekijk de bulksgewijs executor SDK informatie en release notities in [.NET](sql-api-sdk-bulk-executor-dot-net.md) en [Java](sql-api-sdk-bulk-executor-java.md).
* De bulksgewijs Executor-bibliotheek is geïntegreerd in de Cosmos DB Spark-connector, voor meer informatie, Zie [Azure Cosmos DB Spark-connector](spark-connector.md) artikel.  
* De bulksgewijs executor-bibliotheek is ook geïntegreerd in een nieuwe versie van [Azure Cosmos DB connector](https://aka.ms/bulkexecutor-adf-v2) voor Azure Data Factory om gegevens te kopiëren.
