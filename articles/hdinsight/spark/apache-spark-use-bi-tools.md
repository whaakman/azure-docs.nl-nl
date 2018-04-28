---
title: Met behulp van de hulpmiddelen voor gegevensvisualisatie in Azure HDInsight Spark-BI | Microsoft Docs
description: Hulpmiddelen voor gegevensvisualisatie voor analyses met Apache Spark BI op HDInsight-clusters gebruiken
keywords: Apache spark bi, spark bi, spark gegevensvisualisatie, spark business intelligence
services: hdinsight
documentationcenter: ''
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: conceptual
ms.date: 02/14/2018
ms.author: jgao
ms.openlocfilehash: 0e728e17a64acd990b301bac8139c7bb395a3098
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2018
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI hulpmiddelen voor gegevensvisualisatie gebruiken met Azure HDInsight

Informatie over het gebruik [Microsoft Power BI](http://powerbi.microsoft.com) om gegevens in Apache Spark-cluster in Azure HDInsight te visualiseren.

## <a name="prerequisites"></a>Vereisten

* **Voltooien van het artikel [interactieve query's uitvoeren op Spark-clusters in HDInsight](./apache-spark-load-data-run-query.md)**.
* **Power BI**: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) en [Power BI-proefabonnement](https://app.powerbi.com/signupredirect?pbi_source=web) (optioneel).


## <a name="hivetable"></a>Controleer of de gegevens

De Jupyter-notebook die u hebt gemaakt in de [vorige zelfstudie](apache-spark-load-data-run-query.md) code maken bevat een `hvac` tabel. Deze tabel is gebaseerd op het CSV-bestand beschikbaar op alle HDInsight Spark-clusters op **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Gebruik de volgende procedure om te controleren of de gegevens.

1. Plak de volgende code van de Jupyter-notebook en druk vervolgens op **SHIFT + ENTER**. De code wordt gecontroleerd of het bestaan van de tabellen.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    De uitvoer ziet er als volgt uit:

    ![Tabellen in Spark weergeven](./media/apache-spark-use-bi-tools/show-tables.png)

    Als u de notebook afgesloten voordat u begint in deze zelfstudie `hvactemptable` wordt opgeschoond, zodat deze niet in de uitvoer opgenomen wordt.
    Alleen Hive-tabellen die zijn opgeslagen in de metastore (aangeduid door **False** onder de **isTemporary** kolom) toegankelijk is vanaf de BI-hulpprogramma's. In deze zelfstudie maakt u verbinding maken met de **hvac** tabel die u hebt gemaakt.

2. Plak de volgende code in een lege cel en druk vervolgens op **SHIFT + ENTER**. De code controleert of de gegevens in de tabel.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    De uitvoer ziet er als volgt uit:

    ![Rijen uit de tabel hvac in Spark weergeven](./media/apache-spark-use-bi-tools/select-limit.png)

3. Klik in het menu **Bestand** in de notebook op **Sluiten en stoppen**. Sluit de notebook om resources vrij te geven. 















## <a name="powerbi"></a>Power BI gebruiken

In deze sectie kunt u Power BI visualisaties, rapporten en dashboards te maken van de gegevens van Spark-cluster. 

### <a name="create-a-report-in-power-bi-desktop"></a>Een rapport maken in Power BI Desktop
De eerste stap bij het werken met Spark zijn verbinding maken met het cluster in Power BI Desktop, het laden van gegevens uit het cluster en het maken van een basic visualisatie op basis van die gegevens.

> [!NOTE]
> De connector die in dit artikel is momenteel in preview. Geef feedback die u via hebt de [Power BI-Community](https://community.powerbi.com/) site of [Power BI ideeën](https://ideas.powerbi.com/forums/265200-power-bi-ideas).

1. Open [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).
1. Van de **Start** tabblad **gegevens ophalen**, klikt u vervolgens **meer**.

    ![Gegevens in Power BI Desktop ophalen uit HDInsight Apache Spark](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "gegevens in Power BI van Apache Spark BI ophalen")


2. Voer `Spark` selecteren in het zoekvak **Azure HDInsight Spark (bèta)**, en klik vervolgens op **Connect**.

    ![Gegevens in Power BI ophalen van Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "gegevens in Power BI van Apache Spark BI ophalen")

3. Voer de URL van uw cluster (in de vorm `mysparkcluster.azurehdinsight.net`), selecteer **DirectQuery**, en klik vervolgens op **OK**.

    U kunt de connectiviteitsmodus gegevens gebruiken met Spark. Als u DirectQuery, wijzigingen in rapporten zonder de volledige gegevensset te vernieuwen. Als u gegevens importeert, moet u de gegevensset om wijzigingen weer te vernieuwen. Zie voor meer informatie over hoe en wanneer met DirectQuery [met behulp van DirectQuery in Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/). 

4. Voer het hulpprogramma voor het account van de HDInsight-aanmeldingsgegevens en klik vervolgens op **Connect**. De naam van de standaardwaarde is *admin*.

5. Selecteer de `hvac` tabel, wacht om een voorbeeld van de gegevens te bekijken en klik vervolgens op **Load**.

    ![Cluster-gebruikersnaam en wachtwoord Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark cluster-gebruikersnaam en wachtwoord")

    Power BI Desktop heeft de benodigde gegevens voor het verbinding maken met de gegevens voor Spark-cluster en de belasting van de `hvac` tabel. De tabel en de kolommen worden weergegeven in de **velden** deelvenster.  Zie de volgende schermafbeelding:

6. Het verschil tussen de doel-temperatuur- en werkelijke temperatuur voor elk gebouw visualiseren: 

    1. In de **VISUALISATIES** deelvenster **vlakdiagram**. 
    2. Sleep de **BuildingID** veld **as**, en sleep de **ActualTemp** en **TargetTemp** velden **waarde**.

        ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

        Het diagram ziet eruit als:

        ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

        Standaard bevat de visualisatie het totaal voor **ActualTemp** en **TargetTemp**. Klik op de pijl-omlaag naast **ActualTemp** en **TragetTemp** in het deelvenster visualisaties ziet u **som** is geselecteerd.

    3. Klik op de pijl omlaag naast **ActualTemp** en **TragetTemp** selecteert in het deelvenster visualisaties **gemiddelde** ophalen van een gemiddelde van het werkelijke en doel temperaturen voor elk opbouwen.

        ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

        De weergave van uw gegevens zijn vergelijkbaar met de structuur in de schermafbeelding. Plaats de cursor op de visualisatie knopinfo met relevante gegevens ophalen.

        ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

7. Klik op **bestand** vervolgens **opslaan**, en voer de naam `BuildingTemperature.pbix` voor het bestand. 

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Het rapport publiceren naar het Power BI-Service (optioneel)

De Power BI-service kunt u rapporten en dashboards delen in uw organisatie. In deze sectie maakt publiceren u eerst de gegevensset en het rapport. Vervolgens kunt u het rapport aan een dashboard vastmaken. Dashboards worden meestal gebruikt om zich te richten op een subset van gegevens in een rapport; hebt u slechts één visualisatie in uw rapport, maar is het nog steeds nuttig om de stappen doorlopen.

1. Power BI Desktop openen.
2. Van de **Start** tabblad **publiceren**.

    ![Publiceren van Power BI Desktop](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "vanuit Power BI Desktop publiceren")

2. Selecteer een werkruimte voor het publiceren van uw gegevensset en rapporteren aan, klikt u vervolgens op **Selecteer**. In de volgende afbeelding, de standaard **mijn werkruimte** is geselecteerd.

    ![Selecteer werkruimte gegevensset publiceren en rapporteren aan](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "gegevensset publiceren en te rapporteren aan Select-werkruimte") 

3. Nadat het publiceren is voltooid, klikt u op **Open 'BuildingTemperature.pbix' in Power BI**.

    ![Geslaagde publiceren, klikt u referenties invoeren](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "publiceren geslaagd, klikt u referenties invoeren") 

4. Klik in de Power BI-service op **referenties invoeren**.

    ![Voer de referenties in Power BI-service](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Power BI-service moet u referenties invoeren")

5. Klik op **bewerken van referenties**.

    ![De referenties in Power BI-service bewerken](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "bewerken van referenties in Power BI-service")

6. Voer het hulpprogramma voor het account van de HDInsight-aanmeldingsgegevens en klik vervolgens op **aanmelden**. De naam van de standaardwaarde is *admin*.

    ![Aanmelden bij Spark-cluster](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "aanmelden bij Spark-cluster")

7. Ga in het linkerdeelvenster naar **werkruimten** > **mijn werkruimte** > **rapporten**, klikt u vervolgens op **BuildingTemperature**.

    ![Rapport vermeld in de rapporten in het linkerdeelvenster](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "rapport vermeld in de rapporten in het linkerdeelvenster")

    U ziet ook **BuildingTemperature** vermeld onder **GEGEVENSSETS** in het linkerdeelvenster.

    Het visuele element die u hebt gemaakt in Power BI Desktop is nu beschikbaar in de Power BI-service. 

8. Houd de cursor boven de visualisatie en klik vervolgens op het Punaisepictogram op de rechterbovenhoek.

    ![Rapport in Power BI-service](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "rapport in Power BI-service")

9. Selecteer 'Nieuwe dashboard', voer de naam `Building temperature`, klikt u vervolgens op **pincode**.

    ![Aan het nieuwe dashboard vastmaken](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "aan het nieuwe dashboard vastmaken")

10. Klik in het rapport op **gaat u naar dashboard**. 

Het visuele element is vastgemaakt aan het dashboard - kunt u andere visuele elementen toevoegen aan het rapport en ze vastmaken aan het dashboard. Zie voor meer informatie over rapporten en dashboards [rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/)en [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

<!--
## <a name="tableau"></a>Use Tableau Desktop 

> [!NOTE]
> This section is applicable only for Spark 1.5.2 clusters created in Azure HDInsight.
>
>

1. Install [Tableau Desktop](http://www.tableau.com/products/desktop) on the computer where you are running this Apache Spark BI tutorial.

2. Make sure that computer also has Microsoft Spark ODBC driver installed. You can install the driver from [here](http://go.microsoft.com/fwlink/?LinkId=616229).

1. Launch Tableau Desktop. In the left pane, from the list of server to connect to, click **Spark SQL**. If Spark SQL is not listed by default in the left pane, you can find it by click **More Servers**.
2. In the Spark SQL connection dialog box, provide the values as shown in the screenshot, and then click **OK**.

    ![Connect to a cluster for Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect to a cluster for Apache Spark BI")

    The authentication drop-down lists **Microsoft Azure HDInsight Service** as an option, only if you installed the [Microsoft Spark ODBC Driver](http://go.microsoft.com/fwlink/?LinkId=616229) on the computer.
3. On the next screen, from the **Schema** drop-down, click the **Find** icon, and then click **default**.

    ![Find schema for Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Find schema for Apache Spark BI")
4. For the **Table** field, click the **Find** icon again to list all the Hive tables available in the cluster. You should see the **hvac** table you created earlier using the notebook.

    ![Find table for Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Find table for Apache Spark BI")
5. Drag and drop the table to the top box on the right. Tableau imports the data and displays the schema as highlighted by the red box.

    ![Add tables to Tableau for Apache Spark BI](./media/apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Add tables to Tableau for Apache Spark BI")
6. Click the **Sheet1** tab at the bottom left. Make a visualization that shows the average target and actual temperatures for all buildings for each date. Drag **Date** and **Building ID** to **Columns** and **Actual Temp**/**Target Temp** to **Rows**. Under **Marks**, select **Area** to use an area map for Spark data visualization.

     ![Add fields for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Add fields for Spark data visualization")
7. By default, the temperature fields are shown as aggregate. If you want to show the average temperatures instead, you can do so from the drop-down, as shown in the following screenshot:

    ![Take average of temperature for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Take average of temperature for Spark data visualization")

8. You can also super-impose one temperature map over the other to get a better feel of difference between target and actual temperatures. Move the mouse to the corner of the lower area map until you see the handle shape highlighted in a red circle. Drag the map to the other map on the top and release the mouse when you see the shape highlighted in red rectangle.

    ![Merge maps for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Merge maps for Spark data visualization")

     Your data visualization should change as shown in the screenshot:

    ![Tableau output for Spark data visualization](./media/apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau output for Spark data visualization")
9. Click **Save** to save the worksheet. You can create dashboards and add one or more sheets to it.
-->

## <a name="next-steps"></a>Volgende stappen

Tot nu toe hebt u geleerd hoe u een cluster maakt, Spark gegevensframes query uitvoeren op gegevens maken en vervolgens toegang tot die gegevens vanuit BI-tools. U kunt nu instructies voor het beheren van de clusterbronnen en taken die worden uitgevoerd in een HDInsight Spark-cluster voor foutopsporing bekijken.

* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

