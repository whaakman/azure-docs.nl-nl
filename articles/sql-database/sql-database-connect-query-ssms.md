---
title: 'Verbinding maken met SQL Database: SQL Server Management Studio | Microsoft Docs'
description: Ontdek hoe u verbinding maakt met SQL Database in Azure met behulp van SQL Server Management Studio (SSMS). Voer een voorbeeldquery uit met Transact-SQL (T-SQL).
metacanonical: 
keywords: verbinding maken met SQL-database,Sql Server Management Studio
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 7cd2a114-c13c-4ace-9088-97bd9d68de12
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: sstein;carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0eb25eb76c6c6c2446ac0b2b07c65975c3719db0


---
# <a name="connect-to-sql-database-with-sql-server-management-studio-and-execute-a-sample-tsql-query"></a>Verbinding maken met SQL Database met SQL Server Management Studio en een voorbeeld T-SQL-query uitvoeren
> [!div class="op_single_selector"]
> * [Visual Studio](sql-database-connect-query.md)
> * [SSMS](sql-database-connect-query-ssms.md)
> * [Excel](sql-database-connect-excel.md)
> 
> 

In dit artikel wordt beschreven hoe u verbinding maakt met een SQL-database in Azure met behulp van SQL Server Management Studio (SSMS). Nadat de verbinding tot stand is gebracht, voeren we een eenvoudige T-SQL-query (Transact-SQL) uit om te controleren of de communicatie met de database goed verloopt.

[!INCLUDE [SSMS Install](../../includes/sql-server-management-studio-install.md)]

[!INCLUDE [SSMS Connect](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]

## <a name="run-sample-queries"></a>Voorbeeldquery's uitvoeren
Nadat u verbinding hebt gemaakt met uw server, kunt u verbinding maken met een database en een voorbeeldquery uitvoeren. Zie [Writing Transact-SQL Statements](https://msdn.microsoft.com/library/ms365303.aspx) (Transact-SQL-instructies opstellen) als u niet bekend bent met het opstellen van query's.

1. Navigeer in **Objectverkenner** naar een database op de server, zoals de **AdventureWorks**-voorbeelddatabase.
2. Klik met de rechtermuisknop op de database en selecteer vervolgens **Nieuwe query**:
   
    ![Nieuwe query. Verbinding maken met de SQL Database-server: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)
3. Kopieer en plak de volgende code in het queryvenster:
   
        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;
4. Klik op de knop **Uitvoeren**:
   
    ![Geslaagd. Verbinding maken met de SQL Database-server: SQL Server Management Studio](./media/sql-database-connect-query-ssms/5-success.png)

## <a name="next-steps"></a>Volgende stappen
U kunt de T-SQL-instructies gebruiken om databases te maken en te beheren in Azure op ongeveer dezelfde manier als dat kan bij SQL Server. Als u vertrouwd bent met T-SQL met SQL Server, raadpleegt u [Azure SQL Database Transact-SQL-informatie](sql-database-transact-sql-information.md) voor een overzicht van verschillen.

Als u geen ervaring hebt met T-SQL, raadpleegt u de [Zelfstudie: Transact-SQL-instructies schrijven](https://msdn.microsoft.com/library/ms365303.aspx) en het [Transact-SQL-naslagmateriaal (Database-engine)](https://msdn.microsoft.com/library/bb510741.aspx).

Om aan de slag te gaan met het maken van databasegebruikers en databasegebruikersbeheerders raadpleegt u [Aan de slag met Azure SQL Database-beveiliging](sql-database-get-started-security.md)

Zie [Use SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx) (SQL Server Management Studio gebruiken) voor meer informatie over SSMS.




<!--HONumber=Nov16_HO2-->


