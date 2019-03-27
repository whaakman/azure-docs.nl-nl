---
title: "Zelfstudie: Elastische query's uitvoeren met Azure SQL datawarehouse | Microsoft Docs"
description: In deze zelfstudie wordt de functie elastische query's op Azure SQL Data Warehouse-query uit een Azure SQL Database.
services: sql-data-warehouse
author: hirokib
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/14/2018
ms.author: elbutter
ms.reviewer: igorstan
ms.openlocfilehash: 4dc0be045bceaaac4b71c653d82f7f9db834c3ec
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58486042"
---
# <a name="tutorial-use-elastic-query-to-access-data-in-azure-sql-data-warehouse-from-azure-sql-database"></a>Zelfstudie: Elastische query's voor toegang tot gegevens in Azure SQL Data Warehouse gebruik van Azure SQL Database

In deze zelfstudie wordt de functie elastische query's op Azure SQL Data Warehouse-query uit een Azure SQL Database. 

## <a name="prerequisites-for-the-tutorial"></a>Vereisten voor de zelfstudie

Voordat u de zelfstudie begint, moet u de volgende vereisten:

1. Installed SQL Server Management Studio (SSMS).
2. Een Azure SQL-server gemaakt met een database en het datawarehouse binnen deze server.
3. Firewallregels voor toegang tot de Azure SQL-Server instellen.

## <a name="set-up-connection-between-sql-data-warehouse-and-sql-database-instances"></a>Instellen van verbinding tussen SQL Data Warehouse en SQL Database-exemplaren 

1. Open een nieuwe query voor de database met behulp van SSMS of een andere queryclient **master** op uw logische server.

2. Maak een aanmelding en gebruiker die de SQL-database tot datawarehouse gegevensverbinding vertegenwoordigt.

   ```sql
   CREATE LOGIN SalesDBLogin WITH PASSWORD = 'aReallyStrongPassword!@#';
   ```

3. Met behulp van SSMS of een andere queryclient, opent u een nieuwe query voor de **exemplaar van SQL datawarehouse** op uw logische server.

4. Maken van een gebruiker op de datawarehouse-exemplaar met de aanmelding die u hebt gemaakt in stap 2

   ```sql
   CREATE USER SalesDBUser FOR LOGIN SalesDBLogin;
   ```

5. Machtigingen verlenen voor de gebruiker uit stap 4 die u wilt de SQL-Database wilt uitvoeren. In dit voorbeeld is toestemming alleen worden verleend voor selecteren op een specifieke schema, waarin u ziet hoe we query's uit de SQL-database kunnen beperken tot een specifiek domein. 

   ```sql
   GRANT SELECT ON SCHEMA :: [dbo] TO SalesDBUser;
   ```

6. Met behulp van SSMS of een andere queryclient, opent u een nieuwe query voor de **exemplaar van SQL database** op uw logische server.

7. Maak een hoofdsleutel als u nog geen een. 

   ```sql
   CREATE MASTER KEY; 
   ```

8. Maak een database-scoped referentie met de referenties die u in stap 2 hebt gemaakt.

   ```sql
   CREATE DATABASE SCOPED CREDENTIAL SalesDBElasticCredential
   WITH IDENTITY = 'SalesDBLogin',
   SECRET = 'aReallyStrongPassword@#!';
   ```

9. Maak een externe gegevensbron die naar de datawarehouse-exemplaar verwijst.

   ```sql
   CREATE EXTERNAL DATA SOURCE EnterpriseDwSrc WITH 
       (TYPE = RDBMS, 
       LOCATION = '<SERVER NAME>.database.windows.net', 
       DATABASE_NAME = '<SQL DATA WAREHOUSE NAME>', 
       CREDENTIAL = SalesDBElasticCredential, 
   ) ;
   ```

10. U kunt nu externe tabellen die verwijzen naar deze externe gegevensbron maken. Query's met behulp van deze tabellen worden verzonden naar de datawarehouse-exemplaar moet worden verwerkt en verzonden naar de database-instantie.


## <a name="elastic-query-from-sql-database-to-sql-data-warehouse"></a>Elastische Query in SQL database met SQL datawarehouse

We gaan in de volgende stappen een tabel maken in onze datawarehouse-exemplaar met verschillende waarden. Vervolgens worden toegelicht over het instellen van een externe tabel query uitvoeren op de datawarehouse-exemplaar uit de database-instantie.

1. Met behulp van SSMS of een andere queryclient, opent u een nieuwe query voor de **SQL Data Warehouse** op uw logische server.

2. Verzenden van de volgende query voor het maken van een **OrdersInformation** tabel in uw datawarehouse-exemplaar.

   ```sql
   CREATE TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL, 
       [CustomerID] [int] NOT NULL 
   ) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (123, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (149, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (857, 2) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (321, 1) 
   INSERT INTO [dbo].[OrderInformation] ([OrderID], [CustomerID]) VALUES (564, 8)
   ```

3. Met behulp van SSMS of een andere queryclient, opent u een nieuwe query voor de **SQL-database** op uw logische server.

4. Verzenden van de volgende query uit voor het maken van een definitie van de externe tabel die naar verwijst de **OrderInformation** tabel in de datawarehouse-exemplaar.

   ```sql
   CREATE EXTERNAL TABLE [dbo].[OrderInformation]
   ( 
       [OrderID] [int] NOT NULL,
       [CustomerID] [int] NOT NULL 
   ) 
   WITH 
   (
        DATA_SOURCE = EnterpriseDwSrc,
    SCHEMA_NAME = N'dbo',
    OBJECT_NAME = N'OrderInformation'
   )
   ```

5. Bekijk dat u hebt nu een definitie van de externe tabel uw **exemplaar van SQL database**.

   ![definitie van de externe tabel elastische query](media/sql-data-warehouse-elastic-query-with-sql-database/elastic-query-external-table.png)


6. Indienen van de volgende query die de datawarehouse-exemplaar een query uitgevoerd. U ontvangt de vijf waarden die u in stap 2 hebt geplaatst. 

```sql
SELECT * FROM [dbo].[OrderInformation];
```

> [!NOTE]
>
> U ziet dat ondanks enkele waarden, deze query kost veel tijd om terug te keren. Wanneer u een elastische query met DataWarehouse gebruikt, moet een rekening houden met de overheadkosten van de verwerking van query's en het verkeer via de kabel. Externe uitvoering van de elastische query gebruiken wanneer de compute-kracht, latentie, de prioriteit wordt.

Gefeliciteerd, u de basisbeginselen van elastische Query hebt ingesteld. 

## <a name="next-steps"></a>Volgende stappen
Zie voor aanbevelingen [aanbevolen procedures voor het gebruik van elastische query's met Azure SQL Data Warehouse](how-to-use-elastic-query-with-sql-data-warehouse.md).
