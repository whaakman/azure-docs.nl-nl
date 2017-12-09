---
title: Overzicht van Gegevenswetenschap met Spark op Azure HDInsight | Microsoft Docs
description: De toolkit Spark MLlib biedt aanzienlijke machine learning modelleren mogelijkheden voor de gedistribueerde HDInsight-omgeving.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: a4e1de99-a554-4240-9647-2c6d669593c8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: deguhath;bradsev;gokuma
ms.openlocfilehash: e1c4a507214b9686154fc8311121b56f42f5cd40
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Overzicht van gegevenswetenschap met Spark op Azure HDInsight
[!INCLUDE [machine-learning-spark-modeling](../../../includes/machine-learning-spark-modeling.md)]

Deze reeks onderwerpen wordt beschreven hoe HDInsight Spark gebruiken om algemene gegevens wetenschappelijke taken zoals gegevensopname, functie-engineering, modellering en evaluatie van het model te voltooien. De gegevens die worden gebruikt, is een voorbeeld van de 2013 NYC taxi reis en tarief gegevensset. De modellen gebouwd zijn logistic en lineaire regressie, willekeurige forests en kleurovergang gestimuleerd structuren. De onderwerpen bevatten ook het opslaan van deze modellen in Azure blob storage (WASB) en hoe beoordelen en evalueren hun voorspellende prestaties. Meer geavanceerde onderwerpen wordt uitgelegd hoe modellen kunnen worden getraind met sweeping kruisvalidatie en hyper-parameter. In dit onderwerp verwijst ook naar de onderwerpen over het instellen van het Spark-cluster moet u de stappen in de scenario's. 

## <a name="spark-and-mllib"></a>Spark en MLlib
[Spark](http://spark.apache.org/) is een parallelle verwerking van open-source framework die ondersteuning biedt voor in-memory verwerking naar het verbeteren de prestaties van analytische big data-toepassingen. De Spark-verwerkingsengine is gebouwd voor snelheid, gebruiksgemak en geavanceerde analyses. De Spark in-memory gedistribueerde rekencapaciteiten kunnen u een goede keuze voor de herhalende algoritmen in machine learning- en grafiekberekeningen. [MLlib](http://spark.apache.org/mllib/) is modelleren van Spark schaalbare machine learning-bibliotheek die ook de algoritmische mogelijkheden aan deze gedistribueerde omgeving. 

## <a name="hdinsight-spark"></a>Spark in HDInsight
[HDInsight Spark](../../hdinsight/spark/apache-spark-overview.md) is de Azure gehoste oplossing van open-source Spark. Biedt ook ondersteuning voor **Jupyter PySpark-notebooks** op het Spark-cluster dat Spark SQL interactieve query's voor het transformeren, te filteren en visualiseren van gegevens die zijn opgeslagen in Azure BLOB's (WASB) kan worden uitgevoerd. PySpark is de Python-API voor Spark. De codefragmenten die voorzien van de oplossingen en weergeven van de relevante waarnemingspunten om de gegevens die hier worden uitgevoerd in Jupyter-notebooks geïnstalleerd op de Spark-clusters te visualiseren. De stappen modelleren in de volgende onderwerpen bevatten code die laat hoe trainen zien, evalueren, opslaan en gebruiken van elk type model. 

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>Instellen: De Spark-clusters en Jupyter-notebooks
Instellingsstappen en code vindt u in dit scenario voor het gebruik van een HDInsight Spark 1.6. Maar Jupyter-notebooks zijn opgegeven voor zowel HDInsight Spark 1.6 en 2.0 Spark-clusters. Een beschrijving van de laptops en koppelingen naar deze vindt u in de [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) voor de GitHub-opslagplaats met deze. Bovendien moet de code hier en in de gekoppelde laptops is algemeen en moet werken op een Spark-cluster. Als u HDInsight Spark niet gebruikt, is het mogelijk dat de cluster-installatie en beheer stappen enigszins afwijken van wat hier moet worden weergegeven. Voor het gemak zijn hier de koppelingen naar de Jupyter-notebooks voor Spark 1.6 (om te worden uitgevoerd in de pySpark-kernel van de server Jupyter-Notebook) en Spark 2.0 (om te worden uitgevoerd in de kernel pySpark3 van de server Jupyter-Notebook):

### <a name="spark-16-notebooks"></a>Spark 1.6 laptops
Deze laptops zijn in de pySpark-kernel van Jupyter-notebook server moeten worden uitgevoerd.

- [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): bevat informatie over het uitvoeren van gegevensverkenning modelleren en batchscoreberekening met diverse verschillende algoritmen.
- [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): bevat onderwerpen in de notebook #1 en de ontwikkeling van het model met hyperparameter afstemmen en kruisvalidatie.
- [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): laat zien hoe u een opgeslagen model met behulp van Python op HDInsight-clusters operationeel.

### <a name="spark-20-notebooks"></a>Spark 2.0-laptops
Deze laptops zijn in de kernel pySpark3 van Jupyter-notebook server worden uitgevoerd.

- [Spark2.0-pySpark3-machine-Learning-Data-Science-Spark-Advanced-Data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): dit bestand bevat informatie over het uitvoeren van gegevensverkenning, modelleren, en met behulp van de NYC Taxi reis scores in Spark 2.0-clusters en tarief gegevensset-beschreven [hier](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Deze laptop is mogelijk een goed uitgangspunt voor het snel verkennen van de code die we voor Spark 2.0 hebt opgegeven. Voor een meer gedetailleerde notebook de gegevens van de NYC Taxi analyseert, Zie de volgende notebook in deze lijst. Zie de opmerkingen na deze lijst die deze laptops vergelijken. 
- [Spark2.0 pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): dit bestand ziet u hoe u gegevens worsteling (Spark SQL en dataframe bewerkingen), exploratie, model en score berekenen met behulp van de NYC Taxi reis en tarief set gegevens die worden beschreven [hier ](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0 pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): dit bestand wordt beschreven hoe gegevens worsteling (Spark SQL en dataframe bewerkingen), exploratie, model en score berekenen met behulp van de bekende luchtvaartmaatschappij tijdige afwijking uitvoeren de gegevensset van 2011 en 2012. Wij de luchtvaartmaatschappij gegevensset met de luchthaven weergegevens (bijvoorbeeld windsnelheid, temperatuur, hoogte enz.) geïntegreerd vóór modelleren, zodat deze weer-functies kunnen worden opgenomen in het model.

<!-- -->

> [!NOTE]
> De gegevensset luchtvaartmaatschappij is toegevoegd aan de laptops Spark 2.0 ter illustratie van het gebruik van bestandsclassificatie-algoritmen beter. Zie de volgende koppelingen voor meer informatie over luchtvaartmaatschappij tijdige vertrek gegevensset en gegevensset weer:

>- Luchtvaartmaatschappij tijdige vertrek gegevens: [http://www.transtats.bts.gov/ONTIME/](http://www.transtats.bts.gov/ONTIME/)

>- Luchthaven weergegevens: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/) 
> 
> 

<!-- -->

<!-- -->

> [!NOTE]
De notitieblokken Spark 2.0 op de NYC taxi en luchtvaartmaatschappij vlucht vertraging-gegevenssets duurt 10 minuten of langer om uit te voeren (afhankelijk van de grootte van uw HDI-cluster). De eerste laptop in de bovenstaande lijst geeft veel aspecten van de gegevensverkenning, visualisatie en ML-model opleiding in een laptop die het kost minder tijd om uit te voeren met een lagere actieve NYC gegevensset, waarin de bestanden taxi en tarief vooraf lid zijn: [ Spark2.0-pySpark3-machine-Learning-Data-Science-Spark-Advanced-Data-Exploration-Modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb) deze notebook neemt een veel kortere tijd om te voltooien (2-3 minuten) en kan worden een goed startpunt voor de code die we hebben opgegeven snel verkennen voor Spark 2.0. 

<!-- -->

Zie voor instructies over de uitoefening van een model voor Spark 2.0 en de model-verbruik voor score berekenen, de [Spark 1.6 document over het verbruik](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) voor een voorbeeld waarin de vereiste stappen beschreven. Om dit te gebruiken op Spark 2.0, het Python-code-bestand met vervangen [dit bestand](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).

### <a name="prerequisites"></a>Vereisten
De volgende procedures hebben betrekking op Spark 1.6. Gebruik de laptops beschreven en gekoppeld aan de eerder voor de versie 2.0 van Spark. 

1. u moet een Azure-abonnement hebben. Als u nog geen een, Zie [gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. u moet een 1.6 Spark-cluster voor dit scenario. Zie de instructies in om een [aan de slag: maken van Apache Spark in Azure HDInsight](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). Het clustertype en de versie is opgegeven in de **clustertype Selecteer** menu. 

![Cluster configureren](./media/spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Zie voor een onderwerp waarin wordt uitgelegd hoe u Scala in plaats van Python gebruikt om taken voor een end-to-end gegevens wetenschap proces te voltooien, de [Gegevenswetenschap met Spark op Azure met behulp van Scala](scala-walkthrough.md).
> 
> 

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
> 
> 

## <a name="the-nyc-2013-taxi-data"></a>De gegevens van de NYC 2013 Taxi
De gegevens van de NYC Taxi reis is ongeveer 20 GB gecomprimeerde door komma's gescheiden waarden (CSV)-bestanden (~ 48 GB ongecomprimeerde), die bestaat uit meer dan 173 miljoen afzonderlijke reizen en de tarieven voor elke reis betaald. Elke record reis omvat de ophalen en afgiftepunt en tijd, geanonimiseerde hack (van stuurprogramma) licentienummer, en nummer straten (taxi van unieke id). De gegevens bevat informatie over alle reizen in het jaar 2013 en is beschikbaar in de volgende twee gegevenssets voor elke maand:

1. De CSV-bestanden 'trip_data' reis details, zoals het aantal passagiers bevatten, kunnen worden opgepikt en dropoff verwijst, krachtvoertuigen duur en reis lengte. Hier volgen enkele voorbeeldrecords:
   
        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. De 'trip_fare' CSV-bestanden bevatten details van het tarief dat betaald voor elke reis, zoals betalingstype, tarief bedrag, extra kosten en belastingen, tips en tolgelden, en de totale hoeveelheid betaald. Hier volgen enkele voorbeeldrecords:
   
        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

We hebben een steekproef 0,1% van deze bestanden gemaakt en die lid zijn van de reis\_gegevens en reis\_ritbedrag CSV-bestanden in een enkel gegevensset wilt gebruiken als de invoergegevensset voor dit scenario. De unieke sleutel toevoegen reis\_gegevens en reis\_tarief dat bestaat uit de velden: straten, hack\_en ophalen van certificaat\_datetime. Elke record van de gegevensset bevat de volgende kenmerken die een reis NYC Taxi vertegenwoordigt:

| Veld | Korte beschrijving |
| --- | --- |
| straten |Geanonimiseerde taxi straten (taxi unieke id) |
| hack_license |Geanonimiseerde Hackney regeleinde licentienummer |
| vendor_id |Taxi leveranciers-id |
| rate_code |Snelheid van de NYC taxi van tarief |
| store_and_fwd_flag |Store en voorwaarts markering |
| pickup_datetime |Datum en tijd kunnen worden opgepikt |
| dropoff_datetime |Dropoff datum en tijd |
| pickup_hour |Uur ophalen |
| pickup_week |Week van het jaar ophalen |
| werkdag |Werkdag (bereik 1-7) |
| passenger_count |Aantal passagiers in een reis taxi |
| trip_time_in_secs |Reis tijd in seconden |
| trip_distance |Reis afstand afgelegd in mijl |
| pickup_longitude |Lengtegraad ophalen |
| pickup_latitude |Breedtegraad ophalen |
| dropoff_longitude |Lengtegraad Dropoff |
| dropoff_latitude |Dropoff breedtegraad |
| direct_distance |Afstand tussen pick up directe en dropoff locaties |
| payment_type |Betalingstype (CA's, creditcard enz.) |
| fare_amount |Tarief bedrag in |
| Extra kosten |Extra kosten |
| mta_tax |MTA belasting |
| tip_amount |Tip bedrag |
| tolls_amount |Tolgelden bedrag |
| total_amount |Totale hoeveelheid |
| punt |Gekantelde (0/1 voor Nee of Ja) |
| tip_class |Tip klasse (0: $0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > $20) |

## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Voer de code van een Jupyter-notebook in Spark-cluster
U kunt de Jupyter-Notebook vanuit de Azure-portal op te starten. Zoek uw Spark-cluster op uw dashboard en klik op de beheerpagina invoeren voor uw cluster. De notebook die zijn gekoppeld aan het Spark-cluster, klikt u op **Clusterdashboards** -> **Jupyter-Notebook** .

![Clusterdashboards](./media/spark-overview/spark-jupyter-on-portal.png)

U kunt ook bladeren naar ***https://CLUSTERNAME.azurehdinsight.NET/jupyter*** voor toegang tot de Jupyter-Notebooks. De CLUSTERNAAM deel uitmaakt van deze URL vervangen door de naam van uw eigen cluster. U moet het wachtwoord voor uw beheerdersaccount voor toegang tot de laptops.

![Jupyter-Notebooks bladeren](./media/spark-overview/spark-jupyter-notebook.png)

Selecteer PySpark om te zien van een map met een paar voorbeelden van vooraf verpakte laptops die gebruikmaken van de PySpark-API. De laptops die de codevoorbeelden voor dit pakket van Spark onderwerp bevatten zijn beschikbaar op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)

U kunt de laptops rechtstreeks vanuit uploaden [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) naar de server Jupyter-notebook in Spark-cluster. Klik op de startpagina van uw Jupyter op de **uploaden** knop op de juiste deel van het scherm. Hiermee opent u een Windows Verkenner. Hier kunt u de URL GitHub (onbewerkte inhoud) van de Notebook en klik op Plakken **Open**. 

U ziet de bestandsnaam in de lijst met Jupyter-bestand met een **uploaden** knop opnieuw. Klik hierop **uploaden** knop. Nu hebt u de notebook geïmporteerd. Herhaal deze stappen voor het uploaden van de andere notitieblokken van dit scenario.

> [!TIP]
> U kunt met de rechtermuisknop op de koppelingen in de browser en selecteer **koppeling kopiëren** om de URL github onbewerkte inhoud te krijgen. U kunt deze URL in het uploaden van Jupyter explorer bestandsdialoogvenster plakken.
> 
> 

U kunt nu:

* De code door te klikken op de notebook bekijken.
* elke cel uitvoeren door te drukken **SHIFT + ENTER**.
* De volledige notebook uitvoeren door te klikken op **cel** -> **uitvoeren**.
* Gebruik de automatische visualisatie van query's.

> [!TIP]
> De PySpark-kernel visualiseren automatisch de uitvoer van SQL (HiveQL)-query's. Krijgt u de optie kiezen uit verschillende soorten visualisaties (tabel-, cirkel, regel, gebied of balk) met behulp van de **Type** menuknoppen in de notebook:
> 
> 

![Logistic regression ROC-curve voor algemene methode](./media/spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Volgende stappen
Nu dat u een HDInsight Spark-cluster is ingesteld en de Jupyter-notebooks met licentie zijn geüpload, bent u klaar om te werken via de onderwerpen die met de drie PySpark laptops overeenkomen. Ze geven weer hoe Verken uw gegevens en het maken en gebruiken van modellen. De geavanceerde gegevens te verkennen en modellering notebook laat zien hoe naar kruisvalidatie, hyper-parameter verstrekkende, opnemen en model van de evaluatie. 

**Gegevensverkenning en modellering met Spark:** verkennen van de gegevensset en maken, beoordelen en evalueren van de machine learning-modellen doorloopt de [binaire classificatie en regressie modellen voor gegevens met de MLlib Spark maken Toolkit](spark-data-exploration-modeling.md) onderwerp.

**Model verbruik:** Zie voor meer informatie over de classificatie en regressie modellen gemaakt in dit onderwerp te beoordelen, [Score en evalueren van Spark is gebouwd machine learning-modellen](spark-model-consumption.md).

**Kruisvalidatie en hyperparameter sweeping**: Zie [geavanceerde gegevensverkenning en modellering met Spark](spark-advanced-data-exploration-modeling.md) over de manier waarop modellen kunnen getraind met behulp van kruisvalidatie en hyper-parameter sweeping

