---
title: Een .NET-consoletoepassing voor het beheren van gegevens in Azure Cosmos DB SQL API-account maken
description: Informatie over het maken van Azure Cosmos DB SQL API-resources met behulp van een C# consoletoepassing.
author: kirankumarkolli
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 07/09/2019
ms.author: kirankk
ms.openlocfilehash: 6fd7efe38aeb1f1094d240cf1675d432f3766229
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67985727"
---
# <a name="build-a-net-console-app-to-manage-data-in-azure-cosmos-db-sql-api-account"></a>Een .NET-consoletoepassing voor het beheren van gegevens in Azure Cosmos DB SQL API-account maken

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
>

Welkom bij de Azure Cosmos DB SQL API-get-handleiding. Wanneer u deze zelfstudie hebt voltooid, beschikt u over een consoletoepassing waarmee u Azure Cosmos DB-resources kunt maken en er query's op kunt uitvoeren. Deze zelfstudie wordt gebruikgemaakt van [versie 3.0 +](https://www.nuget.org/packages/Microsoft.Azure.Cosmos) van de Azure Cosmos DB .NET-SDK, die kunnen worden gericht op [.NET Framework](https://dotnet.microsoft.com/download) of [.NET Core](https://dotnet.microsoft.com/download).

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

## <a name="step-1-create-an-azure-cosmos-db-account"></a>Stap 1: Maak een Azure Cosmos DB-account
Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt dat u wilt gebruiken, gaat u verder naar de stap [Uw Visual Studio-oplossing instellen](#SetupVS). Als u Azure Cosmos DB Emulator gebruikt, volgt u de stappen in [Azure Cosmos DB Emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Uw Visual Studio-project instellen](#SetupVS) te gaan.

[!INCLUDE [create-dbaccount-preview](../../includes/cosmos-db-create-dbaccount-preview.md)]

## <a id="SetupVS"></a>Stap 2: Uw Visual Studio-project instellen
1. Open **Visual Studio 2017** op uw computer.
1. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.
1. Selecteer in het dialoogvenster **Nieuw project** achtereenvolgens **Visual C#**  / **Consoletoepassing (.NET Framework)** geef een naam op voor uw op en klik vervolgens op **OK**.
    ![Schermopname van het venster Nieuw Project](./media/sql-api-get-started/dotnet-tutorial-visual-studio-new-project.png)

    > [!NOTE]
    > Voor .NET core-doel, in de **nieuw Project** dialoogvenster, selecteer **Visual C#**   /  **Console-App (.NET Core)** , Geef uw project de naam en klik vervolgens op  **OK**

1. Klik in **Solution Explorer** met de rechtermuisknop op uw nieuwe consoletoepassing. Deze bevindt zich onder uw Visual Studio-oplossing. Klik vervolgens op **Manage NuGet Packages...**

    ![Schermafbeelding van het rechtsklikmenu voor het project](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget.png)
1. Klik op het tabblad **NuGet** op **Browse** en typ **Microsoft.Azure.Cosmos** in het zoekvak.
1. Zoek **Microsoft.Azure.Cosmos** in de resultaten en klik op **Install**.
   De pakket-id voor de Azure Cosmos DB SQL-API-clientbibliotheek is [Microsoft Azure Cosmos DB-clientbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/).
   ![Schermopname van het NuGet-Menu voor het vinden van Azure Cosmos DB-Client-SDK](./media/sql-api-get-started/dotnet-tutorial-visual-studio-manage-nuget-2.png)

    Als u een bericht ontvangt over het controleren van wijzigingen in de oplossing, klikt u op **OK**. Als u een bericht ontvangt over het accepteren van de licentie, klikt u op **Accepteren**.

Goed gedaan. De configuratie is voltooid en u kunt nu aan de slag met het schrijven van code. Op [GitHub](https://github.com/Azure-Samples/cosmos-dotnet-getting-started) vindt u een voltooid codeproject voor deze zelfstudie.

## <a id="Connect"></a>Stap 3: Verbinding maken met een Azure Cosmos DB-account
1. Vervang eerst de verwijzingen aan het begin van de C#-toepassing in het bestand **Program.cs** door de volgende verwijzingen:

   ```csharp
   using System;
   using System.Threading.Tasks;
   using System.Configuration;
   using System.Collections.Generic;
   using System.Net;
   using Microsoft.Azure.Cosmos;
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
        private Database database;

        // The container we will create.
        private Container container;

        // The name of the database and container we will create
        private string databaseId = "FamilyDatabase";
        private string containerId = "FamilyContainer";
    }
    ```

    Als u bekend bent met de vorige versie van de .NET-SDK, komen de termen 'verzameling' en 'document' u misschien vertrouwd voor. Azure Cosmos DB biedt ondersteuning voor meerdere API-modellen. Daarom wordt in versie 3.0+ van .NET SDK gebruikgemaakt van de generieke termen 'container' en 'item'. Een container kan een verzameling, een graaf of een tabel zijn. Een item kan een document, rand/hoekpunt of rij zijn en is de inhoud binnen een container. [Meer informatie over databases, containers en items.](databases-containers-items.md)

1. Haal uw eindpunt-URL en primaire sleutel op uit de [Azure-portal](https://portal.azure.com).

    Ga in Azure Portal naar uw Azure Cosmos DB-account en klik op **Sleutels**.

    Kopieer de URI uit de portal en plak deze in `<your endpoint URL>` in het bestand ```Program.cs```. Kopieer de PRIMAIRE SLEUTEL uit de portal en plak deze in `<your primary key>`.

   ![Schermafbeelding van Azure Cosmos DB-sleutels ophalen uit Azure portal](./media/sql-api-get-started/dotnet-tutorial-portal-keys.png)

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

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=Main)]

1. Selecteer **F5** om de toepassing uit te voeren. In het consolevenster wordt het bericht `End of demo, press any key to exit.` weergegeven waarin wordt bevestigd dat de verbinding met Azure Cosmos DB is gemaakt. U kunt vervolgens het consolevenster sluiten.

Gefeliciteerd! U hebt nu verbinding gemaakt met een Azure Cosmos DB-account. 

## <a name="step-4-create-a-database"></a>Stap 4: Een database maken
Een database kan worden gemaakt met de functie [**CreateDatabaseIfNotExistsAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) of [**CreateDatabaseAsync**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclient) van de klasse ``CosmosClient``. Een database is de logische container voor items die zijn gepartitioneerd in containers.

1. Kopieer en plak de **CreateDatabaseAsync** methode hieronder uw **GetStartedDemoAsync** methode. **CreateDatabaseAsync** wordt een nieuwe database maken met de ID ``FamilyDatabase`` als deze nog niet bestaat, met de ID die is opgegeven in de ``databaseId`` veld. 

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateDatabaseAsync&highlight=7)]

1. Kopieer en plak de onderstaande code waar u de CosmosClient om aan te roepen geïnstantieerd de **CreateDatabaseAsync** methode die u zojuist hebt toegevoegd.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);

        //ADD THIS PART TO YOUR CODE
        await this.CreateDatabaseAsync();
    }
    ```

    Op dit moment moet de code er als volgt uitzien, met uw eindpunt en primaire sleutel ingevuld.

    ```csharp
    using System;
    using System.Threading.Tasks;
    using System.Configuration;
    using System.Collections.Generic;
    using System.Net;
    using Microsoft.Azure.Cosmos;

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
            private Database database;

            // The container we will create.
            private Container container;

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

            /// <summary>
            /// Entry point to call methods that operate on Azure Cosmos DB resources in this sample
            /// </summary>
            public async Task GetStartedDemoAsync()
            {
                // Create a new instance of the Cosmos Client
                this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
                await this.CreateDatabaseAsync();
            }

            /// <summary>
            /// Create the database if it does not exist
            /// </summary>
            private async Task CreateDatabaseAsync()
            {
                // Create a new database
                this.database = await this.cosmosClient.CreateDatabaseIfNotExistsAsync(databaseId);
                Console.WriteLine("Created Database: {0}\n", this.database.Id);
            }
        }
    }
    ```

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-database gemaakt.  

## <a id="CreateColl"></a>Stap 5: Een container maken
> [!WARNING]
> Door de methode **CreateContainerIfNotExistsAsync** aan te roepen, wordt er een nieuwe container gemaakt. Hiervoor worden kosten in rekening gebracht. Zie onze [pagina met prijzen](https://azure.microsoft.com/pricing/details/cosmos-db/) voor meer informatie.
>
>

Een container kan worden gemaakt met behulp van de [ **CreateContainerIfNotExistsAsync** ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosdatabase) of [ **CreateContainerAsync** ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosdatabase) werken in de **CosmosDatabase** klasse. Een container bestaat uit items (JSON-documenten als SQL-API) en serverzijde toepassingslogica in JavaScript, bijvoorbeeld, opgeslagen procedures, door gebruiker gedefinieerde functies en triggers die zijn gekoppeld.

1. Kopieer en plak de **CreateContainerAsync** methode hieronder uw **CreateDatabaseAsync** methode. **CreateContainerAsync** wordt een nieuwe container maken met de ID ``FamilyContainer`` als deze nog niet bestaat, met de ID die is opgegeven in de ``containerId`` veld gepartitioneerd op basis van ``LastName`` eigenschap.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=CreateContainerAsync&highlight=9)]

1. Kopieer de onderstaande code en plak deze waar u de CosmosClient hebt geïnstantieerd om de methode **CreateContainer** aan te roepen die u zojuist hebt toegevoegd.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();

        //ADD THIS PART TO YOUR CODE
        await this.CreateContainerAsync();
    }
    ```

   Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt nu een Azure Cosmos DB-container gemaakt.  

## <a id="CreateDoc"></a>Stap 6: Items toevoegen aan de container
Een item kan worden gemaakt met behulp van de [ **CreateItemAsync** ](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmoscontainer) functie van de **CosmosContainer** klasse. Als u de SQL-API gebruikt, worden items als documenten geprojecteerd. Deze zijn door de gebruiker gedefinieerde (willekeurige) JSON-inhoud. U kunt nu een item invoegen in uw Azure Cosmos DB-container.

Eerst gaan we maken een **familie** klasse die aangeeft welke objecten die zijn opgeslagen in Azure Cosmos DB in dit voorbeeld worden. Daarnaast moeten de subklassen **Parent**, **Child**, **Pet** en **Address** worden gemaakt die in de klasse **Family** worden gebruikt. Notitie-item moet hebben een **Id** eigenschap geserialiseerd als **id** in JSON.

1. Selecteer **Ctrl+Shift+A** om het dialoogvenster **Add New Item** te openen. Voeg een nieuwe klasse **Family.cs** toe aan uw project.

    ![Schermafbeelding van het toevoegen van een nieuwe Family.cs-klasse in het project](./media/sql-api-get-started/dotnet-tutorial-visual-studio-add-family-class.png)

1. Kopieer de klassen **Family**, **Parent**, **Child**, **Pet** en **Address** en plak deze in **Family.cs**.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Family.cs)]

1. Ga terug naar **Program.cs** en voeg de **AddItemsToContainerAsync** methode onder uw **CreateContainerAsync** methode.
De code controleert of dat een item met dezelfde ID bestaat al niet voordat u deze maakt. U voegt twee items in, één voor de familie Andersen en één voor de familie Wakefield.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=AddItemsToContainerAsync)]

1. Voeg een aanroep naar ``AddItemsToContainerAsync`` toe in de methode ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();

        //ADD THIS PART TO YOUR CODE
        await this.AddItemsToContainerAsync();
    }
    ```

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt twee Azure Cosmos DB-items gemaakt.  

## <a id="Query"></a>Stap 7: Query's uitvoeren op Azure Cosmos DB-resources
Azure Cosmos DB biedt ondersteuning voor uitgebreide [query's](sql-api-sql-query.md) op de JSON-documenten die zijn opgeslagen in elke verzameling. In de volgende voorbeeldcode ziet u hoe u een query uitvoert voor de items die u in de vorige stap hebt ingevoegd.

1. Kopieer en plak de **QueryItemsAsync** methode hieronder uw **AddItemsToContainerAsync** methode.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=QueryItemsAsync&highlight=10-11,17-18)]

1. Voeg een aanroep naar ``QueryItemsAsync`` toe in de methode ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();

        //ADD THIS PART TO YOUR CODE
        await this.QueryItemsAsync();
    }
    ```

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een query uitgevoerd op een Azure Cosmos DB-container.

## <a id="ReplaceItem"></a>Stap 8: Een JSON-item vervangen
Nu gaat u een item in Azure Cosmos DB bijwerken.

1. Kopieer en plak de **ReplaceFamilyItemAsync** methode hieronder uw **QueryItemsAsync** methode. Houd er rekening mee dat u de eigenschap ``IsRegistered`` van de familie en de ``Grade`` van een van de kinderen wijzigt.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=ReplaceFamilyItemAsync&highlight=15)]

1. Voeg een aanroep naar ``ReplaceFamilyItemAsync`` toe in de methode ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();
        await this.QueryItemsAsync();

        //ADD THIS PART TO YOUR CODE
        await this.ReplaceFamilyItemAsync();
    }
    ```

   Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-item vervangen.

## <a id="DeleteDocument"></a>Stap 9: Item verwijderen
Nu gaat u een item verwijderen uit Azure Cosmos DB.

1. Kopieer en plak de **DeleteFamilyItemAsync** methode hieronder uw **ReplaceFamilyItemAsync** methode.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteFamilyItemAsync&highlight=10)]

1. Voeg een aanroep naar ``DeleteFamilyItemAsync`` toe in de methode ``GetStartedDemoAsync``.

    ```csharp
    public async Task GetStartedDemoAsync()
    {
        // Create a new instance of the Cosmos Client
        this.cosmosClient = new CosmosClient(EndpointUri, PrimaryKey);
        await this.CreateDatabaseAsync();
        await this.CreateContainerAsync();
        await this.AddItemsToContainerAsync();
        await this.QueryItemsAsync();
        await this.ReplaceFamilyItemAsync();

        //ADD THIS PART TO YOUR CODE
        await this.DeleteFamilyItemAsync();
    }
    ```

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-item verwijderd.

## <a id="DeleteDatabase"></a>Stap 10: De database verwijderen
Nu gaat u de database verwijderen. Als u de gemaakte database verwijdert, worden de database en alle onderliggende resources (containers, items en eventuele opgeslagen procedures, door de gebruiker gedefinieerde functies en triggers) verwijderd. U gaat ook het **CosmosClient**-instantie verwijderen.

1. Kopieer en plak de **DeleteDatabaseAndCleanupAsync** methode hieronder uw **DeleteFamilyItemAsync** methode.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=DeleteDatabaseAndCleanupAsync)]

1. Voeg een aanroep naar ``DeleteDatabaseAndCleanupAsync`` toe in de methode ``GetStartedDemoAsync``.

    [!code-csharp[](~/cosmos-dotnet-getting-started/CosmosGettingStartedTutorial/Program.cs?name=GetStartedDemoAsync&highlight=14)]

Selecteer **F5** om de toepassing uit te voeren.

Gefeliciteerd! U hebt een Azure Cosmos DB-database verwijderd.

## <a id="Run"></a>Stap 11: Uw C#-consoletoepassing volledig uitvoeren
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
* Wilt u een complexere ASP.NET MVC-zelfstudie? Zie [ASP.NET MVC-zelfstudie: Webtoepassingsontwikkeling met Azure Cosmos DB](sql-api-dotnet-application-preview.md).
* Wilt u de schaal en prestaties testen met Azure Cosmos DB? Zie [Performance and Scale Testing with Azure Cosmos DB (Prestaties en schaal testen met Azure Cosmos DB)](performance-testing.md)
* Meer informatie over het [controleren van aanvragen, gebruik en opslag voor Azure Cosmos DB](monitor-accounts.md).
* Voer query's uit op onze voorbeeldgegevensset in de [Queryspeelplaats](https://www.documentdb.com/sql/demo).
* Zie [Welkom bij Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction) voor meer informatie over Azure Cosmos DB.

[cosmos-db-create-account]: create-sql-api-java.md#create-a-database-account
