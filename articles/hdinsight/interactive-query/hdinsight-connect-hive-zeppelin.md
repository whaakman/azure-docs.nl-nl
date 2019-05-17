---
title: "Quickstart: Apache Hive-query's uitvoeren in Azure HDInsight - Apache Zeppelin"
description: Informatie over het gebruik van Apache Zeppelin Apache Hive-query's uitvoeren.
keywords: hdinsight, hadoop, hive, LLAP met interactive query
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 05/06/2019
ms.author: hrasheed
ms.openlocfilehash: f4b8495646e83005dc48e8a729a0e5987b832721
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65801048"
---
# <a name="quickstart-execute-apache-hive-queries-in-azure-hdinsight-with-apache-zeppelin"></a>Quickstart: Apache Hive-query's uitvoeren in Azure HDInsight met Apache Zeppelin

In deze snelstartgids leert u hoe u Apache Zeppelin gebruiken om uit te voeren [Apache Hive](https://hive.apache.org/) query's in Azure HDInsight. HDInsight Interactive Query-clusters bevatten [Apache Zeppelin](https://zeppelin.apache.org/) laptops die u gebruiken kunt voor het uitvoeren van interactieve Hive-query's.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Een HDInsight Interactive Query-cluster. Zie [-cluster maken](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) om een HDInsight-cluster te maken.  Zorg ervoor dat u kiest de **Interactive Query** clustertype.

## <a name="create-an-apache-zeppelin-note"></a>Een Apache Zeppelin notitie maken

1. Vervang `CLUSTERNAME` met de naam van het cluster in de volgende URL `https://CLUSTERNAME.azurehdinsight.net/zeppelin`. Voer vervolgens de URL in een webbrowser.

2. Voer uw gebruikersnaam en wachtwoord. Op de pagina Zeppelin kunt u een nieuwe notitie maken of bestaande notities openen. **HiveSample** bevat een voorbeeld van Hive-query's.  

    ![Zeppelin HDInsight Interactive Query](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin.png)

3. Selecteer **nieuwe notitie maken**.

4. Uit de **nieuwe notitie maken** dialoogvenster typt of selecteert u de volgende waarden:

    - Naam van de opmerking: Voer een naam voor de notitie.
    - Standaard-interpreter: Selecteer **jdbc** uit de vervolgkeuzelijst.

5. Selecteer **notitie maken**.

6. Voer de volgende Hive-query in de sectie met voorbeeldcode en druk vervolgens op **Shift + Enter**:

    ```hive
    %jdbc(hive)
    show tables
    ```

    ![Zeppelin HDInsight Interactive Query-query wordt uitgevoerd](./media/hdinsight-connect-hive-zeppelin/hdinsight-hive-zeppelin-query.png)

    De **%jdbc(hive)** -instructie in de eerste regel wordt de notebook geïnstrueerd om het gebruik van het Hive JDBC-interpreter.

    De query wordt een Hive-tabel met de naam retourneren **hivesampletable**.

    Hieronder vindt u twee extra Hive-query's die u kunt uitvoeren op **hivesampletable**:

    ```hive
    %jdbc(hive)
    select * from hivesampletable limit 10

    %jdbc(hive)
    select ${group_name}, count(*) as total_count
    from hivesampletable
    group by ${group_name=market,market|deviceplatform|devicemake}
    limit ${total_count=10}
    ```

    Vergelijken met de traditionele Hive, keert de queryresultaten u terug moet sneller.

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de Quick Start hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Als u wilt verwijderen van een cluster, Zie [verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u Apache Hive-query's uitvoeren in Azure HDInsight met Apache Zeppelin. Voor meer informatie over Hive-query's, het volgende artikel wordt beschreven hoe u voor het uitvoeren van query's met Visual Studio.

> [!div class="nextstepaction"]
> [Verbinding maken met Azure HDInsight en Apache Hive-query's uitvoeren met behulp van Data Lake Tools voor Visual Studio](../hadoop/apache-hadoop-visual-studio-tools-get-started.md)

## <a name="see-also"></a>Zie ook

* [Apache Hive-gegevens visualiseren met Microsoft Power BI in Azure HDInsight](../hadoop/apache-hadoop-connect-hive-power-bi.md).
* [Apache Interactive Query Hive-gegevens visualiseren met Power BI in Azure HDInsight](./apache-hadoop-connect-hive-power-bi-directquery.md).
* [Excel verbinden met HDInsight met het Microsoft Hive ODBC-stuurprogramma](../hadoop/apache-hadoop-connect-excel-hive-odbc-driver.md).
* [Excel verbinden met Apache Hadoop met Power Query](../hadoop/apache-hadoop-connect-excel-power-query.md).
* [Azure HDInsight-hulpprogramma gebruiken voor Visual Studio Code](../hdinsight-for-vscode.md).
* [Gegevens uploaden naar HDInsight](../hdinsight-upload-data.md).
