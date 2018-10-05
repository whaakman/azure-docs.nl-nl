---
title: Azure Database Migration Service-module gebruiken in Microsoft Azure PowerShell voor het migreren van SQL Server on-premises naar Azure SQL DB MI | Microsoft Docs
description: Meer informatie over het migreren van on-premises SQL Server naar Azure SQL DB MI met behulp van Azure PowerShell.
services: database-migration
author: HJToland3
ms.author: rajpo
manager: ''
ms.reviewer: ''
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 08/13/2018
ms.openlocfilehash: c8747b7b8125f097fab3752693f4f14440ed7ce7
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48804375"
---
# <a name="migrate-sql-server-on-premises-to-azure-sql-db-using-azure-powershell"></a>On-premises SQL Server migreren naar Azure SQL-database met behulp van Azure PowerShell
In dit artikel, migreert u de **Adventureworks2012** database hersteld naar een on-premises exemplaar van SQL Server 2005 of hoger met een Azure SQL Database met behulp van Microsoft Azure PowerShell. U kunt databases uit een on-premises SQL Server-exemplaar migreren naar Azure SQL Database met behulp van de `AzureRM.DataMigration` module in Microsoft Azure PowerShell.

In dit artikel leert u het volgende:
> [!div class="checklist"]
> * Maak een resourcegroep.
> * De Azure-portal gebruiken om een Azure Database Migration Service-exemplaar te maken.
> * Een migratieproject maken in een Azure Database Migration Service-exemplaar.
> * De migratie uitvoeren.

## <a name="prerequisites"></a>Vereisten
Als u wilt deze stappen hebt voltooid, hebt u het volgende nodig:

- [SQL Server 2016 of hoger](https://www.microsoft.com/sql-server/sql-server-downloads) (alle edities)
- Om in te schakelen het TCP/IP-protocol, dat standaard met SQL Server Express-installatie is uitgeschakeld. Het TCP/IP-protocol inschakelen door het artikel te volgen [in- of uitschakelen van een Server Network Protocol](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
- Het configureren van uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
- Een Azure SQL Database-exemplaar. U kunt een Azure SQL Database-exemplaar maken met de details in het artikel te volgen [maken van een Azure SQL database in Azure portal](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal).
- [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 of hoger.
- Een VNET gemaakt met behulp van de Azure Resource Manager-implementatiemodel, dat de Azure Database Migration Service met site-naar-site-verbinding met uw on-premises bronservers biedt met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Beoordeling van uw on-premises database en het schema een migratie met behulp van Data Migration Assistant zoals beschreven in het artikel hebt voltooid [ uitvoeren van een SQL Server-migratie-evaluatie](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem)
- Om te downloaden en installeren van de module AzureRM.DataMigration vanuit de PowerShell Gallery met behulp van [Install-Module PowerShell-cmdlet](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1)
- Om ervoor te zorgen dat de referenties waarmee verbinding met bron SQL Server-exemplaar heeft de [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) machtiging.
- Om ervoor te zorgen dat de referenties waarmee verbinding met doel-Azure SQL DB heeft exemplaar de machtiging beheer DATABASE voor de doel-Azure SQL Database-databases.
- Een Azure-abonnement. Als u niet hebt, maakt u een [gratis](https://azure.microsoft.com/free/) account voordat u begint.

## <a name="log-in-to-your-microsoft-azure-subscription"></a>Meld u aan bij uw Microsoft Azure-abonnement
Gebruik de instructies in het artikel [Meld u aan met Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-4.4.1) aanmelden bij uw Azure-abonnement met behulp van PowerShell.

## <a name="create-a-resource-group"></a>Een resourcegroep maken
Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Een resourcegroep maken voordat u een virtuele machine kunt maken.

Maak een resourcegroep met behulp van de [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) opdracht. 

Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *EastUS* regio.

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```
## <a name="create-an-instance-of-the-azure-database-migration-service"></a>Maak een instantie van de Azure Database Migration Service 
U kunt een nieuw exemplaar van Azure Database Migration Service maken met behulp van de `New-AzureRmDataMigrationService` cmdlet. Deze cmdlet wordt verwacht dat de volgende vereiste parameters:
- *De naam van de Azure-resourcegroep*. U kunt [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup?view=azurermps-4.4.1) opdracht voor het maken van Azure-resourcegroep als eerder weergegeven en geeft de naam op als een parameter.
- *Servicenaam*. Tekenreeks die overeenkomt met de naam van de gewenste unieke service voor Azure Database Migration Service 
- *Locatie*. Hiermee geeft u de locatie van de service. Geef een locatie van een Azure data center, zoals VS-West of Zuidoost-Azië
- *SKU*. Deze parameter komt overeen met de naam van de DMS-Sku. Namen van de momenteel ondersteunde Sku zijn *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*
- *Virtuele Subnet-id*. U kunt de cmdlet gebruiken [New-AzureRmVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?view=azurermps-4.4.1) om een subnet te maken. 

Het volgende voorbeeld wordt een service met de naam *MyDMS* in de resourcegroep *MyDMSResourceGroup* zich in de *VS-Oost* regio met behulp van een virtueel netwerk met de naam  *MyVNET* en subnet met de naam *MySubnet*.

```powershell
 $vNet = Get-AzureRmVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzureRmDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Maak een migratieproject
Na het maken van een Azure Database Migration Service-exemplaar, kunt u een migratieproject maken. Een Azure Database Migration Service-project vereist verbindingsgegevens voor zowel de bron en doel-instanties, evenals een lijst met databases die u wilt migreren als onderdeel van het project.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Een Database Connection-Info-object voor de bron en doel-verbindingen maken
U kunt een Database Connection-Info-object maken met behulp van de `New-AzureRmDmsConnInfo` cmdlet. Deze cmdlet wordt verwacht dat de volgende parameters:
- *ServerType*. Het type van de verbinding met database aangevraagd, bijvoorbeeld SQL, Oracle of MySQL. Gebruik SQL voor SQL Server en Azure SQL.
- *DataSource*. De naam of IP-adres van een SQL Server-exemplaar of een Azure SQL-database.
- *AuthType*. Het verificatietype voor verbinding, die SqlAuthentication of WindowsAuthentication kan zijn.
- *TrustServerCertificate* parameter stelt een waarde die aangeeft of het kanaal worden versleuteld terwijl het overslaan van de certificaatketen voor het valideren van vertrouwensrelatie lopen. Waarde kan true of false zijn.

Het volgende voorbeeld wordt een object van de verbindingsgegevens voor de bron-SQL-Server met de naam MySourceSQLServer met behulp van sql-verificatie: 

```powershell
$sourceConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Het volgende voorbeeld toont het maken van de verbindingsgegevens voor een Azure SQL Database Managed Instance-server met de naam targetmanagedinstance.database.windows.net met behulp van sql-verificatie:

```powershell
$targetConnInfo = New-AzureRmDmsConnInfo -ServerType SQL `
  -DataSource "targetmanagedinstance.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Databases bieden voor het migratieproject
Maak een lijst van `AzureRmDataMigrationDatabaseInfo` objecten die Hiermee geeft u de databases als onderdeel van de Azure Database Migration-project dat kunnen worden opgegeven als parameter voor het maken van het project. De Cmdlet `New-AzureRmDataMigrationDatabaseInfo` kan worden gebruikt voor het maken van AzureRmDataMigrationDatabaseInfo. 

Het volgende voorbeeld wordt `AzureRmDataMigrationDatabaseInfo` -project voor de **AdventureWorks** database en toegevoegd aan de lijst om te worden opgegeven als parameter voor het maken van het project.

```powershell
$dbInfo1 = New-AzureRmDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Een project-object maken
Ten slotte kunt u Azure Database Migration-project met de naam *MyDMSProject* zich in *VS-Oost* met behulp van `New-AzureRmDataMigrationProject` en de eerder gemaakte bron en doel-verbindingen en de lijst toe te voegen te migreren databases.

```powershell
$project = New-AzureRmDataMigrationProject -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName MyDMSProject `
  -Location EastUS `
  -SourceType SQL `
  -TargetType SQLMI `
  -SourceConnection $sourceConnInfo `
  -TargetConnection $targetConnInfo `
  -DatabaseInfo $dbList
```

## <a name="create-and-start-a-migration-task"></a>Maak en start u een migratietaak
Ten slotte maken en Azure Database Migration-taak te starten. Azure Database Migration-taak vereist verbinding referentie-informatie voor zowel bron- en doel en lijst met databasetabellen moeten worden gemigreerd naast de al voorzien van het project gemaakt als een vereiste informatie. 

### <a name="create-credential-parameters-for-source-and-target"></a>Referentie-parameters voor de bron en doel maken
Beveiligingsreferenties van de verbinding kunnen worden gemaakt als een [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) object. 

Het volgende voorbeeld toont het maken van *PSCredential* objecten voor zowel bron- en -verbindingen bieden wachtwoorden als tekenreeksvariabelen *$sourcePassword* en *$ doelwachtwoord*. 

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-backup-fileshare-object"></a>Back-up bestandsshare-object maken
Maak nu bestandsshare-object dat de lokale SMB-netwerkshare dat de Azure Database Migration Service duren de bron databaseback-ups voordat kan voor het gebruik van de cmdlet New-AzureRmDmsFileShare vertegenwoordigt.

```powershell
$backupPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$backupCred = New-Object System.Management.Automation.PSCredential ($backupUserName, $backupPassword)

$backupFileSharePath="\\10.0.0.76\SharedBackup"
$backupFileShare = New-AzureRmDmsFileShare -Path $backupFileSharePath -Credential $backupCred
```

### <a name="create-selected-database-object"></a>Geselecteerde database-object maken
De volgende stap is het selecteren van de bron- en -databases met behulp van de cmdlet New-AzureRmDmsSelectedDB, zoals wordt weergegeven in het volgende voorbeeld:

```powershell
$selectedDbs = @()
$selectedDbs += New-AzureRmDmsSelectedDB -MigrateSqlServerSqlDbMi `
  -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -BackupFileShare $backupFileShare `
```

### <a name="sas-uri-for-azure-storage-container"></a>SAS-URI voor Azure-Opslagcontainer
Maak de variabele met de SAS-URI die de Azure Database Migration Service toegang biedt tot de container van het opslagaccount waarnaar de service de back-upbestanden uploadt.

```powershell
$blobSasUri="https://mystorage.blob.core.windows.net/test?st=2018-07-13T18%3A10%3A33Z&se=2019-07-14T18%3A10%3A00Z&sp=rwdl&sv=2018-03-28&sr=c&sig=qKlSA512EVtest3xYjvUg139tYSDrasbftY%3D"
```

### <a name="select-logins"></a>Aanmeldingen selecteren
Overzicht van aanmeldingen moeten worden gemigreerd, zoals wordt weergegeven in onderstaande voorbeeld maken: Houd er rekening mee dat momenteel DMS ondersteunt migratie van SQL-aanmeldingen. 

```powershell
$selectedLogins = @("user1", "user2")
```

### <a name="select-agent-jobs"></a>Selecteer agent-taken
Lijst van de agent-taken moeten worden gemigreerd, zoals wordt weergegeven in onderstaande voorbeeld maken:

>[!NOTE]
>DMS ondersteunt momenteel de taken met T-SQL-subsysteem taakstappen alleen.

```powershell
$selectedAgentJobs = @("agentJob1", "agentJob2")
```

### <a name="create-and-start-a-migration-task"></a>Maak en start u een migratietaak

Gebruik de `New-AzureRmDataMigrationTask` cmdlet voor het maken en starten van een migratietaak. Deze cmdlet wordt verwacht dat de volgende parameters:
- *TaskType*. Type migratietaak maken voor SQL Server naar Azure SQL Database Managaged Instance Migratietype *MigrateSqlServerSqlDbMi* wordt verwacht. 
- *Naam resourcegroep*. De naam van de Azure-resourcegroep waarin de taak te maken.
- *ServiceName*. Azure Database Migration Service-instantie in voor het maken van de taak.
- *ProjectName*. Naam van Azure Database Migration Service-project waarin de taak te maken. 
- *Taaknaam*. De naam van de taak moet worden gemaakt. 
- *SourceConnection*. AzureRmDmsConnInfo-object voor bron SQL Server-verbinding.
- *TargetConnection*. AzureRmDmsConnInfo-object van doel-Azure SQL Database Managed Instance-verbinding.
- *SourceCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) object voor het verbinden met de bronserver.
- *TargetCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) object voor het verbinden met de doelserver.
- *SelectedDatabase*. AzureRmDataMigrationSelectedDB-object voor de bron- en database-toewijzing.
- *BackupFileShare*. FileShare-object dat het lokale netwerk vertegenwoordigt delen dat de Azure Database Migration Service kan duren voordat de bron databaseback-ups op.
- *BackupBlobSasUri*. De SAS-URI die de Azure Database Migration Service toegang biedt tot de container van het opslagaccount waarnaar de service de back-upbestanden uploadt. Informatie over het verkrijgen van de SAS-URI voor de blob-container.
- *SelectedLogins*. Lijst met geselecteerde aanmeldingen te migreren.
- *SelectedAgentJobs*. Lijst met geselecteerde agent-taken te migreren.

Het volgende voorbeeld maakt en een migratietaak met de naam myDMSTask begint:

```powershell
$migTask = New-AzureRmDataMigrationTask -TaskType MigrateSqlServerSqlDbMi `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs`
  -BackupFileShare $backupFileShare `
  -BackupBlobSasUri $blobSasUri `
  -SelectedLogins $selectedLogins `
  -SelectedAgentJobs $selectedJobs `
```

## <a name="monitor-the-migration"></a>Bewaak de migratie
U kunt de migratietaak uitgevoerd door het opvragen van de eigenschap state van de taak, zoals wordt weergegeven in het volgende voorbeeld controleren:

```powershell
if (($mytask.ProjectTask.Properties.State -eq "Running") -or ($mytask.ProjectTask.Properties.State -eq "Queued"))
{
  write-host "migration task running"
}
```

## <a name="next-steps"></a>Volgende stappen
- Bekijk de hulp bij de migratie in de Microsoft [handleiding voor databasemigratie](https://datamigration.microsoft.com/).