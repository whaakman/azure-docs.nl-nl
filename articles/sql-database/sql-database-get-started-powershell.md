---
title: 'Azure PowerShell: een SQL-database maken | Microsoft Docs'
description: Meer informatie over hoe u met behulp van Azure Portal een logische SQL Database-server, een firewallregel op serverniveau en databases maakt.
keywords: zelfstudie over sql-database, een sql-database maken
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.translationtype: Human Translation
ms.sourcegitcommit: 8be2bcb9179e9af0957fcee69680ac803fd3d918
ms.openlocfilehash: e43f2f6bb6d4f7e280f8874c31ec79c01b54a84b
ms.contentlocale: nl-nl
ms.lasthandoff: 06/23/2017

---

<a id="create-a-single-azure-sql-database-using-powershell" class="xliff"></a>

# Een individuele Azure SQL Database maken met PowerShell

PowerShell wordt gebruikt voor het maken en beheren van Azure-resources vanaf de opdrachtregel of in scripts. Deze handleiding geeft meer informatie over het gebruik van PowerShell voor het implementeren van een Azure SQL-database in een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) in een [logische Azure SQL Database-server](sql-database-features.md).

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

Voor deze zelfstudie is module versie 4.0 of hoger van Azure PowerShell vereist. Voer ` Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). 

<a id="log-in-to-azure" class="xliff"></a>

## Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) en volg de instructies op het scherm.

```powershell
Add-AzureRmAccount
```

<a id="create-variables" class="xliff"></a>

## Variabelen maken

Definieer variabelen voor gebruik in de scripts in deze Quick Start.

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
$databasename = "mySampleDatabase"
```

<a id="create-a-resource-group" class="xliff"></a>

## Een resourcegroep maken

Maak een [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en groepsgewijs worden beheerd. In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt op de locatie `westeurope`.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
<a id="create-a-logical-server" class="xliff"></a>

## Een logische server maken

Als u een [logische Azure SQL Database-server](sql-database-features.md) wilt maken, gebruikt u de opdracht [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver). Een logische server bevat een groep met databases die worden beheerd als groep. In het volgende voorbeeld wordt een server met een willekeurige naam gemaakt in de resourcegroep met een beheerdersaccount met de gebruikersnaam `ServerAdmin` en het wachtwoord `ChangeYourAdminPassword1`. U kunt deze vooraf gedefinieerde waarden vervangen.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

<a id="configure-a-server-firewall-rule" class="xliff"></a>

## Een serverfirewallregel configureren

Maak een [firewallregel op Azure SQL Database-serverniveau](sql-database-firewall-configure.md) met de opdracht [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule). Een firewallregel op serverniveau kan een externe toepassing, zoals SQL Server Management Studio of het hulpprogramma SQLCMD verbinding laten maken met een SQL-database via de firewall van de SQL Database-service. In het volgende voorbeeld wordt de firewall alleen geopend voor andere Azure-resources. Voor externe connectiviteit wijzigt u het IP-adres in een correct adres voor uw omgeving. Als u alle IP-adressen wilt openen, gebruikt u 0.0.0.0 als beginadres en 255.255.255.255 als eindadres.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u alleen verbinding maken met uw Azure SQL Database-server als uw IT-afdeling poort 1433 openstelt.
>

<a id="create-a-database-in-the-server-with-sample-data" class="xliff"></a>

## Een database op de server maken met voorbeeldgegevens

Maak een database met een [prestatieniveau van S0](sql-database-service-tiers.md) in de server met de opdracht [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase). In het volgende voorbeeld wordt een database met de naam `mySampleDatabase` gemaakt en worden de gegevens uit het AdventureWorksLT-voorbeeld in deze database geladen. U kunt deze vooraf gedefinieerde waarden desgewenst vervangen. (Andere Quick Starts in deze verzameling zijn echter op de waarden in deze Quick Start gebaseerd.)

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

<a id="clean-up-resources" class="xliff"></a>

## Resources opschonen

Andere Quick Starts in deze verzameling zijn op deze Quick Start gebaseerd. 

> [!TIP]
> Als u van plan bent om door te gaan met andere Quick Starts, verwijdert u de resources die u in deze Quick Start hebt gemaakt niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die door deze Quick Start in Azure Portal zijn gemaakt.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

<a id="next-steps" class="xliff"></a>

## Volgende stappen

Nu u een database hebt, kunt u verbinding maken met een hulpprogramma naar keuze en hiermee query's uitvoeren. Klik op de onderstaande hulpprogramma's voor meer informatie:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)


