---
title: 'Azure Cosmos DB: een web-app ontwikkelen met .NET en de MongoDB-API | Microsoft-documenten'
description: Biedt een voorbeeld van .NET-code die u kunt gebruiken om verbinding te maken met de MongoDB-API van Azure Cosmos DB en er query’s op uit te voeren
services: cosmos-db
author: slyons
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.custom: quick start connect, mvc
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 05/22/2018
ms.author: sclyon
ms.openlocfilehash: 7ab02cf2cc9a25a5c4c7aa6d782d37d932dc8369
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43701948"
---
# <a name="azure-cosmos-db-build-a-mongodb-api-web-app-with-net-and-the-azure-portal"></a>Azure Cosmos DB: een MongoDB-API-web-app ontwikkelen met .NET en de Azure Portal

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

In deze snelstart laten we u zien hoe u een [MongoDB-API](mongodb-introduction.md)-account van Azure Cosmos DB, een documentdatabase en een verzameling kunt maken met behulp van Azure Portal. Vervolgens ontwikkelt en implementeert u een web-app voor taaklijsten op basis van het [MongoDB .NET-stuurprogramma](https://docs.mongodb.com/ecosystem/drivers/csharp/).

## <a name="prerequisites-to-run-the-sample-app"></a>Vereisten voor het uitvoeren van de voorbeeld-app

Als u het voorbeeld wilt uitvoeren, hebt u [Visual Studio](https://www.visualstudio.com/downloads/) en een geldig Azure Cosmos DB-account nodig.

Als u Visual Studio nog niet hebt, downloadt u [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) waarbij de workload **ASP.NET-ontwikkeling en webontwikkeling** tijdens het instellen wordt geïnstalleerd.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 

<a id="create-account"></a>
## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

Het voorbeeld dat in dit artikel wordt beschreven, is compatibel met MongoDB.Driver versie 2.6.1.

## <a name="clone-the-sample-app"></a>De voorbeeld-app klonen

Download eerst de voorbeeld-API-app MongoDB vanuit GitHub. Deze implementeert een lijst met taken met het documentopslagmodel van MongoDB.

1. Open een opdrachtprompt, maak een nieuwe map met de naam git-samples en sluit vervolgens de opdrachtprompt.

    ```bash
    md "C:\git-samples"
    ```

2. Open een git-terminalvenster, bijvoorbeeld git bash, en gebruik de `cd`-opdracht om naar de nieuwe map te gaan voor het installeren van de voorbeeld-app.

    ```bash
    cd "C:\git-samples"
    ```

3. Voer de volgende opdracht uit om de voorbeeldopslagplaats te klonen. Deze opdracht maakt een kopie van de voorbeeld-app op uw computer. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started.git
    ```

Als u niet git wilt gebruiken, kunt u ook [het project als een ZIP-bestand downloaden](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-dotnet-getting-started/archive/master.zip).

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources in de code worden gemaakt, kunt u de volgende codefragmenten bekijken. Als u deze stap wilt overslaan, kunt u verdergaan naar [Uw verbindingsreeks bijwerken](#update-your-connection-string). 

De volgende codefragmenten zijn alle overgenomen uit het bestand Dal.cs in de map DAL.

* Initialiseer de Mongo-client.

    ```cs
        MongoClientSettings settings = new MongoClientSettings();
        settings.Server = new MongoServerAddress(host, 10255);
        settings.UseSsl = true;
        settings.SslSettings = new SslSettings();
        settings.SslSettings.EnabledSslProtocols = SslProtocols.Tls12;

        MongoIdentity identity = new MongoInternalIdentity(dbName, userName);
        MongoIdentityEvidence evidence = new PasswordEvidence(password);

        settings.Credential = new MongoCredential("SCRAM-SHA-1", identity, evidence);

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

* Maakt een taak en voegt deze in de MongoDB-verzameling in

   ```csharp
    public void CreateTask(MyTask task)
    {
        var collection = GetTasksCollectionForEdit();
        try
        {
            collection.InsertOne(task);
        }
        catch (MongoCommandException ex)
        {
            string msg = ex.Message;
        }
    }
   ```
   Op dezelfde manier kunt u documenten bijwerken en verwijderen met behulp van de methoden [collection.UpdateOne()](https://docs.mongodb.com/stitch/mongodb/actions/collection.updateOne/index.html) en [collection.DeleteOne()](https://docs.mongodb.com/stitch/mongodb/actions/collection.deleteOne/index.html). 

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

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe een Azure Cosmos DB-account kunt maken en een web-app kunt uitvoeren met de API voor MongoDB. Nu kunt u aanvullende gegevens in uw Cosmos DB-account importeren. 

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB voor de MongoDB-API](mongodb-migrate.md)

