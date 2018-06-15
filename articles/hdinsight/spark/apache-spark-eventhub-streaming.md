---
title: 'Zelfstudie: Gegevens uit Azure Event Hubs met Apache Spark in Azure HDInsight verwerken | Microsoft Docs'
description: Apache Spark in Azure HDInsight verbinden met Azure Event Hubs en verwerken van de streaminggegevens.
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: jgao
ms.openlocfilehash: 9b59f5d58234aaf8f8385f722d6659548e066933
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787253"
---
# <a name="tutorial-process-tweets-using-azure-event-hubs-and-spark-in-hdinsight"></a>Zelfstudie: Proces tweets met Azure Event Hubs en Spark in HDInsight

In deze zelfstudie leert u hoe u een Apache Spark-streaming-toepassing tweets verzenden naar een Azure event hub en het maken van een andere toepassing de tweets lezen van de event hub maakt. Zie voor een gedetailleerde uitleg van Spark-streaming [Apache Spark-streaming-overzicht](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight biedt dezelfde streaming functies voor een Spark-cluster in Azure.

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Berichten verzenden naar Azure Event Hub
> * Lezen van berichten uit Azure Event Hub

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **Voltooien van het artikel [zelfstudie: gegevens laden en query's uitvoeren op een Apache Spark-cluster in Azure HDInsight](./apache-spark-load-data-run-query.md)**.

## <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

Als u een stream van tweets wilt ontvangen, maakt u een toepassing in Twitter. Volg de instructies een Twitter-toepassing maken en noteer de waarden moet u deze zelfstudie te voltooien.

1. Blader naar [Twitter Toepassingsbeheer](https://apps.twitter.com/).
2. Selecteer **nieuwe App maken**.
3. Geef de volgende waarden op:

    - Naam: Geef de toepassingsnaam. De waarde voor deze zelfstudie gebruikt **HDISparkStreamApp0423**. Deze naam moet een unieke naam.
    - Beschrijving: Geef een korte beschrijving van de toepassing. De waarde voor deze zelfstudie gebruikt **een eenvoudige HDInsight Spark-streaming-toepassing**.
    - Website: Geef op de website van de toepassing. Heeft geen moet een geldige website.  De waarde voor deze zelfstudie gebruikt **http://www.contoso.com**.
    - De URL van de callback: u kunt deze leeg laten.

4. Selecteer **Ja, ik heb gelezen en ga akkoord met de ontwikkelaar Twitter overeenkomst**, en selecteer vervolgens **uw Twitter-toepassing maken**.
5. Selecteer de **sleutels en toegangstokens** tabblad.
6. Selecteer **maken van mijn toegangstoken** aan het einde van de pagina.
7. Noteer de volgende waarden op de pagina.  Verderop in de zelfstudie moet u deze waarden:

    - **Consumentsleutel (API-sleutel)**    
    - **Consumentgeheim (geheim API)**  
    - **Toegangstoken**
    - **Access Token geheim**   

## <a name="create-an-azure-event-hub"></a>Een Azure Event Hub maken

U kunt deze event hub gebruiken voor het opslaan van tweets.

1. Meld u aan bij [Azure-portal](https://ms.portal.azure.com).
2. Selecteer **maken van een resource** op linksboven op het scherm.
3. Selecteer **Internet der dingen**, selecteer daarna **Event Hubs**.

    ![Create event hub voor Spark-streaming-voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "maken event hub voor Spark-streaming-voorbeeld")
4. Voer de volgende waarden voor de nieuwe event hub-naamruimte:

    - **Naam**: Voer een naam voor de event hub.  De waarde voor deze zelfstudie gebruikt **myeventhubns20180403**.
    - **Prijs laag**: Selecteer **standaard**.
    - **Resourcegroep**: U hebt de optie voor een nieuwe maken of selecteren van de resourcegroep voor het Spark-cluster. 
    - **Locatie**: Selecteer hetzelfde **locatie** als uw cluster Apache Spark in HDInsight om latentie en kosten te verminderen.

    ![Geef de naam van een event hub voor Spark-streaming-voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "bieden u de naam van een event hub voor Spark-streaming-voorbeeld")
5. Selecteer **maken** de naamruimte maken.

6. Open de event hub-naamruimte met behulp van de volgende instructies:

    1. Selecteer in de portal **alle services**.
    2. Voer in het filtervak **event hubs**.
    3. Dubbelklik op de naamruimte die u hebt gemaakt.
    4. Selecteer **+ Event Hub**.

6. Selecteer de zojuist gemaakte naamruimte in de lijst van de naamruimte Event Hubs.      
5. Selecteer **Event Hubs**, en selecteer vervolgens **+ Event Hub** voor het maken van nieuwe Event Hub.
  

6. Voer de volgende waarden in:

    - Naam: Geef een naam voor uw Event Hub.
    - Partitie-aantal: 10
    - Bericht bewaren: 1. 
   
    ![Geef details van gebeurtenis hub voor Spark-streaming-voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "event hub-informatie opgeven voor Spark-streaming-voorbeeld")

7. Selecteer **Maken**.
8. Selecteer **gedeeld toegangsbeleid** voor de naamruimte (Opmerking: het is niet het toegangsbeleid van de event hub die wordt gedeeld) en selecteer vervolgens **RootManageSharedAccessKey**.
    
     ![Event Hub-beleid instellen voor de Spark-streaming-voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Event Hub ingesteld beleid voor het Spark-streaming-voorbeeld")

9. Opslaan van de waarden van **primaire sleutel** en **Connection string-primaire sleutel** voor gebruik verderop in de zelfstudie.

     ![Event Hub beleid sleutels voor de Spark-streaming-voorbeeld weergeven](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "weergave Event Hub beleid sleutels voor het voorbeeld van Spark-streaming")


## <a name="send-tweets-to-the-event-hub"></a>Tweets verzenden naar de event hub

U moet een Jupyter-notebook maken en noem deze **SendTweetsToEventHub**. 

1. Voer de volgende code om toe te voegen van de externe Maven-bibliotheken:

    ```
    %%configure
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0,org.twitter4j:twitter4j-core:4.0.6"}}
    ```

2. Voer de volgende code voor het verzenden van tweets naar uw event hub:

    ```
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

3. Open de event hub in de Azure portal.  Op **overzicht**, ziet u enkele grafieken de berichten worden verzonden naar de event hub.

## <a name="read-tweets-from-the-event-hub"></a>Tweets lezen van de event hub

U moet een andere Jupyter-notebook maken en noem deze **ReadTweetsFromEventHub**. 

1. Voer de volgende code om het toevoegen van een externe Maven-bibliotheek:

    ```
    %%configure -f
    {"conf":{"spark.jars.packages":"com.microsoft.azure:azure-eventhubs-spark_2.11:2.2.0"}}
    ```
2. Voer de volgende code om te lezen tweets van uw event hub:

    ```
    import org.apache.spark.eventhubs._
    // Event hub configurations
    // Replace values below with yours        
    val eventHubName = "<Event hub name>"
    val eventHubNSConnStr = "<Event hub namespace connection string>"
    val connStr = ConnectionStringBuilder(eventHubNSConnStr).setEventHubName(eventHubName).build 
    
    val customEventhubParameters = EventHubsConf(connectionString).setMaxEventsPerTrigger(5)
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

Uw gegevens worden opgeslagen in Azure Storage of Azure Data Lake Store met HDInsight, zodat u een cluster veilig verwijderen kunt wanneer deze zich niet in gebruik. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt. Als u van plan bent om te werken op de volgende zelfstudie onmiddellijk, is het raadzaam om het cluster.

Open het cluster in de Azure portal en selecteer **verwijderen**.

![HDInsight-cluster verwijderen](./media/apache-spark-load-data-run-query/hdinsight-azure-portal-delete-cluster.png "verwijderen HDInsight-cluster")

U kunt ook de naam van de resource te openen van de pagina van de groep resource selecteren en selecteer vervolgens **resourcegroep verwijderen**. Door de resourcegroep te verwijderen, verwijdert u zowel de HDInsight Spark-cluster en het standaardopslagaccount.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

* Bericht lezen van een event hub.
Ga naar het volgende artikel om te zien dat kunt u een machine learning-toepassing maken. 

> [!div class="nextstepaction"]
> [Een machine learning-toepassing maken](./apache-spark-ipython-notebook-machine-learning.md)


