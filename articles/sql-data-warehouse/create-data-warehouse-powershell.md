---
title: 'Quickstart: Maken van een Azure SQL datawarehouse - Azure Powershell | Microsoft Docs'
description: Maak snel een logische SQL Database-server, firewallregel op serverniveau en datawarehouse met Azure PowerShell.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 11/15/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 5ef8c8f32422352c01b8c7ceb4811863faad4e57
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57308768"
---
# <a name="quickstart-create-and-query-an-azure-sql-data-warehouse-with-azure-powershell"></a>Quickstart: Maken en het opvragen van een Azure SQL datawarehouse met Azure PowerShell

Maak snel een Azure SQL datawarehouse met behulp van Azure PowerShell.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

> [!NOTE]
> Het maken van een SQL Data Warehouse kan leiden tot een nieuwe factureerbare service.  Zie [Prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) voor meer informatie.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Aanmelden bij uw Azure-abonnement met de [toevoegen AzAccount](/powershell/module/az.profile/add-azaccount) opdracht en volgt u de op het scherm aanwijzingen.

```powershell
Add-AzAccount
```

Als u wilt zien welk abonnement u gebruikt, [Get-AzSubscription](/powershell/module/az.profile/get-azsubscription).

```powershell
Get-AzSubscription
```

Als u gebruiken een ander abonnement dan de standaard wilt, voert u [Set AzContext](/powershell/module/az.profile/set-azcontext).

```powershell
Set-AzContext -SubscriptionName "MySubscription"
```


## <a name="create-variables"></a>Variabelen maken

In deze snelstart leert u om variabelen te definiëren voor gebruik in de scripts.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (don't capitalize)
$servername = "server-$(Get-Random)"
# Set an admin name and password for your database
# The sign-in information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDataWarehosue"
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met behulp van de [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) opdracht. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

```powershell
New-AzResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Een logische server maken

Maak een [logische Azure SQL-server](../sql-database/sql-database-logical-servers.md) met behulp van de [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) opdracht. Een logische server bevat een groep met databases die worden beheerd als groep. Het volgende voorbeeld wordt een willekeurige naam server in de resourcegroep met een gebruiker met de naam `ServerAdmin` en het wachtwoord `ChangeYourAdminPassword1`. U kunt deze vooraf gedefinieerde waarden vervangen.

```powershell
New-AzSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Een serverfirewallregel configureren

Maak een [Azure SQL-firewallregel op serverniveau](../sql-database/sql-database-firewall-configure.md) met behulp van de [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) opdracht. Een firewallregel op serverniveau kan een externe toepassing, zoals SQL Server Management Studio of het hulpprogramma SQLCMD verbinding maken met een SQL datawarehouse via de firewall van de service SQL Data Warehouse. In het volgende voorbeeld wordt de firewall alleen geopend voor andere Azure-resources. Voor externe connectiviteit wijzigt u het IP-adres in een correct adres voor uw omgeving. Als u alle IP-adressen wilt openen, gebruikt u 0.0.0.0 als beginadres en 255.255.255.255 als eindadres.

```powershell
New-AzSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL-Database en SQL Data Warehouse communiceren via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. Als dit het geval is, kunt u zich niet verbinden met uw Azure SQL-server als uw IT-afdeling poort 1433 openstelt.
>


## <a name="create-a-data-warehouse-with-sample-data"></a>Een datawarehouse maken met voorbeeldgegevens
Dit voorbeeld maakt u een datawarehouse met behulp van de eerder gedefinieerde variabelen.  Het geeft de servicedoelstelling als DW400, dit een startpunt voor de lagere kosten voor uw datawarehouse is. 

```Powershell
New-AzSqlDatabase `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -Edition "DataWarehouse" `
    -RequestedServiceObjectiveName "DW400" `
    -CollationName "SQL_Latin1_General_CP1_CI_AS" `
    -MaxSizeBytes 10995116277760
```

De vereiste parameters zijn:

* **RequestedServiceObjectiveName**: De hoeveelheid [datawarehouse-eenheden](what-is-a-data-warehouse-unit-dwu-cdwu.md) u aanvragen. Dit bedrag verhoogt, kosten. Zie voor een lijst van ondersteunde waarden, [limieten voor geheugen en gelijktijdigheid](memory-and-concurrency-limits.md).
* **DatabaseName**: De naam van de SQL Data Warehouse die u maakt.
* **ServerName**: De naam van de server die u voor het maken van gebruikt.
* **ResourceGroupName**: De resourcegroep die u gebruikt. Gebruik Get-AzureResource om beschikbare resourcegroepen in uw abonnement te zoeken.
* **Editie**: 'DataWarehouse' moet zijn om te maken van een SQL Data Warehouse.

De optionele parameters zijn:

- **Sorteringnaam**: De standaardsortering, indien niet opgegeven is SQL_Latin1_General_CP1_CI_AS. Sortering kan niet worden gewijzigd voor een database.
- **MaxSizeBytes**: De standaard maximale grootte van een database is 10 GB.

Zie voor meer informatie over de parameteropties [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase).


## <a name="clean-up-resources"></a>Resources opschonen

Andere zelfstudies in deze verzameling zijn gebaseerd op deze snelstart. 

> [!TIP]
> Als u van plan bent om door te gaan werken met latere zelfstudies, niet schoon dan de resources die zijn gemaakt in deze Quick Start. Als u niet van plan bent om door te gaan, gebruikt u de volgende stappen uit om alle resources die zijn gemaakt door deze Quick Start in de Azure-portal te verwijderen.
>

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Volgende stappen

U hebt nu een datawarehouse, een firewallregel, verbonden met uw datawarehouse gemaakt gemaakt en enkele query's uitvoeren. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.
> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
