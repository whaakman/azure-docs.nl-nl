---
title: Zelfstudie voor MongoDB, Angular en Node voor Azure - deel 4
description: Deel 4 van de serie zelfstudies voor het maken van een MongoDB-app met Angular en Node op Azure Cosmos DB, waarbij gebruik wordt gemaakt van dezelfde API's als voor MongoDB
services: cosmos-db
author: johnpapa
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/05/2017
ms.author: jopapa
ms.custom: mvc
ms.openlocfilehash: 21fd80156bf81b5ab985459bb43ca167fbe2d579
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865978"
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-4-create-an-azure-cosmos-db-account-using-the-azure-cli"></a>Een MongoDB-app maken met Angular en Azure DB Cosmos - Deel 4: Een Azure DB Cosmos-account maken met de Azure CLI

In deze meerdelige zelfstudie leert u hoe u een nieuwe [MongoDB API](mongodb-introduction.md)-app maakt in Node.js met Express, Angular en uw Azure Cosmos DB-database.

Deel 4 van de zelfstudie bouwt voort op [deel 3](tutorial-develop-mongodb-nodejs-part3.md) en beschrijft de volgende taken:

> [!div class="checklist"]
> * Een Azure-resourcegroep maken met de Azure CLI
> * Een Azure Cosmos DB account maken met de Azure CLI

## <a name="video-walkthrough"></a>Video-overzicht

> [!VIDEO https://www.youtube.com/embed/hfUM-AbOh94]

## <a name="prerequisites"></a>Vereisten

Voltooi de stappen in [deel 3](tutorial-develop-mongodb-nodejs-part3.md) van de zelfstudie voordat u aan dit deel begint. 

In dit gedeelte van de zelfstudie kunt u ofwel de Azure Cloud Shell gebruiken (in uw webbrowser) of de lokaal geïnstalleerde [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Log in to Azure](../../includes/login-to-azure.md)]

[!INCLUDE [Create resource group](../../includes/app-service-web-create-resource-group.md)]

> [!TIP]
> Aan de hand van deze zelfstudie wordt u stapsgewijs begeleid bij het maken van de toepassing. Als u het voltooide project wilt downloaden, kunt u de voltooide app vinden in de [angular-cosmosdb-opslagplaats](https://github.com/Azure-Samples/angular-cosmosdb) op GitHub.

## <a name="create-an-azure-cosmos-db-account"></a>Maak een Azure Cosmos DB-account

Maak een Azure Cosmos DB-account met de opdracht [`az cosmosdb create`](/cli/azure/cosmosdb#az-cosmosdb-create).

```azurecli-interactive
az cosmosdb create --name <cosmosdb-name> --resource-group myResourceGroup --kind MongoDB
```

* Gebruik voor `<cosmosdb-name>` uw eigen unieke Azure Cosmos DB-accountnaam. De naam moet uniek zijn binnen alle Azure Cosmos DB-accountnamen in Azure.
* De instelling `--kind MongoDB` maakt MongoDB-clientverbindingen mogelijk in Azure Cosmos DB.

Het duurt mogelijk enkele minuten voor de opdracht is voltooid. Wanneer de opdracht is voltooid geeft het terminalvenster informatie over de nieuwe database. 

Zodra het Azure Cosmos DB-account is gemaakt:
1. Open een nieuw browservenster en ga naar [https://portal.azure.com](https://portal.azure.com)
1. Klik op het logo van Azure Cosmos DB ![Azure Cosmos DB-pictogram in Azure Portal](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-icon.png) in de linkerbalk en toont alle de Azure Cosmos DB’s die u hebt.
1. Klik op het Azure Cosmos DB-account dat u zojuist hebt gemaakt, selecteer het tabblad **Overzicht** en scrol omlaag naar de kaart waarop de database zich bevindt. 

    ![Nieuw Azure Cosmos DB-account in de Azure Portal](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-angular-portal.png)

4. Scrol omlaag op de navigatiebalk aan de linkerkant en klik op het tabblad **Gegevens globaal repliceren**. Er wordt een kaart weergegeven waarop u de verschillende gebieden ziet waarin u kunt repliceren. U kunt bijvoorbeeld klikken op Australië - zuidoost of Australië - oost en uw gegevens repliceren naar Australië. Meer informatie over globale replicatie vindt u in [Gegevens globaal distribueren met Azure Cosmos DB](distribute-data-globally.md). Voorlopig behouden we dit exemplaar. Wanneer we willen repliceren, weten we hoe dit moet.

    ![Nieuw Azure Cosmos DB-account in Azure Portal](./media/tutorial-develop-mongodb-nodejs-part4/azure-cosmos-db-replicate-portal.png)

## <a name="next-steps"></a>Volgende stappen

In dit deel van de zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> * U hebt een Azure-resourcegroep gemaakt met de Azure CLI
> * U hebt een Azure Cosmos DB-account gemaakt met de Azure CLI

U kunt doorgaan met het volgende deel van de zelfstudie om Azure Cosmos DB te koppelen aan uw app met Mongoose.

> [!div class="nextstepaction"]
> [Mongoose gebruiken om verbinding te maken met Azure Cosmos DB](tutorial-develop-mongodb-nodejs-part5.md)
