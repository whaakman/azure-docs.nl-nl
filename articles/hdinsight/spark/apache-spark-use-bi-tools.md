---
title: Met behulp van de hulpmiddelen voor gegevensvisualisatie in Azure HDInsight Spark-BI | Microsoft Docs
description: Hulpmiddelen voor gegevensvisualisatie voor analyses met Apache Spark BI op HDInsight-clusters gebruiken
keywords: Apache spark bi, spark bi, spark gegevensvisualisatie, spark business intelligence
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: d1d5405a635b9f990f53b2bf32c8270a71a0f344
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI hulpmiddelen voor gegevensvisualisatie gebruiken met Azure HDInsight

Informatie over het hulpmiddelen voor gegevensvisualisatie zoals Power BI en Tableau gebruiken voor het analyseren van een onbewerkte voorbeeldgegevensset met BI van Apache Spark in HDInsight-clusters.

> [!NOTE]
> Connectiviteit met BI-tools die worden beschreven in dit artikel wordt niet ondersteund op 2.1 Spark op Azure HDInsight 3.6 Preview. Alleen Spark versie 1.6 en 2.0 (HDInsight 3.4 3.5 respectievelijk) worden ondersteund.
>

Deze zelfstudie is ook beschikbaar als een Jupyter-notebook in een HDInsight Spark-cluster. De ervaring voor de notebook kunt u de Python-codefragmenten uitvoeren vanaf de notebook zelf. Als u de zelfstudie uit binnen een laptop, een Spark-cluster maken, een Jupyter-notebook starten (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), en voer vervolgens de notebook **gebruik BI-tools met Apache Spark in HDInsight.ipynb** onder de **Python** map.

## <a name="prerequisites"></a>Vereisten

* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).


## <a name="hivetable"></a>Gegevens voorbereiden voor Spark gegevensvisualisatie

In deze sectie gebruiken we de [Jupyter](https://jupyter.org) laptop van een HDInsight Spark-cluster aan taken uitvoeren die uw onbewerkte voorbeeldgegevens en deze opslaan in een tabel. De voorbeeldgegevens is een CSV-bestand (hvac.csv) beschikbaar in alle clusters standaard. Wanneer uw gegevens wordt opgeslagen als een tabel, in de volgende sectie gebruiken we BI-tools verbinding maken met de tabel en gegevensvisualisaties uitvoeren.

> [!NOTE]
> Als u de stappen in dit artikel uitvoeren wilt na het voltooien van de instructies in [interactieve query's uitvoeren op een HDInsight Spark-cluster](apache-spark-load-data-run-query.md), kunt u doorgaan naar stap 8 hieronder.
>

1. Klik vanaf het Startboard in [Azure Portal](https://portal.azure.com/) op de tegel voor uw Spark-cluster (als u deze aan het Startboard hebt vastgemaakt). U kunt ook naar uw cluster navigeren onder **Bladeren** > **HDInsight-clusters**.   

2. Klik vanuit de blade Spark-cluster op **Cluster-dashboard** en vervolgens op **Jupyter Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

   > [!NOTE]
   > Mogelijk bereikt u de Jupyter-notebook voor uw cluster ook door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Maak een notebook. Klik op **Nieuw** en klik vervolgens op **PySpark**.

    ![Een Jupyter-notebook maken voor Apache Spark BI](./media/apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "een Jupyter-notebook maken voor Apache Spark BI")

4. Er wordt een nieuwe notebook gemaakt en geopend met de naam Untitled.pynb. Klik bovenaan op de naam van de notebook en wijzig deze in een beschrijvende naam.

    ![Geef een naam voor de notebook voor Apache Spark BI](./media/apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "Geef een naam voor de notebook voor Apache Spark BI")

5. Omdat u de notebook met behulp van de PySpark-kernel hebt gemaakt, hoeft u niet expliciet contexten te maken. De Spark- en Hive-contexten worden automatisch voor u gemaakt wanneer u de eerste codecel uitvoert. Als eerste stap importeert u de typen die voor dit scenario zijn vereist. Om dit te doen, plaatst u de cursor in de cel en druk op **SHIFT + ENTER**.

        from pyspark.sql import *

6. Laad voorbeeldgegevens in een tijdelijke tabel. Wanneer u een Spark-cluster in HDInsight maakt, wordt het voorbeeldgegevensbestand, **hvac.csv**, gekopieerd naar het bijbehorende opslagaccount onder **\HdiSamples\HdiSamples\SensorSampleData\hvac**.

    Plak het volgende codefragment en druk op in een lege cel **SHIFT + ENTER**. In dit fragment registreert de gegevens in een tabel met de naam **hvac**.

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer the schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. Controleer of de tabel is gemaakt. U kunt de `%%sql` magic uitvoeren van Hive rechtstreeks query's. Voor meer informatie over de `%%sql`-magic, en andere magics die voor de PySpark-kernel beschikbaar zijn, raadpleegt u [Beschikbare kernels op Jupyter-notebooks met HDInsight Spark-clusters](apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SHOW TABLES

    Ziet u uitvoer zoals hieronder weergegeven:

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    Alleen de tabellen die zijn false onder de **isTemporary** kolom hive-tabellen die zijn opgeslagen in de metastore en toegankelijk is vanaf de BI-hulpprogramma's zijn. In deze zelfstudie we verbinding maken met de **hvac** tabel is gemaakt.

8. Controleer of de tabel bevat de gewenste gegevens. Kopieer het volgende codefragment en druk op in een lege cel in de notebook **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

9. Sluit de notebook om resources vrij te geven. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**.

## <a name="powerbi"></a>Gebruik Power BI voor Spark gegevensvisualisatie

> [!NOTE]
> Deze sectie geldt alleen voor Spark 1.6 op HDInsight 3.4 en Spark 2.0 op HDInsight 3.5.
>
>

Wanneer u de gegevens hebt opgeslagen als een tabel, kunt u Power BI verbinding maken met de gegevens en visualiseren om te maken van rapporten, dashboards, enzovoort.

1. Zorg ervoor dat u hebt toegang tot Power BI. U krijgt een abonnement gratis preview van Power BI van [http://www.powerbi.com/](http://www.powerbi.com/).

2. Aanmelden bij [Power BI](http://www.powerbi.com/).

3. Klik onder aan het linkerdeelvenster op **gegevens ophalen**.

4. Op de **gegevens ophalen** pagina onder **importeren of verbinding maken met gegevens**, voor **Databases**, klikt u op **ophalen**.

    ![Ophalen van gegevens in Power BI voor Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "gegevens in Power BI voor Apache Spark BI ophalen")

5. Klik op het volgende scherm **Spark op Azure HDInsight** en klik vervolgens op **Connect**. Wanneer u wordt gevraagd, typt u de cluster-URL (`mysparkcluster.azurehdinsight.net`) en de referenties voor verbinding met het cluster.

    ![Verbinding maken met Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "verbinding maken met Apache Spark BI")

    Nadat de verbinding is gemaakt, begint Power BI importeren van gegevens uit het Spark-cluster in HDInsight.

6. Power BI kunt u de gegevens importeren en voegt een **Spark** gegevensset onder de **gegevenssets** kop. Klik op de gegevensset te openen van een nieuw werkblad om de gegevens. U kunt ook het werkblad opslaan als een rapport. Opslaan van een werkblad van de **bestand** menu, klikt u op **opslaan**.

    ![Apache Spark BI-tegel in Power BI-dashboard](./media/apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI-tegel in Power BI-dashboard")
7. U ziet dat de **velden** lijst op de juiste lijsten met de **hvac** tabel die u eerder hebt gemaakt. Vouw in de tabel als de velden in de tabel wilt zien als u eerder in de notebook hebt gedefinieerd.

      ![Lijst met tabellen op Apache Spark BI-dashboard](./media/apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "lijst tabellen op de Apache Spark BI-dashboard")

8. Bouw een visualisatie in om het verschil tussen de doel-temperatuur- en werkelijke temperatuur voor elk gebouw weergeven. Als u wilt uw gegevens visualiseren, selecteer **vlakdiagram** (weergegeven in rood kader). Voor het definiëren van de as, slepen en neerzetten de **BuildingID** veld onder **as**, en **ActualTemp**/**TargetTemp** velden onder **waarde**.

    ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

9. Standaard bevat de visualisatie het totaal voor **ActualTemp** en **TargetTemp**. Voor zowel de velden in de vervolgkeuzelijst selecteert **gemiddelde** ophalen van een gemiddelde van het werkelijke en doel temperaturen voor beide gebouwen.

    ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

10. Uw gegevensvisualisatie moet vergelijkbaar zijn met die in de schermafbeelding. Plaats de cursor op de visualisatie knopinfo met relevante gegevens ophalen.

    ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

11. Klik op **opslaan** in het menu bovenaan en geef de rapportnaam van een. U kunt ook het visuele element vastmaken. Wanneer u een visualisatie vastmaken, wordt opgeslagen op uw dashboard zodat u de laatste waarde in een oogopslag kunt bijhouden.

   U kunt zoveel visualisaties als u voor dezelfde gegevensset wilt en aan het dashboard voor een overzicht van uw gegevens vastmaken kunt toevoegen. Bovendien Spark-clusters in HDInsight zijn verbonden met Power BI met direct connect. Dit zorgt ervoor dat Power BI altijd de meest recente gegevens van uw cluster heeft zodat u niet wilt vernieuwingen plannen voor de gegevensset.

## <a name="tableau"></a>Tableau bureaublad gebruiken voor Spark gegevensvisualisatie

> [!NOTE]
> Deze sectie geldt alleen voor 1.5.2 Spark-clusters die zijn gemaakt in Azure HDInsight.
>
>

1. Installeer [Tableau bureaublad](http://www.tableau.com/products/desktop) op de computer waarop u deze zelfstudie voor Apache Spark BI uitvoert.

2. Zorg ervoor dat deze computer ook Microsoft Spark ODBC-stuurprogramma geïnstalleerd. U kunt het stuurprogramma van [hier](http://go.microsoft.com/fwlink/?LinkId=616229).

1. Start Tableau bureaublad. Klik in het linkerdeelvenster, uit de lijst met de server verbinding wil maken, klikt u op **Spark SQL**. Als Spark SQL niet wordt weergegeven in het linkerdeelvenster standaard, kunt u deze vinden door te klikken op **meer Servers**.
2. Geef de waarden in het dialoogvenster Spark SQL-verbinding zoals weergegeven in de schermafbeelding en klik vervolgens op **OK**.

    ![Verbinding maken met een cluster voor Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "verbinding maken met een cluster voor Apache Spark BI")

    De vervolgkeuzelijsten verificatie **Microsoft Azure HDInsight-Service** als optie alleen als u hebt geïnstalleerd het [ODBC-stuurprogramma van Microsoft Spark](http://go.microsoft.com/fwlink/?LinkId=616229) op de computer.
3. Op het volgende scherm van de **Schema** omlaag, klik op de **vinden** pictogram en klik vervolgens op **standaard**.

    ![Schema voor Apache Spark BI vinden](./media/apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "schema voor Apache Spark BI zoeken")
4. Voor de **tabel** veld, klikt u op de **vinden** pictogram opnieuw naar de lijst met alle Hive-tabellen die beschikbaar zijn in het cluster. U ziet de **hvac** tabel die u eerder met de notebook gemaakt.

    ![Tabel niet vinden voor Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "tabel voor Apache Spark BI zoeken")
5. Slepen en neerzetten van de tabel aan het eerste vak aan de rechterkant. Tableau de gegevens worden geïmporteerd en geeft u het schema weer als gemarkeerde door een rood kader.

    ![Tabellen toevoegen aan Tableau voor Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "tabellen toevoegen aan Tableau voor Apache Spark BI")
6. Klik op de **Sheet1** tabblad linksonder. Controleer een visualisatie waarin de gemiddelde doel en de werkelijke temperaturen voor alle gebouwen voor elke datum. Sleep **datum** en **bouwen ID** naar **kolommen** en **werkelijke Temp**/**Temp doel** naar **rijen**. Onder **aanhalingstekens**, selecteer **gebied** voor het gebruik van een map gebied voor gegevensvisualisatie Spark.

     ![Toevoegen van velden voor Spark gegevensvisualisatie](./media/apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "velden voor Spark gegevensvisualisatie toevoegen")
7. Standaard de temperatuur velden worden weergegeven als de statistische functie. Als u weergeven van de gemiddelde temperatuur in plaats daarvan wilt, u kunt dit doen in de vervolgkeuzelijst, zoals hieronder wordt weergegeven.

    ![Gemiddelde temperatuur voor Spark gegevensvisualisatie nemen](./media/apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "gemiddelde temperatuur voor Spark gegevensvisualisatie duren")

8. U kunt ook super-opleggen één temperatuur toewijzing via de andere met een beter idee van verschil tussen het doel en de werkelijke temperaturen krijgen. Beweeg de muisaanwijzer naar de hoek van de lagere gebied kaart totdat u de ingang shape gemarkeerd in een rode cirkel ziet. Sleep de kaart naar de andere kaart bovenaan en de muisknop loslaat, wanneer u de vorm die is gemarkeerd in rode rechthoek ziet.

    ![Samenvoegen van kaarten voor Spark gegevensvisualisatie](./media/apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "samenvoegen toegewezen voor Spark gegevensvisualisatie")

     De weergave van uw gegevens moet wijzigen, zoals weergegeven in de schermafbeelding:

    ![Tableau uitvoer voor Spark gegevensvisualisatie](./media/apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau uitvoer voor Spark gegevensvisualisatie")
9. Klik op **opslaan** om op te slaan van het werkblad. U kunt dashboards maken en een of meer bladen aan toe te voegen.

## <a name="next-steps"></a>Volgende stappen

Tot nu toe hebt u geleerd hoe u een cluster maakt, Spark gegevensframes query uitvoeren op gegevens maken en vervolgens toegang tot die gegevens vanuit BI-tools. U kunt nu instructies voor het beheren van de clusterbronnen en taken die worden uitgevoerd in een HDInsight Spark-cluster voor foutopsporing bekijken.

* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

