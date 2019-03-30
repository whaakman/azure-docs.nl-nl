---
title: Een Azure SQL Database-elastische-taakagent maken met PowerShell | Microsoft Docs
description: Ontdek hoe u een elastische-taakagent maakt met behulp van PowerShell.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: tutorial
author: johnpaulkee
ms.author: joke
ms.reviwer: sstein
manager: craigg
ms.date: 03/13/2019
ms.openlocfilehash: eb5066185f9301450a68276dd4b2ce2123231b34
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666782"
---
# <a name="create-an-elastic-job-agent-using-powershell"></a>Een elastische-taakagent maken met behulp van PowerShell

[Elastisc Jobs](sql-database-job-automation-overview.md#elastic-database-jobs) maken het mogelijk om één of meerdere T-SQL-scripts (Transact-SQL) parallel in veel databases uit te voeren.

In deze zelfstudie leert u welke stappen u moet uitvoeren om een ​​query uit te voeren op meerdere databases:

> [!div class="checklist"]
> * Een Elastic Jobs-agent maken
> * Taakreferenties maken zodat taken scripts kunnen uitvoeren op de doelen ervan
> * De doelen (servers, elastische pools, databases, shardtoewijzingen) definiëren waarop u de taak wilt uitvoeren
> * Databasereferenties maken in de doeldatabases zodat de agent verbinding kan maken en taken uitvoeren
> * Een taak maken
> * Taakstappen toevoegen aan een taak
> * Uitvoering van een taak starten
> * Een taak bewaken

## <a name="prerequisites"></a>Vereisten

De bijgewerkte versie van de taken voor Elastic Database heeft een nieuwe set PowerShell-cmdlets voor gebruik tijdens de migratie. Deze nieuwe cmdlets dragen alle van de taakreferenties van uw bestaande, gericht op (met inbegrip van databases, servers en aangepaste verzamelingen), taak triggers, taakschema's, de inhoud van de taak en taken naar een nieuwe agent voor elastische taken.

### <a name="install-the-latest-elastic-jobs-cmdlets"></a>Installeer de meest recente cmdlets voor elastische taken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

Installeer de **Az.Sql** 1.1.1-preview-module om op te halen van de meest recente elastische taak-cmdlets. Voer de volgende opdrachten met beheerderstoegang uit in PowerShell.

```powershell
# Installs the latest PackageManagement powershell package which PowershellGet v1.6.5 is dependent on
Find-Package PackageManagement -RequiredVersion 1.1.7.2 | Install-Package -Force

# Installs the latest PowershellGet module which adds the -AllowPrerelease flag to Install-Module
Find-Package PowerShellGet -RequiredVersion 1.6.5 | Install-Package -Force

# Restart your powershell session with administrative access

# Places Az.Sql preview cmdlets side by side with existing Az.Sql version
Install-Module -Name Az.Sql -RequiredVersion 1.1.1-preview -AllowPrerelease

# Import the Az.Sql module
Import-Module Az.Sql -RequiredVersion 1.1.1

# Confirm if module successfully imported - if the imported version is 1.1.1, then continue
Get-Module Az.Sql
```

- Naast de **Az.Sql** 1.1.1-preview-module, in deze zelfstudie is ook vereist de *sqlserver* PowerShell-module. Zie [SQL Server PowerShell-module installeren](https://docs.microsoft.com/sql/powershell/download-sql-server-ps-module) voor meer informatie.


## <a name="create-required-resources"></a>Vereiste resources maken

Voor het maken van een Elastic Jobs-agent is een database (S0 of hoger) vereist om te gebruiken als [taakdatabase](sql-database-job-automation-overview.md#job-database). 

*Het onderstaande script maakt een nieuwe resourcegroep, server en database voor gebruik als de taakdatabase. Het onderstaande script maakt ook een tweede server met 2 lege databases om taken op uit te voeren.*

Elastic Jobs heeft geen specifieke naamgevingsvereisten, zodat u elke naamgevingsconventie kunt gebruiken die u wilt, zolang deze maar voldoet aan de [Azure-vereisten](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

```powershell
# Sign in to your Azure account
Connect-AzAccount

# Create a resource group
Write-Output "Creating a resource group..."
$ResourceGroupName = Read-Host "Please enter a resource group name"
$Location = Read-Host "Please enter an Azure Region"
$Rg = New-AzResourceGroup -Name $ResourceGroupName -Location $Location
$Rg

# Create a server
Write-Output "Creating a server..."
$AgentServerName = Read-Host "Please enter an agent server name"
$AgentServerName = $AgentServerName + "-" + [guid]::NewGuid()
$AdminLogin = Read-Host "Please enter the server admin name"
$AdminPassword = Read-Host "Please enter the server admin password"
$AdminPasswordSecure = ConvertTo-SecureString -String $AdminPassword -AsPlainText -Force
$AdminCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AdminLogin, $AdminPasswordSecure
$AgentServer = New-AzSqlServer -ResourceGroupName $ResourceGroupName -Location $Location -ServerName $AgentServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($AdminCred)

# Set server firewall rules to allow all Azure IPs
Write-Output "Creating a server firewall rule..."
$AgentServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$AgentServer

# Create the job database
Write-Output "Creating a blank SQL database to be used as the Job Database..."
$JobDatabaseName = "JobDatabase"
$JobDatabase = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $AgentServerName -DatabaseName $JobDatabaseName -RequestedServiceObjectiveName "S0"
$JobDatabase
```

```powershell
# Create a target server and some sample databases - uses the same admin credential as the agent server just for simplicity
Write-Output "Creating target server..."
$TargetServerName = Read-Host "Please enter a target server name"
$TargetServerName = $TargetServerName + "-" + [guid]::NewGuid()
$TargetServer = New-AzSqlServer -ResourceGroupName $ResourceGroupName -Location $Location -ServerName $TargetServerName -ServerVersion "12.0" -SqlAdministratorCredentials ($AdminCred)

# Set target server firewall rules to allow all Azure IPs
$TargetServer | New-AzSqlServerFirewallRule -AllowAllAzureIPs
$TargetServer | New-AzSqlServerFirewallRule -StartIpAddress 0.0.0.0 -EndIpAddress 255.255.255.255 -FirewallRuleName AllowAll
$TargetServer

# Create some sample databases to execute jobs against...
$Db1 = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $TargetServerName -DatabaseName "TargetDb1"
$Db1
$Db2 = New-AzSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $TargetServerName -DatabaseName "TargetDb2"
$Db2
```

## <a name="enable-the-elastic-jobs-preview-for-your-subscription"></a>De Elastic Jobs-preview inschakelen voor uw abonnement

Voor het gebruik van elastische taken, de functie in uw Azure-abonnement te registreren met de volgende opdracht. Deze opdracht één keer voor het abonnement waarin u van plan bent voor het inrichten van de agent voor elastische taken uitvoeren. Abonnementen die alleen databases die taak doelen bevatten hoeft te worden geregistreerd.

```powershell
Register-AzProviderFeature -FeatureName sqldb-JobAccounts -ProviderNamespace Microsoft.Sql
```

## <a name="create-the-elastic-job-agent"></a>De Elastic Jobs-agent maken

Een Elastic Jobs-agent is een Azure-resource voor het maken, uitvoeren en beheren van taken. De agent voert taken uit op basis van een planning of als eenmalige taak.

De **New-AzSqlElasticJobAgent** cmdlet is vereist voor een Azure SQL-database moeten al bestaan, waardoor de *ResourceGroupName*, *ServerName*, en  *DatabaseName* parameters moeten allemaal verwijzen naar bestaande resources.

```powershell
Write-Output "Creating job agent..."
$AgentName = Read-Host "Please enter a name for your new Elastic Job agent"
$JobAgent = $JobDatabase | New-AzSqlElasticJobAgent -Name $AgentName
$JobAgent
```

## <a name="create-job-credentials-so-that-jobs-can-execute-scripts-on-its-targets"></a>Taakreferenties maken zodat taken scripts kunnen uitvoeren op de doelen ervan

Taken gebruiken databasereferenties om verbinding te maken met de doeldatabases die tijdens de uitvoering zijn opgegeven door de doelgroep. Deze databasereferenties worden ook gebruikt verbinding te maken met de hoofddatabase om alle databases in een server of elastische pool te inventariseren wanneer een van deze wordt gebruikt als doelgroeplidsoort.

De databasereferenties moeten worden gemaakt in de taakdatabase.  
Alle doeldatabases moeten een aanmelding met voldoende machtigingen hebben om de taak te kunnen voltooien.

![Referenties voor Elastic Jobs](media/elastic-jobs-overview/job-credentials.png)

Let behalve op de inloggegevens in de afbeelding op de toevoeging van de *GRANT*-opdrachten in het volgende script. Deze machtigingen zijn vereist voor het script dat we hebben gekozen voor deze voorbeeldtaak. Omdat in het voorbeeld een nieuwe tabel in de doeldatabases wordt gemaakt, heeft elke doeldatabase de juiste machtigingen nodig om met succes te worden uitgevoerd.

Voer het volgende script uit om de vereiste taakreferenties (in de taakdatabase) te maken:

```powershell
# In the master database (target server)
# - Create the master user login
# - Create the master user from master user login
# - Create the job user login
$Params = @{
  'Database' = 'master'
  'ServerInstance' =  $TargetServer.ServerName + '.database.windows.net'
  'Username' = $AdminLogin
  'Password' = $AdminPassword
  'OutputSqlErrors' = $true
  'Query' = "CREATE LOGIN masteruser WITH PASSWORD='password!123'"
}
Invoke-SqlCmd @Params
$Params.Query = "CREATE USER masteruser FROM LOGIN masteruser"
Invoke-SqlCmd @Params
$Params.Query = "CREATE LOGIN jobuser WITH PASSWORD='password!123'"
Invoke-SqlCmd @Params

# For each of the target databases
# - Create the jobuser from jobuser login
# - Make sure they have the right permissions for successful script execution
$TargetDatabases = @( $Db1.DatabaseName, $Db2.DatabaseName )
$CreateJobUserScript =  "CREATE USER jobuser FROM LOGIN jobuser"
$GrantAlterSchemaScript = "GRANT ALTER ON SCHEMA::dbo TO jobuser"
$GrantCreateScript = "GRANT CREATE TABLE TO jobuser"

$TargetDatabases | % {
  $Params.Database = $_

  $Params.Query = $CreateJobUserScript
  Invoke-SqlCmd @Params

  $Params.Query = $GrantAlterSchemaScript
  Invoke-SqlCmd @Params

  $Params.Query = $GrantCreateScript
  Invoke-SqlCmd @Params
}

# Create job credential in Job database for master user
Write-Output "Creating job credentials..."
$LoginPasswordSecure = (ConvertTo-SecureString -String "password!123" -AsPlainText -Force)

$MasterCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "masteruser", $LoginPasswordSecure
$MasterCred = $JobAgent | New-AzSqlElasticJobCredential -Name "masteruser" -Credential $MasterCred

$JobCred = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList "jobuser", $LoginPasswordSecure
$JobCred = $JobAgent | New-AzSqlElasticJobCredential -Name "jobuser" -Credential $JobCred
```

## <a name="define-the-target-databases-you-want-to-run-the-job-against"></a>De doeldatabases definiëren waarop u de taak wilt uitvoeren

Een [doelgroep](sql-database-job-automation-overview.md#target-group) definieert een verzameling van een of meer databases waarop een taakstap wordt uitgevoerd. 

In het volgende fragment worden twee doelgroepen gemaakt: *ServerGroup* en *ServerGroupExcludingDb2*. *ServerGroup* zijn alle databases die bestaan ​​op de server op het moment van uitvoering, en *ServerGroupExcludingDb2* alle databases op de server, behalve *TargetDb2*:

```powershell
Write-Output "Creating test target groups..."
# Create ServerGroup target group
$ServerGroup = $JobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroup'
$ServerGroup | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -RefreshCredentialName $MasterCred.CredentialName

# Create ServerGroup with an exclusion of Db2
$ServerGroupExcludingDb2 = $JobAgent | New-AzSqlElasticJobTargetGroup -Name 'ServerGroupExcludingDb2'
$ServerGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -RefreshCredentialName $MasterCred.CredentialName
$ServerGroupExcludingDb2 | Add-AzSqlElasticJobTarget -ServerName $TargetServerName -Database $Db2.DatabaseName -Exclude
```

## <a name="create-a-job"></a>Een taak maken

```powershell
Write-Output "Creating a new job"
$JobName = "Job1"
$Job = $JobAgent | New-AzSqlElasticJob -Name $JobName -RunOnce
$Job
```

## <a name="create-a-job-step"></a>Een taakstap maken

In dit voorbeeld worden twee taakstappen gedefinieerd voor de taak die moet worden uitgevoerd. Met de eerste taakstap (*stap 1*) wordt een nieuwe tabel (*Step1Table*) gemaakt in elke database in de doelgroep *ServerGroup*. Met de tweede taakstap (*stap2*) wordt een nieuwe tabel (*Step2Table*) gemaakt in elke database behalve *TargetDb2*, omdat in de doelgroep die [eerder is gedefinieerd](#define-the-target-databases-you-want-to-run-the-job-against) is opgegeven dat deze moet worden uitgesloten.

```powershell
Write-Output "Creating job steps"
$SqlText1 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step1Table')) CREATE TABLE [dbo].[Step1Table]([TestId] [int] NOT NULL);"
$SqlText2 = "IF NOT EXISTS (SELECT * FROM sys.tables WHERE object_id = object_id('Step2Table')) CREATE TABLE [dbo].[Step2Table]([TestId] [int] NOT NULL);"

$Job | Add-AzSqlElasticJobStep -Name "step1" -TargetGroupName $ServerGroup.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText1
$Job | Add-AzSqlElasticJobStep -Name "step2" -TargetGroupName $ServerGroupExcludingDb2.TargetGroupName -CredentialName $JobCred.CredentialName -CommandText $SqlText2
```


## <a name="run-the-job"></a>De taak uitvoeren

Voer de volgende opdracht uit om de taak onmiddellijk te starten:

```powershell
Write-Output "Start a new execution of the job..."
$JobExecution = $Job | Start-AzSqlElasticJob
$JobExecution
```

Na succesvolle voltooiing ziet u twee nieuwe tabellen in TargetDb1 en slechts één nieuwe tabel in TargetDb2:


   ![verificatie van nieuwe tabellen in SSMS](media/elastic-jobs-overview/job-execution-verification.png)




## <a name="monitor-status-of-job-executions"></a>Status van taakuitvoeringen bewaken

Met de volgende codefragmenten worden gegevens opgehaald over de taakuitvoering:

```powershell
# Get the latest 10 executions run
$JobAgent | Get-AzSqlElasticJobExecution -Count 10

# Get the job step execution details
$JobExecution | Get-AzSqlElasticJobStepExecution

# Get the job target execution details
$JobExecution | Get-AzSqlElasticJobTargetExecution -Count 2
```

## <a name="schedule-the-job-to-run-later"></a>De taak plannen om later uit te voeren

Voer de volgende opdracht uit om een ​​taak te plannen die op een specifiek tijdstip moet worden uitgevoerd:

```powershell
# Run every hour starting from now
$Job | Set-AzSqlElasticJob -IntervalType Hour -IntervalCount 1 -StartTime (Get-Date) -Enable
```

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de Azure-bronnen die in deze zelfstudie zijn gemaakt door de resourcegroep te verwijderen.

> [!TIP]
> Als u verder wilt werken met deze gebeurtenis, schoont u de resources die u hebt gemaakt in dit artikel, niet op. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources die in dit artikel zijn gemaakt, te verwijderen.
>

```powershell
Remove-AzResourceGroup -ResourceGroupName $ResourceGroupName
```


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Transact-SQL-script uitgevoerd op een reeks databases.  U hebt geleerd hoe u de volgende taken moet uitvoeren:

> [!div class="checklist"]
> * Een Elastic Jobs-agent maken
> * Taakreferenties maken zodat taken scripts kunnen uitvoeren op de doelen ervan
> * De doelen (servers, elastische pools, databases, shardtoewijzingen) definiëren waarop u de taak wilt uitvoeren
> * Databasereferenties maken in de doeldatabases zodat de agent verbinding kan maken en taken uitvoeren
> * Een taak maken
> * Een taakstap toevoegen aan de taak
> * Een uitvoering van de taak starten
> * De taak bewaken

> [!div class="nextstepaction"]
>[Elastic Jobs beheren met Transact-SQL](elastic-jobs-tsql.md)
