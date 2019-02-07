---
title: 'Azure Cosmos DB: Bulksgewijs Executor .NET API, SDK en resources'
description: Meer informatie over het bulksgewijs Executor .NET API en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB bulksgewijs Executor .NET SDK.
author: tknandu
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 11/19/2018
ms.author: ramkris
ms.openlocfilehash: 76a40c7e9f32deea798441ce53be7c7ef262e2bd
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55770666"
---
# <a name="net-bulk-executor-library-download-information"></a>.NET bulksgewijs Executor-bibliotheek: Informatie downloaden 

> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Wijzigingenfeed](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST-resourceprovider](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulksgewijs Executor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulksgewijs Executor - Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
| **Beschrijving**| De bulksgewijs Executor-bibliotheek kan clienttoepassingen bulksgewijs bewerkingen op Azure Cosmos DB-accounts uit te voeren. Bulksgewijs Executor-bibliotheek biedt BulkImport BulkUpdate en bij bulkverwijdering naamruimten. De module kunt bulksgewijs BulkImport opnemen documenten in een geoptimaliseerde manier zodanig dat de ingerichte doorvoer voor een verzameling wordt gebruikt voor de maximale omvang. Bestaande gegevens in Azure Cosmos DB-containers de BulkUpdate module kunt bulksgewijs bijwerken als patches. De module bij bulkverwijdering bulksgewijs kunt verwijderen, documenten in een geoptimaliseerde manier zodanig dat de ingerichte doorvoer voor een verzameling wordt gebruikt voor de maximale omvang.|
|**SDK downloaden**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/) |
| **BulkExecutor-bibliotheek op GitHub**| [GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)|
|**API-documentatie**|[Documentatie voor .net API-verwijzing](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)|
|**Aan de slag**|[Aan de slag met de bibliotheek bulksgewijs Executor .NET SDK](bulk-executor-dot-net.md)|
| **Huidige ondersteunde framework**| Microsoft .NET Framework 4.5.2, 4.6.1 en .NET Standard 2.0 |

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Met inbegrip van MongoBulkExecutor tot ondersteuning voor .NET Standard 2.0. Deze functie kunt u functioneel equivalent met 1.3.0 loslaat, met de toevoeging van ondersteuning van .NET Standard 2.0 als doelframework.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-Preview

* Toegevoegde .NET Standard 2.0 als een van de ondersteunde frameworks waarmee de BulkExecutor-bibliotheek werken met .NET Core-toepassingen.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

* Een overbelasting van de bewerking bij bulkverwijdering voor SQL-API-accounts te accepteren van de partitiesleutel en document-id tuples verwijderen toegevoegd.
* Toegevoegd een overbelasting van de bewerking bij bulkverwijdering voor SQL-API-accounts te accepteren RequestOptions met de partitiesleutel die de waarde van de partitiesleutel, naast het gebruik van deze als een filter in de invoer voor de query op te geven documenten te verwijderen.
* Er is een probleem, wat de oorzaak van een opmaak probleem in de gebruikersagent die worden gebruikt door BulkExecutor opgelost.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Verbeteringen aangebracht in BulkExecutor import- en API's transparant om aan te passen elastisch schalen van Cosmos DB-container wanneer opslag huidige capaciteit overschrijdt zonder uitzonderingen.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Tegenaan van de afhankelijkheid van de DocumentDB .NET SDK versie 2.1.3.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Er is een probleem waardoor BulkExecutor inzetten JSRT fout terwijl verzamelingen importeren naar vaste opgelost.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Er is ondersteuning toegevoegd voor de bewerking bij bulkverwijdering voor Azure Cosmos DB SQL API-accounts.
* Er is ondersteuning toegevoegd voor BulkImport bewerking voor accounts met Azure Cosmos DB-API voor MongoDB.
* Tegenaan van de afhankelijkheid van de DocumentDB .NET SDK versie 2.0.0. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Er is ondersteuning toegevoegd voor BulkImport-bewerking voor Gremlin-API van Azure Cosmos DB-accounts.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Secundaire opgelost probleem aan de bewerking BulkImport voor Azure Cosmos DB SQL API-accounts.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Er is ondersteuning toegevoegd voor BulkImport and BulkUpdate bewerkingen voor Azure Cosmos DB SQL API-accounts.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het bulksgewijs Executor Java-bibliotheek, het volgende artikel:

[Java bulksgewijs Executor-bibliotheek SDK en release-informatie](sql-api-sdk-bulk-executor-java.md)
