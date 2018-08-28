---
title: Apache Spark-streaming met Kafka - Azure HDInsight
description: Informatie over het gebruik van Spark Apache Spark om gegevens te streamen of naar Apache Kafka met behulp van DStreams. In dit voorbeeld gaat u met behulp van een Jupyter Notebook gegevens streamen van Spark naar HDInsight.
keywords: Voorbeeld van kafka, kafka zookeeper, spark-streaming van kafka, spark-streaming van kafka-voorbeeld
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: jasonh
ms.openlocfilehash: d06e9d26051fbfafc4d717ec180e8760157aefd9
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094163"
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-on-hdinsight"></a>Apache Spark-streaming (DStream)-voorbeeld met Kafka in HDInsight

Informatie over het gebruik van Spark Apache Spark om gegevens te streamen of naar Apache Kafka in HDInsight met behulp van DStreams. In dit voorbeeld maakt gebruik van een Jupyter-notebook die wordt uitgevoerd op het Spark-cluster.

> [!NOTE]
> Met de stappen in dit document wordt een Azure-resourcegroep gemaakt die zowel een Spark in HDInsight- als een Kafka in HDInsight-cluster bevat. Deze clusters bevinden zich beide binnen een Azure Virtual Network, waardoor het Spark-cluster rechtstreeks kan communiceren met het Kafka-cluster.
>
> Nadat u de stappen in dit document hebt doorlopen, moet u niet vergeten de clusters te verwijderen om overtollige kosten te voorkomen.

> [!IMPORTANT]
> In dit voorbeeld maakt gebruik van DStreams, dit een oudere Spark streaming-technologie is. Zie voor een voorbeeld met nieuwere Spark streaming-functies, de [Spark Structured Streaming met Kafka](hdinsight-apache-kafka-spark-structured-streaming.md) document.

## <a name="create-the-clusters"></a>De clusters maken

Apache Kafka in HDInsight biedt geen toegang tot de Kafka-brokers via het openbare internet. Alles wat met Kafka praat moet zich in dezelfde Azure virtual network als de knooppunten in het Kafka-cluster. In dit voorbeeld bevinden de Kafka- en Spark-clusters zich in een Azure-netwerk. Het volgende diagram toont hoe de communicatie tussen de clusters met stromen:

![Diagram van Spark- en Kafka-clusters in een Azure Virtual Network](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Hoewel Kafka zelf beperkt tot communicatie binnen het virtuele netwerk is, kunnen andere services op het cluster zoals SSH en Ambari zijn toegankelijk via internet. Zie [Poorten en URI's die worden gebruikt door HDInsight](hdinsight-hadoop-port-settings-for-services.md) voor meer informatie over de openbare poorten die beschikbaar zijn voor HDInsight.

Hoewel u een Azure-netwerk, Kafka, maken kunt en Spark-clusters handmatig, is het eenvoudiger te gebruiken van een Azure Resource Manager-sjabloon. Gebruik de volgende stappen uit op een Azure-netwerk, Kafka, implementeren en Spark-clusters naar uw Azure-abonnement.

1. Gebruik de volgende knop om u aan te melden bij Azure en de sjabloon in de Azure Portal te openen.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    De Azure Resource Manager-sjabloon bevindt zich op **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    > [!WARNING]
    > Om beschikbaarheid van Kafka op HDInsight te garanderen, moet uw cluster ten minste drie werkknooppunten bevatten. Met deze sjabloon maakt u een Kafka-cluster dat drie werkknooppunten bevat.

    Deze sjabloon maakt een HDInsight 3.6-cluster voor zowel Kafka en Spark.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen in de **aangepaste implementatie** sectie:
   
    ![Aangepaste HDInsight-implementatie](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Resourcegroep**: Maak een groep of Selecteer een bestaande resourcegroep. Deze groep bevat het HDInsight-cluster.

    * **Locatie**: Selecteer een locatie geografisch dicht bij u.

    * **Clusternaam baseren**: deze waarde wordt gebruikt als basisnaam voor de Spark en Kafka-clusters. Bijvoorbeeld, voeren **hdi** maakt u een Spark-cluster met de naam __spark-hdi__ en een Kafka-cluster met de naam **kafka-hdi**.

    * **Gebruikersnaam voor aanmelding cluster**: de beheerdersnaam voor de Spark en Kafka-clusters.

    * **Cluster-wachtwoord voor Clusteraanmelding**: het beheerderswachtwoord voor de Spark en Kafka-clusters.

    * **SSH-gebruikersnaam**: de SSH-gebruiker maken voor de Spark en Kafka-clusters.

    * **SSH-wachtwoord**: het wachtwoord voor de SSH-gebruiker voor de Spark en Kafka-clusters.

3. Lees de **voorwaarden** en schakel vervolgens het selectievakje **Ik ga akkoord met de bovenstaande voorwaarden** in.

4. Schakel tot slot **Vastmaken aan dashboard** in en selecteer **Kopen**. Het duurt ongeveer 20 minuten om de clusters te maken.

Zodra de resources zijn gemaakt, wordt er een overzichtspagina weergegeven.

![Resourcegroep samenvatting voor het vnet en -clusters](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> U ziet dat de namen van de HDInsight-clusters zijn **spark-BASENAME** en **kafka-BASENAME**, waarbij BASENAME is de naam die u hebt opgegeven voor de sjabloon. U gebruikt deze namen in latere stappen bij het verbinden met de clusters.

## <a name="use-the-notebooks"></a>De notebooks gebruiken

De code voor het voorbeeld in dit document is beschikbaar op [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).

Voor het voltooien van dit voorbeeld, volg de stappen in de `README.md`.

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Omdat de stappen in dit document worden beide clusters in de dezelfde Azure-resourcegroep maakt, kunt u de resourcegroep in Azure portal verwijderen. De groep verwijdert, worden alle resources hebt gemaakt op basis van dit document, Azure Virtual Network en storage-account door de clusters worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

In dit voorbeeld hebt u geleerd hoe u Spark voor lezen en schrijven naar Kafka. Gebruik de volgende koppelingen voor het detecteren van andere manieren om te werken met Kafka:

* [Aan de slag met Apache Kafka in HDInsight](kafka/apache-kafka-get-started.md)
* [MirrorMaker gebruiken voor het maken van een replica van Kafka in HDInsight](kafka/apache-kafka-mirroring.md)
* [Apache Storm gebruiken met Kafka in HDInsight](hdinsight-apache-storm-with-kafka.md)

