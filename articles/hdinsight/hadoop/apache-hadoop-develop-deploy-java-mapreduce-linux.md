---
title: Java-MapReduce maken voor Hadoop - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Apache Maven een MapReduce op basis van Java-toepassing maken en vervolgens uitvoeren met Hadoop op Azure HDInsight.
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
ms.assetid: 9ee6384c-cb61-4087-8273-fb53fa27c1c3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 01/25/2018
ms.author: larryfr
ms.openlocfilehash: 16cb62c95784d7c8b284e03f0759028038af7f0a
ms.sourcegitcommit: 99d29d0aa8ec15ec96b3b057629d00c70d30cfec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight"></a>Het ontwikkelen van Java-MapReduce-programma's voor Hadoop in HDInsight

Informatie over het gebruik van Apache Maven een MapReduce op basis van Java-toepassing maken en vervolgens uitvoeren met Hadoop op Azure HDInsight.

> [!NOTE]
> In dit voorbeeld is voor het meest recent getest op HDInsight 3.6.

## <a name="prerequisites"></a>Vereisten

* [Java-JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 of hoger (of een vergelijkbare groep, zoals OpenJDK).
    
    > [!NOTE]
    > HDInsight versie 3.4 en eerder gebruik Java 7. HDInsight 3.5 en groter Java 8 gebruikt.

* [Apache Maven](http://maven.apache.org/)

## <a name="configure-development-environment"></a>De ontwikkelomgeving configureren

De volgende omgevingsvariabelen kunnen worden ingesteld wanneer u Java en de JDK installeert. Echter, moet u controleren of ze bestaan en dat ze de juiste waarden voor uw systeem bevatten.

* `JAVA_HOME`-moet verwijzen naar de map waar Java runtime environment (JRE) is geïnstalleerd. Bijvoorbeeld: op een OS X-, Unix- of Linux-systeem, heeft een waarde gelijkaardig aan `/usr/lib/jvm/java-7-oracle`. In Windows, zou er een waarde gelijkaardig aan`c:\Program Files (x86)\Java\jre1.7`

* `PATH`-moet bevatten de volgende paden:
  
  * `JAVA_HOME`(of equivalente paden)

  * `JAVA_HOME\bin`(of equivalente paden)

  * De map waar Maven is geïnstalleerd

## <a name="create-a-maven-project"></a>Een Maven-project maken

1. Wijzig de mappen naar de locatie die u wilt dit project opslaan van een terminalsessie of de opdrachtregel in uw ontwikkelomgeving.

2. Gebruik de `mvn` opdracht, die is geïnstalleerd met Maven, genereren de steiger voor het project.

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > Als u met behulp van PowerShell, moet u tussen de `-D` parameters tussen dubbele aanhalingstekens.
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Met deze opdracht maakt u een map met de naam die is opgegeven door de `artifactID` parameter (**wordcountjava** in dit voorbeeld.) Deze map bevat de volgende items:

   * `pom.xml`-De [Project Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) die gegevens en configuratie-informatie gebruikt om het project te bouwen bevat.

   * `src`-De map waarin de toepassing.

3. Verwijder de `src/test/java/org/apache/hadoop/examples/apptest.java` bestand. Het wordt niet gebruikt in dit voorbeeld.

## <a name="add-dependencies"></a>Afhankelijkheden toevoegen

1. Bewerk de `pom.xml` -bestand en voeg de volgende tekst in de `<dependencies>` sectie:
   
   ```xml
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-examples</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-client-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
   ```

    Hiermee definieert u vereiste bibliotheken (vermeld in &lt;artefact-id\>) met een specifieke versie (vermeld in &lt;versie\>). Tijdens de compilatie, worden deze afhankelijkheden gedownload vanuit de opslagplaats met Maven standaard. U kunt de [Maven-opslagplaats zoeken](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) om informatie weer te geven.
   
    De `<scope>provided</scope>` vertelt Maven dat deze afhankelijkheden niet moeten worden verpakt met de toepassing die worden geleverd door het HDInsight-cluster tijdens runtime.

    > [!IMPORTANT]
    > De versie die wordt gebruikt, moet overeenkomen met de versie van Hadoop aanwezig is op het cluster. Zie voor meer informatie over de versies, de [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md) document.

2. Het volgende toevoegen aan de `pom.xml` bestand. Deze tekst moet binnen het `<project>...</project>` labels in het bestand, bijvoorbeeld tussen `</dependencies>` en `</project>`.

   ```xml
    <build>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
            <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                </transformer>
            </transformers>
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
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.6.1</version>
            <configuration>
            <source>1.8</source>
            <target>1.8</target>
            </configuration>
        </plugin>
        </plugins>
    </build>
   ```

    De eerste invoegtoepassing configureert u de [Maven grijs-invoegtoepassing](http://maven.apache.org/plugins/maven-shade-plugin/), dat wordt gebruikt voor het bouwen van een uberjar (ook wel een fatjar genoemd), die door de toepassing vereiste afhankelijkheden bevat. Dit voorkomt ook dat licenties binnen het jar-pakket dat problemen voor sommige systemen veroorzaken kan worden gedupliceerd.

    De tweede invoegtoepassing configureert de doelversie voor Java.

    > [!NOTE]
    > HDInsight 3.4 en eerdere versies gebruiken Java 7. HDInsight 3.5 en groter Java 8 gebruikt.

3. Sla het bestand `pom.xml` op.

## <a name="create-the-mapreduce-application"></a>De MapReduce-toepassing maken

1. Ga naar de `wordcountjava/src/main/java/org/apache/hadoop/examples` map en wijzig de naam van de `App.java` van het bestand in `WordCount.java`.

2. Open de `WordCount.java` -bestand in een teksteditor en vervang de inhoud door de volgende tekst:
   
    ```java
    package org.apache.hadoop.examples;

    import java.io.IOException;
    import java.util.StringTokenizer;
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.io.IntWritable;
    import org.apache.hadoop.io.Text;
    import org.apache.hadoop.mapreduce.Job;
    import org.apache.hadoop.mapreduce.Mapper;
    import org.apache.hadoop.mapreduce.Reducer;
    import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
    import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class WordCount {

        public static class TokenizerMapper
            extends Mapper<Object, Text, Text, IntWritable>{

        private final static IntWritable one = new IntWritable(1);
        private Text word = new Text();

        public void map(Object key, Text value, Context context
                        ) throws IOException, InterruptedException {
            StringTokenizer itr = new StringTokenizer(value.toString());
            while (itr.hasMoreTokens()) {
            word.set(itr.nextToken());
            context.write(word, one);
            }
        }
    }

    public static class IntSumReducer
            extends Reducer<Text,IntWritable,Text,IntWritable> {
        private IntWritable result = new IntWritable();

        public void reduce(Text key, Iterable<IntWritable> values,
                            Context context
                            ) throws IOException, InterruptedException {
            int sum = 0;
            for (IntWritable val : values) {
            sum += val.get();
            }
            result.set(sum);
            context.write(key, result);
        }
    }

    public static void main(String[] args) throws Exception {
        Configuration conf = new Configuration();
        String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
        if (otherArgs.length != 2) {
            System.err.println("Usage: wordcount <in> <out>");
            System.exit(2);
        }
        Job job = new Job(conf, "word count");
        job.setJarByClass(WordCount.class);
        job.setMapperClass(TokenizerMapper.class);
        job.setCombinerClass(IntSumReducer.class);
        job.setReducerClass(IntSumReducer.class);
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
        FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
        System.exit(job.waitForCompletion(true) ? 0 : 1);
        }
    }
    ```
   
    Let op de naam van het pakket is `org.apache.hadoop.examples` en de klassenaam van de is `WordCount`. U kunt deze namen gebruiken wanneer u bij het verzenden van de MapReduce-taak.

3. Sla het bestand op.

## <a name="build-the-application"></a>De toepassing bouwen

1. Wijzig in de `wordcountjava` directory, als u nog geen er.

2. Gebruik de volgende opdracht voor het bouwen van een JAR-bestand met de toepassing:

   ```
   mvn clean package
   ```

    Met deze opdracht alle vorige build-artefacten schoongemaakt, downloadt u alle afhankelijkheden die nog niet hebt is geïnstalleerd, en vervolgens builds en pakket de toepassing.

3. Zodra de opdracht is voltooid, de `wordcountjava/target` map bevat een bestand met de naam `wordcountjava-1.0-SNAPSHOT.jar`.
   
   > [!NOTE]
   > De `wordcountjava-1.0-SNAPSHOT.jar` bestand is een uberjar waarin niet alleen de WordCount-taak, maar ook afhankelijkheden die de taak vereist een tijdens runtime.

## <a id="upload"></a>Uploaden van het oppervlak

Gebruik de volgende opdracht het jar-bestand uploaden naar de headnode HDInsight:

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

    Replace __USERNAME__ with your SSH user name for the cluster. Replace __CLUSTERNAME__ with the HDInsight cluster name.

Met deze opdracht kopieert de bestanden van het lokale systeem met het hoofdknooppunt. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a name="run"></a>Hadoop de MapReduce-taak wordt uitgevoerd

1. Verbinding maken met HDInsight met behulp van SSH. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Gebruik de volgende opdracht de MapReduce-toepassing uit te voeren van de SSH-sessie:
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    Deze opdracht start de toepassing WordCount MapReduce. Het invoerbestand is `/example/data/gutenberg/davinci.txt`, en de uitvoermap is `/example/data/wordcountout`. Het bestand voor invoer- en uitvoer worden naar de opslag van de standaard voor het cluster opgeslagen.

3. Zodra de taak is voltooid, gebruikt u de volgende opdracht om de resultaten weer te geven:
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    U ontvangt een lijst met woorden en aantallen met waarden die vergelijkbaar is met de volgende tekst:
   
        zeal    1
        zelus   1
        zenith  2

## <a id="nextsteps"></a>Volgende stappen

In dit document hebt u geleerd hoe een Java-MapReduce-taak ontwikkelen. Zie de volgende documenten voor andere manieren om te werken met HDInsight.

* [Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

Zie voor meer informatie, ook de [Java Developer Center](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]:hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]:apache-hadoop-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-power-query]:apache-hadoop-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

