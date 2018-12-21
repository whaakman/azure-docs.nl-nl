---
title: 'Zelfstudie: Apache Spark Structured Streaming met Apache Kafka - Azure HDInsight'
description: Lees hoe u Apache Spark Streaming gebruikt om gegevens uit of naar Apache Kafka te verzenden. In deze zelfstudie gaat u met behulp van een Jupyter-notebook gegevens streamen van Spark naar HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: tutorial
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: 4ac341d780a3c348f9ba9f8fd0241c351bd5fdc5
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53162424"
---
# <a name="tutorial-use-apache-spark-structured-streaming-with-apache-kafka-on-hdinsight"></a>Zelfstudie: Apache Spark Structured Streaming gebruiken met Apache Kafka op HDInsight

Deze zelfstudie laat zien hoe u [Apache Spark Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) gebruikt om gegevens te lezen en te schrijven met [Apache Kafka](https://kafka.apache.org/) in Azure HDInsight.

Spark Structured Streaming is een streamverwerkingsengine gebaseerd op Spark SQL. Hiermee kunt u streamingberekeningen op dezelfde manier weergeven als batchberekeningen van statische gegevens. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gestructureerd streamen met Kafka
> * Kafka- en Spark-clusters maken
> * Het notebook uploaden naar Spark
> * De notebook gebruiken
> * Resources opschonen

Nadat u de stappen in dit document hebt doorlopen, moet u niet vergeten de clusters te verwijderen om overtollige kosten te voorkomen.

## <a name="prerequisites"></a>Vereisten

* Weten hoe u [Jupyter Notebooks](https://jupyter.org/) gebruikt met Apache Spark on HDInsight. Zie het document [Zelfstudie: Gegevens laden en query's uitvoeren in een Apache Spark-cluster in Azure HDInsight](spark/apache-spark-load-data-run-query.md) voor meer informatie.

* Bekend met de programmeertaal [Scala](https://www.scala-lang.org/). De code die wordt gebruikt in deze zelfstudie, is geschreven in Scala.

* Kennis van Kafka-onderwerpen. Zie het document [Quickstart: Een Apache Kafka-cluster maken in HDInsight](kafka/apache-kafka-get-started.md) voor meer informatie.

> [!IMPORTANT]
> Voor de stappen in dit document is een Azure-resourcegroep nodig die zowel een Spark in HDInsight- als een Kafka in HDInsight-cluster bevat. Deze clusters bevinden zich beide binnen een Azure Virtual Network, waardoor het Spark-cluster rechtstreeks kan communiceren met het Kafka-cluster.
> 
> Voor uw gemak is dit document gekoppeld aan een sjabloon waarmee u alle vereiste Azure-resources kunt maken. 
>
> Zie het document [Azure HDInsight met behulp van een Azure-netwerk uitbreiden](hdinsight-extend-hadoop-virtual-network.md) voor meer informatie over het gebruik van HDInsight in een virtueel netwerk.

## <a name="structured-streaming-with-apache-kafka"></a>Gestructureerd streamen met Apache Kafka

Spark Structured Streaming is een streamverwerkingsengine gebaseerd op de Spark SQL-engine. Wanneer u Structured Streaming gebruikt, kunt u streamingquery's schrijven op de manier waarop u ook batchquery's schrijft.

De volgende codefragmenten laten zien hoe u gegevens kunt lezen uit Kafka om deze daarna op te slaan in een bestand. Het eerste fragment betreft een batchbewerking, terwijl het tweede een streamingbewerking is:

```scala
// Read a batch from Kafka
val kafkaDF = spark.read.format("kafka")
                .option("kafka.bootstrap.servers", kafkaBrokers)
                .option("subscribe", kafkaTopic)
                .option("startingOffsets", "earliest")
                .load()
// Select data and write to file
kafkaDF.select(from_json(col("value").cast("string"), schema) as "trip")
                .write
                .format("parquet")
                .option("path","/example/batchtripdata")
                .option("checkpointLocation", "/batchcheckpoint")
                .save()
```

```scala
// Stream from Kafka
val kafkaStreamDF = spark.readStream.format("kafka")
                .option("kafka.bootstrap.servers", kafkaBrokers)
                .option("subscribe", kafkaTopic)
                .option("startingOffsets", "earliest")
                .load()
// Select data from the stream and write to file
kafkaStreamDF.select(from_json(col("value").cast("string"), schema) as "trip")
                .writeStream
                .format("parquet")
                .option("path","/example/streamingtripdata")
                .option("checkpointLocation", "/streamcheckpoint")
                .start.awaitTermination(30000)
```

In beide codefragmenten worden gegevens gelezen uit Kafka en weggeschreven naar een bestand. Dit zijn de verschillende tussen de voorbeelden:

| Batch | Streaming |
| --- | --- |
| `read` | `readStream` |
| `write` | `writeStream` |
| `save` | `start` |

De streamingbewerking gebruikt ook `awaitTermination(30000)`, waarmee de stream na 30000 ms wordt gestopt. 

Als u Structured Streaming wilt gebruiken met Kafka, moet het project afhankelijk zijn van het pakket `org.apache.spark : spark-sql-kafka-0-10_2.11`. De versie van dit pakket moet overeenkomen met de versie van Spark in HDInsight. Voor Spark 2.2.0 (beschikbaar in HDInsight 3.6) kunt u de afhankelijkheidsgegevens voor verschillende projecttypen vinden in [https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar](https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar).

Voor het Jupyter-notebook dat wordt gebruikt met deze zelfstudie zorgt de code in de volgende cel ervoor dat deze pakketafhankelijkheid wordt geladen:

```
%%configure -f
{
    "conf": {
        "spark.jars.packages": "org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.0",
        "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.11"
    }
}
```

## <a name="create-the-clusters"></a>De clusters maken

Apache Kafka in HDInsight biedt geen toegang tot de Kafka-brokers via het openbare internet. Alles wat gebruikmaakt van Kafka moet zich in hetzelfde Azure Virtual Network bevinden. In deze zelfstudie bevinden de Kafka- en Spark-clusters zich in hetzelfde Azure Virtual Network. 

Het volgende diagram laat zien hoe de communicatie tussen Spark en Kafka verloopt:

![Diagram van Spark- en Kafka-clusters in een Azure Virtual Network](./media/hdinsight-apache-spark-with-kafka/spark-kafka-vnet.png)

> [!NOTE]
> De Kafka-service blijft beperkt tot communicatie binnen het virtuele netwerk. Andere services in het cluster, zoals SSH en Ambari, zijn toegankelijk via internet. Zie [Poorten en URI's die worden gebruikt door HDInsight](hdinsight-hadoop-port-settings-for-services.md) voor meer informatie over de openbare poorten die beschikbaar zijn voor HDInsight.

Gebruik de volgende stappen om eerst een virtueel Azure-netwerk te maken en vervolgens de Kafka- en Spark-clusters:

1. Gebruik de volgende knop om u aan te melden bij Azure en de sjabloon in de Azure Portal te openen.
    
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-kafka-structured-streaming%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-apache-spark-with-kafka/deploy-to-azure.png" alt="Deploy to Azure"></a>
    
    De Azure Resource Manager-sjabloon bevindt zich op **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json**.

    Met deze sjabloon maakt u de volgende bronnen:

    * Een Kafka in HDInsight 3.6-cluster.
    * Een Spark 2.2.0 in HDInsight 3.6-cluster.
    * Een Azure Virtual Network dat de HDInsight-clusters bevat.

    > [!IMPORTANT]
    > Voor het Structured Streaming-notebook dat in deze zelfstudie wordt gebruikt, is Spark 2.2.0 in HDInsight 3.6 vereist. Als u een eerdere versie van Spark in HDInsight gebruikt, worden er fouten gegenereerd bij gebruik van de notebook.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen in de sectie **Aangepaste sjabloon**:

    | Instelling | Waarde |
    | --- | --- |
    | Abonnement | Uw Azure-abonnement |
    | Resourcegroep | De resourcegroep die de resources bevat. |
    | Locatie | De Azure-regio waarin de bronnen worden gemaakt. |
    | Naam Spark-cluster | De naam van het Spark-cluster. De eerste zes tekens moeten verschillen van de naam van het Kafka-cluster. |
    | Naam Kafka-cluster | De naam van het Kafka-cluster. De eerste zes tekens moeten verschillen van de naam van het Spark-cluster. |
    | Gebruikersnaam voor clusteraanmelding | De beheerdersnaam voor de clusters. |
    | Wachtwoord voor clusteraanmelding | Het beheerderswachtwoord voor de clusters. |
    | SSH-gebruikersnaam | De SSH-gebruiker die voor de clusters wordt gemaakt. |
    | SSH-wachtwoord | Het wachtwoord voor de SSH-gebruiker. |
   
    ![Schermafbeelding van de aangepaste sjabloon](./media/hdinsight-apache-kafka-spark-structured-streaming/spark-kafka-template.png)

3. Lees de **voorwaarden** en schakel vervolgens het selectievakje **Ik ga akkoord met de bovenstaande voorwaarden** in.

4. Schakel tot slot **Vastmaken aan dashboard** in en selecteer **Kopen**. 

> [!NOTE]
> Het kan 20 minuten duren voordat het cluster is gemaakt.

## <a name="upload-the-notebook"></a>Het notebook uploaden

Als u de notebook vanuit het project wilt uploaden naar uw Spark in HDInsight-cluster, voert u de volgende stappen uit:

1. Het project downloaden van [https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming](https://github.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming).

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

In deze zelfstudie hebt u geleerd hoe u met [Apache Spark Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) gegevens kunt lezen uit en wegschrijven naar [Apache Kafka](https://kafka.apache.org/) in HDInsight. Gebruik de volgende koppeling voor informatie over hoe u [Apache Storm](https://storm.apache.org/) gebruikt met Kafka.

> [!div class="nextstepaction"]
> [Apache Storm gebruiken met Apache Kafka](hdinsight-apache-storm-with-kafka.md)
