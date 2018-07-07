---
title: CI/CD-pijplijn instellen voor Azure Data Lake Analytics | Microsoft Docs
description: Meer informatie over het instellen van continue integratie en continue implementatie voor Azure Data Lake Analytics.
services: data-lake-analytics
documentationcenter: ''
author: yanancai
manager: ''
editor: ''
ms.assetid: 66dd58b1-0b28-46d1-aaae-43ee2739ae0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/03/2018
ms.author: yanacai
ms.openlocfilehash: a55c8fd95409de4fb1ea725d234de907f79d3c7b
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37889547"
---
# <a name="how-to-set-up-cicd-pipeline-for-azure-data-lake-analytics"></a>CI/CD-pijplijn instellen voor Azure Data Lake Analytics

In dit document leert u hoe u CI/CD-pijplijn voor U-SQL-taken en U-SQL-databases kunt instellen.

## <a name="cicd-for-u-sql-job"></a>CI/CD voor U-SQL-taak

Azure Data Lake Tools voor Visual Studio biedt het U-SQL-projecttype waarmee u orgnize U-SQL-scripts. Maakt de verdere CI/CD-scenario's met behulp van de U-SQL-project voor het beheren van uw U-SQL-code eenvoudig.

## <a name="build-u-sql-project"></a>U-SQL project bouwen

De U-SQL-project kan worden gebouwd met MSBuild door het overeenkomstige parameters doorgeven. Volg de onderstaande stappen voor het instellen van het bouwproces voor U-SQL-projecten.

### <a name="project-migration"></a>Project-migratie

Voordat het instellen van de build-taak voor U-SQL project, zorg ervoor dat u de nieuwste versie van U-SQL project. Open het projectbestand U-SQL in de editor en controleer of u onder-items importeren hebt:

```   
<!-- check for SDK Build target in current path then in USQLSDKPath-->
<Import Project="UsqlSDKBuild.targets" Condition="Exists('UsqlSDKBuild.targets')" />
<Import Project="$(USQLSDKPath)\UsqlSDKBuild.targets" Condition="!Exists('UsqlSDKBuild.targets') And '$(USQLSDKPath)' != '' And Exists('$(USQLSDKPath)\UsqlSDKBuild.targets')" />
``` 

Als dat niet het geval is, hebt u twee opties voor het migreren van het project:

- Optie 1: Wijziging in de oude item naar hierboven importeren.
- Optie 2: Open het oude project in Azure Data Lake Tools voor Visual Studio na versie 2.3.3000.0. De oude projectsjabloon worden automatisch bijgewerkt naar de nieuwste versie. Het nieuwe project na versie 2.3.3000.0 gebruikmaakt van de nieuwe sjabloon rechtstreeks.

### <a name="get-nuget-package"></a>NuGet-pakket downloaden

MSBuild biedt geen ingebouwde ondersteuning voor de U-SQL-projecttype. Als u wilt toevoegen door deze mogelijkheid, moet u een verwijzing voor uw oplossing wilt toevoegen de [Microsoft.Azure.DataLake.USQL.SDK Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) waarmee de gewenste taal-service wordt toegevoegd.

De verwijzing van NuGet-pakket wilt toevoegen, kunt u met de rechtermuisknop op de oplossing in Solution Explorer en kies **NuGet-pakketten beheren**. Of u kunt een bestand met de naam `packages.config` in de map van de oplossing en toe te voegen onder inhoud naartoe.

```xml 
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" targetFramework="net452" />
</packages>
``` 

### <a name="manage-u-sql-database-references"></a>U-SQL-databaseverwijzingen beheren

Als de U-SQL-scripts in de U-SQL project heeft queryinstructies voor de U-SQL database-objecten, bijvoorbeeld query uitvoeren op een U-SQL-tabel of verwijzen naar een assembly, moet u verwijzen naar de bijbehorende U-SQL-databaseproject met de definitie van deze objecten voordat u het bouwen van dit project U-SQL.

[Meer informatie over U-SQL database-project](data-lake-analytics-data-lake-tools-develop-usql-database.md)

### <a name="build-u-sql-project-with-msbuild-command-line"></a>Bouw U-SQL-project met MSBuild-opdrachtregel

Na het migreren van het project en ophalen van het NuGet-pakket, kunt u de standaard MSBuild-opdrachtregel met de extra argumenten hieronder aanroepen om uw U-SQL-project te bouwen:

``` 
msbuild USQLBuild.usqlproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime;USQLTargetType=SyntaxCheck;DataRoot=datarootfolder
``` 

De definitie van de argumenten en waarden zijn:

* USQLSDKPath = < U-SQL-Nuget-pakket > \build\runtime: deze parameter verwijst naar het installatiepad van het NuGet-pakket voor de U-SQL-taalservice.
* USQLTargetType = samenvoegen of SyntaxCheck:
    * Samenvoegen: Code-behind-bestanden, zoals .cs, .py en .r-bestanden en de resulterende bibliotheek van de gebruiker gedefinieerde code inlines wordt gecompileerd van Merge-modus (zoals een DLL-bestand binair, Python of R-code) in de U-SQL-script.
    * SyntaxCheck: SyntaxCheck modus eerst worden samengevoegd code-behind-bestanden in de U-SQL-script, en vervolgens compileert de U-SQL-script voor het valideren van uw code.
* DataRoot =<DataRoot path>: DataRoot is alleen nodig voor de SyntaxCheck modus. MSBuild controleert tijdens het maken van het script met de SyntaxCheck modus, de verwijzingen in het script naar objecten in de database. Zorg ervoor dat het instellen van een overeenkomende lokale omgeving waarin de waarnaar wordt verwezen, objecten uit de U-SQL-database op de build-machine DataRoot map voordat het bouwen. U kunt ook de databaseafhankelijkheden door beheren [verwijst naar een U-SQL-database-project](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project). Houd er rekening mee dat MSBuild naslaginformatie in de database-objecten, bestanden niet alleen wordt gecontroleerd.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Continue integratie met Visual Studio Team Service

Naast de opdrachtregel kunnen klanten bouwen van Visual Studio of MSBuild-taak ook gebruiken voor het bouwen van U-SQL-projecten in Visual Studio Team Service. Als u opbouwtaak instelt, zorg ervoor dat:

1.  Toevoegen NuGet terugzetten taak om op te halen van de oplossing waarnaar wordt verwezen NuGet-pakket inclusief `Azure.DataLake.USQL.SDK`, zodat MSBuild de U-SQL-taal-doelen vindt. 

    ![Data Lake ingesteld CI CD MSBuild-taak voor U-SQL project](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

2.  Set MSBuild-argumenten, en u kunt de argumenten instellen in het bouwen van Visual Studio of MSBuild-taak, zoals hieronder, of u kunt variabelen voor deze argumenten definiëren in VSTS build-definitie.

    ```
    /p:USQLSDKPath=$(Build.SourcesDirectory)/<your project name>/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.1019-preview/build/runtime /p:USQLTargetType=SyntaxCheck /p:DataRoot=$(Build.SourcesDirectory)
    ```

    ![Data Lake CI CD MSBuild variabelen instellen voor U-SQL project](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables.png) 

### <a name="u-sql-project-build-output"></a>De uitvoer van de build project U-SQL

Na het uitvoeren van build, alle scripts in de U-SQL project zijn gebouwd en output naar een zipbestand met de naam `USQLProjectName.usqlpack`. De mapstructuur in uw project worden, bewaard in de uitvoer van de ingepakte build.

>[!NOTE]
>
>Codebestand voor elk U-SQL-script worden samengevoegd als de uitvoer van de build script inline-instructie.
>

## <a name="test-u-sql-script"></a>Testen van U-SQL-script

Azure Data Lake biedt en Testprojecten voor de U-SQL-script en C# UDO/UDAG/UDF:
* [Informatie over het toevoegen van testcases voor U-SQL-script en uitgebreide C#-code](data-lake-analytics-cicd-test.md#test-u-sql-scripts)
* [Meer informatie over het uitvoeren van deze testcases in Visual Studio Team Service](data-lake-analytics-cicd-test.md#run-test-cases-in-visual-studio-team-service)

## <a name="u-sql-job-deployment"></a>Implementatie van U-SQL-taak

Nadat u hebt gecontroleerd code via compilatie- en verwerken, kunt u U-SQL-taken rechtstreeks vanuit Visual Studio Team Service via indienen **Azure PowerShell-taak**. U kunt het script ook implementeren met Azure Data Lake Store/Azure Blob Storage en [de geplande taken worden uitgevoerd via Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

### <a name="submit-u-sql-jobs-through-visual-studio-team-service"></a>U-SQL-taken via Visual Studio Team Service verzenden

De build-uitvoer van het project U-SQL is een zip-bestand met de naam **USQLProjectName.usqlpack** bevat alle U-SQL-scripts in het project. U kunt de [Azure PowserShell taak in Visual Studio Team Service](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) met onderstaande PowserShell-voorbeeldscript om in te dienen de U-SQL taken rechtstreeks vanuit Visual Studio Team Service bouwen of release-pijplijn.

```powershell
param(
    [Parameter(Mandatory=$true)][string]$AnalyticsAccountName, #ADLA account name
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot, #Root folder (e.g. artifacts root folder)
    [Parameter(Mandatory=$false)][string]$DegreeOfParallelism = 1
)

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
        # [System.IO.Compression.ZipFile]::ExtractToDirectory($USQLPackfile, $UnzipOutput, 0)
        # $USQLPackfileZip = Rename-Item -Path $USQLPackfile -NewName $([System.IO.Path]::ChangeExtension($USQLPackfile, ".zip")) -Force -PassThru
        # Expand-Archive -Path $USQLPackfileZip -DestinationPath $UnzipOutput -Force
    }

    $USQLFiles = Get-ChildItem -Path $UnzipOutput -Include *.usql -File -Recurse -ErrorAction SilentlyContinue | Where-Object {$_.DirectoryName -match $subFolder}

    return $USQLFiles
}

Function SubmitAnalyticsJob()
{
    $usqlFiles = GetUsqlFiles

    Write-Output "$($usqlFiles.Count) jobs to be submitted..."
    # submit each usql script and wait for completion before moving ahead.
    foreach ($usqlFile in $usqlFiles)
    {
        $scriptName = "[Release].[$([System.IO.Path]::GetFileNameWithoutExtension($usqlFile.fullname))]"

        Write-Output "($usqlFiles.IndexOf())Submitting job for '{$usqlFile}'"

        $jobToSubmit = Submit-AzureRmDataLakeAnalyticsJob -Account $AnalyticsAccountName -Name $scriptName -ScriptPath $usqlFile -DegreeOfParallelism $DegreeOfParallelism
        LogJobInformation $jobToSubmit
        
        Write-Output "waiting for job to complete. Job ID:'{$($jobToSubmit.JobId)}', Name: '$($jobToSubmit.Name)' "
        $jobResult = Wait-AzureRmDataLakeAnalyticsJob -Account $AnalyticsAccountName -JobId $jobToSubmit.JobId  
        LogJobInformation $jobResult
        
        # ProcessResult $jobResult
    }
}

Function ProcessResult($jobResult)
{
    if ($jobResult.Result -eq "Failed")
    {
        Write-Error "Job Failed. Job Id: $($jobResult.JobId), Job Name: $($jobResult.Name), Log: $($jobResult.LogFolder)"
    }
    else
    {
        Write-Output "Job Succeeded. Job Id: $($jobResult.JobId), Job Name: $($jobResult.Name)"
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

    Write-Output "Starting USQL script deployment..."

    # Submit ADLA jobs with usql scripts in given sub-folder.
    # Order is important here. Scripts with least dependency goes first followed 
    # by scripts which more dependencies.
    
    SubmitAnalyticsJob

    Write-Output "Finished deployment..."
}

Main
```

### <a name="deploy-u-sql-jobs-through-azure-data-factory"></a>U-SQL-taken via Azure Data Factory implementeren

Het feit dat U-SQL-taken rechtstreeks vanuit Visual Studio Team Service kunt indienen, u ook de gemaakte scripts naar Azure Data Lake Store/Azure Blob Storage uploaden kunt en [de geplande taken worden uitgevoerd via Azure Data Factory](https://docs.microsoft.com/azure/data-factory/transform-data-using-data-lake-analytics).

Gebruik de [Azure PowerShell-taak in Visual Studio Team Service](https://docs.microsoft.com/vsts/pipelines/tasks/deploy/azure-powershell?view=vsts) met onderstaande PowerShell-voorbeeldscript voor het uploaden van de U-SQL-scripts naar Azure Data Lake Store-account.

```powershell
param(
    [Parameter(Mandatory=$true)][string]$ADLSName, #ADLA account name
    [Parameter(Mandatory=$true)][string]$ArtifactsRoot #Root folder (e.g. artifacts root folder)
)

Function UploadResources()
{
    Write-Host "************************************************************************"
    Write-Host "Uploading DLL files to $ADLSName"
    Write-Host "***********************************************************************"

    $usqlScripts = GetUsqlFiles
    Import-AzureRmDataLakeStoreItem -AccountName $ADLSName -Path $usqlScripts.FullName -Destination "/ScriptResource2/$usqlScripts" -Force -Recurse
    # $files = @(get-childitem $usqlScripts -recurse)
    # foreach($file in $files)
    # {
    #    Write-Host "Uploading file: $($file.Name)"
    #    Import-AzureRmDataLakeStoreItem -AccountName $ADLSName -Path $file.FullName -Destination "/ScriptResource/$file" -Force -Recurse
    # }
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

    return $UnzipOutput
}

UploadResources
```

## <a name="cicd-for-u-sql-database"></a>CI/CD voor U-SQL-database

Azure Data Lake Tools voor Visual Studio biedt U-SQL-database-projectsjabloon helpt ontwikkelaars ontwikkelen, beheren en implementeren van de U-SQL-databases, snel en eenvoudig. [Meer informatie over de U-SQL-database-project](data-lake-analytics-data-lake-tools-develop-usql-database.md).

## <a name="build-u-sql-database-project"></a>U-SQL database-project maken

### <a name="get-nuget-package"></a>NuGet-pakket downloaden

MSBuild biedt geen ingebouwde ondersteuning voor het projecttype voor U-SQL-database. Als u wilt toevoegen door deze mogelijkheid, moet u een verwijzing voor uw oplossing wilt toevoegen de [Microsoft.Azure.DataLake.USQL.SDK Nuget-pakket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/) waarmee de gewenste taal-service wordt toegevoegd.

De verwijzing van NuGet-pakket wilt toevoegen, kunt u met de rechtermuisknop op de oplossing in Solution Explorer en kies **NuGet-pakketten beheren** voor oplossing, zoek en installeer het NuGet-pakket. Of u kunt een bestand met de naam 'packages.config' in de map van de oplossing toevoegen en toevoegen onder inhoud naartoe.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
  <package id="Microsoft.Azure.DataLake.USQL.SDK" version="1.3.180615" targetFramework="net452" />
</packages>
```

### <a name="build-u-sql-database-project-with-msbuild-command-line"></a>Bouw U-SQL-database-project met MSBuild-opdrachtregel

U kunt de standaard MSBuild vanaf de opdrachtregel en pas de NuGet-pakket van U-SQL-SDK-verwijzing als extra argument zoals hieronder om uw U-SQL-database-project te bouwen aanroepen:

```
msbuild DatabaseProject.usqldbproj /p:USQLSDKPath=packages\Microsoft.Azure.DataLake.USQL.SDK.1.3.180615\build\runtime
```

De argumenten `USQLSDKPath=<U-SQL Nuget package>\build\runtime` verwijst naar het installatiepad van het NuGet-pakket voor de U-SQL-taalservice.

### <a name="continuous-integration-with-visual-studio-team-service"></a>Continue integratie met Visual Studio Team Service

Naast de opdrachtregel, klanten kunnen ook gebruiken **Visual Studio Build** of **MSBuild-taak** U-SQL database-projecten in Visual Studio Team Service bouwen. Als u opbouwtaak instelt, zorg ervoor dat:

1.  Toevoegen NuGet terugzetten taak om op te halen van de oplossing waarnaar wordt verwezen NuGet-pakket inclusief `Azure.DataLake.USQL.SDK`, zodat MSBuild de U-SQL-taal-doelen vindt. 

    ![Data Lake Set CI CD MSBuild-taak voor U-SQL database-project](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-task.png) 

2.  Set MSBuild-argumenten, en u kunt de argumenten instellen in het bouwen van Visual Studio of MSBuild-taak, zoals hieronder, of u kunt variabelen voor deze argumenten definiëren in VSTS build-definitie.

```
/p:USQLSDKPath=$(Build.SourcesDirectory)/<your project name>/packages/Microsoft.Azure.DataLake.USQL.SDK.1.3.1019-preview/build/runtime
```

![Data Lake instellen CI CD MSBuild-variabelen voor de U-SQL-databaseproject](./media/data-lake-analytics-cicd-overview/data-lake-analytics-set-vsts-msbuild-variables-database-project.png) 

### <a name="u-sql-database-project-build-output"></a>De uitvoer van U-SQL-database project build

De uitvoer voor de U-SQL database-project is de implementatiepakket voor een U-SQL-database, met de naam met het achtervoegsel build `.usqldbpack`. De `.usqldbpack` pakket is een zip-bestand bevat alle DDL-componenten in een enkel U-SQL-script in de DDL-map en alle dll's en aanvullende bestanden voor assembly's in de map Temp.

## <a name="test-table-valued-function-and-stored-procedure"></a>Tabelwaardefunctie en opgeslagen procedure testen

Op dit moment wordt rechtstreeks toe te voegen testcases voor tabelfuncties en opgeslagen procedures niet ondersteund. U kunt als tijdelijke oplossing, maken van een U-SQL-project met U-SQL-scripts aanroepen van deze functies en Testscenario's schrijven voor hen. Volg de onderstaande stappen voor het instellen van testcases voor tabelfuncties en opgeslagen procedures die zijn gedefinieerd in het project U-SQL-database:

1.  Maken van een U-SQL-project voor test-doel en U-SQL-scripts aanroepen van de tabelfuncties en opgeslagen procedures te schrijven.
2.  Databaseverwijzing toevoegen aan dit project U-SQL. Om de functie met tabelwaarden en de definitie van de opgeslagen procedure, moet u verwijzen naar de databaseproject met daarin de DDL-instructie. [Meer informatie over de verwijzing naar database](data-lake-analytics-data-lake-tools-develop-usql-database.md#reference-a-u-sql-database-project).
3.  Voeg testcases voor de U-SQL-scripts waarmee tabelfuncties en opgeslagen procedures worden aangeroepen. [Informatie over het toevoegen van testcases voor U-SQL-script](data-lake-analytics-cicd-test.md#test-u-sql-scripts).

## <a name="deploy-u-sql-database-through-visual-studio-team-service"></a>U-SQL-database via Visual Studio Team Service implementeren

`PackageDeploymentTool.exe` biedt de programmering en opdrachtregelinterfaces die helpen bij het implementeren van implementatie package(.usqldbpack) van U-SQL-database. De SDK is opgenomen in [U-SQL SDK NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.DataLake.USQL.SDK/), zoeken naar op build/runtime/PackageDeploymentTool.exe. Met behulp van `PackageDeploymentTool.exe`, kunt u U-SQL databases implementeren met zowel Azure Data Lake Analytics en lokale account.

>[!NOTE]
>
>PowerShell-opdracht regel ondersteuning en Visual Studio Team Service release taak ondersteuning voor de implementatie van de U-SQL-database komt eraan.
>

Volg de onderstaande stappen voor het instellen van de Implementatietaak database in Visual Studio Team Service:

1. Een PowerShell-Script-taak toevoegen in build of release-pijplijn en Voer onderstaande PowerShell-script. Deze taak helpt bij het ophalen van de Azure SDK-afhankelijkheden voor `PackageDeploymentTool.exe`. U kunt de parameter - outputfolder laden van deze afhankelijkheden op een specifieke map instellen. Geeft het pad naar deze map `PackageDeploymentTool.exe` in stap 2. 

    ```powershell
    param (
        [string]$outputfolder = "RequiredDll",
        [string]$workingfolder = ""
    )

    if ([string]::IsNullOrEmpty($workingfolder))
    {
        $scriptpath = $MyInvocation.MyCommand.Path
        $workingfolder = Split-Path $scriptpath
    }
    cd $workingfolder

    echo "workingfolder=$workingfolder, outputfolder=$outputfolder"
    echo "Downloading required packages..."

    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Analytics/3.2.3-preview -outf Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Azure.Management.DataLake.Store/2.3.3-preview -outf Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.3 -outf Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime/2.3.11 -outf Microsoft.Rest.ClientRuntime.2.3.11.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure/3.3.7 -outf Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip
    iwr https://www.nuget.org/api/v2/package/Microsoft.Rest.ClientRuntime.Azure.Authentication/2.3.3 -outf Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip

    echo "Extracting packages..."

    Expand-Archive Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview -Force
    Expand-Archive Microsoft.Azure.Management.DataLake.Store.2.3.3-preview.zip -DestinationPath Microsoft.Azure.Management.DataLake.Store.2.3.3-preview -Force
    Expand-Archive Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3.zip -DestinationPath Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.2.3.11.zip -DestinationPath Microsoft.Rest.ClientRuntime.2.3.11 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.3.3.7.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.3.3.7 -Force
    Expand-Archive Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3.zip -DestinationPath Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3 -Force

    echo "Copy required DLLs to output folder..."

    mkdir $outputfolder -Force
    copy Microsoft.Azure.Management.DataLake.Analytics.3.2.3-preview\lib\net452\*.dll $outputfolder
    copy Microsoft.Azure.Management.DataLake.Store.2.3.3-preview\lib\net452\*.dll $outputfolder
    copy Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\*.dll $outputfolder
    copy Microsoft.Rest.ClientRuntime.2.3.11\lib\net452\*.dll $outputfolder
    copy Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\*.dll $outputfolder
    copy Microsoft.Rest.ClientRuntime.Azure.Authentication.2.3.3\lib\net452\*.dll $outputfolder
    ```

2. Voeg een **opdrachtregeltaak** in bouwen of release-pijplijn en vul in de script-aanroep `PackageDeploymentTool.exe`. Het voorbeeldscript is als volgt: 

    * U-SQL-database lokaal implementeren

        ```
        PackageDeploymentTool.exe deploylocal -Package <package path> -Database <database name> -DataRoot <data root path>
        ```

    * Gebruik interactieve verificatiemodus voor het implementeren van U-SQL-database naar Azure Data Lake Analytics-Account:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -AzureSDKPath <azure sdk path> -Interactive
        ```

    * Gebruik secrete verificatie voor het implementeren van U-SQL-database naar Azure Data Lake Analytics-Account:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete>
        ```

    * CertBestand verificatie gebruiken voor het implementeren van U-SQL-database naar Azure Data Lake Analytics-Account:

        ```
        PackageDeploymentTool.exe deploycluster -Package <package path> -Database <database name> -Account <account name> -ResourceGroup <resource group name> -SubscriptionId <subscript id> -Tenant <tanant name> -ClientId <client id> -Secrete <secrete> -CertFile <certFile>
        ```

**Beschrijving van de parameter PackageDeploymentTool.exe:**

**Algemene Parameters:**

|Parameter|Beschrijving|Standaardwaarde|Vereist|
|---------|-----------|-------------|--------|
|Pakket|Pad van het implementatiepakket voor U-SQL-database om te worden geïmplementeerd|Null|true|
|Database|Databasenaam worden geïmplementeerd voor / of gemaakt|model|false|
|Logboekbestand|Pad van bestand voor logboekregistratie van standaard naar standard uit (console)|Null|false|
|LogLevel|Meld u niveau: uitgebreide, normaal, waarschuwing, fout|LogLevel.Normal|false|

**{De parameter voor lokale implementatie:**

|Parameter|Beschrijving|Standaardwaarde|Vereist|
|---------|-----------|-------------|--------|
|DataRoot|Pad van de hoofdmap van de lokale gegevens|Null|true|

**De parameter voor de implementatie van Azure Data Lake Analytics:**

|Parameter|Beschrijving|Standaardwaarde|Vereist|
|---------|-----------|-------------|--------|
|Account|Hiermee geeft u op welke Azure Data Lake Analytics-account door de naam van implementeren|Null|true|
|ResourceGroup|Naam van Azure-resource voor de Azure Data Lake Analytics-account|Null|true|
|SubscriptionId|Azure-abonnement-ID voor de Azure Data Lake Analytics-account|Null|true|
|Tenant|De naam van tenant (AAD directory-domeinnaam, u vindt deze in de pagina abonnement beheren in Azure portal)|Null|true|
|AzureSDKPath|Pad naar het afhankelijke assembly's zoeken in de Azure SDK|Null|true|
|Interactief|Interactieve modus gebruiken voor verificatie of niet|false|false|
|ClientId|AAD-toepassing-ID voor geen interactieve verificatie vereist is voor geen interactieve verificatie|Null|vereist voor geen interactieve verificatie|
|Secrete|Secrete en wachtwoord voor geen interactieve verificatie, moet deze alleen gebruiken in vertrouwde/beveiligde omgeving|Null|vereist voor geen interactieve verificatie, of gebruik SecreteFile|
|SecreteFile|Bestand secrete/wachtwoord wordt opgeslagen voor geen in interactieve verificatie, moet u ervoor dat deze alleen kan worden gelezen door de huidige gebruiker|Null|vereist voor geen interactieve verificatie, of gebruik Secrete|
|CertBestand|Bestand wordt opgeslagen x.509-certificaat voor geen interactieve verificatie, standaard is het gebruik van de client secrete verificatie|Null|false|
|JobPrefix|Voorvoegsel voor de implementatie van de database dat U-SQL-DDL-taak|Deploy_ + DateTime.Now|false|

## <a name="next-steps"></a>Volgende stappen

- [Het testen van uw Azure Data Lake Analytics-code](data-lake-analytics-cicd-test.md)
- [U-SQL-script uitvoeren op uw lokale computer](data-lake-analytics-data-lake-tools-local-run.md)
- [Gebruik U-SQL database-project voor het ontwikkelen van U-SQL-database](data-lake-analytics-data-lake-tools-develop-usql-database.md)