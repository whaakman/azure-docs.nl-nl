---
title: Jupyter lokaal installeren en verbinding maken met Spark in Azure HDInsight
description: Leer hoe u lokaal Jupyter-notebook op uw computer installeren en verbinden met een Apache Spark-cluster.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/28/2017
ms.author: hrasheed
ms.openlocfilehash: 92f6bc358fe8cc5ab8f7242d94edc3004eaab4b9
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53163375"
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Jupyter-notebook op uw computer installeren en verbinden met Apache Spark in HDInsight

In dit artikel leert u hoe u Jupyter-notebook, met de aangepaste PySpark (voor Python) en Apache Spark (voor Scala) kernels met Spark Magic-pakket installeren en de notebook verbinden met een HDInsight-cluster. Er is een aantal redenen voor het installeren van Jupyter op uw lokale computer en kunnen er ook enkele uitdagingen. Zie de sectie voor meer informatie over dit [waarom moet ik Jupyter installeren op mijn computer](#why-should-i-install-jupyter-on-my-computer) aan het einde van dit artikel.

Er zijn drie belangrijke stappen die betrokken zijn bij de Jupyter- en de Spark-Magic-pakket installeren op uw computer.

* Jupyter-notebook installeren
* De PySpark- en Spark-kernels installeren met de Magic-pakket voor Spark
* Spark Magic-pakket voor toegang tot Spark-cluster in HDInsight configureren

Zie voor meer informatie over de aangepaste kernels en de Spark-magic beschikbaar voor de Jupyter-notebooks met HDInsight-cluster [beschikbare Kernels voor Jupyter-notebooks met Apache Spark Linux-clusters in HDInsight](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Vereisten
De vereisten die hier worden vermeld, zijn niet voor het installeren van Jupyter. Dit zijn voor de Jupyter-notebook verbinding met een HDInsight-cluster nadat het notitieblok is geïnstalleerd.

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie [Apache Spark-clusters maken in Azure HDInsight](apache-spark-jupyter-spark-sql.md) voor instructies.

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter-notebook op uw computer installeren

Voordat u de Jupyter-notebooks kunt installeren, moet u Python installeren. Zowel Python als Jupyter zijn beschikbaar als onderdeel van de [Anaconda distributie](https://www.anaconda.com/download/). Als u Anaconda installeert, installeert u een Python-distributie. Wanneer Anaconda is geïnstalleerd, kunt u de Jupyter-installatie toevoegen door het uitvoeren van opdrachten.

1. Download de [Anaconda installatieprogramma](https://www.anaconda.com/download/) voor uw platform en voer de installatie. Tijdens het uitvoeren van de wizard setup, zorg ervoor dat u selecteert de optie voor het Anaconda toevoegen aan de variabele PATH.
1. Voer de volgende opdracht voor het installeren van Jupyter.

        conda install jupyter

    Zie voor meer informatie over het installeren van Jupyter [installeren Jupyter met behulp van Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>De kernels en Spark Magic-pakket installeren

Voor instructies over het installeren van de Spark-magic, volgt u de PySpark- en Spark-kernels, de installatie-instructies in de [sparkmagic documentatie](https://github.com/jupyter-incubator/sparkmagic#installation) op GitHub. De eerste stap in de documentatie van de Spark-magic, vraagt u Spark Magic-pakket te installeren. Deze eerste stap in de koppeling vervangen door de volgende opdrachten, afhankelijk van de versie van het HDInsight-cluster dat maakt u verbinding met. Volg daarna de resterende stappen in de documentatie van de Spark-magic. Als u installeren van de verschillende kernels wilt, voert u stap 3 in de sectie Spark magic-installatie-instructies.

* Voor clusters v3.4, installeert u sparkmagic 0.2.3 door uit te voeren `pip install sparkmagic==0.2.3`

* Voor clusters v3.5 en v3.6, installeert u sparkmagic 0.11.2 door uit te voeren `pip install sparkmagic==0.11.2`

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Spark magic verbinding maken met HDInsight Spark-cluster configureren

In deze sectie configureert u de Spark-magic die u eerder hebt geïnstalleerd als u wilt verbinding maken met een Apache Spark-cluster moet u al hebt gemaakt in Azure HDInsight.

1. De Jupyter-configuratiegegevens worden meestal opgeslagen in de basismap van gebruikers. Als u wilt de basismap op elk platform OS vinden, typ de volgende opdrachten.

    Start de Python-shell. In een opdrachtvenster, typt u het volgende:

        python

    Voer de volgende opdracht om erachter te komen de basismap van de Python-shell.

        import os
        print(os.path.expanduser('~'))

1. Navigeer naar de oorspronkelijke directory en maak een map genaamd **.sparkmagic** als deze niet al bestaat.
1. Maak een bestand met de naam in de map **config.json** en voeg de volgende JSON-codefragment hierin.

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
          }
        }

1. Vervang **{USERNAME}**, **{CLUSTERDNSNAME}**, en **{BASE64ENCODEDPASSWORD}** met de juiste waarden. U kunt een aantal hulpprogramma's in uw favoriete programmeertaal of online gebruiken voor het genereren van een met base64 gecodeerde wachtwoord voor uw huidige wachtwoord.

1. Configureer de juiste Heartbeat-instellingen in `config.json`. U moet deze instellingen toevoegen op hetzelfde niveau als het `kernel_python_credentials` en `kernel_scala_credentials` codefragmenten uw toegevoegd eerder. Zie voor een voorbeeld van hoe en waar u de heartbeatinstellingen toe te voegen, [voorbeeld config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

    * Voor `sparkmagic 0.2.3` (clusters v3.4), opnemen:

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * Voor `sparkmagic 0.11.2` (clusters v3.5 en v3.6), opnemen:

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    >Heartbeats worden verzonden om ervoor te zorgen dat sessies niet kunnen worden gelekt. Wanneer een computer in de slaapstand gaat of wordt afgesloten, wordt de heartbeat is niet verzonden, wat resulteert in de sessie wordt opgeschoond. Voor clusters v3.4, als u wilt uitschakelen van dit gedrag kunt u instellen de configuratie van Livy `livy.server.interactive.heartbeat.timeout` naar `0` van de Ambari UI. Voor clusters v3.5, als u niet de bovenstaande, 3,5 configuratie stelt worden de sessie niet verwijderd.

1. Jupyter starten. Gebruik de volgende opdracht uit vanaf de opdrachtprompt.

        jupyter notebook

1. Controleer of u verbinding kunt maken met het cluster via de Jupyter-notebook en dat u de Spark-magic beschikbaar met de kernels kunt gebruiken. Voer de volgende stappen uit.

    a. Maak een nieuwe notebook. Klik in de rechterhoek op **nieuw**. U ziet nu de standaard-kernel **Python2** en de twee nieuwe kernels die u installeert, **PySpark** en **Spark**. Klik op **PySpark**.

    ![Kernels op Jupyter-notitieblok](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels op Jupyter-notitieblok")

    b. Voer het volgende codefragment.

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    Als u de uitvoer met succes ophalen kunt, wordt de verbinding met het HDInsight-cluster getest.

    >[!TIP]
    >Als u de configuratie van de notebook verbinding maken met een ander cluster bijwerken wilt, werkt u de config.json met de nieuwe set waarden, zoals wordt weergegeven in stap 3 hierboven.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Waarom zou ik Jupyter installeren op mijn computer?
Er is een aantal redenen waarom u wilt mogelijk Jupyter op uw computer installeren en verbindt dit vervolgens met een Apache Spark-cluster in HDInsight.

* Hoewel Jupyter-notebooks al beschikbaar op het Spark-cluster in Azure HDInsight zijn, Jupyter op uw computer installeren biedt u de optie voor het lokaal uw notitieblokken maken en upload uw toepassing testen op een actief cluster de notitieblokken aan het cluster. Als u wilt de notebooks uploaden naar het cluster, kunt u uploaden met behulp van de Jupyter-notebook die wordt uitgevoerd of het cluster of opslaan naar de map /HdiNotebooks in het opslagaccount dat is gekoppeld aan het cluster. Zie voor meer informatie over hoe notitieblokken worden opgeslagen op het cluster, [waar zijn Jupyter-notebooks opgeslagen](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* De notebooks beschikbaar lokaal, u kunt verbinding maken met andere Spark-clusters op basis van de vereisten van uw toepassing.
* GitHub kunt u een bronbeheersysteem implementeren en versiebeheer voor de laptops hebben. U kunt ook een samenwerkingsomgeving waarin meerdere gebruikers met de dezelfde notebook werken kunnen hebben.
* U kunt lokaal werken met notitieblokken zonder dat ook een cluster omhoog. U hoeft alleen een cluster voor het testen van uw laptops, niet voor het handmatig beheren van uw laptops of een ontwikkelomgeving.
* Het is mogelijk eenvoudiger te configureren van uw eigen lokale ontwikkelomgeving dan de Jupyter-installatie op het cluster configureren.  U kunt profiteren van alle software die u lokaal hebt geïnstalleerd zonder een of meer externe clusters configureren.

> [!WARNING]
> Jupyter op uw lokale computer is geïnstalleerd, kunnen meerdere gebruikers kunnen de dezelfde notebook op de dezelfde Spark-cluster uitvoeren op hetzelfde moment. In een dergelijke situatie worden meerdere Livy-sessies gemaakt. Als u een probleem ondervindt en daarop foutopsporing uitvoeren wilt, is dit een complexe taak om bij te houden welke Livy-sessie behoort tot welke gebruiker.
>
>

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
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op te sporen Apache Spark-toepassingen op afstand](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Apache Zeppelin-notebooks gebruiken met een Apache Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Apache Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
