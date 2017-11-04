---
title: Big-gegevens visualiseren met Power BI in Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Microsoft Power BI om verwerkt door Azure HDInsight Hive-gegevens te visualiseren.
keywords: hdinsight worden hadoop hive interactieve query uitvoert, interactieve hive, LLAP, odbc
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2017
ms.author: jgao
ms.openlocfilehash: 390342eb08ae970fa760b414674b1a6783404d80
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="visualize-hive-data-with-microsoft-power-bi-in-azure-hdinsight"></a>Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight

Informatie over het Microsoft Power BI verbinding met Azure HDInsight en het Hive-gegevens visualiseren. Power BI ondersteunt momenteel alleen ODBC-verbinding met HDInsight. In deze zelfstudie maakt laden u de gegevens uit een hivesampletable Hive-tabel naar Power BI. De Hive-tabel bevat de gebruiksgegevens van sommige mobiele telefoon. Vervolgens kunt u de gebruiksgegevens op een kaart world uitzetten:

![HDInsight Power BI het rapport kaart](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

De informatie geldt ook voor de nieuwe [interactieve Query](../interactive-query/apache-interactive-query-get-started.md) type cluster.

## <a name="prerequisites"></a>Vereisten
Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **HDInsight-cluster**. Het cluster kan bestaan uit een HDInsight-cluster met Hive of een nieuw uitgebrachte interactieve Query-cluster. Zie voor het maken van clusters [cluster maken](apache-hadoop-linux-tutorial-get-started.md#create-cluster).
* **[Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/)**. U kunt downloaden een kopie van de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=45331).

## <a name="create-hive-odbc-data-source"></a>Hive ODBC-gegevensbron maken

Zie [maken Hive ODBC-gegevensbron](apache-hadoop-connect-excel-hive-odbc-driver.md#create-hive-odbc-data-source).

## <a name="load-data-from-hdinsight"></a>Gegevens uit HDInsight laden

De hivesampletable Hive-tabel wordt geleverd bij alle HDInsight-clusters.

1. Aanmelden bij Power BI Desktop.
2. Klik op de **Start** tabblad **gegevens ophalen** van de **externe gegevens** lint en selecteer vervolgens **meer**.

    ![Open HDInsight Power BI-gegevens](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-open-odbc.png)
3. Van de **gegevens ophalen** deelvenster, klikt u op **andere** vanaf de linkerkant, klikt u op **ODBC** vanaf de rechterkant en klik vervolgens op **Connect** onder.
4. Van de **van ODBC** deelvenster, selecteer de gegevensbron naam die u in de laatste sectie hebt gemaakt, en klik vervolgens op **OK**.
5. Van de **Navigator** deelvenster Vouw **ODBC-component > Standaard ->**, selecteer **hivesampletable**, en klik vervolgens op **Load**.

## <a name="visualize-date"></a>Datum visualiseren

Overgenomen van de laatste procedure.

1. Selecteer in het deelvenster visualisaties **kaart**.  Het is een pictogram van globe.

    ![HDInsight Power BI worden aangepast rapport](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. Selecteer in het deelvenster velden **land** en **devicemake**. Hier ziet u de gegevens die op de kaart wordt getekend.
3. Vouw de kaart.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Visualiseer gegevens uit HDInsight met behulp van Power BI.  Zie voor meer informatie de volgende artikelen:

* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight ](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel en Hadoop koppelen met Power Query](apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).
