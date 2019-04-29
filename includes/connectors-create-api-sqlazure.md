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
ms.openlocfilehash: da03c5247b8ebe0a3305b08a05d661264497663f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60540965"
---
* Als u Azure SQL Database gebruikt, volgt u de stappen onder [verbinding maken met Azure SQL Database](#connect-azure-sql-db). 

* Als u van SQL Server gebruikmaakt, volgt u de stappen onder [verbinding maken met SQL Server](#connect-sql-server).

<a name="connect-azure-sql-db"></a>

### <a name="connect-to-azure-sql-database"></a>Verbinding maken met Azure SQL Database

1. Wanneer de SQL-trigger of actie wordt u gevraagd om de verbindingsgegevens, als volgt te werk:

   1. Maak een naam voor de verbinding.

   2. Selecteer uw SQL-server en selecteer vervolgens uw database. 

      De databaselijst wordt weergegeven nadat u uw SQL-server selecteren.
 
   3. Geef uw gebruikersnaam en wachtwoord voor uw server.

      U kunt deze informatie vinden in de Azure-portal onder de eigenschappen van uw SQL-database of in de verbindingsreeks: 
      
      "Gebruikers-ID = <*uwgebruikersnaam*>"
      <br>
      "Wachtwoord = <*yourPassword*>"

   In dit voorbeeld ziet u de verbindingsgegevens voor een trigger, maar deze stappen te werken voor acties.

   ![Azure SQL Database-verbinding maken](./media/connectors-create-api-sqlazure/azure-sql-database-create-connection.png)
   <br>
   Sterretjes (*) geven de vereiste waarden.

   | Eigenschap | Value | Details | 
   |----------|-------|---------| 
   | Verbindingsnaam | <*my-sql-connection*> | De naam voor de verbinding | 
   | SQL Server-naam | <*my-sql-server*> | De naam voor uw SQL-server |
   | SQL-databasenaam | <*my-sql-database*>  | De naam voor uw SQL-database | 
   | Gebruikersnaam | <*my-sql-username*> | De naam van de gebruiker voor toegang tot uw database |
   | Wachtwoord | <*my-sql-password*> | Het wachtwoord voor toegang tot uw database | 
   |||| 

2. Wanneer u klaar bent, kiest u **Maken**.

3. Nadat u uw verbinding maakt, doorgaan met [SQL toevoegen trigger](#add-sql-trigger) of [SQL toevoegen actie](#add-sql-action).

<a name="connect-sql-server"></a>

### <a name="connect-to-sql-server"></a>Verbinding maken met SQL Server

Voordat u uw gateway selecteren kunt, zorg ervoor dat u al [instellen van uw data gateway](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection). Op die manier uw gateway weergegeven in de lijst met gateways bij het maken van de verbinding.

1. Wanneer de SQL-trigger of actie wordt u gevraagd om de verbindingsgegevens, als volgt te werk:

   1. Selecteer in de trigger of actie **verbinding maken via een on-premises gegevensgateway** zodat de SQL server-opties worden weergegeven.

   2. Maak een naam voor de verbinding.

   3. Het adres voor uw SQL-server opgeven, en geef vervolgens de naam voor uw database.
   
      U kunt deze informatie vinden in de verbindingsreeks: 
      
      * "Server = <*yourServerAddress*>"
      * "Database = <*yourDatabaseName*>"

   4. Geef uw gebruikersnaam en wachtwoord voor uw server.

      U kunt deze informatie vinden in de verbindingsreeks: 
      
      * "Gebruikers-ID = <*uwgebruikersnaam*>"
      * "Wachtwoord = <*yourPassword*>"

   5. Als uw SQL-server gebruikmaakt van Windows of basisverificatie, selecteert u het verificatietype.

   6. Selecteer de naam voor uw on-premises data gateway die u eerder hebt gemaakt.
   
      Als uw gateway niet wordt weergegeven in de lijst, controleert u dat u goed [instellen van uw gateway](https://docs.microsoft.com/azure/logic-apps/logic-apps-gateway-connection).

   In dit voorbeeld ziet u de verbindingsgegevens voor een trigger, maar deze stappen te werken voor acties.

   ![SQL Server-verbinding maken](./media/connectors-create-api-sqlazure/sql-server-create-connection.png)
   <br>
   Sterretjes (*) geven de vereiste waarden.

   | Eigenschap | Value | Details | 
   |----------|-------|---------| 
   | Verbinding maken via on-premises gateway | Selecteer deze optie eerst voor SQL Server-instellingen. | | 
   | Verbindingsnaam | <*my-sql-connection*> | De naam voor de verbinding | 
   | SQL Server-naam | <*my-sql-server*> | De naam voor uw SQL-server |
   | SQL-databasenaam | <*my-sql-database*>  | De naam voor uw SQL-database |
   | Gebruikersnaam | <*my-sql-username*> | De naam van de gebruiker voor toegang tot uw database |
   | Wachtwoord | <*my-sql-password*> | Het wachtwoord voor toegang tot uw database | 
   | Authenticatietype | Windows- of Basic | Optioneel: Het verificatietype dat wordt gebruikt door uw SQL-server | 
   | Gateways | <*my-data-gateway*> | De naam van uw on-premises gegevensgateway | 
   |||| 

2. Wanneer u klaar bent, kiest u **Maken**. 

3. Nadat u uw verbinding maakt, doorgaan met [SQL toevoegen trigger](#add-sql-trigger) of [SQL toevoegen actie](#add-sql-action).
