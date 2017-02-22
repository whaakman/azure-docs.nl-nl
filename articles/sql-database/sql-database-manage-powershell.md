---
title: Azure SQL Database beheren met PowerShell | Microsoft Docs
description: Azure SQL Database-beheer met PowerShell.
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: 3f21ad5e-ba99-4010-b244-5e5815074d31
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/15/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: adad6b8e27e0996559d5e6dacb8dd60fbf52a631
ms.openlocfilehash: 0c1ce1c29e447d9db4ef0df7873ef89cb835abee


---
# <a name="managing-azure-sql-database-using-powershell"></a>Azure SQL Database beheren met PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](sql-database-manage-portal.md)
> * [Transact-SQL (SSMS)](sql-database-manage-azure-ssms.md)
> * [PowerShell](sql-database-manage-powershell.md)
> 
> 

In dit ontwerp ontdekt u welke PowerShell-cmdlets worden gebruikt om u verschillende Azure SQL Database-taken uit te voeren. Zie [Azure SQL Database-cmdlets](https://msdn.microsoft.com/library/mt574084\(v=azure.300\).aspx) voor een volledige lijst.

> [!TIP]
> Zie de [zelfstudie Aan de slag](sql-database-get-started-powershell.md) voor een zelfstudie over het maken van een server, het maken van een op een server gebaseerde firewall, het weergeven van servereigenschappen, het maken van verbinding met en het uitvoeren van query's op de hoofddatabase, het maken van een voorbeelddatabase, het uitvoeren van query’s naar database-eigenschappen en het maken van verbinding met en het uitvoeren van query’s op de voorbeelddatabase.
>

## <a name="how-do-i-create-a-resource-group"></a>Hoe maak ik een resourcegroep?
Als u een resourcegroep wilt maken voor uw SQL Database en gerelateerde Azure-resources, gebruikt u de cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt759837\(v=azure.300\).aspx).

```
$resourceGroupName = "resourcegroup1"
$resourceGroupLocation = "northcentralus"
New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
```

Zie [Azure PowerShell gebruiken met Azure Resource Manager](../powershell-azure-resource-manager.md) voor meer informatie.
Zie [Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure PowerShell](sql-database-get-started-powershell.md) voor een volledige zelfstudie.

## <a name="how-do-i-create-a-sql-database-server"></a>Hoe maak ik een SQL Database-server?
Als u een SQL Database-server wilt maken, gebruikt u de cmdlet [New-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715\(v=azure.300\).aspx). Vervang *server1* met de naam voor uw server. De servernamen moeten uniek zijn voor alle Azure SQL Database-servers. U krijgt een foutmelding als de servernaam al in gebruik is. Het uitvoeren van deze opdracht kan enkele minuten in beslag nemen. De resourcegroep moet al bestaan voor uw abonnement.

```
$resourceGroupName = "resourcegroup1"

$sqlServerName = "server1"
$sqlServerVersion = "12.0"
$sqlServerLocation = "northcentralus"
$serverAdmin = "loginname"
$serverPassword = "password" 
$securePassword = ConvertTo-SecureString -String $serverPassword -AsPlainText -Force
$creds = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $serverAdmin, $securePassword


$sqlServer = New-AzureRmSqlServer -ServerName $sqlServerName `
 -SqlAdministratorCredentials $creds -Location $sqlServerLocation `
 -ResourceGroupName $resourceGroupName -ServerVersion $sqlServerVersion
```

Zie [SQL Database-functies](sql-database-features.md) voor meer informatie over servers. Zie [Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure PowerShell](sql-database-get-started-powershell.md) voor een volledige zelfstudie.

## <a name="how-do-i-create-a-sql-database-server-firewall-rule"></a>Hoe maak ik een firewallregel voor een SQL Database-server?
Gebruik de cmdlet [New-AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860\(v=azure.300\).aspx) als u een nieuwe firewallregel wilt maken voor toegang tot de server. Voer de volgende opdracht uit, maar vervang het IP-beginadres en het IP-eindadres door geldige waarden voor uw client. De resourcegroep en server moeten al bestaan voor uw abonnement.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$firewallRuleName = "firewallrule1"
$firewallStartIp = "0.0.0.0"
$firewallEndIp = "255.255.255.255"

New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -FirewallRuleName $firewallRuleName `
 -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
```

Als u andere Azure-services toegang wilt verlenen tot uw server, maakt u een firewallregel en stelt u `-StartIpAddress` en `-EndIpAddress` in op **0.0.0.0**. Met deze speciale firewallregel geeft u al het Azure-verkeer toegang tot de server.

Zie [Azure SQL Database-firewall](https://msdn.microsoft.com/library/azure/ee621782.aspx) voor meer informatie. Zie [Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure PowerShell](sql-database-get-started-powershell.md) voor een volledige zelfstudie.

## <a name="how-do-i-create-a-sql-database"></a>Hoe maak ik een SQL Database?
Als u een SQL Database wilt maken, gebruikt u de cmdlet [New-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339\(v=azure.300\).aspx). De resourcegroep en server moeten al bestaan voor uw abonnement. 

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Standard"
$databaseServiceLevel = "S0"

$currentDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Zie [Wat is SDL Database?](sql-database-technical-overview.md) voor meer informatie. Zie [Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure PowerShell](sql-database-get-started-powershell.md) voor een volledige zelfstudie.

## <a name="how-do-i-change-the-performance-level-of-a-sql-database"></a>Hoe wijzig ik het prestatieniveau van een SQL Database?
Als u het prestatieniveau wilt wijzigen, schaalt u de database omhoog of omlaag met de cmdlet [Set-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433\(v=azure.300\).aspx). De resourcegroep, server en database moeten al bestaan voor uw abonnement. Stel `-RequestedServiceObjectiveName` in als een spatie (zoals in het volgende fragment) voor de laag Basic. Stel het volgende in (*S0*, *S1*, *P1*, *P6*, etc.) voor andere lagen, net als in het voorgaande voorbeeld.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

$databaseName = "database1"
$databaseEdition = "Basic"
$databaseServiceLevel = " "

Set-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName `
 -ServerName $sqlServerName -DatabaseName $databaseName `
 -Edition $databaseEdition -RequestedServiceObjectiveName $databaseServiceLevel
```

Zie [SQL Database-opties en prestaties: wat is er beschikbaar in elke servicelaag](sql-database-service-tiers.md) voor meer informatie. Zie [PowerShell-voorbeeldscript om de servicelaag en het prestatieniveau te wijzigen voor uw SQL Database](sql-database-scale-up-powershell.md#sample-powershell-script-to-change-the-service-tier-and-performance-level-of-your-sql-database) voor een voorbeeldscript.

## <a name="how-do-i-copy-a-sql-database-to-the-same-server"></a>Hoe kopieer ik een SQL Database naar dezelfde server?
Als u een SQL Database naar dezelfde server wilt kopiëren, gebruikt u de cmdlet [New-AzureRmSqlDatabaseCopy](https://msdn.microsoft.com/library/azure/mt603644\(v=azure.300\).aspx). Stel `-CopyServerName` en `-CopyResourceGroupName` in op dezelfde waarden als die van uw brondatabaseserver en resourcegroep.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

$copyDatabaseName = "database1_copy"
$copyServerName = $sqlServerName
$copyResourceGroupName = $resourceGroupName

New-AzureRmSqlDatabaseCopy -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName `
 -CopyDatabaseName $copyDatabaseName -CopyServerName $sqlServerName `
 -CopyResourceGroupName $copyResourceGroupName
```

Zie [Een Azure SQL Database kopiëren](sql-database-copy.md) voor meer informatie. Zie [Een SQL Database PowerShell-script kopiëren](sql-database-copy-powershell.md#example-powershell-script) voor een voorbeeldscript.

## <a name="how-do-i-delete-a-sql-database"></a>Hoe verwijder ik een SQL-database?
Een SQL-database verwijderen: gebruik de cmdlet [Remove-AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368\(v=azure.300\).aspx). De resourcegroep, server en database moeten al bestaan voor uw abonnement.

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"
$databaseName = "database1"

Remove-AzureRmSqlDatabase -DatabaseName $databaseName `
 -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-delete-a-sql-database-server"></a>Hoe verwijder ik een SQL Database-server?
Als u een SQL Database-server wilt verwijderen, gebruikt u de cmdlet [Remove-AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488\(v=azure.300\).aspx).

```
$resourceGroupName = "resourcegroup1"
$sqlServerName = "server1"

Remove-AzureRmSqlServer -ServerName $sqlServerName -ResourceGroupName $resourceGroupName
```

## <a name="how-do-i-create-and-manage-elastic-pools-using-powershell"></a>Hoe maak en beheer ik elastische pools met PowerShell?
Zie [Een nieuwe elastische pool maken met PowerShell](sql-database-elastic-pool-create-powershell.md) voor meer informatie over het maken van elastische pools met PowerShell.

Zie [Een elastische pool bewaken en beheren met PowerShell](sql-database-elastic-pool-manage-powershell.md) voor meer informatie over het beheren van elastische pools met PowerShell.

## <a name="related-information"></a>Gerelateerde informatie
* [Azure SQL Database-cmdlets](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx)
* [Azure-cmdlet-naslaginformatie](https://msdn.microsoft.com/library/azure/dn708514\(v=azure.300\).aspx)




<!--HONumber=Dec16_HO3-->


