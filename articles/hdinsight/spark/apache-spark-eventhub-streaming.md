---
title: Apache Spark-streaming met Event Hubs in Azure HDInsight gebruiken | Microsoft Docs
description: Een Apache Spark-streaming-codevoorbeeld voor het verzenden van een gegevensstroom naar Azure Event Hub en vervolgens deze gebeurtenissen ontvangen in HDInsight Spark-cluster met een scala-toepassing bouwen.
keywords: Apache spark-streaming, spark-streaming, spark-voorbeeld, apache spark-streaming bijvoorbeeld event hub azure voorbeeld, spark-voorbeeld
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: jgao
ms.openlocfilehash: 43ae956ca284485cc68f8120a31af1c493c0b254
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a>Apache Spark-streaming: procesgegevens uit Azure Event Hubs met Spark-cluster in HDInsight

In dit artikel maakt u een Apache Spark-streaming-voorbeeldtoepassing die u de volgende stappen omvat:

1. U een zelfstandige toepassing gebruiken om op te nemen van berichten in een Azure Event Hub.

2. Met twee verschillende benaderingen ophalen u de berichten van Event Hub in realtime met behulp van een toepassing die wordt uitgevoerd in Spark-cluster in Azure HDInsight.

3. U streaming analytische pijplijnen om te blijven behouden van gegevens naar andere opslagsystemen bouwen of inzichten verkrijgen van gegevens op elk gewenst moment.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="spark-streaming-concepts"></a>Concepten Spark-Streaming

Zie voor een gedetailleerde uitleg van Spark-streaming [Apache Spark-streaming-overzicht](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview). HDInsight biedt dezelfde streaming functies voor een Spark-cluster in Azure.  

## <a name="what-does-this-solution-do"></a>Wat doet deze oplossing?

In dit artikel voor het maken van een Spark-streaming-voorbeeld, de volgende stappen uitvoeren:

1. Maak een Azure Event Hub die u een stream van gebeurtenissen ontvangt.

2. Voer een lokale zelfstandige toepassing die wordt gegenereerd gebeurtenissen en stuurt deze naar de Azure Event Hub. De voorbeeldtoepassing waarmee dit wordt gepubliceerd op [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).

3. Een streaming-toepassing op afstand uitvoeren op een Spark-cluster die streaming-gebeurtenissen van Azure Event Hub leest en analyse van de verschillende gegevensverwerking /.

## <a name="create-an-azure-event-hub"></a>Een Azure Event Hub maken

1. Meld u aan bij de [Azure Portal](https://ms.portal.azure.com), en klik op **nieuw** op linksboven op het scherm.

2. Klik op **Internet van dingen** en vervolgens op **Event Hubs**.

    ![Create event hub voor Spark-streaming-voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "maken event hub voor Spark-streaming-voorbeeld")

3. Voer op de blade **Naamruimte maken** een naam in voor de naamruimte. Kies de prijscategorie (basis of standaard). Kies ook een Azure-abonnement, resourcegroep en locatie voor het maken van de resource. Klik op **Maken** om de naamruimte te maken.

      ![Geef de naam van een event hub voor Spark-streaming-voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "bieden u de naam van een event hub voor Spark-streaming-voorbeeld")

    > [!NOTE]
    > Moet u dezelfde **locatie** als uw cluster Apache Spark in HDInsight om latentie en kosten te verminderen.
    >
    >

4. Klik in de lijst met naamruimten van Event Hubs op de zojuist gemaakte naamruimte.      


5. Klik op de blade naamruimte **Event Hubs**, en klik vervolgens op **+ Event Hub** voor het maken van nieuwe Event Hub.
   
    ![Create event hub voor Spark-streaming-voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "maken event hub voor Spark-streaming-voorbeeld")

6. Typ een naam voor uw Event Hub, stelt u het aantal partities op 10, en het bericht bewaren op 1. We zijn de berichten in deze oplossing niet archiveren zodat u kunt de rest als standaard laat en klik vervolgens op **maken**.
   
    ![Geef details van gebeurtenis hub voor Spark-streaming-voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "event hub-informatie opgeven voor Spark-streaming-voorbeeld")

7. De zojuist gemaakte Event Hub wordt vermeld in de Event Hub-blade.
    
     ![Event Hub voor de Spark-streaming-voorbeeld weergeven](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "weergave Event Hub voor de Spark-streaming-voorbeeld")

8. Klik op de blade Naamruimte (niet de specifieke Event Hub-blade) op **Gedeeld toegangsbeleid** en klik vervolgens op **RootManageSharedAccessKey**.
    
     ![Event Hub-beleid instellen voor de Spark-streaming-voorbeeld](./media/apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Event Hub ingesteld beleid voor het Spark-streaming-voorbeeld")

9. Klik op de knop kopiëren kopieert de **RootManageSharedAccessKey** primaire sleutel en de verbindingsreeks naar het Klembord. Sla deze voor gebruik verderop in de zelfstudie.
    
     ![Event Hub beleid sleutels voor de Spark-streaming-voorbeeld weergeven](./media/apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "weergave Event Hub beleid sleutels voor het voorbeeld van Spark-streaming")

## <a name="send-messages-to-azure-event-hub-using-a-sample-scala-application"></a>Berichten verzenden naar Azure Event Hub met behulp van een voorbeeldtoepassing Scala

In deze sectie gebruikt u een zelfstandige lokale Scala toepassing die een stream van gebeurtenissen wordt gegenereerd en verzonden naar Azure Event Hub die u eerder hebt gemaakt. Deze toepassing is beschikbaar op GitHub op [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer). De stappen die hier wordt ervan uitgegaan dat u al deze GitHub-opslagplaats hebben forked.

1. Zorg ervoor dat u hebt het volgende zijn geïnstalleerd op de computer waarop u deze toepassing uitvoert.

    * Oracle Java Development kit. Kunt u het installeren van [hier](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).
    * Apache Maven. U kunt downloaden van [hier](https://maven.apache.org/download.cgi). Vindt u instructies voor het installeren van Maven [hier](https://maven.apache.org/install.html).

2. Open een opdrachtprompt en navigeer naar de locatie die wordt gekloond van de GitHub-repo voor de voorbeeldtoepassing Scala en voer de volgende opdracht om de toepassing te maken.

        mvn package

3. De jar uitvoer voor de toepassing **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, wordt gemaakt onder **/target** directory. U kunt deze JAR verderop in dit artikel gebruiken voor het testen van de volledige oplossing.

## <a name="create-application-to-receive-messages-from-event-hub-into-a-spark-cluster"></a>De toepassing om berichten te ontvangen van Event Hub in een Spark-cluster maken 

Er zijn twee benaderingen Spark-Streaming en Azure Event Hubs, verbinding op basis van de ontvanger en Direct-DStream-verbinding tot stand brengen. Direct DStream gebaseerd op januari 2017 is geïntroduceerd in de 2.0.3 release. Het is veronderstelde ter vervanging van de oorspronkelijke verbinding op basis van een ontvanger, omdat deze meer zodat en resource-efficiënt. Meer details te vinden in [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs). Directe DStream ondersteunt alleen Spark 2.0 +.

### <a name="build-applications-with-the-dependency-to-spark-eventhubs-connector"></a>Toepassingen met de afhankelijkheid met spark-eventhubs-connector maken

Er wordt ook de staging-versie van Spark-EventHubs publiceren in GitHub. Voor het gebruik van de staging-versie van Spark-EventHubs, wordt de eerste stap is om aan te geven van GitHub als de bron-opslagplaats met het toevoegen van de volgende vermelding aan pom.xml:

```xml
<repository>
      <id>spark-eventhubs</id>
      <url>https://raw.github.com/hdinsight/spark-eventhubs/maven-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
</repository>
```

Vervolgens kunt u de volgende afhankelijkheden toevoegen aan uw project te laten de prerelease-versie.

Maven-afhankelijkheid

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

SBT afhankelijkheid

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a>Rechtstreekse verbinding DStream

Een vooraf samengestelde jar-bestand met voorbeelden met directe DStream kan worden gedownload [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).

Het jar-bestand bevat drie voorbeelden waarvan de broncode zijn beschikbaar op [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).

Duurt [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) als een voorbeeld:

```scala
private def createStreamingContext(
  sparkCheckpointDir: String,
  batchDuration: Int,
  namespace: String,
  eventHunName: String,
  eventhubParams: Map[String, String],
  progressDir: String) = {
val ssc = new StreamingContext(new SparkContext(), Seconds(batchDuration))
ssc.checkpoint(sparkCheckpointDir)
val inputDirectStream = EventHubsUtils.createDirectStreams(
  ssc,
  namespace,
  progressDir,
  Map(eventHunName -> eventhubParams))

inputDirectStream.map(receivedRecord => (new String(receivedRecord.getBody), 1)).
  reduceByKeyAndWindow((v1, v2) => v1 + v2, (v1, v2) => v1 - v2, Seconds(batchDuration * 3),
    Seconds(batchDuration)).print()

ssc
}

def main(args: Array[String]): Unit = {

if (args.length != 8) {
  println("Usage: program progressDir PolicyName PolicyKey EventHubNamespace EventHubName" +
    " BatchDuration(seconds) Spark_Checkpoint_Directory maxRate")
  sys.exit(1)
}

val progressDir = args(0)
val policyName = args(1)
val policykey = args(2)
val namespace = args(3)
val name = args(4)
val batchDuration = args(5).toInt
val sparkCheckpointDir = args(6)
val maxRate = args(7)

val eventhubParameters = Map[String, String] (
  "eventhubs.policyname" -> policyName,
  "eventhubs.policykey" -> policykey,
  "eventhubs.namespace" -> namespace,
  "eventhubs.name" -> name,
  "eventhubs.partition.count" -> "32",
  "eventhubs.consumergroup" -> "$Default",
  "eventhubs.maxRate" -> s"$maxRate"
)

val ssc = StreamingContext.getOrCreate(sparkCheckpointDir,
  () => createStreamingContext(sparkCheckpointDir, batchDuration, namespace, name,
    eventhubParameters, progressDir))

ssc.start()
ssc.awaitTermination()
}
```

In het bovenstaande voorbeeld `eventhubParameters` zijn van de parameters die specifiek zijn voor één EventHubs-exemplaar en er moet worden doorgegeven aan de `createDirectStreams` API die wordt gemaakt van een directe DStream objecttoewijzing aan een naamruimte Event Hubs. U kunt via het object Direct DStream DStream API's die door Spark-Streaming API framework aanroepen. In dit voorbeeld berekenen we de frequentie van elk woord in de afgelopen 3 micro batch intervallen.

### <a name="receiver-based-connection"></a>Verbinding op basis van een ontvanger

Een Spark-streaming-voorbeeldtoepassing die zijn geschreven in Scala die gebeurtenissen ontvangt en de aan verschillende bestemmingen routeren, is beschikbaar op [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples). Volg onderstaande stappen voor het bijwerken van de toepassing voor de configuratie van uw Event Hub en de uitvoer jar maken.

1. IntelliJ IDEA Start en selecteer in het startscherm **uitchecken uit versiebeheer** en klik vervolgens op **Git**.
   
    ![Apache Spark-streaming-voorbeeld - get-bronnen van Git](./media/apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark-streaming-voorbeeld - get-bronnen van Git")

2. In de **kloon opslagplaats** dialoogvenster Geef de URL naar de Git-opslagplaats voor klonen uit, geeft u de map te klonen en klikt u op **kloon**.
   
    ![Apache Spark-streaming-voorbeeld - kloon van Git](./media/apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark-streaming-voorbeeld - kloon van Git")
3. Volg de aanwijzingen tot het project volledig is gekloond. Druk op **Alt + 1** openen de **Project-weergave**. Het bestand is vergelijkbaar met het volgende.
   
    ![Apache Spark-streaming-voorbeeld - Project-weergave](./media/apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark-streaming-voorbeeld - Project-weergave")
4. Zorg ervoor dat de toepassingscode is gecompileerd met Java8. Om te controleren of dit, klikt u op **bestand**, klikt u op **projectstructuur**, en klik op de **Project** tabblad, zorg ervoor dat Project taal is ingesteld op **8 - lambda's type aantekeningen, enz**.
   
    ![Apache Spark-streaming-voorbeeld - Set compiler](./media/apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark-streaming-voorbeeld: Set-compiler")
5. Open de **pom.xml** en zorg ervoor dat de versie Spark juist is. Onder `<properties>` knooppunt, zoekt u naar het volgende fragment en controleer of de versie van Spark.

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. De toepassing vereist een afhankelijkheid jar aangeroepen **JDBC-stuurprogramma jar**. Dit is vereist voor de berichten ontvangen van de Event Hub naar een Azure SQL database schrijven. U kunt deze jar downloaden (v4.1 of hoger) van [hier](https://msdn.microsoft.com/sqlserver/aa937724.aspx). Verwijzing naar deze jar toevoegen in de project-bibliotheek. De volgende stappen uitvoeren:
     
     1. Vanuit IntelliJ IDEA venster waarin het hebben van de toepassing openen, klikt u op **bestand**, klikt u op **projectstructuur**, en klik vervolgens op **bibliotheken**. 
     2. Klik op het pictogram toevoegen (![pictogram toevoegen](./media/apache-spark-eventhub-streaming/add-icon.png)), klikt u op **Java**, en navigeer vervolgens naar de locatie waar u de jar JDBC-stuurprogramma gedownload. Volg de aanwijzingen voor het jar-bestand toevoegen aan de project-bibliotheek.

         ![ontbrekende afhankelijkheden toevoegen](./media/apache-spark-eventhub-streaming/add-missing-dependency-jars.png "ontbrekende afhankelijkheid potten toevoegen")
     3. Klik op **Toepassen**.

7. Maak de jar-bestand voor uitvoer. De volgende stappen uitvoeren.

   1. In de **projectstructuur** in het dialoogvenster, klikt u op **artefacten** en klik vervolgens op het plusteken. Klik in het pop-updialoogvenster op **JAR**, en klik vervolgens op **van modules met afhankelijkheden**.      
       
       ![Apache Spark streaming voorbeeld - JAR maken](./media/apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "streaming voorbeeld Apache Spark - JAR maken")
   2. In de **maken JAR van Modules** dialoogvenster vak, klikt u op het weglatingsteken (![weglatingsteken](./media/apache-spark-eventhub-streaming/ellipsis.png)) op basis van de **Main klasse**.
   3. In de **Main-klasse selecteren** dialoogvenster vak, selecteert u een van de beschikbare klassen en klik vervolgens op **OK**.
      
       ![Apache Spark-streaming-voorbeeld - Selecteer klasse voor jar](./media/apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark-streaming-voorbeeld - Selecteer klasse voor jar")
   4. In de **maken JAR van Modules** dialoogvenster vak, zorg ervoor dat de optie voor het **uitpakken naar het doel JAR** is geselecteerd en klik vervolgens op **OK**. Hiermee maakt u een één JAR met alle afhankelijkheden.
      
       ![Apache Spark streaming voorbeeld - jar maken van modules](./media/apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "streaming voorbeeld Apache Spark - jar van modules maken")
   5. De **uitvoer indeling** tabblad bevat alle potten die opgenomen als onderdeel van het Maven-project zijn. U kunt selecteren en het verwijderen van ongewenste waarop de toepassing Scala heeft geen directe afhankelijkheid. Voor de toepassing hier maakt, kunt u alles behalve het laatste bestand (**spark-streaming-gegevens-persistentie-voorbeelden gecompileerd uitvoer**). Selecteer de potten om te verwijderen en klik vervolgens op de **verwijderen** pictogram (![pictogram verwijderen](./media/apache-spark-eventhub-streaming/delete-icon.png)).
      
       ![Apache Spark-streaming-voorbeeld - delete uitgepakt potten](./media/apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark-streaming-voorbeeld - potten uitgepakt verwijderen")
      
       Zorg ervoor dat **bouwen op Controleer** selectievakje is ingeschakeld, die zorgt ervoor dat de jar wordt gemaakt telkens wanneer het project wordt gemaakt of bijgewerkt. Klik op **Toepassen**.
   6. In de **uitvoer indeling** tabblad direct aan de onderkant van de **beschikbare elementen** vak hebt u de SQL JDBC jar die u eerder hebt toegevoegd aan de project-bibliotheek. U moet deze optie om te toevoegen de **uitvoer indeling** tabblad. Met de rechtermuisknop op het jar-bestand en klik vervolgens op **uitpakken naar uitvoer hoofdmap**.
      
       ![Apache Spark-streaming-voorbeeld - extract afhankelijkheid jar](./media/apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark-streaming-voorbeeld - extract afhankelijkheid jar")  
      
       De **uitvoer indeling** tabblad ziet er nu als volgt.
      
       ![Apache Spark-streaming-voorbeeld - uiteindelijke uitvoer tabblad](./media/apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark-streaming-voorbeeld - tabblad uiteindelijke uitvoer")        
      
       In de **projectstructuur** in het dialoogvenster, klikt u op **toepassen** en klik vervolgens op **OK**.    
   7. Klik in de menubalk op **bouwen**, en klik vervolgens op **Project maken**. U kunt ook klikken op **bouwen artefacten** voor het maken van de jar. De uitvoer jar wordt gemaakt onder **\classes\artifacts**.
      
       ![Apache Spark streaming voorbeeld - uitvoer JAR](./media/apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "streaming voorbeeld Apache Spark - uitvoer JAR")

## <a name="run-the-application-remotely-on-a-spark-cluster-using-livy"></a>De toepassing op afstand uitvoeren op een Spark-cluster met behulp van Livy

In dit artikel kunt u hier de Apache Spark-streaming van toepassing op afstand op een Spark-cluster worden uitgevoerd. Zie voor gedetailleerde informatie over het gebruik van Livy met HDInsight Spark-cluster [taken verzenden met een Apache Spark in Azure HDInsight-cluster](apache-spark-livy-rest-interface.md). Voordat u kunt de Spark-streaming-toepassing wordt uitgevoerd, zijn er zijn een aantal dingen die u moet doen:

1. Start de lokale zelfstandige toepassing gebeurtenissen genereren en verzonden naar de Event Hub. Gebruik de volgende opdracht om dit te doen:

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. Kopieer de jar streaming (**spark-streaming-gegevens-persistentie-examples.jar**) naar de Azure Blob-opslag die is gekoppeld aan het cluster. Dit maakt de jar toegankelijk is voor Livy. U kunt [ **AzCopy**](../../storage/common/storage-use-azcopy.md), een opdrachtregelprogramma, om dit te doen. Er zijn veel andere clients die u kunt gebruiken om gegevens te uploaden. U vindt meer informatie hierover op [gegevens voor Hadoop-taken in HDInsight uploaden](../hdinsight-upload-data.md).
3. CURL installeren op de computer waarop u deze toepassingen worden uitgevoerd. We CURL gebruiken om aan te roepen de eindpunten Livy om de taken op afstand uitvoeren.

### <a name="run-the-spark-streaming-application-to-receive-the-events-into-an-azure-storage-blob-as-text"></a>Voer de Spark-streaming-toepassing voor het ontvangen van de gebeurtenissen in een Azure Storage-Blob als tekst

Open een opdrachtprompt, navigeer naar de map waar u CURL geïnstalleerd en voer de volgende opdracht (de vervangen voor de gebruikersnaam en wachtwoord en de cluster-naam):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

De parameters in het bestand **inputBlob.txt** als volgt gedefinieerd:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Laat het ons weten wat de parameters in het bestand voor invoer zijn:

* **bestand** is het pad naar het jar-bestand van toepassing op de Azure storage-account die is gekoppeld aan het cluster.
* **className** is de naam van de klasse in de Directory met jar.
* **argumenten** is de lijst met argumenten dat is vereist door de klasse
* **numExecutors** is het aantal kernen gebruikt door Spark de streaming-toepassing uit te voeren. Dit moet altijd ten minste twee keer het aantal partities van de Event Hub.
* **executorMemory**, **executorCores**, **driverMemory** zijn parameters vereiste resources toegewezen aan de streaming-toepassing.

> [!NOTE]
> U hoeft niet te maken van de uitvoermappen (EventCheckpoint, EventCount/EventCount10) die worden gebruikt als parameters. De streaming-toepassing voor u gemaakt.
>
>

Wanneer u de opdracht uitvoert, ziet u de volgende uitvoer:

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

Noteer de batch-ID in de laatste regel van de uitvoer (in dit voorbeeld is '1'). Om te bevestigen dat de toepassing wordt uitgevoerd, kunt u uw Azure storage-account die is gekoppeld aan het cluster bekijken en ziet u de **/EventCount/EventCount10** map er hebt gemaakt. Deze map moet bevatten blobs waarmee het aantal gebeurtenissen verwerkt binnen de opgegeven periode voor de parameter wordt vastgelegd **batch interval in seconden**.

De Spark-streaming-toepassing blijft actief totdat u het afsluiten. Gebruik de volgende opdracht om dit te doen:

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-the-applications-to-receive-the-events-into-an-azure-storage-blob-as-json"></a>De toepassingen voor het ontvangen van de gebeurtenissen in een Azure Storage-Blob als JSON uitvoeren
Open een opdrachtprompt, navigeer naar de map waar u CURL geïnstalleerd en voer de volgende opdracht (de vervangen voor de gebruikersnaam en wachtwoord en de cluster-naam):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

De parameters in het bestand **inputJSON.txt** als volgt gedefinieerd:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

De parameters zijn vergelijkbaar met wat u hebt opgegeven voor de tekstuitvoer van de in de vorige stap. Opnieuw, hoeft u niet de uitvoermappen (EventCheckpoint, EventCount/EventCount10) die worden gebruikt als parameters maken. De streaming-toepassing voor u gemaakt.

 Nadat u de opdracht uitvoert, u uw Azure storage-account die is gekoppeld aan het cluster bekijken kunt en u ziet de **/EventStore10** map er hebt gemaakt. Open elk bestand voorafgegaan door **onderdeel -** en ziet u de gebeurtenissen in een JSON-indeling wordt verwerkt.

### <a name="run-the-applications-to-receive-the-events-into-a-hive-table"></a>De toepassingen voor het ontvangen van de gebeurtenissen in een Hive-tabel uitvoeren
Als u wilt uitvoeren van de Spark-streaming-toepassing die gebeurtenissen naar een Hive-tabel streams moet u een aantal extra onderdelen. Dit zijn:

* datanucleus api-jdo 3.2.6.jar
* datanucleus-rdbms-3.2.9.jar
* datanucleus-core-3.2.10.jar
* hive-site.xml

De **JAR** bestanden zijn beschikbaar op uw HDInsight Spark-cluster op `/usr/hdp/current/spark-client/lib`. De **hive-site.xml** is beschikbaar op `/usr/hdp/current/spark-client/conf`.

U kunt [WinScp](http://winscp.net/eng/download.php) deze bestanden uit het cluster op uw lokale computer worden overschreven. Vervolgens kunt u hulpprogramma's voor deze bestanden te kopiëren via naar uw opslagaccount die is gekoppeld aan het cluster. Zie voor meer informatie over het uploaden van bestanden naar het opslagaccount [gegevens voor Hadoop-taken in HDInsight uploaden](../hdinsight-upload-data.md).

Zodra u de bestanden over naar uw Azure storage-account hebt gekopieerd, open een opdrachtprompt, gaat u naar de map waar u CURL geïnstalleerd en voer de volgende opdracht (de vervangen voor de gebruikersnaam en wachtwoord en de cluster-naam):

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

De parameters in het bestand **inputHive.txt** als volgt gedefinieerd:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

De parameters zijn vergelijkbaar met wat u hebt opgegeven voor de tekstuitvoer van de in de vorige stappen. Opnieuw, u hoeft te maken van de uitvoermappen (EventCheckpoint, EventCount/EventCount10) of de uitvoer van de Hive-tabel (EventHiveTable10) die worden gebruikt als parameters. De streaming-toepassing voor u gemaakt. Houd er rekening mee dat de **potten** en **bestanden** optie omvat paden naar de JAR-bestanden en de hive-site.xml die u gekopieerd naar het opslagaccount.

Om te controleren dat de hive-tabel is gemaakt, gebruikt de [Ambari Hive-weergave](../hadoop/apache-hadoop-use-hive-ambari-view.md). U kunt een SELECT-query, uitvoeren om de inhoud van de tabel weer te geven.

    SELECT * FROM eventhivetable10 LIMIT 10;

Als het goed is, wordt ongeveer de volgende uitvoer weergegeven:

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-the-applications-to-receive-the-events-into-an-azure-sql-database-table"></a>De toepassingen voor het ontvangen van de gebeurtenissen in een Azure SQL database-tabel uitvoeren
Voordat u deze stap uitvoert, zorg ervoor dat u hebt een Azure SQL-database gemaakt. Zie voor instructies [maken van een SQL-database in minuten](../../sql-database/sql-database-get-started-portal.md). Voor het voltooien van deze sectie, moet u waarden voor de databasenaam, database-servernaam en de beheerdersreferenties voor de database als parameters. U hoeft niet te maken met de databasetabel. De Spark-streaming-toepassing die voor u gemaakt.

Open een opdrachtprompt, navigeer naar de map waar u CURL geïnstalleerd en voer de volgende opdracht:

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

De parameters in het bestand **inputSQL.txt** als volgt gedefinieerd:

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

Om te bevestigen dat de toepassing wordt uitgevoerd, kunt u verbinding met de Azure SQL database met SQL Server Management Studio. Zie voor instructies over hoe u dat doet, [verbinding maken met SQL Database met SQL Server Management Studio](../../sql-database/sql-database-connect-query-ssms.md). Nadat u met de database verbonden bent, kunt u gaat naar de **EventContent** tabel die is gemaakt door de streaming-toepassing. U kunt een snelle query om de gegevens uit de tabel uitvoeren. Voer de volgende query:

    SELECT * FROM EventCount

De uitvoer ziet er als volgt uit:

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)
* [Ontwerp van de verbinding op basis van de ontvanger en Direct DStream](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](../hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](../hdinsight-apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: ../hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
