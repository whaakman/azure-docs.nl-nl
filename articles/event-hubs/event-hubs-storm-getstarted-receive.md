---
title: Gebeurtenissen ontvangen van Azure Event Hubs met behulp van Apache Storm | Microsoft Docs
description: Aan de slag ontvangen van Event Hubs met behulp van Apache Storm
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: java
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2018
ms.author: shvija
ms.openlocfilehash: 3880ffe1b61b59e4d05e594a34e1119188177b56
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/09/2018
ms.locfileid: "40002760"
---
# <a name="receive-events-from-event-hubs-using-apache-storm"></a>Gebeurtenissen ontvangen van Event Hubs met behulp van Apache Storm

[Apache Storm](https://storm.incubator.apache.org) is een gedistribueerde realtime berekeningssysteem voor het vereenvoudigen van betrouwbare verwerking van niet-gebonden gegevensstromen. Deze sectie wordt beschreven hoe u een Azure Event Hubs Storm spout voor het ontvangen van gebeurtenissen van Event Hubs. Met behulp van Apache Storm, kunt u gebeurtenissen splitsen over meerdere processen die worden gehost in verschillende knooppunten. De integratie met Event Hubs met Storm verbruik van gebeurtenissen wordt vereenvoudigd door transparant plaatsen van controlepunten in de voortgang van de Storm Zookeeper-installatie gebruikmaakt, het beheren van permanente controlepunten en parallelle ontvangst van Event Hubs.

Voor meer informatie over Event Hubs ontvangt u patronen, Zie de [overzicht van Event Hubs][Event Hubs overview].

## <a name="create-project-and-add-code"></a>Project maken en code toevoegen

In deze zelfstudie wordt een [HDInsight Storm] [ HDInsight Storm] installatie, die wordt geleverd met de Event Hubs spout al beschikbaar.

1. Ga als volgt de [HDInsight Storm - aan de slag](../hdinsight/storm/apache-storm-overview.md) procedure voor het maken van een nieuw HDInsight-cluster en verbinding maken via Extern bureaublad.
2. Kopieer de `%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar` -bestand naar uw lokale ontwikkelomgeving. Deze bevat gebeurtenissen-storm-spout.
3. Gebruik de volgende opdracht voor het installeren van het pakket in het lokale archief van Maven. Hiermee kunt u wilt toevoegen als een verwijzing in de Storm-project in een latere stap.

    ```shell
    mvn install:install-file -Dfile=target\eventhubs-storm-spout-0.9-jar-with-dependencies.jar -DgroupId=com.microsoft.eventhubs -DartifactId=eventhubs-storm-spout -Dversion=0.9 -Dpackaging=jar
    ```
4. Maak een nieuw Maven-project in Eclipse (Klik op **bestand**, vervolgens **nieuw**, klikt u vervolgens **Project**).
   
    ![][12]
5. Selecteer **gebruik werkruimte standaardlocatie**, klikt u vervolgens op **volgende**
6. Selecteer de **maven-archetype-quickstart** archetype, klikt u vervolgens op **volgende**
7. Voeg een **groeps-id** en **ArtifactId**, klikt u vervolgens op **voltooien**
8. In **pom.xml**, voeg de volgende afhankelijkheden in de `<dependency>` knooppunt.

    ```xml  
    <dependency>
        <groupId>org.apache.storm</groupId>
        <artifactId>storm-core</artifactId>
        <version>0.9.2-incubating</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>com.microsoft.eventhubs</groupId>
        <artifactId>eventhubs-storm-spout</artifactId>
        <version>0.9</version>
    </dependency>
    <dependency>
        <groupId>com.netflix.curator</groupId>
        <artifactId>curator-framework</artifactId>
        <version>1.3.3</version>
        <exclusions>
            <exclusion>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
            </exclusion>
            <exclusion>
                <groupId>org.slf4j</groupId>
                <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
        </exclusions>
        <scope>provided</scope>
    </dependency>
    ```

9. In de **src** map, maakt u een bestand met de naam **Config.properties** en kopieer de volgende inhoud, waarbij de `receive rule key` en `event hub name` waarden:

    ```java
    eventhubspout.username = ReceiveRule
    eventhubspout.password = {receive rule key}
    eventhubspout.namespace = ioteventhub-ns
    eventhubspout.entitypath = {event hub name}
    eventhubspout.partitions.count = 16
       
    # if not provided, will use storm's zookeeper settings
    # zookeeper.connectionstring=localhost:2181
       
    eventhubspout.checkpoint.interval = 10
    eventhub.receiver.credits = 10
    ```
    De waarde voor **eventhub.receiver.credits** bepaalt hoeveel gebeurtenissen worden verzameld voordat ze vrijgeven aan de Storm-pijplijn. In dit voorbeeld wordt deze waarde om het eenvoudig, te ingesteld op 10. In de productieomgeving, moet deze meestal worden ingesteld op hogere waarden; bijvoorbeeld: 1024.
10. Maak een nieuwe klasse met de naam **LoggerBolt** door de volgende code:
    
    ```java
    import java.util.Map;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import backtype.storm.task.OutputCollector;
    import backtype.storm.task.TopologyContext;
    import backtype.storm.topology.OutputFieldsDeclarer;
    import backtype.storm.topology.base.BaseRichBolt;
    import backtype.storm.tuple.Tuple;
    
    public class LoggerBolt extends BaseRichBolt {
        private OutputCollector collector;
        private static final Logger logger = LoggerFactory
                  .getLogger(LoggerBolt.class);
    
        @Override
        public void execute(Tuple tuple) {
            String value = tuple.getString(0);
            logger.info("Tuple value: " + value);
   
            collector.ack(tuple);
        }
   
        @Override
        public void prepare(Map map, TopologyContext context, OutputCollector collector) {
            this.collector = collector;
            this.count = 0;
        }
        
        @Override
        public void declareOutputFields(OutputFieldsDeclarer declarer) {
            // no output fields
        }
    
    }
    ```
    
    Deze bolt Storm Hiermee wordt de inhoud van de ontvangen gebeurtenissen geregistreerd. Dit kan eenvoudig worden uitgebreid voor het opslaan van tuples in een storage-service. De [HDInsight Storm met voorbeeld van de Event Hub] maakt gebruik van deze benadering voor het opslaan van gegevens in Azure Storage en Power BI.
11. Maak een klasse met de naam **LogTopology** door de volgende code:
    
    ```java
    import java.io.FileReader;
    import java.util.Properties;
    import backtype.storm.Config;
    import backtype.storm.LocalCluster;
    import backtype.storm.StormSubmitter;
    import backtype.storm.generated.StormTopology;
    import backtype.storm.topology.TopologyBuilder;
    import com.microsoft.eventhubs.samples.EventCount;
    import com.microsoft.eventhubs.spout.EventHubSpout;
    import com.microsoft.eventhubs.spout.EventHubSpoutConfig;
        
    public class LogTopology {
        protected EventHubSpoutConfig spoutConfig;
        protected int numWorkers;
        
        protected void readEHConfig(String[] args) throws Exception {
            Properties properties = new Properties();
            if (args.length > 1) {
                properties.load(new FileReader(args[1]));
            } else {
                properties.load(EventCount.class.getClassLoader()
                        .getResourceAsStream("Config.properties"));
            }
        
            String username = properties.getProperty("eventhubspout.username");
            String password = properties.getProperty("eventhubspout.password");
            String namespaceName = properties
                    .getProperty("eventhubspout.namespace");
            String entityPath = properties.getProperty("eventhubspout.entitypath");
            String zkEndpointAddress = properties
                    .getProperty("zookeeper.connectionstring"); // opt
            int partitionCount = Integer.parseInt(properties
                    .getProperty("eventhubspout.partitions.count"));
            int checkpointIntervalInSeconds = Integer.parseInt(properties
                    .getProperty("eventhubspout.checkpoint.interval"));
            int receiverCredits = Integer.parseInt(properties
                    .getProperty("eventhub.receiver.credits")); // prefetch count
                                                                // (opt)
            System.out.println("Eventhub spout config: ");
            System.out.println("  partition count: " + partitionCount);
            System.out.println("  checkpoint interval: "
                    + checkpointIntervalInSeconds);
            System.out.println("  receiver credits: " + receiverCredits);
     
            spoutConfig = new EventHubSpoutConfig(username, password,
                    namespaceName, entityPath, partitionCount, zkEndpointAddress,
                    checkpointIntervalInSeconds, receiverCredits);
        
            // set the number of workers to be the same as partition number.
            // the idea is to have a spout and a logger bolt co-exist in one
            // worker to avoid shuffling messages across workers in storm cluster.
            numWorkers = spoutConfig.getPartitionCount();
        
            if (args.length > 0) {
                // set topology name so that sample Trident topology can use it as
                // stream name.
                spoutConfig.setTopologyName(args[0]);
            }
        }
        
        protected StormTopology buildTopology() {
            TopologyBuilder topologyBuilder = new TopologyBuilder();
       
            EventHubSpout eventHubSpout = new EventHubSpout(spoutConfig);
            topologyBuilder.setSpout("EventHubsSpout", eventHubSpout,
                    spoutConfig.getPartitionCount()).setNumTasks(
                    spoutConfig.getPartitionCount());
            topologyBuilder
                    .setBolt("LoggerBolt", new LoggerBolt(),
                            spoutConfig.getPartitionCount())
                    .localOrShuffleGrouping("EventHubsSpout")
                    .setNumTasks(spoutConfig.getPartitionCount());
            return topologyBuilder.createTopology();
        }
        
        protected void runScenario(String[] args) throws Exception {
            boolean runLocal = true;
            readEHConfig(args);
            StormTopology topology = buildTopology();
            Config config = new Config();
            config.setDebug(false);
        
            if (runLocal) {
                config.setMaxTaskParallelism(2);
                LocalCluster localCluster = new LocalCluster();
                localCluster.submitTopology("test", config, topology);
                Thread.sleep(5000000);
                localCluster.shutdown();
            } else {
                config.setNumWorkers(numWorkers);
                StormSubmitter.submitTopology(args[0], config, topology);
            }
        }
        
        public static void main(String[] args) throws Exception {
            LogTopology topology = new LogTopology();
            topology.runScenario(args);
        }
    }
    ```

    Deze klasse maakt een nieuwe Event Hubs-spout, met behulp van de eigenschappen in het configuratiebestand te maken. Het is belangrijk te weten dat dit voorbeeld maakt u net zoveel taken uit spouts als het aantal partities in de event hub, als u wilt gebruiken, het maximum aantal parallelle uitvoering toegestaan door die event hub.

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over Event Hubs vinden via de volgende koppelingen:

* [Event Hubs-overzicht][Event Hubs overview]
* [Een Event Hub maken](event-hubs-create.md)
* [Veelgestelde vragen over Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[HDInsight Storm]: ../hdinsight/storm/apache-storm-overview.md
[Storm voor HDInsight met voorbeeld van de Event Hub]: https://azure.microsoft.com/resources/samples/hdinsight-java-storm-eventhub/

<!-- Images -->

[12]: ./media/event-hubs-get-started-receive-storm/create-storm1.png
