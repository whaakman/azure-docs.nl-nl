---
title: Azure HDInsight-architectuur voor virtueel netwerk
description: Informatie over de beschikbare resources wanneer u een HDInsight-cluster in een Azure-netwerk maakt.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: hrasheed
ms.openlocfilehash: 6d92273298c0448d7377acab6f3b8ea1cc1ed908
ms.sourcegitcommit: 09bb15a76ceaad58517c8fa3b53e1d8fec5f3db7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58762919"
---
# <a name="azure-hdinsight-virtual-network-architecture"></a>Azure HDInsight-architectuur voor virtueel netwerk

In dit artikel wordt uitgelegd dat de resources die aanwezig zijn wanneer u een HDInsight-cluster in een aangepaste Azure Virtual Network implementeren. Deze informatie helpt u om on-premises bronnen in uw HDInsight-cluster in Azure verbinding te maken. Zie voor meer informatie over Azure Virtual Networks [wat is Azure Virtual Network?](../virtual-network/virtual-networks-overview.md).

## <a name="resource-types-in-azure-hdinsight-clusters"></a>Resourcetypen in Azure HDInsight-clusters

Azure HDInsight-clusters hebben verschillende typen virtuele machines of knooppunten. Elk knooppunttype speelt een rol in de werking van het systeem. De volgende tabel geeft een overzicht van deze typen en hun rollen in het cluster.

| Type | Description |
| --- | --- |
| Hoofdknooppunt |  De hoofdknooppunten hosten voor alle clustertypen met uitzondering van Apache Storm, de processen die uitvoer van de gedistribueerde toepassing te beheren. Het hoofdknooppunt is ook het knooppunt dat u SSH naar kunt en uitvoeren van toepassingen die vervolgens worden gecoördineerd om uit te voeren voor de clusterresources. Het aantal hoofdknooppunten, twee voor alle clustertypen is opgelost. |
| ZooKeeper-knooppunt | Zookeeper-coördinaten taken tussen de knooppunten die gegevensverwerking aan het doen zijn. Ook wordt de selectie van leider van het hoofdknooppunt en houdt van welke hoofdknooppunt van een specifieke hoofd-service wordt uitgevoerd. Het aantal ZooKeeper-knooppunten wordt vastgesteld op twee. |
| Werkknooppunt | Hiermee geeft u de knooppunten die ondersteuning bieden voor gegevensverwerking functionaliteit. Worker-knooppunten kunnen worden toegevoegd of verwijderd uit het cluster schalen computing capaciteit en kosten beheren. |
| R Server edge-knooppunt | Het R Server edge-knooppunt staat voor het knooppunt kunt u SSH op en voer toepassingen die vervolgens worden gecoördineerd om uit te voeren voor de clusterresources. Een edge-knooppunt deelnemen niet aan gegevensanalyse binnen het cluster. Dit knooppunt ook als host fungeert voor R Studio Server, zodat u kunt R-toepassing met behulp van een browser uit te voeren. |
| Knooppunt regio | Voor het type van HBase-cluster, wordt in het knooppunt regio is (ook wel een knooppunt gegevens genoemd) de regio-Server wordt uitgevoerd. Regioservers fungeren en een gedeelte van de gegevens die worden beheerd door HBase te beheren. Regio-knooppunten kunnen worden toegevoegd of verwijderd uit het cluster schalen computing capaciteit en kosten beheren.|
| Nimbus-knooppunt | Voor het type van Storm-cluster biedt het Nimbus-knooppunt functionaliteit die vergelijkbaar is met het hoofdknooppunt. Het Nimbus-knooppunt wijst taken toe aan andere knooppunten in een cluster via Zookeeper, die de uitvoering van Storm-topologieën coördineert. |
| Supervisor knooppunt | Voor het type van Storm-cluster voert het supervisor-knooppunt de instructies van het Nimbus-knooppunt tot het uitvoeren van de gewenste verwerking. |

## <a name="basic-virtual-network-resources"></a>Basic virtuele-netwerkbronnen

Het volgende diagram toont de plaatsing van HDInsight-knooppunten en netwerkbronnen in Azure.

![Diagram van HDInsight-entiteiten die zijn gemaakt in Azure aangepaste VNET](./media/hdinsight-virtual-network-architecture/vnet-diagram.png)

De standaardresources aanwezig wanneer HDInsight wordt geïmplementeerd in een Azure-netwerk bevatten de clusterknooppunttypen op die worden vermeld in de vorige tabel, evenals de netwerkapparaten die ondersteuning bieden voor communicatie tussen het virtuele netwerk en buiten netwerken.

De volgende tabel geeft een overzicht van de negen clusterknooppunten die worden gemaakt wanneer HDInsight wordt geïmplementeerd in een aangepaste Azure Virtual Network.

| Resourcetype | Het aantal | Details |
| --- | --- | --- |
|Hoofdknooppunt | twee |    |
|ZooKeeper-knooppunt | drie | |
|Werkknooppunt | twee | Dit nummer kan variëren op basis van de configuratie van het cluster en schalen. Een minimum van drie worker-knooppunten is nodig voor Apache Kafka.  |
|Gateway-knooppunt | twee | Gateway-knooppunten zijn Azure virtuele machines die zijn gemaakt op Azure, maar niet zichtbaar zijn in uw abonnement. Neem contact op met ondersteuning als u wilt deze knooppunten opnieuw opstarten. |

De volgende netwerkresources aanwezig zijn, worden automatisch gemaakt in het virtuele netwerk gebruikt met HDInsight:

| Netwerk-resource | Het aantal | Details |
| --- | --- | --- |
|Load balancer | drie | |
|Netwerkinterfaces | negen | Deze waarde is gebaseerd op een normale cluster, waarbij elk knooppunt een eigen netwerkinterface heeft. De negen interfaces zijn voor de twee hoofdknooppunten, drie zookeeper-knooppunten, twee werkknooppunten en twee gateway-knooppunten die worden vermeld in de vorige tabel. |
|Openbare IP-adressen | twee |    |

## <a name="endpoints-for-connecting-to-hdinsight"></a>Eindpunten voor het verbinden met HDInsight

U kunt toegang tot uw HDInsight-cluster op drie manieren:

- Een HTTPS-eindpunt buiten het virtuele netwerk op `CLUSTERNAME.azurehdinsight.net`.
- Een SSH-eindpunt voor het rechtstreeks verbinding te maken met het hoofdknooppunt op `CLUSTERNAME-ssh.azurehdinsight.net`.
- Een HTTPS-eindpunt in het virtuele netwerk `CLUSTERNAME-int.azurehdinsight.net`. U ziet dat de '-int "in deze URL. Dit eindpunt wordt omgezet in een privé IP-adres in dit virtuele netwerk en is niet toegankelijk is vanaf het openbare internet.

Deze drie eindpunten zijn elk een load balancer toegewezen.

Openbare IP-adressen zijn ook beschikbaar voor de twee eindpunten waarmee de verbinding van buiten het virtuele netwerk.

1. Een openbaar IP-adres is toegewezen aan de load balancer voor de volledig gekwalificeerde domeinnaam (FQDN) om verbinding te maken met het cluster vanaf het internet `CLUSTERNAME.azurehdinsight.net`.
1. Het tweede IP-adres wordt gebruikt voor de naam van het SSH-alleen domein `CLUSTERNAME-ssh.azurehdinsight.net`.

## <a name="next-steps"></a>Volgende stappen

* [Binnenkomend verkeer naar HDInsight-clusters in een virtueel netwerk met persoonlijke eindpunt beveiligen](https://azure.microsoft.com/blog/secure-incoming-traffic-to-hdinsight-clusters-in-a-vnet-with-private-endpoint/)
