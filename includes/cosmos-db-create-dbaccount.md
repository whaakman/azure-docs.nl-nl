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
ms.openlocfilehash: f8ef3070c318293af59a8f22d271b9c836c06f69
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
1. Meld u in een nieuw browservenster aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **Een resource maken** > **Databases** > **Azure Cosmos DB**.
   
   ![Het deelvenster Databases in Azure Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. Voer op de pagina **Nieuw account** de instellingen in voor het nieuwe Azure Cosmos DB-account. 
 
    Instelling|Waarde|Beschrijving
    ---|---|---
    Id|*Voer een unieke naam in*|Voer een unieke naam in om dit Azure Cosmos DB-account te identificeren. Omdat *documents.azure.com* is toegevoegd aan de id die u hebt opgegeven om uw URI te maken, gebruikt u een unieke maar identificeerbare id.<br><br>De id mag alleen kleine letters, cijfers en het koppelteken (-) bevatten en moet 3 tot 50 tekens lang zijn.
    API|SQL|De API bepaalt het type te maken account. Azure Cosmos DB biedt vijf API's die aansluiten bij de behoeften van uw toepassing: SQL (documentdatabase), Gremlin (grafiekdatabase) MongoDB (documentdatabase), Azure Table en Cassandra. Momenteel is voor elke API een afzonderlijk account nodig. <br><br>Selecteer **SQL** omdat deze snelstartgids u maakt in een documentdatabase waarop met behulp van SQL-syntaxis en toegankelijk is met de SQL-API.<br><br>[Meer informatie over de SQL-API](../articles/cosmos-db/documentdb-introduction.md)|
    Abonnement|*Uw abonnement*|Selecteer het Azure-abonnement dat u wilt gebruiken voor dit Azure Cosmos DB-account. 
    Resourcegroep|Nieuw item maken<br><br>*Voer vervolgens de dezelfde unieke naam zoals hierboven omschreven in-ID*|Selecteer **nieuw**, voert u een nieuwe naam resourcegroep voor uw account. Gebruik dezelfde naam als uw id om het uzelf gemakkelijk te maken. 
    Locatie|*Selecteer de regio die het dichtst bij uw gebruikers is gelegen*|Selecteer de geografische locatie waar u het Azure Cosmos DB-account gaat hosten. Gebruik de locatie die zich het dichtst bij uw gebruikers bevindt, zodat ze de snelst mogelijke toegang tot de gegevens hebben.
    Georedundantie inschakelen| Leeg laten | Hiermee maakt u een gerepliceerde versie van uw database in een tweede (gekoppelde) regio. Laat dit leeg.  
    Vastmaken aan dashboard | Selecteer | Selecteer dit vakje, zodat uw nieuwe databaseaccount wordt toegevoegd aan uw portaldashboard voor eenvoudige toegang.

    Klik vervolgens op **Maken**.

    ![De nieuwe pagina voor het account voor Azure Cosmos-DB](./media/cosmos-db-create-dbaccount/azure-cosmos-db-create-new-account.png)

4. Het duurt enkele minuten om het account te maken. Wacht tot de portal om weer te geven de **Gefeliciteerd! Uw Azure DB die Cosmos-account is gemaakt** pagina.

    ![Het deelvenster Meldingen in Azure Portal](./media/cosmos-db-create-dbaccount/azure-cosmos-db-account-created.png)

