---
title: Een Azure Cosmos DB MongoDB-API-account maken
description: In dit artikel wordt uitgelegd hoe u een Azure Cosmos DB MongoDB-API-account maakt in Azure Portal.
services: cosmos-db
author: mimig1
ms.service: cosmos-db
ms.topic: include
ms.date: 03/20/2018
ms.author: mimig
ms.custom: include file
ms.openlocfilehash: 02ea0e011642313b885bc48ec48104fa2789da81
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
1. Meld u in een nieuw venster aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links op **Een resource maken**, klik op **Databases** en klik vervolgens onder **Azure Cosmos DB** op **Maken**.
   
   ![Schermopname van Azure Portal waarbij Meer services en Azure Cosmos DB zijn gemarkeerd](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. Geef op de blade **Nieuw account** **MongoDB** op als de API en voer de gewenste configuratiegegevens in voor het Azure Cosmos DB-account.
 
    ![Schermopname van de blade Nieuwe Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-2.png)

    * **Id** moet een unieke naam zijn voor het Azure Cosmos DB-account. De id mag alleen kleine letters, cijfers en het minteken ('-') bevatten, en moet tussen de 3 en 50 tekens lang zijn.
    * Bij **Abonnement** wordt automatisch uw Azure-abonnement ingevuld.
    * **Resourcegroep** verwijst naar de naam van de resourcegroep voor uw Azure Cosmos DB-account.
    * **Locatie** is de geografische locatie van uw exemplaar van Azure Cosmos DB. Kies de locatie die het dichtst bij uw gebruikers is.

4. Klik op **Maken** om het account te maken.
5. Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.

    ![De melding Implementatie is gestart](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-nosql-notification.png)

6.  Als de implementatie is voltooid, opent u het nieuwe account via de tegel Alle resources. 

    ![Azure Cosmos DB-account op de tegel Alle resources](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-all-resources.png)
