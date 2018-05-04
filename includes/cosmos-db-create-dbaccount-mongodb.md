---
title: bestand opnemen
description: bestand opnemen
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 690c5f07a2b0f7a88e16f0b0bbbaa9ca78e37317
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
1. Meld u in een nieuw venster aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links op **Een resource maken**, klik op **Databases** en klik vervolgens onder **Azure Cosmos DB** op **Maken**.
   
   ![Schermopname van Azure Portal waarbij Meer services en Azure Cosmos DB zijn gemarkeerd](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. Geef op de blade **Nieuw account** **MongoDB** op als de API en voer de gewenste configuratiegegevens in voor het Azure Cosmos DB-account.
 
    * **Id** moet een unieke naam zijn voor het Azure Cosmos DB-account. De id mag alleen kleine letters, cijfers en het minteken ('-') bevatten, en moet tussen de 3 en 50 tekens lang zijn.
    * Bij **Abonnement** wordt automatisch uw Azure-abonnement ingevuld.
    * **Resourcegroep** verwijst naar de naam van de resourcegroep voor uw Azure Cosmos DB-account. Selecteer **Nieuwe maken** en voer een naam voor de nieuwe resourcegroep voor uw account in. Gebruik dezelfde naam als uw id om het uzelf gemakkelijk te maken.
    * **Locatie** is de geografische locatie van uw exemplaar van Azure Cosmos DB. Kies de locatie die het dichtst bij uw gebruikers is.

    Klik vervolgens op **Maken**.

    ![De pagina Nieuw account voor Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-create-new-account.png)

4. Het duurt enkele minuten om het account te maken. Wacht tot de portal de pagina **Gefeliciteerd! Uw Azure Cosmos DB-account met MongoDB API is gereed** weergeeft.

    ![Het deelvenster Meldingen in Azure Portal](./media/cosmos-db-create-dbaccount-mongodb/azure-cosmos-db-account-created.png)
