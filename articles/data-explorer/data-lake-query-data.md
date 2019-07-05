---
title: Query uitvoeren op gegevens in Azure Data Lake met Azure Data Explorer
description: Informatie over het opvragen van gegevens in Azure Data Lake met Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: d6a58d144482e17f7e4b615134115d1da46af6f0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67453175"
---
# <a name="query-data-in-azure-data-lake-using-azure-data-explorer-preview"></a>Query uitvoeren op gegevens in Azure Data Lake met Azure Data Explorer (Preview)

Azure Data Lake Storage is een zeer schaalbare en rendabele data lake-oplossing voor big data-analyses. Het combineert de kracht van een prestatiegericht bestandssysteem met enorme schaal en rendabiliteit om sneller tot inzichten te komen. Data Lake Storage Gen2 breidt de mogelijkheden van Azure Blob Storage uit en is geoptimaliseerd voor analyseworkloads.
 
Azure Data Explorer kan worden geïntegreerd met Azure Blob Storage en Azure Data Lake Storage Gen2, biedt snelle, in de cache opgeslagen, en toegang tot gegevens in de lake geïndexeerd. U kunt analyseren en gegevens in de lake zonder voorafgaande opname in Azure Data Explorer op te vragen. U kunt ook query's uitvoeren voor gegevens opgenomen en uningested systeemeigen lake tegelijkertijd.  

> [!TIP]
> De beste prestaties van query's vereist opnemen van gegevens in Azure Data Explorer. De mogelijkheid om gegevens te doorzoeken in Azure Data Lake Storage Gen2 zonder voorafgaande opname moet alleen worden gebruikt voor historische gegevens of gegevens die zelden worden in de query wordt uitgevoerd.
 
## <a name="optimize-query-performance-in-the-lake"></a>Prestaties van query's in de lake optimaliseren 

* Partitioneren van gegevens voor betere prestaties en geoptimaliseerde uitvoeren van query's.
* Comprimeren van gegevens voor betere prestaties (gzip voor de beste compressie, lz4 voor de beste prestaties).
* Gebruik Azure Blob Storage of Azure Data Lake Storage Gen2 met dezelfde regio als uw Azure Data Explorer-cluster. 

## <a name="create-an-external-table"></a>Een externe tabel maken

1. Gebruik de `.create external table` opdracht voor het maken van een externe tabel in Azure Data Explorer. Externe tabel aanvullende opdrachten, zoals `.show`, `.drop`, en `.alter` zijn gedocumenteerd in [externe tabel opdrachten](/azure/kusto/management/externaltables).

    ```Kusto
    .create external table ArchivedProducts(
    Timestamp:datetime,
    ProductId:long, ProductDescription:string) 
    kind=blob
    partition by bin(Timestamp, 1d) 
    dataformat=csv (h@'http://storageaccount.blob.core.windows.net/container1;secretKey') 
    with (compressed = true)  
    ```

    Deze query maakt u dagelijkse partities *container1/yyyy/MM/dd/all_exported_blobs.csv*. Verbeterde prestaties wordt met gedetailleerdere partitioneren verwacht. Query's over externe tabellen met dagelijkse partities, zoals hierboven, wordt bijvoorbeeld, betere prestaties dan deze query's met maandelijkse gepartitioneerde tabellen hebben.

    > [!NOTE]
    > Momenteel ondersteunde opslagaccounts zijn Azure Blob Storage of Azure Data Lake Storage Gen2. Van ondersteunde gegevensindelingen zijn csv, tsv en txt.

1. De externe tabel wordt weergegeven in het linkerdeelvenster van de Web-UI

    ![externe tabel in de web-UI](media/data-lake-query-data/external-tables-web-ui.png)
 
### <a name="external-table-permissions"></a>Machtigingen voor externe tabel
 
* De databasegebruiker kunt maken van een externe tabel. De maker van de tabel wordt automatisch de beheerder van de tabel.
* Het cluster, database of tabel-beheerder kan een bestaande tabel bewerken.
* Een databasegebruiker of lezer kan query uitvoeren op een externe tabel.
 
## <a name="query-an-external-table"></a>Query uitvoeren op een externe tabel
 
Om te vragen een externe tabel, gebruikt u de `external_table()` functioneren en geef de naam van de tabel als de functieargument. De rest van de query is standaard Kusto-querytaal.

```Kusto
external_table("ArchivedProducts") | take 100
```

> [!TIP]
> IntelliSense wordt momenteel niet ondersteund op externe tabel query's.

## <a name="query-external-and-ingested-data-together"></a>Externe en opgenomen gegevens bij elkaar op te vragen

U kunt een query uitvoeren voor zowel externe tabellen en gegevenstabellen opgenomen binnen dezelfde query. U [ `join` ](/azure/kusto/query/joinoperator) of [ `union` ](/azure/kusto/query/unionoperator) de externe tabel met aanvullende gegevens van Azure Data Explorer, SQL-servers of andere bronnen. Gebruik een [ `let( ) statement` ](/azure/kusto/query/letstatement) een verkorte versie van-naam toewijzen aan een externe tabel-verwijzing.

In het onderstaande voorbeeld *producten* is een gegevenstabel opgenomen en *ArchivedProducts* is een externe tabel met gegevens in de Azure Data Lake Storage Gen2:

```kusto
let T1 = external_table("ArchivedProducts") |  where TimeStamp > ago(100d);
let T = Products; //T is an internal table
T1 | join T on ProductId | take 10
```

## <a name="query-taxirides-external-table-in-the-help-cluster"></a>Query *TaxiRides* externe tabel in het helpcluster

De *TaxiRides* verzameling voorbeeldgegevens bevat gegevens van de New York City over taxi's van [NYC Taxi en Limousine Commissie](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

### <a name="create-external-table-taxirides"></a>Externe tabel maken *TaxiRides* 

> [!NOTE]
> In deze sectie ziet u de query gebruikt voor het maken van de *TaxiRides* externe tabel in de *help* cluster. Omdat deze tabel is al gemaakt kunt u deze sectie overslaan en voeren [query *TaxiRides* Externe tabelgegevens](#query-taxirides-external-table-data). 

1. De volgende query is gebruikt om u te maken van de externe tabel *TaxiRides* in het helpcluster. 

    ```kusto
    .create external table TaxiRides
    (
    trip_id: long,
    vendor_id: string, 
    pickup_datetime: datetime,
    dropoff_datetime: datetime,
    store_and_fwd_flag: string,
    rate_code_id: int,
    pickup_longitude: real,
    pickup_latitude: real,
    dropoff_longitude: real,
    dropoff_latitude: real,
    passenger_count: int,
    trip_distance: real,
    fare_amount: real,
    extra: real,
    mta_tax: real,
    tip_amount: real,
    tolls_amount: real,
    ehail_fee: real,
    improvement_surcharge: real,
    total_amount: real,
    payment_type: string,
    trip_type: int,
    pickup: string,
    dropoff: string,
    cab_type: string,
    precipitation: int,
    snow_depth: int,
    snowfall: int,
    max_temperature: int,
    min_temperature: int,
    average_wind_speed: int,
    pickup_nyct2010_gid: int,
    pickup_ctlabel: string,
    pickup_borocode: int,
    pickup_boroname: string,
    pickup_ct2010: string,
    pickup_boroct2010: string,
    pickup_cdeligibil: string,
    pickup_ntacode: string,
    pickup_ntaname: string,
    pickup_puma: string,
    dropoff_nyct2010_gid: int,
    dropoff_ctlabel: string,
    dropoff_borocode: int,
    dropoff_boroname: string,
    dropoff_ct2010: string,
    dropoff_boroct2010: string,
    dropoff_cdeligibil: string,
    dropoff_ntacode: string,
    dropoff_ntaname: string,
    dropoff_puma: string
    )
    kind=blob 
    partition by bin(pickup_datetime, 1d)
    dataformat=csv
    ( 
    h@'https://externalkustosamples.blob.core.windows.net/taxiridesbyday?st=2019-06-18T14%3A59%3A00Z&se=2029-06-19T14%3A59%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=yEaO%2BrzFHzAq7lvd4d9PeQ%2BTi3AWnho8Rn8hGU0X30M%3D'
    )
    ```
1. De resulterende tabel is gemaakt in de *help* cluster:

    ![Externe tabel TaxiRides](media/data-lake-query-data/taxirides-external-table.png) 

### <a name="query-taxirides-external-table-data"></a>Query *TaxiRides* gegevens in een externe tabel 

Aanmelden bij [ https://dataexplorer.azure.com/clusters/help/databases/Samples ](https://dataexplorer.azure.com/clusters/help/databases/Samples) aan query de *TaxiRides* externe tabel. 

#### <a name="query-taxirides-external-table-without-partitioning"></a>Query *TaxiRides* externe tabel zonder partitioneren

[Voer deze query uit](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAx3LSwqAMAwFwL3gHYKreh1xL7F9YrCtElP84OEV9zM4DZo5DsZjhGt6PqWTgL1p6+qhvaTEKjeI/FqyuZbGiwJf63QAi9vEL2UbAhtMEv6jyAH6+VhS9jOr1dULfUgAm2cAAAA=) op de externe tabel *TaxiRides* naar ritjes voor elke dag van de week weer over de hele gegevensset. 

```kusto
external_table("TaxiRides")
| summarize count() by dayofweek(pickup_datetime)
| render columnchart
```

Deze query geeft de drukste dag van de week. Omdat de gegevens niet is gepartitioneerd, kan deze query een lang om te retourneren van resultaten (maximaal enkele minuten) duren.

![niet-gepartitioneerde query weergeven](media/data-lake-query-data/taxirides-no-partition.png)

#### <a name="query-taxirides-external-table-with-partitioning"></a>Query uitvoeren op externe tabel met partities TaxiRides 

[Voer deze query uit](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA13NQQqDMBQE0L3gHT6ukkVF3fQepXv5SQYMNWmIP6ilh68WuinM6jHMYBPkyPMobGao5s6bv3mHpdF19aZ1QgYlbx8ljY4F4gPIQFYgkvqJGrr+eun6I5ralv58OP27t5QQOPsXiOyzRFGazE6WzSh7wtnIiA75uISdOEtdfQDLWmP+ogAAAA==) op de externe tabel *TaxiRides* taxi CAB-bestand typen (geel of groen) weergeven in januari 2017 gebruikt. 

```kusto
external_table("TaxiRides")
| where pickup_datetime between (datetime(2017-01-01) .. datetime(2017-02-01))
| summarize count() by cab_type
| render piechart
```

Deze query maakt gebruik van partitionering, die optimale uitvoeren van query's en prestaties. De query filteren op een gepartitioneerde kolom (pickup_datetime) en retourneert resultaten in een paar seconden.

![gepartitioneerde query weergeven](media/data-lake-query-data/taxirides-with-partition.png)
  
U kunt extra query's uit te voeren op de externe tabel schrijven *TaxiRides* en meer informatie over de gegevens. 

## <a name="next-steps"></a>Volgende stappen

Uw gegevens in de Azure Data Lake met Azure Data Explorer op te vragen. Meer informatie over het [query's schrijven](write-queries.md) en extra inzichten zijn afgeleid van uw gegevens.