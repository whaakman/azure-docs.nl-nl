---
title: Aangepaste Maven-pakketten gebruiken met Jupyter in Spark op Azure HDInsight
description: Stapsgewijze instructies over het configureren van Jupyter-notebooks met HDInsight Spark-clusters te gebruiken van aangepaste Maven-pakketten.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: jasonh
ms.openlocfilehash: 51099f64546acc6f18269b2e7ec05106bb3baa2d
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39622028"
---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Externe pakketten gebruiken met Jupyter-notebooks in Apache Spark-clusters in HDInsight
> [!div class="op_single_selector"]
> * [Met behulp van Magic-pakket voor cellen](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Met behulp van scriptacties](apache-spark-python-package-installation.md)
>
>

Informatie over het configureren van een Jupyter-notebook in Apache Spark-cluster in HDInsight gebruiken voor externe, door de community geleverde **maven** pakketten die niet zijn opgenomen out-of-the-box in het cluster. 

U kunt zoeken naar de [Maven-opslagplaats](http://search.maven.org/) voor de volledige lijst met pakketten die beschikbaar zijn. U kunt ook een lijst met beschikbare pakketten ophalen uit andere bronnen. Bijvoorbeeld, een volledige lijst van door de community geleverde pakketten is beschikbaar op [Spark pakketten](http://spark-packages.org/).

In dit artikel leert u hoe u de [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) pakket met de Jupyter-notebook.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende:

* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

## <a name="use-external-packages-with-jupyter-notebooks"></a>Externe pakketten gebruiken met Jupyter-notebooks
1. Klik vanuit de [Azure Portal](https://portal.azure.com/), vanaf het startboard, op de tegel voor uw Spark-cluster (als u deze aan het startboard hebt vastgemaakt). U kunt ook naar uw cluster navigeren onder **Bladeren** > **HDInsight-clusters**.   

1. Klik vanuit de blade Spark-cluster op **Snelkoppelingen**. Klik vervolgens vanuit het **Cluster-dashboard** op **Jupyter Notebook**. Voer de beheerdersreferenties voor het cluster in als u daarom wordt gevraagd.

    > [!NOTE]
    > Mogelijk bereikt u de Jupyter-notebook voor uw cluster ook door de volgende URL in uw browser te openen. Vervang **CLUSTERNAME** door de naam van uw cluster.
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

1. Maak een nieuwe notebook. Klik op **nieuw**, en klik vervolgens op **Spark**.
   
    ![Een nieuwe Jupyter-notebook maken](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-create-notebook.png "Een nieuwe Jupyter-notebook maken")

1. Er wordt een nieuwe notebook gemaakt en geopend met de naam Untitled.pynb. Klik bovenaan op de naam van de notebook en wijzig deze in een beschrijvende naam.
   
    ![Een naam opgeven voor de notebook](./media/apache-spark-jupyter-notebook-use-external-packages/hdinsight-spark-name-notebook.png "Een naam opgeven voor de notebook")

1. U gebruikt de `%%configure` Magic-pakket voor het configureren van de notebook voor het gebruik van een extern pakket. Zorg ervoor dat u aanroepen in notitieblokken die externe pakketten gebruiken, de `%%configure` magic in de eerste codecel. Dit zorgt ervoor dat de kernel is geconfigureerd voor het gebruik van het pakket voordat de sessie wordt gestart.

    >[!IMPORTANT] 
    >Als u het configureren van de kernel in de eerste cel vergeet, kunt u de `%%configure` met de `-f` parameter, maar die wordt de sessie opnieuw starten en alle gaan verloren.

    | HDInsight-versie | Opdracht |
    |-------------------|---------|
    |Voor HDInsight 3.3 en 3.4 HDInsight | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | Voor HDInsight 3.5 en HDInsight 3.6 | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

1. Het bovenstaande fragment wordt verwacht dat de maven-coördinaten voor de externe-pakket in Maven Central Repository. In dit fragment `com.databricks:spark-csv_2.10:1.4.0` is de maven-coördinaat voor **spark-csv** pakket. Hier ziet u hoe u de coördinaten voor een pakket samenstelt.
   
    a. Het pakket niet vinden in de Maven-opslagplaats. Voor deze zelfstudie gebruiken we [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Verzamel uit de opslagplaats en de waarden voor **groeps-id**, **ArtifactId**, en **versie**. Zorg ervoor dat de waarden die u verzamelt die overeenkomen met uw cluster. In dit geval gebruiken we een 2.10 Scala en Spark 1.4.0-pakket, maar u moet mogelijk verschillende versies voor de juiste Scala of Spark-versie te selecteren in uw cluster. U vindt de Scala-versie op het cluster door uit te voeren `scala.util.Properties.versionString` op de kernel Spark Jupyter of Spark indienen. U vindt de Spark-versie op het cluster door uit te voeren `sc.version` op Jupyter-notebooks.
   
    ![Externe pakketten gebruiken met Jupyter-notebook](./media/apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "externe pakketten gebruiken met Jupyter-notebook")
   
    c. De drie waarden, gescheiden door een dubbele punt (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

1. Voer de codecel met de `%%configure` Magic-pakket. Hiermee configureert u de onderliggende Livy-sessie voor het gebruik van het pakket dat u hebt opgegeven. U kunt nu het pakket gebruiken in de volgende cellen in het notitieblok, zoals hieronder wordt weergegeven.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

    Voor HDInsight 3.6, moet u het volgende codefragment.

        val df = spark.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

1. Vervolgens kunt u uitvoeren de codefragmenten, zoals hieronder wordt weergegeven tot de gegevens uit het dataframe dat u in de vorige stap hebt gemaakt.
   
        df.show()
   
        df.select("Time").count()

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen

* [Externe python-pakketten gebruiken met Jupyter-notebooks in Apache Spark-clusters in HDInsight Linux](apache-spark-python-package-installation.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
