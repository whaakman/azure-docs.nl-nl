---
title: SQL Data Warehouse maken met Powershell | Microsoft Docs
description: SQL Data Warehouse maken met Powershell
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 97434863-7938-4129-8949-5a119f5949e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: create
ms.date: 10/31/2016
ms.author: elbutter;barbkess
ms.openlocfilehash: a763f1c600c1a3f37cb565a8eb7db3c3f27dcf75
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-sql-data-warehouse-using-powershell"></a>SQL Data Warehouse maken met PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](sql-data-warehouse-get-started-provision.md)
> * [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
> * [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)
>
>

In dit artikel wordt beschreven hoe u een SQL Data Warehouse maakt met PowerShell.

## <a name="prerequisites"></a>Vereisten
Om aan de slag te gaan, hebt u het volgende nodig:

* **Azure-account**: ga naar [Gratis proefversie van Azure][Azure Free Trial] of [Azure-tegoed met MSDN][MSDN Azure Credits] om een account te maken.
* **Azure SQL Server**: zie [Een Azure SQL-database maken met Azure Portal][Create an Azure SQL database in the Azure Portal] of [Een Azure SQL-database maken met PowerShell][Create an Azure SQL database with PowerShell] voor meer informatie.
* **Resourcegroep**: gebruik dezelfde resourcegroep als uw Azure SQL-server of zie [een resourcegroep maken](../azure-resource-manager/resource-group-portal.md).
* **PowerShell-versie 1.0.3 of hoger**: u kunt uw versie controleren door **Get-Module - ListAvailable-Name Azure** uit te voeren.  De meest recente versie kan worden geïnstalleerd met het [Webplatforminstallatieprogramma van Microsoft][Microsoft Web Platform Installer].  Zie [Azure PowerShell installeren en configureren][How to install and configure Azure PowerShell] voor meer informatie over het installeren van de meest recente versie.

> [!NOTE]
> Het maken van een SQL Data Warehouse kan leiden tot een nieuwe factureerbare service.  Zie [Prijzen van SQL Data Warehouse][SQL Data Warehouse pricing] voor meer informatie over prijzen.
>
>

## <a name="create-a-sql-data-warehouse"></a>Een SQL Data Warehouse maken
1. Open Windows PowerShell.
2. Voer deze cmdlet uit om u aan te melden bij Azure Resource Manager.

    ```Powershell
    Login-AzureRmAccount
    ```
3. Selecteer het abonnement dat u voor de huidige sessie wilt gebruiken.

    ```Powershell
    Get-AzureRmSubscription    -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```
4. Maak database. In dit voorbeeld wordt een database met de naam mynewsqldw met servicedoelstellingsniveau DW400 gemaakt op een server met de naam sqldwserver1, die zich bevindt in de resourcegroep mywesteuroperesgp1.

   ```Powershell
   New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
   ```

De vereiste parameters zijn:

* **RequestedServiceObjectiveName**: de hoeveelheid [DWU][DWU] die u aanvraagt.  De ondersteunde waarden zijn: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 en DW6000.
* **DatabaseName**: de naam van de SQL Data Warehouse die u maakt.
* **ServerName**: de naam van de server die u bij het maken gebruikt (moet V12 zijn).
* **ResourceGroupName**: de resourcegroep die u gebruikt.  Gebruik Get-AzureResource om beschikbare resourcegroepen in uw abonnement te zoeken.
* **Edition**: moet DataWarehouse zijn, als u een SQL Data Warehouse wilt maken.

De optionele parameters zijn:

* **CollationName**: de standaardsortering, indien niet opgegeven, is COLLATE SQL_Latin1_General_CP1_CI_AS.  De sortering kan niet worden gewijzigd voor een database.
* **MaxSizeBytes**: de maximale grootte van een database is standaard 10 GB.

Zie [New-AzureRmSqlDatabase][New-AzureRmSqlDatabase] en [Create Database (Azure SQL Data Warehouse)][Create Database (Azure SQL Data Warehouse)] voor meer informatie over de parameteropties.

## <a name="next-steps"></a>Volgende stappen
Wanneer de inrichting van uw SQL Data Warehouse is voltooid, kunt u proberen om [voorbeeldgegevens te laden][loading sample data] of meer informatie te zoeken over [ontwikkelen][develop],[laden][load] of [migreren][migrate].

Als u wilt weten hoe u SQL Data Warehouse programmatisch kunt beheren, lees dan het artikel over het gebruik van [PowerShell cmdlets en REST-API's][PowerShell cmdlets and REST APIs].

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-load-with-bcp.md
[loading sample data]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlets and REST APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall rules]: ../sql-database-configure-firewall-settings.md

[How to install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs
[how to create a SQL Data Warehouse from the Azure Portal]: ./sql-data-warehouse-get-started-provision.md
[Create an Azure SQL database in the Azure Portal]: ../sql-database/sql-database-get-started.md
[Create an Azure SQL database with PowerShell]: ../sql-database/sql-database-get-started-powershell.md
[how to create a resource group]: ../azure-resource-manager/resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references-->
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Microsoft Web Platform Installer]: https://aka.ms/webpi-azps
[SQL Data Warehouse pricing]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Azure Free Trial]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[MSDN Azure Credits]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F
