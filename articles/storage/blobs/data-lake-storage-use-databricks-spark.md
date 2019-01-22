---
title: 'Zelfstudie: Toegang tot Azure Data Lake Storage Gen2 Preview-gegevens met Azure Databricks met behulp van Spark | Microsoft Docs'
description: Deze zelfstudie bevat informatie over het uitvoeren van Spark-query's in een Azure Databricks-cluster voor toegang tot gegevens in een Azure Data Lake Storage Gen2-opslagaccount.
services: storage
author: dineshmurthy
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: tutorial
ms.date: 01/14/2019
ms.author: dineshm
ms.openlocfilehash: e72a4f71a42a892d14fad076b124426f0c32ac7d
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54321803"
---
# <a name="tutorial-access-data-lake-storage-gen2-preview-data-with-azure-databricks-using-spark"></a>Zelfstudie: Toegang tot Data Lake Storage Gen2 Preview-gegevens met Azure Databricks met behulp van Spark

In deze zelfstudie ziet u hoe u een Azure Databricks-cluster kunt verbinden met gegevens die zijn opgeslagen in een Azure-opslagaccount waarvoor Azure Data Lake Storage Gen2 (preview) is ingeschakeld. Deze verbinding stelt u in staat om systeemeigen query’s en analyses van uw cluster uit te voeren op uw gegevens.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Databricks-cluster maken
> * Niet-gestructureerde gegevens opnemen in een opslagaccount
> * Analyse uitvoeren op gegevens in Blob-opslag

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt getoond hoe u de vluchtgegevens kunt gebruiken en doorzoeken die beschikbaar zijn via de [United States Department of Transportation](https://transtats.bts.gov/DL_SelectFields.asp). 

1. Schakel het selectievakje **Prezipped file** in om alle gegevensvelden te selecteren.
2. Selecteer **Downloaden** en sla de resultaten op de computer op.
3. Noteer de bestandsnaam en het pad voor de download. Deze gegevens hebt u nodig in een latere stap.

U hebt een opslagaccount met analytische mogelijkheden nodig om deze zelfstudie te voltooien. We raden u aan om onze [quickstart](data-lake-storage-quickstart-create-account.md) over het onderwerp te voltooien om een nieuw opslagaccount te maken. 

## <a name="set-aside-storage-account-configuration"></a>Opslagaccountconfiguratie instellen

U hebt de naam van uw opslagaccount en een eindpunt-URI voor het bestandssysteem nodig.

Als u de naam van uw opslagaccount in de Azure-portal wilt ophalen, kiest u **Alle services** en filtert u op de term *opslag*. Selecteer vervolgens **Opslagaccounts** en zoek het opslagaccount.

Als u de eindpunt-URI voor het bestandssysteem wilt ophalen, kiest u **Eigenschappen** en zoekt u de waarde van het veld **Primair eindpunt van het ADLS-bestandssysteem** in het deelvenster met de eigenschappen.

Plak beide waarden in een tekstbestand. U hebt deze binnenkort nodig.

<a id="service-principal"/>

## <a name="create-a-service-principal"></a>Een service-principal maken

Maak een service-principal aan de hand van de instructies in dit onderwerp: [Procedure: Gebruik de portal voor het maken van een Azure AD-toepassing en service-principal die toegang hebben tot resources](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

Er zijn enkele specifieke zaken die u moet doen terwijl u de stappen in dat artikel uitvoert.

:heavy_check_mark: Als u de stappen gaat uitvoeren in de sectie [Een Azure Active Directory-toepassing maken](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application) van het artikel, moet u er voor zorgen dat het veld **Aanmeldings-URL** van het dialoogvenster **Maken** is ingesteld op de eindpunt-URI die u zojuist hebt verkregen.

:heavy_check_mark: Als u de stappen gaat uitvoeren in de sectie [De toepassing aan een rol toewijzen](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role) van het artikel, moet u er voor zorgen dat u de toepassing toewijst aan de **rol van inzender voor Blob Storage**.

:heavy_check_mark: Als u de stappen gaat uitvoeren in de sectie [Waarden ophalen voor het aanmelden](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) van het artikel, plakt u de waarden van de tenant-id, de toepassings-id en de verificatiesleutel in een tekstbestand. U hebt deze binnenkort nodig.

## <a name="create-a-databricks-cluster"></a>Een Databricks-cluster maken

De volgende stap is het maken van een Databricks-cluster om een gegevenswerkruimte te maken.

1. Selecteer in de [Azure-portal](https://portal.azure.com) de optie **Een resource maken**.
2. Voer in het zoekveld **Azure Databricks** in.
3. Selecteer **Maken** op de blade Azure Databricks.
4. Geef de Databricks-service de naam **myFlightDataService**. (Schakel het selectievakje *Vastmaken aan dashboard* in bij het maken van de service.)
5. Selecteer **Werkruimte starten** om de werkruimte te openen in een nieuw browservenster.
6. Selecteer **Clusters** in de linkernavigatiebalk.
7. Selecteer **Cluster maken**.
8. Voer **myFlightDataCluster** in het veld **Clusternaam** in.
9. Selecteer **Standard_D8s_v3** in het veld **Type werkrol**.
10. Wijzig de waarde **Min werkrol** in **4**.
11. Selecteer **Cluster maken** boven aan de pagina. (Het duurt maximaal 5 minuten om dit proces te voltooien.)
12. Zodra het proces is voltooid, selecteert u linksboven in de navigatiebalk de optie **Azure Databricks**.
13. Selecteer in de sectie **Nieuw** op de onderste helft van de pagina de optie **Notitieblok**.
14. Kies een naam, voer deze in het veld **Naam** in, en selecteer **Python** als taal.
15. U kunt in alle andere velden de standaardwaarden laten staan.
16. Selecteer **Maken**.
17. Kopieer en plak het volgende codeblok in de eerste cel, maar voer deze code nog niet uit.

    ```Python
    configs = {"fs.azure.account.auth.type": "OAuth",
           "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
           "fs.azure.account.oauth2.client.id": "<application-id>",
           "fs.azure.account.oauth2.client.secret": "<authentication-id>",
           "fs.azure.account.oauth2.client.endpoint": "https://login.microsoftonline.com/<tenant-id>/oauth2/token",
           "fs.azure.createRemoteFileSystemDuringInitialization": "true"}

    dbutils.fs.mount(
    source = "abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/folder1",
    mount_point = "/mnt/flightdata",
    extra_configs = configs)
    ```
18. In dit codeblok vervangt u de tijdelijke aanduidingen `storage-account-name`, `application-id`, `authentication-id` en `tenant-id` door de waarden die u hebt verkregen bij het uitvoeren van de stappen in de secties [Opslagaccountconfiguratie instellen](#config) en [Een service-principal maken](#service-principal) van dit artikel. Vervang de tijdelijke aanduiding `file-system-name` door de naam die u aan uw bestandssysteem wilt geven.

19. Druk op de toetsen **Shift + Enter** om de code in dit blok uit te voeren.

## <a name="ingest-data"></a>Gegevens opnemen

### <a name="copy-source-data-into-the-storage-account"></a>Brongegevens kopiëren naar het opslagaccount

De volgende taak is het gebruiken van AzCopy om gegevens uit het *.csv*-bestand te kopiëren naar Azure Storage. Open een opdrachtpromptvenster en voer de volgende opdrachten in. Zorg ervoor dat u de tijdelijke aanduidingen `<DOWNLOAD_FILE_PATH>`, `<ACCOUNT_NAME>` en `<ACCOUNT_KEY>` vervangt door de bijbehorende waarden die u in de vorige stap hebt genoteerd.

```bash
set ACCOUNT_NAME=<ACCOUNT_NAME>
set ACCOUNT_KEY=<ACCOUNT_KEY>
azcopy cp "<DOWNLOAD_FILE_PATH>" https://<ACCOUNT_NAME>.dfs.core.windows.net/dbricks/folder1/On_Time --recursive 
```

### <a name="use-databricks-notebook-to-convert-csv-to-parquet"></a>Databricks Notebook gebruiken om CSV te converteren naar Parquet

Open Databricks opnieuw in de browser en voer de volgende stappen uit:

1. Selecteer linksboven in de navigatiebalk de optie **Azure Databricks**.
2. Selecteer in de sectie **Nieuw** op de onderste helft van de pagina de optie **Notitieblok**.
3. Voer in het veld **Naam** in: **CSV2Parquet**.
4. U kunt in alle andere velden de standaardwaarden laten staan.
5. Selecteer **Maken**.
6. Plak de volgende code in de cel **Cmd 1**. (Deze code wordt automatisch opgeslagen in de editor.)

    ```python
    # Use the previously established DBFS mount point to read the data
    # create a dataframe to read data
    flightDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/On_Time_On_Time*.csv")
    # read the all the airline csv files and write the output to parquet format for easy query
    flightDF.write.mode("append").parquet("/mnt/flightdata/parquet/flights")
    print("Done")
    ```

## <a name="explore-data"></a>Gegevens verkennen

Ga terug naar de Databricks-werkruimte en selecteer in de linkernavigatiebalk het pictogram **Recent**.

1. Selecteer het notitieblok **Analyse van vluchtgegevens**.
2. Druk op **Ctrl + Alt + N** om een nieuwe cel te maken.

Voer elk van de volgende codeblokken in **Cmd 1** in en druk op **Cmd + Enter** om het Python-script uit te voeren.

Als u een lijst met CSV-bestanden wilt ophalen die zijn geüpload met behulp van AzCopy, voert u het volgende script uit:

```python
import os.path
import IPython
from pyspark.sql import SQLContext
display(dbutils.fs.ls("/mnt/flightdata/temp/"))
```

Als u een nieuw bestand en een nieuwe lijst met bestanden wilt maken in de map *parquet/flights*, voert u dit script uit:

```python
dbutils.fs.put("/mnt/flightdata/temp/1.txt", "Hello, World!", True)
dbutils.fs.ls("/mnt/flightdata/temp/parquet/flights")
```

Met deze codevoorbeelden hebt u de hiërarchische aard van HDFS verkend met behulp van gegevens die zijn opgeslagen in een opslagaccount met Azure Data Lake Storage Gen2.

## <a name="query-the-data"></a>Query’s uitvoeren voor de gegevens

Hierna kunt u beginnen met het doorzoeken van de gegevens die u hebt geüpload in het opslagaccount. Voer elk van de volgende codeblokken in **Cmd 1** in en druk op **Cmd + Enter** om het Python-script uit te voeren.

### <a name="run-simple-queries"></a>Eenvoudige query’s uitvoeren

Als u dataframes wilt maken voor uw gegevensbronnen, voert u het volgende script uit:

> [!IMPORTANT]
> Vervang de tijdelijke aanduiding **<YOUR_CSV_FILE_NAME>**  door de bestandsnaam die u aan het begin van deze zelfstudie hebt gedownload.

```python
#Copy this into a Cmd cell in your notebook.
acDF = spark.read.format('csv').options(header='true', inferschema='true').load("/mnt/flightdata/<YOUR_CSV_FILE_NAME>.csv")
acDF.write.parquet('/mnt/flightdata/parquet/airlinecodes')

#read the existing parquet file for the flights database that was created earlier
flightDF = spark.read.format('parquet').options(header='true', inferschema='true').load("/mnt/flightdata/parquet/flights")

#print the schema of the dataframes
acDF.printSchema()
flightDF.printSchema()

#print the flight database size
print("Number of flights in the database: ", flightDF.count())

#show the first 20 rows (20 is the default)
#to show the first n rows, run: df.show(n)
acDF.show(100, False)
flightDF.show(20, False)

#Display to run visualizations
#preferably run this in a separate cmd cell
display(flightDF)
```

Als u analysequery’s wilt uitvoeren voor deze gegevens, voert u het volgende script uit:

```python
#Run each of these queries, preferably in a separate cmd cell for separate analysis
#create a temporary sql view for querying flight information
FlightTable = spark.read.parquet('/mnt/flightdata/parquet/flights')
FlightTable.createOrReplaceTempView('FlightTable')

#create a temporary sql view for querying airline code information
AirlineCodes = spark.read.parquet('/mnt/flightdata/parquet/airlinecodes')
AirlineCodes.createOrReplaceTempView('AirlineCodes')

#using spark sql, query the parquet file to return total flights in January and February 2016
out1 = spark.sql("SELECT * FROM FlightTable WHERE Month=1 and Year= 2016")
NumJan2016Flights = out1.count()
out2 = spark.sql("SELECT * FROM FlightTable WHERE Month=2 and Year= 2016")
NumFeb2016Flights=out2.count()
print("Jan 2016: ", NumJan2016Flights," Feb 2016: ",NumFeb2016Flights)
Total= NumJan2016Flights+NumFeb2016Flights
print("Total flights combined: ", Total)

# List out all the airports in Texas
out = spark.sql("SELECT distinct(OriginCityName) FROM FlightTable where OriginStateName = 'Texas'") 
print('Airports in Texas: ', out.show(100))

#find all airlines that fly from Texas
out1 = spark.sql("SELECT distinct(Carrier) FROM FlightTable WHERE OriginStateName='Texas'")
print('Airlines that fly to/from Texas: ', out1.show(100, False))
```

### <a name="run-complex-queries"></a>Complexe query’s uitvoeren

Voer de segmenten één voor één uit in het notitieblok en controleer de resultaten om de volgende complexere query’s uit te voeren.

```python
#find the airline with the most flights

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")

#show the top row without truncation
output.show(1, False)

#show the top 10 airlines
output.show(10, False)

#Determine which is the least on time airline

#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v")
spark.sql("CREATE TEMPORARY VIEW v AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE DepDelay>60 or ArrDelay>60 group by Carrier, UniqueCarrier order by NumFlights desc LIMIT 10")
output = spark.sql("select * from v")
#output = spark.sql("SELECT AirlineName FROM AirlineCodes WHERE AirlineCode in (select Carrier from v)")
#show the top row without truncation
output.show(1, False)

#which airline improved its performance
#find the airline with the most improvement in delays
#create a temporary view to hold the flight delay information aggregated by airline, then select the airline name from the Airlinecodes dataframe
spark.sql("DROP VIEW IF EXISTS v1")
spark.sql("DROP VIEW IF EXISTS v2")
spark.sql("CREATE TEMPORARY VIEW v1 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2016 group by Carrier order by NumFlights desc LIMIT 10")
spark.sql("CREATE TEMPORARY VIEW v2 AS SELECT Carrier, count(*) as NumFlights from FlightTable WHERE (DepDelay>0 or ArrDelay>0) and Year=2017 group by Carrier order by NumFlights desc LIMIT 10")
output = spark.sql("SELECT distinct ac.AirlineName, v1.Carrier, v1.NumFlights, v2.NumFlights from v1 INNER JOIN v2 ON v1.Carrier = v2.Carrier INNER JOIN AirlineCodes ac ON v2.Carrier = ac.AirlineCode WHERE v1.NumFlights > v2.NumFlights")
#show the top row without truncation
output.show(10, False)

#display for visual analysis
display(output)
```

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder de resourcegroep en alle gerelateerde resources, wanneer u deze niet meer nodig hebt. Hiervoor selecteert u de resourcegroep voor het opslagaccount en selecteert u **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

[!div class="nextstepaction"] 
> [Gegevens extraheren, transformeren en laden met Apache Hive in Azure HDInsight](data-lake-storage-tutorial-extract-transform-load-hive.md)

