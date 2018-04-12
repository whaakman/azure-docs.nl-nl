---
title: 'Azure Cosmos DB: een web-app ontwikkelen met .NET en de MongoDB-API | Microsoft-documenten'
description: Biedt een voorbeeld van .NET-code die u kunt gebruiken om verbinding te maken met de MongoDB-API van Azure Cosmos DB en er query’s op uit te voeren
services: cosmos-db
documentationcenter: ''
author: SnehaGunda
manager: kfile
ms.assetid: ''
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/19/2018
ms.author: sngun
ms.openlocfilehash: ab14261e939063c5e50050774d1aae3edf1bef19
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-the-azure-portal"></a>Azure Cosmos DB: een MongoDB-API-web-app ontwikkelen met .NET en de Azure Portal

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

In deze snelstart laten we u zien hoe u een [MongoDB-API](mongodb-introduction.md)-account van Azure Cosmos DB, een documentdatabase en een verzameling kunt maken met behulp van Azure Portal. Vervolgens ontwikkelt en implementeert u een web-app voor taaklijsten op basis van het [MongoDB .NET-stuurprogramma](https://docs.mongodb.com/ecosystem/drivers/csharp/).

## <a name="prerequisites-to-run-the-sample-app"></a>Vereisten voor het uitvoeren van de voorbeeld-app

Als u het voorbeeld wilt uitvoeren, hebt u [Visual Studio](https://www.visualstudio.com/downloads/) en een geldig Azure Cosmos DB-account nodig.

Als u Visual Studio nog niet hebt, downloadt u [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) waarbij de workload **ASP.NET-ontwikkeling en webontwikkeling** tijdens het instellen wordt geïnstalleerd.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="clone-the-sample-app"></a>De voorbeeld-app klonen

Download eerst de voorbeeld-API-app MongoDB vanuit GitHub. Deze implementeert een lijst met taken met het documentopslagmodel van MongoDB.

1. Open een venster in een git-terminal zoals git bash en `cd` naar een werkmap.
2. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

Als u niet git wilt gebruiken, kunt u ook [het project als een ZIP-bestand downloaden](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started/archive/master.zip).

## <a name="review-the-code"></a>De code bekijken

Laten we eens kijken wat er precies gebeurt in de app. Open het bestand **Dal.cs** in de map **DAL**. U zult zien dat deze regels met code de Azure Cosmos DB-resources aanmaken. 

* Initialiseer de Mongo-client.

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credentials = new List<MongoCredential>()
        {
            new MongoCredential("SCRAM-SHA-1", identity, evidence)
        };

        MongoClient client = new MongoClient(settings);
    ```

* Haal de database en de verzameling op.

    ```cs
    private string dbName = "Tasks";
    private string collectionName = "TasksList";

    var database = client.GetDatabase(dbName);
    var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
    ```

* Haal alle documenten op.

    ```cs
    collection.Find(new BsonDocument()).ToList();
    ```

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app.

1. Klik in [Azure Portal](http://portal.azure.com/), in uw Azure Cosmos DB-account, in het linker navigatiegedeelte op **Verbindingsreeks** en klik vervolgens op **Sleutels voor lezen/schrijven**. U gebruikt de kopieerknoppen aan de rechterkant van het scherm om de Gebruikersnaam, het Wachtwoord, en de Host naar het bestand Dal.cs te kopiëren. Dit doet u in de volgende stap.

2. Open het bestand **Dal.cs** in de map **DAL**. 

3. Kopieer uw **gebruikersnaam** van de portal (gebruik de kopieerknop) en geef deze waarde aan de **gebruikersnaam** in uw **Dal.cs**-bestand. 

4. Kopieer vervolgens de waarde van **host** van de portal en geef deze waarde aan de **host** in uw **Dal.cs**-bestand. 

5. Kopieer ten slotte de waarde van **wachtwoord** van de portal en geef deze waarde aan het **wachtwoord** in uw **Dal.cs**-bestand. 

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB. 
    
## <a name="run-the-web-app"></a>De web-app uitvoeren

1. Klik in Visual Studio met de rechtermuisknop op het project in **Solution Explorer** en klik vervolgens op **NuGet-pakketten beheren**. 

2. Typ in het vak **Bladeren** in NuGet *MongoDB.Driver*.

3. Installeer de bibliotheek **MongoDB.Driver** uit de lijst met resultaten. Hiermee wordt het MongoDB.Driver-pakket geïnstalleerd, met alle vereiste onderdelen.

4. Klik op Ctrl+F5 om de toepassing uit te voeren. Uw app wordt in uw browser weergegeven. 

5. Klik op **Maken** in de browser en maak een paar nieuwe taken in uw taaklijst-app.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app niet verder gaat gebruiken, kunt u alle resources verwijderen die door deze Quick Start zijn aangemaakt door onderstaande stappen te volgen in Azure Portal:

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe een Azure Cosmos DB-account kunt maken en een web-app kunt uitvoeren met de API voor MongoDB. Nu kunt u aanvullende gegevens in uw Cosmos DB-account importeren. 

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB voor de MongoDB-API](mongodb-migrate.md)

