---
title: 'Azure Cosmos DB: een web-app ontwikkelen met .NET en de SQL API | Microsoft Docs'
description: "Biedt een voorbeeld van .NET-code die u kunt gebruiken om verbinding te maken met de SQL API van Azure Cosmos DB en er query’s op uit te voeren"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 
ms.service: cosmos-db
ms.custom: quick start connect, mvc, devcenter
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 12/15/2017
ms.author: mimig
ms.openlocfilehash: 9541fa7331a5a6a5a5405244dd79eb8a92d96386
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="azure-cosmos-db-build-a-sql-api-web-app-with-net-and-the-azure-portal"></a>Azure Cosmos DB: een SQL API-web-app ontwikkelen met .NET en de Azure Portal

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)] 

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze Quick Start laat zien hoe u een Azure Cosmos DB-account, een documentdatabase en een verzameling kunt maken met behulp van Azure Portal. U moet vervolgens een Takenlijst-web-app maken en implementeren op de [SQL .NET API](sql-api-sdk-dotnet.md), zoals wordt weergegeven in de volgende schermopname. 

![Taken-app met voorbeeldgegevens](./media/create-sql-api-dotnet/azure-comosdb-todo-app-list.png)

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<a id="create-collection"></a>
## <a name="add-a-collection"></a>Een verzameling toevoegen

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Voorbeeldgegevens toevoegen

U kunt nu gegevens aan uw nieuwe verzameling toevoegen met behulp van Data Explorer.

1. De nieuwe database wordt in Data Explorer weergegeven in het deelvenster Verzamelingen. Vouw de database **Taken** uit, vouw de verzameling **Items** uit, klik op **Documenten** en klik vervolgens op **Nieuwe documenten**. 

   ![Nieuwe documenten maken in Data Explorer in de Azure Portal](./media/create-sql-api-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
2. Voeg nu een document toe aan de verzameling met de volgende structuur.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

3. Zodra u de JSON hebt toegevoegd aan het tabblad **Documenten** klikt u op **Opslaan**.

    ![JSON-gegevens kopiëren en op Opslaan klikken in Data Explorer in Azure Portal](./media/create-sql-api-dotnet/azure-cosmosdb-data-explorer-save-document.png)

4.  Maak nog één document en sla dit op. In het document voegt u een unieke waarde toe voor de eigenschap `id`. Wijzig de andere eigenschappen naar eigen inzicht. De nieuwe documenten kunnen elke gewenste structuur hebben, omdat in Azure Cosmos DB uw gegevens geen schema krijgen opgelegd.

     U kunt nu query's in Data Explorer gebruiken om uw gegevens te halen. Data Explorer maakt standaard gebruik van `SELECT * FROM c` om alle documenten in de verzameling op te halen. U kunt dit wijzigen in een andere [SQL-query](sql-api-sql-query.md), zoals `SELECT * FROM c ORDER BY c._ts DESC`, om alle documenten terug te zetten in aflopende volgorde op basis van hun timestamp.
 
     U kunt Data Explorer ook gebruiken voor het maken van opgeslagen procedures, UDF's en triggers om bedrijfslogica aan de serverzijde uit te voeren en doorvoer te schalen. In Data Explorer wordt alle ingebouwde programmatische gegevenstoegang zichtbaar die beschikbaar is in de API's, maar biedt eenvoudige toegang tot uw gegevens in Azure Portal.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

Nu gaan we werken met code. We gaan nu een SQL API-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een venster in een git-terminal zoals git bash en `CD` naar een werkmap.  

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/documentdb-dotnet-todo-app.git
    ```

3. Open vervolgens het todo-oplossingenbestand in Visual Studio. 

## <a name="review-the-code"></a>De code bekijken

Laten we eens kijken wat er precies gebeurt in de app. Open het bestand DocumentDBRepository.cs en u zult zien dat deze regels code de Azure Cosmos DB-resources maken. 

* De DocumentClient wordt geïnitialiseerd op regel 78.

    ```csharp
    client = new DocumentClient(new Uri(ConfigurationManager.AppSettings["endpoint"]), ConfigurationManager.AppSettings["authKey"]);
    ```

* Er wordt een nieuwe database gemaakt op regel 93.

    ```csharp
    await client.CreateDatabaseAsync(new Database { Id = DatabaseId });
    ```

* Er wordt een nieuwe verzameling gemaakt op regel 112.

    ```csharp
    await client.CreateDocumentCollectionAsync(
        UriFactory.CreateDatabaseUri(DatabaseId),
        new DocumentCollection { Id = CollectionId },
        new DocumentCollection
            {
               Id = CollectionId,
               PartitionKey = new PartitionKeyDefinition() { Paths = new Collection<string>() { "/category" } }
            },
        new RequestOptions { OfferThroughput = 1000 });
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app.

1. Klik in [Azure Portal](http://portal.azure.com/), in uw Azure Cosmos DB-account, in het linker navigatiegedeelte op **Sleutels** en klik vervolgens op **Sleutels voor lezen/schrijven**. In de volgende stap gebruikt u de kopieerknoppen aan de rechterkant van het scherm om de URI en primaire sleutel in het bestand web.config te kopiëren.

    ![Een toegangssleutel bekijken en kopiëren in Azure Portal, blade Sleutels](./media/create-sql-api-dotnet/keys.png)

2. Open in Visual Studio 2017 het bestand web.config. 

3. Kopieer uw URI-waarde vanaf de portal (met de kopieerknop) en geef deze als waarde aan de eindpuntsleutel in web.config. 

    `<add key="endpoint" value="FILLME" />`

4. Kopieer vervolgens de waarde van uw PRIMAIRE SLEUTEL vanaf de portal en geef deze als authKey-waarde in web.config. U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

    `<add key="authKey" value="FILLME" />`
    
## <a name="run-the-web-app"></a>De web-app uitvoeren
1. Klik in Visual Studio met de rechtermuisknop op het project in **Solution Explorer** en klik vervolgens op **NuGet-pakketten beheren**. 

2. Typ in het NuGet-vak **Bladeren** *DocumentDB*.

3. Installeer vanuit de resultaten de bibliotheek **Microsoft.Azure.DocumentDB**. Hiermee installeert u het pakket Microsoft.Azure.DocumentDB, evenals alle afhankelijkheden.

4. Klik op Ctrl+F5 om de toepassing uit te voeren. Uw app wordt in uw browser weergegeven. 

5. Klik op **Nieuwe maken** in de browser en maak een paar nieuwe taken in uw taken-app.

   ![Taken-app met voorbeeldgegevens](./media/create-sql-api-dotnet/azure-comosdb-todo-app-list.png)

U kunt nu teruggaan naar Data Explorer en deze nieuwe gegevens bekijken, wijzigen, een query erop uitvoeren of er iets anders mee doen. 

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze Quick Start zijn aangemaakt door onderstaande stappen te volgen in Azure Portal:

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hoe u een verzameling kunt maken met Data Explorer en hebt u een web-app uitgevoerd. Nu kunt u aanvullende gegevens in uw Cosmos DB-account importeren. 

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB](import-data.md)


