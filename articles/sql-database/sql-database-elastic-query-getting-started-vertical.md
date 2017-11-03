---
title: Aan de slag met meerdere databases query's (verticale partities) | Microsoft Docs
description: query voor elastische database gebruiken met verticaal gepartitioneerde databases
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: e5b44b10-c432-4f96-b20e-08615ff4d5dd
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: torsteng
ms.openlocfilehash: ca715c0ef41ccb4aed2924898d7729bed659b0ca
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Aan de slag met meerdere databases query's (verticale partities) (preview)
Elastische database kunt (preview) voor Azure SQL Database u query T-SQL-query's die meerdere databases met behulp van één verbindingspunt omvatten uitvoeren. Dit onderwerp geldt voor [verticaal gepartitioneerd databases](sql-database-elastic-query-vertical-partitioning.md).  

Wanneer voltooid, wordt u: informatie over het configureren en gebruiken van een Azure SQL Database voor het uitvoeren van query's die meerdere verwante databases omvatten. 

Zie voor meer informatie over de functie van de query elastische database [overzicht van Azure SQL Database elastische database query](sql-database-elastic-query-overview.md). 

## <a name="prerequisites"></a>Vereisten

U moet beschikken over een externe gegevensbron ALTER machtiging. Deze machtiging is opgenomen in de machtiging ALTER DATABASE. EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar de onderliggende gegevensbron.

## <a name="create-the-sample-databases"></a>De voorbeelddatabases maken
We moeten worden gestart met maken van twee databases **klanten** en **Orders**, zich in dezelfde of verschillende logische servers.   

De volgende query's uitvoeren op de **Orders** database maken van de **OrderInformation** tabel en voer de voorbeeldgegevens. 

    CREATE TABLE [dbo].[OrderInformation]( 
        [OrderID] [int] NOT NULL, 
        [CustomerID] [int] NOT NULL 
        ) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8) 

Nu volgen query niet uitvoeren op de **klanten** database maken van de **CustomerInformation** tabel en voer de voorbeeldgegevens. 

    CREATE TABLE [dbo].[CustomerInformation]( 
        [CustomerID] [int] NOT NULL, 
        [CustomerName] [varchar](50) NULL, 
        [Company] [varchar](50) NULL 
        CONSTRAINT [CustID] PRIMARY KEY CLUSTERED ([CustomerID] ASC) 
    ) 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (1, 'Jack', 'ABC') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (2, 'Steve', 'XYZ') 
    INSERT INTO [dbo].[CustomerInformation] ([CustomerID], [CustomerName], [Company]) VALUES (3, 'Lylla', 'MNO') 

## <a name="create-database-objects"></a>Database-objecten maken
### <a name="database-scoped-master-key-and-credentials"></a>Database-scoped hoofdsleutel en referenties
1. Open SQL Server Management Studio of SQL Server Data Tools in Visual Studio.
2. Verbinding maken met de database Orders en voer de volgende T-SQL-opdrachten:
   
        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>'; 
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred 
        WITH IDENTITY = '<username>', 
        SECRET = '<password>';  
   
    De 'gebruikersnaam' en 'password' moeten de gebruikersnaam en het wachtwoord dat wordt gebruikt om aan te melden in de database van klanten.
    Verificatie met behulp van Azure Active Directory met elastische query's is momenteel niet ondersteund.

### <a name="external-data-sources"></a>Externe gegevensbronnen
Voer de volgende opdracht op de database Orders voor het maken van een externe gegevensbron: 

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH 
        (TYPE = RDBMS, 
        LOCATION = '<server_name>.database.windows.net', 
        DATABASE_NAME = 'Customers', 
        CREDENTIAL = ElasticDBQueryCred, 
    ) ;

### <a name="external-tables"></a>Externe tabellen
Een externe tabel maken op de database Orders, die overeenkomt met de definitie van de tabel CustomerInformation:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation] 
    ( [CustomerID] [int] NOT NULL, 
      [CustomerName] [varchar](50) NOT NULL, 
      [Company] [varchar](50) NOT NULL) 
    WITH 
    ( DATA_SOURCE = MyElasticDBQueryDataSrc) 

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Een voorbeeld elastische database T-SQL-query uitvoeren
Als u de externe gegevensbron en uw externe tabellen die u kunt nu TSQL-query uitvoeren op uw externe tabellen hebt gedefinieerd. Voer deze query uit op de database Orders: 

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company 
    FROM OrderInformation 
    INNER JOIN CustomerInformation 
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID 

## <a name="cost"></a>Kosten
Op dit moment wordt is de functie van de query elastische database opgenomen in de kosten van uw Azure SQL Database.  

Zie voor informatie over prijzen [prijzen SQL Database](https://azure.microsoft.com/pricing/details/sql-database). 

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van elastische query [elastische query overzicht](sql-database-elastic-query-overview.md).
* Zie voor de syntaxis en voorbeelden query's voor verticaal gepartitioneerde gegevens [gegevens opvragen verticaal gepartitioneerd)](sql-database-elastic-query-vertical-partitioning.md)
* Zie voor een zelfstudie horizontaal partitioneren (sharding) [aan de slag met elastische query voor horizontale partitioneren (sharding)](sql-database-elastic-query-getting-started.md).
* Zie voor de syntaxis en voorbeelden query's voor horizontaal gepartitioneerde gegevens [gegevens opvragen horizontaal gepartitioneerd)](sql-database-elastic-query-horizontal-partitioning.md)
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een enkele externe Azure SQL Database of een set van databases die fungeren als shards in een horizontale partitieschema wordt uitgevoerd.
