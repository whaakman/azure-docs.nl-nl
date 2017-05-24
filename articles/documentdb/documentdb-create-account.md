---
title: Een Azure Cosmos DB-account maken | Microsoft Docs
description: Bouw een NoSQL-database met Azure Cosmos DB. Volg deze instructies om een Azure Cosmos DB-account te maken en bouw een razendsnelle, wereldwijd beschikbare NoSQL-database.
keywords: een database maken
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: monicar
ms.assetid: 0e7f8488-7bb7-463e-b6fd-3ae91a02c03a
ms.service: cosmosdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/17/2017
ms.author: mimig
redirect_url: https://aka.ms/acdbnetqa
ROBOTS: NOINDEX, NOFOLLOW
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 13621d942f2880f4dd1523315f43099eca2607d8
ms.contentlocale: nl-nl
ms.lasthandoff: 05/10/2017


---
# <a name="how-to-create-an-azure-cosmos-db-nosql-account-using-the-azure-portal"></a>Een Azure Cosmos DB NoSQL-account maken via Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](documentdb-create-account.md)
> * [Azure CLI 1.0](documentdb-automation-resource-manager-cli-nodejs.md)
> * [Azure CLI 2.0](documentdb-automation-resource-manager-cli.md)
> * [Azure Powershell](documentdb-manage-account-with-powershell.md)

Als u een database wilt bouwen met Microsoft Azure Cosmos DB, moet u:

* Een Azure-account hebben. Als u nog geen account hebt, kunt u een [gratis Azure-account](https://azure.microsoft.com/free) krijgen.
* Een Azure Cosmos DB-account maken.  

U kunt een Azure Cosmos DB-account maken met Azure Portal, Azure Resource Manager-sjablonen of Azure CLI. In dit artikel wordt uitgelegd hoe u een Azure Cosmos DB-account maakt met Azure Portal. Zie [Het maken van een Azure Cosmos DB-databaseaccount automatiseren](documentdb-automation-resource-manager-cli.md) voor informatie over hoe u een account maakt met Azure Resource Manager of Azure CLI.

Bent u nieuw bij Azure Cosmos DB? Bekijk [deze](https://azure.microsoft.com/documentation/videos/create-documentdb-on-azure/) vier minuten durende video van Scott Hanselman om te zien hoe u de meest algemene taken in de online portal uitvoert.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in de linkernavigatie achtereenvolgens op **Nieuw**, **Databases** en **Azure Cosmos DB**.

   ![Schermopname van Azure Portal waarbij Meer services en NoSQL (Azure Cosmos DB) zijn gemarkeerd](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-1.png)  
3. Geef op de blade **Nieuw account** de gewenste configuratie op voor het Azure Cosmos DB-account.

    ![Schermopname van de blade Nieuwe Azure Cosmos DB](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-2.png)

   * Typ in het vak **Id** een naam om het Azure Cosmos DB-account te kunnen identificeren.  Wanneer de **id** is gevalideerd, verschijnt een groen vinkje in het vak **Id**. De waarde van **Id** wordt de hostnaam binnen de URI. De **id** mag alleen kleine letters, cijfers en het minteken ('-') bevatten, en moet tussen de 3 en 50 tekens zijn. Houd er rekening mee dat *documents.azure.com* wordt toegevoegd aan de eindpuntnaam die u kiest. Het resultaat hiervan wordt het eindpunt van uw Azure Cosmos DB-account.
   * Selecteer in het vak **NoSQL-API** het programmeermodel dat u wilt gebruiken:

     * **DocumentDB**: de API van DocumentDB is beschikbaar via .NET-, Java-, Node.js-, Python- en JavaScript-[SDK's](documentdb-sdk-dotnet.md), evenals HTTP [REST](https://msdn.microsoft.com/library/azure/dn781481.aspx), en biedt programmatische toegang tot alle functionaliteit van DocumentDB.
     * **MongoDB**: DocumentDB biedt ook [ondersteuning op protocolniveau](documentdb-protocol-mongodb.md) voor **MongoDB**-API's. Als u de optie MongoDB-API kiest, kunt u bestaande MongoDB-SDK's en [hulpmiddelen](documentdb-mongodb-mongochef.md) gebruiken voor communicatie met DocumentDB. U kunt uw bestaande MongoDB-apps [zonder codewijzigingen](documentdb-connect-mongodb-account.md) [verplaatsen](documentdb-import-data.md) om DocumentDB te gebruiken. U kunt dan profiteren van een volledig beheerde database als een service, met onbeperkte schaal, globale replicatie en andere functies.
   * Selecteer bij **Abonnement** het Azure-abonnement dat u voor het Azure Cosmos DB-account wilt gebruiken. Als uw account slechts één abonnement heeft, wordt dit account standaard geselecteerd.
   * In **Resourcegroep** selecteert of maakt u een resourcegroep voor uw Azure Cosmos DB-account.  Standaard wordt een nieuwe resourcegroep gemaakt. Zie [Using the Azure portal to manage your Azure resources](../azure-portal/resource-group-portal.md) (Azure Portal gebruiken om uw Azure-resources te beheren) voor meer informatie.
   * Gebruik **Locatie** om de geografische locatie op te geven waar uw Azure Cosmos DB-account moet worden gehost.
4. Nadat de opties voor het nieuwe Azure Cosmos DB-account zijn geconfigureerd, klikt u op **Maken**. Controleer de hub Meldingen als u de status van de implementatie wilt bekijken.  

   ![Snel databases maken: schermopname van Notification Hubs waarop u ziet dat het Azure Cosmos DB-account wordt gemaakt](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-4.png)  

   ![Schermopname van Notification Hubs waarop u ziet dat het Azure Cosmos DB-account is gemaakt en in een resourcegroep is geïmplementeerd - melding van de functie voor het maken van online databases](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-5.png)
5. Nadat het Azure Cosmos DB-account is gemaakt, kan het met de standaardinstellingen worden gebruikt. De standaardconsistentie van het Azure Cosmos DB-account is ingesteld op **Sessie**.  U kunt de standaardconsistentie aanpassen door in het resourcemenu op **Standaardconsistentie** te klikken. Zie [Consistency levels in Azure Cosmos DB](documentdb-consistency-levels.md) (Consistentieniveaus in Azure Cosmos DB) voor meer informatie over de consistentieniveaus die Azure Cosmos DB biedt.

   ![Schermopname van de blade Resourcegroep - begin van toepassingsontwikkeling](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-6.png)  

   ![Schermopname van de blade Consistentieniveau - sessieconsistentie](./media/documentdb-create-account/create-nosql-db-databases-json-tutorial-7.png)  

[How to: Create an Azure Cosmos DB account]: #Howto
[Next steps]: #NextSteps


## <a name="next-steps"></a>Volgende stappen
Nu u beschikt over een Azure Cosmos DB-account, kunt u een Azure Cosmos DB-verzameling en -database maken.

Gebruik een van de volgende manieren om een nieuwe verzameling en database te bouwen:

* Via Azure Portal, zoals is beschreven in [Een Azure Cosmos DB-verzameling maken met Azure Portal](documentdb-create-collection.md).
* Via de uitgebreide zelfstudies, die ook voorbeeldgegevens bevatten: [.NET](documentdb-get-started.md), [.NET MVC](documentdb-dotnet-application.md), [Java](documentdb-java-application.md), [Node.js](documentdb-nodejs-application.md) of [Python](documentdb-python-application.md).
* Via de [.NET](documentdb-dotnet-samples.md#database-examples)-, [Node.js](documentdb-nodejs-samples.md#database-examples)- of [Python](documentdb-python-samples.md#database-examples)-voorbeeldcode die beschikbaar is in GitHub.
* Via de [.NET](documentdb-sdk-dotnet.md)-, [.NET Core](documentdb-sdk-dotnet-core.md)-, [Node.js](documentdb-sdk-node.md)-, [Java](documentdb-sdk-java.md)-, [Python](documentdb-sdk-python.md)- en [REST](https://msdn.microsoft.com/library/azure/mt489072.aspx)-SDK's.

Nadat de database en verzameling zijn gemaakt, gaat u [documenten toevoegen](documentdb-view-json-document-explorer.md) aan de verzamelingen.

Wanneer er documenten in een verzameling staan, kunt u [DocumentDB SQL](documentdb-sql-query.md) gebruiken om [query's uit te voeren](documentdb-sql-query.md#ExecutingSqlQueries) voor uw documenten. U kunt query's uitvoeren met behulp van de [Queryverkenner](documentdb-query-collections-query-explorer.md) in de portal, de [REST-API](https://msdn.microsoft.com/library/azure/dn781481.aspx) of een van de [SDK's](documentdb-sdk-dotnet.md).

### <a name="learn-more"></a>Meer informatie
Raadpleeg de volgende resources voor meer informatie over Azure Cosmos DB:

* [Inleiding in Azure Cosmos DB](../cosmos-db/introduction.md)

