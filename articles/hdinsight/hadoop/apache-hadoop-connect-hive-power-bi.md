---
title: Big-gegevens visualiseren met Power BI in Azure HDInsight
description: Informatie over het gebruik van Microsoft Power BI om verwerkt door Azure HDInsight Hive-gegevens te visualiseren.
keywords: hdinsight, hadoop, hive, interactive query, interactive hive, LLAP, odbc
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: hrasheed
ms.openlocfilehash: e6711a5595c6b7c6caaff97f4c1996817be1abce
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634546"
---
# <a name="visualize-apache-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Apache Hive-gegevens visualiseren met Microsoft Power BI met behulp van ODBC in Azure HDInsight

Informatie over Microsoft Power BI verbinden met Azure HDInsight met behulp van ODBC- en Apache Hive-gegevens visualiseren. 

>[!IMPORTANT]
> U kunt gebruikmaken van het Hive ODBC-stuurprogramma om te via de algemene ODBC-connector in Power BI Desktop importeren. Het is echter niet aanbevolen voor BI workloads vanwege niet-interactieve aard van de Hive-query-engine. [HDInsight Interactive Query-connector](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md) en [HDInsight Spark-connector](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) betere mogelijkheden voor de prestaties zijn.

In deze zelfstudie hebt laden u de gegevens uit een Hive-tabel hivesampletable in Power BI. De Hive-tabel bevat de gebruiksgegevens van sommige mobiele telefoon. Vervolgens kunt u gegevens over gebruik op een wereldkaart brengt:

![HDInsight Power BI het rapport met kaart](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

De informatie is ook van toepassing op de nieuwe [Interactive Query](../interactive-query/apache-interactive-query-get-started.md) clustertype. Voor informatie over het verbinding maken met HDInsight Interactive Query met behulp van directquery, Zie [gegevens met Microsoft Power BI met behulp van directquery in Azure HDInsight Interactive Query Hive visualiseren](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).



## <a name="prerequisites"></a>Vereisten
Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **HDInsight-cluster**. Het cluster kan zijn voor een HDInsight-cluster met Hive of een nieuw uitgebrachte Interactive Query-cluster. Zie voor het maken van clusters, [-cluster maken](apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. U kunt een kopie van downloaden de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Hive ODBC-gegevensbron maken

Zie [maken Hive ODBC-gegevensbron](apache-hadoop-connect-excel-hive-odbc-driver.md#create-apache-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Laden van gegevens uit HDInsight

De Hive-tabel hivesampletable wordt geleverd met alle HDInsight-clusters.

1. Aanmelden bij Power BI Desktop.
2. Klik op de **Start** tabblad **gegevens ophalen** uit de **externe gegevens** lint en selecteer vervolgens **meer**.

    ![Open HDInsight Power BI-gegevens](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)
3. Van de **gegevens ophalen** deelvenster, klikt u op **andere** vanaf de linkerkant, klikt u op **ODBC** vanaf de rechterkant, en klik vervolgens op **verbinding maken met** aan de onderkant.
4. Uit de **uit ODBC** deelvenster, selecteer de gegevensbron naam die u in de laatste sectie hebt gemaakt, en klik vervolgens op **OK**.
5. Uit de **Navigator** deelvenster uitvouwen **HIVE ODBC -> Standaard ->**, selecteer **hivesampletable**, en klik vervolgens op **Load**.

## <a name="visualize-data"></a>Gegevens visualiseren

Volgen op de laatste procedure.

1. Selecteer in het deelvenster visualisaties **kaart**.  Het is een pictogram van globe.

    ![HDInsight Power BI worden aangepast rapport](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. Selecteer in het deelvenster velden **land** en **devicemake**. Hier ziet u de gegevens op de kaart uitgezet.
3. Vouw de kaart.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u gegevens uit HDInsight met behulp van Power BI visualiseren.  Zie de volgende artikelen voor meer informatie:

* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Koppel Excel aan Hadoop met Power Query](apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).
