---
title: Resources beheren voor Apache Spark-cluster in Azure HDInsight
description: Meer informatie over het gebruik van resources beheren voor Spark-clusters op Azure HDInsight voor betere prestaties.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/23/2018
ms.author: hrasheed
ms.openlocfilehash: 166d52322504e57e01a4bd798252fc956c1a3e44
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014581"
---
# <a name="manage-resources-for-apache-spark-cluster-on-azure-hdinsight"></a>Resources beheren voor Apache Spark-cluster in Azure HDInsight 

Informatie over toegang tot de interfaces, zoals Ambari UI, YARN-gebruikersinterface en de Spark-geschiedenis-Server die is gekoppeld aan uw Spark-cluster en het afstemmen van de clusterconfiguratie voor optimale prestaties.

**Vereisten:**

* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

## <a name="open-the-ambari-web-ui"></a>Open de Ambari-Webgebruikersinterface

Apache Ambari is gebruikt om te controleren van het cluster en configuratiewijzigingen aanbrengen. Zie voor meer informatie, [beheren van Hadoop-clusters in HDInsight met behulp van Azure portal](../hdinsight-administer-use-portal-linux.md#open-the-ambari-web-ui)

## <a name="open-the-spark-history-server"></a>Open de Spark-geschiedenis-Server

Spark geschiedenis-Server is de web-UI voor Spark-toepassingen is voltooid en wordt uitgevoerd. Er is een uitbreiding van de Spark-Webgebruikersinterface.

**De Webgebruikersinterface van Spark geschiedenis Server openen**

1. Uit de [Azure-portal](https://portal.azure.com/), opent u het Spark-cluster. Zie voor meer informatie, [clusters tonen en vermelden](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. Van **snelkoppelingen**, klikt u op **Clusterdashboard**, en klik vervolgens op **Spark geschiedenis-Server**

    ![Spark-geschiedenis Server](./media/apache-spark-resource-manager/launch-history-server.png "Spark-geschiedenis-Server")

    Wanneer u hierom wordt gevraagd, voert u de beheerdersreferenties voor de Spark-cluster. U kunt de geschiedenis van Spark Server ook openen door te bladeren naar de volgende URL:

    ```
    https://<ClusterName>.azurehdinsight.net/sparkhistory
    ```

    Vervang <ClusterName> met de naam van uw Spark-cluster.

De Spark-geschiedenis Server web UI ziet eruit zoals:

![Server voor HDInsight Spark-geschiedenis](./media/apache-spark-resource-manager/hdinsight-spark-history-server.png)

## <a name="open-the-yarn-ui"></a>Open de Yarn-gebruikersinterface
U kunt de gebruikersinterface van YARN gebruiken voor het bewaken van toepassingen die momenteel worden uitgevoerd op het Spark-cluster.

1. Uit de [Azure-portal](https://portal.azure.com/), opent u het Spark-cluster. Zie voor meer informatie, [clusters tonen en vermelden](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. Van **snelkoppelingen**, klikt u op **Clusterdashboard**, en klik vervolgens op **YARN**.

    ![Gebruikersinterface van YARN starten](./media/apache-spark-resource-manager/launch-yarn-ui.png)

   > [!TIP]
   > U kunt ook kunt u ook de YARN-gebruikersinterface van de Ambari UI starten. Als u wilt de UI Ambari starten, klikt u op **Clusterdashboard**, en klik vervolgens op **HDInsight-Clusterdashboard**. Klik op de UI Ambari **YARN**, klikt u op **snelkoppelingen**, klikt u op de actieve Resource Manager en klik vervolgens op **Resource Manager UI**.
   >
   >

## <a name="optimize-clusters-for-spark-applications"></a>Clusters voor Spark-toepassingen optimaliseren
De drie belangrijkste parameters die kunnen worden gebruikt voor de configuratie van Spark, afhankelijk van de vereisten van webtoepassingen zijn `spark.executor.instances`, `spark.executor.cores`, en `spark.executor.memory`. Een Executor is een proces gestart voor een Spark-toepassing. Het wordt uitgevoerd op de worker-knooppunt en is verantwoordelijk voor het uitvoeren van de taken voor de toepassing. Het standaardaantal Executor en de grootte van de executor voor elk cluster wordt berekend op basis van het aantal worker-knooppunten en de grootte van het worker-knooppunt. Deze informatie wordt opgeslagen in `spark-defaults.conf` over de hoofdknooppunten van het cluster.

De van de drie configuratieparameters kunnen worden geconfigureerd op het niveau van het cluster (voor alle toepassingen die worden uitgevoerd op het cluster) of voor elke afzonderlijke toepassing en kunnen worden opgegeven.

### <a name="change-the-parameters-using-ambari-ui"></a>De parameters met behulp van Ambari UI wijzigen
1. Van de Ambari UI-Klik **Spark**, klikt u op **Peeringconfiguraties**, en vouw vervolgens **aangepaste spark-standaardinstellingen**.

    ![Parameters instellen met behulp van Ambari](./media/apache-spark-resource-manager/set-parameters-using-ambari.png)
2. De standaardwaarden zijn goede vier Spark-toepassingen gelijktijdig worden uitgevoerd op het cluster. U kunt deze waarden wijzigen via de gebruikersinterface, zoals wordt weergegeven in de volgende schermafbeelding:

    ![Parameters instellen met behulp van Ambari](./media/apache-spark-resource-manager/set-executor-parameters.png)
3. Klik op **opslaan** om op te slaan van de configuratiewijzigingen. U wordt gevraagd aan de bovenkant van de pagina alle betrokken services opnieuw starten. Klik op **Opnieuw opstarten**.

    ![Services opnieuw starten](./media/apache-spark-resource-manager/restart-services.png)

### <a name="change-the-parameters-for-an-application-running-in-jupyter-notebook"></a>Wijzigen van de parameters voor een toepassing die wordt uitgevoerd in Jupyter-notebook
Voor toepassingen die worden uitgevoerd in de Jupyter-notebook, kunt u de `%%configure` Magic-pakket voor de configuratiewijzigingen aanbrengen. In het ideale geval moet u deze wijzigingen aanbrengen aan het begin van de toepassing, voordat u uw eerste codecel uitvoert. Dit zorgt ervoor dat de configuratie wordt toegepast op de Livy-sessie, wanneer deze wordt gemaakt. Als u wilt dat de configuratie op een later tijdstip in de toepassing te wijzigen, moet u de `-f` parameter. Op deze manier is alle wordt uitgevoerd in de toepassing echter verloren gaan.

Het volgende codefragment laat zien hoe de configuratie voor een toepassing die wordt uitgevoerd in Jupyter te wijzigen.

    %%configure
    {"executorMemory": "3072M", "executorCores": 4, "numExecutors":10}

Parameters voor de configuratie moeten worden doorgegeven als een JSON-tekenreeks en moeten op de volgende regel na de Magic-pakket, zoals wordt weergegeven in de voorbeeldkolom.

### <a name="change-the-parameters-for-an-application-submitted-using-spark-submit"></a>De parameters voor een toepassing die wordt verzonden met behulp van spark-submit wijzigen
Na de opdracht is een voorbeeld van het wijzigen van de parameters voor de configuratie voor een batch-toepassing die wordt verzonden met behulp van `spark-submit`.

    spark-submit --class <the application class to execute> --executor-memory 3072M --executor-cores 4 –-num-executors 10 <location of application jar file> <application parameters>

### <a name="change-the-parameters-for-an-application-submitted-using-curl"></a>Wijzigen van de parameters voor een toepassing die wordt verzonden met cURL
De volgende opdracht wordt een voorbeeld van hoe u kunt de parameters voor de configuratie voor een batch-toepassing die wordt verzonden met cURL wijzigen.

    curl -k -v -H 'Content-Type: application/json' -X POST -d '{"file":"<location of application jar file>", "className":"<the application class to execute>", "args":[<application parameters>], "numExecutors":10, "executorMemory":"2G", "executorCores":5' localhost:8998/batches

### <a name="change-these-parameters-on-a-spark-thrift-server"></a>Deze parameters op een Spark Thrift-Server wijzigen
Spark Thrift-Server biedt JDBC/ODBC-toegang tot een Spark-cluster en wordt gebruikt voor service Spark SQL-query's. Hulpprogramma's zoals Power BI, Tableau enzovoort. ODBC-protocol gebruiken om te communiceren met Spark Thrift-Server voor het uitvoeren van Spark SQL-query's als een Spark-toepassing. Als u een Spark-cluster maakt, twee exemplaren van de Spark Thrift-Server zijn gestart, één op elke hoofdknooppunt. Elk Spark Thrift-Server wordt weergegeven als een Spark-toepassing in de gebruikersinterface van YARN.

Spark Thrift-Server maakt gebruik van Spark dynamische executor-toewijzing en wordt daarmee de `spark.executor.instances` wordt niet gebruikt. In plaats daarvan, Spark Thrift-Server gebruikt `spark.dynamicAllocation.minExecutors` en `spark.dynamicAllocation.maxExecutors` om op te geven van het aantal executor. De configuratieparameters `spark.executor.cores` en `spark.executor.memory` wordt gebruikt om de executor-grootte te wijzigen. U kunt deze parameters kunt wijzigen, zoals wordt weergegeven in de volgende stappen uit:

* Vouw de **geavanceerde spark-thrift-sparkconf** categorie om bij te werken van de parameters `spark.dynamicAllocation.minExecutors`, `spark.dynamicAllocation.maxExecutors`, en `spark.executor.memory`.

    ![Spark thrift-server configureren](./media/apache-spark-resource-manager/spark-thrift-server-1.png)    
* Vouw de **aangepaste spark-thrift-sparkconf** categorie om bij te werken met de parameter `spark.executor.cores`.

    ![Spark thrift-server configureren](./media/apache-spark-resource-manager/spark-thrift-server-2.png)

### <a name="change-the-driver-memory-of-the-spark-thrift-server"></a>Wijzig het stuurprogramma-geheugen van de Spark Thrift-Server
Geheugen Spark Thrift-Server is geconfigureerd op 25% van het hoofdknooppunt RAM-geheugen, opgegeven dat het totale RAM-geheugen van het hoofdknooppunt is groter dan 14 GB. Zoals wordt weergegeven in de volgende schermafbeelding, kunt u de Ambari UI te wijzigen van de configuratie van het stuurprogramma-geheugen:

* Van de Ambari UI-Klik **Spark**, klikt u op **Peeringconfiguraties**, vouw **geavanceerde spark-env**, en geef vervolgens de waarde voor **spark_thrift_cmd_opts**.

    ![RAM-geheugen voor Spark thrift-server configureren](./media/apache-spark-resource-manager/spark-thrift-server-ram.png)

## <a name="reclaim-spark-cluster-resources"></a>Spark-clusterresources vrij
Vanwege de dynamische toewijzing van Spark zijn de enige resources die worden verbruikt door thrift-server de resources voor de toepassing twee modellen. Als u wilt vrijmaken van deze resources, moet u de Thrift-Server-services die worden uitgevoerd op het cluster beëindigen.

1. Klik in de UI Ambari, in het linkerdeelvenster op **Spark**.
2. Klik in de volgende pagina op **Spark Thrift Servers**.

    ![Thrift-server opnieuw opstarten](./media/apache-spark-resource-manager/restart-thrift-server-1.png)
3. U ziet de twee hoofdknooppunten waarop de Spark Thrift-Server wordt uitgevoerd. Klik op een van de hoofdknooppunten.

    ![Thrift-server opnieuw opstarten](./media/apache-spark-resource-manager/restart-thrift-server-2.png)
4. De volgende pagina geeft een lijst van alle services die worden uitgevoerd op die het hoofdknooppunt. Klik op de vervolgkeuzelijst naast Spark Thrift-Server in de lijst en klik vervolgens op **stoppen**.

    ![Thrift-server opnieuw opstarten](./media/apache-spark-resource-manager/restart-thrift-server-3.png)
5. Herhaal deze stappen op het andere hoofdknooppunt ook.

## <a name="restart-the-jupyter-service"></a>De Jupyter-service opnieuw starten
Start de Ambari-Webgebruikersinterface, zoals wordt weergegeven in het begin van het artikel. Klik in het linkernavigatiedeelvenster op **Jupyter**, klikt u op **serviceacties**, en klik vervolgens op **start opnieuw alle**. Hiermee start u de Jupyter-service op alle hoofdknooppunten.

![Opnieuw opstarten Jupyter](./media/apache-spark-resource-manager/restart-jupyter.png "Jupyter opnieuw starten")

## <a name="monitor-resources"></a>Resources controleren
Start de gebruikersinterface van Yarn zoals wordt weergegeven in het begin van het artikel. In de tabel van de metrische clustergegevens boven op het scherm, controleert u de waarden van **geheugen gebruikt** en **Totaal geheugen** kolommen. Als de twee waarden sluiten zijn, er mogelijk niet voldoende resources om de volgende toepassing te starten. Dit geldt ook voor de **VCores gebruikt** en **VCores totaal** kolommen. Ook in de hoofdweergave, als er een toepassing zijn gebleven **GEACCEPTEERDE** staat en niet over te stappen naar **uitgevoerd** noch **mislukt** staat, dit kan ook wel een indicatie dat het is niet ophalen van onvoldoende resources om te starten.

![Resourcelimiet](./media/apache-spark-resource-manager/resource-limit.png "resourcelimiet")

## <a name="kill-running-applications"></a>Beëindig actieve toepassingen
1. Klik in de gebruikersinterface van Yarn, in het linkerpaneel **met**. In de lijst van actieve toepassingen, bepalen de toepassing om te worden verwijderd en klik op de **ID**.

    ![App1 kill](./media/apache-spark-resource-manager/kill-app1.png "Kill App1")

2. Klik op **Kill toepassing** in de rechterbovenhoek, klik vervolgens op **OK**.

    ![Kill App2](./media/apache-spark-resource-manager/kill-app2.png "App2 Kill")

## <a name="see-also"></a>Zie ook
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)

### <a name="for-data-analysts"></a>Voor gegevensanalisten

* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)
* [Analyse van Application Insights-telemetriegegevens met behulp van Spark in HDInsight](apache-spark-analyze-application-insight-logs.md)
* [Caffe gebruiken op Azure HDInsight Spark voor gedistribueerde deep learning](apache-spark-deep-learning-caffe.md)

### <a name="for-spark-developers"></a>Voor Spark-ontwikkelaars

* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)
