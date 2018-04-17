---
title: Apache Spark Structured Streaming met Kafka - Azure HDInsight | Microsoft Docs
description: Lees hoe u Apache Spark Streaming (DStream) gebruikt om gegevens uit of naar Apache Kafka te verzenden. In dit voorbeeld gaat u met behulp van een Jupyter Notebook gegevens streamen van Spark naar HDInsight.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2018
ms.author: larryfr
ms.openlocfilehash: 49c13bbea537d7de60ecf509bc28675191c0b34d
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="use-spark-structured-streaming-with-kafka-on-hdinsight"></a>Spark Structured Streaming gebruiken met Kafka in HDInsight

Lees meer over het gebruik van Spark Structured Streaming om gegevens uit Apache Kafka in Azure HDInsight te lezen.

Spark Structured Streaming is een streamverwerkingsengine gebaseerd op Spark SQL. Hiermee kunt u streamingberekeningen op dezelfde manier weergeven als batchberekeningen van statische gegevens. Zie de [Structured Streaming Programming Guide [Alpha]](http://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) op Apache.org voor meer informatie over Structured Streaming.

> [!IMPORTANT]
> In dit voorbeeld wordt Spark 2.2 in HDInsight 3.6 gebruikt.
>
> Met de stappen in dit document wordt een Azure-resourcegroep gemaakt die zowel een Spark in HDInsight- als een Kafka in HDInsight-cluster bevat. Deze clusters bevinden zich beide binnen een Azure Virtual Network, waardoor het Spark-cluster rechtstreeks kan communiceren met het Kafka-cluster.
>
> Nadat u de stappen in dit document hebt doorlopen, moet u niet vergeten de clusters te verwijderen om overtollige kosten te voorkomen.

## <a name="create-the-clusters"></a>De clusters maken

Apache Kafka in HDInsight biedt geen toegang tot de Kafka-brokers via het openbare internet. Alles wat gebruikmaakt van Kafka moet zich in hetzelfde Azure Virtual Network bevinden. In deze zelfstudie bevinden de Kafka- en Spark-clusters zich in hetzelfde Azure Virtual Network. 

Het volgende diagram laat zien hoe de communicatie tussen Spark en Kafka verloopt:

![Diagram van Spark- en Kafka-clusters in een Azure Virtual Network](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> De Kafka-service blijft beperkt tot communicatie binnen het virtuele netwerk. Andere services in het cluster, zoals SSH en Ambari, zijn toegankelijk via internet. Zie [Poorten en URI's die worden gebruikt door HDInsight](hdinsight-hadoop-port-settings-for-services.md) voor meer informatie over de openbare poorten die beschikbaar zijn voor HDInsight.

Voor uw gemak wordt in de volgende stappen gebruikgemaakt van een Azure Resource Manager-sjabloon om Kafka- en Spark-clusters in een virtueel netwerk te maken.

1. Gebruik de volgende knop om u aan te melden bij Azure en de sjabloon in de Azure Portal te openen.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-kafka-structured-streaming%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    De Azure Resource Manager-sjabloon bevindt zich op **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json**.

    Met deze sjabloon maakt u de volgende bronnen:

    * Een Kafka in HDInsight 3.6-cluster.
    * Een Spark 2.2.0 in HDInsight 3.6-cluster.
    * Een Azure Virtual Network dat de HDInsight-clusters bevat.

    > [!IMPORTANT]
    > Voor de Structured Streaming-notebook die in dit voorbeeld wordt gebruikt, is Spark in HDInsight 3.6 vereist. Als u een eerdere versie van Spark in HDInsight gebruikt, worden er fouten gegenereerd bij gebruik van de notebook.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen in de sectie **Aangepaste sjabloon**:

    | Instelling | Waarde |
    | --- | --- |
    | Abonnement | Uw Azure-abonnement |
    | Resourcegroep | De resourcegroep die de resources bevat. |
    | Locatie | De Azure-regio waarin de bronnen worden gemaakt. |
    | Naam Spark-cluster | De naam van het Spark-cluster. |
    | Naam Kafka-cluster | De naam van het Kafka-cluster. |
    | Gebruikersnaam voor clusteraanmelding | De beheerdersnaam voor de clusters. |
    | Wachtwoord voor clusteraanmelding | Het beheerderswachtwoord voor de clusters. |
    | SSH-gebruikersnaam | De SSH-gebruiker die voor de clusters wordt gemaakt. |
    | SSH-wachtwoord | Het wachtwoord voor de SSH-gebruiker. |
   
    ![Schermafbeelding van de aangepaste sjabloon](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

4. Schakel tot slot **Vastmaken aan dashboard** in en selecteer **Kopen**. 

> [!NOTE]
> Het kan 20 minuten duren voordat het cluster is gemaakt.

## <a name="get-the-notebook"></a>De notebook ophalen

De code voor het voorbeeld in dit document is beschikbaar op [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

## <a name="upload-the-notebooks"></a>De notebooks uploaden

Als u de notebook vanuit het project wilt uploaden naar uw Spark in HDInsight-cluster, voert u de volgende stappen uit:

1. Maak vanuit uw webbrowser verbinding met de Jupyter-notebook in uw Spark-cluster. In de volgende URL vervangt u `CLUSTERNAME` door de naam van uw __Spark__-cluster:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Typ desgevraagd de cluster-aanmelding (beheerder) en het cluster-wachtwoord die u hebt gebruikt bij het maken van het cluster.

2. Gebruik rechtsboven op de pagina de knop __Uploaden__ om het __spark-structured-streaming-kafka.ipynb__-bestand naar het cluster te uploaden. Selecteer __Openen__ om te beginnen met uploaden.

    ![De knop voor uploaden gebruiken om een notebook te selecteren en uploaden](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-button.png)

    ![Het bestand KafkaStreaming.ipynb selecteren](./media/hdinsight-apache-kafka-spark-structured-streaming/select-notebook.png)

3. Zoek naar de vermelding __spark-structured-streaming-kafka.ipynb__ in de lijst met notebooks en selecteer de knop __Uploaden__ ernaast.

    ![Als u de notebook wilt uploaden, gebruikt u de knop voor uploaden voor de vermelding KafkaStreaming.ipynb](./media/hdinsight-apache-kafka-spark-structured-streaming/upload-notebook.png)


## <a name="use-the-notebook"></a>De notebook gebruiken

Zodra de bestanden zijn geüpload, selecteert u de vermelding __spark-structured-streaming-kafka.ipynb__ om de notebook te openen. Voor meer informatie over het gebruik van Spark Structured Streaming met Kafka in HDInsight volgt u de instructies in de notebook.

## <a name="clean-up-resources"></a>Resources opschonen

Als u de in deze zelfstudie gemaakte resources wilt opschonen, kunt u de resourcegroep verwijderen. Als u de resourcegroep verwijdert, worden ook het bijbehorende HDInsight-cluster en eventuele andere resources die aan de resourcegroep zijn gekoppeld, verwijderd.

Ga als volgt te werk om de resourcegroep te verwijderen in Azure Portal:

1. Vouw het menu aan de linkerkant in Azure Portal uit om het menu met services te openen en kies __Resourcegroepen__ om de lijst met resourcegroepen weer te geven.
2. Zoek de resourcegroep die u wilt verwijderen en klik met de rechtermuisknop op de knop __Meer__ (... ) aan de rechterkant van de vermelding.
3. Selecteer __Resourcegroep verwijderen__ en bevestig dit.

> [!WARNING]
> De facturering voor het gebruik van HDInsight-clusters begint zodra er een cluster is gemaakt en stopt als een cluster wordt verwijderd. De facturering wordt pro-rato per minuut berekend, dus u moet altijd uw cluster verwijderen wanneer het niet meer wordt gebruikt.
> 
> Door een Kafka in HDInsight-cluster te verwijderen, worden alle gegevens verwijderd die zijn opgeslagen in Kafka.

## <a name="next-steps"></a>Volgende stappen

Nu u hebt geleerd hoe u Spark Structured Streaming gebruikt, kunt u de volgende documenten raadplegen voor meer informatie over het werken met Spark en Kafka:

* [Spark-streaming (DStream) met Kafka gebruiken](hdinsight-apache-spark-with-kafka.md).
* [Beginnen met Jupyter Notebook en Spark in HDInsight](spark/apache-spark-jupyter-spark-sql.md)