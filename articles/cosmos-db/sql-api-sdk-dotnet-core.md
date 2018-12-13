---
title: 'Azure Cosmos DB: SQL .NET Core-API, SDK en resources'
description: Meer informatie over de SQL .NET Core-API en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB .NET Core SDK.
services: cosmos-db
author: rnagpal
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4452484379b73b978cac7391bce5c71b0f6c32cd
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53257196"
---
# <a name="azure-cosmos-db-net-core-sdk-for-sql-api-release-notes-and-resources"></a>Azure Cosmos DB .NET Core-SDK voor SQL-API: Opmerkingen bij de release en resources
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

<table>

<tr><td>**SDK downloaden**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)</td></tr>

<tr><td>**API-documentatie**</td><td>[.NET API-referentiedocumentatie](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Voorbeelden**</td><td>[.NET-codevoorbeelden](sql-api-dotnet-samples.md)</td></tr>

<tr><td>**Aan de slag**</td><td>[Aan de slag met de Azure Cosmos DB .NET Core-SDK](sql-api-dotnetcore-get-started.md)</td></tr>

<tr><td>**Zelfstudie-web-app**</td><td>[Ontwikkeling van webtoepassingen met Azure Cosmos DB](sql-api-dotnet-application.md)</td></tr>

<tr><td>**Huidige ondersteunde framework**</td><td>[.NET standard 1.6 en .NET Standard 1.5](https://www.nuget.org/packages/NETStandard.Library)</td></tr>
</table></br>

## <a name="release-notes"></a>Releaseopmerkingen

De Azure Cosmos DB .NET Core SDK heeft de functiepariteit met de meest recente versie van de [Azure Cosmos DB .NET SDK](sql-api-sdk-dotnet.md).

### <a name="a-name3001-preview3001-preview"></a><a name="3.0.0.1-preview"/>3.0.0.1-Preview
* Preview-versie 1 van [versie 3.0.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) van de .NET-SDK voor de openbare preview.
* Doel .NET Standard, die ondersteuning biedt voor 4.6.1+ van .NET framework en .NET Core 2.0 +
* Nieuw objectmodel, met methoden en op het hoogste niveau CosmosClient verdeeld over relevante CosmosDatabases, CosmosContainers en CosmosItems klassen. 
* Ondersteuning voor stromen. 
* Bijgewerkte CosmosResponseMessage van server-statuscode retourneren en uitzondering alleen genereren als er geen reactie wordt geretourneerd. 

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0

* Toegevoegd voor diagnostische gegevens direct/TCP-transport, TransportException, het type van een interne uitzondering van de SDK. Wanneer aanwezig is in de uitzondering berichten, dit type af te drukken als u meer informatie voor het oplossen van problemen met de netwerkverbinding van de client.

* Toegevoegde nieuwe constructor beschikken overbelasting waarbij een HttpMessageHandler, een HTTP-handler stack moet worden gebruikt voor het verzenden van aanvragen van httpclient maakt (bijvoorbeeld HttpClientHandler).

* Los de fout waar-header met null-waarden zijn niet correct worden verwerkt.

* Verbeterde verzameling Cachevalidatie.

### <a name="a-name213213"></a><a name="2.1.3"/>2.1.3

* Bijgewerkte System.Net.Security naar 4.3.2.

### <a name="a-name212212"></a><a name="2.1.2"/>2.1.2

* Diagnostische tracering verbeteringen.

### <a name="a-name211211"></a><a name="2.1.1"/>2.1.1

* Meer flexibiliteit toegevoegd aan meerdere regio's aanvraag tijdelijke fouten.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0

* Toegevoegde meerdere regio's schrijven ondersteuning.
* Cross-partitie prestatieverbeteringen voor query's met boven- en MaxBufferedItemCount.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0

* Ondersteuning voor toegevoegde aanvraag annuleren.
* Toegevoegde SetCurrentLocation naar ConnectionPolicy, die automatisch wordt ingevuld de gewenste locaties op basis van de regio.
* Probleem opgelost in een Cross-partitie query's met Min/Max en een filter op dat overeenkomt met geen documenten op een afzonderlijke partitie.
* DocumentClient methoden hebben nu pariteit met IDocumentClient.
* Bijgewerkte direct TCP-transport-stack om te beperken van het aantal verbindingen tot stand gebracht.
* Ondersteuning toegevoegd voor Direct via TCP-modus voor niet-Windows-clients.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Ondersteuning voor toegevoegde aanvraag annuleren.
* Toegevoegde SetCurrentLocation naar ConnectionPolicy, die automatisch wordt ingevuld de gewenste locaties op basis van de regio.
* Probleem opgelost in een Cross-partitie query's met Min/Max en een filter op dat overeenkomt met geen documenten op een afzonderlijke partitie.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-Preview

* DocumentClient methoden hebben nu pariteit met IDocumentClient.
* Bijgewerkte direct TCP-transport-stack om te beperken van het aantal verbindingen tot stand gebracht.
* Ondersteuning toegevoegd voor Direct via TCP-modus voor niet-Windows-clients.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0

* Toegevoegde ConsistencyLevel eigenschap FeedOptions.
* Toegevoegde JsonSerializerSettings RequestOptions en FeedOptions.
* Toegevoegde EnableReadRequestsFallback naar ConnectionPolicy.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1

* Vaste KeyNotFoundException voor cross-partitie OrderBy-query's in de hoek gevallen.
* Probleem opgelost waarbij JsonPropery kenmerk in de component select voor LINQ-query's niet is wordt herkend.

### <a name="a-name182182"></a><a name="1.8.2"/>1.8.2

* Fout verholpen die onder bepaalde omstandigheden race is bereikt, wordt die in onregelmatige resulteert "Microsoft.Azure.Documents.NotFoundException: De gelezen sessie is niet beschikbaar voor de invoer sessietoken"fouten bij het gebruik van de sessie consistentieniveau.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1

* Regressie opgelost waarbij FeedOptions.MaxItemCount = -1 heeft veroorzaakt een System.ArithmeticException: het formaat van pagina negatief is.
* Een nieuwe functie ToString() aan QueryMetrics toegevoegd.
* Beschikbaar gemaakte partitiestatistieken voor het lezen van verzamelingen.
* PartitionKey-eigenschap toegevoegd aan ChangeFeedOptions.
* Kleine oplossingen voor problemen.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
 
 * Voegt de mogelijkheid om op te geven unieke indexen voor de documenten met behulp van de eigenschap UniqueKeyPolicy op de documentcollection maakt.
 * Een opgelost waar zijn de aangepaste instellingen voor JsonSerializer niet wordt herkend voor sommige query's en het uitvoeren van een opgeslagen procedure.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
 
 * Wijziging van de Azure DocumentDB in Azure Cosmos DB in de API-verwijzing huisstijl documentatie, informatie over de metagegevens in assembly's en het NuGet-pakket. 
 * Diagnostische gegevens en latentie van de reactie op aanvragen die worden verzonden met directe connectiviteitsmodus weergeven. De namen van eigenschappen zijn RequestDiagnosticsString en RequestLatency op ResourceResponse klasse.
 * Deze SDK-versie vereist de meest recente versie van Azure Cosmos DB Emulator beschikbaar voor downloaden van https://aka.ms/cosmosdb-emulator.
 
### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Verschillende betrouwbaarheidsoplossingen en verbeteringen toegevoegd.

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1 

* Interne wijzigingen met betrekking tot het ondersteunen van [Microsoft.Azure.Graphs](https://docs.microsoft.com/azure/cosmos-db/graph-sdk-dotnet)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Ondersteuning toegevoegd voor PartitionKeyRangeId als een FeedOption voor het vaststellen van de resultaten van de query op een specifieke partitie sleutelbereik-waarde. 
* Ondersteuning toegevoegd voor StartTime als een ChangeFeedOption te bekijken om de wijzigingen na deze periode. 

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Een probleem opgelost in de klasse JsonSerializable dat leiden een uitzondering voor stack overflow tot kan.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Ondersteuning toegevoegd voor aangepaste JsonSerializerSettings op te geven bij het instantiëren van een [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) exemplaar.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   Ondersteunt .NET Standard 1.5 als een van de doel-frameworks.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   Een probleem dat beïnvloed x64 opgelost machines die geen ondersteuning voor SSE4 instructie en SEHException throw bij het uitvoeren van Azure Cosmos DB-query's.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Er is ondersteuning toegevoegd voor een nieuwe consistentieniveau ConsistentPrefix genoemd.
*   Er is ondersteuning toegevoegd voor query metrische gegevens voor afzonderlijke partities.
*   Er is ondersteuning toegevoegd voor het beperken van de grootte van het vervolgtoken voor query's.
*   Ondersteuning toegevoegd voor meer gedetailleerde tracering van mislukte aanvragen.
*   Sommige prestatieverbeteringen doorgevoerd in de SDK.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Er is een probleem dat de opgegeven FeedOptions voor statistische query's PartitionKey-waarde genegeerd opgelost.
* Een probleem opgelost in transparante verwerking van partitie management tijdens halverwege vlucht over meerdere partities Order By queryuitvoering.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Er is een probleem waardoor impassen in enkele van de asynchrone communicatiepoort API's bij gebruik binnen de context van ASP.NET opgelost.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Problemen aan te brengen SDK beter bestand is tegen automatische failover onder bepaalde omstandigheden.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Oplossing voor een probleem waardoor af en toe een WebException: Kan de externe naam niet omzetten.
* De ondersteuning voor het lezen van een getypte document rechtstreeks door toe te voegen nieuwe overloads naar ReadDocumentAsync API toegevoegd.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* LINQ is ondersteuning toegevoegd voor aggregatie-query's (aantal, MIN, MAX, som en gemiddelde).
* Oplossing voor een probleem voor het lekken van geheugen voor de ConnectionPolicy-object dat is veroorzaakt door het gebruik van de gebeurtenis-handler.
* Correctie voor een probleem waarbij de UpsertAttachmentAsync niet werken is wanneer ETag is gebruikt.
* Oplossing voor een probleem waarbij meerdere partitie volgorde door de voortzetting van de query niet werkte bij het sorteren van tekenreeksveld.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Er is ondersteuning toegevoegd voor aggregatie-query's (aantal, MIN, MAX, som en gemiddelde). Zie [ondersteuning voor aggregatie](how-to-sql-query.md#Aggregates).
* Het minimale doorvoer op gepartitioneerde verzamelingen van 10,100 RU/s 2500 RU/s verlaagd.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

De Azure Cosmos DB .NET Core SDK kunt u bouwen snelle, platformoverschrijdende [ASP.NET Core](https://www.asp.net/core) en [.NET Core](https://www.microsoft.com/net/core#windows) apps uit te voeren op Windows, Mac en Linux. De nieuwste versie van de Azure Cosmos DB .NET Core-SDK is volledig [Xamarin](https://www.xamarin.com) compatibel en worden gebruikt voor het bouwen van toepassingen die zijn gericht op iOS, Android en Mono (Linux).  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview

De Azure Cosmos DB .NET Core Preview SDK kunt u bouwen snelle, platformoverschrijdende [ASP.NET Core](https://www.asp.net/core) en [.NET Core](https://www.microsoft.com/net/core#windows) apps uit te voeren op Windows, Mac en Linux.

De Azure Cosmos DB .NET Core Preview SDK heeft de functiepariteit met de meest recente versie van de [Azure Cosmos DB .NET SDK](sql-api-sdk-dotnet.md) en biedt ondersteuning voor het volgende:
* Alle [verbindingsmodus](performance-tips.md#networking): Gateway-modus, Direct via TCP en directe HTTPs. 
* Alle [consistentieniveaus](consistency-levels.md): Sterk, sessie, gebonden veroudering, en uiteindelijk.
* [Gepartitioneerde verzamelingen](partition-data.md). 
* [Databaseaccounts voor meerdere regio's en geo-replicatie](distribute-data-globally.md).

Als u vragen met betrekking tot deze SDK hebt, post naar [StackOverflow](https://stackoverflow.com/questions/tagged/azure-documentdb), of een probleem melden in het [github-opslagplaats](https://github.com/Azure/azure-documentdb-dotnet/issues). 

## <a name="release--retirement-dates"></a>Release & datums buiten gebruik stellen

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [2.2.0](#2.2.0) |07 december 2018 |--- |
| [2.1.3](#2.1.3) |15 oktober 2018 |--- |
| [2.1.2](#2.1.2) |04 oktober 2018 |--- |
| [2.1.1](#2.1.1) |27 september 2018 |--- |
| [2.1.0](#2.1.0) |21 september 2018 |--- |
| [2.0.0](#2.0.0) |07 september 2018 |--- |
| [1.9.1](#1.9.1) |09 maart 2018 |--- |
| [1.8.2](#1.8.2) |21 februari 2018 |--- |
| [1.8.1](#1.8.1) |05 februari 2018 |--- |
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
| [0.1.0-preview](#0.1.0-preview) |15 november 2016 |En met 31 december 2016 |

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) servicepagina. 

