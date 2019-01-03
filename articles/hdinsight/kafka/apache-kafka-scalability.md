---
title: Schaalverhoging Apache Kafka - Azure HDInsight
description: Leer hoe u beheerde schijven voor een Apache Kafka-cluster configureert in Azure HDInsight om de schaalbaarheid te verhogen.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: ab80dd86e544127fc3f40f5459ef9a587c7cd511
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53581499"
---
# <a name="configure-storage-and-scalability-for-apache-kafka-on-hdinsight"></a>Opslag en schaalbaarheid configureren voor Apache Kafka in HDInsight

Informatie over het configureren van het aantal beheerde schijven gebruikt door [Apache Kafka](https://kafka.apache.org/) op HDInsight.

Kafka in HDInsight maakt gebruik van de lokale schijf van de virtuele machines in het HDInsight-cluster. Aangezien Kafka veel gebruikmaakt van invoer/uitvoer, wordt [Azure Managed Disks](../../virtual-machines/windows/managed-disks-overview.md) gebruikt voor een hoge doorvoer en meer opslag per knooppunt. Als u de traditionele VHD (virtuele harde schijven) gebruikt voor Kafka, heeft elk knooppunt een limiet van 1 TB. Met beheerde schijven kunt u meerdere schijven gebruiken en zodat elk knooppunt in het cluster een limiet heeft van 16 TB.

In het volgende diagram ziet u een vergelijking tussen Kafka in HDInsight voordat beheerde schijven werden gebruikt, en Kafka in HDInsight met beheerde schijven:

![Diagram waarin Kafka in HDInsight wordt weergegeven met één virtuele harde schijf per VM versus meerdere beheerde schijven per VM](./media/apache-kafka-scalability/kafka-with-managed-disks-architecture.png)

## <a name="configure-managed-disks-azure-portal"></a>Beheerde schijven configureren: Azure Portal

1. Volg de stappen in [Een HDInsight-cluster maken](../hdinsight-hadoop-create-linux-clusters-portal.md) voor de algemene stappen voor het maken van een cluster met behulp van de portal. Voltooi het creatieproces met behulp van de portal niet.

2. Gebruik in de sectie __Clustergrootte__ het veld __Schijven per werkknooppunt__ om het aantal schijven te configureren.

    > [!NOTE]  
    > Het type beheerde schijf is __Standaard__ (HDD) of __Premium__ (SSD). Premium-schijven worden gebruikt met virtuele machines uit de DS- en GS-reeks. Alle andere VM-typen gebruiken standaardschijven.

    ![Afbeelding van de sectie Clustergrootte met de schijven gemarkeerd per werkknooppunt](./media/apache-kafka-scalability/set-managed-disks-portal.png)

## <a name="configure-managed-disks-resource-manager-template"></a>Beheerde schijven configureren: Resource Manager-sjabloon

Als u het aantal schijven wilt beheren dat wordt gebruikt in de werkknooppunten van een Kafka-cluster, gebruikt u de volgende sectie van de sjabloon:

```json
"dataDisksGroups": [
    {
        "disksPerNode": "[variables('disksPerWorkerNode')]"
    }
    ],
```

U vindt een volledige sjabloon over het configureren van beheerde schijven op [ https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json ](https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-mirror-cluster-in-vnet-v2.1.json).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende documenten voor meer informatie over het werken met Apache Kafka in HDInsight:

* [MirrorMaker gebruiken voor het maken van een replica van Apache Kafka op HDInsight](apache-kafka-mirroring.md)
* [Apache Storm gebruiken met Apache Kafka in HDInsight](../hdinsight-apache-storm-with-kafka.md)
* [Apache Spark gebruiken met Apache Kafka in HDInsight](../hdinsight-apache-spark-with-kafka.md)
* [Verbinding maken met Apache Kafka via een Azure-netwerk](apache-kafka-connect-vpn-gateway.md)

* [HDInsight-blog over beheerde schijven met Apache Kafka](https://azure.microsoft.com/blog/announcing-public-preview-of-apache-kafka-on-hdinsight-with-azure-managed-disks/)
