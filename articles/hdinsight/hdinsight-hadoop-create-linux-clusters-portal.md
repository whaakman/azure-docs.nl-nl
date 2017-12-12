---
title: Met een webbrowser - Azure HDInsight Hadoop-clusters maken | Microsoft Docs
description: Informatie over het maken van Hadoop, HBase, Storm of Spark-clusters op Linux voor HDInsight met behulp van een webbrowser en de Azure preview portal.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 697278cf-0032-4f7c-b9b2-a84c4347659e
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 812b6f323e2ddaee9095a7bdf221d6a8ebd69fd2
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Op basis van Linux-clusters maken in HDInsight met behulp van de Azure-portal
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

De Azure-portal is een webgebaseerde beheerprogramma voor services en bronnen die worden gehost in de Microsoft Azure-cloud. In dit artikel leert u het maken van Linux gebaseerde HDInsight-clusters met behulp van de portal.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een moderne webbrowser**. De Azure portal maakt gebruik van HTML5 en Javascript en werkt mogelijk niet correct in oudere webbrowsers.

## <a name="create-clusters"></a>Clusters maken
De Azure-portal beschrijft de meeste van de eigenschappen van het cluster. Azure Resource Manager-sjabloon kunt u een groot aantal details verbergen. Zie voor meer informatie [maken Linux gebaseerde Hadoop-clusters in HDInsight met behulp van Azure Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Klik op  **+** , klikt u op **Intelligence en analyse**, en klik vervolgens op **HDInsight**.
   
    ![Een nieuw cluster maken in de Azure portal](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster.png "een nieuw cluster maken in de Azure portal")

3. In de **HDInsight** blade, klikt u op **aangepast (grootte, instellingen, apps)**, klikt u op **basisbeginselen**, en voer de volgende informatie.

    ![Een nieuw cluster maken in de Azure portal](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-basics.png "een nieuw cluster maken in de Azure portal")

    * Voer **Clusternaam** in: deze naam moet uniek zijn.

    * Van de **abonnement** vervolgkeuzelijst, selecteer de Azure-abonnement dat wordt gebruikt voor het cluster.

    * Klik op **type Cluster**, en selecteer vervolgens:
   
        * **Clustertype**: als u niet weet wat u moet kiezen, selecteert u **Hadoop**. Dit is het meestgebruikte clustertype.
     
            > [!IMPORTANT]
            > HDInsight clusters worden geleverd in verschillende typen die met de werkbelasting of technologie die het cluster is afgestemd overeenkomen op. Er is geen ondersteunde methode om een cluster waarin meerdere typen zoals Storm en HBase op één cluster te maken. 
            > 
            > 
        
        * **Besturingssysteem**: selecteer **Linux**.
        
        * **Versie**: gebruik de standaardversie als u niet weet wat u moet kiezen. Zie [HDInsight-clusterversies](hdinsight-component-versioning.md) voor meer informatie.
        

    * Voor **clusteraanmeldgegevens** en **Cluster aanmeldingswachtwoord**, gebruikersnaam en wachtwoord opgeven voor de beheerder.

    * Voer een **SSH-gebruikersnaam** en als u wilt de SSH-wachtwoord hetzelfde als de Administrator-wachtwoord dat u eerder hebt opgegeven, selecteer de **gebruik hetzelfde wachtwoord als cluster aanmelding** selectievakje. Als dit niet het geval is, Geef een **wachtwoord** of **openbare sleutel**, die wordt gebruikt voor het verifiëren van de SSH-gebruiker. Het gebruik van een openbare sleutel is de aanbevolen methode. Klik onderaan op **Selecteren** om de referentieconfiguratie op te slaan.
   
        Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

    * Voor **resourcegroep**, opgeven of u wilt een nieuwe resourcegroep maken of een bestaande wilt gebruiken.

    * Geef een datacenter **locatie** waar het cluster wordt gemaakt.

    * Klik op **Volgende**.

4. Op de **opslag** blade aangeven of u Azure Storage (WASB) of de Data Lake Store als uw standaard-opslag. Bekijk de onderstaande tabel voor meer informatie.

    ![Een nieuw cluster maken in de Azure portal](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-storage.png "een nieuw cluster maken in de Azure portal")

    | Storage                                      | Beschrijving |
    |----------------------------------------------|-------------|
    | **Azure Storage-Blobs als standaard opslag**   | <ul><li>Voor **primaire opslagtype**, selecteer **Azure Storage**. Daarna voor **selectiemethode**, kunt u **abonnementen** als u wilt opgeven van een opslagaccount die deel uitmaakt van uw Azure-abonnement en selecteer vervolgens het opslagaccount. Klik anders op **toegangssleutel** en geef de gegevens voor het opslagaccount dat u kiezen wilt uit buiten uw Azure-abonnement.</li><li>Voor **standaardcontainer**, kunt u doorgaan met de standaardnaam van de container voorgesteld door de portal of geef uw eigen.</li><li>Als u WASB als standaard opslag gebruikt, hoeft u (optioneel) op **extra Opslagaccounts** om op te geven van extra opslagaccounts te koppelen aan het cluster. In de **Opslagsleutels Azure** blade, klikt u op **opslag van een sleutel**, en vervolgens kunt u uw storage-account van uw Azure-abonnementen of van andere abonnementen (de storage-account toegang sleutel).</li><li>Als u WASB als standaard opslag gebruikt, hoeft u (optioneel) op **Data Lake Store toegang** om op te geven van Azure Data Lake Store als extra opslagruimte. Zie voor meer informatie [een HDInsight-cluster maken met Data Lake Store met Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</li></ul> |
    | **Azure Data Lake Store als standaard opslag** | Voor **primaire opslagtype**, selecteer **Data Lake Store** en Raadpleeg het artikel [een HDInsight-cluster maken met Data Lake Store met Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md) voor instructies. |
    | **Met externe metastores**                      | U kunt eventueel een SQL-database voor het opslaan van Hive en Oozie-metagegevens gekoppeld aan het cluster opgeven. Voor **selecteert u een SQL-database voor Hive** selecteert u een SQL-database en geef vervolgens de gebruikersnaam en wachtwoord voor de database. Herhaal deze stappen voor Oozie-metagegevens.<br><br>Enkele overwegingen bij het gebruik van Azure SQL-database voor metastores. <ul><li>De Azure SQL database gebruikt voor de metastore moet connectiviteit met andere Azure-services, waaronder Azure HDInsight toestaan. Klik op de servernaam op het dashboard van de Azure SQL database aan de rechterkant. Dit is de server waarop de SQL database-instantie wordt uitgevoerd. Wanneer u zich op de serverweergave, klikt u op **configureren**, en vervolgens voor **Azure Services**, klikt u op **Ja**, en klik vervolgens op **opslaan**.</li><li>Bij het maken van een metastore gebruik niet de naam van een database met streepjes of afbreekstreepjes, omdat hierdoor het maakproces cluster mislukken.</li></ul>                                                                                                                                                                       |

    Klik op **Volgende**. 

    > [!WARNING]
    > Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

5. Klik desgewenst op **toepassingen** toepassingen die met HDInsight-clusters werken te installeren. Deze toepassingen kunnen zijn ontwikkeld door Microsoft, door onafhankelijke softwareleveranciers (ISV) of door u zelf. Zie voor meer informatie [installeren HDInsight-toepassingen](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).


6. Klik op **clustergrootte** om informatie over de knooppunten die worden gemaakt voor dit cluster weer te geven. Stel het aantal worker-knooppunten die u nodig hebt voor het cluster. De geschatte kosten voor het cluster worden in de blade weergegeven.
   
    ![Knooppunt prijzen lagen blade](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-nodes.png "aantal clusterknooppunten opgeven")
   
   > [!IMPORTANT]
   > Als u van plan bent op meer dan 32 worker-knooppunten op het maken van het cluster of door te schalen van het cluster na het maken, selecteert u een grootte van het hoofdknooppunt met ten minste 8 kerngeheugens en 14GB RAM-geheugen.
   > 
   > Zie voor meer informatie over knooppuntgrootten en bijbehorende kosten [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight/).
   > 
   > 
   
   Klik op **volgende** om op te slaan van het knooppunt configuratie prijzen.

7. Klik op **geavanceerde instellingen** voor het configureren van andere optionele instellingen zoals het gebruik van **scriptacties** voor het aanpassen van een cluster om aangepaste onderdelen installeren of lid te worden van een **virtueel netwerk**. Bekijk de onderstaande tabel voor meer informatie.

    ![Knooppunt prijzen lagen blade](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-advanced.png "aantal clusterknooppunten opgeven")

    | Optie | Beschrijving |
    |--------|-------------|
    | **Scriptacties** | Gebruik deze optie als u een aangepast script gebruiken wilt voor het aanpassen van een cluster als het cluster wordt gemaakt. Zie voor meer informatie over scriptacties [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md). |
    | **Virtueel netwerk** | Selecteer een Azure-netwerk en het subnet als u wilt plaatsen van het cluster in een virtueel netwerk. Zie voor meer informatie over het gebruik van HDInsight met een virtueel netwerk, met inbegrip van specifieke configuratievereisten voor het virtuele netwerk [mogelijkheden uitbreiden HDInsight met behulp van een Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md). |

    Klik op **Volgende**.

8. Op de **samenvatting** blade, Controleer de informatie die u eerder hebt ingevoerd, en klik vervolgens op **maken**.

    ![Knooppunt prijzen lagen blade](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-summary.png "aantal clusterknooppunten opgeven")
    
    > [!NOTE]
    > Het duurt enige tijd voor het cluster worden gemaakt, meestal ongeveer 15 minuten. Gebruik de tegel op het Startboard, of de **meldingen** vermelding aan de linkerkant van de pagina om te controleren op het inrichtingsproces.
    > 
    > 
12. Nadat het maken van het proces is voltooid, klikt u op de tegel voor het cluster vanaf het Startboard starten van de cluster-blade. De cluster-blade bevat de volgende informatie.
    
    ![Cluster-blade](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-completed.png "eigenschappen van het Cluster")
    
    Gebruik de volgende om te begrijpen van de pictogrammen aan het begin van deze blade.
    
    * **Overzicht** tabblad bevat alle essentiële informatie over het cluster, zoals de naam, de resourcegroep die hoort bij, de locatie, het besturingssysteem, de URL voor de cluster-dashboard, enzovoort.
    * **Dashboard** stuurt u naar de Ambari-portal die zijn gekoppeld aan het cluster.
    * **Secure Shell**: informatie nodig voor toegang tot het cluster via SSH.
    * **Schaal cluster** kunt u het aantal worker-knooppunten die zijn gekoppeld aan het cluster vergroten.
    * **Verwijderen**: Hiermee verwijdert u het HDInsight-cluster.
    

## <a name="customize-clusters"></a>Clusters aanpassen
* Zie [aanpassen HDInsight-clusters met behulp van de Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Zie [aanpassen HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Het cluster verwijderen
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen
Nu dat u een HDInsight-cluster hebt gemaakt, gebruikt u de volgende voor informatie over het werken met het cluster:

### <a name="hadoop-clusters"></a>Hadoop-clusters
* [Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-clusters
* [Aan de slag met HBase in HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Ontwikkelen van Java-toepassingen voor HBase in HDInsight](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-clusters
* [Java-topologieën ontwikkelen voor Storm op HDInsight](storm/apache-storm-develop-java-topology.md)
* [Python-onderdelen in Storm op HDInsight gebruiken](storm/apache-storm-develop-python-topology.md)
* [Implementeren en bewaken topologieën met Storm op HDInsight](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>Spark-clusters
* [Een zelfstandige toepassing maken met behulp van Scala](spark/apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](spark/apache-spark-livy-rest-interface.md)
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](spark/apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)
* [Spark-streaming: Spark in HDInsight gebruiken voor het bouwen van realtime streamingtoepassingen](spark/apache-spark-eventhub-streaming.md)

