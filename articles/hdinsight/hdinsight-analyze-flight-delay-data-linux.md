---
title: 'Zelfstudie: ETL-bewerkingen (extraheren, transformeren, laden) uitvoeren met Hive in HDInsight - Azure '
description: In deze zelfstudie leert u hoe u gegevens uit een set met onbewerkte CSV-gegevens extraheert, de gegevens met Hive in HDInsight transformeert en de getransformeerde gegevens ten slotte laadt in een Azure SQL-database met behulp van Apache Sqoop.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive,mvc
ms.openlocfilehash: b8a45f3014b3fd5d0f5592b3f9bd408fc37b387b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57999953"
---
# <a name="tutorial-extract-transform-and-load-data-using-apache-hive-on-azure-hdinsight"></a>Zelfstudie: Gegevens uitpakken, transformeren en laden met Apache Hive in Azure HDInsight

In deze zelfstudie gaat u een CSV-bestand met onbewerkte gegevens importeren in een cluster van HDInsight, waarna u de gegevens transformeert met behulp van [Apache Hive](https://hive.apache.org/) in Azure HDInsight. Als de gegevens zijn getransformeerd, laadt u die gegevens met behulp van [Apache Sqoop](https://sqoop.apache.org/) in een Azure SQL-database. In dit artikel gebruikt u vluchtgegevens die vrij beschikbaar zijn.

> [!IMPORTANT]  
> Voor de stappen in dit document hebt u een HDInsight-cluster nodig dat werkt met Linux. Linux is het enige besturingssysteem dat wordt gebruikt in Azure HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

Deze zelfstudie bestaat uit de volgende taken: 

> [!div class="checklist"]
> * De voorbeeldbestanden met vluchtgegevens downloaden
> * Gegevens uploaden naar een HDInsight-cluster
> * De gegevens transformeren met Hive
> * Een tabel maken in de Azure SQL-database
> * Sqoop gebruiken om gegevens te exporteren naar de Azure SQL-database


In de volgende afbeelding ziet u een typische werkstroom van een ETL-toepassing.

![ETL-bewerking met Apache Hive in Azure HDInsight](./media/hdinsight-analyze-flight-delay-data-linux/hdinsight-etl-architecture.png "ETL-bewerking met Apache Hive in Azure HDInsight")

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **Een Hadoop-cluster op basis van Linux in HDInsight**. Zie [Aan de slag met Apache Hadoop in HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md) voor stapsgewijze instructies voor het maken van een nieuw op Linux gebaseerd HDInsight-cluster.

* **Azure SQL-database**. U gebruikt een Azure SQL-database als doelgegevensopslag. Als u geen SQL-database hebt, raadpleegt u [Een Azure SQL-database maken in Azure Portal](../sql-database/sql-database-get-started.md).

* **Azure CLI**. Als u de Azure CLI nog niet hebt geïnstalleerd, raadpleegt u [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) voor meer stappen.

* **Een SSH-client**. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="download-the-flight-data"></a>De vluchtgegevens downloaden

1. Blader naar [Research and Innovative Technology Administration, Bureau of Transportation Statistics][rita-website].

2. Selecteer de volgende waarden op de pagina:

   | Naam | Waarde |
   | --- | --- |
   | Filterjaar |2013 |
   | Filterperiode |Januari |
   | Velden |Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |
   
   Wis alle andere velden. 

3. Selecteer **Download**. U krijgt een ZIP-bestand met de gegevensvelden die u hebt geselecteerd.

## <a name="upload-data-to-an-hdinsight-cluster"></a>Gegevens uploaden naar een HDInsight-cluster

Er zijn veel manieren om gegevens te uploaden naar de opslag die is gekoppeld aan een HDInsight-cluster. In dit gedeelte gebruikt u `scp` om gegevens te uploaden. Zie [Gegevens uploaden naar HDInsight](hdinsight-upload-data.md) voor informatie over andere manieren om gegevens te uploaden.

1. Open een opdrachtprompt en gebruik de volgende opdracht om het ZIP-bestand te uploaden naar het hoofdknooppunt van het HDInsight-cluster:

    ```bash
    scp <FILENAME>.zip <SSH-USERNAME>@<CLUSTERNAME>-ssh.azurehdinsight.net:<FILENAME.zip>
    ```

    Vervang *FILENAME* door de naam van het ZIP-bestand. Vervang *USERNAME* door de SSH-aanmelding voor het HDInsight-cluster. Vervang *CLUSTERNAME* door de naam van het HDInsight-cluster.

   > [!NOTE]  
   > Als u een wachtwoord gebruikt om uw SSH-aanmelding te verifiëren, wordt u gevraagd om het wachtwoord. Als u een openbare sleutel gebruikt, moet u mogelijk de parameter `-i` gebruiken en het pad naar de bijbehorende persoonlijke sleutel opgeven. Bijvoorbeeld `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Nadat het uploaden is voltooid, maakt u via SSH verbinding met het cluster. Voer op de opdrachtprompt de volgende opdracht in:

    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

3. Gebruik de volgende opdracht om ZIP-bestand uit te pakken:

    ```bash
    unzip FILENAME.zip
    ```

    Met deze opdracht wordt een CSV-bestand uitgepakt van ongeveer 60 MB.

4. Gebruik de volgende opdrachten om een map te maken in de HDInsight-opslag en kopieer het CSV-bestand vervolgens naar de map:

    ```bash
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put <FILENAME>.csv /tutorials/flightdelays/data/
    ```

## <a name="transform-data-using-a-hive-query"></a>Gegevens transformeren met behulp van een Hive-query

Er zijn veel manieren om een Hive-taak uit te voeren in een HDInsight-cluster. In dit gedeelte gebruikt u [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline%E2%80%93CommandLineShell) om een Hive-taak uit te voeren. Zie [Hive in Apache HDInsight gebruiken](./hadoop/hdinsight-use-hive.md) voor meer informatie over de andere methoden voor het uitvoeren van een Hive-taak.

Als onderdeel van de Hive-taak, importeert u de gegevens uit het CSV-bestand naar een Hive-tabel met de naam **Delays**.

1. Gebruik op de SSH-prompt die al is geopend voor het HDInsight-cluster de volgende opdracht om een nieuw bestand met de naam **flightdelays.hql** te maken en bewerken:

    ```bash
    nano flightdelays.hql
    ```

2. Gebruik de volgende tekst als de inhoud van dit bestand:

    ```hiveql
    DROP TABLE delays_raw;
    -- Creates an external table over the csv file
    CREATE EXTERNAL TABLE delays_raw (
        YEAR string,
        FL_DATE string,
        UNIQUE_CARRIER string,
        CARRIER string,
        FL_NUM string,
        ORIGIN_AIRPORT_ID string,
        ORIGIN string,
        ORIGIN_CITY_NAME string,
        ORIGIN_CITY_NAME_TEMP string,
        ORIGIN_STATE_ABR string,
        DEST_AIRPORT_ID string,
        DEST string,
        DEST_CITY_NAME string,
        DEST_CITY_NAME_TEMP string,
        DEST_STATE_ABR string,
        DEP_DELAY_NEW float,
        ARR_DELAY_NEW float,
        CARRIER_DELAY float,
        WEATHER_DELAY float,
        NAS_DELAY float,
        SECURITY_DELAY float,
        LATE_AIRCRAFT_DELAY float)
    -- The following lines describe the format and location of the file
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE
    LOCATION '/tutorials/flightdelays/data';

    -- Drop the delays table if it exists
    DROP TABLE delays;
    -- Create the delays table and populate it with data
    -- pulled in from the CSV file (via the external table defined previously)
    CREATE TABLE delays AS
    SELECT YEAR AS year,
        FL_DATE AS flight_date,
        substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier,
        substring(CARRIER, 2, length(CARRIER) -1) AS carrier,
        substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num,
        ORIGIN_AIRPORT_ID AS origin_airport_id,
        substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code,
        substring(ORIGIN_CITY_NAME, 2) AS origin_city_name,
        substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr,
        DEST_AIRPORT_ID AS dest_airport_id,
        substring(DEST, 2, length(DEST) -1) AS dest_airport_code,
        substring(DEST_CITY_NAME,2) AS dest_city_name,
        substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr,
        DEP_DELAY_NEW AS dep_delay_new,
        ARR_DELAY_NEW AS arr_delay_new,
        CARRIER_DELAY AS carrier_delay,
        WEATHER_DELAY AS weather_delay,
        NAS_DELAY AS nas_delay,
        SECURITY_DELAY AS security_delay,
        LATE_AIRCRAFT_DELAY AS late_aircraft_delay
    FROM delays_raw;
    ```

2. Om het bestand op te slaan, drukt u op **Esc** en voert u vervolgens `:x` in.

3. Gebruik de volgende opdracht om Hive te starten en het bestand **flightdelays.hql** uit te voeren:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

4. Als het script __flightdelays.hql__ is voltooid, gebruikt u de volgende opdracht om een interactieve Beeline-sessie te openen:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. Wanneer u de prompt `jdbc:hive2://localhost:10001/>` ziet, gebruikt u de volgende query om gegevens op te halen uit de geïmporteerde gegevens van vertraagde vluchten:

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    Met deze query haalt u een lijst op met plaatsen waar er vertragingen door het weer zijn ontstaan, samen met de gemiddelde vertragingstijd. Deze gegevens worden opgeslagen in `/tutorials/flightdelays/output`. Later worden de gegevens met Sqoop vanaf deze locatie gelezen en geëxporteerd naar Azure SQL Database.

6. Sluit Beeline af door `!quit` in te voeren bij de opdrachtprompt.

## <a name="create-a-sql-database-table"></a>Een SQL-databasetabel maken

In dit gedeelte wordt ervan uitgegaan dat u al een Azure SQL-database hebt gemaakt. Als u nog geen SQL-database hebt, gebruikt u de informatie in [Een Azure SQL-database maken in Azure Portal](../sql-database/sql-database-get-started.md) om er een te maken.

Als u al een SQL-database hebt, moet u de naam van de server opvragen. Ga hiervoor naar [Azure Portal](https://portal.azure.com), selecteer **SQL-databases** en filter vervolgens op de naam van de database die u wilt gebruiken. De naam van de server wordt vermeld in de kolom **Server**.

![Gegevens van Azure SQL-server opvragen](./media/hdinsight-analyze-flight-delay-data-linux/get-azure-sql-server-details.png "Gegevens van Azure SQL-server opvragen")

> [!NOTE]  
> Er zijn veel manieren om verbinding te maken met SQL Database en een tabel te maken. In de volgende stappen wordt [FreeTDS](http://www.freetds.org/) gebruikt vanuit het HDInsight-cluster.


1. Gebruik de volgende opdracht vanuit een SSH-verbinding met het cluster om FreeTDS te installeren:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. Nadat de installatie is voltooid, gebruikt u de volgende opdracht om verbinding te maken met de SQL Database-server. Vervang **serverName** door de naam van de SQL Database-server. Vervang **adminLogin** en **adminPassword** door de aanmeldingsgegevens voor SQL Database. Vervang **databaseName** door de naam van de database.

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -p 1433 -D <databaseName>
    ```

    Voer desgevraagd het wachtwoord voor een beheerdersaccount voor SQL Database in.

    U ziet uitvoer die vergelijkbaar is met de volgende tekst:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. Voer de volgende regels in bij de prompt `1>`:

    ```hiveql
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    Wanneer u de instructie `GO` invoert, worden de vorige instructies geëvalueerd. Met deze query maakt u een tabel met de naam **delays**, met een geclusterde index.

    Gebruik de volgende query om te controleren of de tabel is gemaakt:

    ```hiveql
    SELECT * FROM information_schema.tables
    GO
    ```

    De uitvoer lijkt op het volgende:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo             delays        BASE TABLE
    ```

5. Voer `exit` als reactie op de prompt `1>` om het hulpprogramma tsql af te sluiten.

## <a name="export-data-to-sql-database-using-apache-sqoop"></a>Gegevens met Apache Sqoop exporteren naar een SQL-database

In de vorige gedeelten hebt u de getransformeerde gegevens op `/tutorials/flightdelays/output` gekopieerd. In dit gedeelte gebruikt u Sqoop om de gegevens in '/tutorials/flightdelays/output' te exporteren naar de tabel die u hebt gemaakt in Azure SQL Database. 

1. Gebruik de volgende opdracht om te controleren of Sqoop de SQL-database kan zien:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    Deze opdracht retourneert een lijst met databases, met inbegrip van de database waarin u eerder de tabel delays hebt gemaakt.

2. Gebruik de volgende opdracht om gegevens uit hivesampletable te exporteren naar de tabel delays:

    ```bash
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop maakt verbinding met de database met daarin de tabel delays en exporteert gegevens uit de map `/tutorials/flightdelays/output` naar de tabel delays.

3. Als de sqoop-opdracht is voltooid, gebruikt u het hulpprogramma tsql om verbinding te maken met de database:

    ```bash
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    Gebruik de volgende instructies om te controleren of de gegevens zijn geëxporteerd naar de tabel delays:

    ```sql
    SELECT * FROM delays
    GO
    ```

    U ziet als het goed is een lijst met gegevens in de tabel. De tabel bevat de plaatsnaam en de gemiddelde vertragingstijd voor vluchten van en naar die plaats. 

    Typ `exit` om het hulpprogramma tsql af te sluiten.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u gegevens kunt extraheren, transformeren en laden met behulp van een Apache Hadoop-cluster in HDInsight. Ga verder met de volgende zelfstudie als u wilt weten hoe u op aanvraag Hadoop-clusters van HDInsight maakt met Azure Data Factory.

> [!div class="nextstepaction"]
>[Apache Hadoop-clusters op aanvraag maken in HDInsight met behulp van Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md)

Zie de volgende artikelen voor andere manieren om te werken met gegevens in HDInsight:

* [Zelfstudie: Gegevens extraheren, transformeren en laden met Apache Hive in Azure HDInsight](../storage/data-lake-storage/tutorial-extract-transform-load-hive.md)
* [Apache Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Apache Pig gebruiken met HDInsight][hdinsight-use-pig]
* [Java MapReduce-programma's ontwikkelen voor Apache Hadoop in HDInsight][hdinsight-develop-mapreduce]
* [Streaming MapReduce-programma's ontwikkelen met Python voor HDInsight][hdinsight-develop-streaming]
* [Apache Oozie gebruiken met HDInsight][hdinsight-use-oozie]
* [Apache Sqoop gebruiken met HDInsight][hdinsight-use-sqoop]



[azure-purchase-options]: https://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: https://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: https://azure.microsoft.com/pricing/free-trial/


[rita-website]: https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: https://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[hdinsight-use-oozie]: hdinsight-use-oozie-linux-mac.md
[hdinsight-use-hive]:hadoop/hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hadoop/apache-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]:hadoop/apache-hadoop-use-sqoop-mac-linux.md
[hdinsight-use-pig]:hadoop/hdinsight-use-pig.md
[hdinsight-develop-streaming]:hadoop/apache-hadoop-streaming-python.md
[hdinsight-develop-mapreduce]:hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL

[technetwiki-hive-error]: https://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
