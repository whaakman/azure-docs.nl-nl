<properties
   pageTitle="Een SQL Data Warehouse maken met TSQL | Microsoft Azure"
   description="Meer informatie over het maken van een Azure SQL Data Warehouse met TSQL"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/24/2016"
   ms.author="lodipalm;barbkess;sonyama"/>

# Een SQL Data Warehouse-database maken met behulp van Transact-SQL (TSQL)

> [AZURE.SELECTOR]
- [Azure-portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

In dit artikel wordt beschreven hoe u een SQL Data Warehouse maakt met T-SQL.

## Vereisten

Om aan de slag te gaan, hebt u het volgende nodig: 

- **Azure-account**: ga naar [Gratis proefversie van Azure][] of [Azure-tegoed met MSDN][] om een account te maken.
- **Azure SQL Server**: zie [Een logische Azure SQL-databaseserver met de Azure-portal][] of [Een logische Azure SQL-databaseserver maken met PowerShell][] voor meer informatie.
- **Resourcegroep**: gebruik dezelfde resourcegroep als uw Azure SQL-server of zie [een resourcegroep maken][].
- **Omgeving voor het uitvoeren van T-SQL**: u kunt [Visual Studio][Visual Studio en SSDT installeren], [sqlcmd][] of [SSMS][] gebruiken om T-SQL uit te voeren.

> [AZURE.NOTE] Het maken van een SQL Data Warehouse kan leiden tot een nieuwe factureerbare service.  Zie [Prijzen van SQL Data Warehouse][] voor meer informatie over prijzen.

## Een database maken met Visual Studio

Als u nog niet bekend bent met Visual Studio, verwijzen wij u naar het artikel [Query’s uitvoeren bij Azure SQL Data Warehouse (Visual Studio)][].  Om te beginnen, opent u SQL Server-objectverkenner in Visual Studio en maakt u verbinding met de server die uw SQL Data Warehouse-database gaat hosten.  Als de verbinding is gemaakt, kunt u een SQL Data Warehouse maken door de volgende SQL-opdracht uit te voeren voor **hoofd**database.  Met deze opdracht wordt de database MySqlDwDb gemaakt met een serviceniveaudoelstelling van DW400 en kan de database uitbreiden tot een maximale grootte van 10 TB.

```sql
CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB);
```

## Een database maken met sqlcmd

U kunt dezelfde opdracht ook met sqlcmd uitvoeren door het volgende in de opdrachtprompt uit te voeren.

```sql
sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE MySqlDwDb COLLATE SQL_Latin1_General_CP1_CI_AS (EDITION='datawarehouse', SERVICE_OBJECTIVE = 'DW400', MAXSIZE= 10240 GB)"
```

De standaardsortering, indien niet opgegeven, is COLLATE SQL_Latin1_General_CP1_CI_AS.  De `MAXSIZE` kan liggen tussen 250 GB en 240 TB.  De `SERVICE_OBJECTIVE` kan liggen tussen DW100 en DW2000 [DWU][].  Voor een lijst met alle geldige waarden, verwijzen wij u naar de MSDN-documentatie voor [CREATE DATABASE][].  MAXSIZE en SERVICE_OBJECTIVE kunnen beide worden gewijzigd met de T-SQL-opdracht [ALTER DATABASE][].  De sortering van een database kan niet worden gewijzigd nadat deze is gemaakt.   Wees voorzichtig wanneer u SERVICE_OBJECTIVE verandert. Wanneer de DWU wordt gewijzigd, worden services namelijk opnieuw gestart waarbij alle lopende query's worden geannuleerd.  Voor een verandering van MAXSIZE hoeven services niet opnieuw te worden gestart, omdat dit een eenvoudige metagegevensbewerking is.

## Volgende stappen

Wanneer de inrichting van uw SQL Data Warehouse is voltooid, kunt u [voorbeeldgegevens laden][] of meer informatie zoeken over [ontwikkelen][], [laden][] of [migreren][].

<!--Article references-->
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units
[een SQL Data Warehouse maken vanuit de Azure-portal]: sql-data-warehouse-get-started-provision.md
[Query’s uitvoeren bij Azure SQL Data Warehouse (Visual Studio)]: sql-data-warehouse-query-visual-studio.md
[migreren]: sql-data-warehouse-overview-migrate.md
[ontwikkelen]: sql-data-warehouse-overview-develop.md
[laden]: sql-data-warehouse-overview-load.md
[voorbeeldgegevens laden]: sql-data-warehouse-load-sample-databases.md
[Een logische Azure SQL-databaseserver met de Azure-portal]: ../sql-database/sql-database-get-started.md#create-an-azure-sql-database-logical-server
[Een logische Azure SQL-databaseserver maken met PowerShell]: ../sql-database/sql-database-get-started-powershell.md#database-setup-create-a-resource-group-server-and-firewall-rule
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



<!--HONumber=ago16_HO5-->


