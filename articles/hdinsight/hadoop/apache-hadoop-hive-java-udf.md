---
title: Java gebruiker gedefinieerde functie (UDF met Hive in HDInsight - Azure) | Microsoft Docs
description: Informatie over het maken van een op Java gebaseerd gebruiker gedefinieerde functie (UDF die met Hive werkt). In dit voorbeeld UDF converteert een tabel van tekenreeksen in kleine letters.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 8d4f8efe-2f01-4a61-8619-651e873c7982
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/01/2017
ms.author: larryfr
ms.openlocfilehash: 20473f5e462bfcace9c982786b771e01df337eab
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2017
---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a>Gebruik van een Java UDF met Hive in HDInsight

Informatie over het maken van een op Java gebaseerd gebruiker gedefinieerde functie (UDF die met Hive werkt). De Java-UDF in dit voorbeeld converteert een tabel van tekenreeksen alle kleine tekens.

## <a name="requirements"></a>Vereisten

* Een HDInsight-cluster 

    > [!IMPORTANT]
    > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

    De meeste stappen in dit document werken op zowel Windows - en Linux gebaseerde clusters. De stappen voor het uploaden van de gecompileerde UDF aan het cluster en voer dit zijn echter specifiek voor op basis van Linux-clusters. Vindt u koppelingen naar informatie die kan worden gebruikt met Windows gebaseerde clusters.

* [Java-JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 of hoger (of een vergelijkbare groep, zoals OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Een teksteditor of IDE voor Java

    > [!IMPORTANT]
    > Als u de Python-bestanden op een Windows-client maakt, moet u een editor die LF als een regel beëindigen gebruikt. Als u niet zeker of uw editor LF of CRLF gebruikt weet, raadpleegt u de [probleemoplossing](#troubleshooting) sectie voor stapsgewijze instructies voor het teken CR wordt verwijderd.

## <a name="create-an-example-java-udf"></a>Een voorbeeld Java UDF maken 

1. Gebruik de volgende naar een nieuw Maven-project maken vanaf een opdrachtregel:

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > Als u met behulp van PowerShell, kunt u aanhalingstekens rond de parameters moet zetten. Bijvoorbeeld `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Deze opdracht maakt u een map met de naam **exampleudf**, die het Maven-project bevat.

2. Zodra het project is gemaakt, verwijdert de **src-exampleudf/Testscenario** map die is gemaakt als onderdeel van het project.

3. Open de **exampleudf/pom.xml**, en vervang de bestaande `<dependencies>` vermelding met de volgende XML-code:

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.7.3</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>1.2.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    ```

    Deze vermeldingen Geef de versie van Hadoop en Hive met HDInsight 3.5 opgenomen. U vindt meer informatie over de versies van Hadoop en Hive voorzien in HDInsight via de [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md) document.

    Voeg een `<build>` sectie voordat de `</project>` regel aan het einde van het bestand. Deze sectie, moet de volgende XML bevatten:

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.5  -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <!-- Keep us from getting a can't overwrite file error -->
                    <transformers>
                        <transformer
                                implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                        </transformer>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
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
        </plugins>
    </build>
    ```

    Deze vermeldingen definiëren hoe om het project te bouwen. In het bijzonder de versie van Java die gebruikmaakt van het project en het bouwen van een uberjar voor de implementatie van het cluster.

    Sla het bestand nadat de wijzigingen zijn aangebracht.

4. Wijzig de naam van **exampleudf/src/main/java/com/microsoft/examples/App.java** naar **ExampleUDF.java**, en open vervolgens het bestand in uw editor.

5. Vervang de inhoud van de **ExampleUDF.java** door het volgende bestand en sla het bestand.

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of the UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of the input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If the value is null, return a null
            if(input == null)
                return null;
            // Lowercase the input string and return it
            return input.toLowerCase();
        }
    }
    ```

    Deze code implementeert een UDF die een string-waarde accepteert en retourneert een kleine versie van de tekenreeks.

## <a name="build-and-install-the-udf"></a>Bouwen en de UDF installeren

1. Gebruik de volgende opdracht om te compileren en de UDF pakket:

    ```bash
    mvn compile package
    ```

    Met deze opdracht maakt en pakketten van de UDF in de `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` bestand.

2. Gebruik de `scp` opdracht het bestand kopiëren naar het HDInsight-cluster.

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    Vervang `myuser` met het SSH-gebruikersaccount voor uw cluster. Vervang `mycluster` met de naam van het cluster. Als u een wachtwoord gebruikt om de SSH-account te beveiligen, wordt u gevraagd het wachtwoord invoeren. Als u een certificaat gebruikt, moet u mogelijk gebruik van de `-i` parameter om een bestand met de persoonlijke sleutel.

3. Verbinding maken met het cluster via SSH.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

4. Kopieer het jar-bestand HDInsight-opslag van de SSH-sessie.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a>Gebruik de UDF van Hive

1. Gebruik de volgende op de client Beeline starten vanaf de SSH-sessie.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

    Met deze opdracht wordt ervan uitgegaan dat u de standaardwaarde van gebruikt **admin** voor het aanmeldingsaccount voor uw cluster.

2. Wanneer u aankomt bij de `jdbc:hive2://localhost:10001/>` gevraagd, voert u het volgende als u wilt de UDF toevoegen aan Hive en deze als een functie weer te geven.

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > In dit voorbeeld wordt ervan uitgegaan dat Azure Storage standaard opslagruimte voor het cluster. Als uw cluster in plaats daarvan Data Lake Store gebruikt, wijzigt u de `wasb:///` van waarde naar `adl:///`.

3. De UDF gebruiken om waarden opgehaald uit een tabel naar kleine letters tekenreeksen te converteren.

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    Deze query selecteert het platform van het apparaat (Android, Windows, iOS, enzovoort) uit de tabel, converteert u de tekenreeks voor kleine, en deze vervolgens weergeven. De uitvoer lijkt op de volgende tekst:

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Volgende stappen

Zie voor andere manieren om te werken met Hive, [Hive gebruiken met HDInsight](hdinsight-use-hive.md).

Zie voor meer informatie over functies Hive User-Defined [Hive operatoren en door de gebruiker gedefinieerde functies](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) sectie van de Hive-wiki op apache.org.
