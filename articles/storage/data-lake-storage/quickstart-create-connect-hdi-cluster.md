---
title: Instellingen voor Hadoop, Spark, Kafka, HBase of R-Server maken-Azure HDInsight-cluster
description: Instellen van Hadoop, Kafka, Spark, HBase, R Server of Storm-clusters voor HDInsight vanuit een browser, de Azure CLI, Azure PowerShell, REST of SDK.
keywords: setup van hadoop-cluster, kafka-cluster setup, spark-cluster setup, wat is hadoop cluster
services: storage
author: jamesbak
tags: azure-portal
ms.component: data-lake-storage-gen2
ms.service: storage
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: e0816e8609ba1ab0ef1b4f685731339378fee844
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525583"
---
# <a name="quickstart-set-up-clusters-in-hdinsight"></a>Snelstartgids: Clusters instellen in HDInsight

In deze snelstartgids leert u het instellen en configureren van clusters in HDInsight met Hadoop, Spark, Kafka, Interactive Query, HBase, R Server of Storm. U leert ook hoe u ze toevoegen aan een domein, clusters aanpassen en koppelen aan [Gen2 Preview van Azure Data Lake Storage](introduction.md).

Een Hadoop-cluster bestaat uit meerdere virtuele machines (knooppunten) die worden gebruikt voor gedistribueerde verwerking van taken. Azure HDInsight verwerkt implementatiegegevens van de installatie en configuratie van afzonderlijke knooppunten, zodat u alleen algemene configuratie-informatie te verstrekken.

> [!IMPORTANT]
>De facturering voor het gebruik van HDInsight-clusters begint zodra er een cluster is gemaakt en stopt als een cluster wordt verwijderd. De facturering wordt pro-rato per minuut berekend, dus u moet altijd uw cluster verwijderen wanneer het niet meer wordt gebruikt. Meer informatie over het [een cluster verwijderen.](../../hdinsight/hdinsight-delete-cluster.md)

Data Lake-opslag wordt gebruikt als de gegevenslaag in deze Quick Start. Met de eigen hiërarchische Namespace-Service en [Hadoop stuurprogramma](abfs-driver.md), Data Lake Storage is geoptimaliseerd voor gedistribueerde verwerking en analyse. In Data Lake-opslag opgeslagen gegevens zich blijft voordoen, zelfs nadat u een HDInsight-cluster wordt verwijderd.

## <a name="cluster-setup-methods"></a>Cluster setup-methoden

De volgende tabel ziet u de verschillende methoden die u gebruiken kunt voor het instellen van een HDInsight-cluster.

| Clusters die zijn gemaakt met | Webbrowser | Opdrachtregel | REST-API | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Azure Portal](../../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](../../hdinsight/hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI (versie 1.0)](../../hdinsight/hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](../../hdinsight/hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](../../hdinsight/hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [Azure Resource Manager-sjablonen](../../hdinsight/hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Snel maken: Basic clusterinstallatie

Dit artikel begeleidt u bij instellingen in de [Azure-portal](https://portal.azure.com), hier kunt u maken een HDInsight-cluster met *snelle invoer* of *aangepaste*. 

![hdinsight opties aangepaste snelle invoer maken](media/quickstart-create-connect-hdi-cluster/hdinsight-creation-options.png)

Volg de instructies op het scherm om een eenvoudige installatie doen. Meer informatie vindt u hieronder voor:

* [De naam van resourcegroep](#resource-group-name)
* [Clustertypen en opties configureren](#cluster-types) 
* [Aanmelding bij cluster en SSH-gebruikersnaam](#cluster-login-and-ssh-user-name)
* [Locatie](#location)

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie, [HDInsight 3.3 buiten gebruik stellen](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="resource-group-name"></a>Resourcegroepnaam

[Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) helpt u het gebruik van de resources in uw toepassing als een groep, aangeduid als een Azure-resourcegroep. U kunt implementeren, bijwerken, bewaken of verwijderen van alle resources voor uw toepassing in één gecoördineerde bewerking.

## <a name="cluster-types"></a> Clustertypen en opties configureren
Azure HDInsight biedt momenteel de volgende clustertypen, elk met een set met onderdelen voor bepaalde functies.

> [!IMPORTANT]
> HDInsight-clusters zijn beschikbaar in verschillende typen, elk voor een enkele workload of technologie. Er is geen ondersteunde methode om een cluster die meerdere typen, zoals Storm en HBase op één cluster combineert te maken. Als uw oplossing vereist technologieën die worden verdeeld over meerdere HDInsight-clustertypen, een [virtueel Azure-netwerk](https://docs.microsoft.com/azure/virtual-network) verbinding kunnen maken van de vereiste clustertypen. 
>
>

| Clustertype | Functionaliteit |
| --- | --- |
| [Hadoop](../../hdinsight/hadoop/apache-hadoop-introduction.md) |Batch-query's en analyses van opgeslagen gegevens |
| [HBase](../../hdinsight/hbase/apache-hbase-overview.md) |Verwerking van grote hoeveelheden schemaloos, NoSQL-gegevens |
| [Interactive Query](../../hdinsight/interactive-query/apache-interactive-query-get-started.md) |Caching in het geheugen voor interactieve en snellere Hive-query 's |
| [Kafka](../../hdinsight/kafka/apache-kafka-introduction.md) | Een gedistribueerd streamingplatform dat kan worden gebruikt voor het bouwen van realtime streaming gegevens-pipelines en toepassingen |
| [R Server](../../hdinsight/r-server/r-server-overview.md) |Verschillende big data-statistieken, voorspellende modellen en machine learning-mogelijkheden |
| [Spark](../../hdinsight/spark/apache-spark-overview.md) |In-memory verwerking, interactieve query's, verwerking van microbatches |
| [Storm](../../hdinsight/storm/apache-storm-overview.md) |Gebeurtenissen in realtime verwerken |

### <a name="hdinsight-version"></a>HDInsight-versie

Kies de versie van HDInsight voor dit cluster. Zie voor meer informatie, [ondersteund HDInsight-versies](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="enterprise-security-package"></a>Enterprise-beveiligingspakket

Voor clustertypen Hadoop, Spark en interactieve Query, kunt u kiezen om in te schakelen de **Enterprise-beveiligingspakket**. Dit pakket biedt een optie om de installatie van een beter te beveiligen met behulp van Apache Ranger en integreren met Azure Active Directory. Zie voor meer informatie, [Enterprise-beveiligingspakket in Azure HDInsight](../../hdinsight/domain-joined/apache-domain-joined-introduction.md).

![hdinsight maken opties kiezen enterprise-beveiligingspakket](./media/quickstart-create-connect-hdi-cluster/hdinsight-creation-enterprise-security-package.png)

Voor meer informatie over het maken van aan domein gekoppelde HDInsight-cluster, raadpleegt u [maken aan domein gekoppelde HDInsight sandbox-omgeving](../../hdinsight/domain-joined/apache-domain-joined-configure.md).

## <a name="cluster-login-and-ssh-user-name"></a>Aanmelding bij cluster en SSH-gebruikersnaam

Met HDInsight-clusters, kunt u twee gebruikersaccounts configureren tijdens het maken van een cluster:

* HTTP-gebruiker: de standaardgebruikersnaam is *admin*. Hierbij wordt de basisconfiguratie in Azure portal. Soms wordt genoemd "Cluster gebruiker."
* SSH-gebruiker (Linux-clusters): gebruikt voor verbinding met het cluster via SSH. Zie [SSH gebruiken met HDInsight](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

De Enterprise-beveiligingspakket kunt u HDInsight integreren met Active Directory en Apache Ranger. Meerdere gebruikers kunnen worden gemaakt met behulp van de Enterprise-beveiligingspakket.

## <a name="location"></a>Locatie (regio's) voor clusters en opslag

U hoeft niet expliciet; opgeven van de locatie van het cluster het cluster zich in dezelfde locatie als de standaardopslag. Voor een lijst met ondersteunde regio's, klikt u op de **regio** vervolgkeuzelijst op [HDInsight prijzen](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Opslag-eindpunten voor clusters

Hoewel een on-premises Hadoop-installatie gebruikmaakt van het Hadoop Distributed File System (HDFS) voor opslag op het cluster, in de cloud kunt u opslag-eindpunten die zijn verbonden met het cluster. HDInsight-clusters gebruiken [Data Lake Storage Gen2](abfs-driver.md) of [Blobs in Azure Storage](../../hdinsight/hdinsight-hadoop-use-blob-storage.md). Met behulp van Azure Storage of Data Lake Storage betekent dat u de HDInsight-clusters die worden gebruikt voor berekeningen en nog steeds behoud uw gegevens veilig kunt verwijderen.

> [!WARNING]
> Met behulp van een extra opslagaccount in een andere locatie van het HDInsight-cluster wordt niet ondersteund.

Tijdens de configuratie voor het standaardeindpunt voor opslag geeft u Data Lake-opslag. De standaardopslag bevat toepassings- en Logboeken. Desgewenst kunt u aanvullende, gekoppelde Azure Data Lake Storage-accounts die toegang hebben tot het cluster. Het HDInsight-cluster en de afhankelijke opslagaccounts moeten zich in dezelfde Azure-locatie.

![Instellingen voor de opslag van cluster: HDFS-compatibele eindpunten van opslag](media/quickstart-create-connect-hdi-cluster/hdinsight-cluster-creation-storage2.png)

> [!IMPORTANT]
> Zorg ervoor dat u **Data Lake Store-toegang uitschakelen**. Deze instelling verwijst naar de oude *Data Lake Store* functionaliteit en moet worden uitgeschakeld zodat *Data Lake Storage* functies goed te laten werken.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="optional-metastores"></a>Optionele metastores
U kunt optioneel Hive of Oozie metastores kunt maken. Echter niet alle clustertypen ondersteunen metastores en Azure SQL Data Warehouse is niet compatibel met metastores. 

Zie voor meer informatie, [externe metagegevensopslag in Azure HDInsight gebruiken](../../hdinsight/hdinsight-use-external-metadata-stores.md).

> [!IMPORTANT]
> Wanneer u een aangepaste metastore maakt, gebruik geen streepjes, streepjes of spaties in de naam van de database. Hierdoor kan het proces voor het maken van cluster mislukt.

### <a name="use-hiveoozie-metastore"></a>Hive-metastore

Als u behouden die uw Hive-tabellen wilt nadat u een HDInsight-cluster verwijderen, gebruikt u een aangepaste metastore. U kunt vervolgens de metastore koppelen aan een ander HDInsight-cluster.

Een HDInsight-metastore die is gemaakt voor een HDInsight-clusterversie kan niet worden gedeeld tussen verschillende versies van HDInsight-cluster. Zie voor een lijst van HDInsight-versies, [ondersteund HDInsight-versies](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Oozie-metastore

Voor betere prestaties bij het gebruik van Oozie, gebruikt u een aangepaste metastore. Een metastore biedt ook toegang tot taakgegevens Oozie nadat u uw cluster hebt verwijderd. 

> [!IMPORTANT]
> U kunt een aangepaste Oozie-metastore kan niet opnieuw gebruiken. Voor het gebruik van een aangepaste Oozie-metastore, moet u een lege Azure SQL-Database opgeven bij het maken van het HDInsight-cluster.

## <a name="configure-cluster-size"></a>Grootte van cluster configureren

U wordt gefactureerd voor gebruik voor knooppunt, zolang het cluster bestaat. Facturering wordt gestart wanneer een cluster is gemaakt en stopt als een cluster wordt verwijderd. Clusters kunnen niet worden toewijzing ongedaan gemaakt of in de wachtstand plaatsen.


### <a name="number-of-nodes-for-each-cluster-type"></a>Aantal knooppunten voor elk clustertype
Elk clustertype heeft een eigen aantal knooppunten, terminologie voor knooppunten en standaard VM-grootte. In de volgende tabel is het aantal knooppunten voor elk knooppunttype tussen haakjes.

| Type | Knooppunten | Diagram |
| --- | --- | --- |
| Hadoop |Hoofdknooppunt (2), gegevensknooppunt (1 +) |![De knooppunten van HDInsight Hadoop](media/quickstart-create-connect-hdi-cluster/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |Hoofd-server (2), de regioserver (1 +), hoofdknooppunten/ZooKeeper-knooppunten (3) |![De knooppunten van HDInsight HBase](media/quickstart-create-connect-hdi-cluster/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nimbus-knooppunt (2), supervisor server (1 +), ZooKeeper-knooppunt (3) |![Storm voor HDInsight-clusterknooppunten](media/quickstart-create-connect-hdi-cluster/hdinsight-storm-cluster-type-setup.png) |
| Spark |Hoofdknooppunt (2), worker-knooppunt (1 +), ZooKeeper-knooppunten (3) (gratis voor A1 ZooKeeper VM-grootte) |![Knooppunten voor HDInsight Spark-cluster](media/quickstart-create-connect-hdi-cluster/hdinsight-spark-cluster-type-setup.png) |

Zie voor meer informatie, [standaard configuratie en de virtuele machine knooppuntgrootten voor clusters](../../hdinsight/hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) in "Wat zijn de Hadoop-onderdelen en versies in HDInsight?"

De kosten van HDInsight-clusters wordt bepaald door het aantal knooppunten en de grootte van virtuele machines voor de knooppunten. 

Verschillende clustertypen hebben verschillende knooppunttypen, aantal knooppunten en knooppuntgrootten:
* Hadoop-cluster het standaardtype: 
    * Twee *hoofdknooppunten*  
    * Vier *gegevensknooppunten*
* Storm-cluster het standaardtype: 
    * Twee *Nimbus-knooppunten*
    * Drie *ZooKeeper-knooppunten*
    * Vier *supervisorknooppunten* 

Als u HDInsight slechts uitprobeert, raden wij dat u één gegevensknooppunt gebruiken. Zie voor meer informatie over de prijzen voor HDInsight [HDInsight prijzen](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> De limiet voor de clustergrootte verschilt per Azure-abonnementen. Neem contact op met [Azure ondersteuning voor facturering](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) om de limiet te verhogen.
>

Wanneer u de Azure-portal gebruikt om het cluster te configureren, de grootte van het knooppunt is beschikbaar via de **knooppunt Prijscategorieën** blade. In de portal, kunt u ook de kosten die gepaard gaan met de grootte van verschillende bekijken. 

![Grootte van HDInsight VM](media/quickstart-create-connect-hdi-cluster/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Grootten van virtuele machines 
Wanneer u clusters implementeert, kiest u de compute-resources op basis van de oplossing die u van plan bent om te implementeren. De volgende virtuele machines worden gebruikt voor HDInsight-clusters:
* A- en D1-4-serie VM's: [General-purpose Linux VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* D11-14-serie VM: [geoptimaliseerd voor geheugen Linux-VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Om erachter te komen welke u waarde moeten gebruiken een VM-grootte opgeven tijdens het maken van een cluster met behulp van de verschillende SDK's of tijdens het gebruik van Azure PowerShell, Zie [VM-grootten voor HDInsight-clusters](../../cloud-services/cloud-services-sizes-specs.md#size-tables). In dit artikel gekoppeld, gebruikt u de waarde in de **grootte** kolom van de tabellen.

> [!IMPORTANT]
> Als u meer dan 32 worker-knooppunten in een cluster, moet u de grootte van een hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB aan RAM-geheugen.
>
>

Zie voor meer informatie, [grootten voor virtuele machines](../../virtual-machines/windows/sizes.md). Zie voor meer informatie over de prijzen van de verschillende grootten [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="custom-cluster-setup"></a>Aangepaste installatie
Aangepaste cluster setup bouwt voort op de snelle instellingen maken en voegt de volgende opties:
- [HDInsight-toepassingen](#hdinsight-applications)
- [Grootte van cluster](#cluster-size)
- Geavanceerde instellingen
  - [Scriptacties](#customize-clusters-using-script-action)
  - [Virtueel netwerk](#use-virtual-network)

## <a name="install-hdinsight-applications-on-clusters"></a>HDInsight-toepassingen installeren op clusters

Een HDInsight-toepassing is een toepassing die gebruikers kunnen installeren op een op Linux gebaseerd HDInsight-cluster. U kunt toepassingen geleverd door Microsoft, externe partijen of die u zelf ontwikkelen. Zie voor meer informatie, [Hadoop-toepassingen van derden installeren op Azure HDInsight](../../hdinsight/hdinsight-apps-install-applications.md).

De meeste van de HDInsight-toepassingen zijn geïnstalleerd op een lege edge-knooppunt.  Een lege edge-knooppunt is een Linux-machine met de dezelfde clienthulpprogramma's geïnstalleerd en geconfigureerd zoals in het hoofdknooppunt. U kunt het edge-knooppunt gebruiken voor toegang tot het cluster en hosten van uw client-apps testen van uw clienttoepassingen. Zie voor meer informatie, [lege edge-knooppunten gebruiken in HDInsight](../../hdinsight/hdinsight-apps-use-edge-node.md).

## <a name="advanced-settings-script-actions"></a>Geavanceerde instellingen: scriptacties

U kunt extra onderdelen installeren of de configuratie van het cluster aanpassen met behulp van scripts tijdens het maken van. Deze scripts worden aangeroepen **scriptactie**, dit is een configuratieoptie die kan worden gebruikt vanuit de Azure portal, HDInsight Windows PowerShell-cmdlets of de HDInsight .NET SDK. Zie voor meer informatie, [aanpassen HDInsight-cluster met Script Action](../../hdinsight/hdinsight-hadoop-customize-cluster-linux.md).

Sommige systeemeigen Java-onderdelen, zoals Mahout en trapsgewijze, kunnen worden uitgevoerd op het cluster als Java-archief (JAR)-bestanden. Deze JAR-bestanden kunnen worden gedistribueerd naar Azure Storage of Azure Data Lake Storage en verzonden naar HDInsight-clusters met Hadoop-taak indienen mechanismen. Zie voor meer informatie, [Hadoop indienen via een programma taken](../../hdinsight/hadoop/submit-apache-hadoop-jobs-programmatically.md).

> [!NOTE]
> Als u problemen met het JAR-bestanden implementeren met HDInsight-clusters hebt, of aanroepen van de JAR-bestanden op HDInsight-clusters, contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).
>
> Trapsgewijze wordt niet ondersteund door HDInsight en is niet in aanmerking komen voor Microsoft Support. Zie voor lijsten met ondersteunde onderdelen, [wat is er nieuw in de clusterversies geleverd door HDInsight](../../hdinsight/hdinsight-component-versioning.md).
>
>

Soms wilt u de volgende configuratiebestanden tijdens het maakproces configureren:

* clusterIdentity.xml
* Core-site.xml
* gateway.XML
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Zie voor meer informatie, [aanpassen HDInsight-clusters met Bootstrap](../../hdinsight/hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Geavanceerde instellingen: clusters met een virtueel netwerk uitbreiden
Als uw oplossing vereist technologieën die worden verdeeld over meerdere HDInsight-clustertypen, een [virtueel Azure-netwerk](../../hdinsight/https://docs.microsoft.com/azure/virtual-network) verbinding kunnen maken van de vereiste clustertypen. Deze configuratie staat de clusters, en een code die u voor ze rechtstreeks kan communiceren met elkaar implementeren.

Zie voor meer informatie over het gebruik van een Azure-netwerk met HDInsight [HDInsight uitbreiden met Azure virtual networks](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md).

Zie voor een voorbeeld van het gebruik van twee clustertypen binnen een virtueel Azure-netwerk, [gebruik Spark Structured Streaming met Kafka](../../hdinsight/hdinsight-apache-kafka-spark-structured-streaming.md). Zie voor meer informatie over het gebruik van HDInsight met een virtueel netwerk, met inbegrip van specifieke configuratievereisten voor het virtuele netwerk, [mogelijkheden voor HDInsight uitbreiden met behulp van Azure Virtual Network](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md).

## <a name="troubleshoot-access-control-issues"></a>Problemen met toegang beheren

Zie [Vereisten voor toegangsbeheer](../../hdinsight/hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen

- [Hadoop-bestandssysteem ABFS-stuurprogramma voor Azure Data Lake Storage Gen2](abfs-driver.md)
- [Zelfstudie: Gegevens uitpakken, transformeren en laden met Apache Hive in Azure HDInsight](tutorial-extract-transform-load-hive.md)
- [Wat zijn HDInsight, de Hadoop-ecosysteem en Hadoop-clusters?](../../hdinsight/hadoop/apache-hadoop-introduction.md)
- [Aan de slag met Hadoop in HDInsight](../../hdinsight/hadoop/apache-hadoop-linux-tutorial-get-started.md)
- [Werken in Hadoop op HDInsight vanaf een Windows-PC](../../hdinsight/hdinsight-hadoop-windows-tools.md)
