---
title: 'Azure Cosmos DB: SQL-API voor .NET Core, SDK en resources | Microsoft Docs'
description: Meer informatie over de SQL-API voor .NET Core en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB .NET Core SDK.
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: f899b314-26ac-4ddb-86b2-bfdf05c2abf2
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/17/2017
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f8e3e0e8868c05188d9d6cb26fe6c2bd2891c17d
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="azure-cosmos-db-net-core-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB .NET Core SDK voor SQL-API: releaseopmerkingen en resources
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
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

<table>

<tr><td>**SDK downloaden**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**API-documentatie**</td><td>[.NET API-naslagdocumentatie](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Voorbeelden**</td><td>[.NET-codevoorbeelden](sql-api-dotnet-samples.md)</td></tr>

<tr><td>**Aan de slag**</td><td>[Aan de slag met Azure Cosmos DB .NET Core SDK](sql-api-dotnetcore-get-started.md)</td></tr>

<tr><td>**Zelfstudie voor web-app**</td><td>[Ontwikkeling van webtoepassing met Azure Cosmos-DB](sql-api-dotnet-application.md)</td></tr>

<tr><td>**Huidige ondersteunde framework**</td><td>[.NET standaard 1.6 en .NET standaard 1.5](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

De Azure Cosmos DB .NET Core SDK heeft functie pariteit met de nieuwste versie van de [Azure Cosmos DB .NET SDK](sql-api-sdk-dotnet.md).

> [!NOTE] 
> De Azure Cosmos DB .NET Core SDK is nog niet compatibel met Universal Windows Platform (UWP)-apps. Als u geïnteresseerd in de .NET Core-SDK die ondersteuning biedt voor UWP-apps bent, e-mailbericht verzendt [ askcosmosdb@microsoft.com ](mailto:askcosmosdb@microsoft.com).

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
 
 * Hiermee voegt de mogelijkheid om op te geven unieke indexen voor de documenten met behulp van de eigenschap UniqueKeyPolicy op de documentcollection maakt.
 * Vaste een bug waarin zijn de aangepaste instellingen voor JsonSerializer niet wordt herkend voor een aantal query's en de uitvoering van de opgeslagen procedure.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
 
 * Wijzigingen van de Azure DocumentDB naar Azure Cosmos-database in de API-verwijzing huisstijl documentatie, informatie over de metagegevens in assembly's en het NuGet-pakket. 
 * Diagnostische gegevens en de latentie van de respons van aanvragen die worden verzonden met directe verbinding modus worden blootgesteld. De namen van eigenschappen zijn RequestDiagnosticsString en RequestLatency voor ResourceResponse-klasse.
 * Deze SDK-versie moet de meest recente versie van Azure Cosmos DB Emulator die beschikbaar is voor downloaden van https://aka.ms/cosmosdb-emulator.
 
### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Verschillende oplossingen betrouwbaarheid en verbeteringen toegevoegd.

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1 

* Interne wijzigingen die zijn gerelateerd aan ondersteunende [Microsoft.Azure.Graphs](https://docs.microsoft.com/azure/cosmos-db/graph-sdk-dotnet)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Ondersteuning toegevoegd voor PartitionKeyRangeId als een FeedOption voor het vaststellen van de resultaten van de query naar een specifieke partitiebereikwaarde sleutel. 
* Ondersteuning toegevoegd voor StartTime als een ChangeFeedOption te bekijken om de wijzigingen na dit tijdstip. 

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Een probleem opgelost in de klasse JsonSerializable dat leiden een stackoverloopuitzondering tot kan.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Toegevoegde ondersteuning voor het opgeven van aangepaste JsonSerializerSettings tijdens het instantiëren van een [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) exemplaar.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   Ondersteuning voor .NET standaard 1.5 als een van de doel-frameworks.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Een probleem dat van invloed op een x64 vaste machines die geen SSE4 instructie ondersteunen en genereert SEHException bij het uitvoeren van query's Azure Cosmos DB.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Ondersteuning toegevoegd voor een nieuwe consistentieniveau ConsistentPrefix genoemd.
*   Ondersteuning toegevoegd voor query metrische gegevens voor afzonderlijke partities.
*   Ondersteuning toegevoegd voor het beperken van de grootte van het vervolgtoken voor query's.
*   Ondersteuning toegevoegd voor meer gedetailleerde tracering van mislukte aanvragen.
*   Een aantal prestatieverbeteringen aangebracht in de SDK.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Een probleem dat de waarde in FeedOptions PartitionKey voor cumulatieve query's genegeerd opgelost.
* Een probleem opgelost in transparante verwerking van de partitie management tijdens de vlucht halverwege cross-partitie Order By queryuitvoering.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Een probleem waardoor impassen in sommige van de asynchrone API's bij gebruik binnen de context van ASP.NET opgelost.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Maak SDK toleranter voor automatische failover onder bepaalde omstandigheden corrigeert.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Herstel voor een probleem dat soms een WebException veroorzaakt: de externe naam kan niet worden omgezet.
* De ondersteuning voor het lezen van een getypt document rechtstreeks met het toevoegen van nieuwe overloads aan ReadDocumentAsync API toegevoegd.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Toegevoegde ondersteuning voor LINQ voor aggregatie van query's (COUNT, MIN, MAX, SUM en Gem).
* Herstel voor een probleem geheugen-geheugenlek voor het object ConnectionPolicy is veroorzaakt door het gebruik van gebeurtenis-handler.
* Herstel voor een probleem waarbij de UpsertAttachmentAsync niet werkt is wanneer ETag is gebruikt.
* Herstel voor een probleem waarin cross partitie volgorde door de voortzetting van de query niet werken is bij het sorteren van tekenreeksveld.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Ondersteuning toegevoegd voor aggregatie van query's (COUNT, MIN, MAX, SUM en Gem). Zie [aggregatie ondersteuning](sql-api-sql-query.md#Aggregates).
* Minimaal doorvoer voor gepartitioneerde verzamelingen uit 10,100 RU/s tot 2500 RU/s verlaagd.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

De Azure Cosmos DB .NET Core SDK kunt u snel, platformoverschrijdende bouwen [ASP.NET Core](https://www.asp.net/core) en [.NET Core](https://www.microsoft.com/net/core#windows) apps uit te voeren op Windows, Mac en Linux. De nieuwste versie van de Azure Cosmos DB .NET Core SDK is volledig [Xamarin](https://www.xamarin.com) compatibel en worden gebruikt voor het ontwikkelen van toepassingen die zijn gericht op iOS-, Android- en Mono (Linux).  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview

De Azure Cosmos DB .NET Core Preview SDK kunt u snel, platformoverschrijdende bouwen [ASP.NET Core](https://www.asp.net/core) en [.NET Core](https://www.microsoft.com/net/core#windows) apps uit te voeren op Windows, Mac en Linux.

De Azure Cosmos DB .NET Core Preview SDK heeft functie pariteit met de nieuwste versie van de [Azure Cosmos DB .NET SDK](sql-api-sdk-dotnet.md) en ondersteunt de volgende:
* Alle [verbindingsmodus](performance-tips.md#networking): Gateway-modus, Direct TCP- en HTTPs Direct. 
* Alle [consistentieniveaus](consistency-levels.md): sterke, sessie, gebonden veroudering en Eventual.
* [Gepartitioneerde verzamelingen](partition-data.md). 
* [Meerdere landen/regio database accounts en geo-replicatie](distribute-data-globally.md).

Als u vragen met betrekking tot deze SDK hebt, posten naar [StackOverflow](http://stackoverflow.com/questions/tagged/azure-documentdb), of het bestand een probleem in de [github-opslagplaats](https://github.com/Azure/azure-documentdb-dotnet/issues). 

## <a name="release--retirement-dates"></a>Release & buiten gebruik stellen datums

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [1.7.1](#1.7.1) |16 november 2017 |--- |
| [1.7.0](#1.7.0) |10 november 2017 |--- |
| [1.6.0](#1.6.0) |17 oktober 2017 |--- |
| [1.5.1](#1.5.1) |02 oktober 2017 |--- |
| [1.5.0](#1.5.0) |10 augustus 2017 |--- | 
| [1.4.1](#1.4.1) |07 augustus 2017 |--- |
| [1.4.0](#1.4.0) |02 augustus 2017 |--- |
| [1.3.2](#1.3.2) |12 juni 2017 |--- |
| [1.3.1](#1.3.1) |23 mei 2017 |--- |
| [1.3.0](#1.3.0) |10 mei 2017 |--- |
| [1.2.2](#1.2.2) |19 april 2017 |--- |
| [1.2.1](#1.2.1) |29 maart 2017 |--- |
| [1.2.0](#1.2.0) |25 maart 2017 |--- |
| [1.1.2](#1.1.2) |20 maart 2017 |--- |
| [1.1.1](#1.1.1) |14 maart 2017 |--- |
| [1.1.0](#1.1.0) |16 februari 2017 |--- |
| [1.0.0](#1.0.0) |21 december 2016 |--- |
| [0.1.0-Preview](#0.1.0-preview) |15 november 2016 |31 december 2016 |

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) pagina van de service. 

