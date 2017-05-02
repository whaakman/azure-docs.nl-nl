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
ms.custom: quick start manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/15/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 9ffad92e668b76c9a4e2941b20d075bf52132d16
ms.lasthandoff: 04/19/2017


---
# <a name="azure-sql-database-use-sql-server-management-studio-to-connect-and-query-data"></a>Azure SQL Database: SQL Server Management Studio gebruiken om verbinding te maken en query's uit te voeren voor gegevens

[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SSMS) is een beheerprogramma dat wordt gebruikt om SQL Server-resources te maken en te beheren vanuit de gebruikersinterface of in scripts. In deze Quick Start ziet u hoe u SSMS gebruikt om verbinding te maken met een Azure SQL-database en vervolgens Transact-SQL-instructies gebruikt om gegevens in de database te zoeken, in te voegen, bij te werken en te verwijderen. 

In deze Quick Start wordt dit gebruikt als basis voor het maken van de resources die u hebt gemaakt in een van deze Quick Starts:

- [Database maken - Portal](sql-database-get-started-portal.md)
- [Database maken - CLI](sql-database-get-started-cli.md)

Voordat u begint, zorgt u ervoor dat u de nieuwste versie van [SSMS](https://msdn.microsoft.com/library/mt238290.aspx) hebt geïnstalleerd. 

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen

Haal de verbindingsgegevens op die nodig zijn om verbinding te maken met de Azure SQL-database. U hebt de volledig gekwalificeerde servernaam, databasenaam en aanmeldingsgegevens in de volgende procedures nodig.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. Op de pagina **Overzicht** voor de database controleert u de volledig gekwalificeerde servernaam zoals in de onderstaande afbeelding wordt weergegeven. U kunt de cursor boven de servernaam houden om de optie **Klik om te kopiëren** naar boven te halen.

   ![verbindingsgegevens](./media/sql-database-connect-query-ssms/connection-information.png) 

4. Als u de aanmeldingsgegevens voor uw Azure SQL Database-server bent vergeten, gaat u naar de SQL Database-serverpagina om de beheerdersnaam voor de server weer te geven en, indien nodig, het wachtwoord opnieuw in te stellen. 

## <a name="connect-to-your-database-in-the-sql-database-logical-server"></a>Verbinding maken met uw database op de logische SQL Database-server

Gebruik SQL Server Management Studio om verbinding te maken met uw Azure SQL Database-server. 

> [!IMPORTANT]
> Een logische Azure SQL Database-server luistert naar poort 1433. Als u vanachter een bedrijfsfirewall probeert verbinding te maken met een logische Azure SQL Database-server, moet deze poort in de bedrijfsfirewall zijn geopend om verbinding te kunnen maken.
>

1. Open SQL Server Management Studio.

2. Voer in het dialoogvenster **Verbinding maken met server** de volgende informatie in:
   - **Servertype**: geef de database-engine op
   - **Servernaam**: voer de volledig gekwalificeerde servernaam in, zoals **mynewserver20170313.database.windows.net**
   - **Verificatie**: geef de SQL Server-verificatie op
   - **Aanmelding**: gebruik het beheerdersaccount voor de server
   - **Wachtwoord**: voer het wachtwoord in voor het beheerdersaccount voor de server

   ![verbinding maken met server](./media/sql-database-connect-query-ssms/connect.png)  

3. Klik op **Opties** in het dialoogvenster **Verbinding maken met server**. Voer in de sectie **Verbinding maken met database** **mySampleDatabase** in om verbinding te maken met deze database.

   ![verbinding maken met database op server](./media/sql-database-connect-query-ssms/options-connect-to-db.png)  

4. Klik op **Verbinden**. Het venster Objectverkenner wordt geopend in SSMS. 

   ![verbonden met server](./media/sql-database-connect-query-ssms/connected.png)  

5. In Objectverkenner vouwt u **Databases** en daarna **mySampleDatabase** uit om de objecten in de voorbeelddatabase weer te geven.

## <a name="query-data"></a>Querygegevens

Gebruik de volgende code om op categorie een query uit te voeren voor de 20 populairste producten. Gebruik de Transact-SQL-instructie [SELECT](https://msdn.microsoft.com/library/ms189499.aspx).

1. Klik in Objectverkenner met de rechtermuisknop op **mySampleDatabase** en klik vervolgens op **Nieuwe query**. Er wordt een leeg queryvenster geopend dat is verbonden met uw database.
2. Voer de volgende query in het queryvenster in:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Klik in de werkbalk op **Uitvoeren** om gegevens op te halen uit de tabellen Product en ProductCategory.

    ![query](./media/sql-database-connect-query-ssms/query.png)

## <a name="insert-data"></a>Gegevens invoegen

Gebruik de volgende code om een nieuw product in te voegen in de tabel SalesLT.Product. Gebruik de Transact-SQL-instructie [INSERT](https://msdn.microsoft.com/library/ms174335.aspx).

1. Vervang in het queryvenster de vorige query door de volgende query:

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

2. Klik in de werkbalk op **Uitvoeren** om een nieuwe rij in te voegen in de tabel Product.

    <img src="./media/sql-database-connect-query-ssms/insert.png" alt="insert" style="width: 780px;" />

## <a name="update-data"></a>Gegevens bijwerken

Gebruik de volgende code om het nieuwe product bij te werken dat u eerder hebt toegevoegd. Gebruik de Transact-SQL-instructie [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx).

1. Vervang in het queryvenster de vorige query door de volgende query:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

2. Klik in de werkbalk op **Uitvoeren** om de opgegeven rij in de tabel Product bij te werken.

    <img src="./media/sql-database-connect-query-ssms/update.png" alt="update" style="width: 780px;" />

## <a name="delete-data"></a>Gegevens verwijderen

Gebruik de volgende code om het nieuwe product te verwijderen dat u eerder hebt toegevoegd. Gebruik de Transact-SQL-instructie [DELETE](https://msdn.microsoft.com/library/ms189835.aspx).

1. Vervang in het queryvenster de vorige query door de volgende query:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

2. Klik in de werkbalk op **Uitvoeren** om de opgegeven rij in de tabel Product te verwijderen.

    <img src="./media/sql-database-connect-query-ssms/delete.png" alt="delete" style="width: 780px;" />

## <a name="next-steps"></a>Volgende stappen

- Zie [SQL Server Management Studio gebruiken](https://msdn.microsoft.com/library/ms174173.aspx) voor meer informatie over SSMS.
- Zie [Verbinding maken en query's uitvoeren met Visual Studio Code](sql-database-connect-query-vscode.md) als u verbinding wilt maken en query’s wilt uitvoeren met Visual Studio Code.
- Zie [Verbinding maken en query’s uitvoeren met .NET](sql-database-connect-query-dotnet.md) als u verbinding wilt maken en query’s wilt uitvoeren met .NET.
- Zie [Verbinding maken en query's uitvoeren met PHP](sql-database-connect-query-php.md) als u verbinding wilt maken en query's wilt uitvoeren met PHP.
- Zie [Verbinding maken en query's uitvoeren met Node.js](sql-database-connect-query-nodejs.md) als u verbinding wilt maken en query's wilt uitvoeren met Node.js.
- Zie [Verbinding maken en query's uitvoeren met Java](sql-database-connect-query-java.md) als u verbinding wilt maken en query's wilt uitvoeren met Java.
- Zie [Verbinding maken en query's uitvoeren met Python](sql-database-connect-query-python.md) als u verbinding wilt maken en query's wilt uitvoeren met Python.
- Zie [Verbinding maken en query's uitvoeren met Ruby](sql-database-connect-query-ruby.md) als u verbinding wilt maken en query's wilt uitvoeren met Ruby.

