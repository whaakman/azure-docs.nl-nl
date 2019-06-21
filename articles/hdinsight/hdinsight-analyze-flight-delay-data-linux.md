---
title: 'Zelfstudie: ETL-bewerkingen (extraheren, transformeren, laden) uitvoeren met Hive in HDInsight - Azure '
description: 'Zelfstudie: Lees hoe u gegevens ophalen uit een gegevensset met onbewerkte CSV, transformeren met behulp van Hive in HDInsight en vervolgens de getransformeerde gegevens laden in Azure SQL-database met behulp van Apache Sqoop.'
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: tutorial
ms.date: 05/15/2019
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive,mvc
ms.openlocfilehash: 6eae1a64b6eba54fd35f8371a11e7b5bfeaac2ab
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67296528"
---
# <a name="tutorial-extract-transform-and-load-data-using-apache-hive-in-azure-hdinsight"></a>Zelfstudie: Uitpakken, transformeren en laden van gegevens met behulp van Apache Hive in Azure HDInsight

In deze zelfstudie een onbewerkte CSV-gegevensbestand van vluchtgegevens van openbaar beschikbare nemen, importeren in de opslag van een HDInsight-cluster en transformeert u de gegevens met [Apache Hive](https://hive.apache.org/) in Azure HDInsight. Als de gegevens zijn getransformeerd, laadt u die gegevens met behulp van [Apache Sqoop](https://sqoop.apache.org/) in een Azure SQL-database.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * De voorbeeldbestanden met vluchtgegevens downloaden
> * Gegevens uploaden naar een HDInsight-cluster
> * De gegevens transformeren met Hive
> * Een tabel maken in een Azure SQL database
> * Sqoop gebruiken om gegevens te exporteren naar een Azure SQL database

In de volgende afbeelding ziet u een typische werkstroom van een ETL-toepassing.

![ETL-bewerking met Apache Hive in Azure HDInsight](./media/hdinsight-analyze-flight-delay-data-linux/hdinsight-etl-architecture.png "ETL-bewerking met Apache Hive in Azure HDInsight")

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Een Apache Hadoop-cluster in HDInsight. Zie [aan de slag met HDInsight op Linux](hadoop/apache-hadoop-linux-tutorial-get-started.md).

* An Azure SQL Database. U gebruikt een Azure SQL-database als doelgegevensopslag. Als u geen SQL-database hebt, raadpleegt u [Een Azure SQL-database maken in Azure Portal](../sql-database/sql-database-single-database-get-started.md).

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="download-the-flight-data"></a>De vluchtgegevens downloaden

1. Blader naar [Research and Innovative Technology Administration, Bureau of Transportation Statistics](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time).

2. Op de pagina, schakel alle velden en selecteer vervolgens de volgende waarden:

   | Name | Waarde |
   | --- | --- |
   | Filterjaar |2019 |
   | Filterperiode |Januari |
   | Velden |Jaar, FlightDate, Reporting_Airline, DOT_ID_Reporting_Airline, Flight_Number_Reporting_Airline, OriginAirportID, oorsprong, OriginCityName, OriginState, DestAirportID, doel, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |

3. Selecteer **Download**. U krijgt een ZIP-bestand met de gegevensvelden die u hebt geselecteerd.

## <a name="upload-data-to-an-hdinsight-cluster"></a>Gegevens uploaden naar een HDInsight-cluster

Er zijn veel manieren om gegevens te uploaden naar de opslag die is gekoppeld aan een HDInsight-cluster. In dit gedeelte gebruikt u `scp` om gegevens te uploaden. Zie [Gegevens uploaden naar HDInsight](hdinsight-upload-data.md) voor informatie over andere manieren om gegevens te uploaden.

1. Upload het ZIP-bestand naar het hoofdknooppunt van HDInsight-cluster. De onderstaande opdracht bewerken door te vervangen `FILENAME` met de naam van het ZIP-bestand en `CLUSTERNAME` met de naam van het HDInsight-cluster. Open een opdrachtprompt, instellen van uw werkmap naar de locatie van het bestand en voer vervolgens de opdracht.

    ```cmd
    scp FILENAME.zip sshuser@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.zip
    ```

2. Nadat het uploaden is voltooid, maakt u via SSH verbinding met het cluster. De onderstaande opdracht bewerken door te vervangen `CLUSTERNAME` met de naam van het HDInsight-cluster. Voer vervolgens de volgende opdracht in:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

3. Omgevingsvariabele instellen wanneer een SSH-verbinding tot stand is gebracht. Vervang `FILE_NAME`, `SQL_SERVERNAME`, `SQL_DATABASE`, `SQL_USER`, en `SQL_PASWORD` met de juiste waarden. Voer de opdracht:

    ```bash
    export FILENAME=FILE_NAME
    export SQLSERVERNAME=SQL_SERVERNAME
    export DATABASE=SQL_DATABASE
    export SQLUSER=SQL_USER
    export SQLPASWORD='SQL_PASWORD'
    ```

4. Pak het ZIP-bestand met de onderstaande opdracht:

    ```bash
    unzip $FILENAME.zip
    ```

5. Maak een map op HDInsight-opslag, en vervolgens het CSV-bestand kopiëren naar de map met de onderstaande opdracht:

    ```bash
    hdfs dfs -mkdir -p /tutorials/flightdelays/data
    hdfs dfs -put $FILENAME.csv /tutorials/flightdelays/data/
    ```

## <a name="transform-data-using-a-hive-query"></a>Gegevens transformeren met behulp van een Hive-query

Er zijn veel manieren om een Hive-taak uit te voeren in een HDInsight-cluster. In dit gedeelte gebruikt u [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline%E2%80%93CommandLineShell) om een Hive-taak uit te voeren. Zie [Hive in Apache HDInsight gebruiken](./hadoop/hdinsight-use-hive.md) voor meer informatie over de andere methoden voor het uitvoeren van een Hive-taak.

Als onderdeel van de Hive-taak, importeert u de gegevens uit het CSV-bestand naar een Hive-tabel met de naam **Delays**.

1. De SSH-opdrachtprompt die u al voor het HDInsight-cluster hebt en gebruik de volgende opdracht uit om te maken en bewerken van een nieuw bestand met de naam **flightdelays.hql**:

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

3. Om het bestand hebt opgeslagen, drukt u op **Ctrl + X**, klikt u vervolgens **y**, voert u.

4. Gebruik de volgende opdracht om Hive te starten en het bestand **flightdelays.hql** uit te voeren:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
    ```

5. Als het script **flightdelays.hql** is voltooid, gebruikt u de volgende opdracht om een interactieve Beeline-sessie te openen:

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
    ```

6. Wanneer u de prompt `jdbc:hive2://localhost:10001/>` ziet, gebruikt u de volgende query om gegevens op te halen uit de geïmporteerde gegevens van vertraagde vluchten:

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

7. Sluit Beeline af door `!quit` in te voeren bij de opdrachtprompt.

## <a name="create-a-sql-database-table"></a>Een SQL-databasetabel maken

Er zijn veel manieren om verbinding te maken met SQL Database en een tabel te maken. In de volgende stappen wordt [FreeTDS](http://www.freetds.org/) gebruikt vanuit het HDInsight-cluster.

1. Als u wilt installeren FreeTDS, gebruik de volgende opdracht uit de open SSH-verbinding met het cluster:

    ```bash
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. Nadat de installatie is voltooid, gebruikt u de volgende opdracht om verbinding te maken met de SQL Database-server.

    ```bash
    TDSVER=8.0 tsql -H $SQLSERVERNAME.database.windows.net -U $SQLUSER -p 1433 -D $DATABASE -P $SQLPASWORD
    ```

    U ziet uitvoer die vergelijkbaar is met de volgende tekst:

    ```
    locale is "en_US.UTF-8"
    locale charset is "UTF-8"
    using default charset "UTF-8"
    Default database being set to <yourdatabase>
    1>
    ```

3. Voer de volgende regels in bij de prompt `1>`:

    ```hiveql
    CREATE TABLE [dbo].[delays](
    [origin_city_name] [nvarchar](50) NOT NULL,
    [weather_delay] float,
    CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED
    ([origin_city_name] ASC))
    GO
    ```

    Wanneer u de instructie `GO` invoert, worden de vorige instructies geëvalueerd. Deze instructie maakt een tabel met de naam **vertragingen**, met een geclusterde index.

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

4. Voer `exit` als reactie op de prompt `1>` om het hulpprogramma tsql af te sluiten.

## <a name="export-data-to-sql-database-using-apache-sqoop"></a>Gegevens met Apache Sqoop exporteren naar een SQL-database

In de vorige gedeelten hebt u de getransformeerde gegevens op `/tutorials/flightdelays/output` gekopieerd. In deze sectie gebruikt u Sqoop om de gegevens in `/tutorials/flightdelays/output` te exporteren naar de tabel die u hebt gemaakt in Azure SQL-database.

1. Controleren of Sqoop uw SQL-database zien kan door te voeren van de volgende opdracht:

    ```bash
    sqoop list-databases --connect jdbc:sqlserver://$SQLSERVERNAME.database.windows.net:1433 --username $SQLUSER --password $SQLPASWORD
    ```

    Met deze opdracht retourneert een lijst van databases, met inbegrip van de database waarin u gemaakt de `delays` eerdere tabel.

2. Gegevens exporteren uit `/tutorials/flightdelays/output` naar de `delays` tabel met de onderstaande opdracht:

    ```bash
    sqoop export --connect "jdbc:sqlserver://$SQLSERVERNAME.database.windows.net:1433;database=$DATABASE" --username $SQLUSER --password $SQLPASWORD --table 'delays' --export-dir '/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
    ```

    Sqoop maakt verbinding met de database bevat de `delays` tabel en exporteert u gegevens uit de `/tutorials/flightdelays/output` map naar de `delays` tabel.

3. Nadat de sqoop-opdracht is voltooid, moet u de tsql-hulpprogramma gebruiken om te verbinden met de database met de onderstaande opdracht:

    ```bash
    TDSVER=8.0 tsql -H $SQLSERVERNAME.database.windows.net -U $SQLUSER -p 1433 -D $DATABASE -P $SQLPASWORD
    ```

    Gebruik de volgende instructies om te controleren of de gegevens zijn geëxporteerd naar de tabel delays:

    ```sql
    SELECT * FROM delays
    GO
    ```

    U ziet als het goed is een lijst met gegevens in de tabel. De tabel bevat de plaatsnaam en de gemiddelde vertragingstijd voor vluchten van en naar die plaats. 

    Typ `exit` om het hulpprogramma tsql af te sluiten.

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de zelfstudie hebt voltooid, kunt u het cluster verwijderen. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Als u wilt verwijderen van een cluster, Zie [verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI](./hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een onbewerkte duurde CSV-gegevensbestand importeren in de opslag van een HDInsight-cluster en vervolgens de gegevens met Apache Hive in Azure HDInsight getransformeerd.  Ga verder met de volgende zelfstudie als u wilt weten hoe u op aanvraag Hadoop-clusters van HDInsight maakt met Azure Data Factory.

> [!div class="nextstepaction"]
>[Apache Hadoop-clusters op aanvraag maken in HDInsight met behulp van Azure Data Factory](hdinsight-hadoop-create-linux-clusters-adf.md)

Zie de volgende artikelen voor andere manieren om te werken met gegevens in HDInsight:

* [Zelfstudie: Gegevens extraheren, transformeren en laden met Apache Hive in Azure HDInsight](../storage/blobs/data-lake-storage-tutorial-extract-transform-load-hive.md)
* [Apache Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Java MapReduce-programma's ontwikkelen voor Apache Hadoop op HDInsight](hadoop/apache-hadoop-develop-deploy-java-mapreduce-linux.md)

* [Apache Oozie gebruiken met HDInsight](hdinsight-use-oozie-linux-mac.md)
* [Apache Sqoop gebruiken met HDInsight](hadoop/apache-hadoop-use-sqoop-mac-linux.md)