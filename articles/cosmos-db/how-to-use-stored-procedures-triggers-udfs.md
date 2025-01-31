---
title: Opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies aanroepen met behulp van Azure Cosmos DB-SDK's
description: Informatie over het registeren en aanroepen van opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies met behulp van de Azure Cosmos DB-SDK's
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/21/2019
ms.author: mjbrown
ms.openlocfilehash: d8c32c0122243edc0d202306abe3e47e78090fad
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551105"
---
# <a name="how-to-register-and-use-stored-procedures-triggers-and-user-defined-functions-in-azure-cosmos-db"></a>Opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies registreren en gebruiken in Azure Cosmos DB

De SQL-API in Azure Cosmos DB biedt ondersteuning voor het registreren en aanroepen van opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies (UDF's) die zijn geschreven in JavaScript. U kunt de SQL-API [.NET](sql-api-sdk-dotnet.md), [.NET Core](sql-api-sdk-dotnet-core.md), [Java](sql-api-sdk-java.md), [JavaScript](sql-api-sdk-node.md), [Node.js](sql-api-sdk-node.md), of [Python](sql-api-sdk-python.md) SDK's voor het registreren en aanroepen van de opgeslagen procedures. Als u een of meer opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies hebt gedefinieerd, kunt u deze laden en weergeven in [Azure Portal](https://portal.azure.com/) met behulp van Data Explorer.

## <a id="stored-procedures"></a>Opgeslagen procedures uitvoeren

Opgeslagen procedures worden geschreven met behulp van JavaScript. U kunt er items in een Azure Cosmos-container mee maken, bijwerken, lezen en verwijderen en query's op items uitvoeren. Zie voor meer informatie over het schrijven van opgeslagen procedures in Azure Cosmos DB [over het schrijven van opgeslagen procedures in Azure Cosmos DB](how-to-write-stored-procedures-triggers-udfs.md#stored-procedures) artikel.

De volgende voorbeelden laten zien hoe u een opgeslagen procedure registreert en aanroept met behulp van de Azure Cosmos DB-SDK's. Raadpleeg [Een document maken](how-to-write-stored-procedures-triggers-udfs.md#create-an-item) als de bron voor deze opgeslagen procedure is opgeslagen als `spCreateToDoItem.js`.

> [!NOTE]
> Bij het uitvoeren van een opgeslagen procedure voor gepartitioneerde containers moet een waarde voor de partitiesleutel worden opgegeven in de aanvraagopties. Opgeslagen procedures zijn altijd gerelateerd aan een partitiesleutel. Items met een andere partitiesleutelwaarde zijn niet zichtbaar voor de opgeslagen procedure. Dit geldt ook voor triggers.

### <a name="stored-procedures---net-sdk"></a>Opgeslagen procedures - .NET SDK

Het volgende voorbeeld laat zien hoe u een opgeslagen procedure registreert met behulp van de .NET SDK:

```csharp
string storedProcedureId = "spCreateToDoItem";
StoredProcedure newStoredProcedure = new StoredProcedure
   {
       Id = storedProcedureId,
       Body = File.ReadAllText($@"..\js\{storedProcedureId}.js")
   };
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
var response = await client.CreateStoredProcedureAsync(containerUri, newStoredProcedure);
StoredProcedure createdStoredProcedure = response.Resource;
```

De volgende code laat zien hoe u een opgeslagen procedure aanroept met behulp van de .NET SDK:

```csharp
dynamic newItem = new
{
    category = "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
};

Uri uri = UriFactory.CreateStoredProcedureUri("myDatabase", "myContainer", "spCreateToDoItem");
RequestOptions options = new RequestOptions { PartitionKey = new PartitionKey("Personal") };
var result = await client.ExecuteStoredProcedureAsync<string>(uri, options, newItem);
var id = result.Response;
```

### <a name="stored-procedures---java-sdk"></a>Opgeslagen procedures - Java SDK

Het volgende voorbeeld laat zien hoe u een opgeslagen procedure registreert met behulp van de Java SDK:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
StoredProcedure newStoredProcedure = new StoredProcedure(
    "{" +
        "  'id':'spCreateToDoItem'," +
        "  'body':" + new String(Files.readAllBytes(Paths.get("..\\js\\spCreateToDoItem.js"))) +
    "}");
//toBlocking() blocks the thread until the operation is complete and is used only for demo.  
StoredProcedure createdStoredProcedure = asyncClient.createStoredProcedure(containerLink, newStoredProcedure, null)
    .toBlocking().single().getResource();
```

De volgende code laat zien hoe u een opgeslagen procedure aanroept met behulp van de Java SDK:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String sprocLink = String.format("%s/sprocs/%s", containerLink, "spCreateToDoItem");
final CountDownLatch successfulCompletionLatch = new CountDownLatch(1);

class ToDoItem {
    public String category;
    public String name;
    public String description;
    public boolean isComplete;
}

ToDoItem newItem = new ToDoItem();
newItem.category = "Personal";
newItem.name = "Groceries";
newItem.description = "Pick up strawberries";
newItem.isComplete = false;

RequestOptions requestOptions = new RequestOptions();
requestOptions.setPartitionKey(new PartitionKey("Personal"));

Object[] storedProcedureArgs = new Object[] { newItem };
asyncClient.executeStoredProcedure(sprocLink, requestOptions, storedProcedureArgs)
    .subscribe(storedProcedureResponse -> {
        String storedProcResultAsString = storedProcedureResponse.getResponseAsString();
        successfulCompletionLatch.countDown();
        System.out.println(storedProcedureResponse.getActivityId());
    }, error -> {
        System.err.println("an error occurred while executing the stored procedure: actual cause: "
                + error.getMessage());
    });

successfulCompletionLatch.await();
```

### <a name="stored-procedures---javascript-sdk"></a>Opgeslagen procedures - JavaScript SDK

Het volgende voorbeeld laat zien hoe u een opgeslagen procedure registreert met behulp van de JavaScript SDK

```javascript
const container = client.database("myDatabase").container("myContainer");
const sprocId = "spCreateToDoItem";
await container.storedProcedures.create({
    id: sprocId,
    body: require(`../js/${sprocId}`)
});
```

De volgende code laat zien hoe u een opgeslagen procedure aanroept met behulp van de JavaScript SDK:

```javascript
const newItem = [{
    category: "Personal",
    name: "Groceries",
    description: "Pick up strawberries",
    isComplete: false
}];
const container = client.database("myDatabase").container("myContainer");
const sprocId = "spCreateToDoItem";
const {body: result} = await container.storedProcedure(sprocId).execute(newItem, {partitionKey: newItem[0].category});
```

### <a name="stored-procedures---python-sdk"></a>Opgeslagen procedures - Python SDK

Het volgende voorbeeld laat zien hoe u een opgeslagen procedure registreert met behulp van de Python SDK

```python
with open('../js/spCreateToDoItem.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
sproc_definition = {
            'id': 'spCreateToDoItem',
            'serverScript': file_contents,
        }
sproc = client.CreateStoredProcedure(container_link, sproc_definition)
```

De volgende code laat zien hoe u een opgeslagen procedure aanroept met behulp van de Python SDK

```python
sproc_link = 'dbs/myDatabase/colls/myContainer/sprocs/spCreateToDoItem'
new_item = [{
    'category':'Personal',
    'name':'Groceries',
    'description':'Pick up strawberries',
    'isComplete': False
}]
client.ExecuteStoredProcedure(sproc_link, new_item, {'partitionKey': 'Personal'}
```

## <a id="pre-triggers"></a>Voorafgaande triggers uitvoeren

De volgende voorbeelden laten zien hoe u een voorafgaande trigger registreert en aanroept met behulp van de Azure Cosmos DB-SDK's. Raadpleeg [Voorbeeld van voorafgaande trigger](how-to-write-stored-procedures-triggers-udfs.md#pre-triggers) als de bron voor deze voorafgaande trigger is opgeslagen als `trgPreValidateToDoItemTimestamp.js`.

Bij het uitvoeren worden voorafgaande triggers doorgegeven in het object RequestOptions (aanvraagopties) door `PreTriggerInclude` op te geven en vervolgens de naam van de trigger door te geven in een List-object (lijstobject).

> [!NOTE]
> Ook al wordt de naam van de trigger doorgegeven als een lijst, u kunt slechts één trigger per bewerking uitvoeren.

### <a name="pre-triggers---net-sdk"></a>Voorafgaande triggers - .NET SDK

De volgende code laat zien hoe u een voorafgaande trigger registreert met de .NET SDK:

```csharp
string triggerId = "trgPreValidateToDoItemTimestamp";
Trigger trigger = new Trigger
{
    Id =  triggerId,
    Body = File.ReadAllText($@"..\js\{triggerId}.js"),
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Pre
};
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.CreateTriggerAsync(containerUri, trigger);
```

De volgende code laat zien hoe u een voorafgaande trigger aanroept met de .NET SDK:

```csharp
dynamic newItem = new
{
    category = "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
};

Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
RequestOptions requestOptions = new RequestOptions { PreTriggerInclude = new List<string> { "trgPreValidateToDoItemTimestamp" } };
await client.CreateDocumentAsync(containerUri, newItem, requestOptions);
```

### <a name="pre-triggers---java-sdk"></a>Voorafgaande triggers - Java SDK

De volgende code laat zien hoe u een voorafgaande trigger registreert met de Java SDK:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String triggerId = "trgPreValidateToDoItemTimestamp";
Trigger trigger = new Trigger();
trigger.setId(triggerId);
trigger.setBody(new String(Files.readAllBytes(Paths.get(String.format("..\\js\\%s.js", triggerId)));
trigger.setTriggerOperation(TriggerOperation.Create);
trigger.setTriggerType(TriggerType.Pre);
//toBlocking() blocks the thread until the operation is complete and is used only for demo. 
Trigger createdTrigger = asyncClient.createTrigger(containerLink, trigger, new RequestOptions()).toBlocking().single().getResource();
```

De volgende code laat zien hoe u een voorafgaande trigger aanroept met de Java SDK:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
    Document item = new Document("{ "
            + "\"category\": \"Personal\", "
            + "\"name\": \"Groceries\", "
            + "\"description\": \"Pick up strawberries\", "
            + "\"isComplete\": false, "
            + "}"
            );
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPreTriggerInclude(Arrays.asList("trgPreValidateToDoItemTimestamp"));
//toBlocking() blocks the thread until the operation is complete and is used only for demo. 
asyncClient.createDocument(containerLink, item, requestOptions, false).toBlocking();
```

### <a name="pre-triggers---javascript-sdk"></a>Voorafgaande triggers - JavaScript SDK

De volgende code laat zien hoe u een voorafgaande trigger registreert met de JavaScript SDK:

```javascript
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPreValidateToDoItemTimestamp";
await container.triggers.create({
    id: triggerId,
    body: require(`../js/${triggerId}`),
    triggerOperation: "create",
    triggerType: "pre"
});
```

De volgende code laat zien hoe u een voorafgaande trigger aanroept met de JavaScript SDK:

```javascript
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPreValidateToDoItemTimestamp";
await container.items.create({
    category: "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
}, {preTriggerInclude: [triggerId]});
```

### <a name="pre-triggers---python-sdk"></a>Voorafgaande triggers - Python SDK

De volgende code laat zien hoe u een voorafgaande trigger registreert met de Python SDK:

```python
with open('../js/trgPreValidateToDoItemTimestamp.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
trigger_definition = {
            'id': 'trgPreValidateToDoItemTimestamp',
            'serverScript': file_contents,
            'triggerType': documents.TriggerType.Pre,
            'triggerOperation': documents.TriggerOperation.Create
        }
trigger = client.CreateTrigger(container_link, trigger_definition)
```

De volgende code laat zien hoe u een voorafgaande trigger aanroept met de Python SDK:

```python
container_link = 'dbs/myDatabase/colls/myContainer'
item = { 'category': 'Personal', 'name': 'Groceries', 'description':'Pick up strawberries', 'isComplete': False}
client.CreateItem(container_link, item, { 'preTriggerInclude': 'trgPreValidateToDoItemTimestamp'})
```

## <a id="post-triggers"></a>Navolgende triggers uitvoeren

De volgende voorbeelden laten zien hoe u een navolgende trigger registreert met behulp van de Azure Cosmos DB-SDK's. Raadpleeg [Voorbeeld van navolgende trigger](how-to-write-stored-procedures-triggers-udfs.md#post-triggers) als de bron voor deze navolgende trigger is opgeslagen als `trgPostUpdateMetadata.js`.

### <a name="post-triggers---net-sdk"></a>Navolgende triggers - .NET SDK

De volgende code laat zien hoe u een navolgende trigger registreert met de .NET SDK:

```csharp
string triggerId = "trgPostUpdateMetadata";
Trigger trigger = new Trigger
{
    Id = triggerId,
    Body = File.ReadAllText($@"..\js\{triggerId}.js"),
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Post
};
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.CreateTriggerAsync(containerUri, trigger);
```

De volgende code laat zien hoe u een navolgende trigger aanroept met de .NET SDK:

```csharp
var newItem = { 
    name: "artist_profile_1023",
    artist: "The Band",
    albums: ["Hellujah", "Rotators", "Spinning Top"]
};

RequestOptions options = new RequestOptions { PostTriggerInclude = new List<string> { "trgPostUpdateMetadata" } };
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.createDocumentAsync(containerUri, newItem, options);
```

### <a name="post-triggers---java-sdk"></a>Navolgende triggers - Java SDK

De volgende code laat zien hoe u een navolgende trigger registreert met de Java SDK:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String triggerId = "trgPostUpdateMetadata";
Trigger trigger = new Trigger();
trigger.setId(triggerId);
trigger.setBody(new String(Files.readAllBytes(Paths.get(String.format("..\\js\\%s.js", triggerId)))));
trigger.setTriggerOperation(TriggerOperation.Create);
trigger.setTriggerType(TriggerType.Post);
Trigger createdTrigger = asyncClient.createTrigger(containerLink, trigger, new RequestOptions()).toBlocking().single().getResource();
```

De volgende code laat zien hoe u een navolgende trigger aanroept met de Java SDK:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
Document item = new Document(String.format("{ "
    + "\"name\": \"artist_profile_1023\", "
    + "\"artist\": \"The Band\", "
    + "\"albums\": [\"Hellujah\", \"Rotators\", \"Spinning Top\"]"
    + "}"
));
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPostTriggerInclude(Arrays.asList("trgPostUpdateMetadata"));
//toBlocking() blocks the thread until the operation is complete, and is used only for demo.
asyncClient.createDocument(containerLink, item, requestOptions, false).toBlocking();
```

### <a name="post-triggers---javascript-sdk"></a>Navolgende triggers - JavaScript SDK

De volgende code laat zien hoe u een navolgende trigger registreert met de JavaScript SDK:

```javascript
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPostUpdateMetadata";
await container.triggers.create({
    id: triggerId,
    body: require(`../js/${triggerId}`),
    triggerOperation: "create",
    triggerType: "post"
});
```

De volgende code laat zien hoe u een navolgende trigger aanroept met de JavaScript SDK:

```javascript
const item = {
    name: "artist_profile_1023",
    artist: "The Band",
    albums: ["Hellujah", "Rotators", "Spinning Top"]
};
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPostUpdateMetadata";
await container.items.create(item, {postTriggerInclude: [triggerId]});
```

### <a name="post-triggers---python-sdk"></a>Navolgende triggers - Python SDK

De volgende code laat zien hoe u een navolgende trigger registreert met de Python SDK:

```python
with open('../js/trgPostUpdateMetadata.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
trigger_definition = {
            'id': 'trgPostUpdateMetadata',
            'serverScript': file_contents,
            'triggerType': documents.TriggerType.Post,
            'triggerOperation': documents.TriggerOperation.Create
        }
trigger = client.CreateTrigger(container_link, trigger_definition)
```

De volgende code laat zien hoe u een navolgende trigger aanroept met de Python SDK:

```python
container_link = 'dbs/myDatabase/colls/myContainer'
item = { 'name': 'artist_profile_1023', 'artist': 'The Band', 'albums': ['Hellujah', 'Rotators', 'Spinning Top']}
client.CreateItem(container_link, item, { 'postTriggerInclude': 'trgPostUpdateMetadata'})
```

## <a id="udfs"></a>Werken met door de gebruiker gedefinieerde functies

De volgende voorbeelden laten zien hoe u een door de gebruiker gedefinieerde functie registreert met behulp van de Azure Cosmos DB-SDK's. Raadpleeg [Voorbeeld van door de gebruiker gedefinieerde functie](how-to-write-stored-procedures-triggers-udfs.md#udfs) als de bron voor deze door de gebruiker gedefinieerde functie is opgeslagen als `udfTax.js`.

### <a name="user-defined-functions---net-sdk"></a>Door de gebruiker gedefinieerde functies - .NET SDK

De volgende code laat zien hoe u een door de gebruiker gedefinieerde functie registreert met de .NET SDK:

```csharp
string udfId = "Tax";
var udfTax = new UserDefinedFunction
{
    Id = udfId,
    Body = File.ReadAllText($@"..\js\{udfId}.js"),
};

Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.CreateUserDefinedFunctionAsync(containerUri, udfTax);

```

De volgende code laat zien hoe u een door de gebruiker gedefinieerde functie aanroept met de .NET SDK:

```csharp
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
var results = client.CreateDocumentQuery<dynamic>(containerUri, "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000"));

foreach (var result in results)
{
    //iterate over results
}
```

### <a name="user-defined-functions---java-sdk"></a>Door de gebruiker gedefinieerde functies - Java SDK

De volgende code laat zien hoe u een door de gebruiker gedefinieerde functie registreert met de Java SDK:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String udfId = "Tax";
UserDefinedFunction udf = new UserDefinedFunction();
udf.setId(udfId);
udf.setBody(new String(Files.readAllBytes(Paths.get(String.format("..\\js\\%s.js", udfId)))));
//toBlocking() blocks the thread until the operation is complete and is used only for demo.
UserDefinedFunction createdUDF = client.createUserDefinedFunction(containerLink, udf, new RequestOptions()).toBlocking().single().getResource();
```

De volgende code laat zien hoe u een door de gebruiker gedefinieerde functie aanroept met de Java SDK:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
Observable<FeedResponse<Document>> queryObservable = client.queryDocuments(containerLink, "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000", new FeedOptions());
final CountDownLatch completionLatch = new CountDownLatch(1);
queryObservable.subscribe(
        queryResultPage -> {
            System.out.println("Got a page of query result with " +
                    queryResultPage.getResults().size());
        },
        // terminal error signal
        e -> {
            e.printStackTrace();
            completionLatch.countDown();
        },

        // terminal completion signal
        () -> {
            completionLatch.countDown();
        });
completionLatch.await();
```

### <a name="user-defined-functions---javascript-sdk"></a>Door de gebruiker gedefinieerde functies - JavaScript SDK

De volgende code laat zien hoe u een door de gebruiker gedefinieerde functie registreert met de JavaScript SDK:

```javascript
const container = client.database("myDatabase").container("myContainer");
const udfId = "Tax";
await container.userDefinedFunctions.create({
    id: udfId,
    body: require(`../js/${udfId}`)
```

De volgende code laat zien hoe u een door de gebruiker gedefinieerde functie aanroept met de JavaScript SDK:

```javascript
const container = client.database("myDatabase").container("myContainer");
const sql = "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000";
const {result} = await container.items.query(sql).toArray();
```

### <a name="user-defined-functions---python-sdk"></a>Door de gebruiker gedefinieerde functies - Python SDK

De volgende code laat zien hoe u een door de gebruiker gedefinieerde functie registreert met de Python SDK:

```python
with open('../js/udfTax.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
udf_definition = {
            'id': 'Tax',
            'serverScript': file_contents,
        }
udf = client.CreateUserDefinedFunction(container_link, udf_definition)
```

De volgende code laat zien hoe u een door de gebruiker gedefinieerde functie aanroept met de Python SDK:

```python
container_link = 'dbs/myDatabase/colls/myContainer'
results = list(client.QueryItems(container_link, 'SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000'))
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over concepten en het schrijven of gebruiken van procedures, triggers en door de gebruiker gedefinieerde functies in Azure Cosmos DB:

- [Werken met opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies registreren en gebruiken in Azure Cosmos DB](stored-procedures-triggers-udfs.md)
- [Werken met de JavaScript-query-API in Azure Cosmos DB](javascript-query-api.md)
- [Opgeslagen procedures, triggers en door de gebruiker gedefinieerde functies schrijven in Azure Cosmos DB](how-to-write-stored-procedures-triggers-udfs.md)
- [Opgeslagen procedures en triggers schrijven met de JavaScript-query-API in Azure Cosmos DB](how-to-write-javascript-query-api.md)
