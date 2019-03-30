---
title: Java-de gebruiker gedefinieerde functie (UDF's) met Apache Hive in HDInsight - Azure
description: Informatie over het maken van een op Java gebaseerde gebruiker gedefinieerde functie (UDF's) die met Apache Hive werkt. In dit voorbeeld UDF converteert een tabel met teksttekenreeksen naar kleine letters.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: hrasheed
ms.openlocfilehash: b8417fe4c15259a7fd485254cf9edd2c8c082e92
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629701"
---
# <a name="use-a-java-udf-with-apache-hive-in-hdinsight"></a>Gebruik van een Java UDF met Apache Hive in HDInsight

Informatie over het maken van een op Java gebaseerde gebruiker gedefinieerde functie (UDF's) die met Apache Hive werkt. De Java UDF in dit voorbeeld zijn een tabel met teksttekenreeksen geconverteerd naar alle kleine tekens.

## <a name="prerequisites"></a>Vereisten

* Een Hadoop-cluster op HDInsight. Zie [aan de slag met HDInsight op Linux](./apache-hadoop-linux-tutorial-get-started.md).
* [Java Developer Kit (JDK) versie 8](https://aka.ms/azure-jdks)
* [Apache Maven](https://maven.apache.org/download.cgi) goed [geïnstalleerd](https://maven.apache.org/install.html) op basis van Apache.  Maven is een project maken voor Java-projecten.
* De [URI-schema](../hdinsight-hadoop-linux-information.md#URI-and-scheme) voor uw clusters primaire opslag. Zou dit wasb: / / voor Azure Storage, abfs: / / voor Azure Data Lake Storage Gen2 of adl: / / voor Azure Data Lake Storage Gen1. Als veilige overdracht is ingeschakeld voor Azure Storage of Data Lake Storage Gen2, zou de URI wasbs: / / of abfss: / /, respectievelijk Zie ook [veilige overdracht](../../storage/common/storage-require-secure-transfer.md).

* Een teksteditor of IDE voor Java

    > [!IMPORTANT]  
    > Als u de Python-bestanden op een Windows-client maakt, moet u een editor die gebruikmaakt van LF als het einde van een regel. Als u niet zeker weet of uw editor LF of CRLF gebruikt, Zie de [probleemoplossing](#troubleshooting) sectie voor stapsgewijze instructies voor het verwijderen van het teken CR.

## <a name="test-environment"></a>Testomgeving
De omgeving moet worden gebruikt voor dit artikel is een computer met Windows 10.  De opdrachten zijn uitgevoerd in een opdrachtprompt en de verschillende bestanden zijn bewerkt met Kladblok. Aanpassen voor uw omgeving.

Voer de onderstaande opdrachten om een werkende-omgeving te maken vanaf een opdrachtprompt:

```cmd
IF NOT EXIST C:\HDI MKDIR C:\HDI
cd C:\HDI
```

## <a name="create-an-example-java-udf"></a>Een voorbeeld Java UDF maken

1. Maak een nieuw Maven-project met de volgende opdracht:

    ```cmd
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    Deze opdracht maakt u een map met de naam `exampleudf`, die de Maven-project bevat.

2. Zodra het project is gemaakt, verwijdert de `exampleudf/src/test` map die is gemaakt als onderdeel van het project met de volgende opdracht:

    ```cmd
    cd ExampleUDF
    rmdir /S /Q "src/test"
    ```

3. Open `pom.xml` met de onderstaande opdracht:

    ```cmd
    notepad pom.xml
    ```

    Vervang de bestaande `<dependencies>` vermelding met de volgende XML:

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
                <version>3.2.1</version>
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

4. Voer de opdracht hieronder om te maken en open een nieuw bestand `ExampleUDF.java`:

    ```cmd
    notepad src/main/java/com/microsoft/examples/ExampleUDF.java
    ```

    Kopieer en plak de volgende java-code naar het nieuwe bestand. Sluit het bestand.

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

Vervang in de onderstaande opdrachten `sshuser` met de werkelijke gebruikersnaam als deze verschilt. Vervang `mycluster` met de naam van het daadwerkelijke cluster.

1. Compileren en inpakken van de UDF met de volgende opdracht:

    ```cmd
    mvn compile package
    ```

    Met deze opdracht bouwt en verpakt de UDF in de `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` bestand.

2. Gebruik de `scp` opdracht het bestand kopiëren naar het HDInsight-cluster met de volgende opdracht:

    ```cmd
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net:
    ```

3. Verbinding maken met het cluster via SSH met de volgende opdracht:

    ```cmd
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

4. Kopieer het jar-bestand naar een HDInsight-opslag van de open SSH-sessie.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a>De UDF uit Hive gebruiken

1. De client Beeline beginnen bij de SSH-sessie met de volgende opdracht:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

    Met deze opdracht wordt ervan uitgegaan dat u de standaardwaarde van gebruikt **admin** voor de aanmeldingsaccounts voor uw cluster.

2. Zodra er uitziet de `jdbc:hive2://localhost:10001/>` wordt gevraagd, voert u het volgende als u wilt de UDF toevoegen aan Hive en deze beschikbaar te maken als een functie.

    ```hiveql
    ADD JAR wasbs:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

3. Gebruik de UDF om te converteren die zijn opgehaald uit een tabel naar tekenreeksen met kleine letters.

    ```hiveql
    SELECT tolower(state) AS ExampleUDF, state FROM hivesampletable LIMIT 10;
    ```

    Deze query wordt de status uit de tabel, converteert de tekenreeks voor het lager case en ze vervolgens weergeven samen met de naam van de ongewijzigd. De uitvoer wordt weergegeven die vergelijkbaar is met de volgende tekst:

        +---------------+---------------+--+
        |  exampleudf   |     state     |
        +---------------+---------------+--+
        | california    | California    |
        | pennsylvania  | Pennsylvania  |
        | pennsylvania  | Pennsylvania  |
        | pennsylvania  | Pennsylvania  |
        | colorado      | Colorado      |
        | colorado      | Colorado      |
        | colorado      | Colorado      |
        | utah          | Utah          |
        | utah          | Utah          |
        | colorado      | Colorado      |
        +---------------+---------------+--+

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer de hive-taak wordt uitgevoerd, kunt u een vergelijkbaar met de volgende tekst fout kan optreden:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Dit probleem kan worden veroorzaakt door de regeleinden in de Python-bestand. Groot aantal Windows-editors maken standaard gebruik CRLF als het einde van regel, maar Linux-toepassingen meestal verwachten LF.

U kunt de volgende PowerShell-instructies gebruiken de CR tekens verwijderen voordat u het bestand te uploaden naar HDInsight:

```PowerShell
# Set $original_file to the python file path
$text = [IO.File]::ReadAllText($original_file) -replace "`r`n", "`n"
[IO.File]::WriteAllText($original_file, $text)
```

## <a name="next-steps"></a>Volgende stappen

Zie voor andere manieren om te werken met Hive, [Apache Hive gebruiken met HDInsight](hdinsight-use-hive.md).

Zie voor meer informatie over de functies Hive User-Defined [Apache Hive operatoren en door de gebruiker gedefinieerde functies](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) sectie van de Hive-wiki op apache.org.
