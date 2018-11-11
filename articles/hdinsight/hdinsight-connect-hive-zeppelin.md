---
title: Apache Zeppelin Apache Hive-query's uitvoeren in Azure HDInsight gebruiken
description: Informatie over het gebruik van Apache Zeppelin Apache Hive-query's uitvoeren.
keywords: hdinsight, hadoop, hive, LLAP met interactive query
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: 75ec0e17e9866d2cd3420ff6ecf648bf22a8ae8e
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2018
ms.locfileid: "51277950"
---
# <a name="use-apache-zeppelin-to-run-apache-hive-queries-in-azure-hdinsight"></a>Apache Zeppelin Apache Hive-query's uitvoeren in Azure HDInsight gebruiken 

HDInsight Interactive Query-clusters bevatten Apache Zeppelin-notitieblokken die u gebruiken kunt om uit te voeren van interactieve Hive-query's. In dit artikel leert u hoe u Apache Hive-query's uitvoeren in Azure HDInsight met Apache Zeppelin. 

## <a name="prerequisites"></a>Vereisten
Voordat u verdergaat met dit artikel, hebt u de volgende items:

* **HDInsight Interactive Query-cluster**. Zie [-cluster maken](hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) om een HDInsight-cluster te maken.  Zorg ervoor dat u de interactieve query's te kiezen. 

## <a name="create-a-apache-zeppelin-note"></a>Een Apache Zeppelin notitie maken

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
