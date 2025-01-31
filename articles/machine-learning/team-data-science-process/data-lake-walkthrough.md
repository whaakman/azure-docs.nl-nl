---
title: Schaalbare Gegevenswetenschap met Azure Data Lake - Team Data Science Process
description: Het gebruik van Azure Data Lake gegevens verkennen en binaire classificatie taken uitvoeren op een gegevensset.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: cc37109eda2690b4407f9cd0c92851b7c0e3f915
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60399785"
---
# <a name="scalable-data-science-with-azure-data-lake-an-end-to-end-walkthrough"></a>Schaalbare Gegevenswetenschap met Azure Data Lake: Een end-to-end-overzicht
In dit scenario ziet u hoe u Azure Data Lake gegevens verkennen en binaire classificatie-taken op een voorbeeld van de NYC taxi reis en ritbedrag gegevensset om te voorspellen of een tip door een fare wordt betaald. Dit leidt u door de stappen van de [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/), end-to-end, in gegevens ophalen als model voor training en vervolgens naar de implementatie van een webservice die het model publiceert.

### <a name="azure-data-lake-analytics"></a>Azure Data Lake Analytics
De [Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) heeft alle mogelijkheden die nodig zijn om het voor gegevenswetenschappers eenvoudiger voor het opslaan van gegevens van elke grootte, vorm en snelheid en voor het uitvoeren van gegevensverwerking, geavanceerde analyses en machine learning modellen met hoge schaalbaarheid in een rendabele manier.   U betaalt op basis van per taak alleen wanneer de gegevens daadwerkelijk wordt verwerkt. Azure Data Lake Analytics bevat U-SQL, een taal die de declaratieve aard van SQL met de expressieve kracht van C# voor schaalbare combineert gedistribueerde query-mogelijkheden. Dit kunt u niet-gestructureerde gegevens worden verwerkt door het schema toepassen op lezen, aangepaste logica en de gebruiker gedefinieerde functies (UDF's) in te voegen en uitbreidbaarheid om in te schakelen fijnmazige controle over het uitvoeren van op grote schaal bevat. Zie voor meer informatie over het ontwerpplan achter U-SQL, [blogbericht voor Visual Studio](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).

Data Lake Analytics is ook een belangrijk onderdeel van Cortana Analytics Suite en werkt met Azure SQL Data Warehouse, Power BI en Data Factory. Dit biedt u een volledige big data en geavanceerde analyse-platform.

In dit scenario begint met het vindt u de procedure voor het installeren van de vereisten en bronnen die nodig zijn voor de data science process taken uit te voeren. En vervolgens het geeft een overzicht van de gegevensverwerking stappen met U-SQL en tot slot biedt het die laat zien hoe u Python en Hive gebruiken met Azure Machine Learning Studio om te bouwen en de voorspellende modellen te implementeren.

### <a name="u-sql-and-visual-studio"></a>U-SQL en Visual Studio
In dit scenario raadt het gebruik van Visual Studio om te bewerken van U-SQL-scripts voor het verwerken van de gegevensset. De U-SQL-scripts zijn die hier worden beschreven en vindt u in een afzonderlijk bestand. Het proces omvat het opnemen, verkennen en meting van de gegevens. U ziet ook hoe u kunt een taak met U-SQL-script uitvoeren vanuit Azure portal. Hive-tabellen worden gemaakt voor de gegevens in een gekoppelde HDInsight-cluster om het bouwen en de implementatie van een binair classificeringsmodel in Azure Machine Learning Studio mogelijk te maken.

### <a name="python"></a>Python
In dit scenario bevat ook een sectie waarin wordt uitgelegd hoe te bouwen en implementeren van een Voorspellend model met behulp van Python met Azure Machine Learning Studio. Het biedt een Jupyter-notebook met de Python-scripts voor de stappen in dit proces. De notebook bevat code voor een extra functie engineering stappen en modellen bouwen zoals multiklassen classificatie- en regressiemodellen modelleren naast de binaire classificeringsmodel die hier wordt beschreven. De regressie-taak is het voorspellen van het bedrag van de tip op basis van andere tip-functies.

### <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning Studio wordt gebruikt om te bouwen en de voorspellende modellen te implementeren. Dit wordt gedaan met behulp van twee methoden: eerst met Python-scripts en klik vervolgens met Hive-tabellen in een cluster met HDInsight (Hadoop).

### <a name="scripts"></a>Scripts
Alleen de belangrijkste stappen worden beschreven in dit scenario. U kunt de volledige downloaden **U-SQL-script** en **Jupyter-Notebook** van [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

## <a name="prerequisites"></a>Vereisten
Voordat u deze onderwerpen begint, moet u het volgende hebt:

* Een Azure-abonnement. Als u nog geen een, Zie [gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* (Aanbevolen) Visual Studio 2013 of later. Als u nog geen een van deze versies zijn geïnstalleerd, kunt u een gratis communityversie van downloaden [Visual Studio Community](https://www.visualstudio.com/vs/community/).

> [!NOTE]
> U kunt ook de Azure-portal naar Azure Data Lake-query's verzenden in plaats van Visual Studio gebruiken. Instructies vindt u op hoe u dus beide met Visual Studio en op de portal in de sectie met de titel **gegevens te verwerken met U-SQL**.
>
>


## <a name="prepare-data-science-environment-for-azure-data-lake"></a>Data science-omgeving voor Azure Data Lake voorbereiden
Als u wilt voorbereiden van de data science-omgeving voor dit scenario, maken de volgende bronnen:

* Azure Data Lake Store (ADLS)
* Azure Data Lake Analytics (ADLA)
* Azure Blob storage-account
* Azure Machine Learning Studio-account
* Azure Data Lake Tools voor Visual Studio (aanbevolen)

Deze sectie vindt u instructies voor het maken van elk van deze resources. Als u wilt gebruiken van Hive-tabellen met Azure Machine Learning, in plaats van Python, voor het bouwen van een model, moet u ook het inrichten van een cluster met HDInsight (Hadoop). Deze alternatieve procedure beschreven in de sectie optie 2.


> [!NOTE]
> De **Azure Data Lake Store** kunnen worden gemaakt afzonderlijk of bij het maken van de **Azure Data Lake Analytics** als de standaardopslag. Instructies voor het maken van elk van deze resources afzonderlijk wordt verwezen, maar het Data Lake storage-account moet niet afzonderlijk worden gemaakt.
>
>

### <a name="create-an-azure-data-lake-store"></a>Maken van een Azure Data Lake Store


Maken van een ADLS uit de [Azure-portal](https://portal.azure.com). Zie voor meer informatie, [een HDInsight-cluster maken met Data Lake Store met behulp van Azure portal](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md). Zorg ervoor dat het instellen van de identiteit van de AAD-Cluster in de **DataSource** blade van de **optionele configuratie** blade er beschreven.

 ![3](./media/data-lake-walkthrough/3-create-ADLS.PNG)

### <a name="create-an-azure-data-lake-analytics-account"></a>Een Azure Data Lake Analytics-account maken
Maak een ADLA-account van de [Azure-portal](https://portal.azure.com). Zie voor meer informatie, [zelfstudie: aan de slag met Azure Data Lake Analytics met Azure portal](../../data-lake-analytics/data-lake-analytics-get-started-portal.md).

 ![4](./media/data-lake-walkthrough/4-create-ADLA-new.PNG)

### <a name="create-an-azure-blob-storage-account"></a>Een Azure Blob storage-account maken
Maken van een Azure Blob storage-account van de [Azure-portal](https://portal.azure.com). Zie voor meer informatie, de maken een sectie van de storage-account in [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md).

 ![5](./media/data-lake-walkthrough/5-Create-Azure-Blob.PNG)

### <a name="set-up-an-azure-machine-learning-studio-account"></a>Een Azure Machine Learning Studio-account instellen
Aanmelden bij Azure Machine Learning Studio uit de [Azure Machine Learning studio](https://azure.microsoft.com/services/machine-learning/) pagina. Klik op de **nu aan de slag** knop en kies vervolgens een 'Gratis werkruimte' of 'Standard-werkruimte'. Nu u klaar om experimenten te maken in Azure Machine Learning studio.

### <a name="install-azure-data-lake-tools-recommended"></a>Azure Data Lake Tools (aanbevolen) installeren
Azure Data Lake Tools installeren voor uw versie van Visual Studio van [Azure Data Lake Tools voor Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).

 ![6](./media/data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

Nadat de installatie voltooid is, opent u Visual Studio. U ziet de Data Lake-tabblad van het menu aan de bovenkant. Uw Azure-resources moeten worden weergegeven in het linkerpaneel, wanneer u zich bij uw Azure-account aanmeldt.

 ![7](./media/data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)

## <a name="the-nyc-taxi-trips-dataset"></a>De gegevensset NYC Taxi Trips
De hier gebruikte gegevensset is een openbaar beschikbare gegevensset--de [NYC Taxi Trips gegevensset](https://www.andresmh.com/nyctaxitrips/). De reisgegevens NYC Taxi bestaat uit ongeveer 20 GB gecomprimeerde CSV-bestanden (~ 48 GB niet-gecomprimeerd), voor elke reis vastleggen van meer dan 173 miljoen afzonderlijke trips en de tarieven betalen. Elke record van de fietstocht bevat de locaties ophalen en dropoff en tijden, geanonimiseerde hack (stuurprogramma van) het licentienummer en het nummer van de straten (taxi van de unieke ID). De gegevens bevat informatie over alle gegevens in het jaar 2013 en is beschikbaar in de volgende twee gegevenssets voor elke maand:

Trip_data CSV bevat reis details, zoals het aantal personen, ophalen en dropoff punten, duur van de tocht en lengte van de fietstocht. Hier volgen enkele voorbeeldrecords:

       medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868



De 'trip_fare' CSV bevat details van het tarief voor elke reis, zoals betalingstype, fare bedrag, toeslag en belastingen, tips en tolwegen, betaald en de totale hoeveelheid betaald. Hier volgen enkele voorbeeldrecords:

       medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De unieke sleutel voor deelname aan reis\_gegevens en reis\_fare bestaat uit de volgende drie velden: straten hack\_licentie en ophalen van\_datum/tijd. De onbewerkte CSV-bestanden zijn toegankelijk vanuit een openbare Azure storage-blob. De U-SQL-script voor deze koppeling is in de [reis- en fare tabellen samenvoegen](#join) sectie.

## <a name="process-data-with-u-sql"></a>Gegevens verwerken met U-SQL
De taken van de gegevensverwerking geïllustreerd in deze sectie bevatten opnemen, kwaliteit te controleren, verkennen en meting van de gegevens. Hoe reis- en fare tabellen moeten worden samengevoegd, wordt ook weergegeven. De laatste sectie bevat uitvoeren een U-SQL-script-taak vanuit Azure portal. Hier vindt u koppelingen naar elke subsectie:

* [Opname van gegevens: lezen in de gegevens van een openbare blob](#ingest)
* [Data quality controles](#quality)
* [Gegevens verkennen](#explore)
* [Tabellen reis- en fare samenvoegen](#join)
* [Steekproef nemen voor gegevens](#sample)
* [U-SQL-taken worden uitgevoerd](#run)

De U-SQL-scripts zijn die hier worden beschreven en vindt u in een afzonderlijk bestand. U kunt de volledige downloaden **U-SQL-scripts** van [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).

Voor het uitvoeren van U-SQL, Visual Studio openen, klikt u op **bestand--> Nieuw Project-->** , kiest u **U-SQL Project**, een naam en sla deze op een map.

![8](./media/data-lake-walkthrough/8-create-USQL-project.PNG)

> [!NOTE]
> Het is mogelijk het gebruik van de Azure-Portal voor het uitvoeren van U-SQL in plaats van Visual Studio. U kunt navigeren naar de Azure Data Lake Analytics-resource in de portal en verzenden van query's rechtstreeks als geïllustreerd in de volgende afbeelding:
>
>

![9](./media/data-lake-walkthrough/9-portal-submit-job.PNG)

### <a name="ingest"></a>Opname van gegevens: In de gegevens van een openbare blob lezen

De locatie van de gegevens in de Azure-blob wordt verwezen als **wasb://container\_naam\@blob\_opslag\_account\_name.blob.core.windows.net/blob_name**en kunnen worden geëxtraheerd met behulp van **Extractors.Csv()** . Vervangen door uw eigen containernaam en storage-accountnaam in de volgende scripts voor de container\_naam\@blob\_opslag\_account\_naam in het wasb-adres. Aangezien de bestandsnamen in dezelfde indeling, is het mogelijk te gebruiken **reis\_gegevens\_\{\*\}CSV** in alle 12 reis-bestanden worden gelezen.

    ///Read in Trip data
    @trip0 =
        EXTRACT
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

Aangezien er kopteksten in de eerste rij, moet u de headers verwijderen en wijzigen van kolommen van het type in de juiste waarden. U kunt opslaan de verwerkte gegevens voor het gebruik van Azure Data Lake Storage **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ of voor het gebruik van Azure Blob storage account **wasb: / / container_name\@blob_storage_account_name.blob.core.windows.net/blob_name**.

    // change data types
    @trip =
        SELECT
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data to ADL
    OUTPUT @trip
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv();

    ////Output data to blob
    OUTPUT @trip
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();

Op deze manier kunt u lezen in de tarief-gegevenssets. Met de rechtermuisknop op Azure Data Lake Store, kunt u kiezen om te kijken naar uw gegevens in **Azure portal Data Explorer-->** of **Verkenner** vanuit Visual Studio.

 ![10](./media/data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/data-lake-walkthrough/11-data-in-ADL.PNG)

### <a name="quality"></a>Data quality controles
Nadat de reis-en fare zijn gelezen, kunnen data quality controles worden uitgevoerd in de volgende manier. De resulterende CSV-bestanden kunnen worden uitgevoerd naar Azure Blob storage of Azure Data Lake Store.

Het aantal medallions en uniek aantal medallions zoeken:

    ///check the number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;

    @ex_1 =
        SELECT
        pickup_month,
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv();

Deze medallions waarop meer dan 100 trips zoeken:

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv();

Ongeldige records in termen van pickup_longitude zoeken:

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv();

Zoek de ontbrekende waarden voor enkele variabelen:

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;

    @trip_summary6 =
        SELECT
            vendor_id,
        SUM(missing_medallion) AS medallion_empty,
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <a name="explore"></a>Gegevens verkennen
Voer enkele gegevens verkennen met behulp van de volgende scripts in de gegevens beter inzicht te krijgen.

De distributie van Gekantelde en niet-punt trips zoeken:

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;

    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv();

Zoek de verdeling van de tip hoeveelheid met waarden van de deadline: 0, 5, 10 en 20 bedragen.

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv();

Elementaire statistische gegevens van de fietstocht afstand zoeken:

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

Zoek de percentielen van de fietstocht afstand:

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv();


### <a name="join"></a>Tabellen reis- en fare samenvoegen
Reis-en fare kunnen worden samengevoegd door straten, hack_license en pickup_time.

    //join trip and fare table

    @model_data_full =
    SELECT t.*,
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output to blob
    OUTPUT @model_data_full
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv();

    ////output data to ADL
    OUTPUT @model_data_full
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv();


Voor elk niveau van de passagiers aantal, het aantal records, gemiddelde tip bedrag, afwijking van tip bedrag, percentage van Gekantelde trips te berekenen.

    // contingency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <a name="sample"></a>Steekproef nemen voor gegevens
Willekeurig selecteert eerst 0,1% van de gegevens in de gekoppelde tabel:

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;

    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;

    OUTPUT @model_data_random_sample_1_1000
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv();

Voer de toepassing stratificatie steekproeven door binaire variabele tip_class:

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;

    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output to blob
    OUTPUT @model_data_stratified_sample_1_1000
    TO "wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv();
    ////output data to ADL
    OUTPUT @model_data_stratified_sample_1_1000
    TO "swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv();


### <a name="run"></a>U-SQL-taken worden uitgevoerd
Wanneer u klaar bent met het U-SQL-scripts bewerken, kunt u deze verzenden naar de server met behulp van uw Azure Data Lake Analytics-account. Klik op **Data Lake**, **taak verzenden**, selecteer uw **Analytics-Account**, kiest u **parallelle uitvoering**, en klikt u op **verzenden**  knop.

 ![12](./media/data-lake-walkthrough/12-submit-USQL.PNG)

Wanneer de taak met succes is voldaan, wordt de status van uw project in Visual Studio voor het bewaken van weergegeven. Nadat de taak is voltooid, kunt u ook het uitvoeringsproces taak herhalen en ontdek het knelpunt stappen om uw taak efficiëntie te verbeteren. U kunt ook gaan naar Azure-portal om de status van uw U-SQL-taken te controleren.

 ![13](./media/data-lake-walkthrough/13-USQL-running-v2.PNG)

 ![14](./media/data-lake-walkthrough/14-USQL-jobs-portal.PNG)

U kunt nu de uitvoerbestanden in Azure Blob storage of Azure portal controleren. Gebruik de toepassing stratificatie voorbeeldgegevens voor onze modellen in de volgende stap.

 ![15](./media/data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)

## <a name="build-and-deploy-models-in-azure-machine-learning"></a>Modellen in Azure Machine Learning bouwen en implementeren
Er zijn twee opties beschikbaar voor u voor het ophalen van gegevens in Azure Machine Learning om te bouwen en

* In de eerste optie, gebruikt u de samplinggegevens die is geschreven naar een Azure-Blob (in de **steekproeven te nemen** bovenstaande stap) en Python gebruiken om te bouwen en implementeren van modellen van Azure Machine Learning.
* In de tweede optie kunt opvragen u de gegevens in Azure Data Lake rechtstreeks met behulp van een Hive-query. Deze optie vereist dat u een nieuw HDInsight-cluster maken of een bestaand HDInsight-cluster gebruiken wanneer de Hive-tabellen verwijzen naar de gegevens over taxi's NY in Azure Data Lake Storage.  Deze beide opties worden in de volgende secties besproken.

## <a name="option-1-use-python-to-build-and-deploy-machine-learning-models"></a>Optie 1: Python gebruiken om te bouwen en implementeren van machine learning-modellen
Als u wilt bouwen en implementeren van machine learning-modellen met behulp van Python, moet u een Jupyter-Notebook maken op uw lokale computer of in Azure Machine Learning Studio. De Jupyter-Notebook op opgegeven [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) bevat de volledige code voor het verkennen, visualiseren van gegevens, feature-engineering, modellen en implementatie. In dit artikel worden alleen de modellen en de implementatie besproken.

### <a name="import-python-libraries"></a>Python-bibliotheken importeren
Om uit te voeren van het voorbeeld een Jupyter-Notebook of de Python-script-bestand, de volgende Python pakketten zijn vereist. Als u de service Azure Machine Learning-Notebook gebruikt, zijn deze pakketten vooraf geïnstalleerd.

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-the-data-from-blob"></a>In de gegevens uit blob lezen
* Verbindingsreeks

        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
* Lees in als tekst

        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds to read in "+BLOBNAME) % (t2 - t1))

  ![17](./media/data-lake-walkthrough/17-python_readin_csv.PNG)
* De namen van kolommen toevoegen en afzonderlijke kolommen

        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
* Sommige kolommen wijzigen naar een numerieke waarde

        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a>Machine learning-modellen bouwen
Hier kunt u een binair classificeringsmodel om te voorspellen of een reis is punt of niet maken. In de Jupyter-Notebook vindt u twee modellen: multiklassen classificatie en regressiemodellen.

* U moet eerst dummy variabelen maken die kunnen worden gebruikt in scikit-meer modellen

        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')
* Gegevens een kader voor het model maken

        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])

        X = data.iloc[:,1:]
        Y = data.tipped
* Trainings- en testdoeleinden 60-40 splitsen

        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)
* Logistieke regressie in de trainingset

        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)

       ![c1](./media/data-lake-walkthrough/c1-py-logit-coefficient.PNG)
* Score testen gegevensset

        Y_test_pred = logit_fit.predict(X_test)
* Metrische Evaluatiegegevens berekenen

        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train

        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred)
        print fpr_test, tpr_test, thresholds_test

        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)

        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)

       ![c2](./media/data-lake-walkthrough/c2-py-logit-evaluation.PNG)

### <a name="build-web-service-api-and-consume-it-in-python"></a>Service-Web-API maken en deze gebruiken in Python
Wilt u operationeel maken van de machine learning-model nadat deze is gemaakt. De binaire logistieke model wordt gebruikt als een voorbeeld. Zorg ervoor dat de scikit-versie in uw lokale computer is 0.15.1 meer. U hebt geen zorgen over te maken als u Azure Machine Learning studio.

* De referenties van uw werkruimte van Azure Machine Learning studio instellingen vinden. In Azure Machine Learning Studio, klikt u op **instellingen** --> **naam** --> **Autorisatietokens**.

    ![c3](./media/data-lake-walkthrough/c3-workspace-id.PNG)

        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

* Webservice maken

        @services.publish(workspaceid, auth_token)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)
* Web Servicereferenties ophalen

        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key

        print url
        print api_key

        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass
* Webservice-API aanroepen. U moet wacht 5-10 seconden na de vorige stap.

        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)

       ![c4](./media/data-lake-walkthrough/c4-call-API.PNG)

## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a>Optie 2: Maken en implementeren van modellen rechtstreeks in Azure Machine Learning
Azure Machine Learning Studio gegevens rechtstreeks vanuit Azure Data Lake Store kan lezen en vervolgens worden gebruikt om te maken en implementeren van modellen. Deze methode maakt gebruik van een Hive-tabel die naar de Azure Data Lake Store verwijst. Dit is vereist dat een afzonderlijke Azure HDInsight-cluster worden ingericht, waarop de Hive-tabel is gemaakt. De volgende secties ziet hoe u dit doet.

### <a name="create-an-hdinsight-linux-cluster"></a>Een HDInsight-Cluster voor Linux maken
Maken van een HDInsight-Cluster (Linux) uit de [Azure-portal](https://portal.azure.com). Zie voor meer informatie, de **een HDInsight-cluster maken met toegang tot Azure Data Lake Store** in sectie [een HDInsight-cluster maken met Data Lake Store met behulp van Azure portal](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).

 ![18](./media/data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a>Hive-tabel maken in HDInsight
Nu u Hive-tabellen moet worden gebruikt in Azure Machine Learning Studio in het HDInsight-cluster met behulp van de gegevens die zijn opgeslagen in Azure Data Lake Store in de vorige stap. Ga naar het HDInsight-cluster hebt gemaakt. Klik op **instellingen** --> **eigenschappen** --> **Cluster AAD-identiteit** --> **ADLS toegang**, Zorg ervoor dat uw Azure Data Lake Store-account wordt toegevoegd in de lijst met lezen, schrijven en uitvoeren van rechten.

 ![19](./media/data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)

Klik vervolgens op **Dashboard** naast de **instellingen** knop en een venster wordt weergegeven. Klik op **Hive-weergave** in de rechterbovenhoek van de pagina en u ziet de **Query-Editor**.

 ![20](./media/data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)

Plak de volgende Hive-scripts om een tabel te maken. De locatie van de gegevensbron zich in Azure Data Lake Store-verwijzing op deze manier: **adl://data_lake_store_name.azuredatalakestore.net:443/mapnaam/bestandsnaam**.

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


Wanneer de query is voltooid, ziet u de resultaten als volgt:

 ![22](./media/data-lake-walkthrough/22-Hive-Query-results.PNG)

### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a>Modellen in Azure Machine Learning Studio bouwen en implementeren
U bent nu klaar om te bouwen en implementeren van een model dat al dan niet een tip wordt betaald met Azure Machine Learning voorspelt. De toepassing stratificatie voorbeeldgegevens is gereed om te worden gebruikt in deze binaire classificatie (tip of niet) probleem. De voorspellende modellen met multiklassen classificatie (tip_class) en regressie (tip_amount) kunnen ook worden gebouwd en geïmplementeerd met Azure Machine Learning Studio, maar hier hoe u voor het afhandelen van de aanvraag met behulp van de binaire classificeringsmodel wordt alleen weergegeven.

1. Ophalen van de gegevens in Azure Machine Learning studio via de **importgegevens** -module, beschikbaar in de **gegevensinvoer en uitvoer** sectie. Zie voor meer informatie de [module gegevens importeren](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) -verwijzingspagina.
2. Selecteer **Hive-Query** als de **gegevensbron** in de **eigenschappen** deelvenster.
3. Plak het volgende Hive-script in de **Hive-databasequery** editor

        select * from nyc_stratified_sample;
4. Voer de URI van de HDInsight-cluster (dit kan worden gevonden in Azure portal), de Hadoop-referenties, de locatie van de uitvoergegevens en Azure storage-accountnaam naam/sleutelcontainer.

   ![23](./media/data-lake-walkthrough/23-reader-module-v3.PNG)

Een voorbeeld van een binaire classificatie-experiment lezen gegevens in Hive-tabel worden weergegeven in de volgende afbeelding:

 ![24](./media/data-lake-walkthrough/24-AML-exp.PNG)

Nadat het experiment is gemaakt, klikt u op **webservice ingesteld** --> **voorspellende webservice**

 ![25](./media/data-lake-walkthrough/25-AML-exp-deploy.PNG)

Voer de automatisch gemaakte scoren experiment, wanneer deze is voltooid, klikt u op **webservice implementeren**

 ![26](./media/data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

Het dashboard van de webservice wordt kort weergegeven:

 ![27](./media/data-lake-walkthrough/27-AML-web-api.PNG)

## <a name="summary"></a>Samenvatting
U hebt een data science-omgeving voor het bouwen van schaalbare end-to-end-oplossingen in Azure Data Lake gemaakt door te voeren in dit scenario. Deze omgeving is gebruikt voor het analyseren van grote openbare gegevensset, deze te zetten door de canonieke stappen van de Data Science Process van gegevens ophalen via het trainen van het model en vervolgens naar de implementatie van het model als een webservice. U-SQL is gebruikt om te verwerken, verkennen en de voorbeeldgegevens. Python en Hive zijn gebruikt met Azure Machine Learning Studio bouwen en implementeren van voorspellende modellen.

## <a name="whats-next"></a>Volgende stappen
Het leertraject voor de [Team Data Science Process (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) vindt u koppelingen naar onderwerpen met een beschrijving van elke stap in het proces voor geavanceerde analyses. Er zijn een aantal scenario's die worden gespecificeerd op de [Team Data Science Process walkthroughs](walkthroughs.md) pagina die gebruikmaken van het gebruik van resources en services in verschillende predictive analytics-scenario's:

* [Het Team Data Science Process in actie: met behulp van SQL Data Warehouse](sqldw-walkthrough.md)
* [Het Team Data Science Process in actie: met behulp van HDInsight Hadoop-clusters](hive-walkthrough.md)
* [Het Team Data Science Process: SQL Server met behulp](sql-walkthrough.md)
* [Overzicht van de Data Science Process met behulp van Spark op Azure HDInsight](spark-overview.md)
