---
title: Apache Hadoop-clusters met een webbrowser - Azure HDInsight maken
description: Leer hoe u Apache Hadoop, Apache HBase, Apache Storm of Apache Spark-clusters maken in Linux voor HDInsight met behulp van een webbrowser en de Azure previewportal.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/28/2018
ms.author: hrasheed
ms.openlocfilehash: ca8b398a796284e34ce4e0b577c335552ca75a98
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53974264"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Op basis van Linux-clusters maken in HDInsight met behulp van de Azure portal
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

De Azure-portal is een webgebaseerde beheerhulpprogramma voor services en resources die worden gehost in de Microsoft Azure-cloud. In dit artikel leert u over het maken van HDInsight op basis van Linux-clusters met behulp van de portal.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een moderne webbrowser**. De Azure portal maakt gebruik van HTML5 en Javascript, en werkt mogelijk niet correct in oudere webbrowsers.

## <a name="create-clusters"></a>Clusters maken
De Azure-portal wordt aangegeven dat de meeste van de eigenschappen van het cluster. Met behulp van Azure Resource Manager-sjablonen, kunt u veel details verbergen. Zie voor meer informatie, [Linux maken op basis van Apache Hadoop-clusters in HDInsight met behulp van Azure Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer in het menu links **+ een resource maken**.

1.  Onder **Azure Marketplace**, selecteer **Analytics**.

1.  Onder **aanbevolen**, selecteer **HDInsight**.
   
    ![Het maken van een nieuw cluster in Azure portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster.png "het maken van een nieuw cluster in Azure portal")

1. Uit de **HDInsight** weergeeft, schakelt **aangepast (grootte, instellingen en apps)**.

1. Selecteer **1 basisbeginselen**, en voer de volgende informatie:

    ![Het maken van een nieuw cluster in Azure portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-basics.png "het maken van een nieuw cluster in Azure portal")

    * Voer **clusternaam**: Deze naam moet wereldwijd uniek zijn.

    * Uit de **abonnement** vervolgkeuzelijst, selecteer het Azure-abonnement dat wordt gebruikt voor het cluster.

    * Selecteer **clustertype**, en selecteer vervolgens het type van het cluster (Hadoop, Spark, enzovoort) die u wilt maken. De **besturingssysteem** worden **Linux**. Selecteer vervolgens een versie van een cluster. Gebruik de standaardversie als u niet weet wat u moet kiezen. Zie [HDInsight-clusterversies](hdinsight-component-versioning.md) voor meer informatie.
     
        > [!IMPORTANT]  
        > HDInsight clusters worden geleverd in verschillende typen, die overeenkomen met de werkbelasting of technologie die het cluster is afgestemd op. Er is geen ondersteunde methode om een cluster die meerdere typen, zoals Storm en HBase op één cluster combineert te maken.
        
    * Voor **de gebruikersnaam voor clusteraanmelding** en **wachtwoord voor clusteraanmelding**, de gebruikersnaam en wachtwoord opgeven voor de gebruiker met beheerdersrechten.

    * Voer een **SSH-gebruikersnaam** en als u wilt dat het SSH-wachtwoord hetzelfde als het beheerderswachtwoord dat u eerder hebt opgegeven, selecteert de **hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken** selectievakje. Als dit niet het geval is, bieden een **wachtwoord** of **openbare sleutel**, die wordt gebruikt om de SSH-gebruiker te verifiëren. Het gebruik van een openbare sleutel is de aanbevolen methode. Klik onderaan op **Selecteren** om de referentieconfiguratie op te slaan.
   
        Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

    * Geef bij **Resourcegroep** op of u een nieuwe resourcegroep wilt maken of een bestaande wilt gebruiken.

    * Geef een datacenter **locatie** waar het cluster is gemaakt.

    * Selecteer **volgende** om naar de volgende pagina te gaan.

4. Van **2 beveiliging en netwerken**, kunt u verbinding maken met uw cluster met een virtueel netwerk met behulp van de opgegeven vervolgkeuzelijst. Selecteer een Azure-netwerk en het subnet als u wilt plaatsen van het cluster in een virtueel netwerk. Zie voor meer informatie over het gebruik van HDInsight met een Virtueelnetwerk, met inbegrip van specifieke configuratievereisten voor het Virtueelnetwerk, [mogelijkheden voor HDInsight uitbreiden met behulp van een Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md). Als u wilt gebruiken de **Enterprise-beveiligingspakket**, kunt u ook hier instructies volgen: [Een HDInsight-cluster door middel van Azure Active Directory Domain Services configureren met Enterprise-beveiligingspakket](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds).

    Selecteer **volgende** om naar de volgende pagina te gaan.


5. Van **3 opslag**, opgeven of u wilt dat Azure Storage (WASB) of Data Lake-opslag als de standaardopslag. Bekijk de onderstaande tabel voor meer informatie.

     ![Het maken van een nieuw cluster in Azure portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-storage.png "het maken van een nieuw cluster in Azure portal")

     | Storage                                      | Description |
     |----------------------------------------------|-------------|
     | **Azure Storage-Blobs als standaardopslag**   | <ul><li>Voor **primaire opslagtype**, selecteer **Azure Storage**. Hierna voor **selectiemethode**, kunt u **Mijn abonnementen** als u wilt opgeven van een storage-account die deel uitmaakt van uw Azure-abonnement en selecteer vervolgens het opslagaccount. Klik anders op **toegangssleutel** en geef de informatie voor het opslagaccount dat u kiezen wilt uit buiten uw Azure-abonnement.</li><li>Voor **standaardcontainer**, u kunt kiezen om te gaan met de standaardnaam van de container voorgesteld door de portal of geef uw eigen.</li><li>Als u van WASB als standaardopslag gebruikmaakt, u kunt (optioneel) klikt u op **extra Opslagaccounts** om op te geven van extra opslagaccounts om te koppelen aan het cluster. Voor **Azure Storage-sleutels**, klikt u op **een opslagsleutel toevoegen**, en klikt u vervolgens kunt u een storage-account van uw Azure-abonnementen of bieden via andere abonnementen (door de toegangssleutel voor opslagaccount).</li><li>Als u van WASB als standaardopslag gebruikmaakt, u kunt (optioneel) klikt u op **Data Lake Storage access** om op te geven van Azure Data Lake Storage als extra opslag. Zie voor meer informatie [Snelstart: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
     | **Azure Data Lake Storage als standaardopslag** | Voor **primaire opslagtype**, selecteer **Azure Data Lake Storage Gen1** of **Azure Data Lake Storage Gen2** en Raadpleeg het artikel [Quick Start: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) voor instructies. |
     | **Externe metastores**                      | Desgewenst kunt u een SQL-database als Hive en Oozie-metagegevens die zijn gekoppeld aan het cluster wilt opslaan. Voor **SQL-database voor Hive selecteren** selecteert u een SQL-database, en geef vervolgens de gebruikersnaam en wachtwoord voor de database. Herhaal deze stappen voor Oozie-metagegevens.<br><br>Enkele overwegingen bij het gebruik van Azure SQL-database voor metastores. <ul><li>De Azure SQL-database die wordt gebruikt voor de metastore moet verbinding met andere Azure-services, met inbegrip van Azure HDInsight toestaan. Klik op de servernaam op het dashboard van de Azure SQL database aan de rechterkant. Dit is de server waarop de SQL database-instantie wordt uitgevoerd. Wanneer u van de weergave gebruikmaakt, klikt u op **configureren**, en vervolgens voor **Azure Services**, klikt u op **Ja**, en klik vervolgens op **opslaan**.</li><li>Bij het maken van een metastore, gebruik niet de naam van een database met streepjes of afbreekstreepjes bevatten, zoals dit leiden het proces voor het maken van cluster tot kan mislukt.</li></ul> |

     > [!WARNING]  
     > Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

     Selecteer **volgende** om naar de volgende pagina te gaan.


6. Van **4 toepassingen (optioneel)**, selecteer alle gewenste toepassingen.  Deze toepassingen kunnen zijn ontwikkeld door Microsoft, door onafhankelijke softwareleveranciers (ISV) of door u zelf. Zie voor meer informatie, [HDInsight installeren toepassingen](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).

    Selecteer **volgende** om naar de volgende pagina te gaan.


6. **5 clustergrootte** geeft informatie weer over de knooppunten die worden gebruikt voor dit cluster. Stel het aantal worker-knooppunten die u nodig hebt voor het cluster. De geschatte kosten van het uitvoeren van het cluster wordt ook weergegeven.
   
    ![Prijscategorieën van knooppunten](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-nodes.png "geeft het aantal clusterknooppunten")
   
   > [!IMPORTANT]  
   > Als u van plan bent op meer dan 32 worker-knooppunten op het maken van clusters of door te schalen van het cluster na het maken, moet u de grootte van een hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB RAM-geheugen selecteren.
   > 
   > Zie [Prijsdetails voor Azure HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) voor meer informatie over knooppuntgrootten en de bijbehorende kosten.
   
    Selecteer **volgende** om naar de volgende pagina te gaan.

8. Van **6 scriptacties**, kunt u een cluster om aangepaste onderdelen te installeren.  Gebruik deze optie als u een aangepast script gebruiken wilt voor het aanpassen van een cluster, wanneer het cluster wordt gemaakt. Zie voor meer informatie over scriptacties [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster-linux.md).

   Selecteer **volgende** om naar de volgende pagina te gaan.

9. Van **7 samenvatting**, controleert u de gegevens die u eerder hebt ingevoerd en selecteer vervolgens **maken**.

     ![Prijscategorieën van knooppunten](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-summary.png "geeft het aantal clusterknooppunten")
    
    > [!NOTE]  
    > Meestal duurt het genereren van het cluster ongeveer 20 minuten. Monitor **meldingen** om te controleren op het inrichtingsproces.

10. Zodra het proces voor het maken is voltooid, selecteert u **naar de Resource gaan** uit de **implementatie is voltooid** melding. Het venster cluster bevat de volgende informatie.
    
    ![Cluster-interface](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-completed.png "eigenschappen van het Cluster")
    
    Gebruik het volgende om te begrijpen van de pictogrammen aan de bovenkant.
    
    * **Overzicht** tabblad bevat alle essentiële informatie over het cluster, zoals de naam, de resourcegroep die hoort bij, de locatie, het besturingssysteem, de URL voor de cluster-dashboard, enzovoort.
    * **Dashboard** stuurt u naar de Ambari-portal die zijn gekoppeld aan het cluster.
    * **Secure Shell**: De informatie die nodig is voor toegang tot het cluster via SSH.
    * **Cluster schalen** kunt u het aantal worker-knooppunten die zijn gekoppeld aan het cluster verhogen.
      * **Verwijder**: Hiermee verwijdert u het HDInsight-cluster.
    

## <a name="customize-clusters"></a>Clusters aanpassen
* Zie [aanpassen HDInsight-clusters met Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Zie [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Het cluster verwijderen
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-hadoop-create-linux-clusters-portal.md) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen
Nu dat u een HDInsight-cluster hebt gemaakt, gebruikt u de volgende voor informatie over het werken met uw cluster:

### <a name="apache-hadoop-clusters"></a>Apache Hadoop-clusters
* [Apache Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [UseApache Pig met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)

### <a name="apache-hbase-clusters"></a>Apache HBase-clusters
* [Aan de slag met Apache HBase op HDInsight](hbase/apache-hbase-tutorial-get-started-linux.md)
* [Java-toepassingen voor Apache HBase op HDInsight ontwikkelen](hbase/apache-hbase-build-java-maven-linux.md)

### <a name="apache-storm-clusters"></a>Apache Storm-clusters
* [Java-topologieën ontwikkelen voor Apache Storm op HDInsight](storm/apache-storm-develop-java-topology.md)
* [Python-onderdelen in Apache Storm op HDInsight gebruiken](storm/apache-storm-develop-python-topology.md)
* [Topologieën met Apache Storm op HDInsight implementeren en bewaken](storm/apache-storm-deploy-monitor-topology-linux.md)

### <a name="apache-spark-clusters"></a>Apache Spark-clusters
* [Een zelfstandige toepassing maken met behulp van Scala](spark/apache-spark-create-standalone-application.md)
* [Apache Livy gebruiken om taken op afstand uit te voeren in een Apache Spark-cluster](spark/apache-spark-livy-rest-interface.md)
* [Apache Spark met BI: Interactieve gegevensanalyses met behulp van Spark in HDInsight met BI-hulpprogramma's uitvoeren](spark/apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)

