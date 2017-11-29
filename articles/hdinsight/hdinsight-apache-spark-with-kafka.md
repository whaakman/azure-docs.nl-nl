---
title: Apache Spark-streaming met Kafka - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Spark Apache Spark als stream gegevens van of naar Apache Kafka met DStreams. In dit voorbeeld kunt u gegevens met behulp van een Jupyter-notebook in Spark in HDInsight streamen.
keywords: Voorbeeld van kafka, zookeeper kafka, spark streaming kafka, spark-streaming kafka-voorbeeld
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: dd8f53c1-bdee-4921-b683-3be4c46c2039
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: larryfr
ms.openlocfilehash: a780a7d954486dbce402e4bf45be55af8ad36ab9
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2017
---
# <a name="apache-spark-streaming-dstream-example-with-kafka-on-hdinsight"></a>Apache Spark-streaming (DStream) voorbeeld met Kafka in HDInsight

Informatie over het gebruik van Spark Apache Spark als stream gegevens van of naar Apache Kafka op HDInsight met behulp van DStreams. In dit voorbeeld wordt een Jupyter-notebook die wordt uitgevoerd op het Spark-cluster.

> [!NOTE]
> De stappen in dit document wordt een Azure-resourcegroep met zowel een Spark in HDInsight en een Kafka op HDInsight-cluster maken Deze clusters zijn beide zich binnen een virtueel netwerk van Azure, waardoor het Spark-cluster rechtstreeks communiceren met de Kafka-cluster.
>
> Wanneer u klaar bent met de stappen in dit document, vergeet niet de clusters om te voorkomen dat de overtollige kosten te verwijderen.

> [!IMPORTANT]
> In dit voorbeeld wordt DStreams, dit een oudere Spark-streaming technologie is. Zie voor een voorbeeld met nieuwere Spark-streaming-functies, de [gestructureerde Streaming van Spark met Kafka](hdinsight-apache-kafka-spark-structured-streaming.md) document.

## <a name="create-the-clusters"></a>Clusters maken

Apache Kafka op HDInsight biedt geen toegang voor de beleggingsmakelaars Kafka via het openbare internet. Alles wat wordt gesproken naar Kafka moet zich in hetzelfde virtuele netwerk van Azure als de knooppunten in het cluster Kafka. In dit voorbeeld worden de Kafka- en Spark-clusters bevinden zich in een Azure-netwerk. Het volgende diagram toont hoe de communicatie tussen clusters stroomt:

![Diagram van Spark en Kafka clusters in een Azure-netwerk](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> Hoewel Kafka zelf beperkt tot communicatie binnen het virtuele netwerk is, zijn andere services op het cluster zoals SSH en Ambari toegankelijk via internet. Zie voor meer informatie over de openbare poorten beschikbaar met HDInsight [poorten en URI's die worden gebruikt door HDInsight](hdinsight-hadoop-port-settings-for-services.md).

U kunt een Azure-netwerk Kafka, maken en handmatig Spark-clusters, is het ook eenvoudiger te gebruiken van een Azure Resource Manager-sjabloon. Gebruik de volgende stappen voor het implementeren van een Azure-netwerk Kafka, en Spark-clusters met uw Azure-abonnement.

1. Gebruik de volgende knop om te melden bij Azure en opent u de sjabloon in de Azure portal.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-kafka-spark-cluster-in-vnet-v4.1.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    De Azure Resource Manager-sjabloon bevindt zich op **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-kafka-spark-cluster-in-vnet-v4.1.json**.

    > [!WARNING]
    > Om beschikbaarheid van Kafka op HDInsight te garanderen, moet uw cluster ten minste drie werkknooppunten bevatten. Deze sjabloon maakt een Kafka-cluster dat drie worker-knooppunten bevat.

    Deze sjabloon maakt een 3.6 HDInsight-cluster voor Kafka- en Spark.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen op de **aangepaste implementatie** sectie:
   
    ![Aangepaste HDInsight-implementatie](./media/hdinsight-apache-spark-with-kafka/parameters.png)
   
    * **Resourcegroep**: Maak een groep of een bestaande set selecteren. Deze groep bevat het HDInsight-cluster.

    * **Locatie**: Selecteer een locatie geografisch dicht bij u.

    * **Clusternaam baseren**: deze waarde wordt gebruikt als de basisnaam aan voor de Spark en Kafka-clusters. Bijvoorbeeld, voeren **hdi** maakt een Spark-cluster met de naam __spark hdi__ en een Kafka-cluster met de naam **kafka hdi**.

    * **Aanmeldingsnaam van gebruiker cluster**: de beheerdersgebruikersnaam voor Spark en Kafka-clusters.

    * **Aanmeldingswachtwoord cluster**: het beheerderswachtwoord voor de Spark en Kafka clusters.

    * **SSH-gebruikersnaam**: de SSH-gebruiker maken voor Spark en Kafka-clusters.

    * **SSH-wachtwoord**: het wachtwoord voor de SSH-gebruiker voor de Spark en Kafka clusters.

3. Lees de **voorwaarden en bepalingen**, en selecteer vervolgens **ik ga akkoord met de voorwaarden en bepalingen bovengenoemde**.

4. Controleer ten slotte **vastmaken aan dashboard** en selecteer vervolgens **aankoop**. Het duurt ongeveer 20 minuten om de clusters te maken.

Zodra de resources zijn gemaakt, wordt een overzichtspagina weergegeven.

![Resourcegroep samenvatting voor het vnet en clusters](./media/hdinsight-apache-spark-with-kafka/groupblade.png)

> [!IMPORTANT]
> Merk op dat de namen van de HDInsight-clusters **spark BASENAME** en **kafka BASENAME**, waarbij BASENAME is de naam die u hebt opgegeven in de sjabloon. U kunt deze namen in latere stappen gebruiken bij het verbinden met de clusters.

## <a name="use-the-notebooks"></a>De notebooks gebruiken

De code voor het voorbeeld in dit document beschreven is beschikbaar op [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka).

Als u dit voorbeeld, volg de stappen in de `README.md`.

## <a name="delete-the-cluster"></a>Het cluster verwijderen

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

Aangezien de stappen in dit document beide clusters in de groep met dezelfde Azure-resource maakt, kunt u de resourcegroep in de Azure portal kunt verwijderen. De groep verwijdert, worden alle resources die zijn gemaakt op basis van dit document, Azure Virtual Network en storage-account door de clusters worden gebruikt.

## <a name="next-steps"></a>Volgende stappen

In dit voorbeeld hebt u geleerd Spark gebruiken om te lezen en schrijven naar Kafka. Gebruik de volgende koppelingen voor het detecteren van andere manieren om te werken met Kafka:

* [Aan de slag met Apache Kafka in HDInsight](kafka/apache-kafka-get-started.md)
* [MirrorMaker gebruiken voor het maken van een replica van Kafka in HDInsight](kafka/apache-kafka-mirroring.md)
* [Apache Storm gebruiken met Kafka in HDInsight](hdinsight-apache-storm-with-kafka.md)

