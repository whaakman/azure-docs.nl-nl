---
title: Aan de slag met query's die databaseoverschrijdend (verticale partitionering) | Microsoft Docs
description: query van elastische database gebruiken met verticaal gepartitioneerde databases
services: sql-database
ms.service: sql-database
subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 07/03/2018
ms.openlocfilehash: d78767f9360c8cb3a304af19f619dd9f74d3fb8e
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162981"
---
# <a name="get-started-with-cross-database-queries-vertical-partitioning-preview"></a>Aan de slag met query's die databaseoverschrijdend (verticale partitionering) (preview)

Query van elastische database (preview) voor Azure SQL Database kunt u T-SQL-query's met betrekking tot meerdere databases met behulp van een enkele verbindingspunt uitvoeren. In dit artikel is van toepassing op [verticaal gepartitioneerd databases](sql-database-elastic-query-vertical-partitioning.md).  

Wanneer dit is voltooid, wordt u: meer informatie over het configureren en gebruiken van een Azure SQL Database-query's met betrekking meerdere verwante databases tot uitvoert.

Zie voor meer informatie over de functie elastic database-query, [overzicht van Azure SQL Database elastic database query](sql-database-elastic-query-overview.md).

## <a name="prerequisites"></a>Vereisten

EEN externe gegevensbron ALTER machtiging is vereist. Deze machtiging is opgenomen in de machtiging ALTER DATABASE. EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar de onderliggende gegevensbron.

## <a name="create-the-sample-databases"></a>De voorbeelddatabases maken

Te beginnen met maken van twee databases, **klanten** en **Orders**, in de dezelfde of verschillende logische servers.

De volgende query's uitvoeren op de **Orders** database om te maken de **OrderInformation** tabel en voer de voorbeeldgegevens.

    CREATE TABLE [dbo].[OrderInformation](
        [OrderID] [int] NOT NULL,
        [CustomerID] [int] NOT NULL
        )
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1)
    INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)

Nu de volgende query worden uitgevoerd op de **klanten** database om te maken de **CustomerInformation** tabel en voer de voorbeeldgegevens.

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
2. Verbinding maken met de bestellingsdatabase en voer de volgende T-SQL-opdrachten uit:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<master_key_password>';
        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';  

    De 'gebruikersnaam' en 'wachtwoord' moet de gebruikersnaam en het wachtwoord dat wordt gebruikt aan te melden bij de klanten-database.
    Verificatie met behulp van Azure Active Directory met elastische query's is momenteel niet ondersteund.

### <a name="external-data-sources"></a>Externe gegevensbronnen

Voor het maken van een externe gegevensbron dan de volgende opdracht in de bestellingsdatabase:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
        (TYPE = RDBMS,
        LOCATION = '<server_name>.database.windows.net',
        DATABASE_NAME = 'Customers',
        CREDENTIAL = ElasticDBQueryCred,
    ) ;

### <a name="external-tables"></a>Externe tabellen

Maak een externe tabel in de bestellingsdatabase die overeenkomt met de definitie van de tabel CustomerInformation:

    CREATE EXTERNAL TABLE [dbo].[CustomerInformation]
    ( [CustomerID] [int] NOT NULL,
      [CustomerName] [varchar](50) NOT NULL,
      [Company] [varchar](50) NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc)

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Een voorbeeld elastische database T-SQL-query uitvoeren

Nadat u uw externe gegevensbron en uw externe tabellen hebt gedefinieerd, kunt u nu T-SQL gebruiken om op te vragen van uw externe tabellen. Voer deze query uit op de bestellingsdatabase:

    SELECT OrderInformation.CustomerID, OrderInformation.OrderId, CustomerInformation.CustomerName, CustomerInformation.Company
    FROM OrderInformation
    INNER JOIN CustomerInformation
    ON CustomerInformation.CustomerID = OrderInformation.CustomerID

## <a name="cost"></a>Kosten

Op dit moment is de functie elastic database-query opgenomen in de kosten van uw Azure SQL Database.  

Zie voor informatie over de prijzen [prijzen van SQL Database](https://azure.microsoft.com/pricing/details/sql-database).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van elastische query [elastische query overzicht](sql-database-elastic-query-overview.md).
* Zie voor een voorbeeld en de syntaxis van query's voor verticaal gepartitioneerde gegevens [opvragen van verticaal gepartitioneerde gegevens)](sql-database-elastic-query-vertical-partitioning.md)
* Zie voor een zelfstudie horizontale partitionering (sharding) [aan de slag met elastische query's voor horizontale partitionering (sharding)](sql-database-elastic-query-getting-started.md).
* Zie voor een voorbeeld en de syntaxis van query's voor horizontaal gepartitioneerde gegevens [gegevens opvragen horizontaal gepartitioneerd)](sql-database-elastic-query-horizontal-partitioning.md)
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een één externe SQL-Database van Azure of een set van databases die fungeren als shards in een horizontale partitionering wordt uitgevoerd.
