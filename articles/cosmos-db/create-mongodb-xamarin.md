---
title: Een Xamarin.Forms-app ontwikkelen met .NET en Azure Cosmos DB-API voor MongoDB
description: Deze snelstart bevat een voorbeeld van Xamarin-code dat u kunt gebruiken om verbinding te maken met de Azure Cosmos DB-API voor MongoDB en er query's op uit te voeren
author: codemillmatt
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 06/20/2018
ms.author: masoucou
ms.openlocfilehash: 9236a340c05bc5e342b9fc2c377d6d8eacb94f7d
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54036414"
---
# <a name="quickstart-quickstart-build-a-xamarinforms-app-with-net-and-azure-cosmos-dbs-api-for-mongodb"></a>Snelstartgids: Snelstartgids: Een Xamarin.Forms-app ontwikkelen met .NET en Azure Cosmos DB-API voor MongoDB

> [!div class="op_single_selector"]
> * [.NET](create-mongodb-dotnet.md)
> * [Java](create-mongodb-java.md)
> * [Node.js](create-mongodb-nodejs.md)
> * [Python](create-mongodb-flask.md)
> * [Xamarin](create-mongodb-xamarin.md)
> * [Golang](create-mongodb-golang.md)
>  

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB.

In deze snelstart laten we u zien hoe u een [Cosmos-account maakt dat is geconfigureerd met Azure Cosmos DB-API voor MongoDB](mongodb-introduction.md) en een documentdatabase en verzameling kunt maken met behulp van Azure Portal. U bouwt vervolgens een todo-app van de app Xamarin.Forms met behulp van het [MongoDB .NET-stuurprogramma](https://docs.mongodb.com/ecosystem/drivers/csharp/).

## <a name="prerequisites-to-run-the-sample-app"></a>Vereisten voor het uitvoeren van de voorbeeld-app

Als u het voorbeeld wilt uitvoeren, hebt u [Visual Studio](https://www.visualstudio.com/downloads/) of [Visual Studio voor Mac](https://visualstudio.microsoft.com/vs/mac/) en een geldig Azure Cosmos DB-account nodig.

Als u Visual Studio nog niet hebt, downloadt u [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/) waarbij de workload **Mobile development with .NET** tijdens het instellen wordt geïnstalleerd.

Als u liever op een Mac werkt, download dan [Visual Studio voor Mac](https://visualstudio.microsoft.com/vs/mac/) en voer de installatie uit.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

<a id="create-account"></a>

## <a name="create-a-database-account"></a>Een databaseaccount maken

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

Het voorbeeld dat in dit artikel wordt beschreven, is compatibel met MongoDB.Driver versie 2.6.1.

## <a name="clone-the-sample-app"></a>De voorbeeld-app klonen

Download eerst de voorbeeld-app uit GitHub. Deze implementeert een takenlijst-app met het documentopslagmodel van MongoDB.

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
    git clone https://github.com/Azure-Samples/azure-cosmos-db-mongodb-xamarin-getting-started.git
    ```

Als u niet git wilt gebruiken, kunt u ook [het project als een ZIP-bestand downloaden](https://github.com/Azure-Samples/azure-cosmos-db-mongodb-xamarin-getting-started/archive/master.zip)

## <a name="review-the-code"></a>De code bekijken

Deze stap is optioneel. Als u wilt weten hoe de databaseresources in de code worden gemaakt, kunt u de volgende codefragmenten bekijken. Als u deze stap wilt overslaan, kunt u verdergaan naar [Uw verbindingsreeks bijwerken](#update-your-connection-string).

De volgende codefragmenten zijn allemaal afkomstig uit de klasse `MongoService`, te vinden in het volgende pad: src/TaskList.Core/Services/MongoService.cs.

* Initialiseer de Mongo-client.
    ```cs
    MongoClientSettings settings = MongoClientSettings.FromUrl(
        new MongoUrl(APIKeys.ConnectionString)
    );

    settings.SslSettings =
        new SslSettings() { EnabledSslProtocols = SslProtocols.Tls12 };

    MongoClient mongoClient = new MongoClient(settings);
    ```

* Haal een verwijzing naar de database en verzameling op. De MongoDB .NET-SDK maakt automatisch zowel de database als de verzameling, als deze nog niet bestaan.
    ```cs
    string dbName = "MyTasks";
    string collectionName = "TaskList";

    var db = mongoClient.GetDatabase(dbName);

    var collectionSettings = new MongoCollectionSettings {
        ReadPreference = ReadPreference.Nearest
    };

    tasksCollection = db.GetCollection<MyTask>(collectionName, collectionSettings);
    ```
* Haal alle documenten op als een lijst.
    ```cs
    var allTasks = await TasksCollection
                    .Find(new BsonDocument())
                    .ToListAsync();
    ```

* Query voor specifieke documenten.
    ```cs
    public async Task<List<MyTask>> GetIncompleteTasksDueBefore(DateTime date)
    {
        var tasks = await TasksCollection
                        .AsQueryable()
                        .Where(t => t.Complete == false)
                        .Where(t => t.DueDate < date)
                        .ToListAsync();

        return tasks;
    }
    ```

* Maak een taak en voeg deze aan de verzameling toe.
    ```cs
    public async Task CreateTask(MyTask task)
    {
        await TasksCollection.InsertOneAsync(task);
    }
    ```

* Werk een taak bij in een verzameling.
    ```cs
    public async Task UpdateTask(MyTask task)
    {
        await TasksCollection.ReplaceOneAsync(t => t.Id.Equals(task.Id), task);
    }
    ```

* Verwijder een taak uit een verzameling.
    ```cs
    public async Task DeleteTask(MyTask task)
    {
        await TasksCollection.DeleteOneAsync(t => t.Id.Equals(task.Id));
    }
    ```

<a id="update-your-connection-string"></a>

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

Ga nu terug naar Azure Portal om de verbindingsreeksinformatie op te halen en kopieer deze in de app.

1. Klik in [Azure Portal](https://portal.azure.com/), in uw Azure Cosmos DB-account, in het linker navigatiegedeelte op **Verbindingsreeks** en klik vervolgens op **Sleutels voor lezen/schrijven**. In de volgende stappen gebruikt u de kopieerknoppen aan de rechterkant van het scherm om de Primaire verbindingsreeks te kopiëren.

2. Open het bestand **APIKeys.cs** in de directory **Helpers** van het project **TaskList.Core**.

3. Kopieer de waarde van uw **primaire verbindingsreeks** uit de portal (met behulp van de knop Kopiëren) en maak deze de waarde van het veld **ConnectionString** in uw **APIKeys.cs**-bestand.

U hebt uw app nu bijgewerkt met alle informatie die nodig is voor de communicatie met Azure Cosmos DB.

## <a name="run-the-app"></a>De app uitvoeren

### <a name="visual-studio-2017"></a>Visual Studio 2017

1. Klik in Visual Studio met de rechtermuisknop op elk project in **Solution Explorer** en klik vervolgens op **NuGet-pakketten beheren**.
2. Klik op **Alle NuGet-pakketten herstellen**.
3. Klik met de rechtermuisknop op **TaskList.Android** en selecteer **Instellen als opstartproject**.
4. Druk op F5 om te beginnen met de foutopsporing van de toepassing.
5. Als u op iOS wilt werken, wordt uw machine eerst verbonden met een Mac (hier zijn [instructies](https://docs.microsoft.com/xamarin/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio) over hoe u dat kunt doen).
6. Klik met de rechtermuisknop op **TaskList.iOS**-project en selecteer **Instellen als opstartproject**.
7. Klik op F5 om te beginnen met de foutopsporing van de toepassing.

### <a name="visual-studio-for-mac"></a>Visual Studio voor Mac

1. Selecteer in de platformvervolgkeuzelijst TaskList.iOS of TaskList.Android, afhankelijk van op welk platform u wilt werken.
2. Druk op cmd+Enter om te beginnen met de foutopsporing van de toepassing.

## <a name="review-slas-in-the-azure-portal"></a>SLA’s bekijken in Azure Portal

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u geleerd hoe een Azure Cosmos DB-account kunt maken en een Xamarin.Forms-app kunt uitvoeren met de API voor MongoDB. Nu kunt u aanvullende gegevens in uw Cosmos DB-account importeren.

> [!div class="nextstepaction"]
> [Gegevens importeren in Azure Cosmos DB die zijn geconfigureerd met Azure Cosmos DB-API voor MongoDB](mongodb-migrate.md)
