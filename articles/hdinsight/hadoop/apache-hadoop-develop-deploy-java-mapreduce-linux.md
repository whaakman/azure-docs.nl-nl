---
title: Java MapReduce voor Apache Hadoop - Azure HDInsight maken
description: Informatie over het gebruik van Apache Maven een MapReduce op basis van een Java-toepassing maken en vervolgens uitvoeren met Hadoop op Azure HDInsight.
services: hdinsight
ms.reviewer: jasonh
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: e1b4681bc8e42294be6b47a04a24e28f81703e6f
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2018
ms.locfileid: "53436854"
---
# <a name="develop-java-mapreduce-programs-for-apache-hadoop-on-hdinsight"></a>Java MapReduce-programma's ontwikkelen voor Apache Hadoop op HDInsight

Informatie over het gebruik van Apache Maven een MapReduce op basis van een Java-toepassing maken en vervolgens uitvoeren met Apache Hadoop op Azure HDInsight.

> [!NOTE]
> In dit voorbeeld is onlangs getest in HDInsight 3.6.

## <a name="prerequisites"></a>Vereisten

* [Java JDK](https://www.oracle.com/technetwork/java/javase/downloads/) 8 of hoger (of een equivalent, zoals OpenJDK).
    
    > [!NOTE]
    > HDInsight versie 3.4 en eerder Java 7 gebruiken. Java 8 maakt gebruik van HDInsight 3.5 en hoger.

* [Apache Maven](https://maven.apache.org/)

## <a name="configure-development-environment"></a>De ontwikkelomgeving configureren

De volgende omgevingsvariabelen kunnen worden ingesteld wanneer u Java en de JDK installeert. U moet dan echter wel controleren of ze bestaan en of ze de juiste waarden voor uw systeem bevatten.

* `JAVA_HOME` -moet verwijzen naar de map waarin het Java runtime environment (JRE) is geïnstalleerd. Bijvoorbeeld op een OS X-, Unix- of Linux-systeem, moet er een waarde gelijkaardig aan `/usr/lib/jvm/java-7-oracle`. In Windows, zou er een waarde gelijkaardig aan `c:\Program Files (x86)\Java\jre1.7`

* `PATH` -moet de volgende paden bevatten:
  
  * `JAVA_HOME` (of het equivalent pad)

  * `JAVA_HOME\bin` (of het equivalent pad)

  * De map waarin Maven is geïnstalleerd

## <a name="create-a-maven-project"></a>Maak een Maven-project

1. Vanuit een terminal-sessie, of vanaf de opdrachtregel in uw ontwikkelingsomgeving, wijzig de mappen in de locatie die u wilt opslaan van dit project.

2. Gebruik de `mvn` opdracht, die is geïnstalleerd met Maven, voor het genereren van het hulpprogramma voor het project.

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > Als u PowerShell gebruikt, moet u tussen de `-D` parameters tussen dubbele aanhalingstekens.
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Met deze opdracht maakt een map met de naam die is opgegeven door de `artifactID` parameter (**wordcountjava** in dit voorbeeld.) Deze map bevat de volgende items:

   * `pom.xml` -De [Project Object Model (POM)](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html) waarin gegevens en configuratie-informatie die wordt gebruikt om het project te bouwen.

   * `src` -De map waarin de toepassing.

3. Verwijder de `src/test/java/org/apache/hadoop/examples/apptest.java` bestand. Het wordt niet gebruikt in dit voorbeeld.

## <a name="add-dependencies"></a>Afhankelijkheden toevoegen

1. Bewerk de `pom.xml` -bestand en voeg de volgende tekst binnen de `<dependencies>` sectie:
   
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

    Hiermee definieert u vereiste bibliotheken (vermeld in &lt;artifactId\>) met een specifieke versie (vermeld in &lt;versie\>). Bij het compileren, worden deze afhankelijkheden gedownload vanuit de opslagplaats van de Maven standaard. U kunt de [Maven-opslagplaats zoeken](https://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) om informatie weer te geven.
   
    De `<scope>provided</scope>` vertelt Maven dat deze afhankelijkheden niet moeten worden verpakt met de toepassing die worden geleverd door het HDInsight-cluster tijdens de uitvoering.

    > [!IMPORTANT]
    > De versie die wordt gebruikt, moet overeenkomen met de versie van Hadoop aanwezig zijn op uw cluster. Zie voor meer informatie over versies, de [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md) document.

2. Voeg het volgende toe aan het bestand `pom.xml`. Deze tekst moet zich binnen in de `<project>...</project>` labels in het bestand, bijvoorbeeld tussen `</dependencies>` en `</project>`.

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

    De eerste-invoegtoepassing configureert u de [Maven-invoegtoepassing voor tint](https://maven.apache.org/plugins/maven-shade-plugin/), dat wordt gebruikt voor het bouwen van een uberjar (ook wel een fatjar), die afhankelijkheden die zijn vereist voor de toepassing bevat. Dit voorkomt ook dat de duplicatie van licenties binnen de jar-pakket, waardoor problemen kunnen ontstaan op sommige systemen.

    De tweede invoegtoepassing configureert u de Java-versie van het doel.

    > [!NOTE]
    > HDInsight 3.4 en eerdere versies gebruiken Java 7. Java 8 maakt gebruik van HDInsight 3.5 en hoger.

3. Sla het bestand `pom.xml` op.

## <a name="create-the-mapreduce-application"></a>De MapReduce-toepassing maken

1. Ga naar de `wordcountjava/src/main/java/org/apache/hadoop/examples` directory en de naam van de `App.java` van het bestand in `WordCount.java`.

2. Open de `WordCount.java` in een teksteditor het bestand en vervang de inhoud door de volgende tekst:
   
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
   
    U ziet de naam van het pakket is `org.apache.hadoop.examples` en de naam van de klasse is `WordCount`. U gebruikt deze namen wanneer u bij het verzenden van de MapReduce-taak.

3. Sla het bestand op.

## <a name="build-the-application"></a>De toepassing bouwen

1. Wijzig in het `wordcountjava` directory, als u nog niet er zijn.

2. Gebruik de volgende opdracht om te maken van een JAR-bestand met de toepassing:

   ```
   mvn clean package
   ```

    Met deze opdracht verwijdert vorige build-artefacten, downloadt alle afhankelijkheden die zijn nog niet is geïnstalleerd, en klik vervolgens builds en pakket met de toepassing.

3. Als de opdracht is voltooid, de `wordcountjava/target` map bevat een bestand met de naam `wordcountjava-1.0-SNAPSHOT.jar`.
   
   > [!NOTE]
   > De `wordcountjava-1.0-SNAPSHOT.jar` bestand is een uberjar waarin niet alleen de WordCount-taak, maar ook afhankelijkheden die de taak is vereist tijdens runtime.

## <a id="upload"></a>Uploaden van de jar

Gebruik de volgende opdracht naar het jar-bestand uploaden naar het hoofdknooppunt van HDInsight:

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

Vervang __gebruikersnaam__ met de naam van uw SSH-gebruiker voor het cluster. Vervang __CLUSTERNAME__ met de naam van het HDInsight-cluster.

Met deze opdracht kopieert de bestanden uit het lokale systeem naar het hoofdknooppunt. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

## <a name="run"></a>De MapReduce-taak uitgevoerd op Hadoop

1. Verbinding maken met HDInsight met behulp van SSH. Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

2. Gebruik de volgende opdracht om uit te voeren van de MapReduce-toepassing van de SSH-sessie:
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    Deze opdracht start de toepassing WordCount MapReduce. Het invoerbestand is `/example/data/gutenberg/davinci.txt`, en de uitvoermap is `/example/data/wordcountout`. Zowel het bestand voor invoer en uitvoer worden naar de standaardopslag voor het cluster opgeslagen.

3. Zodra de taak is voltooid, gebruikt u de volgende opdracht uit om de resultaten weer te geven:
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    U ontvangt een lijst met woorden en telt, met waarden die vergelijkbaar is met de volgende tekst:
   
        zeal    1
        zelus   1
        zenith  2

## <a id="nextsteps"></a>Volgende stappen

In dit document hebt u geleerd over het ontwikkelen van een Java MapReduce-taak. Zie de volgende documenten voor andere manieren om te werken met HDInsight.

* [Apache Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)

Zie voor meer informatie, ook de [Java Developer Center](https://azure.microsoft.com/develop/java/).

[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]:hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]:apache-hadoop-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-power-query]:apache-hadoop-connect-excel-power-query.md

[powershell-PSCredential]: https://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

