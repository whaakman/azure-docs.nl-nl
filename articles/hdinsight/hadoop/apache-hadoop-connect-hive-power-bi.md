---
title: Apache Hive-gegevens visualiseren met Power BI - Azure HDInsight
description: Informatie over het gebruik van Microsoft Power BI om verwerkt door Azure HDInsight Hive-gegevens te visualiseren.
keywords: hdinsight, hadoop, hive, interactive query, interactive hive, LLAP, odbc
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: hrasheed
ms.openlocfilehash: 1e0c043e484e4eaf2639f76c9af3fef15ad85047
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237486"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Apache Hive-gegevens visualiseren met Microsoft Power BI met behulp van ODBC in Azure HDInsight

Informatie over Microsoft Power BI Desktop verbinden met Azure HDInsight met behulp van ODBC- en Apache Hive-gegevens visualiseren.

>[!IMPORTANT]
> U kunt gebruikmaken van het Hive ODBC-stuurprogramma om te via de algemene ODBC-connector in Power BI Desktop importeren. Het is echter niet aanbevolen voor BI workloads vanwege niet-interactieve aard van de Hive-query-engine. [HDInsight Interactive Query-connector](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) en [HDInsight Spark-connector](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) betere mogelijkheden voor de prestaties zijn.

In deze zelfstudie hebt u de gegevens worden geladen uit een `hivesampletable` Hive-tabel naar Power BI. De Hive-tabel bevat de gebruiksgegevens van sommige mobiele telefoon. Vervolgens kunt u gegevens over gebruik op een wereldkaart brengt:

![HDInsight Power BI het rapport met kaart](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

De informatie is ook van toepassing op de nieuwe [Interactive Query](../interactive-query/apache-interactive-query-get-started.md) clustertype. Voor informatie over het verbinding maken met HDInsight Interactive Query met behulp van directquery, Zie [gegevens met Microsoft Power BI met behulp van directquery in Azure HDInsight Interactive Query Hive visualiseren](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).

## <a name="prerequisites"></a>Vereisten

Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **HDInsight-cluster**. Het cluster kan zijn voor een HDInsight-cluster met Hive of een nieuw uitgebrachte Interactive Query-cluster. Zie voor het maken van clusters, [-cluster maken](apache-hadoop-linux-tutorial-get-started.md#create-cluster).

* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)** . U kunt een kopie van downloaden de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Hive ODBC-gegevensbron maken

Zie [maken Hive ODBC-gegevensbron](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Laden van gegevens uit HDInsight

De Hive-tabel hivesampletable wordt geleverd met alle HDInsight-clusters.

1. Start Power BI Desktop.

2. In het menu bovenaan, gaat u naar **Start** > **gegevens ophalen** > **meer...** .

    ![Open HDInsight Power BI-gegevens](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)

3. Uit de **gegevens ophalen** dialoogvenster, selecteer **andere** vanaf de linkerkant, selecteer **ODBC** vanaf de rechterkant en selecteer vervolgens **Connect** aan de onderkant.

4. Uit de **uit ODBC** dialoogvenster, selecteer de gegevensbron naam die u hebt gemaakt in de laatste sectie van de vervolgkeuzelijst, en selecteer vervolgens **OK**.

5. Uit de **Navigator** dialoogvenster Vouw **ODBC > HIVE > standaard**, selecteer **hivesampletable**, en selecteer vervolgens **Load**.

6. Uit de **ODBC-stuurprogramma** dialoogvenster, selecteer **standaard of aangepast**en selecteer vervolgens **Connect**.

## <a name="visualize-data"></a>Gegevens visualiseren

Volgen op de laatste procedure.

1. Selecteer in het deelvenster visualisaties **kaart**.  Het is een pictogram van globe.

    ![HDInsight Power BI worden aangepast rapport](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. Uit de **velden** venster **land** en **devicemake**. Hier ziet u de gegevens op de kaart uitgezet.
3. Vouw de kaart.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u gegevens uit HDInsight met behulp van Power BI visualiseren.  Zie de volgende artikelen voor meer informatie:

* [Apache Zeppelin gebruiken voor het Apache Hive-query's uitvoeren in Azure HDInsight](../interactive-query/hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel verbinden met Apache Hadoop met Power Query](apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en Apache Hive-query's uitvoeren met behulp van Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).
