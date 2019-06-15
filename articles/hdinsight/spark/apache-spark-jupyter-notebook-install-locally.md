---
title: Jupyter lokaal installeren en verbinding maken met Spark in Azure HDInsight
description: Leer hoe u lokaal Jupyter-notebook op uw computer installeren en verbinden met een Apache Spark-cluster.
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/06/2019
ms.author: hrasheed
ms.openlocfilehash: 489685485af4e3c8868f7e0281d2f81464a166f6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066179"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Jupyter-notebook op uw computer installeren en verbinden met Apache Spark in HDInsight

In dit artikel leert u hoe u Jupyter-notebook, met de aangepaste PySpark (voor Python) en Apache Spark (voor Scala) kernels met Spark Magic-pakket installeren en de notebook verbinden met een HDInsight-cluster. Er is een aantal redenen voor het installeren van Jupyter op uw lokale computer en kunnen er ook enkele uitdagingen. Zie de sectie voor meer informatie over dit [waarom moet ik Jupyter installeren op mijn computer](#why-should-i-install-jupyter-on-my-computer) aan het einde van dit artikel.

Er zijn vier belangrijke stappen die betrokken zijn bij het installeren van Jupyter en verbinding te maken met Apache Spark in HDInsight.

* Spark-cluster configureren.
* Installeer de Jupyter-notebook.
* Installeer de PySpark- en Spark-kernels met de Spark-magic.
* Configureer magic voor toegang tot Spark-cluster in HDInsight Spark.

Zie voor meer informatie over de aangepaste kernels en de Spark-magic beschikbaar voor de Jupyter-notebooks met HDInsight-cluster [beschikbare Kernels voor Jupyter-notebooks met Apache Spark Linux-clusters in HDInsight](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Vereisten

De vereisten die hier worden vermeld, zijn niet voor het installeren van Jupyter. Dit zijn voor de Jupyter-notebook verbinding met een HDInsight-cluster nadat het notitieblok is geïnstalleerd.

* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter-notebook op uw computer installeren

Voordat u de Jupyter-notebooks kunt installeren, moet u Python installeren. De [Anaconda distributie](https://www.anaconda.com/download/) installeert, Python, en de Jupyter-Notebook.

Download de [Anaconda installatieprogramma](https://www.anaconda.com/download/) voor uw platform en voer de installatie. Tijdens het uitvoeren van de wizard setup, zorg ervoor dat u selecteert de optie voor het Anaconda toevoegen aan de variabele PATH.  Zie ook [installeren Jupyter met behulp van Anaconda](https://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-spark-magic"></a>Spark Magic-pakket installeren

1. Voer een van de onderstaande opdrachten voor het installeren van Spark-magic. Zie ook [sparkmagic documentatie](https://github.com/jupyter-incubator/sparkmagic#installation).

    |Clusterversie | Installatieopdracht |
    |---|---|
    |V3.6 en v3.5 |`pip install sparkmagic==0.12.7`|
    |v3.4|`pip install sparkmagic==0.2.3`|

1. Zorg ervoor dat `ipywidgets` juist is geïnstalleerd met de volgende opdracht:

    ```cmd
    jupyter nbextension enable --py --sys-prefix widgetsnbextension
    ```

## <a name="install-pyspark-and-spark-kernels"></a>PySpark en Spark-kernels installeren

1. Identificeer waar de `sparkmagic` is geïnstalleerd met de volgende opdracht:

    ```cmd
    pip show sparkmagic
    ```

    Wijzig uw werkmap naar de locatie die is aangeduid met de bovenstaande opdracht.

1. Geef een of meer van de onderstaande opdrachten voor het installeren van de gewenste kernel(s) van uw nieuwe werkmap:

    |Kernel | Opdracht |
    |---|---|
    |Spark|`jupyter-kernelspec install sparkmagic/kernels/sparkkernel`|
    |SparkR|`jupyter-kernelspec install sparkmagic/kernels/sparkrkernel`|
    |PySpark|`jupyter-kernelspec install sparkmagic/kernels/pysparkkernel`|
    |PySpark3|`jupyter-kernelspec install sparkmagic/kernels/pyspark3kernel`|

1. Optioneel. Voer de onderstaande opdracht uit om de server-extensie:

    ```cmd
    jupyter serverextension enable --py sparkmagic
    ```

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Spark magic verbinding maken met HDInsight Spark-cluster configureren

In deze sectie configureert u de Spark-magic die u eerder hebt geïnstalleerd als u wilt verbinding maken met een Apache Spark-cluster.

1. Start de Python-shell met de volgende opdracht uit:

    ```cmd
    python
    ```

2. De Jupyter-configuratiegegevens worden meestal opgeslagen in de basismap van gebruikers. Voer de volgende opdracht om te identificeren van de oorspronkelijke directory en maak een map genaamd **.sparkmagic**.  Kan het volledige pad wordt gegenereerd.

    ```python
    import os
    path = os.path.expanduser('~') + "\\.sparkmagic"
    os.makedirs(path)
    print(path)
    exit()
    ```

3. In de map `.sparkmagic`, maken van een bestand met de naam **config.json** en voeg de volgende JSON-codefragment hierin.  

    ```json
    {
      "kernel_python_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "kernel_scala_credentials" : {
        "username": "{USERNAME}",
        "base64_password": "{BASE64ENCODEDPASSWORD}",
        "url": "https://{CLUSTERDNSNAME}.azurehdinsight.net/livy"
      },

      "heartbeat_refresh_seconds": 5,
      "livy_server_heartbeat_timeout_seconds": 60,
      "heartbeat_retry_seconds": 1
    }
    ```

4. De volgende wijzigingen aanbrengen in het bestand:

    |Waarde van de sjabloon | Nieuwe waarde |
    |---|---|
    |{USERNAME}|Aanmelding bij het cluster, standaard is `admin`.|
    |{CLUSTERDNSNAME}|Clusternaam|
    |{BASE64ENCODEDPASSWORD}|Een met base64 gecodeerde wachtwoord voor uw huidige wachtwoord.  U kunt een base64-wachtwoord op genereren [ https://www.url-encode-decode.com/base64-encode-decode/ ](https://www.url-encode-decode.com/base64-encode-decode/).|
    |`"livy_server_heartbeat_timeout_seconds": 60`|Behouden als `sparkmagic 0.12.7` (clusters v3.5 en v3.6).  Als u `sparkmagic 0.2.3` (clusters met v3.4) vervangen door `"should_heartbeat": true`.|

    Ziet u een volledig voorbeeld van het bestand tijdens [voorbeeld config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

   > [!TIP]  
   > Heartbeats worden verzonden om ervoor te zorgen dat sessies niet kunnen worden gelekt. Wanneer een computer in de slaapstand gaat of wordt afgesloten, wordt de heartbeat is niet verzonden, wat resulteert in de sessie wordt opgeschoond. Voor clusters v3.4, als u wilt uitschakelen van dit gedrag kunt u instellen de configuratie van Livy `livy.server.interactive.heartbeat.timeout` naar `0` van de Ambari UI. Voor clusters v3.5, als u niet de bovenstaande, 3,5 configuratie stelt worden de sessie niet verwijderd.

5. Jupyter starten. Gebruik de volgende opdracht uit vanaf de opdrachtprompt.

    ```cmd
    jupyter notebook
    ```

6. Controleer of dat u de Spark-magic beschikbaar met de kernels kunt gebruiken. Voer de volgende stappen uit.

    a. Maak een nieuwe notebook. Selecteer in de rechter hoek **nieuw**. U ziet nu de standaard-kernel **Python 2** of **Python 3** en de kernels die u hebt geïnstalleerd. De werkelijke waarden kunnen variëren, afhankelijk van uw installatie-opties.  Selecteer **PySpark**.

    ![Kernels op Jupyter-notitieblok](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels op Jupyter-notitieblok")

    > [!IMPORTANT]  
    > Na het selecteren van **nieuw** uw shell op fouten controleren.  Als u de volgende fout ziet `TypeError: __init__() got an unexpected keyword argument 'io_loop'` mogelijk is er een bekend probleem met bepaalde versies van Tornado.  Als dit het geval is, stopt de kernel en vervolgens gebruik maken van uw installatie Tornado met de volgende opdracht: `pip install tornado==4.5.3`.

    b. Voer het volgende codefragment.

    ```sql
    %%sql
    SELECT * FROM hivesampletable LIMIT 5
    ```  

    Als u de uitvoer met succes ophalen kunt, wordt de verbinding met het HDInsight-cluster getest.

    Als u wilt de configuratie van de notebook verbinding maken met een ander cluster bij te werken, moet u de config.json bijwerken met de nieuwe set waarden, zoals wordt weergegeven in stap 3 hierboven.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Waarom zou ik Jupyter installeren op mijn computer?

Er is een aantal redenen waarom u wilt mogelijk Jupyter op uw computer installeren en verbindt dit vervolgens met een Apache Spark-cluster in HDInsight.

* Hoewel Jupyter-notebooks al beschikbaar op het Spark-cluster in Azure HDInsight zijn, Jupyter op uw computer installeren biedt u de optie voor het lokaal uw notitieblokken maken en upload uw toepassing testen op een actief cluster de notitieblokken aan het cluster. Als u wilt de notebooks uploaden naar het cluster, kunt u uploaden met behulp van de Jupyter-notebook die wordt uitgevoerd of het cluster of opslaan naar de map /HdiNotebooks in het opslagaccount dat is gekoppeld aan het cluster. Zie voor meer informatie over hoe notitieblokken worden opgeslagen op het cluster, [waar zijn Jupyter-notebooks opgeslagen](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* De notebooks beschikbaar lokaal, u kunt verbinding maken met andere Spark-clusters op basis van de vereisten van uw toepassing.
* GitHub kunt u een bronbeheersysteem implementeren en versiebeheer voor de laptops hebben. U kunt ook een samenwerkingsomgeving waarin meerdere gebruikers met de dezelfde notebook werken kunnen hebben.
* U kunt lokaal werken met notitieblokken zonder dat ook een cluster omhoog. U hoeft alleen een cluster voor het testen van uw laptops, niet voor het handmatig beheren van uw laptops of een ontwikkelomgeving.
* Het is mogelijk eenvoudiger te configureren van uw eigen lokale ontwikkelomgeving dan de Jupyter-installatie op het cluster configureren.  U kunt profiteren van alle software die u lokaal hebt geïnstalleerd zonder een of meer externe clusters configureren.

> [!WARNING]  
> Jupyter op uw lokale computer is geïnstalleerd, kunnen meerdere gebruikers kunnen de dezelfde notebook op de dezelfde Spark-cluster uitvoeren op hetzelfde moment. In een dergelijke situatie worden meerdere Livy-sessies gemaakt. Als u een probleem ondervindt en daarop foutopsporing uitvoeren wilt, is dit een complexe taak om bij te houden welke Livy-sessie behoort tot welke gebruiker.  

## <a name="next-steps"></a>Volgende stappen

* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)
* [Apache Spark met BI: Interactieve gegevensanalyses met behulp van Spark in HDInsight met BI-hulpprogramma's uitvoeren](apache-spark-use-bi-tools.md)
* [Apache Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)