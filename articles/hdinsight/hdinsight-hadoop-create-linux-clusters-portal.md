---
title: Apache Hadoop-clusters maken via een webbrowser, Azure HDInsight
description: Leer hoe u Apache Hadoop, Apache HBase, Apache Storm of Apache Spark-clusters maken in Linux voor HDInsight met behulp van een webbrowser en de Azure previewportal.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 12/28/2018
ms.author: hrasheed
ms.openlocfilehash: 13d053c14b083390bfdd28fdad616caf13e7e4e0
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2019
ms.locfileid: "54911554"
---
# <a name="create-linux-based-clusters-in-hdinsight-by-using-the-azure-portal"></a>Op basis van Linux-clusters maken in HDInsight met behulp van Azure portal
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

De Azure-portal is een webgebaseerde beheerhulpprogramma voor services en resources die worden gehost in de Microsoft Azure-cloud. In dit artikel leert u hoe u Azure HDInsight op basis van Linux-clusters maken met behulp van de portal.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Een Azure-abonnement**. Zie [over het verkrijgen van Azure gratis proefversie voor het testen van Hadoop in HDInsight](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Een moderne webbrowser**. De Azure portal maakt gebruik van HTML5 en JavaScript. Deze mogelijk niet goed in oudere webbrowsers.

## <a name="create-clusters"></a>Clusters maken
De Azure-portal wordt aangegeven dat de meeste van de eigenschappen van het cluster. Met behulp van Azure Resource Manager-sjablonen, kunt u veel details verbergen. Zie voor meer informatie, [Apache Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]


1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **+ Een resource maken** in het menu aan de linkerkant.

1.  Onder **Azure Marketplace**, selecteer **Analytics**.

1.  Onder **Aanbevolen** selecteert u **HDInsight**.
   
    ![Een nieuw cluster maken in Azure portal](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster.png "het maken van een nieuw cluster in Azure portal")

1. Op de **HDInsight** weergeeft, schakelt **aangepast (grootte, instellingen en apps)**.

1. Selecteer **1 basisbeginselen**. Voer de volgende informatie op.

    ![Basisinstellingen configureren](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-basics.png "het maken van een nieuw cluster in Azure portal")

    * Voer de **clusternaam**. Deze naam moet wereldwijd uniek zijn.

    * Uit de **abonnement** vervolgkeuzelijst, selecteert u de Azure-abonnement dat wordt gebruikt voor het cluster.

    * Selecteer **clustertype**. Selecteer vervolgens het type van het cluster dat u wilt maken. Voorbeelden zijn Hadoop en Apache Spark. De **besturingssysteem** worden **Linux**. Selecteer vervolgens een versie van een cluster. Gebruik de standaardversie als u niet weet wat u moet kiezen. Zie [HDInsight-clusterversies](hdinsight-component-versioning.md) voor meer informatie.
     
        > [!IMPORTANT]  
        > HDInsight clusters worden geleverd in verschillende typen. Ze komen overeen met de werkbelasting of technologie die het cluster is afgestemd op. Er is geen ondersteunde methode om een cluster die meerdere typen combineert te maken. Voorbeelden zijn Storm en HBase op één cluster.
        
    * Voor **de gebruikersnaam voor clusteraanmelding** en **wachtwoord voor clusteraanmelding**, de gebruikersnaam en wachtwoord opgeven voor de gebruiker met beheerdersrechten.

    * Voer een **SSH-gebruikersnaam**. Als u wilt dat de dezelfde SSH-wachtwoord als het beheerderswachtwoord dat u eerder hebt opgegeven, selecteert u de **hetzelfde wachtwoord als voor aanmelding bij cluster gebruiken** selectievakje. Als dit niet het geval is, bieden een **wachtwoord** of **openbare sleutel** om de SSH-gebruiker te verifiëren. Een openbare sleutel is de methode die wordt aangeraden. Kies **Selecteer** onder aan de configuratie van de referenties op te slaan.
   
        Zie voor meer informatie, [verbinding maken met HDInsight (Apache Hadoop) met behulp van SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

    * Geef bij **Resourcegroep** op of u een nieuwe resourcegroep wilt maken of een bestaande wilt gebruiken.

    * Geef een datacenter **locatie** waar het cluster is gemaakt.

    * Selecteer **volgende** om naar de volgende pagina te gaan.

4. Van **2 beveiliging en netwerken**, kunt u uw cluster met een virtueel netwerk met behulp van het weergegeven menu van de vervolgkeuzelijst. Selecteer een Azure-netwerk en het subnet als u wilt plaatsen van het cluster in een virtueel netwerk. Zie voor meer informatie over het gebruik van HDInsight met een virtueel netwerk [mogelijkheden voor HDInsight uitbreiden met behulp van een Azure Virtual Network](hdinsight-extend-hadoop-virtual-network.md). Het artikel bevat specifieke configuratievereisten voor het virtuele netwerk. 

    Als u wilt gebruiken de **Enterprise-beveiligingspakket**, volgt u deze instructies: [Een HDInsight-cluster door middel van Azure Active Directory Domain Services configureren met Enterprise-beveiligingspakket](https://docs.microsoft.com/azure/hdinsight/domain-joined/apache-domain-joined-configure-using-azure-adds).

    Selecteer **volgende** om naar de volgende pagina te gaan.


5. Van **3 opslag**, opgeven of u wilt dat Azure Storage of Azure Data Lake Storage als de standaardopslag. Zie de volgende tabel voor meer informatie.

     ![Opslaginstellingen instellen](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-storage.png "het maken van een nieuw cluster in Azure portal")

     | Storage                                      | Description |
     |----------------------------------------------|-------------|
     | **Azure Storage-blobs als standaardopslag**   | <ul><li>Voor **primaire opslagtype**, selecteer **Azure Storage**. Voor **selectiemethode**, kiest u **Mijn abonnementen** als u wilt opgeven van een storage-account die deel uitmaakt van uw Azure-abonnement. Selecteer vervolgens het opslagaccount. Selecteer anders **toegangssleutel**. Geef de informatie voor het opslagaccount dat u kiezen wilt uit buiten uw Azure-abonnement.</li><li>Voor **standaardcontainer**, kiest u de standaardnaam van de container voorgesteld door de portal of geef uw eigen.</li><li>Als Azure Blob-opslag de standaardopslag is, kunt u ook selecteren **extra Opslagaccounts** om op te geven van extra opslagaccounts om te koppelen aan het cluster. Voor **Azure Storage-sleutels**, selecteer **een opslagsleutel toevoegen**. Geef vervolgens een storage-account van uw Azure-abonnementen of van andere abonnementen. Geef de toegangssleutel voor opslagaccount.</li><li>Als de Blob-opslag is de standaardopslag, kunt u ook selecteren **Data Lake Storage access** om op te geven van Azure Data Lake Storage als extra opslag. Zie voor meer informatie [Snelstart: clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md).</li></ul> |
     | **Azure Data Lake Storage als standaardopslag** | Voor **primaire opslagtype**, selecteer **Azure Data Lake Storage Gen1** of **Azure Data Lake Storage Gen2**. Raadpleeg het artikel [Quick Start: Clusters instellen in HDInsight](../storage/data-lake-storage/quickstart-create-connect-hdi-cluster.md) voor instructies. |
     | **Externe metastores**                      | Geef desgewenst een SQL-database als Apache Hive en Apache Oozie-metagegevens die zijn gekoppeld aan het cluster wilt opslaan. Voor **SQL-database voor Hive selecteren**, selecteert u een SQL-database. Geef de gebruikersnaam en het wachtwoord voor de database. Herhaal deze stappen voor Oozie-metagegevens.<br><br>Enkele overwegingen over het gebruik van Azure SQL-database voor metastores zijn als volgt: <ul><li>De Azure SQL-database die wordt gebruikt voor de metastore moet verbinding met andere Azure-services, met inbegrip van Azure HDInsight toestaan. Aan de rechterkant van het dashboard van Azure SQL database, selecteert u de naam van de server. Dit is de server die de SQL database-instantie wordt uitgevoerd op. Nadat u in de Serverweergave bent, selecteert u **configureren**. Vervolgens voor **Azure Services**, selecteer **Ja**. Selecteer vervolgens **Opslaan**.</li><li>Wanneer u een metastore maakt, geen naam van een database met streepjes of afbreekstreepjes bevatten. Deze tekens kunnen leiden tot het proces voor het maken van cluster mislukt.</li></ul> |

     > [!WARNING]  
     > Met behulp van een extra opslagaccount in een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

     Selecteer **volgende** om naar de volgende pagina te gaan.


6. Van **4 toepassingen (optioneel)**, selecteert u alle toepassingen die u wilt. Microsoft, onafhankelijke softwareleveranciers (ISV's), of u kunt deze toepassingen ontwikkelen. Zie voor meer informatie, [toepassingen installeren tijdens het maken van clusters](hdinsight-apps-install-applications.md#install-applications-during-cluster-creation).

    Selecteer **volgende** om naar de volgende pagina te gaan.


6. **5 clustergrootte** geeft informatie weer over de knooppunten die worden gebruikt voor dit cluster. Stel het aantal worker-knooppunten die u nodig hebt voor het cluster. De geschatte kosten van het uitvoeren van het cluster wordt ook weergegeven.
   
    ![Prijscategorieën van knooppunten opgeven](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-nodes.png "geeft het aantal clusterknooppunten")
   
   > [!IMPORTANT]  
   > Als u van plan op meer dan 32 worker-knooppunten bent, selecteert u de grootte van een hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB RAM-geheugen. De knooppunten op het maken van clusters of door te schalen van het cluster na het maken van plan bent. 
   > 
   > Zie voor meer informatie over de grootte en de bijbehorende kosten [prijzen voor Azure HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/).
   
    Selecteer **volgende** om naar de volgende pagina te gaan.

8. Van **6 scriptacties**, kunt u een cluster om aangepaste onderdelen te installeren. Deze optie werkt als u een aangepast script gebruiken wilt voor het aanpassen van een cluster, wanneer het cluster wordt gemaakt. Zie voor meer informatie over scriptacties [aanpassen Linux gebaseerde HDInsight-clusters met behulp van scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

   Selecteer **volgende** om naar de volgende pagina te gaan.

9. Van **7 samenvatting**, controleert u de gegevens die u eerder hebt ingevoerd. Selecteer vervolgens **Maken**.

     ![Configuraties bevestigen](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-summary.png "geeft het aantal clusterknooppunten")
    
    > [!NOTE]  
    > Meestal duurt het genereren van het cluster ongeveer 20 minuten. Monitor **meldingen** om te controleren op het inrichtingsproces.

10. Nadat het proces voor het maken is voltooid, selecteert u **naar de Resource gaan** uit de **implementatie is voltooid** melding. Het venster cluster bevat de volgende informatie.
    
    ![Cluster-interface](./media/hdinsight-hadoop-create-linux-clusters-portal/hdinsight-create-cluster-completed.png "eigenschappen van het Cluster")
    
    De pictogrammen in het venster worden als volgt uitgelegd:
    
    * De **overzicht** tabblad bevat alle essentiële informatie over het cluster. Voorbeelden zijn de naam, de resourcegroep die hoort bij de locatie, het besturingssysteem en de URL voor de clusterdashboard.
    * **Dashboard** stuurt u naar de Ambari-portal die zijn gekoppeld aan het cluster.
    * **Secure Shell** bevat informatie die nodig zijn voor toegang tot het cluster met behulp van SSH.
    * Met behulp van **cluster schalen**, verhoogt u het aantal worker-knooppunten die zijn gekoppeld aan het cluster.
    * **Verwijder** Hiermee verwijdert u het HDInsight-cluster.
    

## <a name="customize-clusters"></a>Clusters aanpassen
* [HDInsight-clusters aanpassen met Bootstrap](hdinsight-hadoop-customize-cluster-bootstrap.md)
* [HDInsight op basis van Linux-clusters aanpassen met behulp van scriptacties](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="delete-the-cluster"></a>Het cluster verwijderen
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Problemen oplossen

Zie [Vereisten voor toegangsbeheer](hdinsight-hadoop-create-linux-clusters-portal.md) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen
U hebt een HDInsight-cluster gemaakt. Nu informatie over het werken met uw cluster.

### <a name="apache-hadoop-clusters"></a>Apache Hadoop-clusters
* [Apache Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
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
* [Taken op afstand uitvoeren in een Apache Spark-cluster met behulp van Apache Livy](spark/apache-spark-livy-rest-interface.md)
* [Apache Spark met BI: Interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-hulpprogramma 's](spark/apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](spark/apache-spark-machine-learning-mllib-ipython.md)

