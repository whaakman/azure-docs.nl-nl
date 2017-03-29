---
title: 'SSMS: verbinding maken met Azure SQL Database en query&quot;s uitvoeren voor gegevens | Microsoft Docs'
description: Ontdek hoe u verbinding maakt met SQL Database in Azure met behulp van SQL Server Management Studio (SSMS). Voer daarna Transact-SQL-instructies (T-SQL) uit om query&quot;s uit te voeren voor gegevens en om gegevens te bewerken.
metacanonical: 
keywords: verbinding maken met SQL-database,Sql Server Management Studio
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.custom: development
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/15/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: ba56eaa154116edbe1dd8962049535cfa57551ac
ms.lasthandoff: 03/25/2017


---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a>Azure SQL Database: SQL Server Management Studio gebruiken om verbinding te maken en query's uit te voeren voor gegevens

Gebruik [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) voor het maken en beheren van SQL Server-resources via de gebruikersinterface of in scripts. In deze handleiding wordt informatie geboden over het gebruik van SSMS om verbinding te maken met een Azure SQL-database, over het uitvoeren van query's en over het invoegen, bijwerken en verwijderen van instructies.

In deze Quick Start wordt dit gebruikt als basis voor het maken van de resources die u hebt gemaakt in een van deze Quick Starts:

- [Database maken - Portal](sql-database-get-started-portal.md)
- [Database maken - CLI](sql-database-get-started-cli.md)

Voordat u begint, zorgt u ervoor dat u de nieuwste versie van [SSMS](https://msdn.microsoft.com/library/mt238290.aspx) hebt geïnstalleerd. 

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen

Haal de volledig gekwalificeerde servernaam van uw Azure SQL Database-server op uit Azure Portal. U gebuikt de volledig gekwalificeerde servernaam om verbinding met uw server te maken via SQL Server Management Studio.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. In het deelvenster **Essentials** van de Azure Portal-pagina van uw database kopieert u de **servernaam**.

    <img src="./media/sql-database-connect-query-ssms/connection-information.png" alt="connection information" style="width: 780px;" />

## <a name="connect-to-the-server"></a>Verbinding maken met de server

Gebruik SQL Server Management Studio om verbinding te maken met uw Azure SQL Database-server.

1. Voer **SSMS** in in het Windows-zoekvak en klik op **Enter** om SSMS te openen.

2. Voer in het dialoogvenster **Verbinding maken met server** de volgende informatie in:
   - **Servertype**: geef de database-engine op
   - **Servernaam**: voer de volledig gekwalificeerde servernaam in, zoals **mynewserver20170313.database.windows.net**
   - **Verificatie**: geef de SQL Server-verificatie op
   - **Aanmelding**: gebruik het beheerdersaccount voor de server
   - **Wachtwoord**: voer het wachtwoord in voor het beheerdersaccount voor de server
 
    <img src="./media/sql-database-connect-query-ssms/connect.png" alt="connect to server" style="width: 780px;" />

3. Klik op **Verbinden**. Het venster Objectverkenner wordt geopend in SSMS. 

    <img src="./media/sql-database-connect-query-ssms/connected.png" alt="connected to server" style="width: 780px;" />

4. In Objectverkenner vouwt u **Databases** en daarna **mySampleDatabase** uit om de objecten in de voorbeelddatabase weer te geven.

## <a name="query-data"></a>Querygegevens

Gebruik de Transact-SQL-instructie [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) om in uw Azure SQL-database query's uit te voeren voor gegevens.

1. Klik in Objectverkenner met de rechtermuisknop op **mySampleDatabase** en klik vervolgens op **Nieuwe query**. Er wordt een leeg queryvenster geopend dat is verbonden met uw database.
2. Voer in het queryvenster de volgende query in:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Klik in de werkbalk op **Uitvoeren** om gegevens op te halen uit de tabellen Product en ProductCategory.

    <img src="./media/sql-database-connect-query-ssms/query.png" alt="query" style="width: 780px;" />

## <a name="insert-data"></a>Gegevens invoegen

Gebruik de Transact-SQL-instructie [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) om in uw Azure SQL-database gegevens in te voegen.

1. Klik op de werkbalk op **Nieuwe query**. Er wordt een leeg queryvenster geopend dat is verbonden met uw database.
2. Voer in het queryvenster de volgende query in:

   ```sql
   INSERT INTO [SalesLT].[Product]
           ( [Name]
           , [ProductNumber]
           , [Color]
           , [ProductCategoryID]
           , [StandardCost]
           , [ListPrice]
           , [SellStartDate]
           )
     VALUES
           ('myNewProduct'
           ,123456789
           ,'NewColor'
           ,1
           ,100
           ,100
           ,GETDATE() );
   ```

3. Klik in de werkbalk op **Uitvoeren** om een nieuwe rij in te voegen in de tabel Product.

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a>Gegevens bijwerken

Gebruik de Transact-SQL-instructie [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) om in uw Azure SQL-database gegevens bij te werken.

1. Klik op de werkbalk op **Nieuwe query**. Er wordt een leeg queryvenster geopend dat is verbonden met uw database.
2. Voer in het queryvenster de volgende query in:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

3. Klik in de werkbalk op **Uitvoeren** om de opgegeven rij in de tabel Product bij te werken.

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a>Gegevens verwijderen

Gebruik de Transact-SQL-instructie [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) om in uw Azure SQL-database gegevens te verwijderen.

1. Klik op de werkbalk op **Nieuwe query**. Er wordt een leeg queryvenster geopend dat is verbonden met uw database.
2. Voer in het queryvenster de volgende query in:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

3. Klik in de werkbalk op **Uitvoeren** om de opgegeven rij in de tabel Product te verwijderen.

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Server Management Studio gebruiken](https://msdn.microsoft.com/library/ms174173.aspx) voor meer informatie over SSMS.
- Zie [Visual Studio Code](https://code.visualstudio.com/docs) voor meer informatie over het uitvoeren van query's voor en het bewerken van gegevens met Visual Studio Code

