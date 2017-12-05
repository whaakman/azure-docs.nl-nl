---
title: Bouw en implementeer een machine learning-model met SQL Server op een virtuele machine in Azure | Microsoft-Docs
description: Geavanceerde analyses proces en de technologie in actie
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 6066b083-262c-4453-a712-a5c05acc3df8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: fashah;bradsev
ms.openlocfilehash: 4157820bad3c0d7c07965e4a5556db2f6fb69fe2
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="the-team-data-science-process-in-action-using-sql-server"></a>De procedure van wetenschappelijke gegevens Team in actie: met behulp van SQL Server
In deze zelfstudie helpt u bij het proces van het maken en implementeren van een machine learning-model met behulp van SQL Server en een openbare gegevensset--de [NYC Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gegevensset. De procedure volgt een standaard wetenschappelijke werkstroom: opnemen en Verken de gegevens, engineering-functies om te vergemakkelijken learning, en vervolgens te bouwen en implementeren van een model.

## <a name="dataset"></a>NYC Taxi reizen gegevensset beschrijving
De gegevens van de NYC Taxi reis is ongeveer 20GB gecomprimeerde CSV-bestanden (~ 48GB ongecomprimeerde), die bestaat uit meer dan 173 miljoen afzonderlijke reizen en de tarieven voor elke reis betaald. Elke record reis omvat de locatie ophalen en inleverbibliotheek en tijd, geanonimiseerde hack (van stuurprogramma) licentienummer en nummer straten (taxi van unieke id). De gegevens bevat informatie over alle reizen in het jaar 2013 en is beschikbaar in de volgende twee gegevenssets voor elke maand:

1. Trip_data CSV bevat reis details, zoals het aantal passagiers, ophalen en dropoff punten reis duur en duur van reis. Hier volgen enkele voorbeeldrecords:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. 'trip_fare' CSV bevat details van het tarief dat betaald voor elke reis, zoals betalingstype, tarief bedrag, extra kosten en belastingen, tips en tolgelden, en het totaalbedrag betaald. Hier volgen enkele voorbeeldrecords:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

De unieke sleutel toevoegen reis\_gegevens en reis\_tarief dat bestaat uit de velden: straten, hack\_en ophalen van certificaat\_datetime.

## <a name="mltasks"></a>Voorbeelden van voorspelling taken
We zullen formuleren drie voorspelling problemen op basis van de *tip\_bedrag*, namelijk:

1. Binaire classificatie: voorspellen of een tip betaald is voor een reis, dat wil zeggen een *tip\_bedrag* die groter is dan $0 een voorbeeld van een positieve is, terwijl een *tip\_bedrag* van $0 is een voorbeeld van een negatief.
2. Multiklassen classificatie: om te voorspellen van het bereik van een tip voor de reis betaald. We delen de *tip\_bedrag* in vijf opslaglocaties of klassen:
   
        Class 0 : tip_amount = $0
        Class 1 : tip_amount > $0 and tip_amount <= $5
        Class 2 : tip_amount > $5 and tip_amount <= $10
        Class 3 : tip_amount > $10 and tip_amount <= $20
        Class 4 : tip_amount > $20
3. Taak regressie: om te voorspellen van de hoeveelheid tip voor een reis betaald.  

## <a name="setup"></a>Instelling van de Azure data wetenschappelijke omgeving voor geavanceerde analyses
Zoals u van ziet de [uw omgeving plannen](plan-your-environment.md) handleiding, er zijn verschillende opties voor het werken met de gegevensset NYC Taxi reizen in Azure:

* Werken met de gegevens in Azure blobs vervolgens Azure Machine Learning-model
* De gegevens in een SQL Server-database en de Azure Machine Learning-model laden

In deze zelfstudie wordt gedemonstreerd parallelle bulkimport van de gegevens naar een SQL-Server, gegevensverkenning, functie engineering en omlaag sampling met behulp van SQL Server Management Studio, alsmede IPython laptop gebruiken. [Voorbeeld van scripts](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/DataScienceScripts) en [IPython notitieblokken](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) in GitHub worden gedeeld. Een voorbeeld IPython notebook werken met de gegevens in Azure BLOB's is ook beschikbaar in dezelfde locatie.

Voor het instellen van uw omgeving voor Gegevenswetenschap Azure:

1. [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md)
2. [Een Azure Machine Learning-werkruimte maken](../studio/create-workspace.md)
3. [Inrichten van een virtuele Machine van de gegevens wetenschappelijke](../data-science-virtual-machine/setup-sql-server-virtual-machine.md), waarmee u een SQL-Server en een IPython laptop-server.
   
   > [!NOTE]
   > De voorbeeldscripts en IPython notitieblokken worden gedownload met uw virtuele-machine voor Gegevenswetenschap tijdens de installatie. Wanneer het VM-script voor na de installatie is voltooid, zijn de voorbeelden in de bibliotheek documenten van de VM:  
   > 
   > * Voorbeeld Scripts:`C:\Users\<user_name>\Documents\Data Science Scripts`  
   > * Voorbeeld IPython notitieblokken:`C:\Users\<user_name>\Documents\IPython Notebooks\DataScienceSamples`  
   >   waar `<user_name>` is Windows-aanmeldingsnaam van de VM. De voorbeeld-mappen als wordt verwezen **voorbeeldscripts** en **voorbeeld IPython notitieblokken**.
   > 
   > 

Op basis van de grootte van de gegevensset, locatie van de gegevensbron en de geselecteerde Azure doelomgeving, dit scenario is vergelijkbaar met [Scenario \#5: grote gegevensset in een lokale bestanden doel-SQL-Server in Azure VM](plan-sample-scenarios.md#largelocaltodb).

## <a name="getdata"></a>De gegevens van openbare bron ophalen
Ophalen van de [NYC Taxi reizen](http://www.andresmh.com/nyctaxitrips/) gegevensset van de openbare locatie, u kunt een van de methoden van [gegevens verplaatsen en naar Azure Blob Storage](move-azure-blob.md) de gegevens kopiëren naar de nieuwe virtuele machine.

Kopieert de gegevens met behulp van AzCopy:

1. Aanmelden bij uw virtuele machine (VM)
2. Een nieuwe map maken in de VM-gegevensschijf (Opmerking: de tijdelijke schijf die wordt geleverd bij de virtuele machine als een gegevensschijf niet gebruiken).
3. Voer de volgende Azcopy-opdrachtregel, waarbij < path_to_data_folder > vervangt door de gegevensmap van uw in (2) gemaakt in een opdrachtpromptvenster:
   
        "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy\azcopy" /Source:https://nyctaxitrips.blob.core.windows.net/data /Dest:<path_to_data_folder> /S
   
    Wanneer de AzCopy is voltooid, in totaal 24 ingepakte CSV-bestanden (12 voor reis\_gegevens en 12 voor reis\_tarief) moet in de map data.
4. Pak het gedownloade bestanden. Onthoud in welke map waar de niet-gecomprimeerde bestanden zich bevinden. Deze map worden aangeduid als de < pad\_naar\_gegevens\_bestanden\>.

## <a name="dbload"></a>Gegevens voor bulksgewijs importeren in SQL Server-Database
De prestaties van laden/overdracht van grote hoeveelheden gegevens naar een SQL-database en de volgende query's kunnen worden verbeterd via *gepartitioneerde tabellen en weergaven*. In deze sectie wordt er volgt u de instructies die worden beschreven [parallelle bulksgewijs gegevens importeren met behulp van SQL partitietabellen](parallel-load-sql-partitioned-tables.md) een nieuwe database maken en de gegevens in gepartitioneerde tabellen parallel laden.

1. Terwijl u bent aangemeld met uw virtuele machine, start **SQL Server Management Studio**.
2. Verbinding maken met Windows-verificatie.
   
    ![SSMS verbinden][12]
3. Als u hebt nog niet gewijzigd van de SQL Server-verificatiemodus en een nieuwe SQL-aanmeldingsgebruiker gemaakt, opent u het script-bestand met de naam **wijzigen\_auth.sql** in de **voorbeeldscripts** map. Standaard-gebruikersnaam en wachtwoord wijzigen. Klik op **! Uitvoeren van** op de werkbalk om het script uitvoeren.
   
    ![Script uitvoeren][13]
4. Controleren en/of de SQL Server-database en logboekbestanden standaardmappen om ervoor te zorgen dat zojuist gemaakt databases worden opgeslagen in een gegevensschijf wijzigen. De installatiekopie van het SQL Server VM die is geoptimaliseerd voor datawarehousing belastingen is vooraf geconfigureerd met gegevens en logboekbestanden schijven. Als uw virtuele machine geen een gegevensschijf bevat en u een nieuwe virtuele harde schijven toegevoegd tijdens het installatieproces van de VM, wijzigt u de standaard-mappen als volgt:
   
   * Met de rechtermuisknop op de naam van de SQL Server in het linkerdeelvenster en klik op **eigenschappen**.
     
       ![Eigenschappen van SQL Server][14]
   * Selecteer **Database-instellingen** van de **een pagina selecteren** lijst aan de linkerkant.
   * Controleren en/of wijzig de **Database standaardlocaties** naar de **gegevensschijf** locaties van uw keuze. Dit is waar nieuwe databases zich bevinden als gemaakt met de standaardinstellingen van de locatie.
     
       ![Standaardinstellingen van de SQL-Database][15]  
5. Voor het maken van een nieuwe database en een set bestandsgroepen voor het opslaan van de gepartitioneerde tabellen, opent u het voorbeeldscript **maken\_db\_default.sql**. Het script maakt een nieuwe database met de naam **TaxiNYC** en 12 bestandsgroepen op de standaardlocatie van de gegevens. Elke bestandsgroep bevatten één maand in reis\_gegevens en reis\_ritbedrag gegevens. Wijzig de naam van de database desgewenst. Klik op **! Uitvoeren van** het script uit te voeren.
6. Maak vervolgens twee partitietabellen, één voor de reis\_gegevens en een andere voor de reis\_tarief. Open het voorbeeldscript **maken\_gepartitioneerde\_table.sql**, die wordt:
   
   * Maak een partitiefunctie als u wilt de gegevens splitsen per maand.
   * Maak een partitieschema van elke maand om gegevens te koppelen aan een andere bestandsgroep.
   * Maak twee gepartitioneerde tabellen die zijn toegewezen aan het partitieschema: **nyctaxi\_reis** bevatten de reis\_gegevens en **nyctaxi\_tarief** bevatten de reis\_ritbedrag gegevens.
     
     Klik op **! Uitvoeren van** uitvoeren van het script en de gepartitioneerde tabellen maken.
7. In de **voorbeeldscripts** map, er zijn twee PowerShell-voorbeeldscripts bedoeld ter illustratie van parallelle bulksgewijs invoer van gegevens naar SQL Server-tabellen.
   
   * **BCP\_parallelle\_generic.ps1** is een algemeen script met parallelle bulksgewijs Importeer gegevens in een tabel. Dit script voor het instellen van de invoer- en doel-variabelen, zoals aangegeven in de regels met opmerkingen in het script wijzigen.
   * **BCP\_parallelle\_nyctaxi.ps1** is een vooraf geconfigureerde versie van het algemene script en naar beide tabellen voor de gegevens van de NYC Taxi reizen laden kan worden gebruikt.  
8. Met de rechtermuisknop op de **bcp\_parallelle\_nyctaxi.ps1** scriptnaam en klikt u op **bewerken** openen in PowerShell. De vooraf ingestelde variabelen bekijken en wijzigen afhankelijk van uw naam van de geselecteerde database, invoergegevens map, doelmap logboek en paden naar de indeling voorbeeldbestanden **nyctaxi_trip.xml** en **nyctaxi\_fare.xml** (opgegeven in de **voorbeeldscripts** map).
   
    ![Gegevens voor bulksgewijs importeren][16]
   
    U kunt ook de verificatiemodus selecteren, de standaardwaarde is Windows-verificatie. Klik op de groene pijl in de werkbalk om uit te voeren. Het script wordt gestart, 24 importeren bulkbewerkingen in parallelle 12 voor elke gepartitioneerde tabel. U kunt de voortgang gegevens importeren door het openen van de standaardgegevensmap voor SQL Server als hierboven.
9. Het PowerShell-script rapporteert de begin- en eindtijd. Wanneer alle bulksgewijs invoer is voltooid, wordt de eindtijd gerapporteerd. Controleer de doelmap logboek om te controleren of de bulkimport gewijzigd, dat wil zeggen zijn, er geen fouten gemeld in de logboekmap doel.
10. Uw database is nu gereed voor exploratie, functie-engineering en andere bewerkingen. Aangezien de tabellen worden gepartitioneerd volgens het **ophalen\_datetime** veld, query's, waaronder **ophalen\_datetime** voorwaarden de **waar** component profiteren van het partitieschema.
11. In **SQL Server Management Studio**, het opgegeven voorbeeldscript verkennen **voorbeeld\_queries.sql**. Als u wilt uitvoeren op een van de voorbeeldquery's, markeer de queryregels en klik vervolgens op **! Uitvoeren van** op de werkbalk.
12. De gegevens van de NYC Taxi heen worden geladen in twee afzonderlijke tabellen. Ter verbetering van de join-bewerkingen, is het raadzaam de tabellen te indexeren. Het voorbeeldscript **maken\_gepartitioneerde\_index.sql** maakt gepartitioneerde indexen voor de samengestelde join-sleutel **straten, hack\_licentie, en ophalen van\_datetime**.

## <a name="dbexplore"></a>Gegevensverkenning en functie-Engineering in SQL Server
In deze sectie voert we exploratie en functie voor het genereren van gegevens door het uitvoeren van SQL-query's rechtstreeks in de **SQL Server Management Studio** eerder met behulp van de SQL Server-database hebt gemaakt. Een voorbeeld van een script met de naam **voorbeeld\_queries.sql** vindt u in de **voorbeeldscripts** map. Het script voor het wijzigen van de naam van de database wijzigen als deze van de standaard verschilt: **TaxiNYC**.

In deze oefening wordt:

* Verbinding maken met **SQL Server Management Studio** gebruikt op Windows-verificatie of SQL-verificatie en de SQL-aanmeldingsnaam en wachtwoord.
* Verken de gegevens distributies van enkele velden in verschillende tijdvensters.
* Onderzoek de kwaliteit van de gegevens van de breedtegraad -velden.
* Genereren van binaire en multiklassen classificatielabels op basis van de **tip\_bedrag**.
* Genereren van functies en reis afstanden compute/vergelijken.
* Voeg de twee tabellen en uitpakken van een steekproef die worden gebruikt voor het bouwen van modellen.

Wanneer u klaar om door te gaan naar Azure Machine Learning bent, mag u ofwel:  

1. Opslaan van de laatste SQL-query voor het uitpakken en de voorbeeldgegevens en kopiëren en plakken van de query rechtstreeks in een [importgegevens] [ import-data] -module in Azure Machine Learning, of
2. Behouden van de steekproef en engineering gegevens die u wilt gebruiken voor model bouwen in een nieuwe database tabel en gebruik de nieuwe tabel in de [importgegevens] [ import-data] -module in Azure Machine Learning.

In deze sectie zullen we de laatste query voor het uitpakken en de voorbeeldgegevens opslaan. De tweede methode wordt geïllustreerd in de [Gegevensverkenning en functie-Engineering in IPython Notebook](#ipnb) sectie.

Voor een snelle verificatie van het aantal rijen en kolommen in de tabellen ingevuld eerder met parallelle bulkimport,

    -- Report number of rows in table nyctaxi_trip without table scan
    SELECT SUM(rows) FROM sys.partitions WHERE object_id = OBJECT_ID('nyctaxi_trip')

    -- Report number of columns in table nyctaxi_trip
    SELECT COUNT(*) FROM information_schema.columns WHERE table_name = 'nyctaxi_trip'

#### <a name="exploration-trip-distribution-by-medallion"></a>Exploratie: Reis distributie door straten
In dit voorbeeld identificeert de straten (taxi getallen) met meer dan 100 reizen binnen een bepaalde periode. De query zou gebruikmaken van de gepartitioneerde tabel omdat deze zijn afhankelijk van het partitieschema van **ophalen\_datetime**. Opvragen van de volledige gegevensset maakt ook gebruik van de gepartitioneerde tabel en/of index van de scan.

    SELECT medallion, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    GROUP BY medallion
    HAVING COUNT(*) > 100

#### <a name="exploration-trip-distribution-by-medallion-and-hacklicense"></a>Exploratie: Reis distributie door straten en hack_license
    SELECT medallion, hack_license, COUNT(*)
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20130131'
    GROUP BY medallion, hack_license
    HAVING COUNT(*) > 100

#### <a name="data-quality-assessment-verify-records-with-incorrect-longitude-andor-latitude"></a>Gegevens beoordeling: Controleer of records met onjuiste lengtegraad en/of breedtegraad
In dit voorbeeld onderzoekt als een van de velden lengtegraad en/of breedtegraad een ongeldige waarde bevatten (radiaal graden moet tussen-90 en 90), of (0, 0) coördinaten.

    SELECT COUNT(*) FROM nyctaxi_trip
    WHERE pickup_datetime BETWEEN '20130101' AND '20130331'
    AND  (CAST(pickup_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(pickup_latitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_longitude AS float) NOT BETWEEN -90 AND 90
    OR    CAST(dropoff_latitude AS float) NOT BETWEEN -90 AND 90
    OR    (pickup_longitude = '0' AND pickup_latitude = '0')
    OR    (dropoff_longitude = '0' AND dropoff_latitude = '0'))

#### <a name="exploration-tipped-vs-not-tipped-trips-distribution"></a>Exploratie: Gekantelde vs. Niet reizen punt distributie
Dit voorbeeld wordt het aantal reizen die zijn punt versus geen punt in een bepaald moment periode (of in de volledige gegevensset als die betrekking hebben op het volledige jaar). Deze verdeling weerspiegelt de distributie van de binaire label moet later worden gebruikt voor binaire classificatie modelleren.

    SELECT tipped, COUNT(*) AS tip_freq FROM (
      SELECT CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped, tip_amount
      FROM nyctaxi_fare
      WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tipped

#### <a name="exploration-tip-classrange-distribution"></a>Exploratie: Tip distributie van de klasse-bereik
In dit voorbeeld berekent de verdeling van tip bereiken in een bepaalde periode (of in de volledige gegevensset als die betrekking hebben op het volledige jaar). Dit is de distributie van de label-klassen die later wordt gebruikt voor multiklassen classificatie modelleren.

    SELECT tip_class, COUNT(*) AS tip_freq FROM (
        SELECT CASE
            WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_fare
    WHERE pickup_datetime BETWEEN '20130101' AND '20131231') tc
    GROUP BY tip_class

#### <a name="exploration-compute-and-compare-trip-distance"></a>Exploratie: Berekenen en reis afstand vergelijken
In dit voorbeeld wordt de lengtegraad ophalen en inleverbibliotheek en breedtegraad naar SQL Geografie verwijst, berekent de reis afstand met behulp van SQL Geografie punten verschil en retourneert een steekproef van de resultaten voor vergelijking. In het voorbeeld de resultaten beperkt tot geldig coördinaten alleen met behulp van de kwaliteit assessment gegevensquery eerder besproken.

    SELECT
    pickup_location=geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326)
    ,dropoff_location=geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326)
    ,trip_distance
    ,computedist=round(geography::STPointFromText('POINT(' + pickup_longitude + ' ' + pickup_latitude + ')', 4326).STDistance(geography::STPointFromText('POINT(' + dropoff_longitude + ' ' + dropoff_latitude + ')', 4326))/1000, 2)
    FROM nyctaxi_trip
    tablesample(0.01 percent)
    WHERE CAST(pickup_latitude AS float) BETWEEN -90 AND 90
    AND   CAST(dropoff_latitude AS float) BETWEEN -90 AND 90
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'

#### <a name="feature-engineering-in-sql-queries"></a>Functie-Engineering in SQL-query 's
Het label genereren en Geografie conversie exploratie query's kunnen ook worden gebruikt voor het genereren van labels en-functies door de tellen deel te verwijderen. Voorbeelden van technische SQL aanvullende functies zijn beschikbaar in de [Gegevensverkenning en functie-Engineering in IPython Notebook](#ipnb) sectie. Het is efficiënter de functie generatie query's uitvoeren op de volledige gegevensset of een grote subset van met behulp van SQL-query's die rechtstreeks op het database-exemplaar van SQL Server worden uitgevoerd. De query's kunnen worden uitgevoerd in **SQL Server Management Studio**, IPython laptop of elk hulpprogramma/ontwikkelomgeving die toegang de database lokaal of extern tot heeft.

#### <a name="preparing-data-for-model-building"></a>Voorbereiden van gegevens voor het Model bouwen
De volgende query joins de **nyctaxi\_reis** en **nyctaxi\_tarief** tabellen, genereert een label binaire classificatie **punt**, een label met meerdere klasse classificatie **tip\_klasse**, en extraheert met een willekeurige steekproef 1% van de volledige gegevensset lid. Deze query kan worden gekopieerd en geplakt rechtstreeks in de [Azure Machine Learning Studio](https://studio.azureml.net) [importgegevens] [ import-data] -module voor directe gegevensopname van het exemplaar van SQL Server-database in Azure. De query worden records met onjuiste uitgesloten (0, 0) coördinaten.

    SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,     f.total_amount, f.tip_amount,
        CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END AS tipped,
        CASE WHEN (tip_amount = 0) THEN 0
            WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
            WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
            WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
            ELSE 4
        END AS tip_class
    FROM nyctaxi_trip t, nyctaxi_fare f
    TABLESAMPLE (1 percent)
    WHERE t.medallion = f.medallion
    AND   t.hack_license = f.hack_license
    AND   t.pickup_datetime = f.pickup_datetime
    AND   pickup_longitude != '0' AND dropoff_longitude != '0'


## <a name="ipnb"></a>Gegevensverkenning en functie-Engineering in IPython Notebook
In deze sectie wordt uitgevoerd, er gegevensverkenning en functie genereren met behulp van Python- en SQL-query's op de SQL Server-database eerder hebt gemaakt. Een voorbeeld IPython notebook met de naam **machine-Learning-data-science-process-sql-story.ipynb** vindt u in de **voorbeeld IPython notitieblokken** map. Deze laptop is ook beschikbaar op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks).

De aanbevolen volgorde bij het werken met big data, is het volgende:

* Gelezen in een kort voorbeeld van de gegevens in een kader gegevens in het geheugen.
* Sommige visualisaties en explorations met behulp van de steekproefgegevens uitvoeren.
* Experimenteer met de functie-engineering met behulp van de steekproefgegevens.
* Voor grotere gegevensverkenning, gegevensmanipulatie en functie-engineering, Python SQL-query's uitgeven rechtstreeks op de SQL Server-database in de Azure VM te gebruiken.
* Bepaal de grootte van de steekproef moet worden gebruikt voor het bouwen van Azure Machine Learning-model.

Als u klaar bent om door te gaan naar Azure Machine Learning kan u:  

1. Opslaan van de laatste SQL-query voor het uitpakken en de voorbeeldgegevens en kopiëren en plakken van de query rechtstreeks in een [importgegevens] [ import-data] -module in Azure Machine Learning. Deze methode wordt geïllustreerd in de [gebouw modellen in Azure Machine Learning](#mlmodel) sectie.    
2. Behouden van de steekproef en engineering-gegevens die u wilt gebruiken voor model bouwen in een nieuwe database en gebruik vervolgens de nieuwe tabel in de [importgegevens] [ import-data] module.

Hier volgen enkele gegevensverkenning: gegevensvisualisatie en voorbeelden engineering-functie. Zie voor meer voorbeelden van de notebook voorbeeld SQL IPython in de **voorbeeld IPython notitieblokken** map.

#### <a name="initialize-database-credentials"></a>Databasereferenties initialiseren
Initialiseren van de verbindingsinstellingen voor database in de volgende variabelen:

    SERVER_NAME=<server name>
    DATABASE_NAME=<database name>
    USERID=<user name>
    PASSWORD=<password>
    DB_DRIVER = <database server>

#### <a name="create-database-connection"></a>Databaseverbinding maken
    CONNECTION_STRING = 'DRIVER={'+DRIVER+'};SERVER='+SERVER_NAME+';DATABASE='+DATABASE_NAME+';UID='+USERID+';PWD='+PASSWORD
    conn = pyodbc.connect(CONNECTION_STRING)

#### <a name="report-number-of-rows-and-columns-in-table-nyctaxitrip"></a>Rapport aantal rijen en kolommen in tabel nyctaxi_trip
    nrows = pd.read_sql('''
        SELECT SUM(rows) FROM sys.partitions
        WHERE object_id = OBJECT_ID('nyctaxi_trip')
    ''', conn)

    print 'Total number of rows = %d' % nrows.iloc[0,0]

    ncols = pd.read_sql('''
        SELECT COUNT(*) FROM information_schema.columns
        WHERE table_name = ('nyctaxi_trip')
    ''', conn)

    print 'Total number of columns = %d' % ncols.iloc[0,0]

* Totale aantal rijen 173179759 =  
* Totale aantal kolommen = 14

#### <a name="read-in-a-small-data-sample-from-the-sql-server-database"></a>Lees in een steekproef kleine hoeveelheden gegevens uit de SQL Server-Database
    t0 = time.time()

    query = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax,
            f.tolls_amount, f.total_amount, f.tip_amount
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (0.05 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
    '''

    df1 = pd.read_sql(query, conn)

    t1 = time.time()
    print 'Time to read the sample table is %f seconds' % (t1-t0)

    print 'Number of rows and columns retrieved = (%d, %d)' % (df1.shape[0], df1.shape[1])

Tijd voor het lezen van dat de voorbeeldtabel is 6.492000 seconden  
Aantal rijen en kolommen opgehaald = (84952, 21)

#### <a name="descriptive-statistics"></a>Beschrijvende statistiek
U bent er nu klaar om te verkennen de steekproefgegevens. We beginnen met het kijken beschrijvende statistieken voor de **reis\_afstand** (of andere) sleutelvelden:

    df1['trip_distance'].describe()

#### <a name="visualization-box-plot-example"></a>Visualisatie: Voorbeeld van grafiek
Vervolgens kijken we de BoxPlot voor de afstand reis de quantiles visualiseren

    df1.boxplot(column='trip_distance',return_type='dict')

![Tekenen van #1][1]

#### <a name="visualization-distribution-plot-example"></a>Visualisatie: Distributie tekent voorbeeld
    fig = plt.figure()
    ax1 = fig.add_subplot(1,2,1)
    ax2 = fig.add_subplot(1,2,2)
    df1['trip_distance'].plot(ax=ax1,kind='kde', style='b-')
    df1['trip_distance'].hist(ax=ax2, bins=100, color='k')

![Tekenen van #2][2]

#### <a name="visualization-bar-and-line-plots"></a>Visualisatie: De balk en de regel waarnemingspunten
In dit voorbeeld we de afstand reis in vijf opslaglocaties bin en visualiseren van de binning resultaten.

    trip_dist_bins = [0, 1, 2, 4, 10, 1000]
    df1['trip_distance']
    trip_dist_bin_id = pd.cut(df1['trip_distance'], trip_dist_bins)
    trip_dist_bin_id

We kunnen de bovenstaande bin verdeling in een balk wordt getekend of regel tekent zoals hieronder

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='bar')

![Tekenen van #3][3]

    pd.Series(trip_dist_bin_id).value_counts().plot(kind='line')

![Tekenen van #4][4]

#### <a name="visualization-scatterplot-example"></a>Visualisatie: Scatterplot voorbeeld
Laten we zien spreidingsgrafiek tekent tussen **reis\_tijd\_in\_sec** en **reis\_afstand** om te zien of er een correlatie

    plt.scatter(df1['trip_time_in_secs'], df1['trip_distance'])

![#6 tekenen][6]

We kunnen op dezelfde manier controleren of de relatie tussen **snelheid\_code** en **reis\_afstand**.

    plt.scatter(df1['passenger_count'], df1['trip_distance'])

![#8 tekenen][8]

### <a name="sub-sampling-the-data-in-sql"></a>Subplan steekproef nemen van de gegevens in SQL
Bij het voorbereiden van gegevens voor model bouwen in [Azure Machine Learning Studio](https://studio.azureml.net), desgewenst kunt u ofwel op de **SQL-query om rechtstreeks in de module gegevens importeren** of behouden van de gegevens ontworpen en opgevangen in een nieuwe tabel, kunt u in de [gegevens importeren] [ import-data] module met een eenvoudige **Selecteer * FROM < uw\_nieuwe\_tabel\_name >**.

In deze sectie maakt we een nieuwe tabel de opgevangen en engineering-gegevens kan bevatten. Een voorbeeld van een directe SQL-query voor het model bouwen vindt u in de [Gegevensverkenning en functie-Engineering in SQL Server](#dbexplore) sectie.

#### <a name="create-a-sample-table-and-populate-with-1-of-the-joined-tables-drop-table-first-if-it-exists"></a>Maken van een voorbeeld van een tabel en vullen met 1% van de gekoppelde tabellen. Verwijderen in de eerste tabel als deze bestaat.
In deze sectie wordt de tabellen samenvoegen **nyctaxi\_reis** en **nyctaxi\_tarief**, met een willekeurige steekproef 1% ophalen en de steekproefgegevens in de naam van een nieuwe tabel behouden **nyctaxi\_één\_procent**:

    cursor = conn.cursor()

    drop_table_if_exists = '''
        IF OBJECT_ID('nyctaxi_one_percent', 'U') IS NOT NULL DROP TABLE nyctaxi_one_percent
    '''

    nyctaxi_one_percent_insert = '''
        SELECT t.*, f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount, f.total_amount, f.tip_amount
        INTO nyctaxi_one_percent
        FROM nyctaxi_trip t, nyctaxi_fare f
        TABLESAMPLE (1 PERCENT)
        WHERE t.medallion = f.medallion
        AND   t.hack_license = f.hack_license
        AND   t.pickup_datetime = f.pickup_datetime
        AND   pickup_longitude <> '0' AND dropoff_longitude <> '0'
    '''

    cursor.execute(drop_table_if_exists)
    cursor.execute(nyctaxi_one_percent_insert)
    cursor.commit()

### <a name="data-exploration-using-sql-queries-in-ipython-notebook"></a>Gegevensverkenning met SQL-query's in IPython Notebook
In deze sectie besproken distributies van gegevens met behulp van de 1-% actieve gegevens die worden bewaard in de nieuwe tabel die eerder is gemaakt. Houd er rekening mee dat vergelijkbare explorations kunnen worden uitgevoerd met behulp van de oorspronkelijke tabellen, waarbij optioneel **TABLESAMPLE** beperken de exploratie voorbeeld of door het beperken van de resultaten in een bepaald moment periode met de **ophalen\_datetime** partities, zoals geïllustreerd in de [Gegevensverkenning en functie-Engineering in SQL Server](#dbexplore) sectie.

#### <a name="exploration-daily-distribution-of-trips"></a>Exploratie: Dagelijkse distributie van reizen
    query = '''
        SELECT CONVERT(date, dropoff_datetime) AS date, COUNT(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY CONVERT(date, dropoff_datetime)
    '''

    pd.read_sql(query,conn)

#### <a name="exploration-trip-distribution-per-medallion"></a>Exploratie: Reis distributie per straten
    query = '''
        SELECT medallion,count(*) AS c
        FROM nyctaxi_one_percent
        GROUP BY medallion
    '''

    pd.read_sql(query,conn)

### <a name="feature-generation-using-sql-queries-in-ipython-notebook"></a>Functie genereren met behulp van SQL-query's in IPython Notebook
In deze sectie er nieuwe labels worden gegenereerd en functies direct met SQL-query's, op de % 1-voorbeeldtabel we gemaakt in de vorige sectie.

#### <a name="label-generation-generate-class-labels"></a>Label generatie: Labels van de klasse te genereren
In het volgende voorbeeld wordt er twee sets van labels te gebruiken voor het modelleren gegenereerd:

1. Binaire klasse Labels **punt** (als een tip krijgt voorspellen)
2. Multiklassen Labels **tip\_klasse** (voor het voorspellen van de tip opslaglocatie of bereik)
   
        nyctaxi_one_percent_add_col = '''
            ALTER TABLE nyctaxi_one_percent ADD tipped bit, tip_class int
        '''
   
        cursor.execute(nyctaxi_one_percent_add_col)
        cursor.commit()
   
        nyctaxi_one_percent_update_col = '''
            UPDATE nyctaxi_one_percent
            SET
               tipped = CASE WHEN (tip_amount > 0) THEN 1 ELSE 0 END,
               tip_class = CASE WHEN (tip_amount = 0) THEN 0
                                WHEN (tip_amount > 0 AND tip_amount <= 5) THEN 1
                                WHEN (tip_amount > 5 AND tip_amount <= 10) THEN 2
                                WHEN (tip_amount > 10 AND tip_amount <= 20) THEN 3
                                ELSE 4
                            END
        '''
   
        cursor.execute(nyctaxi_one_percent_update_col)
        cursor.commit()

#### <a name="feature-engineering-count-features-for-categorical-columns"></a>Functie-Engineering: Aantal functies voor Categorische kolommen
In dit voorbeeld transformeert een categorische veld in een numeriek veld door elke categorie vervangen door het aantal van de instanties in de gegevens.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD cmt_count int, vts_count int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B AS
        (
            SELECT medallion, hack_license,
                SUM(CASE WHEN vendor_id = 'cmt' THEN 1 ELSE 0 END) AS cmt_count,
                SUM(CASE WHEN vendor_id = 'vts' THEN 1 ELSE 0 END) AS vts_count
            FROM nyctaxi_one_percent
            GROUP BY medallion, hack_license
        )

        UPDATE nyctaxi_one_percent
        SET nyctaxi_one_percent.cmt_count = B.cmt_count,
            nyctaxi_one_percent.vts_count = B.vts_count
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion AND A.hack_license = B.hack_license
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-bin-features-for-numerical-columns"></a>Functie-Engineering: Bin functies voor numerieke kolommen
In dit voorbeeld transformeert een continue numeriek veld in de vooraf ingestelde categorie bereiken, numeriek veld dat wil zeggen, transformatie in een categorische veld.

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent ADD trip_time_bin int
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        WITH B(medallion,hack_license,pickup_datetime,trip_time_in_secs, BinNumber ) AS
        (
            SELECT medallion,hack_license,pickup_datetime,trip_time_in_secs,
            NTILE(5) OVER (ORDER BY trip_time_in_secs) AS BinNumber from nyctaxi_one_percent
        )

        UPDATE nyctaxi_one_percent
        SET trip_time_bin = B.BinNumber
        FROM nyctaxi_one_percent A INNER JOIN B
        ON A.medallion = B.medallion
        AND A.hack_license = B.hack_license
        AND A.pickup_datetime = B.pickup_datetime
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="feature-engineering-extract-location-features-from-decimal-latitudelongitude"></a>Functie-Engineering: Functies van de locatie van decimale breedtegraad/lengtegraad uitpakken
In dit voorbeeld uitsplitsing van de decimale weergave van een veld breedtegraad en/of lengtegraad in meerdere regio velden van verschillende mate van granulatie, zoals land, stad, stad, blok, enzovoort. Houd er rekening mee dat de nieuwe geo-velden niet zijn toegewezen aan de werkelijke locaties. Zie voor informatie over toewijzing geocode locaties, [Bing Maps REST Services](https://msdn.microsoft.com/library/ff701710.aspx).

    nyctaxi_one_percent_insert_col = '''
        ALTER TABLE nyctaxi_one_percent
        ADD l1 varchar(6), l2 varchar(3), l3 varchar(3), l4 varchar(3),
            l5 varchar(3), l6 varchar(3), l7 varchar(3)
    '''

    cursor.execute(nyctaxi_one_percent_insert_col)
    cursor.commit()

    nyctaxi_one_percent_update_col = '''
        UPDATE nyctaxi_one_percent
        SET l1=round(pickup_longitude,0)
            , l2 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 1 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),1,1) ELSE '0' END     
            , l3 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 2 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),2,1) ELSE '0' END     
            , l4 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 3 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),3,1) ELSE '0' END     
            , l5 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 4 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),4,1) ELSE '0' END     
            , l6 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 5 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),5,1) ELSE '0' END     
            , l7 = CASE WHEN LEN (PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1)) >= 6 THEN SUBSTRING(PARSENAME(ROUND(ABS(pickup_longitude) - FLOOR(ABS(pickup_longitude)),6),1),6,1) ELSE '0' END
    '''

    cursor.execute(nyctaxi_one_percent_update_col)
    cursor.commit()

#### <a name="verify-the-final-form-of-the-featurized-table"></a>Controleer of de uiteindelijke vorm van de tabel featurized
    query = '''SELECT TOP 100 * FROM nyctaxi_one_percent'''
    pd.read_sql(query,conn)

Er zijn nu gereed om door te gaan model bouwen en implementeren van modellen in [Azure Machine Learning](https://studio.azureml.net). De gegevens is gereed voor een van de eerder vastgestelde namelijk voorspelling-problemen:

1. Binaire classificatie: om te voorspellen of een tip betaald is voor een reis.
2. Multiklassen classificatie: om te voorspellen van het bereik van een tip betaald, volgens de eerder gedefinieerde klassen.
3. Taak regressie: om te voorspellen van de hoeveelheid tip voor een reis betaald.  

## <a name="mlmodel"></a>Gebouw modellen in Azure Machine Learning
Als u wilt de modellering oefening begint, moet u zich aanmelden bij uw Azure Machine Learning-werkruimte. Als u nog geen een machine learning-werkruimte hebt gemaakt, raadpleegt u [maken van een Azure Machine Learning-werkruimte](../studio/create-workspace.md).

1. Aan de slag met Azure Machine Learning Zie [wat is Azure Machine Learning Studio?](../studio/what-is-ml-studio.md)
2. Meld u aan bij [Azure Machine Learning Studio](https://studio.azureml.net).
3. De startpagina van Studio biedt een schat aan informatie, video's, zelfstudies, koppelingen naar de referentie-Modules en andere bronnen. Raadpleeg voor meer informatie over Azure Machine Learning de [Azure Machine Learning-documentatiecentrum](https://azure.microsoft.com/documentation/services/machine-learning/).

Een typische trainingsexperiment bestaat uit het volgende:

1. Maak een **+ nieuw** experimenteren.
2. De gegevens naar Azure Machine Learning ophalen.
3. Vooraf verwerken, transformeren en het bewerken van de gegevens zo nodig.
4. Genereren onderdelen naar behoefte.
5. De gegevens splitsen in training/testen/validatie gegevenssets (of afzonderlijke gegevenssets hebben voor elk).
6. Selecteer een of meer machine learning-algoritmen, afhankelijk van het probleem learning om op te lossen. Bijvoorbeeld binaire classificatie, multiklassen classificatie, regressie.
7. Een of meer modellen met behulp van de gegevensset training trainen.
8. De validatie van gegevensset met behulp van de trained modellen beoordelen.
9. De modellen voor het berekenen van de relevante metrische gegevens voor het probleem learning evalueren.
10. Fijnmazig afstemmen van de modellen en selecteert u het beste model om te implementeren.

In deze oefening hebben we al verkend en engineering van de gegevens in SQL Server en grootte van de steekproef gekozen om op te nemen in Azure Machine Learning. We besloten om een of meer van de voorspellende modellen samen te stellen:

1. Ophalen van de gegevens voor het gebruik van Azure Machine Learning de [importgegevens] [ import-data] module beschikbaar in de **gegevensinvoer en uitvoer** sectie. Zie voor meer informatie de [importgegevens] [ import-data] verwijzingspagina module.
   
    ![Gegevens van Azure Machine Learning importeren][17]
2. Selecteer **Azure SQL Database** als de **gegevensbron** in de **eigenschappen** Configuratiescherm.
3. Voer de naam van de DNS-database in de **databaseservernaam** veld. Indeling:`tcp:<your_virtual_machine_DNS_name>,1433`
4. Voer de **databasenaam** in het bijbehorende veld.
5. Voer de **SQL-gebruikersnaam** in de ** Server aqccount gebruikersnaam en het wachtwoord in de **Server het wachtwoord voor gebruikersaccount**.
7. In de **databasequery** bewerken van tekst, plakt u de query waarmee de benodigde databasevelden (inclusief eventuele berekende velden, zoals de labels) worden opgehaald en omlaag de gegevens naar de gewenste samplegrootte voorbeelden.

Er is een voorbeeld van een binaire indeling experiment lezen van gegevens rechtstreeks vanuit de SQL Server-database in de afbeelding hieronder. Vergelijkbare experimenten kunnen worden samengesteld voor multiklassen classificatie en regressie problemen.

![Azure Machine Learning Train][10]

> [!IMPORTANT]
> In de gegevens modellering extractie en voorbeelden van de query wordt opgegeven in de vorige secties **alle labels voor de drie modellering oefeningen zijn opgenomen in de query**. Een belangrijke (vereist) stap in elk van de oefeningen modellering is het **uitsluiten** de onnodige labels voor de andere twee problemen en andere **doel lekken**. Bijvoorbeeld, wanneer u de binaire classificatie, gebruik voor het label **punt** en wilt uitsluiten van de velden **tip\_klasse**, **tip\_bedrag**, en **totale\_bedrag**. De laatste zijn doel lekken omdat ze de tip impliceren betaald.
> 
> Als u wilt uitsluiten van overbodige kolommen en/of lekken zijn gericht, mag u de [Select Columns in Dataset] [ select-columns] module of de [metagegevens bewerken][edit-metadata]. Zie voor meer informatie [Select Columns in Dataset] [ select-columns] en [metagegevens bewerken] [ edit-metadata] verwijzen naar pagina's.
> 
> 

## <a name="mldeploy"></a>Implementeren van modellen in Azure Machine Learning
Wanneer uw model klaar is, kunt u het eenvoudig implementeren als een webservice rechtstreeks vanuit het experiment. Zie voor meer informatie over het implementeren van Azure Machine Learning-webservices [Azure Machine Learning-webservice implementeren](../studio/publish-a-machine-learning-web-service.md).

Voor het implementeren van een nieuwe webservice, moet u:

1. Een score experiment maken.
2. De webservice implementeren.

Maken van een experiment score van een **voltooid** training experiment, klikt u op **maken score berekenen EXPERIMENTEREN** in de onderste actiebalk.

![Score berekenen voor Azure][18]

Azure Machine Learning probeert te maken van een score experiment op basis van de onderdelen van het trainingsexperiment. In het bijzonder wordt:

1. Sla het getrainde model en het model trainingsmodules verwijderen.
2. Een logische identificeren **poort invoer** vertegenwoordigt het verwachte invoergegevens schema.
3. Een logische identificeren **uitvoerpoort** vertegenwoordigt de uitvoerschema verwachte web service.

Wanneer het experiment scoreprofiel is gemaakt, controleren en indien nodig aanpassen. Een typische aanpassing is invoergegevensset en/of queryparameters vervangen door een exclusief label velden, zoals deze is alleen beschikbaar wanneer de service wordt aangeroepen. Het is ook raadzaam de grootte van de invoergegevensset en/of de query beperken tot een aantal records net voldoende is om aan te geven van de invoer schema. Voor de uitvoerpoort wordt meestal alleen opnemen en uitsluiten van alle invoervelden de **Scored Labels** en **berekend kansen** in het uitvoer met de [Select Columns in Dataset] [ select-columns] module.

Er is een voorbeeld van een experiment score berekenen in de afbeelding hieronder. Wanneer u klaar bent om te implementeren, klikt u op de **WEBSERVICE PUBLICEERT** knop in de onderste actiebalk.

![Azure Machine Learning publiceren][11]

U hebt een Azure data wetenschap-omgeving hebt gewerkt met een grote openbare gegevensset helemaal van aanschaf van gegevens naar het model trainen en implementeren van een Azure Machine Learning-webservice gemaakt voor samenvatting in deze zelfstudie scenario.

### <a name="license-information"></a>Licentie-informatie
In dit voorbeeld scenario en de bijbehorende scripts en IPython notebook(s) worden gedeeld door Microsoft onder de MIT-licentie. Controleer of u het bestand LICENSE.txt in in de map van de voorbeeldcode op GitHub voor meer informatie.

### <a name="references"></a>Verwijzingen
• [Andrés Monroy NYC Taxi reizen downloadpagina](http://www.andresmh.com/nyctaxitrips/)  
• [FOILing NYC Taxi reis gegevens door Chris Whong](http://chriswhong.com/open-data/foil_nyc_taxi/)   
• [NYC Taxi en Limousine Commissie onderzoek en statistieken](https://www1.nyc.gov/html/tlc/html/about/statistics.shtml)

[1]: ./media/sql-walkthrough/sql-walkthrough_26_1.png
[2]: ./media/sql-walkthrough/sql-walkthrough_28_1.png
[3]: ./media/sql-walkthrough/sql-walkthrough_35_1.png
[4]: ./media/sql-walkthrough/sql-walkthrough_36_1.png
[5]: ./media/sql-walkthrough/sql-walkthrough_39_1.png
[6]: ./media/sql-walkthrough/sql-walkthrough_42_1.png
[7]: ./media/sql-walkthrough/sql-walkthrough_44_1.png
[8]: ./media/sql-walkthrough/sql-walkthrough_46_1.png
[9]: ./media/sql-walkthrough/sql-walkthrough_71_1.png
[10]: ./media/sql-walkthrough/azuremltrain.png
[11]: ./media/sql-walkthrough/azuremlpublish.png
[12]: ./media/sql-walkthrough/ssmsconnect.png
[13]: ./media/sql-walkthrough/executescript.png
[14]: ./media/sql-walkthrough/sqlserverproperties.png
[15]: ./media/sql-walkthrough/sqldefaultdirs.png
[16]: ./media/sql-walkthrough/bulkimport.png
[17]: ./media/sql-walkthrough/amlreader.png
[18]: ./media/sql-walkthrough/amlscoring.png


<!-- Module References -->
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[select-columns]: https://msdn.microsoft.com/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
