---
title: Apache Hadoop-clusters in HDInsight met PowerShell - Azure beheren
description: Informatie over het uitvoeren van beheertaken voor de Apache Hadoop-clusters in HDInsight met behulp van Azure PowerShell.
ms.reviewer: tyfox
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: tylerfox
ms.openlocfilehash: a7e129f43b957b271c77f451ab198a9068bb0797
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62096353"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Apache Hadoop-clusters in HDInsight beheren met behulp van Azure PowerShell
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell kan worden gebruikt om te beheren en automatiseren van de implementatie en het beheer van uw workloads in Azure. In dit artikel leert u hoe u voor het beheren van [Apache Hadoop](https://hadoop.apache.org/) -clusters in Azure HDInsight met behulp van de Az van Azure PowerShell-module. Zie voor een lijst van de HDInsight PowerShell-cmdlets, de [Az.HDInsight verwijzing](https://docs.microsoft.com/powershell/module/az.hdinsight).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* De PowerShell [Az Module](https://docs.microsoft.com/powershell/azure/overview) geïnstalleerd.

## <a name="create-clusters"></a>Clusters maken
Zie [maken Linux gebaseerde clusters in HDInsight met behulp van Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Clusters groeperen
Gebruik de volgende opdracht om alle clusters in het huidige abonnement weer te geven:

```powershell
Get-AzHDInsightCluster
```

## <a name="show-cluster"></a>Cluster weergeven
Gebruik de volgende opdracht om de details van een specifiek cluster in het huidige abonnement weer te geven:

```powershell
Get-AzHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Clusters verwijderen
Gebruik de volgende opdracht om een cluster te verwijderen:

```powershell
Remove-AzHDInsightCluster -ClusterName <Cluster Name>
```

U kunt ook een cluster verwijderen door het verwijderen van de resourcegroep waarin het cluster. Een resourcegroep verwijdert, worden alle resources in de groep met inbegrip van het standaardopslagaccount.

```powershell
Remove-AzResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Clusters schalen
Het cluster schalen functie kunt u het aantal worker-knooppunten die worden gebruikt door een cluster dat wordt uitgevoerd in Azure HDInsight zonder te hoeven maken van het cluster opnieuw wijzigen.

De gevolgen van het wijzigen van het aantal gegevensknooppunten voor elk type cluster die door HDInsight worden ondersteund:

* Apache Hadoop

    Het aantal worker-knooppunten in een Hadoop-cluster dat wordt uitgevoerd zonder gevolgen voor alle taken die in behandeling of wordt uitgevoerd, kunt u naadloos verhogen. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking uitgevoerd wordt. Fouten in een bewerking voor vergroten/verkleinen probleemloos verwerkt zodat het cluster altijd in een functionele staat blijft.

    Wanneer een Hadoop-cluster is omlaag geschaald door het aantal gegevensknooppunten te verminderen, zijn sommige van de services in het cluster opnieuw opgestart. Opnieuw starten van services zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken na het voltooien van de bewerking vergroten/verkleinen. U kunt echter de taken opnieuw zodra de bewerking voltooid is.
* Apache HBase

    U kunt naadloos toevoegen of verwijderen van knooppunten in uw HBase-cluster, terwijl deze wordt uitgevoerd. Regionale Servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de vergroten/verkleinen bewerking. U kunt echter ook handmatig de regionale servers in evenwicht door te melden bij het hoofdknooppunt van het cluster en voer de volgende opdrachten vanuit een opdrachtpromptvenster:

    ```bash
    pushd %HBASE_HOME%\bin
    hbase shell
    balancer
    ```

* Apache Storm

    U kunt naadloos toevoegen of verwijderen van gegevensknooppunten naar uw Storm-cluster, terwijl deze wordt uitgevoerd. Maar na een geslaagde bewerking vergroten/verkleinen is voltooid, moet u opnieuw verdelen van de topologie.

    Herverdelen kan worden uitgevoerd op twee manieren:

  * Storm-Webgebruikersinterface
  * Opdrachtregelinterface (CLI)-hulpprogramma

    Raadpleeg de [Apache Storm documentatie](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.

    De Storm-webgebruikersinterface is beschikbaar op het HDInsight-cluster:

    ![HDInsight storm schaal opnieuw verdelen](./media/hdinsight-administer-use-powershell/hdinsight.portal.scale.cluster.png)

    Hier volgt een voorbeeld van hoe u de CLI-opdracht opnieuw verdelen van de Storm-topologie:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Als u wilt de grootte van de Hadoop-cluster wijzigen met behulp van Azure PowerShell, voer de volgende opdracht uit vanaf een clientcomputer:

```powershell
Set-AzHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```


## <a name="grantrevoke-access"></a>Toegang verlenen of in te trekken
HDInsight-clusters hebben de volgende HTTP-webservices (al deze services hebben RESTful-eindpunten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Standaard worden deze services worden verleend om toegang te krijgen. U kunt in te trekken/verlenen toegang tot de. Om in te trekken:

```powershell
Revoke-AzHDInsightHttpServicesAccess -ClusterName <Cluster Name>
```

Om toegang te verlenen:

```powershell
$clusterName = "<HDInsight Cluster Name>"

# Credential option 1
$hadoopUserName = "admin"
$hadoopUserPassword = "<Enter the Password>"
$hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
$credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

# Credential option 2
#$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

Grant-AzHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential
```

> [!NOTE]  
> U herstellen door de toegang verlenen/intrekken, de cluster-gebruikersnaam en wachtwoord.

Verlening en het intrekken van toegang kunnen ook worden gedaan via de portal. Zie [beheren Apache Hadoop-clusters in HDInsight met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md).

## <a name="update-http-user-credentials"></a>HTTP-gebruikersreferenties bijwerken
Dit is dezelfde procedure als Grant/revoke HTTP-toegang. Als het cluster heeft de HTTP-toegang is verleend, moet u eerst het intrekken.  En hen vervolgens machtigen de toegang met nieuwe HTTP-gebruikersreferenties.

## <a name="find-the-default-storage-account"></a>Het standaardopslagaccount vinden
De volgende PowerShell-script laat zien hoe u de standaardnaam van de storage-account en de verwante informatie:

> [!IMPORTANT]  
> De waarden voor `DefaultStorageAccount`, en `DefaultStorageContainer` worden niet geretourneerd van [Get-AzHDInsightCluster](https://docs.microsoft.com/powershell/module/az.hdinsight/get-azhdinsightcluster) wanneer [veilige overdracht](../storage/common/storage-require-secure-transfer.md) is ingeschakeld op het storage-account.


```powershell
#Connect-AzAccount
$clusterName = "<HDInsight Cluster Name>"

$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

echo "Default Storage account name: $defaultStorageName"
echo "Default Storage account type: $defaultStoreageType"

if ($defaultStoreageType -eq "blob")
{
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

    echo "Default Blob container name: $defaultBlobContainerName"
    echo "Default Storage account key: $defaultStorageAccountKey"
}
```


## <a name="find-the-resource-group"></a>Zoek de resourcegroep
In de modus Resource Manager wordt elke HDInsight-cluster hoort bij een Azure-resourcegroep.  Zoek de resourcegroep:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```


## <a name="submit-jobs"></a>Verzenden van taken
**MapReduce-taken indienen**

Zie [uitvoeren van de MapReduce-voorbeelden opgenomen in HDInsight](hadoop/apache-hadoop-run-samples-linux.md).

**Apache Hive-taken indienen**

Zie [uitvoeren Apache Hive-query's met behulp van PowerShell](hadoop/apache-hadoop-use-hive-powershell.md).

**Apache Pig-taken indienen**

Zie [uitvoeren Apache Pig-taken met behulp van PowerShell](hadoop/apache-hadoop-use-pig-powershell.md).

**Apache Sqoop taken indienen**

Zie [Apache Sqoop gebruiken met HDInsight](hadoop/hdinsight-use-sqoop.md).

**Apache Oozie-taken indienen**

Zie [Apache Oozie gebruiken met Apache Hadoop voor het definiëren en een werkstroom uitvoeren in HDInsight](hdinsight-use-oozie-linux-mac.md).

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure Blob-opslag

Zie [gegevens uploaden naar HDInsight](hdinsight-upload-data.md).

## <a name="see-also"></a>Zie ook

* [HDInsight-cmdlet naslagdocumentatie](https://msdn.microsoft.com/library/azure/dn479228.aspx)
* [Apache Hadoop-clusters in HDInsight beheren met behulp van de Azure-portal](hdinsight-administer-use-portal-linux.md)
* [HDInsight met een opdrachtregelinterface beheren](hdinsight-administer-use-command-line.md)
* [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md)
* [Programmatisch verzenden van Apache Hadoop-taken](hadoop/submit-apache-hadoop-jobs-programmatically.md)
* [Aan de slag met Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md)
