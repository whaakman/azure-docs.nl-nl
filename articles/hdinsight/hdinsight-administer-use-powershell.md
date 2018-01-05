---
title: Hadoop-clusters in HDInsight met PowerShell - Azure beheren | Microsoft Docs
description: Informatie over het uitvoeren van beheertaken voor het Hadoop-clusters in HDInsight met behulp van Azure PowerShell.
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: bfdfa754-18e5-4ef9-b0d6-2dbdcebc0283
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: jgao
ms.openlocfilehash: 04bb16d4779741d71f686f53a8f325243ab8499e
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Hadoop-clusters in HDInsight met behulp van Azure PowerShell beheren
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Azure PowerShell kan worden gebruikt voor de implementatie en beheer van uw workloads in Azure te automatiseren. In dit artikel leert u hoe in Azure HDInsight Hadoop-clusters beheren met behulp van Azure PowerShell. Zie voor een overzicht van de HDInsight-PowerShell-cmdlets [HDInsight cmdlet-verwijzing][hdinsight-powershell-reference].

**Vereisten**

Voordat u dit artikel, hebt u de volgende items:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Azure PowerShell installeren
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Als u Azure PowerShell versie 0,9 hebt geïnstalleerd x, moet u deze verwijderen voordat u een nieuwere versie installeert.

De versie van de geïnstalleerde PowerShell controleren:

```powershell
Get-Module *azure*
```

Voer voor het verwijderen van de oudere versie, programma's en onderdelen in het Configuratiescherm.

## <a name="create-clusters"></a>Clusters maken
Zie [maken Linux gebaseerde clusters in HDInsight met behulp van Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Lijst met clusters
Gebruik de volgende opdracht voor een lijst met alle clusters in het huidige abonnement:

```powershell
Get-AzureRmHDInsightCluster
```

## <a name="show-cluster"></a>Cluster weergeven
Gebruik de volgende opdracht om details van een specifiek cluster in het huidige abonnement weer te geven:

```powershell
Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

## <a name="delete-clusters"></a>Clusters verwijderen
Gebruik de volgende opdracht om te verwijderen van een cluster:

```powershell
Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>
```

U kunt ook een cluster verwijderen door het verwijderen van de resourcegroep waarin het cluster. Een resourcegroep verwijdert, worden alle resources in de groep met inbegrip van het standaardopslagaccount.

```powershell
Remove-AzureRmResourceGroup -Name <Resource Group Name>
```

## <a name="scale-clusters"></a>Clusters schalen
Het schalen van de functie cluster kunt u het aantal worker-knooppunten gebruikt door een cluster dat wordt uitgevoerd in Azure HDInsight zonder te hoeven maken van het cluster opnieuw wijzigen.

> [!NOTE]
> Alleen clusters met HDInsight versie 3.1.3 of hoger worden ondersteund. Als u de versie van het cluster niet zeker weet, kunt u de pagina eigenschappen controleren.  Zie [lijst en geeft weer clusters](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
>
>

De gevolgen van het wijzigen van het aantal gegevensknooppunten voor elk type van ondersteund door de HDInsight-cluster:

* Hadoop

    U kunt naadloos Verhoog het aantal worker-knooppunten van een Hadoop-cluster dat wordt uitgevoerd zonder enige impact op alle taken die in behandeling of wordt uitgevoerd. Nieuwe taken kunnen ook worden verzonden terwijl de bewerking uitgevoerd wordt. Fouten in een bewerking voor schaal worden probleemloos verwerkt zodat het cluster altijd in een functionele staat blijft.

    Wanneer een Hadoop-cluster wordt verkleind door het aantal gegevensknooppunten te verminderen, zijn sommige van de services in het cluster opnieuw gestart. Opnieuw starten van services zorgt ervoor dat alle actieve en in behandeling zijnde taken mislukken na het voltooien van de bewerking uit te schalen. U kunt echter de taken verzenden zodra de bewerking voltooid is.
* HBase

    U kunt naadloos toevoegen of verwijderen van knooppunten in uw HBase-cluster, terwijl deze wordt uitgevoerd. Regionale Servers worden automatisch verdeeld binnen een paar minuten na voltooiing van de bewerking uit te schalen. U kunt echter ook handmatig een balans vinden tussen de regionale servers door de logboekregistratie in de headnode van cluster, en voer vervolgens de volgende opdrachten vanuit een opdrachtpromptvenster:

    ```bash
    >pushd %HBASE_HOME%\bin
    >hbase shell
    >balancer
    ```

* Storm

    U kunt naadloos toevoegen of verwijderen van gegevensknooppunten naar uw Storm-cluster, terwijl deze wordt uitgevoerd. Maar na een geslaagde voltooiing van de bewerking uit te schalen, moet u de topologie opnieuw verdelen.

    Herverdeling kan worden uitgevoerd op twee manieren:

  * Storm-webgebruikersinterface
  * Hulpprogramma voor opdrachtregelinterface (CLI)

    Raadpleeg de [Apache Storm documentatie](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) voor meer informatie.

    De Storm-webgebruikersinterface is beschikbaar op het HDInsight-cluster:

    ![HDInsight storm scale deel opnieuw](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Hier volgt een voorbeeld van hoe u met de opdracht CLI opnieuw verdelen van de Storm-topologie:

    ```cli
    ## Reconfigure the topology "mytopology" to use 5 worker processes,
    ## the spout "blue-spout" to use 3 executors, and
    ## the bolt "yellow-bolt" to use 10 executors
    $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10
    ```

Als u wilt de grootte van Hadoop-cluster wijzigen met behulp van Azure PowerShell, voer de volgende opdracht vanaf een clientcomputer:

```powershell
Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>
```


## <a name="grantrevoke-access"></a>Toegang verlenen of in te trekken
HDInsight-clusters hebben de volgende HTTP-webservices (al deze services hebt RESTful eindpunten):

* ODBC
* JDBC
* Ambari
* Oozie
* Templeton

Standaard worden deze services verleend om toegang te krijgen. U kunt in te trekken/verlenen toegang. Voor het intrekken van:

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
> Opnieuw door de toegang verlenen/intrekken, de gebruikersnaam van de cluster en het wachtwoord instellen.
>
>

Verlenen en het intrekken van toegang kunnen ook worden gedaan via de portal. Zie [HDInsight beheren met behulp van de Azure-portal][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>HTTP-gebruikersreferenties updaten
Dit is dezelfde procedure als [Grant/revoke HTTP toegang](#grant/revoke-access). Als het cluster heeft de HTTP-toegang is verleend, moet u het eerst intrekken.  Vervolgens toegang verlenen en de met nieuwe HTTP gebruikersgegevens.

## <a name="find-the-default-storage-account"></a>Het standaardopslagaccount vinden
De volgende PowerShell-script laat zien hoe u de naam van het standaardopslagaccount en de verwante informatie ophalen:

```powershell
#Login-AzureRmAccount
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


## <a name="find-the-resource-group"></a>De resourcegroep niet vinden
In de modus Resource Manager behoort elk HDInsight-cluster tot een Azure-resourcegroep.  Zoek de resourcegroep:

```powershell
$clusterName = "<HDInsight Cluster Name>"

$cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
$resourceGroupName = $cluster.ResourceGroup
```


## <a name="submit-jobs"></a>Verzenden van taken
**MapReduce-taken verzenden**

Zie [uitvoeren Hadoop-MapReduce-voorbeelden in HDInsight op basis van Windows](hdinsight-run-samples.md).

**Hive-taken verzenden**

Zie [uitvoeren Hive-query's met behulp van PowerShell](hadoop/apache-hadoop-use-hive-powershell.md).

**Pig-taken verzenden**

Zie [uitvoeren Pig-taken met behulp van PowerShell](hadoop/apache-hadoop-use-pig-powershell.md).

**Om Sqoop taken te verzenden**

Zie [Sqoop gebruiken met HDInsight](hadoop/hdinsight-use-sqoop.md).

**Oozie-taken verzenden**

Zie [Oozie gebruiken met Hadoop om te definiëren en uitvoeren van een werkstroom in HDInsight](hdinsight-use-oozie.md).

## <a name="upload-data-to-azure-blob-storage"></a>Gegevens uploaden naar Azure Blob-opslag
Zie [Gegevens uploaden naar HDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Zie ook
* [HDInsight-cmdlet-naslagdocumentatie][hdinsight-powershell-reference]
* [HDInsight beheren met behulp van de Azure-portal][hdinsight-admin-portal]
* [HDInsight met behulp van een opdrachtregelinterface beheren][hdinsight-admin-cli]
* [HDInsight-clusters maken][hdinsight-provision]
* [Gegevens uploaden naar HDInsight][hdinsight-upload-data]
* [Hadoop-taken programmatisch verzenden][hdinsight-submit-jobs]
* [Aan de slag met Azure HDInsight][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

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
