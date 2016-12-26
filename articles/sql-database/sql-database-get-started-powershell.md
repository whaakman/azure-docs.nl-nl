---
title: Nieuwe SQL-database instellen met PowerShell | Microsoft Docs
description: Ontdek hoe u een SQL-database maakt met PowerShell. U kunt algemene database-instellingstaken beheren met PowerShell-cmdlets.
keywords: nieuwe sql-database maken,database instellen
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: cgronlun
ms.assetid: 7d99869b-cec5-4583-8c1c-4c663f4afd4d
ms.service: sql-database
ms.custom: single databases
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: powershell
ms.workload: data-management
ms.date: 12/09/2016
ms.author: sstein
translationtype: Human Translation
ms.sourcegitcommit: 145cdc5b686692b44d2c3593a128689a56812610
ms.openlocfilehash: b557c6d3591b1d56144651e876fa4bb1b0646935


---

# <a name="get-started-with-azure-sql-database-servers-databases-and-firewall-rules-by-using-azure-powershell"></a>Aan de slag met Azure SQL Database-servers, -databases en -firewallregels met behulp van Azure PowerShell

In deze aan de slag-zelfstudie leert u hoe u PowerShell kunt gebruiken voor het volgende:

* Een nieuwe Azure-resourcegroep maken
* Een logische Azure SQL-server maken
* Eigenschappen van de Azure SQL-server bekijken
* Een serverfirewallregel maken
* De AdventureWorksLT-voorbeelddatabase als een zelfstandige database maken
* Eigenschappen van de Adventure Works LT-voorbeelddatabase bekijken
* Een lege zelfstandige database maken

In deze zelfstudie leert u ook het volgende:

* Verbinding maken met de logische server en de bijbehorende hoofddatabase
* Eigenschappen van de hoofddatabase bekijken
* Verbinding maken met de voorbeelddatabase
* Eigenschappen van de gebruikersdatabase bekijken

Aan het eind van deze zelfstudie hebt u een voorbeelddatabase en een lege database gemaakt die worden uitgevoerd in een Azure-resourcegroep en zijn gekoppeld aan een logische server. U hebt dan ook een serverfirewallregel geconfigureerd waarmee de serverprincipal wordt aangemeld bij de server vanaf een opgegeven IP-adres (of IP-adresbereik). 

**Geschatte tijd**: deze zelfstudie duurt circa 30 minuten (mits u al aan de vereisten voldoet).


> [!TIP]
> U kunt dezelfde taken uitvoeren in een aan de slag-zelfstudie met [Azure Portal](sql-database-get-started.md).
>


## <a name="prerequisites"></a>Vereisten

* U hebt een Azure-account nodig. U kunt [een gratis Azure-account openen](/pricing/free-trial/?WT.mc_id=A261C142F) of [uw voordelen als Visual Studio-abonnee activeren](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* U moet aangemeld zijn met een account dat lid is van de eigenaar van het abonnement of de rol Inzender. Zie [Aan de slag met toegangsbeheer in Azure Portal](../active-directory/role-based-access-control-what-is.md) voor meer informatie over op rollen gebaseerd toegangsbeheer (RBAC).

* U hebt het .bacpac-bestand van de AdventureWorksLT-voorbeelddatabase nodig in Azure Blob Storage.

### <a name="download-the-adventureworkslt-sample-database-bacpac-file-and-save-it-in-azure-blob-storage"></a>Het .bacpac-bestand van de AdventureWorksLT-voorbeelddatabase downloaden en opslaan in Azure Blob Storage

In deze zelfstudie maakt u een nieuwe AdventureWorksLT-database door een .bacpac-bestand te importeren uit Azure Storage. De eerste stap bestaat uit het ophalen van een kopie van AdventureWorksLT.bacpac en het uploaden van deze kopie naar Blob Storage.
Voer de volgende stappen uit om de voorbeelddatabase klaar te maken voor import:

1. [Download AdventureWorksLT.bacpac](https://sqldbbacpacs.blob.core.windows.net/bacpacs/AdventureWorksLT.bacpac) en sla het op met de bestandsextensie .bacpac.
2. [Maak een opslagaccount](../storage/storage-create-storage-account.md#create-a-storage-account): u kunt het opslagaccount maken met de standaardinstellingen.
3. Maak een nieuwe **container**: blader naar het opslagaccount, selecteer **Blobs** en klik vervolgens op **+Container**.
4. Upload het .bacpac-bestand naar de blobcontainer in uw opslagaccount. Klik hiervoor op de knop **Uploaden** boven aan de containerpagina of gebruik [AzCopy](../storage/storage-use-azcopy.md#blob-upload). 
5. Na het opslaan van AdventureWorksLT.bacpac hebt u de URL en opslagaccountsleutel voor het importcodefragment verderop in deze zelfstudie nodig. 
   * Selecteer het bacpac-bestand en kopieer de URL. Dit is vergelijkbaar met https://{opslagaccountnaam}.blob.core.windows.net/{containernaam}/AdventureWorksLT.bacpac. Klik op de opslagaccountpagina op **Toegangssleutels** en kopieer **key1**.


[!INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]


## <a name="create-a-new-logical-sql-server-using-azure-powershell"></a>Een nieuwe logische SQL-server maken met Azure PowerShell

U hebt een resourcegroep nodig voor de server en daarom bestaat de eerste stap ofwel uit het maken van een nieuwe resourcegroep en server ([New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/new-azurermresourcegroup), [New-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserver)), ofwel uit het ophalen van verwijzingen naar een nieuwe resourcegroep en server ([Get-AzureRmResourceGroup](https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.3.0/get-azurermresourcegroup), [Get-AzureRmSqlServer](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserver)).
Met de volgende codefragmenten worden een resourcegroep en Azure SQL-server gemaakt als deze nog niet bestaan:

Zie [Helper-codefragmenten](#helper-snippets) hieronder voor een lijst van geldige Azure-locaties en de tekenreeksindeling.
```
# Create new, or get existing resource group
############################################

$resourceGroupName = "{resource-group-name}"
$resourceGroupLocation = "{resource-group-location}"

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-Output "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-Output "Resource group $resourceGroupName already exists:"
}
$myResourceGroup



# Create a new, or get existing server
######################################

$serverName = "{server-name}"
$serverVersion = "12.0"
$serverLocation = $resourceGroupLocation
$serverResourceGroupName = $resourceGroupName

$serverAdmin = "{server-admin}"
$serverAdminPassword = "{server-admin-password}"

$securePassword = ConvertTo-SecureString –String $serverAdminPassword –AsPlainText -Force
$serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-Output "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-Output "SQL server $serverName already exists:"
}
$myServer

```


## <a name="view-the-logical-sql-server-properties-using-azure-powershell"></a>De eigenschappen van de logische SQL-server bekijken met behulp van Azure PowerShell

```
#$serverResourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin
```


## <a name="create-a-server-level-firewall-rule-using-azure-powershell"></a>Een firewallregel op serverniveau maken met behulp van Azure PowerShell

U moet weten wat uw openbare IP-adres is om de firewallregel in te kunnen stellen. U kunt uw IP-adres ophalen via een browser naar keuze (vraag "Wat is mijn IP-adres?"). Zie [Firewallregels](sql-database-firewall-configure.md) voor meer informatie.

Hieronder worden de cmdlets [Get-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqlserverfirewallrule) en [New-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqlserverfirewallrule) gebruikt om een verwijzing op te halen of een nieuwe regel te maken. Voor dit codefragment wordt, als de regel al bestaat, alleen een verwijzing ernaar opgehaald en worden de begin- en IP-adressen niet bijgewerkt. U kunt altijd de **ELSE**-component wijzigen om de [Set-AzureRmSqlServerFirewallRule](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/set-azurermsqlserverfirewallrule) te gebruiken voor de functie Maken of Bijwerken.

> [!NOTE] 
> U kunt de SQL-databasefirewall op de server naar een enkel IP-adres of een reeks adressen openen. Als u de firewall opent, kunnen SQL-beheerders en -gebruikers zich aanmelden bij elke database op de server waarvoor ze geldige aanmeldgegevens hebben.
>

```
#$serverName = "{server-name}"
#$serverResourceGroupName = "{resource-group-name}"
$serverFirewallRuleName = "{server-firewall-rule-name}"
$serverFirewallStartIp = "{server-firewall-rule-startIp}"
$serverFirewallEndIp = "{server-firewall-rule-endIp}"

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule
```


## <a name="connect-to-sql-server-using-azure-powershell"></a>Verbinding maken met SQL Server met Azure PowerShell

We gaan een snelle query uitvoeren op de hoofddatabase om te controleren of we verbinding kunnen maken met de server. In het volgende codefragment wordt de [.NET Framework Provider voor SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) gebruikt om verbinding te maken met de hoofddatabase van de server en er een query op uit te voeren. Er wordt een verbindingsreeks gemaakt op basis van de variabelen die we in de vorige codefragmenten hebben gebruikt. Vervang de tijdelijke aanduidingen door de gebruikersnaam en het wachtwoord van de SQL Server-beheerder die u in de vorige stappen hebt gebruikt voor het maken van de server.


```
#$serverName = "{server-name}"
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"



$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```


## <a name="create-new-adventureworkslt-sample-database-using-azure-powershell"></a>Een nieuwe AdventureWorksLT-voorbeelddatabase maken met behulp van Azure PowerShell

Met het volgende codefragment wordt een bacpac van de AdventureWorksLT-voorbeelddatabase geïmporteerd via de cmdlet [New-AzureRmSqlDatabaseImport](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/new-azurermsqldatabaseimport). De bacpac bevindt zich in Azure Blob Storage. Na het uitvoeren van de import-cmdlet kunt u de voortgang van de importbewerking volgen met behulp van de cmdlet [Get-AzureRmSqlDatabaseImportExportStatus](https://docs.microsoft.com/powershell/resourcemanager/azurerm.sql/v2.3.0/get-azurermsqldatabaseimportexportstatus).
De $storageUri is de URL-eigenschap van het bacpac-bestand dat u eerder naar de portal hebt geüpload en ziet er ongeveer als volgt uit: https://{storage-account}.blob.core.windows.net/{container}/AdventureWorksLT.bacpac

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"

$databaseName = "AdventureWorksLT"
$databaseEdition = "Basic"
$databaseServiceLevel = "Basic"

$storageKeyType = "StorageAccessKey"
$storageUri = "{storage-uri}" # URL of bacpac file you uploaded to your storage account
$storageKey = "{storage-key}" # key1 in the Access keys setting of your storage account

$importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $resourceGroupName –ServerName $serverName –DatabaseName $databaseName –StorageKeytype $storageKeyType –StorageKey $storageKey -StorageUri $storageUri –AdministratorLogin $serverAdmin –AdministratorLoginPassword $securePassword –Edition $databaseEdition –ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000


Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus
```



## <a name="view-database-properties-using-azure-powershell"></a>Database-eigenschappen weergeven met behulp van Azure PowerShell

Nadat de database is gemaakt, kunt u enkele eigenschappen van de database bekijken.

```
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
#$databaseName = "{database-name}"

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName


Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status
```

## <a name="connect-and-query-the-sample-database-using-azure-powershell"></a>Verbinding maken met de voorbeelddatabase en er een query op uitvoeren met behulp van Azure PowerShell

We gaan een snelle query uitvoeren op de AdventureWorksLT-database om te controleren of we verbinding kunnen maken. In het volgende codefragment wordt de [.NET Framework Provider voor SQL Server (System.Data.SqlClient)](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx) gebruikt om verbinding te maken met de database en er een query op uit te voeren. Er wordt een verbindingsreeks gemaakt op basis van de variabelen die we in de vorige codefragmenten hebben gebruikt. Vervang de tijdelijke aanduiding door het wachtwoord van de SQL-serverbeheerder.

```
#$serverName = {server-name}
#$serverAdmin = "{server-admin}"
#$serverAdminPassword = "{server-admin-password}"
#$databaseName = {database-name}

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=$serverAdmin;Password=$serverAdminPassword" + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()


$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()
```

## <a name="create-a-new-blank-database-using-azure-powershell"></a>Een nieuwe lege database maken met behulp van Azure PowerShell

```
#$resourceGroupName = {resource-group-name}
#$serverName = {server-name}

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```


## <a name="complete-azure-powershell-script-to-create-a-server-firewall-rule-and-database"></a>Een Azure PowerShell-script voor het maken van een server, firewallregel en de database voltooien



```
# Sign in to Azure and set the subscription to work with
########################################################

$SubscriptionId = "{subscription-id}"

Add-AzureRmAccount
Set-AzureRmContext -SubscriptionId $SubscriptionId

# User variables
################

$myResourceGroupName = "{resource-group-name}"
$myResourceGroupLocation = "{resource-group-location}"

$myServerName = "{server-name}"
$myServerVersion = "12.0"
$myServerLocation = $myResourceGroupLocation
$myServerResourceGroupName = $myResourceGroupName
$myServerAdmin = "{server-admin}"
$myServerAdminPassword = "{server-admin-password}" 

$myServerFirewallRuleName = "{server-firewall-rule-name}"
$myServerFirewallStartIp = "{start-ip}"
$myServerFirewallEndIp = "{end-ip}"

$myDatabaseName = "AdventureWorksLT"
$myDatabaseEdition = "Basic"
$myDatabaseServiceLevel = "Basic"

$myStorageKeyType = "StorageAccessKey"
# Get these values from your Azure storage account:
$myStorageUri = "{http://your-storage-account.blob.core.windows.net/your-container/AdventureWorksLT.bacpac}"
$myStorageKey = "{your-storage-key}"


# Create new, or get existing resource group
############################################

$resourceGroupName = $myResourceGroupName
$resourceGroupLocation = $myResourceGroupLocation

$myResourceGroup = Get-AzureRmResourceGroup -Name $resourceGroupName -ea SilentlyContinue

if(!$myResourceGroup)
{
   Write-host "Creating resource group: $resourceGroupName"
   $myResourceGroup = New-AzureRmResourceGroup -Name $resourceGroupName -Location $resourceGroupLocation
}
else
{
   Write-host "Resource group $resourceGroupName already exists:"
}
$myResourceGroup


# Create a new, or get existing server
######################################

$serverName = $myServerName
$serverVersion = $myServerVersion
$serverLocation = $myServerLocation
$serverResourceGroupName = $myServerResourceGroupName

$serverAdmin = $myServerAdmin
$serverAdminPassword = $myServerAdminPassword

$securePassword = ConvertTo-SecureString –String $serverAdminPassword –AsPlainText -Force
$serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue
if(!$myServer)
{
   Write-host "Creating SQL server: $serverName"
   $myServer = New-AzureRmSqlServer -ResourceGroupName $serverResourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
}
else
{
   Write-host "SQL server $serverName already exists:"
}
$myServer


# View server properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$myServer = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $serverResourceGroupName 

Write-Host "Server name: " $myServer.ServerName
Write-Host "Fully qualified server name: $serverName.database.windows.net"
Write-Host "Server location: " $myServer.Location
Write-Host "Server version: " $myServer.ServerVersion
Write-Host "Server administrator login: " $myServer.SqlAdministratorLogin


# Create or update server firewall rule
#######################################

$serverFirewallRuleName = $myServerFirewallRuleName
$serverFirewallStartIp = $myServerFirewallStartIp
$serverFirewallEndIp = $myServerFirewallEndIp

$myFirewallRule = Get-AzureRmSqlServerFirewallRule -FirewallRuleName $serverFirewallRuleName -ServerName $serverName -ResourceGroupName $serverResourceGroupName -ea SilentlyContinue

if(!$myFirewallRule)
{
   Write-host "Creating server firewall rule: $serverFirewallRuleName"
   $myFirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $serverResourceGroupName -ServerName $serverName -FirewallRuleName $serverFirewallRuleName -StartIpAddress $serverFirewallStartIp -EndIpAddress $serverFirewallEndIp
}
else
{
   Write-host "Server firewall rule $serverFirewallRuleName already exists:"
}
$myFirewallRule


# Connect to the server and master database
###########################################
$databaseName = "master"

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"


$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create the AdventureWorksLT database from a bacpac
####################################################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName

$databaseName = $myDatabaseName
$databaseEdition = $myDatabaseEdition
$databaseServiceLevel = $myDatabaseServiceLevel

$storageKeyType = $myStorageKeyType
$storageUri = $myStorageUri
$storageKey = $myStorageKey

$importRequest = New-AzureRmSqlDatabaseImport –ResourceGroupName $resourceGroupName –ServerName $serverName –DatabaseName $databaseName –StorageKeytype $storageKeyType –StorageKey $storageKey -StorageUri $storageUri –AdministratorLogin $serverAdmin –AdministratorLoginPassword $securePassword –Edition $databaseEdition –ServiceObjectiveName $databaseServiceLevel -DatabaseMaxSizeBytes 5000000

Do {
     $importStatus = Get-AzureRmSqlDatabaseImportExportStatus -OperationStatusLink $importRequest.OperationStatusLink
     Write-host "Importing database..." $importStatus.StatusMessage
     Start-Sleep -Seconds 30
     $importStatus.Status
   }
   until ($importStatus.Status -eq "Succeeded")
$importStatus


# View database properties
##########################

$resourceGroupName = $myResourceGroupName
$serverName = $myServerName
$databaseName = $myDatabaseName

$myDatabase = Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName

Write-Host "Database name: " $myDatabase.DatabaseName
Write-Host "Server name: " $myDatabase.ServerName
Write-Host "Creation date: " $myDatabase.CreationDate
Write-Host "Database edition: " $myDatabase.Edition
Write-Host "Database performance level: " $myDatabase.CurrentServiceObjectiveName
Write-Host "Database status: " $myDatabase.Status


# Query the database
####################

$connectionString = "Server=tcp:" + $serverName + ".database.windows.net" + ",1433;Initial Catalog=" + $databaseName + ";Persist Security Info=False;User ID=" + $myServer.SqlAdministratorLogin + ";Password=" + $myServerAdminPassword + ";MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"

$connection = New-Object System.Data.SqlClient.SqlConnection
$connection.ConnectionString = $connectionString
$connection.Open()
$command = New-Object System.Data.SQLClient.SQLCommand("select * from sys.objects", $connection)

$command.Connection = $connection
$reader = $command.ExecuteReader()

$sysObjects = ""
while ($reader.Read()) {
    $sysObjects += $reader["name"] + "`n"
}
$sysObjects

$connection.Close()


# Create a blank database
#########################

$blankDatabaseName = "blankdb"
$blankDatabaseEdition = "Basic"
$blankDatabaseServiceLevel = "Basic"


$myBlankDatabase = New-AzureRmSqlDatabase -DatabaseName $blankDatabaseName -ServerName $serverName -ResourceGroupName $resourceGroupName -Edition $blankDatabaseEdition -RequestedServiceObjectiveName $blankDatabaseServiceLevel
$myBlankDatabase
```



> [!TIP]
> U kunt geld besparen tijdens de zelfstudie door databases die u niet gebruikt, te verwijderen. U kunt databases uit de Basic-editie binnen 7 dagen herstellen. Verwijder echter geen servers. Als u dit doet, kunt u de server en bijbehorende verwijderde databases niet herstellen.

## <a name="helper-snippets"></a>Helper-codefragmenten

```
# Get a list of Azure regions where you can create SQL resources

$sqlRegions = (Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" })
foreach ($region in $sqlRegions)
{
   $region.Location
}

# Clean up
# Delete a resource group (and all contained resources)
Remove-AzureRmResourceGroup -Name {resource-group-name}
```

> [!TIP]
> U kunt geld besparen tijdens de zelfstudie door databases die u niet gebruikt, te verwijderen. U kunt databases uit de Standaard-editie binnen zeven dagen herstellen. Verwijder echter niet de server. Als u dit doet, kunt u de server en bijbehorende verwijderde databases niet herstellen.
>

## <a name="next-steps"></a>Volgende stappen
Nu u deze eerste zelfstudie hebt voltooid en een database met enkele voorbeeldgegevens hebt gemaakt, wilt u mogelijk aanvullende zelfstudies volgen die aansluiten op wat u in deze zelfstudie hebt geleerd. 

* Zie [Aan de slag met beveiliging](sql-database-get-started-security.md) voor meer informatie over de beveiliging van Azure SQL Database.
* Bent u bekend met Excel? Ontdek dan hoe u [verbinding kunt maken met een SQL Database in Azure via Excel](sql-database-connect-excel.md).
* Als u wilt gaan coderen, kiest u uw programmeertaal in [Verbindingsbibliotheken voor SQL Database en SQL Server](sql-database-libraries.md).
* Als u uw on-premises SQL Server-database wilt verplaatsen naar Azure, raadpleegt u [Een database migreren naar SQL Database](sql-database-cloud-migrate.md).
* Als u met het opdrachtregelprogramma BCP enkele gegevens vanuit een csv-bestand in een nieuwe tabel wilt laden, leest u [Met BCP gegevens vanuit een csv-bestand in SQL Database laden](sql-database-load-from-csv-with-bcp.md).
* Als u tabellen en andere objecten wilt maken, raadpleegt u het onderwerp 'Een tabel maken' in [Een tabel maken](https://msdn.microsoft.com/library/ms365315.aspx).

## <a name="additional-resources"></a>Aanvullende bronnen
[Wat is SQL Database?](sql-database-technical-overview.md)


<!--HONumber=Dec16_HO2-->


