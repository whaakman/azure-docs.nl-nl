---
title: Hadoop-cluster maken met opslagaccounts voor veilige overdracht in Azure HDInsight
description: Informatie over het maken van HDInsight-clusters met Azure-opslag-accounts voor veilige overdracht.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 07/24/2018
ms.openlocfilehash: 5ad49c8e28f0e91b18af6dc485489d11464fbc0d
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53413384"
---
# <a name="create-apache-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Apache Hadoop-cluster maken met veilige overdracht storage-accounts in Azure HDInsight

De functie [Veilige overdracht vereist](../storage/common/storage-require-secure-transfer.md) verhoogt de beveiliging van uw Azure-opslagaccount door alle aanvragen naar uw account af te dwingen via een beveiligde verbinding. Deze functie en het wasbs-schema worden alleen ondersteund door HDInsight-clusterversie 3.6 of nieuwer.

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u over de volgende onderdelen beschikken:

* **Azure-abonnement**: Voor het maken van een gratis proefaccount van één maand, blader naar [azure.microsoft.com/free](https://azure.microsoft.com/free).
* **Een Azure-opslagaccount met veilige overdracht**. Zie [Een opslagaccount maken](../storage/common/storage-quickstart-create-account.md) en [Veilige overdracht vereisen](../storage/common/storage-require-secure-transfer.md) voor instructies.
* **Een Blob-container in het opslagaccount**.

## <a name="create-cluster"></a>Cluster maken

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


In deze sectie maakt u een Hadoop-cluster in HDInsight met behulp van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md). De sjabloon bevindt zich in [GitHub](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-existing-default-storage-account/). Het maken van een Azure Resource Manager-sjabloon is niet vereist voor deze zelfstudie. Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor andere methoden om clusters te maken en inzicht te krijgen in de eigenschappen die worden gebruikt in deze zelfstudie.

1. Klik op de volgende afbeelding om u aan te melden bij Azure en de Resource Manager-sjabloon in Azure Portal te openen.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-existing-default-storage-account%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Volg de instructies voor het maken van het cluster met de volgende specificaties: 

    - Geef HDInsight versie 3.6 op. Versie 3.6 of hoger is vereist.
    - Geef een opslagaccount met veilige overdracht op.
    - Gebruik de korte naam voor het opslagaccount.
    - Zowel het opslagaccount als de blob-container moeten vooraf zijn gemaakt.

      Zie [Cluster maken](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) voor instructies.

Als u een scriptactie gebruikt om uw eigen configuratiebestanden te verstrekken, moet u wasbs gebruiken in de volgende instellingen:

- fs.defaultFS (core-site)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Extra opslagaccounts toevoegen

Er zijn verschillende opties om extra opslagaccounts met veilige overdracht toe te voegen:

- De Azure Resource Manager-sjabloon in de laatste sectie wijzigen.
- Een cluster maken met [Azure Portal](https://portal.azure.com) en een gekoppelde opslagaccount opgeven.
- Een scriptactie gebruiken om extra opslagaccounts met veilige overdracht toe te voegen aan een bestaand HDInsight-cluster. Zie [Extra opslagaccounts toevoegen aan HDInsight](hdinsight-hadoop-add-storage.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe een HDInsight-cluster kunt maken en veilige overdracht kunt inschakelen voor de opslagaccounts.

Zie de volgende artikelen voor meer informatie over het analyseren van gegevens met HDInsight:

* Voor meer informatie over het gebruik van [Apache Hive](https://hive.apache.org/) met HDInsight, waaronder over het uitvoeren van Hive-query's vanuit Visual Studio, Zie [Apache Hive gebruiken met HDInsight][hdinsight-use-hive].
* Voor meer informatie over [Apache Pig](https://pig.apache.org/), een taal die wordt gebruikt voor het transformeren van gegevens, Zie [Apache Pig gebruiken met HDInsight][hdinsight-use-pig].
* Voor meer informatie over [Apache Hadoop MapReduce](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html), een manier om programma's schrijven die gegevens op Hadoop verwerken, Zie [gebruik Apache Hadoop MapReduce met HDInsight][hdinsight-use-mapreduce].
* Zie voor meer informatie over het gebruik van de HDInsight Tools voor Visual Studio om gegevens op HDInsight te analyseren, [aan de slag met Visual Studio Apache Hadoop-hulpprogramma's voor HDInsight](hadoop/apache-hadoop-visual-studio-tools-get-started.md).

Zie de volgende artikelen voor meer informatie over hoe HDInsight gegevens opslaat en over het ophalen van gegevens in HDInsight:

* Zie [Azure Storage gebruiken met HDInsight](hdinsight-hadoop-use-blob-storage.md) voor meer informatie over hoe HDInsight Azure Storage gebruikt.
* Zie [Gegevens uploaden naar HDInsight][hdinsight-upload-data] voor meer informatie over het uploaden van gegevens naar HDInsight.

Zie de volgende artikelen voor meer informatie over het maken of beheren van een HDInsight-cluster:

* Zie voor meer informatie over het beheren van uw HDInsight-cluster op basis van Linux [HDInsight-clusters beheren met Ambari](hdinsight-hadoop-manage-ambari.md).
* Zie voor meer informatie over de opties die u kunt selecteren bij het maken van een HDInsight-cluster [HDInsight op Linux maken met aangepaste opties](hdinsight-hadoop-provision-linux-clusters.md).
* Als u bekend met Linux- en Apache Hadoop bent, maar u meer details over Hadoop op de HDInsight wilt weten, Zie [werken met HDInsight op Linux](hdinsight-hadoop-linux-information.md). Dit artikel biedt informatie zoals:

  * URL's voor services die worden gehost op het cluster, zoals [Apache Ambari](https://ambari.apache.org/) en [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat)
  * De locatie van [Apache Hadoop](https://hadoop.apache.org/) bestanden en voorbeelden op het lokale bestandssysteem.
  * Het gebruik van Azure Storage (WASB) in plaats van [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) als de standaard opslaan

[1]: ../HDInsight/hadoop/apache-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]:hadoop/hdinsight-use-mapreduce.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
