---
title: Azure Cosmos DB tabel bronnen & API .NET SDK | Microsoft Docs
description: Meer informatie over de Azure Cosmos DB tabel API met inbegrip van release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie.
services: cosmos-db
author: rnagpal
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-table
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/26/2018
ms.author: rnagpal
ms.openlocfilehash: 391948af7fe00e0a5e6171d5322c09c05fcd1cc3
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34798471"
---
# <a name="azure-cosmos-db-table-net-api-download-and-release-notes"></a>Azure Cosmos DB tabel .NET API: Downloaden en release-opmerkingen
> [!div class="op_single_selector"]
> * [.NET](table-sdk-dotnet.md)
> * [Java](table-sdk-java.md)
> * [Node.js](table-sdk-nodejs.md)
> * [Python](table-sdk-python.md)

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://aka.ms/acdbtablenuget)|
|**API-documentatie**|[.NET API-naslagdocumentatie](https://aka.ms/acdbtableapiref)|
|**Snelstartgids**|[Azure Cosmos DB: Een app met .NET- en de tabel-API maken](create-table-dotnet.md)|
|**Zelfstudie**|[Azure Cosmos DB: Ontwikkelen met de tabel API in .NET](tutorial-develop-table-dotnet.md)|
|**Huidige ondersteunde framework**|[Microsoft .NET Framework 4.5.1](https://www.microsoft.com/en-us/download/details.aspx?id=40779)|

> [!IMPORTANT]
> Als u tijdens de preview een tabel-API-account hebt gemaakt, moet u een [nieuw tabel-API-account](create-table-dotnet.md#create-a-database-account) maken om te kunnen werken met de algemeen beschikbare SDK’s voor tabel-API's.
>

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Validatie van de toegevoegde voor een verkeerd ingedeelde ETAGs in de directe modus.
* Vaste bug door LINQ-query in de modus van de Gateway.
* Synchrone API's wordt nu uitgevoerd op de thread-groep met SynchronizationContext.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* TableQueryMaxItemCount, TableQueryEnableScan, TableQueryMaxDegreeOfParallelism en TableQueryContinuationTokenLimitInKb toevoegen aan TableRequestOptions
* Oplossingen voor problemen

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* Algemene beschikbaarheid release

### <a name="a-name010-preview090-preview"></a><a name="0.1.0-preview"/>0.9.0-Preview
* Eerste preview-release

## <a name="release-and-retirement-dates"></a>Release-en buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

De [WindowsAzure.Storage PremiumTable](https://www.nuget.org/packages/WindowsAzure.Storage-PremiumTable/0.1.0-preview) preview-pakket is afgeschaft en vervangen door de [Microsoft.Azure.CosmosDB.Table](https://www.nuget.org/packages/Microsoft.Azure.CosmosDB.Table) pakket. De SDK WindowsAzure.Storage PremiumTable wordt buiten gebruik worden gesteld op 15 November 2018, op dat moment aanvragen naar de buiten gebruik gestelde SDK niet worden toegestaan.

Nieuwe functies en functionaliteit en optimalisaties alleen zijn toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo snel mogelijk. 

De aanvragen die naar Azure Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.
<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.1.1](#1.1.1) |26 maart 2018|--- |
| [1.1.0](#1.1.0) |21 februari 2018|--- |
| [1.0.0](#1.0.0) |15 november 2017|--- |
| [0.9.0-preview](#0.9.0-preview) |11 november 2017 |--- |

## <a name="troubleshooting"></a>Problemen oplossen

Als het foutbericht 

```
Unable to resolve dependency 'Microsoft.Azure.Storage.Common'. Source(s) used: 'nuget.org', 
'CliFallbackFolder', 'Microsoft Visual Studio Offline Packages', 'Microsoft Azure Service Fabric SDK'`
```

bij een poging tot het Microsoft.Azure.CosmosDB.Table NuGet-pakket gebruiken, hebt u twee opties om het probleem te verhelpen:

* Pakket beheren Console gebruiken voor het installeren van het pakket Microsoft.Azure.CosmosDB.Table en de bijbehorende afhankelijkheden. Hiertoe typt u het volgende in de Package Manager-Console voor uw oplossing. 
    ```
    Install-Package Microsoft.Azure.CosmosDB.Table -IncludePrerelease
    ```
    
* Met behulp van uw voorkeur Nuget package management-hulpprogramma het Microsoft.Azure.Storage.Common Nuget-pakket installeren voordat u Microsoft.Azure.CosmosDB.Table installeert.

## <a name="faq"></a>Veelgestelde vragen

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over de Azure-API voor tabel Cosmos DB, [Inleiding tot Azure Cosmos DB tabel API](table-introduction.md). 
