---
title: bestand opnemen
description: bestand opnemen
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 09/14/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: a63937169097bc779dd285957420617f57ee76be
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47002228"
---
1. Meld u in een nieuw browservenster aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer **Een resource maken** > **Databases** > **Azure Cosmos DB**.
   
   ![Het deelvenster Databases in Azure Portal](./media/cosmos-db-create-dbaccount-cassandra/create-nosql-db-databases-json-tutorial-1.png)

3. Voer op de pagina **Nieuw account** de instellingen in voor het nieuwe Azure Cosmos DB-account. 
 
    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Id|*Voer een unieke naam in*|Voer een unieke naam in om dit Azure Cosmos DB-account te identificeren. Omdat *cassandra.cosmosdb.azure.com* is toegevoegd aan de id die u hebt opgegeven om het contactpunt te maken, gebruikt u een unieke, maar identificeerbare id.<br><br>De id mag alleen kleine letters, cijfers en het koppelteken (-) bevatten en moet 3 tot 50 tekens lang zijn.
    API|Cassandra|De API bepaalt het type te maken account. Azure Cosmos DB biedt vijf API's die aansluiten bij de behoeften van uw toepassing: SQL (documentdatabase), Gremlin (grafiekdatabase) MongoDB (documentdatabase), Azure Table en Cassandra. Momenteel is voor elke API een afzonderlijk account nodig. <br><br>Selecteer **Cassandra**, omdat u in deze snelstart een database met brede kolommen maakt waarop query's kunnen worden uitgevoerd met behulp van CQL-syntaxis.<br><br>[Meer informatie over de Cassandra-API](../articles/cosmos-db/cassandra-introduction.md).|
    Abonnement|*Uw abonnement*|Selecteer het Azure-abonnement dat u wilt gebruiken voor dit Azure Cosmos DB-account. 
    Resourcegroep|Nieuwe maken<br><br>*Geef dezelfde unieke naam op zoals hierboven bij id*|Selecteer **Nieuwe maken** en voer een naam voor de nieuwe resourcegroep voor uw account in. Gebruik dezelfde naam als uw id om het uzelf gemakkelijk te maken. 
    Locatie|*Selecteer de regio die het dichtst bij uw gebruikers is gelegen*|Selecteer de geografische locatie waar u het Azure Cosmos DB-account gaat hosten. Gebruik de locatie die zich het dichtst bij uw gebruikers bevindt, zodat ze de snelst mogelijke toegang tot de gegevens hebben.
    Vastmaken aan dashboard | Selecteer | Selecteer dit vakje, zodat uw nieuwe databaseaccount wordt toegevoegd aan uw portaldashboard voor eenvoudige toegang.

    Klik vervolgens op **Maken**.

    ![De pagina Nieuw account voor Azure Cosmos DB](./media/cosmos-db-create-dbaccount-cassandra/azure-cosmos-db-create-new-account.png)

4. Het duurt enkele minuten om het account te maken. Wacht tot in de portal de pagina **Gefeliciteerd! Uw Azure Cosmos DB-account is gemaakt** wordt weergegeven.

