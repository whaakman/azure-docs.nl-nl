---
title: 'Azure Cosmos DB: .NET wijziging Feed Processor API, SDK en resources | Microsoft Docs'
description: Meer informatie over de wijziging Feed Processor API en de SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Feed Processor SDK voor .NET wijzigen.
services: cosmos-db
documentationcenter: .net
author: ealsur
manager: kirillg
editor: mimig1
ms.assetid: f2dd9438-8879-4f74-bb6c-e1efc2cd0157
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/05/2017
ms.author: maquaran
ms.openlocfilehash: 7aa58b9a0fe4ca9e162722d277a951f8ac25013a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>.NET wijziging Feed Processor SDK: Downloaden en release-opmerkingen
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Feed van wijzigen](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/documentdb/)
> * [REST-resourceprovider](https://docs.microsoft.com/rest/api/documentdbresourceprovider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API-documentatie**|[API-naslagdocumentatie voor Processor Feed bibliotheek wijzigen](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Aan de slag**|[Aan de slag met de .NET SDK wijziging Feed-Processor](change-feed.md)|
|**Huidige ondersteunde framework**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Voegt ondersteuning toe voor .NET Standard 2.0. Het pakket biedt nu ondersteuning voor `netstandard2.0` en `net451` framework monikers.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.17.0 versies en hoger.
* Compatibel met [SQL .NET Core SDK](sql-api-sdk-dotnet-core.md) 1.5.1 versies en hoger.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Corrigeert een probleem met de berekening van de schatting van de resterende hoeveelheid werk wanneer de wijziging Feed leeg is of geen werk in behandeling was.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.13.2 versies en hoger.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Een methode voor het verkrijgen van een schatting van de resterende hoeveelheid werk moeten worden verwerkt in de wijziging Feed toegevoegd.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.13.2 versies en hoger.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) versies 1.14.1 en lager.

## <a name="release--retirement-dates"></a>Release & buiten gebruik stellen datums
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

Nieuwe functies en functionaliteit en optimalisaties alleen zijn toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo snel mogelijk. 

Elk verzoek aan de Cosmos-database met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.2.0](#1.2.0) |31 oktober 2017 |--- |
| [1.1.1](#1.1.1) |29 augustus 2017 |--- |
| [1.1.0](#1.1.0) |13 augustus 2017 |--- |
| [1.0.0](#1.0.0) |07 juli 2017 |--- |


## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) pagina van de service. 

