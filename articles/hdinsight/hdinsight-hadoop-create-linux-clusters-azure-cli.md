---
title: Apache Hadoop-clusters met behulp van de Azure CLI voor klassiek - Azure HDInsight maken
description: Leer hoe u HDInsight-clusters met behulp van de platformoverschrijdende CLI van Azure classic maken.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: hrasheed
ms.openlocfilehash: 93ee51c8a00e5cfcbffd56f96b627b68dd124aea
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51034690"
---
# <a name="create-hdinsight-clusters-using-the-azure-classic-cli"></a>Maken van HDInsight-clusters met behulp van de klassieke Azure-CLI

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

De stappen in dit document overzicht maken van een HDInsight 3.5-cluster met behulp van de klassieke Azure-CLI.

[!INCLUDE [classic-cli-warning](../../includes/requires-classic-cli.md)]

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure CLI voor klassieke**. De stappen in dit document zijn laatste getest met de klassieke Azure-CLI versie 0.10.14.

## <a name="log-in-to-your-azure-subscription"></a>Aanmelden bij uw Azure-abonnement

Volg de stappen beschreven in [verbinding maken met een Azure-abonnement met de Azure-opdrachtregelinterface](/cli/azure/authenticate-azure-cli) en maak verbinding met uw abonnement met de **aanmelding** methode.

## <a name="create-a-cluster"></a>Een cluster maken

De volgende stappen moeten worden uitgevoerd vanaf de opdrachtregel, zoals PowerShell of Bash.

1. Gebruik de volgende opdracht om te verifiëren bij uw Azure-abonnement:

        azure login

    U wordt gevraagd uw naam en wachtwoord opgeven. Als u meerdere Azure-abonnementen hebt, gebruikt u `azure account set <subscriptionname>` om in te stellen van het abonnement dat de klassieke CLI-opdrachten gebruiken.

2. Schakel over naar modus Azure Resource Manager met de volgende opdracht:

        azure config mode arm

3. Maak een resourcegroep. Deze resourcegroep bevat het HDInsight-cluster en storage-account dat is gekoppeld.

        azure group create groupname location

    * Vervang `groupname` met een unieke naam voor de groep.

    * Vervang `location` met de geografische regio die u wilt maken van de groep in.

       Voor een lijst van geldige locaties, gebruikt u de `azure location list` opdracht in en gebruik een van de locaties van de `Name` kolom.

4. Een opslagaccount maken. Dit opslagaccount wordt gebruikt als de standaardopslag voor het HDInsight-cluster.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * Vervang `groupname` met de naam van de groep in de vorige stap hebt gemaakt.

    * Vervang `location` met de dezelfde locatie die wordt gebruikt in de vorige stap.

    * Vervang `storagename` met een unieke naam voor het opslagaccount.

        > [!NOTE]
        > Voor meer informatie over de parameters die in deze opdracht wordt gebruikt, gebruikt u `azure storage account create -h` om help voor deze opdracht weer te geven.

5. De sleutel die wordt gebruikt voor toegang tot het opslagaccount ophalen.

        azure storage account keys list -g groupname storagename

    * Vervang `groupname` met de naam van de resourcegroep.
    * Vervang `storagename` met de naam van het opslagaccount.

     In de gegevens die wordt geretourneerd, sla de `key` waarde voor de `key1`.

6. Een HDInsight-cluster maken.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Vervang `groupname` met de naam van de resourcegroep.

    * Vervang `Hadoop` met het clustertype dat u wilt maken. Bijvoorbeeld, `Hadoop`, `HBase`, `Kafka`, `Spark`, of `Storm`.

     > [!IMPORTANT]
     > HDInsight clusters worden geleverd in verschillende typen die overeenkomen met de werkbelasting of technologie die het cluster is afgestemd op. Er is geen ondersteunde methode om een cluster die meerdere typen, zoals Storm en HBase op één cluster combineert te maken.

    * Vervang `location` met dezelfde locatie in de vorige stappen gebruikt.

    * Vervang `storagename` met de naam van het opslagaccount.

    * Vervang `storagekey` met de sleutel in de vorige stap hebt verkregen.

    * Voor de `--defaultStorageContainer` parameter, gebruik dezelfde naam als u gebruikt voor het cluster.

    * Vervang `admin` en `httppassword` met de naam en het wachtwoord die u gebruiken wilt bij het openen van het cluster via HTTPS.

    * Vervang `sshuser` en `sshuserpassword` met de gebruikersnaam en het wachtwoord die u gebruiken wilt bij het openen van het cluster via SSH

    > [!IMPORTANT]
    > In dit voorbeeld wordt een cluster met twee worker-knooppunten. U kunt ook het aantal worker-knooppunten wijzigen nadat de cluster is gemaakt door het uitvoeren van vergroten / verkleinen. Als u van plan bent over het gebruik van meer dan 32 worker-knooppunten, moet u de grootte van een hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB RAM-geheugen selecteren. U kunt de grootte van het hoofdknooppunt instellen met behulp van de `--headNodeSize` parameter tijdens het maken van clusters.
    >
    > Zie [Prijsdetails voor Azure HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) voor meer informatie over knooppuntgrootten en de bijbehorende kosten.

    Het duurt enkele minuten voor het cluster maken van het proces is voltooid. Meestal ongeveer 15.

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een HDInsight-cluster met behulp van de klassieke CLI hebt gemaakt, gebruikt u de volgende voor informatie over het werken met uw cluster:

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
