---
title: Azure DB Cosmos-resourcemodel en -concepten | Microsoft Docs
description: "Meer informatie over Azure Cosmos DB hiërarchische model van de databases, verzamelingen, door de gebruiker gedefinieerde functie (UDF), documenten, machtigingen voor het beheren van bronnen en meer."
keywords: "Hiërarchische model cosmosdb, azure, Microsoft azure"
services: cosmos-db
documentationcenter: 
author: rafats
manager: jhubbard
ms.assetid: ef9d5c0c-0867-4317-bb1b-98e219799fd5
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: rafats
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a88f17a658987e1ff3ae0e0f38d6551c3acee1da
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-hierarchical-resource-model-and-core-concepts"></a>Hiërarchisch bronmodel en basisconcepten voor Azure Cosmos DB

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

De database-entiteiten die Azure Cosmos DB beheert worden aangeduid als **resources**. Elke bron wordt uniek geïdentificeerd door een logische URI. U kunt met de resources met behulp van standaard HTTP-termen, aanvraag-/ antwoordheaders en statuscodes werken. 

In dit artikel worden de volgende vragen:

* Wat is Azure Cosmos DB resourcemodel?
* Wat zijn de resources in plaats van de gebruiker gedefinieerde resources gedefinieerd?
* Hoe ik een resource adresseren?
* Hoe werkt ik met verzamelingen?
* Hoe werkt ik met opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies (UDF's)?

## <a name="hierarchical-resource-model"></a>Hiërarchische resourcemodel
Als u het volgende diagram illustreert, hiërarchische Azure Cosmos DB **resourcemodel** bestaat uit sets van resources onder een databaseaccount elke adresseerbare via een logische en stabiele URI. Een set resources worden aangeduid als een **feed** in dit artikel. 

> [!NOTE]
> Azure Cosmos DB biedt een zeer efficiënte TCP-protocol, dat ook RESTful in het communicatiemodel, beschikbaar via de [SQL .NET client-API](sql-api-sdk-dotnet.md).
> 
> 

![Azure hiërarchische resourcemodel Cosmos-DB][1]  
**Hiërarchische resourcemodel**   

Als u wilt werken met resources, moet u [een databaseaccount maken](create-sql-api-dotnet.md) met behulp van uw Azure-abonnement. Een databaseaccount kan bestaan uit een reeks **databases**, elk met meerdere **verzamelingen**, elk van die op zijn beurt bevatten ** opgeslagen procedures, triggers, UDF's, documenten en verwante  **bijlagen**. Een database ook gekoppelde **gebruikers**, elk met een reeks **machtigingen** voor toegang tot de verzamelingen, opgeslagen procedures, triggers, UDF's, documenten of bijlagen. Hoewel databases, gebruikers, machtigingen en verzamelingen systeem gedefinieerde resources met bekende schema's, bevatten documenten en bijlagen willekeurige, door de gebruiker gedefinieerde JSON-inhoud.  

| Resource | Beschrijving |
| --- | --- |
| Databaseaccount |Een databaseaccount is gekoppeld aan een set databases en een vast bedrag van blob-opslag voor bijlagen. U kunt een of meer database accounts met behulp van uw Azure-abonnement maken. Voor meer informatie gaat u naar de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/). |
| Database |Een database is een logische container voor documentopslag, gepartitioneerd in verzamelingen. Het is ook een gebruikerscontainer. |
| Gebruiker |De logische naamruimte voor het vaststellen van machtigingen. |
| Machtiging |Een verificatietoken die zijn gekoppeld aan een gebruiker voor toegang tot een specifieke bron. |
| Verzameling |Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica. Een verzameling is een factureerbare entiteit waar de [kosten](performance-levels.md) wordt bepaald door het prestatieniveau dat is gekoppeld aan de verzameling. Verzamelingen kunnen meerdere partities/servers omvatten en kunnen worden geschaald voor een vrijwel onbeperkte hoeveelheid opslag of doorvoer. |
| Opgeslagen procedure |De toepassingslogica is geschreven in JavaScript, die is geregistreerd bij een verzameling en transactioneel uitgevoerd binnen de database-engine. |
| Trigger |Toepassingslogica geschreven in JavaScript uitgevoerd vóór of na een invoeging vervangen of verwijderen. |
| UDF |Geschreven in JavaScript-toepassingslogica. UDF's kunnen u een aangepaste query-operator model en waardoor de kern van het SQL-API-querytaal uit te breiden. |
| Document |De gebruiker gedefinieerde (willekeurige) JSON-inhoud. Standaard geen schema moet worden gedefinieerd en ook secundaire indexen hoeft te worden opgegeven voor de documenten die zijn toegevoegd aan een verzameling. |
| Bijlage |Een bijlage is een speciale document met verwijzingen en gekoppelde metagegevens voor externe-blob/media. De ontwikkelaar kunt met de blob die worden beheerd door Cosmos DB, of sla deze met een externe blob-provider zoals OneDrive, Dropbox, enz. |

## <a name="system-vs-user-defined-resources"></a>Systeem versus gebruiker gedefinieerde resources
Resources zoals database accounts, databases, verzamelingen, gebruikers, machtigingen, opgeslagen procedures, triggers en UDF - alle hebben een vaste schema en systeembronnen worden genoemd. Daarentegen resources, zoals documenten en bijlagen geldt geen beperking van het schema en zijn voorbeelden van de gebruiker gedefinieerde resources. In Cosmos DB, zowel de systeem- als de gebruiker gedefinieerde resources weergegeven en beheerd als standaard-compatibele JSON. Alle resources, systeem of door de gebruiker gedefinieerde, moet u de volgende algemene eigenschappen hebben:

> [!NOTE]
> Alle systeem gegenereerde eigenschappen in een bron worden voorafgegaan door een onderstrepingsteken (_) in de JSON-weergave.
> 
> 

<table>
    <tbody>
        <tr>
            <td valign="top"><p><strong>Eigenschap</strong></p></td>
            <td valign="top"><p><strong>Gebruiker worden ingesteld of het systeem gegenereerde?</strong></p></td>
            <td valign="top"><p><strong>Doel</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>_rid</p></td>
            <td valign="top"><p>Systeem gegenereerde</p></td>
            <td valign="top"><p>Het systeem gegenereerde, unieke en hiërarchische id van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_etag</p></td>
            <td valign="top"><p>Systeem gegenereerde</p></td>
            <td valign="top"><p>ETag van de bron die vereist zijn voor Optimistisch gelijktijdigheidbeheer</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_ts</p></td>
            <td valign="top"><p>Systeem gegenereerde</p></td>
            <td valign="top"><p>Laatst bijgewerkt timestamp van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>_self</p></td>
            <td valign="top"><p>Systeem gegenereerde</p></td>
            <td valign="top"><p>Unieke adresseerbare URI van de resource</p></td>
        </tr>
        <tr>
            <td valign="top"><p>id</p></td>
            <td valign="top"><p>Beide</p></td>
            <td valign="top"><p>Gebruiker gedefinieerde unieke naam van de resource (met de dezelfde waarde voor de partitiesleutel). Als de gebruiker een id niet is opgegeven, is een id gegenereerd-systeem</p></td>
        </tr>
    </tbody>
</table>

### <a name="wire-representation-of-resources"></a>Lijnweergave van resources
Cosmos DB schrijft niet een eigen uitbreidingen voor de JSON-coderingen voor standard of speciale; Dit proces werkt met standaard compatibele JSON-documenten.  

### <a name="addressing-a-resource"></a>Een resource adressering
Alle resources zijn adresseerbare URI. De waarde van de **_self** eigenschap van een bron vertegenwoordigt de relatieve URI van de resource. De indeling van de URI bestaat uit de /\<feed\>/ padsegmenten {_rid}:  

| Waarde van de _self | Beschrijving |
| --- | --- |
| /DBS |Feed van databases onder een databaseaccount |
| /DBS/ {dbName} |Database met een id die overeenkomt met de waarde {dbName} |
| /colls/ /DBS/ {dbName} |Feed van verzamelingen onder een database |
| /DBS/ {dbName} /colls/ {collName} |De verzameling met een id die overeenkomt met de waarde {collName} |
| /DBS/ {dbName} /colls/ {collName} / docs |Feed van documenten in een verzameling |
| /DBS/ {dbName} /colls/ {collName} /docs/ {docId} |Document met een id die overeenkomt met de waarde {doc} |
| /gebruikers/ /DBS/ {dbName} |Feed gebruikers onder een database |
| /DBS/ {dbName} /gebruikers/ {userId} |Gebruiker met een id die overeenkomt met de waarde {user} |
| /DBS/ {dbName} /gebruikers/ {userId} / machtigingen |Feed machtigingen onder een gebruikersaccount |
| /DBS/ {dbName} /gebruikers/ {userId} /permissions/ {permissionId} |Machtiging met een id die overeenkomt met de waarde {machtiging} |

Elke resource heeft een unieke naam gebruiker gedefinieerde beschikbaar gesteld via de eigenschap id. Opmerking: voor documenten, als de gebruiker niet een id geeft, de SDK's automatisch gegenereerd een unieke id voor het document. De id is een gebruiker gedefinieerde tekenreeks, maximaal 256 tekens die uniek is binnen de context van een specifieke bovenliggende resource. 

Elke resource heeft ook een hiërarchische resource systeem gegenereerde id (ook wel een RID genoemd), die beschikbaar zijn via de eigenschap _rid is. De RID codeert de gehele hiërarchie van een bepaalde bron en is een handige interne weergave voor het afdwingen van referentiële integriteit in een gedistribueerde manier. De RID uniek is binnen een databaseaccount en wordt intern gebruikt door Cosmos DB voor efficiënte routering zonder cross partitie zoekacties. De waarden van de eigenschappen _rid en de _self zijn alternatieve en canonieke vorm van een resource. 

De REST API's ondersteunen van resources adressering en routering van aanvragen door zowel de id als de _rid-eigenschappen.

## <a name="database-accounts"></a>Database-accounts
U kunt een of meer Cosmos DB database accounts met behulp van uw Azure-abonnement inrichten.

U kunt maken en beheren van accounts voor Cosmos-DB-database via Azure portal op [http://portal.azure.com/](https://portal.azure.com/). Maken en beheren van een databaseaccount beheerderstoegang vereist en kan alleen worden uitgevoerd onder uw Azure-abonnement. 

### <a name="database-account-properties"></a>Eigenschappen van de database-account
Als onderdeel van het inrichten en beheren van een databaseaccount kunt u configureren en lees de volgende eigenschappen:  

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>De naam van eigenschap</strong></p></td>
            <td valign="top"><p><strong>Beschrijving</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>Consistentie beleid</p></td>
            <td valign="top"><p>Stel deze eigenschap het standaardniveau voor consistentie voor alle collecties onder uw databaseaccount configureren. U kunt het consistentieniveau per aanvraag uit te voeren met behulp van de [x-ms-consistentie-niveau] aanvraagheader overschrijven. <p><p>Deze eigenschap is alleen van toepassing op de <i>gebruiker gedefinieerde resources</i>. Alle systeem gedefinieerde resources zijn geconfigureerd voor ondersteuning voor leesbewerkingen/query's met sterke consistentie.</p></td>
        </tr>
        <tr>
            <td valign="top"><p>Autorisatie-sleutels</p></td>
            <td valign="top"><p>De primaire en secundaire hoofd- en alleen-lezen sleutels die beheerdersrechten toegang tot alle resources onder de account van de database bieden.</p></td>
        </tr>
    </tbody>
</table>

Naast het inrichten, configureren en beheren van uw databaseaccount vanuit de Azure-portal u kunt ook via een programma maken en Cosmos DB database accounts beheren met behulp van de [Azure Cosmos DB REST API's](/rest/api/documentdb/) evenals [client-SDK's](sql-api-sdk-dotnet.md).  

## <a name="databases"></a>Databases
Een Cosmos-DB-database is een logische container voor een of meer verzamelingen en gebruikers, zoals wordt weergegeven in het volgende diagram. U kunt een willekeurig aantal databases onder een Cosmos-DB-databaseaccount onderworpen aan beperkingen van de aanbieding maken.  

![Account en verzamelingen hiërarchische databasemodel][2]  
**Een Database is een logische container van gebruikers en verzamelingen**

Een database kan vrijwel onbeperkte documentopslag, gepartitioneerd in verzamelingen bevatten.

### <a name="elastic-scale-of-an-azure-cosmos-db-database"></a>Elastisch schalen van een Azure DB die Cosmos-database
Een Cosmos-DB-database is elastisch standaard – variërend van een paar GB voor petabytes aan SSD back documentopslag en ingerichte doorvoer. 

In tegenstelling tot een database in traditionele RDBMS, is een database in Cosmos DB niet afgestemd op één computer. Met Cosmos-DB naar gelang de behoeften van uw toepassing scale groeien, kunt u meer verzamelingen, databases, of beide. Inderdaad hebt verschillende toepassingen in de eerste leveranciers binnen Microsoft gebruikt Azure Cosmos DB op een schaal van de consument door te maken van zeer grote Azure Cosmos DB databases elke met duizenden verzamelingen met terabytes aan opslag van documenten. U kunt vergroten of verkleinen van een database door toe te voegen of te verwijderen van verzamelingen om te voldoen aan de schaal van uw toepassing. 

U kunt een willekeurig aantal verzamelingen binnen een database onderworpen aan de aanbieding maken. Elke verzameling heeft back SSD-opslag en u afhankelijk van de geselecteerde prestatielaag ingerichte doorvoer.

Een Azure DB die Cosmos-database is ook een container van gebruikers. Een gebruiker, beurt, is een logische naamruimte voor een reeks machtigingen die voorziet in fijnmazig autorisatie en toegang tot de verzamelingen, documenten en bijlagen.  

Zoals met andere bronnen in het model van de resource Azure Cosmos DB, databases kunnen worden gemaakt, vervangen, verwijderd, lezen of opgesomd eenvoudig met behulp van de [REST-API's](/rest/api/documentdb/) of een van de [client-SDK's](sql-api-sdk-dotnet.md). Azure Cosmos-DB garandeert een sterke consistentie voor lezen en het opvragen van de metagegevens van de bron van een database. Een database wordt automatisch verwijderd, zorgt u ervoor dat u geen toegang de verzamelingen of gebruikers die zijn opgenomen in deze tot.   

## <a name="collections"></a>Verzamelingen
Een Cosmos-DB-verzameling is een container voor uw JSON-documenten. 

### <a name="elastic-ssd-backed-document-storage"></a>Elastische SSD back-documentopslag
Er is een verzameling intrinsiek elastische - en deze automatisch groeit kleiner naarmate u documenten toevoegen of verwijderen. Verzamelingen zijn logische resources en kunnen een of meer fysieke partities of servers omvatten. Het aantal partities binnen een verzameling wordt bepaald door de Cosmos-DB op basis van de grootte van de opslagruimte en de ingerichte doorvoer van uw verzameling. Elke partitie in Cosmos-database heeft een vaste hoeveelheid back SSD opslag gekoppeld en is gerepliceerd voor hoge beschikbaarheid. Beheer van de partitie is volledig beheerd door Azure Cosmos DB en er geen complexe code schrijven of beheren van de partities. Cosmos DB verzamelingen zijn **vrijwel onbeperkte** in termen van opslag en doorvoer. 

### <a name="automatic-indexing-of-collections"></a>Automatische indexering van verzamelingen
Azure Cosmos-database is een databasesysteem waar zonder schema. Het niet wordt ervan uitgegaan dat of een schema voor de JSON-documenten nodig. Als u documenten aan een verzameling toevoegt, Azure Cosmos DB automatisch geïndexeerd en ze beschikbaar zijn voor u om op te vragen. Automatische indexering van documenten zonder schema of secundaire indexen is een belangrijke functie van Azure Cosmos-database en wordt ingeschakeld door geoptimaliseerd voor schrijven en logboek gestructureerd vergrendeling vrij index onderhoud technieken. Azure Cosmos DB ondersteunt volgehouden volume zeer snel schrijfbewerkingen tijdens afhandeling van nog steeds consistente query's. Zowel document als index opslag worden gebruikt voor het berekenen van de opslag die door elke verzameling worden gebruikt. U kunt de opslag en prestaties verschillen die zijn gekoppeld aan het indexeren door het indexeringsbeleid voor een verzameling configureren kunt beheren. 

### <a name="configuring-the-indexing-policy-of-a-collection"></a>Het indexeringsbeleid van een verzameling configureren
Het indexeringsbeleid van elke verzameling kunt u afweging prestaties en opslag die is gekoppeld aan het indexeren. De volgende opties zijn beschikbaar voor u als onderdeel van de indexing-configuratie:  

* Kies of de verzameling automatisch alle documenten of niet indexeert. Alle documenten worden standaard automatisch geïndexeerd. U kunt automatische indexering uitschakelen en alleen bepaalde documenten selectief toevoegen aan de index. U kunt echter selectief moeten worden uitgesloten van alleen specifieke documenten. U kunt dit doen door de instelling automatische-eigenschap true of false zijn op de indexingPolicy van een verzameling en het gebruik van de aanvraagheader [x-ms-indexingdirective] tijdens het invoegen of verwijderen van een document vervangen.  
* Kies of u wilt opnemen of uitsluiten van specifieke paden of patronen in uw documenten van de index. U kunt dit doen door de instelling includedPaths en excludedPaths op de indexingPolicy van een verzameling respectievelijk. U kunt ook de opslag en prestaties verschillen voor bereik en hash-query's voor een specifiek pad patronen configureren. 
* Kiezen tussen synchrone (consistent) en indexupdates van asynchrone (lazy). De index wordt standaard synchroon bijgewerkt op elke invoegen, vervangen of verwijderen van een document aan de verzameling. Hierdoor kan de query's te houden van hetzelfde consistentieniveau als die van het document leest. Terwijl Azure Cosmos DB schrijven is geoptimaliseerd en volgehouden volumes van document schrijfbewerkingen samen met synchrone index onderhoud ten behoeve van consistente query's ondersteunt, kunt u bepaalde verzamelingen voor het bijwerken van de index traag kunt configureren. Vertraagde indexeren verder de prestaties voor schrijven en is ideaal voor bulksgewijs opname scenario's voor voornamelijk lezen zware verzamelingen.

Het indexeringsbeleid kan worden gewijzigd door het uitvoeren van een PUT voor de verzameling. Dit kan worden bereikt via de [client SDK](sql-api-sdk-dotnet.md), wordt de [Azure-portal](https://portal.azure.com) of de [REST-API's](/rest/api/documentdb/).

### <a name="querying-a-collection"></a>Opvragen van een verzameling
De documenten binnen een verzameling kunnen willekeurige schema's en u kunt documenten binnen een verzameling query zonder op te geven van een schema of secundaire indexen tevoren betaalt. U kunt een query de verzameling met behulp van de [verwijzing naar de Azure Cosmos DB SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx), waarmee u uitgebreide hiërarchische, ruimtelijke en relationele operators en uitbreidingsmogelijkheden via UDF's op basis van JavaScript. JSON-grammatica biedt de mogelijkheid voor het modelleren van JSON-documenten als structuren met labels als structuurknooppunten. Dit is zowel door automatische indexering technieken SQL-API, evenals Azure Cosmos DB SQL-dialect misbruikt. De SQL-querytaal bestaat uit drie belangrijkste aspecten:   

1. Een kleine set querybewerkingen dat natuurlijk worden toegewezen aan de boomstructuur met inbegrip van hiërarchische query's en projecties. 
2. Een subset van relationele bewerkingen inclusief samenstelling, filter, statistische functies, projecties en self joins. 
3. Pure JavaScript gebaseerd UDF's die met werken (1) en (2).  

Het model van Azure DB die Cosmos query probeert toegang te bieden een evenwicht tussen functionaliteit, efficiëntie en eenvoud. De database-engine van Azure DB die Cosmos systeemeigen compileert en voert de SQL-query-instructies. U kunt een verzameling met een query de [REST-API's](/rest/api/documentdb/) of een van de [client-SDK's](sql-api-sdk-dotnet.md). De .NET SDK wordt geleverd met een LINQ-provider.

> [!TIP]
> U kunt de SQL-API uitproberen en SQL-query's uitvoeren op onze gegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).
> 
> 

## <a name="multi-document-transactions"></a>Transacties met meerdere documenten
Databasetransacties bieden een veilige en voorspelbare programmeermodel voor het omgaan met gelijktijdige wijzigingen in de gegevens. In RDBMS, de traditionele manier om te schrijven van bedrijfsregels in te schakelen, is het schrijven van **opgeslagen procedures** en/of **triggers** en verzendt het naar de database-server voor de transactionele uitvoering. In RDBMS, wordt de toepassing programmeurs vereist om te gaan met twee verschillende programmeertalen: 

* De (niet-transactionele) toepassing programmeertaal (bijvoorbeeld, JavaScript, Python, C#, Java, enz.)
* T-SQL, de transactionele programmeertaal die wordt standaard uitgevoerd door de database

Doordat de grondige inzet voor JavaScript en JSON rechtstreeks in de database-engine, Azure Cosmos DB biedt een intuïtieve programmeermodel voor uitvoerende JavaScript-toepassingslogica rechtstreeks gebaseerd op de verzamelingen in termen van opgeslagen procedures en Triggers. Hiermee kunt u het volgende:

* Efficiënte implementatie van gelijktijdigheid van taken worden beheerd, herstel, automatische indexering van de JSON-objectgrafieken rechtstreeks in de database-engine
* Natuurlijk uitdrukken Controlestroom, variabele scoping, toewijzing en integratie van primitieven met databasetransacties rechtstreeks in termen van de programmeertaal JavaScript uitzonderingsverwerking

De JavaScript-logica geregistreerd op het niveau van een verzameling kan databasebewerkingen uitvoeren op de documenten van de opgegeven verzameling vervolgens uitgeven. Azure Cosmos DB verpakt impliciet op basis van de JavaScript opgeslagen procedures en triggers binnen een ambient ACID-transactions met snapshot-isolatie meerdere documenten binnen een verzameling. Als er tijdens de uitvoering een JavaScript-uitzondering optreedt, wordt de volledige transactie afgebroken. Het resulterende programmeermodel is zeer eenvoudig maar krachtige. JavaScript-ontwikkelaars ophalen een 'duurzame' programmeermodel terwijl u ook hun bekend taalconstructs en bibliotheek primitieven   

De mogelijkheid om uit te voeren van JavaScript rechtstreeks in de database-engine in dezelfde adresruimte als de buffergroep maakt zodat en transactionele uitvoering van de activiteiten van de database op de documenten van een verzameling. Bovendien Cosmos DB database-engine maakt een grondige toezegging voor de JSON en JavaScript elimineert impedantie niet overeenkomen tussen de systemen type van de toepassing en de database.   

Nadat u een verzameling maakt, kunt u opgeslagen procedures, triggers en UDF's registreren met een verzameling met de [REST-API's](/rest/api/documentdb/) of een van de [client-SDK's](sql-api-sdk-dotnet.md). U kunt verwijzen naar en ze worden uitgevoerd na de registratie. Houd rekening met de volgende opgeslagen procedure volledig in JavaScript is geschreven, hieronder de code in twee argumenten (naam en de naam van auteur) en maakt een nieuw document, query's voor een document en werkt vervolgens het – alle binnen een impliciete ACID-transactie. Op elk moment tijdens het uitvoeren als een JavaScript-uitzondering is opgetreden, de hele transactie afgebroken.

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

De client kan 'verzenden' de bovenstaande JavaScript-logica voor de database voor transactionele uitvoering via HTTP POST. Zie voor meer informatie over het gebruik van HTTP-methoden [RESTful interacties met Azure Cosmos DB resources](https://msdn.microsoft.com/library/azure/mt622086.aspx). 

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


U ziet dat omdat de database systeemeigen JSON en JavaScript begrijpt, er is geen niet-overeenkomend gegevenstype system, geen 'OR toewijzing' of code generatie magic vereist.   

Opgeslagen procedures en triggers communiceren met een verzameling en de documenten in een verzameling via een goed gedefinieerde objectmodel, waarmee de huidige context van de verzameling.  

Verzamelingen in de SQL-API kunnen worden gemaakt, verwijderd, gelezen of opgesomde eenvoudig met behulp van de [REST-API's](/rest/api/documentdb/) of een van de [client-SDK's](sql-api-sdk-dotnet.md). De SQL-API biedt altijd sterke consistentie voor lezen en het opvragen van de metagegevens van een verzameling. Als u een verzameling verwijdert, wordt automatisch zorgt ervoor dat u geen toegang de documenten, bijlagen, opgeslagen procedures, triggers tot en UDF's daarin.   

## <a name="stored-procedures-triggers-and-user-defined-functions-udf"></a>Opgeslagen procedures, triggers en door gebruiker gedefinieerde functies (UDF)
Zoals beschreven in de vorige sectie, kunt u toepassingslogica rechtstreeks binnen een transactie in de database-engine kunt uitvoeren. De toepassingslogica volledig in JavaScript kan worden geschreven en kan worden gemodelleerd als een opgeslagen procedure, trigger of een UDF. De JavaScript-code binnen een opgeslagen procedure of een trigger kunt invoegen, vervangen, verwijderen, lezen of documenten binnen een verzameling opvragen. JavaScript binnen een UDF kan niet aan de andere kant invoegen, vervangen of verwijderen van documenten. UDF's de documenten van de resultatenset van een query inventariseren en produceren van een andere resultatenset. Voor multitenancy zorgt Azure Cosmos DB een strikte reservering gebaseerde resource governance. Elk opgeslagen procedure, trigger of een UDF opgehaald van een vaste quantum van besturingssysteem resources voor het werk. Bovendien opgeslagen procedures, triggers of UDF's kunnen geen koppeling tegen externe JavaScript-bibliotheken en als deze groter is dan de resource-budgetten toegewezen aan hen zijn gebeurd. U kunt registreren, opgeslagen procedures, triggers of UDF's registratie met een verzameling met de REST-API's.  Bij de registratie een opgeslagen procedure, trigger of een UDF vooraf gecompileerd en opgeslagen als byte-code, die later wordt uitgevoerd. De volgende ssection illustrateshow kunt u de Azure Cosmos DB JavaScript SDK registreert, uit te voeren en hef de registratie van een opgeslagen procedure, trigger en een UDF. De JavaScript-SDK is een eenvoudige wrapper via de [REST-API's](/rest/api/documentdb/). 

### <a name="registering-a-stored-procedure"></a>Registreren van een opgeslagen procedure
De registratie van een opgeslagen procedure maakt een nieuwe resource van de opgeslagen procedure op een verzameling via HTTP POST.  

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
Uitvoering van een opgeslagen procedure wordt gedaan door uitgifte van een HTTP POST op basis van een bestaande resource in de opgeslagen procedure met parameters wordt doorgegeven aan de procedure in de aanvraagtekst.

    var inputDocument = {id : "document1", author: "G. G. Marquez"};
    client.executeStoredProcedureAsync(createdStoredProcedure.resource._self, inputDocument)
        .then(function(executionResult) {
            assert.equal(executionResult, "success - created DOCUMENT1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-stored-procedure"></a>De registratie van een opgeslagen procedure
De registratie van een opgeslagen procedure wordt gewoon door uitgifte van een HTTP DELETE op basis van een bestaande resource in de opgeslagen procedure uitgevoerd.   

    client.deleteStoredProcedureAsync(createdStoredProcedure.resource._self)
        .then(function (response) {
            return;
        }, function(error) {
            console.log("Error");
        });


### <a name="registering-a-pre-trigger"></a>Registratie van een vooraf trigger
De registratie van een trigger wordt uitgevoerd door een nieuwe trigger-resource maken in een verzameling via HTTP POST. U kunt opgeven als de trigger een vooraf is of een trigger post en het type van de bewerking kan het worden gekoppeld aan (bijvoorbeeld: maken, Replace, Delete of alle).   

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

### <a name="executing-a-pre-trigger"></a>Uitvoeren van een vooraf trigger
De uitvoering van een trigger wordt uitgevoerd door de naam van een bestaande trigger opgeven op het moment van de aanvraag POST, PUT, verwijderen van een documentbron via de aanvraagheader uitgeven.  

    client.createDocumentAsync(collection._self, { id: "doc1", key: "Love in the Time of Cholera" }, { preTriggerInclude: "upperCaseId" })
        .then(function(createdDocument) {
            assert.equal(createdDocument.resource.id, "DOC1");
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-pre-trigger"></a>De registratie van een vooraf trigger
De registratie van een trigger wordt gewoon uitgevoerd via het uitgeven van een HTTP DELETE op basis van een bestaande trigger-resource.  

    client.deleteTriggerAsync(createdPreTrigger._self);
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

### <a name="registering-a-udf"></a>Registreren van een UDF
De registratie van een UDF wordt uitgevoerd door een nieuwe UDF-resource maken in een verzameling via HTTP POST.  

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
Een UDF kan worden opgegeven als deel van de SQL-query en wordt gebruikt als een manier om uit te breiden de kern [SQL-querytaal](sql-api-sql-query-reference.md).

    var filterQuery = "SELECT udf.mathSqrt(r.Age) AS sqrtAge FROM root r WHERE r.FirstName='John'";
    client.queryDocuments(collection._self, filterQuery).toArrayAsync();
        .then(function(queryResponse) {
            var queryResponseDocuments = queryResponse.feed;
        }, function(error) {
            console.log("Error");
        });

### <a name="unregistering-a-udf"></a>De registratie van een UDF
De registratie van een UDF gebeurt gewoon door uitgifte van een HTTP DELETE op basis van een bestaande UDF-resource.  

    client.deleteUserDefinedFunctionAsync(createdUdf._self)
        .then(function(response) {
            return;
        }, function(error) {
            console.log("Error");
        });

Hoewel de bovenstaande codefragmenten de registratie (POST), registratie (PUT), (GET) lijst/lezen en uitvoeren (POST) via de [JavaScript SDK](https://github.com/Azure/azure-documentdb-js), u kunt ook de [REST-API's](/rest/api/documentdb/) of andere [client-SDK's](sql-api-sdk-dotnet.md). 

## <a name="documents"></a>Documenten
U kunt invoegen, vervangen, verwijderen, lezen, opsommen en willekeurige JSON-documenten in een verzameling opvragen. Azure Cosmos DB schrijft geen schema niet en vereist geen secundaire indexen ter ondersteuning van uitvoeren van query's over documenten in een verzameling. De maximale grootte voor een document is 2 MB.   

Alleen een echt geopende database-service, komt Azure Cosmos DB niet tot een speciale gegevenstypen (bijvoorbeeld, datum / tijd) of specifieke coderingen voor JSON-documenten. Azure Cosmos DB vereist speciale JSON conventies samen te vatten de relaties tussen verschillende documenten; de SQL-syntaxis van Azure DB die Cosmos biedt een zeer krachtige query voor hiërarchische en relationele operators van de query- en documenten zonder speciale aantekeningen of moeten samen te vatten relaties tussen documenten met elkaar worden onderscheiden eigenschappen.  

Zoals met andere resources, documenten kunnen worden gemaakt, vervangen, verwijderd, lezen, geïnventariseerd en opgevraagd eenvoudig met REST-API's of een van de [client-SDK's](sql-api-sdk-dotnet.md). Verwijderen van een document onmiddellijk vrijgemaakt het quotum dat overeenkomt met alle geneste bijlagen. Het niveau van het lezen van consistentie van documenten volgt het beleid voor consistentiecontrole op de account van de database. Dit beleid kan worden genegeerd op basis van per aanvraag, afhankelijk van de gegevens consistent vereisten van uw toepassing. Tijdens het opvragen van documenten, volgt het lezen van consistentie de indexering modus ingesteld voor de verzameling. Dit volgt de accountbeleid consistentie voor 'consistent'. 

## <a name="attachments-and-media"></a>Bijlagen en media
Azure Cosmos-database kunt u voor het opslaan van binaire blobs/media met Azure Cosmos-DB (maximaal 2 GB per account) of naar uw eigen externe mediastore. Ook kunt u de metagegevens van een medium in termen van een speciale document met de naam van de bijlage vertegenwoordigen. Een bijlage van Azure DB die Cosmos is een speciale (JSON)-document dat verwijst naar de media/blob elders opgeslagen. Een bijlage is gewoon een speciale document dat de metagegevens van een media opgeslagen in de opslag van een externe media (bijvoorbeeld de locatie, de auteur enzovoort) worden vastgelegd. 

Houd rekening met een sociale lezen-toepassing die gebruikmaakt van Azure DB die Cosmos voor het opslaan van aantekeningen en metagegevens, opmerkingen, waaronder markeert, bladwijzers, classificaties, enz. gekoppelde voor een e-book van een bepaalde gebruiker alsof/ervaringen.   

* De inhoud van het rapport zelf wordt opgeslagen in de Mediaopslag ofwel beschikbaar als onderdeel van Azure DB die Cosmos-databaseaccount of een externe mediastore. 
* Een toepassing kan elke gebruiker metagegevens opslaan als een afzonderlijke document--bijvoorbeeld Jan van metagegevens voor Map1 worden opgeslagen in een document waarnaar wordt verwezen door /colls/joe/docs/book1. 
* Bijlagen die verwijst naar de pagina's van een bepaalde boek van een gebruiker worden opgeslagen onder het bijbehorende document bijvoorbeeld inhoud, /colls/joe/docs/book1/chapter1, /colls/joe/docs/book1/chapter2 enzovoort. 

Beschrijvende ID's in de bovenstaande voorbeelden gebruiken om de resource-hiërarchie over te brengen. Bronnen worden geopend via de REST-API's via unieke resource-id. 

Voor de media die wordt beheerd door Azure Cosmos DB, verwijst de eigenschap _media van de bijlage naar de media door de URI. Azure Cosmos DB zorgt voor garbagecollection verzamelen de media als alle uitstaande verwijzingen worden verwijderd. Azure Cosmos-DB automatisch de bijlage wordt gegenereerd tijdens het uploaden van de nieuwe media en de _media om te verwijzen naar de zojuist toegevoegde media gevuld. Als u kiest voor het opslaan van de media in een externe blob-opslag beheerd door uzelf (bijvoorbeeld, OneDrive, Azure Storage, DropBox enzovoort), kunt u de bijlagen nog steeds gebruiken om te verwijzen naar de media. In dit geval u zelf de bijlage maken en vullen van de eigenschap _media.   

Als met alle andere resources, kunnen de bijlagen worden gemaakt, vervangen, verwijderd, lezen of opgesomd eenvoudig met behulp van REST-API's of een van de client-SDK's. Net als bij documenten, volgt het niveau van het lezen van consistentie van bijlagen het beleid voor consistentie op account van de database. Dit beleid kan worden genegeerd op basis van per aanvraag, afhankelijk van de gegevens consistent vereisten van uw toepassing. Bij het opvragen van bijlagen, volgt het lezen van consistentie de indexering modus ingesteld voor de verzameling. Dit volgt de accountbeleid consistentie voor 'consistent'. 
 

## <a name="users"></a>Gebruikers
Een gebruiker Azure Cosmos DB vertegenwoordigt een logische naamruimte voor het groeperen van machtigingen. Een gebruiker met Azure Cosmos DB mogelijk komen overeen met een gebruiker in een identiteitsbeheersysteem of een vooraf gedefinieerde toepassingsrol. Voor Azure Cosmos DB vertegenwoordigt een gebruiker gewoon een abstractie voor het groeperen van een reeks machtigingen onder een database.   

Voor het implementeren van multi-tenancymodus in uw toepassing, kunt u gebruikers maken in Azure Cosmos DB, wat overeenkomt met uw werkelijke gebruikers- of de tenants van uw toepassing. Vervolgens kunt u machtigingen voor een bepaalde gebruiker die met het toegangsbeheer via verschillende verzamelingen, documenten, bijlagen, enzovoort overeenkomen.   

Als uw toepassingen moeten worden uitgebreid met de groei van uw gebruiker, kunt u uw gegevens kunt op verschillende manieren shard vast. U kunt een model van elk van uw gebruikers als volgt:   

* Elke gebruiker is toegewezen aan een database.
* Elke gebruiker is toegewezen aan een verzameling. 
* Overeenkomt met meerdere gebruikers documenten gaat u naar een specifieke verzameling. 
* Overeenkomt met meerdere gebruikers documenten gaat u naar een verzameling van verzamelingen.   

Ongeacht de specifieke sharding-strategie kiest u, kunt u het model van uw werkelijke gebruikers als gebruikers in Azure DB die Cosmos-database en fijnmazige machtigingen aan elke gebruiker koppelen.  

![Verzamelingen van gebruikers][3]  
**Sharding-strategieën en modellering gebruikers**

Net als alle andere resources, gebruikers in Azure Cosmos DB kunnen worden gemaakt, vervangen, verwijderd, lezen of opgesomd eenvoudig met behulp van REST-API's of een van de client-SDK's. Azure Cosmos DB geeft altijd sterke consistentie voor lezen en het opvragen van de metagegevens van een gebruiker een resource. Wordt het bewerken van wijzen dat een gebruiker automatisch verwijderen zorgt ervoor dat u geen toegang de machtigingen van deze tot. Hoewel de Cosmos Azure DB maakt het quotum van de machtigingen als onderdeel van de verwijderde gebruiker op de achtergrond, de verwijderde machtigingen weer beschikbaar is onmiddellijk moet worden gebruikt.  

## <a name="permissions"></a>Machtigingen
Vanuit een access control-perspectief-resources, zoals accounts van de database, databases, gebruikers en machtigingen worden beschouwd als *beheerdersrechten* bronnen omdat deze is vereist voor beheerdersmachtigingen nodig. Aan de andere kant resources inclusief de verzamelingen, documenten, bijlagen, opgeslagen procedures, triggers, UDF's zijn en gericht onder een bepaalde database en beschouwd als *toepassingsresources*. Het model autorisatie overeenkomt met de twee typen resources en de functies die toegang deze (namelijk de beheerder en gebruiker tot), definieert twee soorten *toegangssleutels*: *hoofdsleutel* en  *Bronsleutel*. De hoofdsleutel deel uitmaakt van de account van de database en is opgegeven voor de ontwikkelaars (of de administrator) die het databaseaccount is ingericht. Deze hoofdsleutel heeft administrator-semantiek in dat deze toegang verlenen aan bronnen administratieve en een toepassing kan worden gebruikt. Een resource-sleutel is daarentegen een gedetailleerde toegangssleutel die toegang tot verleent een *specifieke* toepassingsbron. Daarom wordt de relatie tussen de gebruiker van een database en de machtigingen die de gebruiker voor een specifieke bron (bijvoorbeeld verzameling, document, bijlage, opgeslagen procedure, trigger of UDF heeft) vastgelegd.   

De enige manier om het ophalen van een resource-sleutel is door het maken van een resource machtiging onder een bepaalde gebruiker. Houd er rekening mee dat als u wilt maken of een machtiging voor het ophalen, een hoofdsleutel in de autorisatie-header moet worden gepresenteerd. Een machtiging resource verbindt de bron, de toegang en de gebruiker. Na het maken van een resource machtiging, moet de gebruiker alleen de bijbehorende bronsleutel aanwezig om toegang tot de relevante resource te krijgen. Een resource-sleutel kan daarom als een logische en compacte weergave van de machtiging resource worden weergegeven.  

Zoals met andere resources, machtigingen in Azure Cosmos DB kunnen worden gemaakt, vervangen, verwijderd, lezen of opgesomd eenvoudig met behulp van REST-API's of een van de client-SDK's. Azure Cosmos DB geeft altijd sterke consistentie voor lezen of de metagegevens van een machtiging voor het uitvoeren van query's. 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het werken met resources met behulp van HTTP-opdrachten in [RESTful interacties met Azure Cosmos DB resources](https://msdn.microsoft.com/library/azure/mt622086.aspx).

[1]: media/sql-api-resources/resources1.png
[2]: media/sql-api-resources/resources2.png
[3]: media/sql-api-resources/resources3.png

