---
title: 'VS Code: verbinding maken met Azure SQL Database en query&quot;s uitvoeren voor gegevens | Microsoft Docs'
description: Ontdek hoe u verbinding maakt met SQL Database in Azure met behulp van Visual Studio Code. Voer daarna Transact-SQL-instructies (T-SQL) uit om query&quot;s uit te voeren voor gegevens en om gegevens te bewerken.
metacanonical: 
keywords: verbinding maken met sql-database
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 676bd799-a571-4bb8-848b-fb1720007866
ms.service: sql-database
ms.custom: manage
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 03/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 86471fe29bbc9076624d96b83f7001d8755363bc
ms.lasthandoff: 03/25/2017


---
# <a name="azure-sql-database-use-visual-studio-code-to-connect-and-query-data"></a>Azure SQL Database: Visual Studio Code gebruiken om verbinding te maken en query's uit te voeren voor gegevens

[Visual Studio Code](https://code.visualstudio.com/docs) is een grafische code-editor voor Linux, macOS en Windows die ondersteuning biedt voor extensies. Gebruik Visual Studio Code met de [mssql-extensie](https://aka.ms/mssql-marketplace) om verbinding te maken met en query's uit te voeren voor een Azure SQL-database. In deze handleiding wordt informatie geboden over het gebruik van Visual Studio Code om verbinding te maken met een Azure SQL-database, over het uitvoeren van query's en over het invoegen, bijwerken en verwijderen van instructies.

In deze Quick Start wordt dit gebruikt als basis voor het maken van de resources die u hebt gemaakt in een van deze Quick Starts:

- [Database maken - Portal](sql-database-get-started-portal.md)
- [Database maken - CLI](sql-database-get-started-cli.md)

Voordat u begint, zorgt u ervoor dat u de nieuwste versie van [Visual Studio Code](https://code.visualstudio.com/Download) hebt geïnstalleerd en dat de [mssql-extensie](https://aka.ms/mssql-marketplace) is geladen. Zie [VS Code installeren](https://docs.microsoft.com/sql/linux/sql-server-linux-develop-use-vscode#install-vs-code) voor hulp bij het installeren van de mssql-extensie. 

## <a name="get-connection-information"></a>Verbindingsgegevens ophalen

Haal de volledig gekwalificeerde servernaam van uw Azure SQL Database-server op uit Azure Portal. U gebuikt de volledig gekwalificeerde servernaam om verbinding met uw server te maken via Visual Studio Code.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer **SQL-databases** in het menu links en klik op uw database op de pagina **SQL-databases**. 
3. In het deelvenster **Essentials** van de Azure Portal-pagina van uw database kopieert u de **servernaam** voor later gebruik in deze Quick Start.

    <img src="./media/sql-database-connect-query-ssms/connection-information.png" alt="connection information" style="width: 780px;" />

## <a name="set-language-mode-to-sql"></a>Taalmodus instellen op SQL

Zorg ervoor dat de taalmodus in Visual Studio Code is ingesteld op **SQL** om mssql-opdrachten en T-SQL IntelliSense in te schakelen.

1. Open een nieuw Visual Studio Code venster. 

2. Druk op **CTRL+K,M**, typ **SQL** en druk op **ENTER** om de taalmodus in te stellen op SQL. 

<img src="./media/sql-database-connect-query-vscode/vscode-language-mode.png" alt="SQL language mode" style="width: 780px;" />

## <a name="connect-to-the-server"></a>Verbinding maken met de server

Gebruik Visual Studio Code om verbinding te maken met uw Azure SQL Database-server.

1. In VS Code drukt u op **CTRL+SHIFT+P** (of **F1**) om het opdrachtenpalet te openen.

2. Typ **sqlcon** en druk op **ENTER**.

3. Klik op **Ja** om uw taal in te stellen op **SQL**.

4. Druk op **ENTER** om **Verbindingsprofiel maken** te selecteren. Hiermee wordt een verbindingsprofiel gemaakt voor uw exemplaar van SQL Server.

5. Volg de aanwijzingen om de verbindingseigenschappen op te geven voor het nieuwe verbindingsprofiel. Wanneer u een waarde hebt ingevoerd, drukt u op **ENTER** om door te gaan. 

   In de volgende tabel worden de verbindingsprofieleigenschappen beschreven.

   | Instelling | Beschrijving |
   |-----|-----|
   | **Servernaam** | Voer de volledig gekwalificeerde servernaam in, zoals **mynewserver20170313.database.windows.net** |
   | **Databasenaam** | Voer de naam van uw database in, zoals **mySampleDatabase** |
   | **Verificatie** | Selecteer de SQL-aanmelding |
   | **Gebruikersnaam** | Gebruik het beheerdersaccount voor de server |
   | **Wachtwoord (SQL-aanmelding)** | Voer het wachtwoord in voor het beheerdersaccount voor de server | 
   | **Wachtwoord opslaan?** | Selecteer **Ja** of **Nee** |
   | **[Optioneel] Voer een naam in voor dit profiel** | Voer een verbindingsprofielnaam in, zoals **mySampleDatabase**. 

6. Druk op **ESC** om het bericht te sluiten met de informatie dat het profiel is gemaakt en verbonden.

7. Controleer in de statusbalk of er verbinding is gemaakt.

   <img src="./media/sql-database-connect-query-vscode/vscode-connection-status.png" alt="Connection status" style="width: 780px;" />

## <a name="query-data"></a>Querygegevens

Gebruik de Transact-SQL-instructie [SELECT](https://msdn.microsoft.com/library/ms189499.aspx) om in uw Azure SQL-database query's uit te voeren voor gegevens.

1. Voer in het venster **Editor** de volgende query in in het lege queryvenster:

   ```sql
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM [SalesLT].[ProductCategory] pc
   JOIN [SalesLT].[Product] p
   ON pc.productcategoryid = p.productcategoryid;
   ```

3. Druk op **CTRL+SHIFT+E** om gegevens op te halen uit de tabellen Product en ProductCategory.

    <img src="./media/sql-database-connect-query-vscode/query.png" alt="Query" style="width: 780px;" />

## <a name="insert-data"></a>Gegevens invoegen

Gebruik de Transact-SQL-instructie [INSERT](https://msdn.microsoft.com/library/ms174335.aspx) om in uw Azure SQL-database gegevens in te voegen.

1. In het venster **Editor** verwijdert u de eerdere query en voert u de volgende query in:

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

3. Druk op **CTRL+SHIFT+E** om een nieuwe rij in te voegen in de tabel Product.

## <a name="update-data"></a>Gegevens bijwerken

Gebruik de Transact-SQL-instructie [UPDATE](https://msdn.microsoft.com/library/ms177523.aspx) om in uw Azure SQL-database gegevens bij te werken.

1.  In het venster **Editor** verwijdert u de eerdere query en voert u de volgende query in:

   ```sql
   UPDATE [SalesLT].[Product]
   SET [ListPrice] = 125
   WHERE Name = 'myNewProduct';
   ```

3. Druk op **CTRL+SHIFT+E** om de opgegeven rij in de tabel Product bij te werken.

## <a name="delete-data"></a>Gegevens verwijderen

Gebruik de Transact-SQL-instructie [DELETE](https://msdn.microsoft.com/library/ms189835.aspx) om in uw Azure SQL-database gegevens te verwijderen.

1. In het venster **Editor** verwijdert u de eerdere query en voert u de volgende query in:

   ```sql
   DELETE FROM [SalesLT].[Product]
   WHERE Name = 'myNewProduct';
   ```

3. Druk op **CTRL+SHIFT+E** om de opgegeven rij in de tabel Product te verwijderen.

## <a name="next-steps"></a>Volgende stappen

- Zie [Visual Studio Code](https://code.visualstudio.com/docs) voor meer informatie over Visual Studio Code
- Zie [SSMS](https://msdn.microsoft.com/library/ms174173.aspx) voor informatie over het uitvoeren van query's voor en het bewerken van gegevens met SQL Server Management Studio.

