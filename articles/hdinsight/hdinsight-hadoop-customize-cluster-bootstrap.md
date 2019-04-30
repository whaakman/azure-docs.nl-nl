---
title: Aanpassen van configuraties van clusters op Azure HDInsight met bootstrap
description: Informatie over het aanpassen van de configuratie van het HDInsight-cluster programmatisch met behulp van .net, PowerShell en Resource Manager templates.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/19/2019
ms.openlocfilehash: b1bc0a68a9cf52e886c0664a474a4dbb75126698
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60735954"
---
# <a name="customize-hdinsight-clusters-using-bootstrap"></a>HDInsight clusters aanpassen met Bootstrap

Bootstrap-scripts kunnen u installeren en configureren van onderdelen in Azure HDInsight via een programma. 

Er zijn drie manieren instellingen voor het configuratiebestand instellen als uw HDInsight-cluster is gemaakt:

* Azure PowerShell gebruiken
* .NET SDK gebruiken
* Azure Resource Manager-sjabloon gebruiken

Bijvoorbeeld kunt met behulp van deze programmatische methoden, u opties in de volgende bestanden:

* clusterIdentity.xml
* core-site.xml
* gateway.XML
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml
* Server.Properties (kafka-broker-configuratie)

Zie voor meer informatie over het installeren van extra onderdelen in HDInsight-cluster tijdens de aanmaaktijd [aanpassen HDInsight-clusters met Script Action (Linux)](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="prerequisites"></a>Vereisten

* Als u PowerShell gebruikt, moet u de [Az Module](https://docs.microsoft.com/powershell/azure/overview).

## <a name="use-azure-powershell"></a>Azure PowerShell gebruiken

De volgende PowerShell-code past een [Apache Hive](https://hive.apache.org/) configuratie:

> [!IMPORTANT]  
> De parameter `Spark2Defaults` moet mogelijk worden gebruikt met [toevoegen AzHDInsightConfigValues](https://docs.microsoft.com/powershell/module/az.hdinsight/add-azhdinsightconfigvalues). U kunt lege waarden doorgeven aan de parameter zoals wordt weergegeven in het onderstaande codevoorbeeld.


```powershell
# hive-site.xml configuration
$hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValues `
        -HiveSite $hiveConfigValues `
        -Spark2Defaults @{}

New-AzHDInsightCluster `
    -ResourceGroupName $existingResourceGroupName `
    -ClusterName $clusterName `
    -Location $location `
    -ClusterSizeInNodes $clusterSizeInNodes `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -Config $config
```

Een volledig werkend PowerShell-script kan worden gevonden in [bijlage](#appendix-powershell-sample).

**Om te controleren of de wijziging:**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Klik in het menu links op **HDInsight-clusters**. Als u dit niet ziet, klikt u op **alle services** eerste.
3. Klik op het cluster dat u zojuist hebt gemaakt met de PowerShell-script.
4. Klik op **Dashboard** vanaf de bovenkant van de blade te openen van de Ambari UI.
5. Klik op **Hive** in het menu links.
6. Klik op **HiveServer2** van **samenvatting**.
7. Klik op de **Peeringconfiguraties** tabblad.
8. Klik op **Hive** in het menu links.
9. Klik op het tabblad **Geavanceerd**.
10. Schuif naar beneden en vouw vervolgens **geavanceerde hive-site**.
11. Zoek naar **hive.metastore.client.socket.timeout** in de sectie.

Aantal meer voorbeelden over het aanpassen van andere configuratiebestanden:

```xml
# hdfs-site.xml configuration
$HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

# core-site.xml configuration
$CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

# mapred-site.xml configuration
$MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

# oozie-site.xml configuration
$OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120
```

## <a name="use-net-sdk"></a>.NET SDK gebruiken
Zie [maken Linux gebaseerde clusters in HDInsight met behulp van de .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).

## <a name="use-resource-manager-template"></a>Gebruik Resource Manager-sjabloon
U kunt de bootstrap in Resource Manager-sjabloon gebruiken:

```json
"configurations": {
    "hive-site": {
        "hive.metastore.client.connect.retry.delay": "5",
        "hive.execution.engine": "mr",
        "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
    }
}
```

![HDInsight Hadoop aanpassen cluster bootstrap Azure Resource Manager-sjabloon](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

## <a name="see-also"></a>Zie ook
* [Apache Hadoop-clusters maken in HDInsight] [ hdinsight-provision-cluster] vindt u instructies voor het maken van een HDInsight-cluster met behulp van andere aangepaste opties.
* [Script Action-scripts ontwikkelen voor HDInsight][hdinsight-write-script]
* [Installeren en gebruiken van Apache Spark op HDInsight-clusters][hdinsight-install-spark]
* [Installeren en Apache Giraph gebruikt op HDInsight-clusters](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions-linux.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fasen tijdens het maken van clusters"

## <a name="appendix-powershell-sample"></a>Bijlage: Voorbeeld van PowerShell

Dit PowerShell-script maakt een HDInsight-cluster en een Hive-instelling past. Zorg ervoor dat u waarden opgeven voor `$nameToken`, `$httpPassword`, en `$sshPassword`.

> [!IMPORTANT]  
> De waarden voor `DefaultStorageAccount`, en `DefaultStorageContainer` worden niet geretourneerd van [Get-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) wanneer [veilige overdracht](../storage/common/storage-require-secure-transfer.md) is ingeschakeld op het storage-account.

> [!WARNING]  
> Type opslagaccount `BlobStorage` kan niet worden gebruikt voor HDInsight-clusters.


```powershell
####################################
# Set these variables
####################################
#region - used for creating Azure service names
$nameToken = "<ENTER AN ALIAS>" 
#endregion

#region - cluster user accounts
$httpUserName = "admin"  #HDInsight cluster username
$httpPassword = '<ENTER A PASSWORD>' 

$sshUserName = "sshuser" #HDInsight ssh user name
$sshPassword = '<ENTER A PASSWORD>' 
#endregion

####################################
# Service names and varialbes
####################################
#region - service names
$namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

$resourceGroupName = $namePrefix + "rg"
$hdinsightClusterName = $namePrefix + "hdi"
$defaultStorageAccountName = $namePrefix + "store"
$defaultBlobContainerName = $hdinsightClusterName

$location = "East US"
#endregion


####################################
# Connect to Azure
####################################
#region - Connect to Azure subscription
Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# If you have multiple subscriptions, set the one to use
# Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"

#endregion

#region - Create an HDInsight cluster
####################################
# Create dependent components
####################################
Write-Host "Creating a resource group ..." -ForegroundColor Green
New-AzResourceGroup `
    -Name  $resourceGroupName `
    -Location $location

Write-Host "Creating the default storage account and default blob container ..."  -ForegroundColor Green
New-AzStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $defaultStorageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableHttpsTrafficOnly 1

$defaultStorageAccountKey = (Get-AzStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value

$defaultStorageContext = New-AzStorageContext `
                                -StorageAccountName $defaultStorageAccountName `
                                -StorageAccountKey $defaultStorageAccountKey

New-AzStorageContainer `
    -Name $defaultBlobContainerName `
    -Context $defaultStorageContext #use the cluster name as the container name

####################################
# Create a configuration object
####################################
$hiveConfigValues = @{"hive.metastore.client.socket.timeout"="90"}

$config = New-AzHDInsightClusterConfig `
    | Set-AzHDInsightDefaultStorage `
        -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -StorageAccountKey $defaultStorageAccountKey `
    | Add-AzHDInsightConfigValues `
        -HiveSite $hiveConfigValues `
        -Spark2Defaults @{}

####################################
# Create an HDInsight cluster
####################################
$httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
$httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

$sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
$sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

New-AzHDInsightCluster `
    -ResourceGroupName $resourceGroupName `
    -ClusterName $hdinsightClusterName `
    -Location $location `
    -ClusterSizeInNodes 1 `
    -ClusterType Hadoop `
    -OSType Linux `
    -Version "3.6" `
    -HttpCredential $httpCredential `
    -SshCredential $sshCredential `
    -Config $config

####################################
# Verify the cluster
####################################
Get-AzHDInsightCluster `
    -ClusterName $hdinsightClusterName

#endregion
```
