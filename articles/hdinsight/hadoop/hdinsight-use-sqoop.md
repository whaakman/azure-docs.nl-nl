---
title: Apache Sqoop taken uitvoeren met Azure HDInsight (Hadoop) | Microsoft Docs
description: Informatie over het gebruik van Azure PowerShell vanaf een werkstation voor het uitvoeren van Sqoop importeren en exporteren tussen een Hadoop-cluster en een Azure SQL database.
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 2fdcc6b7-6ad5-4397-a30b-e7e389b66c7a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/03/2018
ms.author: jgao
ms.openlocfilehash: e2f4601daf9aa3537f9170c3516d62ab1bd602e5
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a>Sqoop gebruiken met Hadoop in HDInsight
[!INCLUDE [sqoop-selector](../../../includes/hdinsight-selector-use-sqoop.md)]

Informatie over het Sqoop in HDInsight gebruiken om te importeren en exporteren tussen een HDInsight-cluster en Azure SQL database of SQL Server-database.

Hoewel Hadoop een natuurlijke keuze voor het verwerken van ongestructureerde en semigestructureerde gegevens, zoals Logboeken en bestanden, is er mogelijk ook nodig om gestructureerde gegevens die zijn opgeslagen in de relationele databases te verwerken.

[Sqoop] [ sqoop-user-guide-1.4.4] is een hulpprogramma waarmee gegevens worden overgebracht tussen Hadoop-clusters en relationele databases. U kunt deze gebruiken om gegevens te importeren uit een relationele databasebeheersysteem (RDBMS), zoals SQL Server, MySQL of Oracle in het Hadoop distributed file system (HDFS), de gegevens in Hadoop met MapReduce of Hive transformeren en de gegevens vervolgens exporteren naar een RDBMS. In deze zelfstudie maakt u een SQL Server-database gebruikt voor de relationele database.

Zie voor versies die worden ondersteund op HDInsight-clusters Sqoop [wat is er nieuw in de clusterversies geleverd door HDInsight?][hdinsight-versions]

## <a name="understand-the-scenario"></a>Inzicht in het scenario

HDInsight-cluster wordt geleverd met voorbeeldgegevens. U gebruikt de volgende twee voorbeelden:

* Een logboekbestand log4j, bevindt zich op */example/data/sample.log*. De volgende logboeken worden opgehaald uit het bestand:
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* Een Hive-tabel met de naam *hivesampletable*, die verwijst naar het bestand zich bevindt op */hive/warehouse/hivesampletable*. De tabel bevat de gegevens van sommige mobiele apparaten. 
  
  | Veld | Gegevenstype |
  | --- | --- |
  | clientid |tekenreeks |
  | querytime |tekenreeks |
  | markt |tekenreeks |
  | deviceplatform |tekenreeks |
  | devicemake |tekenreeks |
  | devicemodel |tekenreeks |
  | state |tekenreeks |
  | Land |tekenreeks |
  | querydwelltime |dubbel |
  | sessie-id |bigint |
  | sessionpagevieworder |bigint |

In deze zelfstudie gebruikt u deze twee gegevenssets testen Sqoop importeren en exporteren.

## <a name="create-cluster-and-sql-database"></a>Cluster- en SQL-database maken
Deze sectie wordt beschreven hoe u een cluster, een SQL-Database en de SQL-database schema's voor het uitvoeren van de zelfstudie met behulp van de Azure-portal en een Azure Resource Manager-sjabloon maken. De sjabloon kan worden gevonden in [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). De Resource Manager-sjabloon roept een Bacpac-pakket voor het implementeren van het tabelschema met SQL-database.  Het Bacpac-pakket bevindt zich in een openbare blobcontainer, https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac. Als u een persoonlijke container gebruiken voor het Bacpac-bestanden wilt, gebruikt u de volgende waarden in de sjabloon:
   
```json
"storageKeyType": "Primary",
"storageKey": "<TheAzureStorageAccountKey>",
```

Als u liever Azure PowerShell gebruiken voor het maken van het cluster en de SQL-Database, Zie [bijlage A](#appendix-a---a-powershell-sample).

> [!NOTE]
> Importeren met behulp van een sjabloon of de Azure portal ondersteunt alleen een BACPAC-bestand te importeren uit Azure blob-opslag.

**De omgeving met een resource management-sjabloon configureren**
1. Klik op de volgende afbeelding om te openen van een Resource Manager-sjabloon in de Azure portal.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy to Azure"></a>
   
2. Voer de volgende eigenschappen:

    - **Abonnement**: Voer uw Azure-abonnement.
    - **Resourcegroep**: Maak een nieuwe Azure-resourcegroep of Selecteer een bestaande resourcegroep.  Een resourcegroep is voor management doel.  Er is een container voor objecten.
    - **Locatie**: Selecteer een regio.
    - **Clusternaam**: Voer een naam voor het Hadoop-cluster.
    - **Clusteraanmeldgegevens**: de standaardaanmeldnaam is admin.
    - **SSH-aanmeldgegevens**.
    - **SQL server-aanmeldingsnaam en wachtwoord van de database**.
    - **_artifacts locatie**: Gebruik de standaardwaarde, tenzij u het gebruik van uw eigen backpac-bestand in een andere locatie wilt wijzigen.
    - **locatie Sas-Token _artifacts**: laat dit veld leeg.
    - **De bestandsnaam Bacpac-**: Gebruik de standaardwaarde, tenzij u wilt gebruiken van uw eigen backpac-bestand.
     
        De volgende waarden zijn vastgelegd in het gedeelte variabelen:
        
        |Naam|Waarde|
        |----|-----|
        | Naam van het standaardopslagaccount | &lt;CluterName > opslaan |
        | Azure SQL database-servernaam | &lt;ClusterName > dbserver |
        | Naam van een Azure SQL-database | &lt;ClusterName > db |
     
3. Selecteer **ik ga akkoord met de voorwaarden en bepalingen bovengenoemde**.
4. Klik op **Kopen**. U ziet een nieuwe tegel met de titel implementatie indienen voor sjabloonimplementatie. Het duurt ongeveer 20 minuten om het cluster en de SQL-database te maken.

Als u ervoor kiest om bestaande Azure SQL database of Microsoft SQL Server te gebruiken

* **Azure SQL-database**: U moet een firewallregel voor de Azure SQL database-server toegang toestaan via uw werkstation configureren. Zie voor instructies over het maken van een Azure SQL database en het configureren van de firewall [aan de slag met Azure SQL-database][sqldatabase-get-started]. 
  
  > [!NOTE]
  > Standaard kan een Azure SQL database verbindingen van Azure-services, zoals Azure HDInsight. Als deze firewallinstelling is uitgeschakeld, moet u het inschakelen van de Azure-portal. Zie voor instructies over het maken van een Azure SQL database en firewallregels configureren [maken en configureren van de SQL-Database][sqldatabase-create-configue].
  > 
  > 
* **SQL Server**: als uw HDInsight-cluster zich op hetzelfde virtuele netwerk in Azure SQL-Server, kunt u de stappen in dit artikel gebruiken om te importeren en exporteren van gegevens naar een SQL Server-database.
  
  > [!NOTE]
  > HDInsight ondersteunt alleen op basis van locatie virtuele netwerken en het werkt momenteel niet met virtuele netwerken op basis van een affiniteitsgroep.
  > 
  > 
  
  * Als u wilt maken en configureren van een virtueel netwerk, Zie [een virtueel netwerk maken met de Azure-portal](../../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    
    * Wanneer u SQL Server in uw datacenter gebruikt, moet u het virtuele netwerk als configureren *site-naar-site* of *punt-naar-site*.
      
      > [!NOTE]
      > Voor **punt-naar-site** virtuele netwerken, SQL Server moeten worden uitgevoerd de VPN-client configuration toepassing, die beschikbaar via is de **Dashboard** van de configuratie van uw virtuele Azure-netwerk.
      > 
      > 
    * Wanneer u SQL Server op Azure een virtuele machine gebruikt, kan de configuratie van een virtueel netwerk worden gebruikt als de virtuele machine die als host fungeert voor SQL Server een lid van hetzelfde virtuele netwerk als HDInsight.
  * Zie voor informatie over het maken van een HDInsight-cluster op een virtueel netwerk [maken Hadoop-clusters in HDInsight met aangepaste opties](../hdinsight-hadoop-provision-linux-clusters.md)
    
    > [!NOTE]
    > SQL Server moet ook authenticatie toestaan. Een SQL Server-aanmelding moet u de stappen in dit artikel.
    > 
    > 

**Om de configuratie te valideren**

1. Open de resourcegroep in de Azure portal. U ziet vier bronnen in de groep:

    - het cluster
    - de database-server
    - de database
    - het standaardopslagaccount

2. De database opent in Microsoft SQL Server Management Studio.  U ziet twee databases geïmplementeerd:

    ![Azure HDInsight Sqoop SQL Management Studio](./media/hdinsight-use-sqoop/hdinsight-sqoop-sql-management-studio.png)


## <a name="run-sqoop-jobs"></a>Sqoop taken uitvoeren
HDInsight kunt Sqoop taken uitvoeren met behulp van een aantal methoden. Gebruik de volgende tabel om te bepalen welke methode is geschikt voor u en volg de koppeling voor een overzicht.

| **Gebruik deze** als u wilt dat... | ...an **interactieve** shell | ...**batch** verwerken | ...door dit **cluster-besturingssysteem** | ...from dit **clientbesturingssysteem** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](apache-hadoop-use-sqoop-mac-linux.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X of Windows |
| [.NET-SDK voor Hadoop](apache-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |✔ |Linux- of Windows |Windows (voor nu) |
| [Azure PowerShell](apache-hadoop-use-sqoop-powershell.md) |&nbsp; |✔ |Linux- of Windows |Windows |

## <a name="limitations"></a>Beperkingen
* Bulksgewijs export - met Linux gebaseerde HDInsight, de Sqoop-connector gebruikt voor het exporteren van gegevens naar Microsoft SQL Server of Azure SQL Database biedt momenteel geen ondersteuning voor bulksgewijs invoegen.
* Batchverwerking - met HDInsight op basis van Linux, wanneer u de `-batch` bij het uitvoeren van invoeg-switch, Sqoop meerdere invoegen in plaats van de bewerkingen insert batchverwerking wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
Nu hebt u geleerd hoe Sqoop gebruiken. Voor meer informatie zie:

* [Hive gebruiken met HDInsight](../hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](../hdinsight-use-pig.md)
* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]: vinden van andere methoden voor het uploaden van gegevens naar HDInsight/Azure Blob-opslag.

## <a name="appendix-a---a-powershell-sample"></a>Bijlage A - een PowerShell-voorbeeld
De PowerShell-voorbeeld voert de volgende stappen uit:

1. Verbinding maken met Azure.
2. Maak een Azure-resourcegroep. Zie voor meer informatie [Azure PowerShell gebruiken met Azure Resource Manager](../../azure-resource-manager/powershell-azure-resource-manager.md)
3. Een Azure SQL Database-server, een Azure SQL database en twee tabellen maken. 
   
    Als u SQL Server in plaats daarvan gebruikt, gebruikt u de volgende instructies om de tabellen te maken:
   
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))
   
        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])
   
    De eenvoudigste manier om te onderzoeken van de database en tabellen is Visual Studio gebruiken. De databaseserver en de database kunnen worden gecontroleerd met de Azure-portal.
4. Maak een HDInsight-cluster.
   
    Als u wilt onderzoeken van het cluster, kunt u de Azure portal of Azure PowerShell.
5. Het bronbestand van de gegevens vooraf verwerken.
   
    In deze zelfstudie maakt u een een logboekbestand log4j (een bestand met scheidingstekens) en een Hive-tabel exporteren naar een Azure SQL database. Het bestand met scheidingstekens heet */example/data/sample.log*. Eerder in de zelfstudie hebt u enkele voorbeelden van log4j logboeken gezien. In het logboekbestand zijn er enkele lege regels en sommige regels zoals de volgende:
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    Dit is andere voorbeelden die deze gegevens gebruiken, maar deze uitzonderingen moet worden verwijderd voordat we in de Azure SQL-database of SQL Server kunt importeren. Sqoop exporteren niet worden uitgevoerd als er een lege tekenreeks of een regel met een minder element dan het aantal velden die zijn gedefinieerd in de Azure SQL database-tabel. De tabel log4jlogs heeft zeven type string-velden.
   
    Deze procedure maakt u een nieuw bestand op het cluster: tutorials/usesqoop/data/sample.log. Als u wilt onderzoeken het gewijzigde bestand, kunt u de Azure-portal, een hulpprogramma voor Azure Storage explorer of Azure PowerShell. [Aan de slag met HDInsight] [ hdinsight-get-started] is een voorbeeld van code voor het gebruik van Azure PowerShell te downloaden van een bestand en inhoud van het bestand weer te geven.
6. Een gegevensbestand exporteren naar de Azure SQL database.
   
    Het bronbestand is tutorials/usesqoop/data/sample.log. De tabel waar de gegevens worden geëxporteerd naar heet log4jlogs.
   
   > [!NOTE]
   > Anders dan de verbindingsinformatie werken de stappen in deze sectie moeten voor een Azure SQL database of SQL Server. Deze stappen zijn getest met behulp van de volgende configuratie:
   > 
   > * **Punt-naar-site-configuratie voor virtuele Azure-netwerk**: een virtueel netwerk het HDInsight-cluster verbonden met een SQL-Server in een particulier datacenter. Zie [een punt-naar-Site-VPN configureren in de beheerportal](../../vpn-gateway/vpn-gateway-point-to-site-create.md) voor meer informatie.
   > * **Azure HDInsight**: Zie [maken Hadoop-clusters in HDInsight met aangepaste opties](../hdinsight-hadoop-provision-linux-clusters.md) voor informatie over het maken van een cluster in een virtueel netwerk.
   > * **SQL Server 2014**: is geconfigureerd voor verificatie en de VPN-client uitgevoerd configuratiepakket naar een veilige verbinding met het virtuele netwerk toestaan.
   > 
   > 
7. Een Hive-tabel exporteren naar de Azure SQL database.
8. De tabel mobiledata importeren naar het HDInsight-cluster.
   
    Als u wilt onderzoeken het gewijzigde bestand, kunt u de Azure-portal, een hulpprogramma voor Azure Storage explorer of Azure PowerShell.  [Aan de slag met HDInsight] [ hdinsight-get-started] is een voorbeeld van code over het gebruik van Azure PowerShell te downloaden van een bestand en inhoud van het bestand weer te geven.

### <a name="the-powershell-sample"></a>De PowerShell-voorbeeld

```powershell
# Prepare an Azure SQL database to be used by the Sqoop tutorial

#region - provide the following values

$subscriptionID = "<Enter your Azure Subscription ID>"

$sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
$sqlDatabasePassword = "<Enter a Password>"

$httpUserName = "admin"  #HDInsight cluster username
$httpPassword = "<Enter a Password>"

$sshUserName = "sshuser" #HDInsight ssh username
$sshPassword = $httpPassword 

# used for creating Azure service names
$nameToken = "<Enter an alias>" 
$namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
#endregion

#region - variables

# Resource group variables
$resourceGroupName = $namePrefix + "rg"
$location = "East US 2" # used by all Azure services defined in this tutorial

# SQL database varialbes
$sqlDatabaseServerName = $namePrefix + "sqldbserver"
$sqlDatabaseName = $namePrefix + "sqldb"
$sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
$sqlDatabaseMaxSizeGB = 10

# Used for retrieving external IP address and creating firewall rules
$ipAddressRestService = "http://bot.whatismyipaddress.com"
$fireWallRuleName = "UseSqoop"

# Used for creating tables and clustered indexes
$cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
    [t1] [nvarchar](50),
    [t2] [nvarchar](50),
    [t3] [nvarchar](50),
    [t4] [nvarchar](50),
    [t5] [nvarchar](50),
    [t6] [nvarchar](50),
    [t7] [nvarchar](50))"

$cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"

$cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
[clientid] [nvarchar](50),
[querytime] [nvarchar](50),
[market] [nvarchar](50),
[deviceplatform] [nvarchar](50),
[devicemake] [nvarchar](50),
[devicemodel] [nvarchar](50),
[state] [nvarchar](50),
[country] [nvarchar](50),
[querydwelltime] [float],
[sessionid] [bigint],
[sessionpagevieworder][bigint])"

$cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"

# HDInsight variables
$hdinsightClusterName = $namePrefix + "hdi"
$defaultStorageAccountName = $namePrefix + "store"
$defaultBlobContainerName = $hdinsightClusterName
#endregion

# Treat all errors as terminating
$ErrorActionPreference = "Stop"

#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

#region - Create Azure resouce group
Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
try{
    Get-AzureRmResourceGroup -Name $resourceGroupName
}
catch{
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
}
#endregion

#region - Create Azure SQL database server
Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
try{
    Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
catch{
    Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

    $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

    $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                -ResourceGroupName $resourceGroupName `
                                -ServerName $sqlDatabaseServerName `
                                -SqlAdministratorCredentials $credential `
                                -Location $location).ServerName
    Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

    Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
    $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
    New-AzureRmSqlServerFirewallRule `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -FirewallRuleName "$fireWallRuleName-workstation" `
        -StartIpAddress $workstationIPAddress `
        -EndIpAddress $workstationIPAddress

    #To allow other Azure services to access the server add a firewall rule and set both the StartIpAddress and EndIpAddress to 0.0.0.0. 
    #Note that this allows Azure traffic from any Azure subscription to access the server.
    New-AzureRmSqlServerFirewallRule `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -FirewallRuleName "$fireWallRuleName-Azureservices" `
        -StartIpAddress "0.0.0.0" `
        -EndIpAddress "0.0.0.0"
}

#endregion

#region - Create and validate Azure SQL database
Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green

try {
    Get-AzureRmSqlDatabase `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -DatabaseName $sqlDatabaseName
}
catch {
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
    New-AzureRMSqlDatabase `
        -ResourceGroupName $resourceGroupName `
        -ServerName $sqlDatabaseServerName `
        -DatabaseName $sqlDatabaseName `
        -Edition "Standard" `
        -RequestedServiceObjectiveName "S1"
}

#endregion

#region - Create tables
Write-Host "Creating the log4jlogs table and the mobiledata table ..." -ForegroundColor Green

$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = $sqlDatabaseConnectionString
$conn.Open()

# Create the log4jlogs table and index
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.Connection = $conn
$cmd.CommandText = $cmdCreateLog4jTable
$ret = $cmd.ExecuteNonQuery()
$cmd.CommandText = $cmdCreateLog4jClusteredIndex
$cmd.ExecuteNonQuery()

# Create the mobiledata table and index
$cmd.CommandText = $cmdCreateMobileTable
$cmd.ExecuteNonQuery()
$cmd.CommandText = $cmdCreateMobileDataClusteredIndex
$cmd.ExecuteNonQuery()

$conn.close()

#endregion


#region - Create HDInsight cluster

Write-Host "Creating the HDInsight cluster and the dependent services ..." -ForegroundColor Green

# Create the default storage account
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -Type Standard_LRS

# Create the default Blob container
$defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
$defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey 
New-AzureStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageAccountContext 

# Create the HDInsight cluster
$pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

New-AzureRmHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $HDInsightClusterName `
    -Location $location `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version 3.6 `
    -ClusterSizeInNodes 2 `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultStorageContainer $defaultBlobContainerName  

# Validate the cluster
Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
#endregion

#region - pre-process the source file

Write-Host "Preprocessing the source file ..." -ForegroundColor Green

# This procedure creates a new file with $destBlobName
$sourceBlobName = "example/data/sample.log"
$destBlobName = "tutorials/usesqoop/data/sample.log"

# Define the connection string
$storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

# Create block blob objects referencing the source and destination blob.
$storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
$storageClient = $storageAccount.CreateCloudBlobClient();
$storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
$sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
$destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

# Define a MemoryStream and a StreamReader for reading from the source file
$stream = New-Object System.IO.MemoryStream
$stream = $sourceBlob.OpenRead()
$sReader = New-Object System.IO.StreamReader($stream)

# Define a MemoryStream and a StreamWriter for writing into the destination file
$memStream = New-Object System.IO.MemoryStream
$writeStream = New-Object System.IO.StreamWriter $memStream

# Pre-process the source blob
$exString = "java.lang.Exception:"
while(-Not $sReader.EndOfStream){
    $line = $sReader.ReadLine()
    $split = $line.Split(" ")

    # remove the "java.lang.Exception" from the first element of the array
    # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
    if ($split[0] -eq $exString){
        #create a new ArrayList to remove $split[0]
        $newArray = [System.Collections.ArrayList] $split
        $newArray.Remove($exString)

        # update $split and $line
        $split = $newArray
        $line = $newArray -join(" ")
    }

    # remove the lines that has less than 7 elements
    if ($split.count -ge 7){
        write-host $line
        $writeStream.WriteLine($line)
    }
}

# Write to the destination blob
$writeStream.Flush()
$memStream.Seek(0, "Begin")
$destBlob.UploadFromStream($memStream)

#endregion

#region - export a log file from the cluster to the SQL database

Write-Host "Preprocessing the source file ..." -ForegroundColor Green

$tableName_log4j = "log4jlogs"

# Connection string for Azure SQL Database.
# Comment if using SQL Server
$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
# Connection string for SQL Server.
# Uncomment if using SQL Server.
#$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

$exportDir_log4j = "/tutorials/usesqoop/data"

# Submit a Sqoop job
$sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
$sqoopJob = Start-AzureRmHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose
Wait-AzureRmHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
Write-Host "Standard Output" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

#endregion

#region - export a Hive table

$tableName_mobile = "mobiledata"
$exportDir_mobile = "/hive/warehouse/hivesampletable"

$sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
    -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
$sqoopJob = Start-AzureRmHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose

Wait-AzureRmHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Write-Host "Standard Output" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardOutput

#endregion

#region - import a database

$targetDir_mobile = "/tutorials/usesqoop/importeddata/"

$sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
    -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

$sqoopJob = Start-AzureRmHDInsightJob `
                -ClusterName $hdinsightClusterName `
                -HttpCredential $httpCredential `
                -JobDefinition $sqoopDef #-Debug -Verbose

Wait-AzureRmHDInsightJob `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId

Write-Host "Standard Error" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardError

Write-Host "Standard Output" -BackgroundColor Green
Get-AzureRmHDInsightJobOutput `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -DefaultStorageAccountName $defaultStorageAccountName `
    -DefaultStorageAccountKey $defaultStorageAccountKey `
    -DefaultContainer $defaultBlobContainerName `
    -HttpCredential $httpCredential `
    -JobId $sqoopJob.JobId `
    -DisplayOutputType StandardOutput

#endregion
```


[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  ../hdinsight-component-versioning.md
[hdinsight-provision]: ../hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]:apache-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-submit-jobs]:submit-apache-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
