---
title: Azure Data Lake Analytics beheren met Azure PowerShell
description: Dit artikel wordt beschreven hoe u Azure PowerShell gebruikt voor het beheren van Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: matt1883
ms.author: mahi
manager: kfile
editor: jasonwhowell
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.topic: conceptual
ms.date: 06/02/2018
ms.openlocfilehash: 560f36dc64480fd6aceaa50226b191ee40d2486f
ms.sourcegitcommit: 0408c7d1b6dd7ffd376a2241936167cc95cfe10f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/26/2018
ms.locfileid: "36959845"
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Azure Data Lake Analytics beheren met Azure PowerShell
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Dit artikel wordt beschreven hoe u Azure Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken beheren met behulp van Azure PowerShell.

## <a name="prerequisites"></a>Vereisten

Als PowerShell wilt gebruiken met Data Lake Analytics, de volgende soorten informatie te verzamelen: 

* **Abonnements-ID**: de ID van het Azure-abonnement met uw Data Lake Analytics-account.
* **Resourcegroep**: de naam van de Azure-resourcegroep met uw Data Lake Analytics-account.
* **Data Lake Analytics-accountnaam**: de naam van uw Data Lake Analytics-account.
* **Default Data Lake Store-accountnaam**: elke Data Lake Analytics-account heeft een Data Lake Store-standaardaccount.
* **Locatie**: de locatie van uw Data Lake Analytics-account, zoals "VS-Oost 2" of andere locaties ondersteund.

De PowerShell-fragmenten in deze zelfstudie gebruiken deze variabelen om deze informatie op te slaan

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

### <a name="log-in-using-interactive-user-authentication"></a>Aanmelden met behulp van interactieve gebruikersverificatie

Meld u aan met een abonnements-ID of door de naam van abonnement

```powershell
# Using subscription id
Connect-AzureRmAccount -SubscriptionId $subId

# Using subscription name
Connect-AzureRmAccount -SubscriptionName $subname 
```

## <a name="saving-authenticaiton-context"></a>Opslaan van de verificatie-context

De `Connect-AzureRmAccount` cmdlet altijd om referenties gevraagd. U kunt voorkomen dat u wordt gevraagd door met de volgende cmdlets:

```powershell
# Save login session information
Save-AzureRmProfile -Path D:\profile.json  

# Load login session information
Select-AzureRmProfile -Path D:\profile.json 
```

### <a name="log-in-using-a-service-principal-identity-spi"></a>Meld u aan met een Service Principal identiteit (SPI)

```powershell
$tenantid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"  
$spi_appname = "appname" 
$spi_appid = "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" 
$spi_secret = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" 

$pscredential = New-Object System.Management.Automation.PSCredential ($spi_appid, (ConvertTo-SecureString $spi_secret -AsPlainText -Force))
Login-AzureRmAccount -ServicePrincipal -TenantId $tenantid -Credential $pscredential -Subscription $subid
```

## <a name="manage-accounts"></a>Accounts beheren


### <a name="list-accounts"></a>Lijst van accounts

```powershell
# List Data Lake Analytics accounts within the current subscription.
Get-AdlAnalyticsAccount

# List Data Lake Analytics accounts within a specific resource group.
Get-AdlAnalyticsAccount -ResourceGroupName $rg
```

### <a name="create-an-account"></a>Een account maken

Elke Data Lake Analytics-account vereist een Data Lake Store-standaardaccount dat wordt gebruikt voor het opslaan van logboeken. U kunt een bestaand account gebruiken of een account maken. 

```powershell
# Create a data lake store if needed, or you can re-use an existing one
New-AdlStore -ResourceGroupName $rg -Name $adls -Location $location
New-AdlAnalyticsAccount -ResourceGroupName $rg -Name $adla -Location $location -DefaultDataLake $adls
```

### <a name="get-account-information"></a>Accountgegevens ophalen

Informatie ophalen over een account.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

### <a name="check-if-an-account-exists"></a>Controleer of een account bestaat

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

## <a name="manage-data-sources"></a>Gegevensbronnen beheren
Azure Data Lake Analytics ondersteunt momenteel de volgende gegevensbronnen:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Elk Data Lake Analytics-account heeft een Data Lake Store-standaardaccount. De standaard Data Lake Store-account wordt gebruikt voor het opslaan van taak metagegevens en taak controlelogboeken. 

### <a name="find-the-default-data-lake-store-account"></a>Het Data Lake Store-standaardaccount vinden

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

U kunt het Data Lake Store-standaardaccount vinden door het filteren van de lijst van gegevensbronnen door de `IsDefault` eigenschap:

```powershell
Get-AdlAnalyticsDataSource -Account $adla  | ? { $_.IsDefault } 
```

### <a name="add-a-data-source"></a>Een gegevensbron toevoegen

```powershell

# Add an additional Storage (Blob) account.
$AzureStorageAccountName = "<AzureStorageAccountName>"
$AzureStorageAccountKey = "<AzureStorageAccountKey>"
Add-AdlAnalyticsDataSource -Account $adla -Blob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

# Add an additional Data Lake Store account.
$AzureDataLakeStoreName = "<AzureDataLakeStoreAccountName"
Add-AdlAnalyticsDataSource -Account $adla -DataLakeStore $AzureDataLakeStoreName 
```

### <a name="list-data-sources"></a>Lijst met gegevensbronnen

```powershell
# List all the data sources
Get-AdlAnalyticsDataSource -Name $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Name $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>U-SQL-taken verzenden

### <a name="submit-a-string-as-a-u-sql-job"></a>Verzenden van een tekenreeks als een U-SQL-taak

```powershell
$script = @"
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"@

$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 

Submit-AdlJob -AccountName $adla -Script $script -Name "Demo"
```

### <a name="submit-a-file-as-a-u-sql-job"></a>Een bestand verzenden als een U-SQL-taak

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

### <a name="list-jobs"></a>Taken weergeven

De uitvoer bevat de taken die momenteel worden uitgevoerd en de taken die onlangs zijn voltooid.

```powershell
Get-AdlJob -Account $adla
```

### <a name="list-the-top-n-jobs"></a>De top N-taken weergeven

Standaard die wordt de lijst met taken gesorteerd op tijd te verzenden. De meest recent ingediende taken verschijnen dus eerste. De ADLA account onthoudt taken gedurende 180 dagen, maar de cmdlet Get-AdlJob standaard retourneert alleen de eerste 500. Gebruik - bovenste parameter voor een lijst met een specifiek aantal taken.

```powershell
$jobs = Get-AdlJob -Account $adla -Top 10
```

### <a name="list-jobs-by-job-state"></a>Lijst met taken op de taakstatus

Met behulp van de `-State` parameter. U kunt deze waarden combineren:

* `Accepted`
* `Compiling`
* `Ended`
* `New`
* `Paused`
* `Queued`
* `Running`
* `Scheduling`
* `Start`

```powershell
# List the running jobs
Get-AdlJob -Account $adla -State Running

# List the jobs that have completed
Get-AdlJob -Account $adla -State Ended

# List the jobs that have not started yet
Get-AdlJob -Account $adla -State Accepted,Compiling,New,Paused,Scheduling,Start
```

### <a name="list-jobs-by-job-result"></a>Lijst met taken op het resultaat van taak

Gebruik de `-Result` parameter om te detecteren of beëindigd taken is voltooid. Deze heeft de volgende waarden:

* Geannuleerd
* Mislukt
* Geen
* Geslaagd

``` powershell
# List Successful jobs.
Get-AdlJob -Account $adla -State Ended -Result Succeeded

# List Failed jobs.
Get-AdlJob -Account $adla -State Ended -Result Failed
```

### <a name="list-jobs-by-job-submitter"></a>Lijst met taken op taak submitter

De `-Submitter` parameter kunt u bepalen wie een taak heeft ingediend.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

### <a name="list-jobs-by-submission-time"></a>Lijst met taken op de tijd van verzending

De `-SubmittedAfter` is handig bij het filteren van een tijdsbereik.


```powershell
# List  jobs submitted in the last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in the last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="get-job-status"></a>Taakstatus ophalen

Haal de status van een bepaalde taak op.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```


### <a name="cancel-a-job"></a>Een taak annuleren

```powershell
Stop-AdlJob -Account $adla -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a>Wachten op een taak is voltooid

In plaats van herhalende `Get-AdlAnalyticsJob` totdat een taak is voltooid, kunt u de `Wait-AdlJob` cmdlet moet worden gewacht totdat de taak beëindigen.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="analyzing-job-history"></a>Taakgeschiedenis analyseren

Met Azure PowerShell voor het analyseren van de geschiedenis van taken die in Data Lake analytics hebt uitgevoerd, is een krachtige techniek. U kunt deze gebruiken om inzicht in gebruik en de kosten te krijgen. U kunt meer informatie door te kijken naar de [taak geschiedenis Analysis sample opslagplaats](https://github.com/Azure-Samples/data-lake-analytics-powershell-job-history-analysis)  

## <a name="list-job-pipelines-and-recurrences"></a>lijst taak pijplijnen en herhalingen

Gebruik de `Get-AdlJobPipeline` cmdlet om te controleren van de pipeline-informatie taken eerder is verzonden.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla
$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

Gebruik de `Get-AdlJobRecurrence` cmdlet om te controleren van de informatie over het terugkeerpatroon voor eerder verzonden taken.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```


## <a name="manage-compute-policies"></a>Compute-beleid beheren

### <a name="list-existing-compute-policies"></a>Lijst met bestaande compute-beleidsregels

De `Get-AdlAnalyticsComputePolicy` cmdlet haalt de info over compute-beleid voor een Data Lake Analytics-account.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>Een compute-beleid maken

De `New-AdlAnalyticsComputePolicy` cmdlet maakt een nieuw compute-beleid voor een Data Lake Analytics-account. In het volgende voorbeeld wordt de maximale AUs beschikbaar op de opgegeven gebruiker tot 50 en de prioriteit van de minimale job 250.

```powershell
$userObjectId = (Get-AzureRmAdUser -SearchString "garymcdaniel@contoso.com").Id

New-AdlAnalyticsComputePolicy -Account $adla -Name "GaryMcDaniel" -ObjectId $objectId -ObjectType User -MaxDegreeOfParallelismPerJob 50 -MinPriorityPerJob 250
```
## <a name="manage-files"></a>Bestanden beheren

### <a name="check-for-the-existence-of-a-file"></a>Controleer op de aanwezigheid van een bestand.

```powershell
Test-AdlStoreItem -Account $adls -Path "/data.csv"
```

### <a name="uploading-and-downloading"></a>Uploaden en downloaden

Bestand uploaden.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Upload een hele map recursief.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\myData\" -Destination "/myData/" -Recurse
```

Een bestand downloaden.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/data.csv" -Destination "c:\data.csv"
```

Download een hele map recursief.

```powershell
Export-AdlStoreItem -AccountName $adls -Path "/" -Destination "c:\myData\" -Recurse
```

> [!NOTE]
> Als het proces uploaden of downloaden wordt onderbroken, kunt u proberen het proces hervatten door de cmdlet opnieuw uit met de ``-Resume`` vlag.

## <a name="manage-the-u-sql-catalog"></a>De U-SQL-catalogus beheren

De U-SQL-catalogus wordt gebruikt om gegevens en code structuur, zodat ze kunnen worden gedeeld door de U-SQL-scripts. De catalogus kunt de beste prestaties mogelijk met gegevens in Azure Data Lake. Zie [U-SQL-catalogus gebruiken](data-lake-analytics-use-u-sql-catalog.md) voor meer informatie.

### <a name="list-items-in-the-u-sql-catalog"></a>Lijst met items in de U-SQL-catalogus

```powershell
# List U-SQL databases
Get-AdlCatalogItem -Account $adla -ItemType Database 

# List tables within a database
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database"

# List tables within a schema.
Get-AdlCatalogItem -Account $adla -ItemType Table -Path "database.schema"
```

### <a name="list-all-the-assemblies-the-u-sql-catalog"></a>Lijst van alle assembly's de U-SQL-catalogus

```powershell
$dbs = Get-AdlCatalogItem -Account $adla -ItemType Database

foreach ($db in $dbs)
{
    $asms = Get-AdlCatalogItem -Account $adla -ItemType Assembly -Path $db.Name

    foreach ($asm in $asms)
    {
        $asmname = "[" + $db.Name + "].[" + $asm.Name + "]"
        Write-Host $asmname
    }
}
```

### <a name="get-details-about-a-catalog-item"></a>Informatie ophalen over een catalogusitem

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="store-credentials-in-the-catalog"></a>Referenties opslaan in de catalogus

In een U-SQL-database, moet u een referentieobject voor een database die wordt gehost in Azure maken. U-SQL-referenties zijn momenteel het enige type catalogusitem die u via PowerShell kunt maken.

```powershell
$dbName = "master"
$credentialName = "ContosoDbCreds"
$dbUri = "https://contoso.database.windows.net:8080"

New-AdlCatalogCredential -AccountName $adla `
          -DatabaseName $db `
          -CredentialName $credentialName `
          -Credential (Get-Credential) `
          -Uri $dbUri
```

## <a name="manage-firewall-rules"></a>Firewall-regels beheren

### <a name="list-firewall-rules"></a>Lijst met firewall-regels

```powershell
Get-AdlAnalyticsFirewallRule -Account $adla
```

### <a name="add-a-firewall-rule"></a>Een firewallregel toevoegen

```powershell
$ruleName = "Allow access from on-prem server"
$startIpAddress = "<start IP address>"
$endIpAddress = "<end IP address>"

Add-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

### <a name="modify-a-firewall-rule"></a>Een firewallregel wijzigen

```powershell
Set-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName -StartIpAddress $startIpAddress -EndIpAddress $endIpAddress
```

### <a name="remove-a-firewall-rule"></a>Een firewallregel verwijderen

```powershell
Remove-AdlAnalyticsFirewallRule -Account $adla -Name $ruleName
```

### <a name="allow-azure-ip-addresses"></a>Azure-IP-adressen toestaan

```powershell
Set-AdlAnalyticsAccount -Name $adla -AllowAzureIpState Enabled
```

```powershell
Set-AdlAnalyticsAccount -Name $adla -FirewallState Enabled
Set-AdlAnalyticsAccount -Name $adla -FirewallState Disabled
```


## <a name="working-with-azure"></a>Werken met Azure

### <a name="get-details-of-azurerm-errors"></a>Details van fouten AzureRm ophalen

```powershell
Resolve-AzureRmError -Last
```

### <a name="verify-if-you-are-running-as-an-administrator-on-your-windows-machine"></a>Controleer of als u als beheerder worden uitgevoerd op uw Windows-computer

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Een TenantID vinden

Van de naam van een abonnement:

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

Van een abonnement-ID:

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

Van een domeinadres zoals 'contoso.com'

```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Een lijst van al uw abonnementen en tenant-id 's

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Een Data Lake Analytics-account met een sjabloon maken

U kunt ook een Azure-resourcegroep sjabloon met het volgende voorbeeld: [een Data Lake Analytics-account met een sjabloon maken](https://github.com/Azure-Samples/data-lake-analytics-create-account-with-arm-template)

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* Aan de slag met Data Lake Analytics met [Azure-portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)
* Azure Data Lake Analytics beheren met [Azure-portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) 
