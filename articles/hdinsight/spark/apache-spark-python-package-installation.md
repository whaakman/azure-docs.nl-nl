---
title: Script action - pakketten voor Python installeren met Jupyter op Azure HDInsight
description: Stapsgewijze instructies over het gebruik van scriptactie Jupyter-notebooks met HDInsight Spark-clusters configureren om te gebruiken externe python-pakketten.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/06/2018
ms.author: hrasheed
ms.openlocfilehash: af25dcff2302827f2291d50972f09b8b5fda6cd3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255435"
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Scriptactie gebruiken voor het installeren van externe Python-pakketten voor Jupyter-notebooks in Apache Spark-clusters in HDInsight
> [!div class="op_single_selector"]
> * [Met behulp van Magic-pakket voor cellen](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Met behulp van scriptacties](apache-spark-python-package-installation.md)
>
>

Meer informatie over het gebruik van scriptacties voor het configureren van een Apache Spark-cluster op HDInsight (Linux) te gebruiken externe, door de community geleverde **python** pakketten die niet zijn opgenomen out-of-the-box in het cluster.

> [!NOTE]
> U kunt ook een Jupyter-notebook configureren met behulp van `%%configure` Magic-pakket naar externe pakketten gebruiken. Zie voor instructies [externe pakketten gebruiken met Jupyter-notebooks in Apache Spark-clusters in HDInsight](apache-spark-jupyter-notebook-use-external-packages.md).
> 
> 

U kunt zoeken naar de [package index](https://pypi.python.org/pypi) voor de volledige lijst met pakketten die beschikbaar zijn. U kunt ook een lijst met beschikbare pakketten ophalen uit andere bronnen. U kunt bijvoorbeeld pakketten beschikbaar gesteld via installeren [Anaconda](https://docs.continuum.io/anaconda/pkg-docs) of [conda-forge](https://conda-forge.org/feedstocks/).

In dit artikel leert u hoe u voor het installeren van de [TensorFlow](https://www.tensorflow.org/) verpakt met Script Action op het cluster en deze via de Jupyter-notebook gebruiken als voorbeeld.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende:

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

   > [!NOTE]
   > Als u een Spark-cluster in HDInsight Linux niet al hebt, kunt u scriptacties uitvoeren tijdens het maken van een cluster. Ga naar de documentatie op [over het gebruik van aangepaste scriptacties](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   > 
   > 
   
   ## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Ondersteuning voor open source-software die wordt gebruikt op HDInsight-clusters

De Microsoft Azure HDInsight-service maakt gebruik van een ecosysteem van open-source-technologieën die zijn gevormd rond Hadoop. Microsoft Azure biedt een algemeen niveau van ondersteuning voor open-source technologieën. Zie voor meer informatie de **reikwijdte van ondersteuning** sectie van de [ondersteuning Veelgestelde vragen over Azure-website](https://azure.microsoft.com/support/faq/). De HDInsight-service biedt een extra beveiligingsniveau van ondersteuning voor ingebouwde onderdelen.

Er zijn twee typen van open source-componenten die beschikbaar in de HDInsight-service zijn:

* **Ingebouwde onderdelen** -deze onderdelen vooraf zijn geïnstalleerd op HDInsight-clusters en bieden de kernfunctionaliteit van het cluster. Bijvoorbeeld, behoren YARN ResourceManager, de Hive-query-taal (HiveQL) en de Mahout-bibliotheek tot deze categorie. Een volledige lijst van clusteronderdelen van het is beschikbaar in [wat is er nieuw in de Hadoop-clusterversies geleverd door HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-component-versioning).
* **Aangepaste onderdelen** -u, als een gebruiker van het cluster kunt installeren of gebruiken in uw werkbelasting een onderdeel is beschikbaar in de community of door u gemaakte.

> [!WARNING]
> Onderdelen van het HDInsight-cluster worden volledig ondersteund. Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen commercieel redelijke ondersteuning om het probleem verder oplossen met u te helpen te ontvangen. Microsoft-ondersteuning kan mogelijk zijn om het probleem te verhelpen of ze kunnen vragen, moet u contact opnemen met beschikbare kanalen voor de open source-technologieën waar uitgebreide expertise voor deze technologie kan worden gevonden. Bijvoorbeeld, er zijn veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ http://stackoverflow.com ](http://stackoverflow.com). Ook Apache-projecten project-sites hebben op [ http://apache.org ](http://apache.org), bijvoorbeeld: [Hadoop](http://hadoop.apache.org/).


## <a name="use-external-packages-with-jupyter-notebooks"></a>Externe pakketten gebruiken met Jupyter-notebooks

1. Klik vanuit de [Azure Portal](https://portal.azure.com/), vanaf het startboard, op de tegel voor uw Spark-cluster (als u deze aan het startboard hebt vastgemaakt). U kunt ook naar uw cluster navigeren onder **Bladeren** > **HDInsight-clusters**.   

2. Klik in de blade Spark-cluster op **scriptacties** in het linkerdeelvenster. Gebruik het scripttype 'Aangepaste' en voer een beschrijvende naam voor de scriptactie. Het script uitvoert op de **hoofd- en worker-knooppunten** en de parameters-veld leeg laten. De bash-script kan worden verwezen vanuit: https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh gaat u naar de documentatie op [over het gebruik van aangepaste scriptacties](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

   > [!NOTE]
   > Er zijn twee python-installaties in het cluster. Spark gebruiken de Anaconda python-installatie zich bevindt in `/usr/bin/anaconda/bin`. Verwijzen naar die installatie in uw aangepaste acties via `/usr/bin/anaconda/bin/pip` en `/usr/bin/anaconda/bin/conda`.
   > 
   > 

3. Een PySpark-Jupyter-notebook openen

    ![Een nieuwe Jupyter-notebook maken](./media/apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Een nieuwe Jupyter-notebook maken")

4. Er wordt een nieuwe notebook gemaakt en geopend met de naam Untitled.pynb. Klik bovenaan op de naam van de notebook en wijzig deze in een beschrijvende naam.

    ![Een naam opgeven voor de notebook](./media/apache-spark-python-package-installation/hdinsight-spark-name-notebook.png "Een naam opgeven voor de notebook")

5. U wordt nu `import tensorflow` en een hello world-voorbeeld uitvoeren. 

    De code te kopiëren:

        import tensorflow as tf
        hello = tf.constant('Hello, TensorFlow!')
        sess = tf.Session()
        print(sess.run(hello))

    Het resultaat ziet er zo uit:
    
    ![Uitvoering van code TensorFlow](./media/apache-spark-python-package-installation/execution.png "TensorFlow uitvoeren van code")

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
* [Externe pakketten gebruiken met Jupyter-notebooks in Apache Spark-clusters in HDInsight](apache-spark-jupyter-notebook-use-external-packages.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
