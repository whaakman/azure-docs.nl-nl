---
title: Vlucht vertraging gegevens analyseren met Hive in HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van Hive vluchtgegevens op Linux gebaseerde HDInsight analyseren en de gegevens vervolgens exporteren naar SQL-Database met behulp van Sqoop.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 0c23a079-981a-4079-b3f7-ad147b4609e5
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: b2eca1ab7eff006311269c78b1e507cb1417fcc6
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="analyze-flight-delay-data-by-using-hive-on-linux-based-hdinsight"></a>Vertraging vluchtgegevens analyseren met behulp van Hive in HDInsight op basis van Linux

Informatie over hoe vlucht vertraging om gegevens te analyseren met behulp van Hive in HDInsight op basis van Linux en hoe de gegevens exporteren naar Azure SQL Database met behulp van Sqoop.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem gebruikt op Azure HDInsight versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="prerequisites"></a>Vereisten

* **Een HDInsight-cluster**. Zie [aan de slag met Hadoop in HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md) voor stapsgewijze instructies voor het maken van een nieuwe Linux gebaseerde HDInsight-cluster.

* **Azure SQL-database**. U kunt een Azure SQL database gebruiken als een doelgegevensopslagplaats. Als u een SQL-database geen hebt, raadpleegt u [maken van een Azure SQL database in de Azure portal](../sql-database/sql-database-get-started.md).

* **Azure CLI**. Als u de Azure CLI nog niet hebt geïnstalleerd, Zie [installeren van de Azure CLI 1.0](../cli-install-nodejs.md) voor meer stappen.

## <a name="download-the-flight-data"></a>De vluchtgegevens downloaden

1. Blader naar [onderzoek en beheer van innovatieve technologie Bureau vervoer statistieken][rita-website].

2. Selecteer op de pagina de volgende waarden:

   | Naam | Waarde |
   | --- | --- |
   | Filteren van jaar |2013 |
   | Periode filteren |januari |
   | Velden |Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |
   Schakel alle andere velden. 

3. Selecteer **downloaden**.

## <a name="upload-the-data"></a>De gegevens uploaden

1. Gebruik de volgende opdracht voor het uploaden van het ZIP-bestand met het hoofdknooppunt van het HDInsight-cluster:

    ```
    scp FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
    ```

    Vervang *FILENAME* met de naam van het ZIP-bestand. Vervang *gebruikersnaam* met de SSH-aanmelding voor het HDInsight-cluster. Vervang *CLUSTERNAME* met de naam van het HDInsight-cluster.

   > [!NOTE]
   > Als u een wachtwoord gebruikt voor verificatie van uw SSH-aanmelding, wordt u gevraagd om het wachtwoord. Als u een openbare sleutel gebruikt, moet u mogelijk gebruik van de `-i` parameter en het pad naar de overeenkomende persoonlijke sleutel opgeven. Bijvoorbeeld `scp -i ~/.ssh/id_rsa FILENAME.zip USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:`.

2. Nadat het uploaden is voltooid, kunt u verbinding met het cluster via SSH:

    ```ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net```

    Zie voor meer informatie [Verbinding maken met HDInsight (Hadoop) via SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

3. Gebruik de volgende opdracht voor het uitpakken van het ZIP-bestand:

    ```
    unzip FILENAME.zip
    ```

    Met deze opdracht wordt een CSV-bestand dat is ongeveer 60 MB.

4. Gebruik de volgende opdracht in een map op HDInsight-opslag maken en kopieer het bestand naar de map:

    ```
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="create-and-run-the-hiveql"></a>Maken en uitvoeren van de HiveQL

Gebruik de volgende stappen om gegevens uit het CSV-bestand importeren in een Hive-tabel met de naam **vertragingen**.

1. Gebruik de volgende opdracht maken en bewerken van een nieuw bestand met de naam **flightdelays.hql**:

    ```
    nano flightdelays.hql
    ```

    Gebruik de volgende tekst als de inhoud van dit bestand:

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

2. Gebruik Ctrl + X, dan Y voor het opslaan van het bestand.

3. Starten van Hive en voer de **flightdelays.hql** bestand, gebruikt u de volgende opdracht:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

4. Na de __flightdelays.hql__ is voltooid met een script, gebruikt u de volgende opdracht een interactieve Beeline-sessie te openen:

    ```
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

5. Wanneer u ontvangt de `jdbc:hive2://localhost:10001/>` gevraagd, gebruikt u de volgende query gegevens ophalen van de geïmporteerde vluchtgegevens vertraging:

    ```hiveql
    INSERT OVERWRITE DIRECTORY '/tutorials/flightdelays/output'
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    SELECT regexp_replace(origin_city_name, '''', ''),
        avg(weather_delay)
    FROM delays
    WHERE weather_delay IS NOT NULL
    GROUP BY origin_city_name;
    ```

    Deze query haalt u een lijst met plaatsen die ervaren weer, samen met de gemiddelde vertragingstijd een vertraging en wordt opgeslagen in `/tutorials/flightdelays/output`. Later, Sqoop leest de gegevens vanaf deze locatie en exporteert het naar Azure SQL Database.

6. Voer om af te sluiten Beeline `!quit` bij de opdrachtprompt.

## <a name="create-a-sql-database"></a>Een SQL-database maken

Als u al een SQL-database, moet u de naam van de server ophalen. Vinden van de naam van de server in de [Azure-portal](https://portal.azure.com), selecteer **SQL-Databases**, en filter vervolgens op de naam van de database die u wilt gebruiken. Naam van de server wordt weergegeven in de **SERVER** kolom.

Als u een SQL-database nog geen hebt, gebruikt u de informatie in [maken van een Azure SQL database in de Azure portal](../sql-database/sql-database-get-started.md) een maken. De servernaam van de die wordt gebruikt voor de database opgeslagen.

## <a name="create-a-sql-database-table"></a>Een SQL-databasetabel maken

> [!NOTE]
> Er zijn veel manieren om te verbinden met SQL Database en een tabel maken. De volgende stappen uitvoeren om [FreeTDS](http://www.freetds.org/) van het HDInsight-cluster.


1. Als u wilt installeren FreeTDS, gebruik de volgende opdracht vanaf een SSH-verbinding met het cluster:

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

3. Nadat de installatie is voltooid, moet u de volgende opdracht gebruiken om te verbinden met de SQL-Database-server. Vervang **serverName** met de naam van de SQL-Database-server. Vervang **adminLogin** en **adminPassword** met de aanmelding voor SQL-Database. Vervang **databaseName** met de naam van de database.

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -p 1433 -D <databaseName>
    ```

    Voer desgevraagd het wachtwoord voor de aanmelding van SQL Database-beheerder.

    U ontvangt uitvoer die vergelijkbaar is met de volgende tekst:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to sqooptest
    1>
    ```

4. Op de `1>` vragen, voer de volgende regels:

    ```
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
    ([origin_city_name] ASC))
    GO
    ```

    Wanneer de `GO` instructie wordt ingevoerd, worden de vorige instructies geëvalueerd. Deze query maakt een tabel met de naam **vertragingen**, met een geclusterde index.

    Gebruik de volgende query om te controleren of de tabel is gemaakt:

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    De uitvoer lijkt op het volgende:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    databaseName       dbo     delays      BASE TABLE
    ```

5. Voer `exit` op de `1>` vragen om af te sluiten van de tsql-hulpprogramma.

## <a name="export-data-with-sqoop"></a>Gegevens exporteren met Sqoop

1. Gebruik de volgende opdracht om te controleren of de SQL-database door Sqoop zien kan:

    ```
    sqoop list-databases --connect jdbc:sqlserver://<serverName>.database.windows.net:1433 --username <adminLogin> --password <adminPassword>
    ```

    Deze opdracht retourneert een lijst met databases, met inbegrip van de database waarin u eerder hebt gemaakt in de tabel vertragingen.

2. De volgende opdracht gebruiken om gegevens te exporteren uit hivesampletable aan de tabel vertragingen:

    ```
    sqoop export --connect 'jdbc:sqlserver://<serverName>.database.windows.net:1433;database=<databaseName>' --username <adminLogin> --password <adminPassword> --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop verbindt met de database die de tabel vertragingen bevat en exporteert u gegevens uit de `/tutorials/flightdelays/output` map naar de tabel vertragingen.

3. Nadat de sqoop-opdracht is voltooid, moet u de tsql-hulpprogramma gebruiken om te verbinden met de database:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D <databaseName>
    ```

    Gebruik de volgende instructies om te controleren dat de gegevens zijn geëxporteerd naar de tabel vertragingen:

    ```
    SELECT * FROM delays
    GO
    ```

    U ziet een lijst met gegevens in de tabel. Type `exit` om af te sluiten van de tsql-hulpprogramma.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer manieren om te werken met gegevens in HDInsight meer de volgende artikelen:

* [Hive gebruiken met HDInsight][hdinsight-use-hive]
* [Oozie gebruiken met HDInsight][hdinsight-use-oozie]
* [Sqoop gebruiken met HDInsight][hdinsight-use-sqoop]
* [Pig gebruiken met HDInsight][hdinsight-use-pig]
* [Het ontwikkelen van Java-MapReduce-programma's voor Hadoop in HDInsight][hdinsight-develop-mapreduce]
* [Python streaming MapReduce-programma's voor HDInsight ontwikkelen][hdinsight-develop-streaming]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

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

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
