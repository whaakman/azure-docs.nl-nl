---
title: Het Team gegevens wetenschap proces in actie - met een Azure HDInsight Hadoop-Cluster van een gegevensset 1 TB | Microsoft Docs
description: Met behulp van het Team gegevens wetenschap proces voor een end-to-end-scenario die gebruikmaakt van een HDInsight Hadoop-cluster te bouwen en implementeren van een model met behulp van een grote (1 TB) openbaar gegevensset
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 72d958c4-3205-49b9-ad82-47998d400d2b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 13dc1b516946aadc9c8a57a55768113bc925e63e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="the-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a>Het Team gegevens wetenschap proces in actie - met een Azure HDInsight Hadoop-Cluster van een gegevensset 1 TB

In dit overzicht wordt gedemonstreerd met behulp van het Team gegevens wetenschap proces in een end-to-end-scenario met een [Azure HDInsight Hadoop-cluster](https://azure.microsoft.com/services/hdinsight/) om op te slaan, verkennen, functie engineer van en naar beneden voorbeeldgegevens uit een van algemeen beschikbaar [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) gegevenssets. We Azure Machine Learning gebruiken voor het bouwen van een binaire classificatie-model in deze gegevens. Ook laten we zien hoe u een van deze modellen als een webservice publiceert.

Het is ook mogelijk om te gebruiken een laptop IPython taken die zijn gepresenteerd in dit scenario. Gebruikers die u wilt proberen deze benadering moeten contact opnemen met de [Criteo scenario met behulp van een verbinding Hive ODBC](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) onderwerp.

## <a name="dataset"></a>Beschrijving van de gegevensset Criteo
De Criteo gegevens is een klik voorspelling gegevensset die ongeveer 370GB gecomprimeerde gzip TSV-bestanden (~1.3TB ongecomprimeerde), die bestaat uit meer dan 4.3 miljard records. Het is afkomstig uit 24 dagen van klik dan op gegevens die de [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/). Voor het gemak van gegevenswetenschappers, hebben we gegevens beschikbaar voor ons om te experimenteren met uitgepakt.

Elke record in deze gegevensset bevat 40 kolommen:

* de eerste kolom is een labelkolom waarin wordt aangegeven of een gebruiker klikt op een **toevoegen** (waarde 1) of niet op een (waarde 0)
* naast 13 kolommen zijn numerieke, en
* laatste 26 zijn categorische kolommen

De kolommen zijn geanonimiseerde en gebruiken van een reeks geïnventariseerde namen: "Col1' (voor de labelkolom) naar ' Col40 ' (voor de laatste categorische kolom).            

Hier volgt een fragment van de eerste 20 kolommen uit twee rapporten (rijen) van deze gegevensset:

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

Er zijn ontbreken waarden in de numerieke en categorische kolommen in deze dataset. Een eenvoudige methode voor het verwerken van de ontbrekende waarden worden beschreven. Aanvullende details van de gegevens worden verkend wanneer we ze opslaan in de Hive-tabellen.

**Definitie:** *Clickthrough-snelheid (CTR):* dit is het percentage muisklikken in de gegevens. In deze dataset Criteo is de CTR ongeveer 3.3% of 0.033.

## <a name="mltasks"></a>Voorbeelden van voorspelling taken
Twee voorbeeld voorspelling problemen worden in dit scenario behandeld:

1. **Binaire classificatie**: voorspelt of een gebruiker een add geklikt:
   
   * Klasse 0: Er is geen Klik
   * Klasse 1: klik op
2. **Regressie**: de waarschijnlijkheid van een ad-klik van functies voor gebruikers worden voorspeld.

## <a name="setup"></a>Instellen van een HDInsight Hadoop-cluster voor gegevenswetenschap
**Opmerking:** dit is doorgaans een **Admin** taak.

Instellen van uw Azure-Gegevenswetenschap-omgeving voor het bouwen van predictive analytics-oplossingen met HDInsight-clusters in drie stappen:

1. [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md): dit opslagaccount wordt gebruikt voor het opslaan van gegevens in Azure Blob Storage. De gegevens in HDInsight-clusters gebruikt, wordt hier opgeslagen.
2. [Azure HDInsight Hadoop-Clusters aanpassen voor Gegevenswetenschap](customize-hadoop-cluster.md): deze stap maakt u een Azure HDInsight Hadoop-cluster met 64-bits Anaconda Python 2.7 is geïnstalleerd op alle knooppunten. Er zijn twee belangrijke stappen die u (in dit onderwerp beschreven) moeten worden uitgevoerd bij het aanpassen van het HDInsight-cluster.
   
   * Het opslagaccount dat is gemaakt in stap 1 met uw HDInsight-cluster wanneer deze wordt gemaakt, moet u koppelen. Dit opslagaccount wordt gebruikt voor toegang tot gegevens die kunnen worden verwerkt binnen het cluster.
   * Nadat deze is gemaakt, moet u externe toegang inschakelen met het hoofdknooppunt van het cluster. Vergeet niet de RAS-referenties die u hier opgeeft (anders dan die zijn opgegeven voor het cluster bij het maken ervan): u moet de volgende procedures uitvoeren.
3. [Maken van een Azure ML-werkruimte](../studio/create-workspace.md): deze Azure Machine Learning-werkruimte wordt gebruikt voor het bouwen van machine learning-modellen na een initiële gegevensverkenning en omlaag steekproeven op het HDInsight-cluster.

## <a name="getdata"></a>Ophalen van gegevens en gebruiken van een openbare bron
De [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset kan worden geopend door op de koppeling te klikken, de gebruiksvoorwaarden accepteren en een naam geven. Hier ziet u een momentopname van wat ziet deze als eruit:

![Criteo voorwaarden accepteren](./media/hive-criteo-walkthrough/hLxfI2E.png)

Klik op **doorgaan naar downloaden** voor meer informatie over de gegevensset en de beschikbaarheid.

De gegevens zich in een openbare [Azure blob-opslag](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) locatie: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/. De 'wasb"verwijst naar Azure Blob Storage-locatie. 

1. De gegevens in deze openbare blob-opslag bestaat uit drie submappen van uitgepakte gegevens.
   
   1. De submap *Onbewerkte/telling/* bevat de eerste 21 dagen aan gegevens - vanaf dag\_00 op dag\_20
   2. De submap *onbewerkte/train/* bestaat uit één dag van gegevens, dag\_21
   3. De submap *onbewerkte en testen/* bestaat uit twee dagen aan gegevens, dag\_22 en dag\_23
2. Voor degenen die moet worden gestart met de onbewerkte gzip-gegevens, zijn ook beschikbaar in de hoofdmap *onbewerkte /* als day_NN.gz, waarbij NN tussen 00 en 23 gaat.

Een alternatieve methode om toegang tot, onderzoek en het model dat deze gegevens waarvoor lokale downloads niet verderop in dit scenario wordt uitgelegd wanneer we Hive-tabellen maken.

## <a name="login"></a>Meld u aan het cluster headnode bij
Voor het aanmelden bij de headnode van het cluster, gebruikt u de [Azure-portal](https://ms.portal.azure.com) vinden van het cluster. Klik op het pictogram HDInsight plaats aan de linkerkant en dubbelklik vervolgens op de naam van het cluster. Navigeer naar de **configuratie** tabblad, dubbelklik op het pictogram CONNECT aan de onderkant van de pagina en geef uw referenties voor externe toegang als u wordt gevraagd. Hiermee gaat u naar de headnode van het cluster.

Hier volgt een typisch eerst aanmelden op het cluster headnode ziet er als:

![Meld u aan het cluster](./media/hive-criteo-walkthrough/Yys9Vvm.png)

Aan de linkerkant ziet u de ' Hadoop opdrachtregel ', die onze werkpaard voor de gegevensverkenning is. Ook ziet u twee nuttig URL's - 'Hadoop Yarn Status' en 'Hadoop de naam van knooppunt'. De URL van de status yarn geeft de voortgang van de taak en de URL van de naam van knooppunt geeft informatie over de configuratie van het cluster.

Nu we worden ingesteld en klaar om te beginnen met de eerste deel van deze stapsgewijze Kennismaking: gegevensverkenning gebruik van Hive en voorbereiden van gegevens voor Azure Machine Learning.

## <a name="hive-db-tables"></a>Hive-database en tabellen maken
Voor het maken van Hive-tabellen voor onze gegevensset Criteo, opent u de ***Hadoop-opdrachtregel*** op het bureaublad van het hoofdknooppunt en voer de Hive-map met de opdracht

    cd %hive_home%\bin

> [!NOTE]
> Alle Hive-opdrachten uitvoeren in dit overzicht van de opslaglocatie Hive / directory prompt. Dit zorgt voor problemen die pad automatisch. We gebruiken de termen 'Hive directory prompt', ' Hive bin / directory prompt ', en ' Hadoop opdrachtregel ' door elkaar.
> 
> [!NOTE]
> Voor het uitvoeren van een Hive-query, kan een altijd gebruik van de volgende opdrachten:
> 
> 

        cd %hive_home%\bin
        hive

Nadat de Hive-REPL wordt weergegeven met een ' hive > "aanmelden, gewoon knippen en plakken de query voor het uitvoeren van deze.

De volgende code maakt een database 'criteo' en genereert vervolgens 4 tabellen:

* een *tabel voor het genereren van aantallen* gebouwd op dagen dag\_00 op dag\_20,
* een *tabel voor gebruik als de trein gegevensset* gebouwd op dag\_21, en
* twee *tabellen voor gebruik als de test-gegevenssets* gebouwd op dag\_22 en dag\_23 respectievelijk.

We onze testgegevensset opgesplitst in twee verschillende tabellen omdat een van de dagen een vakantie is en we bepalen willen als het model verschillen tussen een feestdag en niet van de snelheid clickthrough kunt detecteren.

Het script [voorbeeld &#95; hive &#95; maken &#95; criteo &#95; database &#95; en &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) voor het gemak Hier wordt weergegeven:

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

We Houd er rekening mee dat alle tabellen in deze externe zijn als we simpelweg naar Azure Blob Storage (wasb)-locaties wijst.

**Er zijn twee manieren een Hive-query die we nu vermeld uit te voeren.**

1. **Met behulp van de opdrachtregel Hive-REPL**: de eerste is voor het verlenen van een opdracht 'hive' en kopieer en plak een query op de opdrachtregel Hive-REPL. U doet dit door het volgende te doen:
   
        cd %hive_home%\bin
        hive
   
     Nu op de opdrachtregel REPL knippen en plakken van de query wordt uitgevoerd het.
2. **Een query's opslaan en uitvoeren van de opdracht**: de tweede is het opslaan van de query's in een bestand .hql ([voorbeeld &#95; hive &#95; maken &#95; criteo &#95; database &#95; en &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) en vervolgens voert u de volgende opdracht de query uit te voeren:
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a>Het maken van database en tabel bevestigen
Vervolgens wordt het maken van de database met de volgende opdracht uit de Hive-opslaglocatie bevestigen / opdrachtprompt van de map:

        hive -e "show databases;"

Hierdoor hebt:

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

Hiermee bevestigt u het maken van de nieuwe database 'criteo'.

Om te zien welke tabellen die zijn gemaakt, geven we gewoon de opdracht uit de Hive-opslaglocatie / opdrachtprompt van de map:

        hive -e "show tables in criteo;"

Vervolgens ziet u de volgende uitvoer:

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <a name="exploration"></a>Gegevensverkenning in component
We kunnen nu klaar voor sommige basic gegevensverkenning in Hive. We beginnen met het aantal voorbeelden in de trein tellen en gegevenstabellen testen.

### <a name="number-of-train-examples"></a>Aantal train-voorbeelden
De inhoud van [voorbeeld &#95; hive &#95; &#95;tellen; train &#95; tabel &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) worden hier weergegeven:

        SELECT COUNT(*) FROM criteo.criteo_train;

Dit levert:

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

U kunt ook een mag de volgende opdracht uit de Hive-opslaglocatie eveneens verlenen / opdrachtprompt van de map:

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a>Aantal voorbeelden van de test in de twee gegevenssets van de test
We tellen het aantal voorbeelden in de twee gegevenssets van de test nu. De inhoud van [voorbeeld &#95; hive &#95; &#95;tellen; criteo &#95; test &#95; dag &#95; 22 &#95; tabel &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) hier zijn:

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

Dit levert:

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

Gebruikelijke we ook het script kan aanroepen vanuit de Hive-opslaglocatie / Active directory vragen door de opdracht:

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

Ten slotte wordt het aantal voorbeelden van de test in de testgegevensset op basis van dag onderzoeken\_23.

De opdracht om dit te doen is vergelijkbaar met alleen wordt weergegeven (Raadpleeg [voorbeeld &#95; hive &#95; &#95;tellen; criteo &#95; test &#95; dag &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

Hierdoor hebt:

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a>Distributiepunten in de gegevensset train label
De distributie van het label in de gegevensset train is van belang. Als u wilt dit ziet, laten we zien inhoud van [voorbeeld &#95; hive &#95; criteo &#95; &#95;label; verdeling &#95; train &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

Dit geeft de label-distributie:

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

Houd er rekening mee dat het percentage positieve labels ongeveer 3.3% (consistent met de oorspronkelijke gegevensset is).

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a>Histogram distributies van bepaalde numerieke variabelen in de gegevensset train
We kunnen gebruiken van Hive systeemeigen ' histogram\_numerieke ' functie om erachter te komen hoe de distributie van de numerieke variabelen eruit ziet. Hier vindt u de inhoud van [voorbeeld &#95; hive &#95; criteo &#95; histogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

Dit resulteert in het volgende:

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

De LATERAL BEELD - vouwen combinatie in Hive fungeert voor het produceren van een SQL-achtige uitvoer in plaats van de gebruikelijke lijst. Houd er rekening mee dat in deze tabel de eerste kolom komt overeen met de opslaglocatie center en de tweede de frequentie van de opslaglocatie.

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a>Geschatte percentielen van bepaalde numerieke variabelen in de gegevensset train
Is ook de berekening van de geschatte percentielen van belang met numerieke variabelen. Hive de systeemeigen ' percentiel\_ongeveer ' Dit wordt uitgevoerd voor ons. De inhoud van [voorbeeld &#95; hive &#95; criteo &#95; geschatte &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) zijn:

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

Dit levert:

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

We dat de distributie van percentielen is verwant aan de distributie van het histogram van een numerieke variabele meestal te schakelen.         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a>Aantal unieke waarden vinden voor sommige categorische kolommen in de gegevensset train
U doorgaat met de gegevensverkenning, vinden we nu, voor sommige categorische kolommen, het aantal unieke waarden die ze nemen. U doet dit door laten we zien inhoud van [voorbeeld &#95; hive &#95; criteo &#95; unieke &#95; waarden &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

Dit levert:

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

We Houd er rekening mee dat Col15 19M unieke waarden bevat. Met behulp van naïve technieken zoals 'één hot codering' is voor het coderen van dergelijke hoge dimensionale categorische variabelen onbruikbare. In het bijzonder we uitleggen en demonstreren van een krachtige, robuuste techniek aangeroepen [Learning met telt](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) voor efficiënt aanpakken van dit probleem.

We beëindigen deze subsectie door te kijken naar het aantal unieke waarden voor sommige andere categorische kolommen ook. De inhoud van [voorbeeld &#95; hive &#95; criteo &#95; unieke &#95; waarden &#95; meerdere &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) zijn:

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

Dit levert:

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

Opnieuw zien we dat Col20, met uitzondering van de kolommen veel unieke waarden.

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a>Mede exemplaar telt paren van categorische variabelen in de gegevensset train

Het aantal mede exemplaar uit paren van categorische variabelen is ook van belang. Dit kan worden bepaald met de code in [voorbeeld &#95; hive &#95; criteo &#95; gekoppeld &#95; categorische &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

We omgekeerde hun exemplaar van het aantal sorteren en zoek boven 15 in dit geval. Hierdoor hebt ons:

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a>Omlaag voorbeeld gegevenssets voor Azure Machine Learning
Met de gegevenssets verkend en gedemonstreerd hoe we kan doen dit soort exploratie geen variabelen (waaronder combinaties), wordt nu naar beneden voorbeeld de gegevenssets zodat we modellen in Azure Machine Learning kunnen bouwen. Let erop dat het probleem dat we richten op: een set voorbeeld kenmerken (functie waarden van Col2 - Col40) opgegeven, we voorspellen als Col1 0 (geen klik) of 1 (klik is).

Omlaag steekproef onze train en gegevenssets niet 1% van de oorspronkelijke grootte testen, gebruiken we Hive van systeemeigen ASELECT() functie. Het volgende script [voorbeeld &#95; hive &#95; criteo &#95; verkleinen &#95; train &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) doet u dit voor de gegevensset train:

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

Dit levert:

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

Het script [voorbeeld &#95; hive &#95; criteo &#95; verkleinen &#95; test &#95; dag &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) gebeurt dit voor testgegevens, dag\_22:

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

Dit levert:

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


Ten slotte wordt het script [voorbeeld &#95; hive &#95; criteo &#95; verkleinen &#95; test &#95; dag &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) gebeurt dit voor testgegevens, dag\_23:

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

Dit levert:

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

Dit zijn gereed voor gebruik van onze omlaag opgevangen train en test gegevenssets voor het bouwen van modellen in Azure Machine Learning.

Er is een definitieve belangrijk onderdeel voordat we verder gaan met Azure Machine Learning, wat betreft de count-tabel. In de volgende subsectie bespreken we dit sommige beschreven.

## <a name="count"></a>Een korte bespreking van de count-tabel
Zoals u hebt gezien, hebben verschillende categorische variabelen een zeer hoge dimensionaliteit. In ons scenario we een krachtige methode aangeroepen in dit [Learning met telt](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) voor het coderen van deze variabelen in een efficiënte en krachtige manier. Meer informatie over deze techniek is in de koppeling.

[!NOTE]
>We richten op het aantal tabellen gebruiken voor het produceren van compact representaties van hoge-dimensionale categorische functies in dit scenario. Dit is niet de enige manier voor het coderen van functies categorische; voor meer informatie over andere technieken geïnteresseerd gebruikers kunnen uitchecken [one-hot-encoding](http://en.wikipedia.org/wiki/One-hot) en [hash-functie](http://en.wikipedia.org/wiki/Feature_hashing).
>

Aantal tabellen op de gegevens van het aantal bouwen, gebruiken we de gegevens in de map onbewerkte/aantal. In de sectie modellering laten we zien gebruikers deze aantal tabellen voor categorische functies helemaal, maken of u kunt ook een aantal vooraf gemaakte tabel gebruiken voor hun explorations. In welke volgt wanneer we verwijzen naar 'vooraf samengestelde aantal tabellen', bedoelen we met behulp van het aantal tabellen die wij verstrekken. Gedetailleerde instructies over toegang krijgen tot deze tabellen zijn beschikbaar in de volgende sectie.

## <a name="aml"></a>Een model met Azure Machine Learning bouwen
Het model bouwen proces in Azure Machine Learning verloopt als volgt:

1. [De gegevens ophalen uit de Hive-tabellen in Azure Machine Learning](#step1)
2. [Het experiment maken: de gegevens en featurize met aantal tabellen opschonen](#step2)
3. [Bouwen, te trainen en het model beoordelen](#step3)
4. [Het model evalueren](#step4)
5. [Het model publiceren als een webservice](#step5)

We zijn nu gereed voor het bouwen van modellen in Azure Machine Learning studio. Onze omlaag steekproefgegevens wordt opgeslagen als Hive-tabellen in het cluster. We gebruiken de Azure Machine Learning **importgegevens** module om deze gegevens te lezen. De referenties voor toegang tot het opslagaccount van dit cluster zijn opgegeven in het volgende.

### <a name="step1"></a>Stap 1: Gegevens ophalen uit de Hive-tabellen in Azure Machine Learning met de module gegevens importeren en selecteren om een machine learning-experiment
Begin met het selecteren een **+ nieuw** -> **EXPERIMENT** -> **leeg Experiment**. Klik vanuit de **Search** vak op de bovenste links, zoek naar 'Gegevens importeren'. Slepen en neerzetten de **importgegevens** module doorsturen naar het experiment (middelste gedeelte van het scherm) met de module voor gegevenstoegang canvas.

Dit is wat de **importgegevens** lijkt tijdens het ophalen van gegevens uit de Hive-tabel:

![Gegevens importeren gegevens worden opgehaald](./media/hive-criteo-walkthrough/i3zRaoj.png)

Voor de **importgegevens** module, de waarden van de parameters die beschikbaar zijn in de afbeelding zijn alleen voorbeelden van de sortering van waarden die u moet opgeven. Hier volgt een aantal algemene richtlijnen voor het invullen van de parameter is ingesteld voor de **importgegevens** module.

1. 'Hive-query' kiezen voor **gegevensbron**
2. In de **Hive databasequery** vak een eenvoudige SELECT * FROM < uw\_database\_name.your\_tabel\_name >-voldoende is.
3. **Hcatalog server-URI**: als uw cluster "abc", wordt dit eenvoudig is: https://abc.azurehdinsight.net
4. **Hadoop-gebruikersaccountnaam**: de naam van de gebruiker gekozen op het moment van het bedrijf stellen van het cluster. (Niet de RAS-gebruikersnaam!)
5. **Het wachtwoord voor gebruikersaccount Hadoop**: het wachtwoord voor de naam van de gebruiker gekozen op het moment van het bedrijf stellen van het cluster. (Niet de RAS-wachtwoord!)
6. **Locatie van uitvoergegevens**: 'Azure' kiezen
7. **Naam van het Azure-opslagaccount**: het storage-account die is gekoppeld aan het cluster
8. **Azure-toegangssleutel**: de sleutel van het opslagaccount die is gekoppeld aan het cluster.
9. **Azure containernaam**: als de clusternaam is "abc", wordt dit gewoon "abc", meestal is.

Eenmaal de **importgegevens** ophalen van gegevens (ziet u de groene maatstreepjes op de Module), zijn voltooid deze gegevens opslaan als een Dataset (met een naam van uw keuze). Wat ziet deze eruit als:

![Gegevens importeren opslaan gegevens](./media/hive-criteo-walkthrough/oxM73Np.png)

Met de rechtermuisknop op de uitvoerpoort van de **importgegevens** module. Dit blijkt een **opslaan als gegevensset** optie en een **Visualize** optie. De **Visualize** optie, als u klikt, wordt weergegeven, 100 rijen van de gegevens, inclusief een rechterpaneel die nuttig is voor sommige samenvattende statistieken. Gegevens moeten worden opgeslagen, selecteert u de **opslaan als gegevensset** en volg de instructies.

Schakel de opgeslagen gegevensset voor gebruik in een machine learning-experiment, zoek de gegevenssets met behulp van de **Search** vak weergegeven in de volgende afbeelding. Typ vervolgens gewoon de naam hebt u de gegevensset gedeeltelijk toegang hebben en sleep de gegevensset naar het hoofdpaneel gegeven. Neer te zetten naar het hoofdpaneel de wordt geselecteerd voor gebruik in machine learning-model.

![Drage gegevensset naar het hoofdpaneel](./media/hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> Doe dit voor zowel de trein en de test-gegevenssets. Bovendien moet de databasenaam en tabelnamen die u hebt opgegeven voor dit doel gebruiken. De waarden die worden gebruikt in de afbeelding dienen uitsluitend ter illustratie purposes.* *
> 
> 

### <a name="step2"></a>Stap 2: Een eenvoudig experiment maken in Azure Machine Learning om te voorspellen klikken / geen klikken
Ons Azure ML-experiment ziet er als volgt:

![Machine Learning-experiment](./media/hive-criteo-walkthrough/xRpVfrY.png)

We nu eens kijken hoe de belangrijkste onderdelen van dit experiment. Als een herinnering moeten we onze opgeslagen train slepen en gegevenssets die u aan bij onze experimentcanvas eerst te testen.

#### <a name="clean-missing-data"></a>De ontbrekende gegevens opschonen
De **Clean Missing Data** module biedt wat de naam al aangeeft: het ontbrekende gegevens op een manier die de gebruiker opgegeven kunnen worden schoongemaakt. In deze module zoekt, ziet u dit:

![Ontbrekende gegevens opschonen](./media/hive-criteo-walkthrough/0ycXod6.png)

Hier hebt alle ontbrekende waarden vervangt door een 0. Er zijn ook andere opties wilt invoeren die kunnen worden gezien door te kijken in de vervolgkeuzelijsten in de module.

#### <a name="feature-engineering-on-the-data"></a>Functie-engineering op de gegevens
Er zijn miljoenen unieke waarden voor sommige categorische functies van grote gegevenssets. Naïve-methoden zoals een hot codering voor het voorstellen van dergelijke hoge dimensionale categorische functies is volledig unfeasible. In dit overzicht ziet u hoe u een aantal functies met ingebouwde Azure Machine Learning-modules voor het genereren van compact representaties van deze hoge-dimensionale categorische variabelen gebruiken. Het eindresultaat is een kleinere model, sneller training en maatstaven voor prestaties die erg vergelijkbaar zijn met andere technieken.

##### <a name="building-counting-transforms"></a>Gebouw transformaties tellen
Aantal functies bouwen, gebruiken we de **bouwen tellen transformeren** module die beschikbaar is in Azure Machine Learning. De module ziet er als volgt:

![Module tellen transformeren maken](./media/hive-criteo-walkthrough/e0eqKtZ.png)
![module bouwen tellen transformeren](./media/hive-criteo-walkthrough/OdDN0vw.png)

> [!IMPORTANT] 
> In de **tellen kolommen** vak vullen we de kolommen die we uitvoeren telt willen op. Dit zijn meestal (zoals vermeld) hoge dimensionale categorische kolommen. Aan het begin wordt vermeld dat de dataset Criteo 26 categorische kolommen bevat: van Col15 naar Col40. Hier we tellen op alle mappen en hun indexen geven (van 15 tot 40 gescheiden door komma's, zoals wordt weergegeven).
> 

De module gebruiken in de modus MapReduce (geschikt voor grote gegevenssets), moeten we toegang tot een HDInsight Hadoop-cluster (gebruikt voor de functie verkenning opnieuw kan worden gebruikt voor dit doel ook) en de referenties. In de vorige afbeeldingen laten zien wat de ingevulde waarden eruit (Vervang de waarden voor de afbeelding met die relevant zijn voor uw eigen gebruiksvoorbeeld).

![Moduleparameters](./media/hive-criteo-walkthrough/05IqySf.png)

In de bovenstaande afbeelding laten we zien hoe de locatie van de blob-invoerbron invoeren. Deze locatie heeft de gegevens die zijn gereserveerd voor het aantal tabellen maken op.

Nadat deze module is voltooid, we kunnen de transformatie voor later opslaan met de rechtermuisknop op de module en selecteert u de **opslaan als transformatie** optie:

![Optie 'Opslaan als transformatie'](./media/hive-criteo-walkthrough/IcVgvHR.png)

In onze experiment architectuur hierboven weergegeven, correspondeert de gegevensset 'ytransform2' nauwkeurig opgeslagen aantal transformatie. Voor de rest van dit experiment, gaan we ervan uit dat de lezer gebruikt een **bouwen tellen transformeren** -module op bepaalde gegevens aantallen genereert, en gebruik vervolgens de aantallen voor het genereren van aantal kunt functies op de trein en test gegevenssets.

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a>Kiezen welke aantal functies voor het opnemen als onderdeel van de trein en test gegevenssets
Zodra er een aantal transformeren gereed hebt, de gebruiker kan kiezen welke functies wilt opnemen in hun train en testen met behulp van gegevenssets de **wijzigen aantal tabel Parameters** module. We deze module alleen weergeven hier voor de volledigheid, maar uit oogpunt van eenvoud daadwerkelijk gebruik deze functie niet bij ons experiment.

![De parameters voor aantal wijzigen](./media/hive-criteo-walkthrough/PfCHkVg.png)

In dit geval zoals u kunt zien, we hebben ervoor gekozen alleen de logboek-kans gebruiken en de achtergrond uit kolom negeren. We kunnen ook parameters instellen, zoals de drempelwaarde van de opslaglocatie garbagecollection hoeveel pseudo eerdere voorbeelden voor het vloeiend maken en of u alle ruis Laplacian of niet toe te voegen. Al deze functies zijn geavanceerde en het is om te worden opgemerkt dat de standaardwaarden zijn een goed uitgangspunt voor gebruikers die niet bekend met dit type functie generatie bent.

##### <a name="data-transformation-before-generating-the-count-features"></a>Gegevenstransformatie vóór het genereren van de count-functies
Nu we gericht op een belangrijk punt over onze train transformeren en testgegevens vóór het genereren van daadwerkelijk aantal functies. Houd er rekening mee dat er twee zijn **R-Script uitvoeren** modules die worden gebruikt voordat we de count-transformatie op onze gegevens toepast.

![Modules R-Script uitvoeren](./media/hive-criteo-walkthrough/aF59wbc.png)

Dit is het eerste R-script:

![Eerste R-script](./media/hive-criteo-walkthrough/3hkIoMx.png)

In deze R-script wijzigen we onze kolommen aan namen 'Col1' naar 'Col40'. Dit komt doordat de count-transformatie namen van deze indeling verwacht.

In het tweede R-script we een balans vinden tussen de verdeling tussen klassen positieve en negatieve (respectievelijk klassen 1 en 0) door de negatieve klasse downsampling. Het R-script laat zien hoe u dit doet:

![Tweede R-script](./media/hive-criteo-walkthrough/91wvcwN.png)

In dit eenvoudige R-script gebruiken we ' pos\_neg\_verhouding ' het bedrag van balans tussen de positieve en negatieve klassen instellen. Dit is belangrijk te doen omdat meestal verbeteren van de klasse onbalans prestatievoordelen voor classificatie problemen waar de klasse-distributie is vervormd (intrekken dat in ons geval we 3.3% positief klasse en 96,7% negatief klasse hebben).

##### <a name="applying-the-count-transformation-on-our-data"></a>De transformatie aantal toepassen op onze gegevens
Ten slotte kunnen we gebruiken de **transformatie toepassen** module voor het toepassen van de count-transformaties op onze train en gegevenssets te testen. Deze module wordt de transformatie opgeslagen aantal als één invoer- en de trein of test gegevenssets als andere invoer en retourneert gegevens met een aantal functies. Hier wordt weergegeven:

![Transformatie module toepassen](./media/hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a>Een fragment van welke het aantal functies eruit
Het is om te zien hoe de functies count eruit in ons geval instructieve. Hier wordt een fragment van deze weergeven:

![Aantal functies](./media/hive-criteo-walkthrough/FO1nNfw.png)

In dit fragment zien we dat voor de kolommen die we op geteld, we het aantal ophalen en kans naast eventuele relevante backoffs logboekbestanden.

Er zijn nu gereed voor het bouwen van een Azure Machine Learning-model met behulp van deze getransformeerde gegevenssets. We zien hoe u kunt dit doen in de volgende sectie.

### <a name="step3"></a>Stap 3: Bouwen, te trainen en het model beoordelen

#### <a name="choice-of-learner"></a>Keuze van cursist
Eerst moet ervoor kiezen een cursist. We gaan een beslissingsstructuur twee class boosted als onze cursist gebruiken. Hier volgen de standaardopties voor deze cursist:

![Two-Class Boosted Decision Tree parameters](./media/hive-criteo-walkthrough/bH3ST2z.png)

We gaan Kies de standaardwaarden voor onze experiment. We Houd er rekening mee dat de standaardwaarden meestal zinvolle zijn en een goede manier om snel basislijnen op de prestaties. U kunt op de prestaties verbeteren door verstrekkende parameters als u zodra er een basislijn.

#### <a name="train-the-model"></a>Het model trainen
Voor training, roepen we gewoon een **Train Model** module. De twee invoeren aan zijn de cursist Two-Class Boosted Decision Tree en onze train-gegevensset. Dit wordt hier weergegeven:

![Module Train-Model](./media/hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-the-model"></a>Het model scoren
Zodra er een getraind model hebt, zijn wij gereed om te beoordelen op de testgegevensset en evalueren van de prestaties. We dit doen via de **Score Model** module weergegeven in de volgende afbeelding, samen met een **Evaluate Model** module:

![De module Score Model (Scoremodel)](./media/hive-criteo-walkthrough/fydcv6u.png)

### <a name="step4"></a>Stap 4: Het model beoordelen
Tot slot willen we model prestaties analyseren. Meestal is een goede indicatie voor twee klasse (binair) classificatie problemen, de AUC. Als u wilt dit visualiseren, we aansluiten de **Score Model** module die u wilt een **Evaluate Model** -module voor dit. Te klikken op **Visualize** op de **Evaluate Model** module resulteert in een afbeelding zoals de volgende:

![Module BDT model evalueren](./media/hive-criteo-walkthrough/0Tl0cdg.png)

Binair (of twee klasse) classificatie problemen met een goede indicatie van nauwkeurigheid is het gebied onder Curve (AUC). In het volgende, laten we zien onze resultaten met dit model op onze testgegevensset. Als u dit, met de rechtermuisknop op de uitvoerpoort van de **Evaluate Model** module en vervolgens **Visualize**.

![Module Evaluate Model visualiseren](./media/hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step5"></a>Stap 5: Het model publiceren als een webservice
De mogelijkheid voor het publiceren van een Azure Machine Learning-model als webservices met een minimum van fuss is een zeer nuttige functie voor het maken van het algemeen beschikbaar. Zodra u dat hebt gedaan, kan iedereen bellen naar de webservice met invoer gegevens die ze voorspellingen voor nodig hebt, en het model om te retourneren die voorspellingen maakt gebruik van de webservice.

U doet dit door opslaan we eerst het getrainde model als een object van het getrainde Model. Dit wordt gedaan met de rechtermuisknop op de **Train Model** module en met behulp van de **opslaan als getrainde Model** optie.

Vervolgens moet voor het maken van de invoer en uitvoer van poorten voor onze webservice:

* een invoerpoort worden gegevens in het formulier van de gegevens die we nodig hebben voorspellingen voor
* een uitvoerpoort de Scored Labels en de bijbehorende waarschijnlijkheid geretourneerd.

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a>Selecteer een paar rijen van de gegevens voor de invoer-poort
Het is gemakkelijk te gebruiken een **toepassen SQL transformatie** module om net 10 rijen als de invoerpoort gegevens te selecteren. Selecteer alleen deze rijen met gegevens voor onze invoerpoort met behulp van de SQL-query die hier worden weergegeven:

![Invoerpoort gegevens](./media/hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a>Webservice
We zijn nu gereed voor gebruik van een kleine experiment die kan worden gebruikt voor het publiceren van de webservice.

#### <a name="generate-input-data-for-webservice"></a>Genereren van de invoergegevens voor de webservice
Als een stap zeroth aangezien de tabel aantal groot, we nemen van een paar regels testgegevens en uitvoergegevens genereren van het aantal functies. Dit kan fungeren als de invoergegevens notatie voor de webservice. Dit wordt hier weergegeven:

![De invoergegevens BDT maken](./media/hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> Voor de indeling invoergegevens gebruiken we nu de uitvoer van de **aantal Featurizer** module. Zodra dit is voltooid met experimenteren, sla de uitvoer van de **aantal Featurizer** module als een Dataset. Deze gegevensset wordt gebruikt voor de invoergegevens in de webservice.
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a>Score berekenen voor experiment voor de publicatie-webservice
Eerst laten we zien hoe dit eruitziet. De structuur van essentieel belang is een **Score Model** module die accepteert onze getrainde model-object en een paar regels met invoergegevens die wordt gegenereerd in de vorige stappen met behulp van de **aantal Featurizer** module. We gebruiken 'Kolommen in gegevensset selecteren' aan de Scored labels en de waarschijnlijkheid Score-project.

![Kolommen in gegevensset selecteren](./media/hive-criteo-walkthrough/kRHrIbe.png)

U ziet hoe de **Select Columns in Dataset** module kan worden gebruikt voor 'uitgefilterd' gegevens uit een gegevensset. We weergeven de inhoud hier:

![Filteren met de Select Columns in Dataset-module](./media/hive-criteo-walkthrough/oVUJC9K.png)

Als u de blauwe invoer en uitvoerpoorten, klikt u op **voorbereiden webservice** aan de onderkant rechts. Dit experiment uitgevoerd ook kan worden uitgevoerd op de webservice publiceren: klik op de **WEBSERVICE PUBLICEERT** pictogram aan de onderkant rechts, weergegeven hier:

![Webservice publiceren](./media/hive-criteo-walkthrough/WO0nens.png)

Als de webservice is gepubliceerd, krijgen we omgeleid naar een pagina die er zo uitziet:

![Web service-dashboard](./media/hive-criteo-walkthrough/YKzxAA5.png)

Twee koppelingen voor webservices zien we aan de linkerkant:

* De **aanvragen/reacties** Service (of RR's) zijn alleen bedoeld voor één voorspellingen en is er met deze workshop gebruiken.
* De **BATCHUITVOERING** Service (BES) wordt gebruikt voor voorspellingen batch en vereist dat de invoergegevens gebruikt voor het maken van voorspellingen zich bevinden in Azure Blob Storage.

Op de koppeling te klikken **aanvragen/reacties** vergt ons naar een pagina die we de code in C#, python en R. vooraf blik Deze code kan gemakkelijk worden gebruikt voor het aanroepen van de webservice. Houd er rekening mee dat de API-sleutel op deze pagina moet worden gebruikt voor verificatie.

Is het handig deze python-code naar een nieuwe cel in de notebook IPython overschreven.

We weergeven hier een segment van python-code met de juiste API-sleutel.

![Python-code](./media/hive-criteo-walkthrough/f8N4L4g.png)

Houd er rekening mee dat we de standaard-API-sleutel hebt vervangen door onze webservices-API-sleutel. Te klikken op **uitvoeren** op deze cel in een IPython notebook resulteert in het volgende antwoord:

![IPython antwoord](./media/hive-criteo-walkthrough/KSxmia2.png)

We zien dat voor de twee test voorbeelden die we gevraagd over (in het kader van de JSON van het pythonscript), we weer antwoorden in de vorm krijgen 'Scored Labels, Scored kansen'. Houd er rekening mee dat in dit geval we hebt gekozen de standaardwaarden vooraf voorgedefinieerde biedt (0 voor alle numerieke kolommen en de tekenreeks '' waarde '' voor alle categorische kolommen).

Hiermee is onze end-to-end procedure voor het afhandelen van grootschalige gegevensset met Azure Machine Learning. We de slag met een terabyte van gegevens, een Voorspellend model samengesteld en wordt geïmplementeerd als een webservice in de cloud.

