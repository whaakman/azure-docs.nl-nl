---
title: 'Zelfstudie: Detectie van afwijkingen in streaming-gegevens met behulp van Azure Databricks'
description: Gebruik de API voor Afwijkingsdetectie Detector en Azure Databricks voor het bewaken van afwijkingen in uw gegevens.
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 05/08/2019
ms.author: aahi
ms.openlocfilehash: d1808bb88ac1bb27f2fd0652585521fbb2abdf56
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734668"
---
# <a name="tutorial-anomaly-detection-on-streaming-data-using-azure-databricks"></a>Zelfstudie: Detectie van afwijkingen in streaming-gegevens met behulp van Azure Databricks

[Azure Databricks](https://azure.microsoft.com/en-us/services/databricks/) is een snel, eenvoudig en gezamenlijke op basis van Apache Spark analytics-service. De API voor Afwijkingsdetectie Detector, onderdeel van Azure Cognitive Services, biedt een manier om uw time series-gegevens controleren. Gebruik deze handleiding om detectie van afwijkingen op een stream met gegevens in bijna-realtime uitgevoerd met behulp van Azure Databricks. U twitter-gegevens met Azure Event Hubs opnemen, en ze importeren in Azure Databricks met behulp van de Spark Event Hubs-connector. Daarna gebruikt u de API voor het detecteren van afwijkingen op de gestreamde gegevens. 

In de volgende afbeelding wordt de stroom van de toepassing weergegeven:

![Azure Databricks met Event Hubs en Cognitive Services](../media/tutorials/databricks-cognitive-services-tutorial.png "Azure Databricks met Event Hubs en Cognitive Services")

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte maken
> * Een Apache Spark-cluster in Azure Databricks maken
> * Een Twitter-app voor toegang tot streaminggegevens maken
> * Notitieblokken maken in Azure Databricks
> * Bibliotheken toevoegen voor Event Hubs en Twitter-API
> * Maken van een bron voor de detectie van afwijkingen en de toegangssleutel ophalen
> * Tweets verzenden naar Event Hubs
> * Tweets lezen van Event Hubs
> * Detectie van afwijkingen op tweets uitvoeren

> [!Note]
> Deze zelfstudie maakt u kennis een benadering voor het implementeren van de aanbevolen [oplossingsarchitectuur](https://azure.microsoft.com/solutions/architecture/anomaly-detector-process/) voor de API van de detectie van afwijkingen.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

> [!Note]
> In deze zelfstudie kan niet worden voltooid met een gratis proefversie sleutel voor de API van de detectie van afwijkingen. Als u een gratis account wilt gebruiken om het Azure Databricks-cluster te maken, gaat u voordat het cluster is gemaakt naar uw profiel en wijzigt u uw abonnement in **betalen per gebruik**. Zie [Gratis Azure-account](https://azure.microsoft.com/free/) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

- Een [Azure Event Hubs-naamruimte](https://docs.microsoft.com/azure/event-hubs/event-hubs-create) en event hub.

- De [verbindingsreeks](../../../event-hubs/event-hubs-get-connection-string.md) voor toegang tot de Event Hubs-naamruimte. De verbindingsreeks moet de indeling te hebben:

    `Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<key name>;SharedAccessKey=<key value>`. 

- De naam van het gedeelde toegangsbeleid en het beleid sleutel voor Event Hubs.

Zie de Azure Event Hubs [snelstartgids](../../../event-hubs/event-hubs-create.md) voor informatie over het maken van een naamruimte en event hub.

## <a name="create-an-azure-databricks-workspace"></a>Een Azure Databricks-werkruimte maken

In deze sectie maakt u een Azure Databricks-werkruimte met de [Azure-portal](https://portal.azure.com/).

1. Selecteer in Azure Portal **Een resource maken** > **Analyse** > **Azure Databricks**.

    ![Databricks in Azure Portal](../media/tutorials/azure-databricks-on-portal.png "Databricks in Azure Portal")

3. Geef bij **Azure Databricks Service** de volgende waarden op voor het maken van een Databricks-werkruimte:


    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Werkruimtenaam**     | Geef een naam op voor uw Databricks-werkruimte.        |
    |**Abonnement**     | Selecteer uw Azure-abonnement in de vervolgkeuzelijst.        |
    |**Resourcegroep**     | Geef aan of u een nieuwe resourcegroep wilt maken of een bestaande groep wilt gebruiken. Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. Zie [Overzicht van Azure Resource Manager](../../../azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    |**Locatie**     | Selecteer **VS-Oost 2** of een van de andere beschikbare regio's. Zie [producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor beschikbaarheid in regio's.        |
    |**Prijscategorie**     |  U kunt kiezen tussen **Standard** en **Premium**. Kies niet **proefversie**. Bekijk de pagina [Prijzen voor Databricks](https://azure.microsoft.com/pricing/details/databricks/) voor meer informatie over deze categorieën.       |

    Selecteer **Maken**.

4. Het maken van de werkruimte duurt enkele minuten. 

## <a name="create-a-spark-cluster-in-databricks"></a>Een Spark-cluster maken in Databricks

1. Ga in Azure Portal naar de Databricks-werkruimte die u hebt gemaakt en selecteer **Werkruimte starten**.

2. U wordt omgeleid naar de Azure Databricks-portal. Selecteer in de portal **nieuw Cluster**.

    ![Databricks in Azure](../media/tutorials/databricks-on-azure.png "Databricks in Azure")

3. In de **nieuw Cluster** pagina, geef de waarden voor het maken van een cluster.

    ![Een Databricks Spark-cluster maken in Azure](../media/tutorials/create-databricks-spark-cluster.png "Een Databricks Spark-cluster maken in Azure")

    Accepteer alle andere standaardwaarden, anders dan de volgende:

   * Voer een naam in voor het cluster.
   * In dit artikel maakt u een cluster met **5.2** runtime. Schakel niet **5.3** runtime.
   * Zorg ervoor dat de **beëindigen na \_ \_ minuten van inactiviteit** selectievakje is ingeschakeld. Geef een duur (in minuten) voor het cluster beëindigen als het cluster niet wordt gebruikt.

     Selecteer **Cluster maken**. 
4. Maken van het cluster duurt enkele minuten. Zodra het cluster wordt uitgevoerd, kunt u notitieblokken koppelen aan het cluster en Spark-taken uitvoeren.

## <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

Als u een stream van tweets wilt ontvangen, moet u een toepassing in Twitter maken. Volg de stappen om een Twitter-toepassing te maken en de waarden vast te leggen die u nodig hebt om deze zelfstudie te voltooien.

1. Ga vanuit een webbrowser naar [Twitter Application Management](https://apps.twitter.com/) en selecteer **Create New App**.

    ![Twitter-toepassing maken](../media/tutorials/databricks-create-twitter-app.png "Twitter-toepassing maken")

2. Voer op de pagina **Create an application** de gegevens voor de nieuwe app in en selecteer **Create your Twitter application**.

    ![Gegevens voor Twitter-toepassing](../media/tutorials/databricks-provide-twitter-app-details.png "Gegevens voor Twitter-toepassing")

3. Selecteer op de toepassingspagina het tabblad **Keys and Access Tokens** en kopieer de waarden voor **Consumer Key** en **Consumer Secret**. Selecteer ook **Create my access token** om de toegangstokens te genereren. Kopieer de waarden voor **Access Token** en **Access Token Secret**.

    ![Gegevens voor Twitter-toepassing](../media/tutorials/twitter-app-key-secret.png "Gegevens voor Twitter-toepassing")

Sla de waarden op die u hebt opgehaald voor de Twitter-toepassing. U hebt deze waarden later in de zelfstudie nodig.

## <a name="attach-libraries-to-spark-cluster"></a>Bibliotheken koppelen aan een Apache Spark-cluster

In deze zelfstudie gebruikt u de Twitter-API's om tweets te verzenden naar Event Hubs. U gebruikt ook de [Apache Spark Event Hubs-connector](https://github.com/Azure/azure-event-hubs-spark) om gegevens naar Azure Event Hubs te lezen en schrijven. Als u deze API's wilt gebruiken als onderdeel van uw cluster, kunt u ze als bibliotheken toevoegen aan Azure Databricks en ze vervolgens aan uw Apache Spark-cluster koppelen. De volgende instructies laten zien hoe u de bibliotheken die u moet toevoegen de **gedeelde** map in uw werkruimte.

1. Selecteer **Werkruimte** in de Azure Databricks-werkruimte en klik vervolgens met de rechtermuisknop op **Gedeeld**. Selecteer **Bibliotheek** > **maken** in het contextmenu.

   ![Dialoogvenster Bibliotheek toevoegen](../media/tutorials/databricks-add-library-option.png "Dialoogvenster Bibliotheek toevoegen")

2. Op de pagina nieuwe bibliotheek voor **bron** Selecteer **Maven**. Voor **coördineert**, voer de coördinaat voor het pakket dat u wilt toevoegen. Dit zijn de Maven-coördinaten voor de bibliotheken die in deze zelfstudie worden gebruikt:

   * Apache Spark Event Hubs-connector - `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.10`
   * Twitter API - `org.twitter4j:twitter4j-core:4.0.7`

     ![Maven-coördinaten opgeven](../media/tutorials/databricks-eventhub-specify-maven-coordinate.png "Maven-coördinaten opgeven")

3. Selecteer **Maken**.

4. Selecteer de map waaraan u de bibliotheek hebt toegevoegd en selecteer vervolgens de naam van de bibliotheek.

    ![Toe te voegen bibliotheek selecteren](../media/tutorials/select-library.png "Toe te voegen bibliotheek selecteren")

5. Als er geen cluster in de bibliotheekpagina is, selecteert u **Clusters** en uit te voeren van het cluster dat u hebt gemaakt. Wacht totdat de status 'Actief' weergegeven en gaat u terug naar de bibliotheekpagina.
Selecteer op de bibliotheekpagina het cluster waar u wilt gebruiken van de bibliotheek en selecteer vervolgens **installeren**. Nadat de bibliotheek is gekoppeld aan het cluster, verandert de status onmiddellijk in **geïnstalleerde**.

    ![Bibliotheek installeren op cluster](../media/tutorials/databricks-library-attached.png "installeren bibliotheek aan cluster")

6. Herhaal deze stappen voor het Twitter-pakket, `twitter4j-core:4.0.7`.

## <a name="get-a-cognitive-services-access-key"></a>Een Cognitive Services-toegangssleutel ophalen

In deze zelfstudie gebruikt u de [Azure Cognitive Services Anomaliedetectie Detector API's](../overview.md) anomaliedetectie uitvoeren op een stream van tweets in bijna realtime. Voordat u de API's gebruiken, moet u een bron voor de detectie van afwijkingen maken op Azure en een toegangssleutel voor het gebruik van de Anomaliedetectie-API's voor Detector ophalen.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

2. Selecteer **+ Een resource maken**.

3. Selecteer onder Azure Marketplace, **AI + Machine Learning** > **alle** > **Cognitive Services - meer**  >  **Anomaliedetectie Detector**. U kunt [deze koppeling](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) naar de **maken** rechtstreeks in het dialoogvenster.

    ![Detectie van afwijkingen resource maken](../media/tutorials/databricks-cognitive-services-anomaly-detector.png "detectie van afwijkingen maken resource")

4. Geef in het dialoogvenster **Maken** de volgende waarden op:

    |Value |Beschrijving  |
    |---------|---------|
    |Name     | Een naam voor de detectie van afwijkingen-resource.        |
    |Abonnement     | Azure-abonnement voor de resource zal worden gekoppeld.        |
    |Locatie     | Een Azure-locatie.        |
    |Prijscategorie     | Een prijscategorie voor de service. Zie voor meer informatie over de prijzen voor de detectie van afwijkingen [pagina met prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/anomaly-detector/).        |
    |Resourcegroep     | Geef aan of u een nieuwe resourcegroep wilt maken of een bestaande groep wilt gebruiken.        |


     Selecteer **Maken**.

5. Nadat de resource is gemaakt, uit de **overzicht** tabblad-, kopieer- en sla de **eindpunt** URL, zoals wordt weergegeven in de schermafbeelding. Selecteer vervolgens **toegangssleutels weergeven**.

    ![Toegangssleutels weergeven](../media/tutorials/cognitive-services-get-access-keys.png "Toegangssleutels weergeven")

6. Onder **sleutels**, selecteer het kopieerpictogram op basis van de sleutel die u wilt gebruiken. Sla de toegangssleutel.

    ![Toegangssleutels kopiëren](../media/tutorials/cognitive-services-copy-access-keys.png "Toegangssleutels kopiëren")

## <a name="create-notebooks-in-databricks"></a>Notitieblokken maken in Databricks

In deze sectie gaat u in de Databricks-werkruimte twee notitieblokken met de volgende namen maken

- **SendTweetsToEventHub**: een notitieblok voor producenten waarmee u tweets kunt ophalen uit Twitter die u kunt streamen naar Event Hubs.
- **AnalyzeTweetsFromEventHub** -een notitieblok voor consumenten waarmee u de tweets uit Event Hubs te lezen en uitvoeren van detectie van afwijkingen.

1. Selecteer in de Azure Databricks-werkruimte **werkruimte** in het linkerdeelvenster. Selecteer **Maken** in de vervolgkeuzelijst **Werkruimte** en selecteer **Notitieblok**.

    ![Een notitieblok maken in Databricks](../media/tutorials/databricks-create-notebook.png "Een notitieblok maken in Databricks")

2. In de **notitieblok maken** dialoogvenster vak, voer **SendTweetsToEventHub** als de naam, selecteer **Scala** als de taal en selecteer het Spark-cluster dat u eerder hebt gemaakt.

    ![Een notitieblok maken in Databricks](../media/tutorials/databricks-notebook-details.png "Een notitieblok maken in Databricks")

    Selecteer **Maken**.

3. Herhaal de stappen om het notitieblok **AnalyzeTweetsFromEventHub** te maken.

## <a name="send-tweets-to-event-hubs"></a>Tweets verzenden naar Event Hubs

Plak in het notitieblok **SendTweetsToEventHub** de volgende code en vervang de tijdelijke aanduiding door waarden voor uw Event Hubs-naamruimte en Twitter-toepassing die u eerder hebt gemaakt. Dit notitieblok tijd voor het maken en het aantal ", zoals" s geëxtraheerd uit tweets met het sleutelwoord 'Azure' en die als gebeurtenissen streamen naar Event Hubs in realtime.

```scala
//
// Send Data to Eventhub
//

import scala.collection.JavaConverters._
import com.microsoft.azure.eventhubs._
import java.util.concurrent._
import com.google.gson.{Gson, GsonBuilder, JsonParser}
import java.util.Date
import scala.util.control._
import twitter4j._
import twitter4j.TwitterFactory
import twitter4j.Twitter
import twitter4j.conf.ConfigurationBuilder

// Event Hub Config
val namespaceName = "[Placeholder: EventHub namespace]"
val eventHubName = "[Placeholder: EventHub name]"
val sasKeyName = "[Placeholder: EventHub access key name]"
val sasKey = "[Placeholder: EventHub access key key]"
val connStr = new ConnectionStringBuilder()
  .setNamespaceName(namespaceName)
  .setEventHubName(eventHubName)
  .setSasKeyName(sasKeyName)
  .setSasKey(sasKey)

// Connect to the Event Hub
val pool = Executors.newScheduledThreadPool(1)
val eventHubClient = EventHubClient.create(connStr.toString(), pool)

def sendEvent(message: String) = {
  val messageData = EventData.create(message.getBytes("UTF-8"))
  eventHubClient.get().send(messageData)
  System.out.println("Sent event: " + message + "\n")
}

case class MessageBody(var timestamp: Date, var favorite: Int)
val gson: Gson = new GsonBuilder().setDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'").create()

val twitterConsumerKey = "[Placeholder: Twitter consumer key]"
val twitterConsumerSecret = "[Placeholder: Twitter consumer seceret]"
val twitterOauthAccessToken = "[Placeholder: Twitter oauth access token]"
val twitterOauthTokenSecret = "[Placeholder: Twitter oauth token secret]"

val cb = new ConfigurationBuilder()
cb.setDebugEnabled(true)
  .setOAuthConsumerKey(twitterConsumerKey)
  .setOAuthConsumerSecret(twitterConsumerSecret)
  .setOAuthAccessToken(twitterOauthAccessToken)
  .setOAuthAccessTokenSecret(twitterOauthTokenSecret)

val twitterFactory = new TwitterFactory(cb.build())
val twitter = twitterFactory.getInstance()

// Getting tweets with keyword "Azure" and sending them to the Event Hub in realtime!

val query = new Query(" #Azure ")
query.setCount(100)
query.lang("en")

var finished = false
var maxStatusId = Long.MinValue
var preMaxStatusId = Long.MinValue
val innerLoop = new Breaks
while (!finished) {
  val result = twitter.search(query)
  val statuses = result.getTweets()
  var lowestStatusId = Long.MaxValue
  innerLoop.breakable {
    for (status <- statuses.asScala) {
      if (status.getId() <= preMaxStatusId) {
        preMaxStatusId = maxStatusId
        innerLoop.break
      }
      if(!status.isRetweet()) {
        sendEvent(gson.toJson(new MessageBody(status.getCreatedAt(), status.getFavoriteCount())))
      }
      lowestStatusId = Math.min(status.getId(), lowestStatusId)
      maxStatusId = Math.max(status.getId(), maxStatusId)
    }
  }
  
  if (lowestStatusId == Long.MaxValue) {
    preMaxStatusId = maxStatusId
  }
  Thread.sleep(10000)
  query.setMaxId(lowestStatusId - 1)
}

// Close connection to the Event Hub
eventHubClient.get().close()
pool.shutdown()
```

Voor het uitvoeren van het notitieblok, drukt u op **SHIFT + ENTER**. De volgende uitvoer wordt weergegeven. Elke gebeurtenis in de uitvoer is een combinatie van de tijdstempel en het aantal ', zoals' s die zijn opgenomen in de Event Hubs.

    Sent event: {"timestamp":"2019-04-24T09:39:40.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:38:48.000Z","favorite":1}

    Sent event: {"timestamp":"2019-04-24T09:38:36.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:37:27.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:37:00.000Z","favorite":2}

    Sent event: {"timestamp":"2019-04-24T09:31:11.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:30:15.000Z","favorite":0}

    Sent event: {"timestamp":"2019-04-24T09:30:02.000Z","favorite":1}

    ...
    ...

## <a name="read-tweets-from-event-hubs"></a>Tweets lezen van Event Hubs

In de **AnalyzeTweetsFromEventHub** laptop, plak de volgende code en vervang de tijdelijke aanduiding door waarden voor de detectie van afwijkingen-resource die u eerder hebt gemaakt. Dit notitieblok leest de tweets die u eerder hebt gestreamd naar Event Hubs met behulp van het **SendTweetsToEventHub**-notitieblok.

Schrijf eerst een client voor het aanroepen van de detectie van afwijkingen. 
```scala

//
// Anomaly Detection Client
//

import java.io.{BufferedReader, DataOutputStream, InputStreamReader}
import java.net.URL
import java.sql.Timestamp

import com.google.gson.{Gson, GsonBuilder, JsonParser}
import javax.net.ssl.HttpsURLConnection

case class Point(var timestamp: Timestamp, var value: Double)
case class Series(var series: Array[Point], var maxAnomalyRatio: Double, var sensitivity: Int, var granularity: String)
case class AnomalySingleResponse(var isAnomaly: Boolean, var isPositiveAnomaly: Boolean, var isNegativeAnomaly: Boolean, var period: Int, var expectedValue: Double, var upperMargin: Double, var lowerMargin: Double, var suggestedWindow: Int)
case class AnomalyBatchResponse(var expectedValues: Array[Double], var upperMargins: Array[Double], var lowerMargins: Array[Double], var isAnomaly: Array[Boolean], var isPositiveAnomaly: Array[Boolean], var isNegativeAnomaly: Array[Boolean], var period: Int)

object AnomalyDetector extends Serializable {

  // Cognitive Services API connection settings
  val subscriptionKey = "[Placeholder: Your Anomaly Detector resource access key]"
  val endpoint = "[Placeholder: Your Anomaly Detector resource endpoint]"
  val latestPointDetectionPath = "/anomalydetector/v1.0/timeseries/last/detect"
  val batchDetectionPath = "/anomalydetector/v1.0/timeseries/entire/detect";
  val latestPointDetectionUrl = new URL(endpoint + latestPointDetectionPath)
  val batchDetectionUrl = new URL(endpoint + batchDetectionPath)
  val gson: Gson = new GsonBuilder().setDateFormat("yyyy-MM-dd'T'HH:mm:ss.SSS'Z'").setPrettyPrinting().create()

  def getConnection(path: URL): HttpsURLConnection = {
    val connection = path.openConnection().asInstanceOf[HttpsURLConnection]
    connection.setRequestMethod("POST")
    connection.setRequestProperty("Content-Type", "text/json")
    connection.setRequestProperty("Ocp-Apim-Subscription-Key", subscriptionKey)
    connection.setDoOutput(true)
    return connection
  }

  // Handles the call to Cognitive Services API.
  def processUsingApi(request: String, path: URL): String = {
    println(request)
    val encoded_text = request.getBytes("UTF-8")
    val connection = getConnection(path)
    val wr = new DataOutputStream(connection.getOutputStream())
    wr.write(encoded_text, 0, encoded_text.length)
    wr.flush()
    wr.close()

    val response = new StringBuilder()
    val in = new BufferedReader(new InputStreamReader(connection.getInputStream()))
    var line = in.readLine()
    while (line != null) {
      response.append(line)
      line = in.readLine()
    }
    in.close()
    return response.toString()
  }

  // Calls the Latest Point Detection API.
  def detectLatestPoint(series: Series): Option[AnomalySingleResponse] = {
    try {
      println("Process Timestamp: " + series.series.apply(series.series.length-1).timestamp.toString + ", size: " + series.series.length)
      val response = processUsingApi(gson.toJson(series), latestPointDetectionUrl)
      println(response)
      // Deserializing the JSON response from the API into Scala types
      val anomaly = gson.fromJson(response, classOf[AnomalySingleResponse])
      Thread.sleep(5000)
      return Some(anomaly)
    } catch {
      case e: Exception => {
        println(e)
        e.printStackTrace()
        return None
      }
    }
  }

  // Calls the Batch Detection API.
  def detectBatch(series: Series): Option[AnomalyBatchResponse] = {
    try {
      val response = processUsingApi(gson.toJson(series), batchDetectionUrl)
      println(response)
      // Deserializing the JSON response from the API into Scala types
      val anomaly = gson.fromJson(response, classOf[AnomalyBatchResponse])
      Thread.sleep(5000)
      return Some(anomaly)
    } catch {
      case e: Exception => {
        println(e)
        return None
      }
    }
  }
}
```

Voor het uitvoeren van het notitieblok, drukt u op **SHIFT + ENTER**. De volgende uitvoer wordt weergegeven.

    import java.io.{BufferedReader, DataOutputStream, InputStreamReader}
    import java.net.URL
    import java.sql.Timestamp
    import com.google.gson.{Gson, GsonBuilder, JsonParser}
    import javax.net.ssl.HttpsURLConnection
    defined class Point
    defined class Series
    defined class AnomalySingleResponse
    defined class AnomalyBatchResponse
    defined object AnomalyDetector

Bereid een statistische functie voor toekomstig gebruik.
```scala
//
// User Defined Aggregation Function for Anomaly Detection
//

import org.apache.spark.sql.Row
import org.apache.spark.sql.expressions.{MutableAggregationBuffer, UserDefinedAggregateFunction}
import org.apache.spark.sql.types.{StructType, TimestampType, FloatType, MapType, BooleanType, DataType}
import scala.collection.immutable.ListMap

class AnomalyDetectorAggregationFunction extends UserDefinedAggregateFunction {
  override def inputSchema: StructType = new StructType().add("timestamp", TimestampType).add("value", FloatType)
  
  override def bufferSchema: StructType = new StructType().add("point", MapType(TimestampType, FloatType))
  
  override def dataType: DataType = BooleanType
  
  override def deterministic: Boolean = false
  
  override def initialize(buffer: MutableAggregationBuffer): Unit = {
    buffer(0) = Map()
  }
  
  override def update(buffer: MutableAggregationBuffer, input: Row): Unit = {
    buffer(0) = buffer.getAs[Map[java.sql.Timestamp, Float]](0) + (input.getTimestamp(0) -> input.getFloat(1))
  }
  
  override def merge(buffer1: MutableAggregationBuffer, buffer2: Row): Unit = {
    buffer1(0) = buffer1.getAs[Map[java.sql.Timestamp, Float]](0) ++ buffer2.getAs[Map[java.sql.Timestamp, Float]](0)
  }
  
  override def evaluate(buffer: Row): Any = {
    val points = buffer.getAs[Map[java.sql.Timestamp, Float]](0)
    if (points.size > 12) {
      val sorted_points = ListMap(points.toSeq.sortBy(_._1.getTime):_*)
      var detect_points: List[Point] = List()
      sorted_points.keys.foreach {
        key => detect_points = detect_points :+ new Point(key, sorted_points(key))
      }
      
      
      // 0.25 is maxAnomalyRatio. It represents 25%, max anomaly ratio in a time series.
      // 95 is the sensitivity of the algorithms.
      // Check Anomaly detector API reference (https://aka.ms/anomaly-detector-rest-api-ref)
      
      val series: Series = new Series(detect_points.toArray, 0.25, 95, "hourly")
      val response: Option[AnomalySingleResponse] = AnomalyDetector.detectLatestPoint(series)
      if (!response.isEmpty) {
        return response.get.isAnomaly
      }
    }
    
    return None
  }
}

```

Voor het uitvoeren van het notitieblok, drukt u op **SHIFT + ENTER**. De volgende uitvoer wordt weergegeven.

    import org.apache.spark.sql.Row
    import org.apache.spark.sql.expressions.{MutableAggregationBuffer, UserDefinedAggregateFunction}
    import org.apache.spark.sql.types.{StructType, TimestampType, FloatType, MapType, BooleanType, DataType}
    import scala.collection.immutable.ListMap
    defined class AnomalyDetectorAggregationFunction

Vervolgens laden van gegevens van event hub voor afwijkingsdetectie. Vervang de tijdelijke aanduiding door waarden voor uw Azure Event Hubs die u eerder hebt gemaakt.

```scala
//
// Load Data from Eventhub
//

import org.apache.spark.eventhubs._
import org.apache.spark.sql.types._
import org.apache.spark.sql.functions._

val connectionString = ConnectionStringBuilder("[Placeholder: EventHub namespace connection string]")
  .setEventHubName("[Placeholder: EventHub name]")
  .build

val customEventhubParameters =
  EventHubsConf(connectionString)
  .setConsumerGroup("$Default")
  .setMaxEventsPerTrigger(100)

val incomingStream = spark.readStream.format("eventhubs").options(customEventhubParameters.toMap).load()

val messages =
  incomingStream
  .withColumn("enqueuedTime", $"enqueuedTime".cast(TimestampType))
  .withColumn("body", $"body".cast(StringType))
  .select("enqueuedTime", "body")

val bodySchema = new StructType().add("timestamp", TimestampType).add("favorite", IntegerType)

val msgStream = messages.select(from_json('body, bodySchema) as 'fields).select("fields.*")

msgStream.printSchema

display(msgStream)

```

De uitvoer lijkt nu op de volgende afbeelding. Houd er rekening mee dat de datum in de tabel van de datum in deze zelfstudie afwijken kan aangezien de gegevens realtime.
![Load Data From Event hub](../media/tutorials/load-data-from-eventhub.png "Load Data From Event Hub")

U hebt nu in bijna realtime gegevens van Azure Event Hubs naar Azure Databricks gestreamd met behulp van de Event Hubs-connector voor Apache Spark. Raadpleeg voor meer informatie over het gebruik van de Event Hubs-connector voor Spark de [connector-documentatie](https://github.com/Azure/azure-event-hubs-spark/tree/master/docs).



## <a name="run-anomaly-detection-on-tweets"></a>Detectie van afwijkingen op tweets uitvoeren

In deze sectie kunt u anomaliedetectie uitvoeren op de tweets die werden ontvangen met de detectie van afwijkingen API. Voor deze sectie voegt u de codefragmenten aan hetzelfde **AnalyzeTweetsFromEventHub**-notitieblok toe.

Hiertoe detectie van afwijkingen, eerst moet u het aantal metrische gegevens per uur.
```scala
//
// Aggregate Metric Count by Hour
//

// If you want to change granularity, change the groupBy window. 
val groupStream = msgStream.groupBy(window($"timestamp", "1 hour"))
  .agg(avg("favorite").alias("average"))
  .withColumn("groupTime", $"window.start")
  .select("groupTime", "average")

groupStream.printSchema

display(groupStream)
```
De uitvoer lijkt nu op de volgende codefragmenten.
```
groupTime                       average
2019-04-23T04:00:00.000+0000    24
2019-04-26T19:00:00.000+0000    47.888888888888886
2019-04-25T12:00:00.000+0000    32.25
2019-04-26T09:00:00.000+0000    63.4
...
...

```

Haal vervolgens het resultaat samengevoegde uitvoer verschillen. Omdat detectie van afwijkingen is vereist voor een langere okno Historie, gebruiken we verschillen voor de geschiedenisgegevens voor het dat u wilt detecteren. Vervang de ' [tijdelijke aanduiding: tabelnaam] "met een gekwalificeerde naam van Delta-tabel moet worden gemaakt (bijvoorbeeld"tweets"). Vervang ' [tijdelijke aanduiding: mapnaam voor controlepunten] "met een tekenreekswaarde die uniek is telkens wanneer u deze code uitvoeren (bijvoorbeeld 'etl-van-eventhub-20190605').
Voor meer informatie over de verschillen Lake op Azure Databricks, Raadpleeg [Delta-Lake-handleiding](https://docs.azuredatabricks.net/delta/index.html)


```scala
//
// Output Aggregation Result to Delta
//

groupStream.writeStream
  .format("delta")
  .outputMode("complete")
  .option("checkpointLocation", "/delta/[Placeholder: table name]/_checkpoints/[Placeholder: folder name for checkpoints]")
  .table("[Placeholder: table name]")

```

Vervang de ' [tijdelijke aanduiding: tabelnaam] ' met dezelfde naam van de Delta-tabel die u hierboven hebt geselecteerd.
```scala
//
// Show Aggregate Result
//

val twitterCount = spark.sql("SELECT COUNT(*) FROM [Placeholder: table name]")
twitterCount.show()

val twitterData = spark.sql("SELECT * FROM [Placeholder: table name] ORDER BY groupTime")
twitterData.show(200, false)

display(twitterData)
```
De uitvoer zoals hieronder: 
```
groupTime                       average
2019-04-08T01:00:00.000+0000    25.6
2019-04-08T02:00:00.000+0000    6857
2019-04-08T03:00:00.000+0000    71
2019-04-08T04:00:00.000+0000    55.111111111111114
2019-04-08T05:00:00.000+0000    2203.8
...
...

```

De samengevoegde time series-gegevens is nu continu opgenomen in de verschillen. U kunt vervolgens een per uur taak voor het detecteren van de anomaliedetectie van laatste herstelpunt plannen. Vervang de ' [tijdelijke aanduiding: tabelnaam] ' met dezelfde naam van de Delta-tabel die u hierboven hebt geselecteerd.

```scala
//
// Anomaly Detection
//

import java.time.Instant
import java.time.format.DateTimeFormatter
import java.time.ZoneOffset
import java.time.temporal.ChronoUnit

val detectData = spark.read.format("delta").table("[Placeholder: table name]")

// You could use Databricks to schedule an hourly job and always monitor the latest data point
// Or you could specify a const value here for testing purpose
// For example, val endTime = Instant.parse("2019-04-16T00:00:00Z")
val endTime = Instant.now()

// This is when your input of anomaly detection starts. It is hourly time series in this tutorial, so 72 means 72 hours ago from endTime.
val batchSize = 72
val startTime = endTime.minus(batchSize, ChronoUnit.HOURS)

val DATE_TIME_FORMATTER = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss").withZone(ZoneOffset.UTC);

val series = detectData.filter($"groupTime" <= DATE_TIME_FORMATTER.format(endTime))
  .filter($"groupTime" > DATE_TIME_FORMATTER.format(startTime))
  .sort($"groupTime")

series.createOrReplaceTempView("series")

//series.show()

// Register the function to access it
spark.udf.register("anomalydetect", new AnomalyDetectorAggregationFunction)

val adResult = spark.sql("SELECT '" + endTime.toString + "' as datetime, anomalydetect(groupTime, average) as anomaly FROM series")
adResult.show()
```
Resultaat zoals hieronder: 

```
+--------------------+-------+
|           timestamp|anomaly|
+--------------------+-------+
|2019-04-16T00:00:00Z|  false|
+--------------------+-------+


That's it! Using Azure Databricks, you have successfully streamed data into Azure Event Hubs, consumed the stream data using the Event Hubs connector, and then run anomaly detection on streaming data in near real time.
Although in this tutorial, the granularity is hourly, you can always change the granularity to meet your need. 

## Clean up resources

After you have finished running the tutorial, you can terminate the cluster. To do so, in the Azure Databricks workspace, select **Clusters** from the left pane. For the cluster you want to terminate, move the cursor over the ellipsis under **Actions** column, and select the **Terminate** icon and then select **Confirm**.

![Stop a Databricks cluster](../media/tutorials/terminate-databricks-cluster.png "Stop a Databricks cluster")

If you don't manually terminate the cluster it will automatically stop, provided you selected the **Terminate after \_\_ minutes of inactivity** checkbox while creating the cluster. In such a case, the cluster will automatically stop if it has been inactive for the specified time.

## Next steps

In this tutorial, you learned how to use Azure Databricks to stream data into Azure Event Hubs and then read the streaming data from Event Hubs in real time. Advance to the next tutorial to learn how to call the Anomaly Detector API and visualize anomalies using Power BI desktop. 

> [!div class="nextstepaction"]
>[Batch anomaly detection with Power BI desktop](batch-anomaly-detection-powerbi.md)
