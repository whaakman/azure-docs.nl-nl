---
title: bestand opnemen
description: bestand opnemen
services: cosmos-db
author: rimman
ms.service: cosmos-db
ms.topic: include
ms.date: 04/08/2019
ms.author: rimman
ms.custom: include file
ms.openlocfilehash: 5d57d7e18befba175a5a8a825494ce512644b5a2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131773"
---
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
1. Selecteer **Een resource maken** > **Databases** > **Azure Cosmos DB**.
   
   ![Het deelvenster Databases in Azure Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

1. Op de **Create Azure Cosmos DB Account** pagina, de basisinstellingen voor de nieuwe Azure Cosmos-account invoeren. 
 
    |Instelling|Waarde|Description |
    |---|---|---|
    |Abonnement|Abonnementsnaam|Selecteer het Azure-abonnement dat u wilt gebruiken voor dit Azure Cosmos-account. |
    |Resourcegroep|Resourcegroepnaam|Selecteer een resourcegroep of selecteer **nieuw**, voer een unieke naam voor de nieuwe resourcegroep. |
    | Accountnaam|Voer een unieke naam in|Voer een unieke naam voor uw Azure Cosmos-account. Gebruik een unieke id omdat *documents.azure.com* is toegevoegd aan de id die u hebt opgegeven om uw URI te maken.<br><br>De id mag alleen kleine letters, cijfers en het koppelteken (-) bevatten. Het moet tussen 3-31 tekens lang zijn.|
    | API|Core (SQL)|De API bepaalt het type te maken account. Azure Cosmos DB heeft vijf API's: Core (SQL) en MongoDB voor documentgegevens, Gremlin voor grafiekgegevens, Azure Table en Cassandra. Op dit moment moet u voor elke API een afzonderlijk account maken. <br><br>Selecteer **Core (SQL)** een documentdatabase en een query maken met behulp van SQL-syntaxis. <br><br>[Meer informatie over de SQL-API](../articles/cosmos-db/documentdb-introduction.md).|
    | Locatie|Selecteer de regio het dichtst in de buurt van uw gebruikers|Selecteer een geografische locatie waar u het Azure Cosmos DB-account wilt hosten. Gebruik de locatie die zich het dichtst bij uw gebruikers zodat ze de snelst mogelijke toegang tot de gegevens.|
   
   ![De pagina Nieuw account voor Azure Cosmos DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account.png)

1. Selecteer **Controleren + maken**. U kunt overslaan de **netwerk** en **Tags** secties. 

1. Controleer de accountinstellingen en selecteer vervolgens **maken**. Het duurt een paar minuten om het account te maken. Wacht tot de portal-pagina om weer te geven **uw implementatie is voltooid**. 

    ![Het deelvenster Meldingen in Azure Portal](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

1. Selecteer **naar de resource gaan** om naar de Azure Cosmos DB-account-pagina te gaan. 

    ![De pagina voor Azure Cosmos DB-account](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created-2.png)
