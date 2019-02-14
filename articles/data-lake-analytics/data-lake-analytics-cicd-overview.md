---
title: Een CI/CD-pijplijn instellen voor Azure Data Lake Analytics
description: Meer informatie over het instellen van continue integratie en continue implementatie voor Azure Data Lake Analytics.
services: data-lake-analytics
author: yanancai
ms.author: yanacai
ms.reviewer: jasonwhowell
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.topic: conceptual
ms.workload: big-data
ms.date: 09/14/2018
ms.openlocfilehash: 937e261405634e88ab234d2fe43ee660a3acc417
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56233656"
---
# <a name="how-to-set-up-a-cicd-pipeline-for-azure-data-lake-analytics"></a>Een CI/CD-pijplijn instellen voor Azure Data Lake Analytics  

In dit artikel leert u hoe u voor het instellen van een continue integratie en implementatie (CI/CD) pijplijn voor U-SQL-taken en U-SQL-databases.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="use-cicd-for-u-sql-jobs"></a>CI/CD voor U-SQL-taken gebruiken

Azure Data Lake Tools voor Visual Studio biedt het U-SQL project-type dat helpt u bij het organiseren van U-SQL-scripts. Met behulp van de U-SQL-project voor het beheren van uw U-SQL-code eenvoudig verder CI/CD-scenario's.

## <a name="build-a-u-sql-project"></a>Bouw een U-SQL-project

Een U-SQL-project kan worden gebouwd met de Microsoft Build Engine (MSBuild) door de bijbehorende parameters doorgeven. Volg de stappen in dit artikel voor het instellen van een buildproces voor een U-SQL-project.

### <a name="project-migration"></a>Project-migratie

Voordat u een taak maken voor een U-SQL project instelt, zorg er dan voor dat u hebt de nieuwste versie van het project U-SQL. Open het bestand met U-SQL project in de editor en controleer of dat u deze hebt items te importeren:

```   
<!-- check for SDK Build target in current path then in USQLSDKPath-->
<Import Project="UsqlSDKBuild.targets" Condition="Exists('UsqlSDKBuild.targets')" />
<Import Project="$(USQLSDKPath)\UsqlSDKBuild.targets" Condition="!Exists('UsqlSDKBuild.targets') And '$(USQLSDKPath)' != '' And Exists('$(USQLSDKPath)\UsqlSDKBuild.targets')" />
``` 

Als dat niet het geval is, hebt u twee opties voor het migreren van het project:

- Optie 1: De oude importeren item wijzigt naar het vorige item.
- Optie 2: Het oude project openen in de Azure Data Lake Tools voor Visual Studio. Gebruik een versie is nieuwer dan 2.3.3000.0. De oude projectsjabloon worden automatisch bijgewerkt naar de nieuwste versie. Nieuwe projecten die zijn gemaakt met versie nieuwer is dan 2.3.3000.0 gebruikgemaakt van de nieuwe sjabloon.

### <a name="get-nuget"></a>NuGet ophalen

MSBuild biedt geen ingebouwde ondersteuning voor U-SQL-projecten. Als u deze ondersteuning, moet u een verwijzing voor uw oplossing wilt toevoegen de [Microsoft.Azure.DataLake.USQL.SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) NuGet-pakket waarmee de gewenste taal-service worden toegevoegd.

De verwijzing van NuGet-pakket wilt toevoegen, met de rechtermuisknop op de oplossing in Visual Studio Solution Explorer en kies **NuGet-pakketten beheren**. Of u kunt een bestand met de naam `packages.config` in de oplossingenmap en put de volgende inhoud in deze:

```xml 
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180620" targetFramework="net452" />
</packages>
``` 

### <a name="manage-u-sql-database-references"></a>U-SQL-databaseverwijzingen beheren

U-SQL-scripts in een U-SQL-project kunnen queryinstructies voor de U-SQL database-objecten hebben. In dat geval moet u verwijzen naar de bijbehorende U-SQL-databaseproject met de definitie van de objecten voordat u de U-SQL-project maken. Een voorbeeld is wanneer u een U-SQL-tabel op of verwijs naar een assembly. 

Meer informatie over [U-SQL-databaseproject](data-lake-analytics-data-lake-tools-develop-usql-database.md).

>[!NOTE]
>U-SQL database-project is momenteel in openbare preview. Als u de instructie DROP in het project hebt, mislukt de build. De instructie DROP zal binnenkort worden toegestaan.
>

### <a name="build-a-u-sql-project-with-the-msbuild-command-line"></a>Bouw een U-SQL project met de MSBuild-opdrachtregel

Eerst het project migreren en ophalen van het NuGet-pakket. Roep vervolgens de standaard MSBuild-opdrachtregel met de volgende aanvullende argumenten voor het ontwikkelen van uw U-SQL-project: 

``` 
msbuild USQLBuild.usqlproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime;USQLTargetType=SyntaxCheck;DataRoot=datarootfolder;/p:EnableDeployment=true
``` 

De definitie van de argumenten en waarden zijn als volgt:

* **USQLSDKPath = < U-SQL-Nuget-pakket > \build\runtime**. Deze parameter verwijst naar het installatiepad van het NuGet-pakket voor de U-SQL-taalservice.
* **USQLTargetType = samenvoegen of SyntaxCheck**:
    * **Merge**. Modus compileert code-behind-bestanden samenvoegen. Voorbeelden zijn **.cs**, **.py**, en **.r** bestanden. Deze inlines de resulterende zelfgedefinieerde-codebibliotheek in de U-SQL-script. Voorbeelden zijn een DLL-bestand binair, Python of R code.
    * **SyntaxCheck**. Code-behind-bestanden samengevoegd SyntaxCheck modus eerst in de U-SQL-script. Vervolgens wordt de U-SQL-script voor het valideren van uw code gecompileerd.
* **DataRoot=<DataRoot path>**. DataRoot is alleen nodig voor SyntaxCheck modus. Wanneer deze SyntaxCheck modus voor het script wordt gemaakt, controleert MSBuild de verwijzingen naar objecten in de database in het script. Voordat u gebouw, instellen van een overeenkomende lokale omgeving waarin de waarnaar wordt verwezen, objecten uit de U-SQL-database in de map DataRoot van de build-machine. U kunt ook de databaseafhankelijkheden door beheren [verwijst naar een U-SQL-database-project](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project). MSBuild alleen verwijzingen in de database-object, bestanden niet gecontroleerd.
* **EnableDeployment = true** of **false**. EnableDeployment wordt aangegeven als het is toegestaan om te distribueren waarnaar wordt verwezen, U-SQL-databases tijdens het bouwproces. Als u verwijst naar een project U-SQL-database en de database-objecten in uw U-SQL-script gebruiken, stel deze parameter in op **waar**.

### <a name="continuous-integration-through-azure-pipelines"></a>Continue integratie met Azure-pijplijnen

Naast de opdrachtregel, kunt u ook de Visual Studio-Build of een taak MSBuild U-SQL-projecten in Azure pijplijnen bouwen gebruiken. Als u een build-pijplijn instelt, zorg ervoor dat u twee taken toevoegen in de build-pijplijn: de taak van een NuGet herstellen en een MSBuild-taak.

![MSBuild-taak voor een U-SQL-project](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

1.  Toevoegen van een taak voor het herstellen van NuGet om op te halen van het op oplossing verwezen NuGet-pakket met `Azure.DataLake.USQL.SDK`, zodat MSBuild de U-SQL-taal-doelen vindt. Stel **Geavanceerd** > **doelmap** naar `$(Build.SourcesDirectory)/packages` als u wilt gebruiken in het voorbeeld MSBuild-argumenten rechtstreeks in stap 2.

    ![NuGet terugzetten taak voor een U-SQL-project](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2.  MSBuild-argumenten in hulpprogramma's voor het bouwen van Visual Studio of in een taak MSBuild ingesteld zoals wordt weergegeven in het volgende voorbeeld. Of u de variabelen voor deze argumenten in de Azure-pijplijnen build-pijplijn kunt definiëren.

    ![CI/CD MSBuild-variabelen voor een U-SQL project definiëren](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables.png) 

    ```
    /p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime /p:USQLTargetType=SyntaxCheck /p:DataRoot=$(Build.SourcesDirectory) /p:EnableDeployment=true
    ```

### <a name="u-sql-project-build-output"></a>De uitvoer van de build project U-SQL

Nadat u een build uitvoert, alle scripts in de U-SQL project zijn gebouwd en uitvoer naar een zipbestand met de naam `USQLProjectName.usqlpack`. De mapstructuur in uw project wordt opgeslagen in de uitvoer van de ingepakte build.

> [!NOTE]
>
> Code-behind-bestanden voor de U-SQL-script worden samengevoegd als een inline-instructie voor de build-scriptuitvoer.
>

## <a name="test-u-sql-scripts"></a>Testen van U-SQL-scripts

Azure Data Lake biedt en Testprojecten voor U-SQL-scripts en C# UDO/UDAG/UDF:
* Meer informatie over het [testcases voor U-SQL-scripts en uitgebreide C#-code toevoegen](data-lake-analytics-cicd-test.md#test-u-sql-scripts).
* Meer informatie over het [testcases uitvoeren in Azure pijplijnen](data-lake-analytics-cicd-test.md#run-test-cases-in-azure-devops).

## <a name="deploy-a-u-sql-job"></a>Implementaties van een U-SQL-taak

Nadat u code bij het bouwen en testen verifieert, kunt u U-SQL-taken rechtstreeks vanuit Azure pijplijnen via een Azure PowerShell-taken kunt indienen. U kunt het script ook implementeren op Azure Data Lake Store of Azure Blob storage en [de geplande taken worden uitgevoerd via Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

### <a name="submit-u-sql-jobs-through-azure-pipelines"></a>U-SQL-taken via Azure pijplijnen verzenden

De build-uitvoer van het project U-SQL is een zip-bestand met de naam **USQLProjectName.usqlpack**. Het zip-bestand bevat alle U-SQL-scripts in het project. U kunt de [Azure PowerShell-taak](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-powershell?view=vsts) in pijplijnen met de volgende PowerShell-voorbeeldscript voor het verzenden van U-SQL-taken rechtstreeks vanuit Azure pijplijnen.

```powershell
<#
    This script can be used to submit U-SQL Jobs with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and submit all scripts one-by-one.

    Note: the code behind file for each U-SQL script will be merged into the built U-SQL script in build output.
          
    Example :
        USQLJobSubmission.ps1 -ADLAAccountName "myadlaaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\" -DegreeOfParallelism 2
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLAAccountName, # ADLA account name to submit U-SQL jobs
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DegreeOfParallelism = 1
)

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

## Get U-SQL scripts in U-SQL project build output(.usqlpack file)
Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    $USQLFiles = Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue

    return $USQLFiles
}

## Submit U-SQL scripts to ADLA account one-by-one
Function SubmitAnalyticsJob()
{
    $usqlFiles = GetUsqlFiles

    Write-Output "$($usqlFiles.Count) jobs to be submitted..."

    # Submit each usql script and wait for completion before moving ahead.
    foreach ($usqlFile in $usqlFiles)
    {
        $scriptName = "[Release].[$([System.IO.Path]::GetFileNameWithoutExtension($usqlFile.fullname))]"

        Write-Output "Submitting job for '{$usqlFile}'"

        $jobToSubmit = Submit-AzDataLakeAnalyticsJob -Account $ADLAAccountName -Name $scriptName -ScriptPath $usqlFile -DegreeOfParallelism $DegreeOfParallelism
        
        LogJobInformation $jobToSubmit
        
        Write-Output "Waiting for job to complete. Job ID:'{$($jobToSubmit.JobId)}', Name: '$($jobToSubmit.Name)' "
        $jobResult = Wait-AzDataLakeAnalyticsJob -Account $ADLAAccountName -JobId $jobToSubmit.JobId  
        LogJobInformation $jobResult
    }
}

Function LogJobInformation($jobInfo)
{
    Write-Output "************************************************************************"
    Write-Output ([string]::Format("Job Id: {0}", $(DefaultIfNull $jobInfo.JobId)))
    Write-Output ([string]::Format("Job Name: {0}", $(DefaultIfNull $jobInfo.Name)))
    Write-Output ([string]::Format("Job State: {0}", $(DefaultIfNull $jobInfo.State)))
    Write-Output ([string]::Format("Job Started at: {0}", $(DefaultIfNull  $jobInfo.StartTime)))
    Write-Output ([string]::Format("Job Ended at: {0}", $(DefaultIfNull  $jobInfo.EndTime)))
    Write-Output ([string]::Format("Job Result: {0}", $(DefaultIfNull $jobInfo.Result)))
    Write-Output "************************************************************************"
}

Function DefaultIfNull($item)
{
    if ($item -ne $null)
    {
        return $item
    }
    return ""
}

Function Main()
{
    Write-Output ([string]::Format("ADLA account: {0}", $ADLAAccountName))
    Write-Output ([string]::Format("Root folde for usqlpack: {0}", $ArtifactsRoot))
    Write-Output ([string]::Format("AU count: {0}", $DegreeOfParallelism))

    Write-Output "Starting USQL script deployment..."
    
    SubmitAnalyticsJob

    Write-Output "Finished deployment..."
}

Main
```

### <a name="deploy-u-sql-jobs-through-azure-data-factory"></a>U-SQL-taken via Azure Data Factory implementeren

U kunt U-SQL-taken rechtstreeks vanuit Azure pijplijnen indienen. Of u kunt de ingebouwde scripts uploaden naar Azure Data Lake Store of Azure Blob-opslag en [de geplande taken worden uitgevoerd via Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

Gebruik de [Azure PowerShell-taak](https://docs.microsoft.com/azure/devops/pipelines/tasks/deploy/azure-powershell?view=vsts) in Azure-pijplijnen met de volgende PowerShell-voorbeeldscript de U-SQL-scripts uploaden naar een Azure Data Lake Store-account:

```powershell
<#
    This script can be used to upload U-SQL files to ADLS with given U-SQL project build output(.usqlpack file).
    This will unzip the U-SQL project build output, and upload all scripts to ADLS one-by-one.
          
    Example :
        FileUpload.ps1 -ADLSName "myadlsaccount" -ArtifactsRoot "C:\USQLProject\bin\debug\"
#>

param(
    [Parameter(Mandatory=$true)][string]$ADLSName, # ADLS account name to upload U-SQL scripts
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, # Root folder of U-SQL project build output
    [Parameter(Mandatory=$false)][string]$DestinationFolder = "USQLScriptSource" # Destination folder in ADLS
)

Function UploadResources()
{
    Write-Host "************************************************************************"
    Write-Host "Uploading files to $ADLSName"
    Write-Host "***********************************************************************"

    $usqlScripts = GetUsqlFiles

    $files = @(get-childitem $usqlScripts -recurse)
    foreach($file in $files)
    {
        Write-Host "Uploading file: $($file.Name)"
        Import-AzDataLakeStoreItem -AccountName $ADLSName -Path $file.FullName -Destination "/$(Join-Path $DestinationFolder $file)" -Force
    }
}

function Unzip($USQLPackfile, $UnzipOutput)
{
    $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
    Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    Rename-Item -Path $USQLPackfileZip -NewName $([System.IO.Path]::ChangeExtension($USQLPackfileZip, ".usqlpack")) -Force
}

Function GetUsqlFiles()
{

    $USQLPackfiles = Get-ChildItem -Path $ArtifactsRoot -Include *.usqlpack -File -Recurse -ErrorAction SilentlyContinue

    $UnzipOutput = Join-Path $ArtifactsRoot -ChildPath "UnzipUSQLScripts"

    foreach ($USQLPackfile in $USQLPackfiles)
    {
        Unzip $USQLPackfile $UnzipOutput
    }

    return Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue
}

UploadResources
```

## <a name="cicd-for-a-u-sql-database"></a>CI/CD voor een U-SQL-database

Azure Data Lake Tools voor Visual Studio biedt U-SQL-database-projectsjablonen waarmee u kunt ontwikkelen, beheren en implementeren van U-SQL-databases. Meer informatie over een [U-SQL-databaseproject](data-lake-analytics-data-lake-tools-develop-usql-database.md).

## <a name="build-u-sql-database-project"></a>U-SQL database-project maken

### <a name="get-the-nuget-package"></a>Het NuGet-pakket downloaden

MSBuild biedt geen ingebouwde ondersteuning voor de U-SQL database-projecten. Als u deze mogelijkheid, moet u een verwijzing voor uw oplossing wilt toevoegen de [Microsoft.Azure.DataLake.USQL.SDK](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) NuGet-pakket waarmee de gewenste taal-service worden toegevoegd.

De verwijzing van NuGet-pakket wilt toevoegen, met de rechtermuisknop op de oplossing in Visual Studio Solution Explorer. Kies **NuGet-pakketten beheren**. Zoek en installeer het NuGet-pakket. Of u kunt een bestand met de naam **packages.config** in de oplossingenmap en put de volgende inhoud in deze:

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180615" targetFramework="net452" />
</packages>
```

### <a name="build-u-sql-a-database-project-with-the-msbuild-command-line"></a>U-SQL-een database-project met de opdrachtregel MSBuild bouwen

Aanroepen van de standaard MSBuild-opdrachtregel voor het bouwen van uw project U-SQL-database, en de verwijzing van U-SQL SDK NuGet-pakket als een extra argument doorgeven. Zie het volgende voorbeeld: 

```
msbuild DatabaseProject.usqldbproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime
```

Het argument `USQLSDKPath=<U-SQL Nuget package>\build\runtime` verwijst naar het installatiepad van het NuGet-pakket voor de U-SQL-taalservice.

### <a name="continuous-integration-with-azure-pipelines"></a>Continue integratie met Azure-pijplijnen

Naast de opdrachtregel, kunt u Visual Studio bouwen of een taak MSBuild om U-SQL database-projecten in Azure-pijplijnen te bouwen. Als u een build-taak instelt, zorg ervoor dat u twee taken toevoegen in de build-pijplijn: de taak van een NuGet herstellen en een MSBuild-taak.

   ![CI/CD MSBuild-taak voor een U-SQL-project](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 


1.  Toevoegen van een taak voor het herstellen van NuGet om op te halen van het pakket NuGet oplossing-waarnaar wordt verwezen, waaronder `Azure.DataLake.USQL.SDK`, zodat MSBuild de U-SQL-taal-doelen vindt. Stel **Geavanceerd** > **doelmap** naar `$(Build.SourcesDirectory)/packages` als u wilt gebruiken in het voorbeeld MSBuild-argumenten rechtstreeks in stap 2.

    ![CI/CD-NuGet-taak voor een U-SQL-project](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-nuget-task.png)

2.  MSBuild-argumenten in hulpprogramma's voor het bouwen van Visual Studio of in een taak MSBuild ingesteld zoals wordt weergegeven in het volgende voorbeeld. Of u de variabelen voor deze argumenten in de Azure-pijplijnen build-pijplijn kunt definiëren.

   ![CI/CD MSBuild-variabelen voor een project U-SQL-database definiëren](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables-database-project.png) 

    ```
    /p:USQLSDKPath=$(Build.SourcesDirectory)/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.180615/build/runtime
    ```
 
### <a name="u-sql-database-project-build-output"></a>De uitvoer van U-SQL-database project build

De uitvoer voor een project U-SQL-database de implementatiepakket voor een U-SQL-database is, met de naam met het achtervoegsel build `.usqldbpack`. De `.usqldbpack` pakket is een zip-bestand met alle DDL-componenten in een enkel U-SQL-script in een DDL-map. Bevat alle **dll** en aanvullende bestanden voor de assembly in een tijdelijke map.

## <a name="test-table-valued-functions-and-stored-procedures"></a>Test tabelfuncties en opgeslagen procedures

Rechtstreeks toe te voegen testcases voor tabelfuncties en opgeslagen procedures wordt momenteel niet ondersteund. Als tijdelijke oplossing, kunt u een U-SQL-project dat U-SQL-scripts die deze functies aanroepen en Testscenario's schrijven voor hen heeft. De volgende stappen voor het instellen van testcases voor tabelfuncties en opgeslagen procedures die zijn gedefinieerd in het project U-SQL-database:

1.  Maken van een U-SQL-project voor testdoeleinden en U-SQL-scripts aanroepen van de tabelfuncties en opgeslagen procedures te schrijven.
2.  Voeg een databaseverwijzing aan het project U-SQL. Als u de functie met tabelwaarden en de definitie van de opgeslagen procedure, moet u verwijzen naar de databaseproject met daarin de DDL-instructie. Meer informatie over [verwijzingen van de database](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).
3.  Voeg testcases voor U-SQL-scripts waarmee tabelfuncties en opgeslagen procedures worden aangeroepen. Meer informatie over het [testcases voor U-SQL-scripts toevoegen](data-lake-analytics-cicd-test.md#test-u-sql-scripts).

## <a name="deploy-u-sql-database-through-azure-pipelines"></a>U-SQL-database via Azure pijplijnen implementeren

`PackageDeploymentTool.exe` bevat het programmeren en opdrachtregelinterfaces die helpen bij het implementeren van U-SQL-database-implementatiepakketten, **.usqldbpack**. De SDK is opgenomen in de [U-SQL SDK NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), dat zich bevindt op **build/runtime/PackageDeploymentTool.exe**. Met behulp van `PackageDeploymentTool.exe`, kunt u U-SQL-databases op zowel de Azure Data Lake Analytics en de lokale accounts implementeren.

> [!NOTE]
>
> PowerShell-opdrachtregel ondersteuning en Azure pijplijnen vrijgevingstaak ondersteuning voor de implementatie van de U-SQL-database momenteel in behandeling is.
>

De volgende stappen voor het instellen van een taak database implementatie in Azure pijplijnen:

1. Een PowerShell-Script-taak in een build toevoegen of release-pijplijn en voer de volgende PowerShell-script. Deze taak helpt bij het ophalen van de Azure SDK-afhankelijkheden voor `PackageDeploymentTool.exe` en `PackageDeploymentTool.exe`. U kunt instellen dat de **- AzureSDK** en **- DBDeploymentTool** parameters om de afhankelijkheden en deployment tool uit naar specifieke mappen te laden. Geeft de **- AzureSDK** pad naar `PackageDeploymentTool.exe` als de **- AzureSDKPath** parameter in stap 2. 

    ```powershell
    <#
        This script is used for getting dependencies and SDKs for U-SQL database deployment.
        PowerShell command line support for deploying U-SQL database package(.usqldbpack file) will come soon.
        
        Example :
            GetUSQLDBDeploymentSDK.ps1 -AzureSDK "AzureSDKFolderPath" -DBDeploymentTool "DBDeploymentToolFolderPath"
    #>

    param (
        [string]$AzureSDK = "AzureSDK", # Folder to cache Azure SDK dependencies
        [string]$DBDeploymentTool = "DBDeploymentTool", # Folder to cache U-SQL database deployment tool
        [string]$workingfolder = "" # Folder to execute these command lines
    )

    if ([string]::IsNullOrEmpty($workingfolder))
    {
        $scriptpath = $MyInvocation.MyCommand.Path
        $workingfolder = Split-Path $scriptpath
    }
    cd $workingfolder

    echo "workingfolder=$workingfolder, outputfolder=$outputfolder"
    echo "Downloading required packages..."

    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Analytics/3.5.1-preview -outf Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Store/2.4.1-preview -outf Microsoft.Azure.Management.DataLake.Store.2.4.1-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.3 -outf Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime/2.3.11 -outf Microsoft.Rest.ClientRuntime.2.3.11.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure/3.3.7 -outf Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure.Authentication/2.3.3 -outf Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip
    iwr https://www.nuget.org/api/v2/package/Newtonsoft.Json/6.0.8 -outf Newtonsoft.Json.6.0.8.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.DataLake.USQL.SDK/ -outf USQLSDK.zip

    echo "Extracting packages..."

    Expand-Archive Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview -Force
    Expand-Archive Microsoft.Azure.Management.DataLake.Store.2.4.1-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Store.2.4.1-preview -Force
    Expand-Archive Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip -DestinationPath Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.2.3.11.zip -DestinationPath Microsoft.Rest.ClientRuntime.2.3.11 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.3.3.7 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3 -Force
    Expand-Archive Newtonsoft.Json.6.0.8.zip -DestinationPath Newtonsoft.Json.6.0.8 -Force
    Expand-Archive USQLSDK.zip -DestinationPath USQLSDK -Force

    echo "Copy required DLLs to output folder..."

    mkdir $AzureSDK -Force
    mkdir $DBDeploymentTool -Force
    copy Microsoft.Azure.Management.DataLake.Analytics.3.5.1-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.Azure.Management.DataLake.Store.2.4.1-preview\lib\net452\*.dll $AzureSDK
    copy Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.2.3.11\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\*.dll $AzureSDK
    copy Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3\lib\net452\*.dll $AzureSDK
    copy Newtonsoft.Json.6.0.8\lib\net45\*.dll $AzureSDK
    copy USQLSDK\build\runtime\*.* $DBDeploymentTool
    ```

2. Voeg een **opdrachtregeltaak** in een build of release-pijplijn en vul in het script door het aanroepen van `PackageDeploymentTool.exe`. `PackageDeploymentTool.exe` bevindt zich onder de gedefinieerde **$DBDeploymentTool** map. Het voorbeeldscript is als volgt: 

    * Lokaal een U-SQL-database implementeren:

        ```
        PackageDeploymentTool.exe deploylocal -Package <package path> -Database <database name> -DataRoot <data root path>
        ```

    * Gebruik interactieve verificatiemodus voor het implementeren van een U-SQL-database aan een Azure Data Lake Analytics-account:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -AzureSDKPath <azure sdk path> -Interactive
        ```

    * Gebruik **secrete** verificatie voor het implementeren van een U-SQL-database aan een Azure Data Lake Analytics-account:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -ClientId <client id> -Secrete <secrete>
        ```

    * Gebruik **CertBestand** verificatie voor het implementeren van een U-SQL-database aan een Azure Data Lake Analytics-account:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tenant name> -ClientId <client id> -Secrete <secrete> -CertFile <certFile>
        ```

### <a name="packagedeploymenttoolexe-parameter-descriptions"></a>Beschrijvingen van de parameters PackageDeploymentTool.exe

#### <a name="common-parameters"></a>Algemene parameters

| Parameter | Description | Standaardwaarde | Vereist |
|---------|-----------|-------------|--------|
|Pakket|Het pad van het implementatiepakket voor U-SQL-database om te worden geïmplementeerd.|null|true|
|Database|De naam van de database moet worden geïmplementeerd naar of gemaakt.|model|false|
|LogFile|Het pad van het bestand voor logboekregistratie. Standaard standard uit (console).|null|false|
|LogLevel|Logboekniveau: Uitgebreide, normaal, waarschuwing of fout.|LogLevel.Normal|false|

#### <a name="parameter-for-local-deployment"></a>{De parameter voor lokale implementatie

|Parameter|Description|Standaardwaarde|Vereist|
|---------|-----------|-------------|--------|
|DataRoot|Het pad van de hoofdmap van de lokale gegevens.|null|true|

#### <a name="parameters-for-azure-data-lake-analytics-deployment"></a>Parameters voor de implementatie van Azure Data Lake Analytics

|Parameter|Description|Standaardwaarde|Vereist|
|---------|-----------|-------------|--------|
|Account|Geeft aan welk Azure Data Lake Analytics-account om te implementeren op met de accountnaam.|null|true|
|ResourceGroup|De groepsnaam van de Azure-resource voor de Azure Data Lake Analytics-account.|null|true|
|SubscriptionId|De Azure-abonnement-ID voor de Azure Data Lake Analytics-account.|null|true|
|Tenant|Naam van de tenant is de domeinnaam van de Azure Active Directory (Azure AD). Deze vinden op de pagina voor het beheer van abonnementen in Azure portal.|null|true|
|AzureSDKPath|Het pad naar het afhankelijke assembly's zoeken in de Azure SDK.|null|true|
|Interactief|Al dan niet de interactieve modus gebruiken voor verificatie.|false|false|
|ClientId|De Azure AD-toepassings-ID is vereist voor niet-interactieve verificatie.|null|Vereist voor niet-interactieve verificatie.|
|Secrete|De secrete of het wachtwoord voor niet-interactieve verificatie. Deze moet worden gebruikt alleen in een betrouwbare en veilige omgeving.|null|Vereist voor niet-interactieve verificatie, anders gebruik SecreteFile.|
|SecreteFile|Het bestand wordt opgeslagen de secrete of het wachtwoord voor niet-interactieve verificatie. Zorg ervoor dat u het leesbare houden alleen door de huidige gebruiker.|null|Vereist voor niet-interactieve verificatie, anders gebruik Secrete.|
|CertFile|Het bestand wordt opgeslagen x.509-certificaat voor niet-interactieve verificatie. De standaardwaarde is het gebruik van de client secrete verificatie.|null|false|
| JobPrefix | Het voorvoegsel voor de implementatie van de database van een U-SQL-DDL-taak. | Deploy_ + DateTime.Now | false |

## <a name="next-steps"></a>Volgende stappen

- [Het testen van uw Azure Data Lake Analytics-code](data-lake-analytics-cicd-test.md).
- [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md).
- [Gebruik U-SQL database-project voor het ontwikkelen van U-SQL-database](data-lake-analytics-data-lake-tools-develop-usql-database.md).
