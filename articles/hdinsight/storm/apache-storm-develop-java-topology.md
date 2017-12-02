---
title: Apache Storm voorbeeld Java-topologie - Azure HDInsight | Microsoft Docs
description: "Informatie over het maken van Apache Storm-topologieën in Java met het maken van een voorbeeld van de word-count-topologie."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: Apache storm, apache storm bijvoorbeeld storm java, storm-topologie-voorbeeld
ms.assetid: a8838f29-9c08-4fd9-99ef-26655d1bf6d7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: ca566aed706d4598c6067d42bdbec08d16dc3841
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a>Maken van een Apache Storm-topologie in Java

Informatie over het maken van een op Java gebaseerde topologie voor Apache Storm. U maakt een Storm-topologie die een word-count-toepassing implementeert. U Maven gebruiken om te bouwen en het project van het pakket. Vervolgens leert u hoe de topologie met behulp van het framework lichtstroom definiëren.

> [!NOTE]
> Het framework lichtstroom is beschikbaar in Storm 0.10.0 of hoger. Storm 0.10.0 is beschikbaar met HDInsight 3.3 en 3.4.

Na het voltooien van de stappen in dit document, kunt u de topologie implementeren voor Apache Storm op HDInsight.

> [!NOTE]
> Een voltooide versie van de Storm-topologie-voorbeelden in dit document gemaakt is beschikbaar op [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).

## <a name="prerequisites"></a>Vereisten

* [Java Developer Kit (JDK) versie 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

* [Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven is een project build-systeem voor Java-projecten.

* Een teksteditor of IDE.

## <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

De volgende omgevingsvariabelen kunnen worden ingesteld wanneer u Java en de JDK installeert. Echter, moet u controleren of ze bestaan en dat ze de juiste waarden voor uw systeem bevatten.

* **JAVA_HOME** -moet verwijzen naar de map waar Java runtime environment (JRE) is geïnstalleerd. Bijvoorbeeld in een Unix- of Linux-distributie, heeft een waarde gelijkaardig aan `/usr/lib/jvm/java-8-oracle`. In Windows, zou er een waarde gelijkaardig aan`c:\Program Files (x86)\Java\jre1.8`

* **PAD** -mag de volgende paden:

  * **JAVA_HOME** (of equivalente paden)

  * **JAVA_HOME\bin** (of equivalente paden)

  * De map waar Maven is geïnstalleerd

## <a name="create-a-maven-project"></a>Een Maven-project maken

De volgende opdracht vanaf de opdrachtregel gebruiken voor het maken van een Maven-project met de naam **WordCount**:

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> Als u met behulp van PowerShell, moet u tussen de`-D` parameters met dubbele aanhalingstekens.
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

Deze opdracht maakt u een map met de naam `WordCount` op de huidige locatie, die een basic Maven-project bevat. De `WordCount` map bevat de volgende items:

* `pom.xml`: Bevat instellingen voor het Maven-project.
* `src\main\java\com\microsoft\example`: Bevat de toepassingscode van uw.
* `src\test\java\com\microsoft\example`: Bevat de tests voor uw toepassing. 

### <a name="remove-the-generated-example-code"></a>Verwijderen van de gegenereerde voorbeeldcode

De gegenereerde test en bestanden voor de toepassing verwijderen:

* **src\test\java\com\microsoft\example\AppTest.Java**
* **src\main\java\com\microsoft\example\App.Java**

## <a name="add-maven-repositories"></a>Maven-opslagplaatsen toevoegen

HDInsight gebaseerd op de Hortonworks Data Platform HDP (), dus wordt u de opslagplaats Hortonworks aangeraden voor het downloaden van afhankelijkheden voor uw Apache Storm-projecten. In de __pom.xml__ -bestand, het volgende XML-bestand toevoegen nadat de `<url>http://maven.apache.org</url>` regel:

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>http://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>http://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a>Eigenschappen toevoegen

Maven kunt u definiëren op projectniveau waarden ' Eigenschappen ' genoemd. In de __pom.xml__, voeg de volgende tekst na de `</repositories>` regel:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from the Hortonworks repository that is compatible with HDInsight 3.6.
    -->
    <storm.version>1.1.0.2.6.1.9-1</storm.version>
</properties>
```

U kunt deze waarde nu gebruiken in andere gedeelten van de `pom.xml`. Bijvoorbeeld, wanneer u de versie van Storm-onderdelen opgeeft, kunt u `${storm.version}` in plaats van een vaste waarde coderen.

## <a name="add-dependencies"></a>Afhankelijkheden toevoegen

Een afhankelijkheid voor Storm-onderdelen toevoegen. Open de `pom.xml` -bestand en voeg de volgende code in de `<dependencies>` sectie:

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of the jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

Tijdens het compileren Maven gebruikt deze informatie om te zoeken `storm-core` in de opslagplaats met Maven. Eerst wordt gezocht in de opslagplaats op uw lokale computer. Als de bestanden niet bevat, wordt Maven downloadt deze vanuit de openbare opslagplaats met Maven en slaat ze op in de lokale opslagplaats.

> [!NOTE]
> U ziet de `<scope>provided</scope>` regel in deze sectie. Deze instelling geeft Maven uitsluiten **storm-core** van JAR-bestanden die zijn gemaakt, omdat dit wordt geleverd door het systeem.

## <a name="build-configuration"></a>Configuratie maken

Maven-invoegtoepassingen kunt u voor het aanpassen van de build fasen van het project. Bijvoorbeeld, hoe het project is gecompileerd of hoe u het pakket naar een JAR-bestand. Open de `pom.xml` -bestand en voeg de volgende code direct boven de `</project>` regel.

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

Deze sectie wordt gebruikt voor invoegtoepassingen, bronnen en andere configuratieopties build toevoegen. Voor een volledige beschrijving van de **pom.xml** bestand, Zie [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).

### <a name="add-plug-ins"></a>Invoegtoepassingen toevoegen

Voor Apache Storm-topologieën die zijn geïmplementeerd in Java, de [Exec Maven-invoegtoepassing](http://www.mojohaus.org/exec-maven-plugin/) zijn nuttig omdat Hiermee kunt u eenvoudig de topologie lokaal in uw ontwikkelingsomgeving uitvoeren. Het volgende toevoegen aan de `<plugins>` sectie van de `pom.xml` dat moet worden opgenomen de Exec Maven-invoegtoepassing:

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.5.0</version>
    <executions>
        <execution>
        <goals>
            <goal>exec</goal>
        </goals>
        </execution>
    </executions>
    <configuration>
        <executable>java</executable>
        <includeProjectDependencies>true</includeProjectDependencies>
        <includePluginDependencies>false</includePluginDependencies>
        <classpathScope>compile</classpathScope>
        <mainClass>${storm.topology}</mainClass>
        <cleanupDaemonThreads>false</cleanupDaemonThreads> 
    </configuration>
</plugin>
```

Andere nuttige invoegtoepassing is de [Apache Maven Compiler invoegtoepassing](http://maven.apache.org/plugins/maven-compiler-plugin/), dat wordt gebruikt voor compilatie-opties wijzigen. De wijzigingen de Java-versie die u Maven voor de bron en doel voor uw toepassing gebruikt.

* Voor HDInsight __3,4 of eerder__, instellen van de bron en doel van Java-versie naar __1.7__.

* Voor HDInsight __3.5__, instellen van de bron en doel van Java-versie naar __1.8__.

Voeg de volgende tekst in de `<plugins>` sectie van de `pom.xml` dat moet worden opgenomen van de Apache Maven Compiler-invoegtoepassing. In dit voorbeeld bevat 1.8, zodat de doel-HDInsight-versie 3.5 is.

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

### <a name="configure-resources"></a>Resources configureren

De sectie resources kunt u om op te nemen met niet-gecodeerde resources, zoals configuratiebestanden die nodig is voor de onderdelen in de topologie. Voeg de volgende tekst in voor dit voorbeeld de `<resources>` sectie van de ' bestand pom.xml.

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

In dit voorbeeld wordt de map bronnen in de hoofdmap van het project (`${basedir}`) als een locatie die resources bevat, en het bestand met de naam `log4j2.xml`. Dit bestand wordt gebruikt om te configureren welke informatie wordt vastgelegd door de topologie.

## <a name="create-the-topology"></a>De topologie maken

Een Java gebaseerde Apache Storm-topologie bestaat uit drie onderdelen die u moet schrijven (of een verwijzing) als een afhankelijkheid.

* **Spouts**: leest gegevens uit externe gegevensbronnen en verzendt gegevensstromen in de topologie.

* **Bolts**: voert verwerkingstaken op streams verzonden door spouts of andere bolts en verzendt een of meer stromen.

* **Topologie**: definieert hoe de spouts bolts gerangschikte en biedt het toegangspunt voor de topologie.

### <a name="create-the-spout"></a>De spout maken

Als u de vereisten voor het instellen van externe gegevensbronnen, verzendt de volgende spout gewoon willekeurig zinnen. Er is een gewijzigde versie van een spout die beschikbaar is in de [Storm Starter-voorbeelden](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).

> [!NOTE]
> Zie voor een voorbeeld van een spout die uit een externe gegevensbron kan lezen, een van de volgende voorbeelden:
>
> * [TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): een voorbeeld spout die uit Twitter lezen
> * [Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): een spout die uit Kafka lezen

Maak een bestand met de naam voor de spout `RandomSentenceSpout.java` in de `src\main\java\com\microsoft\example` directory en gebruik de volgende Java-code als de inhoud:

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used to emit output
  SpoutOutputCollector _collector;
  //Used to generate a random number
  Random _rand;

  //Open is called when an instance of the class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set the instance collector to the one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data to the stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //The sentences that are randomly emitted
    String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
        "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit the sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare the output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> Hoewel deze topologie wordt slechts één spout gebruikt, kunnen anderen er meerdere die gegevens uit verschillende bronnen naar de topologie.

### <a name="create-the-bolts"></a>De bolts maken

Bolts verwerkt de verwerking van gegevens. Deze topologie maakt gebruik van twee bolts:

* **SplitSentence**: splitst de zinnen gegenereerd door **RandomSentenceSpout** in afzonderlijke woorden.

* **WordCount**: telt het aantal keren dat elk woord is opgetreden.

> [!NOTE]
> Bolts kunnen doen alles zijn, bijvoorbeeld berekening, persistentie of praten met externe onderdelen.

Twee nieuwe bestanden maken `SplitSentence.java` en `WordCount.java` in de `src\main\java\com\microsoft\example` directory. Gebruik de volgende tekst als de inhoud voor de bestanden:

#### <a name="splitsentence"></a>SplitSentence

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get the sentence content from the tuple
    String sentence = tuple.getString(0);
    //An iterator to get each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give the iterator the sentence
    boundary.setText(sentence);
    //Find the beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it to the output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get the word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a>WordCount

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often to emit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default to 60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used to trigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get the word contents from the tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment the count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-the-topology"></a>De topologie definiëren

De topologie bindt de spouts en bolts samen in een grafiek, waarmee wordt gedefinieerd hoe gegevens worden uitgewisseld tussen de onderdelen. Het bevat ook parallelle uitvoering hints die gebruikmaakt van Storm bij het maken van exemplaren van de onderdelen binnen het cluster.

De volgende afbeelding is een eenvoudig diagram van de grafiek van onderdelen voor deze topologie.

![diagram van de overeenkomst spouts en bolts](./media/apache-storm-develop-java-topology/wordcount-topology.png)

Maak een bestand met de naam voor het implementeren van de topologie `WordCountTopology.java` in de `src\main\java\com\microsoft\example` directory. Gebruik de volgende Java-code als de inhoud van het bestand:

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for the topology
  public static void main(String[] args) throws Exception {
  //Used to build the topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add the spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add the SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes to the spout, and equally distributes
    //tuples (sentences) across instances of the SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add the counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes to the split bolt, and
    //ensures that the same word is sent to the same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set to false to disable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint to set the number of workers
      conf.setNumWorkers(3);
      //submit the topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap the maximum number of executors that can be spawned
      //for a component to 3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used to run locally
      LocalCluster cluster = new LocalCluster();
      //submit the topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down the cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a>Logboekregistratie configureren

Storm gebruikt Apache Log4j om informatie in een logboek te registreren. Als u logboekregistratie niet configureert, verzendt de topologie diagnostische gegevens. Maak een bestand met de naam om te bepalen wat wordt vastgelegd, `log4j2.xml` in de `resources` directory. Gebruik de volgende XML-code als de inhoud van het bestand.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

Deze XML configureert u een nieuwe logboek voor de `com.microsoft.example` klasse de onderdelen die in dit voorbeeldtopologie bevat. Het niveau is ingesteld op tracering voor dit logboek bevat informatie die door onderdelen in deze topologie logboekregistratie.

De `<Root level="error">` sectie configureert u het hoogste niveau van logboekregistratie (alles wat u niet in `com.microsoft.example`) aan te melden alleen informatie over de fout.

Zie voor meer informatie over het configureren van logboekregistratie voor Log4j [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).

> [!NOTE]
> Storm-versie 0.10.0 en hoger gebruiken Log4j 2.x. Oudere versies van storm gebruikt Log4j 1.x, die een andere indeling voor logboekbestanden-configuratie gebruikt. Zie voor informatie over de configuratie van de oudere, [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).

## <a name="test-the-topology-locally"></a>De topologie lokaal testen

Nadat u de bestanden hebt opgeslagen, moet u de volgende opdracht gebruiken voor het testen van de topologie lokaal.

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

Als dit wordt uitgevoerd, wordt opstartgegevens in de topologie weergegeven. De volgende tekst is een voorbeeld van uitvoer van de word-aantal:

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

Dit voorbeeld logboek geeft aan dat het woord 'en' heeft 113 keer is verzonden. Het aantal nog steeds zolang de topologie wordt uitgevoerd, omdat de spout continu dezelfde zinnen verzendt.

Er is een interval van 5 seconden tussen uitstoot van woorden en aantallen. De **WordCount** onderdeel is geconfigureerd voor alleen informatie verzenden wanneer een tick tuple binnenkomt. Deze aanvragen die tick tuples worden alleen gedownload voor elke vijf seconden.

## <a name="convert-the-topology-to-flux"></a>De topologie naar lichtstroom converteren

Lichtstroom is een nieuw framework beschikbaar met Storm 0.10.0 en hoger, zodat u kunt de configuratie van de toepassing te scheiden. Onderdelen van uw nog steeds in Java zijn gedefinieerd, maar de topologie is gedefinieerd met een YAML-bestand. U kunt de definitie van een standaard-topologie van het pakket met uw project, of een zelfstandig bestand gebruiken bij het indienen van de topologie. Bij het indienen van de Storm-topologie, kunt u omgevingsvariabelen of configuratiebestanden kunt gebruiken voor het vullen van waarden in de definitie van de YAML-topologie.

Het bestand YAML definieert de onderdelen moet worden gebruikt voor de topologie en de gegevens stroom ertussen. U kunt een bestand YAML opnemen als onderdeel van de jar-bestand of u kunt een bestand met externe YAML.

Zie voor meer informatie over lichtstroom [lichtstroom framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

> [!WARNING]
> Vanwege een [bug (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) met Storm 1.0.1, moet u mogelijk installeren een [Storm-ontwikkelomgeving](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) lichtstroom topologieën lokaal uitvoeren.

1. Verplaats de `WordCountTopology.java` bestand buiten het project. Voorheen dit bestand de topologie gedefinieerd, maar met lichtstroom is niet nodig.

2. In de `resources` directory, maakt u een bestand met de naam `topology.yaml`. Gebruik de volgende tekst als de inhoud van dit bestand.

    ```yaml
    name: "wordcount"       # friendly name for the topology

    config:                 # Topology configuration
      topology.workers: 1     # Hint for the number of workers to create

    spouts:                 # Spout definitions
    - id: "sentence-spout"
      className: "com.microsoft.example.RandomSentenceSpout"
      parallelism: 1      # parallelism hint

    bolts:                  # Bolt definitions
    - id: "splitter-bolt"
      className: "com.microsoft.example.SplitSentence"
      parallelism: 1
        
    - id: "counter-bolt"
      className: "com.microsoft.example.WordCount"
      constructorArgs:
        - 10
      parallelism: 1

    streams:                # Stream definitions
    - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
      from: "sentence-spout"       # The stream emitter
      to: "splitter-bolt"          # The stream consumer
      grouping:                    # Grouping type
        type: SHUFFLE
    
    - name: "Splitter -> Counter"
      from: "splitter-bolt"
      to: "counter-bolt"
      grouping:
        type: FIELDS
        args: ["word"]           # field(s) to group on
    ```

3. Breng de volgende wijzigingen aan de `pom.xml` bestand.
   
   * Voeg de volgende nieuwe afhankelijkheden in de `<dependencies>` sectie:
     
        ```xml
        <!-- Add a dependency on the Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * Voeg de volgende-invoegtoepassing voor de `<plugins>` sectie. Deze invoegtoepassing verwerkt het maken van een pakket (jar-bestand) voor het project en enkele specifieke transformaties geldt voor lichtstroom bij het maken van het pakket.
     
        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer to it as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
                    </transformer>
                </transformers>
                <!-- Keep us from getting a bad signature error -->
                <filters>
                    <filter>
                        <artifact>*:*</artifact>
                        <excludes>
                            <exclude>META-INF/*.SF</exclude>
                            <exclude>META-INF/*.DSA</exclude>
                            <exclude>META-INF/*.RSA</exclude>
                        </excludes>
                    </filter>
                </filters>
            </configuration>
            <executions>
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
        ```

   * In de **exec-maven-invoegtoepassing** `<configuration>` sectie, wijzigt u de waarde voor `<mainClass>` naar `org.apache.storm.flux.Flux`. Deze instelling kan lichtstroom voor het afhandelen van de topologie lokaal wordt uitgevoerd in ontwikkeling.

   * In de `<resources>` sectie, het volgende toevoegen aan de `<includes>`. Deze XML bevat de YAML-bestand waarin de topologie worden gedefinieerd als onderdeel van het project.

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-the-flux-topology-locally"></a>De topologie lichtstroom lokaal testen

1. Gebruik de volgende compileren en de lichtstroom-topologie met behulp van Maven uitvoeren:

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    Als u met behulp van PowerShell, gebruikt u de volgende opdracht:

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > Als uw Storm 1.0.1 bits wordt gebruikt, wordt deze opdracht mislukt. Deze fout wordt veroorzaakt door [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055). In plaats daarvan [Storm installeren in uw ontwikkelomgeving](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) en gebruik de volgende stappen:
    >
    > Als u hebt [Storm geïnstalleerd in uw ontwikkelomgeving](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), kunt u de volgende opdrachten in plaats daarvan:
    >
    > ```bash
    > mvn compile package
    > storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    > ```

    De `--local` parameter wordt de topologie in de lokale modus uitgevoerd op uw ontwikkelomgeving. De `-R /topology.yaml` parameter gebruikt de `topology.yaml` resource bestand van de jar-bestand voor het definiëren van de topologie.

    Als dit wordt uitgevoerd, wordt opstartgegevens in de topologie weergegeven. De volgende tekst is een voorbeeld van de uitvoer:

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    Er is een vertraging van 10 seconden tussen batches van geregistreerde gegevens.

2. Maak een kopie van de `topology.yaml` bestand van het project. Naam van het nieuwe bestand `newtopology.yaml`. In de `newtopology.yaml` bestand, zoek de volgende sectie en wijzig de waarde van `10` naar `5`. Deze wijziging verandert het interval tussen het importeerbereik batches van het aantal woorden van 10 seconden tot 5.

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. To run the topology, use the following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    Of als u Storm op uw ontwikkelingsomgeving hebt:

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    Wijzig de `/path/to/newtopology.yaml` naar het pad naar het newtopology.yaml-bestand dat u in de vorige stap hebt gemaakt. Met deze opdracht maakt gebruik van de newtopology.yaml als de definitie van de topologie. Omdat we niet opnemen de `compile` parameter Maven gebruikmaakt van de versie van het project is ingebouwd in de vorige stappen.

    Zodra de topologie start, moet u merkt dat de tijd tussen verzonden batches is gewijzigd om de waarde in newtopology.yaml weer te geven. Zo kunt u zien dat u uw configuratie via een YAML-bestand wijzigen kunt zonder opnieuw te compileren van de topologie.

Zie voor meer informatie over deze en andere functies van het framework lichtstroom [lichtstroom (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).

## <a name="trident"></a>Trident

Trident is een abstractie op hoog niveau die wordt geleverd door Storm. Biedt ondersteuning voor stateful verwerking. Het belangrijkste voordeel van Trident is dat garanderen kunnen dat elk bericht dat de topologie opgeeft slechts één keer wordt verwerkt. Trident gebruikt, kan uw topologie alleen garanderen dat berichten ten minste één keer worden verwerkt. Er zijn ook andere verschillen, zoals de ingebouwde componenten die kunnen worden gebruikt in plaats van bolts maken. Bolts zijn in feite is vervangen door minder-algemene onderdelen, zoals filters, projecties en functies.

Trident toepassingen kunnen worden gemaakt met behulp van Maven-projecten. U dezelfde basisstappen gebruiken, zoals eerder in dit artikel wordt beschreven, alleen de code verschilt. Trident kan niet ook (momenteel) worden gebruikt met lichtstroom framework.

Zie voor meer informatie over Trident de [Trident-API-overzicht](http://storm.apache.org/documentation/Trident-API-Overview.html).

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u maakt een Storm-topologie met Java. Nu informatie over hoe:

* [Implementeren en beheren van Apache Storm-topologieën op HDInsight](apache-storm-deploy-monitor-topology.md)

* [C#-topologieën ontwikkelen voor Apache Storm op HDInsight met behulp van Visual Studio](apache-storm-develop-csharp-visual-studio-topology.md)

U vindt meer voorbeeld Storm-topologieën in via [voorbeeldtopologieën van Storm op HDInsight](apache-storm-example-topology.md).

