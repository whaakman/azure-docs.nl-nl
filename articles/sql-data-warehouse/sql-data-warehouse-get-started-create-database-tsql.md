---
title: Een SQL Data Warehouse maken met TSQL | Microsoft Docs
description: Meer informatie over het maken van een Azure SQL Data Warehouse met TSQL
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: a4e2e68e-aa9c-4dd3-abb0-f7df997d237a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 443e74834a7181560e812ce31db460ef2c4ff911


---
# <a name="create-a-sql-data-warehouse-database-by-using-transactsql-tsql"></a>Een SQL Data Warehouse-database maken met behulp van Transact-SQL (TSQL)
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
> 
> 

In dit artikel wordt beschreven hoe u een SQL Data Warehouse maakt met T-SQL.

## <a name="prerequisites"></a>Vereisten
Om aan de slag te gaan, hebt u het volgende nodig: 

* **Azure-account**: ga naar [Gratis proefversie van Azure][Gratis proefversie van Azure] of [Azure-tegoed met MSDN][Azure-tegoed met MSDN] om een account te maken.
* **Azure SQL-server**: zie [Een logische Azure SQL Database-server maken met Azure Portal][Een logische Azure SQL Database-server maken met Azure Portal] of [Een logische Azure SQL Database-server maken met PowerShell][Een logische Azure SQL Database-server maken met PowerShell] voor meer informatie.
* **Resourcegroep**: gebruik dezelfde resourcegroep als uw Azure SQL-server of zie [een resourcegroep maken][een resourcegroep maken].
* **Omgeving voor het uitvoeren van T-SQL**: u kunt [Visual Studio][Visual Studio en SSDT installeren], [sqlcmd][sqlcmd], of [SSMS][SSMS] gebruiken om T-SQL uit te voeren.

> [!NOTE]
> Het maken van een SQL Data Warehouse kan leiden tot een nieuwe factureerbare service.  Zie [Prijzen van SQL Data Warehouse][Prijzen van SQL Data Warehouse] voor meer informatie over prijzen.
> 
> 

## <a name="create-a-database-with-visual-studio"></a>Een database maken met Visual Studio
Als u nog niet bekend bent met Visual Studio, verwijzen wij u naar het artikel [Query’s uitvoeren bij Azure SQL Data Warehouse (Visual Studio)][Query’s uitvoeren bij Azure SQL Data Warehouse (Visual Studio)].  Om te beginnen, opent u SQL Server-objectverkenner in Visual Studio en maakt u verbinding met de server die uw SQL Data Warehouse-database gaat hosten.  Als de verbinding is gemaakt, kunt u een SQL Data Warehouse maken door de volgende SQL-opdracht uit te voeren voor **hoofd**database.  Met deze opdracht wordt de database MySqlDwDb gemaakt met een serviceniveaudoelstelling van DW400 en kan de database uitbreiden tot een maximale grootte van 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## <a name="create-a-database-with-sqlcmd"></a>Een database maken met sqlcmd
U kunt dezelfde opdracht ook met sqlcmd uitvoeren door het volgende in de opdrachtprompt uit te voeren.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

De standaardsortering, indien niet opgegeven, is COLLATE SQL_Latin1_General_CP1_CI_AS.  De `MAXSIZE` kan liggen tussen 250 GB en 240 TB.  De `SERVICE_OBJECTIVE` kan liggen tussen DW100 en DW2000 [DWU][DWU].  Voor een lijst met alle geldige waarden, verwijzen wij u naar de MSDN-documentatie voor [CREATE DATABASE][CREATE DATABASE].  MAXSIZE en SERVICE_OBJECTIVE kunnen beide worden gewijzigd met de T-SQL-opdracht [ALTER DATABASE][ALTER DATABASE].  De sortering van een database kan niet worden gewijzigd nadat deze is gemaakt.   Wees voorzichtig wanneer u SERVICE_OBJECTIVE verandert. Wanneer de DWU wordt gewijzigd, worden services namelijk opnieuw gestart waarbij alle lopende query's worden geannuleerd.  Voor een verandering van MAXSIZE hoeven services niet opnieuw te worden gestart, omdat dit een eenvoudige metagegevensbewerking is.

## <a name="next-steps"></a>Volgende stappen
Wanneer de inrichting van uw SQL Data Warehouse is voltooid, kunt u [voorbeeldgegevens laden][voorbeeldgegevens laden] of meer informatie zoeken over [ontwikkelen][ontwikkelen], [laden][laden] of [migreren][migreren].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[een SQL Data Warehouse maken vanuit Azure Portal]: sql-data-warehouse-get-started-provision.md
[Query’s uitvoeren bij Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migreren]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md
[laden]: sql-data-warehouse-overview-load.md
[voorbeeldgegevens laden]: sql-data-warehouse-load-sample-databases.md
[Een logische Azure SQL Database-server maken met Azure Portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Een logische Azure SQL Database-server maken met PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[een resourcegroep maken]: ../resource-group-template-deploy-portal.md#create-resource-group
[Visual Studio en SSDT installeren]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references--> 
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[Prijzen van SQL Data Warehouse]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Gratis proefversie van Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Azure-tegoed met MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Nov16_HO2-->


