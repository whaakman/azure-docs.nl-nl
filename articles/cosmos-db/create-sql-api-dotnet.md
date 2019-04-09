---
title: Een .NET-web-app met Azure Cosmos DB ontwikkelen met de SQL API
description: In deze snelstartgids gebruikt u de Azure-portal en een .NET-web-app maken en beheren van resources voor het account in Azure Cosmos DB SQL API.
author: SnehaGunda
ms.author: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 04/05/2019
ms.openlocfilehash: 7ecb2269243ae96b629a20a26956e6220a2e616c
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280839"
---
# <a name="quickstart-build-a-net-web-app-using-sql-api-account-in-azure-cosmos-db"></a>Quickstart: Een .NET-web-app met behulp van de SQL API-account in Azure Cosmos DB maken

> [!div class="op_single_selector"]
> * [.NET](create-sql-api-dotnet.md)
> * [.NET (Preview)](create-sql-api-dotnet-preview.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)
>  
> 

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. Azure Cosmos DB kunt u snel maken en het opvragen van sleutel/waarde-databases, documentdatabases en graph-databases, die allemaal het voordeel van de wereldwijde distributie en horizontale schaalmogelijkheden die ten grondslag van Azure Cosmos DB. 

In deze Quick Start ziet u hoe u de Azure portal gebruiken voor het maken van een Azure Cosmos DB [SQL API](sql-api-introduction.md) account, een documentdatabase en verzameling maakt en gegevens toevoegen aan de verzameling. Vervolgens gebruikt u een [SQL .NET SDK](sql-api-sdk-dotnet.md) web-app voor meer gegevens toevoegen aan de verzameling. 

In deze snelstartgids hebt u Data Explorer in Azure portal gebruiken voor het maken van de database en verzameling. U kunt ook de database en verzameling maken met behulp van de .NET-voorbeeldcode. Zie voor meer informatie, [bekijkt u de .NET-code](#review-the-net-code). 

## <a name="prerequisites"></a>Vereisten

Visual Studio 2017 met de Azure-ontwikkeling-werkstroom geïnstalleerd
- U kunt downloaden en gebruiken de **gratis** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/). Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio. 

Een Azure-abonnement of een gratis proefversie Azure Cosmos DB-account
- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

<a id="create-account"></a>
## <a name="create-an-azure-cosmos-db-account"></a>Maak een Azure Cosmos DB-account

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection-database"></a>
## <a name="add-a-database-and-a-collection"></a>Een database en een verzameling toevoegen 

U kunt Data Explorer in Azure portal om te maken van een database en verzameling. 

1.  Selecteer **Data Explorer** pagina in de linkernavigatiebalk op uw Azure Cosmos DB-account en selecteer vervolgens **nieuwe verzameling**. 
    
    U moet mogelijk naar rechts scrollen om te zien de **verzameling toevoegen** gebied.
    
    ![Azure Portal Data Explorer, deelvenster Verzameling toevoegen](./media/create-sql-api-dotnet/azure-cosmosdb-data-explorer-dotnet.png)
    
1.  Geef op de pagina **Verzameling toevoegen** de instellingen voor de nieuwe verzameling op.
    
    |Instelling|Voorgestelde waarde|Description
    |---|---|---|
    |**Database-id**|Takenlijst|Voer *ToDoList* als de naam voor de nieuwe database. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen `/, \\, #, ?` bevatten en mogen niet eindigen met een spatie.|
    |**Verzamelings-id**|Items|Voer *Items* in als de naam voor de nieuwe verzameling. Verzameling-ID's hebben dezelfde tekenvereisten als voor databasenamen.|
    |**Partitiesleutel**| /category| In het voorbeeld dat wordt beschreven in dit artikel wordt *Category* als de partitiesleutel.|
    |**Doorvoer**|400|Laat de doorvoer in 400 aanvraageenheden per seconde (RU/s). U kunt de doorvoer later opschalen als u de latentie wilt beperken.| 
    
    Voeg niet **unieke sleutels** voor dit voorbeeld. Unieke sleutels kunnen u een laag van de integriteit van gegevens toevoegen aan de database door ervoor te zorgen dat een of meer waarden per partitiesleutel. Zie voor meer informatie, [unieke sleutels in Azure Cosmos DB](unique-keys.md).
    
1.  Selecteer **OK**. 
    In Data Explorer worden de nieuwe database en verzameling weergegeven.
    
    ![Data Explorer in Azure Portal, met de nieuwe database en tabel](./media/create-sql-api-dotnet/azure-cosmos-db-new-collection.png)

## <a name="add-data-to-your-database"></a>Gegevens toevoegen aan uw database

Gegevens toevoegen aan uw nieuwe database met behulp van Data Explorer.

1. In **Data Explorer**, de nieuwe database wordt weergegeven in de **verzamelingen** deelvenster. Vouw de **ToDoList** database, vouw de **Items** verzameling, selecteer **documenten**, en selecteer vervolgens **Nieuw Document**. 
   
   ![Nieuwe documenten maken in Data Explorer in de Azure Portal](./media/create-sql-api-dotnet/azure-cosmosdb-new-document.png)
   
1. De volgende structuur toevoegen aan het document aan de rechterkant van de **documenten** deelvenster:

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

1. Selecteer **Opslaan**.
   
   ![In de json-gegevens kopiëren en selecteer opslaan in Data Explorer in Azure portal](./media/create-sql-api-dotnet/azure-cosmosdb-save-document.png)
   
1. Selecteer **Nieuw Document** opnieuw, en maken en een ander document opslaan met een unieke `id`, en alle andere eigenschappen en waarden die u wilt. Uw documenten kunnen elke gewenste structuur hebben, omdat Azure Cosmos DB een schema voor uw gegevens niet opleggen.

## <a name="query-your-data"></a>Uw gegevens opvragen

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="use-the-net-web-app-to-manage-data"></a>Gebruik de .NET-web-app om gegevens te beheren

Als u wilt zien hoe eenvoudig het is om te werken met uw Azure Cosmos DB-gegevens via een programma, kloon de voorbeeld-web-app voor SQL-API .NET vanuit GitHub, de verbindingsreeks bijwerken en de app voor het bijwerken van uw gegevens uitvoeren. 

U kunt ook de database en verzameling maken met behulp van de .NET-voorbeeldcode. Zie voor meer informatie, [bekijkt u de .NET-code](#review-the-net-code).

### <a name="clone-the-sample-app"></a>De voorbeeld-app klonen

Kloon eerst een C# [SQL API-app](https://github.com/Azure-Samples/documentdb-dotnet-todo-app) vanuit GitHub. 

1. Open een git-terminalvenster zoals Git Bash, maak een nieuwe map met de naam *git-samples*, en wijzig in het: 
   
   ```bash
   mkdir /c/git-samples/
   cd /c/git-samples/
   ```
   
1. Voer de volgende opdracht om de voorbeeldopslagplaats te klonen en maken van een kopie van de voorbeeld-app op uw computer:
   
   ```bash
   git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
   ```

### <a name="update-the-connection-string"></a>De verbindingsreeks bijwerken 

1. Navigeer naar en open de *todo.sln* -bestand van uw gekloonde app in Visual Studio. 

1. In Visual Studio **Solution Explorer**, open de *web.config* bestand. 

1. Ga terug naar de Azure portal om te kopiëren van de verbindingsreeksinformatie te plakken in de *web.config*.
   
   1. Selecteer in uw Azure Cosmos DB-account linkernavigatievenster **sleutels**.
      
      ![Een toegangssleutel bekijken en kopiëren in Azure Portal, blade Sleutels](./media/create-sql-api-dotnet/keys.png)
      
   1. Onder **sleutels voor lezen / schrijven**, Kopieer de **URI** waarde met de kopieerknop aan de rechterkant en plak deze in de `endpoint` sleutel in de *web.config*. Bijvoorbeeld: 
      
      `<add key="endpoint" value="https://mysqlapicosmosdb.documents.azure.com:443/" />`
      
   1. Kopieer de **primaire sleutel** waarde en plak deze in de `authKey` sleutel in de *web.config*. Bijvoorbeeld:
      
      `<add key="authKey" value="19ZDNJAiYL26tmnRvoez6hmtIfBGwjun50PWRjNYMC2ig8Ob9hYk7Fq1RYSv8FcIYnh1TdBISvCh7s6yyb0000==" />`

       
1. Zorg ervoor dat de database en verzameling waarden in de *web.config* overeenkomen met de namen die u eerder hebt gemaakt. 

   ```csharp
   <add key="database" value="ToDoList"/>
   <add key="collection" value="Items"/>
   ```
 
1. Sla de *web.config.* U hebt nu uw app bijgewerkt met alle gegevens die nodig zijn om te communiceren met Azure Cosmos DB.

### <a name="run-the-web-app"></a>De web-app uitvoeren

1. In Visual Studio met de rechtermuisknop op de **todo** project **Solution Explorer**, en selecteer vervolgens **NuGet-pakketten beheren**. 

1. Typ in het NuGet-vak **Bladeren** *DocumentDB*.

1. Installeer vanuit de resultaten de **Microsoft.Azure.DocumentDB** bibliotheek als nog niet is geïnstalleerd. Hiermee installeert u de [Microsoft.Azure.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) pakket en alle afhankelijkheden.
   
   Als de NuGet Package Manager een bericht weergegeven wordt dat sommige pakketten niet aanwezig in de oplossing zijn, selecteert u **herstellen** ze te installeren uit interne bronnen. 

1. Selecteer **Ctrl**+**F5** voor het uitvoeren van de app in uw browser. 

1. Selecteer **nieuw** in de to-do-app en maak een paar nieuwe taken.

   ![Taken-app met voorbeeldgegevens](./media/create-sql-api-dotnet/azure-comosdb-todo-app-list.png)

U kunt teruggaan naar Data Explorer in de Azure portal om te zien, query, wijzigen en werken met uw nieuwe gegevens. 

## <a name="review-the-net-code"></a>De .NET-code bekijken

Deze stap is optioneel. In deze quickstart maakt u een database en een verzameling hebt gemaakt in Azure portal en voorbeeldgegevens toegevoegd met behulp van de .NET-voorbeeld. U kunt de database en de verzameling ook maken met behulp van de .NET-voorbeeld. De volgende codefragmenten bekijken als u geïnteresseerd bent in hoe de database-resources worden gemaakt in de code. De codefragmenten zijn allemaal afkomstig uit de *DocumentDBRepository.cs* -bestand in de **todo** project.

* Deze code initialiseert de `DocumentClient`: 

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
    ```

* Deze code wordt de nieuwe database gemaakt met behulp van de `CreateDatabaseAsync` methode:

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* De volgende code wordt de nieuwe verzameling gemaakt met behulp van de `CreateDocumentCollectionAsync` methode:

    ```csharp
    private static async Task CreateCollectionIfNotExistsAsync()
    {
        try
        {
           await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(DatabaseId, CollectionId));
        }
        catch (DocumentClientException e)
        {
           if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
           {
              await client.CreateDocumentCollectionAsync(
              UriFactory.CreateDatabaseUri(DatabaseId),
              new DocumentCollection
              {
                  Id = CollectionId
              },
              new RequestOptions { OfferThroughput = 400 });
           }
           else
           {
             throw;
           }
        }
    }
    ```

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u een Azure Cosmos DB-account maken, het maken van een database en verzameling met de Data Explorer en het uitvoeren van een .NET-web-app voor het bijwerken van uw gegevens. Nu kunt u aanvullende gegevens in uw Azure Cosmos DB-account importeren. 

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB](import-data.md)

