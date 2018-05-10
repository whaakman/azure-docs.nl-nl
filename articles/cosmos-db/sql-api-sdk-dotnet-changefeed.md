---
title: 'Azure Cosmos DB: .NET wijziging Feed Processor API, SDK en resources | Microsoft Docs'
description: Meer informatie over de wijziging Feed Processor API en de SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Feed Processor SDK voor .NET wijzigen.
services: cosmos-db
documentationcenter: .net
author: ealsur
manager: kfile
ms.assetid: f2dd9438-8879-4f74-bb6c-e1efc2cd0157
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/19/2018
ms.author: maquaran
ms.openlocfilehash: 7ed5772df4d8677fe878d7ced831dc15bbe8cac0
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>.NET wijziging Feed Processor SDK: Downloaden en release-opmerkingen
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET-Feed van wijzigen](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST-resourceprovider](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API-documentatie**|[API-naslagdocumentatie voor Processor Feed bibliotheek wijzigen](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Aan de slag**|[Aan de slag met de .NET SDK wijziging Feed-Processor](change-feed.md)|
|**Huidige ondersteunde framework**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="stable-builds"></a>Stabiele builds

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* Verbeteringen in de schatting van de werkitems in de wachtrij.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* Verbeteringen in stabiliteit.
  * Oplossing voor het verwerken van de geannuleerde taken probleem dat tot leiden kan observers is gestopt op een aantal partities.
* Ondersteuning voor handmatige plaatsen van controlepunten.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.21 versies en hoger.

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

### <a name="pre-release-builds"></a>Voorlopige versie builds

### <a name="a-name202-prerelease202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-prerelease
* Kleine wijzigingen in API:
  * ChangeFeedProcessorOptions.IsAutoCheckpointEnabled die is gemarkeerd als verouderd verwijderd.

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-prerelease
* Verbeteringen in stabiliteit:
  * Betere verwerking van het initialiseren van de lease. Wanneer lease-archief leeg, is alleen kan worden geïnitialiseerd door één exemplaar van de processor, wacht de andere.
  * Meer stabiel/efficiënt lease vernieuwing en release. Vernieuwen en het vrijgeven van een lease één partitie is onafhankelijk van het vernieuwen van anderen. In v1 die sequentieel is uitgevoerd voor alle partities.
* Nieuwe v2 API:
  * Patroon van de opbouwfunctie voor flexibele constructie van de processor: de ChangeFeedProcessorBuilder-klasse.
    * Kan duren voordat een combinatie van parameters.
    * Kan duren DocumentClient exemplaar voor bewaking en/of lease verzameling (niet beschikbaar in v1).
  * IChangeFeedObserver.ProcessChangesAsync neemt nu CancellationToken.
  * IRemainingWorkEstimator - de resterende werk estimator kan afzonderlijk worden gebruikt vanuit de processor.
  * Nieuwe uitbreidbaarheidspunten:
    * IParitionLoadBalancingStrategy - voor de aangepaste-taakverdeling van partities tussen exemplaren van de processor.
    * ILease wordt, ILeaseManager - voor het beheer van aangepaste lease.
    * IPartitionProcessor - verwerking van aangepaste wijzigingen op een partitie.
* Maakt gebruik van logboekregistratie - [LibLog](https://github.com/damianh/LibLog) bibliotheek.
* 100% compatibel met v1 API.
* Nieuwe codebasis.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.21.1 versies en hoger.

## <a name="release--retirement-dates"></a>Release & buiten gebruik stellen datums
Microsoft biedt melding ten minste **12 maanden** voordat het buiten gebruik stellen van een SDK om de overgang naar een nieuwere/ondersteunde versie vloeiend.

Nieuwe functies en functionaliteit en optimalisaties alleen zijn toegevoegd aan de huidige SDK, als zodanig wordt aanbevolen dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo snel mogelijk. 

Elk verzoek aan de Cosmos-database met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.3.2](#1.3.2) |18 april 2018 |--- |
| [1.3.1](#1.3.1) |13 maart 2018 |--- |
| [1.2.0](#1.2.0) |31 oktober 2017 |--- |
| [1.1.1](#1.1.1) |29 augustus 2017 |--- |
| [1.1.0](#1.1.0) |13 augustus 2017 |--- |
| [1.0.0](#1.0.0) |07 juli 2017 |--- |


## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) pagina van de service. 

