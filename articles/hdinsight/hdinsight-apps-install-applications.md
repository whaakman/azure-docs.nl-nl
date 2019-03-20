---
title: Installeren van toepassingen van derden op Azure HDInsight
description: Informatie over het installeren van Hadoop-toepassingen van derden op Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/01/2019
ms.author: hrasheed
ms.openlocfilehash: 810f9bb81d367cfe70e59d62d81a9e129cf80e6b
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58225976"
---
# <a name="install-third-party-apache-hadoop-applications-on-azure-hdinsight"></a>Apache Hadoop-toepassingen van derden installeren op Azure HDInsight

Informatie over het installeren van een derde partij [Apache Hadoop](https://hadoop.apache.org/) toepassing voor Azure HDInsight. Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

Een HDInsight-toepassing is een toepassing die gebruikers op een HDInsight-cluster installeren kunnen. Deze toepassingen kunnen zijn ontwikkeld door Microsoft, door onafhankelijke softwareleveranciers (ISV) of door u zelf.  

De volgende lijst bevat de gepubliceerde toepassingen:

|Toepassing |Cluster-type | Description |
|---|---|---|
|AtScale Intelligence-Platform |Hadoop |AtScale schakelt het HDInsight-cluster in een scale-out van de OLAP-server, zodat u kunt query miljarden rijen met gegevens interactief met behulp van de BI-hulpprogramma's u al kent, eigenaar en vertrouwt: van Microsoft Excel, Power BI, Tableau Software tot QlikView. |
|CDAP 4.2, 4.3 voor HDInsight |HBase |CDAP is het eerste uniforme integratieplatform voor big data die versnelt de tijd aan de waarde voor Hadoop en kunnen IT-Self-servicegebruikers gegevens te verstrekken. Open-source en uitbreidbaar CDAP Hiermee verwijdert u de barrières voor innovatie. Vereisten: 4 regio knooppunten, min D3 v2. |
|Datameer |Hadoop |Datameer van selfservice schaalbaar platform voor het voorbereiden, verkennen en uw gegevens voor analyse van bestuur versnelt complexe meerdere bronnen gegevens omzetten in waardevolle informatie bedrijfsklare, sneller, slimmer inzichten leveren op een enterprise-schaal. |
|Dataiku DSS op HDInsight |Hadoop, Spark |Dataiku DSS in een enterprise data science-platform waarmee gegevenswetenschappers en gegevensanalisten samenwerken om te ontwerpen en uitvoeren van nieuwe gegevens producten en diensten efficiënter, onbewerkte gegevens omzetten in indrukwekkende voorspellingen. |
|WANdisco Fusion HDI App - 2.12.3, 2.12.1, 2.11.2 |Hadoop, Spark,HBase,Storm,Kafka |Gegevens in een gedistribueerde omgeving consistent blijven is van een grootschalige gegevens bewerkingen. WANdisco Fusion, een hoogwaardige software-platform, lost dit probleem op door in te schakelen van de consistentie van niet-gestructureerde gegevens in elke omgeving. |
|H2O SparklingWater voor HDInsight |Spark |H2O Sparkling Water ondersteunt de volgende gedistribueerde algoritmen: GLM, Naïve Bayes, gedistribueerde willekeurige Forest, Gradient Boosting Machine, Deep Neural Networks, Deep learning, K-means, PCA, lage positie modellen gegeneraliseerd, detectie van afwijkingen, Autoencoders. |
|Realtimeplatform voor de integratie van realtime gegevens met HDInsight |Hadoop,HBase,Storm,Spark,Kafka |Realtimeplatform (uitgesproken ' stream') is een end-to-end streaming gegevensintegratie + intelligence-platform, waardoor continu opname, verwerking en analyse van verschillende gegevensstromen. |
|Jumbune voor HDInsight |Hadoop, Spark |Op hoog niveau helpt Jumbune ondernemingen met 1. MapReduce- en Spark-engine Tez versnellen, op basis van Hive, Java, Scala werkbelasting prestaties. 2. Proactieve Hadoop-Cluster bewaken, 3. Tot stand brengen van het beheer van de kwaliteit van de gegevens op distributed file system. |
|Kyligence Enterprise |Hadoop,HBase,Spark |Mogelijk gemaakt door Apache Kylin, kunt Kyligence Enterprise u BI bij Big Data. Als een enterprise-OLAP-engine op Hadoop kunnen Kyligence Enterprise bedrijfsanalist BI op Hadoop met industriestandaard datawarehouse en BI methodologie ontwerpen. |
|Spark-taakserver voor KNIME Spark-uitvoerder |Spark |Spark-taakserver voor Spark-uitvoerder KNIME wordt gebruikt om de Analyseplatform KNIME verbinden met HDInsight-clusters. |
|Stervorm Presto op Azure HDInsight, stervorm Presto (v0.213-e) |Hadoop |Presto is een snelle en schaalbare gedistribueerde SQL query-engine. Ontworpen voor de scheiding van opslagruimte en rekenkracht, is Presto ideaal voor het opvragen van gegevens in Azure Data Lake Storage, Azure Blob Storage, SQL en NoSQL-databases en andere gegevensbronnen. |
|StreamSets Data Collector voor HDInsight-Cloud |Hadoop,HBase,Spark,Kafka |StreamSets Data Collector is een eenvoudige, krachtige engine waarmee gegevens in realtime gegevensstromen. Gegevensverzamelaar voor route en proces gegevens in uw data-stromen gebruiken. Het wordt geleverd met een licentie voor de proefversie van 30 dagen. |
|[Trifacta Wrangler Enterprise](https://www.trifacta.com/) |Hadoop, Spark,HBase |Trifacta Wrangler Enterprise voor HDInsight ondersteunt bedrijfsbrede data wrangling voor elke schaal van gegevens. De kosten van het Trifacta uitvoeren op Azure is een combinatie van abonnementskosten voor Trifacta, plus de kosten voor Azure-infrastructuur voor de virtuele machines. |
|Unifi gegevensplatform 3.1 |Hadoop,HBase,Storm,Spark |Het gegevensplatform Unifi is een naadloos geïntegreerde suite met gegevens selfservice hulpprogramma's die zijn ontworpen om de zakelijke gebruiker hoofduitdagingen van gegevens aanpakken dat station incrementele inkomsten te verminderen van kosten en operationele complexiteit. |
|Unraveldata APM |Spark |Unravel app van de gegevens voor HDInsight Spark-cluster. |
|Waterlijn Data Catalog |Spark |Waterlijn verzamelt, organiseert en geldt voor gegevens met behulp van AI tot de gegevens automatisch labelen met bedrijfstermen. Waterlijn van zakelijke overweg catalog is een essentieel, succes-onderdeel voor selfservice-analyses, naleving en bestuur en initiatieven voor IT-beheer. |

De instructies in dit artikel zijn bestemd voor gebruik met Azure Portal. U kunt ook de Azure Resource Manager-sjabloon exporteren vanuit de portal of een kopie van de Resource Manager-sjabloon van leveranciers verkrijgen, en Azure PowerShell en de klassieke Azure-CLI gebruiken om de sjabloon te implementeren.  Zie [Apache Hadoop-clusters maken in HDInsight met behulp van Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).

## <a name="prerequisites"></a>Vereisten
Als u HDInsight-toepassingen wilt installeren op een bestaand HDInsight-cluster, hebt u een HDInsight-cluster nodig. Zie [Clusters maken](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) voor informatie over het maken van een cluster. U kunt ook HDInsight-toepassingen installeren wanneer u een HDInsight-cluster maakt.

## <a name="install-applications-to-existing-clusters"></a>Toepassingen installeren in bestaande clusters
De volgende procedure beschrijft hoe u HDInsight-toepassingen in een bestaand HDInsight-cluster installeert.

**Een HDInsight-toepassing installeren**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. In het menu links, gaat u naar **alle services** > **Analytics** > **HDInsight-clusters**.
3. Selecteer een HDInsight-cluster in de lijst.  Als u deze niet hebt, maakt u die eerst.  Zie [Clusters maken](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
4. Onder de **instellingen** categorie, selecteer **toepassingen**. U ziet een lijst met geïnstalleerde toepassingen in het hoofdvenster. 
   
    ![Menu van HDInsight-toepassingenportal](./media/hdinsight-apps-install-applications/hdinsight-apps-portal-menu.png)
5. Selecteer **+ toevoegen** in het menu. U ziet een lijst van beschikbare toepassingen.  Als **+ toevoegen** grijs wordt weergegeven, dat betekent dat er zijn geen toepassingen voor deze versie van het HDInsight-cluster.
   
    ![HDInsight-toepassingen, beschikbare toepassingen](./media/hdinsight-apps-install-applications/hdinsight-apps-list.png)
6. Selecteer een van de beschikbare toepassingen en volg de instructies om de juridische voorwaarden te accepteren.

Hier ziet u de installatiestatus van de van de portalmeldingen (Selecteer het belpictogram boven aan de portal). Nadat de toepassing is geïnstalleerd, wordt de toepassing wordt weergegeven in de lijst met geïnstalleerde Apps.

## <a name="install-applications-during-cluster-creation"></a>Toepassingen installeren tijdens het maken van het cluster
U hebt ook de optie HDInsight-toepassingen te installeren wanneer u een cluster maakt. Tijdens het proces worden HDInsight-toepassingen geïnstalleerd nadat het cluster is gemaakt en actief is. Voor het installeren van toepassingen tijdens het maken van een cluster met behulp van de Azure portal, gebruikt u de **aangepaste** optie in plaats van de standaard **snelle invoer** optie.

## <a name="list-installed-hdinsight-apps-and-properties"></a>Lijst van geïnstalleerde HDInsight-apps en -eigenschappen
De portal toont een lijst van de geïnstalleerde HDInsight-toepassingen voor een cluster en de eigenschappen van elke geïnstalleerde toepassing.

**HDInsight-toepassingen en weergave van de eigenschappen weergeven**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. In het menu links, gaat u naar **alle services** > **Analytics** > **HDInsight-clusters**.
3. Selecteer een HDInsight-cluster in de lijst.
4. Onder de **instellingen** categorie, selecteer **toepassingen**. U ziet een lijst met geïnstalleerde toepassingen in het hoofdvenster. 
   
    ![HDInsight-toepassingen, geïnstalleerde apps](./media/hdinsight-apps-install-applications/hdinsight-apps-installed-apps-with-apps.png)
5. Selecteer een van de geïnstalleerde toepassingen om de eigenschap weer te geven. De lijsten met zoekeigenschappen:

    |Eigenschap | Description |
    |---|---|
    |Naam van app |De naam van de toepassing. |
    |Status |Toepassingsstatus. |
    |Webpagina |De URL van de web-App die u hebt geïmplementeerd op het edge-knooppunt. De referenties zijn dezelfde als de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het cluster. |
    |SSH-eindpunt |U kunt SSH gebruiken om te verbinden met het edge-knooppunt. De SSH-referenties zijn dezelfde als de SSH-gebruikersreferenties die u hebt geconfigureerd voor het cluster. Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie. |
    |Description | Beschrijving van de toepassing. |

6. Als u wilt verwijderen van een toepassing, met de rechtermuisknop op de toepassing en klik vervolgens op **verwijderen** in het contextmenu.

## <a name="connect-to-the-edge-node"></a>Verbinding maken met het edge-knooppunt
U kunt verbinding maken met het edge-knooppunt door middel van HTTP en SSH. Informatie over het eindpunt kunt u vinden in de [portal](#list-installed-hdinsight-apps-and-properties). Zie [SSH-sleutels gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor informatie.

De HTTP-eindpuntreferenties zijn de HTTP-gebruikersreferenties die u hebt geconfigureerd voor het HDInsight-cluster. De SSH-eindpuntreferenties zijn de SSH-referenties die u hebt geconfigureerd voor het HDInsight-cluster.

## <a name="troubleshoot"></a>Problemen oplossen
Zie [Problemen met de installatie oplossen](hdinsight-apps-install-custom-applications.md#troubleshoot-the-installation).

## <a name="next-steps"></a>Volgende stappen
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): Informatie over het publiceren van uw aangepaste HDInsight-toepassingen op Azure Marketplace.
* [MSDN: Een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): Meer informatie over het definiëren van HDInsight-toepassingen.
* [Op Linux gebaseerde HDInsight-clusters aanpassen met behulp van een scriptactie](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van een scriptactie om extra toepassingen te installeren.
* [Apache Hadoop op basis van Linux-clusters in HDInsight met behulp van Resource Manager-sjablonen maken](hdinsight-hadoop-create-linux-clusters-arm-templates.md): informatie over het aanroepen van Resource Manager-sjablonen om HDInsight-clusters te maken.
* [Use empty edge nodes in HDInsight](hdinsight-apps-use-edge-node.md) (Lege edge-knooppunten gebruiken in HDInsight): meer informatie over het gebruik van een leeg edge-knooppunt om toegang te krijgen tot het HDInsight-cluster, HDInsight toepassingen te testen en HDInsight-toepassingen te hosten.

