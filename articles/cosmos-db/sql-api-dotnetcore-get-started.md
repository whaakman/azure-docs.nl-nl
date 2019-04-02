---
title: 'Zelfstudie: Een .NET Core-app maken voor het beheren van gegevens die in het SQL-API-account voor Azure Cosmos DB zijn opgeslagen'
description: In deze zelfstudie maakt u een onlinedatabase en een C#-consoletoepassing met de SQL-API .NET Core SDK voor Azure Cosmos DB.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 03/12/2018
ms.author: sngun
Customer intent: As a developer, I want to build a .NET Core application to access and manage Azure Cosmos DB resources so that customers can utilize the global distribution, elastic scaling, multi-master, and other capabilities that Azure Cosmos DB offers.
ms.openlocfilehash: 81c80dbd7ffa8e5c4e7b2ebb1c9d17eb6007ae9f
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58802348"
---
# <a name="tutorial-build-a-net-core-app-to-manage-data-stored-in-a-sql-api-account"></a>Zelfstudie: een .NET Core-app maken voor het beheren van gegevens die in het SQL-API-account zijn opgeslagen

> [!div class="op_single_selector"]
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET Core (preview)](sql-api-dotnet-core-get-started-preview.md)
> * [.NET](sql-api-get-started.md)
> * [.NET (preview)](sql-api-dotnet-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Als ontwikkelaar hebt u mogelijk toepassingen die gebruikmaken van NoSQL-documentgegevens. U kunt het SQL-API-account in Azure Cosmos DB gebruiken om deze documentgegevens op te slaan en te openen. In deze zelfstudie leert u hoe u een .NET Core-app maakt om gegevens te maken en query's uit te voeren op gegevens die zijn opgeslagen in het SQL-API-account van Azure Cosmos DB. 

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een Azure Cosmos-account maken en verbinden
> * Uw Visual Studio-oplossing configureren
> * Een online-database maken
> * Een verzameling maken
> * JSON-documenten maken
> * CRUD-bewerkingen uitvoeren op de items, container en database

Geen tijd om de toepassing te maken? De volledige oplossing is beschikbaar via [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started). 

## <a name="prerequisites"></a>Vereisten

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Download en gebruik de gratis [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Als u een Universal Windows Platform-app (UWP) ontwikkelt, gebruikt u **Visual Studio 2017 met versie 15.4** of hoger. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

    * Voor een Mac OS- of Linux-omgeving kunt u .NET Core-apps ontwikkelen vanaf de opdrachtregel door de [.NET Core SDK](https://www.microsoft.com/net/core#macos) te installeren voor het platform van uw keuze. 

    * Voor een Windows-omgeving kunt u .NET Core-apps ontwikkelen vanaf de opdrachtregel door de [.NET Core SDK](https://www.microsoft.com/net/core#windows) te installeren. 

## <a name="create-an-azure-cosmos-account"></a>Een Azure Cosmos-account maken

Gebruik de volgende stappen om een Azure Cosmos-account te maken:

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Uw Visual Studio-oplossing instellen

1. Open **Visual Studio 2017** op uw computer.

2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.

3. Selecteer in het dialoogvenster **Nieuw project** achtereenvolgens **Sjablonen** > **Visual C#** > **.NET Core** > **Consoletoepassing (.NET Core)**, geef het project de naam **DocumentDBGettingStarted** en selecteer vervolgens **OK**.

   ![Schermafbeelding van het venster Nieuw project](./media/sql-api-dotnetcore-get-started/nosql-tutorial-new-project-2.png)

4. Klik in **Solution Explorer** met de rechtermuisknop op **DocumentDBGettingStarted**.

5. Selecteer in hetzelfde menu **NuGet-pakketten beheren**.

   ![Schermafbeelding van het snelmenu voor het project](./media/sql-api-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)

6. Selecteer op het tabblad **NuGet** de optie **Bladeren** boven in het venster en typ **azure documentdb** in het zoekvak. Zorg ervoor dat de **voorlopige versie opnemen** selectievakje is ingeschakeld.

7. Zoek **Microsoft.Azure.DocumentDB.Core** in de resultaten en selecteer **Installeren**.

   De pakket-id voor de bibliotheek is [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core). Als u zich richt op een .NET Framework-versie (zoals net461) die niet wordt ondersteund door dit .NET Core NuGet-pakket, gebruikt u [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB), die alle versies van .NET Framework vanaf .NET Framework 4.5 ondersteunt.

8. Accepteer de installatie van het NuGet-pakket en de licentieovereenkomst als u daar om wordt gevraagd.

Nu de installatie is voltooid, kunt u code gaan schrijven. Op [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) vindt u een voltooid codeproject voor deze zelfstudie.

## <a id="Connect"></a>Verbinding maken met een Azure Cosmos-account

Maak verbinding met een Azure Cosmos-account door de vereiste afhankelijkheden te importeren. Hiervoor voegt u de volgende code toe aan het begin van het bestand Program.cs:

```csharp
using System;

// ADD THIS PART TO YOUR CODE
using System.Linq;
using System.Threading.Tasks;
using System.Net;
using Microsoft.Azure.Documents;
using Microsoft.Azure.Documents.Client;
using Newtonsoft.Json;
```

Voeg vervolgens deze twee constanten en de variabele *client* toe onder de openbare klasse *Program*.

```csharp
class Program
{
    // ADD THIS PART TO YOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

Ga vervolgens naar [Azure Portal](https://portal.azure.com) om uw URI en primaire sleutel op te halen. Uw toepassing heeft de Azure Cosmos DB-URI en de primaire sleutel nodig om te bepalen waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat Azure Cosmos DB de verbinding van uw toepassing vertrouwt.

Ga in de Azure-portal naar uw Azure Cosmos-account en selecteer **Sleutels**.

Kopieer in de portal de URI en plak deze in `<your endpoint URI>` in het bestand program.cs. Kopieer vervolgens de PRIMAIRE SLEUTEL van de portal en plak deze in `<your key>`. Zorg ervoor dat u de < and > verwijdert, maar laat de dubbele aanhalingstekens rond het eindpunt en de sleutel staan.

![Sleutels ophalen uit Azure Portal][keys]

Voor de toepassing Getting started maken we om te beginnen een nieuw exemplaar van **DocumentClient**.

Voeg onder de methode **Main** de nieuwe asynchrone taak met de naam **GetStartedDemo** toe, waarmee de nieuwe **DocumentClient** wordt geïnstantieerd.

```csharp
static void Main(string[] args)
{
}

// ADD THIS PART TO YOUR CODE
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);
}
```

Voeg de volgende code om uw asynchrone taak met de methode **Main** uit te voeren. Uitzonderingen worden door de methode **Main** onderschept en naar de console geschreven.

```csharp
static void Main(string[] args)
{
        // ADD THIS PART TO YOUR CODE
        try
        {
                Program p = new Program();
                p.GetStartedDemo().Wait();
        }
        catch (DocumentClientException de)
        {
                Exception baseException = de.GetBaseException();
                Console.WriteLine("{0} error occurred: {1}, Message: {2}", de.StatusCode, de.Message, baseException.Message);
        }
        catch (Exception e)
        {
                Exception baseException = e.GetBaseException();
                Console.WriteLine("Error: {0}, Message: {1}", e.Message, baseException.Message);
        }
        finally
        {
                Console.WriteLine("End of demo, press any key to exit.");
                Console.ReadKey();
        }
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing te bouwen en uit te voeren.

## <a id="CreateDatabase"></a>Een database maken

Voordat u de code voor het maken van een database toevoegt, moet u een Help-methode toevoegen om naar de console te kunnen schrijven. Kopieer de methode **WriteToConsoleAndPromptToContinue** en plak deze onder de methode **GetStartedDemo**.

```csharp
// ADD THIS PART TO YOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Maak uw Azure Cosmos DB-database met de methode `CreateDatabaseAsync` van de klasse **DocumentClient**. Een database is een logische container voor een JSON-documentopslag, gepartitioneerd in verzamelingen. Kopieer de volgende code en plak deze in de methode **GetStartedDemo** onder de code voor het maken een client. Hiermee maakt u een database met de naam *FamilyDB*.

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

## <a id="CreateColl"></a>Een verzameling maken

Maak een verzameling met de methode `CreateDocumentCollectionAsync` van de klasse **DocumentClient**. Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica.

> [!WARNING]
> Met **CreateDocumentCollectionAsync** maakt u een nieuwe verzameling met gereserveerde doorvoer, wat gevolgen heeft voor de kosten. Ga naar de [prijzenpagina](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** onder de code voor het maken een database. Met deze code maakt u een documentverzameling met de naam *FamilyCollection_oa*.

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

## <a id="CreateDoc"></a>JSON-documenten maken

Maak een document met de methode `CreateDocumentAsync` van de klasse **DocumentClient**. Documenten bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een of meer documenten invoegen. 

Eerst maakt u de klasse **Family**, die aangeeft welke objecten worden opgeslagen in Azure Cosmos DB. Verder maakt u de subklassen **Parent**, **Child**, **Pet** en **Address**, die in de klasse **Family** worden gebruikt. De documenten moeten een **id**-eigenschap bevatten die in JSON is geserialiseerd als **id**. Maak deze klassen door de volgende interne subklassen achter de methode **GetStartedDemo** toe te voegen. Kopieer de klassen **Family**, **Parent**, **Child**, **Pet** en **Address** en plak deze onder de methode **WriteToConsoleAndPromptToContinue**.

```csharp
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
    Console.WriteLine(format, args);
    Console.WriteLine("Press any key to continue ...");
    Console.ReadKey();
}

// ADD THIS PART TO YOUR CODE
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

Kopieer de methode **CreateFamilyDocumentIfNotExists** en plak deze onder de methode **CreateDocumentCollectionIfNotExists**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task CreateFamilyDocumentIfNotExists(string databaseName, string collectionName, Family family)
{
    try
    {
        await this.client.ReadDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, family.Id));
        this.WriteToConsoleAndPromptToContinue("Found {0}", family.Id);
    }
    catch (DocumentClientException de)
    {
        if (de.StatusCode == HttpStatusCode.NotFound)
        {
            await this.client.CreateDocumentAsync(UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), family);
            this.WriteToConsoleAndPromptToContinue("Created Family {0}", family.Id);
        }
        else
        {
            throw;
        }
    }
}
```

En voeg twee documenten in, één voor de Andersen Family en één voor de Wakefield Family. Kopieer de code die volgt op `// ADD THIS PART TO YOUR CODE` en plak deze in de methode **GetStartedDemo**, onder de code voor het verzamelen van documenten.

```csharp
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });

await this.CreateDocumentCollectionIfNotExists("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
Family andersenFamily = new Family
{
        Id = "Andersen.1",
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

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", andersenFamily);

Family wakefieldFamily = new Family
{
        Id = "Wakefield.7",
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

await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

![Hiërarchische relatie tussen het account, de online database en de verzameling](./media/sql-api-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Query's uitvoeren op Azure Cosmos DB-resources

Azure Cosmos DB biedt ondersteuning voor uitgebreide query's voor de JSON-documenten die zijn opgeslagen in elke verzameling. De volgende voorbeeldcode bevat verschillende query's, waarvoor zowel gebruik wordt gemaakt van de Azure Cosmos DB SQL-syntaxis als LINQ, die u kunt uitvoeren voor de documenten die tijdens de vorige stap zijn ingevoegd.

Kopieer de methode **ExecuteSimpleQuery** en plak deze onder de methode **CreateFamilyDocumentIfNotExists**.

```csharp
// ADD THIS PART TO YOUR CODE
private void ExecuteSimpleQuery(string databaseName, string collectionName)
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions { MaxItemCount = -1 };

        // Here we find the Andersen family via its LastName
        IQueryable<Family> familyQuery = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName), queryOptions)
                .Where(f => f.LastName == "Andersen");

        // The query is executed synchronously here, but can also be executed asynchronously via the IDocumentQuery<T> interface
        Console.WriteLine("Running LINQ query...");
        foreach (Family family in familyQuery)
        {
            Console.WriteLine("\tRead {0}", family);
        }

        // Now execute the same query via direct SQL
        IQueryable<Family> familyQueryInSql = this.client.CreateDocumentQuery<Family>(
                UriFactory.CreateDocumentCollectionUri(databaseName, collectionName),
                "SELECT * FROM Family WHERE Family.LastName = 'Andersen'",
                queryOptions);

        Console.WriteLine("Running direct SQL query...");
        foreach (Family family in familyQueryInSql)
        {
                Console.WriteLine("\tRead {0}", family);
        }

        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** onder het tweede codefragment voor het maken van een document.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

// ADD THIS PART TO YOUR CODE
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

In het volgende diagram ziet u hoe de Azure Cosmos DB SQL-querysyntaxis wordt aangeroepen voor de verzameling u hebt gemaakt. Op de LINQ-query is dezelfde logica van toepassing.

![Diagram ter illustratie van het bereik en de betekenis van de query die wordt gebruikt in de NoSQL-zelfstudie om een toepassing C#-consoletoepassing te maken](./media/sql-api-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

Het trefwoord `FROM` is optioneel in de query omdat Azure Cosmos DB-query's al zijn afgestemd op één verzameling. Daarom kan FROM Families f worden ingewisseld door FROM root r, of een andere gewenste variabelenaam. Azure Cosmos DB leidt af dat 'Families', 'root', of de variabelenaam die u hebt gekozen, standaard verwijst naar de huidige verzameling.

## <a id="ReplaceDocument"></a>JSON-document vervangen

Azure Cosmos DB biedt ondersteuning voor het vervangen van JSON-documenten.  

Kopieer de methode **ReplaceFamilyDocument** en plak deze onder de methode **ExecuteSimpleQuery**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
{
    await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
    this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
}
```

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** onder de queryuitvoering. Nadat het document is vervangen, wordt dezelfde query opnieuw uitgevoerd om het gewijzigde document weer te geven.

```csharp
await this.CreateFamilyDocumentIfNotExists("FamilyDB_oa", "FamilyCollection_oa", wakefieldFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO YOUR CODE
// Update the Grade of the Andersen Family child
andersenFamily.Children[0].Grade = 6;

await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

## <a id="DeleteDocument"></a>JSON-document verwijderen

Azure Cosmos DB biedt ondersteuning voor het verwijderen van JSON-documenten.  

Kopieer de methode **DeleteFamilyDocument** en plak deze onder de methode **ReplaceFamilyDocument**.

```csharp
// ADD THIS PART TO YOUR CODE
private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
{
    await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
    Console.WriteLine("Deleted Family {0}", documentName);
}
```

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** onder de tweede queryuitvoering.

```csharp
await this.ReplaceFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1", andersenFamily);

this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

// ADD THIS PART TO CODE
await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

## <a id="DeleteDatabase"></a>Database verwijderen

Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (verzamelingen, documenten, enz.) verwijderd. Kopieer en plak de volgende code in de methode **GetStartedDemo** onder het document om de volledige database en alle onderliggende resources te verwijderen.

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART TO CODE
// Clean up/delete the database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

## <a id="Run"></a>De C#-consoletoepassing uitvoeren

Druk op de knop **DocumentDBGettingStarted** in Visual Studio om de toepassing te bouwen in de foutopsporingsmodus. In een consolevenster wordt de uitvoer weergegeven van de toepassing die u voor deze zelfstudie hebt gemaakt. De uitvoer bevat de resultaten van de query's die zijn toegevoegd en moet overeenkomen met de onderstaande voorbeeldtekst.

```bash
Created FamilyDB_oa
Press any key to continue ...
Created FamilyCollection_oa
Press any key to continue ...
Created Family Andersen.1
Press any key to continue ...
Created Family Wakefield.7
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Replaced Family Andersen.1
Press any key to continue ...
Running LINQ query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Running direct SQL query...
    Read {"id":"Andersen.1","LastName":"Andersen","District":"WA5","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":6,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":true}
Deleted Family Andersen.1
End of demo, press any key to exit.
```

U hebt de zelfstudie voltooid en beschikt nu over een werkende C#-consoletoepassing.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de resourcegroep, het Azure Cosmos-account en alle gerelateerde resources verwijderen wanneer u ze niet meer nodig hebt. Daarvoor selecteert u de resourcegroep voor de virtuele machine en selecteert u **Verwijderen**. Vervolgens bevestigt u de naam van de resourcegroep die u wilt verwijderen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een .NET Core-app maakt om gegevens te beheren die zijn opgeslagen in het SQL-API-account van Azure Cosmos DB. U kunt nu verdergaan met het volgende artikel:

> [!div class="nextstepaction"]
> [Een Java-console-app ontwikkelen met het SQL-API-account van Azure Cosmos DB](sql-api-java-get-started.md)

[create-sql-api-dotnet.md#create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-dotnetcore-get-started/nosql-tutorial-keys.png
