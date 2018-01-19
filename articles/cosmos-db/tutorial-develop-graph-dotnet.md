---
title: 'Azure Cosmos DB: Ontwikkelen met de Graph API in .NET | Microsoft Docs'
description: Meer informatie over het ontwikkelen met Azure Cosmos DB SQL-API met .NET
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: ddbfe11e4415e1c240914142f4daf54b3032f5d8
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a>Azure Cosmos DB: Ontwikkelen met de Graph API in .NET
Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze zelfstudie laat zien hoe een Azure DB die Cosmos-account maken met de Azure-portal en het maken van een database van de grafiek en een container. De toepassing wordt vervolgens een eenvoudige sociaal netwerk maakt met vier mensen met behulp van de [Graph API](graph-sdk-dotnet.md), vervolgens passeert en de grafiek Gremlin met query's.

Deze zelfstudie bevat de volgende taken:

> [!div class="checklist"]
> * Maak een Azure Cosmos DB-account 
> * Een database van de grafiek en een container maken
> * Hoekpunten en randen naar .NET-objecten te serialiseren
> * Hoekpunten en randen toevoegen
> * Query uitvoeren op de grafiek met Gremlin

## <a name="graphs-in-azure-cosmos-db"></a>De grafieken in Azure Cosmos DB
U kunt Azure Cosmos DB maken, bijwerken en grafieken met behulp van een query de [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) bibliotheek. De Microsoft.Azure.Graph-bibliotheek biedt een methode één uitbreiding `CreateGremlinQuery<T>` boven de `DocumentClient` klasse Gremlin query's uitvoeren.

Gremlin is een functionele programmeertaal die ondersteuning biedt voor schrijven bewerkingen (DML) en query's en traversal bewerkingen. We hebben betrekking op een paar voorbeelden in dit artikel om op te halen met Gremlin uw gestart. Zie [Gremlin query's](gremlin-support.md) voor een gedetailleerd overzicht van Gremlin mogelijkheden beschikbaar zijn in Azure Cosmos DB. 

## <a name="prerequisites"></a>Vereisten
Zorg ervoor dat u over de volgende zaken beschikt:

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 
    * U kunt ook de [lokale emulator](local-emulator.md) voor deze zelfstudie.
* [Visual Studio](http://www.visualstudio.com/).

## <a name="create-database-account"></a>Databaseaccount maken

Begint met het maken van een Azure DB die Cosmos-account in de Azure portal.  

> [!TIP]
> * Hebt u al een Azure DB die Cosmos-account? Als dit het geval is, gaat u verder met [uw Visual Studio-oplossing instellen](#SetupVS)
> * Als u de Emulator Azure Cosmos DB, volgt u de stappen in [Azure Cosmos DB Emulator](local-emulator.md) instellen van de emulator en gaat u verder met [instellen van uw Visual Studio-oplossing](#SetupVS). 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>Uw Visual Studio-oplossing instellen
1. Open **Visual Studio** op uw computer.
2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.
3. In de **nieuw Project** dialoogvenster Selecteer **sjablonen** / **Visual C#** / **Console-App (.NET Framework)**, Geef uw project en klik vervolgens op **OK**.
4. Klik in **Solution Explorer** met de rechtermuisknop op uw nieuwe consoletoepassing. Deze bevindt zich onder uw Visual Studio-oplossing. Klik vervolgens op **Manage NuGet Packages...**
5. In de **NuGet** tabblad **Bladeren**, en het type **Microsoft.Azure.Graphs** in het zoekvak en controleer de **omvatten prerelease-versie**.
6. In de resultaten vinden **Microsoft.Azure.Graphs** en klik op **installeren**.
   
   Als u een bericht ontvangt over het controleren van wijzigingen in de oplossing, klikt u op **OK**. Als u een bericht ontvangt over het accepteren van de licentie, klikt u op **Accepteren**.
   
    De `Microsoft.Azure.Graphs` -bibliotheek biedt een enkel uitbreidingsmethode `CreateGremlinQuery<T>` voor het uitvoeren van bewerkingen Gremlin. Gremlin is een functionele programmeertaal die ondersteuning biedt voor schrijven bewerkingen (DML) en query's en traversal bewerkingen. We hebben betrekking op een paar voorbeelden in dit artikel om op te halen met Gremlin uw gestart. [Query's gremlin](gremlin-support.md) heeft een gedetailleerd overzicht van Gremlin mogelijkheden in Azure Cosmos DB.

## <a id="add-references"></a>Verbinding maken met uw app

Deze twee constanten toevoegen en uw *client* variabele in uw toepassing. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
Vervolgens head terug naar de [Azure-portal](https://portal.azure.com) voor het ophalen van uw eindpunt-URL en de primaire sleutel. Uw toepassing heeft de eindpunt-URL en de primaire sleutel nodig om te weten waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat Azure Cosmos DB de verbinding van uw toepassing vertrouwt. 

Navigeer in de Azure-portal naar uw Azure DB die Cosmos-account, klikt u op **sleutels**, en klik vervolgens op **lezen-schrijven sleutels**. 

De URI van de portal kopiëren en plakken via `Endpoint` in de bovenstaande endpoint-eigenschap. Vervolgens kopieert u de primaire sleutel van de portal en plak deze in de `AuthKey` eigenschap hierboven. 

![Schermopname van de Azure portal gebruikt in de zelfstudie voor het maken van een C#-toepassing. Toont een Cosmos Azure DB account de knop sleutels gemarkeerd op de navigatiebalk Azure Cosmos DB en de waarden URI en primaire sleutel gemarkeerd op de blade sleutels](./media/tutorial-develop-graph-dotnet/keys.png) 
 
## <a id="instantiate"></a>De DocumentClient instantiëren 
Maak vervolgens een nieuw exemplaar van de **DocumentClient**.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Een database maken 

Maak nu een Cosmos Azure DB [database](sql-api-resources.md#databases) met behulp van de [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) methode of [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) methode van de  **DocumentClient** klasse van de [SQL .NET SDK](sql-api-sdk-dotnet.md).  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Een grafiek maken 

Maak vervolgens een grafiek container door met behulp van de [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) methode of [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) methode van de **DocumentClient** klasse. Een verzameling is een container van grafiek entiteiten. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <a id="serializing"></a>Hoekpunten en randen naar .NET-objecten te serialiseren
Azure Cosmos DB gebruikt de [GraphSON draadindeling](gremlin-support.md), definieert een JSON-schema voor hoekpunten, randen en eigenschappen. De Azure Cosmos DB .NET SDK omvat JSON.NET als een afhankelijkheid en deze manier kunnen wij serialisatie GraphSON in .NET-objecten waarmee we in de code kunt werken.

Als voorbeeld gaan we werken met een eenvoudige sociaal netwerk met vier mensen. Kijken we maken `Person` hoekpunten, toevoegen `Knows` relaties tussen deze twee, en vervolgens vragen en in de grafiek om te zoeken naar 'vriend van vriend' relaties passeren. 

De `Microsoft.Azure.Graphs.Elements` naamruimte biedt `Vertex`, `Edge`, `Property` en `VertexProperty` klassen voor het deserialiseren van antwoorden GraphSON goed gedefinieerde .NET-objecten.

## <a name="run-gremlin-using-creategremlinquery"></a>Met behulp van CreateGremlinQuery Gremlin uitvoeren
Gremlin, zoals SQL, biedt ondersteuning voor lezen, schrijven en querybewerkingen. Bijvoorbeeld, het volgende fragment toont het maken van de hoekpunten, randen, voeren sommige voorbeeldquery's met behulp van `CreateGremlinQuery<T>`, en doorlopen asynchroon deze resultaten met `ExecuteNextAsync` en ' HasMoreResults.

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

Bekijk de Gremlin-instructies in de voorgaande sectie nader weergegeven. Eerste we enkele hoekpunten met behulp van Gremlin `addV` methode. Het volgende codefragment maakt bijvoorbeeld een hoekpunt 'Thomas Andersen' van het type 'Persoon' met eigenschappen voor de voornaam en achternaam leeftijd.

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

Vervolgens maken we enkele randen tussen deze hoekpunten met behulp van Gremlin `addE` methode. 

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

We een bestaande hoekpunt kunt bijwerken met behulp van `properties` stap in Gremlin. We de aanroep van de query via overslaan `HasMoreResults` en `ExecuteNextAsync` voor de rest van de voorbeelden.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

U kunt neerzetten randen en hoekpunten met behulp van Gremlin `drop` stap. Hier volgt een codefragment die laat zien hoe een hoekpunt en een edge te verwijderen. Houd er rekening mee dat u verwijdert een hoekpunt een trapsgewijze delete van de bijbehorende randen uitvoert.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a>Query uitvoeren op de grafiek

U kunt query's en traversals ook met Gremlin uitvoeren. Bijvoorbeeld, toont het volgende fragment hoe u het aantal hoekpunten in de grafiek:

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
U kunt uitvoeren met behulp van Gremlin filters `has` en `hasLabel` stappen en ze combineren met `and`, `or`, en `not` om complexere filters samen te stellen:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

U kunt bepaalde eigenschappen in de resultaten van de query met projecteren de `values` stap:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Tot nu toe hebt we alleen query's die in elke database werken gezien. Grafieken zijn snelle en efficiënte voor traversal bewerkingen als u nodig hebt om te navigeren naar de gerelateerde randen en hoekpunten. We vinden alle vrienden of van Thomas. We doen dit met behulp van Gremlin `outE` stap vinden alle de uitgaande randen van Thomas en klik op Bladeren naar de hoekpunten van de randen van Gremlin met `inV` stap:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

De volgende query voert twee hops om alle te vinden Thomas 'vrienden of van vrienden ', door het aanroepen van `outE` en `inV` twee keer. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

Kunt u complexere query's maken en implementeren van krachtige grafiek traversal logica Gremlin, met inbegrip van de combinatie van filterexpressies, uitvoeren met behulp van samenvoegartikel met de `loop` stap en uitvoering voorwaardelijke navigatie met de `choose` stap. Meer informatie over wat u met doen kunt [Gremlin ondersteuning](gremlin-support.md)!

Deze zelfstudie Azure Cosmos DB is voltooid, dat is alles! 

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app verder niet gaat gebruiken, kunt u met de volgende stappen alle resources verwijderen die met deze zelfstudies in Azure Portal zijn gemaakt.  

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Een Azure DB die Cosmos-account gemaakt 
> * Een grafiek database en de container gemaakt
> * Geserialiseerde hoekpunten en randen naar .NET-objecten
> * Toegevoegde hoekpunten en randen
> * De grafiek met Gremlin opgevraagd

U kunt nu complexere query's maken en met Gremlin krachtige logica implementeren om door een graaf te gaan. 

> [!div class="nextstepaction"]
> [Query’s uitvoeren met Gremlin](tutorial-query-graph.md)
