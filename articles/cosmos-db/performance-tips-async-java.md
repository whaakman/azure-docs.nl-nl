---
title: Azure Cosmos DB tips voor betere prestaties voor Async Java
description: Informatie over client-configuratieopties voor het verbeteren van de prestaties van de Azure Cosmos DB-database
keywords: over het verbeteren van de prestaties van de database
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: sngun
ms.openlocfilehash: 440dc13e2c6f4d9acc270b644cc549280e6d91be
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53413571"
---
# <a name="performance-tips-for-azure-cosmos-db-and-async-java"></a>Tips voor betere prestaties voor Azure Cosmos DB en Async Java

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB is een snelle en flexibele gedistribueerde database die kan worden opgeschaald naadloos met een gegarandeerde latentie en doorvoer. U hoeft niet te grote architectuur wijzigingen aanbrengen of complexe code schrijven om te schalen van uw database met Azure Cosmos DB. Omhoog en omlaag schalen is net zo gemakkelijk als het maken van een één API-aanroep of een aanroep van de SDK-methode. Omdat Azure Cosmos DB is toegankelijk via netwerkaanroepen er zijn echter client-side '-optimalisatie u maken kunt voor het behalen van optimale prestaties bij het gebruik van de [SQL Async Java SDK](sql-api-sdk-async-java.md).

Dus als u vraagt "hoe kan ik mijn de databaseprestaties verbeteren?" Houd rekening met de volgende opties:

## <a name="networking"></a>Netwerken
   <a id="same-region"></a>
1. **Clients in hetzelfde Azure-regio voor de prestaties plaatsen**

    Indien mogelijk, plaatst u alle toepassingen aanroepen van Azure Cosmos DB in dezelfde regio als de Azure Cosmos DB-database. Voor een geschatte vergelijking: aanroepen naar Azure Cosmos DB binnen dezelfde regio binnen 1-2 ms voltooid, maar de latentie tussen de West- en oostkust van de Verenigde Staten is > 50 ms. Deze latentie kan waarschijnlijk uit om aan te vragen variëren, afhankelijk van de route die door de aanvraag is uitgevoerd, zoals het aan de grens van de Azure-datacenter van de client doorgeeft. De laagst mogelijke latentie wordt bereikt door ervoor te zorgen dat de aanroepende toepassing bevindt zich in dezelfde Azure-regio als de ingerichte Azure Cosmos DB-eindpunt. Zie voor een lijst van beschikbare regio's, [Azure-regio's](https://azure.microsoft.com/regions/#services).

    ![Afbeelding van het beleid voor Azure Cosmos DB-verbinding](./media/performance-tips/same-region.png)

## <a name="sdk-usage"></a>SDK-gebruik
1. **Installeer de meest recente SDK**

    De Azure Cosmos DB SDK's worden voortdurend verbeterd om de beste prestaties bieden. Zie de [Azure Cosmos DB SDK](sql-api-sdk-async-java.md) pagina's om te bepalen van de meest recente SDK en verbeteringen te bekijken.
2. **Een singleton-Azure Cosmos DB-client gebruiken voor de levensduur van uw toepassing**

    Elk exemplaar AsyncDocumentClient thread-veilig is en efficiënt beheer en adrescaching uitvoert. Om toe te staan efficiënt beheer en betere prestaties door AsyncDocumentClient, is het raadzaam met één exemplaar van AsyncDocumentClient per toepassingsdomein voor de levensduur van de toepassing.

   <a id="max-connection"></a>

3. **ConnectionPolicy afstemmen**

    Azure Cosmos DB-aanvragen via HTTPS/REST worden gemaakt bij het gebruik van het Async Java SDK, en zijn onderworpen aan de grootte van de standaard maximale verbinding-groep (1000). Deze waarde moet zijn ideaal voor het merendeel van de use-cases. Als u een grote verzameling met veel partities hebt, kunt u de maximale grootte van de verbindingsgroep instellen op een groter aantal (bijvoorbeeld 1500) met behulp van setMaxPoolSize.

4. **Parallelle query's voor gepartitioneerde verzamelingen afstemmen**

    Azure Cosmos DB SQL Async Java SDK biedt ondersteuning voor parallelle query's, zodat ze een gepartitioneerde verzameling worden parallel query. Zie voor meer informatie, [codevoorbeelden](https://github.com/Azure/azure-cosmosdb-java/tree/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples) met betrekking tot het werken met de SDK's. Parallelle query's zijn ontworpen voor betere latentie van query en de doorvoer via de seriële equivalent.

    (a) ***afstemmen setMaxDegreeOfParallelism\:***  parallelle query's werken door meerdere partities parallel uitvoeren van query's. Gegevens van een afzonderlijke gepartitioneerde verzameling is echter worden opgehaald met betrekking tot de query. Gebruik setMaxDegreeOfParallelism om in te stellen van het aantal partities waarvoor de maximale kans dat de meeste goed presterende query's en alle andere system-voorwaarden opgegeven bereiken blijven dus hetzelfde. Als u het aantal partities niet weet, kunt u setMaxDegreeOfParallelism om in te stellen van een groot aantal en het systeem kiest de minimale (het aantal partities, door de gebruiker opgegeven invoer) als de maximale graad van parallelle uitvoering.

    Het is belangrijk te weten dat de parallelle query's de beste prestaties opleveren als de gegevens gelijkmatig wordt verdeeld over alle partities met betrekking tot de query. Als de gepartitioneerde verzameling zodanig dat alle of een meerderheid van de gegevens die zijn geretourneerd door een query is geconcentreerd in een paar partities (één partitie in het ergste geval) en vervolgens op de prestaties van de query zou worden knelpunt door deze partities is gepartitioneerd.

    (b) ***afstemmen setMaxBufferedItemCount\:***  parallelle query is ontworpen voor het vooraf ophalen van resultaten terwijl de huidige batch van de resultaten wordt verwerkt door de client. De vooraf ophalen helpt bij het algemene verbetering van de latentie van een query. setMaxBufferedItemCount beperkt het aantal vooraf opgehaalde resultaten. Als u setMaxBufferedItemCount met het verwachte aantal geretourneerde resultaten (of een hogere waarde), kunt de query voor het ontvangen van maximaal profiteren van het vooraf ophalen.

    Op dezelfde manier, ongeacht de MaxDegreeOfParallelism vooraf ophalen werkt en er is één buffer voor de gegevens van alle partities.

5. **Uitstel getRetryAfterInMilliseconds tussenpozen implementeren**

    Tijdens het Prestatietesten van, moet u load vergroten totdat een klein aantal aanvragen te maken met beperkingen. Als beperkt, moet de clienttoepassing uitstel voor de server opgegeven interval. Respecteer de uitstel zorgt ervoor dat u besteden aan de minimale hoeveelheid tijd wachten tussen nieuwe pogingen.
6. **Uw client-workload uitschalen**

    Als u op het niveau van hoge doorvoer testen wilt (> 50.000 RU/s), de clienttoepassing kan knelpunt vanwege de machine beperking af op CPU- of -gebruik. Als u dit punt bereikt, kunt u blijven om de Azure Cosmos DB-account verder door uw client-toepassingen schalen over meerdere servers.

7. **Gebruik op basis van naam-adressering**

    Gebruik op basis van naam-adressering, waarbij koppelingen hebben de indeling `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, in plaats van SelfLinks (\_zelf), hebben de indeling `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` om te voorkomen dat bij het ophalen van ResourceIds van alle resources die worden gebruikt voor het maken van de koppeling. Ook als deze resources ophalen opnieuw gemaakt (mogelijk met dezelfde naam), kunt deze in cache niet.

   <a id="tune-page-size"></a>
8. **De paginagrootte voor query's / lezen-feeds voor betere prestaties afstemmen**

    Bij het uitvoeren van een bulksgewijs documenten met behulp van de feed-functionaliteit (bijvoorbeeld readDocuments) lezen of lezen bij de uitgifte van een SQL-query, worden de resultaten worden geretourneerd in een gesegmenteerde manier als de resultatenset is te groot is. Standaard resultaten worden geretourneerd in chunks van 100 items of 1 MB, het eerste ongeacht welke limiet wordt bereikt.

    Verminder het aantal retouren van netwerk vereist voor het ophalen van alle toepasselijke resultaten, verhoogt u de pagina databasegrootte met behulp de [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) aanvraagheader op maximaal 1000. In gevallen waarin u wilt alleen enkele resultaten weer te geven, bijvoorbeeld als uw gebruiker-interface of de toepassing de API retourneert alleen 10 een tijd resulteert, u kunt ook de paginagrootte van de op 10 te verminderen van de doorvoer die wordt gebruikt voor leesbewerkingen en query's verlagen.

    U kunt ook het formaat van de pagina met de methode setMaxItemCount instellen.

9. **Juiste Scheduler (Vermijd het stelen van Netty threads voor i/o-Event-lus) gebruiken**

    De Async Java-SDK gebruikt [netty](https://netty.io/) voor niet-blokkerende i/o. De SDK gebruikt een vast aantal i/o-netty gebeurtenis lus threads (zo veel CPU-kernen uw computer heeft) voor het uitvoeren van i/o-bewerkingen. De zichtbaar zijn geretourneerd door API verzendt het resultaat van een van de gedeelde i/o-gebeurtenis lus netty threads. Het is dus belangrijk dat u de gedeelde i/o-gebeurtenis lus netty threads worden niet geblokkeerd. CPU-intensief werk- of -bewerking op de i/o-gebeurtenis lus netty thread blokkeren kan ertoe leiden dat impasse of SDK doorvoer aanzienlijk verkorten.

    De volgende code wordt bijvoorbeeld een cpu-intensief werk in de gebeurtenis-lus i/o-netty thread uitgevoerd:

    ```java
    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribe(
      resourceResponse -> {
        //this is executed on eventloop IO netty thread.
        //the eventloop thread is shared and is meant to return back quickly.
        //
        // DON'T do this on eventloop IO netty thread.
        veryCpuIntensiveWork();
      });
    ```

    Nadat het resultaat wordt ontvangen als u wilt doen werken CPU intensieve op het resultaat Vermijd enzovoort gebeurtenis lus i/o-netty thread. U kunt uw eigen software voor uw eigen thread voor het uitvoeren van uw werk in plaats daarvan opgeven.

    ```java
    import rx.schedulers;

    Observable<ResourceResponse<Document>> createDocObs = asyncDocumentClient.createDocument(
      collectionLink, document, null, true);

    createDocObs.subscribeOn(Schedulers.computation())
    subscribe(
      resourceResponse -> {
        // this is executed on threads provided by Scheduler.computation()
        // Schedulers.computation() should be used only when:
        //   1. The work is cpu intensive 
        //   2. You are not doing blocking IO, thread sleep, etc. in this thread against other resources.
        veryCpuIntensiveWork();
      });
    ```

    Op basis van het type van uw werk moet u de juiste bestaande RxJava Scheduler gebruiken voor uw werk. Lees hier [ ``Schedulers`` ](http://reactivex.io/RxJava/1.x/javadoc/rx/schedulers/Schedulers.html).

    Voor meer informatie, bekijk de [GitHub-pagina](https://github.com/Azure/azure-cosmosdb-java) voor Async Java SDK.

10. **Netty van logboekregistratie uitschakelen** Netty bibliotheek logboekregistratie is intensieve en moet worden uitgeschakeld (onderdrukken, meld u in de configuratie mogelijk niet voldoende) om te voorkomen dat extra CPU-kosten. Als u zich niet in foutopsporingsmodus, logboekregistratie uitschakelen netty van kan worden overgeslagen. Als u log4j gebruikt voor het verwijderen van de aanvullende CPU-kosten in rekening gebracht door ``org.apache.log4j.Category.callAppenders()`` van netty kunt u de volgende regel toevoegen aan uw codebasis:

    ```java
    org.apache.log4j.Logger.getLogger("io.netty").setLevel(org.apache.log4j.Level.OFF);
    ```

11. **OS geopende bestanden resourcelimiet** sommige Linux-systemen (zoals Red Hat) hebben een bovengrens op het aantal geopende bestanden en zo het totale aantal verbindingen. Voer het volgende om de huidige limieten weer te geven:

    ```bash
    ulimit -a
    ```

    Het aantal geopende bestanden (nofile) moet groot genoeg is dat voldoende ruimte heeft voor uw geconfigureerde grootte van de verbindingsgroep en andere geopende bestanden door het besturingssysteem. Het kan worden gewijzigd om toe te staan voor een grotere grootte van de verbindingsgroep.

    Open het bestand limits.conf:

    ```bash
    vim /etc/security/limits.conf
    ```
    
    De volgende regels toevoegen/wijzigen:

    ```
    * - nofile 100000
    ```

12. **Gebruik van systeemeigen SSL-implementatie voor netty** Netty OpenSSL rechtstreeks voor SSL-implementatie stack kunt gebruiken voor betere prestaties. In de afwezigheid van deze configuratie netty wordt terugvallen op van de Java standaard SSL-implementatie.

    op Ubuntu:
    ```bash
    sudo apt-get install openssl
    sudo apt-get install libapr1
    ```

    en voeg de volgende afhankelijkheden toe aan uw project maven-afhankelijkheden:
    ```xml
    <dependency>
      <groupId>io.netty</groupId>
      <artifactId>netty-tcnative</artifactId>
      <version>2.0.7.Final</version>
      <classifier>linux-x86_64</classifier>
    </dependency>
    ```

Voor andere platforms (Red Hat, Windows, Mac, enzovoort) verwijzen naar deze instructies https://netty.io/wiki/forked-tomcat-native.html

## <a name="indexing-policy"></a>Indexeringsbeleid
 
1. **Niet-gebruikte paden uitsluiten van indexering voor snellere schrijfbewerkingen**

    Azure Cosmos-DB-indexeringsbeleid kunt u opgeven welke paden document als u wilt opnemen of uitsluiten voor indexering door gebruik te maken van paden indexeren (setIncludedPaths en setExcludedPaths). Het gebruik van het indexeren van paden kan bieden schrijven voor verbeterde prestaties en lagere indexopslag voor scenario's waarin de querypatronen vooraf, bekend als indexering kosten worden direct gecorreleerd aan het aantal unieke paden die zijn geïndexeerd. Bijvoorbeeld, toont de volgende code hoe u kunt een hele sectie van de documenten (ook wel) uitsluiten een substructuur) indexering via de "*" jokerteken.

    ```Java
    Index numberIndex = Index.Range(DataType.Number);
    numberIndex.set("precision", -1);
    indexes.add(numberIndex);
    includedPath.setIndexes(indexes);
    includedPaths.add(includedPath);
    indexingPolicy.setIncludedPaths(includedPaths);
    collectionDefinition.setIndexingPolicy(indexingPolicy);
    ```

    Zie voor meer informatie, [Azure Cosmos DB indexeringsbeleid](indexing-policies.md).

## <a name="throughput"></a>Doorvoer
<a id="measure-rus"></a>

1. **Meet en af te stemmen voor lagere aanvraag aanvraageenheden/seconde gebruik**

    Azure Cosmos DB biedt een uitgebreide set databasebewerkingen, waaronder relationele en hiërarchische query's met UDF's en opgeslagen procedures en triggers; dit alles kan worden uitgevoerd op de documenten in een databaseverzameling. De kosten die gepaard gaan met elk van deze bewerkingen varieert op basis van de CPU, IO en geheugen die nodig is om de bewerking te voltooien. In plaats van nadenken over en beheren van hardware, kunt u een aanvraageenheid (RU) zien als één maateenheid voor de resources die vereist voor het uitvoeren van verschillende databasebewerkingen en een toepassingsaanvraag indienen.

    Doorvoer is ingericht op basis van het aantal [aanvraageenheden](request-units.md) instellen voor elke container. Aanvraag eenheidsverbruik wordt geëvalueerd als een tarief per seconde. Toepassingen die groter zijn dan de snelheid van de eenheid ingerichte aanvragen voor de container zijn beperkt tot het percentage lager is dan het niveau van de ingerichte voor de container. Als uw toepassing een grotere doorvoer nodig is, kunt u uw doorvoer verhogen door in te richten extra aanvraageenheden.

    De complexiteit van een query heeft gevolgen voor het aantal aanvraageenheden voor een bewerking worden verbruikt. Het aantal predikaten, aard van de predikaten, aantal UDF's en de grootte van de set alle gegevens van bron van invloed zijn op de kosten van querybewerkingen.

    Voor het meten van de overhead van elke bewerking (maken, bijwerken of verwijderen), Inspecteer de [x-ms-request-kosten](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) koptekst voor het meten van het aantal aanvraageenheden gebruikt door deze bewerkingen. U kunt ook zoeken op de equivalente RequestCharge-eigenschap in ResourceResponse<T> of FeedResponse<T>.

    ```Java
    ResourceResponse<Document> response = asyncClient.createDocument(collectionLink, documentDefinition, null,
                                                     false).toBlocking.single();
    response.getRequestCharge();
    ```

    De kosten van de aanvraag heeft geretourneerd als deze header is een fractie van de ingerichte doorvoer. Bijvoorbeeld, als u werkt met 2000 RU/s ingericht, en als de voorgaande query 1000 1KB-documenten retourneert, de kosten van de bewerking is 1000. Binnen één seconde houdt de server daarom slechts twee dergelijke aanvragen voordat de volgende aanvragen beperken. Zie voor meer informatie, [Aanvraageenheden](request-units.md) en de [aanvraag eenheid calculator](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Ingang snelheid beperken/snelheid van aanvragen te groot**

    Wanneer een client probeert te overschrijden de gereserveerde doorvoer voor een account, is er geen verslechtering van prestaties optreedt op de server en geen gebruik van doorvoercapaciteit buiten het niveau van de gereserveerde. De server te beëindigen van de aanvraag met RequestRateTooLarge (HTTP-statuscode 429) en retourneren de [x-ms-nieuwe poging-na-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) header die aangeeft van de hoeveelheid tijd in milliseconden, dat de gebruiker voordat het opnieuw proberen wachten moet de aanvraag.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    De SDK's impliciet alle catch dit antwoord, aansluiten bij de server opgegeven opnieuw proberen na de header en probeer de aanvraag. Als uw account wordt door meerdere clients tegelijkertijd wordt geopend, wordt de volgende poging slaagt.

    Hebt u meer dan één client cumulatief werken consistent boven de snelheid van aanvragen, het aantal nieuwe pogingen van standaard momenteel ingesteld op 9 intern door de client niet toereikend zijn; in dit geval, genereert de client een DocumentClientException met de statuscode 429 naar de toepassing. Het standaardaantal-nieuwe pogingen kan worden gewijzigd met behulp van setRetryOptions op de ConnectionPolicy-exemplaar. Standaard wordt de DocumentClientException met de statuscode 429 geretourneerd na een cumulatieve wachttijd van 30 seconden als de aanvraag worden voortgezet boven de snelheid van aanvragen. Dit gebeurt zelfs als het huidige aantal nieuwe pogingen is kleiner dan het maximale aantal, worden deze de standaardwaarde van 9 of een door de gebruiker gedefinieerde waarde.

    Terwijl het gedrag voor automatische opnieuw proberen helpt om tolerantie en gebruiksgemak voor de meeste toepassingen te verbeteren, kan deze op odds zijn bij het uitvoeren van referentiepunten voor prestaties, met name bij het meten van latentie. De latentie waargenomen-client wordt oploopt als het experiment komt binnen via deze server-restrictie en zorgt ervoor dat de client-SDK op de achtergrond opnieuw uit te voeren. Om te voorkomen latentiepieken tijdens prestaties experimenten, meet de geretourneerd door elke bewerking kosten in rekening gebracht en ervoor te zorgen dat aanvragen hieronder de aanvraagsnelheid gereserveerde nog werken. Zie voor meer informatie, [Aanvraageenheden](request-units.md).
3. **Ontwerp voor een kleinere documenten voor een hogere doorvoer**

    De kosten van de aanvraag (de kosten van de verwerking van aanvragen) van een bepaalde bewerking is direct gecorreleerd met de grootte van het document. Bewerkingen voor grote documenten duurder dan bewerkingen voor kleine-documenten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het ontwerpen van uw toepassing voor schaalbaarheid en hoge prestaties, [partitioneren en schalen in Azure Cosmos DB](partition-data.md).
