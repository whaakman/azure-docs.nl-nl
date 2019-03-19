---
title: Gegevenswetenschap met Spark op Azure HDInsight - Team Data Science Process
description: De toolkit Spark MLlib zorgt voor veel machine learning modelleringsmogelijkheden aan de gedistribueerde HDInsight-omgeving.
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
ms.openlocfilehash: 932587afcffcb3b1a259a02a98c648e938e99931
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57845335"
---
# <a name="overview-of-data-science-using-spark-on-azure-hdinsight"></a>Overzicht van gegevenswetenschap met Spark op Azure HDInsight

Deze reeks onderwerpen ziet u hoe u HDInsight Spark algemene datatechnologietaken zoals gegevensopname, feature-engineering, modellen en model evaluatie voltooid. De gegevens die worden gebruikt, is een voorbeeld van de 2013 NYC taxi reis- en fare gegevensset. De gebouwde modellen zijn logistieke en lineaire regressie, willekeurige forests en kleurovergang boosted structuren. De onderwerpen laten ook zien hoe deze modellen worden opgeslagen in Azure blob storage (WASB) en hoe u kunt beoordelen en evalueren van de voorspellende prestaties. Meer geavanceerde onderwerpen besproken hoe modellen kunnen worden getraind met behulp van kruisvalidatie en hyper-parameter sweeping. Dit onderwerp verwijst ook naar de onderwerpen waarin wordt beschreven hoe u voor het instellen van het Spark-cluster die u nodig hebt voor de stappen in de scenario's.

## <a name="spark-and-mllib"></a>Spark en MLlib
[Spark](https://spark.apache.org/) is een framework open-source parallelle verwerking dat ondersteuning biedt voor in-memory verwerking om de prestaties van toepassingen voor gegevensanalyse van big data te verbeteren. De Spark-verwerkingsengine is gebouwd voor snelheid, gebruiksgemak, en geavanceerde analyses. Gedistribueerde berekening in-memory-mogelijkheden van Spark kunnen u een goede keuze voor de zich herhalende algoritmen in machine learning- en grafiekberekeningen gebruikt. [MLlib](https://spark.apache.org/mllib/) is Spark van schaalbare machine learning-bibliotheek die zorgt voor de algoritmische-modelleringsmogelijkheden aan deze gedistribueerde omgeving.

## <a name="hdinsight-spark"></a>HDInsight Spark
[HDInsight Spark](../../hdinsight/spark/apache-spark-overview.md) is de Azure gehoste aanbieding van open-source Spark. Het biedt ook ondersteuning voor **Jupyter PySpark notebooks** op het Spark-cluster dat interactieve Spark SQL-query voor het transformeren, filteren en visualiseren van gegevens die zijn opgeslagen in Azure Blobs (WASB) kan worden uitgevoerd. PySpark is de Python-API voor Spark. De codefragmenten die de oplossingen bieden en weergeven van de relevante grafieken om de gegevens die hier worden uitgevoerd in Jupyter-notebooks geïnstalleerd op de Spark-clusters te visualiseren. De stappen modellen in de volgende onderwerpen bevatten code die laat hoe u trainen zien, evalueren, opslaan en gebruiken van elk type model.

## <a name="setup-spark-clusters-and-jupyter-notebooks"></a>Instellen: Spark-clusters en Jupyter notebooks
Installatiestappen uit en code vindt u in dit scenario voor het gebruik van een HDInsight Spark 1.6. Maar Jupyter-notebooks voor HDInsight Spark 1.6- en Spark 2.0-clusters worden geleverd. Een beschrijving van de laptops en koppelingen naar deze vindt u in de [Readme.md](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Readme.md) voor de GitHub-opslagplaats die ze bevatten. Bovendien is de code hier en in de gekoppelde notebooks is algemeen en werkt op een Spark-cluster. Als u niet met behulp van HDInsight Spark, configureren van het cluster en beheertaken uit kunnen enigszins afwijken van wat wordt hier weergegeven. Hier volgen de koppelingen naar de Jupyter-notebooks voor Spark 1.6 (om te worden uitgevoerd in de pySpark-kernel van de Jupyter-Notebook-server) en Spark 2.0 (om te worden uitgevoerd in de kernel pySpark3 van de Jupyter-Notebook-server) voor het gemak:

### <a name="spark-16-notebooks"></a>Spark 1.6-laptops
Deze laptops zijn in de pySpark-kernel van Jupyter notebook-server moeten worden uitgevoerd.

- [pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-data-exploration-modeling.ipynb): Bevat informatie over het uitvoeren van de gegevens verkennen, modelleren en scoren met diverse verschillende algoritmes.
- [pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Bevat onderwerpen in de notebook #1 en de ontwikkeling van het model met behulp van hyperparameter afstemmen en kruisvalidatie.
- [pySpark-machine-learning-data-science-spark-model-consumption.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb): Laat zien hoe operationeel maken van een opgeslagen model met behulp van Python op HDInsight-clusters.

### <a name="spark-20-notebooks"></a>Spark 2.0-laptops
Deze laptops zijn in de kernel pySpark3 van Jupyter notebook-server moeten worden uitgevoerd.

- [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb): Dit bestand bevat informatie over het uitvoeren van de gegevens verkennen, modellerings- en scoring in Spark 2.0-clusters met behulp van de NYC Taxi reis en fare set gegevens die worden beschreven [hier](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data). Dit notitieblok mogelijk een goed uitgangspunt voor het snel verkennen van de code die we voor Spark 2.0 hebt opgegeven. Voor een meer gedetailleerde notebook de gegevens over taxi's NYC analyseert, Zie de volgende notebook in deze lijst. Zie de opmerkingen na deze lijst die deze laptops vergelijken.
- [Spark2.0-pySpark3_NYC_Taxi_Tip_Regression.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_NYC_Taxi_Tip_Regression.ipynb): Dit bestand ziet u hoe u data wrangling (Spark SQL- en dataframe bewerkingen), gegevensonderzoek, modelleren en scoren met behulp van de NYC Taxi reis en fare set gegevens die worden beschreven uitvoeren [hier](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-spark-overview#the-nyc-2013-taxi-data).
- [Spark2.0-pySpark3_Airline_Departure_Delay_Classification.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0_pySpark3_Airline_Departure_Delay_Classification.ipynb): Dit bestand wordt uitgelegd hoe data wrangling (Spark SQL- en dataframe bewerkingen), gegevensonderzoek, modelleren en scoren met behulp van de bekende gegevensset op tijd vertrek luchtvaartmaatschappij in 2011 en 2012 uitvoeren. We hebben de luchtvaartmaatschappij gegevensset met de weergegevens luchthaven (bijvoorbeeld windsnelheid, temperatuur, hoogte enz.) geïntegreerd vóór het modelleren, zodat deze weer-functies kunnen worden opgenomen in het model.

<!-- -->

> [!NOTE]
> De gegevensset luchtvaartmaatschappij is toegevoegd aan de notebooks Spark 2.0 ter illustratie van het gebruik van bestandsclassificatie-algoritmen beter. Zie de volgende koppelingen voor meer informatie over luchtvaartmaatschappij op tijd vertrek gegevensset en de gegevensset weer:
> 
> - Luchtvaartmaatschappij op tijd vertrek gegevens: [https://www.transtats.bts.gov/ONTIME/](https://www.transtats.bts.gov/ONTIME/)
> 
> - Weergegevens luchthaven: [https://www.ncdc.noaa.gov/](https://www.ncdc.noaa.gov/)

<!-- -->

<!-- -->

> [!NOTE]
> De notebooks Spark 2.0 op de NYC taxi en luchtvaartmaatschappij flight vertraging-gegevenssets kunnen 10 minuten of langer om uit te voeren (afhankelijk van de grootte van het HDI-cluster) duren. In een notitieblok dat minder tijd om uit te voeren naar beneden steekproef NYC gegevens is ingesteld kost, waarin de bestanden over taxi's en fare zijn vooraf is toegevoegd aan ziet het eerste notitieblok in de bovenstaande lijst u veel aspecten van de gegevens verkennen, visualisatie en training voor ML-model: [Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark2.0/Spark2.0-pySpark3-machine-learning-data-science-spark-advanced-data-exploration-modeling.ipynb). Dit notitieblok neemt een veel kortere tijd om te voltooien (2-3 minuten) en kan worden een goed startpunt om snel de die we hebben opgegeven voor Spark 2.0 te verkennen.

<!-- -->

Zie voor hulp bij de uitoefening van een model voor Spark 2.0 en het verbruik model voor het scoren van, de [Spark 1.6-document op verbruik](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/pySpark/Spark1.6/pySpark-machine-learning-data-science-spark-model-consumption.ipynb) voor een voorbeeld geeft een overzicht van de stappen die nodig zijn. Om dit te gebruiken op Spark 2.0, vervangt u het Python-code-bestand met [dit bestand](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/Spark/Python/Spark2.0_ConsumeRFCV_NYCReg.py).

### <a name="prerequisites"></a>Vereisten

De volgende procedures hebben betrekking op Spark 1.6. Gebruik de notebooks beschreven en gekoppeld aan de eerder voor de versie van Spark 2.0.

1. U hebt een abonnement op Azure nodig. Als u nog geen een, Zie [gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

2. U moet een Spark 1.6-cluster voor dit scenario. Als u wilt maken, Zie de instructies in [aan de slag: Apache Spark op Azure HDInsight maakt](../../hdinsight/spark/apache-spark-jupyter-spark-sql.md). De cluster van het type en de versie is opgegeven in de **clustertype selecteren** menu.

![Cluster configureren](./media/spark-overview/spark-cluster-on-portal.png)

<!-- -->

> [!NOTE]
> Zie voor een onderwerp waarin wordt uitgelegd hoe u Scala in plaats van Python gebruikt om taken voor een end-to-end-data science process te voltooien, de [Gegevenswetenschap met Spark op Azure met behulp van Scala](scala-walkthrough.md).
>
>

<!-- -->

> [!INCLUDE [delete-cluster-warning](../../../includes/hdinsight-delete-cluster-warning.md)]
>
>

## <a name="the-nyc-2013-taxi-data"></a>De gegevens over taxi's van NYC 2013
De reisgegevens NYC over taxi's is ongeveer 20 GB aan gecomprimeerde door komma's gescheiden waarden (CSV)-bestanden (~ 48 GB niet-gecomprimeerd), die bestaat uit meer dan 173 miljoen afzonderlijke trips en de tarieven voor elke reis betaald. Elke record van de fietstocht bevat de voorraadtracering en dropoff locatie en tijd, geanonimiseerde hack (van het stuurprogramma) licentienummer, en straten (unieke id van taxi) getal. De gegevens bevat informatie over alle gegevens in het jaar 2013 en is beschikbaar in de volgende twee gegevenssets voor elke maand:

1. De 'trip_data' CSV-bestanden bevatten reis details, zoals het aantal personen, ophalen en duur en de lengte van de fietstocht dropoff verwijst, veroorzaken. Hier volgen enkele voorbeeldrecords:

        medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count,trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
2. De 'trip_fare' CSV-bestanden bevatten details van het tarief voor elke reis, zoals betalingstype, fare bedrag, toeslag en belastingen, tips en tolwegen, betaald en de totale hoeveelheid betaald. Hier volgen enkele voorbeeldrecords:

        medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
        89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
        0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
        DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

We hebben gemaakt van een steekproef 0,1% van deze bestanden en lid is van de fietstocht\_gegevens en reis\_ritbedrag CSV-bestanden in een enkele gegevensset moet worden gebruikt als invoergegevensset voor dit scenario. De unieke sleutel voor deelname aan reis\_gegevens en reis\_fare bestaat uit de velden: straten hack\_en ophalen van certificaat\_datum/tijd. Elke record van de gegevensset bevat de volgende kenmerken die een reis NYC Taxi vertegenwoordigt:

| Veld | Korte beschrijving |
| --- | --- |
| straten |Anonieme taxi straten (unieke taxi-id) |
| hack_license |Anonieme Hackney vervoer licentienummer |
| vendor_id |Taxi leveranciers-id |
| rate_code |NYC taxi aantal fare |
| store_and_fwd_flag |Store en de vlag voor het doorsturen |
| pickup_datetime |Datum en tijd ophalen |
| dropoff_datetime |Dropoff datum en tijd |
| pickup_hour |Uur ophalen |
| pickup_week |Week van het jaar ophalen |
| Weekdag |Weekdag (bereik 1-7) |
| passenger_count |Het aantal personen in een reis over taxi 's |
| trip_time_in_secs |Reis tijd in seconden |
| trip_distance |Reis afstand gereisd in mijl |
| pickup_longitude |Lengtegraad ophalen |
| pickup_latitude |Breedtegraad ophalen |
| dropoff_longitude |Dropoff lengtegraad |
| dropoff_latitude |Dropoff breedtegraad |
| direct_distance |Afstand tussen het kiezen van directe en dropoff locaties |
| payment_type |Betalingstype (geld, creditcard enz.) |
| fare_amount |Fare bedrag in |
| Toeslag |Toeslag |
| mta_tax |MTA belasting |
| tip_amount |Tip bedrag |
| tolls_amount |Tolwegen bedrag |
| total_amount |Totaalbedrag |
| punt |Gekantelde (0/1 voor Nee of Ja) |
| tip_class |Tip van de klasse (0: $0, 1: $0-5, 2: $6-10, 3: $11-20, 4: > $20) |

## <a name="execute-code-from-a-jupyter-notebook-on-the-spark-cluster"></a>Code uitvoeren van een Jupyter-notebook in Spark-cluster
U kunt de Jupyter-Notebook vanuit de Azure-portal starten. Zoek uw Spark-cluster op uw dashboard en klikt u erop om in te voeren management-pagina voor uw cluster. Als u wilt de laptop die is gekoppeld aan het Spark-cluster openen, klikt u op **Clusterdashboards** -> **Jupyter-Notebook** .

![Clusterdashboards](./media/spark-overview/spark-jupyter-on-portal.png)

U kunt ook bladeren naar ***https://CLUSTERNAME.azurehdinsight.net/jupyter*** voor toegang tot de Jupyter-Notebooks. De CLUSTERNAAM deel uitmaken van deze URL vervangen door de naam van uw eigen cluster. U moet het wachtwoord voor uw beheerdersaccount voor toegang tot de notebooks.

![Jupyter-Notebooks bladeren](./media/spark-overview/spark-jupyter-notebook.png)

Selecteer PySpark om te zien van een map met een paar voorbeelden van vooraf verpakte laptops die gebruikmaken van de PySpark-API. De notebooks waarin de codevoorbeelden voor dit pakket van Spark-onderwerp zijn beschikbaar op [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark)

U kunt de notebooks rechtstreeks vanuit uploaden [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/Spark/pySpark) met de Jupyter-notebook-server op uw Spark-cluster. Op de startpagina van uw Jupyter, klikt u op de **uploaden** knop op het juiste deel van het scherm. Hiermee opent u een bestand in bestandenverkenner. Hier kunt u de URL van GitHub (onbewerkte inhoud) van de Notebook en op Plakken **Open**.

Ziet u de bestandsnaam in de lijst met de Jupyter-bestand met een **uploaden** knop opnieuw. Klik hierop **uploaden** knop. Nu hebt u de notebook geïmporteerd. Herhaal deze stappen voor het uploaden van de andere notebooks van dit scenario.

> [!TIP]
> U kunt met de rechtermuisknop op de koppelingen in uw browser en selecteer **koppeling kopiëren** om op te halen van de GitHub onbewerkte inhouds-URL. U kunt deze URL plakken in de Jupyter uploaden bestand explorer in het dialoogvenster.
> 
> 

U kunt nu het volgende doen:

* De code bekijken door te klikken op de notebook.
* Elke cel uitvoeren door te drukken **SHIFT + ENTER**.
* Het hele notitieblok uitvoeren door te klikken op **cel** -> **uitvoeren**.
* Gebruik de automatische visualisatie van query's.

> [!TIP]
> De PySpark-kernel visualiseert automatisch de uitvoer (HiveQL) SQL-query's. Krijgt u de mogelijkheid om te kiezen uit verschillende soorten visualisaties (tabel, Pie, regel, gebied of balk) met behulp van de **Type** menuknoppen in de notebook:
>
>

![Logistieke regressie ROC-curve voor algemene methode](./media/spark-overview/pyspark-jupyter-autovisualization.png)

## <a name="whats-next"></a>Volgende stappen
Nu dat u zijn ingesteld met een HDInsight Spark-cluster en de Jupyter-notebooks hebt geüpload, bent u klaar om te werken via de onderwerpen die met de drie PySpark-notebooks overeenkomen. Ze laten zien hoe u om uw gegevens te verkennen en vervolgens op over het maken en gebruiken van modellen. De geavanceerde gegevens verkennen en modelleren notebook laat zien hoe het opnemen van kruisvalidatie, hyper-parameter verstrekkende, en model van de evaluatie.

**Gegevens verkennen en modelleren met Spark:** De gegevensset te verkennen en maken, te beoordelen en evalueren van de machine learning-modellen door het uitvoeren van de [binaire classificatie- en regressiemodellen modellen voor gegevens maken in de werkset Spark MLlib](spark-data-exploration-modeling.md) onderwerp.

**Model-verbruik:** Zie voor meer informatie over de classificatie- en regressiemodellen modellen die zijn gemaakt in dit onderwerp te beoordelen, [Score en evalueren met Spark gebouwde machine learning-modellen](spark-model-consumption.md).

**Kruisvalidatie en hyperparameter sweeping**: Zie [geavanceerde met Spark gegevens verkennen en modelleren](spark-advanced-data-exploration-modeling.md) op hoe de modellen kunnen worden getraind met behulp van kruisvalidatie en hyper-parameter sweeping

