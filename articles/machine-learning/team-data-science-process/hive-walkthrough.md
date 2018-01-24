---
title: Verken de gegevens in een Hadoop-cluster en modellen maken in Azure Machine Learning | Microsoft Docs
description: Met behulp van het Team gegevens wetenschap proces voor een end-to-end-scenario, die gebruikmaakt van een HDInsight Hadoop-cluster te bouwen en implementeren van een model.
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: e9e76c91-d0f6-483d-bae7-2d3157b86aa0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev
ms.openlocfilehash: daf5168ba9a21a56d72fc14649c349b0fb63a167
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>De procedure van wetenschappelijke gegevens Team in actie: gebruik Azure HDInsight Hadoop-clusters
In dit scenario, gebruiken we de [Team gegevens wetenschap proces (TDSP)](overview.md) in een end-to-end-scenario. We gebruiken een [Azure HDInsight Hadoop-cluster](https://azure.microsoft.com/services/hdinsight/) verkennen, om op te slaan en functie-engineering-gegevens uit de openbaar beschikbare [NYC Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gegevensset, en met de gegevens down-voorbeeld. Voor het afhandelen van binaire en multiklassen classificatie en regressie voorspellende taken, maken we modellen van de gegevens met Azure Machine Learning. 

Zie voor een overzicht waarin wordt uitgelegd hoe u een grotere gegevensset te verwerken, [Team gegevens wetenschap proces - met behulp van Azure HDInsight Hadoop-Clusters op een gegevensset 1 TB](hive-criteo-walkthrough.md).

U kunt ook een laptop IPython gebruiken voor het uitvoeren van de taken die zijn gepresenteerd in de procedure die gebruikmaakt van de gegevensset 1 TB. Zie voor meer informatie [Criteo scenario met behulp van een verbinding Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb).

## <a name="dataset"></a>Beschrijving van de NYC Taxi reizen gegevensset
De gegevens van de NYC Taxi reis is ongeveer 20 GB gecomprimeerde door komma's gescheiden waarden (CSV)-bestanden (~ 48 GB ongecomprimeerde). Deze heeft meer dan 173 miljoen afzonderlijke reizen en bevat de tarieven voor elke reis betaald. Elke record reis omvat de locatie op te halen en inleverbibliotheek en tijd, geanonimiseerde hack (van stuurprogramma) licentienummer en straten getal (de taxi unieke ID). De gegevens bevat informatie over alle reizen in het jaar 2013 en is beschikbaar in de volgende twee gegevenssets voor elke maand:

- De trip_data CSV-bestanden bevatten reis details. Dit omvat het aantal passagiers, op te halen en inleverpunt reis duur en reis lengte. Hier volgen enkele voorbeeldrecords:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
- De trip_fare CSV-bestanden bevatten details van het tarief dat voor elke reis betaald. Dit omvat betalingstype, tarief bedrag, extra kosten en belastingen, tips en tolgelden en het totaalbedrag wordt betaald. Hier volgen enkele voorbeeldrecords:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De unieke sleutel toevoegen reis\_gegevens en reis\_tarief dat bestaat uit de velden: straten, hack\_licentie, en ophalen van\_datetime. Als u alle gegevens relevant zijn voor een bepaalde reis, is het voldoende samen te voegen met deze drie sleutels.

## <a name="mltasks"></a>Voorbeelden van voorspelling taken
Bepaal het soort voorspellingen die u wilt maken op basis van de analyse van gegevens. Dit helpt de taken die u wilt opnemen in uw proces verduidelijken. Hier volgen drie voorbeelden van voorspelling problemen die we in dit overzicht oplossen. Deze zijn gebaseerd op de *tip\_bedrag*:

- **Binaire classificatie**: voorspellen of een tip voor een zakenreis is betaald. Dat wil zeggen, een *tip\_bedrag* die groter is dan $0 een voorbeeld van een positieve is, terwijl een *tip\_bedrag* van $0 is een voorbeeld van een negatief.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
- **Multiklassen classificatie**: het bereik van tip bedragen betaald voor de reis voorspellen. We delen de *tip\_bedrag* in vijf klassen:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
- **Regressie taak**: het bedrag van de tip voor een reis betaald voorspellen.  

## <a name="setup"></a>Instellen van een HDInsight Hadoop-cluster voor geavanceerde analyses
> [!NOTE]
> Dit is meestal een admin-taak.
> 
> 

U kunt instellen wanneer er een Azure-omgeving voor geavanceerde analyses die gebruikmaakt van een HDInsight-cluster in drie stappen:

1. [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md): dit opslagaccount wordt gebruikt voor het opslaan van gegevens in Azure Blob-opslag. De gegevens in HDInsight-clusters gebruikt ook bevinden zich hier.
2. [Azure HDInsight Hadoop-clusters aanpassen voor het proces van Advanced Analytics and Technology](customize-hadoop-cluster.md). Deze stap maakt een HDInsight Hadoop-cluster met 64-bits Anaconda Python 2.7 is geïnstalleerd op alle knooppunten. Er zijn twee belangrijke stappen om te onthouden tijdens het aanpassen van uw HDInsight-cluster.
   
   * Vergeet niet om te koppelen van het opslagaccount in stap 1 met uw HDInsight-cluster gemaakt wanneer u deze maakt. Dit opslagaccount heeft toegang tot gegevens die binnen het cluster wordt verwerkt.
   * Nadat u het cluster hebt gemaakt, kunt u externe toegang inschakelen met het hoofdknooppunt van het cluster. Blader naar de **configuratie** tabblad en selecteer **externe inschakelen**. Deze stap Hiermee geeft u de referenties van de gebruiker gebruikt voor externe aanmelding.
3. [Maken van een Azure Machine Learning-werkruimte](../studio/create-workspace.md): gebruik van deze werkruimte voor het bouwen van machine learning-modellen. Deze taak is gericht na het voltooien van een initiële gegevensverkenning en omlaag-sampling met behulp van het HDInsight-cluster.

## <a name="getdata"></a>De gegevens ophalen uit een openbare bron
> [!NOTE]
> Dit is meestal een admin-taak.
> 
> 

Kopiëren van de [NYC Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gegevensset aan op de computer van de openbare locatie, gebruikt u een van de methoden die worden beschreven [gegevens van en naar Azure Blob-opslag verplaatsen](move-azure-blob.md).

Hier wordt beschreven hoe AzCopy gebruiken om over te dragen van de bestanden met gegevens. Volg de instructies op om te downloaden en installeren van AzCopy, [aan de slag met het AzCopy-opdrachtregelprogramma](../../storage/common/storage-use-azcopy.md).

1. Voer vanuit een opdrachtpromptvenster de volgende opdrachten uit AzCopy, vervangen *< path_to_data_folder >* met de gewenste bestemming:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Wanneer de kopie is voltooid, ziet u een totaal van 24 ZIP-bestanden in de map data gekozen. Pak het gedownloade bestanden op in dezelfde map op uw lokale machine. Noteer de map waar de niet-gecomprimeerde bestanden zich bevinden. Deze map wordt aangeduid als de *< pad\_naar\_unzipped_data\_bestanden\>*  in welke volgt.

## <a name="upload"></a>De gegevens uploaden naar de standaardcontainer van HDInsight Hadoop-cluster
> [!NOTE]
> Dit is meestal een admin-taak.
> 
> 

In de volgende opdrachten uit AzCopy, vervangt u de volgende parameters met de werkelijke waarden die u hebt opgegeven bij het maken van het Hadoop-cluster en de gegevensbestanden ritsen.

* ***&#60; path_to_data_folder >*** de map (samen met het pad) op de computer die de uitgepakte bestanden bevat.  
* ***&#60; opslagaccountnaam van Hadoop-cluster >*** het storage-account die is gekoppeld aan uw HDInsight-cluster.
* ***&#60; standaardcontainer van Hadoop-cluster >*** de standaardcontainer gebruikt door het cluster. Houd er rekening mee dat de naam van de standaardcontainer meestal dezelfde naam als het cluster zelf is. Als het cluster wordt 'abc123.azurehdinsight.net' genoemd, is de standaardcontainer abc123.
* ***&#60; opslagaccountsleutel >*** de sleutel voor het opslagaccount dat wordt gebruikt door het cluster.

Voer de volgende twee AzCopy-opdrachten uit vanaf de opdrachtprompt of een Windows PowerShell-venster.

Met deze opdracht uploadt de reis gegevens naar de ***nyctaxitripraw*** map in de standaardcontainer van het Hadoop-cluster.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Met deze opdracht uploadt het tarief dat gegevens naar de ***nyctaxifareraw*** map in de standaardcontainer van het Hadoop-cluster.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

De gegevens worden nu in Blob storage en klaar om te worden gebruikt binnen het HDInsight-cluster.

## <a name="#download-hql-files"></a>Meld u aan met het hoofdknooppunt van Hadoop-cluster en bereid voor experimentele data-analyse
> [!NOTE]
> Dit is meestal een admin-taak.
> 
> 

Voor toegang tot het hoofdknooppunt van het cluster voor experimentele data-analyse en down-steekproeven van de gegevens, volgt u de procedure beschreven in [toegang tot het hoofdknooppunt van Hadoop-Cluster](customize-hadoop-cluster.md).

In dit scenario, gebruiken we voornamelijk query's die zijn geschreven [Hive](https://hive.apache.org/), een SQL-achtige query language voorlopige gegevens explorations uitvoeren. Het Hive-query's worden opgeslagen in .hql bestanden. We vervolgens omlaag-voorbeeld deze gegevens naar Machine Learning worden gebruikt voor het ontwikkelen van modellen.

Als u het cluster voorbereiden voor experimentele data-analyse, downloaden de .hql-bestanden met de relevante Hive-scripts uit [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) naar een lokale map (C:\temp) op het hoofdknooppunt. Om dit te doen, opent u de opdrachtprompt vanuit het hoofdknooppunt van het cluster en voer de volgende twee opdrachten:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Deze twee opdrachten downloaden alle .hql bestanden die nodig zijn in dit scenario naar een lokale map ***C:\temp &#92;*** in het hoofdknooppunt.

## <a name="#hive-db-tables"></a>Hive-database en per maand gepartitioneerde tabellen maken
> [!NOTE]
> Dit is meestal een admin-taak.
> 
> 

U bent nu klaar om te maken van Hive-tabellen voor de NYC taxi gegevensset.
Open de Hadoop-opdrachtregel op het bureaublad van het hoofdknooppunt in het hoofdknooppunt van het Hadoop-cluster. Voer de Hive-map met de volgende opdracht:

    cd %hive_home%\bin

> [!NOTE]
> Alle Hive-opdrachten uitvoeren in dit overzicht van de opslaglocatie Hive / directory prompt. Dit verwerkt pad problemen automatisch. We gebruiken de termen 'Hive directory prompt', ' Hive bin / directory prompt ', en "Hadoop-opdrachtregel' door elkaar in dit scenario.
> 
> 

Voer de volgende opdracht in de Hadoop-opdrachtregel van het hoofdknooppunt van de directory Hive-prompt. Hiermee wordt de Hive-query voor het maken van de Hive-database en tabellen verzonden:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Hier volgt de inhoud van de **C:\temp\sample\_hive\_maken\_db\_en\_tables.hql** bestand. Hiermee maakt u de database Hive **nyctaxidb**, tabellen en **reis** en **tarief**.

    create database if not exists nyctaxidb;

    create external table if not exists nyctaxidb.trip
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double)  
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/trip' TBLPROPERTIES('skip.header.line.count'='1');

    create external table if not exists nyctaxidb.fare
    (
        medallion string,
        hack_license string,
        vendor_id string,
        pickup_datetime string,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double)
    PARTITIONED BY (month int)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    STORED AS TEXTFILE LOCATION 'wasb:///nyctaxidbdata/fare' TBLPROPERTIES('skip.header.line.count'='1');

Deze Hive-script maakt twee tabellen:

* De **reis** tabel bevat reis details van elke rijpositie (stuurprogrammagegevens, de tijd op te halen, reis afstand en tijden).
* De **tarief** tabel bevat details tarief (tip bedrag tarief bedrag tolgelden en toeslagen).

Als u met deze procedures als u meer hulp nodig hebt of als u wilt onderzoeken alternatieve waarden, Zie de sectie [indienen Hive-query's rechtstreeks vanaf de opdrachtregel Hadoop](move-hive-tables.md#submit).

## <a name="#load-data"></a>Laden van gegevens met Hive-tabellen partities
> [!NOTE]
> Dit is meestal een admin-taak.
> 
> 

De NYC taxi gegevensset heeft een natuurlijke partitioneren per maand, die we gebruiken om in te schakelen sneller verwerking en queryprestaties. De volgende PowerShell-opdrachten (via de opdrachtregel Hadoop uitgegeven door de Hive-map) laden van gegevens naar de reis en ritbedrag Hive-tabellen gepartitioneerd per maand.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

De **voorbeeld\_hive\_laden\_gegevens\_door\_partitions.hql** bestand bevat de volgende **laden** opdrachten:

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Opmerking dat een aantal van de Hive-query's die hier worden gebruikt in het proces exploratie betrekking hebben op slechts één of twee partities kijken. Maar deze query's kunnen worden uitgevoerd in de volledige gegevensset.

### <a name="#show-db"></a>Databases weergeven in de HDInsight Hadoop-cluster
Als u wilt weergeven van de databases die zijn gemaakt in HDInsight Hadoop-cluster in het Hadoop-opdrachtregelvenster, voer de volgende opdracht in de Hadoop-opdrachtregel:

    hive -e "show databases;"

### <a name="#show-tables"></a>Weergeven in de Hive-tabellen in de **nyctaxidb** database
Om weer te geven in de tabellen in de **nyctaxidb** database, voer de volgende opdracht in de Hadoop-opdrachtregel:

    hive -e "show tables in nyctaxidb;"

We kunt bevestigen dat de tabellen worden gepartitioneerd met de volgende opdracht uit te voeren:

    hive -e "show partitions nyctaxidb.trip;"

Dit is de verwachte uitvoer:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 2.075 seconds, Fetched: 12 row(s)

Op deze manier kunnen we dat de tarief-tabel is gepartitioneerd met de volgende opdracht:

    hive -e "show partitions nyctaxidb.fare;"

Dit is de verwachte uitvoer:

    month=1
    month=10
    month=11
    month=12
    month=2
    month=3
    month=4
    month=5
    month=6
    month=7
    month=8
    month=9
    Time taken: 1.887 seconds, Fetched: 12 row(s)

## <a name="#explore-hive"></a>Gegevensverkenning en functie-engineering in component
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

U kunt Hive-query's uitvoeren gegevensverkenning en taken voor de gegevens in de Hive-tabellen geladen engineering-functie. Hier volgen enkele voorbeelden van dergelijke taken:

* De top 10 records weergeven in beide tabellen.
* Verken de gegevens distributies van enkele velden in verschillende tijdvensters.
* Onderzoek de kwaliteit van de gegevens van de breedtegraad -velden.
* Binaire en multiklassen classificatielabels op basis van de hoeveelheid tip te genereren.
* Functies door de directe trip afstanden computing genereren.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Exploratie: De top 10 records in de tabel reis weergeven
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

Om te zien hoe de gegevens eruit ziet, controleert u 10 records uit elke tabel. Om te controleren records, de volgende twee query's ook afzonderlijk uitvoeren vanaf de opdrachtprompt Hive directory in de console van Hadoop-opdrachtregel.

De top 10 records in de tabel reis ophalen uit de eerste maand:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

De top 10 records in de tabel tarief ophalen uit de eerste maand:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

U kunt de records opslaan naar een bestand om handige weer te geven. Een kleine wijziging in de voorgaande query bewerkstelligt dit:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Exploratie: Het aantal records weergeven in elk van de partities 12
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

Van belang is hoe het aantal reizen varieert tijdens het kalenderjaar. Groeperen per maand toont de distributie van heen.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Dit biedt ons de volgende uitvoer:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 283.406 seconds, Fetched: 12 row(s)

Hier worden de eerste kolom is de maand en de tweede is het aantal reizen voor die maand.

We kunnen ook het totale aantal records in onze gegevensset reis tellen door met de volgende opdracht bij de prompt Hive-directory:

    hive -e "select count(*) from nyctaxidb.trip;"

Dit levert:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Vergelijkbaar met die wordt weergegeven voor de gegevensset reis opdrachten kunnen we Hive-query's vanaf de opdrachtprompt Hive directory voor de gegevensset tarief voor het valideren van het aantal records uitgeven.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Dit biedt ons de volgende uitvoer:

    1       14776615
    2       13990176
    3       15749228
    4       15100468
    5       15285049
    6       14385456
    7       13823840
    8       12597109
    9       14107693
    10      15004556
    11      14388451
    12      13971118
    Time taken: 253.955 seconds, Fetched: 12 row(s)

Houd er rekening mee dat het exacte hetzelfde aantal bezoeken per maand wordt geretourneerd voor beide gegevenssets. Dit biedt de eerste validatie van de gegevens correct zijn geladen.

U kunt het totale aantal records in de gegevensset tarief tellen met behulp van de volgende opdracht achter de opdrachtprompt van de directory Hive:

    hive -e "select count(*) from nyctaxidb.fare;"

Dit levert:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Het totale aantal records in beide tabellen is ook hetzelfde. Dit biedt een tweede validatie van de gegevens correct zijn geladen.

### <a name="exploration-trip-distribution-by-medallion"></a>Exploratie: Reis distributie door straten
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

In dit voorbeeld identificeert de medallions (taxi getallen) met groter zijn dan 100 reizen binnen een bepaalde periode. De query profiteert van de toegang tot gepartitioneerde tabellen, omdat deze wordt waarbij door de variabele partitie **maand**. De queryresultaten worden naar een lokaal bestand geschreven **queryoutput.tsv**in `C:\temp` op het hoofdknooppunt.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Hier volgt de inhoud van de **voorbeeld\_hive\_reis\_aantal\_door\_medallion.hql** -bestand voor inspectie.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

De straten in de NYC taxi gegevensset identificeert een unieke CAB-bestand. U kunt identificeren welke cabinetbestanden relatief bezet zijn door vragen aan welke u meer dan een bepaald aantal reizen aangebracht in een bepaalde periode. Het volgende voorbeeld identificeert cabinetbestanden die meer dan honderd reizen aangebracht in de eerste drie maanden en slaat de queryresultaten naar een lokaal bestand **C:\temp\queryoutput.tsv**.

Hier volgt de inhoud van de **voorbeeld\_hive\_reis\_aantal\_door\_medallion.hql** -bestand voor inspectie.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Voer de volgende opdracht vanaf de opdrachtprompt Hive-map:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Exploratie: Reis distributie door straten en hack licentie
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

Wanneer een dataset verkennen, willen we vaak het nummer van co-exemplaren van groepen van waarden te onderzoeken. Deze sectie bevat een voorbeeld van hoe u dit doet voor CAB-bestanden en stuurprogramma's.

De **voorbeeld\_hive\_reis\_aantal\_door\_straten\_license.hql** bestandsgroepen de gegevensset tarief op **straten** en **hack_license**, en retourneert de aantallen voor elke combinatie. Hier volgen de inhoud ervan:

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Deze query retourneert combinaties van CAB-bestand en het stuurprogramma, geordend op aflopende aantal heen.

Vanaf de opdrachtprompt van de map Hive uitvoeren:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

De queryresultaten worden naar een lokaal bestand geschreven **C:\temp\queryoutput.tsv**.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitude-or-latitude-records"></a>Exploratie: Beoordeling van de kwaliteit van de gegevens door te controleren op een ongeldige lengte of breedte records
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

Een algemene doel van experimentele gegevensanalyse is wieden ongeldige of onjuiste records. In het voorbeeld in deze sectie bepaalt of de lengte of de breedte velden een waarde ver buiten het gebied van de NYC bevatten. Aangezien het waarschijnlijk dat dergelijke records de waarde van een foutieve lengtegraad-breedtegraad hebben, willen we deze alle gegevens die moet worden gebruikt voor het modelleren te elimineren.

Hier volgt de inhoud van **voorbeeld\_hive\_kwaliteit\_assessment.hql** -bestand voor inspectie.

        SELECT COUNT(*) FROM nyctaxidb.trip
        WHERE month=1
        AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(pickup_latitude AS float) NOT BETWEEN 30 AND 90
        OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND -30
        OR    CAST(dropoff_latitude AS float) NOT BETWEEN 30 AND 90);


Vanaf de opdrachtprompt van de map Hive uitvoeren:

    hive -S -f "C:\temp\sample_hive_quality_assessment.hql"

De *-S* argument dat is opgenomen in deze opdracht wordt de status scherm afdruk van de kaart/verminderen Hive-taken onderdrukt. Dit is nuttig omdat op deze manier het scherm van de uitvoer van de Hive-query afdrukken beter leesbaar.

### <a name="exploration-binary-class-distributions-of-trip-tips"></a>Exploratie: Binaire klasse distributies reis tips
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

Voor de binair klassificatieprobleem die worden beschreven in de [voorbeelden van voorspelling taken](hive-walkthrough.md#mltasks) sectie is het handig te weten of een tip of niet is opgegeven. Deze verdeling van tips is binaire:

* Tip gegeven (klasse 1, tip\_bedrag > $0)  
* Er is geen tip (klasse 0, tip\_bedrag = $0)

De volgende **voorbeeld\_hive\_punt\_frequencies.hql** dit bestand wordt:

    SELECT tipped, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount > 0, 1, 0) as tipped, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tipped;

Vanaf de opdrachtprompt van de map Hive uitvoeren:

    hive -f "C:\temp\sample_hive_tipped_frequencies.hql"


### <a name="exploration-class-distributions-in-the-multiclass-setting"></a>Exploratie: Klasse distributies in de instelling voor multiklassen
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

Voor het probleem multiklassen classificatie is die worden beschreven in de [voorbeelden van voorspelling taken](hive-walkthrough.md#mltasks) sectie deze gegevensset ook gepaard met een natuurlijke classificatie te voorspellen, de hoeveelheid de tips gegeven. We kunnen opslaglocaties gebruiken voor het definiëren van tip bereiken in de query. Als u de klasse-Distributies voor de verschillende bereiken tip, gebruikt de **voorbeeld\_hive\_tip\_bereik\_frequencies.hql** bestand. Hier vindt u de inhoud ervan.

    SELECT tip_class, COUNT(*) AS tip_freq
    FROM
    (
        SELECT if(tip_amount=0, 0,
            if(tip_amount>0 and tip_amount<=5, 1,
            if(tip_amount>5 and tip_amount<=10, 2,
            if(tip_amount>10 and tip_amount<=20, 3, 4)))) as tip_class, tip_amount
        FROM nyctaxidb.fare
    )tc
    GROUP BY tip_class;

Voer de volgende opdracht vanaf de opdrachtregel Hadoop-console:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-the-direct-distance-between-two-longitude-latitude-locations"></a>Exploratie: De directe afstand tussen twee locaties voor lengtegraad breedtegraad berekenen
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

Mogelijk wilt weten als er een verschil tussen de directe afstand tussen twee locaties en de werkelijke reis afstand van de taxi. Passagiers mogelijk minder waarschijnlijk tip als ze weten dat het stuurprogramma opzettelijk door een langer route getroffen.

Om te zien van de vergelijking tussen de werkelijke reis afstand en de [Haversine afstand](http://en.wikipedia.org/wiki/Haversine_formula) tussen twee lengtegraad breedtegraad punten (de afstand van de 'groot cirkel'), kunt u beschikbare functies trigonometrische binnen Hive:

    set R=3959;
    set pi=radians(180);

    insert overwrite directory 'wasb:///queryoutputdir'

    select pickup_longitude, pickup_latitude, dropoff_longitude, dropoff_latitude, trip_distance, trip_time_in_secs,
    ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
     *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
     *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
     /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
     +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*
     pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance
    from nyctaxidb.trip
    where month=1
    and pickup_longitude between -90 and -30
    and pickup_latitude between 30 and 90
    and dropoff_longitude between -90 and -30
    and dropoff_latitude between 30 and 90;

In de voorgaande query R is de radius van de aarde in mijl en pi wordt omgezet in radialen. Houd er rekening mee dat de lengtegraad breedtegraad punten als u wilt verwijderen van de waarden die ver van het gebied van de NYC worden gefilterd.

In dit geval wordt de resultaten schrijven naar een map met de naam **queryoutputdir**. De volgorde van de volgende opdrachten maakt eerst deze uitvoermap, en vervolgens de Hive-opdracht wordt uitgevoerd.

Vanaf de opdrachtprompt van de map Hive uitvoeren:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


De queryresultaten naar negen Azure blobs worden geschreven (**queryoutputdir/000000\_0** naar **queryoutputdir/000008\_0**), onder de standaardcontainer van het Hadoop-cluster.

Overzicht van de grootte van de afzonderlijke blobs, voer de volgende opdracht achter de opdrachtprompt van de directory Hive:

    hdfs dfs -ls wasb:///queryoutputdir

Overzicht van de inhoud van een bepaald bestand, spreken **000000\_0**, gebruik van Hadoop `copyToLocal` opdracht.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`erg langzaam voor grote bestanden en wordt niet aanbevolen voor gebruik met deze.  
> 
> 

Een groot voordeel van deze gegevens zich bevinden in een Azure-blob is dat we de gegevens in Machine Learning verkennen met behulp van kunt de [importgegevens] [ import-data] module.

## <a name="#downsample"></a>Omlaag-sample data en bouwen van modellen in Machine Learning
> [!NOTE]
> Dit is meestal een wetenschappelijk gegevens taak.
> 
> 

Na de analysefase experimentele gegevens bent er nu klaar om de gegevens voor het bouwen van modellen in Machine Learning down-voorbeeld. In deze sectie laten we zien hoe u een Hive-query met de gegevens down-voorbeeld. In machine Learning vervolgens toegang tot de [importgegevens] [ import-data] module.

### <a name="down-sampling-the-data"></a>De gegevens down-steekproeven
Er zijn twee stappen in deze procedure. Eerst we toevoegen de **nyctaxidb.trip** en **nyctaxidb.fare** tabellen op de drie sleutels die aanwezig in alle records zijn: **straten**, **inbreken\_ licentie**, en **ophalen\_datetime**. Er vervolgens een label binaire classificatie gegenereerd **punt**, en een label multiklassen classificatie **tip\_klasse**.

Kunnen de omlaag actieve gegevens rechtstreeks vanuit de [importgegevens] [ import-data] module in Machine Learning, moet u de resultaten van de voorgaande query naar een interne Hive-tabel opslaan. In het volgende, we een interne Hive-tabel maken en vullen met de gekoppelde en omlaag actieve gegevens de inhoud ervan.

De query is van toepassing standaardfuncties Hive rechtstreeks voor het genereren van de volgende opties in de **ophalen\_datetime** veld:
- Uur van dag
- Week van jaar
- werkdag (1 staat voor maandag en 7 staat voor zondag)

De query wordt ook de directe afstand tussen de locaties op te halen en inleverbibliotheek gegenereerd. Zie voor een volledige lijst van dergelijke functies [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF).

De query vervolgens omlaag-samples de gegevens zodat de queryresultaten in Azure Machine Learning Studio inpassen kunnen. Alleen ongeveer 1 procent van de oorspronkelijke gegevensset wordt geïmporteerd in de studio.

Hier vindt u de inhoud van **voorbeeld\_hive\_voorbereiden\_voor\_aml\_full.hql** worden gegevens voorbereid voor model bouwen in Machine Learning-bestand:

        set R = 3959;
        set pi=radians(180);

        create table if not exists nyctaxidb.nyctaxi_downsampled_dataset (

        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        pickup_hour string,
        pickup_week string,
        weekday string,
        passenger_count int,
        trip_time_in_secs double,
        trip_distance double,
        pickup_longitude double,
        pickup_latitude double,
        dropoff_longitude double,
        dropoff_latitude double,
        direct_distance double,
        payment_type string,
        fare_amount double,
        surcharge double,
        mta_tax double,
        tip_amount double,
        tolls_amount double,
        total_amount double,
        tipped string,
        tip_class string
        )
        row format delimited fields terminated by ','
        lines terminated by '\n'
        stored as textfile;

        --- now insert contents of the join into the above internal table

        insert overwrite table nyctaxidb.nyctaxi_downsampled_dataset
        select
        t.medallion,
        t.hack_license,
        t.vendor_id,
        t.rate_code,
        t.store_and_fwd_flag,
        t.pickup_datetime,
        t.dropoff_datetime,
        hour(t.pickup_datetime) as pickup_hour,
        weekofyear(t.pickup_datetime) as pickup_week,
        from_unixtime(unix_timestamp(t.pickup_datetime, 'yyyy-MM-dd HH:mm:ss'),'u') as weekday,
        t.passenger_count,
        t.trip_time_in_secs,
        t.trip_distance,
        t.pickup_longitude,
        t.pickup_latitude,
        t.dropoff_longitude,
        t.dropoff_latitude,
        t.direct_distance,
        f.payment_type,
        f.fare_amount,
        f.surcharge,
        f.mta_tax,
        f.tip_amount,
        f.tolls_amount,
        f.total_amount,
        if(tip_amount>0,1,0) as tipped,
        if(tip_amount=0,0,
        if(tip_amount>0 and tip_amount<=5,1,
        if(tip_amount>5 and tip_amount<=10,2,
        if(tip_amount>10 and tip_amount<=20,3,4)))) as tip_class

        from
        (
        select
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        pickup_datetime,
        dropoff_datetime,
        passenger_count,
        trip_time_in_secs,
        trip_distance,
        pickup_longitude,
        pickup_latitude,
        dropoff_longitude,
        dropoff_latitude,
        ${hiveconf:R}*2*2*atan((1-sqrt(1-pow(sin((dropoff_latitude-pickup_latitude)
        *${hiveconf:pi}/180/2),2)-cos(pickup_latitude*${hiveconf:pi}/180)
        *cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2)))
        /sqrt(pow(sin((dropoff_latitude-pickup_latitude)*${hiveconf:pi}/180/2),2)
        +cos(pickup_latitude*${hiveconf:pi}/180)*cos(dropoff_latitude*${hiveconf:pi}/180)*pow(sin((dropoff_longitude-pickup_longitude)*${hiveconf:pi}/180/2),2))) as direct_distance,
        rand() as sample_key

        from nyctaxidb.trip
        where pickup_latitude between 30 and 90
            and pickup_longitude between -90 and -30
            and dropoff_latitude between 30 and 90
            and dropoff_longitude between -90 and -30
        )t
        join
        (
        select
        medallion,
        hack_license,
        vendor_id,
        pickup_datetime,
        payment_type,
        fare_amount,
        surcharge,
        mta_tax,
        tip_amount,
        tolls_amount,
        total_amount
        from nyctaxidb.fare
        )f
        on t.medallion=f.medallion and t.hack_license=f.hack_license and t.pickup_datetime=f.pickup_datetime
        where t.sample_key<=0.01

Deze query achter de opdrachtprompt van de directory Hive uitvoeren:

    hive -f "C:\temp\sample_hive_prepare_for_aml_full.hql"

We hebt nu een interne tabel **nyctaxidb.nyctaxi_downsampled_dataset**, die toegankelijk zijn via de [importgegevens] [ import-data] module op basis van Machine Learning. Daarnaast kunnen we deze gegevensset gebruiken voor het bouwen van Machine Learning-modellen.  

### <a name="use-the-import-data-module-in-machine-learning-to-access-the-down-sampled-data"></a>De gegevens importeren-module gebruiken in Machine Learning om omlaag actieve gegevens
Uitgeven van Hive-query's in de [importgegevens] [ import-data] -module van Machine Learning, moet u toegang tot een Machine Learning-werkruimte. U moet ook toegang tot de referenties van het cluster en de gekoppelde storage-account.

Hier vindt u enkele details over de [importgegevens] [ import-data] module en de parameters voor het invoeren van:

**HCatalog server-URI**: als de clusternaam is **abc123**, dit is eenvoudig: https://abc123.azurehdinsight.net.

**Hadoop-gebruikersaccountnaam**: de naam van de gebruiker gekozen voor het cluster (niet de RAS-gebruikersnaam).

**Hadoop ser accountwachtwoord**: het wachtwoord voor het cluster (niet de RAS-wachtwoord) wordt gekozen.

**Locatie van uitvoergegevens**: dit is gekozen als Azure.

**Naam van het Azure-opslagaccount**: naam van het standaardopslagaccount die is gekoppeld aan het cluster.

**Azure containernaam**: dit is de standaardnaam van de container voor het cluster en is meestal hetzelfde als de naam van het cluster. Aangeroepen voor een cluster **abc123**, dit abc123 is.

> [!IMPORTANT]
> Elke tabel opvragen wilt met behulp van de [importgegevens] [ import-data] module in Machine Learning een interne tabel moet zijn.
> 
> 

Ga als volgt om te bepalen of een tabel **T** in een database **D.db** is een interne tabel. Voer de volgende opdracht vanaf de opdrachtprompt Hive-map:

    hdfs dfs -ls wasb:///D.db/T

Als de tabel een interne tabel is en dit ingevuld, wordt de inhoud moeten hier weergegeven.

Een andere manier om te bepalen of een tabel een interne tabel is is het gebruik van Azure Storage Explorer. Gebruik het navigeren naar de standaardnaam van de container van het cluster en vervolgens filteren op naam van de tabel. Als de tabel en de inhoud ervan wordt getoond, bevestigt dit dat het is een interne tabel.

Hier volgt een schermafbeelding van de Hive-query en de [importgegevens] [ import-data] module:

![Schermopname van Hive-query voor gegevens importeren-module](./media/hive-walkthrough/1eTYf52.png)

Omdat onze gegevens omlaag dat zich in de standaardcontainer bevindt, is de resulterende Hive-query van Machine Learning is zeer eenvoudig. Is het een **Selecteer * uit nyctaxidb.nyctaxi\_verkleind\_gegevens**.

De gegevensset kan nu worden gebruikt als startpunt voor het bouwen van Machine Learning-modellen.

### <a name="mlmodel"></a>Bouwen van modellen in Machine Learning
U kunt nu verdergaan naar model bouwen en implementeren van modellen in [Machine Learning](https://studio.azureml.net). De gegevens is gereed voor gebruik bij het oplossen van de eerder vastgestelde problemen voor de voorspelling:

- **Binaire classificatie**: om te voorspellen of een tip betaald is voor een reis.

  **Cursist gebruikt:** Two-class logistic regression

  a. Voor dit probleem op door het doel (of een klasse)-label is **punt**. De oorspronkelijke omlaag actieve gegevensset heeft een aantal kolommen die doel lekken voor dit experiment classificatie. In het bijzonder **tip\_klasse**, **tip\_bedrag**, en **totale\_bedrag** onthullen informatie over het doel een label dat is niet beschikbaar voor het testen van tijd. We deze kolommen verwijderen uit overweging met behulp van de [Select Columns in Dataset] [ select-columns] module.

  Het volgende diagram toont het experiment te voorspellen of een tip voor een bepaalde reis is betaald:

  ![Diagram van een experiment](./media/hive-walkthrough/QGxRz5A.png)

  b. Ons doel label distributies zijn voor dit experiment ongeveer 1:1.

   Het volgende diagram toont de distributie van tip labels van de klasse voor het probleem binaire classificatie:

  ![Diagram van de distributie van tip klasse labels](./media/hive-walkthrough/9mM4jlD.png)

    Als gevolg hiervan krijgen we een gebied onder de curve (AUC) van 0.987, zoals wordt weergegeven in de volgende afbeelding:

  ![Diagram van AUC waarde](./media/hive-walkthrough/8JDT0F8.png)

- **Multiklassen classificatie**: om te voorspellen van het bereik van tip bedragen betaald voor onderweg, met behulp van de eerder gedefinieerde klassen.

  **Cursist gebruikt:** Multiklassen logistic regression

  a. Voor dit probleem op door het label voor onze doel (of een klasse) is **tip\_klasse**, die kan duren voordat een van vijf waarden (0,1,2,3,4). Net als bij de binaire classificatie hebben we enkele kolommen die doel lekken voor dit experiment. In het bijzonder **punt**, **tip\_bedrag**, en **totale\_bedrag** informatie over het doellabel is niet beschikbaar is op basis van duidelijk het testen van tijd. We deze kolommen verwijderen met behulp van de [Select Columns in Dataset] [ select-columns] module.

  Het volgende diagram toont het experiment te voorspellen, in welke opslaglocatie is een tip waarschijnlijk vallen. De opslaglocaties zijn: klasse 0: tip = $0, 1 van de klasse: tip > $0 en tip < = $5, klasse 2: tip > $5 en tip < = $10, klasse 3: tip > $10 en tip < $20 en klasse 4 =: tip > $20.

  ![Diagram van een experiment](./media/hive-walkthrough/5ztv0n0.png)

  Nu zien hoe de distributie van de klasse werkelijke test eruit ziet. Klasse 0 en 1 van de klasse zijn gangbare en de andere klassen niet vaak voorkomen.

  ![Diagram van test klasse distributie](./media/hive-walkthrough/Vy1FUKa.png)

  b. We gebruiken een matrix verwarring om te kijken naar de accuratesse voorspelling voor dit experiment. Dit wordt hier weergegeven:

  ![Verwarring matrix](./media/hive-walkthrough/cxFmErM.png)

  Houd er rekening mee dat de klasse accuratesse op de gangbare klassen zijn behoorlijk goed, het model wordt niet goed werk 'learning' op de zeldzame klassen.

- **Regressie taak**: om te voorspellen van de hoeveelheid tip voor een reis betaald.

  **Cursist gebruikt:** Boosted decision tree

  a. Voor dit probleem op door het doel (of een klasse)-label is **tip\_bedrag**. De doel-lekken in dit geval zijn: **punt**, **tip\_klasse**, en **totale\_bedrag**. Alle deze variabelen onthullen informatie over het bedrag tip die doorgaans niet beschikbaar voor het testen van tijd. We deze kolommen verwijderen met behulp van de [Select Columns in Dataset] [ select-columns] module.

  Het volgende diagram toont het experiment te voorspellen, het bedrag van de opgegeven tip:

  ![Diagram van een experiment](./media/hive-walkthrough/11TZWgV.png)

  b. We meten het accuratesse van de voorspelling door te kijken naar de gekwadrateerde fout in de voorspellingen en de determinatiecoëfficiënt regressie problemen oplossen:

  ![Schermafbeelding van de voorspelling statistieken](./media/hive-walkthrough/Jat9mrz.png)

  Hier, wordt die ongeveer 71 procent van de variantie van wat de determinatiecoëfficiënt 0.709 is, uitgelegd door de coëfficiënten model.

> [!IMPORTANT]
> Zie voor meer informatie over Machine Learning en hoe u toegang tot en het gebruik, [wat is Machine Learning](../studio/what-is-machine-learning.md). Bovendien de [AI-galerie van Azure](https://gallery.cortanaintelligence.com/) bevat informatie over een kleurenbereik van experimenten en biedt een uitgebreide inleiding in het bereik van de mogelijkheden van Machine Learning.
> 
> 

## <a name="license-information"></a>Licentie-informatie
In dit voorbeeld scenario en de bijbehorende scripts worden gedeeld door Microsoft onder de MIT-licentie. Zie voor meer informatie de **LICENSE.txt** bestand in de map van de voorbeeldcode op GitHub.

## <a name="references"></a>Verwijzingen
• [Andrés Monroy NYC Taxi reizen downloadpagina](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxi reis gegevens door Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi en Limousine Commissie onderzoek en statistieken](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[2]: ./media/hive-walkthrough/output-hive-results-3.png
[11]: ./media/hive-walkthrough/hive-reader-properties.png
[12]: ./media/hive-walkthrough/binary-classification-training.png
[13]: ./media/hive-walkthrough/create-scoring-experiment.png
[14]: ./media/hive-walkthrough/binary-classification-scoring.png
[15]: ./media/hive-walkthrough/amlreader.png

<!-- Module References -->
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/



