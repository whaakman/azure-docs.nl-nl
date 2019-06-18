---
title: SQL Server migreren naar Azure SQL Database Managed Instance met de Database migratieservice- en PowerShell | Microsoft Docs
description: Meer informatie over het migreren van on-premises SQL Server naar Azure SQL database Managed Instance met Azure PowerShell.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 04/29/2019
ms.openlocfilehash: d83410efd26f8c2078d3abdb01d061db0b83d33d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65233727"
---
# <a name="migrate-sql-server-on-premises-to-an-azure-sql-database-managed-instance-using-azure-powershell"></a>On-premises SQL Server migreren naar een beheerd exemplaar voor Azure SQL Database met behulp van Azure PowerShell
In dit artikel, migreert u de **Adventureworks2016** database hersteld naar een on-premises exemplaar van SQL Server 2005 of hoger met een Azure SQL-Database beheerd exemplaar met behulp van Microsoft Azure PowerShell. U databases kunt migreren vanaf een on-premises SQL Server-exemplaar naar een beheerd exemplaar voor Azure SQL Database met behulp van de `Az.DataMigration` module in Microsoft Azure PowerShell.

In dit artikel leert u het volgende:
> [!div class="checklist"]
>
> * Maak een resourcegroep.
> * Maak een instantie van Azure Database Migration Service.
> * Een migratieproject maken in een exemplaar van Azure Database Migration Service.
> * De migratie uitvoeren.

[!INCLUDE [online-offline](../../includes/database-migration-service-offline-online.md)]

Dit artikel bevat informatie over hoe u zowel online als offline migratie uitvoert.

## <a name="prerequisites"></a>Vereisten

Als u wilt deze stappen hebt voltooid, hebt u het volgende nodig:

* [SQL Server 2016 of hoger](https://www.microsoft.com/sql-server/sql-server-downloads) (alle edities).
* Een lokale kopie van de **AdventureWorks2016** -database, die gedownload is [hier](https://docs.microsoft.com/sql/samples/adventureworks-install-configure?view=sql-server-2017).
* Om in te schakelen het TCP/IP-protocol, dat standaard met SQL Server Express-installatie is uitgeschakeld. Het TCP/IP-protocol inschakelen door het artikel te volgen [in- of uitschakelen van een Server Network Protocol](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-or-disable-a-server-network-protocol#SSMSProcedure).
* Het configureren van uw [Windows Firewall voor toegang tot de database-engine](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).
* Een Azure-abonnement. Als u nog geen abonnement hebt, [maakt u een gratis account](https://azure.microsoft.com/free/) voordat u begint.
* Een beheerd exemplaar voor Azure SQL Database. U kunt een beheerd exemplaar voor Azure SQL Database maken met de details in het artikel te volgen [maken van een beheerd exemplaar voor Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).
* Downloaden en installeren [Data Migration Assistant](https://www.microsoft.com/download/details.aspx?id=53595) v3.3 of hoger.
* Een Azure-netwerk (VNet) gemaakt met behulp van de Azure Resource Manager-implementatiemodel, dat de Azure Database Migration Service met site-naar-site-verbinding met uw on-premises bronservers biedt met behulp van [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) of [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
* Een voltooide evaluatie van uw on-premises database en het schema een migratie met behulp van Data Migration Assistant, zoals beschreven in het artikel [uitvoeren van een SQL Server-migratie-evaluatie](https://docs.microsoft.com/sql/dma/dma-assesssqlonprem).
* Downloaden en installeren de `Az.DataMigration` module (versie 0.7.2 of hoger) vanuit de PowerShell Gallery met behulp van [Install-Module PowerShell-cmdlet](https://docs.microsoft.com/powershell/module/powershellget/Install-Module?view=powershell-5.1).
* Om ervoor te zorgen dat de referenties waarmee verbinding maken met SQL Server-bronexemplaar de [CONTROL SERVER](https://docs.microsoft.com/sql/t-sql/statements/grant-server-permissions-transact-sql) machtiging.
* Om ervoor te zorgen dat de referenties waarmee verbinding maken met de doel-Azure SQL Database heeft beheerd exemplaar de machtiging beheer DATABASE op de doeldatabase voor Azure SQL-Database beheerd exemplaar.

    > [!IMPORTANT]
    > Online migratie, moet u al uw Azure Active Directory-referenties hebt ingesteld. Zie voor meer informatie het artikel [gebruikt de portal voor het maken van een Azure AD-toepassing en service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

## <a name="sign-in-to-your-microsoft-azure-subscription"></a>Meld u aan met uw Microsoft Azure-abonnement

Meld u aan uw Azure-abonnement met behulp van PowerShell. Zie voor meer informatie het artikel [Meld u aan met Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps).

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische container waarin Azure resources worden geïmplementeerd en beheerd.

Maak een resourcegroep met behulp van de [ `New-AzResourceGroup` ](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) opdracht.

Het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* in de *VS-Oost* regio.

```powershell
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

## <a name="create-an-instance-of-azure-database-migration-service"></a>Maak een instantie van Azure Database Migration Service

U kunt een nieuw exemplaar van Azure Database Migration Service maken met behulp van de `New-AzDataMigrationService` cmdlet.
Deze cmdlet wordt verwacht dat de volgende vereiste parameters:

* *De naam van de Azure-resourcegroep*. U kunt [ `New-AzResourceGroup` ](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) opdracht voor het maken van een Azure-resourcegroep als eerder weergegeven en geeft de naam op als een parameter.
* *Servicenaam*. De tekenreeks die overeenkomt met de naam van de gewenste unieke service voor Azure Database Migration Service.
* *Locatie*. Hiermee geeft u de locatie van de service. Geef een locatie van een Azure data center, zoals VS-West of Zuidoost-Azië.
* *Sku*. Deze parameter komt overeen met de naam van de DMS-Sku. Namen van de momenteel ondersteunde Sku zijn *Basic_1vCore*, *Basic_2vCores*, *GeneralPurpose_4vCores*.
* *Virtual Subnet Identifier*. U kunt de cmdlet [ `New-AzVirtualNetworkSubnetConfig` ](https://docs.microsoft.com//powershell/module/az.network/new-azvirtualnetworksubnetconfig) om een subnet te maken.

Het volgende voorbeeld wordt een service met de naam *MyDMS* in de resourcegroep *MyDMSResourceGroup* zich in de *VS-Oost* regio met behulp van een virtueel netwerk met de naam  *MyVNET* en een subnet met de naam *MySubnet*.

> [!IMPORTANT]
> Het onderstaande codefragment wordt een offline migratie, die niet nodig heeft een exemplaar van Azure Database Migration Service op basis van een Premium-SKU. Voor een online migratie, moet de waarde van de parameter - Sku bevatten een Premium-SKU.

```powershell
$vNet = Get-AzVirtualNetwork -ResourceGroupName MyDMSResourceGroup -Name MyVNET

$vSubNet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vNet -Name MySubnet

$service = New-AzDms -ResourceGroupName myResourceGroup `
  -ServiceName MyDMS `
  -Location EastUS `
  -Sku Basic_2vCores `  
  -VirtualSubnetId $vSubNet.Id`
```

## <a name="create-a-migration-project"></a>Maak een migratieproject

Na het maken van een Azure Database Migration Service-exemplaar, kunt u een migratieproject maken. Een Azure Database Migration Service-project vereist verbindingsgegevens voor zowel de bron en doel-instanties, evenals een lijst met databases die u wilt migreren als onderdeel van het project.

### <a name="create-a-database-connection-info-object-for-the-source-and-target-connections"></a>Een Database Connection-Info-object voor de bron en doel-verbindingen maken

U kunt een Database Connection-Info-object maken met behulp van de `New-AzDmsConnInfo` cmdlet, die wordt verwacht de volgende parameters dat:

* *ServerType*. Het type van de verbinding met database aangevraagd, bijvoorbeeld SQL, Oracle of MySQL. Gebruik SQL voor SQL Server en Azure SQL.
* *DataSource*. De naam of IP-adres van een SQL Server-exemplaar of een Azure SQL Database-exemplaar.
* *AuthType*. Het verificatietype voor verbinding, die SqlAuthentication of WindowsAuthentication kan zijn.
* *TrustServerCertificate*. Deze parameter stelt een waarde die aangeeft of het kanaal worden versleuteld terwijl het overslaan van de certificaatketen voor het valideren van vertrouwensrelatie lopen. De waarde kan zijn `$true` of `$false`.

Het volgende voorbeeld wordt een object met verbindingsgegevens voor een bron SQL Server met de naam *MySourceSQLServer* met behulp van sql-verificatie:

```powershell
$sourceConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource MySourceSQLServer `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$true
```

Het volgende voorbeeld toont het maken van de verbindingsgegevens voor een Azure SQL Database-beheerd exemplaar-server met de naam targetmanagedinstance.database.windows.net met behulp van sql-verificatie:

```powershell
$targetConnInfo = New-AzDmsConnInfo -ServerType SQL `
  -DataSource "targetmanagedinstance.database.windows.net" `
  -AuthType SqlAuthentication `
  -TrustServerCertificate:$false
```

### <a name="provide-databases-for-the-migration-project"></a>Databases bieden voor het migratieproject

Maak een lijst van `AzDataMigrationDatabaseInfo` objecten die Hiermee geeft u de databases als onderdeel van het project Azure Database Migration Service, die kan worden opgegeven als parameter voor het maken van het project. U kunt de cmdlet `New-AzDataMigrationDatabaseInfo` maken `AzDataMigrationDatabaseInfo`.

Het volgende voorbeeld wordt de `AzDataMigrationDatabaseInfo` -project voor de **AdventureWorks2016** database en toegevoegd aan de lijst om te worden opgegeven als parameter voor het maken van het project.

```powershell
$dbInfo1 = New-AzDataMigrationDatabaseInfo -SourceDatabaseName AdventureWorks
$dbList = @($dbInfo1)
```

### <a name="create-a-project-object"></a>Een project-object maken

Ten slotte kunt u een Azure Database Migration Service-project met de naam *MyDMSProject* zich in *VS-Oost* met behulp van `New-AzDataMigrationProject` en de eerder gemaakte bron en doel-verbindingen toevoegen en de lijst met databases om te migreren.

```powershell
$project = New-AzDataMigrationProject -ResourceGroupName myResourceGroup `
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

Vervolgens maken en een Azure Database Migration Service-taak te starten. Deze taak is vereist voor verbinding referentie-informatie voor zowel de bron en doel, evenals de lijst met database-tabellen moeten worden gemigreerd en de informatie die al voorzien van het project gemaakt als een vereiste.

### <a name="create-credential-parameters-for-source-and-target"></a>Referentie-parameters voor de bron en doel maken

Verbinding maken van de beveiligingsreferenties als een [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) object.

Het volgende voorbeeld toont het maken van *PSCredential* objecten voor zowel de bron- en -verbindingen bieden wachtwoorden als tekenreeksvariabelen *$sourcePassword* en *$ doelwachtwoord*.

```powershell
$secpasswd = ConvertTo-SecureString -String $sourcePassword -AsPlainText -Force
$sourceCred = New-Object System.Management.Automation.PSCredential ($sourceUserName, $secpasswd)
$secpasswd = ConvertTo-SecureString -String $targetPassword -AsPlainText -Force
$targetCred = New-Object System.Management.Automation.PSCredential ($targetUserName, $secpasswd)
```

### <a name="create-a-backup-fileshare-object"></a>Een back-up bestandsshare-object maken

Maak nu een bestandsshare-object voor de lokale SMB-netwerkshare waarop Azure Database Migration Service kan duren voordat de bron databaseback-ups met behulp van de `New-AzDmsFileShare` cmdlet.

```powershell
$backupPassword = ConvertTo-SecureString -String $password -AsPlainText -Force
$backupCred = New-Object System.Management.Automation.PSCredential ($backupUserName, $backupPassword)

$backupFileSharePath="\\10.0.0.76\SharedBackup"
$backupFileShare = New-AzDmsFileShare -Path $backupFileSharePath -Credential $backupCred
```

### <a name="create-selected-database-object"></a>Geselecteerde database-object maken

De volgende stap is het selecteren van de bron- en -databases met behulp van de `New-AzDmsSelectedDB` cmdlet.

Het volgende voorbeeld is voor het migreren van een individuele database van SQL Server naar een beheerd exemplaar voor Azure SQL Database:

```powershell
$selectedDbs = @()
$selectedDbs += New-AzDmsSelectedDB -MigrateSqlServerSqlDbMi `
  -Name AdventureWorks2016 `
  -TargetDatabaseName AdventureWorks2016 `
  -BackupFileShare $backupFileShare `
```

Als een volledige SQL Server-exemplaar moet een lift-and-shift naar een Azure SQL-Database beheerd exemplaar, wordt een lus te nemen van alle databases van de bron is hieronder. In het volgende voorbeeld voor $Server $SourceUserName en $SourcePassword, geeft u de bron-SQL Server-gegevens.

```powershell
$Query = "(select name as Database_Name from master.sys.databases where Database_id>4)";
$Databases= (Invoke-Sqlcmd -ServerInstance "$Server" -Username $SourceUserName
-Password $SourcePassword -database master -Query $Query)
$selectedDbs=@()
foreach($DataBase in $Databases.Database_Name)
    {
      $SourceDB=$DataBase
      $TargetDB=$DataBase
      
$selectedDbs += New-AzureRmDmsSelectedDB -MigrateSqlServerSqlDbMi `
                                              -Name $SourceDB `
                                              -TargetDatabaseName $TargetDB `
                                              -BackupFileShare $backupFileShare
      }
```

### <a name="sas-uri-for-azure-storage-container"></a>SAS-URI voor Azure-Opslagcontainer

Maak de variabele met de SAS-URI die de Azure Database Migration Service toegang biedt tot de container van het opslagaccount waarnaar de service de back-upbestanden uploadt.

```powershell
$blobSasUri="https://mystorage.blob.core.windows.net/test?st=2018-07-13T18%3A10%3A33Z&se=2019-07-14T18%3A10%3A00Z&sp=rwdl&sv=2018-03-28&sr=c&sig=qKlSA512EVtest3xYjvUg139tYSDrasbftY%3D"
```

### <a name="additional-configuration-requirements"></a>Aanvullende configuratievereisten te voldoen

Er zijn enkele aanvullende vereisten die u afhandelen moet, maar van elkaar verschillen, afhankelijk van of het uitvoeren van een offline- of -migratie.

#### <a name="offline-migrations"></a>Offline migratie

Voor alleen offline migratie, moet u de volgende aanvullende configuratie-taken uitvoeren.

* **Selecteer aanmeldingen**. Maak een lijst van aanmeldingen moeten worden gemigreerd, zoals wordt weergegeven in het volgende voorbeeld:

    ```powershell
    $selectedLogins = @(“user1”, “user2”)
    ```

    > [!IMPORTANT]
    > Azure Database Migration Service ondersteunt momenteel alleen migreren SQL-aanmeldingen.

* **Selecteer agent-taken**. Lijst van taken moeten worden gemigreerd, zoals wordt weergegeven in het volgende voorbeeld maken:

    ```powershell
    $selectedAgentJobs = @("agentJob1", "agentJob2")
    ```

    > [!IMPORTANT]
    > Azure Database Migration Service ondersteunt momenteel alleen taken met T-SQL-subsysteem taakstappen.

#### <a name="online-migrations"></a>Online migratie

Voor alleen online migraties, moet u de volgende aanvullende configuratie-taken uitvoeren.

* **Azure Active Directory-App configureren**

    ```powershell
    $pwd = "Azure App Key"
    $appId = "Azure App Id"
    $AppPasswd = ConvertTo-SecureString $pwd -AsPlainText -Force
    $app = New-AzDmsAdApp -ApplicationId $appId -AppKey $AppPasswd
    ```

* **Resource voor opslag configureren**

    ```powershell
    $storageResourceId = "Storage Resource Id"
    ```

### <a name="create-and-start-the-migration-task"></a>Maak en start de migratietaak

Gebruik de `New-AzDataMigrationTask` cmdlet voor het maken en starten van een migratietaak.

#### <a name="specify-parameters"></a>Parameters opgeven

##### <a name="common-parameters"></a>Algemene Parameters

Ongeacht of het uitvoeren van een offline- of -migratie, de `New-AzDataMigrationTask` cmdlet wordt verwacht dat de volgende parameters:

* *TaskType*. Type migratietaak maken voor SQL Server naar Azure SQL Database Managed Instance Migratietype *MigrateSqlServerSqlDbMi* wordt verwacht. 
* *Naam resourcegroep*. De naam van de Azure-resourcegroep waarin de taak te maken.
* *ServiceName*. Azure Database Migration Service-instantie in voor het maken van de taak.
* *ProjectName*. Naam van Azure Database Migration Service-project waarin de taak te maken. 
* *TaskName*. De naam van de taak moet worden gemaakt. 
* *SourceConnection*. AzDmsConnInfo-object voor bron SQL Server-verbinding.
* *TargetConnection*. AzDmsConnInfo-object van doel-Azure SQL Database Managed Instance-verbinding.
* *SourceCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) object voor het verbinden met de bronserver.
* *TargetCred*. [PSCredential](https://docs.microsoft.com/dotnet/api/system.management.automation.pscredential?redirectedfrom=MSDN&view=powershellsdk-1.1.0) object voor het verbinden met de doelserver.
* *SelectedDatabase*. AzDataMigrationSelectedDB-object voor de bron- en database-toewijzing.
* *BackupFileShare*. FileShare-object dat het lokale netwerk vertegenwoordigt delen dat de Azure Database Migration Service kan duren voordat de bron databaseback-ups op.
* *BackupBlobSasUri*. De SAS-URI die de Azure Database Migration Service toegang biedt tot de container van het opslagaccount waarnaar de service de back-upbestanden uploadt. Informatie over het verkrijgen van de SAS-URI voor de blob-container.
* *SelectedLogins*. Lijst met geselecteerde aanmeldingen te migreren.
* *SelectedAgentJobs*. Lijst met geselecteerde agent-taken te migreren.

##### <a name="additional-parameters"></a>Extra parameters

De `New-AzDataMigrationTask` cmdlet verwacht ook parameters die uniek zijn voor het type van de migratie, offline of online, die u uitvoert.

* **Offline migratie**. Voor offline migratie, de `New-AzDataMigrationTask` cmdlet verwacht ook de volgende parameters:

  * *SelectedLogins*. Lijst met geselecteerde aanmeldingen te migreren.
  * *SelectedAgentJobs*. Lijst met geselecteerde agent-taken te migreren.

* **Online migratie**. Voor online migratie, de `New-AzDataMigrationTask` cmdlet worden ook de volgende parameters verwacht.

* *AzureActiveDirectoryApp*. Active Directory-toepassing.
* *StorageResourceID*. Resource-ID van de Storage-Account.

#### <a name="create-and-start-an-offline-migration-task"></a>Maak en start een taak offline migratie

Het volgende voorbeeld maakt en start u een offlinemigratie-taak met de naam **myDMSTask**:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMi `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -BackupBlobSasUri $blobSasUri `
  -SelectedLogins $selectedLogins `
  -SelectedAgentJobs $selectedJobs `
```

#### <a name="create-and-start-an-online-migration-task"></a>Maak en start een taak online migratie

Het volgende voorbeeld maakt en start de taak van een online migratie met de naam **myDMSTask**:

```powershell
$migTask = New-AzDataMigrationTask -TaskType MigrateSqlServerSqlDbMiSync `
  -ResourceGroupName myResourceGroup `
  -ServiceName $service.Name `
  -ProjectName $project.Name `
  -TaskName myDMSTask `
  -SourceConnection $sourceConnInfo `
  -SourceCred $sourceCred `
  -TargetConnection $targetConnInfo `
  -TargetCred $targetCred `
  -SelectedDatabase  $selectedDbs `
  -BackupFileShare $backupFileShare `
  -SelectedDatabase $selectedDbs `
  -AzureActiveDirectoryApp $app `
  -StorageResourceId $storageResourceId
```

## <a name="monitor-the-migration"></a>Bewaak de migratie

Voor het controleren van de migratie, moet u de volgende taken uitvoeren.

1. Alle migratiedetails in een variabele met de naam $CheckTask consolideren.

    Als u wilt combineren migratiedetails zoals eigenschappen, status en database-informatie die is gekoppeld aan de migratie, gebruikt u het volgende codefragment:

    ```powershell
    $CheckTask= Get-AzDataMigrationTask     -ResourceGroupName myResourceGroup `
                                            -ServiceName $service.Name `
                                        -ProjectName $project.Name `
                                            -Name myDMSTask `
                                            -ResultType DatabaseLevelOutput `
                        -Expand 
    Write-Host ‘$CheckTask.ProjectTask.Properties.Output’
    ```

2. Gebruik de `$CheckTask` variabele om op te halen van de huidige status van de migratietaak.

    Gebruik de `$CheckTask` variabele om op te halen van de huidige status van de migratietaak, kunt u de migratietaak uitgevoerd door het opvragen van de eigenschap state van de taak, zoals wordt weergegeven in het volgende voorbeeld bewaken:

    ```powershell
    if (($CheckTask.ProjectTask.Properties.State -eq "Running") -or ($CheckTask.ProjectTask.Properties.State -eq "Queued"))
    {
      write-host "migration task running"
    }
    Else if($CheckTask.ProjectTask.Properties.State -eq "Succeeded")
    { 
      write-host "Migration task is completed Successfully"
    }
    Else if($CheckTask.ProjectTask.Properties.State -eq "Failed" -or $CheckTask.ProjectTask.Properties.State -eq "FailedInputValidation"  -or $CheckTask.ProjectTask.Properties.State -eq "Faulted")
    { 
      write-host “Migration Task Failed”
    }
    ```

## <a name="performing-the-cutover-online-migrations-only"></a>Uitvoeren van de cutover (online migratie alleen)

Met een online migratie een volledige back-up en herstel van databases wordt uitgevoerd en vervolgens werken op het herstellen van de transactielogboeken die zijn opgeslagen in de BackupFileShare wordt voortgezet.

Wanneer de database in een beheerd exemplaar voor Azure SQL Database is bijgewerkt met de meest recente gegevens en gesynchroniseerd met de bron-database is, kunt u een cutover uitvoeren.

Het volgende voorbeeld wordt de cutover\migration voltooid. Gebruikers met deze opdracht om naar eigen goeddunken worden aangeroepen.

```powershell
$command = Invoke-AzDmsCommand -CommandType CompleteSqlMiSync `
                               -ResourceGroupName myResourceGroup `
                               -ServiceName $service.Name `
                               -ProjectName $project.Name `
                               -TaskName myDMSTask `
                               -DatabaseName "Source DB Name"
```

## <a name="deleting-the-instance-of-azure-database-migration-service"></a>Verwijderen van het exemplaar van Azure Database Migration Service

Nadat de migratie voltooid is, kunt u de Azure Database Migration Service-exemplaar verwijderen:

```powershell
Remove-AzDms -ResourceGroupName myResourceGroup -ServiceName MyDMS
```

## <a name="additional-resources"></a>Aanvullende resources

Zie voor informatie over aanvullende migreren scenario's (bron-/ doelparen), de Microsoft [handleiding voor databasemigratie](https://datamigration.microsoft.com/).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure Database Migration Service in het artikel [wat is de Azure Database Migration Service?](https://docs.microsoft.com/azure/dms/dms-overview).
