---
title: 'Azure Cosmos DB: ontwikkelen met de Graph-API in .NET | Microsoft Docs'
description: Meer informatie over het ontwikkelen met de SQL-API van Azure Cosmos DB met .NET
services: cosmos-db
documentationcenter: ''
author: luisbosquez
manager: kfile
editor: ''
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: 1843e37d9baf1ab264db96109eb5ffd0704e35b7
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/17/2018
ms.locfileid: "34271286"
---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a>Azure Cosmos DB: ontwikkelen met de Graph-API in .NET
Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze zelfstudie laat zien hoe u een Azure Cosmos DB-account kunt maken met behulp van de Azure-portal, en hoe u een graafdatabase en -container kunt maken. De toepassing maakt dan een eenvoudig sociaal netwerk met vier personen die de [Graph-API ](graph-sdk-dotnet.md)gebruiken, en doorkruist en voert query's uit op de graaf met behulp van Gremlin.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Maak een Azure Cosmos DB-account 
> * Een graafdatabase en -container maken
> * Hoekpunten en randen naar .NET-objecten serialiseren
> * Hoekpunten en randen toevoegen
> * Query uitvoeren op de graaf met behulp van Gremlin

## <a name="graphs-in-azure-cosmos-db"></a>Grafen in Azure Cosmos DB
U kunt Azure Cosmos DB gebruiken om grafen te maken, bij te werken en op te vragen met behulp van de bibliotheek [Microsoft.Azure.Graphs](graph-sdk-dotnet.md). De Microsoft.Azure.Graph-bibliotheek biedt een uitbreidingsmethode, `CreateGremlinQuery<T>`, op de `DocumentClient`-klasse om Gremlin query's uit te voeren.

Gremlin is een functionele programmeertaal die ondersteuning biedt voor schrijfbewerkingen (DML) en query- en doorkruisingsbewerkingen. In dit artikel geven we een paar voorbeelden om u op weg te helpen met Gremlin. Zie [Gremlin query's](gremlin-support.md) voor een gedetailleerd overzicht van Gremlin-mogelijkheden die beschikbaar zijn in Azure Cosmos DB. 

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 
    * U kunt voor deze zelfstudie ook de [lokale emulator](local-emulator.md) gebruiken.
* [Visual Studio](http://www.visualstudio.com/).

## <a name="create-database-account"></a>Databaseaccount maken

Begin met het maken van een Azure Cosmos DB-account in Azure Portal.  

> [!TIP]
> * Hebt u al een Azure Cosmos DB-account? Zo ja, ga dan verder met [Uw Visual Studio-oplossing instellen](#SetupVS)
> * Als u de Azure Cosmos DB Emulator gebruikt, volgt u de stappen in [Azure Cosmos DB Emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Uw Visual Studio-oplossing instellen](#SetupVS) te gaan. 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>Uw Visual Studio-oplossing instellen
1. Open **Visual Studio** op uw computer.
2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.
3. Selecteer in het dialoogvenster **Nieuw project** achtereenvolgens **Sjablonen** / **Visual C#** / **Consoletoepassing (.NET Framework)**, geef een naam op voor uw op en klik vervolgens op **OK**.
4. Klik in **Solution Explorer** met de rechtermuisknop op uw nieuwe consoletoepassing. Deze bevindt zich onder uw Visual Studio-oplossing. Klik vervolgens op **Manage NuGet Packages...**
5. Klik op het tabblad **NuGet** op **Bladeren** en typ **Microsoft.Azure.Graphs** in het zoekvak, en schakel  **Inclusief voorlopige versies** in.
6. Zoek **Microsoft.Azure.Graphs** in de resultaten en klik op **Installeren**.
   
   Als u een bericht ontvangt over het controleren van wijzigingen in de oplossing, klikt u op **OK**. Als u een bericht ontvangt over het accepteren van de licentie, klikt u op **Accepteren**.
   
    De `Microsoft.Azure.Graphs`-bibliotheek bevat één uitbreidingsmethode, `CreateGremlinQuery<T>`, voor het uitvoeren van Gremlin-bewerkingen. Gremlin is een functionele programmeertaal die ondersteuning biedt voor schrijfbewerkingen (DML) en query- en doorkruisingsbewerkingen. In dit artikel geven we een paar voorbeelden om u op weg te helpen met Gremlin. [Gremlin query's](gremlin-support.md) bevat een gedetailleerd overzicht van Gremlin-mogelijkheden in Azure Cosmos DB.

## <a id="add-references"></a>Uw app verbinden

Voeg deze twee constanten en uw *client*variabele toe aan uw toepassing. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
Ga vervolgens terug naar de [Azure-portal](https://portal.azure.com) om uw eindpunt-URL en primaire sleutel op te halen. Uw toepassing heeft de eindpunt-URL en de primaire sleutel nodig om te weten waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat Azure Cosmos DB de verbinding van uw toepassing vertrouwt. 

Navigeer in Azure Portal naar uw Azure Cosmos DB-account, klik op **Sleutels** en daarna op **Sleutels voor lezen/schrijven**. 

Kopieer in de portal de URI en plak deze over `Endpoint` in de bovengenoemde eindpunteigenschap. Kopieer vervolgens de PRIMAIRE SLEUTEL van de portal en plak deze in de `AuthKey`-eigenschap hierboven. 

![Schermopname van de Azure-portal die in de zelfstudie wordt gebruikt om een C#-toepassing te maken. Schermopname van Azure Portal waarin de knop SLEUTELS is gemarkeerd in de navigatie van Azure Cosmos DB en de waarden URI en PRIMAIRE SLEUTEL zijn gemarkeerd op de blade Sleutels](./media/tutorial-develop-graph-dotnet/keys.png) 
 
## <a id="instantiate"></a>De DocumentClient maken 
Maak vervolgens een nieuw exemplaar van de **DocumentClient**.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Een database maken 

Maak nu een Cosmos Azure DB-[database](sql-api-resources.md#databases) met behulp van de methode [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) of de methode [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) van de  **DocumentClient**-klasse van de [SQL .NET SDK](sql-api-sdk-dotnet.md).  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Graaf maken 

Maak vervolgens een graafcontainer met behulp van de methode [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) of [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) van de klasse **DocumentClient**. Een verzameling is een container van graafentiteiten. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 400 }); 
``` 

## <a id="serializing"></a>Hoekpunten en randen naar .NET-objecten serialiseren
In Azure Cosmos DB wordt de [GraphSON-draadindeling](gremlin-support.md) gebruikt, waarmee een JSON-schema voor hoekpunten, randen en eigenschappen wordt gedefinieerd. De Azure Cosmos DB .NET SDK bevat JSON.NET als een afhankelijkheid, waardoor we GraphSON kunnen (de)serialiseren naar .NET-objecten waarmee we in de code kunnen werken.

Als voorbeeld gaan we werken met een eenvoudig sociaal netwerk met vier personen. We bekijken hoe we `Person`-hoekpunten kunnen maken, hoe we `Knows`-relaties daartussen kunnen toevoegen, en hoe we vervolgens de graaf kunnen opvragen en doorkruisen om ‘vriend van een vriend’-relaties te vinden. 

De `Microsoft.Azure.Graphs.Elements`-naamruimte bevat de klassen `Vertex`, `Edge`, `Property` en `VertexProperty` klassen voor het deserialiseren van GraphSON-antwoorden op goed gedefinieerde .NET-objecten.

## <a name="run-gremlin-using-creategremlinquery"></a>Gremlin uitvoeren met behulp van CreateGremlinQuery
Net als SQL biedt Gremlin ondersteuning voor lees-, schrijf- en querybewerkingen. Het volgende fragment laat bijvoorbeeld zien hoe u hoekpunten en randen kunt maken, hoe u enkele voorbeeldquery’s kunt uitvoeren met behulp van `CreateGremlinQuery<T>`, en hoe u deze resultaten met behulp van `ExecuteNextAsync` en `HasMoreResults` herhaald kunt doorlopen.

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a>Hoekpunten en randen toevoegen

Laten we de Gremlin-instructies uit de voorgaande secties eens nader bekijken. Eerste voegen we enkele hoekpunten toe met behulp van de methode `addV` van Gremlin. In het volgende fragment wordt bijvoorbeeld een hoekpunt ‘Thomas Andersen’ van het type ‘Persoon’ gemaakt, met eigenschappen voor voornaam en leeftijd.

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas').property('age', 44)");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

Vervolgens maken we randen tussen deze hoekpunten met behulp van de `addE`-methode van Gremlin. 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

We kunnen een bestaand hoekpunt bijwerken met behulp van de stap `properties` in Gremlin. We slaan de aanroep van de query over om de query uit te voeren via `HasMoreResults` en `ExecuteNextAsync` voor de rest van de voorbeelden.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

U kunt randen en hoekpunten verwijderen met behulp van de stap `drop` van Gremlin. Hier volgt een codefragment dat laat zien hoe u een hoekpunt en een rand kunt verwijderen. U ziet dat door een hoekpunt te verwijderen, de bijbehorende randen trapsgewijs worden verwijderd.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a>Query uitvoeren op de graaf

U kunt met behulp van Gremlin ook query's en doorkruisingen uitvoeren. In het volgende fragment ziet u bijvoorbeeld hoe u het aantal hoekpunten in de graaf telt:

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
U kunt filters toepassen met behulp van de stappen `has` en `hasLabel` van Gremlin en deze combineren met `and`, `or` en `not` om complexere filters samen te stellen:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

U kunt bepaalde eigenschappen in de queryresultaten projecteren met de stap `values`:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Tot nu toe hebt u alleen query's gezien die in elke database werken. Grafieken zijn snel en efficiënt voor overdrachtbewerkingen wanneer u naar gerelateerde randen en hoekpunten moet navigeren. We gaan alle vrienden van Thomas zoeken. Daarvoor gebruiken we stap `outE` van Gremlin om alle uitgaande randen van Thomas te zoeken en vervolgens vanuit die randen de inkomende hoekpunten te onderzoeken met behulp van stap `inV` van Gremlin:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

Met de volgende query worden twee hops uitgevoerd om alle 'vrienden van vrienden' van Thomas te vinden, door `outE` en `inV` twee keer aan te roepen. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

U kunt complexere query's maken en krachtige logica voor grafiekdoorkruising implementeren met Gremlin, met inbegrip van het combineren van filterexpressies, het uitvoeren van lusconstructies met stap `loop` en de implementatie van voorwaardelijke navigatie met stap `choose`. Meer informatie over wat u kunt doen kunt [Gremlin-ondersteuning](gremlin-support.md)

Dat was het, u bent klaar met deze Azure Cosmos DB-zelfstudie! 

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app verder niet gaat gebruiken, kunt u met de volgende stappen alle resources verwijderen die met deze zelfstudies in Azure Portal zijn gemaakt.  

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Een Azure Cosmos DB-account gemaakt 
> * Een graafdatabase en -container gemaakt
> * Hoekpunten en randen naar .NET-objecten geserialiseerd
> * Hoekpunten en randen toegevoegd
> * Query uitgevoerd op de graaf met behulp van Gremlin

U kunt nu complexere query's maken en met Gremlin krachtige logica implementeren om door een graaf te gaan. 

> [!div class="nextstepaction"]
> [Query’s uitvoeren met Gremlin](tutorial-query-graph.md)
