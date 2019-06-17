---
title: "Quickstart: Apache Hive-query's uitvoeren in Azure HDInsight - Apache Zeppelin"
description: In deze snelstartgids leert u hoe u Apache Hive-query's uitvoeren met Apache Zeppelin.
keywords: hdinsight, hadoop, hive, LLAP met interactive query
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 05/06/2019
ms.author: hrasheed
ms.openlocfilehash: 1642c64b0b14c2e290aad689399b59d896660a28
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056694"
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
