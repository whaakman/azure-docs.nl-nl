---
title: 'Quickstart: Maken en controleren van Apache Storm-topologie in Azure HDInsight'
description: In deze quickstart leert u hoe u maken en controleren van een Apache Storm-topologie in Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.date: 06/14/2019
ms.author: hrasheed
ms.custom: mvc
ms.openlocfilehash: 49da6ad20b29193f0430a66658f1bf80625704b3
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274126"
---
# <a name="quickstart-create-and-monitor-an-apache-storm-topology-in-azure-hdinsight"></a>Quickstart: Maken en bewaken van een Apache Storm-topologie in Azure HDInsight

Apache Storm is een gedistribueerd, schaalbaar, fouttolerant en realtime berekeningssysteem voor het verwerken van gegevensstromen. Met Storm in Azure HDInsight kunt u een op een cloud gebaseerd Storm-cluster maken dat in realtime big data-analyses uitvoert.

In deze quickstart maakt u een voorbeeld van de Apache [storm starter-](https://github.com/apache/storm/tree/v2.0.0/examples/storm-starter) project maken en controleren van een Apache Storm-topologie aan een bestaand Apache Storm-cluster.

## <a name="prerequisites"></a>Vereisten

* Een Apache Storm-cluster in HDInsight. Zie [Apache Hadoop-clusters maken met behulp van de Azure-portal](../hdinsight-hadoop-create-linux-clusters-portal.md) en selecteer **Storm** voor **clustertype**.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-the-topology"></a>Maken van de topologie

1. Verbinding maken met uw Storm-cluster. De onderstaande opdracht bewerken door te vervangen `CLUSTERNAME` met de naam van uw Storm-cluster en voer vervolgens de opdracht:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

2. De **WordCount** voorbeeld is opgenomen in uw HDInsight-cluster op `/usr/hdp/current/storm-client/contrib/storm-starter/`. De topologie genereert willekeurige zinnen en telt hoe vaak woorden voorkomen. Gebruik de volgende opdracht om te beginnen de **wordcount** topologie op het cluster:

    ```bash
    storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount
    ```

## <a name="monitor-the-topology"></a>De topologie bewaken

Storm biedt een webinterface voor het werken met actieve topologieën en is opgenomen in uw HDInsight-cluster.

Voer de volgende stappen uit voor het bewaken van de topologie met behulp van de Storm-gebruikersinterface:

1. Als u de Storm-gebruikersinterface wilt weergeven, opent u `https://CLUSTERNAME.azurehdinsight.net/stormui` in een webbrowser. Vervang `CLUSTERNAME` door de naam van uw cluster.

2. Onder **Topology Summary**, selecteer de **wordcount** vermelding in de **naam** kolom. Er wordt informatie over de topologie weergegeven.

    ![Storm-dashboard met informatie over de Storm-Starter WordCount-topologie.](./media/apache-storm-quickstart/topology-summary.png)

    De nieuwe pagina bevat de volgende informatie:

    |Eigenschap | Description |
    |---|---|
    |Topology stats|Algemene informatie over de topologieprestaties, geordend in tijdvensters. Wanneer er een specifiek tijdvenster wordt geselecteerd, verandert het tijdvenster voor informatie die wordt weergegeven in andere gedeelten van de pagina.|
    |Spouts|Basisinformatie over spouts, met inbegrip van de laatste fout die door elke spout is geretourneerd.|
    |Bolts|Basisinformatie over bolts.|
    |Topologieconfiguratie|Gedetailleerde informatie over de topologieconfiguratie van de.|
    |Activeren|Hervat de verwerking van een gedeactiveerde topologie.|
    |deactiveren|Een actieve topologie wordt onderbroken.|
    |Opnieuw verdelen|Hiermee past u de parallelle uitvoering van de topologie. Nadat u het aantal knooppunten in het cluster hebt gewijzigd, moet u actieve topologieën opnieuw verdelen. Met het opnieuw verdelen wordt de parallelle uitvoering aangepast om te compenseren voor het toegenomen/afgenomen aantal knooppunten in het cluster. Zie voor meer informatie, [inzicht in de parallelle uitvoering van een Apache Storm-topologie](https://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).|
    |KILL-instructie|Een Storm-topologie beëindigd na de opgegeven time-out.|

3. Selecteer op deze pagina een item in de sectie **Spouts** of **Bolts**. Er wordt informatie over het geselecteerde onderdeel weergegeven.

    ![Storm-dashboard met informatie over de geselecteerde onderdelen.](./media/apache-storm-quickstart/component-summary.png)

    De nieuwe pagina bevat de volgende informatie:

    |Eigenschap | Description |
    |---|---|
    |Spout/Bolt stats|Basisinformatie over de prestaties van onderdelen, geordend in tijdvensters. Wanneer er een specifiek tijdvenster wordt geselecteerd, verandert het tijdvenster voor informatie die wordt weergegeven in andere gedeelten van de pagina.|
    |Input stats (alleen bolt)|Informatie over de onderdelen die leiden gegevens die worden gebruikt door de bolt tot.|
    |Output stats|Informatie over de gegevens die door deze bolt zijn gegenereerd.|
    |Executor|Informatie over de exemplaren van dit onderdeel.|
    |Fouten|Fouten die worden geproduceerd door dit onderdeel.|

4. Als u details voor een specifiek exemplaar van het onderdeel wilt weergeven, selecteert u een item in de kolom **Port** in de sectie **Executor** terwijl u de details van een spout of bolt bekijkt.

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    In dit voorbeeld komt het woord **seven** 1.493.957 keer voor. Dit getal is het aantal keer dat het woord is gevonden sinds deze topologie is gestart.

## <a name="stop-the-topology"></a>De topologie stoppen

Ga terug naar de pagina **Topology summary** voor de word-count-topologie en klik vervolgens op de knop **Kill** in de sectie **Topology actions**. Wanneer hierom wordt gevraagd, voert u 10 in voor het aantal seconden dat moet worden gewacht voordat de topologie wordt gestopt. Na de time-outperiode wordt de topologie niet meer weergegeven als u het gedeelte **Storm UI** van het dashboard bezoekt.

## <a name="clean-up-resources"></a>Resources opschonen

Nadat u de Quick Start hebt voltooid, kunt u het cluster verwijdert. Met HDInsight worden uw gegevens opgeslagen in Azure Storage zodat u een cluster veilig kunt verwijderen wanneer deze niet wordt gebruikt. Voor een HDInsight-cluster worden ook kosten in rekening gebracht, zelfs wanneer het niet wordt gebruikt. Aangezien de kosten voor het cluster vaak zoveel hoger zijn dan de kosten voor opslag, is het financieel gezien logischer clusters te verwijderen wanneer ze niet worden gebruikt.

Als u wilt verwijderen van een cluster, Zie [verwijderen van een HDInsight-cluster met behulp van uw browser, PowerShell of Azure CLI](../hdinsight-delete-cluster.md).

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gebruikt u een voorbeeld van de Apache [storm starter-](https://github.com/apache/storm/tree/v2.0.0/examples/storm-starter) project maken en controleren van een Apache Storm-topologie aan een bestaand Apache Storm-cluster. Ga naar het volgende artikel voor meer informatie over de basisbeginselen van het beheren en controleren van Apache Storm-topologieën.

> [!div class="nextstepaction"]
>[Apache Storm-topologieën op Azure HDInsight implementeren en beheren ](./apache-storm-deploy-monitor-topology-linux.md)