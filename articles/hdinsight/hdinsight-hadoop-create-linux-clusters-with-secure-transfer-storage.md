---
title: Hadoop-cluster maken met opslagaccounts voor veilige overdracht in Azure HDInsight | Microsoft Docs
description: Informatie over het maken van HDInsight-clusters met Azure-opslag-accounts voor veilige overdracht.
keywords: aan de slag met hadoop, hadoop linux, hadoop Quick Start, veilige overdracht, azure opslagaccount
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/02/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: f89595c6721c00435e714368905ae48a542f8cb9
ms.contentlocale: nl-nl
ms.lasthandoff: 08/03/2017

---
# <a name="create-hadoop-cluster-with-secure-transfer-storage-accounts-in-azure-hdinsight"></a>Hadoop-cluster maken met opslagaccounts voor veilige overdracht in Azure HDInsight

De functie [Veilige overdracht vereist](../storage/storage-require-secure-transfer.md) verhoogt de beveiliging van uw Azure-opslagaccount door alle aanvragen naar uw account af te dwingen via een beveiligde verbinding. Deze functie en het wasbs-schema worden alleen ondersteund door HDInsight-clusterversie 3.6 of nieuwer. 

>[!NOTE] 
> Het maken van clusters met opslagaccounts voor veilige overdracht met .NET SDK wordt momenteel niet ondersteund. De tijdelijke oplossing is het instellen van 'wasbs' in de eigenschap 'fs.defaultFS' in de configuratie van de core-site als onderdeel van ClusterCreateParametersExtended.

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u over de volgende onderdelen beschikken:

* **Azure-abonnement**: voor het maken van een gratis proefaccount van één maand, bezoekt u [azure.microsoft.com/free](https://azure.microsoft.com/free).
* **Een Azure-opslagaccount met veilige overdracht**. Zie [Een opslagaccount maken](../storage/storage-create-storage-account.md#create-a-storage-account) en [Veilige overdracht vereisen](../storage/storage-require-secure-transfer.md) voor instructies.
* **Een Blob-container in het opslagaccount**. 
## <a name="create-cluster"></a>Cluster maken

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]


In deze sectie maakt u een Hadoop-cluster in HDInsight met behulp van een [Azure Resource Manager-sjabloon](../azure-resource-manager/resource-group-template-deploy.md). De sjabloon bevindt zich in een openbare [container](https://hditutorialdata.blob.core.windows.net/securetransfer/azuredeploy-new.json). Het maken van een Azure Resource Manager-sjabloon is niet vereist voor deze zelfstudie. Zie [HDInsight-clusters maken](hdinsight-hadoop-provision-linux-clusters.md) voor andere methoden om clusters te maken en inzicht te krijgen in de eigenschappen die worden gebruikt in deze zelfstudie.

1. Klik op de volgende afbeelding om u aan te melden bij Azure en de Resource Manager-sjabloon in Azure Portal te openen. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fsecuretransfer%2Fazuredeploy-new.json" target="_blank"><img src="./media/hdinsight-hadoop-linux-tutorial-get-started/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. Volg de instructies voor het maken van het cluster met de volgende specificaties: 

    - Geef HDInsight versie 3.6 op.  De standaardversie is 3.5. Versie 3.6 of hoger is vereist.
    - Geef een opslagaccount met veilige overdracht op.
    - Gebruik de korte naam voor het opslagaccount.
    - Zowel het opslagaccount als de blob-container moeten vooraf zijn gemaakt. 

    Zie [Cluster maken](./hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster) voor instructies. 

Als u een scriptactie gebruikt om uw eigen configuratiebestanden te verstrekken, moet u wasbs gebruiken in de volgende instellingen:

- fs.defaultFS (core-site)
- spark.eventLog.dir 
- spark.history.fs.logDirectory

## <a name="add-additional-storage-accounts"></a>Extra opslagaccounts toevoegen

Er zijn verschillende opties om extra opslagaccounts met veilige overdracht toe te voegen:

- De Azure Resource Manager-sjabloon in de laatste sectie wijzigen.
- Een cluster maken met [Azure Portal](https://portal.azure.com) en een gekoppelde opslagaccount opgeven.
- Een scriptactie gebruiken om extra opslagaccounts met veilige overdracht toe te voegen aan een bestaand HDInsight-cluster.  Zie [Extra opslagaccounts toevoegen aan HDInsight](hdinsight-hadoop-add-storage.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe een HDInsight-cluster kunt maken en veilige overdracht kunt inschakelen voor de opslagaccounts.

Zie de volgende artikelen voor meer informatie over het analyseren van gegevens met HDInsight:

* Zie [Hive gebruiken met HDInsight][hdinsight-use-hive] voor meer informatie over het gebruik van Hive met HDInsight, waaronder over het uitvoeren van Hive-query's vanuit Visual Studio.
* Zie [Pig gebruiken met HDInsight][hdinsight-use-pig] voor meer informatie over Pig, een taal die wordt gebruikt voor het omzetten van gegevens.
* Zie [MapReduce gebruiken met HDInsight][hdinsight-use-mapreduce] voor meer informatie over MapReduce, een middel om programma's te schrijven die gegevens verwerken op Hadoop.
* Zie voor meer informatie over het gebruik van de HDInsight-hulpprogramma's voor Visual Studio om gegevens op HDInsight te analyseren [Aan de slag met Visual Studio Hadoop-hulpprogramma's voor HDInsight](hdinsight-hadoop-visual-studio-tools-get-started.md).

Zie de volgende artikelen voor meer informatie over hoe HDInsight gegevens opslaat en over het ophalen van gegevens in HDInsight:

* Zie [Azure Storage gebruiken met HDInsight](hdinsight-hadoop-use-blob-storage.md) voor meer informatie over hoe HDInsight Azure Storage gebruikt.
* Zie [Gegevens uploaden naar HDInsight][hdinsight-upload-data] voor meer informatie over het uploaden van gegevens naar HDInsight.

Zie de volgende artikelen voor meer informatie over het maken of beheren van een HDInsight-cluster:

* Zie voor meer informatie over het beheren van uw HDInsight-cluster op basis van Linux [HDInsight-clusters beheren met Ambari](hdinsight-hadoop-manage-ambari.md).
* Zie voor meer informatie over de opties die u kunt selecteren bij het maken van een HDInsight-cluster [HDInsight op Linux maken met aangepaste opties](hdinsight-hadoop-provision-linux-clusters.md).
* Als u bekend bent met Linux en Hadoop, maar u meer details wilt weten over Hadoop op HDInsight, raadpleegt u [Werken met HDInsight op Linux](hdinsight-hadoop-linux-information.md). Dit artikel biedt informatie zoals:
  
  * URL's voor services die worden gehost op het cluster, zoals Ambari en WebHCat
  * De locatie van Hadoop-bestanden en voorbeelden op het lokale bestandssysteem
  * Het gebruik van Azure Storage (WASB) in plaats van HDFS als de standaard gegevensopslag

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md



