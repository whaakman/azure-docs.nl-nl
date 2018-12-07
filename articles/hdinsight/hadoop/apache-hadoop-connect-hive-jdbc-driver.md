---
title: Apache Hive query via het JDBC-stuurprogramma - Azure HDInsight
description: Het JDBC-stuurprogramma van een Java-toepassing gebruiken voor het indienen van Apache Hive-query's met Hadoop in HDInsight. Verbinding maken via een programma en van de SQuirrel SQL-client.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 04/02/2018
ms.author: hrasheed
ms.openlocfilehash: 225cb3d2f78f41bdb17763d13644c1d95bc62710
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53014698"
---
# <a name="query-apache-hive-through-the-jdbc-driver-in-hdinsight"></a>Apache Hive query via het JDBC-stuurprogramma in HDInsight

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Informatie over het gebruiken van het JDBC-stuurprogramma van een Java-toepassing voor het indienen van Apache Hive-query's op Apache Hadoop in Azure HDInsight. De informatie in dit document laat zien hoe u verbinding maakt via een programma en van de SQuirrel SQL-client.

Zie voor meer informatie over het Hive JDBC-Interface, [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Vereisten

* Een Hadoop op HDInsight-cluster.

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie, [HDInsight 3.3 buiten gebruik stellen](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [SQuirreL SQL](https://squirrel-sql.sourceforge.net/). SQuirreL is een JDBC-clienttoepassing.

* De [Java Developer Kit (JDK) versie 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) of hoger.

* [Apache Maven](https://maven.apache.org). Maven is een project gemaakt systeem voor Java-projecten die door het project dat is gekoppeld aan dit artikel wordt gebruikt.

## <a name="jdbc-connection-string"></a>JDBC-verbindingsreeks

JDBC-verbindingen met een HDInsight-cluster op Azure meer dan 443 zijn gemaakt en het verkeer dat is beveiligd met SSL. De openbare gateway die de clusters bevinden zich achter leidt het verkeer naar de poort die HiveServer2 daadwerkelijk luistert. De volgende verbindingsreeks ziet u de indeling moet worden gebruikt voor HDInsight:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

Vervang `CLUSTERNAME` door de naam van uw HDInsight-cluster.

## <a name="authentication"></a>Verificatie

Bij het maken van de verbinding, moet u de naam van de beheerder HDInsight-cluster en het wachtwoord voor de verificatie bij de gateway van het cluster. Bij het verbinden van clients, zoals SQuirreL SQL JDBC, moet u de naam van de beheerder en het wachtwoord invoeren in de clientinstellingen.

Tot een Java-toepassing, moet u de naam en het wachtwoord gebruiken wanneer een verbinding tot stand brengen. Bijvoorbeeld, opent de volgende Java-code u een nieuwe verbinding met de verbindingsreeks, de naam van de beheerder en het wachtwoord:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>Verbinding maken met SQuirreL SQL-client

SQuirreL SQL is een JDBC-client die kan worden gebruikt voor het extern uitvoeren van Hive-query's met uw HDInsight-cluster. De volgende stappen wordt ervan uitgegaan dat u SQuirreL SQL al hebt geïnstalleerd.

1. Maak een map die de bestanden bevat. Bijvoorbeeld `mkdir hivedriver`.

2. Gebruik de volgende opdrachten om te kopiëren van de bestanden van het HDInsight-cluster vanaf een opdrachtregel:

    ```bash
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-common.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/hadoop-auth.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/lib/log4j-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hadoop-client/lib/slf4j-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/hive-*-1.2*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/httpclient-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/httpcore-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/libthrift-*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/libfb*.jar .
    scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-logging-*.jar .
    ```

    Vervang `USERNAME` met de accountnaam van de SSH-gebruiker voor het cluster. Vervang `CLUSTERNAME` met de naam van het HDInsight-cluster.

3. De SQuirreL SQL-toepassing te starten. Selecteer in de linkerkant van het venster **stuurprogramma's**.

    ![Tabblad stuurprogramma's aan de linkerkant van het venster](./media/apache-hadoop-connect-hive-jdbc-driver/squirreldrivers.png)

4. Uit de pictogrammen aan de bovenkant van de **stuurprogramma's** dialoogvenster, selecteer de **+** pictogram voor het maken van een stuurprogramma.

    ![Pictogrammen van stuurprogramma 's](./media/apache-hadoop-connect-hive-jdbc-driver/driversicons.png)

5. Voeg de volgende informatie in het dialoogvenster stuurprogramma toevoegen:

    * **Naam**: Hive
    * **Voorbeeld-URL**: `jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`
    * **Extra klassepad**: Gebruik de knop toevoegen om toe te voegen van alle eerder hebt gedownload jar-bestanden
    * **Naam klasse**: org.apache.hive.jdbc.HiveDriver

   ![stuurprogramma-dialoogvenster toevoegen](./media/apache-hadoop-connect-hive-jdbc-driver/adddriver.png)

   Klik op **OK** deze instellingen op te slaan.

6. Selecteer aan de linkerkant van het SQuirreL SQL-venster, **aliassen**. Klik vervolgens op de **+** pictogram van de alias van een verbinding maken.

    ![nieuwe alias toevoegen](./media/apache-hadoop-connect-hive-jdbc-driver/aliases.png)

7. Gebruik de volgende waarden voor de **toevoegen Alias** dialoogvenster.

    * **Naam**: Hive in HDInsight

    * **Stuurprogramma**: Gebruik de vervolgkeuzelijst selecteert het **Hive** stuurprogramma

    * **URL**: `jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2`

        Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.

    * **Gebruikersnaam**: de accountnaam van de cluster-aanmelding voor uw HDInsight-cluster. De standaardwaarde is `admin`.

    * **Wachtwoord**: het wachtwoord voor de aanmeldingsaccount van het cluster.

 ![dialoogvenster alias toevoegen](./media/apache-hadoop-connect-hive-jdbc-driver/addalias.png)

    > [!IMPORTANT] 
    > Gebruik de **Test** knop om te controleren of de verbinding werkt. Wanneer **verbinding maken met: Hive in HDInsight** dialoogvenster wordt weergegeven, selecteert u **Connect** om uit te voeren van de test. Als de test is geslaagd, ziet u een **verbinding tot stand gebracht** dialoogvenster. Als er een fout optreedt, raadpleegt u [probleemoplossing](#troubleshooting).

    Om op te slaan de alias van de verbinding, gebruikt u de **Ok** knop aan de onderkant van de **toevoegen Alias** dialoogvenster.

8. Uit de **verbinding maken met** vervolgkeuzelijst aan de bovenkant van SQuirreL SQL, selecteer **Hive in HDInsight**. Wanneer u hierom wordt gevraagd, selecteert u **Connect**.

    ![het dialoogvenster verbinding](./media/apache-hadoop-connect-hive-jdbc-driver/connect.png)

9. Eenmaal verbinding hebben, voert u de volgende query in het dialoogvenster van de SQL-query's, en selecteer vervolgens de **uitvoeren** pictogram. De resultaten van de query moet worden weergegeven in het resultatengebied.

        select * from hivesampletable limit 10;

    ![dialoogvenster voor SQL query's, met inbegrip van resultaten](./media/apache-hadoop-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Verbinding maken vanuit een voorbeeld van Java-toepassing

Een voorbeeld van het gebruik van een Java-client query Hive in HDInsight is beschikbaar op [ https://github.com/Azure-Samples/hdinsight-java-hive-jdbc ](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Volg de instructies in de opslagplaats wilt bouwen en uitvoeren van het voorbeeld.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a>Onverwachte fout opgetreden bij het openen van een SQL-verbinding

**Symptomen**: bij het verbinden met een HDInsight-cluster versie 3.3 of hoger is, verschijnt er een fout die is een onverwachte fout opgetreden. De stack-trace voor deze fout begint met de volgende regels:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Oorzaak**: deze fout wordt veroorzaakt door een oudere versie commons codec.jar-bestand met SQuirreL opgenomen.

**Resolutie**: U kunt deze fout oplossen, gebruik de volgende stappen:

1. Download het commons codec jar-bestand van het HDInsight-cluster.

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. SQuirreL afsluiten en gaat u naar de map waar SQuirreL is geïnstalleerd op uw systeem. In de map SquirreL onder de `lib` directory, vervangen de bestaande commons codec.jar met de versie die is gedownload van het HDInsight-cluster.

3. SQuirreL opnieuw. De fout kan niet meer optreden bij het verbinden met Hive in HDInsight.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe JDBC gebruiken om te werken met Hive, gebruikt u de volgende koppelingen op andere manieren om te werken met Azure HDInsight.

* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Interactive Query Hive-gegevens visualiseren met Power BI in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Koppel Excel aan Hadoop met Power Query](apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md)
* [Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce-taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)
