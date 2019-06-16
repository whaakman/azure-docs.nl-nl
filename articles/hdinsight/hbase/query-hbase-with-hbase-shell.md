---
title: 'Quickstart: Query Apache HBase in Azure HDInsight - HBase-Shell'
description: In deze snelstartgids leert u hoe u Apache HBase-query's uitvoeren met Apache HBase-Shell.
keywords: hdinsight,hadoop,HBase
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: quickstart
ms.date: 06/12/2019
ms.author: hrasheed
ms.openlocfilehash: d937e090895a5b02026c755b1efb7dd1e0b35000
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67054296"
---
# <a name="quickstart-query-apache-hbase-in-azure-hdinsight-with-hbase-shell"></a>Quickstart: Query Apache HBase in Azure HDInsight met HBase-Shell

In deze snelstartgids leert u hoe u een HBase-tabel maken, gegevens invoegen en vervolgens query uitvoeren op de tabel met Apache HBase-Shell.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

* Een Apache HBase-cluster. Zie [-cluster maken](../hadoop/apache-hadoop-linux-tutorial-get-started.md#create-cluster) om een HDInsight-cluster te maken.  Zorg ervoor dat u kiest de **HBase** clustertype.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-a-table-and-manipulate-data"></a>Een tabel maken en bewerken van gegevens

Voor de meeste mensen worden de gegevens weergegeven in een tabelindeling:

![Tabelgegevens in HDInsight HBase](./media/query-hbase-with-hbase-shell/hdinsight-hbase-contacts-tabular.png)

In HBase (een implementatie van [Cloud BigTable](https://cloud.google.com/bigtable/)), dezelfde gegevens ziet eruit als:

![BigTable-gegevens in HDInsight HBase](./media/query-hbase-with-hbase-shell/hdinsight-hbase-contacts-bigtable.png)

U kunt gebruikmaken van SSH verbinding maken met HBase-clusters en vervolgens Apache HBase Shell gebruiken voor het maken van HBase-tabellen, gegevens en querygegevens invoegen.

1. Gebruik `ssh` opdracht verbinding maken met uw HBase-cluster. De onderstaande opdracht bewerken door te vervangen `CLUSTERNAME` met de naam van het cluster, en voer vervolgens de opdracht:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. Gebruik `hbase shell` opdracht om te beginnen de interactieve HBase-shell. Voer de volgende opdracht in uw SSH-verbinding:

    ```bash
    hbase shell
    ```

3. Gebruik `create` opdracht voor het maken van een HBase-tabel met twee kolomfamilies. Voer de volgende opdracht in:

    ```hbase
    create 'Contacts', 'Personal', 'Office'
    ```

4. Gebruik `list` opdracht om een lijst van alle tabellen in HBase. Voer de volgende opdracht in:

    ```hbase
    list
    ```

5. Gebruik `put` opdracht voor het invoegen van waarden op een opgegeven kolom in een opgegeven rij in een bepaalde tabel. Voer de volgende opdracht in:

    ```hbase
    put 'Contacts', '1000', 'Personal:Name', 'John Dole'
    put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
    put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
    put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
    ```

6. Gebruik `scan` opdracht om te scannen en retourneert de `Contacts` tabelgegevens. Voer de volgende opdracht in:

    ```hbase
    scan 'Contacts'
    ```

7. Gebruik `get` opdracht voor het ophalen van inhoud van een rij. Voer de volgende opdracht in:

    ```hbase
    get 'Contacts', '1000'
    ```

    Ziet u hetzelfde resultaat als wanneer u de `scan` opdracht omdat er slechts één rij is.

8. Gebruik `delete` opdracht voor het verwijderen van de waarde van een cel in een tabel. Voer de volgende opdracht in:

    ```hbase
    delete 'Contacts', '1000', 'Office:Address'
    ```

9. Gebruik `disable` opdracht voor het uitschakelen van de tabel. Voer de volgende opdracht in:

    ```hbase
    disable 'Contacts'
    ```

10. Gebruik `drop` opdracht om te verwijderen van een tabel van HBase. Voer de volgende opdracht in:

    ```hbase
    drop 'Contacts'
    ```

11. Gebruik `exit` opdracht om te stoppen van de interactieve HBase-shell. Voer de volgende opdracht in:

    ```hbase
    exit
    ```

Zie voor meer informatie over het HBase-tabelschema [Inleiding tot Apache HBase-schemaontwerp](http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf). Zie voor meer HBase-opdrachten [Snelzoekgids voor Apache HBase](https://hbase.apache.org/book.html#quickstart).

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de Quick Start hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Als u wilt verwijderen van een cluster, Zie [verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe u een HBase-tabel maken, gegevens invoegen en vervolgens query uitvoeren op de tabel met Apache HBase-Shell. Voor meer informatie over gegevens die zijn opgeslagen in HBase, het volgende artikel wordt beschreven hoe u met Apache Spark-query's uitvoeren.

> [!div class="nextstepaction"]
> [Apache Spark gebruiken voor Apache HBase-gegevens lezen en schrijven](../hdinsight-using-spark-query-hbase.md)