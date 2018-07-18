---
title: Apache Spark verbinding te maken met Azure Cosmos DB | Microsoft Docs
description: Gebruik deze zelfstudie voor meer informatie over de Azure Cosmos DB Spark-connector waarmee u verbinding maken met Apache Spark op Azure Cosmos DB om uit te voeren van gedistribueerde aggregaties en data sciences op meerdere tenants wereldwijd gedistribueerd databasesysteem van Microsoft dat ontworpen voor de cloud.
keywords: Apache spark
services: cosmos-db
author: tknandu
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: ramkris
ms.openlocfilehash: cae66a40882231f7762af29cdeaaf658dd2aa968
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39113944"
---
# <a name="accelerate-real-time-big-data-analytics-by-using-the-spark-to-azure-cosmos-db-connector"></a>Realtime analyse van big data te versnellen met behulp van de Spark op Azure Cosmos DB-connector
 
De Spark op Azure Cosmos DB-connector kunt Azure Cosmos DB om te fungeren als invoer of uitvoer voor Apache Spark-taken. Verbinding maken [Spark](http://spark.apache.org/) naar [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) versnelt de mogelijkheid voor het oplossen van problemen met data science ontdekt waar u Azure Cosmos DB kunt gebruiken om snel te behouden en gegevens op te vragen. De Spark op Azure Cosmos DB-connector maakt efficiënter gebruik van de systeemeigen Azure Cosmos DB beheerde indexen. De indexen inschakelen bij te werken kolommen wanneer u analyses uitvoeren en gegevens, die van het Internet of Things (IoT variëren)-push-down predicaatfiltrering ten opzichte van snel veranderende wereldwijd worden gedistribueerd naar scenario's voor data science en analyse.

Meer informatie over de Spark op Azure Cosmos DB-connector in deze video:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T135/player] 

## <a name="connector-components"></a>Connector-onderdelen

De Spark op Azure Cosmos DB-connector maakt gebruik van de volgende onderdelen:

* [Azure Cosmos DB](http://documentdb.com) klanten kunnen inrichten en zowel doorvoer en opslag elastisch schalen naar een willekeurig aantal geografische regio's.  

* [Apache Spark](http://spark.apache.org/) is een krachtig open-source-engine voor gebeurtenisverwerking die gebaseerd op de snelheid, gebruiksgemak, en geavanceerde analyses.  

* [Apache Spark-cluster in Azure Databricks](https://docs.azuredatabricks.net/getting-started/index.html) spark-taken uitvoeren op het spark-cluster.

## <a name="connect-apache-spark-to-azure-cosmos-db"></a>Verbinding maken met Apache Spark op Azure Cosmos DB

Er zijn twee methoden voor het verbinding maken met Apache Spark en Azure Cosmos DB:

1. Met behulp van [Azure Cosmos DB SQL Python SDK](https://github.com/Azure/azure-documentdb-python), een spark Python op basis van Cosmos DB-connector, die ook wel 'pyDocumentDB' wordt genoemd.  

2. Met behulp van de [Azure Cosmos DB SQL Java SDK](https://github.com/Azure/azure-documentdb-java) een op Java gebaseerde spark met Cosmos DB-connector.


**Ondersteunde versies**

| Onderdeel | Versie |
|---------|-------|
|Apache Spark| 2.1.x, 2.2.x, 2.3.x |
| Scala|2.11|
| Databricks runtime-versie | > 3.4 |
| Azure Cosmos DB SQL Java SDK | 1.16.2 |

## <a name="connect-by-using-python-or-pydocumentdb-sdk"></a>Verbinding maken met behulp van Python of pyDocumentDB SDK

De volgende afbeelding toont de architectuur van pyDocumentDB SDK-implementatie:

![Spark op Azure Cosmos DB-gegevensoverdracht via pyDocumentDB DB](./media/spark-connector/spark-pydocumentdb.png)


### <a name="data-flow"></a>Gegevensstroom

Gegevensstroom van de implementatie pyDocumentDB is als volgt:

* Het hoofdknooppunt van spark verbindt met het Azure Cosmos DB-gateway-knooppunt via pyDocumentDB. Een gebruiker Hiermee geeft u de spark- en alleen Azure Cosmos DB-verbindingen. Verbindingen met de bijbehorende hoofd- en gateway-knooppunten zijn transparant voor de gebruiker.  

* Het gateway-knooppunt wordt de query op Azure Cosmos DB, waarbij de query later wordt uitgevoerd op de verzameling partities in de gegevensknooppunten. Het antwoord voor deze query's wordt verzonden naar het gateway-knooppunt en die resultatenset wordt geretourneerd naar het hoofdknooppunt van spark.  

* Volgende query's (bijvoorbeeld op een spark-gegevensframe) worden verzonden naar de Spark-worker-knooppunten voor verwerking.  

Communicatie tussen spark en Azure Cosmos DB is beperkt tot de spark-hoofdknooppunt en Azure Cosmos DB-gateway-knooppunten. De query's gaan zo snel mogelijk de transportlaag is opgehaald tussen deze twee knooppunten kunt.

Voer de volgende stappen uit voor spark verbinding met Azure Cosmos DB met behulp van pyDocumentDB SDK:

1. Maak een [Azure Databricks-werkruimte](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) en een [spark-cluster](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) (Databricks runtime-versie 4.0 (inclusief Apache Spark 2.3.0, Scala 2.11) in die werkruimte.  

2. Nadat het cluster is gemaakt en wordt uitgevoerd, gaat u naar **werkruimte** > **maken** > **bibliotheek**.  
3. Kies in het dialoogvenster Nieuwe bibliotheek **Python-ei uploaden of PyPi** opgeven als de bron **pydocumentdb** als de naam en selecteer **bibliotheek installeren**. PyDocumentdb SDK is al gepubliceerd naar de pip-pakketten, zodat u kunt deze vinden en installeren. 

   ![Maken en koppelen van bibliotheek](./media/spark-connector/create-library.png)

4. Nadat de bibliotheek is geïnstalleerd, koppelt u deze aan het cluster dat u eerder hebt gemaakt.  

5. Vervolgens gaat u naar de **werkruimte** > **maken** > **Notebook**.  

6. In de **notitieblok maken** dialoogvenster vak, typ een beschrijvende naam, kiest u **Python** als de taal. In de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

7. De eenvoud van de communicatie-transport wordt uitvoering van een query uit spark op Azure Cosmos DB met behulp van pyDocumentDB relatief eenvoudig. Vervolgens voert u enkele spark-query's met behulp van de voorbeeldgegevens van vluchten die wordt gehost in 'doctorwho' Cosmos DB-account dat openbaar toegankelijk is. De HTML-versie van de notebook wordt gehost in de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats. U moet downloaden van bestanden van de opslagplaats en navigeer naar `\samples\Documentation_Samples\Read_Batch_PyDocumentDB.html` kunt u de notebook in uw Azure Databricks-account importeren en deze uitvoert. De volgende sectie vindt u de functionaliteit van de codeblokken in gedetailleerde.

Het volgende codefragment laat zien hoe de pyDocumentDB SDK importeren en een query uitvoert in de spark-context. Zoals vermeld in het codefragment bevat de pyDocumentDB SDK de verbindingsparameters vereist voor het verbinding maken met de Azure Cosmos DB-account. Deze invoer van de vereiste bibliotheken, configureert de hoofdsleutel en de host om de Azure Cosmos DB-client (pydocumentdb.document_client) te maken.


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

Vervolgens kunt u query's uitvoeren, het volgende codefragment maakt verbinding met de verzameling airports.codes in het account DoctorWho en voert een query uit om op te halen van de luchthaven steden in de staat Washington. 

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

Nadat de query wordt uitgevoerd, is het resultaat een 'query_iterable. QueryIterable' die wordt geconverteerd naar een lijst met Python, die vervolgens wordt omgezet naar een spark-gegevensframe. 

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

## <a name="considerations-when-using-pydocumentdb-sdk"></a>Overwegingen bij het gebruik van pyDocumentDB SDK
Spark verbinden met Azure Cosmos DB met behulp van pyDocumentDB die SDK wordt aanbevolen in de volgende scenario's:

* Wilt u Python gebruiken.  

* U kunt een relatief klein resultatenset uit Azure Cosmos DB op spark zijn geretourneerd. Houd er rekening mee dat de onderliggende gegevensset in Azure Cosmos DB behoorlijk groot zijn kan en u filters zijn toegepast of met het predicaat filters uitvoeren in uw Azure Cosmos DB-gegevensbron.

## <a name="connect-by-using-the-java-sdk"></a>Verbinding maken met behulp van de Java-SDK

De volgende afbeelding toont de architectuur van Azure Cosmos DB SQL Java SDK-implementatie en gegevens worden verplaatst tussen de spark-worker-knooppunten:

![Gegevensstroom in de Spark op Azure Cosmos DB-connector](./media/spark-connector/spark-connector.png)

### <a name="data-flow"></a>Gegevensstroom

De gegevensstroom van de uitvoering van Java-SDK is als volgt:

* Het spark-hoofdknooppunt verbindt met het Azure Cosmos DB-gateway-knooppunt om op te halen van de partitiekaart. Een gebruiker geeft alleen de spark en Azure Cosmos DB-verbindingen. Verbindingen met de bijbehorende hoofd- en gateway-knooppunten zijn transparant voor de gebruiker.  

* Deze informatie wordt verstrekt terug naar het hoofdknooppunt van spark. Op dit moment zou het mogelijk de query voor het bepalen van de partities en de locaties in Azure Cosmos DB die u nodig hebt om toegang te parseren.  

* Deze informatie wordt verzonden naar de spark-worker-knooppunten.  

* De spark-worker-knooppunten verbinding met de Azure Cosmos DB-partities rechtstreeks naar de gegevens ophalen en de gegevens terug naar de spark-partities in de worker-knooppunten.  

Communicatie tussen spark en Azure Cosmos DB is aanzienlijk sneller vanwege de gegevens verplaatsing is tussen de spark-worker-knooppunten en de Azure Cosmos DB-gegevensknooppunten (partities). In dit document, wordt u voorbeeldgegevens voor twitter verzenden naar Azure Cosmos DB-account en spark-query's uitvoeren met behulp van die gegevens. Gebruik de volgende stappen uit om te schrijven voorbeeld Twitter-gegevens met Azure Cosmos DB:

1. Maak een [Azure Cosmos DB SQL API-account](create-sql-api-dotnet.md#create-a-database-account) en [toevoegen van een verzameling](create-sql-api-dotnet.md#add-a-collection) aan het account.  

2. Download de [TwitterCosmosDBFeed](https://github.com/tknandu/TwitterCosmosDBFeed) voorbeeld uit GitHub, dat wordt gebruikt voor het voorbeeld Twitter-gegevens schrijven naar Azure Cosmos DB.  

3. Open een opdrachtprompt en Tweepy en pyDocumentdb-modules installeren door het uitvoeren van de volgende opdrachten:

   ```bash
   pip install tweepy==3.3.0
   pip install pyDocumentDB
   ```

4. Pak de inhoud van Twitter-feed voorbeeld en open het bestand config.py. Werk de waarden van de hoofdsleutel, host, collectionId, databaseId en preferredLocations.  

5. Navigeer naar `http://apps.twitter.com/` en de Twitter-feed script als een nieuwe toepassing registreren. Nadat een naam en een toepassing voor uw app is gekozen, ontvangt u met een **consumer key, consumer secret, toegangstoken en access token secret**. Kopieer deze waarden en ze in het bestand config.py voor de programmatische toegang tot toepassingen voor Twitter worden bijgewerkt.   

6. Sla het bestand config.py op. Open de opdrachtprompt en voer de python-toepassing met behulp van de volgende opdracht uit:

   ```bash
   Python driver.py
   ```

7. Navigeer naar de Azure Cosmos DB-verzameling in de portal en controleer of dat de twitter-gegevens worden geschreven naar de verzameling.

### <a name="find-and-attach-java-sdk-to-the-spark-cluster"></a>Zoeken en Java-SDK te koppelen aan het spark-cluster

1. Maak een [Azure Databricks-werkruimte](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-an-azure-databricks-workspace) en een [spark-cluster](../azure-databricks/quickstart-create-databricks-workspace-portal.md#create-a-spark-cluster-in-databricks) (Databricks runtime-versie 4.0 (inclusief Apache Spark 2.3.0, Scala 2.11) in die werkruimte.  

2. Nadat het cluster is gemaakt en wordt uitgevoerd, gaat u naar **werkruimte** > **maken** > **bibliotheek**.  

3. Kies in het dialoogvenster Nieuwe bibliotheek **Maven-coördinaat** als de bron, geef de coördinaat waarde **com.microsoft.azure:azure-cosmosdb-spark_2.3.0_2.11:1.2.0**, en selecteer  **Bibliotheek maken**. De afhankelijkheden van Maven worden opgelost en het pakket wordt toegevoegd aan uw werkruimte. In de bovenstaande maven-coördinaat indeling 2.3.0 vertegenwoordigt de spark-versie en 2.11 vertegenwoordigt de scala-versie 1.2.0 vertegenwoordigt de versie van Azure Cosmos DB-connector. 

4. Nadat de bibliotheek is geïnstalleerd, koppelt u deze aan het cluster dat u eerder hebt gemaakt. 

In dit artikel ziet u het gebruik van spark-connector Java SDK in de volgende scenario's:

* Twitter-gegevens lezen uit Azure Cosmos DB  

* Twitter-gegevens die is streaming met Azure Cosmos DB lezen  

* Twitter-gegevens schrijven naar Azure Cosmos DB 

### <a name="read-twitter-data-from-azure-cosmos-db"></a>Twitter-gegevens lezen uit Azure Cosmos DB
 
In deze sectie maakt uitvoeren u spark-query's om te lezen van een batch van Twitter-gegevens uit Azure Cosmos DB. De HTML-versie van de notebook wordt gehost in de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats. U moet downloaden van bestanden van de opslagplaats en navigeer naar `\samples\Documentation_Samples\Read_Batch_Twitter_Data.html` kunt u de notebook in uw Azure Databricks-account importeren, bijwerken van het account URI, hoofdsleutel, database, namen van verzamelingen en deze uitvoert of u kunt als volgt het notitieblok maken:

1. Navigeer naar uw Azure Databricks-account en selecteer de **werkruimte** > **maken** > **Notebook**. 

2. In de **notitieblok maken** dialoogvenster vak, typ een beschrijvende naam, kiest u **Python** als de taal, in de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

3. Het eindpunt, de hoofdsleutel, de database en verzameling waarden verbinding maken met het account en tweets lezen met behulp van de opdracht spark.read.format() bijwerken.

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
|query_maxdegreeofparallelism  | Stelt het aantal gelijktijdige bewerkingen uitvoeren op de client tijdens parallelle queryuitvoering. Als deze is ingesteld op een waarde die groter is dan 0, beperkt het aantal gelijktijdige bewerkingen op de toegewezen waarde. Als deze is ingesteld op minder dan 0, besluit het systeem automatisch het aantal gelijktijdige bewerkingen om uit te voeren. Als de Connector wordt elke partitie van de verzameling met een executor toegewezen, is deze waarde geen effect van de leesbewerking.        |
|query_maxbuffereditemcount     |    Hiermee stelt u het maximum aantal items dat buffer kan worden opgeslagen op de client tijdens parallelle queryuitvoering. Als deze is ingesteld op een waarde die groter is dan 0, beperkt het aantal gebufferde items naar de toegewezen waarde. Als deze is ingesteld op minder dan 0, besluit het systeem automatisch het aantal items aan buffer.     |
|query_enablescan    |   De optie voor het inschakelen van scans op de query's dat kunnen niet worden uitgevoerd omdat het indexeren is ingesteld op de opgegeven paden worden afgemeld.       |
|query_disableruperminuteusage  |  Schakelt Aanvraageenheden (ru's) / minuut capaciteit voor het bieden van de query als gewone ingerichte ru's per seconde leeg is.       |
|query_emitverbosetraces   |   Hiermee stelt u de optie voor het toestaan van query's om te verzenden van uitgebreide traceringen voor onderzoek.      |
|query_pagesize  |   Hiermee stelt u de grootte van de pagina voor het resultaat van query voor elke queryaanvraag. Om te optimaliseren voor doorvoer, gebruikt u een grote pagina's te verminderen van het aantal retouren naar de resultaten van query's worden opgehaald.      |
|query_custom  |  Hiermee stelt u de Azure Cosmos DB-query voor de onderdrukking van de standaardquery tijdens het ophalen van gegevens uit Azure Cosmos DB. Houd er rekening mee dat wanneer deze waarde is opgegeven, deze wordt gebruikt in plaats van een query met predicaten ook gepusht.     |

Afhankelijk van het scenario, moeten verschillende configuratiewaarden worden gebruikt om de prestaties en doorvoer te optimaliseren. Houd er rekening mee dat de configuratiesleutel momenteel niet hoofdlettergevoelig is en de configuratiewaarde altijd een tekenreeks is.

### <a name="read-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Twitter-gegevens die is streaming met Azure Cosmos DB lezen

In deze sectie maakt uitvoeren u spark-query's om te lezen van een wijzigingenfeed van streaminggegevens van twitter. Tijdens het uitvoeren van de query's in deze sectie, zorg ervoor dat uw Twitter-feed van de app wordt uitgevoerd en gegevens met Azure Cosmos DB pompen. De HTML-versie van de notebook wordt gehost in de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats. U moet downloaden van bestanden van de opslagplaats en navigeer naar `\samples\Documentation_Samples\Read_Stream_Twitter_Data.html` kunt u de notebook in uw Azure Databricks-account importeren, bijwerken van het account URI, hoofdsleutel, database, namen van verzamelingen en deze uitvoert of u kunt als volgt het notitieblok maken:

1. Navigeer naar uw Azure Databricks-account en selecteer de **werkruimte** > **maken** > **Notebook**.  

2. In de **notitieblok maken** dialoogvenster vak, typ een beschrijvende naam, kiest u **Scala** als de taal, in de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

3. Het eindpunt, de hoofdsleutel, de database en verzameling waarden verbinding maken met het account bijwerken.

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
4. Feed als een stroom met behulp van de opdracht spark.readStream.format() start lezen-wijzigen:

   ```scala
   var streamData = spark.readStream.format(classOf[CosmosDBSourceProvider].getName).options(sourceConfigMap).load()
   ```
5. Streaming-query naar de console te starten:

   ```scala
   //**RUN THE ABOVE FIRST AND KEEP BELOW IN SEPARATE CELL
   val query = streamData.withColumn("countcol", streamData.col("id").substr(0,0)).groupBy("countcol").count().writeStream.outputMode("complete").format("console").start()
   ```

Java SDK biedt ondersteuning voor de volgende waarden voor de toewijzing van de configuratie:

|Instelling  |Beschrijving  |
|---------|---------|
|readchangefeed   |  Geeft aan dat de inhoud is opgehaald uit cosmos DB-Wijzigingenfeed. De standaardwaarde is false.       |
|changefeedqueryname |   Een aangepaste waarde voor het identificeren van de query. Houdt de connector van de verzameling voortzetting van tokens voor verschillende query's afzonderlijk feed. Als readchangefeedis true, dit is een vereiste configuratie die kan geen lege waarde.      |
|changefeedcheckpointlocation  |   Het pad naar lokale bestandsopslag om vast te leggen voortzetting van tokens in het geval van storingen in knooppunten.      |
|changefeedstartfromthebeginning  |  Wordt ingesteld of wijzigingenfeed moet worden gestart vanaf het begin (true) of van het huidige punt (ONWAAR). Standaard is het begint met de huidige (ONWAAR).       |
|rollingchangefeed  |   Een Booleaanse waarde die aangeeft of de wijzigingenfeed moet uit de laatste query. De standaardwaarde is ingesteld op false, wat betekent dat de wijzigingen moeten worden geteld vanaf het eerste lezen van de verzameling.      |
|changefeedusenexttoken  |   Een Booleaanse waarde voor de ondersteuning van scenario's voor verwerking van fouten. Het wordt gebruikt om aan te geven dat de huidige batch-wijzigingenfeed zonder problemen is verwerkt en de RDD de volgende voortzetting van tokens gebruiken moet om op te halen van de volgende batch van wijzigingen.      |
| InferStreamSchema | Een Booleaanse waarde waarmee wordt aangegeven of het schema van de streaming-gegevens aan het begin van streaming daarvan moet worden genomen. Standaard deze waarde is ingesteld op true. Als deze parameter is ingesteld op true en wijzigingen in het schema van de streaminggegevens nadat de gegevens worden verzameld, zullen toegevoegde eigenschappen in de streaming gegevensframe worden verwijderd. <br/><br/> Als u de streaming gegevensframe moeten schema-agnostische wilt, stelt u deze parameter op onwaar. In deze modus wordt de hoofdtekst van de documenten lezen uit Azure Cosmos DB-wijzigingenfeed verpakt in een eigenschap 'body' in de resulterende streaming gegevensframe afgezien van systeem eigenschapswaarden.
 |

### <a name="connection-settings"></a>Verbindingsinstellingen

Java SDK biedt ondersteuning voor de volgende verbindingsinstellingen:

|Instelling  |Beschrijving  |
|---------|---------|
|connectionmode   |  Hiermee stelt u de verbindingsmodus die voor de interne DocumentClient wordt gebruikt om te communiceren met Azure Cosmos DB. Toegestane waarden zijn **DirectHttps** (standaardwaarde) en **Gateway**. De verbindingsmodus DirectHttps stuurt de aanvragen rechtstreeks naar de CosmosDB-partities en biedt enkele voordelen latentie.       |
|connectionmaxpoolsize   |  Hiermee stelt u de waarde van de grootte van de verbindingsgroep die wordt gebruikt door interne DocumentClient. De standaardwaarde is 100.       |
|connectionidletimeout  |  Hiermee stelt de time-outwaarde voor niet-actieve verbindingen in seconden. De standaardwaarde is 60.       |
|query_maxretryattemptsonthrottledrequests    |  Hiermee stelt u het maximum aantal nieuwe pogingen. Deze waarde wordt gebruikt in het geval van een aanvraag mislukt vanwege snelheidsbeperking op de client. Als deze niet is opgegeven, is de standaardwaarde 1000 nieuwe pogingen.       |
|query_maxretrywaittimeinseconds   |  Sets de maximale tijd voor nieuwe pogingen in seconden. Standaard is deze 1000 seconden.       |

### <a name="write-twitter-data-to-azure-cosmos-db"></a>Twitter-gegevens schrijven naar Azure Cosmos DB 

In deze sectie maakt uitvoeren u spark-query's voor het schrijven van een batch van twitter-gegevens om een nieuwe verzameling in dezelfde database. De HTML-versie van de notebook wordt gehost in de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats. U moet downloaden van bestanden van de opslagplaats en navigeer naar `\samples\Documentation_Samples\Write_Batch_Twitter_Data.html` kunt u de notebook in uw Azure Databricks-account importeren, bijwerken van het account URI, hoofdsleutel, database, namen van verzamelingen en deze uitvoert of u kunt als volgt het notitieblok maken:

1. Navigeer naar uw Azure Databricks-account en selecteer de **werkruimte** > **maken** > **Notebook**.  

2. In de **notitieblok maken** dialoogvenster vak, typ een beschrijvende naam, kiest u **Scala** als de taal, in de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

3. Het eindpunt, de hoofdsleutel, de database en verzameling waarden verbinding maken met de te lezen en schrijven van twitter-gegevens databaseverzameling bijwerken.

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

5. Maak nieuwe gegevensframe van tweets labels en de gegevens opslaan in de nieuwe verzameling. Na het uitvoeren van de volgende code, kunt u terugkeren naar de portal en controleer of dat de gegevens worden geschreven naar de verzameling. 

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
|WritingBatchSize  |   Geeft de batchgrootte te gebruiken bij het schrijven van gegevens naar Azure Cosmos DB-verzameling. <br/><br/> Als BulkImport-parameter is ingesteld op true, WritingBatchSize parameter geeft aan dat de batchgrootte van documenten die zijn opgegeven als invoer voor de importAll API van de BulkExecutor-bibliotheek. Deze waarde is standaard ingesteld op 100 kB. <br/><br/> Als BulkImport-parameter is ingesteld op false, geeft WritingBatchSize parameter aan de batchgrootte te gebruiken bij het schrijven naar Azure Cosmos DB-verzameling. De connector verzendt aanvragen createDocument/upsertDocument asynchroon in batch. Hoe groter de batch het formaat van de meer doorvoer die we kunnen maar liefst, zolang de clusterbronnen beschikbaar zijn. Aan de andere kant, Geef een kleiner aantal batchgrootte om te beperken van de snelheid en de RU-verbruik. Standaard is schrijven batchgrootte ingesteld op 500.  |
|Upsert   |  Een Booleaanse waarde tekenreeks die aangeeft of upsertDocument moet worden gebruikt in plaats van CreateDocument bij het schrijven naar CosmosDB collection.   |
| WriteThroughputBudget |  Een geheel getal-tekenreeks waarmee het aantal RU\s die u wilt toewijzen aan de bulksgewijs opname spark-taak uit de totale doorvoer is toegewezen aan de verzameling. |


### <a name="write-twitter-data-that-is-streaming-to-azure-cosmos-db"></a>Twitter-gegevens die aan het streamen is schrijven naar Azure Cosmos DB 

In deze sectie maakt uitvoeren u spark-query's voor het schrijven van wijzigingenfeed van streaminggegevens van twitter om een nieuwe verzameling in dezelfde database. De HTML-versie van de notebook wordt gehost in de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark/tree/master) GitHub-opslagplaats. U moet downloaden van bestanden van de opslagplaats en navigeer naar `\samples\Documentation_Samples\Write_Stream_Twitter_Data.html` kunt u de notebook in uw Azure Databricks-account importeren, bijwerken van het account URI, hoofdsleutel, database, namen van verzamelingen en deze uitvoert of u kunt als volgt het notitieblok maken:

1. Navigeer naar uw Azure Databricks-account en selecteer de **werkruimte** > **maken** > **Notebook**.  

2. In de **notitieblok maken** dialoogvenster vak, typ een beschrijvende naam, kiest u **Scala** als de taal, in de vervolgkeuzelijst selecteert u het cluster die u eerder hebt gemaakt en selecteer **maken**.  

3. Het eindpunt, de hoofdsleutel, de database en verzameling waarden verbinding maken met de te lezen en schrijven van twitter-gegevens databaseverzameling bijwerken.

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
4. Feed als een stroom met behulp van de opdracht spark.readStream.format() start lezen-wijzigen:
 
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
|Upsert   |  Een Booleaanse waarde tekenreeks die aangeeft of upsertDocument moet worden gebruikt in plaats van CreateDocument bij het schrijven naar CosmosDB collection.   |
|checkpointlocation  |   Het pad naar lokale bestandsopslag om vast te leggen voortzetting van tokens in het geval van storingen in knooppunten.   |
|WritingBatchSize  |  Geeft de batchgrootte te gebruiken bij het schrijven van gegevens naar Azure Cosmos DB-verzameling. De connector verzendt aanvragen createDocument/upsertDocument asynchroon in batch. Hoe groter de batch het formaat van de meer doorvoer die we kunnen maar liefst, zolang de clusterbronnen beschikbaar zijn. Aan de andere kant, Geef een kleiner aantal batchgrootte om te beperken van de snelheid en de RU-verbruik. Standaard is schrijven batchgrootte ingesteld op 500.  |


## <a name="considerations-when-using-java-sdk"></a>Overwegingen bij het gebruik van Java-SDK

Maken van verbinding spark op Azure Cosmos DB met behulp van Java-SDK wordt aanbevolen in de volgende scenario's:

* Wilt u Python en/of een Scala.  

* U hebt een grote hoeveelheid gegevens over te dragen tussen Apache Spark en Azure Cosmos DB, de Java-SDK biedt betere prestaties in vergelijking met de pyDocumentDB. Als u wilt geven u een idee over het verschil van de prestaties van query's, Zie de [Query testuitvoeringen wiki](https://github.com/Azure/azure-cosmosdb-spark/wiki/Query-Test-Runs).

## <a name="next-steps"></a>Volgende stappen

Als u niet hebt gedaan, downloadt u de Spark op Azure Cosmos DB-connector van de [azure-cosmosdb-spark](https://github.com/Azure/azure-cosmosdb-spark) GitHub-opslagplaats en de extra resources in de opslagplaats verkennen:

* [Gedistribueerde aggregaties voorbeelden](https://github.com/Azure/azure-cosmosdb-spark/wiki/Aggregations-Examples)
* [Voorbeeldscripts en -laptops](https://github.com/Azure/azure-cosmosdb-spark/tree/master/samples)

U kunt ook om te controleren de [Apache Spark SQL en DataFrames gegevenssets handleiding](http://spark.apache.org/docs/latest/sql-programming-guide.html) en de [Apache Spark in Azure HDInsight](../hdinsight/spark/apache-spark-jupyter-spark-sql.md) artikel.
