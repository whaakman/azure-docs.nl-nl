---
title: Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight | Microsoft Docs
description: Informatie over het Zeppelin gebruiken voor het uitvoeren van Hive-query's.
keywords: hdinsight, hadoop, hive, interactieve query, LLAP
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
ms.openlocfilehash: 39f99bef252e93db55e0493ee284ef78b7d087a1
ms.sourcegitcommit: 4bd369fc472dced985239aef736fece42fecfb3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2018
---
# <a name="use-zeppelin-to-run-hive-queries-in-azure-hdinsight"></a>Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight 

Interactieve Query HDInsight-clusters bevatten Zeppelin-notebooks kunt u interactieve Hive-query's uitvoeren. In dit artikel leert u het gebruik van Zeppelin Hive-query's uitvoeren in Azure HDInsight. 

## <a name="prerequisites"></a>Vereisten
Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **Interactieve Query HDInsight-cluster**. Zie [cluster maken](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) een HDInsight-cluster te maken.  Zorg ervoor dat de interactieve Query kiezen. 

## <a name="create-a-zeppelin-note"></a>Een notitie Zeppelin maken

1. Blader naar de volgende URL:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    Vervang **CLUSTERNAME** door de naam van uw cluster.

2. Voer uw Hadoop-gebruikersnaam en wachtwoord. Op de pagina Zeppelin kunt u een nieuwe notitie maken of bestaande notities openen. HiveSample bevat een aantal Hive-voorbeeldquery.  

    ![HDInsight interactieve Query zeppelin](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. Klik op **nieuwe notitie maken**.
4. Typ of selecteer de volgende waarden:

    - De naam van de opmerking: Voer een naam voor de opmerking.
    - Standaard-interpreter: Selecteer **JDBC**.

5. Klik op **notitie maken**.
6. Voer de volgende Hive-query:

        %jdbc(hive)
        show tables

    ![HDInsight interactieve Query zeppelin query wordt uitgevoerd](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    De **%jdbc(hive)** -instructie in de eerste regel wordt de notebook gebruiken de JDBC Hive-interpreter uitgelegd.

    De query retourneert een Hive-tabel aangeroepen *hivesampletable*.

    Hieronder vindt u twee meer Hive-query's die u op de hivesampletable uitvoeren kunt. 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    Vergelijken met de traditionele Hive, keert de queryresultaten u terug moet sneller.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Visualiseer gegevens uit HDInsight met behulp van Power BI.  Zie voor meer informatie de volgende artikelen:

* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Interactieve Hive-Query-gegevens visualiseren met Power BI in Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel en Hadoop koppelen met Power Query](hadoop/apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./hdinsight-upload-data.md).
