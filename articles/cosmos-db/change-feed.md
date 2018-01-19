---
title: Werken met de wijziging feed ondersteuning in Azure Cosmos DB | Microsoft Docs
description: Azure Cosmos DB wijzigen feed ondersteuning gebruiken voor het bijhouden van wijzigingen in documenten en het uitvoeren van verwerking op basis van gebeurtenissen zoals triggers en caches en analyses systemen up-to-date te houden.
keywords: Feed wijzigen
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig
documentationcenter: 
ms.assetid: 2d7798db-857f-431a-b10f-3ccbc7d93b50
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 10/30/2017
ms.author: arramac
ms.openlocfilehash: d1968e9fea0fb08edfdbf9e09acca9c4af00b048
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
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

## <a name="how-does-change-feed-work"></a>Hoe wijziging feed werk?

Feed-ondersteuning in Azure Cosmos DB werkt wijzigen door te luisteren naar een verzameling Azure Cosmos DB op basis van wijzigingen. Het levert vervolgens de gesorteerde lijst van documenten die zijn gewijzigd in de volgorde waarin ze zijn gewijzigd. De wijzigingen worden doorgevoerd, kunnen worden verwerkt asynchroon en incrementeel en de uitvoer kan worden verdeeld over een of meer consumenten voor parallelle verwerking. 

Zoals verderop in dit artikel wordt beschreven, kunt u de wijziging in drie verschillende manieren feed lezen:

1.  [Met behulp van Azure Functions](#azure-functions)
2.  [Met behulp van de Azure SDK voor Documentdb-Cosmos](#rest-apis)
3.  [Met behulp van de bibliotheek Azure Cosmos DB wijzigen Feed Processor](#change-feed-processor)

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

## <a name="use-cases-and-scenarios"></a>Gebruiksvoorbeelden en scenario 's

De feed wijziging kan efficiënte verwerking van grote gegevenssets met een groot aantal schrijfbewerkingen en biedt een alternatief voor het opvragen van een volledige gegevensset om te zien wat er is gewijzigd. 

Bijvoorbeeld, met een wijziging van de feed kunt u de volgende taken uitvoeren efficiënt:

* Een cache, search-index of een datawarehouse bijwerken met gegevens die zijn opgeslagen in Azure Cosmos DB.
* Implementeren op toepassingsniveau gegevens lagen en archivering, dat wil zeggen, gegevensopslag ' hot ' in Azure Cosmos DB en na verloop van tijd 'koude gegevens' naar [Azure Blob Storage](../storage/common/storage-introduction.md) of [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md).
* Batch analytics implementeren op de gegevens met [Apache Hadoop](run-hadoop-with-hdinsight.md).
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

<a id="rest-apis"></a>
## <a name="using-the-sdk"></a>Met behulp van de SDK

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

Als u meerdere lezers hebt, kunt u **ChangeFeedOptions** lezen verdelen naar verschillende threads of andere clients.

En dat is alles, met deze paar regels code kunt u starten voor het lezen van de feed wijzigen. Krijgt u de volledige code gebruikt in dit artikel uit de [GitHub-repo-](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor).

In de code in stap 4 hierboven, de **ResponseContinuation** in de laatste regel bevat het laatste logische volgnummer (LSN) van het document, gaat u de volgende keer dat u nieuwe documenten nadat deze volgnummer lezen. Met behulp van de **StartTime** van de **ChangeFeedOption** kunt u uw net als u de documenten verbreden. Ja, als uw **ResponseContinuation** is null, maar uw **StartTime** gaat u terug in tijd krijgt u de documenten die gewijzigd sinds de **StartTime**. Maar als uw **ResponseContinuation** system krijgt u alle documenten sinds die LSN heeft een waarde.

Uw matrix controlepunt wordt dus alleen te houden het LSN voor elke partitie. Maar als u niet wilt gaan met de partities, begintijd van controlepunten, LSN, enz. de optie eenvoudiger is het gebruik van de wijziging Feed Processor-bibliotheek.

<a id="change-feed-processor"></a>
## <a name="using-the-change-feed-processor-library"></a>Met behulp van de wijziging Feed Processor-bibliotheek 

De [Azure Cosmos DB wijzigen Feed Processor bibliotheek](https://docs.microsoft.com/azure/cosmos-db/sql-api-sdk-dotnet-changefeed) kunt u eenvoudig de verwerking van gebeurtenissen te verdelen over meerdere gebruikers. Deze bibliotheek vereenvoudigt lezen wijzigingen in partities en meerdere threads die parallel werken.

Het belangrijkste voordeel van wijziging Feed Processor-bibliotheek is dat u geen hebt voor het beheren van elke partitie en vervolgtoken en u hoeft te peilen handmatig op elke verzameling.

De bibliotheek wijzigen Feed Processor vereenvoudigt lezen wijzigingen in partities en meerdere threads die parallel werken.  Hiermee worden beheerd automatisch lezen wijzigingen meerdere partities met een lease-mechanisme. Zoals u in de volgende afbeelding zien kunt als u twee clients die van de bibliotheek wijzigen Feed Processor gebruikmaken start, verdelen ze het werk onderling. Als u doorgaat naar de clients te verhogen, blijven ze het werk onderling verdelen.

![Feed voor gedistribueerde verwerking van Azure Cosmos DB wijzigen](./media/change-feed/change-feed-output.png)

De linker-client eerst is gestart en het starten van de bewaking van alle partities en vervolgens de tweede client is gestart, en vervolgens de eerste van een deel van de tweede client-leases laten gaan. Zoals u dit ziet is de nice manier voor het distribueren van het werk tussen verschillende apparaten en clients.

Houd er rekening mee dat als er twee zonder server Azure funtions bewaking van dezelfde verzameling en het gebruik van de dezelfde lease, en vervolgens de twee functies kunnen verschillende documenten, al naar gelang hoe de processor-bibliotheek wil processs de partities ophalen.

### <a name="understanding-the-change-feed-processor-library"></a>Wat is de wijziging Feed Processor-bibliotheek?

Vier belangrijke onderdelen van de implementatie van de Feed Processor van wijziging: de bewaakte verzameling, de verzameling van de lease, de processor-host en de consumenten. 

> [!WARNING]
> Maken van een verzameling heeft gevolgen, zoals u bij het reserveren van doorvoer voor de toepassing om te communiceren met Azure Cosmos DB. Voor meer informatie raadpleegt u de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/)
> 
> 

**Bewaakte verzameling:** de bewaakte verzameling is de gegevens op basis waarvan de feed wijziging is gegenereerd. Eventuele toevoegingen en wijzigingen in de bewaakte verzameling worden doorgevoerd in de feed wijziging van de verzameling. 

**Verzameling van de lease:** de lease verzameling coördinaten verwerking van de feed in meerdere werknemers wijziging. Een verzameling afzonderlijke wordt gebruikt voor het opslaan van de leases met een lease per partitie. Het is nuttig voor het opslaan van deze verzameling lease op een ander account met de regio schrijven dichter aan waarop de wijziging Feed Processor wordt uitgevoerd. Een lease-object bevat de volgende kenmerken: 
* Eigenaar: Hiermee geeft u op de host die eigenaar is van de lease
* Voortzetting: Geeft de positie (vervolgtoken) in de feed voor een bepaalde partitie wijziging
* Tijdstempel: Tijd van laatste lease is bijgewerkt. de tijdstempel kan worden gebruikt om te controleren of de lease is verlopen 

**Processor Host:** elke host bepaalt hoeveel partities verwerken op basis van hoeveel andere exemplaren van hosts actieve leases hebben. 
1.  Wanneer u een host start, verkrijgt deze leases om de werkbelasting kan worden verdeeld over alle hosts. Een host vernieuwt periodiek leases, dus leases actief blijven. 
2.  Een host controlepunten lezen voor de laatste vervolgtoken aan de lease voor elk. Een host controleert gelijktijdigheid om veiligheid te garanderen, de ETag voor elke update lease. Andere strategieën controlepunt worden ook ondersteund.  
3.  Bij het afsluiten, is een host alle leases worden vrijgegeven, maar blijft de informatie over de voortzetting dus bij het lezen van het controlepunt van de opgeslagen later kan worden hervat. 

Het aantal hosts mag niet groter zijn dan het aantal partities (leases) op dit moment.

**Consumenten:** consumenten of werknemers, zijn de threads die de verwerking van de wijziging feed gestart door elke host uitvoeren. Elke host processor kan meerdere gebruikers hebben. Elke consumer leest de wijziging feed uit de partitie die is toegewezen aan en ontvangt een melding van de host van wijzigingen en verlopen van leases.

Om verder te begrijpen hoe deze vier elementen van wijziging Feed Processor samenwerken, bekijk een voorbeeld in het volgende diagram. De bewaakte verzameling documenten worden opgeslagen en gebruikt de 'plaats' als de partitiesleutel. We zien dat de blauwe partitie enzovoort documenten met het veld 'plaats' uit "A-E" bevat. Er zijn twee hosts, elk met twee consumenten lezen van de vier partities parallel. De pijlen geven het lezen van een specifieke positie in de feed wijziging consumenten. In de eerste partitie vertegenwoordigt de donkere blauw ongelezen wijzigingen terwijl de lichte blauw de al gelezen wijzigingen in de feed wijzigen vertegenwoordigt. De hosts de lease-verzameling gebruiken voor het opslaan van een waarde 'voortzetting' om de huidige positie lezen voor elke consumer bij te houden. 

![Met behulp van de wijziging Azure Cosmos DB feed processor host](./media/change-feed/changefeedprocessornew.png)

### <a name="working-with-the-change-feed-processor-library"></a>Werken met de wijziging Feed Processor-bibliotheek

Voordat u wijziging Feed Processor NuGet-pakket installeert, eerst installeren: 

* Microsoft.Azure.DocumentDB, versie 1.13.1 of hoger 
* Newtonsoft.Json, versie 9.0.1 of hoger

Installeer de [Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) en deze opnemen als een verwijzing.

Voor het implementeren van de wijziging Feed Processor-bibliotheek die u moet doen na:

1. Implementeer een **DocumentFeedObserver** -object, dat wordt geïmplementeerd **IChangeFeedObserver**.

2. Implementeer een **DocumentFeedObserverFactory**, welke implementeert een **IChangeFeedObserverFactory**.

3. In de **CreateObserver** methode van **DocumentFeedObserverFacory**, exemplaar maken van de **ChangeFeedObserver** die u in stap 1 hebt gemaakt en terugzetten.

    ```
    public IChangeFeedObserver CreateObserver()
    {
              DocumentFeedObserver newObserver = new DocumentFeedObserver(this.client, this.collectionInfo);
              return newObserver;
    }
    ```

4. Instantiëren **DocumentObserverFactory**.

5. Exemplaar maken van een **ChangeFeedEventHost**:

    ```csharp
    ChangeFeedEventHost host = new ChangeFeedEventHost(
                     hostName,
                     documentCollectionLocation,
                     leaseCollectionLocation,
                     feedOptions,
                     feedHostOptions);
    ```

6. Registreer de **DocumentFeedObserverFactory** aan de host.

De code voor de stappen 4 tot en met 6 is: 

```
ChangeFeedOptions feedOptions = new ChangeFeedOptions();
feedOptions.StartFromBeginning = true;

ChangeFeedHostOptions feedHostOptions = new ChangeFeedHostOptions();
 
// Customizing lease renewal interval to 15 seconds.
// Can customize LeaseRenewInterval, LeaseAcquireInterval, LeaseExpirationInterval, FeedPollDelay
feedHostOptions.LeaseRenewInterval = TimeSpan.FromSeconds(15);
 
using (DocumentClient destClient = new DocumentClient(destCollInfo.Uri, destCollInfo.MasterKey))
{
        DocumentFeedObserverFactory docObserverFactory = new DocumentFeedObserverFactory(destClient, destCollInfo);
        ChangeFeedEventHost host = new ChangeFeedEventHost(hostName, documentCollectionLocation, leaseCollectionLocation, feedOptions, feedHostOptions);
        await host.RegisterObserverFactoryAsync(docObserverFactory);
        await host.UnregisterObserversAsync();
}
```

Dat is alles. Na deze stappen in afkomstig van documenten gaat de **DocumentFeedObserver ProcessChangesAsync** methode.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van Azure DB die Cosmos met Azure Functions [Azure Cosmos DB: zonder Server database computing met behulp van Azure Functions](serverless-computing-database.md).

Gebruik de volgende bronnen voor meer informatie over het gebruik van de wijziging Feed Processor-bibliotheek:

* [Informatiepagina](sql-api-sdk-dotnet-changefeed.md) 
* [Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)
* [Stap 1-6 hierboven weergegeven voorbeeldcode](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)
* [Aanvullende voorbeelden op GitHub](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/ChangeFeedProcessor)

Gebruik de volgende bronnen voor meer informatie over het gebruik van de wijziging via de SDK-feed:

* [Informatiepagina SDK](sql-api-sdk-dotnet.md)
