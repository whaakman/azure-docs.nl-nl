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
ms.sourcegitcommit: 5d3bcc3c1434b16279778573ccf3034f9ac28a4d
ms.openlocfilehash: 836d72e32e54ecef9691b55214766a1fc3ff9701


---
# <a name="create-a-sql-data-warehouse-database-by-using-transact-sql-tsql"></a>Een SQL Data Warehouse-database maken met behulp van Transact-SQL (TSQL)
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

In dit artikel wordt beschreven hoe u een SQL Data Warehouse maakt met T-SQL.

## <a name="prerequisites"></a>Vereisten
Om aan de slag te gaan, hebt u het volgende nodig:

* **Azure-account**: ga naar [Gratis proefversie van Azure][Azure Free Trial] of [Azure-tegoed met MSDN][MSDN Azure Credits] om een account te maken.
* **Azure SQL Server**: zie [Een logische Azure SQL Database-server maken met Azure Portal][Create an Azure SQL Database logical server with the Azure Portal] of[ Een logische Azure SQL Database-server maken met PowerShell][Create an Azure SQL Database logical server with PowerShell] voor meer informatie.
* **Resourcegroep**: gebruik dezelfde resourcegroep als uw Azure SQL-server of zie [Een resourcegroep maken][how to create a resource group].
* **Omgeving voor het uitvoeren van T-SQL**: u kunt [Visual Studio][Installing Visual Studio and SSDT], [sqlcmd][sqlcmd] of [SSMS][SSMS] gebruiken om T-SQL uit te voeren.

> [!NOTE]
> Het maken van een SQL Data Warehouse kan leiden tot een nieuwe factureerbare service.  Zie [Prijzen van SQL Data Warehouse][SQL Data Warehouse pricing] voor meer informatie over prijzen.
>
>

## <a name="create-a-database-with-visual-studio"></a>Een database maken met Visual Studio
Als u nog niet bekend bent met Visual Studio, verwijzen wij u naar het artikel [Query’s uitvoeren bij Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)].  Om te beginnen, opent u SQL Server-objectverkenner in Visual Studio en maakt u verbinding met de server die uw SQL Data Warehouse-database gaat hosten.  Als de verbinding is gemaakt, kunt u een SQL Data Warehouse maken door de volgende SQL-opdracht uit te voeren voor **hoofd**database.  Met deze opdracht wordt de database MySqlDwDb gemaakt met een serviceniveaudoelstelling van DW400 en kan de database uitbreiden tot een maximale grootte van 10 TB.

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
Wanneer de inrichting van uw SQL Data Warehouse is voltooid, kunt u [voorbeeldgegevens laden][load sample data] of meer informatie zoeken over [ontwikkelen][develop], [laden][load] of [migreren][migrate].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[how to create a SQL Data Warehouse from the Azure portal]: sql-data-warehouse-get-started-provision.md
[Query Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[load sample data]: sql-data-warehouse-load-sample-databases.md
[Create an Azure SQL Database logical server with the Azure Portal]: ../sql-database/sql-database-get-started.md#create-logical-server-bk
[Create an Azure SQL Database logical server with PowerShell]: ../sql-database/sql-database-get-started-powershell.md#complete-azure-powershell-script-to-create-a-server-firewall-rule-and-database
[how to create a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group
[Installing Visual Studio and SSDT]: sql-data-warehouse-install-visual-studio.md
[sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--MSDN references-->
[CREATE DATABASE]: https://msdn.microsoft.com/library/mt204021.aspx
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx
[SSMS]: https://msdn.microsoft.com/library/mt238290.aspx

<!--Other Web references-->
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Dec16_HO1-->


