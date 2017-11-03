---
title: Met behulp van de opdrachtregel-Azure HDInsight Hadoop-clusters maken | Microsoft Docs
description: Informatie over het maken van HDInsight-clusters met behulp van de platformoverschrijdende Azure CLI 1.0.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 50b01483-455c-4d87-b754-2229005a8ab9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/26/2017
ms.author: larryfr
ms.openlocfilehash: 791edd4e56c7957458d49f6f3bd87b67e96db7a8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-hdinsight-clusters-using-the-azure-cli"></a>Maken van HDInsight-clusters met de Azure CLI

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

De stappen in dit document procedure voor het maken van een 3.5 HDInsight-cluster met behulp van de Azure CLI 1.0.

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.


## <a name="prerequisites"></a>Vereisten

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure CLI**. De stappen in dit document zijn laatste getest met Azure CLI versie 0.10.14.

    > [!IMPORTANT]
    > De stappen in dit document werken niet met Azure CLI 2.0. Azure CLI 2.0 biedt geen ondersteuning voor het maken van een HDInsight-cluster.

## <a name="log-in-to-your-azure-subscription"></a>Aanmelden bij uw Azure-abonnement

Volg de stappen die zijn beschreven in [Verbinding maken met een Azure-abonnement met de Azure-opdrachtregelinterface (Azure CLI)](../xplat-cli-connect.md) en maak verbinding met uw abonnement met behulp van de methode **login**.

## <a name="create-a-cluster"></a>Een cluster maken

De volgende stappen moeten worden uitgevoerd vanaf een opdrachtregel zoals PowerShell of Bash.

1. Gebruik de volgende opdracht om uw Azure-abonnement te verifiëren:

        azure login

    U wordt gevraagd uw naam en wachtwoord opgeven. Als u meerdere Azure-abonnementen hebt, gebruikt u `azure account set <subscriptionname>` instellen van het abonnement dat Azure CLI-opdrachten gebruiken.

2. Schakel over naar modus Azure Resource Manager met de volgende opdracht:

        azure config mode arm

3. Maak een resourcegroep. Deze resourcegroep bevat het HDInsight-cluster en storage-account gekoppeld.

        azure group create groupname location

    * Vervang `groupname` met een unieke naam voor de groep.

    * Vervang `location` met de geografische regio die u wilt maken van de groep in.

       Voor een lijst met geldige locaties, gebruikt u de `azure location list` opdracht en gebruik vervolgens een van de locaties van de `Name` kolom.

4. Een opslagaccount maken. Dit opslagaccount wordt gebruikt als de opslag van de standaard voor het HDInsight-cluster.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * Vervang `groupname` met de naam van de groep in de vorige stap hebt gemaakt.

    * Vervang `location` met dezelfde locatie die wordt gebruikt in de vorige stap.

    * Vervang `storagename` met een unieke naam voor het opslagaccount.

        > [!NOTE]
        > Gebruik voor meer informatie over de parameters in deze opdracht gebruikt `azure storage account create -h` om help voor deze opdracht te geven.

5. De sleutel die wordt gebruikt voor toegang tot het opslagaccount ophalen.

        azure storage account keys list -g groupname storagename

    * Vervang `groupname` met de naam van de resourcegroep.
    * Vervang `storagename` met de naam van het opslagaccount.

     Opslaan in de gegevens die wordt geretourneerd, de `key` waarde voor `key1`.

6. Maak een HDInsight-cluster.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Vervang `groupname` met de naam van de resourcegroep.

    * Vervang `Hadoop` met het clustertype dat u wilt maken. Bijvoorbeeld: `Hadoop`, `HBase`, `Kafka`, `Spark`, of `Storm`.

     > [!IMPORTANT]
     > HDInsight clusters worden geleverd in verschillende typen die met de werkbelasting of technologie die het cluster is afgestemd overeenkomen op. Er is geen ondersteunde methode om een cluster waarin meerdere typen zoals Storm en HBase op één cluster te maken.

    * Vervang `location` met dezelfde locatie gebruikt in de vorige stappen.

    * Vervang `storagename` met de naam van het opslagaccount.

    * Vervang `storagekey` met de sleutel in de vorige stap hebt verkregen.

    * Voor de `--defaultStorageContainer` parameter, gebruik dezelfde naam als u gebruikt voor het cluster.

    * Vervang `admin` en `httppassword` met de naam en het wachtwoord dat u gebruiken wilt bij het openen van het cluster via HTTPS.

    * Vervang `sshuser` en `sshuserpassword` met de gebruikersnaam en wachtwoord die u gebruiken wilt bij het openen van het cluster via SSH

    > [!IMPORTANT]
    > In dit voorbeeld maakt een cluster met twee worker opmerkingen. U kunt het aantal worker-knooppunten ook wijzigen nadat de cluster is gemaakt door het uitvoeren van bewerkingen voor vergroten/verkleinen. Als u van plan bent over het gebruik van meer dan 32 worker-knooppunten, selecteert u een grootte van het hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB RAM-geheugen. U kunt de grootte van het hoofdknooppunt instellen met behulp van de `--headNodeSize` parameter tijdens het maken van het cluster.
    >
    > Zie voor meer informatie over knooppuntgrootten en bijbehorende kosten [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).

    Duurt enkele minuten voor het cluster maken van het proces te voltooien. Meestal ongeveer 15.

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen

Nu dat u een HDInsight-cluster met de Azure CLI hebt gemaakt, gebruikt u de volgende voor informatie over het werken met het cluster:

### <a name="hadoop-clusters"></a>Hadoop-clusters

* [Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-clusters

* [Aan de slag met HBase in HDInsight](hdinsight-hbase-tutorial-get-started-linux.md)
* [Ontwikkelen van Java-toepassingen voor HBase in HDInsight](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-clusters

* [Java-topologieën ontwikkelen voor Storm op HDInsight](hdinsight-storm-develop-java-topology.md)
* [Python-onderdelen in Storm op HDInsight gebruiken](hdinsight-storm-develop-python-topology.md)
* [Implementeren en bewaken topologieën met Storm op HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)
