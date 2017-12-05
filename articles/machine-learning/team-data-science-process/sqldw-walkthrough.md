---
title: 'De procedure van wetenschappelijke gegevens Team in actie: met behulp van SQL Data Warehouse | Microsoft Docs'
description: Geavanceerde analyses proces en de technologie in actie
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 88ba8e28-0bd7-49fe-8320-5dfa83b65724
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2017
ms.author: bradsev;weig
ms.openlocfilehash: 9c858427b01f7b94aae87136a46e1d9ae5e09a1c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="the-team-data-science-process-in-action-using-sql-data-warehouse"></a>De procedure van wetenschappelijke gegevens Team in actie: met behulp van SQL Data Warehouse
In deze zelfstudie wordt beschreven hoe u maken en implementeren van een machine learning-model met behulp van SQL Data Warehouse (SQL DW) voor een openbaar dataset--de [NYC Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gegevensset. Het binaire classificatie model samengesteld voorspelt al dan niet een tip voor een zakenreis is betaald, en modellen voor multiklassen classificatie en regressie worden ook besproken die het distributiepunt voor de tip bedragen betaald voorspellen.

De procedure volgt de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) werkstroom. Laten we zien hoe een wetenschappelijke gegevensomgeving kunt instellen hoe de gegevens in SQL DW laden en hoe SQL DW of een laptop IPython gebruiken om te verkennen van de gegevens en de engineer functies model. Vervolgens laten we zien hoe bouwen en implementeren van een model met Azure Machine Learning.

## <a name="dataset"></a>De gegevensset NYC Taxi reizen
De gegevens van de NYC Taxi reis bestaat uit ongeveer 20GB gecomprimeerde CSV-bestanden (~ 48GB ongecomprimeerde), vastleggen van meer dan 173 miljoen afzonderlijke reizen en de tarieven voor elke reis betaald. Elke record reis omvat de locaties ophalen en inleverbibliotheek en tijden, geanonimiseerde hack (van stuurprogramma) licentienummer en het aantal straten (taxi van unieke id). De gegevens bevat informatie over alle reizen in het jaar 2013 en is beschikbaar in de volgende twee gegevenssets voor elke maand:

1. De **trip_data.csv** bestand reis details, zoals het aantal passagiers, ophalen en dropoff punten reis duur en duur van reis bevat. Hier volgen enkele voorbeeldrecords:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. De **trip_fare.csv** bestand bevat de details van het tarief dat betaald voor elke reis, zoals betalingstype, tarief bedrag, extra kosten en belastingen, tips en tolgelden, en de totale hoeveelheid betaald. Hier volgen enkele voorbeeldrecords:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De **unieke sleutel** gebruikt om lid reis\_gegevens en reis\_tarief dat bestaat uit de volgende drie velden:

* straten,
* inbreken\_licentie en
* ophalen\_datetime.

## <a name="mltasks"></a>Adres van de drie typen taken voorspelling
We formuleren drie voorspelling problemen op basis van de *tip\_bedrag* ter illustratie van de drie soorten taken modelleren:

1. **Binaire classificatie**: te voorspellen of een tip betaald is voor een reis, dat wil zeggen een *tip\_bedrag* die groter is dan $0 een voorbeeld van een positieve is, terwijl een *tip\_bedrag* van $0 is een voorbeeld van een negatief.
2. **Multiklassen classificatie**: om te voorspellen van het bereik van een tip voor de reis betaald. We delen de *tip\_bedrag* in vijf opslaglocaties of klassen:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. **Regressie taak**: om te voorspellen van de hoeveelheid tip voor een reis betaald.  

## <a name="setup"></a>Instellen van de Azure data wetenschappelijke omgeving voor geavanceerde analyses
Voer de volgende stappen uit voordat u uw omgeving voor Gegevenswetenschap Azure kunt instellen.

**Uw eigen Azure blob storage-account maken**

* Wanneer u uw eigen Azure-blobopslag inricht, kiest u een geografische locatie voor uw Azure blob storage in of zo dicht mogelijk bij **Zuid-centraal VS**, dit is waar de gegevens van de NYC Taxi worden opgeslagen. De gegevens worden gekopieerd met behulp van AzCopy van de openbare blob storage-container naar een container in uw eigen opslagaccount. Hoe dichter de Azure blob-opslag Zuid-centraal VS, hoe sneller de (stap 4) van deze taak wordt voltooid.
* Voor het maken van uw eigen Azure storage-account, volg de stappen op [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md). Zorg ervoor dat notities over de waarden voor de volgende opslagaccountreferenties maken als ze later in dit scenario worden vereist.
  
  * **Naam van het opslagaccount**
  * **De sleutel van opslagaccount**
  * **Containernaam** (die u wilt dat de gegevens worden opgeslagen in Azure blob storage)

**Inrichten van uw Azure SQL DW-exemplaar.**
Volg de documentatie op [maken van een SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) voor het inrichten van een exemplaar van SQL Data Warehouse. Zorg ervoor dat u notaties op de volgende SQL Data Warehouse-referenties die worden gebruikt in latere stappen.

* **Servernaam**: <server Name>. database.windows.net
* **De naam van de SQLDW (Database)**
* **Gebruikersnaam**
* **Wachtwoord**

**Installeer Visual Studio en SQL Server Data Tools.** Zie voor instructies [Visual Studio 2015 installeren en/of SSDT (SQL Server Data Tools) voor SQL Data Warehouse](../../sql-data-warehouse/sql-data-warehouse-install-visual-studio.md).

**Verbinding maken met uw Azure SQL DW met Visual Studio.** Voor instructies raadpleegt u de stappen 1 en 2 in [verbinding maken met Azure SQL Data Warehouse met Visual Studio](../../sql-data-warehouse/sql-data-warehouse-connect-overview.md).

> [!NOTE]
> De volgende SQL-query worden uitgevoerd op de database die u hebt gemaakt in uw SQL Data Warehouse (in plaats van de query is opgegeven in stap 3 van het onderwerp connect) tot **een hoofdsleutel**.
> 
> 

    BEGIN TRY
           --Try to create the master key
        CREATE MASTER KEY
    END TRY
    BEGIN CATCH
           --If the master key exists, do nothing
    END CATCH;

**Een Azure Machine Learning-werkruimte onder uw Azure-abonnement maken.** Zie voor instructies [maken van een Azure Machine Learning-werkruimte](../studio/create-workspace.md).

## <a name="getdata"></a>De gegevens in SQL Data Warehouse laden
Open een Windows PowerShell-opdracht-console. Voer de volgende PowerShell-opdrachten voor het downloaden van het voorbeeld SQL scriptbestanden die we delen met u op GitHub naar een lokale map die u met de parameter opgeeft *- DestDir*. U kunt de waarde van parameter wijzigen *- DestDir* naar een lokale map. Als *- DestDir* niet bestaat, wordt deze gemaakt door het PowerShell-script.

> [!NOTE]
> Mogelijk moet u **als Administrator uitvoeren** bij het uitvoeren van de volgende PowerShell-script als uw *DestDir* directory moet Administrator-bevoegdheden voor het maken of om ernaar te schrijven.
> 
> 

    $source = "https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/Download_Scripts_SQLDW_Walkthrough.ps1"
    $ps1_dest = "$pwd\Download_Scripts_SQLDW_Walkthrough.ps1"
    $wc = New-Object System.Net.WebClient
    $wc.DownloadFile($source, $ps1_dest)
    .\Download_Scripts_SQLDW_Walkthrough.ps1 –DestDir 'C:\tempSQLDW'

Na voltooiing van uitvoering, de huidige werkmap wijzigingen in *- DestDir*. U moet kunnen scherm wordt weergegeven zoals hieronder:

![][19]

In uw *- DestDir*, het volgende PowerShell-script uitvoeren in de beheerdersmodus:

    ./SQLDW_Data_Import.ps1

Wanneer het PowerShell-script wordt uitgevoerd voor de eerste keer, wordt u gevraagd voor het invoeren van de gegevens van uw Azure SQL DW en uw Azure blob storage-account. Wanneer dit PowerShell-script is voltooid wordt voor het eerst uitvoert, de referenties u invoer zijn geschreven in een configuratiebestand SQLDW.conf in de huidige werkmap. De toekomstige uitvoeren van dit bestand PowerShell-script heeft de optie voor het lezen van dat alle parameters van dit configuratiebestand nodig. Als u enkele parameters veranderen moet, kunt u kiezen voor het invoeren van de parameters op het scherm na de prompt dit configuratiebestand verwijderd en de parameterwaarden invoeren als u wordt gevraagd of de parameterwaarden met het bewerken van het bestand SQLDW.conf in uw wijzigen*- DestDir* directory.

> [!NOTE]
> Om te voorkomen dat schema naam een conflict veroorzaakt met degenen die al aanwezig zijn in uw Azure SQL DW bij het lezen van parameters rechtstreeks vanuit het bestand SQLDW.conf, wordt een willekeurig getal 3-cijferige toegevoegd aan de schemanaam uit het bestand SQLDW.conf als de naam van het standaard schema voor elke uitvoering. Het PowerShell-script wordt u mogelijk gevraagd voor de naam van een schema: de naam van de gebruiker goeddunken worden opgegeven.
> 
> 

Dit **PowerShell-script** bestand is voltooid de volgende taken:

* **Downloadt en installeert AzCopy**als AzCopy nog niet is geïnstalleerd
  
        $AzCopy_path = SearchAzCopy
        if ($AzCopy_path -eq $null){
               Write-Host "AzCopy.exe is not found in C:\Program Files*. Now, start installing AzCopy..." -ForegroundColor "Yellow"
            InstallAzCopy
            $AzCopy_path = SearchAzCopy
        }
            $env_path = $env:Path
            for ($i=0; $i -lt $AzCopy_path.count; $i++){
                if ($AzCopy_path.count -eq 1){
                    $AzCopy_path_i = $AzCopy_path
                } else {
                    $AzCopy_path_i = $AzCopy_path[$i]
                }
                if ($env_path -notlike '*' +$AzCopy_path_i+'*'){
                    Write-Host $AzCopy_path_i 'not in system path, add it...'
                    [Environment]::SetEnvironmentVariable("Path", "$AzCopy_path_i;$env_path", "Machine")
                    $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine")
                    $env_path = $env:Path
                }
* **Gegevens worden gekopieerd naar uw persoonlijke blob storage-account** van de openbare blob met AzCopy
  
        Write-Host "AzCopy is copying data from public blob to yo storage account. It may take a while..." -ForegroundColor "Yellow"
        $start_time = Get-Date
        AzCopy.exe /Source:$Source /Dest:$DestURL /DestKey:$StorageAccountKey /S
        $end_time = Get-Date
        $time_span = $end_time - $start_time
        $total_seconds = [math]::Round($time_span.TotalSeconds,2)
        Write-Host "AzCopy finished copying data. Please check your storage account to verify." -ForegroundColor "Yellow"
        Write-Host "This step (copying data from public blob to your storage account) takes $total_seconds seconds." -ForegroundColor "Green"
* **Laden van gegevens met Polybase (door het uitvoeren van LoadDataToSQLDW.sql) naar uw Azure SQL DW** van uw persoonlijke blob storage-account met de volgende opdrachten.
  
  * Een schema maken
    
          EXEC (''CREATE SCHEMA {schemaname};'');
  * Een-scoped databasereferentie maken
    
          CREATE DATABASE SCOPED CREDENTIAL {KeyAlias}
          WITH IDENTITY = ''asbkey'' ,
          Secret = ''{StorageAccountKey}''
  * Maken van een externe gegevensbron voor een Azure storage-blob
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_trip_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
    
          CREATE EXTERNAL DATA SOURCE {nyctaxi_fare_storage}
          WITH
          (
              TYPE = HADOOP,
              LOCATION =''wasbs://{ContainerName}@{StorageAccountName}.blob.core.windows.net'',
              CREDENTIAL = {KeyAlias}
          )
          ;
  * Maak een externe bestandsindeling voor een csv-bestand. Gegevens zijn niet-gecomprimeerde en velden worden gescheiden met het sluisteken.
    
          CREATE EXTERNAL FILE FORMAT {csv_file_format}
          WITH
          (   
              FORMAT_TYPE = DELIMITEDTEXT,
              FORMAT_OPTIONS  
              (
                  FIELD_TERMINATOR ='','',
                  USE_TYPE_DEFAULT = TRUE
              )
          )
          ;
  * Externe tarief en tabellen voor NYC taxi gegevensset reis maken in Azure blob-opslag.
    
          CREATE EXTERNAL TABLE {external_nyctaxi_fare}
          (
              medallion varchar(50) not null,
              hack_license varchar(50) not null,
              vendor_id char(3),
              pickup_datetime datetime not null,
              payment_type char(3),
              fare_amount float,
              surcharge float,
              mta_tax float,
              tip_amount float,
              tolls_amount float,
              total_amount float
          )
          with (
              LOCATION    = ''/nyctaxifare/'',
              DATA_SOURCE = {nyctaxi_fare_storage},
              FILE_FORMAT = {csv_file_format},
              REJECT_TYPE = VALUE,
              REJECT_VALUE = 12     
          )  

            CREATE EXTERNAL TABLE {external_nyctaxi_trip}
            (
                   medallion varchar(50) not null,
                   hack_license varchar(50)  not null,
                   vendor_id char(3),
                   rate_code char(3),
                   store_and_fwd_flag char(3),
                   pickup_datetime datetime  not null,
                   dropoff_datetime datetime,
                   passenger_count int,
                   trip_time_in_secs bigint,
                   trip_distance float,
                   pickup_longitude varchar(30),
                   pickup_latitude varchar(30),
                   dropoff_longitude varchar(30),
                   dropoff_latitude varchar(30)
            )
            with (
                LOCATION    = ''/nyctaxitrip/'',
                DATA_SOURCE = {nyctaxi_trip_storage},
                FILE_FORMAT = {csv_file_format},
                REJECT_TYPE = VALUE,
                REJECT_VALUE = 12         
            )

    - Gegevens uit externe tabellen in Azure blob-opslag laden in SQL Data Warehouse

            CREATE TABLE {schemaname}.{nyctaxi_fare}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_fare}
            ;

            CREATE TABLE {schemaname}.{nyctaxi_trip}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            SELECT *
            FROM   {external_nyctaxi_trip}
            ;

    - Een tabel met voorbeeld (NYCTaxi_Sample) maken en gegevens voor het invoegen van het selecteren van de SQL-query's op de tabellen reis en tarief. (Een aantal stappen van dit scenario moet deze voorbeeldtabel gebruikt.)

            CREATE TABLE {schemaname}.{nyctaxi_sample}
            WITH
            (   
                CLUSTERED COLUMNSTORE INDEX,
                DISTRIBUTION = HASH(medallion)
            )
            AS
            (
                SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount,
                tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
                tip_class = CASE
                        WHEN (tip_amount = 0) THEN 0
                        WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                        WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                        WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                        ELSE 4
                    END
                FROM {schemaname}.{nyctaxi_trip} t, {schemaname}.{nyctaxi_fare} f
                WHERE datepart("mi",t.pickup_datetime) = 1
                AND t.medallion = f.medallion
                AND   t.hack_license = f.hack_license
                AND   t.pickup_datetime = f.pickup_datetime
                AND   pickup_longitude <> ''0''
                AND   dropoff_longitude <> ''0''
            )
            ;

Laadtijden van invloed op de geografische locatie van uw storage-accounts.

> [!NOTE]
> Afhankelijk van de geografische locatie van uw persoonlijke blob storage-account, het kopiëren van gegevens uit een openbare blob naar uw persoonlijke opslagaccount kan duren ongeveer 15 minuten of zelfs nog langer, en het proces van het laden van gegevens uit uw storage-account aan uw Azure SQL DW kan duurt ongeveer 20 minuten of langer.  
> 
> 

U moet beslissen welke komen als er dubbele bron en doel-bestanden.

> [!NOTE]
> Als de CSV-bestanden worden gekopieerd van de openbare blob-opslag naar uw persoonlijke blob storage-account bestaat al in uw persoonlijke blob storage-account, AzCopy u wordt gevraagd of u wilt overschrijven. Als u niet overschrijven wilt, invoer  **n**  wanneer u wordt gevraagd. Als u wilt overschrijven **alle** , invoer **een** wanneer u wordt gevraagd. U kunt ook invoer **y** afzonderlijk .csv-bestanden overschrijven.
> 
> 

![#21 tekenen][21]

U kunt uw eigen gegevens. Als uw gegevens in uw on-premises machine in uw toepassing praktijk is, kunt u AzCopy on-premises gegevens uploaden naar uw persoonlijke Azure-blobopslag. U hoeft alleen te wijzigen de **bron** locatie, `$Source = "http://getgoing.blob.core.windows.net/public/nyctaxidataset"`, in de AzCopy-opdracht van het PowerShell-scriptbestand naar een lokale map met uw gegevens.

> [!TIP]
> Als uw gegevens in uw toepassing praktijk al in uw persoonlijke Azure-blobopslag is, kunt u het overslaan van AzCopy in het PowerShell-script en de gegevens rechtstreeks te uploaden naar Azure SQL DW. Hiervoor moet de aanvullende bewerkingen van het script aan te passen deze naar de indeling van uw gegevens.
> 
> 

Deze Powershell-script ook wordt in de Azure SQL DW-informatie in de gegevensbestanden van de exploratie-voorbeeld SQLDW_Explorations.sql SQLDW_Explorations.ipynb en SQLDW_Explorations_Scripts.py zodat deze drie bestanden gereed om te worden uitgevoerd zijn uit onmiddellijk na het PowerShell-script is voltooid.

Na een geslaagde uitvoering, ziet u scherm, zoals hieronder:

![][20]

## <a name="dbexplore"></a>Gegevensverkenning en functie-engineering in Azure SQL Data Warehouse
In deze sectie we gegevens te verkennen en functie generatie uitvoeren door met het SQL-query's uitvoeren in Azure SQL DW direct met **Visual Studio Data Tools**. Alle SQL-query's gebruikt in deze sectie vindt u in het voorbeeld van een script met de naam *SQLDW_Explorations.sql*. Dit bestand is al gedownload naar uw lokale directory door het PowerShell-script. U kunt ook ophalen via [GitHub](https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/SQLDW/SQLDW_Explorations.sql). Maar het bestand in GitHub beschikt niet over de Azure SQL DW-informatie die is aangesloten.

Verbinding maken met uw Azure SQL DW met Visual Studio met SQL DW-aanmeldingsnaam en wachtwoord en opent u de **SQL Object Explorer** om te bevestigen dat de database en tabellen zijn geïmporteerd. Ophalen van de *SQLDW_Explorations.sql* bestand.

> [!NOTE]
> U kunt een Parallel Data Warehouse (PDW) query-editor te openen met de **nieuwe Query** opdracht terwijl uw PDW is geselecteerd de **SQL Object Explorer**. De standaard SQL-query-editor wordt niet ondersteund door PDW.
> 
> 

Hier vindt u het type gegevens te verkennen en functie generatie uitgevoerd in deze sectie:

* Verken de gegevens distributies van enkele velden in verschillende tijdvensters.
* Onderzoek de kwaliteit van de gegevens van de breedtegraad -velden.
* Genereren van binaire en multiklassen classificatielabels op basis van de **tip\_bedrag**.
* Genereren van functies en reis afstanden compute/vergelijken.
* Voeg de twee tabellen en uitpakken van een steekproef die worden gebruikt voor het bouwen van modellen.

### <a name="data-import-verification"></a>Gegevens importeren verificatie
Deze query's bieden een snelle controle van het aantal rijen en kolommen in de tabellen ingevuld met oudere versies van Polybase parallelle bulksgewijs importeert,

    -- Report number of rows in table <nyctaxi_trip> without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')

    -- Report number of columns in table <nyctaxi_trip>
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = '<nyctaxi_trip>' AND table_schema = '<schemaname>'

**Uitvoer:** krijgt u 173,179,759 rijen en 14 kolommen.

### <a name="exploration-trip-distribution-by-medallion"></a>Exploratie: Reis distributie door straten
Deze voorbeeldquery identificeert de medallions (taxi getallen) die meer dan 100 reizen voltooid binnen een opgegeven periode. De query zou gebruikmaken van de gepartitioneerde tabel omdat deze zijn afhankelijk van het partitieschema van **ophalen\_datetime**. Opvragen van de volledige gegevensset maakt ook gebruik van de gepartitioneerde tabel en/of index van de scan.

    SELECT medallion, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

**Uitvoer:** de query een tabel met rijen geven de 13,369 medallions (taxi's) en het aantal reis voltooid door ze in 2013 moet retourneren. De laatste kolom bevat het aantal reizen voltooid.

### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Exploratie: Reis distributie door straten en hack_license
In dit voorbeeld identificeert de medallions (taxi getallen) en hack_license getallen (stuurprogramma's) die voltooid zijn meer dan 100 reizen binnen een opgegeven periode.

    SELECT medallion, hack_license, COUNT(*)
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

**Uitvoer:** de query moet een tabel geretourneerd met 13,369 rijen geven de 13,369 auto/stuurprogramma-id's die meer voltooid dat 100 in 2013 reizen. De laatste kolom bevat het aantal reizen voltooid.

### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Gegevens beoordeling: Controleer of records met onjuiste lengtegraad en/of breedtegraad
In dit voorbeeld onderzoekt als een van de velden lengtegraad en/of breedtegraad een ongeldige waarde bevatten (radiaal graden moet tussen-90 en 90), of (0, 0) coördinaten.

    SELECT COUNT(*) FROM <schemaname>.<nyctaxi_trip>
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

**Uitvoer:** de query retourneert 837,467 reizen die ongeldige lengtegraad en/of breedtegraad velden hebben.

### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Exploratie: Punt versus niet Gekantelde reizen distributie
Dit voorbeeld wordt het aantal reizen zijn vergeleken met het nummer van de punt wanneer geen punt zijn in een opgegeven periode (of in de volledige gegevensset als die betrekking hebben op het volledige jaar als u deze hier is ingesteld). Deze verdeling weerspiegelt de distributie van de binaire label moet later worden gebruikt voor binaire classificatie modelleren.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM <schemaname>.<nyctaxi_fare>
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

**Uitvoer:** retourneert de query moet de volgende frequenties van tip voor het jaar 2013: 90,447,622 punt en 82,264,709 niet punt.

### <a name="exploration-tip-classrange-distribution"></a>Exploratie: Verdeling Tip klasse/bereik
In dit voorbeeld berekent de verdeling van tip bereiken in een bepaalde periode (of in de volledige gegevensset als die betrekking hebben op het volledige jaar). Dit is de distributie van de label-klassen die later wordt gebruikt voor multiklassen classificatie modelleren.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_fare>
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

**Uitvoer:**

| tip_class | tip_freq |
| --- | --- |
| 1 |82230915 |
| 2 |6198803 |
| 3 |1932223 |
| 0 |82264625 |
| 4 |85765 |

### <a name="exploration-compute-and-compare-trip-distance"></a>Exploratie: Berekenen en reis afstand vergelijken
In dit voorbeeld wordt de lengtegraad ophalen en inleverbibliotheek en breedtegraad naar SQL Geografie verwijst, berekent de reis afstand met behulp van SQL Geografie punten verschil en retourneert een steekproef van de resultaten voor vergelijking. In het voorbeeld de resultaten beperkt tot geldig coördinaten alleen met behulp van de kwaliteit assessment gegevensquery eerder besproken.

    /****** Object:  UserDefinedFunction [dbo].[fnCalculateDistance] ******/
    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function to calculate the direct distance  in mile between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

### <a name="feature-engineering-using-sql-functions"></a>Functie-engineering met behulp van SQL-functies
SQL-functies kunnen soms een efficiënte optie voor functie-engineering zijn. In dit overzicht hebben we een SQL-functie voor het berekenen van de directe afstand tussen de locaties ophalen en dropoff gedefinieerd. U kunt de volgende SQL-scripts uitvoeren in **Visual Studio Data Tools**.

Hier volgt het SQL-script waarmee wordt gedefinieerd met de functie distance.

    SET ANSI_NULLS ON
    GO

    SET QUOTED_IDENTIFIER ON
    GO

    IF EXISTS (SELECT * FROM sys.objects WHERE type IN ('FN', 'IF') AND name = 'fnCalculateDistance')
      DROP FUNCTION fnCalculateDistance
    GO

    -- User-defined function calculate the direct distance between two geographical coordinates.
    CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)

    RETURNS float
    AS
    BEGIN
          DECLARE @distance decimal(28, 10)
          -- Convert to radians
          SET @Lat1 = @Lat1 / 57.2958
          SET @Long1 = @Long1 / 57.2958
          SET @Lat2 = @Lat2 / 57.2958
          SET @Long2 = @Long2 / 57.2958
          -- Calculate distance
          SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
          --Convert to miles
          IF @distance <> 0
          BEGIN
            SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
          END
          RETURN @distance
    END
    GO

Hier volgt een voorbeeld van de functie voor het genereren van functies in uw SQL-query:

    -- Sample query to call the function to create features
    SELECT pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude,
    dbo.fnCalculateDistance(pickup_latitude, pickup_longitude, dropoff_latitude, dropoff_longitude) AS DirectDistance
    FROM <schemaname>.<nyctaxi_trip>
    WHERE datepart("mi",pickup_datetime)=1
    AND CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND pickup_longitude != '0' AND dropoff_longitude != '0'

**Uitvoer:** deze query wordt een tabel (met 2,803,538 rijen) gegenereerd met ophalen en dropoff Latitude en lengten en de bijbehorende directe afstanden in mijl. Hier volgen de resultaten voor de eerste 3 rijen:

|  | pickup_latitude | pickup_longitude | dropoff_latitude | dropoff_longitude | DirectDistance |
| --- | --- | --- | --- | --- | --- |
| 1 |40.731804 |-74.001083 |40.736622 |-73.988953 |.7169601222 |
| 2 |40.715794 |-74,010635 |40.725338 |-74.00399 |.7448343721 |
| 3 |40.761456 |-73.999886 |40.766544 |-73.988228 |0.7037227967 |

### <a name="prepare-data-for-model-building"></a>Gegevens voorbereiden voor het model bouwen
De volgende query joins de **nyctaxi\_reis** en **nyctaxi\_tarief** tabellen, genereert een label binaire classificatie **punt**, een meerdere klasse classificatie label **tip\_klasse**, en een voorbeeld van een geëxtraheerd uit de volledige gegevensset lid. De steekproeven wordt uitgevoerd door bij het ophalen van een subset van de reizen op basis van tijd ophalen.  Deze query kan worden gekopieerd en geplakt rechtstreeks in de [Azure Machine Learning Studio](https://studio.azureml.net) [importgegevens] [ import-data] -module voor directe gegevensopname van het SQL-database-exemplaar in Azure. De query worden records met onjuiste uitgesloten (0, 0) coördinaten.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
    WHERE datepart("mi",t.pickup_datetime) = 1
    AND   t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

Wanneer u klaar om door te gaan naar Azure Machine Learning bent, mag u ofwel:  

1. Opslaan van de laatste SQL-query voor het uitpakken en de voorbeeldgegevens en kopiëren en plakken van de query rechtstreeks in een [importgegevens] [ import-data] -module in Azure Machine Learning, of
2. Behouden van de steekproef en engineering-gegevens die u wilt gebruiken voor model bouwen in een nieuwe SQL DW-tabel en het gebruik van de nieuwe tabel in de [importgegevens] [ import-data] -module in Azure Machine Learning. Het PowerShell-script in een eerdere stap heeft dit voor u gedaan. U kunt lezen rechtstreeks uit deze tabel in de module gegevens importeren.

## <a name="ipnb"></a>Gegevensverkenning en functie-engineering in IPython notebook
In deze sectie wordt uitgevoerd, er gegevensverkenning en functie genereren met behulp van beide Python en SQL-query's op de SQL DW eerder hebt gemaakt. Een voorbeeld IPython notebook met de naam **SQLDW_Explorations.ipynb** en een scriptbestand Python **SQLDW_Explorations_Scripts.py** naar uw lokale directory zijn gedownload. Ze zijn ook beschikbaar is op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/SQLDW). Deze twee bestanden zijn identiek in Python-scripts. Het Python-scriptbestand wordt geleverd aan u, als er geen een IPython laptop-server. Deze twee bestanden zijn ontworpen onder Python steekproef **Python 2.7**.

De benodigde gegevens aan Azure SQL DW in de steekproef IPython Notebook en het scriptbestand Python gedownload naar uw lokale computer is aangesloten door het PowerShell-script eerder. Ze zijn uitvoerbare zonder dat aanpassingen.

Als u al een AzureML-werkruimte hebt ingesteld, kunt u rechtstreeks de steekproef IPython Notebook uploaden naar de AzureML IPython laptop-service en start het uitvoert. Hier volgen de stappen voor het uploaden naar AzureML IPython laptop-service:

1. Aanmelden bij uw AzureML-werkruimte, klikt u boven 'Studio' en 'NOTITIEBLOKKEN' aan de linkerkant van de webpagina op.
   
    ![#22 tekenen][22]
2. Klik op 'Nieuw' op de links benedenhoek van de webpagina en selecteer ' Python 2". Vervolgens, Geef een naam op voor de notebook en klik op het vinkje om de nieuwe, lege IPython-Notebook maken.
   
    ![#23 tekenen][23]
3. Klik op het symbool 'Jupyter' in de bovenste linkerhoek van het nieuwe IPython laptop.
   
    ![#24 tekenen][24]
4. Slepen en neerzetten van de steekproef IPython Notebook naar de **structuur** pagina van uw service van AzureML IPython Notebook en klik op **uploaden**. Het voorbeeld IPython Notebook wordt vervolgens geüpload naar de service voor met AzureML IPython Notebook.
   
    ![#25 tekenen][25]

Om te kunnen uitvoeren van de steekproef script IPython laptop of de Python-bestand, de volgende Python pakketten zijn vereist. Als u de service voor met AzureML IPython laptop gebruikt, zijn deze pakketten vooraf worden geïnstalleerd.

    - pandas
    - numpy
    - matplotlib
    - pyodbc
    - PyTables

De aanbevolen volgorde wanneer geavanceerde analytische oplossingen bouwen op AzureML met grote hoeveelheden gegevens is het volgende:

* Gelezen in een kort voorbeeld van de gegevens in een kader gegevens in het geheugen.
* Sommige visualisaties en explorations met behulp van de steekproefgegevens uitvoeren.
* Experimenteer met de functie-engineering met behulp van de steekproefgegevens.
* Voor grotere gegevensverkenning, gegevensmanipulatie en functie-engineering, Python uitgeven rechtstreeks met de SQL DW SQL-query's te gebruiken.
* Bepaal de grootte van de steekproef geschikt is voor het bouwen van Azure Machine Learning-model.

De volgende elementen zijn een paar gegevensverkenning, gegevensvisualisatie en voorbeelden engineering-functie. Meer gegevens explorations vindt u in het voorbeeld IPython Notebook en het voorbeeldbestand Python-script.

### <a name="initialize-database-credentials"></a>Databasereferenties initialiseren
Initialiseren van de verbindingsinstellingen voor database in de volgende variabelen:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database driver>

### <a name="create-database-connection"></a>Databaseverbinding maken
Hier volgt de verbindingsreeks die de verbinding met de database worden gemaakt.

    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Rapport aantal rijen en kolommen in de tabel < nyctaxi_trip >
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_trip>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_trip>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Totale aantal rijen 173179759 =  
* Totale aantal kolommen = 14

### <a name="report-number-of-rows-and-columns-in-table-nyctaxifare"></a>Rapport aantal rijen en kolommen in de tabel < nyctaxi_fare >
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_fare>')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('<nyctaxi_fare>') AND table_schema = ('<schemaname>')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Totale aantal rijen 173179759 =  
* Totale aantal kolommen = 11

### <a name="read-in-a-small-data-sample-from-the-sql-data-warehouse-database"></a>Lees in een steekproef kleine hoeveelheden gegevens uit de SQL Data Warehouse-Database
    t0 = time.time()

    query = '''
        SELECT TOP 10000 t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM <schemaname>.<nyctaxi_trip> t, <schemaname>.<nyctaxi_fare> f
        WHERE datepart("mi",t.pickup_datetime) = 1
        AND   t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Tijd voor het lezen van dat de voorbeeldtabel is 14.096495 seconden.  
Aantal rijen en kolommen opgehaald = (1000, 21).

### <a name="descriptive-statistics"></a>Beschrijvende statistiek
U bent nu klaar om te verkennen de steekproefgegevens. We beginnen met het kijken sommige beschrijvende statistieken voor de **reis\_afstand** (of andere velden die u kiest om op te geven).

    df1['trip_distance'].describe()

### <a name="visualization-box-plot-example"></a>Visualisatie: Voorbeeld van grafiek
Vervolgens kijken we de BoxPlot voor de afstand reis de quantiles visualiseren.

    df1.boxplot(column='trip_distance',return_type='dict')

![Tekenen van #1][1]

### <a name="visualization-distribution-plot-example"></a>Visualisatie: Distributie tekent voorbeeld
Waarnemingspunten die de distributie en een histogram voor de steekproef reis afstanden visualiseren.

    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Tekenen van #2][2]

### <a name="visualization-bar-and-line-plots"></a>Visualisatie: Staaf- en tekent de regel
In dit voorbeeld we de afstand reis in vijf opslaglocaties bin en visualiseren van de binning resultaten.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

We kunnen de bovenstaande bin verdeling in een balk wordt getekend of tekent met regel:

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Tekenen van #3][3]

en

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Tekenen van #4][4]

### <a name="visualization-scatterplot-examples"></a>Visualisatie: Scatterplot voorbeelden
Laten we zien spreidingsgrafiek tekent tussen **reis\_tijd\_in\_sec** en **reis\_afstand** om te zien of er een correlatie

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 tekenen][6]

We kunnen op dezelfde manier controleren of de relatie tussen **snelheid\_code** en **reis\_afstand**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 tekenen][8]

### <a name="data-exploration-on-sampled-data-using-sql-queries-in-ipython-notebook"></a>Gegevensverkenning op steekproefgegevens met SQL-query's in IPython notebook
In deze sectie besproken distributies van gegevens met behulp van de steekproefgegevens die wordt bewaard in de nieuwe tabel die eerder is gemaakt. Houd er rekening mee dat vergelijkbare explorations kunnen worden uitgevoerd met behulp van de oorspronkelijke tabellen.

#### <a name="exploration-report-number-of-rows-and-columns-in-the-sampled-table"></a>Exploratie: Rapport aantal rijen en kolommen in de steekproef tabel
    nrows = pd.read_sql('''SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('<schemaname>.<nyctaxi_sample>')''', conn)
    print 'Number of rows in sample = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''SELECT count(*) FROM information_schema.columns WHERE table_name = ('<nyctaxi_sample>') AND table_schema = '<schemaname>'''', conn)
    print 'Number of columns in sample = %d' % ncols.iloc[0,0]

#### <a name="exploration-tippednot-tripped-distribution"></a>Exploratie: Punt/niet omzeild distributie
    query = '''
        SELECT tipped, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tipped
        '''

    pd.read_sql(query, conn)

#### <a name="exploration-tip-class-distribution"></a>Exploratie: Tip klasse distributie
    query = '''
        SELECT tip_class, count(*) AS tip_freq
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY tip_class
    '''

    tip_class_dist = pd.read_sql(query, conn)

#### <a name="exploration-plot-the-tip-distribution-by-class"></a>Exploratie: De tip distributie door klasse tekenen
    tip_class_dist['tip_freq'].plot(kind='bar')

![#26 tekenen][26]

#### <a name="exploration-daily-distribution-of-trips"></a>Exploratie: Dagelijkse distributie van reizen
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Exploratie: Reis distributie per straten
    query = '''
        SELECT medallion,count(*) AS c
        FROM <schemaname>.<nyctaxi_sample>
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-by-medallion-and-hack-license"></a>Exploratie: Reis distributie door straten en hack licentie
    query = '''select medallion, hack_license,count(*) from <schemaname>.<nyctaxi_sample> group by medallion, hack_license'''
    pd.read_sql(query,conn)


#### <a name="exploration-trip-time-distribution"></a>Exploratie: Verdeling reis
    query = '''select trip_time_in_secs, count(*) from <schemaname>.<nyctaxi_sample> group by trip_time_in_secs order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-trip-distance-distribution"></a>Exploratie: Reis afstand distributie
    query = '''select floor(trip_distance/5)*5 as tripbin, count(*) from <schemaname>.<nyctaxi_sample> group by floor(trip_distance/5)*5 order by count(*) desc'''
    pd.read_sql(query,conn)

#### <a name="exploration-payment-type-distribution"></a>Exploratie: Betaling type distributiepunt
    query = '''select payment_type,count(*) from <schemaname>.<nyctaxi_sample> group by payment_type'''
    pd.read_sql(query,conn)

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Controleer of de uiteindelijke vorm van de tabel featurized
    query = '''SELECT TOP 100 * FROM <schemaname>.<nyctaxi_sample>'''
    pd.read_sql(query,conn)

## <a name="mlmodel"></a>Bouwen van modellen in Azure Machine Learning
Er zijn nu gereed om door te gaan model bouwen en implementeren van modellen in [Azure Machine Learning](https://studio.azureml.net). De gegevens is gereed om te worden gebruikt in een van de eerder vastgestelde namelijk voorspelling-problemen:

1. **Binaire classificatie**: om te voorspellen of een tip betaald is voor een reis.
2. **Multiklassen classificatie**: om te voorspellen van het bereik van een tip betaald, volgens de eerder gedefinieerde klassen.
3. **Regressie taak**: om te voorspellen van de hoeveelheid tip voor een reis betaald.  

Om te beginnen met de oefening modellering, meld u aan bij uw **Azure Machine Learning** werkruimte. Als u nog geen een machine learning-werkruimte hebt gemaakt, raadpleegt u [maken van een Azure ML-werkruimte](../studio/create-workspace.md).

1. Aan de slag met Azure Machine Learning Zie [wat is Azure Machine Learning Studio?](../studio/what-is-ml-studio.md)
2. Meld u aan bij [Azure Machine Learning Studio](https://studio.azureml.net).
3. De startpagina van Studio biedt een schat aan informatie, video's, zelfstudies, koppelingen naar de referentie-Modules en andere bronnen. Raadpleeg voor meer informatie over Azure Machine Learning, de [Azure Machine Learning-documentatiecentrum](https://azure.microsoft.com/documentation/services/machine-learning/).

Een typische trainingsexperiment bestaat uit de volgende stappen uit:

1. Maak een **+ nieuw** experimenteren.
2. De gegevens in Azure ML ophalen.
3. Vooraf verwerken, transformeren en het bewerken van de gegevens zo nodig.
4. Genereren onderdelen naar behoefte.
5. De gegevens splitsen in training/testen/validatie gegevenssets (of afzonderlijke gegevenssets hebben voor elk).
6. Selecteer een of meer machine learning-algoritmen, afhankelijk van het probleem learning om op te lossen. Bijvoorbeeld binaire classificatie, multiklassen classificatie, regressie.
7. Een of meer modellen met behulp van de gegevensset training trainen.
8. De validatie van gegevensset met behulp van de trained modellen beoordelen.
9. De modellen voor het berekenen van de relevante metrische gegevens voor het probleem learning evalueren.
10. Fijnmazig afstemmen van de modellen en selecteert u het beste model om te implementeren.

In deze oefening hebben we al verkend en de gegevens in SQL Data Warehouse engineering en besloten op de grootte van de steekproef om op te nemen in de Azure ML. Hier volgt de procedure voor het bouwen van een of meer van de voorspellende modellen:

1. De gegevens ophalen met behulp van Azure ML de [importgegevens] [ import-data] module beschikbaar in de **gegevensinvoer en uitvoer** sectie. Zie voor meer informatie de [importgegevens] [ import-data] verwijzingspagina module.
   
    ![Gegevens importeren voor Azure ML][17]
2. Selecteer **Azure SQL Database** als de **gegevensbron** in de **eigenschappen** Configuratiescherm.
3. Voer de naam van de DNS-database in de **databaseservernaam** veld. Indeling:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Voer de **databasenaam** in het bijbehorende veld.
5. Voer de *SQL-gebruikersnaam* in de **Server gebruikersaccountnaam**, en de *wachtwoord* in de **Server het wachtwoord voor gebruikersaccount**.
7. In de **databasequery** bewerken van tekst, plakt u de query waarmee de benodigde databasevelden (inclusief eventuele berekende velden, zoals de labels) worden opgehaald en omlaag de gegevens naar de gewenste samplegrootte voorbeelden.

Een voorbeeld van een binaire indeling experiment lezen van gegevens rechtstreeks vanuit de SQL Data Warehouse-database is in de afbeelding hieronder (Vergeet niet de tabel namen nyctaxi_trip en nyctaxi_fare vervangt door de naam van het schema en de tabelnamen van de die u in uw scenario gebruikt). Vergelijkbare experimenten kunnen worden samengesteld voor multiklassen classificatie en regressie problemen.

![Azure ML Train][10]

> [!IMPORTANT]
> In de gegevens modellering extractie en voorbeelden van de query wordt opgegeven in de vorige secties **alle labels voor de drie modellering oefeningen zijn opgenomen in de query**. Een belangrijke (vereist) stap in elk van de oefeningen modellering is het **uitsluiten** de onnodige labels voor de andere twee problemen en andere **doel lekken**. Bijvoorbeeld: bij gebruik van binaire classificatie gebruiken het label **punt** en wilt uitsluiten van de velden **tip\_klasse**, **tip\_bedrag**, en **totale\_bedrag**. De laatste zijn doel lekken omdat ze de tip impliceren betaald.
> 
> Als u eventuele overbodige kolommen uitsluiten of lekken zijn gericht, mag u de [Select Columns in Dataset] [ select-columns] module of de [metagegevens bewerken][edit-metadata]. Zie voor meer informatie [Select Columns in Dataset] [ select-columns] en [metagegevens bewerken] [ edit-metadata] verwijzen naar pagina's.
> 
> 

## <a name="mldeploy"></a>Modellen in Azure Machine Learning implementeren
Wanneer uw model klaar is, kunt u het eenvoudig implementeren als een webservice rechtstreeks vanuit het experiment. Zie voor meer informatie over het implementeren van Azure ML webservices [Azure Machine Learning-webservice implementeren](../studio/publish-a-machine-learning-web-service.md).

Voor het implementeren van een nieuwe webservice, moet u:

1. Een score experiment maken.
2. De webservice implementeren.

Maken van een experiment score van een **voltooid** training experiment, klikt u op **maken score berekenen EXPERIMENTEREN** in de onderste actiebalk.

![Score berekenen voor Azure][18]

Azure Machine Learning probeert te maken van een score experiment op basis van de onderdelen van het trainingsexperiment. In het bijzonder wordt:

1. Sla het getrainde model en het model trainingsmodules verwijderen.
2. Een logische identificeren **poort invoer** vertegenwoordigt het verwachte invoergegevens schema.
3. Een logische identificeren **uitvoerpoort** vertegenwoordigt de uitvoerschema verwachte web service.

Wanneer het experiment scoreprofiel is gemaakt, controleren en maken indien nodig aanpassen. Een typische aanpassing is invoergegevensset en/of queryparameters vervangen door een exclusief label velden, zoals deze is alleen beschikbaar wanneer de service wordt aangeroepen. Het is ook raadzaam de grootte van de invoergegevensset en/of de query beperken tot een aantal records net voldoende is om aan te geven van de invoer schema. Voor de uitvoerpoort wordt meestal alleen opnemen en uitsluiten van alle invoervelden de **Scored Labels** en **berekend kansen** in het uitvoer met de [Select Columns in Dataset] [ select-columns] module.

Een voorbeeld van een experiment score berekenen is opgegeven in de afbeelding hieronder. Wanneer u klaar bent om te implementeren, klikt u op de **WEBSERVICE PUBLICEERT** knop in de onderste actiebalk.

![Azure ML publiceren][11]

## <a name="summary"></a>Samenvatting
Als u samenvatting wat we hebben gedaan in deze zelfstudie scenario, wilt u hebt gemaakt een Azure data wetenschap-omgeving hebt gewerkt met een grote openbare gegevensset duurt het door het proces Team gegevens wetenschap, helemaal uit gegevens overname naar model training, en vervolgens naar de implementatie van een Azure Machine Learning-webservice.

### <a name="license-information"></a>Licentie-informatie
In dit voorbeeld scenario en de bijbehorende scripts en IPython notebook(s) worden gedeeld door Microsoft onder de MIT-licentie. Controleer of u het bestand LICENSE.txt in in de map van de voorbeeldcode op GitHub voor meer informatie.

## <a name="references"></a>Verwijzingen
• [Andrés Monroy NYC Taxi reizen downloadpagina](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxi reis gegevens door Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi en Limousine Commissie onderzoek en statistieken](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[1]: ./media/sqldw-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/sqldw-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/sqldw-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/sqldw-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/sqldw-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/sqldw-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/sqldw-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/sqldw-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/sqldw-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/sqldw-walkthrough/azuremltrain.png
[11]: ./media/sqldw-walkthrough/azuremlpublish.png
[12]: ./media/sqldw-walkthrough/ssmsconnect.png
[13]: ./media/sqldw-walkthrough/executescript.png
[14]: ./media/sqldw-walkthrough/sqlserverproperties.png
[15]: ./media/sqldw-walkthrough/sqldefaultdirs.png
[16]: ./media/sqldw-walkthrough/bulkimport.png
[17]: ./media/sqldw-walkthrough/amlreader.png
[18]: ./media/sqldw-walkthrough/amlscoring.png
[19]: ./media/sqldw-walkthrough/ps_download_scripts.png
[20]: ./media/sqldw-walkthrough/ps_load_data.png
[21]: ./media/sqldw-walkthrough/azcopy-overwrite.png
[22]: ./media/sqldw-walkthrough/ipnb-service-aml-1.png
[23]: ./media/sqldw-walkthrough/ipnb-service-aml-2.png
[24]: ./media/sqldw-walkthrough/ipnb-service-aml-3.png
[25]: ./media/sqldw-walkthrough/ipnb-service-aml-4.png
[26]: ./media/sqldw-walkthrough/tip_class_hist_1.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
