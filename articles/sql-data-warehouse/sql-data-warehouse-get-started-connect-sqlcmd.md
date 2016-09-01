<properties
   pageTitle="Query’s uitvoeren bij Azure SQL Data Warehouse (sqlcmd)| Microsoft Azure"
   description="Query’s uitvoeren bij Azure SQL Data Warehouse met het opdrachtregelhulpprogramma sqlcmd."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/22/2016"
   ms.author="mausher;barbkess;sonyama"/>

# Query’s uitvoeren bij Azure SQL Data Warehouse (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 

In dit scenario maakt u gebruik van het opdrachtregelhulpprogramma sqlcmd om een query uit te voeren bij een Azure SQL Data Warehouse.  

## Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

-  [sqlcmd.exe][]. Zie [Microsoft Command Line Utilities 11 voor SQL Server][] om dit opdrachtregelhulpprogramma te downloaden. Mogelijk is hiervoor ook [Microsoft ODBC-stuurprogramma 11 voor SQL Server Windows][] vereist.

## 1. Verbinding maken

Open om te beginnen de opdrachtregel met sqlcmd en voer **sqlcmd** in, gevolgd door de verbindingstekenreeks voor uw SQL Data Warehouse-database. De verbindingsreeks heeft de volgende parameters nodig:

+ **Server (-S):** server in de notatie `<`servernaam`>`.database.windows.net
+ **Database (-d):** databasenaam.
+ **Gebruiker (-U):** servergebruiker in de notatie `<`gebruiker`>`
+ **Wachtwoord (-P):** wachtwoord dat is gekoppeld aan de gebruiker.
+ **ID's tussen aanhalingstekens inschakelen (-I):** id's tussen aanhalingstekens moeten zijn ingeschakeld om verbinding te kunnen maken met een exemplaar van SQL Data Warehouse.

Een voorbeeld: uw verbindingstekenreeks kan er als volgt uitzien:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

> [AZURE.NOTE] De optie -I waarmee u id’s tussen aanhalingstekens kunt opgeven, is momenteel vereist om verbinding te maken met SQL Data Warehouse.

## 2. Query’s uitvoeren

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

## Volgende stappen

Zie [sqlcmd-documentatie][sqlcmd.exe] voor meer informatie over de opties die beschikbaar zijn in sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd.exe]: https://msdn.microsoft.com/library/ms162773.aspx
[Microsoft ODBC-stuurprogramma 11 voor SQL Server Windows]: https://www.microsoft.com/download/details.aspx?id=36434
[Microsoft Command Line Utilities 11 voor SQL Server]: http://go.microsoft.com/fwlink/?LinkId=321501
[Azure-portal]: https://portal.azure.com

<!--Other Web references-->



<!--HONumber=ago16_HO4-->


