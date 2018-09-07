---
title: Azure Cosmos DB-resourcemodel en -concepten | Microsoft Docs
description: Meer informatie over Azure Cosmos DB hiërarchische model van databases, verzamelingen, door de gebruiker gedefinieerde functie (UDF's), documenten, machtigingen voor het beheren van resources, en meer.
keywords: Hiërarchische model, cosmosdb, azure, Microsoft azure
services: cosmos-db
author: rafats
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rafats
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0869881ace689d12272affb38d3689965e107e8f
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44050929"
---
# <a name="azure-cosmos-db-hierarchical-resource-model-and-core-concepts"></a>Hiërarchisch bronmodel en basisconcepten voor Azure Cosmos DB

De database-entiteiten die worden beheerd met Azure Cosmos DB worden aangeduid als **resources**. Elke resource wordt uniek geïdentificeerd door een logische URI. U kunt met de resources met behulp van standaard HTTP-termen, headers voor aanvraag/antwoord en statuscodes werken. 

In dit artikel worden de volgende vragen:

* Wat is Azure Cosmos DB-resourcemodel?
* Wat worden door het systeem gedefinieerde resources in plaats van de gebruiker gedefinieerde resources?
* Hoe ik een resource oplossen?
* Hoe werk ik met verzamelingen?
* Hoe werk ik met opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies (UDF's)?

## <a name="hierarchical-resource-model"></a>Hiërarchisch bronmodel
Als het volgende diagram illustreert, de Azure Cosmos DB-hiërarchische **resourcemodel** bestaat uit sets van resources onder een databaseaccount, elk adresseerbaar via een URI logische en stabiel. Een set met resources worden aangeduid als een **feed** in dit artikel. 

> [!NOTE]
> Azure Cosmos DB biedt een zeer efficiënte TCP-protocol, dat ook RESTful in het communicatiemodel is, beschikbaar via de [SQL .NET-client-API](sql-api-sdk-dotnet.md).
> 
> 

![Azure Cosmos DB hiërarchisch bronmodel][1]  
**Hiërarchisch bronmodel**   

Als u wilt werken met resources, moet u [een databaseaccount maken](create-sql-api-dotnet.md) met behulp van uw Azure-abonnement. Een databaseaccount kan bestaan uit een set **databases**, elk met meerdere **verzamelingen**, elk van die op zijn beurt bevatten **opgeslagen procedures, triggers, UDF's, documenten, en die betrekking hebben bijlagen**. Een database ook is gekoppeld **gebruikers**, elk met een set **machtigingen** voor toegang tot de verzamelingen, opgeslagen procedures, triggers, UDF's, documenten of bijlagen. Databases, gebruikers, machtigingen en verzamelingen zijn systeem gedefinieerde resources met bekende schema's, documenten en bijlagen bevatten echter willekeurige, door de gebruiker gedefinieerde JSON-inhoud.  

| Resource | Beschrijving |
| --- | --- |
| Databaseaccount |Een databaseaccount is gekoppeld aan een set van databases en een vast bedrag van blob-opslag voor bijlagen. U kunt een of meer database-accounts met behulp van uw Azure-abonnement maken. Voor meer informatie gaat u naar de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/). |
| Database |Een database is een logische container voor documentopslag, gepartitioneerd in verzamelingen. Het is ook een container voor gebruikers. |
| Gebruiker |De logische naamruimte voor het vaststellen van machtigingen. |
| Machtiging |Een verificatietoken dat is gekoppeld aan een gebruiker voor toegang tot een specifieke resource. |
| Verzameling |Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica. Verzamelingen kunnen meerdere partities/servers omvatten en kunnen worden geschaald voor een vrijwel onbeperkte hoeveelheid opslag of doorvoer. |
| Opgeslagen procedure |Logica van toepassingen die zijn geschreven in JavaScript, die is geregistreerd bij een verzameling en transactioneel uitvoeren binnen de database-engine. |
| Trigger |Logica van toepassingen die zijn geschreven in JavaScript uitgevoerd vóór of na een van beide een insert-, vervangen of verwijderen. |
| UDF |De toepassingslogica is geschreven in JavaScript. UDF's kunnen u een aangepaste query-operator modelleren en waardoor de belangrijkste SQL-API-querytaal uitbreiden. |
| Document |Door gebruiker gedefinieerde (willekeurige) JSON-inhoud. Standaard geen schema moet worden gedefinieerd en ook geen secundaire indexen hoeft te worden opgegeven voor alle documenten die worden toegevoegd aan een verzameling. |
| Bijlage |Een bijlage is een speciaal document met verwijzingen en gekoppelde metagegevens voor externe blob/media. De ontwikkelaar kunt kiezen om de blob die wordt beheerd door Cosmos DB hebt of op te slaan met een externe blob-serviceprovider, zoals OneDrive, Dropbox, enzovoort. |

## <a name="system-vs-user-defined-resources"></a>Systeem versus de gebruiker gedefinieerde resources
Resources, zoals de database-accounts en -databases, verzamelingen, gebruikers, machtigingen, opgeslagen procedures, triggers en UDF's - hebben een vast schema en systeembronnen worden genoemd. Daarentegen, resources, zoals documenten en bijlagen gelden geen beperkingen op het schema en zijn voorbeelden van de gebruiker gedefinieerde resources. In Cosmos DB, worden zowel de systeem- als de gebruiker gedefinieerde resources weergegeven en beheerd als standaard-compatibele JSON. Alle resources, systeembestanden of de gebruiker gedefinieerde, moet u de volgende algemene eigenschappen hebben:

> [!NOTE]
> Alle eigenschappen van het systeem gegenereerd in een resource worden voorafgegaan door een onderstrepingsteken (_) in de JSON-weergave.
> 
> 

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Eigenschap</strong></p></td>
            <td valign="top"><p><strong>Instelbare gebruiker of door het systeem gegenereerde?</strong></p></td>
            <td valign="top"><p><strong>Doel</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Door het systeem gegenereerde</p></td>
            <td valign="top"><p>Het systeem gegenereerde, unieke en hiërarchische id van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Door het systeem gegenereerde</p></td>
            <td valign="top"><p>ETag van de resource die is vereist voor Optimistisch gelijktijdigheidbeheer</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Door het systeem gegenereerde</p></td>
            <td valign="top"><p>De laatst bijgewerkte timestamp van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Door het systeem gegenereerde</p></td>
            <td valign="top"><p>De unieke adresseerbare URI van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>id</p></td>
            <td valign="top"><p>Een van beide</p></td>
            <td valign="top"><p>Gebruiker gedefinieerde unieke naam van de resource (met de dezelfde waarde voor de partitiesleutel). Als de gebruiker geen id opgeeft heeft, is een id door het systeem gegenereerde</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Wire-weergave van resources
Cosmos DB schrijft niet een eigen uitbreidingen naar de JSON-coderingen voor standard- of speciale; het werkt met standaard, compatibel zijn JSON-documenten.  

### <a name="addressing-a-resource"></a>Een resource-adressering
Alle resources zijn URI beschikbaar komen. De waarde van de **_self** eigenschap van een resource vertegenwoordigt de relatieve URI van de resource. De indeling van de URI bestaat uit de /\<feed\>/ padsegmenten {_rid}:  

| Waarde van de _self | Beschrijving |
| --- | --- |
| /dbs |Feed met databases onder een databaseaccount |
| /dbs/{dbName} |Database met een id die overeenkomt met de waarde {dbName} |
| /colls/ /DBS/ {dbName} |Feed van verzamelingen in een database |
| /DBS/ {dbName} /colls/ {collName} |Verzameling met de id die overeenkomt met de waarde {collName} |
| /DBS/ {dbName} /colls/ {collName} / docs |Feed van documenten in een verzameling |
| /DBS/ {dbName} /colls/ {collName} /docs/ {docId} |Document met een id die overeenkomt met de waarde {doc} |
| /gebruikers/ /DBS/ {dbName} |Feed van gebruikers in een database |
| /DBS/ {dbName} /gebruikers/ {userId} |Gebruiker met de id die overeenkomt met de waarde {user} |
| /DBS/ {dbName} /gebruikers/ {userId} / machtigingen |Feed van machtigingen onder een gebruikersaccount |
| /DBS/ {dbName} /gebruikers/ {userId} /permissions/ {permissionId} |Machtiging met de id die overeenkomt met de waarde {machtiging} |

Elke resource heeft een unieke naam gebruiker gedefinieerde beschikbaar via de eigenschap-id. Opmerking: voor documenten, als de gebruiker geen een id geeft, de SDK's genereert automatisch een unieke id voor het document. De id is een tekenreeks zelfgedefinieerde van maximaal 256 tekens bevatten die uniek is binnen de context van een specifieke bovenliggende resource. 

Elke resource heeft ook een hiërarchische resource door het systeem gegenereerde id (ook wel een RID genoemd), die beschikbaar zijn via de eigenschap _rid is. De RID codeert de gehele hiërarchie van een bepaalde resource en is een handige interne weergave voor het afdwingen van referentiële integriteit in een gedistribueerde manier. De RID is uniek zijn binnen een databaseaccount en wordt intern gebruikt door Cosmos DB voor efficiënte routering zonder cross partitie zoekacties. De waarden van de eigenschappen _rid en de _self zijn alternatieve en canonieke vorm van een resource. 

De REST API's ondersteunen van resources adressering en routering van aanvragen door de id en de _rid-eigenschappen.

## <a name="database-accounts"></a>Database-accounts
U kunt een of meer Cosmos DB-database-accounts met behulp van uw Azure-abonnement inrichten.

U kunt maken en beheren van Cosmos DB-database-accounts via Azure portal op [ http://portal.azure.com/ ](https://portal.azure.com/). Het maken en beheren van een databaseaccount met beheerdersrechten toegang is vereist en kan alleen worden uitgevoerd onder uw Azure-abonnement. 

### <a name="database-account-properties"></a>Eigenschappen van de database-account
Als onderdeel van het inrichten en beheren van een databaseaccount kunt u configureren en lezen van de volgende eigenschappen:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>De naam van eigenschap</strong></p></td>
            <td valign="top"><p><strong>Beschrijving</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Consistentie van beleid</p></td>
            <td valign="top"><p>Stel deze eigenschap het standaardconsistentieniveau voor alle verzamelingen voor uw databaseaccount configureren. U kunt het consistentieniveau van de op basis van per aanvraag met behulp van de aanvraagheader van [x-ms-consistentie-niveau] overschrijven. <p><p>Deze eigenschap is alleen van toepassing op de <i>gebruiker gedefinieerde resources</i>. Alle systeem gedefinieerde resources zijn geconfigureerd ter ondersteuning van leesbewerkingen/query's met sterke consistentie.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Sleutels voor de verificatieregel</p></td>
            <td valign="top"><p>De primaire en secundaire hoofd- en alleen-lezen sleutels die met beheerdersrechten toegang tot alle resources onder het account van de database bieden.</p></td>
        </tr>
    </tbody>
</table>

Naast het inrichten, configureren en beheren van uw databaseaccount vanuit Azure portal, u kunt ook programmatisch maken en Cosmos DB-database-accounts beheren met behulp van de [REST API's van Azure Cosmos DB](/rest/api/cosmos-db/) evenals [client-SDK's](sql-api-sdk-dotnet.md).  

## <a name="databases"></a>Databases
Een Cosmos DB-database is een logische container voor een of meer verzamelingen en gebruikers, zoals wordt weergegeven in het volgende diagram. U kunt een willekeurig aantal databases onder een Cosmos DB-databaseaccount is onderworpen aan beperkingen van de aanbieding maken.  

![Database-account en verzamelingen hiërarchische model][2]  
**Een Database is een logische container voor gebruikers en verzamelingen**

Een database kan onbeperkt documentopslag, gepartitioneerd in verzamelingen bevatten.

### <a name="elastic-scale-of-an-azure-cosmos-db-database"></a>Elastisch schalen van een Azure Cosmos DB-database
Een Cosmos DB-database is standaard – variërend van een paar GB tot petabytes aan document-SSD-opslag en ingerichte doorvoer elastisch. 

Een database in Cosmos DB is in tegenstelling tot een database in traditionele RDBMS, niet afgestemd op één computer. Met Cosmos DB, naar gelang de behoeften van uw toepassing schalen om te groeien, kunt u meer verzamelingen, databases of beide. Inderdaad, verschillende toepassingen in de eerste partij binnen Microsoft hebt gebruikt Azure Cosmos DB op een schaal consumenten door het maken van zeer grote databases met Azure Cosmos DB elke met duizenden verzamelingen met terabytes aan opslag van documenten. U kunt vergroten of verkleinen van een database toevoegen of verwijderen van verzamelingen om te voldoen aan schaalvereisten van uw toepassing. 

U kunt een willekeurig aantal verzamelingen in een database onderworpen aan de aanbieding maken. Elke verzameling of een set van verzamelingen (in een database), heeft SSD ondersteund opslag en doorvoer die is ingericht voor u, afhankelijk van het geselecteerde pakket.

Een Azure Cosmos DB-database is ook een container van gebruikers. Een gebruiker, in de inschakelen, is een logische naamruimte voor een reeks machtigingen die voorziet in uiterst gedetailleerde autorisatie en toegang tot de verzamelingen, documenten en bijlagen.  

Zoals met andere resources in het Azure Cosmos DB-resourcemodel,-databases kunnen worden gemaakt, vervangen, verwijderd, lezen of geïnventariseerd eenvoudig met behulp van de [REST-API's](/rest/api/cosmos-db/) of een van de [client-SDK's](sql-api-sdk-dotnet.md). Azure Cosmos DB garandeert een sterke consistentie voor het lezen van of het uitvoeren van query's de metagegevens van een databaseresource. Automatisch verwijderen van de database, zorgt u ervoor dat u geen toegang de verzamelingen of gebruikers die hierin zijn opgenomen tot.   

## <a name="collections"></a>Verzamelingen
Een Cosmos DB-verzameling is een container voor uw JSON-documenten. 

### <a name="elastic-ssd-backed-document-storage"></a>Elastische document SSD-opslag
Er is een verzameling intrinsiek elastische - deze automatisch groeit en krimpt als u documenten toevoegen of verwijderen. Verzamelingen zijn logische resources en een of meer fysieke partities of servers kunnen omvatten. Het aantal partities die zijn toegewezen aan een verzameling wordt bepaald door Cosmos DB op basis van de opslaggrootte en de ingerichte doorvoer voor de verzameling of een set van verzamelingen. Elke partitie in Cosmos DB heeft een vaste hoeveelheid SSD-opslag gekoppeld en wordt gerepliceerd voor hoge beschikbaarheid. Beheer van de partitie wordt volledig beheerd door Azure Cosmos DB en er geen complexe code schrijven of beheren van de partities. Cosmos DB-verzamelingen zijn **onbeperkte** in termen van opslag en doorvoer. 

### <a name="automatic-indexing-of-collections"></a>Automatische indexering van verzamelingen
Azure Cosmos DB is een databasesysteem true zonder schema. Het niet wordt ervan uitgegaan dat of een schema nodig voor de JSON-documenten. Als u documenten aan een verzameling toevoegt, Azure Cosmos DB automatisch geïndexeerd en ze beschikbaar zijn voor u om op te vragen. Automatische indexering van documenten zonder schema of secundaire indexen is een belangrijke functie van Azure Cosmos DB en technieken voor schrijven geoptimaliseerde en indexeringstechnologie vergrendeling gratis index onderhoud is ingeschakeld. Azure Cosmos DB biedt ondersteuning voor doorlopend volume van zeer snel schrijfbewerkingen terwijl nog steeds consistente-query's. Document-en index worden gebruikt voor het berekenen van de opslag die door elke verzameling. U kunt de opslag en prestaties wisselwerking die zijn gekoppeld aan het indexeren van door het configureren van het indexeringsbeleid voor een verzameling beheren. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Het indexeringsbeleid van een verzameling configureren
Het indexeringsbeleid van elke verzameling kunt u prestaties en opslagvereisten wisselwerking die zijn gekoppeld aan het indexeren. De volgende opties zijn beschikbaar voor u als onderdeel van de indexing-configuratie:  

* Kies of de verzameling automatisch alle documenten of niet indexeert. Alle documenten worden standaard automatisch geïndexeerd. U kunt automatische indexering uitgeschakeld en wordt selectief alleen specifieke documenten toevoegen aan de index. U kunt ook selectief wilt uitsluiten van alleen specifieke documenten. U kunt dit doen door het instellen van de automatische eigenschap moet true of false zijn op de indexingPolicy van een verzameling en het gebruik van de aanvraagheader van [x-ms-indexingdirective] tijdens het invoegen, vervangen of verwijderen van een document.  
* Kies of u wilt opnemen of uitsluiten van specifieke paden of patronen in uw documenten uit de index. U kunt dit doen door de instelling includedPaths en excludedPaths op de indexingPolicy van een verzameling respectievelijk. U kunt ook de opslag en prestaties wisselwerking voor bereik en hash-query's voor specifieke padpatronen configureren. 
* (In overeenstemming) Kies tussen synchrone en asynchrone (lazy) index-updates. De index wordt standaard synchroon bijgewerkt op elke invoegen, vervangen of verwijderen van een document aan de verzameling. Hierdoor kan de query's te voldoen aan de dezelfde consistentieniveau als die van het document lezen. Azure Cosmos DB is geoptimaliseerd voor schrijven en duurzame aantallen document schrijfopdrachten naast synchrone indexonderhoud ten behoeve van consistente-query's ondersteunt, kunt u bepaalde verzamelingen voor het bijwerken van de index lazily kunt configureren. Vertraagde indexeren verder de prestaties voor schrijven en is ideaal voor het bulksgewijs opname scenario's voor voornamelijk leesintensief verzamelingen.

Het indexeringsbeleid kan worden gewijzigd door het uitvoeren van een PUT voor de verzameling. Dit kan worden bereikt via de [client-SDK](sql-api-sdk-dotnet.md), wordt de [Azure-portal](https://portal.azure.com), of de [REST-API's](/rest/api/cosmos-db/).

### <a name="querying-a-collection"></a>Uitvoeren van een verzameling query 's
De documenten in een verzameling kunnen willekeurige schema's en u kunt documenten binnen een verzameling een query zonder op te geven van een schema of secundaire indexen van tevoren. U kunt een query de verzameling met behulp van de [naslaginformatie over Azure Cosmos DB SQL-syntaxis](https://msdn.microsoft.com/library/azure/dn782250.aspx), waarmee u uitgebreide hiërarchische, relationele en ruimtelijke operators en uitbreidingsmogelijkheden via op basis van JavaScript UDF's. JSON-grammatica biedt de mogelijkheid voor het modelleren van JSON-documenten als structuren met labels als structuurknooppunten. Dit wordt misbruikt zowel door de automatische indexeringstechnieken SQL-API, evenals Azure Cosmos DB SQL-dialect. De SQL-querytaal bestaat uit drie belangrijke aspecten:   

1. Een kleine set van querybewerkingen die op een natuurlijke manier worden toegewezen aan de boomstructuur met inbegrip van hiërarchische query's en projecties. 
2. Een subset van relationele bewerkingen, met inbegrip van de samenstelling, filter, projecties, statistische functies en self joins. 
3. Pure JavaScript op basis van UDF's die met werken (1) en (2).  

Het model van Azure Cosmos DB-query is geprobeerd een evenwicht tot stand tussen de functionaliteit, efficiëntie en eenvoud. De Azure Cosmos DB-database-engine systeemeigen kan worden gecompileerd en uitgevoerd van de SQL-query-instructies. U kunt een query een verzameling met de [REST-API's](/rest/api/cosmos-db/) of een van de [client-SDK's](sql-api-sdk-dotnet.md). De .NET SDK wordt geleverd met een LINQ-provider.

> [!TIP]
> U kunt proberen de SQL-API en SQL-query's uitvoeren in onze gegevensset in de [testomgeving voor Query's](https://www.documentdb.com/sql/demo).
> 
> 

## <a name="multi-document-transactions"></a>Transacties met meerdere documenten
Databasetransacties bieden een veilige en voorspelbare programmeermodel voor het omgaan met gelijktijdige wijzigingen in de gegevens. In een RDBMS van TOPKLASSE, de traditionele manier om te schrijven bedrijfslogica is het schrijven van **opgeslagen procedures** en/of **triggers** en verzendt deze naar de database-server voor de transactionele uitvoering. In een RDBMS van TOPKLASSE, is de programmeur toepassing vereist om op te lossen met twee verschillende programmeertalen: 

* De (niet-transactionele) API-taal (bijvoorbeeld, JavaScript, Python, C#, Java, enz.)
* T-SQL, de transactionele programmeertaal die standaard door de database wordt uitgevoerd

Omdat de deep inzet voor JavaScript en JSON rechtstreeks in de database-engine, Azure Cosmos DB biedt een intuïtief programmeermodel voor uitvoerende JavaScript-toepassingslogica gebaseerd rechtstreeks op de verzamelingen in termen van opgeslagen procedures en wordt geactiveerd. Hiermee kunt u het volgende:

* Efficiënte implementatie van gelijktijdigheid beheren, herstel, automatische indexering van de JSON-objectgrafieken rechtstreeks in de database-engine
* Op een natuurlijke manier uitdrukken Controlestroom, variabele scoping, toewijzing en integratie van primitieven databasetransacties rechtstreeks in termen van de programmeertaal JavaScript afhandeling van uitzonderingen

De JavaScript-logica die is geregistreerd op het niveau van een verzameling kan vervolgens databasebewerkingen op de documenten van de opgegeven verzameling. Azure Cosmos DB impliciet loopt de JavaScript-gebaseerde opgeslagen procedures en triggers binnen een ambient ACID-transactie met snapshot-isolatie documenten binnen een verzameling. Als er tijdens de uitvoering een JavaScript-uitzondering optreedt, wordt de volledige transactie afgebroken. De resulterende programmeermodel is eenvoudige maar krachtige. JavaScript-ontwikkelaars ophalen een "duurzame" programmeermodel tijdens het nog steeds gebruik van hun vertrouwde taalconstructies en bibliotheek primitieven   

De mogelijkheid voor het uitvoeren van JavaScript direct in de database-engine in de adresruimte hetzelfde als de buffergroep kan goed presterende en transactionele uitvoering van databasebewerkingen op de documenten van een verzameling. Bovendien Cosmos DB-database-engine maakt een diepgaande betrokkenheid bij de JSON en JavaScript wordt voorkomen dat een wat betreft impedantie tussen de systemen type van de toepassing en de database.   

Na het maken van een verzameling, kunt u opgeslagen procedures, triggers en UDF's registreren met een verzameling met de [REST-API's](/rest/api/cosmos-db/) of een van de [client-SDK's](sql-api-sdk-dotnet.md). Na de registratie, kunt u verwijzen naar en ze worden uitgevoerd. Houd rekening met de volgende opgeslagen procedure volledig in JavaScript geschreven en de onderstaande code neemt twee argumenten (boeknaam en naam van de auteur) en maakt u een nieuw document, zoekt naar een document en werkt vervolgens het – alle binnen een impliciete ACID-transactie. Op elk gewenst moment tijdens het uitvoeren als een JavaScript-uitzondering is opgetreden, de hele transactie wordt afgebroken.

    function businessLogic(name, author) {
        var context = getContext();
        var collectionManager = context.getCollection();        
        var collectionLink = collectionManager.getSelfLink()

        // create a new document.
        collectionManager.createDocument(collectionLink,
            {id: name, author: author},
            function(err, documentCreated) {
                if(err) throw new Error(err.message);

                // filter documents by author
                var filterQuery = "SELECT * from root r WHERE r.author = 'George R.'";
                collectionManager.queryDocuments(collectionLink,
                    filterQuery,
                    function(err, matchingDocuments) {
                        if(err) throw new Error(err.message);

                        context.getResponse().setBody(matchingDocuments.length);

                        // Replace the author name for all documents that satisfied the query.
                        for (var i = 0; i < matchingDocuments.length; i++) {
                            matchingDocuments[i].author = "George R. R. Martin";
                            // we don’t need to execute a callback because they are in parallel
                            collectionManager.replaceDocument(matchingDocuments[i]._self,
                                matchingDocuments[i]);   
                        }
                    })
            })
    };

De client kan "verzenden" de bovenstaande JavaScript-logica aan de database voor de transactionele uitvoering via HTTP POST. Zie voor meer informatie over het gebruik van HTTP-methoden [RESTful interactie met Azure Cosmos DB-resources](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

    client.createStoredProcedureAsync(collection._self, {id: "CRUDProc", body: businessLogic})
       .then(function(createdStoredProcedure) {
            return client.executeStoredProcedureAsync(createdStoredProcedure.resource._self,
                "NoSQL Distilled",
                "Martin Fowler");
        })
        .then(function(result) {
            console.log(result);
        },
        function(error) {
            console.log(error);
        });


U ziet dat omdat de database systeemeigen JSON en JavaScript begrijpt, er is geen niet-overeenkomend gegevenstype system, geen 'Of toewijzing' of code generation magic vereist.   

Opgeslagen procedures en triggers, communiceren met een verzameling en documenten in een verzameling via een goed gedefinieerde objectmodel, waarmee wordt aangegeven dat de huidige context van de verzameling.  

Verzamelingen in de SQL-API kunnen worden gemaakt, verwijderd, lezen of geïnventariseerde eenvoudig met behulp van de [REST-API's](/rest/api/cosmos-db/) of een van de [client-SDK's](sql-api-sdk-dotnet.md). De SQL-API biedt altijd sterke consistentie voor het lezen van of het uitvoeren van query's de metagegevens van een verzameling. Verwijderen van een verzameling automatisch zorgt ervoor dat u geen toegang de documenten, bijlagen, opgeslagen procedures, triggers tot en UDF's die hierin zijn opgenomen.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Opgeslagen procedures, triggers en door gebruiker gedefinieerde functies (UDF's)
Zoals wordt beschreven in de vorige sectie, kunt u toepassingslogica om uit te voeren rechtstreeks binnen een transactie in de database-engine kunt schrijven. De toepassingslogica volledig in JavaScript kan worden geschreven en kan worden gemodelleerd als een opgeslagen procedure, trigger of een UDF. De JavaScript-code in een opgeslagen procedure of een trigger kunt invoegen, vervangen, verwijderen, lezen of query's voor documenten binnen een verzameling. De JavaScript binnen een UDF kan niet aan de andere kant invoegen, vervangen of verwijderen van documenten. UDF's de documenten van de resultatenset van een query inventariseren en produceren van een andere resultatenset. Azure Cosmos DB afgedwongen voor multitenancy, een strikte reservering gebaseerde resourcebeheer. Elk opgeslagen procedure, trigger of een UDF haalt u een vaste quantum van besturingssysteemresources om zijn werk te doen. Bovendien opgeslagen procedures, triggers en UDF's kunnen niet koppelen aan externe JavaScript-bibliotheken en zijn op de blokkeringslijst als ze groter is dan de resource-budget uitgeven aan hen is toegewezen. U kunt registreren, registratie opheffen opgeslagen procedures, triggers en UDF's met een verzameling met behulp van de REST API's.  Bij de registratie een opgeslagen procedure, trigger of een UDF vooraf gecompileerd en opgeslagen als byte-code, die later wordt uitgevoerd. De volgende sectie ziet u hoe u de JavaScript-SDK van Azure Cosmos DB kunt gebruiken om te registreren, uitvoeren en de registratie van een opgeslagen procedure, trigger en een UDF. De JavaScript-SDK is een eenvoudige wrapper boven de [REST-API's](/rest/api/cosmos-db/). 

### <a name="registering-a-stored-procedure"></a>Registreren van een opgeslagen procedure
Registratie van een opgeslagen procedure maakt een nieuwe resource van de opgeslagen procedure op een verzameling via HTTP POST.  

    var storedProc = {
        id: "validateAndCreate",
        body: function (documentToCreate) {
            documentToCreate.id = documentToCreate.id.toUpperCase();

            var collectionManager = getContext().getCollection();
            collectionManager.createDocument(collectionManager.getSelfLink(),
                documentToCreate,
                function(err, documentCreated) {
                    if(err) throw new Error('Error while creating document: ' + err.message;
                    getContext().getResponse().setBody('success - created ' + 
                            documentCreated.name);
                });
        }
    };

    client.createStoredProcedureAsync(collection._self, storedProc)
        .then(function (createdStoredProcedure) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-stored-procedure"></a>Uitvoeren van een opgeslagen procedure
Uitvoering van een opgeslagen procedure wordt uitgevoerd door het uitgeven van een HTTP POST op basis van een bestaande resource in de opgeslagen procedure door parameters doorgeven aan de procedure in de aanvraagtekst.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>Registratie van een opgeslagen procedure
Registratie van een opgeslagen procedure wordt uitgevoerd door het uitgeven van een HTTP DELETE op basis van een bestaande resource in de opgeslagen procedure.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Registratie van een vooraf trigger
Registratie van een trigger wordt uitgevoerd door het maken van een nieuwe resource van de trigger op een verzameling via HTTP POST. U kunt opgeven als de trigger een vooraf is of een post-trigger en het type bewerking deze gekoppeld aan (bijvoorbeeld maken, vervangen, verwijderen of alle worden kan).   

    var preTrigger = {
        id: "upperCaseId",
        body: function() {
                var item = getContext().getRequest().getBody();
                item.id = item.id.toUpperCase();
                getContext().getRequest().setBody(item);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.All
    }

    client.createTriggerAsync(collection._self, preTrigger)
        .then(function (createdPreTrigger) {
            console.log("Successfully created trigger");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-pre-trigger"></a>Een pre-trigger wordt uitgevoerd
Uitvoering van een trigger wordt uitgevoerd door de naam van een bestaande trigger op te geven op het moment van de aanvraag POST, PUT en verwijderen van de resource van een document via de aanvraagheader uitgeven.  

    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>Registratie van een pre-trigger
Registratie van een trigger wordt uitgevoerd via het uitgeven van een HTTP DELETE op basis van een bestaande resource in de trigger.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Registreren van een UDF
Registratie van een UDF wordt uitgevoerd door het maken van een nieuwe UDF-resource op een verzameling via HTTP POST.  

    var udf = { 
        id: "mathSqrt",
        body: function(number) {
                return Math.sqrt(number);
        },
    };
    client.createUserDefinedFunctionAsync(collection._self, udf)
        .then(function (createdUdf) {
            console.log("Successfully created stored procedure");
        }, function(error) {
            console.log("Error");
        });

### <a name="executing-a-udf-as-part-of-the-query"></a>Een UDF als onderdeel van de query wordt uitgevoerd
Een UDF kan worden opgegeven als onderdeel van de SQL-query en wordt gebruikt als een manier om uit te breiden, de belangrijkste [SQL-querytaal](sql-api-sql-query-reference.md).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>Registratie van een UDF
Registratie van een UDF loopt gewoon door uitgifte van een HTTP DELETE op basis van een bestaande UDF-resource.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Hoewel de bovenstaande codefragmenten bleek de registratie (POST), verwijderen van registratie (opslag), (GET) lijst/lezen en kan worden uitgevoerd (POST) via dat de [JavaScript SDK](https://github.com/Azure/azure-documentdb-js), u kunt ook de [REST-API's](/rest/api/cosmos-db/) of andere [client-SDK's](sql-api-sdk-dotnet.md). 

## <a name="documents"></a>Documenten
U kunt invoegen, vervangen, verwijderen, lezen, opsommen en query uitvoeren op willekeurige JSON-documenten in een verzameling. Azure Cosmos DB schrijft niet een schema en hoeven geen secundaire indexen ter ondersteuning van query's op documenten in een verzameling. De maximale grootte voor een document is 2 MB.   

Wordt een daadwerkelijk open databaseservice, biedt Azure Cosmos DB niet tot een gespecialiseerde gegevenstypen (bijvoorbeeld datum en tijd) of specifieke coderingen voor JSON-documenten. Azure Cosmos DB is een speciale JSON-conventies samen te vatten de relaties tussen verschillende documenten; niet vereist de SQL-syntaxis van Azure Cosmos DB biedt krachtige query voor hiërarchische en relationele operators op documenten in query- en project zonder speciale aantekeningen of moet samen te vatten relaties tussen documenten met elkaar worden onderscheiden eigenschappen.  

Als met alle andere resources, documenten kunnen worden gemaakt, vervangen, verwijderd, lezen, geïnventariseerd en waaruit gegevens eenvoudig met behulp van REST-API's of een van de [client-SDK's](sql-api-sdk-dotnet.md). Verwijderen van een document direct maakt u de quota die overeenkomt met alle geneste bijlagen vrij. Het niveau van het lezen van consistentie van documenten volgt het beleid consistentie van de databaseaccount. Dit beleid kan worden genegeerd op basis van per aanvraag, afhankelijk van de vereisten voor consistentie van gegevens van uw toepassing. Bij het opvragen van documenten, het lezen van consistentie, volgt de indexing-modus instellen op de verzameling. Voor "consistente" volgt dit beleid van de account van de consistentie. 

## <a name="attachments-and-media"></a>Bijlagen en media
Azure Cosmos DB kunt u voor het opslaan van binaire blobs/media met Azure Cosmos DB (maximaal 2 GB per account) of naar de mediastore van uw eigen externe. U kunt er ook voor de metagegevens van een medium in termen van een speciaal document met de naam van de bijlage. Een bijlage in Azure Cosmos DB is een speciaal document (JSON) die verwijst naar de media/blob ergens anders opgeslagen. Een bijlage is gewoon een speciaal document dat de metagegevens van een media die zijn opgeslagen in de opslag van een externe media (bijvoorbeeld, locatie, auteur enzovoort) worden vastgelegd. 

Houd rekening met een sociale lezen-toepassing die gebruikmaakt van Azure Cosmos DB voor het opslaan van aantekeningen en metagegevens, opmerkingen, waaronder markeert, bladwijzers, beoordelingen, enz. die is gekoppeld voor een e-book van een bepaalde gebruiker likes/ervaringen.   

* De inhoud van het boek zelf wordt opgeslagen in de opslag van media ofwel beschikbaar als onderdeel van Azure Cosmos DB-databaseaccount of een externe mediastore. 
* Een toepassing kan de metagegevens van elke gebruiker worden opgeslagen als een afzonderlijke document--bijvoorbeeld Jaap van metagegevens voor Map1 is opgeslagen in een document waarnaar wordt verwezen door /colls/joe/docs/book1. 
* Bijlagen die verwijst naar de contentpagina van een bepaalde boek van een gebruiker worden opgeslagen onder het desbetreffende document, bijvoorbeeld /colls/joe/docs/book1/chapter1, /colls/joe/docs/book1/chapter2 enzovoort. 

Beschrijvende ID's in de bovenstaande voorbeelden gebruiken om de resource-hiërarchie over te brengen. Resources zijn toegankelijk via de REST-API's via een unieke resource-id. 

Voor de media die wordt beheerd door Azure Cosmos DB, verwijst de eigenschap _media van de bijlage naar de media door de URI. Azure Cosmos DB zorgt ervoor dat aan garbagecollection verzamelen van de media wanneer alle van de openstaande verwijzingen worden verwijderd. Azure Cosmos DB automatisch de bijlage wordt gegenereerd tijdens het uploaden van de nieuwe media en vult de _media om te verwijzen naar de zojuist toegevoegde media. Als u ervoor kiest om op te slaan van het medium in een externe blob-opslag, beheerd door u (bijvoorbeeld OneDrive, Azure Storage, DropBox, enzovoort), kunt u de bijlagen nog steeds gebruiken om te verwijzen naar de media. In dit geval u zelf de bijlage maken en vullen van de eigenschap _media.   

Als met alle andere resources, kunnen de bijlagen worden gemaakt, vervangen, verwijderd, lezen of geïnventariseerd eenvoudig met behulp van REST-API's of een van de client-SDK's. Net als bij documenten, volgt het niveau van het lezen van consistentie van bijlagen het beleid consistentie van de databaseaccount. Dit beleid kan worden genegeerd op basis van per aanvraag, afhankelijk van de vereisten voor consistentie van gegevens van uw toepassing. Bij het opvragen van om bijlagen, volgt het lezen van consistentie de indexing-modus instellen op de verzameling. Voor "consistente" volgt dit beleid van de account van de consistentie. 
 

## <a name="users"></a>Gebruikers
Een Azure Cosmos DB-gebruiker vertegenwoordigt een logische naamruimte voor het groeperen van machtigingen. Een Azure Cosmos DB-gebruiker kan komen overeen met een gebruiker in een identiteitsbeheersysteem of een vooraf gedefinieerde toepassingsrol. Voor Azure Cosmos DB vertegenwoordigt een gebruiker gewoon een abstractie aan de groep van een reeks machtigingen onder een database.   

Voor het implementeren van meerdere tenants in uw toepassing, kunt u gebruikers maken in Azure Cosmos DB, wat overeenkomt met de werkelijke gebruikers- of de tenants van uw toepassing. Vervolgens kunt u machtigingen voor een bepaalde gebruiker die met de toegangscontrole voor verschillende verzamelingen, documenten, bijlagen, enzovoort overeenkomen.   

Als uw toepassingen om te schalen met een groei van de gebruiker nodig hebt, kunt u uw gegevens kunt op verschillende manieren shard vast. U kunt een model van elk van uw gebruikers als volgt:   

* Elke gebruiker wordt toegewezen aan een database.
* Elke gebruiker worden toegewezen aan een verzameling. 
* Documenten die overeenkomt met meerdere gebruikers gaat u naar een specifieke verzameling. 
* Documenten die overeenkomt met meerdere gebruikers gaat u naar een set van verzamelingen.   

Ongeacht de specifieke shardingstrategie kiest u, kunt u het model van uw werkelijke gebruikers als gebruikers in Azure Cosmos DB-database en goed korrelig dat machtigingen aan elke gebruiker koppelen.  

![Verzamelingen van gebruikers][3]  
**Shardingstrategieën en modellering van gebruikers**

Als alle andere resources, gebruikers in Azure Cosmos DB kunnen worden gemaakt, vervangen, verwijderd, lezen of geïnventariseerd eenvoudig met behulp van REST-API's of een van de client-SDK's. Azure Cosmos DB biedt altijd sterke consistentie voor het lezen van of het uitvoeren van query's de metagegevens van een gebruiker een resource. Het is om te zeggen, dat als u een gebruiker verwijdert, wordt automatisch zorgt ervoor dat u geen toegang de machtigingen die hierin zijn opgenomen tot. Hoewel de Azure Cosmos DB maakt het quotum van de machtigingen als onderdeel van de verwijderde gebruiker op de achtergrond, de verwijderde machtigingen is onmiddellijk beschikbaar opnieuw moet worden gebruikt.  

## <a name="permissions"></a>Machtigingen
Vanuit een access control perspectief, resources, zoals de database-accounts, databases, gebruikers en machtigingen worden beschouwd als *administratieve* resources omdat deze is vereist voor beheerdersmachtigingen nodig. Aan de andere kant bronnen, inclusief de verzamelingen, documenten, bijlagen en opgeslagen procedures, triggers en UDF's zijn binnen het bereik van onder een bepaalde database en beschouwd als *toepassingsresources*. Overeenkomt met de twee typen resources en de rollen die toegang hebben tot deze (namelijk de beheerder en gebruiker), de autorisatiemodel definieert twee typen *toegangssleutels*: *hoofdsleutel* en  *Bronsleutel*. De hoofdsleutel is een onderdeel van het account van de database en wordt aangeboden aan de ontwikkelaar (of de beheerder) die de account van de database wordt ingericht. Deze hoofdsleutel is beheerder semantiek, dat kan worden gebruikt om toegang tot zowel de administratieve en de toepassing resources te verlenen. Een resource-sleutel is daarentegen een gedetailleerde toegangssleutel voor toegang tot een *specifieke* toepassingsresource. Zo wordt de relatie tussen de gebruiker van een database en de machtigingen die de gebruiker voor een specifieke resource (bijvoorbeeld verzameling, document, bijlage, opgeslagen procedure, trigger of UDF heeft) vastgelegd.   

De enige manier om het ophalen van een resource-sleutel is door het maken van een machtiging resource onder een bepaalde gebruiker. Als u wilt maken of ophalen van een machtiging, moet een hoofdsleutel in de autorisatie-header worden gepresenteerd. Een resource machtiging verbindt de resource, de toegang en de gebruiker. Na het maken van een resource machtiging, moet de gebruiker alleen om weer te geven van de sleutel van de gekoppelde resource om te kunnen krijgen van toegang tot de betreffende resource. Daarom kan kan de sleutel van een resource worden weergegeven als een logische en compact weergave van de resource machtiging.  

Als met alle andere resources, machtigingen in Azure Cosmos DB kunnen worden gemaakt, vervangen, verwijderd, lezen of geïnventariseerd eenvoudig met behulp van REST-API's of een van de client-SDK's. Azure Cosmos DB biedt altijd sterke consistentie voor het lezen van of het uitvoeren van query's de metagegevens van een machtiging. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het werken met resources met behulp van HTTP-opdrachten in [RESTful interactie met Azure Cosmos DB-resources](https://msdn.microsoft.com/library/azure/mt622086.aspx).

[1]: media/sql-api-resources/resources1.png
[2]: media/sql-api-resources/resources2.png
[3]: media/sql-api-resources/resources3.png

