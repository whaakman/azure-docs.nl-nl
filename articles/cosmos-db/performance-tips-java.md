---
title: Azure Cosmos DB tips voor betere prestaties voor Java
description: Informatie over client-configuratieopties voor het verbeteren van de prestaties van de Azure Cosmos DB-database
keywords: over het verbeteren van de prestaties van de database
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 01/02/2018
ms.author: sngun
ms.openlocfilehash: afbeb8211baea6da363a5e0162e92a0588581346
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52872982"
---
# <a name="performance-tips-for-azure-cosmos-db-and-java"></a>Tips voor betere prestaties voor Azure Cosmos DB en Java

> [!div class="op_single_selector"]
> * [Async Java](performance-tips-async-java.md)
> * [Java](performance-tips-java.md)
> * [.NET](performance-tips.md)
> 

Azure Cosmos DB is een snelle en flexibele gedistribueerde database die kan worden opgeschaald naadloos met een gegarandeerde latentie en doorvoer. U hoeft niet te grote architectuur wijzigingen aanbrengen of complexe code schrijven om te schalen van uw database met Azure Cosmos DB. Omhoog en omlaag schalen is net zo gemakkelijk als het maken van één API-aanroep. Zie voor meer informatie, [over het inrichten van containerdoorvoer](how-to-provision-container-throughput.md) of [over het inrichten van database doorvoer](how-to-provision-database-throughput.md). Omdat Azure Cosmos DB is toegankelijk via netwerkaanroepen er zijn echter client-side '-optimalisatie u maken kunt voor het behalen van optimale prestaties bij het gebruik van de [SQL Java SDK](documentdb-sdk-java.md).

Dus als u vraagt "hoe kan ik mijn de databaseprestaties verbeteren?" Houd rekening met de volgende opties:

## <a name="networking"></a>Netwerken
<a id="direct-connection"></a>

1. **Verbindingsmodus: gebruik DirectHttps**

    Hoe een client verbinding maakt met Azure Cosmos DB heeft belangrijke gevolgen voor de prestaties, met name wat betreft waargenomen client-side-latentie. Er is een belangrijke configuratie-instelling beschikbaar voor het configureren van de client [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy) : de [ConnectionMode](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode).  De twee beschikbare ConnectionModes zijn:

   1. [Gateway (standaard)](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode)
   2. [DirectHttps](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_mode)

    Gateway-modus wordt ondersteund op alle platforms van de SDK en de geconfigureerde standaardwaarde is.  Als uw toepassing wordt uitgevoerd vanuit een bedrijfsnetwerk met strikte firewallbeperkingen, is Gateway de beste keuze, omdat deze de standaard HTTPS-poort en één eindpunt gebruikt. De negatieve gevolgen voor de prestaties, is echter dat modus van de Gateway bestaat uit een aanvullend netwerk hop telkens wanneer gegevens worden gelezen of geschreven naar Azure Cosmos DB. Als gevolg hiervan biedt DirectHttps modus betere prestaties vanwege minder netwerkhops. 

    De Java-SDK maakt gebruik van HTTPS als een transportprotocol. HTTPS gebruikt SSL voor de initiële verificatie en het versleutelen van verkeer. Wanneer u de Java-SDK gebruikt, moet alleen HTTPS-poort 443 geopend. 

    De ConnectionMode is geconfigureerd tijdens het samenstellen van het DocumentClient-exemplaar met de ConnectionPolicy-parameter. 

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

    ![Afbeelding van het beleid voor Azure Cosmos DB-verbinding](./media/performance-tips-java/connection-policy.png)

   <a id="same-region"></a>
2. **Clients in hetzelfde Azure-regio voor de prestaties plaatsen**

    Indien mogelijk, plaatst u alle toepassingen aanroepen van Azure Cosmos DB in dezelfde regio als de Azure Cosmos DB-database. Voor een geschatte vergelijking: aanroepen naar Azure Cosmos DB binnen dezelfde regio binnen 1-2 ms voltooid, maar de latentie tussen de West- en oostkust van de Verenigde Staten is > 50 ms. Deze latentie kan waarschijnlijk uit om aan te vragen variëren, afhankelijk van de route die door de aanvraag is uitgevoerd, zoals het aan de grens van de Azure-datacenter van de client doorgeeft. De laagst mogelijke latentie wordt bereikt door ervoor te zorgen dat de aanroepende toepassing bevindt zich in dezelfde Azure-regio als de ingerichte Azure Cosmos DB-eindpunt. Zie voor een lijst van beschikbare regio's, [Azure-regio's](https://azure.microsoft.com/regions/#services).

    ![Afbeelding van het beleid voor Azure Cosmos DB-verbinding](./media/performance-tips/same-region.png)
   
## <a name="sdk-usage"></a>SDK-gebruik
1. **Installeer de meest recente SDK**

    De Azure Cosmos DB SDK's worden voortdurend verbeterd om de beste prestaties bieden. Zie de [Azure Cosmos DB SDK](documentdb-sdk-java.md) pagina's om te bepalen van de meest recente SDK en verbeteringen te bekijken.
2. **Een singleton-Azure Cosmos DB-client gebruiken voor de levensduur van uw toepassing**

    Elke [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client) exemplaar thread-veilig is en efficiënt verbinding management en adrescaching als het wordt uitgevoerd in de directe modus uitgevoerd. Om toe te staan efficiënt beheer en betere prestaties door DocumentClient, is het aanbevolen met één exemplaar van de DocumentClient per toepassingsdomein voor de levensduur van de toepassing.

   <a id="max-connection"></a>
3. **MaxPoolSize per host verhogen als u de Gateway gebruikt**

    Azure Cosmos DB-aanvragen via HTTPS/REST worden gemaakt wanneer u de Gateway gebruikt, en zijn onderworpen aan de standaardlimiet voor de verbinding per hostnaam of IP-adres. Mogelijk moet u de MaxPoolSize ingesteld op een hogere waarde (200-1000), zodat de clientbibliotheek van meerdere gelijktijdige verbindingen met Azure Cosmos DB gebruikmaken kan. In de Java-SDK, de standaardwaarde voor [ConnectionPolicy.getMaxPoolSize](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.getmaxpoolsize) is 100. Gebruik [setMaxPoolSize]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.setmaxpoolsize) de waarde te wijzigen.

4. **Parallelle query's voor gepartitioneerde verzamelingen afstemmen**

    Azure Cosmos DB SQL Java SDK versie 1.9.0 en hoger ondersteuning parallelle query's, zodat ze een gepartitioneerde verzameling worden parallel query. Zie voor meer informatie, [codevoorbeelden](https://github.com/Azure/azure-documentdb-java/tree/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples) met betrekking tot het werken met de SDK's. Parallelle query's zijn ontworpen voor betere latentie van query en de doorvoer via de seriële equivalent.

    (a) ***afstemmen setMaxDegreeOfParallelism\:***  parallelle query's werken door meerdere partities parallel uitvoeren van query's. Gegevens van een afzonderlijke gepartitioneerde verzameling is echter worden opgehaald met betrekking tot de query. Gebruik daarom, [setMaxDegreeOfParallelism](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options.setmaxdegreeofparallelism) opgegeven om in te stellen van het aantal partities waarvoor de maximale kans dat het bereiken van de meeste goed presterende query's en alle andere system omstandigheden gelijk blijven. Als u het aantal partities niet weet, kunt u setMaxDegreeOfParallelism om in te stellen van een groot aantal en het systeem kiest de minimale (het aantal partities, door de gebruiker opgegeven invoer) als de maximale graad van parallelle uitvoering. 

    Het is belangrijk te weten dat de parallelle query's de beste prestaties opleveren als de gegevens gelijkmatig wordt verdeeld over alle partities met betrekking tot de query. Als de gepartitioneerde verzameling zodanig dat alle of een meerderheid van de gegevens die zijn geretourneerd door een query is geconcentreerd in een paar partities (één partitie in het ergste geval) en vervolgens op de prestaties van de query zou worden knelpunt door deze partities is gepartitioneerd.

    (b) ***afstemmen setMaxBufferedItemCount\:***  parallelle query is ontworpen voor het vooraf ophalen van resultaten terwijl de huidige batch van de resultaten wordt verwerkt door de client. De vooraf ophalen helpt bij het algemene verbetering van de latentie van een query. setMaxBufferedItemCount beperkt het aantal vooraf opgehaalde resultaten. Door in te stellen [setMaxBufferedItemCount](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options.setmaxbuffereditemcount) naar het verwachte aantal geretourneerde resultaten (of een hogere waarde), Hierdoor is de query voor het ontvangen van maximaal profiteren van het vooraf ophalen.

    Op dezelfde manier, ongeacht de MaxDegreeOfParallelism vooraf ophalen werkt en er is één buffer voor de gegevens van alle partities.  

5. **Uitstel getRetryAfterInMilliseconds tussenpozen implementeren**

    Tijdens het Prestatietesten van, moet u load vergroten totdat een klein aantal aanvragen te maken met beperkingen. Als beperkt, moet de clienttoepassing uitstel op vertraging voor de server opgegeven interval. Respecteer de uitstel zorgt ervoor dat u besteden aan de minimale hoeveelheid tijd wachten tussen nieuwe pogingen. Ondersteuning voor beleid voor nieuwe pogingen is opgenomen in versie 1.8.0 en hoger van de [Java SDK](documentdb-sdk-java.md). Zie voor meer informatie, [getRetryAfterInMilliseconds](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client_exception.getretryafterinmilliseconds).

6. **Uw client-workload uitschalen**

    Als u op het niveau van hoge doorvoer testen wilt (> 50.000 RU/s), de clienttoepassing kan knelpunt vanwege de machine beperking af op CPU- of -gebruik. Als u dit punt bereikt, kunt u blijven om de Azure Cosmos DB-account verder door uw client-toepassingen schalen over meerdere servers.

7. **Gebruik op basis van naam-adressering**

    Gebruik op basis van naam-adressering, waarbij koppelingen hebben de indeling `dbs/MyDatabaseId/colls/MyCollectionId/docs/MyDocumentId`, in plaats van SelfLinks (\_zelf), hebben de indeling `dbs/<database_rid>/colls/<collection_rid>/docs/<document_rid>` om te voorkomen dat bij het ophalen van ResourceIds van alle resources die worden gebruikt voor het maken van de koppeling. Ook als deze resources ophalen opnieuw gemaakt (mogelijk met dezelfde naam), opslaan in cache deze mogelijk niet helpen.

   <a id="tune-page-size"></a>
8. **De paginagrootte voor query's / lezen-feeds voor betere prestaties afstemmen**

    Wanneer het uitvoeren van een bulksgewijze wordt gelezen van documenten met behulp van lezen functionaliteit feed (bijvoorbeeld [readDocuments]( https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.readdocuments#com_microsoft_azure_documentdb__document_client_readDocuments_String_FeedOptions_c) of bij de uitgifte van een SQL-query, de resultaten worden geretourneerd in een gesegmenteerde manier als de resultatenset is te groot is. Standaard resultaten worden geretourneerd in chunks van 100 items of 1 MB, het eerste ongeacht welke limiet wordt bereikt.

    Verminder het aantal retouren van netwerk vereist voor het ophalen van alle toepasselijke resultaten, verhoogt u de pagina databasegrootte met behulp de [x-ms-max-item-count](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-request-headers) aanvraagheader op maximaal 1000. In gevallen waarin u wilt alleen enkele resultaten weer te geven, bijvoorbeeld als uw gebruiker-interface of de toepassing de API retourneert alleen 10 een tijd resulteert, u kunt ook de paginagrootte van de op 10 te verminderen van de doorvoer die wordt gebruikt voor leesbewerkingen en query's verlagen.

    U kunt ook instellen dat de pagina grootte met behulp van de [setPageSize methode](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_options_base.setpagesize#com_microsoft_azure_documentdb__feed_options_base_setPageSize_Integer).

## <a name="indexing-policy"></a>Indexeringsbeleid
 
1. **Niet-gebruikte paden uitsluiten van indexering voor snellere schrijfbewerkingen**

    Azure Cosmos-DB-indexeringsbeleid kunt u opgeven welke paden document als u wilt opnemen of uitsluiten voor indexering door gebruik te maken van paden indexeren ([setIncludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._indexing_policy.setincludedpaths) en [setExcludedPaths](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._indexing_policy.setexcludedpaths)). Het gebruik van het indexeren van paden kan bieden schrijven voor verbeterde prestaties en lagere indexopslag voor scenario's waarin de querypatronen vooraf, bekend als indexering kosten worden direct gecorreleerd aan het aantal unieke paden die zijn geïndexeerd.  Bijvoorbeeld, toont de volgende code hoe u kunt een hele sectie van de documenten (ook wel) uitsluiten een substructuur) indexering via de "*" jokerteken.

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

    Voor het meten van de overhead van elke bewerking (maken, bijwerken of verwijderen), Inspecteer de [x-ms-request-kosten](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) header (of de equivalente RequestCharge-eigenschap in [ResourceResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._resource_response) of [FeedResponse<T> ](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._feed_response) voor het meten van het aantal aanvraageenheden gebruikt door deze bewerkingen.

    ```Java
    ResourceResponse<Document> response = client.createDocument(collectionLink, documentDefinition, null, false);

    response.getRequestCharge();
    ```             

    De kosten van de aanvraag heeft geretourneerd als deze header is een fractie van de ingerichte doorvoer. Bijvoorbeeld, als u werkt met 2000 RU/s ingericht, en als de voorgaande query 1000 1KB-documenten retourneert, de kosten van de bewerking is 1000. Binnen één seconde houdt de server daarom slechts twee dergelijke aanvragen voordat de volgende aanvragen beperken. Zie voor meer informatie, [Aanvraageenheden](request-units.md) en de [aanvraag eenheid calculator](https://www.documentdb.com/capacityplanner).
<a id="429"></a>
1. **Ingang snelheid beperken/snelheid van aanvragen te groot**

    Wanneer een client probeert te overschrijden de gereserveerde doorvoer voor een account, is er geen verslechtering van prestaties optreedt op de server en geen gebruik van doorvoercapaciteit buiten het niveau van de gereserveerde. De server te beëindigen van de aanvraag met RequestRateTooLarge (HTTP-statuscode 429) en retourneren de [x-ms-nieuwe poging-na-ms](https://docs.microsoft.com/rest/api/cosmos-db/common-cosmosdb-rest-response-headers) header die aangeeft van de hoeveelheid tijd in milliseconden, dat de gebruiker voordat het opnieuw proberen wachten moet de aanvraag.

        HTTP Status 429,
        Status Line: RequestRateTooLarge
        x-ms-retry-after-ms :100

    De SDK's impliciet alle catch dit antwoord, aansluiten bij de server opgegeven opnieuw proberen na de header en probeer de aanvraag. Als uw account wordt door meerdere clients tegelijkertijd wordt geopend, wordt de volgende poging slaagt.

    Hebt u meer dan één client cumulatief werken consistent boven de snelheid van aanvragen, het aantal nieuwe pogingen van standaard momenteel ingesteld op 9 intern door de client niet toereikend zijn; in dit geval de client genereert een [DocumentClientException](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client_exception) code met de status 429 naar de toepassing. Het standaardaantal-nieuwe pogingen kan worden gewijzigd met behulp van [setRetryOptions](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy.setretryoptions) op de [ConnectionPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._connection_policy) exemplaar. Standaard wordt de DocumentClientException met de statuscode 429 geretourneerd na een cumulatieve wachttijd van 30 seconden als de aanvraag worden voortgezet boven de snelheid van aanvragen. Dit gebeurt zelfs als het huidige aantal nieuwe pogingen is kleiner dan het maximale aantal, worden deze de standaardwaarde van 9 of een door de gebruiker gedefinieerde waarde.

    Terwijl het gedrag voor automatische opnieuw proberen helpt om tolerantie en gebruiksgemak voor de meeste toepassingen te verbeteren, kan deze op odds zijn bij het uitvoeren van referentiepunten voor prestaties, met name bij het meten van latentie.  De latentie waargenomen-client wordt oploopt als het experiment komt binnen via deze server-restrictie en zorgt ervoor dat de client-SDK op de achtergrond opnieuw uit te voeren. Om te voorkomen latentiepieken tijdens prestaties experimenten, meet de geretourneerd door elke bewerking kosten in rekening gebracht en ervoor te zorgen dat aanvragen hieronder de aanvraagsnelheid gereserveerde nog werken. Zie voor meer informatie, [Aanvraageenheden](request-units.md).
3. **Ontwerp voor een kleinere documenten voor een hogere doorvoer**

    De kosten van de aanvraag (de kosten van de verwerking van aanvragen) van een bepaalde bewerking is direct gecorreleerd met de grootte van het document. Bewerkingen voor grote documenten duurder dan bewerkingen voor kleine-documenten.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het ontwerpen van uw toepassing voor schaalbaarheid en hoge prestaties, [partitioneren en schalen in Azure Cosmos DB](partition-data.md).
