---
title: 'Zelfstudie: Apache Spark Structured Streaming met Apache Kafka - Azure HDInsight'
description: Lees hoe u Apache Spark Streaming gebruikt om gegevens uit of naar Apache Kafka te verzenden. In deze zelfstudie gaat u met behulp van een Jupyter-notebook gegevens streamen van Spark naar HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: tutorial
ms.date: 05/22/2019
ms.author: hrasheed
ms.openlocfilehash: 51f84234ac35be5f60d1aaa5dac661ad9ce5e0c2
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257904"
---
# <a name="tutorial-use-apache-spark-structured-streaming-with-apache-kafka-on-hdinsight"></a>Zelfstudie: Apache Spark Structured Streaming gebruiken met Apache Kafka op HDInsight

Deze zelfstudie laat zien hoe u [Apache Spark Structured Streaming](https://spark.apache.org/docs/latest/structured-streaming-programming-guide) gebruikt om gegevens te lezen en te schrijven met [Apache Kafka](https://kafka.apache.org/) in Azure HDInsight.

Spark Structured Streaming is een streamverwerkingsengine gebaseerd op Spark SQL. Hiermee kunt u streamingberekeningen op dezelfde manier weergeven als batchberekeningen van statische gegevens.  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Resource Manager-sjabloon gebruiken om clusters te maken
> * Gebruik Spark Structured Streaming met Kafka

Nadat u de stappen in dit document hebt doorlopen, moet u niet vergeten de clusters te verwijderen om overtollige kosten te voorkomen.

## <a name="prerequisites"></a>Vereisten

* jq, een opdrachtregelprogramma JSON-processor.  Zie [ https://stedolan.github.io/jq/ ](https://stedolan.github.io/jq/).

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

De streaming bewerking eveneens `awaitTermination(30000)`, die de stroom niet meer nadat 30.000 ms. 

Als u Structured Streaming wilt gebruiken met Kafka, moet het project afhankelijk zijn van het pakket `org.apache.spark : spark-sql-kafka-0-10_2.11`. De versie van dit pakket moet overeenkomen met de versie van Spark in HDInsight. Voor Spark 2.2.0 (beschikbaar in HDInsight 3.6) kunt u de afhankelijkheidsgegevens voor verschillende projecttypen vinden in [https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar](https://search.maven.org/#artifactdetails%7Corg.apache.spark%7Cspark-sql-kafka-0-10_2.11%7C2.2.0%7Cjar).

Voor de Jupyter-Notebook gebruikt in combinatie met deze zelfstudie, wordt deze pakketafhankelijkheid geladen met de volgende cel:

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

    De Azure Resource Manager-sjabloon bevindt zich op **https://raw.githubusercontent.com/Azure-Samples/hdinsight-spark-kafka-structured-streaming/master/azuredeploy.json** .

    Met deze sjabloon maakt u de volgende bronnen:

   * Een Kafka in HDInsight 3.6-cluster.
   * Een Spark 2.2.0 in HDInsight 3.6-cluster.
   * Een Azure Virtual Network dat de HDInsight-clusters bevat.

     > [!IMPORTANT]  
     > Voor het Structured Streaming-notebook dat in deze zelfstudie wordt gebruikt, is Spark 2.2.0 in HDInsight 3.6 vereist. Als u een eerdere versie van Spark in HDInsight gebruikt, worden er fouten gegenereerd bij gebruik van de notebook.

2. Gebruik de volgende informatie voor het vullen van de vermeldingen in de sectie **Aangepaste sjabloon**:

    | Instelling | Value |
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

## <a name="use-spark-structured-streaming"></a>Spark Structured Streaming gebruiken

In dit voorbeeld laat zien hoe u Spark Structured Streaming met Kafka in HDInsight. Het maakt gebruik van gegevens op de gegevens over taxi's, die wordt geleverd door de New York City.  De gegevensset die worden gebruikt door dit notitieblok afkomstig is van [2016 groen Taxi reisgegevens](https://data.cityofnewyork.us/Transportation/2016-Green-Taxi-Trip-Data/hvrh-b6nb).

1. Verzamel informatie over de host. Gebruik de curl en [jq](https://stedolan.github.io/jq/) verkrijgen van uw Kafka-ZooKeeper en hosts informatie broker onderstaande opdrachten. De opdrachten zijn ontworpen voor een Windows-opdrachtprompt, kleine variaties er nodig zijn voor andere omgevingen. Vervang `KafkaCluster` met de naam van uw Kafka-cluster en `KafkaPassword` met het wachtwoord voor clusteraanmelding. Vervang ook `C:\HDI\jq-win64.exe` met het werkelijke pad naar de jq-installatie. Voer de opdrachten in een Windows-opdrachtprompt en sla de uitvoer voor gebruik in latere stappen.

    ```cmd
    set CLUSTERNAME=KafkaCluster
    set PASSWORD=KafkaPassword
    
    curl -u admin:%PASSWORD% -G "https://%CLUSTERNAME%.azurehdinsight.net/api/v1/clusters/%CLUSTERNAME%/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" | C:\HDI\jq-win64.exe -r "["""\(.host_components[].HostRoles.host_name):2181"""] | join(""",""")"
    
    curl -u admin:%PASSWORD% -G "https://%CLUSTERNAME%.azurehdinsight.net/api/v1/clusters/%CLUSTERNAME%/services/KAFKA/components/KAFKA_BROKER" | C:\HDI\jq-win64.exe -r "["""\(.host_components[].HostRoles.host_name):9092"""] | join(""",""")"
    ```

2. Maak vanuit uw webbrowser verbinding met de Jupyter-notebook in uw Spark-cluster. In de volgende URL vervangt u `CLUSTERNAME` door de naam van uw __Spark__-cluster:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Typ desgevraagd de cluster-aanmelding (beheerder) en het cluster-wachtwoord die u hebt gebruikt bij het maken van het cluster.

3. Selecteer **New > Spark** om een notitieblok te maken.

4. Pakketten die worden gebruikt door de Notebook door te voeren van de volgende informatie in een cel Notebook worden geladen. Voer de opdracht uit met behulp van **CTRL + ENTER**.

    ```
    %%configure -f
    {
        "conf": {
            "spark.jars.packages": "org.apache.spark:spark-sql-kafka-0-10_2.11:2.2.0",
            "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.11"
        }
    }
    ```

5. De Kafka-onderwerp maken. De onderstaande opdracht bewerken door te vervangen `YOUR_ZOOKEEPER_HOSTS` hosten met de Zookeeper informatie in de eerste stap hebt uitgepakt. Voer de opdracht in de bewerkte in uw Jupyter-Notebook maken de `tripdata` onderwerp.

    ```scala
    %%bash
    export KafkaZookeepers="YOUR_ZOOKEEPER_HOSTS"

    /usr/hdp/current/kafka-broker/bin/kafka-topics.sh --create --replication-factor 3 --partitions 8 --topic tripdata --zookeeper $KafkaZookeepers
    ```

6. Gegevens over taxi trips ophalen. Voer de opdracht in de volgende cel om gegevens op de gegevens over taxi's in New York City te laden. De gegevens zijn geladen in een dataframe en vervolgens het gegevensframe wordt weergegeven als de uitvoer van de cel.

    ```scala
    import spark.implicits._

    // Load the data from the New York City Taxi data REST API for 2016 Green Taxi Trip Data
    val url="https://data.cityofnewyork.us/resource/pqfs-mqru.json"
    val result = scala.io.Source.fromURL(url).mkString
    
    // Create a dataframe from the JSON data
    val taxiDF = spark.read.json(Seq(result).toDS)
    
    // Display the dataframe containing trip data
    taxiDF.show()
    ```

7. De gegevens van Kafka broker hosts ingesteld. Vervang `YOUR_KAFKA_BROKER_HOSTS` met de broker hosts informatie die u in stap 1 hebt uitgepakt.  Voer de opdracht in bewerkt in de volgende Jupyter-Notebook cel.

    ```scala
    // The Kafka broker hosts and topic used to write to Kafka
    val kafkaBrokers="YOUR_KAFKA_BROKER_HOSTS"
    val kafkaTopic="tripdata"
    
    println("Finished setting Kafka broker and topic configuration.")
    ```

8. De gegevens verzenden naar Kafka. In de volgende opdracht de `vendorid` veld wordt gebruikt als de waarde van de sleutel voor het Kafka-bericht. De sleutel wordt gebruikt door Kafka worden gebruikt bij het partitioneren van gegevens. Alle velden zijn opgeslagen in het Kafka-bericht als een JSON-tekenreekswaarde. Voer de volgende opdracht in Jupyter om op te slaan van de gegevens met Kafka met behulp van een batchquery.

    ```scala
    // Select the vendorid as the key and save the JSON string as the value.
    val query = taxiDF.selectExpr("CAST(vendorid AS STRING) as key", "to_JSON(struct(*)) AS value").write.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("topic", kafkaTopic).save()

    println("Data sent to Kafka")
    ```

9. Declareer een schema. De volgende opdracht laat zien hoe u een schema gebruikt bij het lezen van JSON-gegevens van kafka. Voer de opdracht in de volgende Jupyter cel.

    ```scala
    // Import bits useed for declaring schemas and working with JSON data
    import org.apache.spark.sql._
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._
    
    // Define a schema for the data
    val schema = (new StructType).add("dropoff_latitude", StringType).add("dropoff_longitude", StringType).add("extra", StringType).add("fare_amount", StringType).add("improvement_surcharge", StringType).add("lpep_dropoff_datetime", StringType).add("lpep_pickup_datetime", StringType).add("mta_tax", StringType).add("passenger_count", StringType).add("payment_type", StringType).add("pickup_latitude", StringType).add("pickup_longitude", StringType).add("ratecodeid", StringType).add("store_and_fwd_flag", StringType).add("tip_amount", StringType).add("tolls_amount", StringType).add("total_amount", StringType).add("trip_distance", StringType).add("trip_type", StringType).add("vendorid", StringType)
    // Reproduced here for readability
    //val schema = (new StructType)
    //   .add("dropoff_latitude", StringType)
    //   .add("dropoff_longitude", StringType)
    //   .add("extra", StringType)
    //   .add("fare_amount", StringType)
    //   .add("improvement_surcharge", StringType)
    //   .add("lpep_dropoff_datetime", StringType)
    //   .add("lpep_pickup_datetime", StringType)
    //   .add("mta_tax", StringType)
    //   .add("passenger_count", StringType)
    //   .add("payment_type", StringType)
    //   .add("pickup_latitude", StringType)
    //   .add("pickup_longitude", StringType)
    //   .add("ratecodeid", StringType)
    //   .add("store_and_fwd_flag", StringType)
    //   .add("tip_amount", StringType)
    //   .add("tolls_amount", StringType)
    //   .add("total_amount", StringType)
    //   .add("trip_distance", StringType)
    //   .add("trip_type", StringType)
    //   .add("vendorid", StringType)
    
    println("Schema declared")
    ```

10. Selecteer gegevens en de stroom start. De volgende opdracht laat zien hoe u voor het ophalen van gegevens van kafka met behulp van een batchquery en klikt u vervolgens de resultaten uit naar schrijven HDFS op het Spark-cluster. In dit voorbeeld wordt de `select` haalt het bericht (in het waardeveld) van Kafka en wordt het schema op toegepast. De gegevens worden vervolgens naar HDFS (WASB of ADL) in parquet-indeling geschreven. Voer de opdracht in de volgende Jupyter cel.

    ```scala
    // Read a batch from Kafka
    val kafkaDF = spark.read.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("subscribe", kafkaTopic).option("startingOffsets", "earliest").load()
    
    // Select data and write to file
    val query = kafkaDF.select(from_json(col("value").cast("string"), schema) as "trip").write.format("parquet").option("path","/example/batchtripdata").option("checkpointLocation", "/batchcheckpoint").save()
    
    println("Wrote data to file")
    ```

11. U kunt controleren dat de bestanden zijn gemaakt met de opdracht in de volgende Jupyter cel. Geeft een lijst van de bestanden in de `/example/batchtripdata` directory.

    ```scala
    %%bash
    hdfs dfs -ls /example/batchtripdata
    ```

12. Hoewel het vorige voorbeeld gebruikt een batchquery, geeft de volgende opdracht laat zien hoe u hetzelfde doen met behulp van een streaming-query. Voer de opdracht in de volgende Jupyter cel.

    ```scala
    // Stream from Kafka
    val kafkaStreamDF = spark.readStream.format("kafka").option("kafka.bootstrap.servers", kafkaBrokers).option("subscribe", kafkaTopic).option("startingOffsets", "earliest").load()
    
    // Select data from the stream and write to file
    kafkaStreamDF.select(from_json(col("value").cast("string"), schema) as "trip").writeStream.format("parquet").option("path","/example/streamingtripdata").option("checkpointLocation", "/streamcheckpoint").start.awaitTermination(30000)
    println("Wrote data to file")
    ```

13. Voer de volgende cel om te controleren of de bestanden zijn geschreven door de streaming-query.

    ```scala
    %%bash
    hdfs dfs -ls /example/streamingtripdata
    ```

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
