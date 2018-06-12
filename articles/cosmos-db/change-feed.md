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
ms.openlocfilehash: 2600565493a334c7227e5c0d67a5808f30751108
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261064"
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

* Microsoft.Azure.DocumentDB, versie 1.13.1 of hoger 
* Newtonsoft.Json, versie 9.0.1 of hoger

Installeer de [Microsoft.Azure.DocumentDB.ChangeFeedProcessor Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/) en deze opnemen als een verwijzing.

Voor het implementeren van de wijziging feed processor-bibliotheek die u moet doen na:

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

Dat is alles. Na deze stappen in afkomstig van documenten gaat de **DocumentFeedObserver ProcessChangesAsync** methode. Zoek de bovenstaande code in [GitHub-repo-](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples/ChangeFeedProcessor)

## <a name="faq"></a>Veelgestelde vragen

### <a name="what-are-the-different-ways-you-can-read-change-feed-and-when-to-use-each-method"></a>Wat zijn de verschillende manieren kunt u lezen Feed wijzigen? en het gebruik van elke methode?

Er zijn drie opties voor het lezen van de feed wijzigen:

* **[Met behulp van Azure Cosmos .NET SDK voor Documentdb SQL-API](#sql-sdk)**
   
   Met deze methode, moet u lage mate van controle op wijziging feed ophalen. U kunt het controlepunt beheren, u toegang hebt tot een bepaalde partitie sleutels enzovoort. Als u meerdere lezers hebt, kunt u [ChangeFeedOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.changefeedoptions?view=azure-dotnet) lezen verdelen naar verschillende threads of andere clients. .

* **[Met behulp van de wijziging Azure Cosmos DB feed processor-bibliotheek](#change-feed-processor)**

   Als u wilt uitbesteden van veel van de complexiteit van de feed wijzigen kunt u wijzigen feed processor bibliotheek gebruiken. Deze bibliotheek verborgen veel complexiteit, maar nog steeds hebt die u volledige controle op feed wijzigen. Deze bibliotheek volgt een [zien patroon](https://en.wikipedia.org/wiki/Observer_pattern), de verwerking van de functie wordt aangeroepen door de SDK. 

   Als u een hoge doorvoersnelheid wijziging in de feed hebt, kunt u meerdere clients voor het lezen van de feed wijziging instantiëren. Omdat u 'wijzigen feed processor bibliotheek' gebruikt, wordt deze automatisch de belasting van andere clients delen. U beschikt niet over verder niets te doen. De complexiteit wordt verwerkt door de SDK. Echter als u een eigen netwerkverdeler hebt wilt, kunt klikt u vervolgens u implementeren IParitionLoadBalancingStrategy voor aangepaste partitie-strategie. Implementeren IPartitionProcessor – aangepaste verwerking wijzigingen op een partitie. Echter kunt u een partitiebereik verwerken met de SDK, maar als u wilt laten verwerken van een bepaalde partitiesleutel hebt u met de SDK voor SQL-API.

* **[Met behulp van Azure Functions](#azure-functions)** 
   
   De laatste optie Azure-functie is de eenvoudigste optie. U wordt aangeraden deze optie gebruikt. Wanneer u een Azure DB die Cosmos-trigger in een Azure Functions-app maakt, u de Azure DB die Cosmos-verzameling verbinding maken met selecteren en de functie wordt geactiveerd wanneer een wijziging aan de verzameling wordt aangebracht. Bekijk een [scherm cast](https://www.youtube.com/watch?v=Mnq0O91i-0s&t=14s) functioneren van het gebruik van Azure en feed wijzigen

   Triggers kunnen worden gemaakt in de Azure Functions-portal in de Azure DB die Cosmos-portal of programmatisch. Visual Studio en VS-Code heeft uitstekende ondersteuning voor het schrijven van Azure-functie. U kunt schrijven opsporen in de code op het bureaublad en vervolgens de functie met één klik implementeren. Zie voor meer informatie [Azure Cosmos DB: zonder Server database computing met behulp van Azure Functions](serverless-computing-database.md) artikel.

### <a name="what-is-the-sort-order-of-documents-in-change-feed"></a>Wat is de sorteervolgorde van documenten in de feed wijzigen?

Wijziging documenten feed wordt geleverd in volgorde van de tijd van de wijziging. Deze volgorde is gegarandeerd alleen per partitie.

### <a name="for-a-multi-region-account-what-happens-to-the-change-feed-when-the-write-region-fails-over-does-the-change-feed-also-failover-would-the-change-feed-still-appear-contiguous-or-would-the-fail-over-cause-change-feed-to-reset"></a>Voor een account meerdere landen/regio, wat gebeurt er wanneer de write-regio mislukt-over-kanaal wijzigen? De wijziging feed ook failover? De wijziging nog steeds feed lijkt aaneengesloten of zou veranderen de oorzaak van de failover-feed opnieuw instellen?

Ja, feed wijzigen via de handmatige failover-bewerking werkt en zal deze aaneengesloten.

### <a name="how-long-change-feed-persist-the-changed-data-if-i-set-the-ttl-time-to-live-property-for-the-document-to--1"></a>Hoe lang de feed wijzigen de gewijzigde gegevens behouden als ik de TTL (Time to Live)-eigenschap voor het document is ingesteld op-1?

Wijziging feed bewaard permanent. Als gegevens niet wordt verwijderd, blijft deze in de feed wijzigen.

### <a name="how-can-i-configure-azure-functions-to-read-from-a-particular-region-as-change-feed-is-available-in-all-the-read-regions-by-default"></a>Hoe kan ik Azure functions lezen van een bepaald gebied configureren zoals de wijziging feed beschikbaar is in alle lezen regio's standaard?

Het is momenteel niet mogelijk voor het configureren van Azure Functions lezen van een bepaald gebied. Er is een probleem met de GitHub in de Azure Functions-opslagplaats instellen van de gewenste regio's van Azure DB die Cosmos-binding en trigger.

Azure Functions maakt gebruik van het standaardbeleid. U kunt verbindingsmodus configureren in Azure Functions en standaard, wordt gelezen uit de regio schrijven, dus is het beste Azure Functions in dezelfde regio CO-locatie.

### <a name="what-is-the-default-size-of-batches-in-azure-functions"></a>Wat is de standaardgrootte van batches in Azure Functions?

100 documenten bij elke aanroep van Azure Functions. Dit nummer kan echter worden geconfigureerd in het bestand function.json. Hier is voltooid [lijst met configuratieopties](../azure-functions/functions-run-local.md). Als u lokaal ontwikkelt, werkt u de toepassingsinstellingen binnen de [local.settings.json](../azure-functions/functions-run-local.md) bestand.

### <a name="i-am-monitoring-a-collection-and-reading-its-change-feed-however-i-see-i-am-not-getting-all-the-inserted-document-some-documents-are-missing-what-is-going-on-here"></a>Ik ben een verzameling bewaking en het lezen van de wijziging feed, maar ik zie ik krijg niet ingevoegd document, bepaalde documenten ontbreken. Wat er gebeurt hier?

Zorg ervoor dat er geen andere functie lezen van dezelfde verzameling met dezelfde lease-verzameling. Het probleem optrad aan mij en later ik de ontbrekende documenten worden verwerkt door mijn andere Azure-functies, die ook gebruikmaakt van de lease dezelfde gerealiseerd.

Dus als u meerdere Azure Functions lezen maakt wijzigen dezelfde feed ze moeten gebruiken om het verzamelen van verschillende lease of de configuratie van het 'leasePrefix' gebruiken voor het delen van dezelfde verzameling. Wanneer u wijziging feed processor bibliotheek kunt u meerdere exemplaren van de functie starten en SDK wordt verdeeld tussen verschillende exemplaren automatisch voor u documenten.

### <a name="my-document-is-updated-every-second-and-i-am-not-getting-all-the-changes-in-azure-functions-listening-to-change-feed"></a>Mijn document per seconde wordt bijgewerkt en ik krijg niet alle wijzigingen in de Azure Functions luisteren om te wijzigen van de feed.

Azure Functions polls wijziging feed voor elke 5 seconden, zodat eventuele wijzigingen die zijn aangebracht tussen 5 seconden gaan verloren. Er wordt dus de 5e wijziging in het document, Azure Cosmos DB slechts één versie slaat voor elke vijf seconden. Echter als u wilt gaat dan 5 seconden en wijzig elke seconde Feed polling wilt, u kunt de tijd van polling 'feedPollTime' configureren, Zie [bindingen van Azure DB die Cosmos](../azure-functions/functions-bindings-cosmosdb.md#trigger---configuration). Dit is gedefinieerd in milliseconden met een standaardwaarde van 5000. Is mogelijk, maar niet aangeraden, als u wilt beginnen met branden meer CPU hieronder 1 seconde.

### <a name="i-inserted-a-document-in-the-mongo-api-collection-but-when-i-get-the-document-in-change-feed-it-shows-a-different-id-value-what-is-wrong-here"></a>Een document ingevoegd in de verzameling Mongo-API, maar wanneer ik het document in de feed wijzigen ophalen, wordt een andere id-waarde. Wat is het probleem hier?

De verzameling is Mongo-API-verzameling. Denk eraan, feed wijzigen met behulp van de SQL-client wordt gelezen en serialiseert items in de JSON-indeling. Vanwege de JSON opmaak, clients krijgen MongoDB geformatteerd een discrepantie tussen BSON geformatteerd documenten en de JSON feed wijzigen. U ziet de representatie van een document BSON in JSON is. Als u binaire kenmerken in een Mongo-accounts gebruikt, wordt deze omgezet naar JSON.

### <a name="is-there-a-way-to-control-change-feed-for-updates-only-and-not-inserts"></a>Is er een manier om te bepalen van de feed alleen voor updates wijzigen en niet ingevoegd?

Geen vandaag de dag, maar deze functionaliteit is voor roadmap. Vandaag de dag, kunt u een voorlopig markering toevoegen op het document voor updates.

### <a name="is-there-a-way-to-get-deletes-in-change-feed"></a>Is er een manier om verwijderingen in de feed wijzigen?

Wijziging feed niet verwijdert momenteel voor logboekbestanden. Continu verbetering van de feed wijzigen en deze functionaliteit wordt roadmap. Vandaag de dag, kunt u een voorlopig markering toevoegen op het document om te verwijderen. Een kenmerk toevoegen aan het document 'verwijderd' genoemd en stel deze in op 'true' en een TTL-waarde instellen voor het document, zodat kan automatisch worden verwijderd.

### <a name="can-i-read-change-feed-for-historic-documentsfor-example-documents-that-were-added-5-years-back-"></a>Kan ik wijziging feed voor historische documenten (bijvoorbeeld documenten die zijn toegevoegd 5 jaar terug) lezen?

Ja, als het document is niet verwijderd. u kunt de wijziging lezen feed mate van de oorsprong van uw verzameling.

### <a name="can-i-read-change-feed-using-javascript"></a>Kan ik een wijziging feed met JavaScript lezen?

Ja, is de eerste ondersteuning Node.js-SDK voor wijziging feed onlangs toegevoegd. Het kan worden gebruikt zoals in het volgende voorbeeld Neem update documentdb-module voor de huidige versie voordat u de code uitvoeren:

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
