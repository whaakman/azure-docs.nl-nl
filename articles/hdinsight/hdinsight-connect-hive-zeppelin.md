---
title: Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight
description: Leer hoe u Zeppelin gebruiken voor het uitvoeren van Hive-query's.
keywords: hdinsight, hadoop, hive, LLAP met interactive query
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: jasonh
ms.openlocfilehash: d4767c4d86d03827b0c055af41638988afd632a1
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39595930"
---
# <a name="use-zeppelin-to-run-hive-queries-in-azure-hdinsight"></a>Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight 

HDInsight Interactive Query-clusters bevatten Zeppelin-notitieblokken die u gebruiken kunt om uit te voeren van interactieve Hive-query's. In dit artikel leert u hoe u Zeppelin gebruiken voor het uitvoeren van Hive-query's in Azure HDInsight. 

## <a name="prerequisites"></a>Vereisten
Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **HDInsight Interactive Query-cluster**. Zie [-cluster maken](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) om een HDInsight-cluster te maken.  Zorg ervoor dat u de interactieve query's te kiezen. 

## <a name="create-a-zeppelin-note"></a>Een notitie Zeppelin maken

1. Blader naar de volgende URL:

        https://CLUSTERNAME.azurehdinsight.net/zeppelin
    Vervang **CLUSTERNAME** door de naam van uw cluster.

2. Voer uw Hadoop-gebruikersnaam en wachtwoord. Op de pagina Zeppelin kunt u een nieuwe notitie maken of bestaande notities openen. HiveSample bevat een voorbeeld van Hive-query's.  

    ![Zeppelin HDInsight Interactive Query](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)
3. Klik op **nieuwe notitie maken**.
4. Typ of selecteer de volgende waarden:

    - Naam van de notitie: Voer een naam voor de opmerking.
    - Standaard-interpreter: Selecteer **JDBC**.

5. Klik op **notitie maken**.
6. Voer de volgende Hive-query:

        %jdbc(hive)
        show tables

    ![Zeppelin HDInsight Interactive Query-query wordt uitgevoerd](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    De **%jdbc(hive)** -instructie in de eerste regel wordt de notebook geïnstrueerd om het gebruik van het Hive JDBC-interpreter.

    De query wordt een Hive-tabel met de naam retourneren *hivesampletable*.

    Hier volgen twee meer Hive-query's die u op de hivesampletable uitvoeren kunt. 

        %jdbc(hive)
        select * from hivesampletable limit 10

        %jdbc(hive)
        select ${group_name}, count(*) as total_count
        from hivesampletable
        group by ${group_name=market,market|deviceplatform|devicemake}
        limit ${total_count=10}

    Vergelijken met de traditionele Hive, keert de queryresultaten u terug moet sneller.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u gegevens uit HDInsight met behulp van Power BI visualiseren.  Zie de volgende artikelen voor meer informatie:

* [Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Interactive Query Hive-gegevens visualiseren met Power BI in Azure HDInsight](./interactive-query/apache-hadoop-connect-hive-power-bi-directquery.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Koppel Excel aan Hadoop met Power Query](hadoop/apache-hadoop-connect-excel-power-query.md).
* [Verbinding maken met Azure HDInsight en het uitvoeren van Hive-query's met Data Lake Tools voor Visual Studio](hadoop/apache-hadoop-visual-studio-tools-get-started.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](./hdinsight-upload-data.md).
