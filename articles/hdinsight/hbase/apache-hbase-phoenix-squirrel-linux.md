---
title: 'Quickstart: Query Apache HBase in Azure HDInsight - Apache Phoenix'
description: In deze snelstartgids leert u hoe u Apache Phoenix gebruiken in HDInsight. Ook informatie over het installeren en SQLLine instellen op uw computer verbinding maken met een HBase-cluster in HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: fd812698ef68a4077b7de6a612392d30d195ec39
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076280"
---
# <a name="quickstart-query-apache-hbase-in-azure-hdinsight-with-apache-phoenix"></a>Quickstart: Query Apache HBase in Azure HDInsight met Apache Phoenix

In deze snelstartgids leert u hoe u de Apache Phoenix met HBase-query's uitvoeren in Azure HDInsight. Apache Phoenix is een SQL-query-engine voor Apache HBase. Deze is toegankelijk als een JDBC-stuurprogramma en maakt het uitvoeren van query's en beheren van HBase-tabellen SQL mogelijk. [SQLLine](http://sqlline.sourceforge.net/) is een opdrachtregelprogramma voor het uitvoeren van SQL.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Een Apache HBase-cluster. Zie [-cluster maken](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) om een HDInsight-cluster te maken.  Zorg ervoor dat u kiest de **HBase** clustertype.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="identify-a-zookeeper-node"></a>Identificeren van een ZooKeeper-knooppunt

Wanneer u verbinding met een HBase-cluster maakt, moet u verbinding maken met een van de Apache ZooKeeper-knooppunten. Elke HDInsight-cluster heeft drie ZooKeeper-knooppunten. CURL kan worden gebruikt om snel te identificeren een ZooKeeper-knooppunt. De curl-opdracht hieronder bewerken door te vervangen `PASSWORD` en `CLUSTERNAME` met de relevante waarden, en voer de opdracht in een opdrachtprompt:

```cmd
curl -u admin:PASSWORD -sS -G https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER
```

Een gedeelte van de uitvoer wordt als volgt uitzien:

```output
    {
      "href" : "http://hn1-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net:8080/api/v1/clusters/myCluster/hosts/zk0-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net/host_components/ZOOKEEPER_SERVER",
      "HostRoles" : {
        "cluster_name" : "myCluster",
        "component_name" : "ZOOKEEPER_SERVER",
        "host_name" : "zk0-brim.432dc3rlshou3ocf251eycoapa.bx.internal.cloudapp.net"
      }
```

Noteer de waarde voor `host_name` voor later gebruik.

## <a name="create-a-table-and-manipulate-data"></a>Een tabel maken en bewerken van gegevens

U kunt gebruikmaken van SSH verbinding maken met HBase-clusters en vervolgens Apache Phoenix gebruiken voor het maken van HBase-tabellen, gegevens en querygegevens invoegen.

1. Gebruik `ssh` opdracht verbinding maken met uw HBase-cluster. De onderstaande opdracht bewerken door te vervangen `CLUSTERNAME` met de naam van het cluster, en voer vervolgens de opdracht:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Wijzig de map naar de Phoenix-client. Voer de volgende opdracht in:

    ```bash
    cd /usr/hdp/current/phoenix-client/bin
    ```

3. Start [SQLLine](http://sqlline.sourceforge.net/). De onderstaande opdracht bewerken door te vervangen `ZOOKEEPER` met de ZooKeeper-knooppunt eerder hebt geïdentificeerd, voert u de opdracht:

    ```bash
    ./sqlline.py ZOOKEEPER:2181:/hbase-unsecure
    ```

4. Maak een HBase-tabel. Voer de volgende opdracht in:

    ```sql
    CREATE TABLE Company (company_id INTEGER PRIMARY KEY, name VARCHAR(225));
    ```

5. Gebruik de SQLLine `!tables` opdracht om een lijst van alle tabellen in HBase. Voer de volgende opdracht in:

    ```sqlline
    !tables
    ```

6. Waarden in de tabel invoegen. Voer de volgende opdracht in:

    ```sql
    UPSERT INTO Company VALUES(1, 'Microsoft');
    UPSERT INTO Company VALUES(2, 'Apache');
    ```

7. Query uitvoeren op de tabel. Voer de volgende opdracht in:

    ```sql
    SELECT * FROM Company;
    ```

8. Een record verwijderen. Voer de volgende opdracht in:

    ```sql
    DELETE FROM Company WHERE COMPANY_ID=1;
    ```

9. De tabel verwijderen. Voer de volgende opdracht in:

    ```hbase
    DROP TABLE Company;
    ```

10. Gebruik de SQLLine `!quit` opdracht om SQLLine af te sluiten. Voer de volgende opdracht in:

    ```sqlline
    !quit
    ```

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de Quick Start hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Als u wilt verwijderen van een cluster, Zie [verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u de Apache Phoenix met HBase-query's uitvoeren in Azure HDInsight. Voor meer informatie over Apache Phoenix, biedt het volgende artikel een dieper onderzoek.

> [!div class="nextstepaction"]
> [Apache Phoenix in HDInsight](../hdinsight-phoenix-in-hdinsight.md)
