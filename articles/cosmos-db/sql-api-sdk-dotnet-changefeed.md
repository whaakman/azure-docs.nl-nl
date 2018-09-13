---
title: 'Azure Cosmos DB: .NET Change Feed Processor-API, SDK en resources | Microsoft Docs'
description: Meer informatie over de Change Feed Processor-API en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de .NET Change Feed Processor-SDK.
services: cosmos-db
author: ealsur
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/21/2018
ms.author: maquaran
ms.openlocfilehash: 6c4dafa5b15548b3dbc02a9c093232197b3f1400
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44716550"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>.NET change Feed Processor SDK: Downloaden en opmerkingen bij de release
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
> * [BulkExecutor - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [BulkExecutor - Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**SDK downloaden**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API-documentatie**|[Wijzigen van de Feed Processor-bibliotheek API-referentiedocumentatie](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Aan de slag**|[Aan de slag met de Change Feed Processor .NET SDK](change-feed.md)|
|**Huidige ondersteunde framework**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="v2-builds"></a>v2-builds

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Nieuwe API, taak toegevoegd&lt;IReadOnlyList&lt;RemainingPartitionWork&gt; &gt; IRemainingWorkEstimator.GetEstimatedRemainingWorkPerPartitionAsync(). Dit kan worden gebruikt om hun werk uit geschatte voor elke partitie.
* Microsoft.Azure.DocumentDB SDK 2.0 ondersteunt. Microsoft.Azure.DocumentDB 2.0 of hoger vereist.

### <a name="a-name206206"></a><a name="2.0.6"/>2.0.6
* ChangeFeedEventHost.HostName openbare eigenschap toegevoegd voor compativility met v1.

### <a name="a-name205205"></a><a name="2.0.5"/>2.0.5
* Vaste een zeldzame situatie die wordt uitgevoerd tijdens het splitsen van de partitie. De racevoorwaarde kan leiden tot lease verkrijgen en onmiddellijk het verloren gaan tijdens het splitsen van de partitie en conflicten veroorzaakt. De race voorwaarde probleem is opgelost met deze release.

### <a name="a-name204204"></a><a name="2.0.4"/>2.0.4
* GA-SDK

### <a name="a-name203-prerelease203-prerelease"></a><a name="2.0.3-prerelease"/>2.0.3-prerelease
* De volgende problemen zijn verholpen:
  * Als partition split gebeurt, kan er dubbele verwerking van documenten die zijn gewijzigd vóór de splitsing.
  * De API GetEstimatedRemainingWork heeft 0 geretourneerd als er geen leases aanwezig in de leaseverzameling zijn.

* De volgende uitzonderingen worden openbaar gemaakt. Extensies die IPartitionProcessor implementeren, kunnen deze uitzonderingen genereren.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="a-name202-prerelease202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-prerelease
* Kleine API-wijzigingen:
  * Verwijderd ChangeFeedProcessorOptions.IsAutoCheckpointEnabled die is gemarkeerd als verouderd.

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-prerelease
* Stabiliteitsverbeteringen:
  * Betere verwerking van de initialisatie van de lease-store. Wanneer de lease-archief is leeg is, wordt slechts één exemplaar van de processor kan worden geïnitialiseerd, wacht de andere.
  * Meer stable/efficiënte lease vernieuwen/vrijgeven. Vernieuwen en het vrijgeven van een lease één partitie is onafhankelijk van anderen te vernieuwen. In v1 die sequentieel worden verwerkt voor alle partities is uitgevoerd.
* Nieuwe v2-API:
  * Patroon voor flexibele constructie van de processor Builder: de klasse ChangeFeedProcessorBuilder.
    * Kan duren voordat een combinatie van parameters.
    * Kan duren DocumentClient-exemplaar voor bewaking en/of lease verzameling (niet beschikbaar in v1).
  * IChangeFeedObserver.ProcessChangesAsync duurt nu nog maar CancellationToken.
  * IRemainingWorkEstimator - de resterende werk estimator kan afzonderlijk worden gebruikt vanuit de processor.
  * Nieuwe uitbreidbaarheidspunten:
    * IParitionLoadBalancingStrategy - voor aangepaste taakverdeling van partities tussen exemplaren van de processor.
    * ILease, ILeaseManager - voor het leasebeheer van aangepaste wordt.
    * IPartitionProcessor - voor aangepaste het verwerken van wijzigingen op een partitie.
* Maakt gebruik van logboekregistratie - [LibLog](https://github.com/damianh/LibLog) bibliotheek.
* 100% compatibel met v1-API.
* Nieuwe codebasis.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) versies 1.21.1 en hoger.

### <a name="v1-builds"></a>V1-builds

### <a name="a-name133133"></a><a name="1.3.3"/>1.3.3
* Meer logboekregistratie toegevoegd.
* Er is een geheugenlek DocumentClient opgelost bij het aanroepen van de schatting onderhanden werk meerdere keren.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* Verbeteringen in de schatting onderhanden werk.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* Verbeteringen in de stabiliteit.
  * Oplossing voor het verwerken van de geannuleerde taken probleem dat tot leiden kan gestopt waarnemers op aantal partities.
* Ondersteuning voor handmatige plaatsen van controlepunten.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) versie 1.21 en hoger.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Voegt ondersteuning toe voor .NET Standard 2.0. Het pakket biedt nu ondersteuning voor `netstandard2.0` en `net451` framework monikers.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) versies 1.17.0 en hoger.
* Compatibel met [SQL .NET Core SDK](sql-api-sdk-dotnet-core.md) versies 1.5.1 en hoger.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Wanneer de Change Feed leeg is of geen werk in behandeling is, moet u een probleem opgelost met de berekening van de schatting van de resterende hoeveelheid werk.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) versies 1.13.2 en hoger.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Een methode voor het verkrijgen van een schatting van resterende hoeveelheid werk moeten worden verwerkt in de Change Feed toegevoegd.
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) versies 1.13.2 en hoger.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA-SDK
* Compatibel met [SQL .NET SDK](sql-api-sdk-dotnet.md) versies 1.14.1 en lager.


## <a name="release--retirement-dates"></a>Datums release & buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voorafgaand aan buiten gebruik stellen van een SDK soepel te verwerken de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en -optimalisatie worden alleen toegevoegd aan de huidige SDK, daarom is het raadzaam dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo vroeg mogelijk. 

Een aanvraag voor het Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.3.3](#1.3.3) |08 mei 2018 |--- |
| [1.3.2](#1.3.2) |18 april 2018 |--- |
| [1.3.1](#1.3.1) |13 maart 2018 |--- |
| [1.2.0](#1.2.0) |31 oktober 2017 |--- |
| [1.1.1](#1.1.1) |29 augustus 2017 |--- |
| [1.1.0](#1.1.0) |13 augustus 2017 |--- |
| [1.0.0](#1.0.0) |07 juli 2017 |--- |


## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) servicepagina. 

