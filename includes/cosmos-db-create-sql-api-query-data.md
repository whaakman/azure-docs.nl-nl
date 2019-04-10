---
title: bestand opnemen
description: bestand opnemen
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/05/2019
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 9971b16da42cdf1de0464857291c74a947535735
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59286994"
---
U kunt query's in Data Explorer gebruiken om te halen en te filteren van uw gegevens.

1. Aan de bovenkant van de **documenten** tabblad in Data Explorer, controleert u de standaardquery `SELECT * FROM c`. Deze query worden opgehaald en worden alle documenten in de verzameling in volgorde van de ID wordt weergegeven. 
   
   ![De standaardquery in Data Explorer is 'SELECT * FROM c'](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-query.png)
   
1. Als de query wijzigen, selecteert u **Filter bewerken**, vervangt u de standaard-query met `ORDER BY c._ts DESC`, en selecteer vervolgens **Filter toepassen**.
   
   ![Wijzig de standaardquery door ORDER BY c._ts DESC toe te voegen en te klikken op Filter toepassen](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edit-query.png)

   De gewijzigde query worden weergegeven de documenten in aflopende volgorde op basis van hun tijdstempel, dus nu uw tweede document wordt eerst weergegeven. 
   
   ![ORDER BY c._ts DESC query en klikken op Filter toepassen gewijzigd](./media/cosmos-db-create-sql-api-query-data/azure-cosmosdb-data-explorer-edited-query.png)

Als u bekend met SQL-syntaxis bent, kunt u een ondersteund [SQL-query's](../articles/cosmos-db/sql-api-sql-query.md) in het vak querypredicaat. U kunt Data Explorer ook gebruiken voor het maken van opgeslagen procedures, UDF's en triggers voor bedrijfslogica op de server. 

Data Explorer biedt eenvoudige Azure portal toegang tot alle van de gegevens met ingebouwde programmatische toegang tot functies die beschikbaar zijn in de API's. U kunt ook de portal gebruiken doorvoer kunt schalen, sleutels en verbindingsreeksen en bekijk metrische gegevens en sla's voor uw Azure Cosmos DB-account. 

