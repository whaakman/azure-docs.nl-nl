---
title: Apache Hadoop-clusters in HDInsight met PowerShell - Azure beheren
description: Informatie over het uitvoeren van beheertaken voor de Apache Hadoop-clusters in HDInsight met behulp van Azure PowerShell.
services: hdinsight
ms.reviewer: tyfox
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: tylerfox
ms.openlocfilehash: 8454a22b02f28ece45ce04464f8f2658f745b53e
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163341"
---
# <a name="manage-apache-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Apache Hadoop-clusters in HDInsight beheren met behulp van Azure PowerShell
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell kan worden gebruikt om te beheren en automatiseren van de implementatie en het beheer van uw workloads in Azure. In dit artikel leert u hoe u voor het beheren van [Apache Hadoop](https://hadoop.apache.org/) -clusters in Azure HDInsight met behulp van Azure PowerShell. Zie voor een lijst van de HDInsight PowerShell-cmdlets, [HDInsight-cmdlet-verwijzing](https://msdn.microsoft.com/library/azure/dn479228.aspx).

**Vereisten**

Voordat u dit artikel, hebt u de volgende items:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Azure PowerShell installeren
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Als u Azure PowerShell versie 0.9 hebt geïnstalleerd x, moet u deze verwijderen voordat u een nieuwere versie installeert.

Om te controleren of de versie van de geïnstalleerd PowerShell:

```powershell
Get-Module *azure*
```

Voer voor het verwijderen van de oudere versie, programma's en onderdelen in het Configuratiescherm.

## <a name="create-clusters"></a>Clusters maken
Zie [maken Linux gebaseerde clusters in HDInsight met behulp van Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Clusters groeperen
Gebruik de volgende opdracht om alle clusters in het huidige abonnement weer te geven:

```powershell
Get-AzureRmHDInsightCluster
```

## <a name="show-cluster"></a>Cluster weergeven
Gebruik de volgende opdracht om de details van een specifiek cluster in het huidige abonnement weer te geven:

```powershell
Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Clusters verwijderen
Gebruik de volgende opdracht om een cluster te verwijderen:

```powershell
Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

U kunt ook een cluster verwijderen door het verwijderen van de resourcegroep waarin het cluster. Een resourcegroep verwijdert, worden alle resources in de groep met inbegrip van het standaardopslagaccount.

```powershell
Remove-AzureRmResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Clusters schalen
Het cluster schalen functie kunt u het aantal worker-knooppunten die worden gebruikt door een cluster dat wordt uitgevoerd in Azure HDInsight zonder te hoeven maken van het cluster opnieuw wijzigen.

> [!NOTE]
> Alleen clusters met HDInsight versie 3.1.3 of hoger worden ondersteund. Als u de versie van het cluster niet zeker weet, kunt u de eigenschappenpagina controleren.  Zie [clusters tonen en vermelden](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
>
>

De gevolgen van het wijzigen van het aantal gegevensknooppunten voor elk type cluster die door HDInsight worden ondersteund:

* Apache Hadoop

    Het aantal worker-knooppunten in een Hadoop-cluster dat wordt uitgevoerd zonder gevolgen voor alle taken die in behandeling of wordt uitgevoerd, kunt u naadloos verhogen. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking uitgevoerd wordt. Fouten in een bewerking voor vergroten/verkleinen probleemloos verwerkt zodat het cluster altijd in een functionele staat blijft.

    Wanneer een Hadoop-cluster is omlaag geschaald door het aantal gegevensknooppunten te verminderen, zijn sommige van de services in het cluster opnieuw opgestart. Opnieuw starten van services zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken na het voltooien van de bewerking vergroten/verkleinen. U kunt echter de taken opnieuw zodra de bewerking voltooid is.
* Apache HBase

    U kunt naadloos toevoegen of verwijderen van knooppunten in uw HBase-cluster, terwijl deze wordt uitgevoerd. Regionale Servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de vergroten/verkleinen bewerking. U kunt echter ook handmatig de regionale servers in evenwicht door te melden bij het hoofdknooppunt van het cluster en voer de volgende opdrachten vanuit een opdrachtpromptvenster:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

* Apache Storm

    U kunt naadloos toevoegen of verwijderen van gegevensknooppunten naar uw Storm-cluster, terwijl deze wordt uitgevoerd. Maar na een geslaagde bewerking vergroten/verkleinen is voltooid, moet u opnieuw verdelen van de topologie.

    Herverdelen kan worden uitgevoerd op twee manieren:

  * Storm-Webgebruikersinterface
  * Opdrachtregelinterface (CLI)-hulpprogramma

    Raadpleeg de [Apache Storm documentatie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.

    De Storm-webgebruikersinterface is beschikbaar op het HDInsight-cluster:

    ![HDInsight storm schaal opnieuw verdelen](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Hier volgt een voorbeeld van hoe u de CLI-opdracht opnieuw verdelen van de Storm-topologie:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Als u wilt de grootte van de Hadoop-cluster wijzigen met behulp van Azure PowerShell, voer de volgende opdracht uit vanaf een clientcomputer:

```powershell
Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
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
Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>
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

Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential
```

> [!NOTE]
> U herstellen door de toegang verlenen/intrekken, de cluster-gebruikersnaam en wachtwoord.
>
>

Verlening en het intrekken van toegang kunnen ook worden gedaan via de portal. Zie [HDInsight beheren met behulp van de Azure-portal][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>HTTP-gebruikersreferenties bijwerken
Dit is dezelfde procedure als [Grant/revoke HTTP toegang](#grant/revoke-access). Als het cluster heeft de HTTP-toegang is verleend, moet u eerst het intrekken.  En hen vervolgens machtigen de toegang met nieuwe HTTP-gebruikersreferenties.

## <a name="find-the-default-storage-account"></a>Het standaardopslagaccount vinden
De volgende PowerShell-script laat zien hoe u de standaardnaam van de storage-account en de verwante informatie:

```powershell
#Connect-AzureRmAccount
$clusterName = "<HDInsight Cluster Name>"

$clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$storageInfo = $clusterInfo.DefaultStorageAccount.split('.')
$defaultStoreageType = $storageInfo[1]
$defaultStorageName = $storageInfo[0]

echo "Default Storage account name: $defaultStorageName"
echo "Default Storage account type: $defaultStoreageType"

if ($defaultStoreageType -eq "blob")
{
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

    echo "Default Blob container name: $defaultBlobContainerName"
    echo "Default Storage account key: $defaultStorageAccountKey"
}
```


## <a name="find-the-resource-group"></a>Zoek de resourcegroep
In de modus Resource Manager wordt elke HDInsight-cluster hoort bij een Azure-resourcegroep.  Zoek de resourcegroep:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```


## <a name="submit-jobs"></a>Verzenden van taken
**Apache Hadoop MapReduce-taken indienen**

Zie [uitvoeren van de Apache Hadoop MapReduce-voorbeelden opgenomen in HDInsight](hadoop/apache-hadoop-run-samples-linux.md).

**Apache Hive-taken indienen**

Zie [uitvoeren Apache Hive-query's met behulp van PowerShell](hadoop/apache-hadoop-use-hive-powershell.md).

**Apache Pig-taken indienen**

Zie [uitvoeren Apache Pig-taken met behulp van PowerShell](hadoop/apache-hadoop-use-pig-powershell.md).

**Apache Sqoop taken indienen**

Zie [Apache Sqoop gebruiken met HDInsight](hadoop/hdinsight-use-sqoop.md).

**Apache Oozie-taken indienen**

Zie [Apache Oozie gebruiken met Apache Hadoop voor het definiëren en een werkstroom uitvoeren in HDInsight](hdinsight-use-oozie.md).

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure Blob-opslag
Zie [Gegevens uploaden naar HDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Zie ook
* [HDInsight-cmdlet naslagdocumentatie](https://msdn.microsoft.com/library/azure/dn479228.aspx)
* [HDInsight beheren met behulp van Azure portal][hdinsight-admin-portal]
* [HDInsight met een opdrachtregelinterface beheren][hdinsight-admin-cli]
* [HDInsight-clusters maken][hdinsight-provision]
* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Programmatisch verzenden van Apache Hadoop-taken][hdinsight-submit-jobs]
* [Aan de slag met Azure HDInsight][hdinsight-get-started]

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]:hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]:hadoop/submit-apache-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
