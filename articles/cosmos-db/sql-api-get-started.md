---
title: Een .NET-consoletoepassing voor het beheren van gegevens in Azure Cosmos DB SQL API-account maken
description: Een zelfstudie waarmee u een online database maakt en C# console-app met behulp van de SQL-API.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: sngun
ms.openlocfilehash: 64aef17663fdc28a467172bbe8954fc06fdb7ff0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60686395"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Een .NET-consoletoepassing voor het beheren van gegevens in Azure Cosmos DB SQL API-account maken

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET (preview)](sql-api-dotnet-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET Core (preview)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Welkom bij de Azure Cosmos DB SQL API-get-handleiding. Nadat u deze zelfstudie hebt voltooid, hebt u een console-app maakt en query's Azure Cosmos DB-resources.

In deze zelfstudie ontdekt u hoe u:

> [!div class="checklist"]
>
> - Een Azure Cosmos DB-account maken en er verbinding mee maken
> - Een Visual Studio-oplossing configureren
> - Een database maken
> - Een verzameling maken
> - JSON-documenten maken
> - Query uitvoeren op de verzameling
> - Een JSON-document bijwerken
> - Een document verwijderen
> - De database verwijderen

## <a name="prerequisites"></a>Vereisten

Visual Studio 2017 met de Azure-ontwikkeling-werkstroom geïnstalleerd:
- U kunt downloaden en gebruiken de **gratis** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio. 

Een Azure-abonnement of een gratis proefaccount van Cosmos DB:
- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
  
- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  
  
- Als u de Azure Cosmos DB-Emulator gebruikt, volgt u de stappen in [Azure Cosmos DB-Emulator](local-emulator.md) voor het instellen van de emulator. Start de zelfstudie op [instellen van de Visual Studio-oplossing](#SetupVS).
  
## <a name="get-the-completed-solution"></a>De voltooide oplossing ophalen

Als u geen tijd om de zelfstudie hebt voltooid, of gewoon de codevoorbeelden wilt, kunt u de volledige oplossing uit downloaden [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). 

De gedownloade volledige oplossing uitvoeren: 

1. Zorg ervoor dat u hebt de [vereisten](#prerequisites) geïnstalleerd. 
1. Open het gedownloade *GetStarted.sln* oplossingsbestand in Visual Studio.
1. In **Solution Explorer**, met de rechtermuisknop op de **GetStarted** project en selecteer **NuGet-pakketten beheren**.
1. Op de **NuGet** tabblad **herstellen** om terug te zetten van de verwijzingen naar de Azure Cosmos DB .NET SDK.
1. In de *App.config* bestand de `EndpointUrl` en `PrimaryKey` waarden zoals beschreven in de [verbinding maken met de Azure Cosmos DB-account](#Connect) sectie.
1. Selecteer **Debug** > **starten zonder foutopsporing** of druk op **Ctrl**+**F5** wilt bouwen en uitvoeren van de app.

## <a name="create-an-azure-cosmos-db-account"></a>Maak een Azure Cosmos DB-account

Volg deze instructies voor het maken van een Azure Cosmos DB-account in Azure portal. Als u al een Azure Cosmos DB-account moet worden gebruikt, gaat u verder met [instellen van de Visual Studio-oplossing](#SetupVS). 

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>De Visual Studio-oplossing instellen

1. Selecteer in Visual Studio 2017 **bestand** > **nieuw** > **Project**.
   
1. In de **nieuw Project** dialoogvenster, selecteer **Visual C#**   >  **Console-App (.NET Framework)**, Geef uw project de naam *AzureCosmosDBApp* , en selecteer vervolgens **OK**.
   
   ![Schermafbeelding van het venster Nieuw project](./media/sql-api-get-started/nosql-tutorial-new-project-2.png)
   
1. In **Solution Explorer**, met de rechtermuisknop op de **AzureCosmosDBApp** project en selecteer **NuGet-pakketten beheren**.
   
   ![Contextmenu project](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges.png)
   
1. Op de **NuGet** tabblad **Bladeren**, en voer *azure documentdb* in het zoekvak in.
   
1. Zoek en selecteer **Microsoft.Azure.DocumentDB**, en selecteer **installeren** als deze nog niet geïnstalleerd.
   
   De pakket-id voor de Azure Cosmos DB SQL-API-clientbibliotheek is [Microsoft Azure Cosmos DB-clientbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).

   > [!NOTE]
   > Als u .NET Core gebruikt, raadpleegt u [de .NET Core docs](./sql-api-dotnetcore-get-started.md).

   ![Schermopname van het NuGet-Menu voor het vinden van Azure Cosmos DB-Client-SDK](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)
   
   Als u een bericht ontvangt over wijzigingen in de oplossing is beschikbaar als Preview, selecteer **OK**. Als u een bericht ontvangt over de acceptatie van de licentie, selecteer **ik ga akkoord met**.

## <a id="Connect"></a>Verbinding maken met de Azure Cosmos DB-account

Nu aan de slag schrijven van code. De volledige *Project.cs* voor deze zelfstudie is in het bestand [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs).

1. In **Solution Explorer**, selecteer *Program.cs*, en voeg de volgende verwijzingen naar het begin van het bestand in de code-editor:
   
   ```csharp
   using System.Net;
   using Microsoft.Azure.Documents;
   using Microsoft.Azure.Documents.Client;
   using Newtonsoft.Json;
   ```
   
1. Voeg vervolgens de volgende twee constanten en de `client` variabele `public class Program`.
   
   ```csharp
   
   public class Program
   {
      private const string EndpointUrl = "<your endpoint URL>";
      private const string PrimaryKey = "<your primary key>";
      private DocumentClient client;
   ```
   
1. De eindpunt-URL en primaire sleutel kunnen uw app verbinding maken met uw Azure Cosmos DB-account en de Azure Cosmos DB-account te vertrouwen van de verbinding. Kopieer de sleutels van de [Azure-portal](https://portal.azure.com), en plak deze in uw code. 

   
   1. Selecteer in uw Azure Cosmos DB-account linkernavigatievenster **sleutels**.
      
      ![Toegang tot sleutels in Azure portal bekijken en kopiëren](./media/sql-api-get-started/nosql-tutorial-keys.png)
      
   1. Onder **sleutels voor lezen / schrijven**, Kopieer de **URI** waarde met de kopieerknop aan de rechterkant en plak deze in `<your endpoint URL>` in *Program.cs*. Bijvoorbeeld: 
      
      `private const string EndpointUrl = "https://mysqlapicosmosdb.documents.azure.com:443/";`
      
   1. Kopieer de **primaire sleutel** waarde en plak deze in `<your primary key>` in *Program.cs*. Bijvoorbeeld: 
      
      `private const string PrimaryKey = "19ZDNJAiYL26tmnRvoez6hmtIfBGwjun50PWRjNYMC2ig8Ob9hYk7Fq1RYSv8FcIYnh1TdBISvCh7s6yyb0000==";`
   
1. Na de `Main` -methode een asynchrone taak met de naam `GetStartedDemo`, die wordt een nieuwe `DocumentClient` met de naam `client`.
   
   ```csharp
      private async Task GetStartedDemo()
      {
        client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
      }
   ```
   
1. Voeg de volgende code aan de `Main` methode om uit te voeren de `GetStartedDemo` taak. De `Main` methode vangt uitzonderingen en schrijft deze naar de console.
   
   ```csharp
      static void Main(string[] args)
      {
        try
        {
           Program p = new Program();
           p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
           Exception baseException = de.GetBaseException();
           Console.WriteLine($"{de.StatusCode} error occurred: {de.Message}, Message: {baseException.Message}");
        }
        catch (Exception e)
        {
           Exception baseException = e.GetBaseException();
           Console.WriteLine($"Error: {e.Message}, Message: {baseException.Message}");
        }
        finally
        {
           Console.WriteLine("End of demo, press any key to exit.");
           Console.ReadKey();
        }
      }
   ```
   
1. Druk op **F5** uw app uit te voeren. 
   
1. Als u het bericht ziet **einde van de demo, drukt u op een willekeurige toets om af te sluiten** in het consolevenster, betekent dit dat de verbinding is geslaagd. Op een willekeurige toets om het consolevenster te sluiten. 

U hebt verbonden met uw Azure Cosmos DB-account. Nu werken met een Azure Cosmos DB-resources.  

## <a name="create-a-database"></a>Een database maken

Een Azure Cosmos DB [database](databases-containers-items.md#azure-cosmos-databases) is een logische container van JSON-documentopslag, gepartitioneerd in verzamelingen. U een database maken met behulp van de [CreateDatabaseIfNotExistsAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync) -methode van de `DocumentClient` klasse. 

1. Voordat u de code voor het maken van een database toevoegt, moet u een Help-methode toevoegen om naar de console te kunnen schrijven. Kopieer en plak het volgende `WriteToConsoleAndPromptToContinue` methode na de `GetStartedDemo` methode in uw code.
   
   ```csharp
   private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
   {
      Console.WriteLine(format, args);
      Console.WriteLine("Press any key to continue...");
      Console.ReadKey();
   }
   ```
   
1. Kopieer en plak de volgende regel om uw `GetStartedDemo` methode na de `client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);` regel. Deze code maakt een database met de naam `FamilyDB`.
   
   ```csharp
      await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
   ```
   
1. Druk op **F5** uw app uit te voeren.

U hebt een Azure Cosmos DB-database gemaakt. Ziet u de database in de [Azure-portal](https://portal.azure.com) hiervoor **Data Explorer** in het linkernavigatievenster van uw Azure Cosmos DB-account. 

## <a id="CreateColl"></a>Een verzameling maken

Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica. U maakt een verzameling met behulp van de [CreateDocumentCollectionIfNotExistsAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync#overloads) -methode van de `DocumentClient` klasse. 

> [!IMPORTANT]
> **CreateDocumentCollectionIfNotExistsAsync** maakt een nieuwe verzameling met gereserveerde doorvoer, wat gevolgen heeft. Voor meer informatie gaat u naar de [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/).
> 

1. Kopieer en plak de volgende code naar uw `GetStartedDemo` methode na de `await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });` regel. Deze code maakt een documentverzameling met de naam `FamilyCollection`.
   
   ```csharp
      await client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });
   ```
   
1. Druk op **F5** uw app uit te voeren.

U hebt een Azure Cosmos DB-documentverzameling gemaakt. U kunt zien dat de verzameling onder uw **FamilyDB** -database in **Data Explorer** in Azure portal.  

## <a id="CreateDoc"></a>JSON-documenten maken

Documenten bestaan uit door gebruiker gedefinieerde, willekeurige JSON-inhoud. Documenten moet een ID-eigenschap geserialiseerd als `id` in JSON. U documenten maken met behulp van de [CreateDocumentAsync](/dotnet/api/microsoft.azure.documents.client.documentclient.createdocumentasync#overloads) -methode van de `DocumentClient` klasse. 

> [!TIP]
> Als u al gegevens die u wilt opslaan in uw database hebt, kunt u de Azure Cosmos DB [hulpprogramma voor gegevensmigratie](import-data.md) om deze te importeren.
>

De volgende code wordt gemaakt en voegt twee documenten in uw databaseverzameling. U maakt eerst een `Family` klasse en `Parent`, `Child`, `Pet`, en `Address` subklassen om te gebruiken binnen `Family`. Maak vervolgens een `CreateFamilyDocumentIfNotExists` methode, en maken en Voeg twee documenten. 

1. Kopieer en plak het volgende `Family`, `Parent`, `Child`, `Pet`, en `Address` klassen na de `WriteToConsoleAndPromptToContinue` methode in uw code.
   
   ```csharp
    public class Family
    {
        [JsonProperty(PropertyName = "id")]
        public string Id { get; set; }
        public string LastName { get; set; }
        public Parent[] Parents { get; set; }
        public Child[] Children { get; set; }
        public Address Address { get; set; }
        public bool IsRegistered { get; set; }
        public override string ToString()
        {
            return JsonConvert.SerializeObject(this);
        }
    }

    public class Parent
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
    }

    public class Child
    {
        public string FamilyName { get; set; }
        public string FirstName { get; set; }
        public string Gender { get; set; }
        public int Grade { get; set; }
        public Pet[] Pets { get; set; }
    }

    public class Pet
    {
        public string GivenName { get; set; }
    }

    public class Address
    {
        public string State { get; set; }
        public string County { get; set; }
        public string City { get; set; }
    }
   ```
   
1. Kopieer en plak het volgende `CreateFamilyDocumentIfNotExists` methode na de `Address` klasse die u zojuist hebt toegevoegd.
   
   ```csharp
    private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
    {
        try
        {
            await client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
            WriteToConsoleAndPromptToContinue($"Found {family.Id}");
        }
        catch (DocumentClientException de)
        {
            if (de.StatusCode == HttpStatusCode.NotFound)
            {
                await client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
                WriteToConsoleAndPromptToContinue($"Created Family {family.Id}");
            }
            else
            {
                throw;
            }
        }
    }
   ```
   
1. Kopieer en plak de volgende code aan het einde van uw `GetStartedDemo` methode na de `await client.CreateDocumentCollectionIfNotExistsAsync` regel. Deze code maakt en voegt twee documenten, één voor de Andersen en Wakefield families.
   
   ```csharp
    Family andersenFamily = new Family
    {
        Id = "AndersenFamily",
        LastName = "Andersen",
        Parents = new Parent[]
        {
            new Parent { FirstName = "Thomas" },
            new Parent { FirstName = "Mary Kay" }
        },
        Children = new Child[]
        {
            new Child
            {
                FirstName = "Henriette Thaulow",
                Gender = "female",
                Grade = 5,
                Pets = new Pet[]
                {
                    new Pet { GivenName = "Fluffy" }
                }
            }
        },
        Address = new Address { State = "WA", County = "King", City = "Seattle" },
        IsRegistered = true
    };

    await CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

    Family wakefieldFamily = new Family
    {
        Id = "WakefieldFamily",
        LastName = "Wakefield",
        Parents = new Parent[]
        {
            new Parent { FamilyName = "Wakefield", FirstName = "Robin" },
            new Parent { FamilyName = "Miller", FirstName = "Ben" }
        },
        Children = new Child[]
        {
            new Child
            {
                FamilyName = "Merriam",
                FirstName = "Jesse",
                Gender = "female",
                Grade = 8,
                Pets = new Pet[]
                {
                    new Pet { GivenName = "Goofy" },
                    new Pet { GivenName = "Shadow" }
                }
            },
            new Child
            {
                FamilyName = "Miller",
                FirstName = "Lisa",
                Gender = "female",
                Grade = 1
            }
        },
        Address = new Address { State = "NY", County = "Manhattan", City = "NY" },
        IsRegistered = false
    };

    await CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);
   ```
   
1. Druk op **F5** uw app uit te voeren.

U hebt twee Azure Cosmos DB-documenten gemaakt. U kunt zien dat de documenten in uw **FamilyDB** database en **FamilyCollection** verzameling in **Data Explorer** in Azure portal.   

![Diagram waarin u ziet de hiërarchische relatie tussen het account, de online database, de verzameling en de documenten](./media/sql-api-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Query's uitvoeren op Azure Cosmos DB-resources

Azure Cosmos DB biedt ondersteuning voor uitgebreide [query's](how-to-sql-query.md) op basis van JSON-documenten die zijn opgeslagen in verzamelingen. De volgende voorbeeldcode maakt gebruik van LINQ- en Azure Cosmos DB SQL-syntaxis voor het uitvoeren van een query voor de voorbeelddocumenten.

1. Kopieer en plak het volgende `ExecuteSimpleQuery` methode na de `CreateFamilyDocumentIfNotExists` methode in uw code.
   
   ```csharp
    private void ExecuteSimpleQuery(string databaseName, string collectionName)
    {
        // Set some common query options.
        FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Find the Andersen family by its LastName.
        IQueryable<Family> familyQuery = client.CreateDocumentQuery<Family>(
            UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
            .Where(f => f.LastName == "Andersen");

        // Execute the query synchronously. 
        // You could also execute it asynchronously using the IDocumentQuery<T> interface.
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine($"\tRead {family}");
        }

        // Now execute the same query using direct SQL.
        IQueryable<Family> familyQueryInSql = client.CreateDocumentQuery<Family>(
            UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
            "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
            queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
            Console.WriteLine($"\tRead {family}");
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
    }
   ```
   
1. Kopieer en plak de volgende code aan het einde van uw `GetStartedDemo` methode na de `await CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);` regel.
   
   ```csharp
      ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
   ```
   
1. Druk op **F5** uw app uit te voeren.

De voorgaande query retourneert het volledige artikel voor de Andersen family. U hebt een query is uitgevoerd op een Azure Cosmos DB-verzameling.

Het volgende diagram illustreert hoe de Azure Cosmos DB SQL-querysyntaxis wordt aangeroepen voor de verzameling. Op de LINQ-query is dezelfde logica van toepassing.

![Diagram ter illustratie van het bereik en de betekenis van de query die wordt gebruikt in de NoSQL-zelfstudie om een toepassing C#-consoletoepassing te maken](./media/sql-api-get-started/nosql-tutorial-collection-documents.png)

De [FROM](how-to-sql-query.md#FromClause) sleutelwoord in de SQL-query is optioneel, omdat Azure Cosmos DB-query's al zijn afgestemd op één verzameling. U kunt wisselen `FROM Families f` met `FROM root r`, of een andere variabele naam die u kiest. Azure Cosmos DB die wordt afgeleid `Families`, `root`, of de naam van de variabele die u kiest verwijst naar de huidige verzameling.

## <a id="ReplaceDocument"></a>Een JSON-document bijwerken

Azure Cosmos DB SQL API biedt ondersteuning voor bijwerken en het vervangen van JSON-documenten.  

1. Kopieer en plak het volgende `ReplaceFamilyDocument` methode na de `ExecuteSimpleQuery` methode in uw code.
   
   ```csharp
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
       await client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
       WriteToConsoleAndPromptToContinue($"Replaced Family {familyName}");
    }
   ```
   
1. Kopieer en plak de volgende code aan het einde van uw `GetStartedDemo` methode na de `ExecuteSimpleQuery("FamilyDB", "FamilyCollection");` regel. De code werkt u de gegevens in een van de documenten en voert vervolgens de query opnieuw uit om weer te geven van het gewijzigde document.
   
   ```csharp
   // Update the Grade of the Andersen Family child
   andersenFamily.Children[0].Grade = 6;
   await ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "AndersenFamily", andersenFamily);
   ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
   ```
   
1. Druk op **F5** uw app uit te voeren.

De query-uitvoer ziet u dat de `Grade` voor de Andersen Family onderliggende bijgewerkt van `5` naar `6`. U hebt is bijgewerkt en een Azure Cosmos DB-document vervangen. 

## <a id="DeleteDocument"></a>Een JSON-document verwijderen

Azure Cosmos DB SQL API biedt ondersteuning voor verwijderen van JSON-documenten.  

1. Kopieer en plak het volgende `DeleteFamilyDocument` methode na de `ReplaceFamilyDocument` methode.
   
   ```csharp
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
        await client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
        Console.WriteLine($"Deleted Family {documentName}");
    }
   ```
   
1. Kopieer en plak de volgende code aan het einde van uw `GetStartedDemo` methode, na het tweede `ExecuteSimpleQuery("FamilyDB", "FamilyCollection");` regel.
   
   ```csharp
   await DeleteFamilyDocument("FamilyDB", "FamilyCollection", "AndersenFamily");
   ```
   
1. Druk op **F5** uw app uit te voeren.

U hebt een Azure Cosmos DB-document verwijderd. 

## <a id="DeleteDatabase"></a>Database verwijderen

Verwijder de database die u hebt gemaakt om deze en alle bijbehorende onderliggende resources, met inbegrip van de verzameling en documenten te verwijderen. 

1. Kopieer en plak de volgende code aan het einde van uw `GetStartedDemo` methode na de `await DeleteFamilyDocument("FamilyDB", "FamilyCollection", "AndersenFamily");` regel. 
   
   ```csharp
   // Clean up - delete the database
   await client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));
   ```
   
1. Druk op **F5** uw app uit te voeren.

U hebt de Azure Cosmos DB-database verwijderd. U kunt zien in de **Data Explorer** voor uw Azure Cosmos DB-account dat de FamilyDB-database worden verwijderd. 

## <a id="Run"></a>Voer de volledige C# console-app

Druk op **F5** in Visual Studio wilt bouwen en uitvoeren van de volledige C# console-app in de foutopsporingsmodus. Hier ziet u de volgende uitvoer in het consolevenster:

```bash
Created Family AndersenFamily
Press any key to continue ...
 Created Family WakefieldFamily
Press any key to continue ...
 Running LINQ query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Press any key to continue ...
 Replaced Family AndersenFamily
Press any key to continue ...
 Running LINQ query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
        Read {"id":"AndersenFamily","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Press any key to continue ...
 Deleted Family AndersenFamily
End of demo, press any key to exit.
```

Gefeliciteerd! U hebt de zelfstudie voltooid en beschikken over een werkende C# console-app die wordt gemaakt, query's, bijgewerkt en verwijderd van Azure Cosmos DB-resources.  

## <a name="next-steps"></a>Volgende stappen
* Zie [Welkom bij Azure Cosmos DB](introduction.md) voor meer informatie over Azure Cosmos DB.
* Zie voor een complexere ASP.NET MVC-zelfstudie [ASP.NET MVC-zelfstudie: Webtoepassingsontwikkeling met Azure Cosmos DB](sql-api-dotnet-application.md).
* Om uit te voeren op schaal en prestaties testen met Azure Cosmos DB, Zie [prestaties en schaal testen met Azure Cosmos DB](performance-testing.md).
* Zie voor meer informatie over het bewaken van Azure Cosmos DB-aanvragen, gebruik en opslag, [accounts bewaken](monitor-accounts.md).
* Voer query's uit op een voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).

