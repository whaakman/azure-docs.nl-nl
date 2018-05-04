---
title: Azure Cosmos DB-API voor MongoDB gebruiken voor het bouwen van een web-app | Microsoft Docs
description: Een Azure DB die Cosmos-zelfstudie waarmee u een online databaseweb-app maakt met behulp van de API voor MongoDB.
keywords: mongodb-voorbeelden
services: cosmos-db
author: AndrewHoh
manager: kfile
editor: ''
documentationcenter: ''
ms.assetid: 61a2ab3a-2fc3-4d49-a263-ed87c66628f6
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: anhoh
ms.custom: mvc
ms.openlocfilehash: 76a8e19bacdbde938758bf41ed7f209521f513aa
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/24/2018
---
# <a name="azure-cosmos-db-connect-to-a-mongodb-app-using-net"></a>Azure Cosmos DB: verbinding maken met een MongoDB-app met behulp van .NET

Azure Cosmos DB is de globaal gedistribueerde multimodel-databaseservice van Microsoft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query’s op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Azure Cosmos DB. 

Deze zelfstudie laat zien hoe u een Azure Cosmos DB-account maakt met behulp van de Azure-portal, en hoe u een database en verzameling maakt om gegevens op te slaan met behulp van de [MongoDB-API ](mongodb-introduction.md). 

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Maak een Azure Cosmos DB-account 
> * Uw verbindingsreeks bijwerken
> * Een MongoDB-app maken op een virtuele machine 


## <a name="create-a-database-account"></a>Een databaseaccount maken

Begin met het maken van een Azure Cosmos DB-account in Azure Portal.  

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 
[!INCLUDE [cosmos-db-emulator-mongodb](../../includes/cosmos-db-emulator-mongodb.md)]

> [!TIP]
> * Hebt u al een Azure Cosmos DB-account? Zo ja, ga dan verder met [Uw Visual Studio-oplossing instellen](#SetupVS)
> * Als u de Azure Cosmos DB Emulator gebruikt, volgt u de stappen in [Azure Cosmos DB Emulator](local-emulator.md) om de emulator in te stellen en meteen naar [Uw Visual Studio-oplossing instellen](#SetupVS) te gaan. 
>
>

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="update-your-connection-string"></a>Uw verbindingsreeks bijwerken

1. Selecteer in de Azure-portal op de pagina **Azure Cosmos DB** de API voor MongoDB-account. 
2. Klik in de linkerbalk van de accountblade op **Quick Start**. 
3. Kies uw platform (*.NET-stuurprogramma*, *Node.js-stuurprogramma*, *MongoDB-shell*, *Java stuurprogramma*, *Python-stuurprogramma*). Maak u geen zorgen als u uw stuurprogramma of hulpprogramma niet in de lijst ziet; we documenteren voortdurend meer verbindingscodefragmenten. 
4. Kopieer en plak het codefragment in uw MongoDB-app, en u bent klaar om van start te gaan.

## <a name="set-up-your-mongodb-app"></a>De MongoDB-app instellen

U kunt de zelfstudie [Een web-app in Azure maken die verbinding maakt met MongoDB op een virtuele machine](../app-service/app-service-web-tutorial-nodejs-mongodb-app.md) met een minimale wijziging gebruiken om snel een MongoDB-toepassing in te stellen (lokaal of gepubliceerd naar een Azure-web-app) die verbinding maakt met een API voor MongoDB-account.  

1. Volg de zelfstudie, met één wijziging.  Vervang de Dal.cs-code door deze:

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

2. Wijzig de volgende variabelen in het bestand Dal.cs overeenkomstig uw accountinstellingen op de pagina Sleutels in de Azure-portal:

    ```csharp   
    private string userName = "<your user name>";
    private string host = "<your host>";
    private string password = "<your password>";
    ```

3. Gebruik de app!

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze app verder niet gaat gebruiken, kunt u met de volgende stappen alle resources verwijderen die met deze zelfstudies in Azure Portal zijn gemaakt. 

1. Klik in het menu aan de linkerkant in Azure Portal op **Resourcegroepen** en klik vervolgens op de resource die u hebt gemaakt. 
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ de naam van de resource die u wilt verwijderen in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * Maak een Azure Cosmos DB-account 
> * Uw verbindingsreeks bijwerken
> * Een MongoDB-app maken op een virtuele machine

U kunt doorgaan met de volgende zelfstudie en uw MongoDB-gegevens importeren in Azure Cosmos DB.  

> [!div class="nextstepaction"]
> [MongoDB-gegevens importeren in Azure Cosmos DB](mongodb-migrate.md)

