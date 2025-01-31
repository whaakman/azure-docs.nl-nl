---
title: Opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies schrijven in Azure Cosmos DB
description: Meer informatie over het definiëren van opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/21/2019
ms.author: mjbrown
ms.openlocfilehash: 66e0a7e13df9eddcd722492c9c894721517af5f9
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968919"
---
# <a name="how-to-write-stored-procedures-triggers-and-user-defined-functions-in-azure-cosmos-db"></a>Opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies schrijven in Azure Cosmos DB

Azure Cosmos DB biedt taalgeïntegreerde, transactionele uitvoering van JavaScript waarmee u **opgeslagen procedures**, **triggers** en **door de gebruiker gedefinieerde functies (UDF's)** kunt schrijven. Wanneer u de SQL-API in Azure Cosmos DB gebruikt, kunt u de opgeslagen procedures, triggers en UDF's definiëren in JavaScript-taal. U kunt uw logica in JavaScript schrijven en uitvoeren in de database-engine. U kunt triggers, opgeslagen procedures en UDF's maken en uitvoeren met behulp van [Azure Portal](https://portal.azure.com/), de [met JavaScript-taal geïntegreerde query-API in Azure Cosmos DB](javascript-query-api.md) en de [client SDK's van de Cosmos DB SQL-API](sql-api-dotnet-samples.md). 

Als u een opgeslagen procedure, trigger en een door de gebruiker gedefinieerde functie wilt aanroepen, moet u deze registreren. Zie [Werken met opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies in Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md) voor meer informatie.

> [!NOTE]
> Bij het uitvoeren van een opgeslagen procedure voor gepartitioneerde containers moet een waarde voor de partitiesleutel worden opgegeven in de aanvraagopties. Opgeslagen procedures zijn altijd gerelateerd aan een partitiesleutel. Items met een andere partitiesleutelwaarde zijn niet zichtbaar voor de opgeslagen procedure. Dit geldt ook voor triggers.

## <a id="stored-procedures"></a>Opgeslagen procedures uitvoeren schrijven

Opgeslagen procedures worden geschreven met behulp van JavaScript. Met deze kunt u items binnen een Azure Cosmos-container maken, bijwerken, lezen, query's ernaar uitvoeren en verwijderen. Opgeslagen procedures worden geregistreerd per verzameling en kunnen worden uitgevoerd op elk document of een in deze verzameling aanwezige bijlage.

**Voorbeeld**

Hier volgt een eenvoudige, opgeslagen procedure die een 'Hallo wereld'-antwoord retourneert.

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

Het contextobject biedt toegang tot alle bewerkingen die in Azure Cosmos DB kunnen worden uitgevoerd, evenals de toegang tot de aanvraag- en antwoordobjecten. U gebruikt in dit geval het antwoordobject om de hoofdtekst van het antwoord in te stellen dat terug naar de client wordt verzonden.

Na te zijn geschreven moet de opgeslagen procedure worden geregistreerd bij een verzameling. Zie het artikel [Opgeslagen procedures in Azure Cosmos DB gebruiken](how-to-use-stored-procedures-triggers-udfs.md#stored-procedures) voor meer informatie.

### <a id="create-an-item"></a>Een item maken met behulp van opgeslagen procedure

Wanneer u een item maakt met behulp van opgeslagen procedure, het item wordt ingevoegd in de Azure Cosmos DB-container en een id voor het nieuwe item wordt geretourneerd. Het maken van een item is een asynchrone bewerking en is afhankelijk van de callback-functies van JavaScript. De callback-functie heeft twee parameters: een voor het foutobject in geval dat de bewerking mislukt en een voor een retourwaarde; in dit geval het gemaakte object. Binnen de callback kunt u de uitzondering verwerken of een fout genereren. Indien er geen callback is opgegeven en er een fout is opgetreden, genereert de Azure Cosmos DB-runtime een fout. 

De opgeslagen procedure bevat ook een parameter voor het instellen van de beschrijving, het is een Booleaanse waarde. Als de parameter is ingesteld op true en de beschrijving ontbreekt, genereert de opgeslagen procedure een uitzondering. Anders gaat de uitvoering van de rest van de opgeslagen procedure verder.

Met het volgende opgeslagen procedurevoorbeeld neemt u een nieuw item in de Azure Cosmos DB als invoer, voegt u dit toe aan de Azure Cosmos DB-container en retourneert u de id voor het zojuist gemaakt item. In dit voorbeeld maken we gebruik van het ToDoList-voorbeeld van de [snelstart .NET SQL-API](create-sql-api-dotnet.md)

```javascript
function createToDoItem(itemToCreate) {

    var context = getContext();
    var container = context.getCollection();

    var accepted = container.createDocument(container.getSelfLink(),
        itemToCreate,
        function (err, itemCreated) {
            if (err) throw new Error('Error' + err.message);
            context.getResponse().setBody(itemCreated.id)
        });
    if (!accepted) return;
}
```

### <a name="arrays-as-input-parameters-for-stored-procedures"></a>Matrices als invoerparameters voor opgeslagen procedures 

Bij het definiëren van een opgeslagen procedure in Azure Portal worden invoerparameters altijd verzonden naar de opgeslagen procedure als een tekenreeks. Zelfs als u een matrix met tekenreeksen als invoer doorgeeft, wordt de matrix geconverteerd naar een tekenreeks en verzonden naar de opgeslagen procedure. Om dit te omzeilen kunt u een functie definiëren binnen de opgeslagen procedure voor het parseren van de tekenreeks als matrix. De volgende code laat zien hoe u een invoerparameter van de tekenreeks als matrix kunt parseren:

```javascript
function sample(arr) {
    if (typeof arr === "string") arr = JSON.parse(arr);

    arr.forEach(function(a) {
        // do something here
        console.log(a);
    });
}
```

### <a id="transactions"></a>Transacties in opgeslagen procedures

U kunt transacties implementeren in items in een container met behulp van een opgeslagen procedure. In het volgende voorbeeld worden transacties ingezet binnen een fantasy football gaming-app om in één bewerking spelers te ruilen tussen twee teams. De opgeslagen procedure probeert de twee Azure Cosmos DB-items te lezen, waarbij elk item overeenkomt met de speler-id's die als argument zijn doorgegeven. Als beide spelers worden gevonden,worden vervolgens de items door de opgeslagen procedure bijgewerkt door hun teams om te wisselen. Als er gaandeweg fouten optreden, genereert de opgeslagen procedure een JavaScript-uitzondering waarmee impliciet de transactie wordt afgebroken.

```javascript
// JavaScript source code
function tradePlayers(playerId1, playerId2) {
    var context = getContext();
    var container = context.getCollection();
    var response = context.getResponse();

    var player1Document, player2Document;

    // query for players
    var filterQuery = 
    {     
        'query' : 'SELECT * FROM Players p where p.id = @playerId1',
        'parameters' : [{'name':'@playerId1', 'value':playerId1}] 
    };
            
    var accept = container.queryDocuments(container.getSelfLink(), filterQuery, {},
        function (err, items, responseOptions) {
            if (err) throw new Error("Error" + err.message);

            if (items.length != 1) throw "Unable to find both names";
            player1Item = items[0];

            var filterQuery2 = 
            {     
                'query' : 'SELECT * FROM Players p where p.id = @playerId2',
                'parameters' : [{'name':'@playerId2', 'value':playerId2}] 
            };
            var accept2 = container.queryDocuments(container.getSelfLink(), filterQuery2, {},
                function (err2, items2, responseOptions2) {
                    if (err2) throw new Error("Error" + err2.message);
                    if (items2.length != 1) throw "Unable to find both names";
                    player2Item = items2[0];
                    swapTeams(player1Item, player2Item);
                    return;
                });
            if (!accept2) throw "Unable to read player details, abort ";
        });

    if (!accept) throw "Unable to read player details, abort ";

    // swap the two players’ teams
    function swapTeams(player1, player2) {
        var player2NewTeam = player1.team;
        player1.team = player2.team;
        player2.team = player2NewTeam;

        var accept = container.replaceDocument(player1._self, player1,
            function (err, itemReplaced) {
                if (err) throw "Unable to update player 1, abort ";

                var accept2 = container.replaceDocument(player2._self, player2,
                    function (err2, itemReplaced2) {
                        if (err) throw "Unable to update player 2, abort"
                    });

                if (!accept2) throw "Unable to update player 2, abort";
            });

        if (!accept) throw "Unable to update player 1, abort";
    }
}
```

### <a id="bounded-execution"></a>Gebonden uitvoering in opgeslagen procedures

Hier volgt een voorbeeld van een opgeslagen procedure die bulksgewijs items importeert in een Azure Cosmos-container. De opgeslagen procedure verwerkt gebonden uitvoering door de Booleaanse geretourneerde waarde van `createDocument` te controleren, en gebruikt vervolgens het aantal items dat in elke aanroep van de opgeslagen procedure wordt ingevoegd om de voortgang van alle batches bij te houden en te hervatten.

```javascript
function bulkImport(items) {
    var container = getContext().getCollection();
    var containerLink = container.getSelfLink();

    // The count of imported items, also used as current item index.
    var count = 0;

    // Validate input.
    if (!items) throw new Error("The array is undefined or null.");

    var itemsLength = items.length;
    if (itemsLength == 0) {
        getContext().getResponse().setBody(0);
    }

    // Call the create API to create an item.
    tryCreate(items[count], callback);

    // Note that there are 2 exit conditions:
    // 1) The createDocument request was not accepted.
    //    In this case the callback will not be called, we just call setBody and we are done.
    // 2) The callback was called items.length times.
    //    In this case all items were created and we don’t need to call tryCreate anymore. Just call setBody and we are done.
    function tryCreate(item, callback) {
        var isAccepted = container.createDocument(containerLink, item, callback);

        // If the request was accepted, callback will be called.
        // Otherwise report current count back to the client,
        // which will call the script again with remaining set of items.
        if (!isAccepted) getContext().getResponse().setBody(count);
    }

    // This is called when container.createDocument is done in order to process the result.
    function callback(err, item, options) {
        if (err) throw err;

        // One more item has been inserted, increment the count.
        count++;

        if (count >= itemsLength) {
            // If we created all items, we are done. Just set the response.
            getContext().getResponse().setBody(count);
        } else {
            // Create next document.
            tryCreate(items[count], callback);
        }
    }
}
```

## <a id="triggers"></a>Triggers schrijven

Azure Cosmos DB biedt ondersteuning aan pre-triggers en post-triggers. Pre-triggers worden uitgevoerd voordat een database-item wordt gewijzigd en post-triggers worden uitgevoerd nadat een database-item wordt gewijzigd.

### <a id="pre-triggers"></a>Pre-triggers

Het volgende voorbeeld laat zien hoe een pre-trigger wordt gebruikt voor het valideren van de eigenschappen van een Azure Cosmos DB-item dat wordt gemaakt. In dit voorbeeld maken we gebruik van het ToDoList-voorbeeld van de [snelstartgids .NET SQL-API](create-sql-api-dotnet.md) om een timestamp-eigenschap toe te voegen aan een nieuw toegevoegd item als deze er geen bevat.

```javascript
function validateToDoItemTimestamp() {
    var context = getContext();
    var request = context.getRequest();

    // item to be created in the current operation
    var itemToCreate = request.getBody();

    // validate properties
    if (!("timestamp" in itemToCreate)) {
        var ts = new Date();
        itemToCreate["timestamp"] = ts.getTime();
    }

    // update the item that will be created
    request.setBody(itemToCreate);
}
```

Pre-triggers kunnen geen invoerparameters hebben. Het aanvraagobject in de trigger wordt gebruikt voor het bewerken van het aanvraagbericht dat is gekoppeld aan de bewerking. In het vorige voorbeeld wordt de pre-trigger uitgevoerd bij het maken van een Azure Cosmos DB-item, en de berichttekst van de aanvraag bevat het item dat moet worden gemaakt in JSON-indeling.

Wanneer triggers zijn geregistreerd, kunt u de bewerkingen opgeven waarmee deze kan worden uitgevoerd. Deze trigger moet worden gemaakt met een waarde `TriggerOperation` van `TriggerOperation.Create`, wat betekent dat het gebruik van de trigger in een vervangingsbewerking zoals wordt weergegeven in de volgende code niet is toegestaan.

Zie artikelen [Pre-triggers](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) en [Post-triggers](how-to-use-stored-procedures-triggers-udfs.md#post-triggers) voor voorbeelden van het registreren en aanroepen van een pre-trigger. 

### <a id="post-triggers"></a>Post-triggers

Hieronder ziet u een voorbeeld van een post-trigger. Deze trigger zoekt naar het metagegevensitem en werkt dit bij met informatie over het nieuwe item.


```javascript
function updateMetadata() {
var context = getContext();
var container = context.getCollection();
var response = context.getResponse();

// item that was created
var createdItem = response.getBody();

// query for metadata document
var filterQuery = 'SELECT * FROM root r WHERE r.id = "_metadata"';
var accept = container.queryDocuments(container.getSelfLink(), filterQuery,
    updateMetadataCallback);
if(!accept) throw "Unable to update metadata, abort";

function updateMetadataCallback(err, items, responseOptions) {
    if(err) throw new Error("Error" + err.message);
        if(items.length != 1) throw 'Unable to find metadata document';

        var metadataItem = items[0];

        // update metadata
        metadataItem.createdItems += 1;
        metadataItem.createdNames += " " + createdItem.id;
        var accept = container.replaceDocument(metadataItem._self,
            metadataItem, function(err, itemReplaced) {
                    if(err) throw "Unable to update metadata, abort";
            });
        if(!accept) throw "Unable to update metadata, abort";
        return;
}
```

Eén ding dat belangrijk is om te weten is de transactionele uitvoering van triggers in Azure Cosmos DB. De na trigger wordt uitgevoerd als onderdeel van dezelfde transactie voor het onderliggende item zelf. Een uitzondering opgetreden tijdens het uitvoeren van de na mislukt de hele transactie. Alles doorgevoerd wordt teruggedraaid en een uitzondering geretourneerd.

Zie de artikelen [Pre-triggers](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) en [Post-triggers](how-to-use-stored-procedures-triggers-udfs.md#post-triggers) voor voorbeelden van het registreren en aanroepen van een pre-trigger. 

## <a id="udfs"></a>Door de gebruiker gedefinieerde functies schrijven

In het volgende voorbeeld wordt een UDF gemaakt voor het berekenen van de inkomstenbelasting voor verschillende inkomensschijven. Deze door de gebruiker gedefinieerde functie kan vervolgens worden gebruikt in een query. Voor de doeleinden van dit voorbeeld wordt ervan uitgegaan dat er een container is met de naam 'Inkomsten' met de volgende eigenschappen:

```json
{
   "name": "Satya Nadella",
   "country": "USA",
   "income": 70000
}
```

Hier volgt een functiedefinitie voor het berekenen van de inkomstenbelasting voor verschillende inkomensschijven:

```javascript
function tax(income) {

        if(income == undefined)
            throw 'no input';

        if (income < 1000)
            return income * 0.1;
        else if (income < 10000)
            return income * 0.2;
        else
            return income * 0.4;
    }
```

Zie het artikel [Door de gebruiker gedefinieerde functies gebruiken in Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md#udfs) voor voorbeelden van het registreren en gebruiken van een door de gebruiker gedefinieerde functie.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over concepten en het schrijven of gebruiken van procedures, triggers en door de gebruiker gedefinieerde functies in Azure Cosmos DB:

* [Opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies registreren en gebruiken in Azure Cosmos DB](how-to-use-stored-procedures-triggers-udfs.md)

* [Opgeslagen procedures en triggers schrijven met de JavaScript-query-API in Azure Cosmos DB](how-to-write-javascript-query-api.md)

* [Werken met opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies registreren en gebruiken in Azure Cosmos DB](stored-procedures-triggers-udfs.md)

* [Werken met de JavaScript-query-API in Azure Cosmos DB](javascript-query-api.md)
