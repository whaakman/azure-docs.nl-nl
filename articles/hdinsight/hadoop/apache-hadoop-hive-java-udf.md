---
title: Java-de gebruiker gedefinieerde functie (UDF's) met Apache Hive in HDInsight - Azure
description: Informatie over het maken van een op Java gebaseerde gebruiker gedefinieerde functie (UDF's) die met Apache Hive werkt. In dit voorbeeld UDF converteert een tabel met teksttekenreeksen naar kleine letters.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: a58c61287cfc7828fe0dc15beda9d54f02a694c3
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814143"
---
# <a name="use-a-java-udf-with-apache-hive-in-hdinsight"></a>Gebruik van een Java UDF met Apache Hive in HDInsight

Informatie over het maken van een op Java gebaseerde gebruiker gedefinieerde functie (UDF's) die met Apache Hive werkt. De Java UDF in dit voorbeeld zijn een tabel met teksttekenreeksen geconverteerd naar alle kleine tekens.

## <a name="requirements"></a>Vereisten

* Een HDInsight-cluster 

    > [!IMPORTANT]
    > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

    De meeste stappen in dit document worden gebruikt voor zowel Windows - en Linux gebaseerde clusters. De stappen voor het uploaden van de compilatie UDF aan het cluster en de App uitvoeren, zijn echter specifiek voor op basis van Linux-clusters. Vindt u koppelingen naar informatie die kan worden gebruikt met Windows gebaseerde clusters.

* [Java JDK](https://www.oracle.com/technetwork/java/javase/downloads/) 8 of hoger (of een equivalent, zoals OpenJDK)

* [Apache Maven](https://maven.apache.org/)

* Een teksteditor of IDE voor Java

    > [!IMPORTANT]
    > Als u de Python-bestanden op een Windows-client maakt, moet u een editor die gebruikmaakt van LF als het einde van een regel. Als u niet zeker weet of uw editor LF of CRLF gebruikt, Zie de sectie over probleemoplossing voor stapsgewijze instructies over het verwijderen van het teken CR.

## <a name="create-an-example-java-udf"></a>Een voorbeeld Java UDF maken 

1. Gebruik de volgende naar een nieuw Maven-project maken vanaf een opdrachtregel:

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > Als u PowerShell gebruikt, moet u aanhalingstekens rond de parameters geplaatst. Bijvoorbeeld `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Deze opdracht maakt u een map met de naam **exampleudf**, die de Maven-project bevat.

2. Zodra het project is gemaakt, verwijdert de **exampleudf/src/test** map die is gemaakt als onderdeel van het project.

3. Open de **exampleudf/pom.xml**, en vervang de bestaande `<dependencies>` vermelding met de volgende XML:

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

    Deze vermeldingen opgeven de versie van Hadoop en Hive met HDInsight 3.6 opgenomen. U vindt meer informatie over de versies van Hadoop en Hive met HDInsight van opgegeven de [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md) document.

    Voeg een `<build>` sectie voordat u de `</project>` regel aan het einde van het bestand. In deze sectie bevatten de volgende XML:

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.6  -->
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

    Deze items definiëren hoe om het project te bouwen. Met name de versie van Java die gebruikmaakt van het project en over het bouwen van een uberjar voor implementatie in het cluster.

    Sla het bestand nadat de wijzigingen zijn aangebracht.

4. Wijzig de naam van **exampleudf/src/main/java/com/microsoft/examples/App.java** naar **ExampleUDF.java**, en open vervolgens het bestand in de editor.

5. Vervang de inhoud van de **ExampleUDF.java** bestand door het volgende en sla het bestand.

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

## <a name="build-and-install-the-udf"></a>Bouw en installeer de UDF

1. Gebruik de volgende opdracht om te compileren en de UDF pakket:

    ```bash
    mvn compile package
    ```

    Met deze opdracht bouwt en verpakt de UDF in de `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` bestand.

2. Gebruik de `scp` opdracht naar het bestand kopiëren naar het HDInsight-cluster.

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    Vervang `myuser` met de SSH-gebruikersaccount voor uw cluster. Vervang `mycluster` met de naam van het cluster. Als u een wachtwoord gebruikt om de SSH-account te beveiligen, wordt u gevraagd het wachtwoord invoeren. Als u een certificaat gebruikt, moet u mogelijk gebruik van de `-i` parameter opgeven voor het persoonlijke sleutelbestand.

3. Verbinding maken met het cluster via SSH.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

4. Kopieer het jar-bestand naar een HDInsight-opslag van de SSH-sessie.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a>De UDF uit Hive gebruiken

1. Gebruik de volgende de Beeline-client starten vanuit de SSH-sessie.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

    Met deze opdracht wordt ervan uitgegaan dat u de standaardwaarde van gebruikt **admin** voor de aanmeldingsaccounts voor uw cluster.

2. Zodra er uitziet de `jdbc:hive2://localhost:10001/>` wordt gevraagd, voert u het volgende als u wilt de UDF toevoegen aan Hive en deze beschikbaar te maken als een functie.

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > In dit voorbeeld wordt ervan uitgegaan dat Azure Storage standaardopslag voor het cluster. Als uw cluster maakt in plaats daarvan gebruik van de Data Lake-opslag, wijzigt u de `wasb:///` waarde die moet worden `adl:///`.

3. Gebruik de UDF om te converteren die zijn opgehaald uit een tabel naar tekenreeksen met kleine letters.

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    Deze query selecteert het apparaatplatform (Android, Windows, iOS, enzovoort) uit de tabel, converteert de tekenreeks voor kleine en ze vervolgens weergeven. De uitvoer wordt weergegeven die vergelijkbaar is met de volgende tekst:

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

Zie voor andere manieren om te werken met Hive, [Apache Hive gebruiken met HDInsight](hdinsight-use-hive.md).

Zie voor meer informatie over de functies Hive User-Defined [Apache Hive operatoren en door de gebruiker gedefinieerde functies](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) sectie van de Hive-wiki op apache.org.
