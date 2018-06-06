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
ms.openlocfilehash: 013e230aa7e096f6a90ed7cf9e93a44fbdeb3bd6
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34678208"
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

   1. Selecteer in de trigger of actie **verbinden via on-premises gegevensgateway** dat de SQL server-opties worden weergegeven.

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
   | Verbinding maken via de on-premises gateway | Selecteer deze optie eerst voor SQL Server-instellingen. | | 
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
