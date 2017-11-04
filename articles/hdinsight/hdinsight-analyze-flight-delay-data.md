---
title: Vlucht vertraging gegevens analyseren met Hadoop in HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van een Windows PowerShell-script voor het maken van een HDInsight-cluster, een Hive-taak uitvoert, voer een Sqoop en verwijdert het cluster.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00e26aa9-82fb-4dbe-b87d-ffe8e39a5412
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 5da745901ec2fe57530e4d7fe38a055e0b8691ac
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Vertraging vluchtgegevens analyseren met behulp van Hive in HDInsight
Hive biedt een methode voor Hadoop MapReduce-taken uitgevoerd via een SQL-achtige scripttaal genaamd  *[HiveQL][hadoop-hiveql]*, die naar samenvatten, opvragen en analyseren van grote hoeveelheden gegevens kunnen worden toegepast.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight op basis van Windows-cluster. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie. Zie voor stappen die met een cluster op basis van Linux werken [vertraging vluchtgegevens analyseren met behulp van Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md).

Een van de belangrijkste voordelen van Azure HDInsight is het scheiden van de opslag van gegevens en berekeningen. HDInsight gebruikt Azure Blob-opslag voor gegevensopslag. Een typische taak bestaat uit drie delen:

1. **Gegevens opslaan in Azure Blob-opslag.**  Bijvoorbeeld, weerbericht gegevens, sensorgegevens, weblogboeken en in dit geval vertraging vluchtgegevens worden opgeslagen in Azure Blob-opslag.
2. **Taken worden uitgevoerd.** Wanneer is het tijd om de gegevens verwerken, loopt u een Windows PowerShell-script (of een clienttoepassing) voor het maken van een HDInsight-cluster, taken uitvoeren en het cluster verwijdert. De taken opslaan uitvoergegevens naar Azure Blob-opslag. De uitvoergegevens wordt bewaard, zelfs nadat het cluster wordt verwijderd. Op deze manier u betaalt alleen wat u hebt gebruikt.
3. **De uitvoer ophalen uit Azure Blob storage**, of in deze zelfstudie kunt u de gegevens exporteren naar een Azure SQL database.

Het volgende diagram illustreert het scenario en de structuur van deze zelfstudie:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

Houd er rekening mee dat de getallen in het diagram met de Sectietitels overeenkomen. **M** staat voor het hoofdproces. **Een** staat voor de inhoud in de bijlage.

Het belangrijkste deel van de zelfstudie laat zien hoe u een Windows PowerShell-script gebruiken de volgende taken uitvoeren:

* Maak een HDInsight-cluster.
* Een Hive-taak op het cluster om te berekenen gemiddelde vertragingen op luchthavens uitvoeren. De vertraging vluchtgegevens wordt opgeslagen in een Azure Blob storage-account.
* Voer een Sqoop om de uitvoer van de Hive-taak exporteren naar een Azure SQL database.
* Het HDInsight-cluster verwijderen.

In de bijlagen vindt u de instructies voor de vertraging vluchtgegevens uploaden, maken/uploaden van een Hive-query-tekenreeks en de Azure SQL database voorbereiden voor de taak Sqoop.

> [!NOTE]
> De stappen in dit document zijn specifiek voor HDInsight op basis van Windows-clusters. Zie voor stappen die met een cluster op basis van Linux werken [gegevens te analyseren vlucht vertraging met Hive in HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

### <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u beschikken over de volgende items:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een werkstation met Azure PowerShell**.

    > [!IMPORTANT]
    > Azure PowerShell-ondersteuning voor het beheer van HDInsight-resources met behulp van Azure Service Manager is **afgeschaft** en per 1 januari 2017 verdwenen. In de stappen in dit document worden de nieuwe HDInsight-cmdlets gebruikt die met Azure Resource Manager werken.
    >
    > Volg de stappen in [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) als u de nieuwste versie van Azure PowerShell wilt installeren. Als u scripts hebt die moeten worden gewijzigd om ze te kunnen gebruiken met de nieuwe cmdlets die werken met Azure Resource Manager, raadpleegt u [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Migreren naar op Azure Resource Manager gebaseerde hulpprogramma’s voor HDInsight-clusters) voor meer informatie.

**Bestanden die in deze zelfstudie worden gebruikt**

Deze zelfstudie wordt gebruikgemaakt van de prestaties op het moment van luchtvaartmaatschappij vluchtgegevens van [onderzoek en innovatieve technologie-beheer, Bureau vervoer statistieken of RITA][rita-website].
Een kopie van de gegevens is geüpload naar een Azure Blob storage-container met de openbare Blob-machtiging.
Een gedeelte van het PowerShell-script kopieert de gegevens uit de openbare blob-container naar de standaard blob-container van het cluster. Het HiveQL-script wordt ook gekopieerd naar de blobcontainer met dezelfde.
Als u weten hoe de gegevens naar uw eigen opslagaccount get of uploadt en hoe het scriptbestand HiveQL maken of uploadt wilt, raadpleeg dan [bijlage A](#appendix-a) en [bijlage B](#appendix-b).

De volgende tabel bevat de bestanden in deze zelfstudie gebruikt:

<table border="1">
<tr><th>Bestanden</th><th>Beschrijving</th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>Het script HiveQL bestand gebruikt door het Hive-taak. Dit script is geüpload naar een Azure Blob storage-account met de openbare toegang. <a href="#appendix-b">Bijlage B</a> instructies over het voorbereiden en dit bestand te uploaden naar uw eigen Azure Blob storage-account heeft.</td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>De invoergegevens van de Hive-taak. De gegevens is geüpload naar een Azure Blob storage-account met de openbare toegang. <a href="#appendix-a">Bijlage A</a> instructies heeft op de gegevens zijn opgehaald en de gegevens uploaden naar uw eigen Azure Blob storage-account.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>Het uitvoerpad voor de Hive-taak. De standaardcontainer wordt gebruikt voor het opslaan van de uitvoergegevens.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>Status map van de Hive-taak op de standaardcontainer.</td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a>Cluster maken en Hive/Sqoop taken uitvoeren
Hadoop-MapReduce is batchverwerking. De meest rendabele manier om uit te voeren van een Hive-taak is een cluster maken voor de taak en de taak te verwijderen nadat de taak is voltooid. Het volgende script bevat informatie over het hele proces.
Zie voor meer informatie over het maken van een HDInsight-cluster en het uitvoeren van Hive-taken [maken Hadoop-clusters in HDInsight] [ hdinsight-provision] en [Hive gebruiken met HDInsight][hdinsight-use-hive].

**Voor het uitvoeren van Hive-query's met Azure PowerShell**

1. Een Azure SQL database en de tabel voor de uitvoer van de taak Sqoop maken met behulp van de instructies in [bijlage C](#appendix-c).
2. Open Windows PowerShell ISE en voer het volgende script:

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure the follwing variables
    # for an existing Azure SQL Database
    ###########################################
    $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
    $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
    $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
    $existingSqlDatabaseName = "<Azure SQL Database name>"

    $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files.
    $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on the version, the folder can be different

    ###########################################
    # (Optional) configure the following variables
    ###########################################

    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2"

    $HDInsightClusterName = $namePrefix + "hdi"
    $httpUserName = "admin"
    $httpPassword = "<Enter the Password>"

    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $HDInsightClusterName # use the cluster name

    $existingSqlDatabaseTableName = "AvgDelays"
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"

    $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql"

    $jobStatusFolder = "/tutorials/flightdelays/jobstatus"

    ###########################################
    # Login
    ###########################################
    try{
        $acct = Get-AzureRmSubscription
    }
    catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionID $subscriptionID

    ###########################################
    # Create a new HDInsight cluster
    ###########################################

    # Create ARM group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create the default storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS

    # Create the default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext

    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $existingDefaultBlobContainerName

    ###########################################
    # Prepare the HiveQL script and source data
    ###########################################

    # Create the temp location
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
    #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload data to default container

    $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"

    Invoke-Expression -Command:$azcopycmd

    ###########################################
    # Submit the Hive job
    ###########################################
    Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
    $response = Invoke-AzureRmHDInsightHiveJob `
                    -Files $hqlScriptFile `
                    -DefaultContainer $defaultBlobContainerName `
                    -DefaultStorageAccountName $defaultStorageAccountName `
                    -DefaultStorageAccountKey $defaultStorageAccountKey `
                    -StatusFolder $jobStatusFolder

    write-Host $response

    ###########################################
    # Submit the Sqoop job
    ###########################################
    $exportDir = "wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                    -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ResourceGroupName $resourceGroupName `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -HttpCredential $httpCredential `
        -WaitTimeoutInSeconds 3600 `
        -Job $sqoopJob.JobId

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -DefaultContainer $existingDefaultBlobContainerName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    ###########################################
    # Delete the cluster
    ###########################################
    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName
    ```
3. Verbinding maken met uw SQL-database en gemiddelde vertragingen per plaats in de tabel AvgDelays Zie:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <a id="appendix-a"></a>Bijlage A - vluchtgegevens met vertraging van uploaden naar Azure Blob-opslag
Uploaden van het gegevensbestand en de scriptbestanden HiveQL (Zie [bijlage B](#appendix-b)) vereist een planning. Het idee is voor het opslaan van de gegevensbestanden en het bestand HiveQL vóór het maken van een HDInsight-cluster en de Hive-taak uitgevoerd. U hebt hiervoor twee opties:

* **Gebruik de Azure Storage-account die wordt gebruikt door het HDInsight-cluster als het standaardbestandssysteem.** Omdat het HDInsight-cluster de toegangssleutel voor Opslagaccount, moet u geen extra wijzigingen aanbrengt.
* **Gebruik een ander Azure Storage-account van het standaardbestandssysteem van HDInsight-cluster.** Als dit het geval is, moet u de maken deel uit van de Windows PowerShell script gevonden in wijzigen [maken HDInsight-cluster en voer Hive/Sqoop taken](#runjob) de Storage-account als een aanvullende Storage-account te koppelen. Zie voor instructies [maken Hadoop-clusters in HDInsight][hdinsight-provision]. Het HDInsight-cluster kent de toegangssleutel voor het opslagaccount.

> [!NOTE]
> Het pad van de Blob-opslag voor het gegevensbestand is moeilijk gecodeerd in het bestand HiveQL-script. U moet deze dienovereenkomstig bijwerken.

**De vluchtgegevens downloaden**

1. Blader naar [onderzoek en beheer van innovatieve technologie Bureau vervoer statistieken][rita-website].
2. Selecteer op de pagina de volgende waarden:

    <table border="1">
    <tr><th>Naam</th><th>Waarde</th></tr>
    <tr><td>Filteren van jaar</td><td>2013 </td></tr>
    <tr><td>Periode filteren</td><td>Januari</td></tr>
    <tr><td>Velden</td><td>*Jaar*, *FlightDate*, *UniqueCarrier*, *Carrier*, *FlightNum*, *OriginAirportID*, *oorsprong*, *OriginCityName*, *OriginState*, *DestAirportID*, *Dest*, *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*, *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*,  *NASDelay*, *SecurityDelay*, *LateAircraftDelay* (alle andere velden wissen)</td></tr>
    </table>
3.Klik op **downloaden**.
4. Pak het bestand naar de **C:\Tutorials\FlightDelay\2013Data** map. Elk bestand is van een CSV-bestand en ongeveer 60GB groot is.
5. Naam van het bestand op de naam van de maand waarin het gegevens voor bevat. Bijvoorbeeld: het bestand met de gegevens januari naam *January.csv*.
6. Herhaal stappen 2 en 5 een bestand te downloaden voor elk van de 12 maanden in 2013. U moet minimaal één bestand worden uitgevoerd van de zelfstudie.

**De vertraging vluchtgegevens uploaden naar Azure Blob-opslag**

1. Bereid de parameters:

    <table border="1">
    <tr><th>Naam variabele</th><th>Opmerkingen</th></tr>
    <tr><td>$storageAccountName</td><td>Het Azure Storage-account waar u de gegevens te uploaden.</td></tr>
    <tr><td>$blobContainerName</td><td>De Blob-container waar u de gegevens te uploaden.</td></tr>
    </table>
2. Open Azure PowerShell ISE.
3. Plak het volgende script in het scriptvenster:

    ```powershell
    [CmdletBinding()]
    Param(

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #Region - Variables
    $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # The source folder
    $destFolder = "tutorials/flightdelay/2013data"     #The blob name prefix for the files to be uploaded
    #EndRegion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
    # Validate the Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate the container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #Region - Copy the file from local workstation to Azure Blob storage
    if (test-path -Path $localFolder)
    {
        foreach ($item in Get-ChildItem -Path $localFolder){
            $fileName = "$localFolder\$item"
            $blobName = "$destFolder/$item"

            Write-Host "Copying $fileName to $blobName" -ForegroundColor Green

            Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
        }
    }
    else
    {
        Write-Host "The source folder on the workstation doesn't exist" -ForegroundColor Red
    }

    # List the uploaded files on HDInsight
    Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
    #EndRegion
    ```
4. Druk op **F5** om het script uit te voeren.

Als u een andere methode gebruiken kiest voor het uploaden van bestanden, Controleer of dat het bestandspad is flightdelay-zelfstudies-gegevens. De syntaxis voor toegang tot de bestanden is:

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

Het pad zelfstudies/flightdelay/gegevens zijn de virtuele map die u hebt gemaakt toen u de bestanden geüpload. Controleer of er 12 bestanden, één voor elke maand.

> [!NOTE]
> U kunt de Hive-query om te lezen van de nieuwe locatie moet bijwerken.
>
> U moet ofwel de toegangsrechten voor de container openbaar of het opslagaccount binden aan het HDInsight-cluster configureren. Anders is de Hive-query-tekenreeks niet mogelijk toegang tot de gegevensbestanden.

- - -

## <a id="appendix-b"></a>Bijlage B - maken en uploaden van een HiveQL-script
Met Azure PowerShell, kunt u meerdere HiveQL-instructies een tegelijk uitvoeren of de instructie van HiveQL in een scriptbestand van het pakket. Deze sectie leest u hoe een HiveQL-script maken en uploaden van het script naar Azure Blob-opslag met behulp van Azure PowerShell. Hive vereist de HiveQL-scripts moeten worden opgeslagen in Azure Blob-opslag.

Het HiveQL-script wordt het volgende doen:

1. **Verwijderen van de tabel delays_raw**, als de tabel al bestaat.
2. **De delays_raw externe Hive-tabel maken** die verwijst naar de locatie van Blob-opslag met de zwarte vertraging-bestanden. Deze query geeft aan dat de velden worden gescheiden door ',' en dat de regels worden beëindigd door '\n'. Dit is een probleem wanneer veldwaarden komma's bevatten omdat Hive kan geen onderscheid maken tussen een door komma's die een veldscheidingsteken en een die deel uitmaakt van een veldwaarde (dit is het geval in veldwaarden voor de oorsprong\_STAD\_naam en doel\_STAD\_naam). Om dit op te lossen, maakt de query TEMP kolommen bevatten gegevens die onjuist is onderverdeeld in kolommen.
3. **Verwijderen van de tabel vertragingen**, als de tabel al bestaat.
4. **Maken van de tabel vertragingen**. Het is nuttig voor het opschonen van de gegevens voor verdere verwerking. Deze query maakt een nieuwe tabel *vertragingen*, uit de tabel delays_raw. Opmerking de TEMP-kolommen (zoals eerder vermeld) worden niet gekopieerd, en dat de **subtekenreeks** functie aanhalingstekens verwijderen uit de gegevens wordt gebruikt.
5. **De gemiddelde weer vertraging en groepen de resultaten plaatsnaam berekenen.** Dit wordt ook de resultaten naar Blob storage uitvoeren. De query, enkele aanhalingstekens wordt verwijderd uit de gegevens en uitsluit rijen waar de waarde voor **weather_delay** is null. Dit is nodig omdat Sqoop, die verderop in deze zelfstudie wordt gebruikt, kunnen niet worden verwerkt die waarden probleemloos standaard.

Zie voor een volledige lijst van de opdrachten HiveQL [Hive Data Definition Language][hadoop-hiveql]. Elke opdracht HiveQL moet eindigen met een puntkomma.

**Maken van een scriptbestand HiveQL**

1. Bereid de parameters:

    <table border="1">
    <tr><th>Naam variabele</th><th>Opmerkingen</th></tr>
    <tr><td>$storageAccountName</td><td>Het Azure Storage-account waar u het HiveQL-script om te uploaden.</td></tr>
    <tr><td>$blobContainerName</td><td>De Blob-container waar u het HiveQL-script om te uploaden.</td></tr>
    </table>
2. Open Azure PowerShell ISE.
3. Kopieer en plak het volgende script in het scriptvenster:

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Blob storage variables
        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure storage account name for creating a new HDInsight cluster. If the account doesn't exist, the script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter the Azure blob container name for creating a new HDInsight cluster. If not specified, the HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #region - Define variables
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    # The HiveQL script file is exported as this file before it's uploaded to Blob storage
    $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"

    # The HiveQL script file will be uploaded to Blob storage as this blob name
    $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"

    # These two constants are used by the HiveQL script file
    #$srcDataFolder = "tutorials/flightdelay/data"
    $dstDataFolder = "/tutorials/flightdelay/output"
    #endregion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating the Azure Storage account and the Blob container..." -ForegroundColor Green
    # Validate the Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "The storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate the container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "The Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #region - Validate the file and file path

    # Check if a file with the same file name already exists on the workstation
    Write-Host "`nvalidating the folder structure on the workstation for saving the HQL script file ..."  -ForegroundColor Green
    if (test-path $hqlLocalFileName){

        $isDelete = Read-Host 'The file, ' $hqlLocalFileName ', exists.  Do you want to overwirte it? (Y/N)'

        if ($isDelete.ToLower() -ne "y")
        {
            Exit
        }
    }

    # Create the folder if it doesn't exist
    $folder = split-path $hqlLocalFileName
    if (-not (test-path $folder))
    {
        Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green

        new-item $folder -ItemType directory
    }
    #end region

    #region - Write the Hive script into a local file
    Write-Host "`nWriting the Hive script into a file on your workstation ..." `
                -ForegroundColor Green

    $hqlDropDelaysRaw = "DROP TABLE delays_raw;"

    $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
            "YEAR string, " +
            "FL_DATE string, " +
            "UNIQUE_CARRIER string, " +
            "CARRIER string, " +
            "FL_NUM string, " +
            "ORIGIN_AIRPORT_ID string, " +
            "ORIGIN string, " +
            "ORIGIN_CITY_NAME string, " +
            "ORIGIN_CITY_NAME_TEMP string, " +
            "ORIGIN_STATE_ABR string, " +
            "DEST_AIRPORT_ID string, " +
            "DEST string, " +
            "DEST_CITY_NAME string, " +
            "DEST_CITY_NAME_TEMP string, " +
            "DEST_STATE_ABR string, " +
            "DEP_DELAY_NEW float, " +
            "ARR_DELAY_NEW float, " +
            "CARRIER_DELAY float, " +
            "WEATHER_DELAY float, " +
            "NAS_DELAY float, " +
            "SECURITY_DELAY float, " +
            "LATE_AIRCRAFT_DELAY float) " +
        "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
        "LINES TERMINATED BY '\n' " +
        "STORED AS TEXTFILE " +
        "LOCATION 'wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"

    $hqlDropDelays = "DROP TABLE delays;"

    $hqlCreateDelays = "CREATE TABLE delays AS " +
        "SELECT YEAR AS year, " +
            "FL_DATE AS flight_date, " +
            "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
            "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
            "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
            "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
            "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
            "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
            "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
            "DEST_AIRPORT_ID AS dest_airport_id, " +
            "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
            "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
            "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
            "DEP_DELAY_NEW AS dep_delay_new, " +
            "ARR_DELAY_NEW AS arr_delay_new, " +
            "CARRIER_DELAY AS carrier_delay, " +
            "WEATHER_DELAY AS weather_delay, " +
            "NAS_DELAY AS nas_delay, " +
            "SECURITY_DELAY AS security_delay, " +
            "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
        "FROM delays_raw;"

    $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
        "SELECT regexp_replace(origin_city_name, '''', ''), " +
            "avg(weather_delay) " +
        "FROM delays " +
        "WHERE weather_delay IS NOT NULL " +
        "GROUP BY origin_city_name;"

    $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal

    $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
    #endregion

    #region - Upload the Hive script to the default Blob container
    Write-Host "`nUploading the Hive script to the default Blob container ..." -ForegroundColor Green

    # Create a storage context object
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Upload the file from local workstation to Blob storage
    Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
    #endregion

    Write-host "`nEnd of the PowerShell script" -ForegroundColor Green
    ```

    Hier volgen de variabelen die worden gebruikt in het script:

   * **$hqlLocalFileName** -het scriptbestand HiveQL het script lokaal opgeslagen voordat u dit uploadt naar blobopslag. Dit is de bestandsnaam. De standaardwaarde is <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
   * **$hqlBlobName** -dit is de HiveQL-script bestand blob-naam gebruikt in de Azure-blobopslag. De standaardwaarde is tutorials/flightdelay/flightdelays.hql. Omdat het bestand rechtstreeks naar Azure Blob-opslag geschreven, is er een '/' aan het begin van de blob-naam. Als u toegang tot het bestand van Blob-opslag wilt, moet u een "/" aan het begin van het bestand toevoegen.
   * **$srcDataFolder** en **$dstDataFolder** -= 'flightdelay-zelfstudies/gegevens' = 'flightdelay-zelfstudies/uitvoer'

- - -
## <a id="appendix-c"></a>Bijlage C - een Azure SQL database voorbereiden voor de uitvoer van de taak Sqoop
**Voorbereiden van de SQL-database (samenvoegen dit met het script Sqoop)**

1. Bereid de parameters:

    <table border="1">
    <tr><th>Naam variabele</th><th>Opmerkingen</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>De naam van de Azure SQL database-server. Voer niets voor het maken van een nieuwe server.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>De aanmeldingsnaam voor de Azure SQL database-server. Als een bestaande server $sqlDatabaseServerName is worden de aanmeldingsnaam en het wachtwoord bij de aanmelding gebruikt voor verificatie met de server. Anders worden ze gebruikt voor het maken van een nieuwe server.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>Het aanmeldingswachtwoord voor de Azure SQL database-server.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Deze waarde wordt alleen gebruikt als u een nieuwe Azure-database-server maakt.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>De SQL-database die wordt gebruikt voor het maken van de tabel AvgDelays voor de taak Sqoop. Leeg laat, wordt een database met de naam HDISqoop maken. De naam van de tabel voor de uitvoer van de taak Sqoop is AvgDelays. </td></tr>
    </table>
2. Open Azure PowerShell ISE.
3. Kopieer en plak het volgende script in het scriptvenster:

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Resource group variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure resource group name. It will be created if it doesn't exist.")]
        [String]$resourceGroupName,

        # SQL database server variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database Server Name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseServer,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database admin user.")]
        [String]$sqlDatabaseLogin,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the Azure SQL Database admin user password.")]
        [String]$sqlDatabasePassword,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter the region to create the Database in.")]
        [String]$sqlDatabaseLocation,   #For example, West US.

        # SQL database variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter the database name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseName # specify the database name if you have one created. Otherwise use "" to have the script create one for you.
    )

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Constants and variables

    # IP address REST service used for retrieving external IP address and creating firewall rules
    [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
    [String]$fireWallRuleName = "FlightDelay"

    # SQL database variables
    [String]$sqlDatabaseMaxSizeGB = 10

    #SQL query string for creating AvgDelays table
    [String]$sqlDatabaseTableName = "AvgDelays"
    [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [origin_city_name] [nvarchar](50) NOT NULL,
                [weather_delay] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
                [origin_city_name] ASC
            )
            )"
    #endregion

    #Region - Connect to Azure subscription
    Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #region - Create and validate Azure resouce group
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
    }

    #EndRegion

    #region - Create and validate Azure SQL database server
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress

        #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. Note that this allows Azure traffic from any Azure subscription to access the server.
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database

    try {
        Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region -  Execute an SQL command to create the AvgDelays table

    Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = $sqlCreateAvgDelaysTable
    $cmd.executenonquery()

    $conn.close()

    Write-host "`nEnd of the PowerShell script" -ForegroundColor Green
    ```

   > [!NOTE]
   > Het script maakt gebruik van een representational state transfer (REST)-service, http://bot.whatismyipaddress.com, voor het ophalen van het externe IP-adres. Het IP-adres wordt gebruikt voor het maken van een firewallregel voor uw SQL database-server.

    Hier volgen enkele variabelen die worden gebruikt in het script:

   * **$ipAddressRestService** -de standaardwaarde is http://bot.whatismyipaddress.com. Het is een openbaar IP-adres REST-service voor het ophalen van het externe IP-adres. U kunt andere services als u wilt gebruiken. Het externe IP-adres opgehaald via de service wordt gebruikt om een firewallregel voor uw Azure SQL database-server te maken zodat u toegang hebt tot de database vanuit uw werkstation (met behulp van Windows PowerShell-script).
   * **$fireWallRuleName** -dit is de naam van de firewallregel voor de Azure SQL database-server. De standaardnaam is <u>FlightDelay</u>. U kunt deze desgewenst wijzigen.
   * **$sqlDatabaseMaxSizeGB** -deze waarde wordt alleen gebruikt wanneer u een nieuwe Azure SQL database-server maakt. De standaardwaarde is 10GB. 10GB is voldoende voor deze zelfstudie.
   * **$sqlDatabaseName** -deze waarde wordt alleen gebruikt wanneer u een nieuwe Azure SQL database maakt. De standaardwaarde is HDISqoop. Als u de naam wijzigt, moet u het Sqoop Windows PowerShell-script dienovereenkomstig bijwerken.
4. Druk op **F5** om het script uit te voeren.
5. De uitvoer van het script valideren. Zorg ervoor dat het script is uitgevoerd.

## <a id="nextsteps"></a> Volgende stappen
Nu begrijpen u hoe u een bestand te uploaden naar Azure Blob-opslag, hoe u een Hive-tabel vullen met behulp van de gegevens uit Azure Blob-opslag, het uitvoeren van Hive-query's en Sqoop gebruiken gegevens uit HDFS exporteren naar een Azure SQL database. Zie voor meer informatie de volgende artikelen:

* [Aan de slag met HDInsight][hdinsight-get-started]
* [Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Oozie gebruiken met HDInsight][hdinsight-use-oozie]
* [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop]
* [Pig gebruiken met HDInsight][hdinsight-use-pig]
* [Het ontwikkelen van Java-MapReduce-programma's voor HDInsight][hdinsight-develop-mapreduce]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
