---
title: Op basis van tijd Hadoop Oozie-coordinator gebruiken in HDInsight
description: Op basis van tijd Hadoop Oozie-coordinator gebruiken in HDInsight, een big data-service. Leer hoe u Oozie workflows en coördinatoren definieert en het verzenden van taken.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.author: hrasheed
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 10/04/2017
ROBOTS: NOINDEX
ms.openlocfilehash: a47a30995f651204782325a9f984086fdf382a03
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58202194"
---
# <a name="use-time-based-apache-oozie-coordinator-with-apache-hadoop-in-hdinsight-to-define-workflows-and-coordinate-jobs"></a>Apache Oozie-coördinator op basis van tijd met Apache Hadoop in HDInsight gebruiken voor het definiëren van werkstromen en coördinatie van taken
In dit artikel leert u hoe u workflows en coördinatoren definieert en hoe u activeert de coördinator taken, op basis van tijd. Is het handig om te gaan via [Apache Oozie gebruiken met HDInsight] [ hdinsight-use-oozie] voordat u dit artikel leest. Naast Oozie, kunt u ook met Azure Data Factory taken plannen. Zie voor meer Azure Data Factory [gebruik Apache Pig- en Apache Hive met Data Factory](../data-factory/transform-data.md).

> [!NOTE]  
> Dit artikel gebruikmaken van een Windows-gebaseerde HDInsight-cluster. Zie voor meer informatie over het gebruik van Oozie, met inbegrip van taken die op basis van tijd, op een cluster op basis van Linux, [gebruik Oozie met Hadoop om te definiëren en een werkstroom uitgevoerd op Linux gebaseerde HDInsight](hdinsight-use-oozie-linux-mac.md)

## <a name="what-is-oozie"></a>Wat is Oozie
Apache Oozie is een werkstroomcoördinatie/systeem waarmee Hadoop-taken worden beheerd. Dit is geïntegreerd met de Hadoop-stack en Hadoop-taken worden ondersteund voor Apache Hadoop MapReduce, Apache Pig, Apache Hive en Apache Sqoop. Het kan ook worden gebruikt voor het plannen van taken die specifiek voor een systeem, zoals Java-programma's of shell-scripts zijn.

De volgende afbeelding ziet u de werkstroom die u wilt implementeren:

![Diagram van werkstroom][img-workflow-diagram]

De werkstroom bevat twee acties:

1. Een Hive-bewerking wordt een HiveQL-script uit om te tellen hoe vaak elk type logboek-niveau in een Apache log4j-logboekbestand. Elke log4j-logbestand bestaat uit een line-of velden met een veld [LOGBOEKNIVEAU] om weer te geven van het type en de ernst, bijvoorbeeld:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    De Hive-script-uitvoer is vergelijkbaar met:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    Zie voor meer informatie over Hive [Apache Hive gebruiken met HDInsight][hdinsight-use-hive].
2. Een actie Sqoop exporteert u de uitvoer van de actie HiveQL naar een tabel in een Azure SQL database. Zie voor meer informatie over Sqoop [Apache Sqoop gebruiken met HDInsight][hdinsight-use-sqoop].

> [!NOTE]  
> Zie voor ondersteunde versies van de Oozie op HDInsight-clusters, [wat is er nieuw in de clusterversies geleverd door HDInsight?] [hdinsight-versions].
>
>

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een werkstation met Azure PowerShell**.

    > [!IMPORTANT]  
    > Azure PowerShell-ondersteuning voor het beheer van HDInsight-resources met behulp van Azure Service Manager is **afgeschaft** en verdwijnt per 1 januari 2017. In de stappen in dit document worden de nieuwe HDInsight-cmdlets gebruikt die met Azure Resource Manager werken.
    >
    > Volg de stappen in [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs) (Azure PowerShell installeren en configureren) als u de nieuwste versie van Azure PowerShell wilt installeren. Als u scripts hebt die moeten worden gewijzigd om ze te kunnen gebruiken met de nieuwe cmdlets die werken met Azure Resource Manager, raadpleegt u [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Migreren naar op Azure Resource Manager gebaseerde hulpprogramma’s voor HDInsight-clusters) voor meer informatie.

* **Een HDInsight-cluster**. Zie voor meer informatie over het maken van een HDInsight-cluster [maken van HDInsight-clusters][hdinsight-provision], of [aan de slag met HDInsight][hdinsight-get-started]. U moet de volgende gegevens op de zelfstudie te volgen:

    |Cluster-eigenschap|Naam van de Windows PowerShell-variabele|Value|Description|
    |---|---|---|---|
    |De naam van de HDInsight-cluster|$clusterName||Het HDInsight-cluster op waarop u deze zelfstudie wordt uitgevoerd.|
    |Gebruikersnaam van de HDInsight-cluster|$clusterUsername||De naam van de gebruiker in de HDInsight-cluster. |
    |Gebruikerswachtwoord van HDInsight-cluster |$clusterPassword||Het wachtwoord van de gebruiker in de HDInsight-cluster.|
    |Naam van het Azure-opslagaccount|$storageAccountName||Een Azure Storage-account beschikbaar voor het HDInsight-cluster. Voor deze zelfstudie gebruikt u het standaardopslagaccount die u hebt opgegeven tijdens het inrichten cluster.|
    |Naam van een Azure Blob-container|$containerName||In dit voorbeeld gebruikt u de Azure Blob storage-container die wordt gebruikt voor het standaardbestandssysteem voor de HDInsight-cluster. Standaard heeft dezelfde naam als het HDInsight-cluster.|


* **Een Azure SQL database**. U moet een firewallregel voor de SQL-Database-server zodat toegang vanaf uw werkstation configureren. Zie voor instructies over het maken van een Azure SQL database en het configureren van de firewall [aan de slag met Azure SQL-database][sqldatabase-get-started]. Dit artikel bevat een Windows PowerShell-script voor het maken van de Azure SQL database-tabel die u nodig hebt voor deze zelfstudie.

    |SQL database-eigenschap|Naam van de Windows PowerShell-variabele|Value|Description|
    |---|---|---|---|
    |SQL database-servernaam|$sqlDatabaseServer||De SQL-databaseserver waarop Sqoop gegevens worden geëxporteerd. |
    |Aanmeldingsnaam voor SQL database|$sqlDatabaseLogin||Aanmeldingsnaam voor SQL-Database.|
    |Aanmeldingswachtwoord voor SQL database|$sqlDatabaseLoginPassword||Aanmeldingswachtwoord voor SQL-Database.|
    |SQL-databasenaam|$sqlDatabaseName||De Azure SQL-database waarnaar Sqoop gegevens worden geëxporteerd. |

  > [!NOTE]   
  > Een Azure SQL database staat standaard verbindingen van Azure-Services, zoals Azure HDInsight. Als deze firewallinstelling is uitgeschakeld, moet u dit inschakelen via de Azure-portal. Zie voor instructies over het maken van een SQL-Database en firewallregels configureren, [maken en SQL Database configureren][sqldatabase-get-started].

> [!NOTE]  
> Invullen de waarden in de tabellen. Dit is handig voor deze zelfstudie te doorlopen.

## <a name="define-oozie-workflow-and-the-related-hiveql-script"></a>Oozie-workflow en het bijbehorende HiveQL-script definiëren
Oozie werkstromen definities worden geschreven in hPDL (een XML-proces definition language). De standaardnaam van de werkstroom-bestand is *workflow.xml*.  U wordt een bestand van de werkstroom lokaal opslaan en vervolgens implementeert op het HDInsight-cluster met behulp van Azure PowerShell verderop in deze zelfstudie.

De Hive-actie in de werkstroom wordt een bestand HiveQL-script. Dit scriptbestand bevat drie HiveQL-instructies:

1. **De instructie DROP TABLE** de log4j Hive-tabel worden verwijderd als deze bestaat.
2. **De instructie CREATE TABLE** maakt een log4j Hive externe tabel, die naar de locatie van de log4j-logboekbestand verwijst.
3. **De locatie van het log4j-logboekbestand**. Veldscheidingsteken is ','. Het scheidingsteken voor regel is '\n'. Een externe Hive-tabel wordt gebruikt om te voorkomen dat het bestand wordt verwijderd uit de oorspronkelijke locatie, als u wilt de Oozie-workflow meerdere keren uitvoeren.
4. **De instructie INSERT OVERSCHRIJVEN** telt het aantal exemplaren van elk type logboek-niveau van de log4j Hive-tabel en de uitvoer wordt opgeslagen naar een Azure Blob storage-locatie.

> [!NOTE]  
> Er is een bekend probleem van de Hive-pad. U wordt dit probleem ondervindt bij het indienen van een Oozie-taak. De instructies voor het verhelpen van het probleem kunnen worden gevonden op de TechNet-Wiki: [HDInsight Hive-fout: Kan de naam van][technetwiki-hive-error].

**De HiveQL-script-bestand om te worden aangeroepen door de werkstroom definiëren**

1. Maak een tekstbestand met de volgende inhoud:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Er zijn drie variabelen die worden gebruikt in het script:

   * ${hiveTableName}
   * ${hiveDataFolder}
   * ${hiveOutputFolder}

     Bestand van de werkstroom-definitie (workflow.xml in deze zelfstudie) wordt deze waarden doorgegeven aan deze HiveQL-script uit tijdens de uitvoering.
2. Sla het bestand op als **C:\Tutorials\UseOozie\useooziewf.hql** met behulp van ANSI (ASCII)-codering. (Kladblok gebruiken als uw teksteditor niet met deze optie biedt). Dit scriptbestand wordt later in de zelfstudie aan de HDInsight-cluster worden geïmplementeerd.

**Om een werkstroom te definiëren**

1. Maak een tekstbestand met de volgende inhoud:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>
    ```

    Er zijn twee acties die zijn gedefinieerd in de werkstroom. De actie start voor *RunHiveScript*. Als de actie wordt uitgevoerd *OK*, is de volgende actie *RunSqoopExport*.

    De RunHiveScript heeft meerdere variabelen. U geeft de waarden bij het verzenden van de taak Oozie vanaf uw werkstation met behulp van Azure PowerShell.

    Werkstroomvariabelen voor de

    |Werkstroomvariabelen voor de|Description|
    |---|---|
    |${jobTracker}|Geef de URL van het beheer van Hadoop-taak. Gebruik **jobtrackerhost:9010** cluster in HDInsight versie 3.0 en 2.0.|
    |${nameNode}|Geef de URL van het knooppunt van de naam van Hadoop. Gebruik de standaard-bestand system wasb: / / -adres, bijvoorbeeld *wasb: / /&lt;containerName&gt;\@&lt;storageAccountName&gt;. blob.core.windows.net*.|
    |${queueName}|Hiermee geeft u de naam van de wachtrij die de taak om te worden verzonden. Gebruik **standaard**.|

    Variabelen voor de hive

    |Hive-takenreeksbewerkingsvariabele|Description|
    |----|----|
    |${hiveDataFolder}|De bronmap voor de opdracht Create Table Hive.|
    |${hiveOutputFolder}|De map voor uitvoer voor de instructie INSERT OVERSCHRIJVEN.|
    |${hiveTableName}|De naam van de Hive-tabel die verwijst naar de log4j-gegevensbestanden.|

    Variabelen voor de Sqoop

    |Sqoop takenreeksbewerkingsvariabele|Description|
    |---|---|
    |${sqlDatabaseConnectionString}|Verbindingsreeks van de SQL-Database.|
    |${sqlDatabaseTableName}|De Azure SQL database-tabel op waarin de gegevens worden geëxporteerd.|
    |${hiveOutputFolder}|De map voor uitvoer voor de instructie Hive invoegen OVERSCHRIJVEN. Dit is dezelfde map voor het exporteren met Sqoop (export-dir).|

    Zie voor meer informatie over Oozie-workflow en het gebruik van de werkstroomacties [Apache Oozie 4.0 documentatie] [ apache-oozie-400] (voor HDInsight-clusterversie 3.0) of [Apache Oozie 3.3.2-documentatie ] [ apache-oozie-332] (voor HDInsight-clusterversie 2.1).

1. Sla het bestand op als **C:\Tutorials\UseOozie\workflow.xml** met behulp van ANSI (ASCII)-codering. (Kladblok gebruiken als uw teksteditor niet met deze optie biedt).

**Coördinator definiëren**

1. Maak een tekstbestand met de volgende inhoud:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    Er zijn vijf variabelen die worden gebruikt in het definitiebestand:

   | Variabele | Description |
   | --- | --- |
   | ${coordFrequency} |Tijd voor de taak onderbreken. Frequentie wordt altijd uitgedrukt in minuten. |
   | ${coordStart} |Begintijd van taak. |
   | ${coordEnd} |Eindtijd van taak. |
   | ${coordTimezone} |Oozie verwerkt u coordinator-taken in een vaste tijdzone met geen zomertijd (doorgaans weergegeven met behulp van UTC). Deze tijdzone wordt verwezen als de "Oozie verwerking tijdzone." |
   | ${wfPath} |Het pad voor de workflow.xml.  Als de naam van de werkstroom is niet de standaard-bestandsnaam (workflow.xml), moet u deze opgeven. |
2. Sla het bestand op als **C:\Tutorials\UseOozie\coordinator.xml** met behulp van de ANSI (ASCII)-codering. (Kladblok gebruiken als uw teksteditor niet met deze optie biedt).

## <a name="deploy-the-oozie-project-and-prepare-the-tutorial"></a>Het project Oozie implementeren en voorbereiden van de zelfstudie
U kunt een Azure PowerShell-script om uit te voeren de volgende wordt uitgevoerd:

* Kopieer het HiveQL-script (useoozie.hql) naar Azure Blob-opslag, wasb:///tutorials/useoozie/useoozie.hql.
* Kopieer workflow.xml naar wasb:///tutorials/useoozie/workflow.xml.
* Kopieer coordinator.xml naar wasb:///tutorials/useoozie/coordinator.xml.
* Kopieer het bestand (/ example/data/sample.log) naar wasb:///tutorials/useoozie/data/sample.log.
* Maak een Azure SQL database-tabel voor het opslaan van gegevens van Sqoop exporteren. De tabelnaam van de is *log4jLogCount*.

**Informatie over HDInsight-opslag**

HDInsight maakt gebruik van Azure Blob-opslag voor gegevensopslag. wasb: / / Microsoft implementatie van het Hadoop distributed file system (HDFS) in Azure Blob-opslag is. Zie voor meer informatie, [Azure Blob storage gebruiken met HDInsight][hdinsight-storage].

Wanneer u een HDInsight-cluster inrichten, wordt een Azure Blob storage-account en een specifieke container uit dat account aangewezen als het standaardbestandssysteem, zoals in HDFS. Naast dit opslagaccount wordt gebruikt, u kunt extra opslagaccounts toevoegen uit hetzelfde Azure-abonnement of uit verschillende Azure-abonnementen tijdens het inrichtingsproces. Zie voor instructies over het toevoegen van extra opslagaccounts [inrichten HDInsight-clusters][hdinsight-provision]. Ter vereenvoudiging van de Azure PowerShell-script in deze zelfstudie gebruikt, worden alle bestanden opgeslagen in de systeemcontainer van de standaard-bestand dat zich bevindt in */zelfstudies/useoozie*. Deze container heeft dezelfde naam als de naam van het HDInsight-cluster.
De syntaxis is:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]  
> Alleen de *wasb: / /* syntaxis wordt ondersteund in HDInsight-clusterversie 3.0. De oudere *asv: / /* syntaxis wordt ondersteund in HDInsight 2.1 en 1.6-clusters, maar het wordt niet ondersteund in HDInsight 3.0-clusters.
>
> De wasb: / / pad is een virtueel pad. Zie voor meer informatie [Azure Blob storage gebruiken met HDInsight][hdinsight-storage].

Een bestand dat is opgeslagen in de systeemcontainer van de standaard-bestand kan worden geopend vanuit HDInsight met behulp van de volgende URI's (ik gebruik workflow.xml als voorbeeld):

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Als u toegang tot het bestand rechtstreeks vanuit het opslagaccount wilt, wordt de naam van de blob voor het bestand is:

    tutorials/useoozie/workflow.xml

**Hive-tabellen voor interne en externe begrijpen**

Er zijn enkele dingen die u wilt weten over de Hive-tabellen voor interne en externe:

* De opdracht CREATE TABLE maakt een interne tabel, ook wel bekend als een beheerde tabel. Het bestand moet zich bevinden in de standaard-container.
* De opdracht CREATE TABLE het bestand verplaatst naar de/hive/datawarehouse/<TableName> map in de standaard-container.
* De opdracht CREATE EXTERNAL TABLE maakt een externe tabel. Het gegevensbestand kan buiten de standaardcontainer zich bevinden.
* Het bestand met verplaatst de opdracht CREATE EXTERNAL TABLE niet.
* De opdracht CREATE EXTERNAL TABLE toegestaan niet in alle Deelmappen onder de map die is opgegeven in de component locatie. Dit is de reden waarom de zelfstudie een kopie van het bestand sample.log maakt.

Zie voor meer informatie, [HDInsight: Apache Hive-tabellen voor interne en externe Intro][cindygross-hive-tables].

**Voorbereiden van de zelfstudie**

1. Open de Windows PowerShell ISE (Typ in het scherm Start van Windows 8 **PowerShell_ISE**, en klik vervolgens op **Windows PowerShell ISE**. Zie voor meer informatie, [Start Windows PowerShell op Windows 8 en Windows][powershell-start]).
2. Voer in het onderste deelvenster de volgende opdracht uit om te verbinden met uw Azure-abonnement:

    ```powershell
    Add-AzureAccount
    ```

    U wordt gevraagd de referenties van uw Azure-account in te voeren. Deze methode van het toevoegen van een abonnementsverbinding een time-out optreedt, en nadat de 12 uur, hebt u de cmdlet opnieuw uitvoeren.

   > [!NOTE]  
   > Als u meerdere Azure-abonnementen hebt en het standaardabonnement niet de versie die u wilt gebruiken is, gebruikt u de <strong>Select-AzureSubscription</strong> cmdlet om een abonnement te selecteren.

3. Kopieer het volgende script in het scriptvenster en stelt u de eerste zes variabelen:

    ```powershell
    # WASB variables
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<BlobStorageContainerName>"

    # SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    # Oozie files for the tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing the Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use the long path here
    ```

    Zie voor meer beschrijvingen van de variabelen voor de [vereisten](#prerequisites) sectie in deze zelfstudie.

4. Het script op in het scriptvenster voegt u het volgende:

    ```powershell
    # Create a storage context object
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    function uploadOozieFiles()
    {
        Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
        Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
        Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
        Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
    }

    function prepareHiveDataFile()
    {
        Write-Host "Make a copy of the sample.log file ... " -ForegroundColor Green
        Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
    }

    function prepareSQLDatabase()
    {
        # SQL query string for creating log4jLogsCount table
        $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [Level] [nvarchar](10) NOT NULL,
                [Total] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
            [Level] ASC
            )
            )"

        #Create the log4jLogsCount table
        Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $cmdCreateLog4jCountTable
        $cmd.executenonquery()

        $conn.close()
    }

    # upload workflow.xml, coordinator.xml, and ooziewf.hql
    uploadOozieFiles;

    # make a copy of example/data/sample.log to example/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. Klik op **-Script uitvoeren** of druk op **F5** het script uit te voeren. De uitvoer is vergelijkbaar met:

    ![Uitvoer van de zelfstudie voorbereiden][img-preparation-output]

## <a name="run-the-oozie-project"></a>Voer het project Oozie
Azure PowerShell biedt op dit moment geen cmdlets voor het definiëren van Oozie-taken. U kunt de **Invoke-RestMethod** cmdlet om aan te roepen Oozie-webservices. De Oozie web services-API is een HTTP REST-API JSON. Zie voor meer informatie over de webservices Oozie API [Apache Oozie 4.0 documentatie] [ apache-oozie-400] (voor HDInsight-clusterversie 3.0) of [Apache Oozie 3.3.2 documentatie] [ apache-oozie-332] (voor HDInsight-clusterversie 2.1).

**Een Oozie-taak verzenden**

1. Open de Windows PowerShell ISE (in Windows 8-startscherm, typt u **PowerShell_ISE**, en klik vervolgens op **Windows PowerShell ISE**. Zie voor meer informatie, [Start Windows PowerShell op Windows 8 en Windows][powershell-start]).
2. Kopieer het volgende script in het scriptvenster en stelt u de eerste veertien variabelen (echter overslaan **$storageUri**).

    ```powershell
    #HDInsight cluster variables
    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterUserPassword>"

    #Azure Blob storage (WASB) variables
    $storageAccountName = "<StorageAccountName>"
    $storageContainerName = "<BlobContainerName>"
    $storageUri="wasb://$storageContainerName@$storageAccountName.blob.core.windows.net"

    #Azure SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"

    #Oozie WF/coordinator variables
    $coordStart = "2014-03-21T13:45Z"
    $coordEnd = "2014-03-21T13:45Z"
    $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
    $coordTimezone = "UTC"    #UTC/GMT

    $oozieWFPath="$storageUri/tutorials/useoozie"  # The default name is workflow.xml. And you don't need to specify the file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"  
    $sqlDatabaseTableName = "log4jLogsCount"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)
    ```

    Zie voor meer beschrijvingen van de variabelen voor de [vereisten](#prerequisites) sectie in deze zelfstudie.

    $coordstart en $coordend zijn de werkstroom begin- en eindtijd. Als u wilt weten de UTC/GMT-tijd, zoeken naar "utc-tijd' op bing.com. De $coordFrequency is hoe vaak in minuten die u wilt uitvoeren van de werkstroom.
3. Hiermee voegt u het volgende aan het script. Dit gedeelte definieert de nettolading van de Oozie:

    ```powershell
    #OoziePayload used for Oozie web service submission
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
            <name>nameNode</name>
            <value>$storageUrI</value>
        </property>

        <property>
            <name>jobTracker</name>
            <value>jobtrackerhost:9010</value>
        </property>

        <property>
            <name>queueName</name>
            <value>default</value>
        </property>

        <property>
            <name>oozie.use.system.libpath</name>
            <value>true</value>
        </property>

        <property>
            <name>oozie.coord.application.path</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>wfPath</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>coordStart</name>
            <value>$coordStart</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>$coordEnd</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>$coordFrequency</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>$coordTimezone</value>
        </property>

        <property>
            <name>hiveScript</name>
            <value>$hiveScript</value>
        </property>

        <property>
            <name>hiveTableName</name>
            <value>$hiveTableName</value>
        </property>

        <property>
            <name>hiveDataFolder</name>
            <value>$hiveDataFolder</value>
        </property>

        <property>
            <name>hiveOutputFolder</name>
            <value>$hiveOutputFolder</value>
        </property>

        <property>
            <name>sqlDatabaseConnectionString</name>
            <value>&quot;$sqlDatabaseConnectionString&quot;</value>
        </property>

        <property>
            <name>sqlDatabaseTableName</name>
            <value>$SQLDatabaseTableName</value>
        </property>

        <property>
            <name>user.name</name>
            <value>admin</value>
        </property>

    </configuration>
    "@
    ```

   > [!NOTE]  
   > Het belangrijkste verschil in vergelijking met het bestand van de werkstroom verzending van de nettolading van de variabele is **oozie.coord.application.path**. Wanneer u een werkstroomtaak verzendt, gebruikt u **oozie.wf.application.path** in plaats daarvan.

4. Hiermee voegt u het volgende aan het script. Dit onderdeel controleert de status van de Oozie-web-service:

    ```powershell
    function checkOozieServerStatus()
    {
        Write-Host "Checking Oozie server status..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieServerStatus = $jsonResponse[0].("systemMode")
        Write-Host "Oozie server status is $oozieServerStatus..."

        if($oozieServerStatus -notmatch "NORMAL")
        {
            Write-Host "Oozie server status is $oozieServerStatus...cannot submit Oozie jobs. Check the server status and re-run the job."
        }
    }
    ```

5. Hiermee voegt u het volgende aan het script. Dit gedeelte maakt u een taak Oozie:

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending the following Payload to the cluster:" -ForegroundColor Green
        Write-Host "`n--------`n$OoziePayload`n--------"
        $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieJobId = $jsonResponse[0].("id")
        Write-Host "Oozie job id is $oozieJobId..."

        return $oozieJobId
    }
    ```

   > [!NOTE]  
   > Wanneer u een werkstroomtaak verzendt, moet u een andere webservice aanroepen van de taak starten nadat de taak is gemaakt. In dit geval wordt de coördinator-taak geactiveerd door de tijd. De taak wordt automatisch gestart.

6. Hiermee voegt u het volgende aan het script. Dit onderdeel controleert de status van de Oozie-taak:

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until the job metadata is populated in the Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for the job to complete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for the job to complete..."
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")
        }

        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
        if($JobStatus -notmatch "SUCCEEDED")
        {
            Write-Host "Check logs at http://headnode0:9014/cluster for detais."
        }
    }
    ```

7. (Optioneel) Hiermee voegt u het volgende aan het script.

    ```powershell
    function listOozieJobs()
    {
        Write-Host "Listing Oozie jobs..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

        write-host "Job ID                                   App Name        Status      Started                         Ended"
        write-host "----------------------------------------------------------------------------------------------------------------------------------"
        foreach($job in $response.workflows)
        {
            Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
        }
    }

    function ShowOozieJobLog($oozieJobId)
    {
        Write-Host "Showing Oozie job info..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
        write-host $response
    }

    function killOozieJob($oozieJobId)
    {
        Write-Host "Killing the Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for the 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. Aan het script voegt u het volgende:

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

De tekens # verwijderen als u wilt de aanvullende functies uitvoeren.

1. Als uw HDInsight-cluster versie 2.1 is, vervangt u 'https://$clusterName.azurehdinsight.net:443/oozie/v2/"met 'https://$clusterName.azurehdinsight.net:443/oozie/v1/'. HDInsight-clusterversie 2.1 wordt niet ondersteund versie 2 van de web-services.
1. Klik op **-Script uitvoeren** of druk op **F5** het script uit te voeren. De uitvoer is vergelijkbaar met:

    ![Zelfstudie werkstroomuitvoer uitvoeren][img-runworkflow-output]
1. Verbinding maken met uw SQL-Database om de geëxporteerde gegevens te bekijken.

**Om te controleren of het foutenlogboek van taak**

Voor het oplossen van een werkstroom, kan het logboekbestand Oozie worden gevonden op C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log van het hoofdknooppunt van het cluster. Zie voor informatie over RDP, [beheren Apache Hadoop-clusters in HDInsight met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md).

**De zelfstudie opnieuw uit te voeren**

Als u wilt opnieuw met het uitvoeren van de werkstroom, moet u de volgende taken uitvoeren:

* Het Hive-script-uitvoerbestand verwijderen.
* De gegevens in de tabel log4jLogsCount verwijderen.

Hier volgt een voorbeeld Windows PowerShell-script die u kunt gebruiken:

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete the Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all the records from the log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een Oozie-workflow en een Oozie-coördinator definieert en hoe u een Oozie-coordinator-taak uitvoeren met behulp van Azure PowerShell. Zie de volgende artikelen voor meer informatie:

* [Aan de slag met HDInsight][hdinsight-get-started]
* [Azure Blob storage gebruiken met HDInsight][hdinsight-storage]
* [HDInsight beheren met behulp van Azure PowerShell][hdinsight-admin-powershell]
* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Apache Sqoop gebruiken met HDInsight][hdinsight-use-sqoop]
* [Apache Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Apache Pig gebruiken met HDInsight][hdinsight-use-pig]
* [Java MapReduce-programma's ontwikkelen voor HDInsight][hdinsight-develop-java-mapreduce]

[hdinsight-cmdlets-download]: https://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md

[hdinsight-use-sqoop]:hadoop/hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: https://hadoop.apache.org/
[apache-oozie-400]: https://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: https://oozie.apache.org/docs/3.3.2/

[powershell-download]: https://azure.microsoft.com/downloads/
[powershell-about-profiles]: https://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: https://docs.microsoft.com/powershell/scripting/setup/starting-windows-powershell?view=powershell-6
[powershell-script]: https://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: https://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
