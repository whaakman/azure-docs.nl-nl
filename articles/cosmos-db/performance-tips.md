---
title: Azure Cosmos DB tips voor betere prestaties voor .NET | Microsoft Docs
description: Informatie over client-configuratieopties voor het verbeteren van de prestaties van de Azure Cosmos DB-database
keywords: over het verbeteren van de prestaties van de database
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: sngun
ms.openlocfilehash: 47896b681c2ba43d4ca41682ec51e0dfd167c9d5
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700953"
---
# <a name="performance-tips-for-azure-cosmos-db-and-net"></a>Tips voor betere prestaties voor Azure Cosmos DB en .NET

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB is een snelle en flexibele gedistribueerde database die kan worden opgeschaald naadloos met een gegarandeerde latentie en doorvoer. U hoeft niet te grote architectuur wijzigingen aanbrengen of complexe code schrijven om te schalen van uw database met Azure Cosmos DB. Omhoog en omlaag schalen is net zo gemakkelijk als het maken van één API-aanroep of [SDK methodeaanroep](set-throughput.md#set-throughput-sdk). Omdat Azure Cosmos DB is toegankelijk via netwerkaanroepen er zijn echter client-side '-optimalisatie u maken kunt voor het behalen van optimale prestaties bij het gebruik van de [SQL .NET SDK](documentdb-sdk-dotnet.md).

Dus als u vraagt "hoe kan ik mijn de databaseprestaties verbeteren?" Houd rekening met de volgende opties:

## <a name="networking"></a>Netwerken
<a id="direct-connection"></a>

1. **Verbindingsbeleid: modus met rechtstreekse verbinding gebruiken**

    Hoe een client verbinding maakt met Azure Cosmos DB heeft belangrijke gevolgen voor de prestaties, met name wat betreft waargenomen client-side-latentie. Er zijn twee belangrijke configuratie-instellingen beschikbaar voor het configureren van client verbindingsbeleid: de verbinding *modus* en de [verbinding *protocol*](#connection-protocol).  De twee modi beschikbaar zijn:

   * Gateway-modus (standaard)
      
     Gateway-modus wordt ondersteund op alle platforms van de SDK en de geconfigureerde standaardwaarde is. Als uw toepassing wordt uitgevoerd vanuit een bedrijfsnetwerk met strikte firewallbeperkingen, is Gateway-modus de beste keuze, omdat deze de standaard HTTPS-poort en één eindpunt gebruikt. De negatieve gevolgen voor de prestaties, is echter dat modus van de Gateway bestaat uit een aanvullend netwerk hop telkens wanneer gegevens worden gelezen of geschreven naar Azure Cosmos DB. Als gevolg hiervan biedt de modus Direct betere prestaties vanwege minder netwerkhops. Gateway-verbindingsmodus wordt ook aanbevolen wanneer u toepassingen in omgevingen met een beperkt aantal socketverbindingen, bijvoorbeeld uitvoeren bij het gebruik van Azure Functions of als u van een verbruiksabonnement gebruikmaakt. 

   * De modus direct

     De modus direct biedt ondersteuning voor connectiviteit via TCP- en HTTPS-protocollen. Direct wordt momenteel ondersteund in .NET Standard 2.0 voor alleen Windows-platform. Als u Direct gebruikt, moet u er twee protocolopties die beschikbaar zijn:

    * TCP
    * HTTPS

    Wanneer u de Gateway gebruikt, Azure Cosmos DB maakt gebruik van poort 443 en MongoDB-API maakt gebruik van 10250, 10255 en 10256 poorten. De 10250 poort is toegewezen aan een Mongodb-exemplaar van standaard zonder geo-replicatie en 10255/10256 poorten toewijzen aan de Mongodb-exemplaar met geo-replicatie-functionaliteit. Wanneer u TCP in de directe modus, behalve de poorten van de Gateway, moet u om te controleren of de poort bereik tussen 10000 en 20000 is geopend, omdat Azure Cosmos DB maakt gebruik van dynamische TCP-poorten. Als deze poorten niet geopend zijn en u probeert om TCP te gebruiken, ontvangt u een 503 Service niet beschikbaar-fout. De volgende tabel ziet u modi voor connectiviteit beschikbaar voor verschillende API's en de gebruiker van de service-poorten voor elke API:

    |Verbindingsmodus  |Ondersteunde protocollen  |Ondersteunde SDK 's  |API/Service-poort  |
    |---------|---------|---------|---------|
    |Gateway  |   HTTPS    |  Alle SDK 's    |   SQL(443), Mongo (10250, 10255, 10256), Table(443), Cassandra(443), Graph(443)    |
    |Rechtstreeks    |    HTTPS     |  .NET en Java-SDK    |    SQL(443)   |
    |Rechtstreeks    |     TCP    |  .NET SDK    | Poorten binnen het bereik van 10.000 20.000 |

    Azure Cosmos DB biedt een eenvoudige en open RESTful-programmeermodel via HTTPS. Daarnaast biedt deze een efficiënte TCP-protocol, dat is ook RESTful in het communicatiemodel en is beschikbaar via de .NET-client-SDK. Zowel Direct via TCP- en HTTPS gebruik van SSL voor initiële verificatie en versleuteling van verkeer. Gebruik indien mogelijk de TCP-protocol voor de beste prestaties.

    De Connectiviteitsmodus wordt geconfigureerd tijdens het samenstellen van het DocumentClient-exemplaar met de ConnectionPolicy-parameter. Als de modus Direct wordt gebruikt, kan het Protocol ook binnen de ConnectionPolicy-parameter worden ingesteld.

    ```csharp
    var serviceEndpoint = new Uri("https://contoso.documents.net");
    var authKey = new "your authKey from the Azure portal";
    DocumentClient client = new DocumentClient(serviceEndpoint, authKey,
    new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp
    });
    ```

    Omdat TCP wordt alleen ondersteund in de directe modus als Gateway-modus wordt gebruikt, klikt u vervolgens het HTTPS-protocol wordt altijd gebruikt voor communicatie met de Gateway en de waarde van het Protocol in de ConnectionPolicy wordt genegeerd.

    ![Afbeelding van het beleid voor Azure Cosmos DB-verbinding](./media/performance-tips/connection-policy.png)

2. **OpenAsync om te voorkomen dat latentie opstarten op de eerste aanvraag aanroepen**

    Standaard is de eerste aanvraag een hogere latentie omdat er voor het ophalen van de adres-routeringstabel. Om te voorkomen dat deze latentie opstarten op de eerste aanvraag, moet u OpenAsync() als volgt één keer tijdens de initialisatie van aanroepen.

        await client.OpenAsync();
   <a id="same-region"></a>
3. **Clients in hetzelfde Azure-regio voor de prestaties plaatsen**

    Indien mogelijk, plaatst u alle toepassingen aanroepen van Azure Cosmos DB in dezelfde regio als de Azure Cosmos DB-database. Voor een geschatte vergelijking: aanroepen naar Azure Cosmos DB binnen dezelfde regio binnen 1-2 ms voltooid, maar de latentie tussen de West- en oostkust van de Verenigde Staten is > 50 ms. Deze latentie kan waarschijnlijk uit om aan te vragen variëren, afhankelijk van de route die door de aanvraag is uitgevoerd, zoals het aan de grens van de Azure-datacenter van de client doorgeeft. De laagst mogelijke latentie wordt bereikt door ervoor te zorgen dat de aanroepende toepassing bevindt zich in dezelfde Azure-regio als de ingerichte Azure Cosmos DB-eindpunt. Zie voor een lijst van beschikbare regio's, [Azure-regio's](https://azure.microsoft.com/regions/#services).

    ![Afbeelding van het beleid voor Azure Cosmos DB-verbinding](./media/performance-tips/same-region.png)
   <a id="increase-threads"></a>
4. **Verhoog het aantal threads/taken**

    Omdat aanroepen naar Azure Cosmos DB zijn gemaakt via het netwerk, moet u mogelijk de graad van parallelle uitvoering van de aanvragen verschillen, zodat de clienttoepassing in de taakwachtrij doorbrengt weinig tijd wachten tussen aanvragen. Bijvoorbeeld, als u gebruikt. De NET [taak parallelle bibliotheek](https://msdn.microsoft.com//library/dd460717.aspx), maken in de volgorde van 100s van taken lezen of schrijven naar Azure Cosmos DB.

## <a name="sdk-usage"></a>SDK-gebruik
1. **Installeer de meest recente SDK**

    De Azure Cosmos DB SDK's worden voortdurend verbeterd om de beste prestaties bieden. Zie de [Azure Cosmos DB SDK](documentdb-sdk-dotnet.md) pagina's om te bepalen van de meest recente SDK en verbeteringen te bekijken.
2. **Een singleton-Azure Cosmos DB-client gebruiken voor de levensduur van uw toepassing**

    Elk DocumentClient-exemplaar is thread-veilige en efficiënte Verbindingsbeheer en adrescaching als het wordt uitgevoerd in de directe modus uitgevoerd. Om toe te staan efficiënt beheer en betere prestaties door DocumentClient, is het aanbevolen met één exemplaar van de DocumentClient per toepassingsdomein voor de levensduur van de toepassing.

   <a id="max-connection"></a>
3. **System.Net MaxConnections per host verhogen als u de Gateway gebruikt**

    Azure Cosmos DB-aanvragen via HTTPS/REST worden gemaakt wanneer u de Gateway gebruikt, en zijn onderworpen aan de standaardlimiet voor de verbinding per hostnaam of IP-adres. Mogelijk moet u de MaxConnections ingesteld op een hogere waarde (100-1000), zodat de clientbibliotheek van meerdere gelijktijdige verbindingen met Azure Cosmos DB gebruikmaken kan. In de .NET SDK 1.8.0 en hoger wordt de standaardwaarde voor [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit.aspx) 50 en de waarde wilt wijzigen, kunt u instellen de [Documents.Client.ConnectionPolicy.MaxConnectionLimit](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.connectionpolicy.maxconnectionlimit.aspx)op een hogere waarde.   
4. **Parallelle query's voor gepartitioneerde verzamelingen afstemmen**

     SQL .NET SDK versie 1.9.0 en hoger ondersteuning parallelle query's, zodat ze een gepartitioneerde verzameling worden parallel query (Zie [werken met de SDK's](sql-api-partition-data.md#working-with-the-azure-cosmos-db-sdks) en de verwante [codevoorbeelden](https://github.com/Azure/azure-documentdb-dotnet/blob/master/samples/code-samples/Queries/Program.cs) voor meer informatie). Parallelle query's zijn ontworpen voor betere latentie van query en de doorvoer via de seriële equivalent. Parallelle query's bieden twee parameters die gebruikers afstemmen kunnen om zodat de vereisten (a) MaxDegreeOfParallelism: om te bepalen het maximum aantal partities vervolgens kan worden opgevraagd in parallelle instructies, en (b) MaxBufferedItemCount: voor het beheren van het aantal vooraf opgehaalde resultaten.

    (a) ***afstemmen MaxDegreeOfParallelism\:***  parallelle query werkt door het opvragen van meerdere partities parallel. Gegevens van een afzonderlijke gepartitioneerde verzamelen is echter worden opgehaald met betrekking tot de query. Dus heeft de MaxDegreeOfParallelism instellen voor het aantal partities de maximale kans dat het bereiken van de meeste goed presterende query, mits alle andere system omstandigheden gelijk blijven. Als u het aantal partities niet weet, kunt u de MaxDegreeOfParallelism instellen op een groot aantal en het systeem de minimale (het aantal partities, door de gebruiker opgegeven invoer) als de MaxDegreeOfParallelism kiest.

    Het is belangrijk te weten dat de parallelle query's de beste prestaties opleveren als de gegevens gelijkmatig wordt verdeeld over alle partities met betrekking tot de query. Als de gepartitioneerde verzameling zodanig dat alle of een meerderheid van de gegevens die zijn geretourneerd door een query is geconcentreerd in een paar partities (één partitie in het ergste geval) en vervolgens op de prestaties van de query zou worden knelpunt door deze partities is gepartitioneerd.

    (b) ***afstemmen MaxBufferedItemCount\:***  parallelle query is ontworpen voor het vooraf ophalen van resultaten terwijl de huidige batch van de resultaten wordt verwerkt door de client. De vooraf ophalen helpt bij het algemene verbetering van de latentie van een query. MaxBufferedItemCount is de parameter om het aantal vooraf opgehaalde resultaten te beperken. MaxBufferedItemCount met het verwachte aantal geretourneerde resultaten (of een hoger getal) in te stellen, kunt de query voor het ontvangen van maximaal profiteren van het vooraf ophalen.

    Op dezelfde manier, ongeacht de MaxDegreeOfParallelism vooraf ophalen werkt en er is één buffer voor de gegevens van alle partities.  
5. **GC-server-side inschakelen**

    De frequentie van de garbagecollection verminderen kan in sommige gevallen helpen. Stel in .NET, [gcServer](https://msdn.microsoft.com/library/ms229357.aspx) op ' True '.
6. **Uitstel RetryAfter tussenpozen implementeren**

    Tijdens het Prestatietesten van, moet u load vergroten totdat een klein aantal aanvragen te maken met beperkingen. Als beperkt, moet de clienttoepassing uitstel op vertraging voor de server opgegeven interval. Respecteer de uitstel zorgt ervoor dat u besteden aan de minimale hoeveelheid tijd wachten tussen nieuwe pogingen. Ondersteuning voor beleid voor nieuwe pogingen is opgenomen in versie 1.8.0 en hoger van de SQL [.NET](sql-api-sdk-dotnet.md) en [Java](sql-api-sdk-java.md), versie 1.9.0 en hoger van de [Node.js](sql-api-sdk-node.md) en [Python](sql-api-sdk-python.md), en alle ondersteunde versies van de [.NET Core](sql-api-sdk-dotnet-core.md) SDK's. Zie voor meer informatie, [meer dan gereserveerd doorvoerlimieten](request-units.md#RequestRateTooLarge) en [RetryAfter](https://msdn.microsoft.com/library/microsoft.azure.documents.documentclientexception.retryafter.aspx).
    
    Met versie 1.19 en hoger van de .NET SDK is er een mechanisme voor het melden van aanvullende diagnostische gegevens en problemen met latentie oplossen, zoals wordt weergegeven in het volgende voorbeeld. U kunt de diagnostische tekenreeks voor aanvragen met een hogere leeslatentie vastleggen. De vastgelegde diagnostische tekenreeks krijgt u inzicht in het aantal keren dat 429s voor een bepaalde aanvraag zich heeft voorgedaan.
    ```csharp
    ResourceResponse<Document> readDocument = await this.readClient.ReadDocumentAsync(oldDocuments[i].SelfLink);
    readDocument.RequestDiagnosticsString 
    ```
    
7. **Uw client-workload uitschalen**

    Als u op het niveau van hoge doorvoer testen wilt (> 50.000 RU/s), de clienttoepassing kan knelpunt vanwege de machine beperking af op CPU- of -gebruik. Als u dit punt bereikt, kunt u blijven om de Azure Cosmos DB-account verder door uw client-toepassingen schalen over meerdere servers.
8. **Document-URI's voor lagere latentie voor lezen in de cache**

    Document-URI's indien mogelijk voor de beste prestaties lezen in de cache. U hebt voor het definiëren van logica voor de resourceid in de cache bij het maken van de resource. ResourceID op basis van zoekopdrachten zijn sneller dan de naam op basis van zoekopdrachten, zodat deze waarden opslaan in cache worden de prestaties verbeterd. 

   <a id="tune-page-size"></a>
1. **De paginagrootte voor query's / lezen-feeds voor betere prestaties afstemmen**

    Bij het uitvoeren van een bulksgewijs documenten met behulp van de feed-functionaliteit (bijvoorbeeld ReadDocumentFeedAsync) lezen of lezen bij de uitgifte van een SQL-query, worden de resultaten worden geretourneerd in een gesegmenteerde manier als de resultatenset is te groot is. Standaard resultaten worden geretourneerd in chunks van 100 items of 1 MB, het eerste ongeacht welke limiet wordt bereikt.

    Verminder het aantal retouren van netwerk vereist voor het ophalen van alle toepasselijke resultaten, verhoogt u de pagina databasegrootte met behulp [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) aanvraagheader op maximaal 1000. In gevallen waarin u wilt alleen enkele resultaten weer te geven, bijvoorbeeld als uw gebruiker-interface of de toepassing de API retourneert alleen 10 een tijd resulteert, u kunt ook de paginagrootte van de op 10 te verminderen van de doorvoer die wordt gebruikt voor leesbewerkingen en query's verlagen.

    U kunt ook het formaat van de pagina met behulp van de beschikbare SDK van Azure Cosmos DB's instellen.  Bijvoorbeeld:

        IQueryable<dynamic> authorResults = client.CreateDocumentQuery(documentCollection.SelfLink, "SELECT p.Author FROM Pages p WHERE p.Title = 'About Seattle'", new FeedOptions { MaxItemCount = 1000 });
10. **Verhoog het aantal threads/taken**

    Zie [verhoogt het aantal threads/taken](#increase-threads) in de sectie over netwerken.

11. **64-bits host verwerking gebruiken**

    De SQL-SDK werkt op een 32-bits proces wanneer u van SQL .NET SDK versie 1.11.4 gebruikmaakt en hoger. Als u cross-partitie query's gebruikt, wordt echter 64-bits host verwerking aanbevolen voor betere prestaties. De volgende typen toepassingen hebben het hostproces van 32-bits als de standaard, dus als u wilt wijzigen in 64-bits, volg deze stappen op basis van het type van uw toepassing:

    - Voor uitvoerbare toepassingen, kan dit worden gedaan door het uitschakelen van de **Prefer 32-bits** optie in de **Projecteigenschappen** venster op de **bouwen** tabblad.

    - Voor VSTest gebaseerd en Testprojecten, kunt u dit doen door het selecteren van **testen**->**instellingen testen**->**processorarchitectuur standaard als X64**, uit de **Visual Studio Test** menu-optie.

    - Voor lokaal geïmplementeerde ASP.NET-webtoepassingen, kunt dit doen door het controleren van de **de 64-bits versie van IIS Express gebruiken voor websites en projecten**onder **extra**->**opties**  -> **Projecten en oplossingen**->**Web-projecten**.

    - Voor ASP.NET-webtoepassingen op Azure is geïmplementeerd, kan dit worden gedaan door het kiezen van de **Platform als 64-bits** in de **toepassingsinstellingen** in Azure portal.

## <a name="indexing-policy"></a>Indexeringsbeleid
 
1. **Niet-gebruikte paden uitsluiten van indexering voor snellere schrijfbewerkingen**

    Indexeringsbeleid van cosmos DB kunt u opgeven welke paden document als u wilt opnemen of uitsluiten voor indexering door gebruik te maken van paden indexeren (IndexingPolicy.IncludedPaths en IndexingPolicy.ExcludedPaths). Het gebruik van het indexeren van paden kan bieden schrijven voor verbeterde prestaties en lagere indexopslag voor scenario's waarin de querypatronen vooraf, bekend als indexering kosten worden direct gecorreleerd aan het aantal unieke paden die zijn geïndexeerd.  Bijvoorbeeld, toont de volgende code hoe u kunt een hele sectie van de documenten (ook wel) uitsluiten een substructuur) indexering via de "*" jokerteken.

    ```csharp
    var collection = new DocumentCollection { Id = "excludedPathCollection" };
    collection.IndexingPolicy.IncludedPaths.Add(new IncludedPath { Path = "/*" });
    collection.IndexingPolicy.ExcludedPaths.Add(new ExcludedPath { Path = "/nonIndexedContent/*");
    collection = await client.CreateDocumentCollectionAsync(UriFactory.CreateDatabaseUri("db"), excluded);
    ```

    Zie voor meer informatie, [Azure Cosmos DB indexeringsbeleid](indexing-policies.md).

## <a name="throughput"></a>Doorvoer
<a id="measure-rus"></a>

1. **Meet en af te stemmen voor lagere aanvraag aanvraageenheden/seconde gebruik**

    Azure Cosmos DB biedt een uitgebreide set databasebewerkingen, waaronder relationele en hiërarchische query's met UDF's en opgeslagen procedures en triggers; dit alles kan worden uitgevoerd op de documenten in een databaseverzameling. De kosten die gepaard gaan met elk van deze bewerkingen varieert op basis van de CPU, IO en geheugen die nodig is om de bewerking te voltooien. In plaats van nadenken over en beheren van hardware, kunt u een aanvraageenheid (RU) zien als één maateenheid voor de resources die vereist voor het uitvoeren van verschillende databasebewerkingen en een toepassingsaanvraag indienen.

    Doorvoer is ingericht op basis van het aantal [aanvraageenheden](request-units.md) instellen voor elke container. Aanvraag eenheidsverbruik wordt geëvalueerd als een tarief per seconde. Toepassingen die groter zijn dan de snelheid van de eenheid ingerichte aanvragen voor de container zijn beperkt tot het percentage lager is dan het niveau van de ingerichte voor de container. Als uw toepassing een grotere doorvoer nodig is, kunt u uw doorvoer verhogen door in te richten extra aanvraageenheden. 

    De complexiteit van een query heeft gevolgen voor het aantal Aanvraageenheden voor een bewerking worden verbruikt. Het aantal predikaten, aard van de predikaten, aantal UDF's en de grootte van de set alle gegevens van bron van invloed zijn op de kosten van querybewerkingen.

    Voor het meten van de overhead van elke bewerking (maken, bijwerken of verwijderen), Inspecteer de [x-ms-request-kosten in rekening gebracht](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) header (of de equivalente RequestCharge-eigenschap in ResourceResponse<T> of FeedResponse<T> in de. NET SDK) voor het meten van het aantal aanvraageenheden gebruikt door deze bewerkingen.

    ```csharp
    // Measure the performance (request units) of writes
    ResourceResponse<Document> response = await client.CreateDocumentAsync(collectionSelfLink, myDocument);
    Console.WriteLine("Insert of document consumed {0} request units", response.RequestCharge);
    // Measure the performance (request units) of queries
    IDocumentQuery<dynamic> queryable = client.CreateDocumentQuery(collectionSelfLink, queryString).AsDocumentQuery();
    while (queryable.HasMoreResults)
         {
              FeedResponse<dynamic> queryResponse = await queryable.ExecuteNextAsync<dynamic>();
              Console.WriteLine("Query batch consumed {0} request units", queryResponse.RequestCharge);
         }
    ```             

    De kosten van de aanvraag heeft geretourneerd als deze header is een fractie van de ingerichte doorvoer (dat wil zeggen, 2000 ru's per seconde). Bijvoorbeeld, als de voorgaande query 1000 1KB-documenten retourneert, is de kosten van de bewerking 1000. Binnen één seconde houdt de server daarom slechts twee dergelijke aanvragen voordat de volgende aanvragen beperken. Zie voor meer informatie, [Aanvraageenheden](request-units.md) en de [aanvraag eenheid calculator](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Ingang snelheid beperken/snelheid van aanvragen te groot**

    Wanneer een client probeert te overschrijden de gereserveerde doorvoer voor een account, is er geen verslechtering van prestaties optreedt op de server en geen gebruik van doorvoercapaciteit buiten het niveau van de gereserveerde. De server te beëindigen van de aanvraag met RequestRateTooLarge (HTTP-statuscode 429) en retourneren de [x-ms-nieuwe poging-na-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) header die aangeeft van de hoeveelheid tijd in milliseconden, dat de gebruiker voordat het opnieuw proberen wachten moet de aanvraag.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    De SDK's impliciet alle catch dit antwoord, aansluiten bij de server opgegeven opnieuw proberen na de header en probeer de aanvraag. Als uw account wordt door meerdere clients tegelijkertijd wordt geopend, wordt de volgende poging slaagt.

    Hebt u meer dan één client cumulatief werken consistent boven de snelheid van aanvragen, het aantal nieuwe pogingen van standaard momenteel ingesteld op 9 intern door de client niet toereikend zijn; in dit geval, genereert de client een DocumentClientException met de statuscode 429 naar de toepassing. Het standaardaantal-nieuwe pogingen kan worden gewijzigd door in te stellen de RetryOptions op de ConnectionPolicy-instantie. Standaard wordt de DocumentClientException met de statuscode 429 geretourneerd na een cumulatieve wachttijd van 30 seconden als de aanvraag worden voortgezet boven de snelheid van aanvragen. Dit gebeurt zelfs als het huidige aantal nieuwe pogingen is kleiner dan het maximale aantal, worden deze de standaardwaarde van 9 of een door de gebruiker gedefinieerde waarde.

    Terwijl het gedrag voor automatische opnieuw proberen helpt om tolerantie en gebruiksgemak voor de meeste toepassingen te verbeteren, kan deze op odds zijn bij het uitvoeren van referentiepunten voor prestaties, met name bij het meten van latentie.  De latentie waargenomen-client wordt oploopt als het experiment komt binnen via deze server-restrictie en zorgt ervoor dat de client-SDK op de achtergrond opnieuw uit te voeren. Om te voorkomen latentiepieken tijdens prestaties experimenten, meet de geretourneerd door elke bewerking kosten in rekening gebracht en ervoor te zorgen dat aanvragen hieronder de aanvraagsnelheid gereserveerde nog werken. Zie voor meer informatie, [Aanvraageenheden](request-units.md).
3. **Ontwerp voor een kleinere documenten voor een hogere doorvoer**

    De aanvraag-kosten (dat wil zeggen aanvraagverwerking kosten) van een bepaalde bewerking is direct gecorreleerd met de grootte van het document. Bewerkingen voor grote documenten duurder dan bewerkingen voor kleine-documenten.

## <a name="next-steps"></a>Volgende stappen
Zie voor een voorbeeldtoepassing die wordt gebruikt om te evalueren van Azure Cosmos DB voor scenario's met hoge prestaties op een paar clientapparaten, [prestaties en schaal testen met Azure Cosmos DB](performance-testing.md).

Zie ook voor meer informatie over het ontwerpen van uw toepassing voor schaalbaarheid en hoge prestaties, [partitioneren en schalen in Azure Cosmos DB](partition-data.md).
