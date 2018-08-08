---
title: Met een webbrowser - Azure HDInsight Hadoop-clusters maken
description: Leer hoe u Hadoop, HBase, Storm of Spark-clusters maken in Linux voor HDInsight met behulp van een webbrowser en de Azure previewportal.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: jasonh
ms.openlocfilehash: 210ece6825393917f77e213939770c4ee867fd20
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39600843"
---
# <a name="create-linux-based-clusters-in-hdinsight-using-the-azure-portal"></a>Op basis van Linux-clusters maken in HDInsight met behulp van de Azure portal
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

De Azure-portal is een webgebaseerde beheerhulpprogramma voor services en resources die worden gehost in de Microsoft Azure-cloud. In dit artikel leert u over het maken van HDInsight op basis van Linux-clusters met behulp van de portal.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een moderne webbrowser**. De Azure portal maakt gebruik van HTML5 en Javascript, en werkt mogelijk niet correct in oudere webbrowsers.

## <a name="create-clusters"></a>Clusters maken
De Azure-portal wordt aangegeven dat de meeste van de eigenschappen van het cluster. Met behulp van Azure Resource Manager-sjabloon, kunt u veel details verbergen. Zie voor meer informatie, [maken Linux gebaseerde Hadoop-clusters in HDInsight met behulp van Azure Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Klik op **+**, klikt u op **Intelligence en analyse**, en klik vervolgens op **HDInsight**.
   
    ![Het maken van een nieuw cluster in Azure portal](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster.png "het maken van een nieuw cluster in Azure portal")

3. In de **HDInsight** blade, klikt u op **aangepast (grootte, instellingen en apps)**, klikt u op **basisbeginselen**, en voer de volgende informatie.

    ![Het maken van een nieuw cluster in Azure portal](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-basics.png "het maken van een nieuw cluster in Azure portal")

    * Voer **Clusternaam** in: deze naam moet uniek zijn.

    * Uit de **abonnement** vervolgkeuzelijst, selecteer het Azure-abonnement dat wordt gebruikt voor het cluster.

    * Klik op **clustertype**, en selecteer vervolgens het type van het cluster (Hadoop, Spark, enzovoort) die u wilt maken. Voor **besturingssysteem**, klikt u op **Linux** en selecteer vervolgens een versie. Gebruik de standaardversie als u niet weet wat u moet kiezen. Zie [HDInsight-clusterversies](hdinsight-component-versioning.md) voor meer informatie.

        Voor Hadoop, Spark en interactieve Query clustertypen, u kunt ook selecteren voor het installeren van de **Enterprise-beveiligingspakket**. Enterprise-beveiligingspakket kunt beveiligingsfuncties, zoals Azure Active Directory-integratie en Apache Ranger voor de clusters. Zie voor meer informatie, [Enterprise-beveiligingspakket in Azure HDInsight](./domain-joined/apache-domain-joined-introduction.md).

        ![Inschakelen van Enterprise-beveiligingspakket](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-enable-enterprise-security-package.png "Enterprise-beveiligingspakket inschakelen")
     
        > [!IMPORTANT]
        > HDInsight clusters worden geleverd in verschillende typen, die overeenkomen met de werkbelasting of technologie die het cluster is afgestemd op. Er is geen ondersteunde methode om een cluster die meerdere typen, zoals Storm en HBase op één cluster combineert te maken. 
        > 
        > 
        
    * Voor **de gebruikersnaam voor clusteraanmelding** en **wachtwoord voor clusteraanmelding**, de gebruikersnaam en wachtwoord opgeven voor de gebruiker met beheerdersrechten.

    * Voer een **SSH-gebruikersnaam** en als u wilt dat het SSH-wachtwoord hetzelfde als het beheerderswachtwoord dat u eerder hebt opgegeven, selecteert de **hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken** selectievakje. Als dit niet het geval is, bieden een **wachtwoord** of **openbare sleutel**, die wordt gebruikt om de SSH-gebruiker te verifiëren. Het gebruik van een openbare sleutel is de aanbevolen methode. Klik onderaan op **Selecteren** om de referentieconfiguratie op te slaan.
   
    Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

    * Geef bij **Resourcegroep** op of u een nieuwe resourcegroep wilt maken of een bestaande wilt gebruiken.

    * Geef een datacenter **locatie** waar het cluster is gemaakt.

    * Klik op **Volgende**.

4. Voor **opslag**, opgeven of u wilt dat Azure Storage (WASB) of Data Lake-opslag als de standaardopslag. Bekijk de onderstaande tabel voor meer informatie.

    ![Het maken van een nieuw cluster in Azure portal](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-storage.png "het maken van een nieuw cluster in Azure portal")

    | Storage                                      | Beschrijving |
    |----------------------------------------------|-------------|
    | **Azure Storage-Blobs als standaardopslag**   | <ul><li>Voor **primaire opslagtype**, selecteer **Azure Storage**. Hierna voor **selectiemethode**, kunt u **Mijn abonnementen** als u wilt opgeven van een storage-account die deel uitmaakt van uw Azure-abonnement en selecteer vervolgens het opslagaccount. Klik anders op **toegangssleutel** en geef de informatie voor het opslagaccount dat u kiezen wilt uit buiten uw Azure-abonnement.</li><li>Voor **standaardcontainer**, u kunt kiezen om te gaan met de standaardnaam van de container voorgesteld door de portal of geef uw eigen.</li><li>Als u van WASB als standaardopslag gebruikmaakt, u kunt (optioneel) klikt u op **extra Opslagaccounts** om op te geven van extra opslagaccounts om te koppelen aan het cluster. Voor **Azure Storage-sleutels**, klikt u op **een opslagsleutel toevoegen**, en klikt u vervolgens kunt u een storage-account van uw Azure-abonnementen of bieden via andere abonnementen (door de toegangssleutel voor opslagaccount).</li><li>Als u van WASB als standaardopslag gebruikmaakt, u kunt (optioneel) klikt u op **Data Lake Store-toegang** om op te geven van Azure Data Lake Storage als extra opslag. Zie voor meer informatie, [Quick Start: clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
    | **Azure Data Lake Storage als standaardopslag** | Voor **primaire opslagtype**, selecteer **Azure Data Lake Storage Gen1** of **Azure Data Lake Storage Gen2 (Preview)** en Raadpleeg het artikel [Quick Start : Stel-clusters in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) voor instructies. |
    | **Externe metastores**                      | Desgewenst kunt u een SQL-database als Hive en Oozie-metagegevens die zijn gekoppeld aan het cluster wilt opslaan. Voor **SQL-database voor Hive selecteren** selecteert u een SQL-database, en geef vervolgens de gebruikersnaam en wachtwoord voor de database. Herhaal deze stappen voor Oozie-metagegevens.<br><br>Enkele overwegingen bij het gebruik van Azure SQL-database voor metastores. <ul><li>De Azure SQL-database die wordt gebruikt voor de metastore moet verbinding met andere Azure-services, met inbegrip van Azure HDInsight toestaan. Klik op de servernaam op het dashboard van de Azure SQL database aan de rechterkant. Dit is de server waarop de SQL database-instantie wordt uitgevoerd. Wanneer u van de weergave gebruikmaakt, klikt u op **configureren**, en vervolgens voor **Azure Services**, klikt u op **Ja**, en klik vervolgens op **opslaan**.</li><li>Bij het maken van een metastore, gebruik niet de naam van een database met streepjes of afbreekstreepjes bevatten, zoals dit leiden het proces voor het maken van cluster tot kan mislukt.</li></ul> |

    Klik op **Volgende**. 

    > [!WARNING]
    > Het gebruik van een extra opslagaccount op een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

5. Klik desgewenst op **toepassingen** voor het installeren van toepassingen die met HDInsight-clusters werken. Deze toepassingen kunnen zijn ontwikkeld door Microsoft, door onafhankelijke softwareleveranciers (ISV) of door u zelf. Zie voor meer informatie, [HDInsight installeren toepassingen](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).


6. Klik op **clustergrootte** informatie weergeven over de knooppunten die worden gebruikt voor dit cluster. Stel het aantal worker-knooppunten die u nodig hebt voor het cluster. De geschatte kosten van het uitvoeren van het cluster wordt ook weergegeven.
   
    ![Prijscategorieën van knooppunten](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-nodes.png "geeft het aantal clusterknooppunten")
   
   > [!IMPORTANT]
   > Als u van plan bent op meer dan 32 worker-knooppunten op het maken van clusters of door te schalen van het cluster na het maken, moet u de grootte van een hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB RAM-geheugen selecteren.
   > 
   > Zie [Prijsdetails voor Azure HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) voor meer informatie over knooppuntgrootten en de bijbehorende kosten.
   > 
   > 
   
   Klik op **volgende** om op te slaan de prijsconfiguratie voor knooppunten.

7. Klik op **geavanceerde instellingen** het configureren van andere optionele instellingen zoals het gebruik van **scriptacties** voor het aanpassen van een cluster voor het installeren van aangepaste onderdelen zijn of wanneer een **Virtueelnetwerk**. Bekijk de onderstaande tabel voor meer informatie.

    ![Prijscategorieën van knooppunten](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-advanced.png "geeft het aantal clusterknooppunten")

    | Optie | Beschrijving |
    |--------|-------------|
    | **Scriptacties** | Gebruik deze optie als u een aangepast script gebruiken wilt voor het aanpassen van een cluster, wanneer het cluster wordt gemaakt. Zie voor meer informatie over scriptacties [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster-linux.md). |
    | **Virtueel netwerk** | Selecteer een Azure-netwerk en het subnet als u wilt plaatsen van het cluster in een virtueel netwerk. Zie voor meer informatie over het gebruik van HDInsight met een Virtueelnetwerk, met inbegrip van specifieke configuratievereisten voor het Virtueelnetwerk, [mogelijkheden voor HDInsight uitbreiden met behulp van een Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md). |

    Klik op **Volgende**.

8. Voor **samenvatting**, controleert u de gegevens die u eerder hebt ingevoerd en klik vervolgens op **maken**.

    ![Prijscategorieën van knooppunten](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-summary.png "geeft het aantal clusterknooppunten")
    
    > [!NOTE]
    > Het duurt enige tijd voor het cluster moet worden gemaakt, meestal ongeveer 15 minuten. Gebruik de tegel op het Startboard of de **meldingen** vermelding aan de linkerkant van de pagina om te controleren op het inrichtingsproces.
    > 
    > 
12. Zodra het proces voor het maken is voltooid, klikt u op de tegel voor het cluster vanaf het Startboard. Het venster cluster bevat de volgende informatie.
    
    ![Cluster-interface](./media/hdinsight-hadoop-create-linux-cluster-portal/hdinsight-create-cluster-completed.png "eigenschappen van het Cluster")
    
    Gebruik het volgende om te begrijpen van de pictogrammen aan de bovenkant.
    
    * **Overzicht** tabblad bevat alle essentiële informatie over het cluster, zoals de naam, de resourcegroep die hoort bij, de locatie, het besturingssysteem, de URL voor de cluster-dashboard, enzovoort.
    * **Dashboard** stuurt u naar de Ambari-portal die zijn gekoppeld aan het cluster.
    * **Secure Shell**: informatie die nodig zijn voor toegang tot het cluster via SSH.
    * **Cluster schalen** kunt u het aantal worker-knooppunten die zijn gekoppeld aan het cluster verhogen.
    * **Verwijder**: Hiermee verwijdert u het HDInsight-cluster.
    

## <a name="customize-clusters"></a>Clusters aanpassen
* Zie [aanpassen HDInsight-clusters met Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md).
* Zie [aanpassen HDInsight-clusters met Script Action](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>Het cluster verwijderen
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen
Nu dat u een HDInsight-cluster hebt gemaakt, gebruikt u de volgende voor informatie over het werken met uw cluster:

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

