---
title: Interactive Query Hive-gegevens visualiseren met Power BI in Azure HDInsight
description: Informatie over het gebruik van Microsoft Power BI om verwerkt door Azure HDInsight Interactive Query Hive-gegevens te visualiseren.
keywords: hdinsight, hadoop, hive, interactive query, interactive hive, LLAP, directquery
services: hdinsight
ms.service: hdinsight
author: jasonwhowell
ms.author: jasonh
editor: jasonwhowell
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/14/2018
ms.openlocfilehash: 4dcfcb5e70b9eb6626be1f3528781a8c5b1bd5c4
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39593023"
---
# <a name="visualize-interactive-query-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Interactive Query Hive-gegevens visualiseren met Microsoft Power BI met behulp van directquery in Azure HDInsight

Leer hoe u Microsoft Power BI verbinden met Azure HDInsight Interactive Query-clusters en visualiseren van de Hive-gegevens met behulp van directquery. In deze zelfstudie hebt laden u de gegevens uit een Hive-tabel hivesampletable in Power BI. De Hive-tabel bevat de gebruiksgegevens van sommige mobiele telefoon. Vervolgens kunt u gegevens over gebruik op een wereldkaart brengt:

![HDInsight Power BI het rapport met kaart](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

U kunt gebruikmaken van de [Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-hive-power-bi.md) via de algemene ODBC-connector in Power BI Desktop importeren. Het is echter niet aanbevolen voor BI workloads vanwege niet-interactieve aard van de Hive-query-engine. [HDInsight Interactive Query-connector](./apache-hadoop-connect-hive-power-bi-directquery.md) en [HDInsight Spark-connector](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) betere mogelijkheden voor de prestaties zijn.

## <a name="prerequisites"></a>Vereisten
Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **HDInsight-cluster**. Het cluster kan zijn voor een HDInsight-cluster met Hive of een nieuw uitgebrachte Interactive Query-cluster. Zie voor het maken van clusters, [-cluster maken](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. U kunt een kopie van downloaden de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Laden van gegevens uit HDInsight

De Hive-tabel hivesampletable wordt geleverd met alle HDInsight-clusters.

1. Aanmelden bij Power BI Desktop.
2. Klik op de **Start** tabblad **gegevens ophalen** uit de **externe gegevens** lint en selecteer vervolgens **meer**.

    ![Open HDInsight Power BI-gegevens](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
3. Uit de **gegevens ophalen** deelvenster, type **hdinsight** in het zoekvak in. Als er geen **HDInsight Interactive Query (bèta)**, moet u uw Power BI Desktop bijwerken naar de nieuwste versie.
4. Klik op **HDInsight Interactive Query (bèta)**, en klik vervolgens op **Connect**.
5. Klik op **doorgaan** sluiten de **Preview-connector** waarschuwingsvenster.
6. Van **HDInsight Interactive Query**Selecteer of typ de volgende informatie:

    - **Server**: Voer de naam van het Interactive Query-cluster, bijvoorbeeld *myiqcluster.azurehdinsight.net*.
    - **Database**: voor deze zelfstudie voert u **standaard**.
    - **Gegevensverbindingsmodus**: Selecteer voor deze zelfstudie **DirectQuery**.

    ![HDInsight interactive query power bi directquery-verbinding](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)
7. Klik op **OK**.
8. Voer de referenties van de HTTP-gebruiker en klik vervolgens op **OK**.  De standaardgebruikersnaam is **admin**
9. Selecteer in het linkerdeelvenster **hivesampletale**, en klik vervolgens op **Load**.

    ![HDInsight interactive query power bi hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data"></a>Gegevens visualiseren

Volgen op de laatste procedure.

1. Selecteer in het deelvenster visualisaties **kaart**.  Het is een pictogram van globe.

    ![HDInsight Power BI worden aangepast rapport](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
2. Selecteer in het deelvenster velden **land** en **devicemake**. Hier ziet u de gegevens op de kaart uitgezet.
3. Vouw de kaart.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u gegevens uit HDInsight met behulp van Power BI visualiseren.  Zie de volgende artikelen voor meer informatie:

* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight met behulp van ODBC](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Koppel Excel aan Hadoop met Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).
