---
title: Interactieve Hive-Query-gegevens visualiseren met Power BI in Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Microsoft Power BI om interactieve Query Hive-gegevens die zijn verwerkt door Azure HDInsight te visualiseren.
keywords: hdinsight worden hadoop hive interactieve query uitvoert, interactieve hive, LLAP, directquery
services: hdinsight
documentationcenter: ''
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.devlang: na
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: jgao
ms.openlocfilehash: b8da1f17b9e477caf9031cf94ee14f3a181e247e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="visualize-interactive-query-hive-data-with-microsoft-power-bi-using-direct-query-in-azure-hdinsight"></a>Interactieve Hive-Query gegevens visualiseren met Microsoft Power BI met behulp van directe query in Azure HDInsight

Informatie over het verbinding maken met Microsoft Power BI interactieve Query van Azure HDInsight-clusters en visualiseren van de Hive-gegevens met behulp van directe query. In deze zelfstudie maakt laden u de gegevens uit een hivesampletable Hive-tabel naar Power BI. De Hive-tabel bevat de gebruiksgegevens van sommige mobiele telefoon. Vervolgens kunt u de gebruiksgegevens op een kaart world uitzetten:

![HDInsight Power BI het rapport kaart](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-visualization.png)

U kunt gebruikmaken van de [Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-hive-power-bi.md) importeren via de algemene ODBC-connector in Power BI Desktop. Het is echter niet aangeraden voor BI werkbelastingen vanwege niet-interactieve aard van de Hive-query-engine. [HDInsight interactieve Query connector](./apache-hadoop-connect-hive-power-bi-directquery.md) en [HDInsight Spark connector](https://docs.microsoft.com/power-bi/spark-on-hdinsight-with-direct-connect) betere keuzes voor de prestaties zijn.

## <a name="prerequisites"></a>Vereisten
Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **HDInsight-cluster**. Het cluster kan bestaan uit een HDInsight-cluster met Hive of een nieuw uitgebrachte interactieve Query-cluster. Zie voor het maken van clusters [cluster maken](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. U kunt downloaden een kopie van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="load-data-from-hdinsight"></a>Gegevens uit HDInsight laden

De hivesampletable Hive-tabel wordt geleverd bij alle HDInsight-clusters.

1. Aanmelden bij Power BI Desktop.
2. Klik op de **Start** tabblad **gegevens ophalen** van de **externe gegevens** lint en selecteer vervolgens **meer**.

    ![Open HDInsight Power BI-gegevens](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-open-odbc.png)
3. Van de **gegevens ophalen** deelvenster, type **hdinsight** in het zoekvak. Als er geen **HDInsight interactieve Query (bèta)**, moet u uw Power BI Desktop bijwerken naar de nieuwste versie.
4. Klik op **HDInsight interactieve Query (bèta)**, en klik vervolgens op **Connect**.
5. Klik op **doorgaan** sluiten de **Preview connector** waarschuwingsdialoogvenster.
6. Van **HDInsight interactieve Query**Selecteer of typ de volgende informatie:

    - **Server**: Voer de naam van de interactieve Query-cluster bijvoorbeeld *myiqcluster.azurehdinsight.net*.
    - **Database**: Voer voor deze zelfstudie **standaard**.
    - **Gegevens connectiviteitsmodus**: Selecteer voor deze zelfstudie **DirectQuery**.

    ![HDInsight interactieve query power bi directquery verbinding maken](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-connect.png)
7. Klik op **OK**.
8. Voer de gebruikersreferenties voor HTTP en klik vervolgens op **OK**.  De standaardgebruikersnaam **admin**
9. Selecteer in het linkerdeelvenster **hivesampletale**, en klik vervolgens op **Load**.

    ![HDInsight interactieve query power bi hivesampletable](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-interactive-query-power-bi-hivesampletable.png)

## <a name="visualize-data"></a>Gegevens visualiseren

Overgenomen van de laatste procedure.

1. Selecteer in het deelvenster visualisaties **kaart**.  Het is een pictogram van globe.

    ![HDInsight Power BI worden aangepast rapport](./media/apache-hadoop-connect-hive-power-bi-directquery/hdinsight-power-bi-customize.png)
2. Selecteer in het deelvenster velden **land** en **devicemake**. Hier ziet u de gegevens die op de kaart wordt getekend.
3. Vouw de kaart.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Visualiseer gegevens uit HDInsight met behulp van Power BI.  Zie voor meer informatie de volgende artikelen:

* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight met behulp van ODBC](../hadoop/apache-hadoop-connect-hive-power-bi.md). 
* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel en Hadoop koppelen met Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).
