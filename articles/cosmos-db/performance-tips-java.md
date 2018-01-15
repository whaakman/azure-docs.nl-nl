---
title: Azure DB Cosmos prestatietips voor Java | Microsoft Docs
description: Meer informatie over client-configuratieopties voor het verbeteren van de prestaties van Azure DB die Cosmos-database
keywords: voor de verbetering van prestaties van de database
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: dfe8f426-3c98-4edc-8094-092d41f2795e
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: mimig
ms.openlocfilehash: 4d7657d305332cc0014187d52396ae3af4818d5e
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
> [!div class="op_single_selector"]
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 
> 

# <a name="performance-tips-for-azure-cosmos-db-and-java"></a>Tips voor betere prestaties voor Azure Cosmos DB en Java
Azure Cosmos-database is een snelle en flexibele gedistribueerde database waarin naadloos met gegarandeerde latentie en doorvoer schaalt. U beschikt niet over belangrijke architectuur wijzigen of complexe code schrijven om te schalen van uw database met Azure Cosmos DB. Omhoog en omlaag schalen is net zo eenvoudig als het maken van één API-aanroep of [SDK-aanroep van methode](set-throughput.md#set-throughput-java). Omdat Azure Cosmos DB wordt benaderd via het netwerk aanroepen er zijn echter clientzijde optimalisaties voor optimale prestaties bij gebruik van kunt u de [SQL Java SDK](documentdb-sdk-java.md).

Dus als u vraagt "hoe kan ik mijn de databaseprestaties verbeteren?" Houd rekening met de volgende opties:

## <a name="networking"></a>Netwerken
<a id="direct-connection"></a>

1. **Verbindingsmodus: gebruik DirectHttps**

    Hoe een client verbinding maakt met Azure Cosmos DB heeft belangrijke gevolgen voor de prestaties, met name in termen van waargenomen-clientzijde latentie. Er is een belangrijke configuratie-instelling voor het configureren van de client beschikbare [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy) -het [ConnectionMode](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode).  De twee beschikbare ConnectionModes zijn:

   1. [Gateway (standaard)](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode.gateway)
   2. [DirectHttps](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode.directhttps)

    Gateway-modus wordt ondersteund op alle platforms van de SDK en de geconfigureerde standaardwaarde is.  Als uw toepassing wordt uitgevoerd binnen een bedrijfsnetwerk met strikte firewallbeperkingen, is de Gateway de beste keuze omdat maakt gebruik van de standaard HTTPS-poort en één eindpunt. De verhouding prestaties is echter dat Gateway modus betrekking heeft op een extra netwerk-hop telkens wanneer gegevens worden gelezen of geschreven naar Azure Cosmos DB. Als gevolg hiervan levert DirectHttps modus betere prestaties vanwege minder netwerkhops. 

    De Java SDK maakt gebruik van HTTPS als een transportprotocol. HTTPS gebruikt SSL voor eerste authenticatie en verkeer te versleutelen. Wanneer u de Java SDK, moet alleen HTTPS-poort 443 openen. 

    De ConnectionMode is tijdens het samenstellen van de DocumentClient-exemplaar met de parameter ConnectionPolicy geconfigureerd. 

    ```Java
    public ConnectionPolicy getConnectionPolicy() {
        ConnectionPolicy policy = new ConnectionPolicy();
        policy.setConnectionMode(ConnectionMode.DirectHttps);
        policy.setMaxPoolSize(1000);
        return policy;
    }
        
    ConnectionPolicy connectionPolicy = new ConnectionPolicy();
    DocumentClient client = new DocumentClient(HOST, MASTER_KEY, connectionPolicy, null);
    ```

    ![Afbeelding van het beleid van Azure DB die Cosmos-verbinding](./media/performance-tips-java/connection-policy.png)

   <a id="same-region"></a>
2. **Clients in hetzelfde Azure-regio voor prestaties plaatsen**

    Indien mogelijk, plaatst u alle toepassingen die Azure Cosmos DB aanroepen in dezelfde regio bevinden als de Azure DB die Cosmos-database. Voor een geschatte vergelijking aanroepen naar Azure Cosmos DB binnen dezelfde regio binnen 1-2 ms voltooid, maar de latentie tussen de West en oostkust van de VS is > 50 ms. Deze latentie kan waarschijnlijk uit om aan te vragen variëren, afhankelijk van de route die door de aanvraag wordt uitgevoerd het doorgeven van de client aan de grens van de Azure-datacenter. De laagst mogelijke latentie wordt bereikt door ervoor te zorgen dat de aanroepende toepassing bevindt zich in dezelfde Azure-regio als de ingerichte Azure DB die Cosmos-eindpunt. Zie voor een lijst met beschikbare regio's, [Azure-gebieden](https://azure.microsoft.com/regions/#services).

    ![Afbeelding van het beleid van Azure DB die Cosmos-verbinding](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>Gebruik van de SDK
1. **De meest recente SDK installeren**

    De Azure Cosmos DB SDK's zijn voortdurend wordt verbeterd, zodat de beste prestaties bieden. Zie de [Azure Cosmos DB SDK](documentdb-sdk-java.md) pagina's om te bepalen van de meest recente SDK en verbeteringen controleren.
2. **Een singleton Azure DB die Cosmos-client gedurende de levensduur van uw toepassing gebruiken**

    Elke [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client) exemplaar is van thread-veilige en efficiënte Verbindingsbeheer en adrescaching in directe modus wordt uitgevoerd. Zodat efficiënt Verbindingsbeheer en betere prestaties door DocumentClient verdient het gebruik van één exemplaar van de DocumentClient per AppDomain gedurende de levensduur van de toepassing.

   <a id="max-connection"></a>
3. **MaxPoolSize per host verhogen als u de Gateway-modus**

    Azure DB van de Cosmos-aanvragen worden gedaan via HTTPS/REST bij gebruik van Gateway-modus en zijn onderworpen aan de standaardlimiet verbinding per hostnaam of IP-adres. Mogelijk moet u de MaxPoolSize ingesteld op een hogere waarde (200-1000), zodat meerdere gelijktijdige verbindingen met Azure Cosmos DB van de clientbibliotheek gebruikmaken kan. In de SDK voor Java, de standaardwaarde voor [ConnectionPolicy.getMaxPoolSize](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.gsetmaxpoolsize) is 100. Gebruik [setMaxPoolSize]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.setmaxpoolsize) de waarde te wijzigen.

4. **Parallelle query's voor gepartitioneerde verzamelingen afstemmen**

    Azure Cosmos DB SQL Java SDK versie 1.9.0 en hoger ondersteuning parallelle query's waarmee u kunt een query uitvoeren op een gepartitioneerde verzameling worden parallel (Zie [werken met de SDK's](sql-api-partition-data.md#working-with-the-azure-cosmos-db-sdks) en de verwante [codevoorbeelden](https://github.com/Azure/azure-documentdb-java/tree/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples) voor meer informatie). Parallelle query's zijn ontworpen voor betere Querylatentie en doorvoer via hun seriële equivalent.

    (a) ***afstemmen setMaxDegreeOfParallelism\:***  Parallel werk query een query uitgevoerd op meerdere partities parallel. Echter worden gegevens uit een afzonderlijke gepartitioneerde verzameling opeenvolgend opgehaald met betrekking tot de query. Dus gebruiken [setMaxDegreeOfParallelism](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options.setmaxdegreeofparallelism) instellen van het aantal partities die de maximale kans op de meeste zodat query heeft opgegeven alle andere voorwaarden system hetzelfde blijven. Als u het aantal partities niet weet, kunt u setMaxDegreeOfParallelism om in te stellen van een groot aantal en het systeem kiest het minimale (aantal partities, invoer van de gebruiker opgegeven) als de maximale mate van parallelle uitvoering. 

    Het is belangrijk te weten dat parallelle query's de beste prestaties opleveren als de gegevens evenredig verdeeld over alle partities ten opzichte van de query. Als de gepartitioneerde verzameling zodanig dat alle of een meerderheid van de gegevens die door een query zijn geretourneerd voornamelijk in een paar partities (één partitie in ergste geval) en de prestaties van de query als knelpunt zou worden verwerkt door deze partities is gepartitioneerd.

    (b) ***afstemmen setMaxBufferedItemCount\:***  parallelle query is ontworpen om de resultaten vooraf ophaalt tijdens de huidige batch met resultaten wordt verwerkt door de client. Het vooraf ophalen helpt in algemene verbetering van de latentie van een query. setMaxBufferedItemCount beperkt het aantal vooraf opgehaalde resultaten. Door in te stellen [setMaxBufferedItemCount](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options.setmaxbuffereditemcount) het verwachte aantal resultaten geretourneerd (of een hogere waarde), Hierdoor is de query voor het ontvangen van maximaal profiteren van het vooraf ophalen.

    Vooraf ophalen werkt op dezelfde manier ongeacht de MaxDegreeOfParallelism en er is één buffer voor de gegevens van alle partities.  

5. **Backoff getRetryAfterInMilliseconds intervallen implementeren**

    Tijdens het testen van prestaties, moet u load verhogen tot een klein aantal aanvragen ophalen beperkt. Als beperkt, moet de clienttoepassing backoff op vertraging voor de server opgegeven interval. De backoff te respecteren, zorgt u ervoor dat u besteden aan de minimale hoeveelheid tijd wachten tussen nieuwe pogingen. Ondersteuning voor nieuwe pogingen Groepsbeleid is opgenomen in versie 1.8.0 en hoger van de [Java SDK](documentdb-sdk-java.md). Zie voor meer informatie [Exceeding gereserveerd doorvoerlimieten](request-units.md#RequestRateTooLarge) en [getRetryAfterInMilliseconds](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client_exception.getretryafterinmilliseconds).
6. **Uitbreiden van de werkbelasting van uw client**

    Als u op niveaus met hoge doorvoer testen wilt (> 50.000 RU/s), de clienttoepassing mogelijk het knelpunt als gevolg van de machine beperking af op CPU- of -gebruik. Als u deze punt bereikt, kunt u blijven voor de push-het account van Azure DB die Cosmos verder door het uitbreiden van uw clienttoepassingen op meerdere servers.

7. **Gebruik gebaseerd adressering**

    Gebruik op basis van naam adressering, waarbij koppelingen hebben de indeling `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, in plaats van SelfLinks (_self), die de indeling hebben `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` om te voorkomen dat bij het ophalen van ResourceIds van alle resources die worden gebruikt voor het opstellen van de koppeling. Ook als deze bronnen ophalen opnieuw gemaakt (mogelijk met dezelfde naam), kunt opslaan in cache deze niet.

   <a id="tune-page-size"></a>
8. **De paginagrootte van de voor query's leestijd feeds voor betere prestaties afstemmen**

    Wanneer het uitvoeren van een bulksgewijze wordt gelezen van documenten met behulp van lezen functionaliteit feed (bijvoorbeeld [readDocuments]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.readdocuments#com_microsoft_azure_documentdb__document_client_readDocuments_String_FeedOptions_c) of bij de uitgifte van een SQL-query de resultaten worden geretourneerd in een gesegmenteerde manier als de resultatenset is te groot is. Standaard resultaten worden geretourneerd in segmenten van 100 items of 1 MB, eerst de limiet is bereikt.

    Verminder het aantal netwerk retouren vereist voor het ophalen van alle toepasselijke resultaten, verhoogt u de pagina grootte met behulp van de [x-ms-max-item-aantal](https://docs.microsoft.com/rest/api/documentdb/common-documentdb-rest-request-headers) aanvraagheader en maximaal 1000. In gevallen waarin u wilt alleen enkele resultaten weer te geven bijvoorbeeld, als uw gebruikers-interface of toepassing API retourneert alleen 10 een tijd resulteert, u kunt ook de paginagrootte van de op 10 tot en met de doorvoer verbruikt voor leesbewerkingen en query's verminderen verlagen.

    U kunt ook instellen dat de pagina grootte gebruikt het de [setPageSize methode](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options_base.setpagesize#com_microsoft_azure_documentdb__feed_options_base_setPageSize_Integer).

## <a name="indexing-policy"></a>Indexeringsbeleid
 
1. **Ongebruikte paden uitsluiten van indexering voor snellere schrijfbewerkingen**

    Azure Cosmos-DB indexeringsbeleid kunt u opgeven welke paden document wilt opnemen of uitsluiten door gebruik te indexeren paden te indexeren ([setIncludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._indexing_policy.setincludedpaths) en [setExcludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._indexing_policy.setexcludedpaths)). Het gebruik van de indexering van paden kunt bieden verbeterde schrijfprestaties en lagere opslagkosten voor index voor scenario's waarin de querypatronen tevoren bekend als indexering kosten rechtstreeks worden gecorreleerd met het aantal unieke paden die zijn geïndexeerd.  De volgende code ziet u bijvoorbeeld een hele sectie van de documenten (ook uitsluiten een substructuur) vanuit indexering met de ' * ' jokerteken.

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

    Voor het meten van de overhead van bewerkingen (maken, bijwerken of verwijderen) Inspecteer de [x-ms-aanvraag-kosten](https://docs.microsoft.com/rest/api/documentdb/common-documentdb-rest-response-headers) header (of de equivalente RequestCharge-eigenschap in [ResourceResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._resource_response) of [FeedResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_response) voor het meten van het aantal aanvraageenheden verbruikt door deze bewerkingen.

    ```Java
    ResourceResponse<Document> response = client.createDocument(collectionLink, documentDefinition, null, false);

    response.getRequestCharge();
    ```             

    De aanvraag kosten geretourneerd in deze header is een fractie van de ingerichte doorvoer. Bijvoorbeeld, als er 2000 RU/s is ingericht en als de voorgaande query 1000 1KB-documenten retourneert, de kosten van de bewerking is 1000. Als zodanig binnen één seconde respecteert de-server slechts twee dergelijke aanvragen voordat de beperking van de volgende aanvragen. Zie voor meer informatie [Aanvraageenheden](request-units.md) en de [aanvraag eenheid Rekenmachine](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
2. **Verwerken frequentie beperken/aanvraagsnelheid te groot**

    Wanneer een client probeert overschrijdt de gereserveerde doorvoer voor een account, is er geen verslechtering van prestaties optreedt op de server en geen gebruik van doorvoercapaciteit afgezien van het niveau van de gereserveerde. De server wordt optie preventief beëindigen van de aanvraag met RequestRateTooLarge (HTTP-statuscode 429) en retourneert de [x-ms-opnieuw-na-ms](https://docs.microsoft.com/rest/api/documentdb/common-documentdb-rest-response-headers) header die aangeeft van de hoeveelheid tijd in milliseconden, dat de gebruiker alvorens nogmaals te proberen wachten moet de aanvraag.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    De SDK's alle impliciet dit antwoord catch, wordt de server opgegeven probeer het opnieuw nadat de header en de aanvraag opnieuw proberen. Tenzij uw account wordt door meerdere clients tegelijkertijd geopend, wordt de volgende poging slaagt.

    Als er meer dan één client cumulatief werken consistent boven het percentage aanvragen, het aantal nieuwe pogingen van standaard ingesteld op 9 intern door de client niet toereikend zijn; in dit geval wordt de client genereert een [DocumentClientException](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client_exception) met de status code 429 tot de toepassing. Het aantal standaard pogingen kan worden gewijzigd met behulp van [setRetryOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.setretryoptions) op de [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy) exemplaar. Standaard wordt de DocumentClientException met statuscode 429 na een cumulatieve wachttijd van 30 seconden geretourneerd, als de aanvraag blijft werken boven het percentage aanvragen. Dit gebeurt zelfs als het huidige aantal pogingen is kleiner dan het maximale aantal, worden deze de standaardwaarde van 9 of een door de gebruiker gedefinieerde waarde.

    Terwijl het gedrag voor het automatisch opnieuw kan worden verbeterd tolerantie en bruikbaarheid voor de meeste toepassingen, kan deze op odds zijn bij het uitvoeren van prestatiebenchmarks, vooral wanneer latentie meten.  De latentie waargenomen-client wordt pieken als het experiment treffers in de beperking van de server en zorgt ervoor dat de client SDK achtergrond opnieuw proberen. Meten van de kosten die zijn geretourneerd door elke bewerking om te voorkomen latentiepieken tijdens prestaties experimenten, en zorg ervoor dat aanvragen worden gebruikt onder het gereserveerde percentage. Zie voor meer informatie [Aanvraageenheden](request-units.md).
3. **Ontwerp voor kleinere documenten voor een hogere doorvoer**

    De kosten van de aanvraag (de kosten van de verwerking van aanvragen) van een bepaalde bewerking wordt rechtstreeks gecorreleerd met de grootte van het document. Bewerkingen op grote documenten duurder dan bewerkingen voor kleine documenten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het ontwerpen van uw aanvraag voor schaal en hoge prestaties, [partitionering en schalen in Azure Cosmos DB](partition-data.md).
