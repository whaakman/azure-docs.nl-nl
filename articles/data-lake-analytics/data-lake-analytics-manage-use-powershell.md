---
title: Azure Data Lake Analytics beheren met Azure PowerShell
description: In dit artikel wordt beschreven hoe u Azure PowerShell gebruiken voor het beheren van Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: matt1883
ms.author: mahi
ms.reviewer: jasonwhowell
ms.assetid: ad14d53c-fed4-478d-ab4b-6d2e14ff2097
ms.topic: conceptual
ms.date: 06/29/2018
ms.openlocfilehash: 14454c20e541a8b4450cd5a55ce5fed4268debc6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041945"
---
# <a name="manage-azure-data-lake-analytics-using-azure-powershell"></a>Azure Data Lake Analytics beheren met Azure PowerShell
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

In dit artikel wordt beschreven hoe u Azure Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken beheren met behulp van Azure PowerShell.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van PowerShell met Data Lake Analytics, verzamelt de volgende soorten informatie: 

* **Abonnements-ID**: de ID van het Azure-abonnement met uw Data Lake Analytics-account.
* **Resourcegroep**: de naam van de Azure-resourcegroep waarin uw Data Lake Analytics-account.
* **Data Lake Analytics-accountnaam**: de naam van uw Data Lake Analytics-account.
* **Standaard Data Lake Store-accountnaam**: elke Data Lake Analytics-account heeft een Data Lake Store-standaardaccount.
* **Locatie**: de locatie van uw Data Lake Analytics-account, zoals "VS-Oost 2" of andere ondersteunde locaties.

De PowerShell-fragmenten in deze zelfstudie gebruiken deze variabelen om deze informatie op te slaan

```powershell
$subId = "<SubscriptionId>"
$rg = "<ResourceGroupName>"
$adla = "<DataLakeAnalyticsAccountName>"
$adls = "<DataLakeStoreAccountName>"
$location = "<Location>"
```

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

### <a name="log-in-using-interactive-user-authentication"></a>Meld u met behulp van interactieve gebruikersverificatie

Meld u bij het gebruik van een abonnements-ID of door de naam van abonnement

```powershell
# Using subscription id
Connect-AzureRmAccount -SubscriptionId $subId

# Using subscription name
Connect-AzureRmAccount -SubscriptionName $subname 
```

## <a name="saving-authentication-context"></a>Opslaan van de verificatiecontext

De `Connect-AzureRmAccount` cmdlet vraagt altijd om referenties. U kunt voorkomen dat u wordt gevraagd door met de volgende cmdlets:

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

### <a name="get-account-information"></a>Ophalen van accountgegevens

Meer informatie over een account.

```powershell
Get-AdlAnalyticsAccount -Name $adla
```

### <a name="check-if-an-account-exists"></a>Controleer of er bestaat al een account

```powershell
Test-AdlAnalyticsAccount -Name $adla
```

## <a name="manage-data-sources"></a>Gegevensbronnen beheren
Azure Data Lake Analytics ondersteunt momenteel de volgende gegevensbronnen:

* [Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md)
* [Azure Storage](../storage/common/storage-introduction.md)

Elk Data Lake Analytics-account heeft een Data Lake Store-standaardaccount. De standaard Data Lake Store-account wordt gebruikt voor het opslaan van taak metagegevens en taak auditlogboeken. 

### <a name="find-the-default-data-lake-store-account"></a>De standaard Data Lake Store-account zoeken

```powershell
$adla_acct = Get-AdlAnalyticsAccount -Name $adla
$dataLakeStoreName = $adla_acct.DefaultDataLakeAccount
```

U kunt het Data Lake Store-standaardaccount vinden door te filteren van de lijst met gegevensbronnen met de `IsDefault` eigenschap:

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
Get-AdlAnalyticsDataSource -Account $adla

# List attached Data Lake Store accounts
Get-AdlAnalyticsDataSource -Account $adla | where -Property Type -EQ "DataLakeStore"

# List attached Storage accounts
Get-AdlAnalyticsDataSource -Account $adla | where -Property Type -EQ "Blob"
```

## <a name="submit-u-sql-jobs"></a>U-SQL-taken verzenden

### <a name="submit-a-string-as-a-u-sql-job"></a>Een tekenreeks dient als een U-SQL-taak

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

### <a name="submit-a-file-as-a-u-sql-job"></a>Een bestand als een U-SQL-taak verzenden

```powershell
$scriptpath = "d:\test.usql"
$script | Out-File $scriptpath 
Submit-AdlJob -AccountName $adla –ScriptPath $scriptpath -Name "Demo"
```

### <a name="list-jobs"></a>Lijst met taken

De uitvoer bevat de taken die momenteel worden uitgevoerd en de taken die onlangs zijn voltooid.

```powershell
Get-AdlJob -Account $adla
```

### <a name="list-the-top-n-jobs"></a>Lijst van de top N-taken

Standaard die de lijst met taken wordt gesorteerd op tijd te verzenden. Dus weergegeven de meest recent verzonden taken eerst. Standaard de ADLA account onthoudt taken gedurende 180 dagen, maar de cmdlet Get-AdlJob standaard retourneert alleen de eerste 500. Gebruik de - bovenste parameter om een bepaald aantal taken weer te geven.

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

### <a name="list-jobs-by-job-result"></a>Lijst met taken op taak resultaat

Gebruik de `-Result` parameter om te detecteren of beëindigde taken is voltooid. Deze heeft de volgende waarden:

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

### <a name="list-jobs-by-job-submitter"></a>Lijst met taken op de indiener van taak

De `-Submitter` parameter kunt u identificeren die een taak hebt verzonden.

```powershell
Get-AdlJob -Account $adla -Submitter "joe@contoso.com"
```

### <a name="list-jobs-by-submission-time"></a>Lijst met taken op tijd van verzending

De `-SubmittedAfter` is handig bij het filteren van een tijdsbereik.


```powershell
# List  jobs submitted in the last day.
$d = [DateTime]::Now.AddDays(-1)
Get-AdlJob -Account $adla -SubmittedAfter $d

# List  jobs submitted in the last seven day.
$d = [DateTime]::Now.AddDays(-7)
Get-AdlJob -Account $adla -SubmittedAfter $d
```

### <a name="get-job-status"></a>Taakstatus

Haal de status van een bepaalde taak op.

```powershell
Get-AdlJob -AccountName $adla -JobId $job.JobId
```


### <a name="cancel-a-job"></a>Een taak annuleren

```powershell
Stop-AdlJob -Account $adla -JobID $jobID
```

### <a name="wait-for-a-job-to-finish"></a>Wachten op een taak is voltooid

In plaats van herhalende `Get-AdlAnalyticsJob` totdat een taak is voltooid, kunt u de `Wait-AdlJob` cmdlet na afloop van de taak te beëindigen.

```powershell
Wait-AdlJob -Account $adla -JobId $job.JobId
```

## <a name="analyzing-job-history"></a>Taakgeschiedenis analyseren

Met Azure PowerShell voor het analyseren van de geschiedenis van taken die in Data Lake analytics hebt uitgevoerd, is een krachtige techniek. U kunt deze gebruiken voor inzicht in gebruik en kosten. U kunt meer informatie door te kijken de [voorbeeldopslagplaats geschiedenis projectanalyse](https://github.com/Azure-Samples/data-lake-analytics-powershell-job-history-analysis)  

## <a name="list-job-pipelines-and-recurrences"></a>Lijst met taak-pijplijnen en herhalingen

Gebruik de `Get-AdlJobPipeline` cmdlet om te controleren van de pijplijngegevens eerder verzonden taken.

```powershell
$pipelines = Get-AdlJobPipeline -Account $adla
$pipeline = Get-AdlJobPipeline -Account $adla -PipelineId "<pipeline ID>"
```

Gebruik de `Get-AdlJobRecurrence` cmdlet het terugkeerpatroon om informatie te zien voor eerder verzonden taken.

```powershell
$recurrences = Get-AdlJobRecurrence -Account $adla

$recurrence = Get-AdlJobRecurrence -Account $adla -RecurrenceId "<recurrence ID>"
```


## <a name="manage-compute-policies"></a>Compute-beleid beheren

### <a name="list-existing-compute-policies"></a>Lijst met bestaande compute-beleidsregels

De `Get-AdlAnalyticsComputePolicy` cmdlet wordt informatie over de compute-beleid voor een Data Lake Analytics-account opgehaald.

```powershell
$policies = Get-AdlAnalyticsComputePolicy -Account $adla
```

### <a name="create-a-compute-policy"></a>Een compute-beleid maken

De `New-AdlAnalyticsComputePolicy` met de cmdlet maakt een nieuw beleid voor compute voor een Data Lake Analytics-account. In het volgende voorbeeld wordt het maximumaantal AU's beschikbaar voor de opgegeven gebruiker tot 50 en de prioriteit van de minimale job tot 250 tekens.

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

Upload een bestand.

```powershell
Import-AdlStoreItem -AccountName $adls -Path "c:\data.tsv" -Destination "/data_copy.csv" 
```

Een volledige map recursief uploaden.

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
> Als het proces uploaden of downloaden wordt onderbroken, kunt u proberen te hervatten tijdens het proces door de cmdlet opnieuw uit met de ``-Resume`` vlag.

## <a name="manage-the-u-sql-catalog"></a>De U-SQL-catalogus beheren

De U-SQL-catalogus wordt gebruikt voor het structureren van gegevens en de code, zodat ze kunnen worden gedeeld met U-SQL-scripts. De catalogus biedt de hoogste prestaties mogelijk is met gegevens in Azure Data Lake. Zie [U-SQL-catalogus gebruiken](data-lake-analytics-use-u-sql-catalog.md) voor meer informatie.

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

### <a name="get-details-about-a-catalog-item"></a>Meer informatie over een catalogusitem

```powershell
# Get details of a table
Get-AdlCatalogItem  -Account $adla -ItemType Table -Path "master.dbo.mytable"

# Test existence of a U-SQL database.
Test-AdlCatalogItem  -Account $adla -ItemType Database -Path "master"
```

### <a name="store-credentials-in-the-catalog"></a>Store-referenties in de catalogus

Maak in een U-SQL-database, een object voor de referenties voor een database die wordt gehost in Azure. U-SQL-referenties zijn momenteel het enige type catalogusitem dat u via PowerShell maken kunt.

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

## <a name="manage-firewall-rules"></a>Firewallregels beheren

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

### <a name="modify-a-firewall-rule"></a>Een firewall-regel wijzigen

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

### <a name="verify-if-you-are-running-as-an-administrator-on-your-windows-machine"></a>Controleer of u als beheerder worden uitgevoerd op uw Windows-computer

```powershell
function Test-Administrator  
{  
    $user = [Security.Principal.WindowsIdentity]::GetCurrent();
    $p = New-Object Security.Principal.WindowsPrincipal $user
    $p.IsInRole([Security.Principal.WindowsBuiltinRole]::Administrator)  
}
```

### <a name="find-a-tenantid"></a>Een tenant-id vinden

Uit de naam van een abonnement:

```powershell
function Get-TenantIdFromSubcriptionName( [string] $subname )
{
    $sub = (Get-AzureRmSubscription -SubscriptionName $subname)
    $sub.TenantId
}

Get-TenantIdFromSubcriptionName "ADLTrainingMS"
```

Uit een abonnements-ID:

```powershell
function Get-TenantIdFromSubcriptionId( [string] $subid )
{
    $sub = (Get-AzureRmSubscription -SubscriptionId $subid)
    $sub.TenantId
}

$subid = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
Get-TenantIdFromSubcriptionId $subid
```

Verzendadres van een domein, zoals 'contoso.com'

```powershell
function Get-TenantIdFromDomain( $domain )
{
    $url = "https://login.windows.net/" + $domain + "/.well-known/openid-configuration"
    return (Invoke-WebRequest $url|ConvertFrom-Json).token_endpoint.Split('/')[3]
}

$domain = "contoso.com"
Get-TenantIdFromDomain $domain
```

### <a name="list-all-your-subscriptions-and-tenant-ids"></a>Lijst met al uw abonnementen en tenant-id 's

```powershell
$subs = Get-AzureRmSubscription
foreach ($sub in $subs)
{
    Write-Host $sub.Name "("  $sub.Id ")"
    Write-Host "`tTenant Id" $sub.TenantId
}
```

## <a name="create-a-data-lake-analytics-account-using-a-template"></a>Een Data Lake Analytics-account met behulp van een sjabloon maken

U kunt ook een Azure-resourcegroep-sjabloon met behulp van het volgende voorbeeld: [een Data Lake Analytics-account met behulp van een sjabloon maken](https://github.com/Azure-Samples/data-lake-analytics-create-account-with-arm-template)

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* Aan de slag met Data Lake Analytics met [Azure-portal](data-lake-analytics-get-started-portal.md) | [Azure PowerShell](data-lake-analytics-get-started-powershell.md) | [CLI 2.0](data-lake-analytics-get-started-cli2.md)
* Azure Data Lake Analytics beheren met [Azure-portal](data-lake-analytics-manage-use-portal.md) | [Azure PowerShell](data-lake-analytics-manage-use-powershell.md) | [CLI](data-lake-analytics-manage-use-cli.md) 
