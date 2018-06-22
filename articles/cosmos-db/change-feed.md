---
title: Werken met de wijziging feed ondersteuning in Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB wijzigen feed ondersteuning gebruiken voor het bijhouden van wijzigingen in documenten en het uitvoeren van verwerking op basis van gebeurtenissen zoals triggers en caches en analyses systemen up-to-date te houden.
keywords: Feed wijzigen
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: rafats
ms.openlocfilehash: 8475c79782730e989f9590566c31ccd50af9f144
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36302043"
---
# <a name="working-with-the-change-feed-support-in-azure-cosmos-db"></a>Werken met de ondersteuning in Azure Cosmos DB feed wijziging

[Azure Cosmos DB](../cosmos-db/introduction.md) is een snelle en flexibele globaal gerepliceerd database, geschikt voor IoT, games, retail, en operationele logboekregistratie toepassingen. Er wordt een algemene ontwerppatroon in deze toepassingen met wijzigingen in de gegevens ere van aanvullende acties. Deze extra acties mogelijk het volgende: 

* Activering van een melding of een aanroep naar een API wanneer een document wordt ingevoegd of gewijzigd.
* De stroom verwerken voor IoT of het uitvoeren van analyses.
* Verplaatsing van de aanvullende gegevens door synchronisatie met een cache, de zoekmachine of het datawarehouse of archiveren van gegevens op koude opslag.

De **wijziging feed ondersteuning** in Azure Cosmos DB kunt u efficiënter en schaalbare oplossingen bouwen voor elk van deze patronen, zoals wordt weergegeven in de volgende afbeelding:

![Power realtime analyses en gebeurtenisafhankelijke scenario's met computers met behulp van Azure DB die Cosmos wijziging feed](./media/change-feed/changefeedoverview.png)

> [!NOTE]
> Wijziging feed ondersteuning is beschikbaar voor alle gegevensmodellen en containers in Azure Cosmos DB. Echter de feed wijzigen met behulp van de SQL-client wordt gelezen en serialiseert items in de JSON-indeling. Vanwege de JSON opmaak, clients krijgen MongoDB geformatteerd een discrepantie tussen BSON geformatteerd documenten en de JSON feed wijzigen.

In de volgende video Azure Cosmos DB Program Manager Andrew Liu laat zien hoe de wijziging Azure Cosmos DB feed werkt.

> [!VIDEO https://www.youtube.com/embed/mFnxoxeXlaU]
>
>

## <a name="how-does-change-feed-work"></a>Hoe wijziging feed werk?

Feed-ondersteuning in Azure Cosmos DB werkt wijzigen door te luisteren naar een verzameling Azure Cosmos DB op basis van wijzigingen. Het levert vervolgens de gesorteerde lijst van documenten die zijn gewijzigd in de volgorde waarin ze zijn gewijzigd. De wijzigingen worden doorgevoerd, kunnen worden verwerkt asynchroon en incrementeel en de uitvoer kan worden verdeeld over een of meer consumenten voor parallelle verwerking. 

Zoals verderop in dit artikel wordt beschreven, kunt u de wijziging in drie verschillende manieren feed lezen:

*   [Met behulp van Azure Functions](#azure-functions)
*   [Met behulp van de Azure SDK voor Documentdb-Cosmos](#sql-sdk)
*   [Met behulp van de wijziging Azure Cosmos DB feed processor-bibliotheek](#change-feed-processor)

De feed wijziging is beschikbaar voor elke partitiesleutelbereik binnen de documentenverzameling en dus kan worden verdeeld over een of meer consumenten voor parallelle verwerking zoals weergegeven in de volgende afbeelding.

![Feed voor gedistribueerde verwerking van Azure Cosmos DB wijzigen](./media/change-feed/changefeedvisual.png)

Aanvullende details:
* Wijziging feed is standaard ingeschakeld voor alle accounts.
* U kunt uw [ingerichte doorvoer](request-units.md) in uw regio schrijven of een willekeurige [lezen regio](distribute-data-globally.md) om te lezen van de feed wijziging, net als elke andere Azure DB die Cosmos-bewerking.
* De feed wijziging omvat INSERT en update-bewerkingen die zijn aangebracht in de documenten binnen de verzameling. U kunt vastleggen verwijderingen door een 'soft-delete' vlag binnen uw documenten in plaats van verwijderingen. U kunt ook een eindige vervalperiode voor uw documenten via instellen de [TTL mogelijkheid](time-to-live.md), bijvoorbeeld 24 uur en gebruik de waarde van die eigenschap voor het vastleggen van verwijderingen. Met deze oplossing hebt u wijzigingen verwerken binnen een kortere periode dan de TTL verloopperiode.
* Elke wijziging in een document wordt slechts één keer weergegeven in de feed wijziging en clients beheren hun logica plaatsen van controlepunten. De wijziging feed processor-bibliotheek biedt plaatsen van controlepunten en 'ten minste eenmaal' semantiek.
* Alleen de meest recente wijziging voor een bepaald document is opgenomen in het logboekbestand. Tussentijdse wijzigingen zijn mogelijk niet beschikbaar.
* De feed wijzigen is door wijzigingen in elke waarde voor de partitiesleutel gesorteerd. Er is geen gegarandeerde volgorde voor partitie-sleutelwaarden.
* Wijzigingen kunnen worden gesynchroniseerd vanuit elk punt in tijd, er is geen vaste Gegevensretentieperiode waarvoor wijzigingen beschikbaar zijn.
* Wijzigingen zijn beschikbaar in segmenten van de partitie sleutelbereiken. Op deze manier kunt wijzigingen in grote verzamelingen parallel worden verwerkt door meerdere gebruikers /-servers.
* Toepassingen kunnen meerdere wijziging feeds gelijktijdig op dezelfde verzameling aanvragen.
* ChangeFeedOptions.StartTime kan worden gebruikt om een initiële beginpunt, bijvoorbeeld, het vervolgtoken overeenkomt met de opgegeven kloktijd vinden. De ContinuationToken wins indien opgegeven, via de StartTime en StartFromBeginning waarden. De precisie van ChangeFeedOptions.StartTime is ~ 5 seconden. 

## <a name="use-cases-and-scenarios"></a>Gebruiksvoorbeelden en scenario 's

De feed wijziging kan efficiënte verwerking van grote gegevenssets met een groot aantal schrijfbewerkingen en biedt een alternatief voor het opvragen van een volledige gegevensset om te zien wat er is gewijzigd. 

Bijvoorbeeld, met een wijziging van de feed kunt u de volgende taken uitvoeren efficiënt:

* Een cache, search-index of een datawarehouse bijwerken met gegevens die zijn opgeslagen in Azure Cosmos DB.
* Implementeren op toepassingsniveau gegevens lagen en archivering, dat wil zeggen, gegevensopslag ' hot ' in Azure Cosmos DB en na verloop van tijd 'koude gegevens' naar [Azure Blob Storage](../storage/common/storage-introduction.md) of [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Nul uitvaltijd migraties naar een andere Azure DB die Cosmos-account met een andere partitieschema niet uitvoeren.
* Implementeer [lambda pijplijnen op Azure](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/) met Azure Cosmos DB. Azure Cosmos DB biedt een schaalbare database-oplossing die kan opname en query verwerken en implementeren van lambda-architecturen met lage totale Eigendomskosten. 
* Ontvangen en verwerken van deze gebeurtenissen in realtime met opslaan van gebeurtenisgegevens van apparaten, sensoren, infrastructuur en toepassingen [Azure Stream Analytics](../stream-analytics/stream-analytics-documentdb-output.md), [Apache Storm](../hdinsight/storm/apache-storm-overview.md), of [Apache Spark](../hdinsight/spark/apache-spark-overview.md). 

De volgende afbeelding ziet u hoe de feed ondersteuning voor het wijzigen van lambda-pijplijnen die zowel voor opnemen en query met behulp van Azure DB die Cosmos kunt gebruiken: 

![Azure DB Cosmos gebaseerde lambda-pijplijn voor opname en query](./media/change-feed/lambda.png)

Bovendien binnen uw [zonder server](http://azure.com/serverless) web- en mobiele apps, kunt u gebeurtenissen bijhouden zoals wijzigingen in uw klant profiel, voorkeuren of locatie voor het activeren van bepaalde acties zoals pushmeldingen verzenden naar hun apparaten via [Azure Functions](#azure-functions). Als u gebruikmaakt van Azure DB die Cosmos om een spel samen te stellen, kunt u, bijvoorbeeld gebruik feed voor het implementeren van realtime scoreborden op basis van scores van voltooide games wijzigen.

<a id="azure-functions"></a>
## <a name="using-azure-functions"></a>Met behulp van Azure Functions 

Als u van Azure Functions gebruikmaakt, is de eenvoudigste manier om verbinding maken met een Azure Cosmos DB wijzigen feed een Azure DB die Cosmos-trigger toevoegen aan uw app in Azure Functions. Wanneer u een Azure DB die Cosmos-trigger in een Azure Functions-app maakt, u de Azure DB die Cosmos-verzameling verbinding maken met selecteren en de functie wordt geactiveerd wanneer een wijziging aan de verzameling wordt aangebracht. 

Triggers kunnen worden gemaakt in de Azure Functions-portal in de Azure DB die Cosmos-portal of programmatisch. Zie voor meer informatie [Azure Cosmos DB: zonder Server database computing met behulp van Azure Functions](serverless-computing-database.md).

<a id="sql-sdk"></a>
## <a name="using-the-sdk"></a>De SDK gebruiken

De [SQL SDK](sql-api-sdk-dotnet.md) voor Azure Cosmos DB biedt u de bevoegdheid om te lezen en beheren van een wijziging in de feed. Maar met geweldige power te veel verantwoordelijkheden geleverd. Als u wilt beheren van controlepunten, behandelt document volgnummers en hebben gedetailleerde controle over partitiesleutels, klik met de SDK mogelijk de juiste aanpak.

Deze sectie wordt uitgelegd hoe u de SQL-SDK gebruiken om te werken met een wijziging in de feed.

1. Lezen van de volgende bronnen van appconfig starten. Instructies voor het ophalen van de endpoint- en autorisatie-sleutel zijn beschikbaar in [bijwerken van de verbindingsreeks](create-sql-api-dotnet.md#update-your-connection-string).

    ``` csharp
    DocumentClient client;
    string DatabaseName = ConfigurationManager.AppSettings["database"];
    string CollectionName = ConfigurationManager.AppSettings["collection"];
    string endpointUrl = ConfigurationManager.AppSettings["endpoint"];
    string authorizationKey = ConfigurationManager.AppSettings["authKey"];
    ```

2. Hiermee maakt u de client als volgt:

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

4. ExecuteNextAsync-aanroep voor elke partitiesleutelbereik:

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
> In plaats van `ChangeFeedOptions.PartitionKeyRangeId`, kunt u `ChangeFeedOptions.PartitionKey` om op te geven van een enkele partitiesleutel waarvoor een wijziging in de feed ophalen. Bijvoorbeeld `PartitionKey = new PartitionKey("D8CFA2FD-486A-4F3E-8EA6-F3AA94E5BD44")`.
> 
>

Als u meerdere lezers hebt, kunt u **ChangeFeedOptions** lezen verdelen naar verschillende threads of andere clients.

En dat is alles, met deze paar regels code kunt u starten voor het lezen van de feed wijzigen. Krijgt u de volledige code gebruikt in dit artikel uit de [GitHub-repo-](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeed).

In de code in stap 4 hierboven, de **ResponseContinuation** in de laatste regel bevat het laatste logische volgnummer (LSN) van het document, gaat u de volgende keer dat u nieuwe documenten nadat deze volgnummer lezen. Met behulp van de **StartTime** van de **ChangeFeedOption** kunt u uw net als u de documenten verbreden. Ja, als uw **ResponseContinuation** is null, maar uw **StartTime** gaat u terug in tijd krijgt u de documenten die gewijzigd sinds de **StartTime**. Maar als uw **ResponseContinuation** system krijgt u alle documenten sinds die LSN heeft een waarde.

Uw matrix controlepunt wordt dus alleen te houden het LSN voor elke partitie. Maar als u niet wilt gaan met de partities, begintijd van controlepunten, LSN, enz. de optie eenvoudiger is het gebruik van de wijziging feed processor-bibliotheek.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Met behulp van de wijziging feed processor-bibliotheek 

De [Azure Cosmos DB wijzigen feed processor bibliotheek](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) kunt u eenvoudig de verwerking van gebeurtenissen te verdelen over meerdere gebruikers. Deze bibliotheek vereenvoudigt lezen wijzigingen in partities en meerdere threads die parallel werken.

Het belangrijkste voordeel van wijziging feed processor-bibliotheek is dat u geen hebt voor het beheren van elke partitie en vervolgtoken en u hoeft te peilen handmatig op elke verzameling.

De bibliotheek van de feed processor wijziging vereenvoudigt lezen wijzigingen in partities en meerdere threads die parallel werken.  Hiermee worden beheerd automatisch lezen wijzigingen meerdere partities met een lease-mechanisme. Zoals u in de volgende afbeelding zien kunt als u twee clients die van de feed processor bibliotheek wijziging gebruikmaken start, verdelen ze het werk onderling. Als u doorgaat naar de clients te verhogen, blijven ze het werk onderling verdelen.

![Feed voor gedistribueerde verwerking van Azure Cosmos DB wijzigen](./media/change-feed/change-feed-output.png)

De linker-client eerst is gestart en het starten van de bewaking van alle partities en vervolgens de tweede client is gestart, en vervolgens de eerste van een deel van de tweede client-leases laten gaan. Zoals u dit ziet is de nice manier voor het distribueren van het werk tussen verschillende apparaten en clients.

Houd er rekening mee dat als er twee zonder server Azure funtions bewaking van dezelfde verzameling en het gebruik van de dezelfde lease, en vervolgens de twee functies kunnen verschillende documenten, al naar gelang hoe de processor-bibliotheek wil processs de partities ophalen.

<a id="understand-cf"></a>
### <a name="understanding-the-change-feed-processor-library"></a>Informatie over de wijziging feed processor-bibliotheek

Vier belangrijke onderdelen van de implementatie van de feed processor-bibliotheek van wijziging: de bewaakte verzameling, de verzameling van de lease, de processor-host en de consumenten. 

> [!WARNING]
> Het maken van een verzameling heeft gevolgen voor de prijs, omdat u doorvoer reserveert voor de toepassing om te communiceren met Azure Cosmos DB. Zie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie
> 
> 

**Bewaakte verzameling:** de bewaakte verzameling is de gegevens op basis waarvan de feed wijziging is gegenereerd. Eventuele toevoegingen en wijzigingen in de bewaakte verzameling worden doorgevoerd in de feed wijziging van de verzameling. 

**Verzameling van de lease:** de lease verzameling coördinaten verwerking van de feed in meerdere werknemers wijziging. Een verzameling afzonderlijke wordt gebruikt voor het opslaan van de leases met een lease per partitie. Het is nuttig voor het opslaan van deze verzameling lease op een ander account met de regio schrijven dichter aan waarop de wijziging feed processor wordt uitgevoerd. Een lease-object bevat de volgende kenmerken: 
* Eigenaar: Hiermee geeft u op de host die eigenaar is van de lease
* Voortzetting: Geeft de positie (vervolgtoken) in de feed voor een bepaalde partitie wijziging
* Tijdstempel: Tijd van laatste lease is bijgewerkt. de tijdstempel kan worden gebruikt om te controleren of de lease is verlopen 

**Processor Host:** elke host bepaalt hoeveel partities verwerken op basis van hoeveel andere exemplaren van hosts actieve leases hebben. 
1.  Wanneer u een host start, verkrijgt deze leases om de werkbelasting kan worden verdeeld over alle hosts. Een host vernieuwt periodiek leases, dus leases actief blijven. 
2.  Een host controlepunten lezen voor de laatste vervolgtoken aan de lease voor elk. Een host controleert gelijktijdigheid om veiligheid te garanderen, de ETag voor elke update lease. Andere strategieën controlepunt worden ook ondersteund.  
3.  Bij het afsluiten, is een host alle leases worden vrijgegeven, maar blijft de informatie over de voortzetting dus bij het lezen van het controlepunt van de opgeslagen later kan worden hervat. 

Het aantal hosts mag niet groter zijn dan het aantal partities (leases) op dit moment.

**Consumenten:** consumenten of werknemers, zijn de threads die de verwerking van de wijziging feed gestart door elke host uitvoeren. Elke host processor kan meerdere gebruikers hebben. Elke consumer leest de wijziging feed uit de partitie die is toegewezen aan en ontvangt een melding van de host van wijzigingen en verlopen van leases.

Voor meer inzicht in hoe deze vier elementen van de feed wijziging processor werk samen Bekijk een voorbeeld in het volgende diagram. De bewaakte verzameling documenten worden opgeslagen en gebruikt de 'plaats' als de partitiesleutel. We zien dat de blauwe partitie enzovoort documenten met het veld 'plaats' uit "A-E" bevat. Er zijn twee hosts, elk met twee consumenten lezen van de vier partities parallel. De pijlen geven het lezen van een specifieke positie in de feed wijziging consumenten. In de eerste partitie vertegenwoordigt de donkere blauw ongelezen wijzigingen terwijl de lichte blauw de al gelezen wijzigingen in de feed wijzigen vertegenwoordigt. De hosts de lease-verzameling gebruiken voor het opslaan van een waarde 'voortzetting' om de huidige positie lezen voor elke consumer bij te houden. 

![Met behulp van de wijziging Azure Cosmos DB feed processor host](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>Werken met de wijziging feed processor-bibliotheek

Voordat processor NuGet-pakket installeren wijziging feeds eerst installeren: 

* Microsoft.Azure.DocumentDB, de meest recente versie.
* Newtonsoft.Json, de meest recente versie

Installeer de [Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) en deze opnemen als een verwijzing.

Voor het implementeren van de wijziging feed processor-bibliotheek die u moet doen na:

1. Implementeer een **DocumentFeedObserver** -object, dat wordt geïmplementeerd **IChangeFeedObserver**.
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

5. bouwen de **ChangeFeedProcessorBuilder** na het definiëren van de relevante objecten 

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

That’s it. After these few steps documents will start showing up into the **DocumentFeedObserver.ProcessChangesAsync** method.

Above code is for illustration purpose to show different kind of objects and their interaction. You have to define proper variables and initiate them with correct values. You can get the complete code used in this article from the [GitHub repo](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor).

> [!NOTE]
> You should never have a master key in your code or in config file as shown in above code. Please see [how to use Key-Vault to retrive the keys](https://sarosh.wordpress.com/2017/11/23/cosmos-db-and-key-vault/).


## FAQ

### What are the different ways you can read Change Feed? and when to use each method?

There are three options for you to read change feed:

* **[Using Azure Cosmos DB SQL API .NET SDK](#sql-sdk)**
   
   By using this method, you get low level of control on change feed. You can manage the checkpoint, you can access a particular partition key etc. If you have multiple readers, you can use [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) to distribute read load to different threads or different clients. .

* **[Using the Azure Cosmos DB change feed processor library](#change-feed-processor)**

   If you want to outsource lot of complexity of change feed then you can use change feed processor library. This library hides lot of complexity, but still gives you complete control on change feed. This library follows an [observer pattern](https://en.wikipedia.org/wiki/Observer_pattern), your processing function is called by the SDK. 

   If you have a high throughput change feed, you can instantiate multiple clients to read the change feed. Because you are using “change feed processor library”, it will automatically divide the load among different clients. You do not have to do anything. All the complexity is handled by SDK. However, if you want to have your own load balancer, then you can implement IParitionLoadBalancingStrategy for custom partition strategy. Implement IPartitionProcessor – for custom processing changes on a partition. However, with SDK, you can process a partition range but if you want to process a particular partition key then you have to use SDK for SQL API.

* **[Using Azure Functions](#azure-functions)** 
   
   The last option Azure Function is the simplest option. We recommend using this option. When you create an Azure Cosmos DB trigger in an Azure Functions app, you select the Azure Cosmos DB collection to connect to and the function is triggered whenever a change to the collection is made. watch a [screen cast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) of using Azure function and change feed

   Triggers can be created in the Azure Functions portal, in the Azure Cosmos DB portal, or programmatically. Visual Studio and VS Code has great support to write Azure Function. You can write and debug the code on your desktop, and then deploy the function with one click. For more information, see [Azure Cosmos DB: Serverless database computing using Azure Functions](serverless-computing-database.md) article.

### What is the sort order of documents in change feed?

Change feed documents comes in order of their modification time. This sort order is guaranteed only per partition.

### For a multi-region account, what happens to the change feed when the write-region fails-over? Does the change feed also failover? Would the change feed still appear contiguous or would the fail-over cause change feed to reset?

Yes, change feed will work across the manual failover operation and it will be contiguous.

### How long change feed persist the changed data if I set the TTL (Time to Live) property for the document to -1?

Change feed will persist forever. If data is not deleted, it will remain in change feed.

### How can I configure Azure functions to read from a particular region, as change feed is available in all the read regions by default?

Currently it’s not possible to configure Azure Functions to read from a particular region. There is a GitHub issue in the Azure Functions repo to set the preferred regions of any Azure Cosmos DB binding and trigger.

Azure Functions uses the default connection policy. You can configure connection mode in Azure Functions and by default, it reads from the write region, so it is best to co-locate Azure Functions on the same region.

### What is the default size of batches in Azure Functions?

100 documents at every invocation of Azure Functions. However, this number is configurable within the function.json file. Here is complete [list of configuration options](../azure-functions/functions-run-local.md). If you are developing locally, update the application settings within the [local.settings.json](../azure-functions/functions-run-local.md) file.

### I am monitoring a collection and reading its change feed, however I see I am not getting all the inserted document, some documents are missing. What is going on here?

Please make sure that there is no other function reading the same collection with the same lease collection. It happened to me, and later I realized the missing documents are processed by my other Azure functions, which is also using the same lease.

Therefore, if you are creating multiple Azure Functions to read the same change feed then they must use different lease collection or use the “leasePrefix” configuration to share the same collection. However, when you use change feed processor library you can start multiple instances of your function and SDK will divide the documents between different instances automatically for you.

### My document is updated every second, and I am not getting all the changes in Azure Functions listening to change feed.

Azure Functions polls change feed for every 5 seconds, so any changes made between 5 seconds are lost. Azure Cosmos DB stores just one version for every 5 seconds so you will get the 5th change on the document. However, if you want to go below 5 second, and want to poll change Feed every second, You can configure the polling time “feedPollTime”, see [Azure Cosmos DB bindings](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). It is defined in milliseconds with a default of 5000. Below 1 second is possible but not advisable, as you will start burning more CPU.

### I inserted a document in the Mongo API collection, but when I get the document in change feed, it shows a different id value. What is wrong here?

Your collection is Mongo API collection. Remember, change feed is read using the SQL client and serializes items into JSON format. Because of the JSON formatting, MongoDB clients will experience a mismatch between BSON formatted documents and the JSON formatted change feed. You are seeing is the representation of a BSON document in JSON. If you use binary attributes in a Mongo accounts, they are converted to JSON.

### Is there a way to control change feed for updates only and not inserts?

Not today, but this functionality is on roadmap. Today, you can add a soft marker on the document for updates.

### Is there a way to get deletes in change feed?

Currently change feed doesn’t log deletes. Change feed is continuously improving, and this functionality is on roadmap. Today, you can add a soft marker on the document for delete. Add an attribute on the document called “deleted” and set it to “true” and set a TTL on the document so that it can be automatically deleted.

### Can I read change feed for historic documents(for example, documents that were added 5 years back) ?

Yes, if the document is not deleted you can read the change feed as far as the origin of your collection.

### Can I read change feed using JavaScript?

Yes, Node.js SDK initial support for change feed is recently added. It can be used as shown in the following example, please update documentdb module to current version before you run the code:

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

### <a name="can-i-read-change-feed-using-java"></a>Kan ik een wijziging feed met Java lezen?

Java-bibliotheek moet worden gelezen wijziging feed is beschikbaar in [Github-opslagplaats](https://github.com/Azure/azure-documentdb-changefeedprocessor-java). Echter, Java-bibliotheek is momenteel enkele versies achter .NET-bibliotheek. Snel zowel de bibliotheken worden gesynchroniseerd.

### <a name="can-i-use-etag-lsn-or-ts-for-internal-bookkeeping-which-i-get-in-response"></a>Kan ik _etag, _lsn of _ts gebruiken voor interne administratie die ik in het antwoord krijgen?

_etag indeling interne is en u moet niet afhankelijk (komen niet parseren deze) omdat deze op elk gewenst moment kunt wijzigen.
_ts is wijziging of maken van het tijdstempel. U kunt _ts gebruiken voor chronologische vergelijking.
een batch-id die is toegevoegd voor de feed wijziging alleen _lsn is is, is de transactie-id uit het archief... Veel documenten mogelijk dezelfde _lsn.
Één ding te weten, ETag op FeedResponse is anders dan de _etag u in het document ziet. _etag is een interne id en gebruikt voor gelijktijdigheid van taken, vertelt over de versie van het document en ETag wordt gebruikt voor het sequentiëren van de feed.

### <a name="does-reading-change-feed-add-any-additional-cost-"></a>Wijziging feed lezen eventuele extra kosten toevoegen?

U in rekening worden gebracht voor de RU dat wil zeggen verbruikt de verplaatsing van gegevens in en uit Azure Cosmos DB verzamelingen altijd RU gebruiken. Gebruikers wordt gebruikt door de verzameling lease RU aangerekend.

### <a name="can-multiple-azure-functions-read-one-collections-change-feed"></a>Kan meerdere Azure Functions één verzameling wijziging feed lezen?

Ja. Meerdere Azure-functies kunnen dezelfde verzameling wijziging feed lezen. De Azure Functions moet echter een afzonderlijke leaseCollectionPrefix gedefinieerd hebben.

### <a name="should-the-lease-collection-be-partitioned"></a>De verzameling van de lease worden gepartitioneerd?

Nee, de verzameling van de lease kan worden verholpen. Gepartitioneerde lease verzameling is niet nodig en wordt momenteel niet ondersteund.

### <a name="can-i-read-change-feed-from-spark"></a>Kan ik wijziging lezen feed vanuit Spark?

Ja, dat is mogelijk. Zie [Azure Cosmos DB Spark Connector](spark-connector.md). Hier volgt een [scherm cast](https://www.youtube.com/watch?v=P9Qz4pwKm_0&t=1519s) die laat zien hoe u de wijziging feed als een structured stream kunt verwerken.

### <a name="if-i-am-processing-change-feed-by-using-azure-functions-say-a-batch-of-10-documents-and-i-get-an-error-at-7th-document-in-that-case-the-last-three-documents-are-not-processed-how-can-i-start-processing-from-the-failed-documentie-7th-document-in-my-next-feed"></a>Als ik ben wijzigen met behulp van Azure Functions-feed verwerken is een batch van 10 documenten spreken en verschijnt het foutbericht op 7 Document. In dat geval de laatste drie documenten niet zijn verwerkt hoe kan ik beginnen verwerking uit het document (eenledige 7 document) in de volgende feed?

Voor het afhandelen van de fout is het aanbevolen patroon inpakken van uw code met trycatch-blok. Catch de fout en dat document in een wachtrij (onbestelbare) plaatsen en vervolgens definiëren logica te gaan met de documenten die de fout. Met deze methode als u een batch 200-document en slechts één document is mislukt, hebt hoeft u niet te weggooien van de hele batch.

Voor het geval van fout moet u het selectievakje punt terug naar het begin niet terugspoelen wordt anders u kunt steeds die documenten van de feed wijzigen. Vergeet niet wijzigen feed houdt de laatste laatste module opname van de documenten, omdat u de vorige momentopname aan het document kunnen worden verbroken. de feed wijziging blijft slechts één van de laatste versie van het document en tussen andere processen kunnen worden geleverd en het document wijzigen.

Als u uw code blijven hersteld, vindt u snel geen documenten in de wachtrij voor onbestelbare berichten.
Azure Functions automatisch wordt aangeroepen door de feed system wijzigen en controlepunt enzovoort wordt intern beheerd door de Azure-functie. Als u wilt terugdraaien, het selectievakje punt en elk aspect van het beheren, kunt u overwegen gebruik wijziging feed Processor SDK.


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure DB die Cosmos met Azure Functions [Azure Cosmos DB: zonder Server database computing met behulp van Azure Functions](serverless-computing-database.md).

Gebruik de volgende bronnen voor meer informatie over het gebruik van de feed processor bibliotheek wijziging:

* [Informatiepagina](sql-api-sdk-dotnet-changefeed.md) 
* [Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Stap 1-6 hierboven weergegeven voorbeeldcode](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [Aanvullende voorbeelden op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

Gebruik de volgende bronnen voor meer informatie over het gebruik van de wijziging via de SDK-feed:

* [Informatiepagina SDK](sql-api-sdk-dotnet.md)
