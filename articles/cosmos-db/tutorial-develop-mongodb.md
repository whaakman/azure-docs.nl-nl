---
title: Azure Cosmos DB-API voor MongoDB gebruiken voor het bouwen van een web-app | Microsoft Docs
description: Een Azure DB die Cosmos-zelfstudie waarmee u maakt een online database web-app met behulp van de API voor MongoDB.
keywords: Voorbeelden van mongodb
services: cosmos-db
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 61a2ab3a-2fc3-4d49-a263-ed87c66628f6
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: f7aed1c69e930bdd5e5b451fa2726a75b78515eb
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="azure-cosmos-db-connect-to-a-mongodb-app-using-net"></a>Azure Cosmos DB: Verbinding maken met een MongoDB-app met .NET

Azure Cosmos DB is de wereldwijd gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafen en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de wereldwijde distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze zelfstudie laat zien hoe een Azure DB die Cosmos-account maken met de Azure-portal en het maken van een database en verzameling voor het opslaan van gegevens met behulp van de [MongoDB API](mongodb-introduction.md). 

Deze zelfstudie bevat de volgende taken:

> [!div class="checklist"]
> * Maak een Azure Cosmos DB-account 
> * Uw verbindingsreeks bijwerken
> * Een MongoDB-app op een virtuele machine maken 


## <a name="create-a-database-account"></a>Een databaseaccount maken

Begint met het maken van een Azure DB die Cosmos-account in de Azure portal.  

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

> [!TIP]
> * Hebt u al een Azure DB die Cosmos-account? Als dit het geval is, gaat u verder met [uw Visual Studio-oplossing instellen](#SetupVS)
> * Als u de Emulator Azure Cosmos DB, volgt u de stappen in [Azure Cosmos DB Emulator](local-emulator.md) instellen van de emulator en gaat u verder met [instellen van uw Visual Studio-oplossing](#SetupVS). 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

1. In de Azure-portal in de **Azure Cosmos DB** pagina, selecteert u de API voor MongoDB-account. 
2. Klik in de linkerbalk van de accountblade op **snel starten**. 
3. Kies uw platform (*.NET stuurprogramma*, *Node.js stuurprogramma*, *MongoDB-Shell*, *Java stuurprogramma*, *Python stuurprogramma*). Als u het hulpprogramma vermeld of niet ziet, maak je geen zorgen, we continu meer verbinding codefragmenten-document. 
4. Kopiëren en plakken het codefragment in uw app MongoDB, en u bent klaar om te gaan.

## <a name="set-up-your-mongodb-app"></a>Stelt de MongoDB-app

U kunt de [een web-app maken in Azure die verbinding maakt met MongoDB uitgevoerd op een virtuele machine](../app-service/app-service-web-tutorial-nodejs-mongodb-app.md) zelfstudie, met minimale wijziging, om snel een MongoDB-toepassing in te stellen (een lokaal of gepubliceerd naar een Azure-web-app) die is verbonden aan een API voor MongoDB-account.  

1. Volg de zelfstudie, met één wijziging.  Vervang de code Dal.cs dit:

    ```csharp   
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using MyTaskListApp.Models;
    using MongoDB.Driver;
    using MongoDB.Bson;
    using System.Configuration;
    using System.Security.Authentication;
   
    namespace MyTaskListApp
    {
        public class Dal : IDisposable
        {
            //private MongoServer mongoServer = null;
            private bool disposed = false;
   
            // To do: update the connection string with the DNS name
            // or IP address of your server. 
            //For example, "mongodb://testlinux.cloudapp.net
            private string connectionString = "mongodb://localhost:27017";
            private string userName = "<your user name>";
            private string host = "<your host>";
            private string password = "<your password>";
   
            // This sample uses a database named "Tasks" and a 
            //collection named "TasksList".  The database and collection 
            //will be automatically created if they don't already exist.
            private string dbName = "Tasks";
            private string collectionName = "TasksList";
   
            // Default constructor.        
            public Dal()
            {
            }
   
            // Gets all Task items from the MongoDB server.        
            public List<MyTask> GetAllTasks()
            {
                try
                {
                    var collection = GetTasksCollection();
                    return collection.Find(new BsonDocument()).ToList();
                }
                catch (MongoConnectionException)
                {
                    return new List<MyTask>();
                }
            }
   
            // Creates a Task and inserts it into the collection in MongoDB.
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
   
            private IMongoCollection<MyTask> GetTasksCollection()
            {
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
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            private IMongoCollection<MyTask> GetTasksCollectionForEdit()
            {
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
                var database = client.GetDatabase(dbName);
                var todoTaskCollection = database.GetCollection<MyTask>(collectionName);
                return todoTaskCollection;
            }
   
            # region IDisposable
   
            public void Dispose()
            {
                this.Dispose(true);
                GC.SuppressFinalize(this);
            }
   
            protected virtual void Dispose(bool disposing)
            {
                if (!this.disposed)
                {
                    if (disposing)
                    {
                    }
                }
   
                this.disposed = true;
            }
   
            # endregion
        }
    }
    ```

2. Wijzig de volgende variabelen in het bestand Dal.cs per de instellingen van uw account via de pagina sleutels in de Azure-portal:

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. De app gebruiken!

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app verder niet gaat gebruiken, kunt u met de volgende stappen alle resources verwijderen die met deze zelfstudies in Azure Portal zijn gemaakt. 

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Maak een Azure Cosmos DB-account 
> * Uw verbindingsreeks bijwerken
> * Een MongoDB-app op een virtuele machine maken

U kunt doorgaan met de volgende zelfstudie en uw MongoDB-gegevens importeren in Azure Cosmos DB.  

> [!div class="nextstepaction"]
> [MongoDB-gegevens importeren in Azure Cosmos DB](mongodb-migrate.md)

