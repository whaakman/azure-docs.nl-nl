---
title: Met behulp van de hulpmiddelen voor gegevensvisualisatie in Azure HDInsight Spark-BI | Microsoft Docs
description: Hulpmiddelen voor gegevensvisualisatie voor analyses met Apache Spark BI op HDInsight-clusters gebruiken
keywords: Apache spark bi, spark bi, spark gegevensvisualisatie, spark business intelligence
services: hdinsight
documentationcenter: 
author: mumian
manager: cgronlun
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2017
ms.author: jgao
ms.openlocfilehash: fabf48da80cf44e82068d180c896ebbca7078d18
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI hulpmiddelen voor gegevensvisualisatie gebruiken met Azure HDInsight

Informatie over het gebruik van Power BI en Tableau om gegevens in een Apache Spark-cluster in Azure HDInsight te visualiseren.

## <a name="prerequisites"></a>Vereisten

* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).
* Voorbeeldgegevens in het cluster. Zie voor instructies [interactieve query's uitvoeren op een HDInsight Spark-cluster](apache-spark-load-data-run-query.md).
* Power BI: [Power BI Desktop](https://powerbi.microsoft.com/en-us/desktop/) en [Power BI-proefabonnement](https://app.powerbi.com/signupredirect?pbi_source=web) (optioneel).
* Tableau: [Tableau bureaublad](http://www.tableau.com/products/desktop) en [Spark ODBC-stuurprogramma](http://go.microsoft.com/fwlink/?LinkId=616229).


## <a name="hivetable"></a>Voorbeeldgegevens controleren

De Jupyter-notebook die u hebt gemaakt in de [vorige zelfstudie](apache-spark-load-data-run-query.md) code maken bevat een `hvac` tabel. Deze tabel is gebaseerd op het CSV-bestand beschikbaar op alle HDInsight Spark-clusters op **\HdiSamples\HdiSamples\SensorSampleData\hvac\hvac.csv**. We bekijken de gegevens in het Spark-cluster voordat u visualisaties maakt.

1. Controleer of de verwachte tabellen bestaan. Kopieer het volgende codefragment en druk op in een lege cel in de notebook **SHIFT + ENTER**.

        %%sql
        SHOW TABLES

    Ziet u uitvoer zoals hieronder weergegeven:

    ![Tabellen in Spark weergeven](./media/apache-spark-use-bi-tools/show-tables.png)

    Als u de notebook afgesloten voordat u begint in deze zelfstudie `hvactemptable` wordt opgeschoond, zodat deze niet in de uitvoer opgenomen wordt.
    Alleen hive-tabellen die zijn opgeslagen in de metastore (aangeduid door **False** onder de **isTemporary** kolom) toegankelijk is vanaf de BI-hulpprogramma's. In deze zelfstudie we verbinding maken met de **hvac** tabel is gemaakt.

2. Controleer of de tabel bevat de verwachte gegevens. Kopieer het volgende codefragment en druk op in een lege cel in de notebook **SHIFT + ENTER**.

        %%sql
        SELECT * FROM hvac LIMIT 10

    Ziet u uitvoer zoals hieronder weergegeven:

    ![Rijen uit de tabel hvac in Spark weergeven](./media/apache-spark-use-bi-tools/select-limit.png)

3. Sluit de notebook om resources vrij te geven. Dit doet u door in het menu **Bestand** in de notebook te klikken op **Sluiten en stoppen**.

## <a name="powerbi"></a>Gebruik Power BI voor Spark gegevensvisualisatie

Nu dat u hebt geverifieerd dat de verwachte gegevens bestaat, kunt u Power BI visualisaties, rapporten en dashboards te maken van deze gegevens. In dit artikel hebt we een eenvoudig voorbeeld met statische gegevens weergeven, maar laat ons weten in de opmerkingen als u complexere streaming voorbeelden.

### <a name="create-a-report-in-power-bi-desktop"></a>Een rapport maken in Power BI Desktop
De eerste stap bij het werken met Spark zijn verbinding maken met het cluster in Power BI Desktop, het laden van gegevens uit het cluster en het maken van een basic visualisatie op basis van die gegevens.

> [!NOTE]
> De connector die in dit artikel is momenteel in preview, maar we raden u aan deze gebruiken en eventuele feedback hebt u via de [Power BI-Community](https://community.powerbi.com/) site of [Power BI ideeën](https://ideas.powerbi.com/forums/265200-power-bi-ideas).

1. In Power BI Desktop op de **Start** tabblad **gegevens ophalen**, klikt u vervolgens **meer**.

2. Zoeken naar `Spark`, selecteer **Azure HDInsight Spark**, klikt u vervolgens op **Connect**.

    ![Gegevens in Power BI ophalen van Apache Spark BI](./media/apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "gegevens in Power BI van Apache Spark BI ophalen")

3. Voer de URL van uw cluster (in de vorm `mysparkcluster.azurehdinsight.net`), selecteer **DirectQuery**, klikt u vervolgens op **OK**.

    ![Verbinding maken met Apache Spark BI](./media/apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "verbinding maken met Apache Spark BI")

    > [!NOTE]
    > U kunt de modus voor verbinding met Spark. Als u DirectQuery, wijzigingen in rapporten zonder de volledige gegevensset te vernieuwen. Als u gegevens importeert, moet u de gegevensset om wijzigingen weer te vernieuwen. Zie voor meer informatie over hoe en wanneer met DirectQuery [met behulp van DirectQuery in Power BI](https://powerbi.microsoft.com/documentation/powerbi-desktop-directquery-about/). 

4. Geef de gegevens op HDInsight aanmelding voor **gebruikersnaam** en **wachtwoord** (het standaardaccount is `admin`), klikt u vervolgens op **Connect**.

    ![Cluster-gebruikersnaam en wachtwoord Spark](./media/apache-spark-use-bi-tools/user-password.png "Spark cluster-gebruikersnaam en wachtwoord")

5. Selecteer de `hvac` tabel en wacht om een voorbeeld van de gegevens te bekijken. Klik vervolgens op **Load**.

    ![Cluster-gebruikersnaam en wachtwoord Spark](./media/apache-spark-use-bi-tools/apache-spark-bi-select-table.png "Spark cluster-gebruikersnaam en wachtwoord")

    Power BI Desktop heeft nu alle informatie die nodig is verbinding maken met de gegevens voor Spark-cluster en de belasting van de `hvac` tabel. De tabel en de kolommen worden weergegeven in de **velden** deelvenster.

    ![Lijst met tabellen op Apache Spark BI-dashboard](./media/apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "lijst tabellen op de Apache Spark BI-dashboard")

7. Bouw een visualisatie in om het verschil tussen de doel-temperatuur- en werkelijke temperatuur voor elk gebouw weergeven: 

    1. In de **VISUALISATIES** deelvenster **vlakdiagram**. Sleep de **BuildingID** veld **as**, en sleep de **ActualTemp** en **TargetTemp** velden **waarde**.

        ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

    2. Standaard bevat de visualisatie het totaal voor **ActualTemp** en **TargetTemp**. Voor beide velden in de vervolgkeuzelijst selecteert **gemiddelde** ophalen van een gemiddelde van het werkelijke en doel temperaturen voor elk gebouw.

        ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

    3. Uw gegevensvisualisatie moet vergelijkbaar zijn met die in de schermafbeelding. Plaats de cursor op de visualisatie knopinfo met relevante gegevens ophalen.

        ![Spark maken met behulp van Apache Spark BI gegevensvisualisaties](./media/apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "gegevensvisualisaties Spark maken met behulp van Apache Spark BI")

11. Klik op **bestand** vervolgens **opslaan**, en voer de naam `spark.pbix` voor het bestand. 

### <a name="publish-the-report-to-the-power-bi-service-optional"></a>Het rapport publiceren naar het Power BI-Service (optioneel)
U hebt nu een volledig functionele rapport in Power BI Desktop, en kunt u er stoppen, maar veel mensen willen profiteren van de Power BI-service, waardoor u eenvoudig rapporten en dashboards delen in uw organisatie. 

In deze sectie publiceert u de gegevensset en het rapport dat is opgenomen in het Power BI Desktop-bestand dat u hebt gemaakt. U vervolgens vastmaken de visualisatie uit het rapport aan een dashboard. Dashboards worden meestal gebruikt om zich te richten op een subset van gegevens in een rapport; hebt u slechts één visualisatie in uw rapport, maar is het nog steeds nuttig om de stappen doorlopen.

1. In Power BI Desktop op de **Start** tabblad **publiceren**.

    ![Publiceren van Power BI Desktop](./media/apache-spark-use-bi-tools/apache-spark-bi-publish.png "vanuit Power BI Desktop publiceren")

2. Selecteer een werkruimte voor het publiceren van uw gegevensset en rapporteren aan, klikt u vervolgens op **Selecteer**. In de volgende afbeelding, de standaard **mijn werkruimte** is geselecteerd.

    ![Selecteer werkruimte gegevensset publiceren en rapporteren aan](./media/apache-spark-use-bi-tools/apache-spark-bi-select-workspace.png "gegevensset publiceren en te rapporteren aan Select-werkruimte") 

3. Nadat de Power BI Desktop geretourneerd met een voltooiingsbericht te zien, klikt u op **'spark.pbix' openen in Power BI**.

    ![Geslaagde publiceren, klikt u referenties invoeren](./media/apache-spark-use-bi-tools/apache-spark-bi-publish-success.png "publiceren geslaagd, klikt u referenties invoeren") 

4. Klik in de Power BI-service op **referenties invoeren**.

    ![Voer de referenties in Power BI-service](./media/apache-spark-use-bi-tools/apache-spark-bi-enter-credentials.png "Power BI-service moet u referenties invoeren")

5. Klik op **bewerken van referenties**.

    ![De referenties in Power BI-service bewerken](./media/apache-spark-use-bi-tools/apache-spark-bi-edit-credentials.png "bewerken van referenties in Power BI-service")

6. Voer de accountgegevens van de HDInsight-aanmelding (doorgaans de standaardinstelling `admin` account die u in Power BI Desktop gebruikt), klikt u vervolgens op **aanmelden**.

    ![Aanmelden bij Spark-cluster](./media/apache-spark-use-bi-tools/apache-spark-bi-sign-in.png "aanmelden bij Spark-cluster")

7. Ga in het linkerdeelvenster naar **werkruimten** > **mijn werkruimte** > **rapporten**, klikt u vervolgens op **spark**.

    ![Rapport vermeld in de rapporten in het linkerdeelvenster](./media/apache-spark-use-bi-tools/apache-spark-bi-service-left-pane.png "rapport vermeld in de rapporten in het linkerdeelvenster")

    U ziet ook **spark** vermeld onder **GEGEVENSSETS** in het linkerdeelvenster.

8. Het visuele element die u hebt gemaakt in Power BI Desktop is nu beschikbaar in de service. Beweeg de muisaanwijzer over het visuele element voor dit visuele element aan een dashboard vastmaken, en klik op het pictogram pincode.

    ![Rapport in Power BI-service](./media/apache-spark-use-bi-tools/apache-spark-bi-service-report.png "rapport in Power BI-service")

9. Selecteer 'Nieuwe dashboard', voer de naam `SparkDemo`, klikt u vervolgens op **pincode**.

    ![Aan het nieuwe dashboard vastmaken](./media/apache-spark-use-bi-tools/apache-spark-bi-pin-dashboard.png "aan het nieuwe dashboard vastmaken")

10. Klik in het rapport op **gaat u naar dashboard**. 

    ![Ga naar het dashboard](./media/apache-spark-use-bi-tools/apache-spark-bi-open-dashboard.png "gaat u naar dashboard")

Het visuele element is vastgemaakt aan het dashboard - kunt u andere visuele elementen toevoegen aan het rapport en ze vastmaken aan het dashboard. Zie voor meer informatie over rapporten en dashboards [rapporten in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-reports/)en [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

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

