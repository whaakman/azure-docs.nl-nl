---
title: 'Zelfstudie: Verwerken van gegevens uit Azure Event Hubs met Apache Spark in Azure HDInsight '
description: 'Zelfstudie: Apache Spark in Azure HDInsight verbinden met Azure Event Hubs en de streaming gegevens verwerken.'
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/24/2019
ms.openlocfilehash: c8c99d976f416d0c1d07fb3a266d37ecd6235fdb
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67295358"
---
# <a name="tutorial-process-tweets-using-azure-event-hubs-and-apache-spark-in-hdinsight"></a>Zelfstudie: Op tweets met behulp van Azure Event Hubs en Apache Spark in HDInsight

In deze zelfstudie leert u over het maken van een [Apache Spark](https://spark.apache.org/) streaming van toepassing op tweets verzenden naar een Azure event hub en een andere toepassing de tweets lezen van de event hub maken. Zie voor een gedetailleerde uitleg van de Spark-streaming [Apache Spark-streaming overzicht](https://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight biedt de dezelfde streaming-functies voor een Spark-cluster in Azure.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Berichten verzenden naar Azure Event Hub
> * Berichten lezen uit Azure Event Hub

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Een Apache Spark-cluster in HDInsight. Zie [maken van een Apache Spark-cluster](./apache-spark-jupyter-spark-sql-use-portal.md).

* Weten hoe u Jupyter Notebook gebruikt met Spark on HDInsight. Zie voor meer informatie, [gegevens laden en query's uitvoeren met Apache Spark op HDInsight](./apache-spark-load-data-run-query.md).

* Een [Twitter-account](https://twitter.com/i/flow/signup).

## <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

Als u een stream van tweets wilt ontvangen, maakt u een toepassing in Twitter. Volg de instructies voor het maken van een Twitter-toepassing en noteer de waarden die u nodig hebt om deze zelfstudie te voltooien.

1. Blader naar [Twitter Application Management](https://apps.twitter.com/).

1. Selecteer **nieuwe App maken**.

1. Geef de volgende waarden op:

    |Eigenschap |Value |
    |---|---|
    |Name|Geef de toepassingsnaam. De waarde die wordt gebruikt voor deze zelfstudie is **HDISparkStreamApp0423**. Deze naam moet een unieke naam.|
    |Description|Geef een korte beschrijving van de toepassing. De waarde die wordt gebruikt voor deze zelfstudie is **een eenvoudige HDInsight Spark-streaming toepassing**.|
    |Website|Geef op de website van de toepassing. Dit hoeft niet te worden van een geldige website.  De waarde die wordt gebruikt voor deze zelfstudie is **http://www.contoso.com** .|
    |URL voor terugbellen|U kunt deze leeg laten.|

1. Selecteer **Ja, ik heb de licentiebepalingen gelezen en ga akkoord met de Twitter-Developer-overeenkomst**, en selecteer vervolgens **uw Twitter-toepassing maken**.

1. Selecteer de **Keys and Access Tokens** tabblad.

1. Selecteer **maken van mijn toegangstoken** aan het einde van de pagina.

1. Noteer de volgende waarden in op de pagina.  U hebt deze waarden later in de zelfstudie nodig:

    - **Consumentsleutel (API-sleutel)**    
    - **Consumer Secret (API-geheim)**  
    - **Toegangstoken**
    - **Access Token Secret**   

## <a name="create-an-azure-event-hubs-namespace"></a>Een Azure Event Hubs-naamruimte maken

U kunt deze event hub gebruiken voor het opslaan van tweets.

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Selecteer in het menu links **alle services**.  

3. Onder **INTERNET of THINGS**, selecteer **Event Hubs**. 

    ![De gebeurtenishub maken voor Spark streaming voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "gebeurtenishub maken voor Spark streaming-voorbeeld")

4. Selecteer **+ Toevoegen**.

5. Voer de volgende waarden in voor de nieuwe Event Hubs-naamruimte:

    |Eigenschap |Value |
    |---|---|
    |Name|Voer een naam voor de event hub.  De waarde die wordt gebruikt voor deze zelfstudie is **myeventhubns20180403**.|
    |Prijscategorie|selecteer **Standaard**.|
    |Abonnement|Selecteer het juiste abonnement.|
    |Resourcegroep|Selecteer een bestaande resourcegroep in de vervolgkeuzelijst of selecteer **nieuw** om een nieuwe resourcegroep te maken.|
    |Locatie|Selecteer dezelfde **locatie** als uw Apache Spark-cluster in HDInsight latentie en kosten te verminderen.|
    |Schakel automatisch vergroten (optioneel) |Automatisch vergroten, schaalt automatisch het aantal Throughput Units toegewezen aan uw Event Hubs-Namespace wanneer uw verkeer groter is dan de capaciteit van de Throughput Units zijn toegewezen.  |
    |Maximum aantal Throughput Units (optioneel) automatisch vergroten|Deze schuifregelaar wordt alleen weergegeven als u het selectievakje **inschakelen automatisch vergroten**.  |

    ![Geef de naam van een event hub voor Spark streaming voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "bieden u de naam van een event hub voor Spark streaming-voorbeeld")

6. Selecteer **maken** om de naamruimte te maken.  De implementatie duurt een paar minuten.

## <a name="create-an-azure-event-hub"></a>Een Azure event hub maken
Een event hub maken nadat de Event Hubs-naamruimte is geïmplementeerd.  Vanuit de portal:

1. Selecteer in het menu links **alle services**.  

1. Onder **INTERNET of THINGS**, selecteer **Event Hubs**.  

1. Selecteer uw Event Hubs-naamruimte in de lijst.  

1. Uit de **Event Hubs Namespace** weergeeft, schakelt **+ Event Hub**.  
1. Voer de volgende waarden in de **Event Hub maken** pagina:

    - **Naam**: Geef een naam voor uw Event Hub. 
 
    - **Aantal partities**: 10.  

    - **Bewaarperiode van bericht is**: 1.   
   
      ![Details van event hub bieden voor Spark streaming voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "details van event hub bieden voor Spark streaming-voorbeeld")

1. Selecteer **Maken**.  De implementatie moet worden voltooid binnen een paar seconden en keert u terug naar de Event Hubs-Namespace-pagina.

1. Onder **instellingen**, selecteer **beleid voor gedeelde toegang**.

1. Selecteer **RootManageSharedAccessKey**.
    
     ![Event Hub-beleid instellen voor de Spark-streaming voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "ingesteld Event Hub-beleid voor de Spark-streaming-voorbeeld")

1. Sla de waarden van **primaire sleutel** en **verbindingsreeks-primaire sleutel** voor gebruik verderop in de zelfstudie.

     ![Event Hub-beleid-sleutels weergeven voor de Spark-streaming voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "weergave Event Hub-beleid sleutels voor het voorbeeld van Spark-streaming")


## <a name="send-tweets-to-the-event-hub"></a>Tweets verzenden naar de event hub

Een Jupyter-notebook maken en geef deze de naam **SendTweetsToEventHub**. 

1. Voer de volgende code om toe te voegen van de externe Apache Maven-bibliotheken:

    ```
    %%configure
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0,org.twitter4j:twitter4j-core:4.0.6"}}
    ```

2. De onderstaande code bewerken door te vervangen `<Event hub name>`, `<Event hub namespace connection string>`, `<CONSUMER KEY>`, `<CONSUMER SECRET>`, `<ACCESS TOKEN>`, en `<TOKEN SECRET>` met de juiste waarden. Voer de bewerkte code voor het verzenden van tweets naar uw event hub:

    ```scala
    import java.util._
    import scala.collection.JavaConverters._
    import java.util.concurrent._
    
    import org.apache.spark._
    import org.apache.spark.streaming._
    import org.apache.spark.eventhubs.ConnectionStringBuilder

    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    import com.microsoft.azure.eventhubs._
    val pool = Executors.newFixedThreadPool(1)
    val eventHubClient = EventHubClient.create(connStr.toString(), pool)
    
    def sendEvent(message: String) = {
          val messageData = EventData.create(message.getBytes("UTF-8"))
          eventHubClient.get().send(messageData)
          println("Sent event: " + message + "\n")
    }
    
    import twitter4j._
    import twitter4j.TwitterFactory
    import twitter4j.Twitter
    import twitter4j.conf.ConfigurationBuilder

    // Twitter application configurations
    // Replace values below with yours   
    val twitterConsumerKey = "<CONSUMER KEY>"
    val twitterConsumerSecret = "<CONSUMER SECRET>"
    val twitterOauthAccessToken = "<ACCESS TOKEN>"
    val twitterOauthTokenSecret = "<TOKEN SECRET>"
    
    val cb = new ConfigurationBuilder()
    cb.setDebugEnabled(true).setOAuthConsumerKey(twitterConsumerKey).setOAuthConsumerSecret(twitterConsumerSecret).setOAuthAccessToken(twitterOauthAccessToken).setOAuthAccessTokenSecret(twitterOauthTokenSecret)
    
    val twitterFactory = new TwitterFactory(cb.build())
    val twitter = twitterFactory.getInstance()

    // Getting tweets with keyword "Azure" and sending them to the Event Hub in realtime!
    
    val query = new Query(" #Azure ")
    query.setCount(100)
    query.lang("en")
    var finished = false
    while (!finished) {
      val result = twitter.search(query)
      val statuses = result.getTweets()
      var lowestStatusId = Long.MaxValue
      for (status <- statuses.asScala) {
        if(!status.isRetweet()){
          sendEvent(status.getText())
        }
        lowestStatusId = Math.min(status.getId(), lowestStatusId)
        Thread.sleep(2000)
      }
      query.setMaxId(lowestStatusId - 1)
    }
    
    // Closing connection to the Event Hub
    eventHubClient.get().close()
    ```

3. Open de event hub in Azure portal.  Op **overzicht**, ziet u enkele grafieken die de berichten worden verzonden naar de event hub.

## <a name="read-tweets-from-the-event-hub"></a>Tweets lezen van de event hub

Een andere Jupyter-notebook maken en geef deze de naam **ReadTweetsFromEventHub**. 

1. Voer de volgende code om toe te voegen een externe bibliotheek voor Apache Maven:

    ```
    %%configure -f
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0"}}
    ```

2. De onderstaande code bewerken door te vervangen `<Event hub name>`, en `<Event hub namespace connection string>` met de juiste waarden. Voer de bewerkte code voor het lezen van tweets van uw event hub:

    ```scala
    import org.apache.spark.eventhubs._
    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build
    
    val customEventhubParameters = EventHubsConf(connStr).setMaxEventsPerTrigger(5)
    val incomingStream = spark.readStream.format("eventhubs").options(customEventhubParameters.toMap).load()
    //incomingStream.printSchema    
    
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._
    
    // Event Hub message format is JSON and contains "body" field
    // Body is binary, so you cast it to string to see the actual content of the message
    val messages = incomingStream.withColumn("Offset", $"offset".cast(LongType)).withColumn("Time (readable)", $"enqueuedTime".cast(TimestampType)).withColumn("Timestamp", $"enqueuedTime".cast(LongType)).withColumn("Body", $"body".cast(StringType)).select("Offset", "Time (readable)", "Timestamp", "Body")
    
    messages.printSchema
    
    messages.writeStream.outputMode("append").format("console").option("truncate", false).start().awaitTermination()
    ```

## <a name="clean-up-resources"></a>Resources opschonen

Uw gegevens worden opgeslagen in Azure Storage of Azure Data Lake Storage met HDInsight, zodat u een cluster veilig verwijderen kunt wanneer deze zich niet in gebruik. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Als u van plan bent om te werken onmiddellijk op de volgende zelfstudie, is het raadzaam het cluster bewaren, anders gaan en verwijderen van het cluster.

Open het cluster in Azure Portal en selecteer **Verwijderen**.

![HDInsight-cluster verwijderen](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "HDInsight-cluster verwijderen")

U kunt ook de naam van de resourcegroep selecteren om de pagina van de resourcegroep te openen en vervolgens **Resourcegroep verwijderen** selecteren. Als u de resourcegroep verwijdert, verwijdert u zowel het HDInsight Spark-cluster als het standaardopslagaccount.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt geleerd hoe u een Apache Spark-streaming-toepassing om tweets te verzenden naar een Azure event hub maken en een andere toepassing de tweets lezen van de event hub hebt gemaakt.  Ga naar het volgende artikel voor ziet dat u een machine learning-toepassing kunt maken.

> [!div class="nextstepaction"]
> [Een machine learning-toepassing maken](./apache-spark-ipython-notebook-machine-learning.md)
