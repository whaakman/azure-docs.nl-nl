---
title: Een Azure Cosmos DB .NET-toepassing ontwikkelen met behulp van de Table-API | Microsoft-documenten
description: Aan de slag met de Table-API van Azure Cosmos DB met behulp van .NET
services: cosmosdb
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmosdb
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: cba0b278d84e25876a8b73cedb7e35f84500fc5e
ms.contentlocale: nl-nl
ms.lasthandoff: 05/11/2017


---
# <a name="azure-cosmos-db-build-a-net-application-using-the-table-api"></a>Azure Cosmos DB: een .NET-toepassing ontwikkelen met de Table-API

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze Quick Start laat zien hoe u een Azure Cosmos DB-account en een tabel binnen dat account kunt maken met behulp van de Azure Portal. Vervolgens schrijft u code om entiteiten in te voegen, bij te werken en te verwijderen, en voert u enkele query’s uit. U kunt de [Azure Storage Preview SDK](https://aka.ms/premiumtablenuget) downloaden van NuGet. Deze heeft dezelfde klassen en handtekeningen voor methodes als de openbare [Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), maar biedt ook de mogelijkheid verbinding te maken met Azure Cosmos DB-accounts met behulp van de [Table-API](table-introduction.md) (preview). 

## <a name="prerequisites"></a>Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmosdb-create-dbaccount-table](../../includes/cosmosdb-create-dbaccount-table.md)]

## <a name="add-a-table"></a>Een tabel toevoegen

[!INCLUDE [cosmosdb-create-table](../../includes/cosmosdb-create-table.md)]

## <a name="add-sample-data"></a>Voorbeeldgegevens toevoegen

U kunt nu gegevens aan uw nieuwe tabel toevoegen met behulp van Data Explorer.

1. Vouw in Data Explorer **sample-database**, **sample-table** uit, klik op **Entiteiten** en klik vervolgens op **Entiteit toevoegen**.
2. Voeg nu gegevens toe aan de vakken met een waarde voor PartitionKey en RowKey en klik op **Entiteit toevoegen**.

   ![Nieuwe documenten maken in Data Explorer in de Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
  
    U kunt nu meer entiteiten toevoegen aan uw tabel, uw entiteiten bewerken of een query op uw gegevens uitvoeren in Data Explorer. Data Explorer is ook de plek waar u uw doorvoer kunt schalen en opgeslagen procedures, door de gebruiker gedefinieerde functies en triggers aan uw tabel kunt toevoegen.

## <a name="clone-the-sample-application"></a>De voorbeeldtoepassing klonen

We gaan nu een DocumentDB API-app klonen vanaf github, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een venster in een git-terminal zoals git bash en `cd` naar een werkmap.  

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Open vervolgens het oplossingenbestand in Visual Studio. 

## <a name="review-the-code"></a>De code bekijken

Laten we eens kijken wat er precies gebeurt in de app. Open het bestand Program.cs en u zult zien dat deze regels code de Azure Cosmos DB-resources maken. 

* De DocumentClient wordt geïnitialiseerd.

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* Er wordt een nieuwe tabel gemaakt als er nog geen tabel bestaat.

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* Er wordt een nieuwe Table-container gemaakt. Deze code is vergelijkbaar met de reguliere code van de Azure Table Storage-SDK 

    ```csharp
    CustomerEntity item = new CustomerEntity()
                {
                    PartitionKey = Guid.NewGuid().ToString(),
                    RowKey = Guid.NewGuid().ToString(),
                    Email = $"{GetRandomString(6)}@contoso.com",
                    PhoneNumber = "425-555-0102",
                    Bio = GetRandomString(1000)
                };
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app.

1. Klik in [Azure Portal](http://portal.azure.com/), in uw Azure Cosmos DB-account, in het linker navigatiegedeelte op **Sleutels** en klik vervolgens op **Sleutels voor lezen/schrijven**. In de volgende stap gebruikt u de kopieerknoppen aan de rechterkant van het scherm om de URI en Primaire sleutel in het bestand app.config te kopiëren.

    ![Een toegangssleutel bekijken en kopiëren in Azure Portal, blade Sleutels](./media/create-documentdb-dotnet-core/keys.png)

2. In Visual Studio opent u het bestand app.config. 

3. Kopieer de accountnaam van uw Azure Cosmos DB van de portal en geef deze als waarde aan de Accountnaam in de tekenreekswaarde voor PremiumStorageConnection in app.config. In bovenstaande schermopname is de accountnaam cosmos-db-quickstart. Uw accountnaam staat boven aan in de portal.

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`

4. Kopieer vervolgens de waarde van uw PRIMAIRE SLEUTEL van de portal en geef deze als waarde aan de AccountKey in PremiumStorageConnectionString. 

    `AccountKey=AUTHKEY`

5. Kopieer ten slotte uw URI-waarde van de pagina Sleutels van de portal (met de kopieerknop) en geef deze als waarde aan de TableEndpoint van de PremiumStorageConnectionString.

    `TableEndpoint=https://COSMOSDB.documents.azure.com`

    U kunt de StandardStorageConnectionString ongewijzigd laten.

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

## <a name="run-the-web-app"></a>De web-app uitvoeren

1. Klik in Visual Studio met de rechtermuisknop op het project in **Solution Explorer** en klik vervolgens op **NuGet-pakketten beheren**. 

2. In het NuGet-vak **Bladeren** typt u *WindowsAzure.Storage* en schakelt u het selectievakje **Inclusief voorlopige versie** in. 

3. Installeer de bibliotheek **WindowsAzure.Storage** vanuit de lijst met resultaten. Nu wordt de preview geïnstalleerd van het Azure Cosmos DB Table-API-pakket en alle daarvoor vereiste onderdelen.

4. Klik op Ctrl+F5 om de toepassing uit te voeren.

    In het consolevenster worden de gegevens weergegeven die aan de tabel worden toegevoegd. Zodra het script is voltooid, sluit u het consolevenster. 

U kunt nu teruggaan naar de Data Explorer en deze nieuwe gegevens bekijken, wijzigen, een query erop uitvoeren of er iets anders mee doen. 

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze Quick Start zijn aangemaakt door onderstaande stappen te volgen in Azure Portal: 

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een tabel gemaakt met de Data Explorer en hebt u een app uitgevoerd.  Nu kunt u een query uitvoeren op uw gegevens met de Table-API.  

> [!div class="nextstepaction"]
> [Query uitvoeren met behulp van de Table-API](tutorial-query-table.md)


