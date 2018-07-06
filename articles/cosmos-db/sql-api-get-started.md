---
title: 'Azure Cosmos DB: zelfstudie Aan de slag met de SQL-API | Microsoft Docs'
description: Een zelfstudie waarmee u een online database en een C#-consoletoepassing maakt met de SQL-API.
keywords: nosql zelfstudie, onlinedatabase, c#-consoletoepassing
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 08/16/2017
ms.author: sngun
ms.openlocfilehash: 461279b5deb992be998e449f8f1ed062e91aae9b
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081435"
---
# <a name="azure-cosmos-db-sql-api-getting-started-tutorial"></a>Azure Cosmos DB: zelfstudie Aan de slag met de SQL-API
> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Node.js voor MongoDB](mongodb-samples.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Java](sql-api-java-get-started.md) 
> 

Welkom bij de zelfstudie Aan de slag met de Azure Cosmos DB SQL-API. Wanneer u deze zelfstudie hebt voltooid, beschikt u over een consoletoepassing waarmee u Azure Cosmos DB-resources kunt maken en er query's op kunt uitvoeren.

In deze zelfstudie komt het volgende aan bod:

* Een Azure Cosmos DB-account maken en er verbinding mee maken
* Uw Visual Studio-oplossing configureren
* Een online-database maken
* Een verzameling maken
* JSON-documenten maken
* Query's uitvoeren op de verzameling
* Een document vervangen
* Een document verwijderen
* De database verwijderen

Hebt u geen tijd? Geen probleem. De volledige oplossing is beschikbaar via [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). Ga naar de sectie [De volledige NoSQL-zelfstudieoplossing ophalen](#GetSolution) voor beknopte instructies.

Tijd om aan de slag te gaan.

## <a name="prerequisites"></a>Vereisten

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [!INCLUDE [cosmos-db-emulator-vs](../../includes/cosmos-db-emulator-vs.md)].

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Stap 1: een Azure Cosmos DB-account maken
Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt dat u wilt gebruiken, gaat u verder naar de stap [Uw Visual Studio-oplossing instellen](#SetupVS). Als u Azure Cosmos DB Emulator gebruikt, volgt u de stappen in [Azure Cosmos DB Emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Uw Visual Studio-oplossing instellen](#SetupVS) te gaan.

[!INCLUDE [create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupVS"></a>Stap 2: uw Visual Studio-oplossing instellen
1. Open **Visual Studio 2017** op uw computer.
2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.
3. Selecteer in het dialoogvenster **Nieuw project** achtereenvolgens **Sjablonen** / **Visual C#** / **Consoletoepassing**, geef een naam op voor uw op en klik vervolgens op **OK**.
   ![Schermopname van het venster Nieuw project](./media/sql-api-get-started/nosql-tutorial-new-project-2.png)
4. Klik in **Solution Explorer** met de rechtermuisknop op uw nieuwe consoletoepassing. Deze bevindt zich onder uw Visual Studio-oplossing. Klik vervolgens op **Manage NuGet Packages...**
    
    ![Schermopname van het rechtsklikmenu voor het project](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges.png)
5. Klik op het tabblad **NuGet** op **Bladeren** en typ **azure documentdb** in het zoekvak.
6. Zoek **Microsoft.Azure.DocumentDB** in de resultaten en klik op **Installeren**.
   De pakket-id voor de Azure Cosmos DB SQL-API-clientbibliotheek is [Microsoft Azure Cosmos DB-clientbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/).
   ![Schermopname van het NuGet-menu voor het zoeken naar de Azure Cosmos DB-client-SDK](./media/sql-api-get-started/nosql-tutorial-manage-nuget-pacakges-2.png)

    Als u een bericht ontvangt over het controleren van wijzigingen in de oplossing, klikt u op **OK**. Als u een bericht ontvangt over het accepteren van de licentie, klikt u op **Accepteren**.

Goed gedaan. De configuratie is voltooid en u kunt nu aan de slag met het schrijven van code. Op [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started/blob/master/src/Program.cs) vindt u een voltooid codeproject voor deze zelfstudie.

## <a id="Connect"></a>Stap 3: verbinding maken met een Azure Cosmos DB-account
Voeg eerst deze verwijzingen toe aan het begin van de C#-toepassing in het bestand Program.cs:

    using System;
    using System.Linq;
    using System.Threading.Tasks;

    // ADD THIS PART TO YOUR CODE
    using System.Net;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.Documents.Client;
    using Newtonsoft.Json;

> [!IMPORTANT]
> U moet de bovenstaande afhankelijkheden toevoegen om deze zelfstudie te kunnen voltooien.
> 
> 

Voeg nu deze twee constanten en de variabele *client* toe onder de openbare klasse *Program*.

    public class Program
    {
        // ADD THIS PART TO YOUR CODE
        private const string EndpointUrl = "<your endpoint URL>";
        private const string PrimaryKey = "<your primary key>";
        private DocumentClient client;

Ga vervolgens terug naar de [Azure-portal](https://portal.azure.com) om uw eindpunt-URL en primaire sleutel op te halen. Uw toepassing heeft de eindpunt-URL en de primaire sleutel nodig om te weten waarmee verbinding moet worden gemaakt en om ervoor te zorgen dat Azure Cosmos DB de verbinding van uw toepassing vertrouwt.

Ga in Azure Portal naar uw Azure Cosmos DB-account en klik op **Sleutels**.

Kopieer in de portal de URI en plak deze in `<your endpoint URL>` in het bestand program.cs. Kopieer vervolgens de PRIMAIRE SLEUTEL van de portal en plak deze in `<your primary key>`.

![Schermopname van Azure Portal die voor de NoSQL-zelfstudie wordt gebruikt om een C#-consoletoepassing te maken. Hierin wordt een Azure Cosmos DB-account weergegeven met de hub ACTIEF gemarkeerd. Verder is de knop SLEUTELS gemarkeerd op de Azure Cosmos DB-accountpagina en zijn de waarden URI, PRIMAIRE SLEUTEL en SECUNDAIRE SLEUTEL gemarkeerd op de pagina Sleutels][keys]

We starten nu de toepassing door een nieuwe instantie van de **DocumentClient** te maken.

Voeg onder de methode **Main** de nieuwe asynchrone taak met de naam **GetStartedDemo** toe, waarmee de nieuwe **DocumentClient** wordt geïnstantieerd.

    static void Main(string[] args)
    {
    }

    // ADD THIS PART TO YOUR CODE
    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);
    }

Voeg de volgende code om uw asynchrone taak met de methode **Main** uit te voeren. Uitzonderingen worden door de methode **Main** onderschept en naar de console geschreven.

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

Druk op **F5** om uw toepassing uit te voeren. In het consolevenster wordt nu het bericht `End of demo, press any key to exit.` weergegeven met de bevestiging dat de verbinding is gemaakt.  U kunt vervolgens het consolevenster sluiten. 

Gefeliciteerd. U hebt verbinding gemaakt met een Azure Cosmos DB-account. U gaat nu aan de slag met Azure Cosmos DB-resources.  

## <a name="step-4-create-a-database"></a>Stap 4: een database maken
Voordat u de code voor het maken van een database toevoegt, moet u een Help-methode toevoegen om naar de console te kunnen schrijven.

Kopieer de methode **WriteToConsoleAndPromptToContinue** en plak deze na de methode **GetStartedDemo**.

    // ADD THIS PART TO YOUR CODE
    private void WriteToConsoleAndPromptToContinue(string format, params object[] args)
    {
            Console.WriteLine(format, args);
            Console.WriteLine("Press any key to continue ...");
            Console.ReadKey();
    }

Uw Azure Cosmos DB-[database](sql-api-resources.md#databases) kan worden gemaakt met de methode [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) van de klasse **DocumentClient**. Een database is een logische container voor een JSON-documentopslag, gepartitioneerd in verzamelingen.

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** na het maken van de client. Hiermee maakt u een database met de naam *FamilyDB*.

    private async Task GetStartedDemo()
    {
        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        // ADD THIS PART TO YOUR CODE
        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

Druk op **F5** om uw toepassing uit te voeren.

Gefeliciteerd. U hebt een Azure Cosmos DB-database gemaakt.  

## <a id="CreateColl"></a>Stap 5: een verzameling maken
> [!WARNING]
> Met **CreateDocumentCollectionIfNotExistsAsync** maakt u een nieuwe verzameling met gereserveerde doorvoer, wat gevolgen heeft voor de kosten. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.
> 
> 

U kunt een [verzameling](sql-api-resources.md#collections) maken met de methode [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) van de klasse **DocumentClient**. Een verzameling is een container van JSON-documenten en de bijbehorende JavaScript-toepassingslogica.

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** na het maken van de database. Hiermee maakt u een documentverzameling met de naam *FamilyCollection*.

        this.client = new DocumentClient(new Uri(EndpointUrl), PrimaryKey);

        await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });

        // ADD THIS PART TO YOUR CODE
         await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });

Druk op **F5** om uw toepassing uit te voeren.

Gefeliciteerd. U hebt een Azure Cosmos DB-documentverzameling gemaakt.  

## <a id="CreateDoc"></a>Stap 6: JSON-documenten maken
U kunt een [document](sql-api-resources.md#documents) maken met de methode [CreateDocumentAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentasync.aspx) van de klasse **DocumentClient**. Documenten bestaan uit door gebruikers gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een of meer documenten invoegen. Als u al gegevens hebt die u in de database wilt opslaan, kunt u het [hulpprogramma voor gegevensmigratie](import-data.md) van Azure Cosmos DB gebruiken voor het importeren van gegevens in een database.

Eerst moet de klasse **Family** worden gemaakt, die aangeeft welke objecten in dit voorbeeld worden opgeslagen in Azure Cosmos DB. Daarnaast moeten de subklassen **Parent**, **Child**, **Pet** en **Address** worden gemaakt die in de klasse **Family** worden gebruikt. Houd er rekening mee dat de documenten een **id**-eigenschap moeten bevatten die in JSON is geserialiseerd als **id**. Maak deze klassen door de volgende interne subklassen achter de methode **GetStartedDemo** toe te voegen.

Kopieer de klassen **Family**, **Parent**, **Child**, **Pet** en **Address** en plak deze na de methode **WriteToConsoleAndPromptToContinue**.

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

Kopieer de methode **CreateFamilyDocumentIfNotExists** en plak deze onder de klasse **Address**.

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

En voeg twee documenten in, één voor de Andersen Family en één voor de Wakefield Family.

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** na het maken van de documentverzameling.

    await this.client.CreateDatabaseIfNotExistsAsync(new Database { Id = "FamilyDB" });
    
    await this.client.CreateDocumentCollectionIfNotExistsAsync(UriFactory.CreateDatabaseUri("FamilyDB"), new DocumentCollection { Id = "FamilyCollection" });


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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", andersenFamily);

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

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

Druk op **F5** om uw toepassing uit te voeren.

Gefeliciteerd. U hebt twee Azure Cosmos DB-documenten gemaakt.  

![Diagram waarin u de hiërarchische relatie ziet tussen het account, de online database, de verzameling en de documenten die in de NoSQL-zelfstudie worden gebruikt om een a C#-consoletoepassing te maken](./media/sql-api-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>Stap 7: query's uitvoeren op Azure Cosmos DB-resources
Azure Cosmos DB biedt ondersteuning voor uitgebreide [query's](sql-api-sql-query.md) op de JSON-documenten die zijn opgeslagen in elke verzameling.  De volgende voorbeeldcode bevat verschillende query's, waarvoor zowel gebruik wordt gemaakt van de Azure Cosmos DB SQL-syntaxis als LINQ, die u kunt uitvoeren voor de documenten die zijn ingevoegd tijdens de vorige stap.

Kopieer de methode **ExecuteSimpleQuery** en plak deze na de methode **CreateFamilyDocumentIfNotExists**.

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

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** na het maken van het tweede document.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    // ADD THIS PART TO YOUR CODE
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Druk op **F5** om uw toepassing uit te voeren.

Gefeliciteerd. U hebt een query uitgevoerd op een Azure Cosmos DB-verzameling.

In het volgende diagram ziet u hoe de Azure Cosmos DB SQL-querysyntaxis wordt aangeroepen voor de verzameling die u hebt gemaakt. Dezelfde logica is ook van toepassing op de LINQ-query.

![Diagram ter illustratie van het bereik en de betekenis van de query die wordt gebruikt in de NoSQL-zelfstudie om een toepassing C#-consoletoepassing te maken](./media/sql-api-get-started/nosql-tutorial-collection-documents.png)

Het trefwoord [FROM](sql-api-sql-query.md#FromClause) is optioneel in de query omdat Azure Cosmos DB-query's al zijn afgestemd op één verzameling. Daarom kan FROM Families f worden ingewisseld door FROM root r, of een andere gewenste variabelenaam. Azure Cosmos DB leidt af dat 'Families', 'root' of de variabelenaam die u hebt gekozen, standaard verwijst naar de huidige verzameling.

## <a id="ReplaceDocument"></a>Stap 8: JSON-document vervangen
Azure Cosmos DB biedt ondersteuning voor het vervangen van JSON-documenten.  

Kopieer de methode **ReplaceFamilyDocument** en plak deze na de methode **ExecuteSimpleQuery**.

    // ADD THIS PART TO YOUR CODE
    private async Task ReplaceFamilyDocument(string databaseName, string collectionName, string familyName, Family updatedFamily)
    {
         await this.client.ReplaceDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, familyName), updatedFamily);
         this.WriteToConsoleAndPromptToContinue("Replaced Family {0}", familyName);
    }

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** na de queryuitvoering, aan het einde van de methode. Nadat het document is vervangen, wordt dezelfde query opnieuw uitgevoerd om het gewijzigde document weer te geven.

    await this.CreateFamilyDocumentIfNotExists("FamilyDB", "FamilyCollection", wakefieldFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    // ADD THIS PART TO YOUR CODE
    // Update the Grade of the Andersen Family child
    andersenFamily.Children[0].Grade = 6;

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

Druk op **F5** om uw toepassing uit te voeren.

Gefeliciteerd. U hebt een Azure Cosmos DB-document vervangen.

## <a id="DeleteDocument"></a>Stap 9: JSON-document verwijderen
Azure Cosmos DB biedt ondersteuning voor het verwijderen van JSON-documenten.  

Kopieer en plak de methode **DeleteFamilyDocument** na de methode **ReplaceFamilyDocument**.

    // ADD THIS PART TO YOUR CODE
    private async Task DeleteFamilyDocument(string databaseName, string collectionName, string documentName)
    {
         await this.client.DeleteDocumentAsync(UriFactory.CreateDocumentUri(databaseName, collectionName, documentName));
         Console.WriteLine("Deleted Family {0}", documentName);
    }

Kopieer de volgende code en plak deze in de methode **GetStartedDemo** na de tweede queryuitvoering, aan het einde van de methode.

    await this.ReplaceFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1", andersenFamily);
    
    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");
    
    // ADD THIS PART TO CODE
    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

Druk op **F5** om uw toepassing uit te voeren.

Gefeliciteerd. U hebt een Azure Cosmos DB-document verwijderd.

## <a id="DeleteDatabase"></a>Stap 10: de database verwijderen
Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (verzamelingen, documenten, enz.) verwijderd.

Kopieer en plak de volgende code in de methode **GetStartedDemo** na het verwijderen van het document om de volledige database en alle onderliggende resources te verwijderen.

    this.ExecuteSimpleQuery("FamilyDB", "FamilyCollection");

    await this.DeleteFamilyDocument("FamilyDB", "FamilyCollection", "Andersen.1");

    // ADD THIS PART TO CODE
    // Clean up/delete the database
    await this.client.DeleteDatabaseAsync(UriFactory.CreateDatabaseUri("FamilyDB"));

Druk op **F5** om uw toepassing uit te voeren.

Gefeliciteerd. U hebt een Azure Cosmos DB-database verwijderd.

## <a id="Run"></a>Stap 11: uw C#-consoletoepassing volledig uitvoeren
Druk in Visual Studio op F5 om de toepassing in de foutopsporingsmodus op te bouwen.

In een consolevenster zou de uitvoer moeten worden weergegeven van de toepassing die we voor deze zelfstudie hebben gemaakt. De uitvoer bevat de resultaten van de query's die zijn toegevoegd en moet overeenkomen met de onderstaande voorbeeldtekst.

    Created FamilyDB
    Press any key to continue ...
    Created FamilyCollection
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

Gefeliciteerd. U hebt de zelfstudie voltooid en beschikt nu over een werkende C#-consoletoepassing.

## <a id="GetSolution"></a> De volledige zelfstudieoplossing ophalen
Als u geen tijd hebt gehad om de stappen in deze zelfstudie uit te voeren of als u alleen de codevoorbeelden wilt downloaden, kunt u deze ophalen van [GitHub](https://github.com/Azure-Samples/documentdb-dotnet-getting-started). 

Voor het bouwen van de oplossing GetStarted hebt u het volgende nodig:

* Een actief Azure-account. Als u nog geen account hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/free/).
* Een [Azure Cosmos DB-account][cosmos-db-create-account].
* De [GetStarted](https://github.com/Azure-Samples/documentdb-dotnet-getting-started)-oplossing die beschikbaar is via GitHub.

Als u de verwijzingen naar de Azure Cosmos DB .NET SDK in Visual Studio wilt herstellen, klikt u in Solution Explorer met de rechtermuisknop op de oplossing **GetStarted** en klikt u vervolgens op **Enable NuGet Package Restore** (NuGet-pakket herstellen inschakelen). Werk vervolgens in het bestand App.config de waarden bij voor EndpointUrl en AuthorizationKey, zoals wordt beschreven in [Verbinding maken met een Azure Cosmos DB-account](#Connect).

Dat is alles, bouw nu de oplossing. Succes!


## <a name="next-steps"></a>Volgende stappen
* Wilt u een complexere ASP.NET MVC-zelfstudie? Zie [Zelfstudie ASP.NET MVC: webtoepassingen ontwikkelen met Azure Cosmos DB](sql-api-dotnet-application.md).
* Wilt u de schaal en prestaties testen met Azure Cosmos DB? Zie [Performance and Scale Testing with Azure Cosmos DB (Prestaties en schaal testen met Azure Cosmos DB)](performance-testing.md)
* Meer informatie over het [controleren van aanvragen, gebruik en opslag voor Azure Cosmos DB](monitor-accounts.md).
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).
* Zie [Welkom bij Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) voor meer informatie over Azure Cosmos DB.

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
[cosmos-db-create-account]: create-sql-api-dotnet.md#create-account
