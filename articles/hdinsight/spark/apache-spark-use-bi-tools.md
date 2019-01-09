---
title: 'Zelfstudie: Gegevens van Apache Spark analyseren met Power BI in Azure HDInsight '
description: Gebruik Microsoft Power BI om Apache Spark-gegevens te visualiseren die zijn opgeslagen in HDInsight-clusters
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,mvc
ms.topic: tutorial
ms.date: 05/07/2018
ms.openlocfilehash: c058cfa3bc9fa976726731cedeb80eb76a1f0810
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999268"
---
# <a name="tutorial-analyze-apache-spark-data-using-power-bi-in-hdinsight"></a>Zelfstudie: Gegevens van Apache Spark analyseren met Power BI in HDInsight 

Meer informatie over hoe u [Microsoft Power BI](https://powerbi.microsoft.com/) kunt gebruiken om gegevens te visualiseren in een [Apache Spark](https://spark.apache.org/)-cluster in [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/).

In deze zelfstudie leert u het volgende:
> [!div class="checklist"]
> * Spark-gegevens visualiseren met behulp van Power BI

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **Voltooi het artikel [Zelfstudie: Gegevens laden en query's uitvoeren in een Apache Spark-cluster in Azure HDInsight](./apache-spark-load-data-run-query.md)**.
* **Power BI**: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) en een [proefabonnement van Power BI](https://app.powerbi.com/signupredirect?pbi_source=web) (optioneel).


## <a name="verify-the-data"></a>De gegevens controleren

Het [Jupyter Notebook](https://jupyter.org/) dat u hebt gemaakt in de [vorige zelfstudie](apache-spark-load-data-run-query.md) bevat code voor het maken van een `hvac`-tabel. Deze tabel is gebaseerd op het CSV-bestand dat voor alle HDInsight Spark-clusters beschikbaar is op **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. Gebruik de volgende procedure om de gegevens te controleren.

1. Plak de volgende code uit het Jupyter-notebook en druk vervolgens op **Shift+Enter**. Deze code controleert of de tabellen bestaan.

    ```PySpark
    %%sql
    SHOW TABLES
    ```

    De uitvoer ziet er als volgt uit:

    ![Tabellen weergeven in Spark](./media/apache-spark-use-bi-tools/show-tables.png)

    Als u het notebook voorafgaand aan deze zelfstudie hebt gesloten, is `hvactemptable` opgeschoond en wordt deze niet opgenomen in de uitvoer.  Alleen Hive-tabellen die zijn opgeslagen in de metastore (aangegeven met **False** in de kolom **isTemporary**) zijn toegankelijk vanuit de BI-hulpprogramma's. In deze zelfstudie maakt u verbinding met de **hvac**-tabel die u hebt gemaakt.

2. Plak de volgende code in een lege cel en druk op **Shift+Enter**. De code controleert de gegevens in de tabel.

    ```PySpark
    %%sql
    SELECT * FROM hvac LIMIT 10
    ```

    De uitvoer ziet er als volgt uit:

    ![Rijen uit de hvac-tabel in Spark weergeven](./media/apache-spark-use-bi-tools/select-limit.png)

3. Klik in het menu **Bestand** in de notebook op **Sluiten en stoppen**. Sluit het notebook om de resources vrij te geven. 

## <a name="visualize-the-data"></a>De gegevens visualiseren

In dit gedeelte gebruikt u Power BI om visualisaties, rapporten en dashboards te maken van de gegevens in het Spark-cluster. 

### <a name="create-a-report-in-power-bi-desktop"></a>Een rapport maken in Power BI Desktop
De eerste stappen om te werken met Spark zijn verbinding maken met het cluster in Power BI Desktop, gegevens uit het cluster laden en eenvoudige visualisatie maken op basis van die gegevens.

> [!NOTE]  
> De connector die in dit artikel wordt gedemonstreerd is momenteel in preview. Eventuele feedback kunt u doorgeven via de site [Power BI-Community](https://community.powerbi.com/) of via [Power BI Ideas](https://ideas.powerbi.com/forums/265200-power-bi-ideas).

1. Open [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/).
1. Klik op het tabblad **Startpagina** op **Gegevens ophalen** en daarna op **Meer**.

    ![Gegevens van HDInsight Apache Spark overbrengen naar Power BI Desktop](./media/apache-spark-use-bi-tools/hdinsight-spark-power-bi-desktop-get-data.png "Gegevens van HDInsight Apache Spark overbrengen naar Power BI Desktop")


2. Typ `Spark` in het zoekvak, selecteer **Azure HDInsight Spark** en klik vervolgens op **Verbinding maken**.

    ![Gegevens van Apache Spark BI overbrengen naar Power BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Gegevens van Apache Spark BI overbrengen naar Power BI")

3. Voer de URL van het cluster (met de notatie `mysparkcluster.azurehdinsight.net`), selecteer **DirectQuery** en klik vervolgens op **OK**.

    U kunt beide gegevensverbindingsmodi gebruiken met Spark. Als u DirectQuery gebruikt, worden wijzigingen doorgevoerd in rapporten zonder dat de hele gegevensset wordt vernieuwd. Als u gegevens importeert, moet u de gegevensset vernieuwen om de wijzigingen te zien. Zie [DirectQuery gebruiken in Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/) voor meer informatie over hoe en wanneer u DirectQuery kunt gebruiken. 

4. Voer de gegevens voor het aanmeldingsaccount van HDInsight in en klik vervolgens op **Verbinding maken**. De standaardaccountnaam is *admin*.

5. Selecteer de tabel `hvac`, wacht tot u een voorbeeld van de gegevens ziet en klik dan op **Laden**.

    ![Gebruikersnaam en wachtwoord voor Spark-cluster](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Gebruikersnaam en wachtwoord voor Spark-cluster")

    Power BI Desktop beschikt over de gegevens die nodig zijn om verbinding te maken met het Spark-cluster en om gegevens te laden uit de tabel `hvac`. De tabel en de kolommen worden weergegeven in het deelvenster **Velden**.  Zie de volgende schermafbeelding:

6. Visualiseer het verschil tussen de gewenste temperatuur en de werkelijke temperatuur voor elk gebouw: 

    1. Selecteer **Vlakdiagram** in het deelvenster **Visualisaties**. 
    2. Sleep het veld **BuildingID** naar **As**, en sleep de velden **ActualTemp** en **TargetTemp** naar **Waarde**.

        ![Visualisaties van Spark-gegevens maken met behulp van Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Visualisaties van Spark-gegevens maken met behulp van Apache Spark BI")

        Het diagram ziet er zo uit:

        ![Visualisaties van Spark-gegevens maken met behulp van Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph-sum.png "Visualisaties van Spark-gegevens maken met behulp van Apache Spark BI")

        De visualisatie bevat standaard de som van **ActualTemp** en **TargetTemp**. Klik op de pijl-omlaag naast **ActualTemp** en **TragetTemp** in het deelvenster Visualisaties. U ziet dat **Som** is geselecteerd.

    3. Klik op de pijl omlaag naast **ActualTemp** en **TragetTemp** in het deelvenster Visualisaties, selecteer **Gemiddelde** om voor elk gebouw het gemiddelde weer te geven van de werkelijke temperatuur en de beoogde temperatuur.

        ![Visualisaties van Spark-gegevens maken met behulp van Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Visualisaties van Spark-gegevens maken met behulp van Apache Spark BI")

        De gegevensvisualisatie moet er ongeveer uitzien zoals in de schermafbeelding. Beweeg de cursor over de visualisatie om knopinfo met relevante gegevens weer te geven.

        ![Visualisaties van Spark-gegevens maken met behulp van Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Visualisaties van Spark-gegevens maken met behulp van Apache Spark BI")

7. Klik op **Bestand** en vervolgens op **Opslaan**, en voer de naam `BuildingTemperature.pbix` in voor het bestand. 

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Het rapport publiceren naar de Power BI-service (optioneel)

Met behulp van de Power BI-service kunt u rapporten en dashboards delen binnen uw organisatie. In dit gedeelte gaat u eerst de gegevensset en het rapport publiceren. Vervolgens maakt u het rapport vast aan een dashboard. Dashboards worden meestal gebruikt om te focussen op een subset van gegevens in een rapport. U hebt maar één visualisatie in uw rapport, maar het is toch handig om even de stappen te doorlopen.

1. Open Power BI Desktop.
2. Klik op het tabblad **Startpagina** op **Publiceren**.

    ![Publiceren vanuit Power BI Desktop](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "Publiceren vanuit Power BI Desktop")

2. Selecteer de werkruimte waarnaar u de gegevensset wilt publiceren en rapporteren, en klik vervolgens op **Selecteren**. In de volgende afbeelding is de standaardwerkruimte **Mijn werkruimte** geselecteerd.

    ![Werkruimte selecteren voor publiceren en rapporteren van gegevensset](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "Werkruimte selecteren voor publiceren en rapporteren van gegevensset") 

3. Nadat het publiceren is voltooid, klikt u op **'BuildingTemperature.pbix' openen in Power BI**.

    ![Publiceren voltooid, klikken om referenties in te voeren](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "Publiceren voltooid, klikken om referenties in te voeren") 

4. Klik in de Power BI-service op **Referenties invoeren**.

    ![Referenties invoeren in Power BI-service](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Referenties invoeren in Power BI-service")

5. Klik op **Referenties bewerken**.

    ![Referenties bewerken in Power BI-service](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "Referenties bewerken in Power BI-service")

6. Voer de gegevens voor het aanmeldingsaccount van HDInsight in en klik vervolgens op **Aanmelden**. De standaardaccountnaam is *admin*.

    ![Aanmelden bij Spark-cluster](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "Aanmelden bij Spark-cluster")

7. Ga in het linkerdeelvenster naar **Werkruimten** > **Mijn werkruimte** > **RAPPORTEN** en klik op **BuildingTemperature**.

    ![Rapport vermeld onder Rapporten in het linkerdeelvenster](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "Rapport vermeld onder Rapporten in het linkerdeelvenster")

    Ook moet **BuildingTemperature** worden vermeld **GEGEVENSSETS** in het linkerdeelvenster.

    De visualisatie die u hebt gemaakt in Power BI Desktop is nu beschikbaar in de Power BI-service. 

8. Beweeg de cursor over de visualisatie en klik vervolgens op de speld in de rechterbovenhoek.

    ![Rapport in de Power BI-service](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "Rapport in de Power BI-service")

9. Selecteer 'Nieuw dashboard', voer de naam `Building temperature` in en klik vervolgens op **Vastmaken**.

    ![Vastmaken aan nieuw dashboard](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "Vastmaken aan nieuw dashboard")

10. Klik in het rapport op **Naar dashboard**. 

De visualisatie wordt vastgemaakt aan het dashboard. U kunt andere visualisaties toevoegen aan het rapport en deze aan hetzelfde dashboard vastmaken. Zie [Rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/) en [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/) voor meer informatie over rapporten en dashboards.

<!--
## <a name="tableau"></a>Use Tableau Desktop 

> [!NOTE]
> This section is applicable only for Spark 1.5.2 clusters created in Azure HDInsight.
>
>

1. Install [Tableau Desktop](https://www.tableau.com/products/desktop) on the computer where you are running this Apache Spark BI tutorial.

2. Make sure that computer also has Microsoft Spark ODBC driver installed. You can install the driver from [here](https://go.microsoft.com/fwlink/?LinkId=616229).

1. Launch Tableau Desktop. In the left pane, from the list of server to connect to, click **Spark SQL**. If Spark SQL is not listed by default in the left pane, you can find it by click **More Servers**.
2. In the Spark SQL connection dialog box, provide the values as shown in the screenshot, and then click **OK**.

    ![Connect to a cluster for Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Connect to a cluster for Apache Spark BI")

    The authentication drop-down lists **Microsoft Azure HDInsight Service** as an option, only if you installed the [Microsoft Spark ODBC Driver](https://go.microsoft.com/fwlink/?LinkId=616229) on the computer.
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

In deze zelfstudie heeft u het volgende geleerd:

- Apache Spark-gegevens visualiseren met behulp van Power BI.

Ga naar het volgende artikel om te zien hoe de gegevens die u hebt geregistreerd in Spark kunnen worden overgebracht naar een BI-hulpprogramma voor analyse zoals Power BI. 
> [!div class="nextstepaction"]
> [Een Apache Spark-streamingtaak uitvoeren](apache-spark-eventhub-streaming.md)

