---
title: 'Azure Cosmos DB: Grafiek analyses uitvoeren met behulp van Spark en Apache TinkerPop Gremlin | Microsoft Docs'
description: Dit artikel bevat instructies voor het instellen en uitvoeren van grafiek analytics en parallelle berekeningen in Azure Cosmos DB met Spark en TinkerPop SparkGraphComputer.
services: cosmosdb
documentationcenter: 
author: khdang
manager: shireest
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: gremlin
ms.topic: article
ms.date: 09/08/2017
ms.author: khdang
ms.openlocfilehash: d2ea692c5c353586cc2b653e57eca7ccb8c9c7ce
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-cosmos-db-perform-graph-analytics-by-using-spark-and-apache-tinkerpop-gremlin"></a>Azure Cosmos DB: Grafiek analyses uitvoeren met behulp van Spark en Apache TinkerPop Gremlin

[Azure Cosmos DB](introduction.md) is de globaal gedistribueerd en modellen database-service van Microsoft. U kunt maken en query document, de sleutel/waarde en de grafiek databases, die allemaal van de mogelijkheden voor algemene distributie en horizontale schaal de kern van Azure Cosmos DB profiteren. Azure Cosmos DB ondersteunt online transactieverwerking (OLTP) grafiek werkbelastingen die gebruikmaken van [Apache TinkerPop Gremlin](graph-introduction.md).

[Spark](http://spark.apache.org/) is een Apache Software Foundation-project dat gericht op het verwerken van gegevens voor algemene doeleinden online analytical processing (OLAP). Spark biedt een hybride in-memory/op basis van schijven gedistribueerde computermodel die vergelijkbaar is met het Hadoop MapReduce-model. U kunt een Apache Spark in de cloud implementeren met behulp van [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-spark/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

U kunt door een combinatie van Azure DB die Cosmos en Spark OLTP- en OLAP-werkbelastingen uitvoeren wanneer u Gremlin gebruikt. In dit artikel snel starten laat zien hoe Gremlin query's uitvoeren op Azure Cosmos DB op een Azure HDInsight Spark-cluster.

## <a name="prerequisites"></a>Vereisten

Voordat u met dit voorbeeld aan de slag gaat, moet u aan de volgende vereisten voldoen:
* Azure HDInsight Spark-cluster 2.0
* JDK 1.8 + (als u geen JDK, voert u `apt-get install default-jdk`.)
* Maven (als u geen Maven, voert u `apt-get install maven`.)
* Een Azure-abonnement ([!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)])

Zie voor meer informatie over het instellen van een Azure HDInsight Spark-cluster [inrichten van HDInsight-clusters](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

## <a name="create-an-azure-cosmos-db-database-account"></a>Een Azure DB die Cosmos-databaseaccount maken

Maak eerst een databaseaccount met de Graph API als volgt:

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-collection"></a>Een verzameling toevoegen

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-graph.md)]

## <a name="get-apache-tinkerpop"></a>Apache TinkerPop ophalen

Apache TinkerPop ophalen door het volgende te doen:

1. Extern naar het hoofdknooppunt van het HDInsight-cluster `ssh tinkerpop3-cosmosdb-demo-ssh.azurehdinsight.net`.

2. De broncode TinkerPop3 klonen, lokaal bouwen en installeer deze met Maven-cache.

    ```bash
    git clone https://github.com/apache/tinkerpop.git
    cd tinkerpop
    mvn clean install
    ```

3. De Spark-Gremlin invoegtoepassing installeren 

    a. De installatie van de invoegtoepassing wordt verwerkt door gedeeltelijk. Vul de informatie opslagplaatsen voor gedeeltelijk zodat het kan worden gedownload de invoegtoepassing en de bijbehorende afhankelijkheden. 

      Het druivensap configuratiebestand maken als het is niet aanwezig zijn bij `~/.groovy/grapeConfig.xml`. Gebruik de volgende instellingen:

    ```xml
    <ivysettings>
    <settings defaultResolver="downloadGrapes"/>
    <resolvers>
        <chain name="downloadGrapes">
        <filesystem name="cachedGrapes">
            <ivy pattern="${user.home}/.groovy/grapes/[organisation]/[module]/ivy-[revision].xml"/>
            <artifact pattern="${user.home}/.groovy/grapes/[organisation]/[module]/[type]s/[artifact]-[revision].[ext]"/>
        </filesystem>
        <ibiblio name="codehaus" root="http://repository.codehaus.org/" m2compatible="true"/>
        <ibiblio name="central" root="http://central.maven.org/maven2/" m2compatible="true"/>
        <ibiblio name="jitpack" root="https://jitpack.io" m2compatible="true"/>
        <ibiblio name="java.net2" root="http://download.java.net/maven/2/" m2compatible="true"/>
        <ibiblio name="apache-snapshots" root="http://repository.apache.org/snapshots/" m2compatible="true"/>
        <ibiblio name="local" root="file:${user.home}/.m2/repository/" m2compatible="true"/>
        </chain>
    </resolvers>
    </ivysettings>
    ``` 

    b. Start de console Gremlin `bin/gremlin.sh`.
        
    c. De Spark-Gremlin invoegtoepassing installeren met versie 3.3.0-SNAPSHOT die u in de vorige stappen hebt gebouwd:

    ```bash
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :install org.apache.tinkerpop spark-gremlin 3.3.0-SNAPSHOT
    ==>loaded: [org.apache.tinkerpop, spark-gremlin, 3.3.0-SNAPSHOT] - restart the console to use [tinkerpop.spark]
    gremlin> :q
    $ bin/gremlin.sh

            \,,,/
            (o o)
    -----oOOo-(3)-oOOo-----
    plugin activated: tinkerpop.server
    plugin activated: tinkerpop.utilities
    plugin activated: tinkerpop.tinkergraph
    gremlin> :plugin use tinkerpop.spark
    ==>tinkerpop.spark activated
    ```

4. Controleer of `Hadoop-Gremlin` is geactiveerd met een `:plugin list`. Schakel deze invoegtoepassing zijn, omdat dit ertoe dat de invoegtoepassing Spark-Gremlin leiden kan `:plugin unuse tinkerpop.hadoop`.

## <a name="prepare-tinkerpop3-dependencies"></a>TinkerPop3 afhankelijkheden voorbereiden

Wanneer u TinkerPop3 in de vorige stap hebt gemaakt, het proces alle afhankelijkheden van de jar ook voor Spark en Hadoop in de doelmap opgehaald. Gebruik de potten die vooraf zijn geïnstalleerd met HDI en ophalen van extra afhankelijkheden alleen indien nodig.

1. Ga naar de doelmap Gremlin Console op `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone`. 

2. Verplaats alle potten onder `ext/` naar `lib/`: `find ext/ -name '*.jar' -exec mv {} lib/ \;`.

3. Verwijder alle bibliotheken onder jar `lib/` die niet zijn opgenomen in de volgende lijst:

    ```bash
    # TinkerPop3
    gremlin-console-3.3.0-SNAPSHOT.jar
    gremlin-core-3.3.0-SNAPSHOT.jar       
    gremlin-groovy-3.3.0-SNAPSHOT.jar     
    gremlin-shaded-3.3.0-SNAPSHOT.jar     
    hadoop-gremlin-3.3.0-SNAPSHOT.jar     
    spark-gremlin-3.3.0-SNAPSHOT.jar      
    tinkergraph-gremlin-3.3.0-SNAPSHOT.jar

    # Gremlin depedencies
    asm-3.2.jar                                
    avro-1.7.4.jar                             
    caffeine-2.3.1.jar                         
    cglib-2.2.1-v20090111.jar                  
    gbench-0.4.3-groovy-2.4.jar                
    gprof-0.3.1-groovy-2.4.jar                 
    groovy-2.4.9-indy.jar                      
    groovy-2.4.9.jar                           
    groovy-console-2.4.9.jar                   
    groovy-groovysh-2.4.9-indy.jar             
    groovy-json-2.4.9-indy.jar                 
    groovy-jsr223-2.4.9-indy.jar               
    groovy-sql-2.4.9-indy.jar                  
    groovy-swing-2.4.9.jar                     
    groovy-templates-2.4.9.jar                 
    groovy-xml-2.4.9.jar                       
    hadoop-yarn-server-nodemanager-2.7.2.jar   
    hppc-0.7.1.jar                             
    javatuples-1.2.jar                         
    jaxb-impl-2.2.3-1.jar                      
    jbcrypt-0.4.jar                            
    jcabi-log-0.14.jar                         
    jcabi-manifests-1.1.jar                    
    jersey-core-1.9.jar                        
    jersey-guice-1.9.jar                       
    jersey-json-1.9.jar                        
    jettison-1.1.jar                           
    scalatest_2.11-2.2.6.jar                   
    servlet-api-2.5.jar                        
    snakeyaml-1.15.jar                         
    unused-1.0.0.jar                           
    xml-apis-1.3.04.jar                        
    ```

## <a name="get-the-azure-cosmos-db-spark-connector"></a>Ophalen van de Azure Cosmos DB Spark-connector

1. Ophalen van de connector Azure Cosmos DB Spark `azure-cosmosdb-spark-0.0.3-SNAPSHOT.jar` en Cosmos DB Java SDK `azure-documentdb-1.12.0.jar` van [Azure DB Spark Cosmos-Connector op GitHub](https://github.com/Azure/azure-cosmosdb-spark/tree/master/releases/azure-cosmosdb-spark-0.0.3_2.0.2_2.11).

2. U kunt ook kunt u deze lokaal. Omdat de nieuwste versie van Spark Gremlin is gebouwd met Spark 1.6.1 en niet compatibel met Spark 2.0.2, die momenteel in de Azure Cosmos DB Spark-connector wordt gebruikt is, kunt u de meest recente TinkerPop3 code bouwen en de potten handmatig installeren. Ga als volgt te werk:

    a. Kloon van de Azure Cosmos DB Spark-connector.

    b. Bouw TinkerPop3 (in de vorige stappen hebt). Installeer alle TinkerPop 3.3.0-SNAPSHOT potten lokaal.

    ```bash
    mvn install:install-file -Dfile="gremlin-core-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-core -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar
    mvn install:install-file -Dfile="gremlin-groovy-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-groovy -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="gremlin-shaded-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=gremlin-shaded -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="hadoop-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=hadoop-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="spark-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=spark-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    mvn install:install-file -Dfile="tinkergraph-gremlin-3.3.0-SNAPSHOT.jar" -DgroupId=org.apache.tinkerpop -DartifactId=tinkergraph-gremlin -Dversion=3.3.0-SNAPSHOT -Dpackaging=jar`
    ```

    c. Update `tinkerpop.version` `azure-documentdb-spark/pom.xml` naar `3.3.0-SNAPSHOT`.
    
    d. Bij het maken van Maven. De benodigde potten worden geplaatst `target` en `target/alternateLocation`.

    ```bash
    git clone https://github.com/Azure/azure-cosmosdb-spark.git
    cd azure-documentdb-spark
    mvn clean package
    ```

3. De eerder genoemde potten kopiëren naar een lokale map op ~ / azure-documentdb-spark:

    ```bash
    $ azure-documentdb-spark:
    mkdir ~/azure-documentdb-spark
    cp target/azure-documentdb-spark-0.0.3-SNAPSHOT.jar ~/azure-documentdb-spark
    cp target/alternateLocation/azure-documentdb-1.10.0.jar ~/azure-documentdb-spark
    ```

## <a name="distribute-the-dependencies-to-the-spark-worker-nodes"></a>De afhankelijkheden met de werkrolknooppunten Spark distribueren 

1. Omdat de transformatie van grafiekgegevens is afhankelijk van TinkerPop3, moet u de bijbehorende afhankelijkheden om alle Spark worker-knooppunten te distribueren.

2. Kopieer de eerder genoemde Gremlin afhankelijkheden, CosmosDB Spark connector jar en CosmosDB Java SDK met de werkrolknooppunten als volgt:

    a. Kopieer alle potten in `~/azure-documentdb-spark`.

    ```bash
    $ /home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone:
    cp lib/* ~/azure-documentdb-spark
    ```

    b. De lijst met alle Spark worker-knooppunten, kunt u vinden op Ambari-Dashboard in de `Spark2 Clients` lijst de `Spark2` sectie.

    c. Kopieer de map aan elk van de knooppunten.

    ```bash
    scp -r ~/azure-documentdb-spark sshuser@wn0-cosmos:/home/sshuser
    scp -r ~/azure-documentdb-spark sshuser@wn1-cosmos:/home/sshuser
    ...
    ```
    
## <a name="set-up-the-environment-variables"></a>De omgevingsvariabelen instellen

1. Zoek de HDP-versie van het Spark-cluster. Het is de naam van de map onder `/usr/hdp/` (bijvoorbeeld 2.5.4.2-7).

2. Stel hdp.version voor alle knooppunten. Ga in de Ambari-Dashboard naar **YARN-sectie** > **Configs** > **Geavanceerd**, en voer de volgende handelingen uit: 
 
    a. In `Custom yarn-site`, een nieuwe eigenschap toevoegen `hdp.version` met de waarde van de versie HDP op het hoofdknooppunt. 
     
    b. Sla de configuraties. Er zijn waarschuwingen die u kunt negeren. 
     
    c. De YARN en Oozie-services opnieuw starten als de meldingspictogrammen geven.

3. De volgende omgevingsvariabelen worden ingesteld op het hoofdknooppunt (Vervang de waarden waar nodig):

    ```bash
    export HADOOP_GREMLIN_LIBS=/home/sshuser/tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/ext/spark-gremlin/lib
    export CLASSPATH=$CLASSPATH:$HADOOP_CONF_DIR:/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    export HDP_VERSION=2.5.4.2-7
    export HADOOP_HOME=${HADOOP_HOME:-/usr/hdp/current/hadoop-client}
    ```

## <a name="prepare-the-graph-configuration"></a>Voorbereiden van de configuratie van de grafiek

1. Een configuratiebestand met de Azure DB die Cosmos verbindingsparameters en Spark-instellingen maken en plaatsen op `tinkerpop/gremlin-console/target/apache-tinkerpop-gremlin-console-3.3.0-SNAPSHOT-standalone/conf/hadoop/gremlin-spark.properties`.

    ```
    gremlin.graph=org.apache.tinkerpop.gremlin.hadoop.structure.HadoopGraph
    gremlin.hadoop.jarsInDistributedCache=true
    gremlin.hadoop.defaultGraphComputer=org.apache.tinkerpop.gremlin.spark.process.computer.SparkGraphComputer

    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    gremlin.hadoop.graphWriter=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBOutputRDD

    ####################################
    # SparkGraphComputer Configuration #
    ####################################
    spark.master=yarn
    spark.executor.memory=3g
    spark.executor.instances=6
    spark.serializer=org.apache.spark.serializer.KryoSerializer
    spark.kryo.registrator=org.apache.tinkerpop.gremlin.spark.structure.io.gryo.GryoRegistrator
    gremlin.spark.persistContext=true

    # Classpath for the driver and executors
    spark.driver.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    spark.executor.extraClassPath=/usr/hdp/current/spark2-client/jars/*:/home/sshuser/azure-documentdb-spark/*
    
    ######################################
    # DocumentDB Spark connector         #
    ######################################
    spark.documentdb.connectionMode=Gateway
    spark.documentdb.schema_samplingratio=1.0
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    spark.documentdb.preferredRegions=FILLIN
    ```

2. Update de `spark.driver.extraClassPath` en `spark.executor.extraClassPath` om op te nemen van de map van de potten die u in de vorige stap hebt gedistribueerd, in dit geval `/home/sshuser/azure-documentdb-spark/*`.

3. Azure Cosmos DB bieden de volgende details:

    ```
    spark.documentdb.Endpoint=https://FILLIN.documents.azure.com:443/
    spark.documentdb.Masterkey=FILLIN
    spark.documentdb.Database=FILLIN
    spark.documentdb.Collection=FILLIN
    # Optional
    #spark.documentdb.preferredRegions=West\ US;West\ US\ 2
    ```
   
## <a name="load-the-tinkerpop-graph-and-save-it-to-azure-cosmos-db"></a>Laden van de grafiek TinkerPop en sla deze op Azure Cosmos-DB
Als u wilt laten zien hoe u een grafiek behouden in Azure Cosmos DB, in dit voorbeeld voorgedefinieerde gebruikt de TinkerPop TinkerPop moderne grafiek. De grafiek wordt opgeslagen in Kryo indeling en in de opslagplaats TinkerPop opgegeven.

1. Omdat u Gremlin in de YARN-modus uitvoert, moet u de grafiekgegevens in het Hadoop-bestandssysteem beschikbaar maken. Gebruik de volgende opdrachten om te maken van een map en kopieer het bestand lokaal grafiek naar het. 

    ```bash
    $ tinkerpop:
    hadoop fs -mkdir /graphData
    hadoop fs -copyFromLocal ~/tinkerpop/data/tinkerpop-modern.kryo /graphData/tinkerpop-modern.kryo
    ```

2. Tijdelijk bijwerken de `gremlin-spark.properties` om te gebruiken bestand `GryoInputFormat` lezen van de grafiek. Ook duiden op `inputLocation` als de map u maakt, zoals in het volgende:

    ```
    gremlin.hadoop.graphReader=org.apache.tinkerpop.gremlin.hadoop.structure.io.gryo.GryoInputFormat
    gremlin.hadoop.inputLocation=/graphData/tinkerpop-modern.kryo
    ```

3. Start Gremlin-Console en maak vervolgens de volgende berekening stappen voor het behouden van gegevens naar de geconfigureerde Azure DB die Cosmos-verzameling:  

    a. Maken van de grafiek `graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")`.

    b. SparkGraphComputer gebruiken om te schrijven `graph.compute(SparkGraphComputer.class).result(GraphComputer.ResultGraph.NEW).persist(GraphComputer.Persist.EDGES).program(TraversalVertexProgram.build().traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)),"gremlin-groovy","g.V()").create(graph)).submit().get()`.

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[gryoinputformat->documentdboutputrdd]
    gremlin> hg = graph.
                compute(SparkGraphComputer.class).
                result(GraphComputer.ResultGraph.NEW).
                persist(GraphComputer.Persist.EDGES).
                program(TraversalVertexProgram.build().
                    traversal(graph.traversal().withComputer(Computer.compute(SparkGraphComputer.class)), "gremlin-groovy", "g.V()").
                    create(graph)).
                submit().
                get() 
    ==>result[hadoopgraph[documentdbinputrdd->documentdboutputrdd],memory[size:1]]
    ```

4. U kunt controleren dat de gegevens heeft is opgeslagen in Azure Cosmos DB uit Data Explorer.

## <a name="load-the-graph-from-azure-cosmos-db-and-run-gremlin-queries"></a>De grafiek uit Azure Cosmos DB laden en Gremlin query's uitvoeren

1. Als u wilt laden van de grafiek bewerken `gremlin-spark.properties` instellen `graphReader` naar `DocumentDBInputRDD`:

    ```
    gremlin.hadoop.graphReader=com.microsoft.azure.documentdb.spark.gremlin.DocumentDBInputRDD
    ```

2. Laden van de grafiek, bladeren door de gegevens en Gremlin query's uitvoeren met dit als volgt:

    a. Start de Console Gremlin `bin/gremlin.sh`.

    b. De grafiek te maken met de configuratie `graph = GraphFactory.open('conf/hadoop/gremlin-spark.properties')`.

    c. Een grafiek traversal maken met SparkGraphComputer `g = graph.traversal().withComputer(SparkGraphComputer)`.

    d. Voer de volgende query's voor Gremlin-grafiek:

    ```bash
    gremlin> graph = GraphFactory.open("conf/hadoop/gremlin-spark.properties")
    ==>hadoopgraph[documentdbinputrdd->documentdboutputrdd]
    gremlin> g = graph.traversal().withComputer(SparkGraphComputer)
    ==>graphtraversalsource[hadoopgraph[documentdbinputrdd->documentdboutputrdd], sparkgraphcomputer]
    gremlin> g.V().count()
    ==>6
    gremlin> g.E().count()
    ==>6
    gremlin> g.V(1).out().values('name')
    ==>josh
    ==>vadas
    ==>lop
    gremlin> g.V().hasLabel('person').coalesce(values('nickname'), values('name'))
    ==>josh
    ==>peter
    ==>vadas
    ==>marko
    gremlin> g.V().hasLabel('person').
            choose(values('name')).
                option('marko', values('age')).
                option('josh', values('name')).
                option('vadas', valueMap()).
                option('peter', label())
    ==>josh
    ==>person
    ==>[name:[vadas],age:[27]]
    ==>29
    ```

> [!NOTE]
> Ga voor meer gedetailleerde logboekregistratie, stelt u het logboek niveau in `conf/log4j-console.properties` naar een uitgebreidere niveau.
>

## <a name="next-steps"></a>Volgende stappen

U kunt het werken met grafieken door een combinatie van Azure DB die Cosmos en Spark hebt geleerd in dit artikel snel starten.

> [!div class="nextstepaction"]
