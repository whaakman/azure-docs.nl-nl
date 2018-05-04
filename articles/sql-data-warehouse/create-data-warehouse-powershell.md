---
title: 'Snelstartgids: Maak een Azure SQL datawarehouse - Azure Powershell | Microsoft Docs'
description: Een logische SQL Database-server, een firewallregel op serverniveau en een datawarehouse snel maken met Azure PowerShell.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: e0bb014ec0706d458ff2f38e409efba5d66aaf18
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="quickstart-create-and-query-an-azure-sql-data-warehouse-with-azure-powershell"></a>Snelstartgids: Maken en query uitvoeren op een Azure SQL datawarehouse met Azure PowerShell

Een Azure SQL datawarehouse met Azure PowerShell om snel maken.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

Deze zelfstudie vereist Azure PowerShell-moduleversie 5.1.1 of hoger. Voer `Get-Module -ListAvailable AzureRM` vinden van de versie u hebt op dit moment. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). 


> [!NOTE]
> Het maken van een SQL Data Warehouse kan leiden tot een nieuwe factureerbare service.  Zie [Prijzen van SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) voor meer informatie.
>
>

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) en volg de instructies op het scherm.

```powershell
Add-AzureRmAccount
```

Als u wilt zien welke abonnement u gebruikt, [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription).

```powershell
Get-AzureRmSubscription
```

Als u gebruiken een ander abonnement dan de standaardwaarde wilt, voert u [Select-AzureRmSubscription](/powershell/module/azurerm.profile/select-azurermsubscription).

```powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```


## <a name="create-variables"></a>Variabelen maken

In deze snelstart leert u om variabelen te definiëren voor gebruik in de scripts.

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# The login information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDataWarehosue"
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Een logische server maken

Maak een [logische Azure SQL-server](../sql-database/sql-database-servers-databases.md#what-is-an-azure-sql-logical-server) met behulp van de [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) opdracht. Een logische server bevat een groep met databases die worden beheerd als groep. In het volgende voorbeeld wordt een server met een willekeurige naam gemaakt in de resourcegroep met een beheerdersaccount met de gebruikersnaam `ServerAdmin` en het wachtwoord `ChangeYourAdminPassword1`. U kunt deze vooraf gedefinieerde waarden vervangen.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Een serverfirewallregel configureren

Maak een [Azure SQL firewallregel op serverniveau](../sql-database/sql-database-firewall-configure.md) met behulp van de [nieuw AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) opdracht. Een firewallregel op serverniveau kunt een externe toepassing, zoals SQL Server Management Studio of het hulpprogramma SQLCMD verbinding maken met een SQL datawarehouse via de firewall van de service SQL Data Warehouse. In het volgende voorbeeld wordt de firewall alleen geopend voor andere Azure-resources. Voor externe connectiviteit wijzigt u het IP-adres in een correct adres voor uw omgeving. Als u alle IP-adressen wilt openen, gebruikt u 0.0.0.0 als beginadres en 255.255.255.255 als eindadres.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL-Database en SQL Data Warehouse communiceren via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. Als dit het geval is, is het niet mogelijk verbinding maken met uw Azure SQL-server, tenzij uw IT-afdeling poort 1433 wordt geopend.
>


## <a name="create-a-data-warehouse-with-sample-data"></a>Een datawarehouse met voorbeeldgegevens maken
In dit voorbeeld maakt u een datawarehouse met behulp van de eerder gedefinieerde variabelen.  Dit geeft de servicedoelstelling als DW400, dit een startpunt voor de lagere kosten voor uw datawarehouse is. 

```Powershell
New-AzureRmSqlDatabase `
    -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -Edition "DataWarehouse" `
    -RequestedServiceObjectiveName "DW400" `
    -CollationName "SQL_Latin1_General_CP1_CI_AS" `
    -MaxSizeBytes 10995116277760
```

De vereiste parameters zijn:

* **RequestedServiceObjectiveName**: de hoeveelheid [datawarehouse eenheden](what-is-a-data-warehouse-unit-dwu-cdwu.md) u hebt aangevraagd. Deze hoeveelheid verhoogt, compute-kosten. Zie voor een lijst met ondersteunde waarden [geheugen en gelijktijdigheid limieten](memory-and-concurrency-limits.md).
* **DatabaseName**: de naam van de SQL Data Warehouse die u maakt.
* **ServerName**: de naam van de server die u gebruikt voor het maken van.
* **ResourceGroupName**: de resourcegroep die u gebruikt. Gebruik Get-AzureResource om beschikbare resourcegroepen in uw abonnement te zoeken.
* **Edition**: moet DataWarehouse zijn, als u een SQL Data Warehouse wilt maken.

De optionele parameters zijn:

- **CollationName**: de standaardsortering, indien niet opgegeven, is COLLATE SQL_Latin1_General_CP1_CI_AS. De sortering kan niet worden gewijzigd voor een database.
- **MaxSizeBytes**: de maximale grootte van een database is standaard 10 GB.

Zie voor meer informatie over de parameteropties [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase).


## <a name="clean-up-resources"></a>Resources opschonen

Andere zelfstudies in deze verzameling zijn gebaseerd op deze snelstart. 

> [!TIP]
> Als u van plan bent om door te gaan met andere zelfstudies, verwijder dan niet de resources die u in deze snelstart hebt gemaakt. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in Azure Portal zijn gemaakt.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Volgende stappen

U hebt nu een datawarehouse gemaakt, een firewallregel gemaakt die is verbonden met uw datawarehouse, en enkele query's uitgevoerd. Voor meer informatie over Azure SQL Data Warehouse gaat u verder met de zelfstudie voor het laden van gegevens.
> [!div class="nextstepaction"]
>[Gegevens laden in een SQL-datawarehouse](load-data-from-azure-blob-storage-using-polybase.md)
