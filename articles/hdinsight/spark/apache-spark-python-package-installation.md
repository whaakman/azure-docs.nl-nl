---
title: Script action - pakketten voor Python installeren met Jupyter op Azure HDInsight
description: Stapsgewijze instructies over het gebruik van scriptactie Jupyter-notebooks met HDInsight Spark-clusters configureren om te gebruiken externe python-pakketten.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: hrasheed
ms.openlocfilehash: 8bc44949d804349de37796a2695edbdc64693edf
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518674"
---
# <a name="use-script-action-to-install-external-python-packages-for-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Scriptactie gebruiken voor het installeren van externe Python-pakketten voor Jupyter-notebooks in Apache Spark-clusters in HDInsight
> [!div class="op_single_selector"]
> * [Met behulp van Magic-pakket voor cellen](apache-spark-jupyter-notebook-use-external-packages.md)
> * [Met behulp van scriptacties](apache-spark-python-package-installation.md)

Meer informatie over het gebruik scriptacties om te configureren van een [Apache Spark](https://spark.apache.org/) -cluster in HDInsight gebruiken voor externe, door de community geleverde **python** pakketten die niet zijn opgenomen out-of-the-box in het cluster.

> [!NOTE]  
> U kunt ook een Jupyter-notebook configureren met behulp van `%%configure` Magic-pakket naar externe pakketten gebruiken. Zie voor instructies [externe pakketten gebruiken met Jupyter-notebooks in Apache Spark-clusters in HDInsight](apache-spark-jupyter-notebook-use-external-packages.md).

U kunt zoeken naar de [package index](https://pypi.python.org/pypi) voor de volledige lijst met pakketten die beschikbaar zijn. U kunt ook een lijst met beschikbare pakketten ophalen uit andere bronnen. U kunt bijvoorbeeld pakketten beschikbaar gesteld via installeren [conda-forge](https://conda-forge.org/feedstocks/).

In dit artikel leert u hoe u voor het installeren van de [TensorFlow](https://www.tensorflow.org/) verpakt met Script Action op het cluster en deze via de Jupyter-notebook gebruiken als voorbeeld.

## <a name="prerequisites"></a>Vereisten
U hebt het volgende:

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

   > [!NOTE]  
   > Als u een Spark-cluster in HDInsight Linux niet al hebt, kunt u scriptacties uitvoeren tijdens het maken van een cluster. Ga naar de documentatie op [over het gebruik van aangepaste scriptacties](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).
   
## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>Ondersteuning voor open source-software die wordt gebruikt op HDInsight-clusters

De Microsoft Azure HDInsight-service maakt gebruik van een ecosysteem van open-source-technologieën die zijn gevormd rond Apache Hadoop. Microsoft Azure biedt een algemeen niveau van ondersteuning voor open-source technologieën. Zie voor meer informatie de **reikwijdte van ondersteuning** sectie van de [ondersteuning Veelgestelde vragen over Azure-website](https://azure.microsoft.com/support/faq/). De HDInsight-service biedt een extra beveiligingsniveau van ondersteuning voor ingebouwde onderdelen.

Er zijn twee typen van open source-componenten die beschikbaar in de HDInsight-service zijn:

* **Ingebouwde onderdelen** -deze onderdelen vooraf zijn geïnstalleerd op HDInsight-clusters en bieden de kernfunctionaliteit van het cluster. Bijvoorbeeld, behoren Apache Hadoop YARN ResourceManager, de taal in Apache Hive-query (HiveQL) en de Mahout-bibliotheek tot deze categorie. Een volledige lijst van clusteronderdelen van het is beschikbaar in [wat is er nieuw in het Apache Hadoop-clusterversies geleverd door HDInsight](https://docs.microsoft.com/azure/hdinsight/hdinsight-component-versioning).
* **Aangepaste onderdelen** -u, als een gebruiker van het cluster kunt installeren of gebruiken in uw werkbelasting een onderdeel is beschikbaar in de community of door u gemaakte.

> [!IMPORTANT]   
> Onderdelen van het HDInsight-cluster worden volledig ondersteund. Microsoft Support helpt bij het opsporen en oplossen van problemen met betrekking tot deze onderdelen.
>
> Aangepaste onderdelen commercieel redelijke ondersteuning om het probleem verder oplossen met u te helpen te ontvangen. Microsoft-ondersteuning kan mogelijk zijn om het probleem te verhelpen of ze kunnen vragen, moet u contact opnemen met beschikbare kanalen voor de open source-technologieën waar uitgebreide expertise voor deze technologie kan worden gevonden. Er zijn bijvoorbeeld veel communitysites die kunnen worden gebruikt, zoals: [MSDN-forum voor HDInsight](https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight), [ https://stackoverflow.com ](https://stackoverflow.com). Ook Apache-projecten project-sites hebben op [ https://apache.org ](https://apache.org), bijvoorbeeld: [Hadoop](https://hadoop.apache.org/).


## <a name="use-external-packages-with-jupyter-notebooks"></a>Externe pakketten gebruiken met Jupyter-notebooks

1. Uit de [Azure-portal](https://portal.azure.com/), gaat u naar het cluster.  

2. Met uw cluster is geselecteerd, in het linkerdeelvenster onder **instellingen**, selecteer **scriptacties**.

3. Selecteer **+ nieuwe indienen**.

4. Voer de volgende waarden voor de **scriptactie verzenden** venster:  


    |Parameter | Value |
    |---|---|
    |Scripttype | Selecteer **: aangepaste** uit de vervolgkeuzelijst.|
    |Name |Voer `tensorflow` in het tekstvak in.|
    |Bash-script-URI |Voer `https://hdiconfigactions.blob.core.windows.net/linuxtensorflow/tensorflowinstall.sh` in het tekstvak in. |
    |Knooppunttype(n) | Selecteer de **Head**, en **Worker** selectievakjes. |

    `tensorflowinstall.sh` bevat de volgende opdrachten:

    ```bash
    #!/usr/bin/env bash
    /usr/bin/anaconda/bin/conda install -c conda-forge tensorflow
    ```

5. Selecteer **Maken**.  Ga naar de documentatie op [over het gebruik van aangepaste scriptacties](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux).

6. Wacht tot het script te voltooien.  De **scriptacties** deelvenster wordt status **nieuwe scriptacties kunnen worden verzonden nadat de huidige Clusterbewerking is voltooid** terwijl het script wordt uitgevoerd.  Een voortgangsbalk kan worden bekeken via de UI Ambari **bewerkingen op de achtergrond** venster.

7. Open een PySpark-Jupyter-notebook.  Zie [maken van een Jupyter-notebook in Spark HDInsight](./apache-spark-jupyter-notebook-kernels.md#create-a-jupyter-notebook-on-spark-hdinsight) voor stappen.

    ![Een nieuwe Jupyter-notebook maken](./media/apache-spark-python-package-installation/hdinsight-spark-create-notebook.png "Een nieuwe Jupyter-notebook maken")

8. U wordt nu `import tensorflow` en een hello world-voorbeeld uitvoeren. Voer de volgende code in:

    ```
    import tensorflow as tf
    hello = tf.constant('Hello, TensorFlow!')
    sess = tf.Session()
    print(sess.run(hello))
    ```

    Het resultaat ziet er zo uit:
    
    ![Uitvoering van code TensorFlow](./media/apache-spark-python-package-installation/execution.png "TensorFlow uitvoeren van code")

> [!NOTE]  
> Er zijn twee python-installaties in het cluster. Spark gebruiken de Anaconda python-installatie zich bevindt in `/usr/bin/anaconda/bin` en standaard op de omgeving voor Python 2.7. Aan de pakketten voor Python 3.x en installeren in de kernel PySpark3, gebruikt u het pad naar de `conda` uitvoerbare voor die omgeving en gebruik de `-n` parameter opgeven voor de omgeving. Bijvoorbeeld, de opdracht `/usr/bin/anaconda/envs/py35/bin/conda install -c conda-forge ggplot -n py35`, installeert de `ggplot` pakket met de Python 3.5-omgeving via het `conda-forge` kanaal.

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Apache Spark met BI: Interactieve gegevensanalyses met behulp van Spark in HDInsight met BI-hulpprogramma's uitvoeren](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor de resultaten van voedingsinspectie voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Websitelogboekanalyse met Apache Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Apache Livy gebruiken om taken op afstand uit te voeren in een Apache Spark-cluster](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [Externe pakketten gebruiken met Jupyter-notebooks in Apache Spark-clusters in HDInsight](apache-spark-jupyter-notebook-use-external-packages.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op te sporen Apache Spark-toepassingen op afstand](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Apache Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Jupyter op uw computer installeren en verbinding maken met een HDInsight Spark-cluster](apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
