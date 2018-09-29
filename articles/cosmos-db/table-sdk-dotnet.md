---
title: Azure Cosmos DB tabel-API .NET SDK en bronnen | Microsoft Docs
description: Meer informatie over de Azure Cosmos DB Table-API met inbegrip van release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie.
services: cosmos-db
author: rnagpal
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 08/17/2018
ms.author: rnagpal
ms.openlocfilehash: 1f2fae7bf500a469bc789fc2296fac5b653d1538
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451762"
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Azure Cosmos DB Table-API .NET: Downloaden en opmerkingen bij de release
> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://aka.ms/acdbtablenuget)|
|**API-documentatie**|[.NET API-referentiedocumentatie](https://aka.ms/acdbtableapiref)|
|**Snelstartgids**|[Azure Cosmos DB: Een app ontwikkelen met .NET en de tabel-API](create-table-dotnet.md)|
|**Zelfstudie**|[Azure Cosmos DB: Ontwikkelen met de tabel-API in .NET](tutorial-develop-table-dotnet.md)|
|**Huidige ondersteunde framework**|[Microsoft .NET Framework 4.5.1](https://www.microsoft.com/en-us/download/details.aspx?id=40779)|

> [!IMPORTANT]
> Als u tijdens de preview een tabel-API-account hebt gemaakt, moet u een [nieuw tabel-API-account](create-table-dotnet.md#create-a-database-account) maken om te kunnen werken met de algemeen beschikbare SDK’s voor tabel-API's.
>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Ondersteuning voor extra meerdere regio's schrijven
* Vaste NuGet-pakketafhankelijkheden op Microsoft.Azure.DocumentDB, Microsoft.OData.Core, Microsoft.OData.Edm, Microsoft.Spatial

### <a name="a-name113113"></a><a name="1.1.3"/>1.1.3
* Vaste NuGet-pakketafhankelijkheden op Microsoft.Azure.Storage.Common en Microsoft.Azure.DocumentDB.
* Oplossingen voor problemen op tabel serialisatie wanneer JsonConvert.DefaultSettings zijn geconfigureerd.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Toegevoegde validatie voor onjuiste ETAGs in de directe modus.
* Probleem opgelost door LINQ-query in de modus van de Gateway.
* Synchrone API's wordt nu uitgevoerd op de thread-groep met SynchronizationContext.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* TableQueryMaxItemCount, TableQueryEnableScan TableQueryMaxDegreeOfParallelism en TableQueryContinuationTokenLimitInKb aan TableRequestOptions toevoegen
* Oplossingen voor problemen

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Versie met algemene beschikbaarheid

### <a name="a-name010-preview090-preview"></a><a name="0.1.0-preview"/>0.9.0-Preview
* Eerste preview-release

## <a name="release-and-retirement-dates"></a>Release-en buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voorafgaand aan buiten gebruik stellen van een SDK soepel te verwerken de overgang naar een nieuwere/ondersteunde versie.

De [WindowsAzure.Storage-PremiumTable](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable/0.1.0-preview) preview-pakket is afgeschaft en vervangen door de [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) pakket. De SDK WindowsAzure.Storage-PremiumTable wordt beëindigd op 15 November 2018, op dat moment aanvragen naar de buiten gebruik gestelde SDK niet worden toegestaan. De `Microsoft.Azure.CosmosDB.Table` bibliotheek is alleen beschikbaar voor .NET Standard, het is nog niet beschikbaar voor .NET Core.

Nieuwe functies en functionaliteit en -optimalisatie worden alleen toegevoegd aan de huidige SDK, daarom is het raadzaam dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo vroeg mogelijk. 

De aanvragen die naar Azure Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.
<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.1.3](#1.1.3) |17 juli 2018|--- |
| [1.1.1](#1.1.1) |26 maart 2018|--- |
| [1.1.0](#1.1.0) |21 februari 2018|--- |
| [1.0.0](#1.0.0) |15 november 2017|--- |
| [0.9.0-preview](#0.9.0-preview) |11 november 2017 |--- |

## <a name="troubleshooting"></a>Problemen oplossen

Als u de fout optreedt 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Microsoft Azure Service Fabric SDK'`
```

Wanneer u probeert het Microsoft.Azure.CosmosDB.Table NuGet-pakket, hebt u twee opties om het probleem te verhelpen:

* Pakket beheren-Console gebruiken voor het installeren van het pakket Microsoft.Azure.CosmosDB.Table en de bijbehorende afhankelijkheden. Om dit te doen, typt u het volgende in de Package Manager-Console voor uw oplossing. 
    ```
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```
    
* Met behulp van uw voorkeur beheerprogramma van de NuGet-pakket, de Microsoft.Azure.Storage.Common NuGet-pakket installeren voordat u Microsoft.Azure.CosmosDB.Table installeert.

## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over de Azure Cosmos DB Table-API, [Inleiding tot Azure Cosmos DB Table-API](table-introduction.md). 
