---
title: 'Azure Cosmos DB: Bulksgewijs Executor .NET API, SDK en resources | Microsoft Docs'
description: Meer informatie over het bulksgewijs Executor .NET API en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB bulksgewijs Executor .NET SDK.
services: cosmos-db
author: tknandu
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/14/2018
ms.author: ramkris
ms.openlocfilehash: ffd8f438429cd8769ac0dbff7f489327166e0000
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46294455"
---
# <a name="net-bulk-executor-library-download-information"></a>Bibliotheek voor .NET bulksgewijs Executor: informatie downloaden 

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
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [Bulksgewijs Executor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulksgewijs Executor - Java](sql-api-sdk-bulk-executor-java.md)

<table>

<tr><td>**Beschrijving**</td><td>De bulksgewijs Executor-bibliotheek kan clienttoepassingen bulksgewijs bewerkingen uitvoeren in Azure Cosmos DB-accounts. Bulksgewijs Executor-bibliotheek biedt BulkImport, BulkUpdate en bij bulkverwijdering naamruimten. De module kunt bulksgewijs BulkImport opnemen documenten in een geoptimaliseerde manier zodanig dat de ingerichte doorvoer voor een verzameling wordt gebruikt voor de maximale omvang. Bestaande gegevens in Azure Cosmos DB-containers de BulkUpdate module kunt bulksgewijs bijwerken als patches. De module bij bulkverwijdering bulksgewijs kunt verwijderen, documenten in een geoptimaliseerde manier zodanig dat de ingerichte doorvoer voor een verzameling wordt gebruikt voor de maximale omvang.</td></tr>

<tr><td>**SDK downloaden**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.BulkExecutor/)</td></tr>

<tr><td>**BulkExecutor-bibliotheek op GitHub**</td><td>[GitHub](https://github.com/Azure/azure-cosmosdb-bulkexecutor-dotnet-getting-started)</td></tr>

<tr><td>**API-documentatie**</td><td>[Documentatie voor .net API-verwijzing](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor?view=azure-dotnet)</td></tr>

<tr><td>**Aan de slag**</td><td>[Aan de slag met de bibliotheek bulksgewijs Executor .NET SDK](bulk-executor-dot-net.md)</td></tr>

<tr><td>**Huidige ondersteunde framework**</td><td><ul><li>[Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)(versie > = 2.0.0)</li><li>
[Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json/)(versie > = 9.0.1)
</li></ul></td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Er is ondersteuning toegevoegd voor de bewerking bij bulkverwijdering voor Azure Cosmos DB SQL API-accounts.
* Er is ondersteuning toegevoegd voor BulkImport-bewerking voor Azure Cosmos DB MongoDB API-accounts.
* Tegenaan van de afhankelijkheid van de DocumentDB .NET SDK versie 2.0.0. 

### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2

* Er is ondersteuning toegevoegd voor BulkImport-bewerking voor Gremlin-API van Azure Cosmos DB-accounts.

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1

* Secundaire opgelost probleem aan de bewerking BulkImport voor Azure Cosmos DB SQL API-accounts.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

* Er is ondersteuning toegevoegd voor BulkImport and BulkUpdate bewerkingen voor Azure Cosmos DB SQL API-accounts.
