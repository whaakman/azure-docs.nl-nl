---
title: Apache Hadoop-clusters met behulp van PowerShell - Azure HDInsight maken
description: Leer hoe u Apache Hadoop, Apache HBase, Apache Storm of Apache Spark-clusters maken in Linux voor HDInsight met behulp van Azure PowerShell.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: hrasheed
ms.openlocfilehash: 76bef5fcae8e95a52b7a35fdbb657db5dde01bc3
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51035183"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Op basis van Linux-clusters maken in HDInsight met behulp van Azure PowerShell

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Azure PowerShell is een krachtige scripting omgeving die u gebruiken kunt om te beheren en automatiseren van de implementatie en het beheer van uw workloads in Microsoft Azure. Dit document bevat informatie over het maken van een Linux gebaseerde HDInsight-cluster met behulp van Azure PowerShell. Dit omvat ook een voorbeeldscript.

> [!NOTE]
> Azure PowerShell is alleen beschikbaar op Windows-clients. Als u van een Linux-, Unix- of Mac OS X-client gebruikmaakt, Zie [maken van een Linux gebaseerde HDInsight-cluster met behulp van de klassieke Azure-CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md) voor informatie over het gebruik van de klassieke CLI om een cluster te maken.

## <a name="prerequisites"></a>Vereisten
U kunt het volgende voordat u begint met deze procedure moet zijn:

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Azure PowerShell](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > Azure PowerShell-ondersteuning voor het beheer van HDInsight-resources met behulp van Azure Service Manager is **afgeschaft** en per 1 januari 2017 verdwenen. In de stappen in dit document worden de nieuwe HDInsight-cmdlets gebruikt die met Azure Resource Manager werken.
    >
    > Volg de stappen in [Azure PowerShell installeren](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) voor het installeren van de meest recente versie van Azure PowerShell. Als u scripts hebt die moeten worden gewijzigd om ze te kunnen gebruiken met de nieuwe cmdlets die werken met Azure Resource Manager, raadpleegt u [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Migreren naar op Azure Resource Manager gebaseerde hulpprogramma’s voor HDInsight-clusters) voor meer informatie.

## <a name="create-cluster"></a>Cluster maken

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Voor het maken van een HDInsight-cluster met behulp van Azure PowerShell, moet u de volgende procedures uitvoeren:

* Een Azure-resourcegroep maken
* Een Azure Storage-account maken
* Een Azure Blob-container maken
* Een HDInsight-cluster maken

Het volgende script laat zien hoe u een nieuw cluster maken:

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

De waarden die u voor de aanmelding bij cluster opgeeft worden gebruikt om de Hadoop-gebruikersaccount voor het cluster te maken. Gebruik dit account verbinding maken met services die worden gehost op het cluster, zoals web-UI's of REST-API's.

De waarden die u voor de SSH-gebruiker opgeeft worden gebruikt om te maken van de SSH-gebruiker voor het cluster. Dit account gebruiken voor een externe SSH-sessie starten op het cluster en taken uitvoeren. Zie het document [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

> [!IMPORTANT]
> Als u van plan bent gebruik van meer dan 32 worker-knooppunten (op bij het maken van clusters of door te schalen van het cluster na het maken van), moet u ook een hoofdknooppunt grootte opgeven met ten minste 8 kerngeheugens en 14 GB aan RAM-geheugen.
>
> Zie [Prijsdetails voor Azure HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) voor meer informatie over knooppuntgrootten en de bijbehorende kosten.

Het kan duren voordat tot 20 minuten om een cluster te maken.

## <a name="create-cluster-configuration-object"></a>Cluster maken: Configuration-object

U kunt ook maken een gebruiken voor het object HDInsight configuratie `New-AzureRmHDInsightClusterConfig` cmdlet. U kunt dit configuratieobject zodat extra configuratieopties voor uw cluster wijzigen. Gebruik tot slot de `-Config` parameter van de `New-AzureRmHDInsightCluster` cmdlet de configuratie te gebruiken.

Het volgende script maakt u een configuratieobject voor het configureren van een R Server op HDInsight-clustertype. De configuratie maakt het mogelijk een edge-knooppunt, RStudio en een extra opslagaccount.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> Met behulp van een storage-account in een andere locatie dan het HDInsight-cluster wordt niet ondersteund. Wanneer u in dit voorbeeld gebruikt, moet u de extra storage-account maken in dezelfde locatie als de server.

## <a name="customize-clusters"></a>Clusters aanpassen

* Zie [aanpassen HDInsight-clusters met Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* Zie [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een HDInsight-cluster hebt gemaakt, gebruikt u de volgende bronnen voor informatie over het werken met uw cluster.

### <a name="hadoop-clusters"></a>Hadoop-clusters

* [Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-clusters

* [Aan de slag met HBase in HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Java-toepassingen voor HBase op HDInsight ontwikkelen](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-clusters

* [Java-topologieën ontwikkelen voor Storm op HDInsight](storm/apache-storm-develop-java-topology.md)
* [Python-onderdelen in Storm op HDInsight gebruiken](storm/apache-storm-develop-python-topology.md)
* [Topologieën met Storm op HDInsight implementeren en bewaken](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark-clusters

* [Een zelfstandige toepassing maken met behulp van Scala](spark/apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](spark/apache-spark-livy-rest-interface.md)
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](spark/apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)

