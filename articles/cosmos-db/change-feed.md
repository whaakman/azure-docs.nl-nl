---
title: Werken met de wijziging feed support in Azure Cosmos DB | Microsoft Docs
description: Gebruik Azure Cosmos DB change feed support bijhouden van wijzigingen in documenten en uitvoeren op basis van gebeurtenissen verwerken, zoals triggers en caches en analysefuncties systemen up-to-date te houden.
keywords: feed wijzigen
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: 3170ee1b48aa332a8730ba835396761ca5ef44c7
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287322"
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Werken met de change feed support in Azure Cosmos DB

[Azure Cosmos DB](../cosmos-db/introduction.md) is een snelle en flexibele wereldwijd gerepliceerde database, zeer geschikt voor IoT, games, detailhandel, en operationele logboekregistratie toepassingen. Een algemene ontwerppatroon in deze toepassingen is het gebruik van wijzigingen in de gegevens naar een vliegende start extra acties. Deze extra acties zijn bijvoorbeeld het volgende: 

* Een melding of een aanroep naar een API wordt geactiveerd wanneer een document wordt ingevoegd of gewijzigd.
* Stream voor IoT verwerken of het uitvoeren van analyses.
* Gegevens verder te verplaatsen door synchronisatie met een cache, zoekmachine of datawarehouse of archiveren van gegevens naar koude opslag.

De **ondersteuning wijzigingenfeed** in Azure Cosmos DB kunt u efficiënt en schaalbare oplossingen bouwen voor elk van deze patronen, zoals wordt weergegeven in de volgende afbeelding:

![Met behulp van Azure Cosmos DB change feed power realtime analyses en gebeurtenissen gebaseerde computing-scenario 's](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Ondersteuning wijzigingenfeed is opgegeven voor alle gegevensmodellen en containers in Azure Cosmos DB. Echter de wijzigingenfeed is lezen met behulp van de SQL-client en items serialiseert in JSON-indeling. Vanwege de opmaak, JSON MongoDB clients zult ervaren opgemaakt een discrepantie tussen BSON opgemaakt documenten en de JSON-wijzigingenfeed.

## <a name="how-does-change-feed-work"></a>Hoe wijzigingenfeed werk?

Ondersteuning wijzigingenfeed in Azure Cosmos DB werkt door te luisteren naar een Azure Cosmos DB-verzameling van eventuele wijzigingen. Het voert vervolgens de gesorteerde lijst met documenten die zijn gewijzigd in de volgorde waarin ze zijn gewijzigd. De wijzigingen worden doorgevoerd, kunnen worden verwerkt asynchroon en incrementeel en de uitvoer kan worden verdeeld over een of meer consumenten voor parallelle verwerking. 

U kunt de wijzigingenfeed op drie verschillende manieren kunt lezen, zoals verderop in dit artikel wordt beschreven:

*   [Met behulp van Azure Functions](#azure-functions)
*   [Met behulp van de Azure Cosmos DB-SDK](#sql-sdk)
*   [Met behulp van de Azure Cosmos DB-change feed processor-bibliotheek](#change-feed-processor)

De wijzigingenfeed is beschikbaar voor elke partitiesleutelbereik binnen de documentenverzameling, en dus kan worden verdeeld over een of meer consumenten voor parallelle verwerking zoals wordt weergegeven in de volgende afbeelding.

![Gedistribueerde verwerking van Azure Cosmos DB-wijzigingenfeed](./media/change-feed/changefeedvisual.png)

Aanvullende details:
* Wijzigingenfeed is standaard ingeschakeld voor alle accounts.
* U kunt uw [ingerichte doorvoer](request-units.md) in uw regio voor schrijven of een willekeurige [lezen regio](distribute-data-globally.md) om te lezen uit de feed wijzigen, net als elke andere Azure Cosmos DB-bewerking.
* De wijzigingenfeed bevat INSERT en update-bewerkingen die zijn aangebracht in documenten binnen de verzameling. U kunt hiermee vastleggen door een vlag 'voorlopig verwijderen' in uw documenten in plaats van verwijdert. U kunt ook een beperkte verloopperiode voor uw documenten via instellen de [TTL mogelijkheid](time-to-live.md), bijvoorbeeld 24 uur en gebruik de waarde van deze eigenschap om vast te leggen verwijdert. Met deze oplossing hebt u voor het verwerken van wijzigingen binnen een kortere periode dan de verloopperiode TTL-waarde.
* Elke wijziging in een document wordt exact één keer weergegeven in de feed wijzigen en clients beheren de logica voor het plaatsen van controlepunten. De change feed processor-bibliotheek biedt automatische plaatsen van controlepunten en 'ten minste eenmaal' semantiek.
* Alleen de meest recente wijziging voor een bepaald document is opgenomen in het logboekbestand. Tussentijdse wijzigingen zijn mogelijk niet beschikbaar.
* De wijzigingenfeed is gesorteerd door wijzigingen in elke waarde voor de partitiesleutel. Er is geen gegarandeerde volgorde tussen partitiesleutel waarden.
* Wijzigingen kunnen worden gesynchroniseerd vanuit een point-in-time, dat wil zeggen, er is geen vaste Gegevensretentieperiode waarvoor wijzigingen beschikbaar zijn.
* Wijzigingen zijn beschikbaar in chunks van partitie sleutelbereiken. Op deze manier kunt wijzigingen van grote verzamelingen moeten parallel worden verwerkt door meerdere consumenten/servers.
* Toepassingen kunnen meerdere wijzigingsfeeds tegelijkertijd op dezelfde verzameling aanvragen.
* ChangeFeedOptions.StartTime kan worden gebruikt voor een initieel beginpunt, bijvoorbeeld, het vervolgtoken dat overeenkomt met de opgegeven clock-tijd vinden. De ContinuationToken, wins indien opgegeven, boven de StartTime en StartFromBeginning waarden. De precisie van de ChangeFeedOptions.StartTime is ongeveer 5 seconden. 

## <a name="use-cases-and-scenarios"></a>Use cases en scenario 's

De wijzigingenfeed efficiënte verwerking van grote gegevenssets met een groot aantal schrijfbewerkingen en biedt een alternatief voor het uitvoeren van query's een volledige gegevensset om te bepalen wat er is gewijzigd. 

Bijvoorbeeld, met een wijzigingsfeed, u kunt de volgende taken uitvoeren efficiënt:

* Werk een cache, search-index of een datawarehouse met gegevens die zijn opgeslagen in Azure Cosmos DB.
* Implementeren op toepassingsniveau gegevens cloudlagen en archiveren van gegevens, dat wil zeggen, "gegevens" opslaan in Azure Cosmos DB en leeftijd van 'cold data' naar [Azure Blob Storage](../storage/common/storage-introduction.md) of [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Nul uitval migraties naar een ander Azure Cosmos DB-account met een andere partitieschema uitvoeren.
* Implementeer [lambda-pijplijnen op Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) met Azure Cosmos DB. Azure Cosmos DB biedt een schaalbare database-oplossing waarmee u kunt opnemen en query verwerken, en implementeren van lambda-architecturen met een lage totale Eigendomskosten. 
* Ontvangen en gebeurtenisgegevens van apparaten, sensoren, infrastructuur en toepassingen worden opgeslagen en verwerkt deze gebeurtenissen in realtime met [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/storm/apache-storm-overview.md), of [Apache Spark](../hdinsight/spark/apache-spark-overview.md). 

De volgende afbeelding ziet u hoe de feed ondersteuning voor het wijzigen van lambda-pijplijnen die beide opnemen en query's met Azure Cosmos DB kunt gebruiken: 

![Azure Cosmos DB op basis van lambda-pijplijn voor gegevensopname en query's uitvoeren](./media/change-feed/lambda.png)

Bovendien binnen uw [serverloze](http://azure.com/serverless) web en mobiele apps, kunt u gebeurtenissen bijhouden zoals wijzigingen in van uw klant-profiel, voorkeuren of locatie voor het activeren van bepaalde acties zoals pushmeldingen verzenden naar hun apparaten via [Azure Functions](#azure-functions). Als u gebruikmaakt van Azure Cosmos DB een game ontwikkelt, kunt u, bijvoorbeeld, gebruik wijzigingenfeed voor het implementeren van realtime scoreborden op basis van scores van voltooide games.

<a id="azure-functions"></a>
## <a name="using-azure-functions"></a>Met behulp van Azure Functions 

Als u Azure Functions, is de eenvoudigste manier om verbinding maken met een Azure Cosmos DB-wijzigingenfeed op een Azure Cosmos DB-trigger toevoegen aan uw Azure Functions-app. Wanneer u een Azure Cosmos DB-trigger in een Azure Functions-app maakt, u de Azure Cosmos DB-verzameling verbinden met selecteren en de functie wordt geactiveerd wanneer er een wijziging in de verzameling is gemaakt. 

Triggers kunnen worden gemaakt in de portal voor Azure Functions in de Azure Cosmos DB-portal of via een programma. Zie voor meer informatie, [Azure Cosmos DB: Serverless database computing met behulp van Azure Functions](serverless-computing-database.md).

<a id="sql-sdk"></a>
## <a name="using-the-sdk"></a>De SDK gebruiken

De [SQL-SDK](sql-api-sdk-dotnet.md) voor Azure Cosmos DB biedt u de kracht om te lezen en beheren van een feed wijzigen. Maar met grote te veel verantwoordelijkheden schuilt. Als u wilt beheren van controlepunten, volgnummers document behandelt en hebt nauwkeurige controle over partitiesleutels, klik met de SDK mogelijk de juiste aanpak.

In deze sectie u leert hoe u de SQL-SDK gebruiken om te werken met een feed wijzigen.

1. Beginnen met het lezen van de volgende bronnen van appconfig. Instructies over het ophalen van de sleutel-eindpunt en autorisatie zijn beschikbaar in [uw verbindingsreeks bijwerken](create-sql-api-dotnet.md#update-your-connection-string).

    ``` csharp
    DocumentClient client;
    string DatabaseName = ConfigurationManager.AppSettings["database"];
    string CollectionName = ConfigurationManager.AppSettings["collection"];
    string endpointUrl = ConfigurationManager.AppSettings["endpoint"];
    string authorizationKey = ConfigurationManager.AppSettings["authKey"];
    ```

2. Maak de client als volgt:

    ```csharp
    using (client = new DocumentClient(new Uri(endpointUrl), authorizationKey,
    new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    {
    }
    ```

3. De partitie sleutelbereiken ophalen:

    ```csharp
    FeedResponse pkRangesResponse = await client.ReadPartitionKeyRangeFeedAsync(
        collectionUri,
        new FeedOptions
            {RequestContinuation = pkRangesResponseContinuation });
     
    partitionKeyRanges.AddRange(pkRangesResponse);
    pkRangesResponseContinuation = pkRangesResponse.ResponseContinuation;
    ```

4. Bel ExecuteNextAsync voor elke partitiesleutelbereik:

    ```csharp
    foreach (PartitionKeyRange pkRange in partitionKeyRanges){
        string continuation = null;
        checkpoints.TryGetValue(pkRange.Id, out continuation);
        IDocumentQuery<Document> query = client.CreateDocumentChangeFeedQuery(
            collectionUri,
            new ChangeFeedOptions
            {
                PartitionKeyRangeId = pkRange.Id,
                StartFromBeginning = true,
                RequestContinuation = continuation,
                MaxItemCount = -1,
                // Set reading time: only show change feed results modified since StartTime
                StartTime = DateTime.Now - TimeSpan.FromSeconds(30)
            });
        while (query.HasMoreResults)
            {
                FeedResponse<dynamic> readChangesResponse = query.ExecuteNextAsync<dynamic>().Result;
    
                foreach (dynamic changedDocument in readChangesResponse)
                    {
                         Console.WriteLine("document: {0}", changedDocument);
                    }
                checkpoints[pkRange.Id] = readChangesResponse.ResponseContinuation;
            }
    }
    ```

> [!NOTE]
> In plaats van `ChangeFeedOptions.PartitionKeyRangeId`, kunt u `ChangeFeedOptions.PartitionKey` om op te geven van een enkele partitiesleutel waarvoor u wilt een wijziging feed ophalen. Bijvoorbeeld `PartitionKey = new PartitionKey("D8CFA2FD-486A-4F3E-8EA6-F3AA94E5BD44")`.
> 
>

Als u meerdere lezers hebt, kunt u **ChangeFeedOptions** lezen verdelen naar verschillende threads of andere clients.

En dat is alles, met deze enkele regels code kunt u starten voor het lezen van de wijzigingenfeed. Krijgt u de volledige code die wordt gebruikt in dit artikel uit de [GitHub-opslagplaats](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed).

In de code in stap 4 hierboven, de **ResponseContinuation** in de laatste regel bevat het laatste logische volgnummer (LSN) van het document, die u gebruikt de volgende keer dat u nieuwe documenten nadat deze volgnummer lezen. Met behulp van de **StartTime** van de **ChangeFeedOption** u uw net als u de documenten kunt uitbreiden. Dus als uw **ResponseContinuation** null is, maar uw **StartTime** gaat u terug in tijd krijgt u de documenten die zijn gewijzigd sinds de **StartTime**. Maar als uw **ResponseContinuation** heeft een waarde en vervolgens system u alle documenten sinds die LSN krijgt.

Uw matrix controlepunt wordt dus alleen te houden het LSN voor elke partitie. Maar als u niet te bekommeren om de partities wilt, controlepunten, LSN, begintijd, enz. de eenvoudigere optie is het gebruik van de change feed processor-bibliotheek.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Met behulp van de change feed processor-bibliotheek 

De [Azure Cosmos DB change feed processor-bibliotheek](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) kunt u eenvoudig gebeurtenisverwerking verdelen over meerdere consumenten worden gedistribueerd. Deze bibliotheek vereenvoudigt lezen wijzigingen in partities en meerdere threads die parallel werken.

Het belangrijkste voordeel van change feed processor-bibliotheek is dat u hoeft te beheren van elke partitie en vervolgtoken en u hoeft op te vragen voor elke verzameling handmatig.

De change feed processor-bibliotheek vereenvoudigt lezen wijzigingen in partities en meerdere threads die parallel werken.  Hiermee worden beheerd automatisch lezen wijzigingen over meerdere partities met behulp van een lease-mechanisme. Zoals u in de volgende afbeelding, ziet als u twee clients die van de change feed processor-bibliotheek gebruikmaken wordt gestart, verdelen zij het werk onderling. Als u doorgaat met het verhogen van de clients, houden ze het werk onderling verdelen.

![Gedistribueerde verwerking van Azure Cosmos DB-wijzigingenfeed](./media/change-feed/change-feed-output.png)

De linker-client eerst is gestart en het starten van de bewaking van alle partities, en vervolgens de tweede client die is gestart, en vervolgens de eerste van een aantal van de tweede client-leases laten gaan. Zoals u dit ziet is de leuke manier om het werk tussen verschillende apparaten en clients te distribueren.

Merk op dat als u twee serverloze Azure funtions bewaking van dezelfde verzameling en het gebruik van de dezelfde lease en vervolgens de twee functies kunnen verschillende documenten hebt, afhankelijk van hoe de processor-bibliotheek wil overbrengen de partities ophalen.

<a id="understand-cf"></a>
### <a name="understanding-the-change-feed-processor-library"></a>Informatie over de wijziging feed processor-bibliotheek

Er zijn vier belangrijke onderdelen van de implementatie van de change feed processor-bibliotheek: de bewaakte verzameling, de leaseverzameling, de processor host en de consumenten. 

> [!WARNING]
> Het maken van een verzameling heeft gevolgen voor de prijs, omdat u doorvoer reserveert voor de toepassing om te communiceren met Azure Cosmos DB. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie
> 
> 

**Bewaakte verzameling:** de bewaakte verzameling zijn de gegevens op basis waarvan de wijzigingenfeed is gegenereerd. Alle toevoegingen en wijzigingen in de bewaakte verzameling worden weerspiegeld in de wijzigingenfeed van de verzameling. 

**Leaseverzameling:** de lease verzameling coördinaten voor het verwerken van de feed over meerdere werknemers wijzigen. Een verzameling afzonderlijke wordt gebruikt voor het opslaan van de leases met één lease per partitie. Het is nuttig voor het opslaan van deze leaseverzameling op een ander account met de schrijfregio dichter in de buurt waar de change feed processor wordt uitgevoerd. Een lease-object bevat de volgende kenmerken: 
* Eigenaar: Hiermee geeft u de host die eigenaar is van de lease
* Voortzetting: Geeft de positie (vervolgtoken) in de feed voor een bepaalde partitie wijzigen
* Tijdstempel: Laatste keer dat de lease is bijgewerkt. de tijdstempel kan worden gebruikt om te controleren of de lease is verlopen 

**Processor-Host:** elke host wordt bepaald hoeveel partities verwerken op basis van hoeveel andere instanties van hosts actieve leases hebben. 
1.  Wanneer een host wordt gestart, krijgt deze leases om de werkbelasting in balans tussen alle hosts. Een host wordt leases, regelmatig vernieuwd, zodat de lease actief blijven. 
2.  Een host-controlepunten lezen voor de laatste vervolgtoken naar de lease voor elk. Een host controleert gelijktijdigheid om veiligheid te garanderen, de ETag voor elke update van de lease. Andere strategieën controlepunt worden ook ondersteund.  
3.  Bij het afsluiten, een host alle leases worden vrijgegeven, maar blijft de voortzetting van informatie, zodat het lezen van de opgeslagen controlepunt later kan worden hervat. 

Op dit moment mag het aantal hosts niet groter zijn dan het aantal partities (lease).

**Consumenten:** consumenten of werknemers, worden threads die de wijzigingenfeed verwerking gestart door elke host uitvoeren. Elke host processor kan meerdere consumenten hebben. Elke consument leest de wijziging feed van de partitie die is toegewezen aan en ontvangt van de host van wijzigingen en verlopen van leases.

Om verder te begrijpen hoe deze vier onderdelen van de wijzigingenfeed processor werk samen, bekijk een voorbeeld in het volgende diagram. De bewaakte verzameling documenten worden opgeslagen en gebruikt de 'plaats' als de partitiesleutel. We zien dat de blauwe partitie enzovoort documenten met het veld 'plaats' uit "A-E" bevat. Er zijn twee hosts, elk met twee consumenten van de vier partities parallel lezen. De pijlen geven de consumenten lezen van een specifieke positie in de feed wijzigen. In de eerste partitie vertegenwoordigt de donkerder blauw ongelezen wijzigingen terwijl de lichtblauw de al lezen wijzigingen in de feed wijzigen vertegenwoordigt. De hosts de leaseverzameling gebruiken voor het opslaan van een waarde 'voortzetting' om de positie van de huidige lezen voor elke consument bij te houden. 

![Met behulp van de Azure Cosmos DB-wijziging feed processor-host](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>Werken met de change feed processor-bibliotheek

Voordat de installatie van de wijziging feed processor NuGet-pakket, eerst installeren: 

* Microsoft.Azure.DocumentDB, de meest recente versie.
* Newtonsoft.Json, meest recente versie

Installeer vervolgens de [Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) en deze als een verwijzing opnemen.

Voor het implementeren van de change feed processor-bibliotheek die u moet doen te volgen:

1. Implementeer een **DocumentFeedObserver** object, dat wordt geïmplementeerd **IChangeFeedObserver**.
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;
    using Microsoft.Azure.Documents.Client;

    /// <summary>
    /// This class implements the IChangeFeedObserver interface and is used to observe 
    /// changes on change feed. ChangeFeedEventHost will create as many instances of 
    /// this class as needed. 
    /// </summary>
    public class DocumentFeedObserver : IChangeFeedObserver
    {
    private static int totalDocs = 0;

        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserver" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        /// <param name="client"> Client connected to destination collection </param>
        /// <param name="destCollInfo"> Destination collection information </param>
        public DocumentFeedObserver()
        {
            
        }

        /// <summary>
        /// Called when change feed observer is opened; 
        /// this function prints out observer partition key id. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task OpenAsync(IChangeFeedObserverContext context)
        {
            Console.ForegroundColor = ConsoleColor.Magenta;
            Console.WriteLine("Observer opened for partition Key Range: {0}", context.PartitionKeyRangeId);
            return Task.CompletedTask;
        }

        /// <summary>
        /// Called when change feed observer is closed; 
        /// this function prints out observer partition key id and reason for shut down. 
        /// </summary>
        /// <param name="context">The context specifying partition for this observer, etc.</param>
        /// <param name="reason">Specifies the reason the observer is closed.</param>
        /// <returns>A Task to allow asynchronous execution</returns>
        public Task CloseAsync(IChangeFeedObserverContext context, ChangeFeedObserverCloseReason reason)
        {
            Console.ForegroundColor = ConsoleColor.Cyan;
            Console.WriteLine("Observer closed, {0}", context.PartitionKeyRangeId);
            Console.WriteLine("Reason for shutdown, {0}", reason);
            return Task.CompletedTask;
        }

        public Task ProcessChangesAsync(IChangeFeedObserverContext context, IReadOnlyList<Document> docs, CancellationToken cancellationToken)
        {
            Console.ForegroundColor = ConsoleColor.Green;
            Console.WriteLine("Change feed: PartitionId {0} total {1} doc(s)", context.PartitionKeyRangeId, Interlocked.Add(ref totalDocs, docs.Count));
            foreach (Document doc in docs)
            {
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine(doc.Id.ToString());
            }

            return Task.CompletedTask;
        }
    }
    ```

2. Implementeer een **DocumentFeedObserverFactory**, welke implementeert een **IChangeFeedObserverFactory**.
    ```csharp
     using Microsoft.Azure.Documents.ChangeFeedProcessor.FeedProcessing;

    /// <summary>
    /// Factory class to create instance of document feed observer. 
    /// </summary>
    public class DocumentFeedObserverFactory : IChangeFeedObserverFactory
    {
        /// <summary>
        /// Initializes a new instance of the <see cref="DocumentFeedObserverFactory" /> class.
        /// Saves input DocumentClient and DocumentCollectionInfo parameters to class fields
        /// </summary>
        public DocumentFeedObserverFactory()
        {
        }

        /// <summary>
        /// Creates document observer instance with client and destination collection information
        /// </summary>
        /// <returns>DocumentFeedObserver with client and destination collection information</returns>
        public IChangeFeedObserver CreateObserver()
        {
            DocumentFeedObserver newObserver = new DocumentFeedObserver();
            return newObserver as IChangeFeedObserver;
        }
    }
    ```

3. Definieer *CancellationTokenSource* en *ChangeFeedProcessorBuilder*

    ```csharp
    private readonly CancellationTokenSource cancellationTokenSource = new CancellationTokenSource();
    private readonly ChangeFeedProcessorBuilder builder = new ChangeFeedProcessorBuilder();
    ```

5. Bouw de **ChangeFeedProcessorBuilder** na het definiëren van de relevante objecten 

    ```csharp
            string hostName = Guid.NewGuid().ToString();
      
            // monitored collection info 
            DocumentCollectionInfo documentCollectionInfo = new DocumentCollectionInfo
            {
                Uri = new Uri(this.monitoredUri),
                MasterKey = this.monitoredSecretKey,
                DatabaseName = this.monitoredDbName,
                CollectionName = this.monitoredCollectionName
            };
            
            DocumentCollectionInfo leaseCollectionInfo = new DocumentCollectionInfo
                {
                    Uri = new Uri(this.leaseUri),
                    MasterKey = this.leaseSecretKey,
                    DatabaseName = this.leaseDbName,
                    CollectionName = this.leaseCollectionName
                };
            DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory();
            ChangeFeedOptions feedOptions = new ChangeFeedOptions();

            /* ie customize StartFromBeginning so change feed reads from beginning
                can customize MaxItemCount, PartitonKeyRangeId, RequestContinuation, SessionToken and StartFromBeginning
            */

            feedOptions.StartFromBeginning = true;
        
            ChangeFeedProcessorOptions feedProcessorOptions = new ChangeFeedProcessorOptions();

            // ie. customizing lease renewal interval to 15 seconds
            // can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay 
            feedProcessorOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);

            this.builder
                .WithHostName(hostName)
                .WithFeedCollection(documentCollectionInfo)
                .WithLeaseCollection(leaseCollectionInfo)
                .WithProcessorOptions (feedProcessorOptions)
                .WithObserverFactory(new DocumentFeedObserverFactory());               
                //.WithObserver<DocumentFeedObserver>();  If no factory then just pass an observer

            var result =  await this.builder.BuildAsync();
            await result.StartAsync();
            Console.Read();
            await result.StopAsync();    
    ```

Dat is alles. Na deze stappen documenten wordt gestart, weergegeven in de **DocumentFeedObserver.ProcessChangesAsync** methode.

Is voor gebruik van de afbeelding om weer te geven van ander soort objecten en hun tussenkomst van de bovenstaande code. U moet de juiste variabelen definiëren en start ze bij de juiste waarden. Krijgt u de volledige code die wordt gebruikt in dit artikel uit de [GitHub-opslagplaats](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessorV2).

> [!NOTE]
> U hebt een hoofdsleutel nooit in uw code of in het configuratiebestand, zoals wordt weergegeven in bovenstaande code. Raadpleeg [over het gebruik van Key Vault populatieregel voor de sleutels](https://sarosh.wordpress.com/2017/11/23/cosmos-db-and-key-vault/).


## <a name="faq"></a>Veelgestelde vragen

### <a name="what-are-the-different-ways-you-can-read-change-feed-and-when-to-use-each-method"></a>Wat zijn de verschillende manieren waarop u kunt lezen Change Feed? en wanneer u elke methode?

Er zijn drie opties voor u wijzigingenfeeds lezen:

* **[Met behulp van Azure Cosmos DB SQL API .NET SDK](#sql-sdk)**
   
   Met deze methode, krijgt u lage mate van controle op wijzigingenfeed. U kunt het controlepunt beheren, u hebt toegang tot een bepaalde partitie sleutels enzovoort. Als u meerdere lezers hebt, kunt u [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) lezen verdelen naar verschillende threads of andere clients. .

* **[Met behulp van de Azure Cosmos DB-change feed processor-bibliotheek](#change-feed-processor)**

   Als u veel van de complexiteit van de wijzigingenfeed uitbesteden kunt u change feed processor-bibliotheek gebruiken. Deze bibliotheek veel complexiteit verborgen, maar nog steeds hebt die u volledige controle over feed wijzigen. Deze bibliotheek volgt een [waarnemer patroon](https://en.wikipedia.org/wiki/Observer_pattern), de verwerking van de functie wordt aangeroepen door de SDK. 

   Als u een hoge doorvoer wijzigingenfeeds hebt, kunt u meerdere clients om te lezen van de wijzigingenfeed instantiëren. Omdat u 'feed processor-bibliotheek wijzigen' gebruikt, wordt deze automatisch de belasting tussen verschillende clients delen. U hoeft niet verder niets te doen. Alle complexiteit wordt verwerkt door de SDK. Echter, als u wilt dat uw eigen load balancer, klikt u vervolgens kunt u implementeren IParitionLoadBalancingStrategy voor aangepaste partitie strategie. Implementeer IPartitionProcessor – voor aangepaste het verwerken van wijzigingen op een partitie. Echter kunt u een partitiebereik verwerken met SDK, maar als u wilt verwerken van een bepaalde partitie-sleutel hebt u het gebruik van de SDK voor SQL-API.

* **[Met behulp van Azure Functions](#azure-functions)** 
   
   De laatste optie Azure-functie is de eenvoudigste optie. U wordt aangeraden deze optie gebruikt. Wanneer u een Azure Cosmos DB-trigger in een Azure Functions-app maakt, u de Azure Cosmos DB-verzameling verbinden met selecteren en de functie wordt geactiveerd wanneer er een wijziging in de verzameling is gemaakt. Bekijk een [scherm cast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) functioneren van het gebruik van Azure en wijzigingenfeed

   Triggers kunnen worden gemaakt in de portal voor Azure Functions in de Azure Cosmos DB-portal of via een programma. Visual Studio en Visual Studio-Code heeft uitstekende ondersteuning voor het schrijven van Azure-functie. U kunt schrijven en fouten opsporen in de code op uw bureaublad en klikt u vervolgens de functie met één klik implementeren. Zie voor meer informatie, [Azure Cosmos DB: Serverless database computing met behulp van Azure Functions](serverless-computing-database.md) artikel.

### <a name="what-is-the-sort-order-of-documents-in-change-feed"></a>Wat is de sorteervolgorde van documenten in de feed wijzigen?

Feed documenten wijzigen wordt geleverd in de volgorde van hun tijd van wijziging. Deze sorteervolgorde wordt gegarandeerd alleen per partitie.

### <a name="for-a-multi-region-account-what-happens-to-the-change-feed-when-the-write-region-fails-over-does-the-change-feed-also-failover-would-the-change-feed-still-appear-contiguous-or-would-the-fail-over-cause-change-feed-to-reset"></a>Voor een account meerdere regio's, wat gebeurt er wanneer de schrijfregio mislukt-over-feed wijzigen? De wijzigingenfeed ook failover? Wordt nog steeds de wijzigingenfeed weergegeven aaneengesloten of kan de oorzaak van de failover-wijzigingenfeed opnieuw in te stellen?

Ja, wijzigingenfeed werkt voor de handmatige failover-bewerking en is aaneengesloten.

### <a name="how-long-change-feed-persist-the-changed-data-if-i-set-the-ttl-time-to-live-property-for-the-document-to--1"></a>Hoe lang wijzigingenfeed de gewijzigde gegevens behouden als ik de TTL (Time to Live)-eigenschap voor het document op-1 instelt?

Wijzigingenfeed blijven altijd behouden. Als gegevens worden niet verwijderd, blijft deze in de wijzigingenfeed.

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region-as-change-feed-is-available-in-all-the-read-regions-by-default"></a>Hoe kan ik Azure functions om te lezen uit een bepaalde regio, configureren, zoals de wijzigingenfeed is beschikbaar in de leesregio's standaard?

Het is momenteel niet mogelijk om te configureren van Azure Functions voor het lezen van een bepaalde regio. Er is een GitHub-probleem in de Azure Functions-opslagplaats naar de gewenste regio's van Azure Cosmos DB-binding en trigger instellen.

Azure Functions maakt gebruik van het standaardbeleid voor de verbinding. U kunt de verbindingsmodus configureren in Azure Functions en standaard, wordt gelezen vanuit de schrijfregio, dus is het beste worden geplaatst op Azure Functions in dezelfde regio bevinden.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>Wat is de standaardgrootte van batches in Azure Functions?

100 documenten bij elke aanroep van Azure Functions. Dit nummer is echter worden geconfigureerd in de function.json-bestand. Hier is voltooid [lijst met configuratieopties](../azure-functions/functions-run-local.md). Als u lokaal ontwikkelt, werkt u de toepassingsinstellingen in de [local.settings.json](../azure-functions/functions-run-local.md) bestand.

### <a name="i-am-monitoring-a-collection-and-reading-its-change-feed-however-i-see-i-am-not-getting-all-the-inserted-document-some-documents-are-missing-what-is-going-on-here"></a>Ik ben een verzameling bewaking en lezen van de wijziging feed, maar ik zie ik ben niet ophalen van het ingevoegd document, bepaalde documenten ontbreken. Wat gebeurt er hier?

Zorg ervoor dat er geen andere functie dezelfde verzameling met de dezelfde leaseverzameling lezen. Er is er gebeurd met mij en later gerealiseerde ik dat de ontbrekende documenten worden verwerkt door de andere Azure functions, die ook gebruik van de dezelfde lease.

Dus als u meerdere Azure Functions om te lezen maakt wijzigingenfeed dezelfde moeten ze verschillende leaseverzameling gebruiken of de configuratie "leasePrefix" gebruiken voor het delen van dezelfde verzameling. Wanneer u change feed processor-bibliotheek kunt u meerdere exemplaren van uw functie starten en SDK de documenten tussen verschillende exemplaren automatisch voor u wordt verdeeld.

### <a name="my-document-is-updated-every-second-and-i-am-not-getting-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Mijn document elke seconde wordt bijgewerkt en ik krijg niet alle wijzigingen in Azure Functions luisteren als u wilt wijzigen van de feed.

Azure Functions polls wijzigingenfeed voor elke 5 seconden, zodat tussen 5 seconden wijzigingen gaan verloren. Azure Cosmos DB slaat slechts één versie voor om de vijf seconden, zodat u de 5e wijziging in het document krijgt. Echter als u wilt om te gaan dan 5 seconden en wilt pollen Feed elke seconde wijzigen, kunt u de pollingtijd 'feedPollTime' configureren, Zie [Azure Cosmos DB-bindingen](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). Dit is gedefinieerd in milliseconden met een standaardwaarde van 5000. Is mogelijk, maar niet aangeraden, als u wilt beginnen met branden meer CPU nodig is dan 1 seconde.

### <a name="i-inserted-a-document-in-the-mongo-api-collection-but-when-i-get-the-document-in-change-feed-it-shows-a-different-id-value-what-is-wrong-here"></a>Kan ik een document ingevoegd in de verzameling Mongo-API, maar wanneer ik het document in wijzigingenfeed, wordt een andere id-waarde. Wat is hier verkeerd?

Uw verzameling is Mongo-API-verzameling. Denk eraan dat wijzigingenfeed is lezen met behulp van de SQL-client en items serialiseert in JSON-indeling. Vanwege de opmaak, JSON MongoDB clients zult ervaren opgemaakt een discrepantie tussen BSON opgemaakt documenten en de JSON-wijzigingenfeed. Er is een weergave van een document BSON in JSON. Als u binaire kenmerken in een Mongo-accounts gebruiken, moeten ze worden geconverteerd naar JSON.

### <a name="is-there-a-way-to-control-change-feed-for-updates-only-and-not-inserts"></a>Is er een manier voor het beheren van wijzigingenfeeds voor alleen-updates en niet invoegen?

Niet vandaag, maar deze functionaliteit is op de roadmap. U kunt vandaag de dag een voorlopig markering toevoegen aan het document voor updates.

### <a name="is-there-a-way-to-get-deletes-in-change-feed"></a>Is er een manier om verwijderingen in wijzigingenfeed?

Op dit moment Meld niet worden verwijderd u wijzigingenfeed. Wijzigingenfeed is voortdurend verbeteren en deze functionaliteit is op de roadmap. U kunt vandaag de dag een voorlopig markering toevoegen op het document verwijderen. Een kenmerk toevoegen aan het document met de naam 'verwijderd' en stel deze in op 'true' en een TTL-waarde voor het document zo instellen dat deze automatisch kan worden verwijderd.

### <a name="can-i-read-change-feed-for-historic-documentsfor-example-documents-that-were-added-5-years-back-"></a>Kan ik wijzigingenfeeds voor historische documenten (bijvoorbeeld documenten die zijn toegevoegd 5 jaar terug) lezen?

Ja, als het document is niet verwijderd. u kunt de wijziging lezen feed zo de oorsprong van uw verzameling.

### <a name="can-i-read-change-feed-using-javascript"></a>Kan ik met JavaScript wijzigingenfeeds lezen?

Ja, eerste Node.js-SDK-ondersteuning voor wijzigingenfeed onlangs is toegevoegd. Deze kan worden gebruikt zoals weergegeven in het volgende voorbeeld update documentdb-module voor de huidige versie voordat u de code uitvoeren:

```js

var DocumentDBClient = require('documentdb').DocumentClient;
const host = "https://your_host:443/";
const masterKey = "your_master_key==";
const databaseId = "db";
const collectionId = "c1";
const dbLink = 'dbs/' + databaseId;
const collLink = dbLink + '/colls/' + collectionId;
var client = new DocumentDBClient(host, { masterKey: masterKey });
let options = {
    a_im: "Incremental feed",
    accessCondition: {
        type: "IfNoneMatch",        // Use: - empty condition (or remove accessCondition entirely) to start from beginning.
        //      - '*' to start from current.
        //      - specific etag value to start from specific continuation.
        condition: ""
    }
};
 
var query = client.readDocuments(collLink, options);
query.executeNext((err, results, headers) =&gt; {
    // Now we have headers.etag, which can be used in next readDocuments in accessCondition option.
    console.log(results);
    console.log(headers.etag);
    console.log(results.length);
    options.accessCondition = { type: "IfNoneMatch", condition: headers.etag };
    var query = client.readDocuments(collLink, options);
    query.executeNext((err, results, headers) =&gt; {
        console.log("next one:", results[0]);
    });
});<span id="mce_SELREST_start" style="overflow:hidden;line-height:0;"></span>

```

### <a name="can-i-read-change-feed-using-java"></a>Kan ik met behulp van Java wijzigingenfeeds lezen?

Java-bibliotheek wijzigingenfeeds lezen is beschikbaar in [Github-opslagplaats](https://github.com/Azure/azure-documentdb-changefeedprocessor-java). Op dit moment is Java-bibliotheek echter enkele versies achter .NET-bibliotheek. Binnenkort wordt zowel de bibliotheken worden gesynchroniseerd.

### <a name="can-i-use-etag-lsn-or-ts-for-internal-bookkeeping-which-i-get-in-response"></a>Kan ik _etag, _lsn of _ts gebruiken voor interne boekhouding, die in het antwoord verschijnt?

_etag indeling interne is en u moet niet afhankelijk is van het (doen niet parseren) omdat deze op elk gewenst moment kunt wijzigen.
_ts is gewijzigd of maken van het tijdstempel. U kunt _ts gebruiken voor chronologische vergelijking.
_lsn is een batch-id die alleen voor de wijzigingenfeed is toegevoegd, staat voor de transactie-id uit de store... Veel documenten mogelijk dezelfde _lsn.
Nog één ding te weten, ETag op FeedResponse is anders dan de _etag u op het document ziet. _etag is een interne id en gebruikt om gelijktijdigheid, krijgt over de versie van het document en ETag is gebruikt voor het sequentiëren van de feed.

### <a name="does-reading-change-feed-add-any-additional-cost-"></a>Wijzigingenfeeds lezen extra kosten toevoegen?

U betaalt voor de RU's dat wil zeggen gebruikt altijd verbruiken RU verplaatsing van gegevens in en uit Azure Cosmos DB-verzamelingen. Gebruikers wordt in rekening gebracht voor RU verbruikt door de leaseverzameling.

### <a name="can-multiple-azure-functions-read-one-collections-change-feed"></a>Kan meerdere Azure-functies van een verzameling wijzigingenfeeds lezen?

Ja. Meerdere Azure-functies kunnen dezelfde verzameling wijzigingenfeeds lezen. De Azure-functies moet echter een afzonderlijke leaseCollectionPrefix gedefinieerd hebben.

### <a name="should-the-lease-collection-be-partitioned"></a>De leaseverzameling worden gepartitioneerd?

Nee, leaseverzameling kan worden opgelost. Gepartitioneerde leaseverzameling is niet nodig en wordt momenteel niet ondersteund.

### <a name="can-i-read-change-feed-from-spark"></a>Kan ik lezen change feed van Spark?

Ja, dat is mogelijk. Raadpleeg [Spark-Connector voor Azure Cosmos DB](spark-connector.md). Hier volgt een [scherm cast](https://www.youtube.com/watch?v=P9Qz4pwKm_0&t=1519s) wordt weergegeven hoe u als een gestructureerde stream-wijzigingenfeed kunt verwerken.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-say-a-batch-of-10-documents-and-i-get-an-error-at-7th-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-documentie-7th-document-in-my-next-feed"></a>Als ik wijzigen met behulp van Azure Functions-feed verwerken ben, Stel dat een batch van 10 documenten en er verschijnt een foutbericht op 7 Document. In dat geval worden de laatste drie documenten niet verwerkt hoe kan ik start verwerking uit het document (zoals) 7-document) in de volgende feed?

Voor het afhandelen van de fout, wordt het aanbevolen patroon is het inpakken van uw code met probeer catch-blok. Bekijk de fout en plaats van dat document in een wachtrij (dead-letter uitvoeren) en definieer vervolgens logica voor het werken met de documenten die de fout heeft geproduceerd. Met deze methode als u een batch 200-document en slechts één document is mislukt, hebt hoeft u niet te weggooien van de hele batch.

In het geval van fout moet u het controlepunt terug naar begin niet terugspoelen zal anders u kunt houden ophalen die documenten uit wijzigingenfeed. Denk eraan dat change feed houdt de laatste schermopname van de documenten, omdat u de laatste module kunnen verloren gaan de vorige momentopname van het document. wijzigingenfeed blijft slechts één van de laatste versie van het document en in andere processen kunnen afkomstig zijn en wijzigen van het document.

Als u uw code herstellen houden, vindt u binnenkort geen documenten op dead-letter-wachtrij.
Azure Functions wordt automatisch aangeroepen door change feed system en controlepunt enzovoort intern wordt beheerd door Azure-functie. Als u wilt het controlepunt terugdraaien en elk aspect van het beheren, moet u rekening houden met behulp van de wijziging feed Processor-SDK.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure Cosmos DB met Azure Functions [Azure Cosmos DB: Serverless database computing met behulp van Azure Functions](serverless-computing-database.md).

Gebruik de volgende bronnen voor meer informatie over het gebruik van de change feed processor-bibliotheek:

* [Pagina met informatie](sql-api-sdk-dotnet-changefeed.md) 
* [Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Voorbeeldcode van stappen 1-6 hierboven](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [Aanvullende voorbeelden op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

Gebruik de volgende bronnen voor meer informatie over het gebruik van de wijzigingenfeed via de SDK:

* [Informatiepagina SDK](sql-api-sdk-dotnet.md)
