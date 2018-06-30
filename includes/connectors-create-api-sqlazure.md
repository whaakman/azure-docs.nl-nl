---
title: bestand opnemen
description: bestand opnemen
services: logic-apps
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 05/15/2018
ms.author: estfan
ms.custom: include file
ms.openlocfilehash: 4ffda692da0ab7b63f7376c36dfab0bec914e334
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37138062"
---
* Als u Azure SQL Database gebruikt, volg de stappen onder [verbinding maken met Azure SQL Database](#connect-azure-sql-db). 

* Als u SQL Server gebruikt, volg de stappen onder [verbinding maken met SQL Server](#connect-sql-server).

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Verbinding maken met Azure SQL Database

1. Wanneer de SQL-trigger of actie wordt u gevraagd om de verbindingsgegevens, als volgt te werk:

   1. Maak een naam voor de verbinding.

   2. Selecteer uw SQL-server en selecteer vervolgens de database. 

      De lijst van de database wordt pas weergegeven als u uw SQL server selecteren.
 
   3. Geef uw gebruikersnaam en wachtwoord voor uw server.

      U vindt deze informatie in de Azure-portal onder de eigenschappen van uw SQL-database of in de verbindingsreeks: 
      
      "Gebruikers-ID = <*uwgebruikersnaam*>"
      <br>
      "Wachtwoord = <*yourPassword*>"

   Dit voorbeeld ziet u de verbindingsgegevens voor een trigger, maar deze stappen te werken voor acties.

   ![Maak verbinding met Azure SQL Database](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)
   <br>
   Sterretjes (*) geven de vereiste waarden.

   | Eigenschap | Waarde | Details | 
   |----------|-------|---------| 
   | Verbindingsnaam | <*Mijn sql-verbinding*> | De naam voor de verbinding | 
   | SQL Server-naam | <*Mijn sql-server*> | De naam voor uw SQL-server |
   | SQL-databasenaam | <*Mijn sql-database*>  | De naam voor de SQL-database | 
   | Gebruikersnaam | <*Mijn sql-gebruikersnaam*> | De gebruikersnaam op voor toegang tot uw database |
   | Wachtwoord | <*Mijn sql-wachtwoord*> | Het wachtwoord voor toegang tot uw database | 
   |||| 

2. Wanneer u klaar bent, kiest u **Maken**.

3. Nadat u uw verbinding maakt, doorgaan met [trigger toevoegen SQL](#add-sql-trigger) of [toevoegen SQL actie](#add-sql-action).

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>Verbinding maken met SQL Server

Voordat u uw gateway selecteren kunt, zorg ervoor dat u al [instellen van uw data gateway](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection). Op die manier uw gateway wordt weergegeven in de lijst gateways wanneer u uw verbinding maken.

1. Wanneer de SQL-trigger of actie wordt u gevraagd om de verbindingsgegevens, als volgt te werk:

   1. Selecteer in de trigger of actie **verbinden via lokale gegevensgateway** dat de SQL server-opties worden weergegeven.

   2. Maak een naam voor de verbinding.

   3. Geef het adres voor uw SQL-server en geef vervolgens de naam voor uw database.
   
      U kunt deze informatie vinden in de verbindingsreeks: 
      
      * "Server = <*yourServerAddress*>"
      * "Database = <*yourDatabaseName*>"

   4. Geef uw gebruikersnaam en wachtwoord voor uw server.

      U kunt deze informatie vinden in de verbindingsreeks: 
      
      * "Gebruikers-ID = <*uwgebruikersnaam*>"
      * "Wachtwoord = <*yourPassword*>"

   5. Als uw SQL server gebruikmaakt van Windows of basisverificatie, selecteert u het verificatietype.

   6. Selecteer de naam voor uw on-premises gegevens-gateway die u eerder hebt gemaakt.
   
      Als uw gateway niet wordt weergegeven in de lijst, controleert u dat u goed [instellen van uw gateway](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection).

   Dit voorbeeld ziet u de verbindingsgegevens voor een trigger, maar deze stappen te werken voor acties.

   ![SQL Server-verbinding maken](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)
   <br>
   Sterretjes (*) geven de vereiste waarden.

   | Eigenschap | Waarde | Details | 
   |----------|-------|---------| 
   | Verbinding maken via de lokale gateway | Selecteer deze optie eerst voor SQL Server-instellingen. | | 
   | Verbindingsnaam | <*Mijn sql-verbinding*> | De naam voor de verbinding | 
   | SQL Server-naam | <*Mijn sql-server*> | De naam voor uw SQL-server |
   | SQL-databasenaam | <*Mijn sql-database*>  | De naam voor de SQL-database |
   | Gebruikersnaam | <*Mijn sql-gebruikersnaam*> | De gebruikersnaam op voor toegang tot uw database |
   | Wachtwoord | <*Mijn sql-wachtwoord*> | Het wachtwoord voor toegang tot uw database | 
   | Authenticatietype | Windows- of Basic | Optioneel: Het verificatietype dat wordt gebruikt door uw SQL-server | 
   | Gateways | <*Mijn-gegevensgateway*> | De naam voor uw on-premises gegevensgateway | 
   |||| 

2. Wanneer u klaar bent, kiest u **Maken**. 

3. Nadat u uw verbinding maakt, doorgaan met [trigger toevoegen SQL](#add-sql-trigger) of [toevoegen SQL actie](#add-sql-action).
