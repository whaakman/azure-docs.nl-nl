---
title: "Query’s uitvoeren bij Azure SQL Data Warehouse (sqlcmd)| Microsoft Docs"
description: "Query’s uitvoeren bij Azure SQL Data Warehouse met het opdrachtregelhulpprogramma sqlcmd."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 6e2b69e5-4806-4e91-9ea1-e2b63bf28c46
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1f2cf8003e46a1df30810a2594bc1d380bc13bcf


---
# <a name="query-azure-sql-data-warehouse-sqlcmd"></a>Query’s uitvoeren bij Azure SQL Data Warehouse (sqlcmd)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

In dit scenario maakt u gebruik van het opdrachtregelhulpprogramma [sqlcmd][sqlcmd] om een query uit te voeren bij een Azure SQL Data Warehouse.  

## <a name="1-connect"></a>1. Verbinding maken
U gaat als volgt aan de slag met [sqlcmd][sqlcmd]: open de opdrachtprompt en voer **sqlcmd** in, gevolgd door de verbindingstekenreeks voor uw SQL Data Warehouse-database. De verbindingstekenreeks moet de volgende parameters bevatten:

* **Server (-S):** server in de notatie `<`servernaam`>`.database.windows.net
* **Database (-d):** databasenaam.
* **Id's tussen aanhalingstekens inschakelen (-I):** id's tussen aanhalingstekens moeten zijn ingeschakeld om verbinding te kunnen maken met een exemplaar van SQL Data Warehouse.

Als u gebruik wilt maken van SQL Server-verificatie, moet u de gebruikersnaam- en wachtwoordparameters toevoegen:

* **Gebruiker (-U):** servergebruiker in de notatie `<`gebruiker`>`
* **Wachtwoord (-P):** wachtwoord dat is gekoppeld aan de gebruiker.

Een voorbeeld: uw verbindingstekenreeks kan er als volgt uitzien:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

Als u geïntegreerde verificatie van Azure Active Directory wilt gebruiken, moet u de Azure Active Directory-parameters toevoegen:

* **Azure Active Directory Authentication (-G):** Azure Active Directory gebruiken voor verificatie

Een voorbeeld: uw verbindingstekenreeks kan er als volgt uitzien:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -G -I
```

> [!NOTE]
> U moet [Azure Active Directory Authentication inschakelen](sql-data-warehouse-authentication.md) om te verifiëren met Active Directory.
> 
> 

## <a name="2-query"></a>2. Query’s uitvoeren
Wanneer verbinding is gemaakt, kunt u elke ondersteunde Transact-SQL-instructie voor het exemplaar uitvoeren.  In dit voorbeeld worden query's in de interactieve modus verzonden.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

In de volgende voorbeelden ziet u hoe u uw query's in de batchmodus uitvoert met behulp van de optie -Q of door uw SQL naar sqlcmd te sluizen.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## <a name="next-steps"></a>Volgende stappen
Zie [sqlcmd-documentatie][sqlcmd] voor meer informatie over de opties die beschikbaar zijn in sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure Portal]: https://portal.azure.com

<!--Other Web references-->



<!--HONumber=Nov16_HO2-->


