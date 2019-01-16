---
title: 'Zelfstudie: ETL-bewerkingen (extraheren, transformeren, laden) uitvoeren met Apache Hive in Azure HDInsight'
description: In deze zelfstudie leert u hoe u gegevens uit een set met onbewerkte CSV-gegevens extraheert, de gegevens met Apache Hive in Azure HDInsight transformeert en de getransformeerde gegevens ten slotte laadt in Azure SQL Database met behulp van Sqoop.
services: storage
author: jamesbak
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 01/07/2019
ms.author: jamesbak
ms.openlocfilehash: 65d2d69c788a54371664d1a443a79bd121332470
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54105148"
---
# <a name="tutorial-extract-transform-and-load-data-by-using-apache-hive-on-azure-hdinsight"></a>Zelfstudie: Gegevens extraheren, transformeren en laden met Apache Hive in Azure HDInsight

In deze zelfstudie voert u een ETL-bewerking uit: gegevens extraheren, transformeren en laden. U neemt een CSV-bestand met onbewerkte gegevens, importeert deze in een Azure HDInsight-cluster, transformeert deze met Apache Hive en laadt de gegevens in een Azure SQL-database met Apache Sqoop.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Gegevens extraheren en uploaden naar een HDInsight-cluster.
> * De gegevens transformeren met behulp van Apache Hive.
> * De gegevens laden in een Azure SQL-database met behulp van Sqoop.

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **Een Hadoop-cluster op basis van Linux in HDInsight**: Zie [Clusters instellen in HDInsight met Hadoop, Spark, Kafka en meer](./data-lake-storage-quickstart-create-connect-hdi-cluster.md) voor instructies voor het maken van een nieuw HDInsight-cluster op basis van Linux.

* **Azure SQL Database**: U gebruikt een Azure SQL-database als doelgegevensopslag. Als u geen SQL-database hebt, raadpleegt u [Een Azure SQL-database maken in Azure Portal](../../sql-database/sql-database-get-started.md).

* **Azure CLI**: Als u de Azure CLI nog niet hebt geïnstalleerd, raadpleegt u [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

* **Een SSH-client**: Zie voor meer informatie [Verbinding maken met HDInsight (Hadoop) via SSH](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

> [!IMPORTANT]
> Voor de stappen in dit artikel hebt u een HDInsight-cluster nodig dat werkt met Linux. Linux is het enige besturingssysteem dat wordt gebruikt in Azure HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

### <a name="download-the-flight-data"></a>De vluchtgegevens downloaden

In deze zelfstudie worden vluchtgegevens van het Bureau of Transport Statistics gebruikt om te laten zien hoe u een ETL-bewerking kunt uitvoeren. U moet deze gegevens downloaden om de zelfstudie te kunnen voltooien.

1. Ga naar [Research and Innovative Technology Administration, Bureau of Transportation Statistics](https://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time).

1. Selecteer de volgende waarden op de pagina:

   | Naam | Waarde |
   | --- | --- |
   | **Filter Year** |2013 |
   | **Filter Period** |Januari |
   | **Fields** |Year, FlightDate, UniqueCarrier, Carrier, FlightNum, OriginAirportID, Origin, OriginCityName, OriginState, DestAirportID, Dest, DestCityName, DestState, DepDelayMinutes, ArrDelay, ArrDelayMinutes, CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay. |

1. Wis alle andere velden.

1. Selecteer **Download**. U krijgt een ZIP-bestand met de gegevensvelden die u hebt geselecteerd.

## <a name="extract-and-upload-the-data"></a>De gegevens extraheren en uploaden

In deze sectie gebruikt u `scp` om gegevens naar uw HDInsight-cluster te uploaden.

Open een opdrachtprompt en gebruik de volgende opdracht om het ZIP-bestand te uploaden naar het hoofdknooppunt van het HDInsight-cluster:

```bash
scp <FILE_NAME>.zip <SSH_USER_NAME>@<CLUSTER_NAME>-ssh.azurehdinsight.net:<FILE_NAME.zip>
```

* Vervang \<FILE_NAME> door de naam van het ZIP-bestand.
* Vervang \<SSH_USER_NAME> door de SSH-aanmelding voor het HDInsight-cluster.
* Vervang \<CLUSTER_NAME> door de naam van het HDInsight-cluster.

Als u een wachtwoord gebruikt om uw SSH-aanmelding te verifiëren, wordt u gevraagd om het wachtwoord. 

Als u een openbare sleutel gebruikt, moet u mogelijk de parameter `-i` gebruiken en het pad naar de bijbehorende persoonlijke sleutel opgeven. Bijvoorbeeld `scp -i ~/.ssh/id_rsa FILE_NAME.zip USER_NAME@CLUSTER_NAME-ssh.azurehdinsight.net:`.

Nadat het uploaden is voltooid, maakt u via SSH verbinding met het cluster. Voer op de opdrachtprompt de volgende opdracht in:

```bash
ssh <SSH_USER_NAME>@<CLUSTER_NAME>-ssh.azurehdinsight.net
```

Gebruik de volgende opdracht om ZIP-bestand uit te pakken:

```bash
unzip <FILE_NAME>.zip
```

Met deze opdracht wordt een **CSV**-bestand uitgepakt van ongeveer 60 MB.

Gebruik de volgende opdrachten om een map te maken en kopieer het *CSV*-bestand naar de map:

```bash
hdfs dfs -mkdir -p abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data
hdfs dfs -put <FILE_NAME>.csv abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data/
```

Gebruik de volgende opdracht om het Data Lake Storage Gen2-bestandssysteem te maken.

```bash
hadoop fs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/
```

## <a name="transform-the-data"></a>De gegevens transformeren

In deze sectie gebruikt u Beeline om een Apache Hive-taak uit te voeren.

Als onderdeel van de Apache Hive-taak, importeert u de gegevens uit het CSV-bestand in een Apache Hive-tabel met de naam **delays**.

Gebruik op de SSH-prompt die al is geopend voor het HDInsight-cluster de volgende opdracht om een nieuw bestand met de naam **flightdelays.hql** te maken en bewerken:

```bash
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
 LOCATION 'abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/data';

-- Drop the delays table if it exists
DROP TABLE delays;
-- Create the delays table and populate it with data
-- pulled in from the CSV file (via the external table defined previously)
CREATE TABLE delays
LOCATION abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/processed
AS
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

Druk op de Esc-toets en typ `:x` om het bestand op te slaan.

Gebruik de volgende opdracht om Hive te starten en het bestand **flightdelays.hql** uit te voeren:

```bash
beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -f flightdelays.hql
```

Als het script __flightdelays.hql__ is voltooid, gebruikt u de volgende opdracht om een interactieve Beeline-sessie te openen:

```bash
beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http'
```

Wanneer u de prompt `jdbc:hive2://localhost:10001/>` ziet, gebruikt u de volgende query om gegevens op te halen uit de geïmporteerde gegevens van vertraagde vluchten:

```hiveql
INSERT OVERWRITE DIRECTORY 'abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output'
ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
SELECT regexp_replace(origin_city_name, '''', ''),
    avg(weather_delay)
FROM delays
WHERE weather_delay IS NOT NULL
GROUP BY origin_city_name;
```

Met deze query haalt u een lijst op met plaatsen waar er vertragingen door het weer zijn ontstaan, samen met de gemiddelde vertragingstijd. Deze gegevens worden opgeslagen in `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output`. Later worden de gegevens met Sqoop vanaf deze locatie gelezen en geëxporteerd naar Azure SQL Database.

Sluit Beeline af door `!quit` in te voeren bij de opdrachtprompt.

## <a name="create-a-sql-database-table"></a>Een SQL-databasetabel maken

Voor deze bewerking hebt u de servernaam van uw SQL-database nodig. Voer deze stappen uit om de servernaam te achterhalen.

1. Ga naar de [Azure Portal](https://portal.azure.com).

1. Selecteer **SQL-databases**.

1. Filter op de naam van de database die u wilt kiezen. De naam van de server wordt vermeld in de kolom **Server**.

1. Filter op de naam van de database die u wilt gebruiken. De naam van de server wordt vermeld in de kolom **Server**.

    ![Gegevens van Azure SQL-server opvragen](./media/data-lake-storage-tutorial-extract-transform-load-hive/get-azure-sql-server-details.png "Gegevens van Azure SQL-server opvragen")

    Er zijn veel manieren om verbinding te maken met SQL Database en een tabel te maken. In de volgende stappen wordt [FreeTDS](http://www.freetds.org/) gebruikt vanuit het HDInsight-cluster.

Gebruik de volgende opdracht vanuit een SSH-verbinding met het cluster om FreeTDS te installeren:

```bash
sudo apt-get --assume-yes install freetds-dev freetds-bin
```

Nadat de installatie is voltooid, gebruikt u de volgende opdracht om verbinding te maken met de SQL Database-server.

* Vervang \<SERVER_NAME> door de naam van de SQL Database-server.
* Vervang \<ADMIN_LOGIN> door de aanmeldingsgegevens van de SQL-Database-beheerder.
* Vervang \<DATABASE_NAME> door de naam van de database.

```bash
TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -p 1433 -D <DATABASE_NAME>
```

Voer desgevraagd het wachtwoord voor de SQL Database-beheerder in.

U ziet uitvoer die vergelijkbaar is met de volgende tekst:

```
locale is "en_US.UTF-8"
locale charset is "UTF-8"
using default charset "UTF-8"
Default database being set to sqooptest
1>
```

Voer de volgende instructies in bij de prompt `1>`:

```hiveql
CREATE TABLE [dbo].[delays](
[origin_city_name] [nvarchar](50) NOT NULL,
[weather_delay] float,
CONSTRAINT [PK_delays] PRIMARY KEY CLUSTERED   
([origin_city_name] ASC))
GO
```

Wanneer u de instructie `GO` invoert, worden de vorige instructies geëvalueerd.
Met de query maakt u een tabel met de naam **delays**, met een geclusterde index.

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

Voer `exit` als reactie op de prompt `1>` om het hulpprogramma tsql af te sluiten.

## <a name="export-and-load-the-data"></a>De gegevens exporteren en laden

In de vorige secties hebt u de getransformeerde gegevens op de locatie `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output` gekopieerd. In deze sectie gebruikt u Sqoop om de gegevens in `abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/tutorials/flightdelays/output` te exporteren naar de tabel die u hebt gemaakt in Azure SQL Database.

Gebruik de volgende opdracht om te controleren of Sqoop de SQL-database kan zien:

```bash
sqoop list-databases --connect jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433 --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD>
```

De opdracht retourneert een lijst met databases, met inbegrip van de database waarin u de tabel **delays** hebt gemaakt.

Gebruik de volgende opdracht om gegevens uit de tabel **hivesampletable** te exporteren naar de tabel **delays**:

```bash
sqoop export --connect 'jdbc:sqlserver://<SERVER_NAME>.database.windows.net:1433;database=<DATABASE_NAME>' --username <ADMIN_LOGIN> --password <ADMIN_PASSWORD> --table 'delays' --export-dir 'abfs://<FILE_SYSTEM_NAME>@.dfs.core.windows.net/tutorials/flightdelays/output' --fields-terminated-by '\t' -m 1
```

Sqoop maakt verbinding met de database met de tabel **delays** en exporteert gegevens uit de map `/tutorials/flightdelays/output` naar de tabel **delays**.

Als de `sqoop`-opdracht is voltooid, gebruikt u het hulpprogramma tsql om verbinding te maken met de database:

```bash
TDSVER=8.0 tsql -H <SERVER_NAME>.database.windows.net -U <ADMIN_LOGIN> -P <ADMIN_PASSWORD> -p 1433 -D <DATABASE_NAME>
```

Gebruik de volgende instructies om te controleren of de gegevens zijn geëxporteerd naar de tabel **delays**:

```sql
SELECT * FROM delays
GO
```

U ziet als het goed is een lijst met gegevens in de tabel. De tabel bevat de plaatsnaam en de gemiddelde vertragingstijd voor vluchten van en naar die plaats.

Typ `exit` en druk op Enter om het hulpprogramma tsql af te sluiten.

## <a name="clean-up-resources"></a>Resources opschonen

Alle resources die in deze zelfstudie worden gebruikt, zijn bestaande resources. Opschonen is niet nodig.

## <a name="next-steps"></a>Volgende stappen

Zie het volgende artikel voor andere manieren om te werken met gegevens in HDInsight:

> [!div class="nextstepaction"]
> [Gegevens extraheren, transformeren en laden met Azure Databricks](./data-lake-storage-use-hdi-cluster.md)
