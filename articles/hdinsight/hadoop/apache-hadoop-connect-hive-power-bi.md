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
ms.date: 01/03/2018
ms.author: jgao
ms.openlocfilehash: 8b4347fde274d0d7520ef3acbd081fdb83d7dc7d
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="visualize-hive-data-with-microsoft-power-bi-using-odbc-in-azure-hdinsight"></a>Hive-gegevens visualiseren met Microsoft Power BI met ODBC in Azure HDInsight

Informatie over het Microsoft Power BI verbinden met Azure HDInsight met behulp van ODBC en de Hive-gegevens visualiseren. In deze zelfstudie maakt laden u de gegevens uit een hivesampletable Hive-tabel naar Power BI. De Hive-tabel bevat de gebruiksgegevens van sommige mobiele telefoon. Vervolgens kunt u de gebruiksgegevens op een kaart world uitzetten:

![HDInsight Power BI het rapport kaart](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-visualization.png)

De informatie geldt ook voor de nieuwe [interactieve Query](../interactive-query/apache-interactive-query-get-started.md) type cluster. Voor het verbinding maken met de interactieve Query HDInsight met behulp van directe query, raadpleegt [interactieve Hive-Query visualiseren gegevens met Microsoft Power BI in Azure HDInsight met behulp van directe query](../interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).

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

## <a name="visualize-data"></a>Gegevens visualiseren

Overgenomen van de laatste procedure.

1. Selecteer in het deelvenster visualisaties **kaart**.  Het is een pictogram van globe.

    ![HDInsight Power BI worden aangepast rapport](./media/apache-hadoop-connect-hive-power-bi/hdinsight-power-bi-customize.png)
2. Selecteer in het deelvenster velden **land** en **devicemake**. Hier ziet u de gegevens die op de kaart wordt getekend.
3. Vouw de kaart.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Visualiseer gegevens uit HDInsight met behulp van Power BI.  Zie voor meer informatie de volgende artikelen:

* [Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight](./../hdinsight-connect-hive-zeppelin.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](./apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel en Hadoop koppelen met Power Query](apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./../hdinsight-upload-data.md).
