---
title: 'Azure Cosmos DB: aan de slag met de SQL-API met .NET Core | Microsoft Docs'
description: Dit is een zelfstudie waarmee u een onlinedatabase en een C#-consoletoepassing maakt met de Azure Cosmos DB SQL-API .NET Core SDK.
services: cosmos-db
author: SnehaGunda
manager: kfile
editor: ''
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 03/12/2018
ms.author: sngun
ms.custom: devcenter
ms.openlocfilehash: ca2f855166d9a36f600889059368ebeb313e789c
ms.sourcegitcommit: 542964c196a08b83dd18efe2e0cbfb21a34558aa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51636285"
---
# <a name="tutorial-build-a-net-core-app-to-manage-azure-cosmos-db-sql-api-data"></a>Zelfstudie: Een .Net Core-app bouwen om Azure Cosmos DB SQL-API-gegevens te beheren

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

In deze zelfstudie leert u hoe u een .Net Core-app maakt om Azure Cosmos DB SQL-API-gegevens te maken en er query's op uit te voeren. 

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een Azure Cosmos DB-account maken en er verbinding mee maken
> * Uw Visual Studio-oplossing configureren
> * Een online-database maken
> * Een verzameling maken
> * JSON-documenten maken
> * CRUD-bewerkingen uitvoeren op de items, container en database

Geen tijd om de toepassing te maken? Geen probleem. De volledige oplossing is beschikbaar via [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started). Spring naar de sectie [De volledige oplossing gebruiken](#GetSolution) voor beknopte instructies.

Wilt u een Xamarin iOS-, Android- of Forms-toepassing bouwen met de SQL-API en .NET Core SDK? Zie [Mobiele toepassingen maken met Xamarin en Azure Cosmos DB](mobile-apps-with-xamarin.md).

## <a name="prerequisites"></a>Vereisten

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de gratis [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Als u een Universal Windows Platform-app (UWP) ontwikkelt, gebruikt u **Visual Studio 2017 met versie 15.4** of hoger. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

    * Als u in een Mac OS- of Linux-omgeving werkt, kunt u .NET Core-apps ontwikkelen vanaf de opdrachtregel door de [.NET Core SDK](https://www.microsoft.com/net/core#macos) te installeren voor het platform van uw keuze. 

    * Als u in een Windows-omgeving werkt, kunt u .NET Core-apps ontwikkelen vanaf de opdrachtregel door de [.NET Core SDK](https://www.microsoft.com/net/core#windows) te installeren. 

    * U kunt uw eigen editor gebruiken of [Visual Studio Code](https://code.visualstudio.com/) downloaden. Dit is gratis en werkt onder Windows, Linux en MacOS. 

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Stap 1: een Azure Cosmos DB-account maken

Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt dat u wilt gebruiken, gaat u verder naar de stap [Uw Visual Studio-oplossing instellen](#SetupVS). Als u Azure Cosmos DB Emulator gebruikt, volgt u de stappen in [Azure Cosmos DB Emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Uw Visual Studio-oplossing instellen](#SetupVS) te gaan.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Stap 2: uw Visual Studio-oplossing instellen

1. Open **Visual Studio 2017** op uw computer.

2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.

3. Selecteer in het dialoogvenster **Nieuw project** achtereenvolgens **Sjablonen** / **Visual C#** / **.NET Core**/**Consoletoepassing (.NET Core)**, geef het project de naam **DocumentDBGettingStarted** en selecteer vervolgens **OK**.

   ![Schermafbeelding van het venster Nieuw project](./media/sql-api-dotnetcore-get-started/nosql-tutorial-new-project-2.png)

4. Klik in de **Solution Explorer** met de rechtermuisknop op **DocumentDBGettingStarted**.

5. Selecteer in hetzelfde menu **NuGet-pakketten beheren...**.

   ![Schermafbeelding van het rechtsklikmenu voor het project](./media/sql-api-dotnetcore-get-started/nosql-tutorial-manage-nuget-pacakges.png)

6. Selecteer in het tabblad **NuGet** **Bladeren** bovenaan in het venster en typ **azure documentdb** in het zoekvak.

7. Zoek **Microsoft.Azure.DocumentDB.Core** in de resultaten en selecteer **Installeren**.

   De pakket-id voor de bibliotheek is [Microsoft.Azure.DocumentDB.Core](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core). Als u zich richt op een .NET Framework-versie (zoals net461) die niet wordt ondersteund door dit .NET Core NuGet-pakket, gebruikt u [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB) die alle versies van .NET Framework vanaf .NET Framework 4.5 ondersteunt.

8. Accepteer de installatie van het NuGet-pakket en de licentieovereenkomst als u daar om wordt gevraagd.

Goed gedaan. Nu de installatie is voltooid, kunt u code gaan schrijven. Op [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started) vindt u een voltooid codeproject voor deze zelfstudie.

## <a id="Connect"></a>Stap 3: verbinding maken met een Azure Cosmos DB-account

Importeer de vereiste afhankelijkheden door de volgende code toe te voegen aan het begin van uw C#-toepassing, in het bestand Program.cs:

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

Voeg nu deze twee constanten en de variabele *client* toe onder de openbare klasse *Program*.

```csharp
class Program
{
    // ADD THIS PART TO YOUR CODE
    private const string EndpointUri = "<your endpoint URI>";
    private const string PrimaryKey = "<your key>";
    private DocumentClient client;
```

Ga vervolgens naar [Azure Portal](https://portal.azure.com) om uw URI en primaire sleutel op te halen. Uw toepassing heeft de Azure Cosmos DB-URI en de primaire sleutel nodig om te bepalen waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat Azure Cosmos DB de verbinding van uw toepassing vertrouwt.

Ga in Azure Portal naar uw Azure Cosmos DB-account en selecteer **Sleutels**.

Kopieer in de portal de URI en plak deze in `<your endpoint URI>` in het bestand program.cs. Kopieer vervolgens de PRIMAIRE SLEUTEL van de portal en plak deze in `<your key>`. Als u gebruikmaakt van de Azure Cosmos DB-emulator, gebruikt u `https://localhost:8081` als eindpunt. Gebruik daarnaast de goed gedefinieerde autorisatiesleutel uit [Ontwikkelen met behulp van de Azure Cosmos DB-emulator](local-emulator.md). Zorg ervoor dat u de < and > verwijdert, maar laat de dubbele aanhalingstekens rond het eindpunt en de sleutel staan.

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

Gefeliciteerd! U hebt verbinding gemaakt met een Azure Cosmos DB-account. U gaat nu aan de slag met Azure Cosmos DB-resources.  

## <a name="step-4-create-a-database"></a>Stap 4: een database maken

Voordat u de code voor het maken van een database toevoegt, moet u een Help-methode toevoegen om naar de console te kunnen schrijven.

Kopieer de methode **WriteToConsoleAndPromptToContinue** en plak deze onder de methode **GetStartedDemo**.

```csharp
// ADD THIS PART TO YOUR CODE
private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
{
        Console.WriteLine(format, args);
        Console.WriteLine("Press any key to continue ...");
        Console.ReadKey();
}
```

Uw Azure Cosmos DB-[database](databases-containers-items.md#azure-cosmos-databases) kan worden gemaakt met de methode [CreateDatabaseAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) van de klasse **DocumentClient**. Een database is een logische container voor een JSON-documentopslag, gepartitioneerd in verzamelingen.

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** onder de code voor het maken een client. Hiermee maakt u een database met de naam *FamilyDB*.

```csharp
private async Task GetStartedDemo()
{
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB_oa" });
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-database gemaakt.  

## <a id="CreateColl"></a>Stap 5: een verzameling maken

> [!WARNING]
> Met **CreateDocumentCollectionAsync** maakt u een nieuwe verzameling met gereserveerde doorvoer, wat gevolgen heeft voor de kosten. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.

U kunt een verzameling maken met de methode [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) van de klasse **DocumentClient**. Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica.

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** onder de code voor het maken een database. Met deze code maakt u een documentverzameling met de naam *FamilyCollection_oa*.

```csharp
    this.client = new DocumentClient(new Uri(EndpointUri), PrimaryKey);

    await this.client.CreateDatabaseIfNotExistsAsync("FamilyDB_oa");

    // ADD THIS PART TO YOUR CODE
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"), new DocumentCollection { Id = "FamilyCollection_oa" });
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-documentverzameling gemaakt.  

## <a id="CreateDoc"></a>Stap 6: JSON-documenten maken

U kunt een document maken met de methode [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) van de klasse **DocumentClient**. Documenten bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een of meer documenten invoegen. Als u al gegevens hebt die u in de database wilt opslaan, kunt u het [hulpprogramma voor gegevensmigratie](import-data.md) van Azure Cosmos DB gebruiken.

Eerst maakt u de klasse **Family**, die aangeeft welke objecten worden opgeslagen in Azure Cosmos DB. Daarnaast maakt u ook de subklassen **Parent**, **Child**, **Pet** en **Address**, die in de klasse **Family** worden gebruikt. De documenten moeten een **id**-eigenschap bevatten die in JSON is geserialiseerd als **id**. Maak deze klassen door de volgende interne subklassen achter de methode **GetStartedDemo** toe te voegen.

Kopieer de klassen **Family**, **Parent**, **Child**, **Pet** en **Address** en plak deze onder de methode **WriteToConsoleAndPromptToContinue**.

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

En voeg twee documenten in, één voor de Andersen Family en één voor de Wakefield Family.

Kopieer de code die volgt op `// ADD THIS PART TO YOUR CODE` en plak deze in de methode **GetStartedDemo**, onder de code voor het verzamelen van documenten.

```csharp
await this.CreateDatabaseIfNotExistsAsync("FamilyDB_oa");

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

Gefeliciteerd! U hebt twee Azure Cosmos DB-documenten gemaakt.  

![Hiërarchische relatie tussen het account, de online database en de verzameling](./media/sql-api-dotnetcore-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Stap 7: query's uitvoeren op Azure Cosmos DB-resources

Azure Cosmos DB biedt ondersteuning voor uitgebreide [query's](sql-api-sql-query.md) op de JSON-documenten die zijn opgeslagen in elke verzameling. De volgende voorbeeldcode bevat verschillende query's, waarvoor zowel gebruik wordt gemaakt van de Azure Cosmos DB SQL-syntaxis als LINQ, die u kunt uitvoeren voor de documenten die zijn ingevoegd tijdens de vorige stap.

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

Gefeliciteerd! U hebt een query uitgevoerd op een Azure Cosmos DB-verzameling.

In het volgende diagram ziet u hoe de Azure Cosmos DB SQL-querysyntaxis wordt aangeroepen voor de verzameling die u hebt gemaakt. Dezelfde logica is ook van toepassing op de LINQ-query.

![Diagram ter illustratie van het bereik en de betekenis van de query die wordt gebruikt in de NoSQL-zelfstudie om een toepassing C#-consoletoepassing te maken](./media/sql-api-dotnetcore-get-started/nosql-tutorial-collection-documents.png)

Het trefwoord [FROM](sql-api-sql-query.md#FromClause) is optioneel in de query omdat Azure Cosmos DB-query's al zijn afgestemd op één verzameling. Daarom kan FROM Families f worden ingewisseld door FROM root r, of een andere gewenste variabelenaam. Azure Cosmos DB leidt af dat 'Families', 'root', of de variabelenaam die u hebt gekozen, standaard verwijst naar de huidige verzameling.

## <a id="ReplaceDocument"></a>Stap 8: JSON-document vervangen

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

Gefeliciteerd! U hebt een Azure Cosmos DB-document vervangen.

## <a id="DeleteDocument"></a>Stap 9: JSON-document verwijderen

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

Gefeliciteerd! U hebt een Azure Cosmos DB-document verwijderd.

## <a id="DeleteDatabase"></a>Stap 10: de database verwijderen

Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (verzamelingen, documenten, enz.) verwijderd.

Kopieer en plak de volgende code in de methode **GetStartedDemo** onder de code voor het verwijderen van het document om de volledige database en alle onderliggende resources te verwijderen.

```csharp
this.ExecuteSimpleQuery("FamilyDB_oa", "FamilyCollection_oa");

await this.DeleteFamilyDocument("FamilyDB_oa", "FamilyCollection_oa", "Andersen.1");

// ADD THIS PART TO CODE
// Clean up/delete the database
await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB_oa"));
```

Druk op de knop **DocumentDBGettingStarted** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-database verwijderd.

## <a id="Run"></a>Stap 11: uw C#-consoletoepassing uitvoeren

Druk op de knop **DocumentDBGettingStarted** in Visual Studio om de toepassing te bouwen in de foutopsporingsmodus.

In een consolevenster wordt de uitvoer weergegeven van de toepassing die u voor deze zelfstudie hebt gemaakt. De uitvoer bevat de resultaten van de query's die zijn toegevoegd en moet overeenkomen met de onderstaande voorbeeldtekst.

```
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

Gefeliciteerd! U hebt de zelfstudie voltooid en beschikt nu over een werkende C#-consoletoepassing.

## <a id="GetSolution"></a> De volledige zelfstudieoplossing ophalen

Als u een GetStarted-oplossing wilt bouwen die alle voorbeelden uit dit artikel bevat, hebt u het volgende nodig:

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een [Azure Cosmos DB-account][create-sql-api-dotnet.md#create-account].
* De [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-core-getting-started)-oplossing die beschikbaar is via GitHub.

Als u de verwijzingen naar de SQL-API voor Azure Cosmos DB .NET Core SDK in Visual Studio wilt herstellen, klikt u in Solution Explorer met de rechtermuisknop op de oplossing **GetStarted** en selecteert u **Enable NuGet Package Restore** (NuGet-pakket herstellen inschakelen). Werk vervolgens in het bestand Program.cs de waarden bij voor EndpointUrl en AuthorizationKey, zoals wordt beschreven in [Verbinding maken met een Azure Cosmos DB-account](#Connect).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een .Net Core-app ontwikkelt om SQL-API-gegevens van Azure Cosmos DB te beheren. U kunt nu verdergaan met het volgende artikel:

> [!div class="nextstepaction"]
> [Een Java-console-app ontwikkelen met een Azure Cosmos DB SQL-API-account](sql-api-java-get-started.md)

[create-sql-api-dotnet.md#create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-dotnetcore-get-started/nosql-tutorial-keys.png
