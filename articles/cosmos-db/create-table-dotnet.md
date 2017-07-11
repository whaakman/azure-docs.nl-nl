---
title: Een Azure Cosmos DB .NET-toepassing ontwikkelen met behulp van de Table-API | Microsoft-documenten
description: Aan de slag met de Table-API van Azure Cosmos DB met behulp van .NET
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 66327041-4d5e-4ce6-a394-fee107c18e59
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/22/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 8be2bcb9179e9af0957fcee69680ac803fd3d918
ms.openlocfilehash: 29e7eebda5177d6e852ef04ad82d9d38a8d30ed8
ms.contentlocale: nl-nl
ms.lasthandoff: 06/23/2017


---
<a id="azure-cosmos-db-build-a-net-application-using-the-table-api" class="xliff"></a>

# Azure Cosmos DB: een .NET-toepassing ontwikkelen met de Table-API

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze Quick Start laat zien hoe u een Azure Cosmos DB-account en een tabel binnen dat account kunt maken met behulp van de Azure Portal. U moet vervolgens code schrijven om entiteiten in te voegen, bij te werken en te verwijderen en enkele query's uit te voeren met behulp van het nieuwe [Windows Azure Storage Premium Table](https://aka.ms/premiumtablenuget)-pakket (preview) van NuGet. Deze bibliotheek heeft dezelfde klassen en handtekeningen voor methodes als de openbare [Windows Azure Storage SDK](https://www.nuget.org/packages/WindowsAzure.Storage), maar biedt ook de mogelijkheid verbinding te maken met Azure Cosmos DB-accounts met behulp van de [Table-API](table-introduction.md) (preview). 

<a id="prerequisites" class="xliff"></a>

## Vereisten

Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken. Zorg ervoor dat u **Azure-ontwikkeling** inschakelt tijdens de installatie van Visual Studio.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-a-database-account" class="xliff"></a>

## Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount-table](../../includes/cosmos-db-create-dbaccount-table.md)]

<a id="add-a-table" class="xliff"></a>

## Een tabel toevoegen

[!INCLUDE [cosmos-db-create-table](../../includes/cosmos-db-create-table.md)]

<a id="add-sample-data" class="xliff"></a>

## Voorbeeldgegevens toevoegen

U kunt nu gegevens aan uw nieuwe tabel toevoegen met behulp van Data Explorer (Preview).

1. Vouw in Data Explorer **sample-table** uit, klik op **Entiteiten** en klik vervolgens op **Entiteit toevoegen**.

   ![Nieuwe entiteiten maken in Data Explorer in Azure Portal](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-document.png)
2. Voeg nu gegevens toe aan de vakken met een waarde voor PartitionKey en RowKey en klik op **Entiteit toevoegen**.

   ![De partitiesleutel en de rijsleutel instellen voor een nieuwe entiteit](./media/create-table-dotnet/azure-cosmosdb-data-explorer-new-entity.png)
  
    U kunt nu meer entiteiten toevoegen aan uw tabel, uw entiteiten bewerken of een query op uw gegevens uitvoeren in Data Explorer. Data Explorer is ook de plek waar u uw doorvoer kunt schalen en opgeslagen procedures, door de gebruiker gedefinieerde functies en triggers aan uw tabel kunt toevoegen.

<a id="clone-the-sample-application" class="xliff"></a>

## De voorbeeldtoepassing klonen

We gaan nu een Table-app klonen vanaf GitHub, de verbindingsreeks instellen en de app uitvoeren. U zult zien hoe gemakkelijk het is om op een programmatische manier met gegevens te werken. 

1. Open een venster in een git-terminal zoals git bash en `cd` naar een werkmap.  

2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-table-dotnet-getting-started.git
    ```

3. Open vervolgens het oplossingenbestand in Visual Studio. 

<a id="review-the-code" class="xliff"></a>

## De code bekijken

Laten we eens kijken wat er precies gebeurt in de app. Open het bestand Program.cs en u zult zien dat deze regels code de Azure Cosmos DB-resources maken. 

* De CloudTableClient is geïnitialiseerd.

    ```csharp
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString); 
    CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
    ```

* Er wordt een nieuwe tabel gemaakt als er nog geen tabel bestaat.

    ```csharp
    CloudTable table = tableClient.GetTableReference("people");
    table.CreateIfNotExists();
    ```

* Er wordt een nieuwe Table-container gemaakt. Deze code is vergelijkbaar met de reguliere code van de Azure Table Storage-SDK. 

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

<a id="update-your-connection-string" class="xliff"></a>

## Uw verbindingsreeks bijwerken

We werken nu de verbindingsreeksinformatie bij, zodat uw app kan communiceren met Azure Cosmos DB. 

1. In Visual Studio opent u het bestand app.config. 

2. In [Azure Portal](http://portal.azure.com/) klikt u in het Azure Cosmos DB-navigatiemenu links op **Verbindingsreeks**. Klik vervolgens in het nieuwe deelvenster op de knop Kopiëren voor de verbindingsreeks. 

    ![Het eindpunt en de accountsleutel in het deelvenster Verbindingsreeks weergeven en kopiëren](./media/create-table-dotnet/keys.png)

3. Plak de waarde in het bestand app.config als de waarde van de PremiumStorageConnectionString. 

    `<add key="PremiumStorageConnectionString" 
        value="DefaultEndpointsProtocol=https;AccountName=MYSTORAGEACCOUNT;AccountKey=AUTHKEY;TableEndpoint=https://COSMOSDB.documents.azure.com" />`    

    U kunt de StandardStorageConnectionString ongewijzigd laten.

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 

<a id="run-the-web-app" class="xliff"></a>

## De web-app uitvoeren

1. Klik in Visual Studio met de rechtermuisknop op het project in **PremiumTableGetStarted** in **Solution Explorer** en klik vervolgens op **NuGet-pakketten beheren**. 

2. Typ in het vak **Bladeren** in NuGet *WindowsAzure.Storage-PremiumTable*.

3. Controleer het vak **Prerelease insluiten**. 

4. Installeer de bibliotheek **WindowsAzure.Storage-PremiumTable** vanuit de lijst met resultaten. Nu wordt de preview geïnstalleerd van het Azure Cosmos DB Table-API-pakket en alle daarvoor vereiste onderdelen. Houd er rekening mee dat dit een ander NuGet-pakket is dan het Windows Azure Storage-pakket dat wordt gebruikt door Azure Table Storage. 

5. Klik op Ctrl+F5 om de toepassing uit te voeren.

    In het consolevenster worden de gegevens weergegeven die worden toegevoegd, opgehaald, opgevraagd, vervangen en verwijderd uit de tabel. Zodra het script is voltooid, drukt u op een willekeurige toets om het consolevenster te sluiten. 
    
    ![Console-uitvoer van de snelstartgids](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-console-output.png)

6. Als u de nieuwe entiteiten in Data Explorer wilt weergeven, plaatst u opmerkingen bij regels 188-208 in program.cs, zodat deze niet worden verwijderd. Vervolgens voert u het voorbeeld opnieuw uit. 

    U kunt nu terug naar Data Explorer. Klik op **Vernieuwen**, vouw de tabel **Mensen** uit en klik op **Entiteiten**. Maar vervolgens gebruik van deze nieuwe gegevens. 

    ![Nieuwe entiteiten in Data Explorer](./media/create-table-dotnet/azure-cosmosdb-table-quickstart-data-explorer.png)

<a id="review-slas-in-the-azure-portal" class="xliff"></a>

## SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

<a id="clean-up-resources" class="xliff"></a>

## Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze Quick Start zijn aangemaakt door onderstaande stappen te volgen in Azure Portal: 

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

<a id="next-steps" class="xliff"></a>

## Volgende stappen

In deze Quick Start hebt u geleerd hoe u een Azure Cosmos DB-account kunt maken, hebt u een tabel gemaakt met de Data Explorer en hebt u een app uitgevoerd.  Nu kunt u een query uitvoeren op uw gegevens met de Table-API.  

> [!div class="nextstepaction"]
> [Query uitvoeren met behulp van de Table-API](tutorial-query-table.md)


