---
title: Verbinding maken met Azure SQL Data Warehouse sqlcmd | Microsoft Docs
description: Gebruik het opdracht regel hulpprogramma Sqlcmd om verbinding te maken met en een query uit te zoeken op een Azure SQL Data Warehouse.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 04/17/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: f3b93660fb9f8f3b0bfdddc37105b9e998ed9eee
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479511"
---
# <a name="connect-to-sql-data-warehouse-with-sqlcmd"></a>Verbinding maken met SQL Data Warehouse met sqlcmd
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Gebruik het opdracht regel hulpprogramma [Sqlcmd][sqlcmd] om verbinding te maken met en een query uit te zoeken op een Azure SQL Data Warehouse.  

## <a name="1-connect"></a>1. Verbinden
U gaat als volgt aan de slag met [sqlcmd][sqlcmd]: open de opdrachtprompt en voer **sqlcmd** in, gevolgd door de verbindingstekenreeks voor uw SQL Data Warehouse-database. De verbindingstekenreeks moet de volgende parameters bevatten:

* **Server (-S):** Server in de naam `<``>`van de formulier server. database.Windows.net
* **Data Base (-d):** Database naam.
* **Id's van aanhalings tekens inschakelen (-I):** Id's tussen aanhalings tekens moeten zijn ingeschakeld om verbinding te maken met een SQL Data Warehouse-exemplaar.

Als u gebruik wilt maken van SQL Server-verificatie, moet u de gebruikersnaam- en wachtwoordparameters toevoegen:

* **Gebruiker (-U):** Server gebruiker in de formulier `<`gebruiker`>`
* **Wacht woord (-P):** Het wacht woord dat is gekoppeld aan de gebruiker.

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
Zie [Sqlcmd-documentatie][sqlcmd] voor meer informatie over de beschik bare opties in Sqlcmd.

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd]: https://msdn.microsoft.com/library/ms162773.aspx
[Azure portal]: https://portal.azure.com

<!--Other Web references-->
