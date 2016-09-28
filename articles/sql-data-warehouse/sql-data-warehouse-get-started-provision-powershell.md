<properties
   pageTitle="SQL Data Warehouse maken met Powershell | Microsoft Azure"
   description="SQL Data Warehouse maken met Powershell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/25/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# SQL Data Warehouse maken met PowerShell

> [AZURE.SELECTOR]
- [Azure-portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In dit artikel wordt beschreven hoe u een SQL Data Warehouse maakt met PowerShell.

## Vereisten

Om aan de slag te gaan, hebt u het volgende nodig:

- **Azure-account**: ga naar [Gratis proefversie van Azure][] of [Azure-tegoed met MSDN][] om een account te maken.
- **Azure SQL Server**: zie [Een logische Azure SQL-databaseserver met de Azure-portal][] of [Een logische Azure SQL-databaseserver maken met PowerShell][] voor meer informatie.
- **Resourcegroep**: gebruik dezelfde resourcegroep als uw Azure SQL-server of zie [een resourcegroep maken][].
- **PowerShell-versie 1.0.3 of hoger**: u kunt uw versie controleren door **Get-Module - ListAvailable-Name Azure** uit te voeren.  De meest recente versie kan worden geïnstalleerd met het [Webplatforminstallatieprogramma voor Microsoft][].  Zie [Azure PowerShell installeren en configureren][] voor meer informatie over het installeren van de meest recente versie.

> [AZURE.NOTE] Het maken van een SQL Data Warehouse kan leiden tot een nieuwe factureerbare service.  Zie [Prijzen van SQL Data Warehouse][] voor meer informatie over prijzen.

## Een SQL Data Warehouse maken

1. Open Windows PowerShell.
2. Voer deze cmdlet uit om u aan te melden bij Azure Resource Manager.

    ```Powershell
    Login-AzureRmAccount
    ```
    
3. Selecteer het abonnement dat u voor de huidige sessie wilt gebruiken.

    ```Powershell
    Get-AzureRmSubscription -SubscriptionName "MySubscription" | Select-AzureRmSubscription
    ```

4.  Maak database. In dit voorbeeld wordt een database met de naam mynewsqldw met servicedoelstellingsniveau DW400 gemaakt op een server met de naam sqldwserver1, die zich bevindt in de resourcegroep mywesteuroperesgp1.

    ```Powershell
    New-AzureRmSqlDatabase -RequestedServiceObjectiveName "DW400" -DatabaseName "mynewsqldw" -ServerName "sqldwserver1" -ResourceGroupName "mywesteuroperesgp1" -Edition "DataWarehouse" -CollationName "SQL_Latin1_General_CP1_CI_AS" -MaxSizeBytes 10995116277760
    ```

De vereiste parameters zijn:

- **RequestedServiceObjectiveName**: de hoeveelheid [DWU][] die u aanvraagt.  De ondersteunde waarden zijn: DW100, DW200, DW300, DW400, DW500, DW600, DW1000, DW1200, DW1500, DW2000, DW3000 en DW6000.
- **DatabaseName**: de naam van de SQL Data Warehouse die u maakt.
- **ServerName**: de naam van de server die u bij het maken gebruikt (moet V12 zijn).
- **ResourceGroupName**: de resourcegroep die u gebruikt.  Gebruik Get-AzureResource om beschikbare resourcegroepen in uw abonnement te zoeken.
- **Edition**: moet DataWarehouse zijn, als u een SQL Data Warehouse wilt maken.

De optionele parameters zijn:

- **CollationName**: de standaardsortering, indien niet opgegeven, is COLLATE SQL_Latin1_General_CP1_CI_AS.  De sortering kan niet worden gewijzigd voor een database.
- **MaxSizeBytes**: de maximale grootte van een database is standaard 10 GB.


Zie [New-AzureRmSqlDatabase][] en [Create Database (Azure SQL Data Warehouse)][] voor meer informatie over de parameteropties.

## Volgende stappen

Wanneer de inrichting van uw SQL Data Warehouse is voltooid, kunt u proberen om [voorbeeldgegevens te laden][] of meer informatie te zoeken over [ontwikkelen][], [laden][] of [migreren][].

Als u wilt weten hoe u SQL Data Warehouse programmatisch kunt beheren, lees dan het artikel over het gebruik van [PowerShell cmdlets and REST APIs][] (PowerShell-cmdlets en REST-API's).

<!--Image references-->

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[migreren]: ./sql-data-warehouse-overview-migrate.md
[ontwikkelen]: ./sql-data-warehouse-overview-develop.md
[laden]: ./sql-data-warehouse-load-with-bcp.md
[voorbeeldgegevens te laden]: ./sql-data-warehouse-load-sample-databases.md
[PowerShell cmdlets and REST APIs]: ./sql-data-warehouse-reference-powershell-cmdlets.md
[firewall-regels]: ../sql-database-configure-firewall-settings.md

[Azure PowerShell installeren en configureren]: ../powershell/powershell-install-configure.md
[een SQL Data Warehouse maken vanuit de Azure-portal]: ./sql-data-warehouse-get-started-provision.md
[Een logische Azure SQL-databaseserver met de Azure-portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Een logische Azure SQL-databaseserver maken met PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
[een resourcegroep maken]: ../resource-group-template-deploy-portal.md#create-resource-group

<!--MSDN references--> 
[MSDN]: https://msdn.microsoft.com/library/azure/dn546722.aspx
[New-AzureRmSqlDatabase]: https://msdn.microsoft.com/library/mt619339.aspx
[Create Database (Azure SQL Data Warehouse)]: https://msdn.microsoft.com/library/mt204021.aspx

<!--Other Web references-->
[Webplatforminstallatieprogramma voor Microsoft]: https://aka.ms/webpi-azps
[Prijzen van SQL Data Warehouse]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Gratis proefversie van Azure]: https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F
[Azure-tegoed met MSDN]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F



<!--HONumber=Sep16_HO3-->


