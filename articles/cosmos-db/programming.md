---
title: Programmeren-server JavaScript voor Azure Cosmos DB | Microsoft Docs
description: Informatie over het gebruik van Azure DB die Cosmos om te schrijven van opgeslagen procedures, databasetriggers en de gebruiker gedefinieerde functies (UDF's) in JavaScript. Gebruik database programing tips en meer.
keywords: Database-triggers, opgeslagen procedure, opgeslagen procedure, database-programma, sproc, azure, Microsoft azure
services: cosmos-db
documentationcenter: 
author: aliuy
manager: jhubbard
editor: mimig
ms.assetid: 0fba7ebd-a4fc-4253-a786-97f1354fbf17
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: andrl
ms.openlocfilehash: d8438d126c1f994e51871e80bb11610ec95b0814
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Azure DB Cosmos programmeren-server: opgeslagen procedures, databasetriggers en UDF's

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Meer informatie over hoe Azure Cosmos DB taal geïntegreerd, transactionele uitvoering van JavaScript kan ontwikkelaars schrijven **opgeslagen procedures**, **triggers** en **de gebruiker gedefinieerde functies (UDF's)** systeemeigen in een [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript. Hiermee kunt u toepassingslogica van het database-programma dat kan worden verzonden en rechtstreeks uitgevoerd op de database-opslag-partities schrijven. 

Het is raadzaam om aan de slag door het bekijken van de volgende video, waarbij Andrew Liu een korte inleiding tot programmeermodel Cosmos-database-server-side '-database bevat. 

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Demo-A-Quick-Intro-to-Azure-DocumentDBs-Server-Side-Javascript/player]
> 
> 

Keer vervolgens terug naar dit artikel, waarin u leert de antwoorden op de volgende vragen:  

* Hoe schrijf ik een een opgeslagen procedure, trigger of UDF met JavaScript?
* Hoe garandeert Cosmos DB ACID
* Hoe werken transacties in Cosmos-database?
* Wat zijn vooraf activeert en na activeert en hoe schrijf ik een?
* Hoe ik registreren en uitvoeren van een opgeslagen procedure, trigger of UDF in een RESTful manier met behulp van HTTP?
* Wat Cosmos DB SDK's zijn beschikbaar voor het maken en uitvoeren, opgeslagen procedures, triggers en UDF's?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Inleiding tot de opgeslagen Procedure en UDF programmering
Deze benadering van *'JavaScript als een moderne dag T-SQL'* toepassingsontwikkelaars van de complexiteit van het systeem niet-overeenkomende typen en -technologieën voor object-relationele gegevens maakt. Er wordt ook een aantal ingebouwde voordelen die kunnen worden gebruikt om uitgebreide toepassingen te bouwen:  

* **Procedurele logica:** JavaScript als een hoog niveau programmeertaal biedt een uitgebreide en vertrouwd interface bedrijfslogica Express. U kunt complexe reeksen operations dichter in de gegevens kunt uitvoeren.
* **Atomische transacties:** Cosmos DB wordt gegarandeerd dat de bewerkingen die worden uitgevoerd binnen een enkele opgeslagen procedure of trigger database atomic zijn. Hiermee kunt een toepassing gerelateerde bewerkingen in één batch combineren zodat ze allemaal slagen of geen van beide slagen. 
* **Prestaties:** het feit dat JSON intrinsiek is toegewezen aan het typesysteem Javascript-taal en ook de basiseenheid voor opslag in Cosmos DB is kunt u een aantal optimalisaties zoals vertraagde materialisatie van JSON-documenten in de buffergroep en ze op verzoek beschikbaar voor het uitvoeren van code. Er zijn meer prestatievoordelen die zijn gekoppeld aan back-upfunctie bedrijfsregels in te schakelen naar de database:
  
  * Batchverwerking: ontwikkelaars kunnen groep bewerkingen zoals ingevoegd en deze bulksgewijs te verzenden. De netwerklatentie verkeer de kosten en de store-overhead voor het maken van afzonderlijke transacties aanzienlijk verminderd. 
  * Vooraf compilatie – Cosmos DB precompiles opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies (UDF's) om te voorkomen dat JavaScript compilatie kosten voor elke aanroep. De overhead van het bouwen van de byte-code voor de procedurele logica is afgelost op een minimale waarde.
  * Sequentiëren – veel bewerkingen moet een neveneffect ('trigger') die mogelijk het uitvoeren van een of meer secundaire store-bewerkingen. Behalve atomisch, maar dit is meer zodat wanneer verplaatst naar de server. 
* **Inkapseling:** opgeslagen procedures kunnen worden gebruikt voor het groeperen van bedrijfsregels in te schakelen op één plek. Dit heeft twee voordelen:
  * Een abstractielaag boven op de onbewerkte gegevens, zodat gegevens architecten ontwikkelen hun toepassingen onafhankelijk van de gegevens worden toegevoegd. Dit is vooral nuttig wanneer de gegevens zonder schema, vanwege de brosse veronderstellingen die worden standaard uitbreidbaar in de toepassing moeten mogelijk als ze hebben om te gaan met de gegevens rechtstreeks.  
  * Deze abstractie kan bedrijven hun gegevens beveiligen door af te stroomlijnen de toegang van de scripts.  

Het maken en de uitvoering van de databasetriggers, opgeslagen procedure en aangepaste query's wordt ondersteund door de [Azure-portal](https://portal.azure.com), wordt de [REST-API](/rest/api/documentdb/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), en [client-SDK's](sql-api-sdk-dotnet.md) op verschillende platforms, waaronder .NET, Node.js en JavaScript.

Deze zelfstudie wordt gebruikgemaakt van de [Node.js-SDK met Q](http://azure.github.io/azure-documentdb-node-q/) ter illustratie van de syntaxis en het gebruik van opgeslagen procedures, triggers en UDF's.   

## <a name="stored-procedures"></a>Opgeslagen procedures
### <a name="example-write-a-simple-stored-procedure"></a>Voorbeeld: Een eenvoudige opgeslagen procedure schrijven
Laten we beginnen met een eenvoudige opgeslagen procedure die een antwoord 'Hallo wereld' geretourneerd.

    var helloWorldStoredProc = {
        id: "helloWorld",
        serverScript: function () {
            var context = getContext();
            var response = context.getResponse();

            response.setBody("Hello, World");
        }
    }


Opgeslagen procedures worden geregistreerd per verzameling en kunnen worden uitgevoerd op een document en bijlage aanwezig zijn in die verzameling. Het volgende fragment toont hoe de helloWorld opgeslagen procedure registreren bij een verzameling. 

    // register the stored procedure
    var createdStoredProcedure;
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
        .then(function (response) {
            createdStoredProcedure = response.resource;
            console.log("Successfully created stored procedure");
        }, function (error) {
            console.log("Error", error);
        });


Zodra de opgeslagen procedure is geregistreerd, kunnen we uitgevoerd op de verzameling en bekijk de resultaten weer op de client. 

    // execute the stored procedure
    client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
        .then(function (response) {
            console.log(response.result); // "Hello, World"
        }, function (err) {
            console.log("Error", error);
        });


Het contextobject biedt toegang tot alle bewerkingen die kunnen worden uitgevoerd op Cosmos databaseopslag, evenals de toegang tot de aanvraag en antwoord-objecten. In dit geval wordt het object response gebruikt om de hoofdtekst van het antwoord dat is verzonden naar de client. Raadpleeg voor meer informatie de [Azure Cosmos DB JavaScript server SDK-documentatie](http://azure.github.io/azure-documentdb-js-server/).  

Laat het ons uitvouwt op dit voorbeeld en voeg dat meer verwante functionaliteit aan de opgeslagen procedure-database. Opgeslagen procedures kunnen maken, bijwerken, lezen, opvragen en verwijderen van documenten en bijlagen binnen de verzameling.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Voorbeeld: Een opgeslagen procedure voor het maken van een document schrijven
Het volgende fragment toont hoe het context-object gebruiken om te communiceren met de resources van de Cosmos-DB.

    var createDocumentStoredProc = {
        id: "createMyDocument",
        serverScript: function createMyDocument(documentToCreate) {
            var context = getContext();
            var collection = context.getCollection();

            var accepted = collection.createDocument(collection.getSelfLink(),
                  documentToCreate,
                  function (err, documentCreated) {
                      if (err) throw new Error('Error' + err.message);
                      context.getResponse().setBody(documentCreated.id)
                  });
            if (!accepted) return;
        }
    }


Deze opgeslagen procedure neemt als invoer documentToCreate, de hoofdtekst van een document moet worden gemaakt in de huidige verzameling. Alle dergelijke bewerkingen zijn asynchroon en zijn afhankelijk van retouraanroepen van JavaScript-functie. De callbackfunctie heeft twee parameters, één voor het foutobject in geval mislukt de bewerking en één voor het gemaakte object. Binnen de callback kunnen gebruikers de uitzondering verwerken of een fout. Als een retouraanroep niet opgegeven is en er een fout is, wordt in de Azure DB die Cosmos-runtime een fout genereert.   

De callback genereert een fout opgetreden in het bovenstaande voorbeeld als de bewerking is mislukt. Anders wordt de id van het gemaakte document als de berichttekst van het antwoord op de client ingesteld. Hier ziet u hoe u deze opgeslagen procedure uitvoeren met de invoerparameters.

    // register the stored procedure
    client.createStoredProcedureAsync('dbs/testdb/colls/testColl', createDocumentStoredProc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;

            // run stored procedure to create a document
            var docToCreate = {
                id: "DocFromSproc",
                book: "The Hitchhiker’s Guide to the Galaxy",
                author: "Douglas Adams"
            };

            return client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/createMyDocument',
                  docToCreate);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response); // "DocFromSproc"
    }, function (error) {
        console.log("Error", error);
    });


Houd er rekening mee dat deze opgeslagen procedure kan worden aangepast voor het uitvoeren van een matrix van document instanties als invoer en maak ze allemaal in dezelfde opgeslagen procedure uitvoering in plaats van meerdere netwerkaanvragen ze allemaal afzonderlijk maken. Dit kan worden gebruikt voor het implementeren van een efficiënte bulksgewijs importfunctie voor Cosmos-DB (Zie verderop in deze zelfstudie).   

In het voorbeeld dat wordt beschreven hoe u opgeslagen procedures gedemonstreerd. Wordt uitgelegd triggers en door de gebruiker gedefinieerde functies (UDF's) verderop in de zelfstudie.

## <a name="database-program-transactions"></a>Programma databasetransacties
Transactie in een typische database kan worden gedefinieerd als een reeks bewerkingen die worden uitgevoerd als één logische eenheid van het werk. Elke transactie biedt **ACID garanties**. ACID is een bekende acroniem die voor de vier eigenschappen - atomisch, consistentie, isolatie en duurzaamheid staat.  

Kort samengevat kunt atomisch zorgt ervoor dat alle het werk dat binnen een transactie wordt behandeld als één eenheid waar beide alles is doorgevoerd of none. Consistentie zorgt ervoor dat de gegevens altijd in goede staat verkeren interne over transacties is. Isolatie zorgt ervoor dat geen twee transacties verstoren elkaar – algemeen, de meeste commerciële systemen Geef meerdere isolatieniveaus die kunnen worden gebruikt op basis van de behoeften van de toepassing. Duurzaamheid zorgt ervoor dat elke wijziging die vastgelegd in de database altijd aanwezig zijn.   

JavaScript wordt in Cosmos-database gehost in de geheugenruimte als de database. Daarom aanvragen uitgevoerd in opgeslagen procedures en triggers in hetzelfde bereik van een databasesessie uitvoeren. Hierdoor Cosmos DB ACID voor alle bewerkingen die deel van één opgeslagen procedure/trigger uitmaken te garanderen. Houd rekening met de volgende opgeslagen procedure-definitie:

    // JavaScript source code
    var exchangeItemsSproc = {
        id: "exchangeItems",
        serverScript: function (playerId1, playerId2) {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            var player1Document, player2Document;

            // query for players
            var filterQuery = 'SELECT * FROM Players p where p.id  = "' + playerId1 + '"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery, {},
                function (err, documents, responseOptions) {
                    if (err) throw new Error("Error" + err.message);

                    if (documents.length != 1) throw "Unable to find both names";
                    player1Document = documents[0];

                    var filterQuery2 = 'SELECT * FROM Players p where p.id = "' + playerId2 + '"';
                    var accept2 = collection.queryDocuments(collection.getSelfLink(), filterQuery2, {},
                        function (err2, documents2, responseOptions2) {
                            if (err2) throw new Error("Error" + err2.message);
                            if (documents2.length != 1) throw "Unable to find both names";
                            player2Document = documents2[0];
                            swapItems(player1Document, player2Document);
                            return;
                        });
                    if (!accept2) throw "Unable to read player details, abort ";
                });

            if (!accept) throw "Unable to read player details, abort ";

            // swap the two players’ items
            function swapItems(player1, player2) {
                var player1ItemSave = player1.item;
                player1.item = player2.item;
                player2.item = player1ItemSave;

                var accept = collection.replaceDocument(player1._self, player1,
                    function (err, docReplaced) {
                        if (err) throw "Unable to update player 1, abort ";

                        var accept2 = collection.replaceDocument(player2._self, player2,
                            function (err2, docReplaced2) {
                                if (err) throw "Unable to update player 2, abort"
                            });

                        if (!accept2) throw "Unable to update player 2, abort";
                    });

                if (!accept) throw "Unable to update player 1, abort";
            }
        }
    }

    // register the stored procedure in Node.js client
    client.createStoredProcedureAsync(collection._self, exchangeItemsSproc)
        .then(function (response) {
            var createdStoredProcedure = response.resource;
        }
    );

Deze opgeslagen procedure maakt gebruik van transacties binnen een gaming-app handel items tussen twee spelers in één bewerking. De opgeslagen procedure probeert te lezen van de twee documenten elke overeenkomt met de player-id's als argument doorgegeven. Als beide player-documenten zijn gevonden, werkt de opgeslagen procedure de documenten door hun items wisselen. Als er fouten langs de manier optreden, genereert het een JavaScript-uitzondering die impliciet afbreken van de transactie.

Als de verzameling de opgeslagen procedure is geregistreerd op basis van een verzameling van één partitie, wordt de transactie is afgestemd op de documenten binnen de verzameling. Als de verzameling is gepartitioneerd, worden de opgeslagen procedures in het transactiebereik van een enkele partitiesleutel uitgevoerd. Elke opgeslagen procedure-uitvoering moet een waarde voor de partitiesleutel overeenkomt met het bereik dat de transactie moet worden uitgevoerd onder vervolgens bevatten. Zie voor meer informatie [Azure Cosmos DB partitioneren](partition-data.md).

### <a name="commit-and-rollback"></a>Commit- en rollback
Transacties zijn systeemeigen en nauw geïntegreerd in Cosmos-database JavaScript-programmeermodel. Binnen een JavaScript-functie, worden alle bewerkingen automatisch ingepakt onder één transactie. Als JavaScript zonder een uitzondering is voltooid, wordt de bewerkingen in de database zijn doorgevoerd. De instructies 'BEGIN TRANSACTION' en 'COMMIT TRANSACTION' in relationele databases zijn in feite impliciete in Cosmos-database.  

Als er een uitzondering die wordt doorgegeven vanuit het script, wordt Cosmos-database JavaScript-runtime Draai de hele transactie terug. Zoals u in het vorige voorbeeld, is het effectief gelijk is aan een 'ROLLBACK TRANSACTION' toe aan DB Cosmos om uitzondering opgetreden.

### <a name="data-consistency"></a>Gegevensconsistentie
Opgeslagen procedures en triggers worden altijd uitgevoerd op de primaire replica van de Azure DB die Cosmos-container. Dit zorgt ervoor dat leesbewerkingen van binnen procedures aanbieding sterke consistentie opgeslagen. Query's op basis van de gebruiker gedefinieerde functies kunnen worden uitgevoerd op de primaire of elke secundaire replica, maar we ervoor zorgen dat de aangevraagde consistentieniveau te voldoen aan de juiste replica kiezen.

## <a name="bounded-execution"></a>Begrensde uitvoering
Alle bewerkingen van de Cosmos-DB moeten voltooid binnen de opgegeven server time-outduur aanvragen. Deze beperking geldt ook voor JavaScript-functies (opgeslagen procedures, triggers en gebruiker gedefinieerde functies). Als een bewerking niet wordt voltooid met deze termijn, de transactie is teruggedraaid. JavaScript-functies moeten worden voltooid binnen de tijdslimiet of implementeren van een voortzetting op basis van model voor het uitvoeren van batch/hervatten.  

Alle functies in het verzamelingsobject (voor het maken, lezen, vervangen en verwijderen van documenten en bijlagen) retourneren om de ontwikkeling van opgeslagen procedures en triggers voor het afhandelen van de tijdslimiet vereenvoudigen, een Booleaanse waarde die aangeeft of die voor deze bewerking wordt voltooid. Als deze waarde false is, is het een indicatie dat de tijdslimiet is bijna verlopen en dat de procedure van uitvoering inpakken moet.  Bewerkingen in de wachtrij vóór de eerste niet-geaccepteerde-bewerking te voltooien als u de opgeslagen procedure is voltooid in de tijd en niet in een wachtrij geen aanvullende aanvragen meer zijn gegarandeerd.  

JavaScript-functies worden ook begrensd op resourceverbruik. Cosmos DB reserveert doorvoer per verzameling op basis van de ingerichte grootte van een databaseaccount. Doorvoer wordt uitgedrukt in termen van genormaliseerde eenheid van de CPU, geheugen en i/o-verbruik aanvraageenheden of RUs genoemd. JavaScript-functies kunnen gebruiken om een groot aantal RUs binnen een korte periode en mogelijk ophalen snelheid beperkte als de verzameling limiet is bereikt. Resource intensieve opgeslagen procedures kunnen ook in quarantaine zodat de beschikbaarheid van primitieve databasebewerkingen.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Voorbeeld: Bulksgewijs importeren van gegevens in een database-programma
Hieronder volgt een voorbeeld van een opgeslagen procedure die is geschreven naar bulkimport documenten in een verzameling. Houd er rekening mee hoe de opgeslagen procedure begrensde uitvoering verwerkt door het controleren van de Booleaanse waarde retourneren vanuit createDocument, en gebruikt vervolgens het aantal documenten dat is ingevoegd in elke aanroep van de opgeslagen procedure volgen en voortgang in batches hervatten.

    function bulkImport(docs) {
        var collection = getContext().getCollection();
        var collectionLink = collection.getSelfLink();

        // The count of imported docs, also used as current doc index.
        var count = 0;

        // Validate input.
        if (!docs) throw new Error("The array is undefined or null.");

        var docsLength = docs.length;
        if (docsLength == 0) {
            getContext().getResponse().setBody(0);
        }

        // Call the create API to create a document.
        tryCreate(docs[count], callback);

        // Note that there are 2 exit conditions:
        // 1) The createDocument request was not accepted. 
        //    In this case the callback will not be called, we just call setBody and we are done.
        // 2) The callback was called docs.length times.
        //    In this case all documents were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
        function tryCreate(doc, callback) {
            var isAccepted = collection.createDocument(collectionLink, doc, callback);

            // If the request was accepted, callback will be called.
            // Otherwise report current count back to the client, 
            // which will call the script again with remaining set of docs.
            if (!isAccepted) getContext().getResponse().setBody(count);
        }

        // This is called when collection.createDocument is done in order to process the result.
        function callback(err, doc, options) {
            if (err) throw err;

            // One more document has been inserted, increment the count.
            count++;

            if (count >= docsLength) {
                // If we created all documents, we are done. Just set the response.
                getContext().getResponse().setBody(count);
            } else {
                // Create next document.
                tryCreate(docs[count], callback);
            }
        }
    }

## <a id="trigger"></a>Databasetriggers
### <a name="database-pre-triggers"></a>Vooraf databasetriggers
Cosmos DB biedt triggers die zijn uitgevoerd of geactiveerd door een bewerking op een document. U kunt bijvoorbeeld een vooraf trigger opgeven wanneer u bij het maken van een document: deze vooraf trigger wordt uitgevoerd voordat het document wordt gemaakt. Hier volgt een voorbeeld van hoe vooraf triggers kunnen worden gebruikt voor het valideren van de eigenschappen van een document dat wordt gemaakt:

    var validateDocumentContentsTrigger = {
        id: "validateDocumentContents",
        serverScript: function validate() {
            var context = getContext();
            var request = context.getRequest();

            // document to be created in the current operation
            var documentToCreate = request.getBody();

            // validate properties
            if (!("timestamp" in documentToCreate)) {
                var ts = new Date();
                documentToCreate["my timestamp"] = ts.getTime();
            }

            // update the document that will be created
            request.setBody(documentToCreate);
        },
        triggerType: TriggerType.Pre,
        triggerOperation: TriggerOperation.Create
    }


En de bijbehorende Node.js-clientzijde registratiecode voor de trigger:

    // register pre-trigger
    client.createTriggerAsync(collection.self, validateDocumentContentsTrigger)
        .then(function (response) {
            console.log("Created", response.resource);
            var docToCreate = {
                id: "DocWithTrigger",
                event: "Error",
                source: "Network outage"
            };

            // run trigger while creating above document 
            var options = { preTriggerInclude: "validateDocumentContents" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function (error) {
            console.log("Error", error);
        })
    .then(function (response) {
        console.log(response.resource); // document with timestamp property added
    }, function (error) {
        console.log("Error", error);
    });


Vooraf triggers kunnen geen invoer parameters hebben. Het request-object kan worden gebruikt voor het bewerken van het aanvraagbericht die zijn gekoppeld aan de bewerking. Hier worden de vooraf trigger wordt uitgevoerd met het maken van een document en de berichttekst van de aanvraag bevat het document moet worden gemaakt in JSON-indeling.   

Wanneer triggers zijn geregistreerd, kunnen gebruikers de bewerkingen die kan worden uitgevoerd met opgeven. Deze trigger is gemaakt met TriggerOperation.Create, wat betekent dat het volgende is niet toegestaan.

    var options = { preTriggerInclude: "validateDocumentContents" };

    client.replaceDocumentAsync(docToReplace.self,
                  newDocBody, options)
    .then(function (response) {
        console.log(response.resource);
    }, function (error) {
        console.log("Error", error);
    });

    // Fails, can’t use a create trigger in a replace operation

### <a name="database-post-triggers"></a>Na databasetriggers
Na triggers, zoals pre triggers zijn gekoppeld aan een bewerking op een document en geen invoerparameters bevatten geen onderneemt. Ze worden uitgevoerd **nadat** de bewerking is voltooid en hebben toegang tot het response-bericht dat naar de client wordt verzonden.   

Het volgende voorbeeld ziet u na triggers in actie:

    var updateMetadataTrigger = {
        id: "updateMetadata",
        serverScript: function updateMetadata() {
            var context = getContext();
            var collection = context.getCollection();
            var response = context.getResponse();

            // document that was created
            var createdDocument = response.getBody();

            // query for metadata document
            var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
            var accept = collection.queryDocuments(collection.getSelfLink(), filterQuery,
                updateMetadataCallback);
            if(!accept) throw "Unable to update metadata, abort";

            function updateMetadataCallback(err, documents, responseOptions) {
                if(err) throw new Error("Error" + err.message);
                         if(documents.length != 1) throw 'Unable to find metadata document';

                         var metadataDocument = documents[0];

                         // update metadata
                         metadataDocument.createdDocuments += 1;
                         metadataDocument.createdNames += " " + createdDocument.id;
                         var accept = collection.replaceDocument(metadataDocument._self,
                               metadataDocument, function(err, docReplaced) {
                                      if(err) throw "Unable to update metadata, abort";
                               });
                         if(!accept) throw "Unable to update metadata, abort";
                         return;                    
            }                                                                                            
        },
        triggerType: TriggerType.Post,
        triggerOperation: TriggerOperation.All
    }


De trigger kan worden geregistreerd, zoals wordt weergegeven in het volgende voorbeeld.

    // register post-trigger
    client.createTriggerAsync('dbs/testdb/colls/testColl', updateMetadataTrigger)
        .then(function(createdTrigger) { 
            var docToCreate = { 
                name: "artist_profile_1023",
                artist: "The Band",
                albums: ["Hellujah", "Rotators", "Spinning Top"]
            };

            // run trigger while creating above document 
            var options = { postTriggerInclude: "updateMetadata" };

            return client.createDocumentAsync(collection.self,
                  docToCreate, options);
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });


Deze trigger wordt gevraagd om het metagegevensdocument en bijgewerkt met informatie over het zojuist gemaakte document.  

Één ding dat is belangrijk te weten is de **transactionele** uitvoering van triggers in Cosmos-database. Deze na trigger wordt uitgevoerd als onderdeel van de dezelfde transactie als het maken van het originele document. Dus als we Veroorzaak een uitzondering van de na trigger (zeg als we niet bijwerken van het metagegevensdocument), de hele transactie zal mislukken en worden teruggedraaid. Er is geen document wordt gemaakt en wordt een uitzondering geretourneerd.  

## <a id="udf"></a>Gebruiker gedefinieerde functies
Gebruiker gedefinieerde functies (UDF's) worden gebruikt voor het uitbreiden van de Azure Cosmos DB SQL query language-grammatica en aangepaste bedrijfslogica implementeren. Ze kunnen alleen worden aangeroepen vanuit in query's. Ze hebben geen toegang tot het contextobject en zijn bedoeld om te worden gebruikt als JavaScript compute alleen-lezen. Daarom kunnen UDF's worden uitgevoerd op secundaire replica's van de Cosmos-DB-service.  

Het volgende voorbeeld maakt een UDF voor het berekenen van de fiscale op basis van de tarieven voor verschillende inkomsten haken en vervolgens alle mensen die meer dan 20.000 $ betaald belastingen te vinden binnen een query gebruikt.

    var taxUdf = {
        id: "tax",
        serverScript: function tax(income) {

            if(income == undefined) 
                throw 'no input';

            if (income < 1000) 
                return income * 0.1;
            else if (income < 10000) 
                return income * 0.2;
            else
                return income * 0.4;
        }
    }


De UDF kan vervolgens worden gebruikt in query's zoals in het volgende voorbeeld:

    // register UDF
    client.createUserDefinedFunctionAsync('dbs/testdb/colls/testColl', taxUdf)
        .then(function(response) { 
            console.log("Created", response.resource);

            var query = 'SELECT * FROM TaxPayers t WHERE udf.tax(t.income) > 20000'; 
            return client.queryDocuments('dbs/testdb/colls/testColl',
                   query).toArrayAsync();
        }, function(error) {
            console.log("Error" , error);
        })
    .then(function(response) {
        var documents = response.feed;
        console.log(response.resource); 
    }, function(error) {
        console.log("Error" , error);
    });

## <a name="javascript-language-integrated-query-api"></a>JavaScript-taal geïntegreerd query API
Naast het uitgeven van query's met behulp van de SQL-grammatica Azure Cosmos DB, kunt de SDK-serverzijde u geoptimaliseerde query's met een beheersen JavaScript-interface zonder dat daarvoor kennis van SQL kan uitvoeren. De JavaScript-query die API u programmatisch query's opbouwen kunt door predikaat functies in de chainable functie aanroept met een bekende ECMAScript5 van matrix dient te worden en populaire JavaScript-bibliotheken zoals lodash syntaxis. Query's worden geparseerd door de JavaScript-runtime efficiënt met behulp van Azure Cosmos DB indices worden uitgevoerd.

> [!NOTE]
> `__`(double liggend streepje) is een alias voor `getContext().getCollection()`.
> <br/>
> Met andere woorden, u kunt `__` of `getContext().getCollection()` voor toegang tot de query die JavaScript API.
> 
> 

Ondersteunde functies omvatten:

<ul>
<li>
<b>... chain(). waarde ([retouraanroep] [, opties])</b>
<ul>
<li>
Start een keten-aanroep die moet worden afgesloten met value().
</li>
</ul>
</li>
<li>
<b>filter (predicateFunction [, opties] [, callback])</b>
<ul>
<li>
Filtert de invoer met behulp van een predicaatfunctie die resulteert in waar/onwaar om te filteren in/out invoerdocumenten in de resulterende set. Dit is het gedrag vergelijkbaar met een WHERE-component in SQL.
</li>
</ul>
</li>
<li>
<b>kaart (transformationFunction [, opties] [, callback])</b>
<ul>
<li>
Van toepassing is een projectie een transformatiefunctie waarbij elk item invoer wordt toegewezen aan een JavaScript-object of een waarde opgegeven. Dit is het gedrag vergelijkbaar met een SELECT-clausule in SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, opties] [, callback])</b>
<ul>
<li>
Dit is een snelkoppeling naar een kaart waarmee de waarde van één eigenschap worden opgehaald van elk item invoer.
</li>
</ul>
</li>
<li>
<b>plat ([isShallow] [, opties] [, callback])</b>
<ul>
<li>
Combineert en matrices van elk item invoer in op één array worden samengevoegd. Dit is het gedrag vergelijkbaar met SelectMany in LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predicate] [, opties] [, callback])</b>
<ul>
<li>
Een nieuwe reeks documenten produceren de documenten in de stroom invoerdocument in oplopende volgorde met behulp van het gegeven predicaat wordt gesorteerd. Dit is het gedrag vergelijkbaar met een component ORDER BY van SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predicate] [, opties] [, callback])</b>
<ul>
<li>
Een nieuwe reeks documenten produceren de documenten in de stroom invoerdocument in aflopende volgorde, met behulp van het gegeven predicaat wordt gesorteerd. Dit is het gedrag vergelijkbaar met een x DESC ORDER BY-clausule in SQL.
</li>
</ul>
</li>
</ul>


Wanneer opgenomen binnen een predikaat en/of selector-functies, krijgen automatisch de volgende JavaScript-constructs geoptimaliseerd direct op Azure Cosmos DB indexen uitvoeren:

* Eenvoudige operators: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Letterlijke waarden, met inbegrip van het object letterlijke: {}
* var, return

De volgende JavaScript-constructs komen niet ophalen geoptimaliseerd voor Azure Cosmos DB indexen:

* Transportbesturing (bijvoorbeeld als, terwijl)
* Functieaanroepen

Zie voor meer informatie onze [serverzijde JSDocs](http://azure.github.io/azure-documentdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Voorbeeld: Een opgeslagen procedure met behulp van de query die JavaScript API schrijven
Het volgende codevoorbeeld is een voorbeeld van hoe de JavaScript-API-Query kan worden gebruikt in de context van een opgeslagen procedure. De opgeslagen procedure voegt een document, dat is opgegeven door een invoerparameter en updates van metagegevens van een document, met behulp van de `__.filter()` methode met minimaal, maxSize en totalSize op basis van de eigenschap size het invoerdocument.

    /**
     * Insert actual doc and update metadata doc: minSize, maxSize, totalSize based on doc.size.
     */
    function insertDocumentAndUpdateMetadata(doc) {
      // HTTP error codes sent to our callback funciton by DocDB server.
      var ErrorCode = {
        RETRY_WITH: 449,
      }

      var isAccepted = __.createDocument(__.getSelfLink(), doc, {}, function(err, doc, options) {
        if (err) throw err;

        // Check the doc (ignore docs with invalid/zero size and metaDoc itself) and call updateMetadata.
        if (!doc.isMetadata && doc.size > 0) {
          // Get the meta document. We keep it in the same collection. it's the only doc that has .isMetadata = true.
          var result = __.filter(function(x) {
            return x.isMetadata === true
          }, function(err, feed, options) {
            if (err) throw err;

            // We assume that metadata doc was pre-created and must exist when this script is called.
            if (!feed || !feed.length) throw new Error("Failed to find the metadata document.");

            // The metadata document.
            var metaDoc = feed[0];

            // Update metaDoc.minSize:
            // for 1st document use doc.Size, for all the rest see if it's less than last min.
            if (metaDoc.minSize == 0) metaDoc.minSize = doc.size;
            else metaDoc.minSize = Math.min(metaDoc.minSize, doc.size);

            // Update metaDoc.maxSize.
            metaDoc.maxSize = Math.max(metaDoc.maxSize, doc.size);

            // Update metaDoc.totalSize.
            metaDoc.totalSize += doc.size;

            // Update/replace the metadata document in the store.
            var isAccepted = __.replaceDocument(metaDoc._self, metaDoc, function(err) {
              if (err) throw err;
              // Note: in case concurrent updates causes conflict with ErrorCode.RETRY_WITH, we can't read the meta again 
              //       and update again because due to Snapshot isolation we will read same exact version (we are in same transaction).
              //       We have to take care of that on the client side.
            });
            if (!isAccepted) throw new Error("replaceDocument(metaDoc) returned false.");
          });
          if (!result.isAccepted) throw new Error("filter for metaDoc returned false.");
        }
      });
      if (!isAccepted) throw new Error("createDocument(actual doc) returned false.");
    }

## <a name="sql-to-javascript-cheat-sheet"></a>SQL voor Javascript-referentieoverzicht
De volgende tabel geeft verschillende SQL-query's en de bijbehorende JavaScript-query's.

Als het document sleutels met SQL-query's (bijvoorbeeld `doc.id`) zijn hoofdlettergevoelig.

|SQL| Query die JavaScript API|Onderstaande beschrijving|
|---|---|---|
|SELECTEER *<br>VAN documenten| {__.map(Function(doc) <br>&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc;<br>});|1|
|Selecteer docs.id, docs.message als gegevenstarieven in rekening, docs.actions <br>VAN documenten|{__.map(Function(doc)<br>&nbsp;&nbsp;&nbsp;&nbsp;{retourneren<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bericht: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Actions:doc.Actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|2|
|SELECTEER *<br>VAN documenten<br>WAAR docs.id="X998_Y998 '|{__.filter(Function(doc)<br>&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc.id === 'X998_Y998';<br>});|3|
|SELECTEER *<br>VAN documenten<br>WAAR ARRAY_CONTAINS (documenten. Tags, 123)|{__.filter(Function(x)<br>&nbsp;&nbsp;&nbsp;&nbsp;retourneren van x.Tags & & x.Tags.indexOf(123) > -1;<br>});|4|
|Selecteer docs.id, docs.message als bericht<br>VAN documenten<br>WAAR docs.id="X998_Y998 '|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;{.filter(function(doc)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc.id === 'X998_Y998';<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;{.map(function(doc)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{retourneren<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bericht: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.Value();|5|
|De SELECT VALUE-tag<br>VAN documenten<br>Voeg de tag IN documenten. Labels<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;{.filter(function(doc)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;doc retourneren. Labels & & Array.isArray (doc. Tags);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;{.sortBy(function(doc)<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc._ts;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.Value()|6|

De volgende beschrijvingen uitgelegd elke query in de bovenstaande tabel.
1. Resultaten in alle documenten (gepagineerd met vervolgtoken) als is.
2. De id, het bericht (alias voor bericht) en de actie van alle documenten projecten.
3. Query's voor documenten met het predicaat: id = 'X998_Y998'.
4. Query's voor documenten die een eigenschap voor Tags en labels hebben, is een matrix met de waarde 123.
5. Query's voor documenten met een predicaat id = "X998_Y998" en vervolgens projecteert de id en het bericht (alias voor bericht).
6. Filters voor documenten met een matrixeigenschap, Tags, en de resulterende documenten door de eigenschap _ts timestamp-systeem en projecten sorteert + de matrix labels worden samengevoegd.


## <a name="runtime-support"></a>Runtime-ondersteuning
De Azure DB die Cosmos [JavaScript server side API](http://azure.github.io/azure-documentdb-js-server/) biedt ondersteuning voor de meeste van de algemene JavaScript-taalfuncties als gestandaardiseerde door [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Beveiliging
JavaScript opgeslagen procedures en triggers zijn sandbox zodat de gevolgen van een script niet naar het andere gelekt komen zonder tussenkomst van de transactie snapshot-isolatie op databaseniveau. De runtimeomgevingen zijn gegroepeerd, maar na elke uitvoering van de context is verwijderd. Ze zijn daarom gegarandeerd veilig van onbedoelde nadelen van elkaar.

### <a name="pre-compilation"></a>Vooraf compileren
Opgeslagen procedures, triggers en UDF's zijn om te voorkomen dat compilatie kosten op het moment van elke aanroep script impliciet vooraf gecompileerde naar de indeling van de code byte. Dit zorgt ervoor dat de aanroepen van opgeslagen procedures zijn snel en een lage footprint hebben.

## <a name="client-sdk-support"></a>Ondersteuning voor client-SDK
Naast de Cosmos Azure DB [Node.js](sql-api-sdk-node.md) API, Azure Cosmos DB heeft [.NET](sql-api-sdk-dotnet.md), [.NET Core](sql-api-sdk-dotnet-core.md), [Java](sql-api-sdk-java.md), [JavaScript ](http://azure.github.io/azure-documentdb-js/), en [Python SDK's](sql-api-sdk-python.md) voor de SQL-API. Opgeslagen procedures, triggers en UDF's kunnen worden gemaakt en wordt uitgevoerd met een van deze SDK's ook. Het volgende voorbeeld laat zien hoe kunnen maken en uitvoeren van een opgeslagen procedure met de .NET-client. Houd er rekening mee hoe de .NET-typen zijn doorgegeven naar de opgeslagen procedure als JSON en de inhoud weer.

    var markAntiquesSproc = new StoredProcedure
    {
        Id = "ValidateDocumentAge",
        Body = @"
                function(docToCreate, antiqueYear) {
                    var collection = getContext().getCollection();    
                    var response = getContext().getResponse();    

                    if(docToCreate.Year != undefined && docToCreate.Year < antiqueYear){
                        docToCreate.antique = true;
                    }

                    collection.createDocument(collection.getSelfLink(), docToCreate, {}, 
                        function(err, docCreated, options) { 
                            if(err) throw new Error('Error while creating document: ' + err.message);                              
                            if(options.maxCollectionSizeInMb == 0) throw 'max collection size not found'; 
                            response.setBody(docCreated);
                    });
             }"
    };

    // register stored procedure
    StoredProcedure createdStoredProcedure = await client.CreateStoredProcedureAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), markAntiquesSproc);
    dynamic document = new Document() { Id = "Borges_112" };
    document.Title = "Aleph";
    document.Year = 1949;

    // execute stored procedure
    Document createdDocument = await client.ExecuteStoredProcedureAsync<Document>(UriFactory.CreateStoredProcedureUri("db", "coll", "ValidateDocumentAge"), document, 1920);


Dit voorbeeld laat zien hoe u de [SQL .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) voor het maken van een vooraf trigger en maken van een document met de trigger is ingeschakeld. 

    Trigger preTrigger = new Trigger()
    {
        Id = "CapitalizeName",
        Body = @"function() {
            var item = getContext().getRequest().getBody();
            item.id = item.id.toUpperCase();
            getContext().getRequest().setBody(item);
        }",
        TriggerOperation = TriggerOperation.Create,
        TriggerType = TriggerType.Pre
    };

    Document createdItem = await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri("db", "coll"), new Document { Id = "documentdb" },
        new RequestOptions
        {
            PreTriggerInclude = new List<string> { "CapitalizeName" },
        });


En het volgende voorbeeld laat zien hoe door de gebruiker gedefinieerde functies (UDF) maken en deze gebruiken in een [SQL-query](sql-api-sql-query.md).

    UserDefinedFunction function = new UserDefinedFunction()
    {
        Id = "LOWER",
        Body = @"function(input) 
        {
            return input.toLowerCase();
        }"
    };

    foreach (Book book in client.CreateDocumentQuery(UriFactory.CreateDocumentCollectionUri("db", "coll"),
        "SELECT * FROM Books b WHERE udf.LOWER(b.Title) = 'war and peace'"))
    {
        Console.WriteLine("Read {0} from query", book);
    }

## <a name="rest-api"></a>REST API
Alle Azure DB die Cosmos-bewerkingen kunnen worden uitgevoerd op een RESTful manier. Opgeslagen procedures, triggers en de gebruiker gedefinieerde functies kunnen worden geregistreerd in een verzameling met behulp van HTTP POST. Hier volgt een voorbeeld van hoe u kunt een opgeslagen procedure registreren:

    POST https://<url>/sprocs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT


    var x = {
      "name": "createAndAddProperty",
      "body": function (docToCreate, addedPropertyName, addedPropertyValue) {
                var collectionManager = getContext().getCollection();
                collectionManager.createDocument(
                    collectionManager.getSelfLink(),
                    docToCreate,
                    function(err, docCreated) {
                      if(err) throw new Error('Error:  ' + err.message);
                      docCreated[addedPropertyName] = addedPropertyValue;
                      getContext().getResponse().setBody(docCreated);
                    });
            }
    }


De opgeslagen procedure wordt geregistreerd door een POST-aanvraag tegen de URI-databases/testdb/colls/testColl/sprocs uitvoeren met de hoofdtekst van de opgeslagen procedure maken. Triggers en UDF's kunnen op dezelfde manier worden geregistreerd door uitgifte van een POST tegen/triggers en /udfs respectievelijk.
Deze opgeslagen procedure kan vervolgens worden uitgevoerd door uitgifte van een POST-aanvraag tegen de resourcekoppeling:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Hier worden de invoer van de opgeslagen procedure wordt doorgegeven in de aanvraagtekst. Houd er rekening mee dat de invoer is doorgegeven als een JSON-matrix van invoerparameters. De opgeslagen procedure wordt de eerste invoer als een document dat een antwoordtekst. Het antwoord dat is ontvangen, is als volgt:

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: ‘Autumn of the Patriarch’,
      id: ‘V7tQANV3rAkDAAAAAAAAAA==‘,
      ts: 1407830727,
      self: ‘dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/’,
      etag: ‘6c006596-0000-0000-0000-53e9cac70000’,
      attachments: ‘attachments/’,
      Price: 200
    }


In tegenstelling tot opgeslagen procedures, triggers kunnen niet rechtstreeks worden uitgevoerd. In plaats daarvan worden uitgevoerd als onderdeel van een bewerking op een document. We kunt opgeven dat de triggers om uit te voeren met een aanvraag met behulp van HTTP-headers. Hieronder vindt een aanvraag voor het maken van een document.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger


    {
       "name": "newDocument",
       “title”: “The Wizard of Oz”,
       “author”: “Frank Baum”,
       “pages”: 92
    }


Hier worden de trigger vooraf moeten worden uitgevoerd met de aanvraag is opgegeven in de x-ms-documentdb-pre-trigger-include header. Dienovereenkomstig, eventuele na triggers zijn opgegeven in de x-ms-documentdb-post-trigger-include-header. Houd er rekening mee dat zowel vóór en na triggers kunnen worden opgegeven voor een bepaalde aanvraag.

## <a name="sample-code"></a>Voorbeeldcode
U vindt meer serverzijde codevoorbeelden (inclusief [bulk verwijderen](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/bulkDelete.js), en [bijwerken](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples/stored-procedures/update.js)) op onze [GitHub-opslagplaats](https://github.com/Azure/azure-documentdb-js-server/tree/master/samples).

Wilt u uw geweldig opgeslagen procedure delen? Stuur ons een pull-aanvraag. 

## <a name="next-steps"></a>Volgende stappen
Zodra u hebt een of meer opgeslagen procedures, triggers en gebruiker gedefinieerde functies die zijn gemaakt, kunt u ze laden en ze weergeven in de Azure-portal met behulp van Data Explorer.

Ook kan nuttig voor u de volgende verwijzingen en resources in het pad voor meer informatie over Azure Cosmos dB programmeren-server:

* [Azure Cosmos DB SDK 's](sql-api-sdk-dotnet.md)
* [DocumentDB-Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
* [JSON](http://www.json.org/) 
* [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [Veilige en draagbare Database uitbreidbaarheid](http://dl.acm.org/citation.cfm?id=276339) 
* [Service Oriented Architecture van de Database](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [De .NET Runtime in Microsoft SQL server die als host fungeert](http://dl.acm.org/citation.cfm?id=1007669)

