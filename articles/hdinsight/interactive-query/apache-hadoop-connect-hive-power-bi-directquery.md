---
title: Interactive Query Hive-gegevens visualiseren met Power BI in Azure HDInsight
description: Microsoft Power BI gebruiken om gegevens uit Azure HDInsight Interactive Query Hive te visualiseren
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/25/2018
ms.openlocfilehash: d9639a4a116e06e17005ebddbb26379882491b33
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56867827"
---
# <a name="visualize-interactive-query-apache-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Interactieve Query Apache Hive-gegevens visualiseren met Microsoft Power BI met behulp van directquery in Azure HDInsight

In dit artikel wordt beschreven hoe u Microsoft Power BI verbinden met Azure HDInsight Interactive Query-clusters en Apache Hive-gegevens met behulp van directe query visualiseren. Het opgegeven voorbeeld laadt de gegevens uit een `hivesampletable` Hive-tabel naar Power BI. De `hivesampletable` Hive-tabel bevat de gebruiksgegevens van sommige mobiele telefoon. Vervolgens kunt u gegevens over gebruik op een wereldkaart brengt:

![HDInsight Power BI het rapport met kaart](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

U kunt gebruikmaken van de [Apache Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-hive-power-bi.md) via de algemene ODBC-connector in Power BI Desktop importeren. Het is echter niet aanbevolen voor BI workloads vanwege niet-interactieve aard van de Hive-query-engine. [HDInsight Interactive Query-connector](./apache-hadoop-connect-hive-power-bi-directquery.md) en [HDInsight Apache Spark-connector](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) betere mogelijkheden voor de prestaties zijn.

## <a name="prerequisites"></a>Vereisten
Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **HDInsight-cluster**. Het cluster kan zijn op een HDInsight-cluster met Apache Hive of een nieuw uitgebrachte Interactive Query-cluster. Zie voor het maken van clusters, [-cluster maken](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. U kunt een kopie van downloaden de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Laden van gegevens uit HDInsight

De `hivesampletable` Hive-tabel wordt geleverd met alle HDInsight-clusters.

1. Start Power BI Desktop.

2. In de menubalk, gaat u naar **Start** > **gegevens ophalen** > **meer...** .

    ![Open HDInsight Power BI-gegevens](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)

3. Uit de **gegevens ophalen** venster, geef **hdinsight** in het zoekvak in.  

4. Selecteer in de lijst met zoekresultaten **HDInsight Interactive Query**, en selecteer vervolgens **Connect**.  Als er geen **HDInsight Interactive Query**, moet u uw Power BI Desktop bijwerken naar de nieuwste versie.

5. Selecteer **doorgaan** sluiten de **verbinding maken met een service van derden** dialoogvenster.

6. In de **HDInsight Interactive Query** venster, voer de volgende informatie op en selecteer vervolgens **OK**:

    |Eigenschap | Waarde |
    |---|---|
    |Server |Voer de naam van het cluster, bijvoorbeeld *myiqcluster.azurehdinsight.net*.|
    |Database |Voer **standaard** voor dit artikel.|
    |Gegevensverbindingsmodus |Selecteer **DirectQuery** voor dit artikel.|

    ![HDInsight interactive query Power BI DirectQuery-verbinding](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)

7. Voer de referenties van de HTTP- en selecteer vervolgens **Connect**. De standaardnaam van de gebruiker is **admin**.

8. Uit de **Navigator** venster in het linkerdeelvenster, selecteer **hivesampletale**.

9. Selecteer **Load** vanuit het hoofdvenster.

    ![HDInsight interactive query Power BI hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data-on-a-map"></a>Visualiseer gegevens op een kaart

Volgen op de laatste procedure.

1. Selecteer in het deelvenster visualisaties **kaart**, het pictogram voor de hele wereld. Een algemene kaart wordt vervolgens weergegeven in het hoofdvenster.

    ![HDInsight Power BI worden aangepast rapport](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)

2. Selecteer in het deelvenster velden **land** en **devicemake**. Een wereldkaart met de gegevenspunten wordt weergegeven in het hoofdvenster na een paar seconden.

3. Vouw de kaart.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u gegevens uit HDInsight met behulp van Microsoft Power BI visualiseren.  Zie de volgende artikelen voor meer informatie over gegevensvisualisatie:

* [Apache Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight met behulp van ODBC](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Apache Zeppelin gebruiken voor het Apache Hive-query's uitvoeren in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel verbinden met Apache Hadoop met Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en Apache Hive-query's uitvoeren met behulp van Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).
