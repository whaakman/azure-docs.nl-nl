---
title: Verbinding maken met Apache Spark op Azure Cosmos DB | Microsoft Docs
description: Meer informatie over de Azure Cosmos DB Spark-connector waarmee u verbinding maken met Apache Spark op Azure Cosmos DB. U kunt gedistribueerde aggregaties uitvoeren op het systeem met meerdere tenants, wereldwijd gedistribueerde database van Microsoft.
keywords: Apache spark
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: ramkris
ms.openlocfilehash: 4fa28e2d3f5d94d7ab47ec3b1e1e3240e5c770de
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48042970"
---
# <a name="accelerate-big-data-analytics-by-using-the-apache-spark-to-azure-cosmos-db-connector"></a>Versnel de analyse van big data met behulp van de Apache Spark op Azure Cosmos DB-connector
 
De Apache Spark op Azure Cosmos DB-connector kunt Azure Cosmos DB moet een invoer of uitvoer voor Apache Spark-taken. Verbinding maken [Spark](http://spark.apache.org/) naar [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) helpt data science-problemen ontdekt op te lossen. U kunt Azure Cosmos DB gebruiken om snel te behouden en gegevens op te vragen. De connector maakt efficiënt gebruik van de systeemeigen Azure Cosmos DB beheerde indexen. De indexen inschakelen bij te werken kolommen bij het uitvoeren van analyses en push-down-predicaat filteren op basis van snel veranderende, wereldwijd gedistribueerde gegevens. Dit soort gegevens kan variëren van Internet of Things (IoT) voor scenario's voor data science en analyse.

## <a name="connector-components"></a>Connector-onderdelen

De Spark op Azure Cosmos DB-connector heeft de volgende onderdelen:

* [Azure Cosmos DB](http://documentdb.com) kunt u te richten en zowel doorvoer en opslag elastisch schalen naar een willekeurig aantal geografische regio's.  

* [Apache Spark](http://spark.apache.org/) is een krachtig open-source engine voor gebeurtenisverwerking, gebaseerd op de snelheid, gebruiksgemak, en geavanceerde analyses.  

* [Apache Spark-cluster in Azure Databricks](https://docs.azuredatabricks.net/getting-started/index.html) kunt u Spark-taken uitvoeren op het Spark-cluster.

## <a name="connect-apache-spark-to-azure-cosmos-db"></a>Verbinding maken met Apache Spark op Azure Cosmos DB

Er zijn twee methoden voor het verbinding maken met Apache Spark en Azure Cosmos DB:

- [Azure Cosmos DB SQL Python SDK](https://github.com/Azure/azure-documentdb-python), een op basis van een Python-connector, die ook wordt aangeduid als *pyDocumentDB*.  

- [Azure Cosmos DB SQL Java SDK](https://github.com/Azure/azure-documentdb-java), een connector op basis van Java.


**Ondersteunde versies**

| Onderdeel | Versie |
|---------|-------|
|Apache Spark| 2.1.x, 2.2.x, 2.3.x |
| Scala|2.11|
| Azure Databricks-runtimeversie | > 3.4 |
| Azure Cosmos DB SQL Java SDK | 1.16.2 |

## <a name="connect-by-using-python-or-pydocumentdb-sdk"></a>Verbinding maken met behulp van Python of pyDocumentDB SDK

Het volgende diagram toont de architectuur van de implementatie van de SDK pyDocumentDB:

![Diagram van Spark op Azure Cosmos DB-gegevensoverdracht via pyDocumentDB DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow"></a>Gegevensstroom

Gegevensstroom van de implementatie pyDocumentDB is als volgt:

* Het hoofdknooppunt van Spark verbindt met het Azure Cosmos DB-gateway-knooppunt via pyDocumentDB. U alleen de verbindingen Spark en Azure Cosmos DB. Verbindingen met de bijbehorende hoofd- en gateway-knooppunten zijn transparant.  

* Het gateway-knooppunt wordt de query op basis van Azure Cosmos DB, waarbij de query later wordt uitgevoerd op de verzameling partities in de gegevensknooppunten. Het antwoord voor deze query's wordt verzonden naar het gateway-knooppunt en die resultatenset wordt geretourneerd naar het hoofdknooppunt van Spark.  

* Volgende query's (bijvoorbeeld op een Spark-gegevensframe) worden verzonden naar de Spark-worker-knooppunten voor verwerking.  

Communicatie tussen Spark en Azure Cosmos DB is beperkt tot de Spark-hoofdknooppunt en Azure Cosmos DB-gateway-knooppunten. De query's gaan zo snel mogelijk de transportlaag is opgehaald tussen deze twee knooppunten kunt.

Voer de volgende stappen uit voor Spark verbinding met Azure Cosmos DB met behulp van pyDocumentDB SDK:

1. Maak een [Azure Databricks-werkruimte](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) en een [Spark-cluster](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks). Azure Databricks runtime-versie 4.0 omvat Apache Spark 2.3.0 en Scala 2.11 binnen die werkruimte.  

2. Als het cluster is gemaakt en wordt uitgevoerd, gaat u naar **werkruimte** > **maken** > **bibliotheek**.  
3. Kies in het dialoogvenster Nieuwe bibliotheek **Python-ei uploaden of PyPi** als de bron. Geef **pydocumentdb** als de naam en selecteer **bibliotheek installeren**. pyDocumentdb SDK is al gepubliceerd naar de pip-pakketten, zodat u kunt zoeken en installeren. 

   ![Schermopname van het dialoogvenster Nieuwe-bibliotheek](./media/spark-connector/create-library.png)

4. Nadat de bibliotheek is geïnstalleerd, koppelt u deze aan het cluster dat u eerder hebt gemaakt.  

5. Ga naar **werkruimte** > **maken** > **Notebook**.  

6. In de **notitieblok maken** in het dialoogvenster, voer een beschrijvende naam in en kies **Python** als de taal. In de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

7. Voer enkele Spark-query's met behulp van de vluchten sample van gegevens die worden gehost in Azure Cosmos DB-account 'doctorwho'. (Dit account is openbaar toegankelijk is.) De [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats als host fungeert voor de HTML-versie van de notebook. Downloaden van bestanden van de opslagplaats en gaat u naar `\samples\Documentation_Samples\Read_Batch_PyDocumentDB.html`. U kunt de notebook in uw Azure Databricks-account importeren en deze uitvoert. De volgende sectie wordt de functionaliteit van de codeblokken in detail uitgelegd.

Het volgende codefragment laat zien hoe de pyDocumentDB SDK importeren en een query uitvoeren in de Spark-context. Zoals vermeld in het codefragment bevat de pyDocumentDB SDK de verbindingsparameters vereist voor het verbinding maken met de Azure Cosmos DB-account. De invoer van de vereiste bibliotheken, en configureert deze de hoofdsleutel en de host, het maken van de Azure Cosmos DB-client (pydocumentdb.document_client).


```python
# Import Necessary Libraries
import pydocumentdb
from pydocumentdb import document_client
from pydocumentdb import documents
import datetime

# Configuring the connection policy (allowing for endpoint discovery)
connectionPolicy = documents.ConnectionPolicy()
connectionPolicy.EnableEndpointDiscovery
connectionPolicy.PreferredLocations = ["Central US", "East US 2", "Southeast Asia", "Western Europe","Canada Central"]

# Set keys to connect to Azure Cosmos DB
masterKey = 'le1n99i1w5l7uvokJs3RT5ZAH8dc3ql7lx2CG0h0kK4lVWPkQnwpRLyAN0nwS1z4Cyd1lJgvGUfMWR3v8vkXKA=='
host = 'https://doctorwho.documents.azure.com:443/'
client = document_client.DocumentClient(host, {'masterKey': masterKey}, connectionPolicy)

```

Vervolgens kunt u query's uitvoeren. Het volgende codefragment maakt verbinding met de verzameling airports.codes in het account doctorwho en voert een query voor het uitpakken van de luchthaven steden in de staat Washington. 

```python
# Configure Database and Collections
databaseId = 'airports'
collectionId = 'codes'

# Configurations the Azure Cosmos DB client will use to connect to the database and collection
dbLink = 'dbs/' + databaseId
collLink = dbLink + '/colls/' + collectionId

# Set query parameter
querystr = "SELECT c.City FROM c WHERE c.State='WA'"

```

Nadat u de query uitvoert, is het resultaat een 'query_iterable. QueryIterable' die wordt geconverteerd naar een Python-lijst. Deze lijst wordt op zijn beurt geconverteerd naar een Spark-gegevensframe. 

```python
# Query documents
query = client.QueryDocuments(collLink, querystr, options=None, partition_key=None)

# Query for partitioned collections
# query = client.QueryDocuments(collLink, query, options= { 'enableCrossPartitionQuery': True }, partition_key=None)

# Create `df` Spark DataFrame from `elements` Python list
df = spark.createDataFrame(list(query))

# Show data
df.show()
```

### <a name="considerations-when-using-pydocumentdb-sdk"></a>Overwegingen bij het gebruik van pyDocumentDB SDK
Spark verbinden met Azure Cosmos DB met behulp van pyDocumentDB die SDK wordt aanbevolen in de volgende scenario's:

* Wilt u Python gebruiken.  

* U kunt een relatief klein resultatenset uit Azure Cosmos DB op Spark zijn geretourneerd. Houd er rekening mee dat de onderliggende gegevensset in Azure Cosmos DB kan behoorlijk groot zijn, en u filters zijn toegepast of met het predicaat filters uitvoeren in uw Azure Cosmos DB-gegevensbron.

## <a name="connect-by-using-the-java-sdk"></a>Verbinding maken met behulp van de Java-SDK

Het volgende diagram toont de architectuur van Azure Cosmos DB SQL Java SDK-implementatie en gegevens worden verplaatst tussen de Spark-worker-knooppunten:

![Diagram van de gegevensstroom in de Spark op Azure Cosmos DB-connector](./media/spark-connector/spark-connector.png)

### <a name="data-flow"></a>Gegevensstroom

De gegevensstroom van de uitvoering van Java-SDK is als volgt:

* Het Spark-hoofdknooppunt verbindt met het Azure Cosmos DB-gateway-knooppunt om op te halen van de partitiekaart. U opgeven alleen de Spark- en Azure Cosmos DB-verbindingen. Verbindingen met de bijbehorende hoofd- en gateway-knooppunten zijn transparant.  

* Deze informatie wordt verstrekt terug naar het hoofdknooppunt van Spark. Op dit moment zou het mogelijk de query voor het bepalen van de partities en de locaties in Azure Cosmos DB die u nodig hebt om toegang te parseren.  

* Deze informatie wordt verzonden naar de Spark-worker-knooppunten.  

* De werkrolknooppunten Spark rechtstreeks verbinding maken met de Azure Cosmos DB-partities, om te halen en de gegevens terug naar de Spark-partities in de worker-knooppunten.  

Communicatie tussen Spark en Azure Cosmos DB is aanzienlijk sneller, omdat de verplaatsing van gegevens tussen de Spark-worker-knooppunten en de Azure Cosmos DB-gegevensknooppunten (partities). In dit voorbeeld kunt u enkele voorbeeldgegevens Twitter verzenden naar het Azure Cosmos DB-account en Spark-query's uitvoeren met behulp van die gegevens. Gebruik de volgende stappen uit om te schrijven voorbeeld Twitter-gegevens met Azure Cosmos DB:

1. Maak een [Azure Cosmos DB SQL API-account](create-sql-api-dotnet.md#create-a-database-account) en [toevoegen van een verzameling](create-sql-api-dotnet.md#add-a-collection) aan het account.  

2. Download de [TwitterCosmosDBFeed](https://github.com/tknandu/TwitterCosmosDBFeed) voorbeeld uit GitHub. U gebruikt deze feed voorbeeld Twitter-gegevens schrijven naar Azure Cosmos DB.  

3. Open een opdrachtprompt en Tweepy en pyDocumentdb-modules installeren door het uitvoeren van de volgende opdrachten:

   ```bash
   pip install tweepy==3.3.0
   pip install pyDocumentDB
   ```

4. Pak de inhoud van het voorbeeld van Twitter-feed en open het bestand config.py. Werk de waarden van de hoofdsleutel, host, collectionId, databaseId en preferredLocations.  

5. Ga naar `http://apps.twitter.com/`, en de Twitter-feed van de toepassing te registreren. Nadat een naam voor uw toepassing is gekozen, ontvangt u met een **consumer key, consumer secret, toegangstoken en access token secret**. Kopieer deze waarden en werk deze bij in het bestand config.py voor de de Twitter-feed programmatische toegang tot toepassingen op Twitter.   

6. Sla het bestand config.py op. Open een opdrachtprompt en voer de Python-toepassing met behulp van de volgende opdracht uit:

   ```bash
   Python driver.py
   ```

7. Ga naar de Azure Cosmos DB-verzameling in de portal en controleer of dat de Twitter-gegevens worden geschreven naar de verzameling.

### <a name="find-and-attach-the-java-sdk-to-the-spark-cluster"></a>Zoeken en de Java-SDK te koppelen aan het Spark-cluster

1. Maak een [Azure Databricks-werkruimte](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) en een [Spark-cluster](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks). Azure Databricks runtime-versie 4.0 omvat Apache Spark 2.3.0 en Scala 2.11 binnen die werkruimte.  

2. Als het cluster is gemaakt en wordt uitgevoerd, gaat u naar **werkruimte** > **maken** > **bibliotheek**.  

3. Uit de **nieuwe bibliotheek** dialoogvenster vak, kiest u **Maven-coördinaat** als de bron. Geef de coördinaat waarde **com.microsoft.azure:azure-cosmosdb-spark_2.3.0_2.11:1.2.0**, en selecteer **bibliotheek maken**. De afhankelijkheden van Maven worden opgelost en het pakket wordt toegevoegd aan uw werkruimte. In de voorgaande Maven-coördinaat indeling 2.3.0 vertegenwoordigt de Spark-versie en 2.11 vertegenwoordigt de Scala-versie 1.2.0 vertegenwoordigt de versie van Azure Cosmos DB-connector. 

4. Nadat de bibliotheek is geïnstalleerd, koppelt u deze aan het cluster dat u eerder hebt gemaakt. 

In dit artikel ziet u het gebruik van de Spark-connector Java SDK in de volgende scenario's:

* Twitter-gegevens lezen uit Azure Cosmos DB.  

* Twitter-lezen gegevens die met Azure Cosmos DB is streaming.  

* Twitter-gegevens schrijven naar Azure Cosmos DB. 

### <a name="read-twitter-data-from-azure-cosmos-db"></a>Twitter-gegevens lezen uit Azure Cosmos DB
 
In deze sectie maakt uitvoeren u Spark query's om te lezen van een batch van Twitter-gegevens uit Azure Cosmos DB. De [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats als host fungeert voor de HTML-versie van de notebook. Downloaden van bestanden van de opslagplaats en gaat u naar `\samples\Documentation_Samples\Read_Batch_Twitter_Data.html`. U kunt de notebook in uw Azure Databricks-account importeren en bijwerken van de URI-account, de hoofdsleutel, database en namen van verzamelingen. U kunt uitvoeren van het notitieblok, of maak als volgt:

1. Ga naar uw Azure Databricks-account en selecteer **werkruimte** > **maken** > **Notebook**. 

2. In de **notitieblok maken** in het dialoogvenster, voer een beschrijvende naam in en kies **Python** als de taal. In de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

3. Het eindpunt, de hoofdsleutel, database en verzameling waarden verbinding maken met het account bijwerken. Tweets lezen met behulp van de opdracht spark.read.format().

   ```python
   # Configuration Map
   tweetsConfig = {
   "Endpoint" : "<Your Azure Cosmos DB endpoint>",
   "Masterkey" : "<Primary key of your Azure Cosmos DB account>",
   "Database" : "<Your Azure Cosmos DB database name>",
   "Collection" : "<Your Azure Cosmos DB collection name>", 
   "preferredRegions" : "East US",
   "SamplingRatio" : "1.0",
   "schema_samplesize" : "200000",
   "query_custom" : "SELECT c.id, c.created_at, c.user.screen_name, c.user.lang, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   }
   # Read Tweets
   tweets = spark.read.format("com.microsoft.azure.cosmosdb.spark").options(**tweetsConfig).load()
   tweets.createOrReplaceTempView("tweets")
   #tweets.cache()

   ```

4. Voer de query uit om het aantal tweets door verschillende hashtags van gegevens in de cache. 

   ```python
   %sql
   select hashtags.text, count(distinct id) as tweets
   from (
   select 
     explode(hashtags) as hashtags,
     id
   from tweets
   ) a
   group by hashtags.text
   order by tweets desc
   limit 10
   ```

Java SDK biedt ondersteuning voor de volgende waarden voor de toewijzing van de configuratie: 

|Instelling  |Beschrijving  |
|---------|---------|
|query_maxdegreeofparallelism  | Stelt het aantal gelijktijdige bewerkingen uitvoeren op de client tijdens parallelle queryuitvoering. Als deze is ingesteld op een waarde die groter is dan 0, beperkt het aantal gelijktijdige bewerkingen op de toegewezen waarde. Als deze is ingesteld op minder dan 0, besluit het systeem automatisch het aantal gelijktijdige bewerkingen om uit te voeren. Als de connector wordt elke partitie van de verzameling met een executor toegewezen, is deze waarde geen effect van de leesbewerking.        |
|query_maxbuffereditemcount     |    Hiermee stelt u het maximum aantal items dat buffer kan worden opgeslagen op de client tijdens parallelle queryuitvoering. Als deze is ingesteld op een waarde die groter is dan 0, beperkt het aantal gebufferde items naar de toegewezen waarde. Als deze is ingesteld op minder dan 0, besluit het systeem automatisch het aantal items aan buffer.     |
|query_enablescan    |   De optie voor het inschakelen van scans op de query's die niet kunnen worden weergegeven omdat indexeren is ingesteld op de opgegeven paden worden afgemeld.       |
|query_disableruperminuteusage  |  Aanvraageenheden (ru's) voor uitschakelen / minuut capaciteit voor de query, fungeren als regular ingerichte ru's per seconde is verbruikt.       |
|query_emitverbosetraces   |   Hiermee stelt u de optie voor het toestaan van query's om te verzenden van uitgebreide traceringen voor onderzoek.      |
|query_pagesize  |   Hiermee stelt u de grootte van de pagina voor het resultaat van query voor elke queryaanvraag. Gebruik voor het optimaliseren van doorvoer, een grote pagina's te verminderen van het aantal retouren resultaten ophalen.      |
|query_custom  |  Hiermee stelt u de Azure Cosmos DB-query voor de onderdrukking van de standaardquery tijdens het ophalen van gegevens uit Azure Cosmos DB. Houd er rekening mee dat wanneer u deze waarde opgeeft, deze wordt gebruikt in plaats van een query met predicaten ook gepusht.     |

Afhankelijk van het scenario, moet u verschillende configuratiewaarden gebruiken om de prestaties en doorvoer te optimaliseren. Houd er rekening mee dat de configuratiesleutel momenteel niet hoofdlettergevoelig is en de configuratiewaarde altijd een tekenreeks is.

### <a name="read-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Lezen van Twitter-gegevens die met Azure Cosmos DB is streaming

In deze sectie maakt uitvoeren u Spark query's om te lezen van een wijzigingenfeed van streaminggegevens van Twitter. Tijdens het uitvoeren van de query's in deze sectie, zorg ervoor dat uw Twitter-feed van de app wordt uitgevoerd en gegevens met Azure Cosmos DB pompen. De [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats als host fungeert voor de HTML-versie van de notebook. Downloaden van bestanden van de opslagplaats en gaat u naar `\samples\Documentation_Samples\Read_Stream_Twitter_Data.html`. U kunt de notebook in uw Azure Databricks-account importeren en bijwerken van de URI-account, de hoofdsleutel, database en namen van verzamelingen. U kunt uitvoeren van het notitieblok, of maak als volgt:

1. Ga naar uw Azure Databricks-account en selecteer **werkruimte** > **maken** > **Notebook**.  

2. In de **notitieblok maken** in het dialoogvenster, voer een beschrijvende naam in en kies **Scala** als de taal. In de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

3. Het eindpunt, de hoofdsleutel, database en verzameling waarden verbinding maken met het account bijwerken.

   ```scala
   // This script does the following:
   // - creates a structured stream from a Twitter feed CosmosDB collection (on top of change feed)
   // - get the count of tweets
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import org.codehaus.jackson.map.ObjectMapper
   import com.microsoft.azure.cosmosdb.spark.streaming._
   import java.time._

   val sourceConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB collection name>", 
   "ConnectionMode" -> "Gateway",
   "ChangeFeedCheckpointLocation" -> "/tmp",
   "changefeedqueryname" -> "Streaming Query from Cosmos DB Change Feed Internal Count")
   ```
4. Start als een stroom lezen van de wijziging feed met behulp van de opdracht spark.readStream.format():

   ```scala
   var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
   ```
5. De query in de console wilt streamen:

   ```scala
   //**RUN THE ABOVE FIRST AND KEEP BELOW IN SEPARATE CELL
   val query = streamData.withColumn("countcol", streamData.col("id").substr(0,0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
   ```

Java SDK biedt ondersteuning voor de volgende waarden voor de toewijzing van de configuratie:

|Instelling  |Beschrijving  |
|---------|---------|
|readchangefeed   |  Geeft aan dat de inhoud is opgehaald uit cosmos DB-Wijzigingenfeed. De standaardwaarde is false.       |
|changefeedqueryname |   Een aangepaste waarde voor het identificeren van de query. Houdt de connector van de verzameling voortzetting van tokens voor verschillende query's afzonderlijk feed. Als readchangefeed true is, is dit een vereiste configuratie die lege waarde kan niet uitvoeren.      |
|changefeedcheckpointlocation  |   Het pad naar lokale bestandsopslag om vast te leggen voortzetting van tokens in het geval van storingen in knooppunten.      |
|changefeedstartfromthebeginning  |  Wordt ingesteld of wijzigingenfeed moet worden gestart vanaf het begin (true) of van het huidige punt (ONWAAR). Standaard is het begint met de huidige (ONWAAR).       |
|rollingchangefeed  |   Een Booleaanse waarde die aangeeft of de wijzigingenfeed moet uit de laatste query. De standaardwaarde is ingesteld op false, wat betekent dat de wijzigingen van de eerste lezen van de verzameling worden geteld.      |
|changefeedusenexttoken  |   Een Booleaanse waarde voor de ondersteuning van scenario's voor verwerking van fouten. Hiermee wordt aangegeven dat de huidige batch-wijzigingenfeed zonder problemen is verwerkt. De gegevensset Resiliant gedistribueerd moet de volgende voortzetting van tokens gebruiken om op te halen van de volgende batch van wijzigingen.      |
| InferStreamSchema | Een Booleaanse waarde die aangeeft of het schema van de streaming-gegevens aan het begin van streaming daarvan moet worden genomen. Standaard deze waarde is ingesteld op true. Als deze parameter is ingesteld op true en wijzigingen in het schema van de streaminggegevens nadat de gegevens worden verzameld, zullen toegevoegde eigenschappen in de streaming gegevensframe worden verwijderd. <br/><br/> Als u de streaming gegevensframe moeten schema-agnostische wilt, stelt u deze parameter op onwaar. In deze modus wordt de hoofdtekst van de documenten lezen van de Azure Cosmos DB-wijzigingenfeed verpakt in een instantie-eigenschap. Deze eigenschap is in de resulterende streaming gegevensframe, afgezien van systeem eigenschapswaarden.
 |

### <a name="connection-settings"></a>Verbindingsinstellingen

Java SDK biedt ondersteuning voor de volgende verbindingsinstellingen:

|Instelling  |Beschrijving  |
|---------|---------|
|connectionmode   |  Hiermee stelt u de verbindingsmodus die voor de interne DocumentClient wordt gebruikt om te communiceren met Azure Cosmos DB. Toegestane waarden zijn **DirectHttps** (standaardwaarde) en **Gateway**. De verbindingsmodus DirectHttps stuurt de aanvragen rechtstreeks naar de CosmosDB-partities en biedt enkele voordelen latentie.       |
|connectionmaxpoolsize   |  Hiermee stelt u de waarde van de grootte van de verbindingsgroep die wordt gebruikt door interne DocumentClient. De standaardwaarde is 100.       |
|connectionidletimeout  |  Hiermee stelt de time-outwaarde in seconden, voor niet-actieve verbindingen. De standaardwaarde is 60.       |
|query_maxretryattemptsonthrottledrequests    |  Hiermee stelt u het maximum aantal nieuwe pogingen. U gebruikt deze waarde in het geval van een aanvraag mislukt vanwege snelheidsbeperking op de client. Als deze niet is opgegeven, is de standaardwaarde 1000 nieuwe pogingen.       |
|query_maxretrywaittimeinseconds   |  Sets de maximale tijd voor nieuwe pogingen in seconden. Standaard is deze 1000 seconden.       |

### <a name="write-twitter-data-to-azure-cosmos-db"></a>Twitter-gegevens schrijven naar Azure Cosmos DB 

In deze sectie maakt uitvoeren u Spark query's voor het schrijven van een batch van Twitter-gegevens om een nieuwe verzameling in dezelfde database. De [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats als host fungeert voor de HTML-versie van de notebook. Downloaden van bestanden van de opslagplaats en gaat u naar `\samples\Documentation_Samples\Write_Batch_Twitter_Data.html`. U kunt de notebook in uw Azure Databricks-account importeren en bijwerken van de URI-account, de hoofdsleutel, database en namen van verzamelingen. U kunt uitvoeren van het notitieblok, of maak als volgt:

1. Ga naar uw Azure Databricks-account en selecteer **werkruimte** > **maken** > **Notebook**.  

2. In de **notitieblok maken** in het dialoogvenster, voer een beschrijvende naam in en kies **Scala** als de taal. In de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

3. Het eindpunt, hoofdsleutel, database en verzameling waarden verbinding maken met de te lezen en schrijven van Twitter-gegevens databaseverzameling bijwerken.

   ```scala
   %scala
   // Import Necessary Libraries
   import org.joda.time._
   import org.joda.time.format._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.config.Config

   // Maps
   val readConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000",
   "query_custom" -> "SELECT c.id, c.created_at, c.user.screen_name, c.user.location, c.text, c.retweet_count, c.entities.hashtags, c.entities.user_mentions, c.favorited, c.source FROM c"
   )
   val writeConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "preferredRegions" -> "East US",
   "SamplingRatio" -> "1.0",
   "schema_samplesize" -> "200000"
   ) 

   // Configs
   // get read
   val readConfig = Config(readConfigMap)
   val tweets = spark.read.cosmosDB(readConfig)
   tweets.createOrReplaceTempView("tweets")
   tweets.cache()

   // get write
   val writeConfig = Config(writeConfigMap)
   ```
4. Voer de query uit om het aantal tweets door verschillende hashtags van gegevens in de cache. 

   ```scala
   %sql
   select hashtags.text, count(distinct id) as tweets
   from (
   select 
     explode(hashtags) as hashtags,
     id
   from tweets
   ) a
   group by hashtags.text
   order by tweets desc
   limit 10
   ```

5. Maak een nieuwe gegevensframe van tweets tags en de gegevens opslaan in de nieuwe verzameling. Na het uitvoeren van de volgende code, kunt u terugkeren naar de portal en controleer of dat de gegevens worden geschreven naar de verzameling. 

   ```scala
   %scala
   // Import SaveMode so you can Overwrite, Append, ErrorIfExists, Ignore
   import org.apache.spark.sql.{Row, SaveMode, SparkSession}

   // Create new DataFrame of tweets tags
   val tweets_bytags = spark.sql("select '2018-06-12' as currdt, hashtags.text as hashtags, count(distinct id) as tweets from ( select explode(hashtags) as hashtags, id from tweets ) a group by hashtags.text order by tweets desc limit 10")

   // Save to Cosmos DB (using Append in this case)
   // Ensure the baseConfig contains a Read-Write Key
   // The key provided in our examples is a Read-Only Key

   tweets_bytags.write.mode(SaveMode.Overwrite).cosmosDB(writeConfig)
   ```

Java SDK biedt ondersteuning voor de volgende waarden voor de toewijzing van de configuratie:

|Instelling  |Beschrijving  |
|---------|---------|
| BulkImport | Een Booleaanse waarde die aangeeft of de gegevens moeten worden geïmporteerd met behulp van de BulkExecutor-bibliotheek. Standaard deze waarde is ingesteld op true. |
|WritingBatchSize  |   Geeft aan dat de batchgrootte moet worden gebruikt wanneer u bij het schrijven van gegevens naar Azure Cosmos DB-verzameling. <br/><br/> Als BulkImport-parameter is ingesteld op true, WritingBatchSize parameter geeft aan dat de batchgrootte van documenten die zijn opgegeven als invoer voor de importAll API van de BulkExecutor-bibliotheek. Deze waarde is standaard ingesteld op 100 kB. <br/><br/> Als BulkImport-parameter is ingesteld op false, geeft WritingBatchSize parameter aan de batchgrootte moet worden gebruikt wanneer u aan de Azure Cosmos DB-verzameling ontwikkelt. De connector verzendt aanvragen createDocument en upsertDocument asynchroon in batch. Hoe groter de batchgrootte, de meer doorvoer bereiken kan, zolang de clusterbronnen beschikbaar zijn. Aan de andere kant, Geef een kleiner aantal batchgrootte om te beperken van de snelheid en de RU-verbruik. Standaard is schrijven batchgrootte ingesteld op 500.  |
|Upsert   |  Een Booleaanse waarde tekenreeks die aangeeft of upsertDocument gebruiken in plaats van CreateDocument tijdens het schrijven naar de Azure Cosmos DB-verzameling.   |
| WriteThroughputBudget |  Een geheel getal-tekenreeks waarmee het aantal RU\s die u wilt toewijzen aan de bulksgewijs opname Spark-taak, buiten de totale doorvoer is toegewezen aan de verzameling. |


### <a name="write-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Twitter-gegevens die aan het streamen is schrijven naar Azure Cosmos DB 

In deze sectie maakt uitvoeren u Spark query's voor het schrijven van een wijzigingenfeed van streaminggegevens van Twitter om een nieuwe verzameling in dezelfde database. De [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats als host fungeert voor de HTML-versie van de notebook. Downloaden van bestanden van de opslagplaats en gaat u naar `\samples\Documentation_Samples\Write_Stream_Twitter_Data.html`. U kunt de notebook in uw Azure Databricks-account importeren en bijwerken van de URI-account, de hoofdsleutel, database en namen van verzamelingen. U kunt uitvoeren van het notitieblok, of maak als volgt:

1. Ga naar uw Azure Databricks-account en selecteer **werkruimte** > **maken** > **Notebook**.  

2. In de **notitieblok maken** in het dialoogvenster, voer een beschrijvende naam in en kies **Scala** als de taal. In de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

3. Het eindpunt, hoofdsleutel, database en verzameling waarden verbinding maken met de te lezen en schrijven van Twitter-gegevens databaseverzameling bijwerken.

   ```scala
   import com.microsoft.azure.cosmosdb.spark._
   import com.microsoft.azure.cosmosdb.spark.schema._
   import com.microsoft.azure.cosmosdb.spark.config.Config
   import com.microsoft.azure.cosmosdb.spark.streaming._

   // Configure connection to Azure Cosmos DB Change Feed (Trades)
   val ConfigMap = Map(
   // Account settings
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB source collection name>", 
   // Change feed settings
   "ReadChangeFeed" -> "true",
   "ChangeFeedStartFromTheBeginning" -> "true",
   "ChangeFeedCheckpointLocation" -> "dbfs:/cosmos-feed",
   "ChangeFeedQueryName" -> "Structured Stream Read",
   "InferStreamSchema" -> "true"
   )
   ```
4. Start als een stroom lezen van de wijziging feed met behulp van de opdracht spark.readStream.format():
 
   ```scala
   // Start reading change feed of trades as a stream
   var streamdata = spark
     .readStream
     .format(classOf[CosmosDBSourceProvider].getName)
     .options(ConfigMap)
     .load()
   ```

5. De configuratie van de doelverzameling definiëren en start het streaming-taak met behulp van writeStream.format() methode:

   ```scala
   val sinkConfigMap = Map(
   "Endpoint" -> "<Your Azure Cosmos DB endpoint>",
   "Masterkey" -> "<Primary key of your Azure Cosmos DB account>",
   "Database" -> "<Your Azure Cosmos DB database name>",
   "Collection" -> "<Your Azure Cosmos DB destination collection name>", 
   "checkpointLocation" -> "streamingcheckpointlocation6",
   "WritingBatchSize" -> "100",
   "Upsert" -> "true")

   // Start the stream writer
   val streamingQueryWriter = streamdata
    .writeStream
    .format(classOf[CosmosDBSinkProvider].getName)
    .outputMode("append")
    .options(sinkConfigMap)
    .start()
    ```

Java SDK biedt ondersteuning voor de volgende waarden voor de toewijzing van de configuratie:

|Instelling  |Beschrijving  |
|---------|---------|
|Upsert   |  Een Booleaanse waarde tekenreeks die aangeeft of upsertDocument gebruiken in plaats van CreateDocument tijdens het schrijven naar de Azure Cosmos DB-verzameling.   |
|checkpointlocation  |   Het pad naar lokale bestandsopslag om vast te leggen voortzetting van tokens in het geval van storingen in knooppunten.   |
|WritingBatchSize  |  Geeft de batchgrootte te gebruiken bij het schrijven van gegevens naar de Azure Cosmos DB-verzameling. De connector verzendt aanvragen createDocument en upsertDocument asynchroon in batch. Hoe groter de batchgrootte, de meer doorvoer bereiken kan, zolang de clusterbronnen beschikbaar zijn. Aan de andere kant, Geef een kleiner aantal batchgrootte om te beperken van de snelheid en de RU-verbruik. Standaard is schrijven batchgrootte ingesteld op 500.  |


### <a name="considerations-when-using-java-sdk"></a>Overwegingen bij het gebruik van Java-SDK

Spark verbinding te maken met Azure Cosmos DB met behulp van Java-SDK wordt aanbevolen in de volgende scenario's:

* U wilt gebruiken, Python of Scala.  

* U hebt een grote hoeveelheid gegevens over te dragen tussen Apache Spark en Azure Cosmos DB. De Java SDK presteert beter dan de pyDocumentDB. Zie voor meer informatie over het verschil van de prestaties van query's de [querytest uitgevoerd wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="next-steps"></a>Volgende stappen

Als u niet hebt gedaan, downloadt u de Spark op Azure Cosmos DB-connector van de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-opslagplaats. Bekijk de volgende aanvullende resources in de opslagplaats:

* [Aggregaties voorbeelden](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Voorbeeldscripts en -laptops](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

U kunt ook om te controleren de [Apache Spark SQL en DataFrames gegevenssets handleiding](http://spark.apache.org/docs/latest/sql-programming-guide.html), en de [Apache Spark in Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) artikel.
