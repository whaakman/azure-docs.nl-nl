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
ms.date: 11/06/2018
ms.openlocfilehash: 68f2314b995eb0f2f67307b44cbfd177f5c5a796
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52309267"
---
# <a name="visualize-interactive-query-apache-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Interactieve Query Apache Hive-gegevens visualiseren met Microsoft Power BI met behulp van directquery in Azure HDInsight

In dit artikel wordt beschreven hoe u Microsoft Power BI verbinden met Azure HDInsight Interactive Query-clusters en Apache Hive-gegevens met behulp van directe query visualiseren. Het opgegeven voorbeeld laadt de gegevens van een hivesampletable Hive-tabel naar Power BI. De hivesampletable Hive-tabel bevat de gebruiksgegevens van sommige mobiele telefoon. Vervolgens kunt u gegevens over gebruik op een wereldkaart brengt:

![HDInsight Power BI het rapport met kaart](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

U kunt gebruikmaken van de [Apache Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-hive-power-bi.md) via de algemene ODBC-connector in Power BI Desktop importeren. Het is echter niet aanbevolen voor BI workloads vanwege niet-interactieve aard van de Hive-query-engine. [HDInsight Interactive Query-connector](./apache-hadoop-connect-hive-power-bi-directquery.md) en [HDInsight Apache Spark-connector](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) betere mogelijkheden voor de prestaties zijn.

## <a name="prerequisites"></a>Vereisten
Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **HDInsight-cluster**. Het cluster kan zijn op een HDInsight-cluster met Apache Hive of een nieuw uitgebrachte Interactive Query-cluster. Zie voor het maken van clusters, [-cluster maken](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. U kunt een kopie van downloaden de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Laden van gegevens uit HDInsight

De Hive-tabel hivesampletable wordt geleverd met alle HDInsight-clusters.

1. Aanmelden bij Power BI Desktop.

2. Klik op de **Start** tabblad **gegevens ophalen** uit de **externe gegevens** lint en selecteer vervolgens **meer...** .

    ![Open HDInsight Power BI-gegevens](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
    
3. Uit de **gegevens ophalen** deelvenster, type **hdinsight** in het zoekvak in. Als er geen **HDInsight Interactive Query (bèta)**, moet u uw Power BI Desktop bijwerken naar de nieuwste versie.

4. Selecteer **HDInsight Interactive Query (bèta)**, en selecteer vervolgens **Connect**.

5. Selecteer **doorgaan** sluiten de **Preview-connector** waarschuwingsvenster.

6. Van **HDInsight Interactive Query**Selecteer of typ de volgende informatie:

    - **Server**: Voer de naam van het Interactive Query-cluster, bijvoorbeeld *myiqcluster.azurehdinsight.net*.

    - **Database**: voor deze zelfstudie voert u **standaard**.
    
    - **Gegevensverbindingsmodus**: Selecteer voor deze zelfstudie **DirectQuery**.

    ![HDInsight interactive query Power BI DirectQuery-verbinding](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)

7. Klik op **OK**.

8. Voer de referenties van de HTTP-gebruiker en klik vervolgens op **OK**. De standaardgebruikersnaam is **admin**

9. Selecteer in het linkerdeelvenster **hivesampletale**, en klik vervolgens op **Load**.

    ![HDInsight interactive query Power BI hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data-on-a-map"></a>Visualiseer gegevens op een kaart

Volgen op de laatste procedure.

1. Selecteer in het deelvenster visualisaties **kaart**.  Het is een pictogram van globe.

    ![HDInsight Power BI worden aangepast rapport](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
    
2. Selecteer in het deelvenster velden **land** en **devicemake**. Hier ziet u de gegevens op de kaart uitgezet.

3. Vouw de kaart.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u gegevens uit HDInsight met behulp van Power BI visualiseren.  Zie de volgende artikelen voor meer informatie over gegevensvisualisatie:

* [Apache Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight met behulp van ODBC](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Apache Zeppelin gebruiken voor het Apache Hive-query's uitvoeren in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel verbinden met Apache Hadoop met Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en Apache Hive-query's uitvoeren met behulp van Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).
