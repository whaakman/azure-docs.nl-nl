---
title: 'Zelfstudie: Gegevens streamen naar Azure Databricks met behulp van Event Hubs | Microsoft Docs'
description: Informatie over het gebruik van Azure Databricks met Event Hubs voor het opnemen van streaminggegevens van Twitter en het lezen van de gegevens in bijna-realtime.
services: azure-databricks
documentationcenter: ''
author: lenadroid
manager: cgronlun
editor: cgronlun
ms.service: azure-databricks
ms.custom: mvc
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 03/27/2018
ms.author: alehall
ms.openlocfilehash: 0d168c0a127c037c1bca2a6b875e9b3d5707ba70
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2018
---
# <a name="tutorial-stream-data-into-azure-databricks-using-event-hubs"></a>Zelfstudie: Gegevens streamen naar Azure Databricks met behulp van Event Hubs

In deze zelfstudie maakt u verbinding met een systeem voor gegevensopname met Azure Databricks om in bijna-realtime gegevens te streamen in een Apache Spark-cluster. U stelt een systeem voor gegevensopname in met Azure Event Hubs en verbindt dit vervolgens met Azure Databricks voor het verwerken van de berichten die ervan afkomstig zijn. Voor toegang tot een stroom van gegevens, kunt u Twitter-API's gebruiken om tweets op te nemen in Event Hubs. Zodra u de gegevens in Azure Databricks hebt, kunt u analytische taken uitvoeren voor het verder analyseren van de gegevens. 

Aan het einde van deze zelfstudie hebt u gestreamde tweets van Twitter waarin de term 'Azure' is opgenomen en hebt u de tweets in Azure Databricks gelezen.

In de volgende afbeelding wordt de stroom van de toepassing weergegeven:

![Azure Databricks met Event Hubs](./media/databricks-stream-from-eventhubs/databricks-eventhubs-tutorial.png "Azure Databricks met Event Hubs")

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte maken
> * Een Apache Spark-cluster in Azure Databricks maken
> * Een Twitter-app voor toegang tot streaminggegevens maken
> * Notitieblokken maken in Azure Databricks
> * Bibliotheken toevoegen voor Event Hubs en Twitter-API
> * Tweets verzenden naar Event Hubs
> * Tweets lezen van Event Hubs

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

Zorg ervoor dat u aan de volgende vereisten voldoet voordat u met deze zelfstudie begint:
- Een Azure Event Hubs-naamruimte.
- Een Event Hub in de naamruimte.
- De verbindingsreeks voor toegang tot de Event Hubs-naamruimte. De verbindingsreeks moet een vergelijkbare bestandsindeling hebben als `Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<key name>;SharedAccessKey=<key value>`.
- Beleid voor gedeelde toegang en beleidssleutel voor Event Hubs.

U kunt aan deze vereisten voldoen via de stappen in het artikel [Een Azure Event Hubs-naamruimte en een event hub maken](../event-hubs/event-hubs-create.md).

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-databricks-workspace"></a>Een Azure Databricks-werkruimte maken

In deze sectie gaat u een Azure Databricks-werkruimte maken met behulp van Azure Portal.

1. Selecteer in Azure Portal **Een resource maken** > **Gegevens en analyses** > **Azure Databricks**.

    ![Databricks in Azure Portal](./media/databricks-stream-from-eventhubs/azure-databricks-on-portal.png "Databricks in Azure Portal")

3. Geef bij **Azure Databricks Service** de waarden op voor het maken van een Databricks-werkruimte.

    ![Een Azure Databricks-werkruimte maken](./media/databricks-stream-from-eventhubs/create-databricks-workspace.png "Een Azure Databricks-werkruimte maken")

    Geef de volgende waarden op:

    |Eigenschap  |Beschrijving  |
    |---------|---------|
    |**Werkruimtenaam**     | Geef een naam op voor uw Databricks-werkruimte.        |
    |**Abonnement**     | Selecteer uw Azure-abonnement in de vervolgkeuzelijst.        |
    |**Resourcegroep**     | Geef aan of u een nieuwe resourcegroep wilt maken of een bestaande groep wilt gebruiken. Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) voor meer informatie. |
    |**Locatie**     | Selecteer **VS - oost 2**. Zie [Producten beschikbaar per regio](https://azure.microsoft.com/regions/services/) voor andere beschikbare regio's.        |
    |**Prijscategorie**     |  U kunt kiezen tussen **Standard** en **Premium**. Bekijk de pagina [Prijzen voor Databricks](https://azure.microsoft.com/pricing/details/databricks/) voor meer informatie over deze categorieën.       |

    Selecteer **Vastmaken aan dashboard** en selecteer **Maken**.

4. Het duurt enkele minuten om het account te maken. Tijdens het maken van het account wordt rechts in de portal de tegel **Implementatie verzenden voor Azure Databricks** weergegeven. Mogelijk moet u op uw dashboard naar rechts scrollen om de tegel te zien. Bovenaan het scherm wordt ook een voortgangsbalk weergegeven. U kunt beide gebieden bekijken voor de voortgang.

    ![Tegel Databricks-implementatie](./media/databricks-stream-from-eventhubs/databricks-deployment-tile.png "Tegel Databricks-implementatie")

## <a name="create-a-spark-cluster-in-databricks"></a>Een Spark-cluster maken in Databricks

1. Ga in de Azure Portal naar de Databricks-werkruimte die u hebt gemaakt en selecteer **Werkruimte starten**.

2. U wordt omgeleid naar de Azure Databricks-portal. Klik in de portal op **Cluster**.

    ![Databricks in Azure](./media/databricks-stream-from-eventhubs/databricks-on-azure.png "Databricks in Azure")

3. Op de pagina **Nieuw cluster** geeft u de waarden op waarmee een nieuw cluster wordt gemaakt.

    ![Een Databricks Spark-cluster maken in Azure](./media/databricks-stream-from-eventhubs/create-databricks-spark-cluster.png "Een Databricks Spark-cluster maken in Azure")

    Accepteer alle andere standaardwaarden, anders dan de volgende:

    * Voer een naam in voor het cluster.
    * Voor dit artikel maakt u een cluster met een **4.0**-runtime.
    * Zorg ervoor dat u het selectievakje **Beëindigen na ____ minuten van inactiviteit** inschakelt. Geef een duur (in minuten) op waarna het cluster moet worden beëindigd als het niet wordt gebruikt.

    Selecteer **Cluster maken**. Zodra het cluster wordt uitgevoerd, kunt u notitieblokken koppelen aan het cluster en Spark-taken uitvoeren.

## <a name="create-a-twitter-application"></a>Een Twitter-toepassing maken

Als u een stream van tweets wilt ontvangen, maakt u een toepassing in Twitter. Volg de instructies om een Twitter-toepassing te maken en leg de waarden vast die u nodig hebt om deze zelfstudie te voltooien.

1. Ga in een webbrowser naar [Twitter Application Management](https://apps.twitter.com/) en selecteer **Create New App**.

    ![Twitter-toepassing maken](./media/databricks-stream-from-eventhubs/databricks-create-twitter-app.png "Twitter-toepassing maken")

2. Voer op de pagina **Create an application** de gegevens voor de nieuwe app in en selecteer **Create your Twitter application**.

    ![Gegevens voor Twitter-toepassing](./media/databricks-stream-from-eventhubs/databricks-provide-twitter-app-details.png "Gegevens voor Twitter-toepassing")

3. Selecteer op de toepassingspagina het tabblad **Keys and Access Tokens** en kopieer de waarden voor **Consumer Key** en **Consumer Secret**. Selecteer ook **Create my access token** om de toegangstokens te genereren. Kopieer de waarden voor **Access Token** en **Access Token Secret**.

    ![Gegevens voor Twitter-toepassing](./media/databricks-stream-from-eventhubs/twitter-app-key-secret.png "Gegevens voor Twitter-toepassing")

Sla de waarden op die u hebt opgehaald voor de Twitter-toepassing. U hebt deze waarden later in de zelfstudie nodig.

## <a name="attach-libraries-to-spark-cluster"></a>Bibliotheken koppelen aan een Apache Spark-cluster

In deze zelfstudie gebruikt u de Twitter-API's om tweets te verzenden naar Event Hubs. U gebruikt ook de [Apache Spark Event Hubs-connector](https://github.com/Azure/azure-event-hubs-spark) om gegevens naar Azure Event Hubs te lezen en schrijven. Als u deze API's wilt gebruiken als onderdeel van uw cluster, kunt u ze als bibliotheken toevoegen aan Azure Databricks en ze vervolgens aan uw Apache Spark-cluster koppelen. De volgende instructies laten zien hoe de bibliotheek wordt toegevoegd aan de map **Gedeeld** in uw werkruimte.

1.  Selecteer **Werkruimte** in de Azure Databricks-werkruimte en klik vervolgens met de rechtermuisknop op **Gedeeld**. Selecteer **Bibliotheek** > **maken** in het contextmenu.

    ![Dialoogvenster Bibliotheek toevoegen](./media/databricks-stream-from-eventhubs/databricks-add-library-option.png "Dialoogvenster Bibliotheek toevoegen")

2. Op de pagina Nieuwe bibliotheek selecteert u bij **Bron** de optie **Maven-coördinaat**. Voer bij **Coördinaat** de coördinaat in voor het pakket dat u wilt toevoegen. Dit zijn de Maven-coördinaten voor de bibliotheken die in deze zelfstudie worden gebruikt:

    * Apache Spark Event Hubs-connector - `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1`
    * Twitter API - `org.twitter4j:twitter4j-core:4.0.6`

    ![Maven-coördinaten opgeven](./media/databricks-stream-from-eventhubs/databricks-eventhub-specify-maven-coordinate.png "Maven-coördinaten opgeven")

3. Selecteer **Bibliotheek maken**.

4. Selecteer de map waaraan u de bibliotheek hebt toegevoegd en selecteer vervolgens de naam van de bibliotheek.

    ![Toe te voegen bibliotheek selecteren](./media/databricks-stream-from-eventhubs/select-library.png "Toe te voegen bibliotheek selecteren")

5. Selecteer op de bibliotheekpagina het cluster waar u de bibliotheek wilt gebruiken. Nadat de bibliotheek aan het cluster is gekoppeld, verandert de status onmiddellijk in **Toegevoegd**.

    ![Bibliotheek koppelen aan cluster](./media/databricks-stream-from-eventhubs/databricks-library-attached.png "Bibliotheek koppelen aan cluster")

6. Herhaal deze stappen voor het Twitter-pakket, `twitter4j-core:4.0.6`.

## <a name="create-notebooks-in-databricks"></a>Notitieblokken maken in Azure Databricks

In deze sectie kunt u twee notitieblokken in de Databricks-werkruimte maken met de volgende namen:

- **SendTweetsToEventHub** - een notitieblok voor producenten waarmee u tweets kunt ophalen uit Twitter en ze kunt streamen naar Event Hubs.
- **ReadTweetsFromEventHub** - een notitieblok voor consumenten waarmee u de tweets uit Event Hubs kunt lezen.

1. Selecteer **Werkruimte** in het linkerdeelvenster. Selecteer in de **Werkruimte**-vervolgkeuzelijst, **Notitieblok** > **maken**.

    ![Een notitieblok maken in Databricks](./media/databricks-stream-from-eventhubs/databricks-create-notebook.png "Een notitieblok maken in Databricks")

2. Voer in het dialoogvenster **Notitieblok maken** als naam **SendTweetsToEventHub** in, selecteer **Scala** als taal en selecteer het Apache Spark-cluster dat u eerder hebt gemaakt.

    ![Een notitieblok maken in Databricks](./media/databricks-stream-from-eventhubs/databricks-notebook-details.png "Een notitieblok maken in Databricks")

    Selecteer **Maken**.

3. Herhaal de stappen voor het maken van het notitieblok **ReadTweetsFromEventHub**.

## <a name="send-tweets-to-event-hubs"></a>Tweets verzenden naar Event Hubs

Plak in het notitieblok **SendTweetsToEventHub**de volgende code en vervang de tijdelijke aanduidingen door de waarden voor uw Event Hubs-naamruimte en Twitter-toepassing die u eerder hebt gemaakt. Dit notitieblok streamt tweets met het sleutelwoord 'Azure' in realtime naar Event Hubs.

```scala
    import java.util._
    import scala.collection.JavaConverters._
    import com.microsoft.azure.eventhubs._
    import java.util.concurrent._

    val namespaceName = "<EVENT HUBS NAMESPACE>"
    val eventHubName = "<EVENT HUB NAME>"
    val sasKeyName = "<POLICY NAME>"
    val sasKey = "<POLICY KEY>"
    val connStr = new ConnectionStringBuilder()
                .setNamespaceName(namespaceName)
                .setEventHubName(eventHubName)
                .setSasKeyName(sasKeyName)
                .setSasKey(sasKey)

    val pool = Executors.newFixedThreadPool(1)
    val eventHubClient = EventHubClient.create(connStr.toString(), pool)

    def sendEvent(message: String) = {
      val messageData = EventData.create(message.getBytes("UTF-8"))
      eventHubClient.get().send(messageData)
      System.out.println("Sent event: " + message + "\n")
    }

    import twitter4j._
    import twitter4j.TwitterFactory
    import twitter4j.Twitter
    import twitter4j.conf.ConfigurationBuilder

    // Twitter configuration!
    // Replace values below with yours

    val twitterConsumerKey = "<CONSUMER KEY>"
    val twitterConsumerSecret = "<CONSUMER SECRET>"
    val twitterOauthAccessToken = "<ACCESS TOKEN>"
    val twitterOauthTokenSecret = "<TOKEN SECRET>"

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

Voor het uitvoeren van het notitieblok, drukt u op **SHIFT + ENTER**. U ziet uitvoer zoals het onderstaande codefragment. Elke gebeurtenis in de uitvoer is een tweet die in Event Hubs met de term 'Azure' wordt opgenomen.

    Sent event: @Microsoft and @Esri launch Geospatial AI on Azure https://t.co/VmLUCiPm6q via @geoworldmedia #geoai #azure #gis #ArtificialIntelligence

    Sent event: Public preview of Java on App Service, built-in support for Tomcat and OpenJDK
    https://t.co/7vs7cKtvah
    #cloudcomputing #Azure

    Sent event: 4 Killer #Azure Features for #Data #Performance https://t.co/kpIb7hFO2j by @RedPixie

    Sent event: Migrate your databases to a fully managed service with Azure SQL Database Managed Instance | #Azure | #Cloud https://t.co/sJHXN4trDk

    Sent event: Top 10 Tricks to #Save Money with #Azure Virtual Machines https://t.co/F2wshBXdoz #Cloud

    ...
    ...

## <a name="read-tweets-from-event-hubs"></a>Tweets lezen van Event Hubs

Plak in het notitieblok **ReadTweetsFromEventHub**de volgende code en vervang de tijdelijke aanduiding door de waarden voor uw Azure Event Hubs die u eerder hebt gemaakt. Dit notitieblok leest de tweets die u eerder hebt gestreamd naar Event Hubs met behulp van het **SendTweetsToEventHub**-notitieblok.

```scala
    import org.apache.spark.eventhubs._

    // Build connection string with the above information
    val connectionString = ConnectionStringBuilder("<EVENT HUBS CONNECTION STRING>")
      .setEventHubName("<EVENT HUB NAME>")
      .build

    val customEventhubParameters =
      EventHubsConf(connectionString)
      .setMaxEventsPerTrigger(5)

    val incomingStream = spark.readStream.format("eventhubs").options(customEventhubParameters.toMap).load()

    incomingStream.printSchema

    // Sending the incoming stream into the console.
    // Data comes in batches!
    incomingStream.writeStream.outputMode("append").format("console").option("truncate", false).start().awaitTermination()
```

U krijgt de volgende uitvoer:


    root
     |-- body: binary (nullable = true)
     |-- offset: long (nullable = true)
     |-- seqNumber: long (nullable = true)
     |-- enqueuedTime: long (nullable = true)
     |-- publisher: string (nullable = true)
     |-- partitionKey: string (nullable = true)

    -------------------------------------------
    Batch: 0
    -------------------------------------------
    +------+------+--------------+---------------+---------+------------+
    |body  |offset|sequenceNumber|enqueuedTime   |publisher|partitionKey|
    +------+------+--------------+---------------+---------+------------+
    |[50 75 62 6C 69 63 20 70 72 65 76 69 65 77 20 6F 66 20 4A 61 76 61 20 6F 6E 20 41 70 70 20 53 65 72 76 69 63 65 2C 20 62 75 69 6C 74 2D 69 6E 20 73 75 70 70 6F 72 74 20 66 6F 72 20 54 6F 6D 63 61 74 20 61 6E 64 20 4F 70 65 6E 4A 44 4B 0A 68 74 74 70 73 3A 2F 2F 74 2E 63 6F 2F 37 76 73 37 63 4B 74 76 61 68 20 0A 23 63 6C 6F 75 64 63 6F 6D 70 75 74 69 6E 67 20 23 41 7A 75 72 65]                              |0     |0             |2018-03-09 05:49:08.86 |null     |null        |
    |[4D 69 67 72 61 74 65 20 79 6F 75 72 20 64 61 74 61 62 61 73 65 73 20 74 6F 20 61 20 66 75 6C 6C 79 20 6D 61 6E 61 67 65 64 20 73 65 72 76 69 63 65 20 77 69 74 68 20 41 7A 75 72 65 20 53 51 4C 20 44 61 74 61 62 61 73 65 20 4D 61 6E 61 67 65 64 20 49 6E 73 74 61 6E 63 65 20 7C 20 23 41 7A 75 72 65 20 7C 20 23 43 6C 6F 75 64 20 68 74 74 70 73 3A 2F 2F 74 2E 63 6F 2F 73 4A 48 58 4E 34 74 72 44 6B]            |168   |1             |2018-03-09 05:49:24.752|null     |null        |
    +------+------+--------------+---------------+---------+------------+

    -------------------------------------------
    Batch: 1
    -------------------------------------------
    ...
    ...

Omdat de uitvoer in een binaire modus is, gebruikt u het volgende fragment om de uitvoer te converteren naar een tekenreeks.

```scala
    import org.apache.spark.sql.types._
    import org.apache.spark.sql.functions._

    // Event Hub message format is JSON and contains "body" field
    // Body is binary, so we cast it to string to see the actual content of the message
    val messages =
      incomingStream
      .withColumn("Offset", $"offset".cast(LongType))
      .withColumn("Time (readable)", $"enqueuedTime".cast(TimestampType))
      .withColumn("Timestamp", $"enqueuedTime".cast(LongType))
      .withColumn("Body", $"body".cast(StringType))
      .select("Offset", "Time (readable)", "Timestamp", "Body")

    messages.printSchema

    messages.writeStream.outputMode("append").format("console").option("truncate", false).start().awaitTermination()
```

De uitvoer lijkt nu op die in het volgende codefragment:

    root
     |-- Offset: long (nullable = true)
     |-- Time (readable): timestamp (nullable = true)
     |-- Timestamp: long (nullable = true)
     |-- Body: string (nullable = true)

    -------------------------------------------
    Batch: 0
    -------------------------------------------
    +------+-----------------+----------+-------+
    |Offset|Time (readable)  |Timestamp |Body
    +------+-----------------+----------+-------+
    |0     |2018-03-09 05:49:08.86 |1520574548|Public preview of Java on App Service, built-in support for Tomcat and OpenJDK
    https://t.co/7vs7cKtvah
    #cloudcomputing #Azure          |
    |168   |2018-03-09 05:49:24.752|1520574564|Migrate your databases to a fully managed service with Azure SQL Database Managed Instance | #Azure | #Cloud https://t.co/sJHXN4trDk    |
    |0     |2018-03-09 05:49:02.936|1520574542|@Microsoft and @Esri launch Geospatial AI on Azure https://t.co/VmLUCiPm6q via @geoworldmedia #geoai #azure #gis #ArtificialIntelligence|
    |176   |2018-03-09 05:49:20.801|1520574560|4 Killer #Azure Features for #Data #Performance https://t.co/kpIb7hFO2j by @RedPixie                                                    |
    +------+-----------------+----------+-------+
    -------------------------------------------
    Batch: 1
    -------------------------------------------
    ...
    ...

Dat is alles. Met behulp van Azure Databricks hebt u gegevens naar Azure Event Hubs gestreamd in bijna-realtime. U hebt vervolgens de stroomgegevens gebruikt met behulp van de Event Hubs-connector voor Apache Spark. Raadpleeg voor meer informatie over het gebruik van de Event Hubs-connector voor Spark de [connector-documentatie](https://github.com/Azure/azure-event-hubs-spark/tree/master/docs).

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de zelfstudie hebt voltooid, kunt u het cluster beëindigen. Dit doet u door vanuit de Azure Databricks-werkruimte in het linkerdeelvenster **Clusters** te selecteren. Voor het cluster dat u wilt beëindigen, plaatst u de cursor op het weglatingsteken onder de kolom **Acties** en selecteert u het **beëindigingspictogram**.

![Een Databricks-cluster stopzetten](./media/databricks-stream-from-eventhubs/terminate-databricks-cluster.png "Een Databricks-cluster stopzetten")

Als u het cluster niet handmatig beëindigt, stopt het cluster automatisch, op voorwaarde dat het selectievakje **Beëindigen na __ minuten inactiviteit** is ingeschakeld tijdens het maken van het cluster. In dat geval stopt het cluster automatisch als het gedurende de opgegeven tijd inactief is geweest.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een Azure Databricks-werkruimte maken
> * Een Apache Spark-cluster in Azure Databricks maken
> * Een Twitter-app voor het genereren van streaminggegevens maken
> * Notitieblokken maken in Azure Databricks
> * Bibliotheken toevoegen voor Event Hubs en Twitter API
> * Tweets verzenden naar Event Hubs
> * Tweets lezen van Event Hubs

Ga verder naar de volgende zelfstudie voor meer informatie over het uitvoeren van sentimentanalyses op de gestreamde gegevens met behulp van Azure Databricks en [Microsoft Cognitive Services API](../cognitive-services/text-analytics/overview.md).

> [!div class="nextstepaction"]
>[Zelfstudie: Sentimentanalyse voor streaming-gegevens met behulp van Azure Databricks](databricks-sentiment-analysis-cognitive-services.md)
