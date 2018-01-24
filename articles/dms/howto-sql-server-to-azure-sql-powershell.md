---
title: Gebruik de module Azure migratie databaseservice in Microsoft Azure PowerShell voor het migreren van SQL Server on-premises naar Azure SQL DB | Microsoft Docs
description: Informatie over het migreren van on-premises SQL-Server naar Azure SQL met behulp van Azure PowerShell.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: 
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 01/24/2018
ms.openlocfilehash: 8569bf65d04f677a45935284dc61d68879014c10
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-db-using-azure-powershell"></a>SQL Server on-premises, migreren naar Azure SQL-database met Azure PowerShell
In dit artikel leert u de migratie de **Adventureworks2012** database teruggezet naar een lokaal exemplaar van SQL Server 2016 of hoger naar een Azure SQL Database met behulp van Microsoft Azure PowerShell. U kunt databases van een lokale SQL Server-instantie migreren naar Azure SQL Database met behulp van de `AzureRM.DataMigration` -module in Microsoft Azure PowerShell.

In dit artikel leert u hoe:
> [!div class="checklist"]
> * Maak een resourcegroep.
> * Geen exemplaar maken van de Service Azure Database migratie.
> * Maak een migratieproject in een Azure-Database migratieservice-exemplaar.
> * Voer de migratie uit.

## <a name="prerequisites"></a>Vereisten
Deze stappen uit te voeren, hebt u het volgende nodig:

- [SQL Server 2016 of hoger](https://www.microsoft.com/sql-server/sql-server-downloads) (alle versies)
- TCP/IP-protocol is standaard SQL Server Express-installatie uitgeschakeld. Deze inschakelen door de [instructies in dit artikel](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Voor het configureren van uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Een Azure SQL Database-exemplaar. U kunt een Azure SQL Database-exemplaar maken door de details in het artikel [maken van een Azure SQL database in de Azure portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- [Migratie-assistent gegevens](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 of hoger.
- De Service Azure Database migratie vereist een VNET gemaakt met behulp van het Azure Resource Manager-implementatiemodel, waardoor site-naar-site-verbinding met uw on-premises bronservers met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [ VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- De beoordeling van de lokale database en het schema migratie met behulp van migratie-assistent gegevens zoals beschreven in het artikel is voltooid [ uitvoeren van een beoordeling van de migratie van SQL Server](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)
- Download en installeer de module AzureRM.DataMigration via de PowerShell Gallery door [Install-Module PowerShell-cmdlet](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1)
- De referenties waarmee verbinding met bron SQL Server-exemplaar moeten hebben de [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) machtiging.
- De referenties waarmee verbinding met Azure SQL DB doelexemplaar moeten de machtiging beheer DATABASE hebben op de doel-Azure SQL Database-databases.
- Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Aanmelden bij uw Microsoft Azure-abonnement
Gebruik de instructies in het artikel [aanmelden met Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-4.4.1) aan te melden bij uw Azure-abonnement met behulp van PowerShell.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Een resourcegroep maken voordat u een virtuele machine kunt maken.

Een resourcegroep maken met behulp van de [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) opdracht. 

Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *EastUS* regio.

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```
## <a name="create-an-azure-database-migration-service-instance"></a>Azure-Database migratieservice-exemplaar maken 
U kunt nieuwe exemplaar van Azure Database migratieservice maken met behulp van de `New-AzureRmDataMigrationService` cmdlet. Deze cmdlet verwacht dat de volgende parameters vereist:
- *De naam van de Azure-resourcegroep*. U kunt [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) opdracht voor het maken van Azure-resourcegroep zoals eerder weergegeven en geeft de naam op als een parameter.
- *Servicenaam*. Tekenreeks die overeenkomt met de gewenste unieke servicenaam voor migratie van Azure databaseservice 
- *Locatie*. Hiermee geeft u de locatie van de service. Geef een locatie van de Azure data center zoals VS-West of Zuidoost-Azië
- *SKU*. Deze parameter komt overeen met de naam van de DMS-Sku. Ondersteunde Sku-namen zijn *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*
- *Virtuele Subnet-id*. U kunt de cmdlet gebruiken [nieuw AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-4.4.1) om een subnet te maken. 

Het volgende voorbeeld wordt een service met de naam *MyDMS* in de resourcegroep *MyDMSResourceGroup*, die zich bevindt in *VS-Oost* regio een virtueel netwerk met de naam *MyVNET* en subnet met de naam *MySubnet*.

```powershell
 $vNet = Get-AzureRmVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzureRmDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Een migratieproject maken
Na het maken van een Azure-Database migratieservice-exemplaar, moet u een migratieproject maken. Een Azure-serviceproject Database migratie vereist verbindingsgegevens voor zowel de bron en doel-exemplaren, evenals een lijst met databases die u wilt migreren als onderdeel van het project.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Een object van het Database-verbindingsgegevens voor de bron en doel-verbindingen maken
U kunt een verbindingsgegevens van de Database-object maken met behulp van de `New-AzureRmDmsConnInfo` cmdlet. Deze cmdlet verwacht dat de volgende parameters:
- *ServerType*. Het type van de verbinding met database aangevraagd, bijvoorbeeld SQL, Oracle of MySQL. Gebruik SQL voor SQL Server en SQL Azure.
- *Gegevensbron*. De naam of IP-adres van een SQL-exemplaar of Azure SQL-server.
- *AuthType*. Het verificatietype dat voor de verbinding die SqlAuthentication of WindowsAuthentication kan zijn.
- *TrustServerCertificate* parameter stelt een waarde die aangeeft of het kanaal wordt versleuteld tijdens het omzeilen van roulatie van de certificaatketen voor het valideren van de vertrouwensrelatie. Mogelijke waarden zijn true of false.

Het volgende voorbeeld wordt een object verbindingsgegevens voor de gegevensbron SQL Server met de naam MySQLSourceServer via sql-verificatie 

```powershell
$sourceConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource MySQLSourceServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Volgende voorbeeld ziet u het maken van verbindingsgegevens voor Azure SQL database-server genoemd MySQLAzureTarget via sql-verificatie

```powershell
$targetConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource "mysqlazuretarget.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Databases voor het migratieproject opgeven
Maak een lijst van `AzureRmDataMigrationDatabaseInfo` objecten die Hiermee geeft u de databases als onderdeel van de migratie van de Database Azure serviceproject die kunnen worden opgegeven als parameter voor het maken van het project. Cmdlet `New-AzureRmDataMigrationDatabaseInfo` kan worden gebruikt voor het maken van AzureRmDataMigrationDatabaseInfo. 

Het volgende voorbeeld wordt `AzureRmDataMigrationDatabaseInfo` project voor de database AdventureWorks2016 en voegt het toe aan de lijst om te worden opgegeven als parameter voor het maken van het project.

```powershell
$dbInfo1 = New-AzureRmDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks2016
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Een projectobject maken
Ten slotte kunt u Azure databasemigratie project met de naam *MyDMSProject* zich in *VS-Oost* met `New-AzureRmDataMigrationProject` en het toevoegen van de eerder gemaakte bron en doel-verbindingen en de lijst met databases te migreren.

```powershell
$project = New-AzureRmDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLDB `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfos $dbList
```

## <a name="create-and-start-a-migration-task"></a>Maak en start een migratietaak
Ten slotte maakt en start de migratietaak van Azure-Database. Azure-Database migratietaak vereist verbinding referentie-informatie voor zowel bron- en doel en lijst met databasetabellen naast de informatie die al zijn opgegeven aan het project is gemaakt als een vereiste worden gemigreerd. 

### <a name="create-credential-parameters-for-source-and-target"></a>Referentie-parameters voor de bron en doel maken
Beveiligingsreferenties van de verbinding kunnen worden gemaakt als een [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) object. 

Het volgende voorbeeld toont het maken van *PSCredential* objecten voor de bron- en doelserver verbindingen bieden wachtwoorden als tekenreeksvariabelen *$sourcePassword* en *$ doelwachtwoord*. 

```powershell
secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-table-map-and-select-source-and-target-parameters-for-migration"></a>Maak een tabel-kaart en selecteer parameters voor de bron en doel voor de migratie
Een andere parameter die nodig zijn voor de migratie is toewijzing van de tabellen van bron naar doel worden gemigreerd. Woordenlijst waarmee een toewijzing tussen bron- en doeltabellen voor migratie van tabellen maken. Het volgende voorbeeld wordt de toewijzing tussen bron- en doeltabellen Human Resources-schema voor de database AdventureWorks 2016.

```powershell
$tableMap = New-Object 'system.collections.generic.dictionary[string,string]'
$tableMap.Add("HumanResources.Department", "HumanResources.Department")
$tableMap.Add("HumanResources.Employee","HumanResources.Employee")
$tableMap.Add("HumanResources.EmployeeDepartmentHistory","HumanResources.EmployeeDepartmentHistory")
$tableMap.Add("HumanResources.EmployeePayHistory","HumanResources.EmployeePayHistory")
$tableMap.Add("HumanResources.JobCandidate","HumanResources.JobCandidate")
$tableMap.Add("HumanResources.Shift","HumanResources.Shift")
```

De volgende stap is het selecteren van de bron en doel-databases en geef tabeltoewijzing als u wilt migreren als een parameter met behulp van de `New-AzureRmDmsSqlServerSqlDbSelectedDB` cmdlet, zoals weergegeven in het volgende voorbeeld:

```powershell
$selectedDbs = New-AzureRmDmsSqlServerSqlDbSelectedDB -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -TableMap $tableMap
```

### <a name="create-and-start-a-migration-task"></a>Maak en start een migratietaak

Gebruik de `New-AzureRmDataMigrationTask` cmdlet voor het maken en starten van een migratietaak. Deze cmdlet verwacht dat de volgende parameters:
- *TaskType*. Type migratietaak te maken voor SQL Server naar SQL Azure Migratietype *MigrateSqlServerSqlDb* wordt verwacht. 
- *De Resourcegroepnaam*. Naam van de Azure-resourcegroep waarin u de taak te maken.
- *ServiceName*. Azure Migratieservice voor Database-exemplaar waarin de taak te maken.
- *ProjectName*. Naam van Azure databasemigratie project waarin de taak te maken. 
- *Taaknaam*. Naam van de taak moet worden gemaakt. 
- *Gegevensbronverbinding*. AzureRmDmsConnInfo-object dat de verbinding met gegevensbron vertegenwoordigt.
- *Doel verbinding*. AzureRmDmsConnInfo-object van doelverbinding.
- *SourceCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) object voor het verbinden met de bronserver.
- *TargetCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) object voor het verbinden met de doelserver.
- *SelectedDatabase*. AzureRmDmsSqlServerSqlDbSelectedDB-object dat de bron en doel-database-toewijzing vertegenwoordigt.

Het volgende voorbeeld wordt gemaakt en wordt gestart van een migratietaak met de naam myDMSTask:

```powershell
$migTask = New-AzureRmDataMigrationTask -TaskType MigrateSqlServerSqlDb `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs`
```

## <a name="monitor-the-migration"></a>De migratie bewaken
U kunt de migratietaak uitgevoerd door het opvragen van de eigenschap state van de taak, zoals weergegeven in het volgende voorbeeld controleren:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="next-steps"></a>Volgende stappen
- Bekijk de migratie-instructies in de Microsoft [Database Migratiehandleiding](https://datamigration.microsoft.com/).