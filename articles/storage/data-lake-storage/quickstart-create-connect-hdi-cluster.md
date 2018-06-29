---
title: Setup voor Hadoop, Spark, HBase, Kafka of R Server - Azure HDInsight-cluster
description: Stel Hadoop, Kafka, Spark, HBase, R Server of Storm-clusters voor HDInsight vanuit een browser, de Azure CLI, Azure PowerShell, REST of SDK.
keywords: setup van hadoop-cluster, kafka configuratie van een cluster, spark-cluster setup, wat is hadoop-cluster
services: hdinsight
documentationcenter: ''
author: jamesbak
manager: jahogg
tags: azure-portal
ms.component: data-lake-storage-gen2
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: 6707610fc3410eab6eb2985c7d8a0ef821354689
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37062076"
---
# <a name="quickstart-set-up-clusters-in-hdinsight"></a>Snelstartgids: Clusters in HDInsight instellen

In deze snelstartgids u informatie over het instellen en configureren van clusters in HDInsight met Hadoop, Spark, Kafka, interactieve Query, HBase, R Server of Storm. U leert ook clusters aanpassen, voeg ze toe aan een domein en koppelen aan [Gen2 Preview van Azure Data Lake Storage](introduction.md).

Een Hadoop-cluster bestaat uit meerdere virtuele machines (knooppunten) die worden gebruikt voor gedistribueerde verwerking van taken. Azure HDInsight verwerkt implementatiegegevens van de installatie en configuratie van afzonderlijke knooppunten, hoeft u alleen om algemene configuratie-informatie te geven.

> [!IMPORTANT]
>De facturering voor het gebruik van HDInsight-clusters begint zodra er een cluster is gemaakt en stopt als een cluster wordt verwijderd. De facturering wordt pro-rato per minuut berekend, dus u moet altijd uw cluster verwijderen wanneer het niet meer wordt gebruikt. Meer informatie over hoe [verwijderen van een cluster.](../../hdinsight/hdinsight-delete-cluster.md)

Data Lake Storage wordt gebruikt als de gegevenslaag in deze snelstartgids. Met de hiërarchische Namespace-Service en [Hadoop stuurprogramma](abfs-driver.md), Data Lake Storage is geoptimaliseerd voor gedistribueerde verwerking en analyse. Gegevens die zijn opgeslagen in Data Lake Storage persistente zelfs nadat u een HDInsight-cluster is verwijderd.

## <a name="cluster-setup-methods"></a>Cluster setup-methoden

De volgende tabel bevat de verschillende methoden die u gebruiken kunt voor het instellen van een HDInsight-cluster.

| Clusters die zijn gemaakt met | Webbrowser | Opdrachtregel | REST-API | SDK | 
| --- |:---:|:---:|:---:|:---:|
| [Azure Portal](../../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](../../hdinsight/hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [Azure CLI ({ver 1.0)](../../hdinsight/hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Azure PowerShell](../../hdinsight/hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [cURL](../../hdinsight/hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [Azure Resource Manager-sjablonen](../../hdinsight/hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Snelle invoer: Basic clusterinstallatie

Dit artikel begeleidt u bij de instellingen in de [Azure-portal](https://portal.azure.com), waarin kunt u een HDInsight-cluster gebruikt *snelle invoer* of *aangepaste*. 

![hdinsight opties aangepaste snelle invoer maken](media/quickstart-create-connect-hdi-cluster/hdinsight-creation-options.png)

Volg de instructies op het scherm een basic clusterinstallatie doen. Details worden hieronder:

* [De naam van resourcegroep](#resource-group-name)
* [Clustertypen en configuratie](#cluster-types) 
* [Cluster-aanmelding en SSH-gebruikersnaam](#cluster-login-and-ssh-username)
* [Locatie](#location)

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie [HDInsight 3.3 buiten gebruik stellen](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="resource-group-name"></a>Resourcegroepnaam

[Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) helpt u werkt met de resources in uw toepassing als een groep aangeduid als een Azure-resourcegroep. U kunt implementeren, bijwerken, bewaken of verwijderen van alle resources voor uw toepassing in een enkele, gecoördineerde bewerking.

## <a name="cluster-types"></a> Clustertypen en configuratie
Azure HDInsight biedt momenteel de volgende clustertypen, elk met een reeks onderdelen voor bepaalde functies.

> [!IMPORTANT]
> HDInsight-clusters zijn beschikbaar in verschillende typen, elk voor een enkele workload of technologie. Er is geen ondersteunde methode om een cluster waarin meerdere typen zoals Storm en HBase op één cluster te maken. Als uw oplossing technologieën die worden verdeeld over meerdere HDInsight-clustertypen vereist een [virtuele Azure-netwerk](https://docs.microsoft.com/azure/virtual-network) verbinding kunnen maken van de vereiste clustertypen. 
>
>

| Clustertype | Functionaliteit |
| --- | --- |
| [Hadoop](../../hdinsight/hadoop/apache-hadoop-introduction.md) |Batch-query's en analyse van opgeslagen gegevens |
| [HBase](../../hdinsight/hbase/apache-hbase-overview.md) |Voor grote hoeveelheden schemaloos, NoSQL-gegevens verwerken |
| [Interactieve Query](../../hdinsight/interactive-query/apache-interactive-query-get-started.md) |In-memory caching voor interactieve en sneller Hive-query 's |
| [Kafka](../../hdinsight/kafka/apache-kafka-introduction.md) | Een gedistribueerde streaming-platform die kan worden gebruikt voor het bouwen van realtime streaming gegevenspijplijnen en toepassingen |
| [R Server](../../hdinsight/r-server/r-server-overview.md) |Diverse big data-statistieken, voorspellende modellen en machine learning-mogelijkheden |
| [Spark](../../hdinsight/spark/apache-spark-overview.md) |In-memory verwerking, interactieve query's micro batch stroom verwerken |
| [Storm](../../hdinsight/storm/apache-storm-overview.md) |Gebeurtenissen in realtime verwerken |

### <a name="hdinsight-version"></a>HDInsight-versie

Selecteer de versie van HDInsight voor dit cluster. Zie voor meer informatie [versies van HDInsight ondersteund](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="enterprise-security-package"></a>Enterprise-beveiligingspakket

Voor Hadoop, Spark en interactieve Query clustertypen, kunt u kiezen om in te schakelen de **Enterprise-beveiligingspakket**. Dit pakket biedt een optie om de installatie van een beter te beveiligen met behulp van Apache Zwerver en integreren met Azure Active Directory. Zie voor meer informatie [Enterprise-beveiligingspakket in Azure HDInsight](../../hdinsight/domain-joined/apache-domain-joined-introduction.md).

![hdinsight maken opties kiezen enterprise-beveiligingspakket](./media/quickstart-create-connect-hdi-cluster/hdinsight-creation-enterprise-security-package.png)

Voor meer informatie over het maken van domein HDInsight-cluster, Zie [maken domein HDInsight sandbox-omgeving](../../hdinsight/domain-joined/apache-domain-joined-configure.md).

## <a name="cluster-login-and-ssh-user-name"></a>Cluster-aanmelding en SSH-gebruikersnaam

Met HDInsight-clusters, kunt u twee gebruikersaccounts configureren tijdens het maken van het cluster:

* HTTP-gebruiker: de standaardgebruikersnaam is *admin*. Dit maakt gebruik van de basisconfiguratie in de Azure portal. Dit wordt wel "Cluster gebruiker."
* SSH-gebruiker (Linux-clusters): verbinding maken met het cluster via SSH. Zie [SSH gebruiken met HDInsight](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

Het beveiligingspakket Enterprise kunt u HDInsight integreren met Active Directory en Apache Zwerver. Meerdere gebruikers kunnen worden gemaakt met het beveiligingspakket Enterprise.

## <a name="location"></a>Locatie (regio's) voor clusters en opslag

U hoeft niet te geeft u de locatie van het cluster expliciet; het cluster zich op dezelfde locatie als de standaard-opslag. Voor een lijst met ondersteunde regio's, klikt u op de **regio** vervolgkeuzelijst op [HDInsight prijzen](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Opslag-eindpunten voor clusters

Hoewel een lokale installatie van Hadoop Hadoop Distributed File System (HDFS) voor opslag op het cluster gebruikt, in de cloud kunt u opslag-eindpunten die zijn verbonden met het cluster. HDInsight-clusters gebruiken [Data Lake Storage Gen2](abfs-driver.md) of [Blobs in Azure Storage](../../hdinsight/hdinsight-hadoop-use-blob-storage.md). Met behulp van Azure Storage of de Data Lake Storage betekent dat u de HDInsight-clusters gebruikt voor berekeningen en nog steeds behoud uw gegevens veilig kunt verwijderen.

> [!WARNING]
> Met een account extra opslagruimte in een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

Tijdens de configuratie voor het standaardeindpunt opslag u Data Lake Storage. De standaard opslag bevat toepassings- en Logboeken. U kunt desgewenst aanvullende gekoppelde Azure Data Lake Storage-accounts toegang het cluster tot hebben opgeven. Het HDInsight-cluster en de afhankelijke storage-accounts moeten in dezelfde Azure-locatie.

![Instellingen voor de opslag van cluster: HDFS-compatibele eindpunten van opslag](media/quickstart-create-connect-hdi-cluster/hdinsight-cluster-creation-storage-TEMP.png)

> [!IMPORTANT]
> Zorg ervoor dat **Data Lake Store toegang uitschakelen**. Deze instellingen verwijst naar de oude *Data Lake Store* functionaliteit en moet worden uitgeschakeld om *Data Lake Storage* functies goed te laten werken.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="optional-metastores"></a>Optionele metastores
U kunt optioneel Hive of Oozie metastores kunt maken. Niet alle clustertypen ondersteunen metastores echter en Azure SQL Data Warehouse is niet compatibel met metastores. 

Zie voor meer informatie [archieven van de externe metagegevens in Azure HDInsight](../../hdinsight/hdinsight-use-external-metadata-stores.md).

> [!IMPORTANT]
> Wanneer u een aangepaste metastore maakt, gebruik geen streepjes, streepjes of spaties in de naam van de database. Hierdoor kan het proces voor het maken van cluster mislukken.

### <a name="use-hiveoozie-metastore"></a>Hive-metastore

Als u behouden die uw Hive-tabellen wilt nadat u een HDInsight-cluster verwijderen, gebruikt u een aangepaste metastore. Vervolgens kunt u de metastore koppelen aan een andere HDInsight-cluster.

Een HDInsight-metastore die is gemaakt voor een HDInsight-cluster versie kan niet worden gedeeld tussen verschillende versies van HDInsight-cluster. Zie voor een lijst met versies van HDInsight, [versies van HDInsight ondersteund](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Oozie-metastore

Gebruik voor betere prestaties bij gebruik van Oozie, een aangepaste metastore. Een metastore biedt ook toegang tot de taakgegevens Oozie nadat u uw cluster hebt verwijderd. 

> [!IMPORTANT]
> U kunt een aangepaste Oozie-metastore niet opnieuw gebruiken. Voor het gebruik van een aangepaste Oozie-metastore, moet u een leeg Azure SQL Database opgeven bij het maken van het HDInsight-cluster.

## <a name="configure-cluster-size"></a>De clustergrootte van het configureren

U wordt gefactureerd voor gebruik van knooppunt voor zolang het cluster bestaat. Omdat facturering begint wanneer een cluster wordt gemaakt en stopt wanneer het cluster wordt verwijderd. Clusters niet ongedaan worden toegewezen of in de wachtstand plaatsen.


### <a name="number-of-nodes-for-each-cluster-type"></a>Het aantal knooppunten voor elk clustertype
Elk clustertype heeft een eigen aantal knooppunten, terminologie voor knooppunten en standaard VM-grootte. In de volgende tabel is het aantal knooppunten voor elk knooppunttype tussen haakjes.

| Type | Knooppunten | Diagram |
| --- | --- | --- |
| Hadoop |Hoofdknooppunt (2), gegevensknooppunt (1 +) |![Clusterknooppunten HDInsight Hadoop](media/quickstart-create-connect-hdi-cluster/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |HEAD-server (2), de regio-server (1 +) master/ZooKeeper-knooppunt (3) |![Clusterknooppunten HDInsight HBase](media/quickstart-create-connect-hdi-cluster/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nimbus-knooppunt (2), supervisor server (1 +), ZooKeeper-knooppunt (3) |![HDInsight Storm clusterknooppunten](media/quickstart-create-connect-hdi-cluster/hdinsight-storm-cluster-type-setup.png) |
| Spark |Hoofdknooppunt (2), werkrolknooppunt (1 +), ZooKeeper-knooppunt (3) (gratis voor A1 ZooKeeper VM-grootte) |![HDInsight Spark clusterknooppunten](media/quickstart-create-connect-hdi-cluster/hdinsight-spark-cluster-type-setup.png) |

Zie voor meer informatie [standaard configuratie en de virtuele machine knooppuntgrootten voor clusters](../../hdinsight/hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) in "Wat zijn de onderdelen en -versies in HDInsight?"

De kosten van het HDInsight-clusters wordt bepaald door het aantal knooppunten en de grootte van de virtuele machines voor de knooppunten. 

Verschillende clustertypen hebben verschillende knooppunttypen, aantal knooppunten en knooppuntgrootten:
* Standaardtype voor Hadoop-cluster: 
    * Twee *hoofdknooppunten*  
    * Vier *gegevensknooppunten*
* Standaardtype storm-cluster: 
    * Twee *Nimbus-knooppunten*
    * Drie *ZooKeeper-knooppunten*
    * Vier *supervisor knooppunten* 

Als u alleen uit HDInsight probeert, raden wij dat u één gegevensknooppunt gebruiken. Zie voor meer informatie over prijzen voor HDInsight [HDInsight prijzen](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> De limiet voor de clustergrootte verschilt per Azure-abonnementen. Neem contact op met [ondersteuning voor facturering aan Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) om de limiet te verhogen.
>

Wanneer u de Azure-portal gebruikt om de cluster te configureren, de grootte van het knooppunt is beschikbaar via de **knooppunt Prijscategorieën** blade. Zie ook de kosten die zijn gekoppeld aan de grootte van het andere knooppunt in de portal. 

![HDInsight VM knooppuntgrootten](media/quickstart-create-connect-hdi-cluster/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Grootten van virtuele machines 
Wanneer u clusters implementeert, kiest u rekenresources op basis van de oplossing die u wilt implementeren. De volgende virtuele machines worden gebruikt voor HDInsight-clusters:
* A en D1 4 reeks VMs: [General-purpose Linux VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* D11 14 reeks VM: [geoptimaliseerd voor geheugen Linux VM-grootten](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Om erachter te komen welke u waarde moeten gebruiken voor een VM-grootte opgeven tijdens het maken van een cluster met behulp van de verschillende SDK's of tijdens het gebruik van Azure PowerShell, Zie [VM-grootten voor HDInsight-clusters](../../cloud-services/cloud-services-sizes-specs.md#size-tables). In dit artikel gekoppelde, gebruikt u de waarde in de **grootte** kolom van de tabellen.

> [!IMPORTANT]
> Als u meer dan 32 worker-knooppunten in een cluster, moet u een grootte van het hoofdknooppunt met ten minste 8 kerngeheugens en 14 GB RAM-geheugen.
>
>

Zie voor meer informatie [grootten voor virtuele machines](../../virtual-machines/windows/sizes.md). Zie voor meer informatie over prijzen van de verschillende grootten [HDInsight prijzen](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="custom-cluster-setup"></a>Aangepaste installatie
Aangepaste cluster setup bouwt voort op de snelle instellingen maken en voegt de volgende opties:
- [HDInsight-toepassingen](#hdinsight-applications)
- [De clustergrootte van het](#cluster-size)
- Geavanceerde instellingen
  - [Scriptacties](#customize-clusters-using-script-action)
  - [Virtueel netwerk](#use-virtual-network)

## <a name="install-hdinsight-applications-on-clusters"></a>HDInsight-toepassingen installeren op clusters

Een HDInsight-toepassing is een toepassing die gebruikers kunnen installeren op een op Linux gebaseerd HDInsight-cluster. U kunt toepassingen gebruiken, mits door Microsoft, derden of die u zelf. Zie voor meer informatie [Hadoop-toepassingen van derden installeren op Azure HDInsight](../../hdinsight/hdinsight-apps-install-applications.md).

De meeste van de HDInsight-toepassingen zijn geïnstalleerd op een lege edge-knooppunt.  Een leeg edge-knooppunt is een virtuele Linux-machine met de dezelfde clienthulpprogramma's geïnstalleerd en geconfigureerd zoals in het hoofdknooppunt. U kunt het edge-knooppunt gebruiken voor toegang tot het cluster, testen van uw clienttoepassingen en die als host fungeert voor uw clienttoepassingen. Zie voor meer informatie [leeg edge-knooppunten gebruiken in HDInsight](../../hdinsight/hdinsight-apps-use-edge-node.md).

## <a name="advanced-settings-script-actions"></a>Geavanceerde instellingen: Script acties

U kunt extra onderdelen installeren of clusterconfiguratie aanpassen met behulp van scripts tijdens het maken van. Deze scripts worden aangeroepen **scriptactie**, dit is een configuratieoptie die kan worden gebruikt vanuit de Azure-portal, HDInsight Windows PowerShell-cmdlets of de HDInsight .NET SDK. Zie voor meer informatie [aanpassen HDInsight-cluster via scriptactie](../../hdinsight/hdinsight-hadoop-customize-cluster-linux.md).

Sommige systeemeigen Java-onderdelen, zoals Mahout en trapsgewijze, kunnen worden uitgevoerd op het cluster als Java-archief (JAR)-bestanden. Deze JAR-bestanden worden gedistribueerd naar Azure Storage of Azure Data Lake Storage en verzonden naar HDInsight-clusters met Hadoop taak verzending mechanismen. Zie voor meer informatie [Hadoop verzenden via een programma taken](../../hdinsight/hadoop/submit-apache-hadoop-jobs-programmatically.md).

> [!NOTE]
> Als u problemen met het implementeren van de JAR-bestanden op HDInsight-clusters hebt, of het aanroepen van de JAR-bestanden op HDInsight-clusters, neem contact op met [Microsoft Support](https://azure.microsoft.com/support/options/).
>
> Trapsgewijze bewerking wordt niet ondersteund door HDInsight en is niet in aanmerking komen voor Microsoft Support. Zie voor een lijst met ondersteunde onderdelen, [wat is er nieuw in de clusterversies geleverd door HDInsight](../../hdinsight/hdinsight-component-versioning.md).
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

Zie voor meer informatie [aanpassen HDInsight-clusters met behulp van de Bootstrap](../../hdinsight/hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Geavanceerde instellingen: clusters met een virtueel netwerk uitbreiden
Als uw oplossing technologieën die worden verdeeld over meerdere HDInsight-clustertypen vereist een [virtuele Azure-netwerk](../../hdinsight/https://docs.microsoft.com/azure/virtual-network) verbinding kunnen maken van de vereiste clustertypen. Deze configuratie kan de clusters en alle code die u implementeert voor ze rechtstreeks met elkaar communiceren.

Zie voor meer informatie over het gebruik van een virtuele Azure-netwerk met HDInsight [HDInsight uitbreiden met virtuele netwerken van Azure](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md).

Zie voor een voorbeeld van het gebruik van twee clustertypen binnen een virtuele Azure-netwerk [gebruik Spark gestructureerde streamen met Kafka](../../hdinsight/hdinsight-apache-kafka-spark-structured-streaming.md). Zie voor meer informatie over het gebruik van HDInsight met een virtueel netwerk, met inbegrip van specifieke configuratievereisten voor het virtuele netwerk [mogelijkheden uitbreiden HDInsight met behulp van Azure Virtual Network](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md).

## <a name="troubleshoot-access-control-issues"></a>Besturingselement toegangsproblemen

Zie [Vereisten voor toegangsbeheer](../../hdinsight/hdinsight-administer-use-portal-linux.md#create-clusters) als u problemen ondervindt met het maken van HDInsight-clusters.

## <a name="next-steps"></a>Volgende stappen

- [Hadoop-bestandssysteem ABFS stuurprogramma voor Azure Data Lake Storage Gen2](abfs-driver.md)
- [Zelfstudie: Extraheren, transformeren en laden van gegevens met behulp van Apache Hive in Azure HDInsight](tutorial-extract-transform-load-hive.md)
- [Wat zijn HDInsight, het Hadoop-ecosysteem en Hadoop-clusters?](../../hdinsight/hadoop/apache-hadoop-introduction.md)
- [Aan de slag met Hadoop in HDInsight](../../hdinsight/hadoop/apache-hadoop-linux-tutorial-get-started.md)
- [Werken in Hadoop in HDInsight via Windows-PC](../../hdinsight/hdinsight-hadoop-windows-tools.md)
