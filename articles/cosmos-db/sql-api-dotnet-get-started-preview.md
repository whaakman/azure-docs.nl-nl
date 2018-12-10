---
title: Een .NET-console-app bouwen om gegevens te beheren in het Azure Cosmos DB SQL-API-account (SDK versie 3, preview)
description: Een zelfstudie waarmee u een online database en een C#-consoletoepassing maakt met de SQL-API.
keywords: nosql zelfstudie, onlinedatabase, c#-consoletoepassing
author: deborahc
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 12/01/2018
ms.author: dech
ms.openlocfilehash: 4ac770b58e4a4d9a547916997a8f9d181b2fa895
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52852567"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account-sdk-version-3-preview"></a>Een .NET-console-app bouwen om gegevens te beheren in het Azure Cosmos DB SQL-API-account (SDK versie 3, preview)

> [!div class="op_single_selector"]
> * [.NET (preview)](sql-api-dotnet-get-started-preview.md)
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET Core (preview)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Welkom bij de zelfstudie Aan de slag met de Azure Cosmos DB SQL-API. Wanneer u deze zelfstudie hebt voltooid, beschikt u over een consoletoepassing waarmee u Azure Cosmos DB-resources kunt maken en er query's op kunt uitvoeren. In deze zelfstudie wordt gebruikgemaakt van [versie 3.0+](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) van de Azure Cosmos DB .NET SDK. Deze is gericht op [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard).

In deze zelfstudie komt het volgende aan bod:

> [!div class="checklist"]
> * Een Azure Cosmos-account maken en verbinden
> * Uw project configureren in Visual Studio
> * Een database en een container maken
> * Items toevoegen aan de container
> * Een query uitvoeren op de container
> * CRUD-bewerkingen uitvoeren op het item
> * De database verwijderen

Hebt u geen tijd? Geen probleem. De volledige oplossing is beschikbaar via [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started). Ga naar de sectie [De volledige zelfstudieoplossing ophalen](#GetSolution) voor beknopte instructies.

Tijd om aan de slag te gaan.

## <a name="prerequisites"></a>Vereisten

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)]

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Stap 1: een Azure Cosmos DB-account maken
Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt dat u wilt gebruiken, gaat u verder naar de stap [Uw Visual Studio-oplossing instellen](#SetupVS). Als u Azure Cosmos DB Emulator gebruikt, volgt u de stappen in [Azure Cosmos DB Emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Uw Visual Studio-project instellen](#SetupVS) te gaan.

[!INCLUDE [create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount-preview.md)]

## <a id="SetupVS"></a>Stap 2: uw Visual Studio-project instellen
1. Open **Visual Studio 2017** op uw computer.
1. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.
1. Selecteer in het dialoogvenster **Nieuw project** achtereenvolgens **Visual C#** / **Consoletoepassing (.NET Framework)** geef een naam op voor uw op en klik vervolgens op **OK**.
    ![Schermopname van het venster Nieuw project](./media/sql-api-get-started/dotnet-tutorial-visual-studio-new-project.png)
1. Klik in **Solution Explorer** met de rechtermuisknop op uw nieuwe consoletoepassing. Deze bevindt zich onder uw Visual Studio-oplossing. Klik vervolgens op **Manage NuGet Packages...**
    
    ![Schermopname van het rechtsklikmenu voor het project](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget.png)
1. Klik op het tabblad **NuGet** op **Bladeren** en typ **Microsoft.Azure.Cosmos** in het zoekvak. Schakel *Prelease opnemen* in om de preview-versie te kunnen vinden.
1. Zoek **Microsoft.Azure.Cosmos** in de resultaten en klik op **Installeren**.
   De pakket-id voor de Azure Cosmos DB SQL-API-clientbibliotheek is [Microsoft Azure Cosmos DB-clientbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/).
   ![Schermopname van het NuGet-menu voor het zoeken naar de Azure Cosmos DB-client-SDK](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

    Als u een bericht ontvangt over het controleren van wijzigingen in de oplossing, klikt u op **OK**. Als u een bericht ontvangt over het accepteren van de licentie, klikt u op **Accepteren**.

Goed gedaan. De configuratie is voltooid en u kunt nu aan de slag met het schrijven van code. Op [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started) vindt u een voltooid codeproject voor deze zelfstudie.

## <a id="Connect"></a>Stap 3: verbinding maken met een Azure Cosmos DB-account
1. Vervang eerst de verwijzingen aan het begin van de C#-toepassing in het bestand **Program.cs** door de volgende verwijzingen:
    ```csharp
    using System;
    using System.Threading.Tasks;
    using System.Configuration;
    using Microsoft.Azure.Cosmos;
    using System.Collections.Generic;
    using System.Net;
    ```
1. Voeg nu deze constanten en variabelen toe in de openbare klasse ``Program``.
    ```csharp
    public class Program
    {
        // ADD THIS PART TO YOUR CODE

        // The Azure Cosmos DB endpoint for running this sample.
        private static readonly string EndpointUri = "<your endpoint here>";
        // The primary key for the Azure Cosmos account.
        private static readonly string PrimaryKey = "<your primary key>";

        // The Cosmos client instance
        private CosmosClient cosmosClient;

        // The database we will create
        private CosmosDatabase database;

        // The container we will create.
        private CosmosContainer container;

        // The name of the database and container we will create
        private string databaseId = "FamilyDatabase";
        private string containerId = "FamilyContainer";
    }
    ```
    Als u bekend bent met de vorige versie van de .NET-SDK, komen de termen 'verzameling' en 'document' u misschien vertrouwd voor. Azure Cosmos DB biedt ondersteuning voor meerdere API-modellen. Daarom wordt in versie 3.0+ van de .NET-SDK gebruikgemaakt van de generieke termen 'container' en 'item'. Een container kan een verzameling, een graaf of een tabel zijn. Een item kan een document, rand/hoekpunt of rij zijn en is de inhoud binnen een container. [Meer informatie over databases, containers en items.](databases-containers-items.md)

1. Haal uw eindpunt-URL en primaire sleutel op uit de [Azure-portal](https://portal.azure.com).

    Ga in Azure Portal naar uw Azure Cosmos DB-account en klik op **Sleutels**.

    Kopieer de URI uit de portal en plak deze in `<your endpoint URL>` in het bestand ```Program.cs```. Kopieer de PRIMAIRE SLEUTEL uit de portal en plak deze in `<your primary key>`.

   ![Schermopname van het ophalen van de Azure Cosmos DB-sleutels uit de Azure-portal](./media/sql-api-get-started/dotnet-tutorial-portal-keys.png)

1. Vervolgens maakt u een nieuw exemplaar van ```CosmosClient``` en stelt u een basis voor het programma in.

    Voeg onder de methode **Main** de nieuwe asynchrone taak met de naam **GetStartedDemoAsync** toe, waarmee de nieuwe ```CosmosClient``` wordt geïnstantieerd. U gebruikt **GetStartedDemoAsync** als het toegangspunt dat de methoden aanroept die gebruikmaken van Azure Cosmos DB-resources.

    ```csharp
    public static async Task Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    /*
        Entry point to call methods that operate on Azure Cosmos DB resources in this sample
    */   
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
    }
    ```

1. Voeg de volgende code toe om de asynchrone taak **GetStartedDemoAsync** uit te voeren vanuit de methode **Main**. Uitzonderingen worden door de methode **Main** onderschept en naar de console geschreven.
    ```csharp
    public static async Task Main(string[] args)
    {
        // ADD THIS PART TO YOUR CODE
        try
        {
            Console.WriteLine("Beginning operations...\n");
            Program p = new Program();
            await p.GetStartedDemoAsync();
        }
        catch (CosmosException de)
        {
            Exception baseException = de.GetBaseException();
            Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
        }
        catch (Exception e)
        {
            Console.WriteLine("Error: {0}\n", e);
        }
        finally
        {
            Console.WriteLine("End of demo, press any key to exit.");
            Console.ReadKey();
        }
    }
    ```

1. Selecteer **F5** om de toepassing uit te voeren. In het consolevenster wordt het bericht `End of demo, press any key to exit.` weergegeven waarin wordt bevestigd dat de verbinding met Azure Cosmos DB is gemaakt. U kunt vervolgens het consolevenster sluiten. 

Gefeliciteerd! U hebt nu verbinding gemaakt met een Azure Cosmos DB-account. 

## <a name="step-4-create-a-database"></a>Stap 4: een database maken
Een database kan worden gemaakt met de functie [**CreateDatabaseIfNotExistsAsync**](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.cosmos.cosmosdatabases) of [**CreateDatabaseAsync**](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.cosmos.cosmosdatabases) van de klasse ``CosmosDatabases``. Een database is de logische container voor items die zijn gepartitioneerd in containers.
    
1. Kopieer de methode **CreateDatabase** en plak deze onder de methode **GetStartedDemoAsync**. Met **CreateDatabase** wordt een nieuwe database gemaakt met id ``FamilyDatabase``, als deze nog niet bestaat, met de id die is opgegeven vanuit het veld ``databaseId``. 

    ```csharp
    /*
        Create the database if it does not exist
    */    
    private async Task CreateDatabase()
    {
        // Create a new database
        this.database = await this.cosmosClient.Databases.CreateDatabaseIfNotExistsAsync(databaseId);
        Console.WriteLine("Created Database: {0}\n", this.database.Id);
    }
    ```

1. Kopieer de onderstaande code en plak deze waar u de CosmosClient hebt geïnstantieerd om de methode **CreateDatabase** aan te roepen die u zojuist hebt toegevoegd.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabase(); 
    }
    ```

    Op dit moment moet de code er als volgt uitzien, met uw eindpunt en primaire sleutel ingevuld.
    ```csharp
    using System;
    using System.Threading.Tasks;
    using System.Configuration;
    using Microsoft.Azure.Cosmos;
    using System.Collections.Generic;
    using System.Net;

    namespace CosmosGettingStarted
    {
        class Program
        {
            // The Azure Cosmos DB endpoint for running this sample.
            private static readonly string EndpointUri = "<your endpoint here>";
            // The primary key for the Azure Cosmos account.
            private static readonly string PrimaryKey = "<your primary key>";

            // The Cosmos client instance
            private CosmosClient cosmosClient;
        
            // The database we will create
            private CosmosDatabase database;

            // The container we will create.
            private CosmosContainer container;

            // The name of the database and container we will create
            private string databaseId = "FamilyDatabase";
            private string containerId = "FamilyContainer";

            public static async Task Main(string[] args)
            {
                try
                {
                    Console.WriteLine("Beginning operations...");
                    Program p = new Program();
                    await p.GetStartedDemoAsync();
                }
                catch (CosmosException de)
                {
                    Exception baseException = de.GetBaseException();
                    Console.WriteLine("{0} error occurred: {1}\n", de.StatusCode, de);
                }
                catch (Exception e)
                {
                    Console.WriteLine("Error: {0}\n", e);
                }
                finally
                {
                    Console.WriteLine("End of demo, press any key to exit.");
                    Console.ReadKey();
                }
            }

            /*
                Entry point to call methods that operate on Azure Cosmos DB resources in this sample
            */
            public async Task GetStartedDemoAsync()
            {
                // Create a new instance of the Cosmos Client
                this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
                await this.CreateDatabase();
            }

            /*
                Create the database if it does not exist
            */    
            private async Task CreateDatabase()
            {
                // Create a new database
                this.database = await this.cosmosClient.Databases.CreateDatabaseIfNotExistsAsync(databaseId);
                Console.WriteLine("Created Database: {0}\n", this.database.Id);
            }
        }
    }
    ```

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-database gemaakt.  

## <a id="CreateColl"></a>Stap 5: een container maken
> [!WARNING]
> Door de methode **CreateContainerIfNotExistsAsync** aan te roepen, wordt er een nieuwe container gemaakt. Hiervoor worden kosten in rekening gebracht. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.
> 
> 

Een container kan worden gemaakt met behulp van de functie [**CreateContainerIfNotExistsAsync**](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.cosmos.cosmoscontainers) of [**CreateContainerAsync**](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.cosmos.cosmoscontainers) in de klasse **CosmosContainers**. Een container bestaat uit items - in het geval van de SQL-API zijn dit JSON-documenten - en bijbehorende JavaScript-toepassingslogica, zoals opgeslagen procedures, door de gebruiker gedefinieerde functies, en triggers.

1. Kopieer de methode **CreateContainer** en plak deze onder de methode **CreateDatabase**. Met **CreateContainer** wordt een nieuwe container gemaakt met id ``FamilyContainer``, als deze nog niet bestaat, met de id die is opgegeven vanuit het veld ``containerId``. 

    ```csharp
    /*
        Create the container if it does not exist. 
        Specifiy "/LastName" as the partition key since we're storing family information, to ensure good distribution of requests and storage.
    */
    private async Task CreateContainer()
    {
        // Create a new container
        this.container = await this.database.Containers.CreateContainerIfNotExistsAsync(containerId, "/LastName");
        Console.WriteLine("Created Container: {0}\n", this.container.Id);
    }
    ```

1. Kopieer de onderstaande code en plak deze waar u de CosmosClient hebt geïnstantieerd om de methode **CreateContainer** aan te roepen die u zojuist hebt toegevoegd.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 

        //ADD THIS PART TO YOUR CODE
        await this.CreateContainer();
    }

Select **F5** to run your application.

Congratulations! You have successfully created an Azure Cosmos DB container.  

## <a id="CreateDoc"></a>Step 6: Add items to the container
An item can be created by using the [**CreateItemAsync**](https://docs.microsoft.com/en-us/dotnet/api/microsoft.azure.cosmos.cosmositems) function of the **CosmosItems** class. When using the SQL API, items are projected as documents, which are user-defined (arbitrary) JSON content. You can now insert an item into your Azure Cosmos DB container.

First, we need to create a **Family** class that will represent objects stored within Azure Cosmos DB in this sample. We will also create **Parent**, **Child**, **Pet**, **Address** subclasses that are used within **Family**. Note that documents must have an **Id** property serialized as **id** in JSON. 
1. Select **Ctrl+Shift+A** to open the **Add New Item** dialog. Add a new class **Family.cs** to your project. 

    ![Screen shot of adding a new Family.cs class into the project](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1. Copy and paste the **Family**, **Parent**, **Child**, **Pet**, and **Address** class into **Family.cs**. 
    ```csharp
    using Newtonsoft.Json;

    namespace CosmosGettingStarted
    {
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
    }
    ```
1. Ga terug naar **Program.cs** en voeg de methode **AddItemsToContainer** toe onder de methode **CreateContainer**. Voordat het item wordt gemaakt, controleert de code of er niet al een item met dezelfde id bestaat. U voegt twee items in, één voor de familie Andersen en één voor de familie Wakefield.

    ```csharp
    /*
        Add Family items to the container
    */
    private async Task AddItemsToContainer()
    {
        // Create a family object for the Andersen family
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

        // Read the item to see if it exists. Note ReadItemAsync will not throw an exception if an item does not exist. Instead, we check the StatusCode property off the response object. 
        CosmosItemResponse<Family> andersenFamilyResponse = await this.container.Items.ReadItemAsync<Family>(andersenFamily.LastName, andersenFamily.Id);

        if (andersenFamilyResponse.StatusCode == HttpStatusCode.NotFound)
        {
            // Create an item in the container representing the Andersen family. Note we provide the value of the partition key for this item, which is "Andersen"
            andersenFamilyResponse = await this.container.Items.CreateItemAsync<Family>(andersenFamily.LastName, andersenFamily);

            // Note that after creating the item, we can access the body of the item with the Resource property off the CosmosItemResponse. 
            //We can also access the RequestCharge property to see the amount of RUs consumed on this request.
            Console.WriteLine("Created item in database with id: {0} Operation consumed {1} RUs.\n", andersenFamilyResponse.Resource.Id, andersenFamilyResponse.RequestCharge);
        }
        else
        {
            Console.WriteLine("Item in database with id: {0} already exists\n", andersenFamilyResponse.Resource.Id);
        }

        // Create a family object for the Wakefield family
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

        // Read the item to see if it exists
        CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.ReadItemAsync<Family>(wakefieldFamily.LastName, wakefieldFamily.Id);

        if (wakefieldFamilyResponse.StatusCode == HttpStatusCode.NotFound)
        {
            // Create an item in the container representing the Wakefield family. Note we provide the value of the partition key for this item, which is "Wakefield"
            wakefieldFamilyResponse = await this.container.Items.CreateItemAsync<Family>(wakefieldFamily.LastName, wakefieldFamily);

            // Note that after creating the item, we can access the body of the item with the Resource property off the CosmosItemResponse. 
            //We can also access the RequestCharge property to see the amount of RUs consumed on this request.
            Console.WriteLine("Created item in database with id: {0} Operation consumed {1} RUs.\n", wakefieldFamilyResponse.Resource.Id, wakefieldFamilyResponse.RequestCharge);
        }
        else
        {
            Console.WriteLine("Item in database with id: {0} already exists\n", wakefieldFamilyResponse.Resource.Id);
        }
    }
    ```
1. Voeg een aanroep naar ``AddItemsToContainer`` toe in de methode ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();

        //ADD THIS PART TO YOUR CODE
        await this.AddItemsToContainer();
    }
    ```

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt twee Azure Cosmos DB-items gemaakt.  


## <a id="Query"></a>Stap 7: query's uitvoeren op Azure Cosmos DB-resources
Azure Cosmos DB biedt ondersteuning voor uitgebreide [query's](sql-api-sql-query.md) op de JSON-documenten die zijn opgeslagen in elke verzameling. In de volgende voorbeeldcode ziet u hoe u een query uitvoert voor de items die u in de vorige stap hebt ingevoegd.

1. Kopieer de methode **RunQuery** en plak deze onder de methode **AddItemsToContainer**.

    ```csharp
    /*
        Run a query (using Azure Cosmos DB SQL syntax) against the container
    */
    private async Task RunQuery()
    {
        var sqlQueryText = "SELECT * FROM c WHERE c.LastName = 'Andersen'";
        var partitionKeyValue = "Andersen";

        Console.WriteLine("Running query: {0}\n", sqlQueryText);

        CosmosSqlQueryDefinition queryDefinition = new CosmosSqlQueryDefinition(sqlQueryText);
        CosmosResultSetIterator<Family> queryResultSetIterator = this.container.Items.CreateItemQuery<Family>(queryDefinition, partitionKeyValue);

        List<Family> families = new List<Family>();

        while (queryResultSetIterator.HasMoreResults)
        {
            CosmosQueryResponse<Family> currentResultSet = await queryResultSetIterator.FetchNextSetAsync();
            foreach (Family family in currentResultSet)
            {
                families.Add(family);
                Console.WriteLine("\tRead {0}\n", family);
            }
        }
    }
    ```
1. Voeg een aanroep naar ``RunQuery`` toe in de methode ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();

        //ADD THIS PART TO YOUR CODE
        await this.RunQuery();
    }
    ```

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een query uitgevoerd op een Azure Cosmos DB-container.

## <a id="ReplaceItem"></a>Stap 8: een JSON-item vervangen
Nu gaat u een item in Azure Cosmos DB bijwerken.

1. Kopieer de methode **ReplaceFamilyItem** en plak deze onder de methode **RunQuery**. Houd er rekening mee dat u de eigenschap ``IsRegistered`` van de familie en de ``Grade`` van een van de kinderen wijzigt.
    ```csharp
    /*
    Update an item in the container
    */
    private async Task ReplaceFamilyItem()
    {
        CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.ReadItemAsync<Family>("Wakefield", "Wakefield.7");
        var itemBody = wakefieldFamilyResponse.Resource;
        
        // update registration status from false to true
        itemBody.IsRegistered = true;
        // update grade of child
        itemBody.Children[0].Grade = 6;

        // replace the item with the updated content
        wakefieldFamilyResponse = await this.container.Items.ReplaceItemAsync<Family>(itemBody.LastName, itemBody.Id, itemBody);
        Console.WriteLine("Updated Family [{0},{1}]\n. Body is now: {2}\n", itemBody.LastName, itemBody.Id, wakefieldFamilyResponse.Resource);
    }
    ```
1. Voeg een aanroep naar ``ReplaceFamilyItem`` toe in de methode ``GetStartedDemo``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();
        await this.RunQuery();

        //ADD THIS PART TO YOUR CODE
        await this.ReplaceFamilyItem();
    }

Select **F5** to run your application.

Congratulations! You have successfully replaced an Azure Cosmos DB item.

## <a id="DeleteDocument"></a>Step 9: Delete item
Now, we will delete an item in Azure Cosmos DB.

1. Copy and paste the **DeleteFamilyItem** method below your **ReplaceFamilyItem** method.
    ```csharp
    /*
    Delete an item in the container
    */
    private async Task DeleteFamilyItem()
    {
        var partitionKeyValue = "Wakefield";
        var familyId = "Wakefield.7";

        // Delete an item. Note we must provide the partition key value and id of the item to delete
        CosmosItemResponse<Family> wakefieldFamilyResponse = await this.container.Items.DeleteItemAsync<Family>(partitionKeyValue, familyId);
        Console.WriteLine("Deleted Family [{0},{1}]\n", partitionKeyValue, familyId);
    }
    ```
1. Voeg een aanroep naar ``DeleteFamilyItem`` toe in de methode ``GetStartedDemo``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();
        await this.RunQuery();
        await this.ReplaceFamilyItem();

        //ADD THIS PART TO YOUR CODE
        await this.DeleteFamilyItem();
    }
    ```

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-item verwijderd.

## <a id="DeleteDatabase"></a>Stap 10: de database verwijderen
Nu gaat u de database verwijderen. Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (containers, items en eventuele opgeslagen procedures, door de gebruiker gedefinieerde functies en triggers) verwijderd. U gaat ook het **CosmosClient**-instantie verwijderen.

1. Kopieer de methode **DeleteDatabaseAndCleanup** en plak deze onder de methode **DeleteFamilyItem**.
    ```csharp
    /*
    Delete the database and dispose of the Cosmos Client instance
    */
    private async Task DeleteDatabaseAndCleanup()
    {
        CosmosDatabaseResponse databaseResourceResponse = await this.database.DeleteAsync();
        // Also valid: await this.cosmosClient.Databases["FamilyDatabase"].DeleteAsync();

        Console.WriteLine("Deleted Database: {0}\n", this.databaseId);

        //Dispose of CosmosClient
        this.cosmosClient.Dispose();
    }
    ```
1. Voeg een aanroep naar ``DeleteDatabaseAndCleanup`` toe in de methode ``GetStartedDemo``.
    
    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabase(); 
        await this.CreateContainer();
        await this.AddItemsToContainer();
        await this.RunQuery();
        await this.ReplaceFamilyItem();
        await this.DeleteFamilyItem();

        //ADD THIS PART TO YOUR CODE        
        await this.DeleteDatabaseAndCleanup();
    }
    ```

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-database verwijderd.

## <a id="Run"></a>Stap 11: uw C#-consoletoepassing volledig uitvoeren
Druk in Visual Studio op F5 en compileer en voer de toepassing uit in de foutopsporingsmodus.

Als het goed is, wordt de uitvoer van de volledige app in een consolevenster weergegeven. De uitvoer bevat de resultaten van de query's die zijn toegevoegd en moet overeenkomen met de onderstaande voorbeeldtekst.

```
Beginning operations...

Created Database: FamilyDatabase

Created Container: FamilyContainer

Created item in database with id: Andersen.1 Operation consumed 11.43 RUs.

Created item in database with id: Wakefield.7 Operation consumed 14.29 RUs.

Running query: SELECT * FROM c WHERE c.LastName = 'Andersen'

        Read {"id":"Andersen.1","LastName":"Andersen","Parents":[{"FamilyName":null,"FirstName":"Thomas"},{"FamilyName":null,"FirstName":"Mary Kay"}],"Children":[{"FamilyName":null,"FirstName":"Henriette Thaulow","Gender":"female","Grade":5,"Pets":[{"GivenName":"Fluffy"}]}],"Address":{"State":"WA","County":"King","City":"Seattle"},"IsRegistered":false}

Updated Family [Wakefield,Wakefield.7].
        Body is now: {"id":"Wakefield.7","LastName":"Wakefield","Parents":[{"FamilyName":"Wakefield","FirstName":"Robin"},{"FamilyName":"Miller","FirstName":"Ben"}],"Children":[{"FamilyName":"Merriam","FirstName":"Jesse","Gender":"female","Grade":6,"Pets":[{"GivenName":"Goofy"},{"GivenName":"Shadow"}]},{"FamilyName":"Miller","FirstName":"Lisa","Gender":"female","Grade":1,"Pets":null}],"Address":{"State":"NY","County":"Manhattan","City":"NY"},"IsRegistered":true}

Deleted Family [Wakefield,Wakefield.7]

Deleted Database: FamilyDatabase

End of demo, press any key to exit.
```

Gefeliciteerd! U hebt de zelfstudie voltooid en beschikt nu over een werkende C#-consoletoepassing.

## <a id="GetSolution"></a> De volledige zelfstudieoplossing ophalen
Als u geen tijd hebt gehad om de stappen in deze zelfstudie uit te voeren of als u alleen de codevoorbeelden wilt downloaden, kunt u deze ophalen van [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started). 

Voor het bouwen van de oplossing GetStarted hebt u het volgende nodig:

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een [Azure Cosmos DB-account][cosmos-db-create-account].
* De [GetStarted](https://github.com/Azure-Samples/cosmos-dotnet-getting-started)-oplossing die beschikbaar is via GitHub.

Als u de verwijzingen naar de Azure Cosmos DB .NET-SDK in Visual Studio wilt herstellen, klikt u in Solution Explorer met de rechtermuisknop op de oplossing **GetStarted** en klikt u vervolgens op **Restore NuGet Packages**. Werk vervolgens in het bestand App.config de waarden bij voor EndPointUri en PrimaryKey, zoals wordt beschreven in [Verbinding maken met een Azure Cosmos DB-account](#Connect).

Dat is alles, bouw nu de oplossing. Succes!


## <a name="next-steps"></a>Volgende stappen
* Wilt u een complexere ASP.NET MVC-zelfstudie? Zie [Zelfstudie ASP.NET MVC: webtoepassingen ontwikkelen met Azure Cosmos DB](sql-api-dotnet-application-preview.md).
* Wilt u de schaal en prestaties testen met Azure Cosmos DB? Zie [Performance and Scale Testing with Azure Cosmos DB (Prestaties en schaal testen met Azure Cosmos DB)](performance-testing.md)
* Meer informatie over het [controleren van aanvragen, gebruik en opslag voor Azure Cosmos DB](monitor-accounts.md).
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).
* Zie [Welkom bij Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) voor meer informatie over Azure Cosmos DB.

[cosmos-db-create-account]: create-sql-api-dotnet-preview.md#create-account
