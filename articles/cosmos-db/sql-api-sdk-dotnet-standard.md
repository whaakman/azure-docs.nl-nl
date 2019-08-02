---
title: 'Azure Cosmos DB: SQL .NET Standard API, SDK & resources'
description: Meer informatie over de SQL-API en .NET SDK, inclusief release datums, pensioen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB .NET SDK.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 07/12/2019
ms.author: dech
ms.openlocfilehash: 0368e99135dd6e377dd2820b3e673c55182319b9
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663816"
---
# <a name="azure-cosmos-db-net-standard-sdk-for-sql-api-download-and-release-notes"></a>Azure Cosmos DB .NET Standard SDK voor SQL-API: Down load en release-opmerkingen
> [!div class="op_single_selector"]
> * [.NET Standard](sql-api-sdk-dotnet-standard.md)
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
> * [Bulk-uitvoerder-.NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulk-uitvoerder-java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**SDK downloaden**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)|
|**API-documentatie**|[.NET API-referentiedocumentatie](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Voorbeelden**|[.NET-codevoorbeelden](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples)|
|**Aan de slag**|[Aan de slag met de Azure Cosmos DB .NET SDK](sql-api-get-started.md)|
|**Zelfstudie-web-app**|[Ontwikkeling van webtoepassingen met Azure Cosmos DB](sql-api-dotnet-application.md)|
|**Huidige ondersteunde framework**|[Microsoft .NET standaard 2,0](/dotnet/standard/net-standard)|

## <a name="release-notes"></a>Releaseopmerkingen
### <a name="a-name310310"></a><a name="3.1.0"/>3.1.0
#### <a name="added"></a>Toegevoegd
- [#541](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/541) Consistentie niveau toegevoegd aan client-en query opties
- [#544](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/544) Vervolg token ondersteuning toegevoegd voor LINQ
- [#557](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/557) Trigger opties toegevoegd aan opties voor item aanvragen
- [#571](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/571) Er is een standaard JSON.net-serialisatiefunctie met optionele instellingen toegevoegd
- [#572](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/572) Validatie van partitie sleutels toegevoegd op CreateContainerIfNotExistsAsync
- [#581](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/581) LINQ toevoegen aan de QueryDefinition-API
- [#592](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/592) CreateIfNotExistsAsync toegevoegd aan de opbouw functie voor containers
- [#597](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/597) De vervolg token eigenschap is toegevoegd aan ResponseMessage
- [#604](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/604) De LINQ ToStreamIterator-extensie methode is toegevoegd

#### <a name="fixed"></a>Vast
- [#548](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/548) Onjuist getypt bericht in CosmosException. ToString ();
- [#558](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/558) Oplossing voor LocationCache ConcurrentDict-vergrendelings conflicten
- [#561](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/561) GetItemLinqQueryable werkt nu met een null-query
- [#567](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/567) De query verwerkt verschillende taal culturen op de juiste wijze
- [#574](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/574) Fout bericht ' Fixed empty ' als het parseren van query's mislukt van onverwachte uitzonde ring
- [#576](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/576) Met de query wordt de invoer op de juiste wijze geserialiseerd in een stroom

### <a name="a-name300300"></a><a name="3.0.0"/>3.0.0 
* Algemene Beschik baarheid van [versie 3.0.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) van de .NET SDK
* Streeft naar .NET Standard 2,0, die ondersteuning biedt voor .NET Framework 4.6.1 + en .NET Core 2.0 +
* Nieuw object model, met CosmosClient en methoden op het hoogste niveau splitsen over de relevante data base en container klassen
* Nieuwe zeer krachtige stream-Api's
* Ingebouwde ondersteuning voor het wijzigen van de feed-processor-Api's
* Api's van Fluent Builder voor CosmosClient, container en feed-processor wijzigen
* Api's voor idiomatisch-doorvoer beheer
* Gedetailleerde RequestOptions en ResponseTypes voor data base-, container-, item-, query-en doorvoer aanvragen
* Mogelijkheid om niet-gepartitioneerde containers te schalen 
* Uitbreid bare en aanpas bare serialisatiefunctie
* Uitbreid bare aanvraag pijplijn met ondersteuning voor aangepaste handlers


## <a name="release--retirement-dates"></a>Uittredings datums &
Micro soft biedt een kennisgeving van ten minste **twaalf maanden** voor het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie te versoepelen.

Nieuwe functies en functionaliteiten en Optima Lise ringen worden alleen toegevoegd aan de huidige SDK. het wordt daarom aangeraden dat u zo snel mogelijk een upgrade naar de nieuwste SDK-versie uitvoert. 

Aanvragen voor het Azure Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Version | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [3.1.0](#3.1.0) |29 juli 2019 |--- |
| [3.0.0](#3.0.0) |15 juli 2019 |--- |


## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) servicepagina. 

