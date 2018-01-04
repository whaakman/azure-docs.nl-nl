---
title: Hive query via de JDBC-stuurprogramma - Azure HDInsight | Microsoft Docs
description: Gebruikt de JDBC-stuurprogramma van een Java-toepassing voor het verzenden van Hive-query's en Hadoop op HDInsight. Verbinding maken via een programma en van de SQuirrel SQL-client.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 928f8d2a-684d-48cb-894c-11c59a5599ae
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: larryfr
ms.openlocfilehash: 7da4a7e0a60fd1e5c78f53b0a8e7ab333c5d2465
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="query-hive-through-the-jdbc-driver-in-hdinsight"></a>Hive query via JDBC-stuurprogramma in HDInsight

[!INCLUDE [ODBC-JDBC-selector](../../../includes/hdinsight-selector-odbc-jdbc.md)]

Informatie over het JDBC-stuurprogramma van een Java-toepassing gebruikt voor het verzenden van Hive-query's en Hadoop in Azure HDInsight. De informatie in dit document laat zien hoe u verbinding maken via een programma en van de SQuirrel SQL-client.

Zie voor meer informatie over de Interface JDBC Hive [HiveJDBCInterface](https://cwiki.apache.org/confluence/display/Hive/HiveJDBCInterface).

## <a name="prerequisites"></a>Vereisten

* Een Hadoop op HDInsight-cluster. Linux- of Windows clusters werkt.

  > [!IMPORTANT]
  > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie [HDInsight 3.3 buiten gebruik stellen](../hdinsight-component-versioning.md#hdinsight-windows-retirement).

* [SQuirreL SQL](http://squirrel-sql.sourceforge.net/). SQuirreL is een JDBC-clienttoepassing.

* De [Java Developer Kit (JDK) versie 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) of hoger.

* [Apache Maven](https://maven.apache.org). Maven is een project bouwen voor Java-projecten-systeem dat wordt gebruikt door het project gekoppeld aan dit artikel.

## <a name="jdbc-connection-string"></a>JDBC-verbindingsreeks

JDBC verbindingen met een HDInsight-cluster in Azure gemaakt dan 443 en het verkeer is beveiligd met SSL. De openbare gateway die voor de clusters bevinden zich achter stuurt het verkeer naar de poort die HiveServer2 daadwerkelijk luistert op. De volgende verbindingsreeks ziet u de indeling moet worden gebruikt voor HDInsight:

    jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2

Vervang `CLUSTERNAME` door de naam van uw HDInsight-cluster.

## <a name="authentication"></a>Verificatie

Bij het maken van de verbinding, moet u de HDInsight-cluster admin naam en het wachtwoord voor de verificatie bij de gateway van het cluster. Bij het verbinden van clients JDBC zoals SQuirreL SQL, moet u de naam van de serverbeheerder en het wachtwoord in de clientinstellingen.

Van een Java-toepassing, moet u de naam en het wachtwoord gebruiken wanneer een verbinding tot stand brengen. De volgende Java-code opent u bijvoorbeeld een nieuwe verbinding met de verbindingsreeks, de naam van de serverbeheerder en het wachtwoord:

```java
DriverManager.getConnection(connectionString,clusterAdmin,clusterPassword);
```

## <a name="connect-with-squirrel-sql-client"></a>Verbinding maken met SQuirreL SQL-client

SQuirreL SQL is een JDBC-client die op afstand Hive-query's uitvoeren met uw HDInsight-cluster kan worden gebruikt. De volgende stappen wordt ervan uitgegaan dat u SQuirreL SQL al hebt geïnstalleerd.

1. Kopieer de Hive-JDBC-stuurprogramma's van uw HDInsight-cluster.

    * Voor **Linux gebaseerde HDInsight** cluster versie 3.5 of 3.6, gebruik de volgende stappen voor het downloaden van de vereiste jar-bestanden.

        1. Maak een map waarin de bestanden. Bijvoorbeeld `mkdir hivedriver`.

        2. Gebruik de volgende opdrachten om te kopiëren van het HDInsight-cluster vanaf een opdrachtregel:

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

    * Voor **HDInsight op basis van Windows**, gebruik de volgende stappen om de jar-bestanden te downloaden.

        1. Vanuit de Azure-portal, selecteer uw HDInsight-cluster en selecteer vervolgens de **extern bureaublad** pictogram.

            ![Pictogram voor extern bureaublad](./media/apache-hadoop-connect-hive-jdbc-driver/remotedesktopicon.png)

        2. In de sectie extern bureaublad gebruiken de **Connect** knop verbinding maken met het cluster. Als het externe bureaublad niet is ingeschakeld, gebruikt u de indeling naar een gebruikersnaam en wachtwoord opgeven en selecteer vervolgens **inschakelen** extern bureaublad inschakelen voor het cluster.

            ![Extern bureaublad-sectie](./media/apache-hadoop-connect-hive-jdbc-driver/remotedesktopblade.png)

            Na het selecteren van **Connect**, wordt er een. RDP-bestand wordt gedownload. Dit bestand gebruiken voor het starten van de extern bureaublad-client. Wanneer u wordt gevraagd, gebruikt u de gebruikersnaam en wachtwoord die u hebt opgegeven voor toegang tot extern bureaublad.

        3. Kopieer de volgende bestanden vanuit de extern bureaublad-sessie eenmaal zijn verbonden, op uw lokale computer. Plaatsen in een lokale map met de naam `hivedriver`.

            * C:\apps\dist\hive-0.14.0.2.2.9.1-7\lib\hive-JDBC-0.14.0.2.2.9.1-7-standalone.jar
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\hadoop-Common-2.6.0.2.2.9.1-7.jar
            * C:\apps\dist\hadoop-2.6.0.2.2.9.1-7\share\hadoop\common\lib\hadoop-auth-2.6.0.2.2.9.1-7.jar

            > [!NOTE]
            > Het versienummer dat is opgenomen in de paden en bestandsnamen kunnen afwijken voor uw cluster.

        4. Verbreek de verbinding met extern bureaublad-sessie wanneer u klaar bent met het kopiëren van de bestanden.

2. Start de toepassing SQuirreL SQL. Selecteer in de links in het venster **stuurprogramma's**.

    ![Tabblad stuurprogramma's aan de linkerkant van het venster](./media/apache-hadoop-connect-hive-jdbc-driver/squirreldrivers.png)

3. Van de pictogrammen aan het begin van de **stuurprogramma's** dialoogvenster, selecteer de  **+**  pictogram voor het maken van een stuurprogramma.

    ![Stuurprogramma's pictogrammen](./media/apache-hadoop-connect-hive-jdbc-driver/driversicons.png)

4. Voeg de volgende informatie in het dialoogvenster stuurprogramma toevoegen:

    * **Naam**: Hive
    * **Voorbeeld-URL**:`jdbc:hive2://localhost:443/default;transportMode=http;ssl=true;httpPath=/hive2`
    * **Extra klassepad**: Gebruik de knop toevoegen om toe te voegen van de jar-bestanden eerder hebt gedownload
    * **Klassenaam**: org.apache.hive.jdbc.HiveDriver

   ![stuurprogramma-dialoogvenster toevoegen](./media/apache-hadoop-connect-hive-jdbc-driver/adddriver.png)

   Klik op **OK** deze instellingen op te slaan.

5. Selecteer aan de linkerkant van het venster SQuirreL SQL **aliassen**. Klik vervolgens op de  **+**  pictogram een alias verbinding maakt.

    ![nieuwe alias toevoegen](./media/apache-hadoop-connect-hive-jdbc-driver/aliases.png)

6. Gebruik de volgende waarden voor de **Alias toevoegen** dialoogvenster.

    * **Naam**: Hive in HDInsight

    * **Stuurprogramma**: Gebruik de vervolgkeuzelijst selecteren de **Hive** stuurprogramma

    * **URL**:`jdbc:hive2://CLUSTERNAME.azurehdinsight.net:443/default;transportMode=http;ssl=true;httpPath=/hive2`

        Vervang **CLUSTERNAME** door de naam van uw HDInsight-cluster.

    * **Gebruikersnaam**: de accountnaam van de cluster-aanmelding voor uw HDInsight-cluster. De standaardwaarde is `admin`.

    * **Wachtwoord**: het wachtwoord voor de aanmeldingsaccount van het cluster.

 ![dialoogvenster alias toevoegen](./media/apache-hadoop-connect-hive-jdbc-driver/addalias.png)

    Gebruik de **Test** knop om te controleren of de verbinding werkt. Wanneer **verbinding maken met: Hive in HDInsight** dialoogvenster wordt weergegeven, selecteert u **Connect** om uit te voeren van de test. Als de test is geslaagd, ziet u een **verbinding tot stand gebracht** dialoogvenster. Als er een fout optreedt, raadpleegt u [probleemoplossing](#troubleshooting).

    Voor het opslaan van de verbinding alias gebruiken de **Ok** knop aan de onderkant van de **Alias toevoegen** dialoogvenster.

7. Van de **verbinding maken met** selecteert u de vervolgkeuzelijst aan de bovenkant van de SQL-SQuirreL **Hive in HDInsight**. Wanneer u wordt gevraagd, selecteert u **Connect**.

    ![het dialoogvenster voor verbinding](./media/apache-hadoop-connect-hive-jdbc-driver/connect.png)

8. Eenmaal zijn verbonden, voert u de volgende query in het dialoogvenster van de SQL-query's en selecteer vervolgens de **uitvoeren** pictogram. De resultaten van de query moet worden weergegeven in het resultatengebied.

        select * from hivesampletable limit 10;

    ![dialoogvenster voor SQL query's, met inbegrip van resultaten](./media/apache-hadoop-connect-hive-jdbc-driver/sqlquery.png)

## <a name="connect-from-an-example-java-application"></a>Verbinding maken vanaf een voorbeeld van Java-toepassing

Een voorbeeld van het gebruik van een Java-client aan query Hive in HDInsight is beschikbaar op [https://github.com/Azure-Samples/hdinsight-java-hive-jdbc](https://github.com/Azure-Samples/hdinsight-java-hive-jdbc). Volg de instructies in de opslagplaats het bouwen en uitvoeren van het voorbeeld.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="unexpected-error-occurred-attempting-to-open-an-sql-connection"></a>Onverwachte fout opgetreden bij het openen van een SQL-verbinding

**Symptomen**: bij het verbinden met een HDInsight-cluster versie 3.3 of hoger is, verschijnt er een fout die is een onverwachte fout opgetreden. De stacktracering voor deze fout begint met de volgende regels:

```java
java.util.concurrent.ExecutionException: java.lang.RuntimeException: java.lang.NoSuchMethodError: org.apache.commons.codec.binary.Base64.<init>(I)V
at java.util.concurrent.FutureTas...(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:206)
```

**Oorzaak**: deze fout wordt veroorzaakt door een oudere versie commons codec.jar-bestand met SQuirreL opgenomen.

**Resolutie**: U kunt deze fout oplossen door de volgende stappen gebruiken:

1. Download het commons codec jar-bestand van uw HDInsight-cluster.

        scp USERNAME@CLUSTERNAME:/usr/hdp/current/hive-client/lib/commons-codec*.jar ./commons-codec.jar

2. Sluit SQuirreL en Ga naar de map waar SQuirreL op uw systeem is geïnstalleerd. In de map SquirreL onder de `lib` directory, vervangen de bestaande commons codec.jar met de versie die wordt gedownload van het HDInsight-cluster.

3. Opnieuw opstarten SQuirreL. De fout mag niet langer optreden bij het verbinden met Hive in HDInsight.

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe JDBC gebruiken om te werken met Hive, gebruik de volgende koppelingen om te verkennen andere manieren om te werken met Azure HDInsight.

* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](apache-hadoop-connect-hive-power-bi.md).
* [Interactieve Hive-Query-gegevens visualiseren met Power BI in Azure HDInsight](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel en Hadoop koppelen met Power Query](apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md)
* [Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce-taken gebruiken met HDInsight](hdinsight-use-mapreduce.md)
