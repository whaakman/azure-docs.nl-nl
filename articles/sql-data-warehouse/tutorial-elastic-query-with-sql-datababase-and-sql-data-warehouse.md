---
title: 'Zelfstudie: Elastische Query met Azure SQL datawarehouse | Microsoft Docs'
description: Deze zelfstudie wordt de functie elastische Query op Azure SQL Data Warehouse-query uit Azure SQL Database.
services: sql-data-warehouse
author: hirokib
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/14/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: a31f035b5ec086a046028956c4a9c0de0d6a313d
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
ms.locfileid: "31526189"
---
# <a name="tutorial-use-elastic-query-to-access-data-in-azure-sql-data-warehouse-from-azure-sql-database"></a>Zelfstudie: Gebruik elastische Query voor toegang tot gegevens in Azure SQL Data Warehouse van Azure SQL Database

Deze zelfstudie wordt de functie elastische Query op Azure SQL Data Warehouse-query uit Azure SQL Database. 

## <a name="prerequisites-for-the-tutorial"></a>Vereisten voor de zelfstudie

Voordat u de zelfstudie begint, hebt u de volgende vereisten:

1. SQL Server Management Studio (SSMS) geïnstalleerd.
2. Een Azure SQL-server gemaakt met een database en het datawarehouse binnen deze server.
3. Firewallregels voor toegang tot de Azure SQL Server Setup.

## <a name="set-up-connection-between-sql-data-warehouse-and-sql-database-instances"></a>Verbinding tussen SQL Data Warehouse en SQL Database-exemplaren instellen 

1. Open een nieuwe query voor de database met SSMS of een andere query client **master** op uw logische server.

2. Maak een gebruikersnaam en een gebruiker die de SQL-database naar datawarehouse verbinding vertegenwoordigt.

   ```sql
   CREATE LOGIN SalesDBLogin WITH PASSWORD = 'aReallyStrongPassword!@#';
   ```

3. Met behulp van SSMS of een andere query-client, opent u een nieuwe query voor de **exemplaar van SQL datawarehouse** op uw logische server.

4. Maken van een gebruiker op het exemplaar van datawarehouse met de aanmelding die u hebt gemaakt in stap 2

   ```sql
   CREATE USER SalesDBUser FOR LOGIN SalesDBLogin;
   ```

5. Machtigingen verlenen aan de gebruiker uit stap 4 die u wilt de SQL-Database wilt uitvoeren. In dit voorbeeld is machtiging alleen worden verleend voor SELECT op een specifiek schema, ter illustratie van hoe kunnen we query's uit de SQL-database beperken tot een specifiek domein. 

   ```sql
   GRANT SELECT ON SCHEMA :: [dbo] TO SalesDBUser;
   ```

6. Met behulp van SSMS of een andere query-client, opent u een nieuwe query voor de **SQL database-instantie** op uw logische server.

7. Maak een hoofdsleutel als u nog geen een. 

   ```sql
   CREATE MASTER KEY; 
   ```

8. Maak een-scoped databasereferentie met de referenties die u in stap 2 hebt gemaakt.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL SalesDBElasticCredential
   WITH IDENTITY = 'SalesDBLogin',
   SECRET = 'aReallyStrongPassword@#!';
   ```

9. Maak een externe gegevensbron die naar het exemplaar van het datawarehouse verwijst.

   ```sql
   CREATE EXTERNAL DATA SOURCE EnterpriseDwSrc WITH 
       (TYPE = RDBMS, 
       LOCATION = '<SERVER NAME>.database.windows.net', 
       DATABASE_NAME = '<SQL DATA WAREHOUSE NAME>', 
       CREDENTIAL = SalesDBElasticCredential, 
   ) ;
   ```

10. U kunt nu externe tabellen die verwijzen naar deze externe gegevensbron maken. Query's op basis van die tabellen worden verzonden naar het exemplaar van datawarehouse moet worden verwerkt en verzonden naar de database-instantie.


## <a name="elastic-query-from-sql-database-to-sql-data-warehouse"></a>Elastische Query in SQL-database met SQL datawarehouse

We gaan in de volgende stappen een tabel maken in onze datawarehouse-exemplaar met verschillende waarden. Het instellen van een externe tabel query uitvoeren op het exemplaar van datawarehouse vanuit het database-exemplaar wordt vervolgens gedemonstreerd.

1. Met behulp van SSMS of een andere query-client, opent u een nieuwe query voor de **SQL Data Warehouse** op uw logische server.

2. Verzenden van de volgende query voor het maken van een **OrdersInformation** tabel in uw exemplaar van datawarehouse.

   ```sql
   CREATE TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL 
   ,   [CustomerID] [int] NOT NULL 
   ) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)
   ```

3. Met behulp van SSMS of een andere query-client, opent u een nieuwe query voor de **SQL-database** op uw logische server.

4. Verzenden van de volgende query voor het maken van de definitie van een externe tabel die naar verwijst de **OrdersInformation** tabel in het exemplaar van datawarehouse.

   ```sql
   CREATE EXTERNAL TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL
   ,   [CustomerID] [int] NOT NULL 
   ) 
   WITH 
   (
        DATA_SOURCE = EnterpriseDwSrc
   ,    SCHEMA_NAME = N'dbo'
   ,    OBJECT_NAME = N'OrderInformation'
   )
   ```

5. Houd rekening met dat u hebt nu een definitie van een externe tabel uw **SQL database-instantie**.

   ![de definitie van de externe tabel elastische query](media/sql-data-warehouse-elastic-query-with-sql-database/elastic-query-external-table.png)


6. De volgende query die een exemplaar van het datawarehouse een query indienen. U ontvangt de vijf waarden die u in stap 2 hebt geplaatst. 

```sql
SELECT * FROM [dbo].[OrderInformation];
```

> [!NOTE]
>
> U ziet dat ondanks enkele waarden, deze query neemt veel tijd om terug te keren. Wanneer u Elastische query bij het datawarehouse, moet een rekening houden met de overheadkosten van de verwerking van query's en het verkeer via de kabel. Gebruikmaken van externe uitvoering van de elastische query wanneer de rekencapaciteit, latentie en de prioriteit.

Gefeliciteerd, u de basisbeginselen van elastische Query hebt ingesteld. 

## <a name="next-steps"></a>Volgende stappen
Zie voor aanbevelingen [aanbevolen procedures voor het gebruik van elastische Query met Azure SQL Data Warehouse](how-to-use-elastic-query-with-sql-data-warehouse.md).