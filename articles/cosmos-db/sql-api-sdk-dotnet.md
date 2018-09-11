---
title: 'Azure Cosmos DB: SQL .NET API, SDK en resources | Microsoft Docs'
description: Meer informatie over de SQL .NET API en SDK, inclusief release datums, buiten gebruik stellen datums en wijzigingen die zijn aangebracht tussen elke versie van de Azure Cosmos DB .NET SDK.
services: cosmos-db
author: rnagpal
manager: kfile
editor: cgronlun
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/09/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 26de3545c5d79c711703fa97cb796cd6c504f663
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44346471"
---
# <a name="azure-cosmos-db-net-sdk-for-sql-api-download-and-release-notes"></a>Azure Cosmos DB .NET SDK voor SQL-API: downloaden en opmerkingen bij de release
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

<tr><td>**SDK downloaden**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>

<tr><td>**API-documentatie**</td><td>[.NET API-referentiedocumentatie](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Voorbeelden**</td><td>[.NET-codevoorbeelden](sql-api-dotnet-samples.md)</td></tr>

<tr><td>**Aan de slag**</td><td>[Aan de slag met Azure Cosmos DB .NET SDK](sql-api-get-started.md)</td></tr>

<tr><td>**Zelfstudie-web-app**</td><td>[Ontwikkeling van webtoepassingen met Azure Cosmos DB](sql-api-dotnet-application.md)</td></tr>

<tr><td>**Huidige ondersteunde framework**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Opmerkingen bij de release
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

### <a name="a-name12201220"></a><a name="1.22.0"/>1.22.0

* Toegevoegde ConsistencyLevel eigenschap FeedOptions.
* Toegevoegde JsonSerializerSettings RequestOptions en FeedOptions.
* Toegevoegde EnableReadRequestsFallback naar ConnectionPolicy.

### <a name="a-name12111211"></a><a name="1.21.1"/>1.21.1

* Vaste KeyNotFoundException voor cross-partitie OrderBy-query's in de hoek gevallen.
* Probleem opgelost waarbij JsonProperty kenmerk in de component select voor LINQ-query's niet is wordt herkend.

### <a name="a-name12021202"></a><a name="1.20.2"/>1.20.2

* Fout verholpen die onder bepaalde omstandigheden race is bereikt, wordt die in onregelmatige resulteert "Microsoft.Azure.Documents.NotFoundException: de lezen-sessie is niet beschikbaar voor de invoer sessietoken" fouten bij het gebruik van de sessie consistentieniveau.

### <a name="a-name12011201"></a><a name="1.20.1"/>1.20.1

* Regressie opgelost waarbij FeedOptions.MaxItemCount = -1 heeft veroorzaakt een System.ArithmeticException: het formaat van pagina negatief is.
* Een nieuwe functie ToString() aan QueryMetrics toegevoegd.
* Beschikbaar gemaakte partitiestatistieken voor het lezen van verzamelingen.
* PartitionKey-eigenschap toegevoegd aan ChangeFeedOptions.
* Kleine oplossingen voor problemen.

### <a name="a-name11911191"></a><a name="1.19.1"/>1.19.1

* Voegt de mogelijkheid om op te geven unieke indexen voor de documenten met behulp van de eigenschap UniqueKeyPolicy op de documentcollection maakt.
* Een opgelost waar zijn de aangepaste instellingen voor JsonSerializer niet wordt herkend voor sommige query's en het uitvoeren van een opgeslagen procedure.

### <a name="a-name11901190"></a><a name="1.19.0"/>1.19.0

* Wijziging van de Azure DocumentDB in Azure Cosmos DB in de API-verwijzing huisstijl documentatie, informatie over de metagegevens in assembly's en het NuGet-pakket. 
* Diagnostische gegevens en latentie van de reactie op aanvragen die worden verzonden met directe connectiviteitsmodus weergeven. De namen van eigenschappen zijn RequestDiagnosticsString en RequestLatency op ResourceResponse klasse.
* Deze SDK-versie vereist de meest recente versie van Azure Cosmos DB Emulator beschikbaar voor downloaden van https://aka.ms/cosmosdb-emulator. 

### <a name="a-name11811181"></a><a name="1.18.1"/>1.18.1 

* Interne wijzigingen voor Microsoft vrienden assembly's.

### <a name="a-name11801180"></a><a name="1.18.0"/>1.18.0 

* Verschillende betrouwbaarheidsoplossingen en verbeteringen toegevoegd.

### <a name="a-name11701170"></a><a name="1.17.0"/>1.17.0 

* Ondersteuning toegevoegd voor PartitionKeyRangeId als een FeedOption voor het vaststellen van de resultaten van de query op een specifieke partitie sleutelbereik-waarde. 
* Ondersteuning toegevoegd voor StartTime als een ChangeFeedOption te bekijken om de wijzigingen na deze periode.

### <a name="a-name11611161"></a><a name="1.16.1"/>1.16.1
* Een probleem opgelost in de klasse JsonSerializable dat leiden een uitzondering voor stack overflow tot kan.

### <a name="a-name11601160"></a><a name="1.16.0"/>1.16.0
*   Een probleem opgelost dat vereist compileren van de toepassing vanwege de introductie van JsonSerializerSettings als een optionele parameter in de DocumentClient-constructor.
* De DocumentClient-constructor die verouderd zijn gemarkeerd die vereiste JsonSerializerSettings als de laatste parameter zijn om toe te staan voor de standaardwaarden van ConnectionPolicy en ConsistencyLevel parameters als in de parameter JsonSerializerSettings wordt doorgegeven.

### <a name="a-name11501150"></a><a name="1.15.0"/>1.15.0
*   Ondersteuning toegevoegd voor aangepaste JsonSerializerSettings op te geven bij het instantiëren [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet).

### <a name="a-name11411141"></a><a name="1.14.1"/>1.14.1
*   Een probleem dat beïnvloed x64 opgelost machines die geen ondersteuning voor SSE4 instructie en genereert een SEHException bij het uitvoeren van Azure Cosmos DB SQL-query's.

### <a name="a-name11401140"></a><a name="1.14.0"/>1.14.0
*   Er is ondersteuning toegevoegd voor een nieuwe consistentieniveau ConsistentPrefix genoemd.
*   Er is ondersteuning toegevoegd voor query metrische gegevens voor afzonderlijke partities.
*   Er is ondersteuning toegevoegd voor het beperken van de grootte van het vervolgtoken voor query's.
*   Ondersteuning toegevoegd voor meer gedetailleerde tracering van mislukte aanvragen.
*   Sommige prestatieverbeteringen doorgevoerd in de SDK.

### <a name="a-name11341134"></a><a name="1.13.4"/>1.13.4
* Functioneel hetzelfde als 1.13.3. Aantal interne wijzigingen aangebracht.

### <a name="a-name11331133"></a><a name="1.13.3"/>1.13.3
* Functioneel hetzelfde als 1.13.2. Aantal interne wijzigingen aangebracht.

### <a name="a-name11321132"></a><a name="1.13.2"/>1.13.2
* Er is een probleem dat de opgegeven FeedOptions voor statistische query's PartitionKey-waarde genegeerd opgelost.
* Een probleem opgelost in transparante verwerking van partitie management tijdens halverwege vlucht over meerdere partities Order By queryuitvoering.

### <a name="a-name11311131"></a><a name="1.13.1"/>1.13.1
* Er is een probleem waardoor impassen in enkele van de asynchrone communicatiepoort API's bij gebruik binnen de context van ASP.NET opgelost.

### <a name="a-name11301130"></a><a name="1.13.0"/>1.13.0
* Problemen aan te brengen SDK beter bestand is tegen automatische failover onder bepaalde omstandigheden.

### <a name="a-name11221122"></a><a name="1.12.2"/>1.12.2
* Oplossing voor een probleem waardoor af en toe een WebException: de externe naam kan niet worden omgezet.
* De ondersteuning voor het lezen van een getypte document rechtstreeks door toe te voegen nieuwe overloads naar ReadDocumentAsync API toegevoegd.

### <a name="a-name11211121"></a><a name="1.12.1"/>1.12.1
* LINQ is ondersteuning toegevoegd voor aggregatie-query's (aantal, MIN, MAX, som en gemiddelde).
* Oplossing voor een probleem voor het lekken van geheugen voor de ConnectionPolicy-object dat is veroorzaakt door het gebruik van de gebeurtenis-handler.
* Correctie voor een probleem waarbij de UpsertAttachmentAsync niet werken is wanneer ETag is gebruikt.
* Oplossing voor een probleem waarbij meerdere partitie volgorde door de voortzetting van de query niet werkte bij het sorteren van tekenreeksveld.

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Er is ondersteuning toegevoegd voor aggregatie-query's (aantal, MIN, MAX, som en gemiddelde). Zie [ondersteuning voor aggregatie](sql-api-sql-query.md#Aggregates).
* Het minimale doorvoer op gepartitioneerde verzamelingen van 10,100 RU/s 2500 RU/s verlaagd.

### <a name="a-name11141114"></a><a name="1.11.4"/>1.11.4
* Correctie voor een probleem waarbij sommige van de partitie-overkoepelende query's in het hostproces van de 32-bits zijn mislukken.
* Oplossing voor een probleem waarbij de sessie-container is niet bijgewerkt met het token voor mislukte aanvragen in de modus van de Gateway.
* Oplossing voor een probleem waarbij een query met UDF-in projectie aanroepen is in sommige gevallen mislukt.
* Prestaties aan clientzijde oplossingen voor het verhogen van de doorvoer voor lezen en schrijven van de aanvragen.

### <a name="a-name11131113"></a><a name="1.11.3"/>1.11.3
* Correctie voor een probleem waarbij de sessie-container niet met het token voor mislukte aanvragen bijgewerkt is.
* Ondersteuning toegevoegd voor de SDK om te werken in een 32-bits proces. Houd er rekening mee dat als u cross-partitie query's, 64-bits host verwerking wordt aanbevolen voor verbeterde prestaties.
* Verbeterde prestaties voor scenario's met betrekking tot query's met een groot aantal partitiesleutelwaarden die zijn in een expressie IN.
* Ingevuld verschillende resource quotum statistieken in de ResourceResponse voor documentverzameling schijfleesaanvragen als PopulateQuotaInfo aanvraagoptie is ingesteld.

### <a name="a-name11111111"></a><a name="1.11.1"/>1.11.1
* Secundaire prestaties oplossing voor de CreateDocumentCollectionIfNotExistsAsync-API die is geïntroduceerd in 1.11.0.
* Prestaties oplossen in de SDK voor scenario's voor hoge mate van gelijktijdige aanvragen.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Ondersteuning voor nieuwe klassen en methoden voor het verwerken van de [wijzigingenfeed](change-feed.md) van documenten binnen een verzameling.
* Ondersteuning voor voortzetting van de partitie-overkoepelende query en een aantal verbeteringen voor prestaties voor partitie-overkoepelende query's.
* Toevoeging van CreateDatabaseIfNotExistsAsync en CreateDocumentCollectionIfNotExistsAsync methoden.
* LINQ-ondersteuning voor systeemfuncties: IsDefined-, IsNull- en IsPrimitive.
* Oplossing voor automatische binplacing Microsoft.Azure.Documents.ServiceInterop.dll en DocumentDB.Spatial.Sql.dll assembly's naar de map bin van toepassing wanneer u het Nuget-pakket met projecten die mogelijk project.json.
* Ondersteuning voor het verzenden van de client side ETW-traceringen die mogelijk nuttig zijn bij het opsporen van fouten in scenario's.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Directe connectiviteit toegevoegde ondersteuning voor gepartitioneerde verzamelingen.
* Verbeterde prestaties voor het consistentieniveau gebonden veroudering.
* Toegevoegde veelhoek en LineString DataTypes tijdens het instellen van de verzameling indexeringsbeleid voor geofencing ruimtelijke query's.
* LINQ is ondersteuning toegevoegd voor StringEnumConverter, IsoDateTimeConverter en UnixDateTimeConverter bij het vertalen van predikaten.
* Oplossingen voor verschillende SDK-fouten.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Er is een probleem waardoor de volgende NotFoundException opgelost: de lezen-sessie is niet beschikbaar voor de invoer sessietoken. Deze uitzondering is opgetreden in sommige gevallen bij query's uitvoeren voor de leesregio van een geografisch gedistribueerde-account.
* De eigenschap ResponseStream in de klasse ResourceResponse, waarmee directe toegang tot de onderliggende stream van een antwoord weergegeven.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* De ResourceResponse, FeedResponse, StoredProcedureResponse en MediaResponse klassen voor het implementeren van de bijbehorende openbare interface, zodat ze kunnen worden simulatie voor test-driven implementatie (TDD) is gewijzigd.
* Er is een probleem dat de koptekst van een onjuist gevormd partitie heeft bij het gebruik van een aangepaste JsonSerializerSettings-object om gegevens te serialiseren opgelost.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Er is een probleem dat langlopende query's mislukken met fout veroorzaakt opgelost: verificatietoken is niet geldig op dit moment.
* Er is een probleem dat de oorspronkelijke SqlParameterCollection van cross-partitie boven/volgorde-by-query's verwijderd opgelost.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Ondersteuning toegevoegd voor parallelle query's voor gepartitioneerde verzamelingen.
* Ondersteuning toegevoegd voor cross-partitie ORDER BY en boven-query's voor gepartitioneerde verzamelingen.
* De ontbrekende verwijzingen naar DocumentDB.Spatial.Sql.dll en Microsoft.Azure.Documents.ServiceInterop.dll die vereist zijn wanneer u verwijst naar een Azure Cosmos DB-project met een verwijzing naar het Azure Cosmos DB Nuget-pakket opgelost.
* Parameters van de verschillende typen kunt gebruiken bij het gebruik van de gebruiker gedefinieerde functies in LINQ opgelost. 
* Een bug voor wereldwijd gerepliceerde accounts opgelost waarbij Upsert-aanroepen zijn omgeleid naar de locaties in plaats van schrijven locaties lezen.
* Toegevoegde methoden aan de interface IDocumentClient die ontbraken: 
  * UpsertAttachmentAsync-methode die mediaStream en opties als parameters
  * CreateAttachmentAsync-methode die opties als een parameter
  * Methode CreateOfferQuery die querySpec als parameter.
* Niet-verzegeld openbare klassen die worden weergegeven in de interface IDocumentClient.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* De ondersteuning voor databaseaccounts voor meerdere regio's toegevoegd.
* Ondersteuning toegevoegd voor nieuwe pogingen op beperkte aanvragen.  Gebruiker kan het aantal nieuwe pogingen en de maximale wachttijd aanpassen door het configureren van de eigenschap ConnectionPolicy.RetryOptions.
* Een nieuwe IDocumentClient-interface waarmee de handtekeningen van alle DocumenClient eigenschappen en methoden toegevoegd.  Als onderdeel van deze wijziging ook gewijzigd uitbreidingsmethoden IQueryable en IOrderedQueryable methoden voor de DocumentClient-klasse zelf maken.
* De configuratieoptie om in te stellen de ServicePoint.ConnectionLimit voor een opgegeven Uri van de Azure Cosmos DB-eindpunt is toegevoegd.  Gebruik ConnectionPolicy.MaxConnectionLimit om te wijzigen van de standaardwaarde, die is ingesteld op 50.
* Afgeschafte IPartitionResolver en de uitvoering ervan.  Ondersteuning voor IPartitionResolver is nu verouderd. Het verdient aanbeveling gebruik te maken van gepartitioneerde verzamelingen voor hogere opslag en doorvoer.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Toegevoegd aan een overbelasting naar Uri op basis van ExecuteStoredProcedureAsync-methode die RequestOptions als parameter.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Toegevoegde tijd naar live (TTL) ondersteuning voor documenten.

### <a name="a-name163163"></a><a name="1.6.3"/>1.6.3
* Een bug opgelost in Nuget-pakket van .NET-SDK voor het verpakken als onderdeel van een Azure Cloud Service-oplossing.

### <a name="a-name162162"></a><a name="1.6.2"/>1.6.2
* Geïmplementeerd [gepartitioneerde verzamelingen](partition-data.md) en [niveau van de gebruiker gedefinieerde prestaties](performance-levels.md). 

### <a name="a-name153153"></a><a name="1.5.3"/>1.5.3
* **[Vaste]**  Uitvoeren van query's van Azure Cosmos DB-eindpunt genereert: ' System.Net.Http.HttpRequestException: fout bij het kopiëren van inhoud naar een stream'.

### <a name="a-name152152"></a><a name="1.5.2"/>1.5.2
* Uitgebreide LINQ ondersteunen met inbegrip van nieuwe operators voor wisselbestand, voorwaardelijke expressies en bereik van de vergelijking.
  * Selecteer boven in het LINQ inschakelen-operator nemen
  * De operator CompareTo bereik tekenreeksvergelijkingen inschakelen
  * Voorwaardelijke (?) en het samenvoegen van operators (?)
* **[Vaste]**  Opgetreden bij het combineren van Model projectie met waar u In een LINQ-query. [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* **[Vaste]**  Als Selecteer niet de laatste expressie de LINQ-Provider ervan uitgegaan dat er geen projectie en selecteren die worden geproduceerd * onjuist.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Geïmplementeerde Upsert, UpsertXXXAsync toegevoegd methoden
* Verbeterde prestaties voor alle aanvragen
* Provider van de LINQ-ondersteuning voor voorwaardelijke, samenvoegen, en CompareTo methoden voor tekenreeksen
* **[Vaste]**  LINQ-provider--> implementeren bevat de methode die u in de lijst voor het genereren van de dezelfde SQL op IEnumerable en matrix
* **[Vaste]**  BackoffRetryUtility de dezelfde het HttpRequestMessage opnieuw gebruikt in plaats van het maken van een nieuw wachtwoord opnieuw wordt uitgevoerd
* **[Verouderd]**  UriFactory.CreateCollection--> moet nu UriFactory.CreateDocumentCollection gebruiken

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1
* **[Vaste]**  Lokalisatie problemen bij het gebruik van niet-en cultuur informatie, zoals nl-NL, enzovoort. 

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* ID-gebaseerde routering toegevoegd
  * Nieuwe UriFactory helper om te helpen bij het maken van-ID op basis van resourcekoppelingen
  * Nieuwe overloads op DocumentClient te nemen in de URI
* IsValid() en IsValidDetailed() in LINQ voor georuimtelijke toegevoegd
* Verbeterde ondersteuning voor LINQ-Provider:
  * **Math** -Abs, Acos, Asin, Atan, maximum, Cos, Exp, Floor, Log, Log10, Pow, Round, ondertekenen, Sin, WORTEL, Tan, afkappen
  * **Tekenreeks** -Concat, bevat, EndsWith, IndexOf, aantal, ToLower, TrimStart, vervangen, omkeren, TrimEnd, StartsWith, subtekenreeks, ToUpper
  * **Matrix** -Concat, bevat, aantal
  * **IN** operator

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Ondersteuning toegevoegd voor indexering beleid aan te passen.
  * Nieuwe ReplaceDocumentCollectionAsync-methode in DocumentClient
  * Nieuwe IndexTransformationProgress eigenschap in ResourceResponse<T> voor het bijhouden van percentage voortgang van de index bij beleidswijzigingen
  * Er is nu DocumentCollection.IndexingPolicy veranderlijke
* Ondersteuning toegevoegd voor ruimtelijke indexering en query's uitvoeren.
  * Nieuwe Microsoft.Azure.Documents.Spatial naamruimte voor het serialiseren van/deserialiseren van ruimtelijke typen, zoals Point- and -veelhoek
  * Nieuwe SpatialIndex klasse voor het indexeren van GeoJSON-gegevens die zijn opgeslagen in Cosmos DB
* **[Vaste]**  Onjuist SQL-query gegenereerd op basis van een LINQ-expressie [#38](https://github.com/Azure/azure-documentdb-net/issues/38).

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Een afhankelijkheid op Newtonsoft.Json v5.0.7 toegevoegd.
* Wijzigingen voor de ondersteuning van Order By:
  
  * Ondersteuning voor LINQ-provider voor OrderBy() of OrderByDescending()
  * IndexingPolicy ter ondersteuning van Order By 
    
    **Mogelijk belangrijke wijziging** 
    
    Als u bestaande code die bepaalde verzamelingen met een aangepast indexeringsbeleid hebt, klikt u vervolgens moet uw bestaande code worden bijgewerkt ter ondersteuning van de nieuwe IndexingPolicy-klasse. Als u geen aangepast indexeringsbeleid hebt, klikt u vervolgens deze wijziging heeft geen invloed op u.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Er is ondersteuning toegevoegd voor het partitioneren van gegevens met behulp van de nieuwe HashPartitionResolver en RangePartitionResolver-klassen en de IPartitionResolver.
* Toegevoegde DataContract serialisatie.
* Toegevoegde GUID ondersteund in LINQ-provider.
* Toegevoegde UDF ondersteund in LINQ.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA-SDK

## <a name="release--retirement-dates"></a>Datums release & buiten gebruik stellen
Microsoft biedt melding ten minste **12 maanden** voorafgaand aan buiten gebruik stellen van een SDK soepel te verwerken de overgang naar een nieuwere/ondersteunde versie.

Nieuwe functies en functionaliteit en -optimalisatie worden alleen toegevoegd aan de huidige SDK, daarom is het raadzaam dat u altijd een upgrade uitvoert naar de nieuwste SDK versie zo vroeg mogelijk. 

De aanvragen die naar Azure Cosmos DB met behulp van een buiten gebruik gestelde SDK worden geweigerd door de service.

<br/>

| Versie | Releasedatum | Vervaldatum |
| --- | --- | --- |
| [2.0.0](#2.0.0) |07 september 2018 |--- |
| [1.22.0](#1.22.0) |19 april 2018 |--- |
| [1.21.1](#1.20.1) |09 maart 2018 |--- |
| [1.20.2](#1.20.1) |21 februari 2018 |--- |
| [1.20.1](#1.20.1) |05 februari 2018 |--- |
| [1.19.1](#1.19.1) |16 november 2017 |--- |
| [1.19.0](#1.19.0) |10 november 2017 |--- |
| [1.18.1](#1.18.1) |07 november 2017 |--- |
| [1.18.0](#1.18.0) |17 oktober 2017 |--- |
| [1.17.0](#1.17.0) |10 augustus 2017 |--- |
| [1.16.1](#1.16.1) |07 augustus 2017 |--- |
| [1.16.0](#1.16.0) |02 augustus 2017 |--- |
| [1.15.0](#1.15.0) |En met 30 juni 2017 |--- |
| [1.14.1](#1.14.1) |23 mei 2017 |--- |
| [1.14.0](#1.14.0) |10 mei 2017 |--- |
| [1.13.4](#1.13.4) |09 mei 2017 |--- |
| [1.13.3](#1.13.3) |06 mei 2017 |--- |
| [1.13.2](#1.13.2) |19 april 2017 |--- |
| [1.13.1](#1.13.1) |29 maart 2017 |--- |
| [1.13.0](#1.13.0) |24 maart 2017 |--- |
| [1.12.2](#1.12.2) |20 maart 2017 |--- |
| [1.12.1](#1.12.1) |14 maart 2017 |--- |
| [1.12.0](#1.12.0) |15 februari 2017 |--- |
| [1.11.4](#1.11.4) |06 februari 2017 |--- |
| [1.11.3](#1.11.3) |26 januari 2017 |--- |
| [1.11.1](#1.11.1) |21 december 2016 |--- |
| [1.11.0](#1.11.0) |08 december 2016 |--- |
| [1.10.0](#1.10.0) |27 september 2016 |--- |
| [1.9.5](#1.9.5) |01 september 2016 |--- |
| [1.9.4](#1.9.4) |24 augustus 2016 |--- |
| [1.9.3](#1.9.3) |15 augustus 2016 |--- |
| [1.9.2](#1.9.2) |23 juli 2016 |--- |
| [1.8.0](#1.8.0) |14 juni 2016 |--- |
| [1.7.1](#1.7.1) |06 mei 2016 |--- |
| [1.7.0](#1.7.0) |26 april 2016 |--- |
| [1.6.3](#1.6.3) |08 april 2016 |--- |
| [1.6.2](#1.6.2) |29 maart 2016 |--- |
| [1.5.3](#1.5.3) |19 februari 2016 |--- |
| [1.5.2](#1.5.2) |14 december 2015 |--- |
| [1.5.1](#1.5.1) |23 november 2015 |--- |
| [1.5.0](#1.5.0) |05 oktober 2015 |--- |
| [1.4.1](#1.4.1) |25 augustus 2015 |--- |
| [1.4.0](#1.4.0) |13 augustus 2015 |--- |
| [1.3.0](#1.3.0) |05 augustus 2015 |--- |
| [1.2.0](#1.2.0) |06 juli 2015 |--- |
| [1.1.0](#1.1.0) |En met 30 april 2015 |--- |
| [1.0.0](#1.0.0) |08 april 2015 |--- |


## <a name="faq"></a>Veelgestelde vragen
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Zie ook
Zie voor meer informatie over Cosmos DB, [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) servicepagina. 

