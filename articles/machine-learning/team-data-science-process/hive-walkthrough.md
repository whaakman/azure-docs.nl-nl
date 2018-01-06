---
title: Verken de gegevens in een Hadoop-cluster en modellen maken in Azure Machine Learning | Microsoft Docs
description: Met behulp van het Team gegevens wetenschap proces voor een end-to-end-scenario die gebruikmaakt van een HDInsight Hadoop-cluster te bouwen en implementeren van een model met behulp van een openbaar gegevensset.
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
ms.openlocfilehash: d10026b4f04ab77accf7d089e98270c4c769b636
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="the-team-data-science-process-in-action-use-azure-hdinsight-hadoop-clusters"></a>De procedure van wetenschappelijke gegevens Team in actie: gebruik Azure HDInsight Hadoop-clusters
In dit scenario, gebruiken we de [Team gegevens wetenschap proces (TDSP)](overview.md) in een end-to-end-scenario. We gebruiken een [Azure HDInsight Hadoop-cluster](https://azure.microsoft.com/services/hdinsight/) verkennen, om op te slaan en functie-engineering-gegevens uit de openbaar beschikbare [NYC Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gegevensset, en met de gegevens down-voorbeeld. Modellen van de gegevens zijn gebouwd met Azure Machine Learning om af te handelen binaire en multiklassen classificatie en regressie voorspellende taken.

Zie voor een overzicht waarin wordt uitgelegd hoe u een grotere gegevensset (1 terabyte) voor een vergelijkbaar scenario met HDInsight Hadoop-clusters voor gegevensverwerking verwerken [Team gegevens wetenschap proces - met behulp van Azure HDInsight Hadoop-Clusters op een gegevensset 1 TB](hive-criteo-walkthrough.md).

Het is ook mogelijk om te gebruiken van een laptop IPython taken voor het scenario met behulp van de gegevensset 1 TB weergegeven. Gebruikers die u wilt proberen deze benadering moeten contact opnemen met de [Criteo scenario met behulp van een verbinding Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) onderwerp.

## <a name="dataset"></a>Beschrijving van de NYC Taxi reizen gegevensset
De gegevens van de NYC Taxi reis is ongeveer 20GB gecomprimeerde door komma's gescheiden waarden (CSV)-bestanden (~ 48GB ongecomprimeerde). Het omvat meer dan 173 miljoen afzonderlijke reizen en de tarieven voor elke reis betaald. Elke record reis omvat de locatie ophalen en inleverbibliotheek en tijd, geanonimiseerde hack (van stuurprogramma) licentienummer en nummer straten (taxi van unieke id). De gegevens bevat informatie over alle reizen in het jaar 2013 en is beschikbaar in de volgende twee gegevenssets voor elke maand:

1. De 'trip_data' CSV-bestanden bevatten reis details. Dit omvat aantal passagiers, ophalen en dropoff punten reis duur en duur van reis. Hier volgen enkele voorbeeldrecords:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. De 'trip_fare' CSV-bestanden bevatten details van het tarief dat voor elke reis betaald. Dit omvat betalingstype, tarief bedrag, extra kosten en belastingen, tips en tolgelden en het totaalbedrag wordt betaald. Hier volgen enkele voorbeeldrecords:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De unieke sleutel toevoegen reis\_gegevens en reis\_tarief dat bestaat uit de velden: straten, hack\_en ophalen van certificaat\_datetime.

Als u alle gegevens relevant zijn voor een bepaalde reis, voldoende samen te voegen met drie sleutels is: 'straten', ' inbreken\_licentie ' en ' ophalen\_datetime-waarde '.

Sommige meer details van de gegevens worden beschreven wanneer we deze opslaan in de Hive-tabellen binnenkort.

## <a name="mltasks"></a>Voorbeelden van voorspelling taken
Wanneer bijna gegevens, het bepalen van het soort voorspellingen die u wilt maken, gebaseerd op de analysis, kunt de taken die u wilt opnemen in uw proces moet verduidelijken.
Hier volgen drie voorbeelden van voorspelling problemen die we in dit overzicht waarvan formulering is gebaseerd oplossen op de *tip\_bedrag*:

1. **Binaire classificatie**: voorspellen of een tip voor een zakenreis is betaald. Dat wil zeggen, een *tip\_bedrag* die groter is dan $0 een voorbeeld van een positieve is, terwijl een *tip\_bedrag* van $0 is een voorbeeld van een negatief.
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0
2. **Multiklassen classificatie**: om te voorspellen van het bereik van tip bedragen voor de reis betaald. We delen de *tip\_bedrag* in vijf opslaglocaties of klassen:
   
        Class 0: tip_amount = $0
        Class 1: tip_amount > $0 and tip_amount <= $5
        Class 2: tip_amount > $5 and tip_amount <= $10
        Class 3: tip_amount > $10 and tip_amount <= $20
        Class 4: tip_amount > $20
3. **Regressie taak**: om te voorspellen van de hoeveelheid de tip voor een reis betaald.  

## <a name="setup"></a>Instellen van een HDInsight Hadoop-cluster voor geavanceerde analyses
> [!NOTE]
> Dit is doorgaans een **Admin** taak.
> 
> 

U kunt instellen wanneer er een Azure-omgeving voor geavanceerde analyses die gebruikmaakt van een HDInsight-cluster in drie stappen:

1. [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md): dit opslagaccount wordt gebruikt voor het opslaan van gegevens in Azure Blob Storage. De gegevens in HDInsight-clusters gebruikt ook bevinden zich hier.
2. [Azure HDInsight Hadoop-clusters aanpassen voor het proces van Advanced Analytics and Technology](customize-hadoop-cluster.md). Deze stap maakt een Azure HDInsight Hadoop-cluster met 64-bits Anaconda Python 2.7 geïnstalleerd op alle knooppunten. Er zijn twee belangrijke stappen om te onthouden tijdens het aanpassen van uw HDInsight-cluster.
   
   * Vergeet niet de in stap 1 met uw HDInsight-cluster wordt gemaakt bij het maken van het storage-account te koppelen. Dit opslagaccount wordt gebruikt voor toegang tot gegevens die binnen het cluster is verwerkt.
   * Nadat het cluster is gemaakt, kunt u externe toegang inschakelen met het hoofdknooppunt van het cluster. Navigeer naar de **configuratie** tabblad en klik op **externe inschakelen**. Deze stap Hiermee geeft u de referenties van de gebruiker gebruikt voor externe aanmelding.
3. [Maken van een Azure Machine Learning-werkruimte](../studio/create-workspace.md): deze Azure Machine Learning-werkruimte wordt gebruikt voor het bouwen van machine learning-modellen. Deze taak is gericht, na het voltooien van een initiële gegevensverkenning en omlaag steekproeven van het HDInsight-cluster.

## <a name="getdata"></a>De gegevens ophalen uit een openbare bron
> [!NOTE]
> Dit is doorgaans een **Admin** taak.
> 
> 

Kopiëren van de [NYC Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gegevensset aan op de computer van de openbare locatie, u kunt een van de methoden van [gegevens verplaatsen en naar Azure Blob Storage](move-azure-blob.md).

Hier wordt beschreven hoe AzCopy gebruiken om over te dragen van de bestanden met gegevens. Volg de instructies op om te downloaden en installeren van AzCopy, [aan de slag met het AzCopy-opdrachtregelprogramma](../../storage/common/storage-use-azcopy.md).

1. Uitgeven vanuit een opdrachtpromptvenster de volgende opdrachten van AzCopy, vervangen *< path_to_data_folder >* met de gewenste bestemming:

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S

1. Wanneer de kopie is voltooid, zijn in totaal 24 ZIP-bestanden in de map data gekozen. Pak het gedownloade bestanden op in dezelfde map op uw lokale machine. Noteer de map waar de niet-gecomprimeerde bestanden zich bevinden. Deze map worden aangeduid als de *< pad\_naar\_unzipped_data\_bestanden\>*  is het volgende.

## <a name="upload"></a>De gegevens uploaden naar de standaardcontainer van Azure HDInsight Hadoop-cluster
> [!NOTE]
> Dit is doorgaans een **Admin** taak.
> 
> 

In de volgende opdrachten uit AzCopy, vervangt u de volgende parameters met de werkelijke waarden die u hebt opgegeven bij het maken van het Hadoop-cluster en de gegevensbestanden ritsen.

* ***&#60; path_to_data_folder >*** de map (samen met het pad) op uw computer waarin de uitgepakte bestanden  
* ***&#60; opslagaccountnaam van Hadoop-cluster >*** het storage-account die is gekoppeld aan uw HDInsight-cluster
* ***&#60; standaardcontainer van Hadoop-cluster >*** de standaardcontainer gebruikt door het cluster. Houd er rekening mee dat de naam van de standaardcontainer meestal dezelfde naam als het cluster zelf is. Als het cluster wordt 'abc123.azurehdinsight.net' genoemd, is de standaardcontainer abc123.
* ***&#60; opslagaccountsleutel >*** de sleutel voor het opslagaccount dat wordt gebruikt door het cluster

Voer de volgende twee AzCopy-opdrachten uit vanaf de opdrachtprompt of een Windows PowerShell-venster op de computer.

Met deze opdracht uploadt de gegevens reis naar ***nyctaxitripraw*** map in de standaardcontainer van het Hadoop-cluster.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxitripraw /DestKey:<storage account key> /S /Pattern:trip_data_*.csv

Met deze opdracht uploadt het tarief dat gegevens naar ***nyctaxifareraw*** map in de standaardcontainer van het Hadoop-cluster.

        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:<path_to_unzipped_data_files> /Dest:https://<storage account name of Hadoop cluster>.blob.core.windows.net/<default container of Hadoop cluster>/nyctaxifareraw /DestKey:<storage account key> /S /Pattern:trip_fare_*.csv

De gegevens moet nu in Azure Blob Storage en klaar om te worden gebruikt binnen het HDInsight-cluster.

## <a name="#download-hql-files"></a>Meld u aan bij het hoofdknooppunt van Hadoop-cluster en bereid voor experimentele data-analyse
> [!NOTE]
> Dit is doorgaans een **Admin** taak.
> 
> 

Voor toegang tot het hoofdknooppunt van het cluster voor experimentele data-analyse en down-steekproeven van de gegevens, volgt u de procedure beschreven in [toegang tot de hoofd-knooppunt van Hadoop-Cluster](customize-hadoop-cluster.md).

In dit scenario, gebruiken we voornamelijk query's die zijn geschreven [Hive](https://hive.apache.org/), een SQL-achtige query language voorlopige gegevens explorations uitvoeren. Het Hive-query's worden opgeslagen in .hql bestanden. We vervolgens omlaag-voorbeeld deze gegevens naar Azure Machine Learning worden gebruikt voor het ontwikkelen van modellen.

Als u het cluster voorbereiden voor experimentele data-analyse, we de .hql-bestanden met de relevante Hive-scripts uit downloaden [github](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) naar een lokale map (C:\temp) op het hoofdknooppunt. Om dit te doen, opent u de **opdrachtprompt** van binnen het hoofdknooppunt van het cluster en de volgende twee opdrachten uitgeven:

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/DataScienceProcess/DataScienceScripts/Download_DataScience_Scripts.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

Deze twee opdrachten downloaden alle .hql bestanden die nodig zijn in dit scenario naar een lokale map ***C:\temp &#92;*** in het hoofdknooppunt.

## <a name="#hive-db-tables"></a>Hive-database en per maand gepartitioneerde tabellen maken
> [!NOTE]
> Dit is doorgaans een **Admin** taak.
> 
> 

Er zijn nu gereed voor het maken van Hive-tabellen voor de NYC taxi gegevensset.
Open in het hoofdknooppunt van het Hadoop-cluster, de ***Hadoop-opdrachtregel*** op het bureaublad van het hoofdknooppunt en voer de Hive-map met de opdracht

    cd %hive_home%\bin

> [!NOTE]
> **Voer alle Hive-opdrachten in dit scenario uit de bovenstaande Hive-opslaglocatie / directory prompt. Dit zorgt voor problemen die pad automatisch. We gebruiken de termen 'Hive directory prompt', ' Hive bin / directory prompt ', en "Hadoop-opdrachtregel ' door elkaar in dit scenario.**
> 
> 

Voer de volgende opdracht in Hadoop opdrachtregel van het hoofdknooppunt van het verzenden van de Hive-query om Hive-database en tabellen te maken van de directory Hive-prompt:

    hive -f "C:\temp\sample_hive_create_db_and_tables.hql"

Hier volgt de inhoud van de ***C:\temp\sample\_hive\_maken\_db\_en\_tables.hql*** -bestand dat wordt gemaakt van Hive database ***nyctaxidb*** en tabellen ***reis*** en ***tarief***.

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

* de tabel 'trip' bevat reis details van elke rijpositie (ophalen tijd stuurprogrammagegevens reis afstand en tijden)
* de tabel 'tarief' bevat tarief details (tip bedrag tarief bedrag tolgelden en toeslagen).

Als u met deze procedures als u meer hulp nodig hebt of wilt onderzoeken alternatieve waarden, Zie de sectie [indienen Hive-query's rechtstreeks vanaf de opdrachtregel van Hadoop](move-hive-tables.md#submit).

## <a name="#load-data"></a>Laden van gegevens met Hive-tabellen partities
> [!NOTE]
> Dit is doorgaans een **Admin** taak.
> 
> 

De NYC taxi gegevensset heeft een natuurlijke partitioneren per maand, die we gebruiken om in te schakelen sneller verwerking en queryprestaties. De volgende PowerShell-opdrachten (uitgegeven vanuit de Hive-map met de **Hadoop-opdrachtregel**) laden van gegevens naar de 'trip' en 'tarief' Hive-tabellen gepartitioneerd per maand.

    for /L %i IN (1,1,12) DO (hive -hiveconf MONTH=%i -f "C:\temp\sample_hive_load_data_by_partitions.hql")

De *voorbeeld\_hive\_laden\_gegevens\_door\_partitions.hql* bestand bevat de volgende **laden** opdrachten:

    LOAD DATA INPATH 'wasb:///nyctaxitripraw/trip_data_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.trip PARTITION (month=${hiveconf:MONTH});
    LOAD DATA INPATH 'wasb:///nyctaxifareraw/trip_fare_${hiveconf:MONTH}.csv' INTO TABLE nyctaxidb.fare PARTITION (month=${hiveconf:MONTH});

Opmerking een aantal Hive-query's die we hier in het proces exploratie gebruiken betrekking hebben op slechts één partitie of op een aantal partities opzoeken. Maar deze query's kunnen worden uitgevoerd over de volledige gegevens.

### <a name="#show-db"></a>Databases weergeven in de HDInsight Hadoop-cluster
Als u wilt weergeven van de databases die zijn gemaakt in HDInsight Hadoop-cluster in het Hadoop-opdrachtregelvenster, voer de volgende opdracht in de Hadoop-opdrachtregel:

    hive -e "show databases;"

### <a name="#show-tables"></a>Het Hive-tabellen in de database nyctaxidb weergeven
Als de tabellen in de database nyctaxidb weergeven, voer de volgende opdracht in de Hadoop-opdrachtregel:

    hive -e "show tables in nyctaxidb;"

We kunt bevestigen dat de tabellen worden gepartitioneerd door de onderstaande opdracht:

    hive -e "show partitions nyctaxidb.trip;"

De verwachte uitvoer wordt hieronder weergegeven:

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

Op deze manier kunt we ervoor zorgen dat de tarief-tabel is gepartitioneerd door de onderstaande opdracht:

    hive -e "show partitions nyctaxidb.fare;"

De verwachte uitvoer wordt hieronder weergegeven:

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
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

Gegevensverkenning en taken voor de gegevens in de Hive-tabellen geladen engineering-functie kunnen worden gerealiseerd met Hive-query's. Hier volgen enkele voorbeelden van dergelijke taken dat we in deze sectie doorlopen:

* De top 10 records weergeven in beide tabellen.
* Verken de gegevens distributies van enkele velden in verschillende tijdvensters.
* Onderzoek de kwaliteit van de gegevens van de breedtegraad -velden.
* Genereren van binaire en multiklassen classificatielabels op basis van de **tip\_bedrag**.
* Functies door de directe trip afstanden computing genereren.

### <a name="exploration-view-the-top-10-records-in-table-trip"></a>Exploratie: De top 10 records in de tabel reis weergeven
> [!NOTE]
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

Als u wilt zien hoe de gegevens eruit ziet, controleert u 10 records uit elke tabel. De volgende twee query's afzonderlijk uitvoeren van de opdrachtprompt van de map Hive in de console Hadoop vanaf de opdrachtregel om te controleren van de records.

De top 10 records in de tabel 'trip' van de eerste maand ophalen:

    hive -e "select * from nyctaxidb.trip where month=1 limit 10;"

De top 10 records in de tabel 'tarief' van de eerste maand ophalen:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;"

Is het vaak nuttig voor het opslaan van de records in een bestand om handige weer te geven. Een kleine wijziging in de voorgaande query bewerkstelligt dit:

    hive -e "select * from nyctaxidb.fare where month=1 limit 10;" > C:\temp\testoutput

### <a name="exploration-view-the-number-of-records-in-each-of-the-12-partitions"></a>Exploratie: Het aantal records weergeven in elk van de partities 12
> [!NOTE]
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

Van belang is hoe het aantal reizen varieert tijdens het kalenderjaar. Groeperen per maand kunt laten zien hoe deze verdeling van reizen eruit.

    hive -e "select month, count(*) from nyctaxidb.trip group by month;"

Dit biedt ons de uitvoer:

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

We kunnen ook het totale aantal records in onze gegevensset reis tellen door de volgende opdracht bij de prompt Hive-directory:

    hive -e "select count(*) from nyctaxidb.trip;"

Dit levert:

    173179759
    Time taken: 284.017 seconds, Fetched: 1 row(s)

Vergelijkbaar met die wordt weergegeven voor de gegevensset reis opdrachten kunnen we Hive-query's vanaf de opdrachtprompt Hive directory voor de gegevensset tarief voor het valideren van het aantal records uitgeven.

    hive -e "select month, count(*) from nyctaxidb.fare group by month;"

Dit biedt ons de uitvoer:

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

Telling van het totale aantal records in de gegevensset tarief dat kan worden gedaan met de volgende opdracht achter de opdrachtprompt van de directory Hive:

    hive -e "select count(*) from nyctaxidb.fare;"

Dit levert:

    173179759
    Time taken: 186.683 seconds, Fetched: 1 row(s)

Het totale aantal records in beide tabellen is ook hetzelfde. Dit biedt een tweede validatie van de gegevens correct zijn geladen.

### <a name="exploration-trip-distribution-by-medallion"></a>Exploratie: Reis distributie door straten
> [!NOTE]
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

In dit voorbeeld identificeert de straten (taxi getallen) met meer dan 100 reizen binnen een bepaalde periode. De query profiteert van de gepartitioneerde tabeltoegang omdat deze wordt waarbij door de variabele partitie **maand**. De queryresultaten worden geschreven naar een lokaal bestand queryoutput.tsv in `C:\temp` op het hoofdknooppunt.

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

Hier volgt de inhoud van *voorbeeld\_hive\_reis\_aantal\_door\_medallion.hql* -bestand voor inspectie.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

De straten in de gegevensset van de NYC taxi identificeert een unieke CAB-bestand. We kunnen identificeren welke cab-bestanden zijn 'bezet' door vragen aan welke u meer dan een bepaald aantal reizen aangebracht in een bepaalde periode. Het volgende voorbeeld identificeert cabinetbestanden die meer dan honderd reizen aangebracht in de eerste drie maanden en slaat de queryresultaten naar een lokaal bestand C:\temp\queryoutput.tsv.

Hier volgt de inhoud van *voorbeeld\_hive\_reis\_aantal\_door\_medallion.hql* -bestand voor inspectie.

    SELECT medallion, COUNT(*) as med_count
    FROM nyctaxidb.fare
    WHERE month<=3
    GROUP BY medallion
    HAVING med_count > 100
    ORDER BY med_count desc;

Geef de volgende opdracht vanaf de opdrachtprompt Hive-map:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion.hql" > C:\temp\queryoutput.tsv

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Exploratie: Reis distributie door straten en hack_license
> [!NOTE]
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

Wanneer een dataset verkennen, willen we vaak het nummer van co-exemplaren van groepen van waarden te onderzoeken. Deze sectie bevat een voorbeeld van hoe u dit doet voor CAB-bestanden en stuurprogramma's.

De *voorbeeld\_hive\_reis\_aantal\_door\_straten\_license.hql* bestand groepen van de gegevensset tarief op 'straten' en 'hack_license' en retourneert de aantallen voor elke combinatie. Hier volgen de inhoud ervan:

    SELECT medallion, hack_license, COUNT(*) as trip_count
    FROM nyctaxidb.fare
    WHERE month=1
    GROUP BY medallion, hack_license
    HAVING trip_count > 100
    ORDER BY trip_count desc;

Deze query retourneert CAB-bestand en een bepaald stuurprogramma combinaties gesorteerd op aflopende aantal heen.

Vanaf de opdrachtprompt van de map Hive uitvoeren:

    hive -f "C:\temp\sample_hive_trip_count_by_medallion_license.hql" > C:\temp\queryoutput.tsv

De queryresultaten worden geschreven naar een lokaal bestand C:\temp\queryoutput.tsv.

### <a name="exploration-assessing-data-quality-by-checking-for-invalid-longitudelatitude-records"></a>Exploratie: Beoordeling van de kwaliteit van de gegevens door te controleren op Ongeldige lengtegraad/breedtegraad records
> [!NOTE]
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

Een algemene doel van experimentele gegevensanalyse is wieden ongeldige of onjuiste records. In het voorbeeld in deze sectie bepaalt of de lengte of de breedte velden een waarde ver buiten het gebied van de NYC bevatten. Aangezien het waarschijnlijk dat dergelijke records de waarde van een foutieve lengtegraad-breedtegraad hebben, willen we deze alle gegevens die moet worden gebruikt voor het modelleren te elimineren.

Hier volgt de inhoud van *voorbeeld\_hive\_kwaliteit\_assessment.hql* -bestand voor inspectie.

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
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

Voor de binair klassificatieprobleem die worden beschreven in de [voorbeelden van voorspelling taken](hive-walkthrough.md#mltasks) sectie is het handig te weten of een tip of niet is opgegeven. Deze verdeling van tips is binaire:

* Tip gegeven (klasse 1, tip\_bedrag > $0)  
* Er is geen tip (klasse 0, tip\_bedrag = $0).

De volgende *voorbeeld\_hive\_punt\_frequencies.hql* dit bestand wordt:

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
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

Voor het probleem multiklassen classificatie is die worden beschreven in de [voorbeelden van voorspelling taken](hive-walkthrough.md#mltasks) sectie deze gegevensset ook gepaard met een natuurlijke classificatie waar willen we het bedrag van de opgegeven tips voorspellen. We kunnen opslaglocaties gebruiken voor het definiëren van tip bereiken in de query. Als u de klasse-Distributies voor de verschillende bereiken tip, gebruiken we de *voorbeeld\_hive\_tip\_bereik\_frequencies.hql* bestand. Hier vindt u de inhoud ervan.

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

Voer de volgende opdracht uit vanaf de opdrachtregel Hadoop-console:

    hive -f "C:\temp\sample_hive_tip_range_frequencies.hql"

### <a name="exploration-compute-direct-distance-between-two-longitude-latitude-locations"></a>Exploratie: Compute Direct afstand tussen twee lengtegraad breedtegraad-locaties
> [!NOTE]
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

Met een meting van de directe afstand kan wij vinden het verschil tussen deze en de werkelijke reis afstand. We stimuleren dit onderdeel door af te wijzen dat passagiers mogelijk minder waarschijnlijk tip als ze weten dat het stuurprogramma opzettelijk door een veel langer route getroffen.

Om te zien van de vergelijking tussen de werkelijke reis afstand en de [Haversine afstand](http://en.wikipedia.org/wiki/Haversine_formula) tussen twee lengtegraad breedtegraad punten (de afstand van de 'groot cirkel'), gebruiken we de goniometrische functies beschikbaar binnen Hive, dus:

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

In de voorgaande query R is de radius van de aarde in mijl en pi wordt omgezet in radialen. Houd er rekening mee dat de lengtegraad breedtegraad punten zijn 'gefilterd' om de waarden die ver van het gebied van de NYC te verwijderen.

In dit geval schrijven we de resultaten naar een map met de naam 'queryoutputdir'. De reeks opdrachten hieronder weergegeven maakt eerst deze uitvoermap en vervolgens de Hive-opdracht wordt uitgevoerd.

Vanaf de opdrachtprompt van de map Hive uitvoeren:

    hdfs dfs -mkdir wasb:///queryoutputdir

    hive -f "C:\temp\sample_hive_trip_direct_distance.hql"


De queryresultaten worden geschreven naar negen Azure blobs ***queryoutputdir/000000\_0*** naar ***queryoutputdir/000008\_0*** onder de standaardcontainer van het Hadoop-cluster.

Overzicht van de grootte van de afzonderlijke blobs uitvoeren we de volgende opdracht achter de opdrachtprompt van de directory Hive:

    hdfs dfs -ls wasb:///queryoutputdir

Overzicht van de inhoud van een bepaald bestand, spreken 000000\_0, gebruiken we de Hadoop `copyToLocal` opdracht dus.

    hdfs dfs -copyToLocal wasb:///queryoutputdir/000000_0 C:\temp\tempfile

> [!WARNING]
> `copyToLocal`erg langzaam voor grote bestanden en wordt niet aanbevolen voor gebruik met deze.  
> 
> 

Een groot voordeel van deze gegevens zich bevinden in een Azure-blob is dat besproken voor de gegevens in Azure Machine Learning met behulp van mogelijk de [importgegevens] [ import-data] module.

## <a name="#downsample"></a>Omlaag gegevens en build samplemodellen in Azure Machine Learning
> [!NOTE]
> Dit is doorgaans een **gegevens wetenschappelijk** taak.
> 
> 

Na de analysefase experimentele gegevens bent er nu klaar om de gegevens voor het bouwen van modellen in Azure Machine Learning down-voorbeeld. In deze sectie laten we zien hoe u een Hive-query met de gegevens down-voorbeeld. Deze vervolgens toegankelijk is vanuit de [importgegevens] [ import-data] -module in Azure Machine Learning.

### <a name="down-sampling-the-data"></a>Omlaag steekproef nemen van de gegevens
Er zijn twee stappen in deze procedure. Eerst we toevoegen de **nyctaxidb.trip** en **nyctaxidb.fare** tabellen op de drie sleutels die aanwezig in alle records zijn: 'straten', ' inbreken\_licentie ', en "ophalen\_datetime-waarde '. Er vervolgens een label binaire classificatie gegenereerd **punt** en een label met meerdere klasse classificatie **tip\_klasse**.

Kunnen de omlaag actieve gegevens rechtstreeks vanuit de [importgegevens] [ import-data] module in Azure Machine Learning, is het nodig zijn voor het opslaan van de resultaten van de voorgaande query naar een interne Hive-tabel. In het volgende, we een interne Hive-tabel maken en vullen met de gekoppelde en omlaag actieve gegevens de inhoud ervan.

De query is van toepassing standaardfuncties Hive rechtstreeks voor het genereren van het uur van de dag, week van jaar werkdag (1 staat voor maandag en 7 staat voor zondag) van de ' ophalen\_datetime ' veld en de directe afstand tussen de locaties ophalen en dropoff. Gebruikers kunnen verwijzen naar [LanguageManual UDF](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) voor een volledige lijst van deze functies.

De query vervolgens omlaag-samples de gegevens zodat de queryresultaten in Azure Machine Learning Studio inpassen kunnen. Alleen ongeveer 1% van de oorspronkelijke gegevensset wordt geïmporteerd in de Studio.

Hier vindt u de inhoud van *voorbeeld\_hive\_voorbereiden\_voor\_aml\_full.hql* worden gegevens voorbereid voor model bouwen in Azure Machine Learning-bestand:

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

We hebt nu een interne tabel 'nyctaxidb.nyctaxi_downsampled_dataset', wat kan worden geopend met behulp van de [importgegevens] [ import-data] module op basis van Azure Machine Learning. We kunnen deze gegevensset bovendien gebruiken voor het bouwen van Machine Learning-modellen.  

### <a name="use-the-import-data-module-in-azure-machine-learning-to-access-the-down-sampled-data"></a>De gegevens importeren-module gebruiken in Azure Machine Learning om omlaag actieve gegevens
Vereisten voor het uitgeven van Hive query's in de [importgegevens] [ import-data] -module van Azure Machine Learning, moeten we toegang tot een Azure Machine Learning-werkruimte. Er moet ook toegang tot de referenties van het cluster en de gekoppelde storage-account.

Sommige gegevens op de [importgegevens] [ import-data] module en de parameters voor het invoeren van:

**HCatalog server-URI**: als de clusternaam abc123 is, is dit gewoon: https://abc123.azurehdinsight.net

**Hadoop-gebruikersaccountnaam**: de naam van de gebruiker gekozen voor het cluster (**niet** de gebruikersnaam voor externe toegang)

**Hadoop ser accountwachtwoord**: het wachtwoord voor het cluster gekozen (**niet** het wachtwoord voor externe toegang)

**Locatie van uitvoergegevens**: dit is gekozen als Azure.

**Naam van het Azure-opslagaccount**: naam van het standaardopslagaccount die is gekoppeld aan het cluster.

**Azure containernaam**: dit is de standaardnaam van de container voor het cluster en is meestal hetzelfde als de naam van het cluster. Voor een cluster 'abc123' genoemd, is dit alleen abc123.

> [!IMPORTANT]
> **Elke tabel wilt opvragen met behulp van de [importgegevens] [ import-data] module in Azure Machine Learning een interne tabel moet zijn.** Een tip om te bepalen of een tabel T in een database D.db een interne tabel is is als volgt.
> 
> 

Geef de opdracht uit vanaf de opdrachtprompt Hive-map:

    hdfs dfs -ls wasb:///D.db/T

Als de tabel een interne tabel is en dit ingevuld, wordt de inhoud moeten hier weergegeven. Er is een andere manier om te bepalen of een tabel een interne tabel is met de Azure Storage Explorer. Gebruik het navigeren naar de standaardnaam van de container van het cluster en vervolgens filteren op naam van de tabel. Als de tabel en de inhoud ervan wordt getoond, bevestigt dit dat het is een interne tabel.

Hier volgt een momentopname van de Hive-query en de [importgegevens] [ import-data] module:

![Hive-query voor gegevens importeren-module](./media/hive-walkthrough/1eTYf52.png)

Omdat onze gegevens omlaag dat zich in de standaardcontainer, de resulterende Hive-query uit Azure Machine Learning is zeer eenvoudig en is alleen een "Selecteer * uit nyctaxidb.nyctaxi\_verkleind\_gegevens '.

De gegevensset kan nu worden gebruikt als startpunt voor het bouwen van Machine Learning-modellen.

### <a name="mlmodel"></a>Bouwen van modellen in Azure Machine Learning
Er wordt nu voortgezet model bouwen en implementeren van modellen in [Azure Machine Learning](https://studio.azureml.net). De gegevens is gereed voor gebruik bij het oplossen van de voorspelling problemen hierboven:

**1. Binaire classificatie**: om te voorspellen of een tip betaald is voor een reis.

**Cursist gebruikt:** Two-class logistic regression

a. Voor dit probleem op door is het doel (of een klasse)-label 'punt'. De oorspronkelijke omlaag actieve gegevensset heeft een aantal kolommen die doel lekken voor dit experiment classificatie. Met name: tip\_klasse, tip\_bedrag en totaal\_bedrag onthullen informatie over het doellabel is niet beschikbaar voor het testen van tijd. We deze kolommen verwijderen uit overweging met behulp van de [Select Columns in Dataset] [ select-columns] module.

De momentopname van de volgende ziet u ons experiment te voorspellen of een tip voor een bepaalde reis is betaald:

![Experiment momentopname](./media/hive-walkthrough/QGxRz5A.png)

b. Ons doel label distributies zijn voor dit experiment ongeveer 1:1.

De volgende momentopname toont de distributie van tip labels van de klasse voor het probleem binaire classificatie:

![Distributie van tip klasse labels](./media/hive-walkthrough/9mM4jlD.png)

Als gevolg hiervan verkrijgen we een AUC van 0.987 zoals in de volgende afbeelding:

![AUC waarde](./media/hive-walkthrough/8JDT0F8.png)

**2. Multiklassen classificatie**: om te voorspellen van het bereik van tip bedragen betaald voor onderweg, met de eerder gedefinieerde klassen.

**Cursist gebruikt:** Multiklassen logistic regression

a. Voor dit probleem op door het label voor onze doel (of een klasse) is ' tip\_klasse ' die kan duren voordat een van vijf waarden (0,1,2,3,4). Net als bij de binaire classificatie hebben we enkele kolommen die doel lekken voor dit experiment. Met name: punt, tip\_bedrag totale\_bedrag onthullen informatie over het doellabel is niet beschikbaar voor het testen van tijd. Verwijderen we deze kolommen met behulp van de [Select Columns in Dataset] [ select-columns] module.

De momentopname van de volgende ziet u het experiment te voorspellen, in welke opslaglocatie is een tip waarschijnlijk valt (klasse 0: tip = $0, 1 klasse: tip > $0 en tip < $5, klasse 2 =: tip > $5 en tip < = $10, klasse 3: tip > $10 en tip < = $20 Klasse 4: tip > $20)

![Experiment momentopname](./media/hive-walkthrough/5ztv0n0.png)

Nu zien hoe de distributie van de klasse werkelijke test eruit ziet. Ziet u dat hoewel klasse 0 en 1 van de klasse een bekende, de andere klassen zeldzame zijn.

![Klasse distributie testen](./media/hive-walkthrough/Vy1FUKa.png)

b. We gebruiken een matrix verwarring om te kijken naar de accuratesse voorspelling voor dit experiment. Dit wordt hier weergegeven:

![Verwarring matrix](./media/hive-walkthrough/cxFmErM.png)

Houd er rekening mee dat de klasse accuratesse op de gangbare klassen zijn behoorlijk goed, het model wordt niet goed werk 'learning' op de zeldzame klassen.

**3. Regressie taak**: om te voorspellen van de hoeveelheid tip voor een reis betaald.

**Cursist gebruikt:** Boosted decision tree

a. Voor dit probleem op door het doel (of een klasse)-label is ' tip\_bedrag '. De doel-lekken in dit geval zijn: punt, tip\_klasse, totale\_bedrag; deze variabelen die zijn op basis van informatie over het bedrag tip die doorgaans niet beschikbaar bij het testen van de tijd is duidelijk. Verwijderen we deze kolommen met behulp van de [Select Columns in Dataset] [ select-columns] module.

De momentopname van de volgende ziet u het experiment te voorspellen, het bedrag van de opgegeven tip:

![Experiment momentopname](./media/hive-walkthrough/11TZWgV.png)

b. We meten het accuratesse van de voorspelling door te kijken naar de gekwadrateerde fout in de voorspellingen, de determinatiecoëfficiënt en dergelijke regressie problemen oplossen:

![Voorspelling statistieken](./media/hive-walkthrough/Jat9mrz.png)

Door de coëfficiënten model wordt: de overdracht van ongeveer 71% van de variantie zien we dat 0.709 over de determinatiecoëfficiënt is, uitgelegd.

> [!IMPORTANT]
> Raadpleeg voor meer informatie over Azure Machine Learning en hoe u toegang tot en gebruik dit [wat is Machine Learning](../studio/what-is-machine-learning.md). Een zeer nuttig resource voor het afspelen van met een aantal Machine Learning-experimenten in Azure Machine Learning is de [AI-galerie van Azure](https://gallery.cortanaintelligence.com/). De galerie bevat informatie over een kleurenbereik van experimenten en biedt een uitgebreide inleiding in het bereik van de mogelijkheden van Azure Machine Learning.
> 
> 

## <a name="license-information"></a>Licentie-informatie
In dit voorbeeld scenario en de bijbehorende scripts worden gedeeld door Microsoft onder de MIT-licentie. Controleer het bestand LICENSE.txt in de map van de voorbeeldcode op GitHub voor meer informatie.

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
