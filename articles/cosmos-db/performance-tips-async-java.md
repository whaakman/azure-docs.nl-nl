---
title: Azure DB Cosmos prestatietips voor asynchrone Java | Microsoft Docs
description: Meer informatie over client-configuratieopties voor het verbeteren van de prestaties van Azure DB die Cosmos-database
keywords: voor de verbetering van prestaties van de database
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: dfe8f426-3c98-4edc-8094-092d41f2795e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2018
ms.author: sngun
ms.openlocfilehash: 95f6e3d6d9db5a88b5b974daf6e36573b60878a5
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/20/2018
---
> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 
> 

# <a name="performance-tips-for-azure-cosmos-db-and-async-java"></a>Tips voor betere prestaties voor Azure Cosmos DB en asynchrone Java
Azure Cosmos-database is een snelle en flexibele gedistribueerde database waarin naadloos met gegarandeerde latentie en doorvoer schaalt. U beschikt niet over belangrijke architectuur wijzigen of complexe code schrijven om te schalen van uw database met Azure Cosmos DB. Omhoog en omlaag schalen is net zo eenvoudig als het doorvoeren van een één API-aanroep of de SDK-aanroep van methode. Omdat Azure Cosmos DB wordt benaderd via het netwerk aanroepen er zijn echter clientzijde optimalisaties voor optimale prestaties bij gebruik van kunt u de [SQL Async Java SDK](sql-api-sdk-async-java.md).

Dus als u vraagt "hoe kan ik mijn de databaseprestaties verbeteren?" Houd rekening met de volgende opties:

## <a name="networking"></a>Netwerken
   <a id="same-region"></a>
1. **Clients in hetzelfde Azure-regio voor prestaties plaatsen**

    Indien mogelijk, plaatst u alle toepassingen die Azure Cosmos DB aanroepen in dezelfde regio bevinden als de Azure DB die Cosmos-database. Voor een geschatte vergelijking aanroepen naar Azure Cosmos DB binnen dezelfde regio binnen 1-2 ms voltooid, maar de latentie tussen de West en oostkust van de VS is > 50 ms. Deze latentie kan waarschijnlijk uit om aan te vragen variëren, afhankelijk van de route die door de aanvraag wordt uitgevoerd het doorgeven van de client aan de grens van de Azure-datacenter. De laagst mogelijke latentie wordt bereikt door ervoor te zorgen dat de aanroepende toepassing bevindt zich in dezelfde Azure-regio als de ingerichte Azure DB die Cosmos-eindpunt. Zie voor een lijst met beschikbare regio's, [Azure-gebieden](https://azure.microsoft.com/regions/#services).

    ![Afbeelding van het beleid van Azure DB die Cosmos-verbinding](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>Gebruik van de SDK
1. **De meest recente SDK installeren**

    De Azure Cosmos DB SDK's zijn voortdurend wordt verbeterd, zodat de beste prestaties bieden. Zie de [Azure Cosmos DB SDK](sql-api-sdk-async-java.md) pagina's om te bepalen van de meest recente SDK en verbeteringen controleren.
2. **Een singleton Azure DB die Cosmos-client gedurende de levensduur van uw toepassing gebruiken**

    Elke AsyncDocumentClient-exemplaar is van thread-veilige en efficiënte Verbindingsbeheer en adrescaching wordt uitgevoerd. Als u wilt toestaan efficiënt Verbindingsbeheer en betere prestaties door AsyncDocumentClient, verdient het met één exemplaar van AsyncDocumentClient per AppDomain voor de levensduur van de toepassing.

   <a id="max-connection"></a>

3. **ConnectionPolicy afstemmen**

    Azure DB van de Cosmos-aanvragen worden gedaan via HTTPS/REST bij gebruik van de asynchrone Java SDK en zijn onderworpen aan de standaard maximale grootte van de beheerverbindingsgroep (1000). Deze waarde moet ideaal voor de meeste gevallen zijn. Als u een zeer grote verzameling met veel partities hebt, kunt u de maximale grootte van de beheerverbindingsgroep instellen op een groter aantal (bijvoorbeeld 1500) met behulp van setMaxPoolSize.

4. **Parallelle query's voor gepartitioneerde verzamelingen afstemmen**

    Azure Cosmos DB SQL Async Java SDK biedt ondersteuning voor parallelle query's waarmee u kunt een query uitvoeren op een gepartitioneerde verzameling worden parallel (Zie [werken met de SDK's](sql-api-partition-data.md#working-with-the-azure-cosmos-db-sdks) en de verwante [codevoorbeelden](https://github.com/Azure/azure-cosmosdb-java/tree/master/examples/src/test/java/com/microsoft/azure/cosmosdb/rx/examples) voor meer informatie). Parallelle query's zijn ontworpen voor betere Querylatentie en doorvoer via hun seriële equivalent.

    (a) ***afstemmen setMaxDegreeOfParallelism\:***  Parallel werk query een query uitgevoerd op meerdere partities parallel. Echter worden gegevens uit een afzonderlijke gepartitioneerde verzameling opeenvolgend opgehaald met betrekking tot de query. Gebruik setMaxDegreeOfParallelism instellen van het aantal partities met de maximale kans op de meeste query zodat alle andere system voorwaarden opgegeven blijven dus hetzelfde. Als u het aantal partities niet weet, kunt u setMaxDegreeOfParallelism om in te stellen van een groot aantal en het systeem kiest het minimale (aantal partities, invoer van de gebruiker opgegeven) als de maximale mate van parallelle uitvoering. 

    Het is belangrijk te weten dat parallelle query's de beste prestaties opleveren als de gegevens evenredig verdeeld over alle partities ten opzichte van de query. Als de gepartitioneerde verzameling zodanig dat alle of een meerderheid van de gegevens die door een query zijn geretourneerd voornamelijk in een paar partities (één partitie in ergste geval) en de prestaties van de query als knelpunt zou worden verwerkt door deze partities is gepartitioneerd.

    (b) ***afstemmen setMaxBufferedItemCount\:***  parallelle query is ontworpen om de resultaten vooraf ophaalt tijdens de huidige batch met resultaten wordt verwerkt door de client. Het vooraf ophalen helpt in algemene verbetering van de latentie van een query. setMaxBufferedItemCount beperkt het aantal vooraf opgehaalde resultaten. Als u setMaxBufferedItemCount met het verwachte aantal resultaten geretourneerd (of een hogere waarde), kunt de query voor het ontvangen van maximaal profiteren van het vooraf ophalen.

    Vooraf ophalen werkt op dezelfde manier ongeacht de MaxDegreeOfParallelism en er is één buffer voor de gegevens van alle partities.  

5. **Backoff getRetryAfterInMilliseconds intervallen implementeren**

    Tijdens het testen van prestaties, moet u load verhogen tot een klein aantal aanvragen ophalen beperkt. Als beperkt, moet de clienttoepassing backoff voor de server opgegeven interval. De backoff te respecteren, zorgt u ervoor dat u besteden aan de minimale hoeveelheid tijd wachten tussen nieuwe pogingen. Zie voor meer informatie [Exceeding gereserveerd doorvoerlimieten](request-units.md#RequestRateTooLarge) en DocumentClientException.getRetryAfterInMilliseconds.
6. **Uitbreiden van de werkbelasting van uw client**

    Als u op niveaus met hoge doorvoer testen wilt (> 50.000 RU/s), de clienttoepassing mogelijk het knelpunt als gevolg van de machine beperking af op CPU- of -gebruik. Als u deze punt bereikt, kunt u blijven voor de push-het account van Azure DB die Cosmos verder door het uitbreiden van uw clienttoepassingen op meerdere servers.

7. **Gebruik gebaseerd adressering**

    Gebruik op basis van naam adressering, waarbij koppelingen hebben de indeling `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, in plaats van SelfLinks (\_self), die de indeling hebben `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` om te voorkomen dat bij het ophalen van ResourceIds van alle resources die worden gebruikt voor het opstellen van de koppeling. Ook als deze bronnen ophalen opnieuw gemaakt (mogelijk met dezelfde naam), kunt opslaan in cache ze niet.

   <a id="tune-page-size"></a>
8. **De paginagrootte van de voor query's leestijd feeds voor betere prestaties afstemmen**

    Bij het uitvoeren van een bulksgewijs documenten met behulp van de feed functionaliteit (bijvoorbeeld readDocuments) lezen of lezen bij de uitgifte van een SQL-query, worden de resultaten in een gesegmenteerde manier geretourneerd als de resultatenset is te groot is. Standaard resultaten worden geretourneerd in segmenten van 100 items of 1 MB, eerst de limiet is bereikt.

    Verminder het aantal netwerk retouren vereist voor het ophalen van alle toepasselijke resultaten, verhoogt u de pagina grootte met behulp van de [x-ms-max-item-aantal](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) aanvraagheader en maximaal 1000. In gevallen waarin u wilt alleen enkele resultaten weer te geven bijvoorbeeld, als uw gebruikers-interface of toepassing API retourneert alleen 10 een tijd resulteert, u kunt ook de paginagrootte van de op 10 tot en met de doorvoer verbruikt voor leesbewerkingen en query's verminderen verlagen.

    U kunt ook de paginagrootte via de methode setMaxItemCount instellen.
    
9. **Juiste Scheduler (Vermijd stelen Eventloop IO Netty threads) gebruiken**

    De asynchrone Java SDK gebruikt [netty](https://netty.io/) voor niet-blokkerende IO. De SDK gebruikt een vast aantal i/o netty eventloop threads (zo veel CPU-kernen op de computer is) voor het uitvoeren van i/o-bewerkingen. De opslagtijd geretourneerd door de API verzendt het resultaat van een van de gedeelde IO eventloop netty threads. Het is daarom belangrijk dat u de gedeelde netty threads van de i/o-eventloop niet blokkeren. CPU-intensieve werk of blokkeren van de bewerking op de i/o eventloop netty thread kan impasse veroorzaken of SDK doorvoer aanzienlijk verminderen.

    De volgende code wordt een intensieve cpu-werk bijvoorbeeld uitgevoerd in de netty eventloop i/o-thread:

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

    Nadat resultaat is ontvangen als u wilt doen, CPU-intensieve werkt op het resultaat Vermijd netty enzovoort eventloop i/o-thread. U kunt uw eigen Scheduler om uw eigen thread voor het uitvoeren van uw werk in plaats daarvan opgeven.

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

    Op basis van het type van uw werk moet u de juiste bestaande RxJava planner voor uw werk gebruiken. Lees hier [ ``Schedulers`` ](http://reactivex.io/RxJava/1.x/javadoc/rx/schedulers/Schedulers.html).

    Voor meer informatie, Zie de [Github-pagina](https://github.com/Azure/azure-cosmosdb-java) voor asynchrone Java SDK.

10. **Netty van logboekregistratie uitschakelen** Netty bibliotheek-logboekregistratie is chatty en moet worden uitgeschakeld (het onderdrukken van het logboek in de configuratie mogelijk niet voldoende) om te voorkomen dat extra CPU-kosten. Als u zich niet in de foutopsporingsmodus, logboekfunctie helemaal uitschakelen netty van. Als u log4j gebruikt voor het verwijderen van de aanvullende CPU kosten van ``org.apache.log4j.Category.callAppenders()`` van netty kunt u de volgende regel toevoegen aan uw codebasis:

    ```java
    org.apache.log4j.Logger.getLogger("io.netty").setLevel(org.apache.log4j.Level.OFF);
    ```

11. **OS geopende bestanden Resource limiet** Some Linux-systemen (zoals Redhat) een bovenlimiet hebben op het aantal geopende bestanden en kan dus het totale aantal verbindingen. Voer het volgende om de huidige limieten weer te geven:

    ```bash
    ulimit -a
    ```

    Het aantal geopende bestanden (nofile) moet groot genoeg zijn om er voldoende ruimte voor de geconfigureerde grootte van de beheerverbindingsgroep en andere geopende bestanden door het besturingssysteem. Het kan worden gewijzigd als u wilt toestaan voor een grotere grootte van de beheerverbindingsgroep.

    Open het bestand limits.conf:

    ```bash
    vim /etc/security/limits.conf
    ```
    
    De volgende regels toevoegen/wijzigen:

    ```
    * - nofile 100000
    ```

12. **Systeemeigen SSL-implementatie gebruiken voor netty** Netty OpenSSL rechtstreeks voor SSL-implementatie stack kunt gebruiken voor betere prestaties bereiken. Bij gebrek aan dit configuratie-netty wordt terugvallen op Java van standaard SSL-implementatie.

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

Verwijzen naar deze instructies voor andere platforms (Redhat, Windows, Mac, enz.) https://netty.io/wiki/forked-tomcat-native.html

## <a name="indexing-policy"></a>Indexeringsbeleid
 
1. **Ongebruikte paden uitsluiten van indexering voor snellere schrijfbewerkingen**

    Azure Cosmos-DB indexeringsbeleid kunt u opgeven welke paden document wilt opnemen of uitsluiten door gebruik te indexeren paden (setIncludedPaths en setExcludedPaths) te indexeren. Het gebruik van de indexering van paden kunt bieden verbeterde schrijfprestaties en lagere opslagkosten voor index voor scenario's waarin de querypatronen tevoren bekend als indexering kosten rechtstreeks worden gecorreleerd met het aantal unieke paden die zijn geïndexeerd.  De volgende code ziet u bijvoorbeeld een hele sectie van de documenten (ook uitsluiten een substructuur) vanuit indexering met de ' * ' jokerteken.

    ```Java
    Index numberIndex = Index.Range(DataType.Number);
    numberIndex.set("precision", -1);
    indexes.add(numberIndex);
    includedPath.setIndexes(indexes);
    includedPaths.add(includedPath);
    indexingPolicy.setIncludedPaths(includedPaths);
    collectionDefinition.setIndexingPolicy(indexingPolicy);
    ```

    Zie voor meer informatie [Azure Cosmos DB indexeren beleid](indexing-policies.md).

## <a name="throughput"></a>Doorvoer
<a id="measure-rus"></a>

1. **Meten en stemmen voor lagere aanvraag eenheden/seconde gebruik**

    Azure Cosmos-DB biedt een uitgebreide reeks databasebewerkingen, met inbegrip van relationele en hiërarchische query's met UDF's, opgeslagen procedures en triggers – alle operationele op de documenten binnen een verzameling van de database. De kosten die zijn gekoppeld aan elk van deze bewerkingen varieert op basis van de CPU, IO en geheugen dat nodig is om de bewerking te voltooien. In plaats van het denken over en beheer van hardwareresources, kunt u een aanvraag-eenheid (RU) beschouwen als een enkele meting voor de resources die zijn vereist voor het uitvoeren van verschillende databasebewerkingen en de aanvraag voor een toepassing.

    Doorvoer is ingericht op basis van het aantal [aanvraageenheden](request-units.md) instellen voor elke container. Aanvraag eenheidsverbruik wordt geëvalueerd als een percentage per seconde. Toepassingen die groter is dan de frequentie van de eenheid ingerichte aanvraag voor hun container zijn beperkt tot de snelheid onder het ingerichte niveau voor de container zakt. Als uw toepassing een hogere mate van doorvoer vereist, kunt u uw doorvoer verhogen door extra aanvraageenheden provisioning. 

    De complexiteit van een query heeft gevolgen voor het aantal aanvraageenheden voor een bewerking worden verbruikt. Het aantal predicaten, aard van de predicaten, aantal UDF's en de grootte van de bron-gegevensset alle invloed hebben op de kosten van querybewerkingen.

    Voor het meten van de overhead van bewerkingen (maken, bijwerken of verwijderen) Inspecteer de [x-ms-aanvraag-kosten](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) header voor het meten van het aantal aanvraageenheden verbruikt door deze bewerkingen. U kunt ook zoeken op de equivalente RequestCharge-eigenschap in ResourceResponse<T> of FeedResponse<T>.

    ```Java
    ResourceResponse<Document> response = asyncClient.createDocument(collectionLink, documentDefinition, null,
                                                     false).toBlocking.single();
    response.getRequestCharge();
    ```             

    De aanvraag kosten geretourneerd in deze header is een fractie van de ingerichte doorvoer. Bijvoorbeeld, als er 2000 RU/s is ingericht en als de voorgaande query 1000 1KB-documenten retourneert, de kosten van de bewerking is 1000. Als zodanig binnen één seconde respecteert de-server slechts twee dergelijke aanvragen voordat de beperking van de volgende aanvragen. Zie voor meer informatie [Aanvraageenheden](request-units.md) en de [aanvraag eenheid Rekenmachine](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Verwerken frequentie beperken/aanvraagsnelheid te groot**

    Wanneer een client probeert overschrijdt de gereserveerde doorvoer voor een account, is er geen verslechtering van prestaties optreedt op de server en geen gebruik van doorvoercapaciteit afgezien van het niveau van de gereserveerde. De server wordt optie preventief beëindigen van de aanvraag met RequestRateTooLarge (HTTP-statuscode 429) en retourneert de [x-ms-opnieuw-na-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) header die aangeeft van de hoeveelheid tijd in milliseconden, dat de gebruiker alvorens nogmaals te proberen wachten moet de aanvraag.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    De SDK's alle impliciet dit antwoord catch, wordt de server opgegeven probeer het opnieuw nadat de header en de aanvraag opnieuw proberen. Tenzij uw account wordt door meerdere clients tegelijkertijd geopend, wordt de volgende poging slaagt.

    Als er meer dan één client cumulatief werken consistent boven het percentage aanvragen, het aantal nieuwe pogingen van standaard ingesteld op 9 intern door de client niet toereikend zijn; in dit geval, genereert de client een DocumentClientException met statuscode 429 tot de toepassing. Het aantal standaard pogingen kan worden gewijzigd met behulp van setRetryOptions op het exemplaar ConnectionPolicy. Standaard wordt de DocumentClientException met statuscode 429 na een cumulatieve wachttijd van 30 seconden geretourneerd, als de aanvraag blijft werken boven het percentage aanvragen. Dit gebeurt zelfs als het huidige aantal pogingen is kleiner dan het maximale aantal, worden deze de standaardwaarde van 9 of een door de gebruiker gedefinieerde waarde.

    Terwijl het gedrag voor het automatisch opnieuw kan worden verbeterd tolerantie en bruikbaarheid voor de meeste toepassingen, kan deze op odds zijn bij het uitvoeren van prestatiebenchmarks, vooral wanneer latentie meten.  De latentie waargenomen-client wordt pieken als het experiment treffers in de beperking van de server en zorgt ervoor dat de client SDK achtergrond opnieuw proberen. Meten van de kosten die zijn geretourneerd door elke bewerking om te voorkomen latentiepieken tijdens prestaties experimenten, en zorg ervoor dat aanvragen worden gebruikt onder het gereserveerde percentage. Zie voor meer informatie [Aanvraageenheden](request-units.md).
3. **Ontwerp voor kleinere documenten voor een hogere doorvoer**

    De kosten van de aanvraag (de kosten van de verwerking van aanvragen) van een bepaalde bewerking wordt rechtstreeks gecorreleerd met de grootte van het document. Bewerkingen op grote documenten duurder dan bewerkingen voor kleine documenten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het ontwerpen van uw aanvraag voor schaal en hoge prestaties, [partitionering en schalen in Azure Cosmos DB](partition-data.md).
