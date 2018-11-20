---
title: JavaScript programmeren-server voor Azure Cosmos DB | Microsoft Docs
description: Informatie over het gebruik van Azure Cosmos DB opgeslagen procedures, databasetriggers en gebruikersgedefinieerde functies (UDF's) in JavaScript schrijven. Get-database programmeren tips en nog veel meer.
keywords: Triggers, opgeslagen procedure, opgeslagen procedure, database-programma, opgeslagen procedure, azure, Microsoft azure-database
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: andrl
ms.openlocfilehash: dd1fff79d6f611ae29307d666860d3740f4372f1
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52162123"
---
# <a name="azure-cosmos-db-server-side-programming-stored-procedures-database-triggers-and-udfs"></a>Azure Cosmos DB-server-side-programmering: opgeslagen procedures, databasetriggers en UDF's

Leer hoe u met Azure Cosmos DB-taal geïntegreerd, transactionele uitvoering van JavaScript kunt schrijven ontwikkelaars **opgeslagen procedures**, **triggers**, en **gebruikersgedefinieerde functies (UDF's)**  systeemeigen in een [ECMAScript 2015](http://www.ecma-international.org/ecma-262/6.0/) JavaScript. JavaScript-integratie kunt u het schrijven van programmalogica die kan worden verzonden en rechtstreeks in de database-opslag-partities worden uitgevoerd. 

In dit artikel leert u de volgende vragen worden beantwoord:  

* Hoe schrijf ik een opgeslagen procedure, trigger of met behulp van JavaScript-UDF?
* Hoe garandeert Cosmos DB ACID?
* Hoe werken transacties in Cosmos DB?
* Wat zijn vooraf wordt geactiveerd en na activeert en hoe schrijf ik een?
* Hoe ik registreren en een opgeslagen procedure, trigger of UDF uitvoeren op een RESTful manier met behulp van HTTP?
* Wat Cosmos DB SDK's zijn beschikbaar om te maken en uitvoeren van opgeslagen procedures, triggers en UDF's?

## <a name="introduction-to-stored-procedure-and-udf-programming"></a>Inleiding tot de opgeslagen Procedure en de UDF-programmering
Deze benadering van *'JavaScript als een moderne T-SQL-dag'* kunnen ontwikkelaars van toepassingen van de complexiteit van het systeem niet-overeenkomende typen en -technologieën voor object-relationele gegevens. Het bevat ook een aantal ingebouwde voordelen die kan worden gebruikt om uitgebreide toepassingen te bouwen:  

* **Procedurele logica:** JavaScript als een op hoog niveau programmeertaal, biedt een rijke en vertrouwde interface om zakelijke logica. U kunt complexe reeksen van bewerkingen dichter in de gegevens uitvoeren.
* **Atomische transacties:** Cosmos DB-garanties die database-bewerkingen die worden uitgevoerd in een enkele opgeslagen procedure of een trigger zijn atomisch. Deze atomic functionaliteit kunt een toepassing gerelateerde bewerkingen in een enkel batchproces combineren, zodat ze allemaal slagen of geen van beide slagen. 
* **Prestaties:** het feit dat JSON intrinsiek is toegewezen aan de taal Javascript-typesysteem, en ook de basiseenheid voor opslag in Cosmos DB is kunt u een aantal optimalisaties, zoals vertraagde materialisatie van JSON-documenten in de buffergroep en zodat ze on-demand beschikbaar voor het uitvoeren van code. Er zijn meer prestatievoordelen verzending bedrijfslogica aan de database is gekoppeld:
  
  * Batchverwerking: ontwikkelaars kunnen bewerkingen zoals toevoegingen groep en legt u deze in één bulkbewerking. De netwerklatentie verkeer de kosten en de store-overhead voor het maken van afzonderlijke transacties zijn aanzienlijk verminderd. 
  * Vooraf compileren – precompiles Cosmos DB opgeslagen procedures, triggers en gebruikersgedefinieerde functies (UDF's) om te voorkomen dat JavaScript compilatie kosten voor elke aanroep. De overhead van het bouwen van de byte-code voor de procedurele logica is afgeschreven op een minimale waarde.
  * Sequentiëren – veel bewerkingen moet een-neveneffect ("trigger") die mogelijk het uitvoeren van een of meer secundaire store-bewerkingen. Afgezien van wat atomisch, maar dit is beter wanneer verplaatst naar de server. 
* **Inkapseling:** opgeslagen procedures kunnen worden gebruikt voor het groeperen van bedrijfslogica op één plek, met twee voordelen:
  * Een abstractielaag bovenop de onbewerkte gegevens, zodat gegevensarchitecten ontwikkelen van hun toepassingen onafhankelijk van de gegevens worden toegevoegd. Deze laag van abstractie is nuttig wanneer de gegevens zonder schema, vanwege de Fragiel blijft veronderstellingen die mogelijk in de toepassing worden standaard als ze zich hoeven te bekommeren om gegevens rechtstreeks.  
  * Deze abstractie kan bedrijven hun gegevens beveiligen door af te stroomlijnen van de toegang van de scripts.  

Het maken en uitvoeren van de databasetriggers en opgeslagen procedures en aangepaste query's wordt ondersteund door de [Azure-portal](https://portal.azure.com), wordt de [REST-API](/rest/api/cosmos-db/), [Azure DocumentDB Studio](https://github.com/mingaliu/DocumentDBStudio/releases), en [client-SDK's](sql-api-sdk-dotnet.md) op verschillende platforms, waaronder .NET, Node.js en JavaScript.

In deze zelfstudie wordt de [Node.js-SDK met Q beloften](http://azure.github.io/azure-documentdb-node-q/) ter illustratie van de syntaxis en het gebruik van opgeslagen procedures, triggers en UDF's.   

## <a name="stored-procedures"></a>Opgeslagen procedures
### <a name="example-write-a-stored-procedure"></a>Voorbeeld: Een opgeslagen procedure schrijven
Laten we beginnen met een eenvoudige, opgeslagen procedure die een 'Hallo wereld'-antwoord geretourneerd.

```javascript
var helloWorldStoredProc = {
    id: "helloWorld",
    serverScript: function () {
        var context = getContext();
        var response = context.getResponse();

        response.setBody("Hello, World");
    }
}
```

Opgeslagen procedures zijn geregistreerd per verzameling en kunnen worden uitgevoerd op een document en de bijlage die aanwezig zijn in deze verzameling. Het volgende fragment toont informatie over het registreren van de helloWorld opgeslagen procedure met een verzameling. 


```javascript
// register the stored procedure
var createdStoredProcedure;
client.createStoredProcedureAsync('dbs/testdb/colls/testColl', helloWorldStoredProc)
    .then(function (response) {
        createdStoredProcedure = response.resource;
        console.log("Successfully created stored procedure");
    }, function (error) {
        console.log("Error", error);
    });
```


Zodra de opgeslagen procedure is geregistreerd, kunt u uitvoeren op basis van de verzameling en bekijk de resultaten terug op de client. 

```javascript
// execute the stored procedure
client.executeStoredProcedureAsync('dbs/testdb/colls/testColl/sprocs/helloWorld')
    .then(function (response) {
        console.log(response.result); // "Hello, World"
    }, function (err) {
        console.log("Error", error);
    });
```

Het contextobject biedt toegang tot alle bewerkingen die kunnen worden uitgevoerd op Cosmos DB-opslag, evenals de toegang tot de aanvraag- en -objecten. In dit geval kunt u de antwoordobject voor de hoofdtekst van het antwoord dat is verzonden naar de client ingesteld. Zie voor meer informatie de [Azure Cosmos DB JavaScript-serverzijde API-verwijzing](https://azure.github.io/azure-cosmosdb-js-server/).  

Laat het ons Vouw op het volgende voorbeeld en meer met betrekking tot database functionaliteit toevoegen aan de opgeslagen procedure. Opgeslagen procedures kunnen maken, bijwerken, lezen, query's uitvoeren en verwijderen van documenten en bijlagen in de verzameling.    

### <a name="example-write-a-stored-procedure-to-create-a-document"></a>Voorbeeld: Een opgeslagen procedure voor het maken van een document schrijven
Het volgende codefragment laat zien hoe het context-object gebruiken om te communiceren met Cosmos DB-resources.

```javascript
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
```


Deze opgeslagen procedure wordt gebruikt als invoer documentToCreate, de hoofdtekst van een document moet worden gemaakt in de huidige verzameling. Alle dergelijke bewerkingen zijn asynchroon en afhankelijk zijn van de callbacks van JavaScript-functie. De callbackfunctie heeft twee parameters, één voor het foutobject in geval die de bewerking is mislukt en één voor het gemaakte object. Binnen de terugbelfunctie kunnen gebruikers de uitzondering verwerken of foutmelding. Een retouraanroep is niet opgegeven en er een fout is, genereert de Azure Cosmos DB-runtime een fout.   

De callback genereert een fout in het bovenstaande voorbeeld als de bewerking is mislukt. Anders wordt de ID van de gemaakte document ingesteld als de hoofdtekst van het antwoord op de client. Hier ziet u hoe u deze opgeslagen procedure uitvoeren met de invoerparameters.

```javascript
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
```

Deze opgeslagen procedure kan worden gewijzigd voor het uitvoeren van een matrix van document instanties als invoer en maken ze allemaal in dezelfde opgeslagen procedure uitvoering in plaats van meerdere aanvragen voor elk van deze afzonderlijk maken. Deze opgeslagen procedure kan worden gebruikt voor het implementeren van een efficiënte bulksgewijs importprogramma voor Cosmos DB (Zie verderop in deze zelfstudie).   

In het voorbeeld dat wordt beschreven hoe u opgeslagen procedures geïllustreerd. Vervolgens leert u over triggers en gebruikersgedefinieerde functies (UDF's) later in de zelfstudie.

### <a name="known-issues"></a>Bekende problemen

Bij het definiëren van een opgeslagen procedure met behulp van Azure portal, worden invoerparameters altijd verzonden als een tekenreeks voor de opgeslagen procedure. Zelfs als u een matrix met tekenreeksen als invoer doorgeeft, wordt de matrix geconverteerd naar een tekenreeks en verzonden naar de opgeslagen procedure. Tijdelijke oplossing dit probleem, kunt u een functie binnen de opgeslagen procedure voor het parseren van de tekenreeks als een matrix. De volgende code is een voorbeeld van de tekenreeks als een matrix parseren: 

```javascript
function sample(arr) {
    if (typeof arr === "string") arr = JSON.parse(arr);
    
    arr.forEach(function(a) {
        // do something here
        console.log(a);
    });
}
```

## <a name="database-program-transactions"></a>Programma databasetransacties
Transactie in een typische database kan worden gedefinieerd als een reeks bewerkingen die worden uitgevoerd als één logische eenheid van het werk. Elke transactie biedt **ACID-garanties**. ACID is een bekende afkorting van vier eigenschappen - atomisch, consistent, isolatie en duurzaamheid.  

Kort samengevat kunt atomisch zorgt ervoor dat alle werk binnen een transactie wordt beschouwd als één eenheid waar een van beide allemaal is doorgevoerd of geen. Consistentie zorgt ervoor dat de gegevens altijd in goede staat verkeren interne voor transacties is. Isolatie zorgt ervoor dat er zijn geen twee transacties leiden tot problemen met elkaar – algemeen, de meeste commerciële systemen bieden verschillende isolatieniveaus die kunnen worden gebruikt op basis van de vereisten voor de toepassing. Duurzaamheid zorgt ervoor dat eventuele wijzigingen die doorgevoerd in de database altijd aanwezig zijn.   

JavaScript wordt in Cosmos DB worden gehost in dezelfde geheugenruimte als de database. Daarom kan aanvragen in opgeslagen procedures en triggers in hetzelfde bereik van de databasesessie van een uitvoeren. Deze functie kunt Cosmos DB om te waarborgen van ACID voor alle bewerkingen die deel van één opgeslagen procedure/trigger uitmaken. Houd rekening met de volgende definitie van de opgeslagen procedure:

```javascript
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
```

Deze opgeslagen procedure maakt gebruik van transacties binnen een gaming-app voor handel items tussen twee spelers in één bewerking. De opgeslagen procedure probeert te lezen van de twee documenten elke overeenkomt met de speler-id's als argument doorgegeven. Als beide player-documenten worden gevonden, wordt de opgeslagen procedure de documenten werkt door te wisselen hun items. Als er fouten optreden weg, dit genereert een JavaScript-uitzondering die impliciet Hiermee de transactie wordt afgebroken.

Als de verzameling de opgeslagen procedure is geregistreerd op basis van een verzameling met één partitie, wordt de transactie is afgestemd op de documenten binnen de verzameling. Als de verzameling is gepartitioneerd, worden klikt u vervolgens opgeslagen procedures uitgevoerd binnen het transactiebereik van een enkele partitiesleutel. De opgeslagen procedure uitvoeren moet vervolgens een waarde voor de partitiesleutel overeenkomt met het bereik dat de transactie moet worden uitgevoerd onder bevatten. Zie voor meer informatie, [partitioneren van Azure Cosmos DB](partition-data.md).

### <a name="commit-and-rollback"></a>Doorvoeren en ongedaan maken
Transacties zijn systeemeigen en diep geïntegreerd in JavaScript Cosmos-DB-programmeermodel. Binnen een JavaScript-functie, worden alle bewerkingen automatisch verpakt in één transactie. Als de JavaScript zonder een uitzondering is voltooid, zijn de bewerkingen in de database zijn doorgevoerd. De instructies 'BEGIN TRANSACTION' en 'COMMIT TRANSACTION' in relationele databases zijn in feite impliciet in Cosmos DB.  

Als er een uitzondering die van het script wordt doorgegeven, wordt Cosmos-DB JavaScript-runtime Draai de hele transactie terug. Zoals weergegeven in het vorige voorbeeld, is die een uitzondering veroorzaakt effectief gelijk is aan een 'ROLLBACK TRANSACTION' in Cosmos DB.

### <a name="data-consistency"></a>Gegevensconsistentie
Opgeslagen procedures en triggers worden altijd uitgevoerd op de primaire replica van de Azure Cosmos DB-container. Dit zorgt ervoor dat leesbewerkingen van in procedures aanbieding sterke consistentie opgeslagen. Query's met behulp van de gebruiker gedefinieerde functies kunnen worden uitgevoerd op de primaire of een secundaire replica's, maar u ervoor zorgen dat de aangevraagde consistentieniveau te voldoen aan de juiste replica kiezen.

## <a name="bounded-execution"></a>Gebonden uitvoering
Alle Cosmos DB-bewerkingen moeten voltooid binnen de opgegeven server time-out aanvraagduur. Deze beperking geldt ook voor JavaScript-functies (opgeslagen procedures, triggers en gebruikersgedefinieerde functies). Als een bewerking niet wordt voltooid met deze termijn, wordt de transactie teruggedraaid. JavaScript-functies moeten worden voltooid binnen de tijdslimiet of implementeren van een model op basis van een voortzetting van batch/hervatten kan worden uitgevoerd.  

Vereenvoudigt de ontwikkeling van opgeslagen procedures en triggers om af te handelen tijdslimieten, alle functies in de verzamelingsobject (voor het maken, lezen, vervangen en verwijderen van documenten en bijlagen) retourneren een Booleaanse waarde die vertegenwoordigt of de waarde die voor deze bewerking wordt voltooid. Als deze waarde ingesteld op false is, is het een indicatie dat de tijdslimiet is bijna verlopen en dat de procedure van kan worden uitgevoerd inpakken moet.  Bewerkingen in de wachtrij vóór de eerste bewerking in niet-geaccepteerde store om uit te voeren als de opgeslagen procedure is voltooid in de tijd en geen aanvullende aanvragen niet in de wachtrij worden gegarandeerd.  

JavaScript-functies zijn ook gebonden op resourceverbruik. Cosmos DB reserveert doorvoer per verzameling of voor een set met containers. Doorvoer wordt uitgedrukt in termen van een genormaliseerde eenheid van CPU, geheugen en i/o-verbruik van aanvraageenheden of ru's genoemd. JavaScript-functies kunnen gebruiken om een groot aantal ru's binnen een korte periode en kunnen er rate-limited als van de verzameling limiet is bereikt. Resource-intensieve opgeslagen procedures mogelijk ook in quarantaine worden geplaatst voor beschikbaarheid van primitieve databasebewerkingen.  

### <a name="example-bulk-importing-data-into-a-database-program"></a>Voorbeeld: Bulksgewijs importeren van gegevens in een database-programma
Hieronder volgt een voorbeeld van een opgeslagen procedure die worden geschreven naar bulkimport documenten in een verzameling. Houd er rekening mee hoe de opgeslagen procedure omgaat met begrensde worden uitgevoerd door het controleren van de Booleaanse waarde retourneren vanuit createDocument, en gebruikt vervolgens het aantal documenten die zijn ingevoegd in elke aanroep van de opgeslagen procedure bij te houden en de voortgang van alle batches hervatten.

```javascript
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
```

## <a id="trigger"></a> Databasetriggers
### <a name="database-pre-triggers"></a>Vóór databasetriggers
Cosmos DB biedt triggers die zijn uitgevoerd of geactiveerd door een bewerking op een document. U kunt bijvoorbeeld een pre-trigger opgeven wanneer u het maken van een document – deze vooraf trigger wordt uitgevoerd voordat het document wordt gemaakt. Het volgende voorbeeld ziet u hoe vooraf triggers kunnen worden gebruikt voor het valideren van de eigenschappen van een document dat wordt gemaakt:

```javascript
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
```

En de bijbehorende Node.js registratie aan clientkant code voor de trigger:

```javascript
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
```

Vooraf triggers kunnen geen invoer parameters hebben. Het request-object kan worden gebruikt voor het bewerken van het aanvraagbericht dat is gekoppeld aan de bewerking. Hier worden de pre-trigger wordt uitgevoerd met het maken van een document en de berichttekst van de aanvraag bevat het document dat moet worden gemaakt in JSON-indeling.   

Als triggers zijn geregistreerd, kunnen gebruikers de bewerkingen die het kan worden uitgevoerd met opgeven. Deze trigger is gemaakt met TriggerOperation.Create, wat betekent met behulp van de trigger in een vervangingsbewerking dat zoals wordt weergegeven in de volgende code is niet toegestaan.

```javascript
var options = { preTriggerInclude: "validateDocumentContents" };

client.replaceDocumentAsync(docToReplace.self,
              newDocBody, options)
.then(function (response) {
    console.log(response.resource);
}, function (error) {
    console.log("Error", error);
});

// Fails, can’t use a create trigger in a replace operation
```
### <a name="database-post-triggers"></a>Na databasetriggers
Na triggers, zoals pre triggers zijn gekoppeld aan een bewerking op een document en geen duren voordat alle invoerparameters die zijn opgegeven. Ze worden uitgevoerd **nadat** de bewerking is voltooid en toegang hebben tot het antwoordbericht dat wordt verzonden naar de client.   

Het volgende voorbeeld ziet u na triggers in actie:
```javascript
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

```
De trigger kan worden geregistreerd, zoals wordt weergegeven in het volgende voorbeeld.
```javascript
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
```

Deze trigger zoekt naar het document met metagegevens en bijgewerkt met informatie over het zojuist gemaakte document.  

Eén ding dat is belangrijk te weten is de **transactionele** uitvoering van triggers in Cosmos DB. Deze na trigger wordt uitgevoerd als onderdeel van dezelfde transactie als het maken van het originele document. Als u een uitzondering genereren door de na trigger (zeg als u niet de metadata-document bij te werken), de hele transactie mislukken en worden teruggedraaid. Er is geen document wordt gemaakt en wordt een uitzondering geretourneerd.  

## <a id="udf"></a>De gebruiker gedefinieerde functies
Gebruiker gedefinieerde functies (UDF's) worden gebruikt voor het uitbreiden van de Azure Cosmos DB SQL-query language-grammatica en aangepaste bedrijfslogica implementeren. Ze kunnen alleen worden aangeroepen vanuit in query's. Ze hebben geen toegang tot het context-object en zijn bedoeld om te worden gebruikt als alleen-compute-JavaScript. Daarom kunnen UDF's worden uitgevoerd op secundaire replica's van de Cosmos DB-service.  

Het volgende voorbeeld maakt een UDF voor het berekenen van de inkomstenbelasting op basis van tarieven voor verschillende inkomsten vierkante haken, en vervolgens in een query gebruikt om te zoeken naar alle personen die meer dan $20.000 betaald belastingen.

```javascript
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
```

De UDF kan vervolgens worden gebruikt in query's, zoals in het volgende voorbeeld:

```javascript
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
```

## <a name="javascript-language-integrated-query-api"></a>JavaScript language integrated query API
Naast het uitgeven van query's met behulp van Azure Cosmos DB SQL-grammatica, de [server-side SDK](https://azure.github.io/azure-cosmosdb-js-server/) kunt u geoptimaliseerde query's uitvoeren met een gestroomlijnde JavaScript-interface zonder enige kennis van SQL. De JavaScript-query-API u query's via een programma kunt maken kunt met het predicaat functies wordt doorgegeven aan de chainable functie aanroept, met een syntaxis die bekend is bij de matrix dient te worden en populaire JavaScript-bibliotheken zoals Lodash van ECMAScript5. Query's worden geparseerd door het JavaScript-runtime efficiënt met behulp van Azure Cosmos DB-indexen worden uitgevoerd.

> [!NOTE]
> `__` (dubbel-onderstrepingsteken) is een alias voor `getContext().getCollection()`.
> <br/>
> Met andere woorden, kunt u `__` of `getContext().getCollection()` voor toegang tot de JavaScript API-query.
> 
> 

Ondersteunde functies zijn onder andere:

<ul>
<li>
<b>... chain(). waarde ([callback] [, opties voor])</b>
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
Filtert de invoer met behulp van een predicaat functie die resulteert in waar/onwaar om te filteren in/uit-invoer documenten in de resulterende set. Deze functie werkt die vergelijkbaar is met een WHERE-component in SQL.
</li>
</ul>
</li>
<li>
<b>kaart (transformationFunction [, opties] [, callback])</b>
<ul>
<li>
Van toepassing is een projectie een transformatiefunctie die elk item invoer wordt toegewezen aan een JavaScript-object of een waarde opgegeven. Deze functie werkt die vergelijkbaar is met een SELECT-component in SQL.
</li>
</ul>
</li>
<li>
<b>pluck ([propertyName] [, opties] [, callback])</b>
<ul>
<li>
Deze functie is een snelkoppeling voor een kaart die de waarde van één eigenschap van elk item invoer geëxtraheerd.
</li>
</ul>
</li>
<li>
<b>samenvoegen ([isShallow] [, opties] [, callback])</b>
<ul>
<li>
Combineert en matrices van elk item invoer in om een matrix te worden samengevoegd. Deze functie werkt die vergelijkbaar is met SelectMany in LINQ.
</li>
</ul>
</li>
<li>
<b>sortBy ([predicate] [, opties] [, callback])</b>
<ul>
<li>
Een nieuwe set documenten door te sorteren van de documenten in de stroom invoerdocument in oplopende volgorde met behulp van de opgegeven predicaat produceren. Deze functie werkt die vergelijkbaar is met een ORDER BY-component in SQL.
</li>
</ul>
</li>
<li>
<b>sortByDescending ([predicate] [, opties] [, callback])</b>
<ul>
<li>
Een nieuwe set documenten door te sorteren van de documenten in de stroom invoerdocument in aflopende volgorde met behulp van de opgegeven predicaat produceren. Deze functie werkt die vergelijkbaar is met een component ORDER BY x DESC in SQL.
</li>
</ul>
</li>
</ul>


Wanneer ze zijn opgenomen in het predikaat en/of de selectie van functies, worden de volgende JavaScript-constructies automatisch geoptimaliseerd om te worden uitgevoerd op Azure Cosmos DB-indexen:

* Eenvoudige operators: = + - * / % | ^ &amp; == != === !=== &lt; &gt; &lt;= &gt;= || &amp;&amp; &lt;&lt; &gt;&gt; &gt;&gt;&gt;! ~
* Letterlijke waarden, met inbegrip van de letterlijke object: {}
* var, Ga terug

De volgende JavaScript-constructies kunnen niet worden geoptimaliseerd voor Azure Cosmos DB-indexen:

* Controlestroom (bijvoorbeeld, als voor, tijdens)
* Functieaanroepen

Zie voor meer informatie de [serverzijde JSDocs](https://azure.github.io/azure-cosmosdb-js-server/).

### <a name="example-write-a-stored-procedure-using-the-javascript-query-api"></a>Voorbeeld: Een opgeslagen procedure met behulp van de JavaScript API-query schrijven
Het volgende codevoorbeeld is een voorbeeld van hoe de JavaScript-Query-API kan worden gebruikt in de context van een opgeslagen procedure. De opgeslagen procedure voegt u een document, komt overeen met een invoerparameter en updates van de metagegevens van een document, met behulp van de `__.filter()` methode, met minSize, maxSize en totalSize op basis van de eigenschap van de grootte van het ingevoerde document.

```javascript
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
```

## <a name="sql-to-javascript-cheat-sheet"></a>SQL voor het referentiemateriaal voor Javascript
De volgende tabel bevat verschillende SQL-query's en de bijbehorende JavaScript-query's.

Als u met SQL-query's, documenteer sleutels (bijvoorbeeld `doc.id`) zijn hoofdlettergevoelig.

|SQL| JavaScript API-Query|Onderstaande beschrijving|
|---|---|---|
|SELECTEER *<br>VAN docs| __.map(Function(doc) { <br>&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc-bestand;<br>});|1|
|Selecteer docs.id, docs.message als msg, docs.actions <br>VAN docs|__.map(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;{retourneren<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Actions:doc.Actions<br>&nbsp;&nbsp;&nbsp;&nbsp;};<br>});|2|
|SELECTEER *<br>VAN docs<br>WAAR docs.id="X998_Y998"|__.filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc.id === 'X998_Y998';<br>});|3|
|SELECTEER *<br>VAN docs<br>WAAR ARRAY_CONTAINS (docs. -Tags, 123)|__.filter(Function(x) {<br>&nbsp;&nbsp;&nbsp;&nbsp;retourneren van x.Tags & & x.Tags.indexOf(123) > -1;<br>});|4|
|Selecteer docs.id, docs.message als msg<br>VAN docs<br>WAAR docs.id="X998_Y998"|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc.id === 'X998_Y998';<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.map(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{retourneren<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID: doc.id,<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;msg: doc.message<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;};<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>.value();|5|
|SELECT VALUE-tag<br>VAN docs<br>Neem deel aan een tag IN docs. Tags<br>ORDER BY docs._ts|__.chain()<br>&nbsp;&nbsp;&nbsp;&nbsp;.filter(Function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;document retourneren. Labels & & Array.isArray (doc-bestand. -Tags);<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.sortBy(function(doc) {<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;retourneren van doc._ts;<br>&nbsp;&nbsp;&nbsp;&nbsp;})<br>&nbsp;&nbsp;&nbsp;&nbsp;.pluck("Tags")<br>&nbsp;&nbsp;&nbsp;&nbsp;.flatten()<br>&nbsp;&nbsp;&nbsp;&nbsp;.value()|6|

De volgende beschrijvingen uitgelegd elke query in de bovenstaande tabel.
1. Resultaten in alle documenten (gepagineerde met vervolgtoken) als is.
2. De-id, een bericht (alias voor msg) en een actie uit alle documenten projecten.
3. Query's voor documenten met het predicaat: id = 'X998_Y998'.
4. Query's voor documenten die een eigenschap Tags en labels hebben, is een matrix met de waarde 123.
5. Query's voor documenten met een predicaat, id = 'X998_Y998', en vervolgens de id en het bericht (alias voor msg)-projecten.
6. Filters voor documenten met een matrixeigenschap, labels, en sorteert de resulterende documenten door de eigenschap _ts timestamp-systeem en projecten + de matrix Tags worden samengevoegd.


## <a name="runtime-support"></a>Runtime-ondersteuning
De Azure Cosmos DB [JavaScript-API van server side](https://azure.github.io/azure-cosmosdb-js-server/) biedt ondersteuning voor de meeste van de algemene functies van JavaScript taal als gestandaardiseerde door [ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm).

### <a name="security"></a>Beveiliging
JavaScript opgeslagen procedures en triggers zijn sandbox zodat de gevolgen van een script niet naar het andere gelekt kunnen zonder tussenkomst van de snapshot-isolatie voor transactie op het databaseniveau van de. De runtime-omgevingen zijn gegroepeerd, maar na elke uitvoering van de context is verwijderd. Ze worden daarom gegarandeerd veilig van een onbedoelde neveneffecten van elkaar.

### <a name="pre-compilation"></a>Vooraf compileren
Opgeslagen procedures, triggers en UDF's zijn impliciet vooraf gecompileerde naar de byte-codeopmaak om te voorkomen dat compilatie kosten op het moment van elke aanroepen van scripts. Vooraf compilatie aanroep van opgeslagen procedures is snel en een lage footprint hebben.

## <a name="client-sdk-support"></a>Client SDK-ondersteuning
Naast de Azure Cosmos DB [Node.js](sql-api-sdk-node.md) API, Azure Cosmos DB heeft [.NET](sql-api-sdk-dotnet.md), [.NET Core](sql-api-sdk-dotnet-core.md), [Java](sql-api-sdk-java.md), [JavaScript ](sql-api-sdk-node.md), en [Python SDK's](sql-api-sdk-python.md) voor de SQL-API. Opgeslagen procedures, triggers en UDF's kunnen worden gemaakt en uitgevoerd met behulp van een van deze SDK's ook. Het volgende voorbeeld ziet hoe u maakt en een opgeslagen procedure met behulp van de .NET-client uitvoeren. Houd er rekening mee hoe de .NET-typen zijn doorgegeven aan de opgeslagen procedure als JSON en terug te lezen.

```javascript
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
```

In dit voorbeeld laat zien hoe u de [SQL .NET API](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet) een pre-trigger maken en een document maken met de trigger ingeschakeld. 

```javascript
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
```

En het volgende voorbeeld laat zien hoe een door de gebruiker gedefinieerde functie (UDF's) maken en deze gebruiken in een [SQL-query](how-to-sql-query.md).

```javascript
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
```

## <a name="rest-api"></a>REST-API
Alle Azure Cosmos DB-bewerkingen kunnen worden uitgevoerd op een RESTful manier. Opgeslagen procedures, triggers en gebruikersgedefinieerde functies kunnen worden geregistreerd bij een verzameling met behulp van HTTP POST. Het volgende voorbeeld ziet u informatie over het registreren van een opgeslagen procedure:

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


De opgeslagen procedure is geregistreerd door het uitvoeren van een POST-aanvraag op basis van de URI databases/testdb/colls/testColl/sprocs met de opgeslagen procedure maken in de hoofdtekst. Triggers en UDF's kunnen op dezelfde manier worden geregistreerd met behulp van een bericht tegen/triggers en /udfs respectievelijk.
Deze opgeslagen procedure kan vervolgens worden uitgevoerd met behulp van een POST-aanvraag op basis van de resourcekoppeling:

    POST https://<url>/sprocs/<sproc> HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:20 GMT


    [ { "name": "TestDocument", "book": "Autumn of the Patriarch"}, "Price", 200 ]


Hier worden de invoer voor de opgeslagen procedure wordt doorgegeven in de aanvraagtekst. De invoer wordt doorgegeven als een JSON-matrix van invoerparameters die zijn opgegeven. De opgeslagen procedure wordt de eerste invoer als een document dat een antwoordtekst. Het antwoord u ontvangt is als volgt:

    HTTP/1.1 200 OK

    { 
      name: 'TestDocument',
      book: 'Autumn of the Patriarch',
      id: 'V7tQANV3rAkDAAAAAAAAAA==',
      ts: 1407830727,
      self: 'dbs/V7tQAA==/colls/V7tQANV3rAk=/docs/V7tQANV3rAkDAAAAAAAAAA==/',
      etag: '6c006596-0000-0000-0000-53e9cac70000',
      attachments: 'attachments/',
      Price: 200
    }


In tegenstelling tot opgeslagen procedures, triggers kunnen niet rechtstreeks worden uitgevoerd. In plaats daarvan worden uitgevoerd als onderdeel van een bewerking op een document. U kunt opgeven dat de triggers om uit te voeren met een aanvraag met behulp van HTTP-headers. De volgende code toont de aanvraag voor het maken van een document.

    POST https://<url>/docs/ HTTP/1.1
    authorization: <<auth>>
    x-ms-date: Thu, 07 Aug 2014 03:43:10 GMT
    x-ms-documentdb-pre-trigger-include: validateDocumentContents 
    x-ms-documentdb-post-trigger-include: bookCreationPostTrigger

    {
       "name": "newDocument",
       "title": "The Wizard of Oz",
       "author": "Frank Baum",
       "pages": 92
    }


Hier wordt de trigger vooraf moeten worden uitgevoerd met de aanvraag opgegeven in de x-ms-documentdb-pre-trigger-include header. Dienovereenkomstig, geen na triggers zijn opgegeven in de x-ms-documentdb-post-trigger-include header. Zowel vóór en na triggers kunnen worden opgegeven voor een bepaalde aanvraag.

## <a name="sample-code"></a>Voorbeeldcode
U vindt meer voorbeelden van code op de server (met inbegrip van [bulksgewijs verwijderen](https://github.com/Azure/azure-cosmosdb-js-server/blob/master/samples/stored-procedures/bulkDelete.js), en [bijwerken](https://github.com/Azure/azure-cosmosdb-js-server/blob/master/samples/stored-procedures/update.js)) in de [GitHub-opslagplaats](https://github.com/Azure/azure-cosmosdb-js-server/tree/master/samples).

Wilt u uw geweldige opgeslagen procedure delen? bijdragen aan de opslagplaats en maakt u een pull-aanvraag. 

## <a name="next-steps"></a>Volgende stappen
Zodra u hebt een of meer opgeslagen procedures, triggers en gebruikersgedefinieerde functies die zijn gemaakt, kunt u ze laden en ze weergeven in de Azure-portal met behulp van Data Explorer.

Ook kan nuttig voor u de volgende verwijzingen en resources in het pad voor meer informatie over Azure Cosmos dB-serverzijde programmeren:

* [Azure Cosmos DB JavaScript-serverzijde API-naslaginformatie](https://azure.github.io/azure-cosmosdb-js-server/)
* [DocumentDB-Studio](https://github.com/mingaliu/DocumentDBStudio/releases)
* [JavaScript ECMA-262](http://www.ecma-international.org/publications/standards/Ecma-262.htm)
* [Veilige en draagbare Database uitbreidbaarheid](http://dl.acm.org/citation.cfm?id=276339) 
* [Service Oriented Architecture voor Database](http://dl.acm.org/citation.cfm?id=1066267&coll=Portal&dl=GUIDE) 
* [De .NET Runtime in Microsoft SQL server die als host fungeert](http://dl.acm.org/citation.cfm?id=1007669)
