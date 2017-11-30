---
title: Jupyter lokaal installeren en te verbinden met een Azure HDInsight Spark-cluster | Microsoft Docs
description: Informatie over het installeren van de Jupyter-notebook lokaal op uw computer en te verbinden met een Apache Spark-cluster in Azure HDInsight.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 48593bdf-4122-4f2e-a8ec-fdc009e47c16
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 1508faf7a05461de65b7a4c2f68e2ef9bbd7e19d
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2017
---
# <a name="install-jupyter-notebook-on-your-computer-and-connect-to-apache-spark-on-hdinsight"></a>Jupyter-notebook installeren op uw computer en maak verbinding met Apache Spark in HDInsight

In dit artikel leert u hoe u Jupyter-notebook installeren met de aangepaste PySpark (voor Python) en kernels met Spark-magic Spark (voor Scala) en de notebook verbinding met een HDInsight-cluster. Er is een aantal redenen voor het installeren van Jupyter op uw lokale computer en kunnen er ook enkele uitdagingen. Zie de sectie voor meer informatie over dit [waarom zou ik Jupyter installeren op mijn computer](#why-should-i-install-jupyter-on-my-computer) aan het einde van dit artikel.

Er zijn drie belangrijke stappen die betrokken zijn bij het installeren van Jupyter en de Spark-magie op uw computer.

* Jupyter-notebook installeren
* De PySpark- en Spark kernels installeren met de magische Spark
* Spark-magic voor toegang tot Spark-cluster in HDInsight configureren

Zie voor meer informatie over de aangepaste kernels en het beschikbaar voor Jupyter-notebooks met HDInsight-cluster Spark-magic [beschikbare Kernels voor Jupyter-notebooks met Apache Spark Linux-clusters in HDInsight](apache-spark-jupyter-notebook-kernels.md).

## <a name="prerequisites"></a>Vereisten
De hier vermelde vereisten zijn niet beschikbaar voor het installeren van Jupyter. Dit zijn voor het verbinden van de Jupyter-notebook met een HDInsight-cluster als de laptop is geïnstalleerd.

* Een Azure-abonnement. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Een Apache Spark-cluster in HDInsight. Zie voor instructies [maken Apache Spark-clusters in Azure HDInsight](apache-spark-jupyter-spark-sql.md).

## <a name="install-jupyter-notebook-on-your-computer"></a>Jupyter-notebook op uw computer installeren

Voordat u de Jupyter-notebooks kunt installeren, moet u Python installeren. Zowel Python en Jupyter zijn beschikbaar als onderdeel van de [Anaconda distributie](https://www.continuum.io/downloads). Als u Anaconda installeert, installeert u een distributiepunt van Python. Nadat Anaconda is geïnstalleerd, kunt u de installatie van de Jupyter toevoegen door de juiste opdrachten uit te voeren.

1. Download de [Anaconda-installatieprogramma](https://www.continuum.io/downloads) voor uw platform en voer de installatie. Zorg dat u de optie Anaconda toevoegen aan de padvariabele tijdens het uitvoeren van de wizard setup.
2. Voer de volgende opdracht voor het installeren van Jupyter.

        conda install jupyter

    Zie voor meer informatie over het installeren van Jupyter [installeren Jupyter met Anaconda](http://jupyter.readthedocs.io/en/latest/install.html).

## <a name="install-the-kernels-and-spark-magic"></a>Installeer de kernels en Spark verwerkt Magic-pakket

Voor instructies over het installeren van de Spark-magic, de kernels PySpark en Spark installatie-instructies in de [sparkmagic documentatie](https://github.com/jupyter-incubator/sparkmagic#installation) op GitHub. De eerste stap in de documentatie van de Spark-magic vraagt u Spark verwerkt Magic-pakket installeren. Deze eerste stap in de koppeling met de volgende opdrachten, afhankelijk van de versie van het HDInsight-cluster dat maakt u verbinding met vervangen. Volg daarna de overige stappen in de documentatie van Spark-magic. Als u installeren van de verschillende kernels wilt, voert u stap 3 in de sectie Spark magische installatie-instructies.

* Voor clusters v3.4, installeert u sparkmagic 0.2.3 door te voeren`pip install sparkmagic==0.2.3`

* Installeren voor clusters v3.5 en v3.6 sparkmagic 0.11.2 door`pip install sparkmagic==0.11.2`

## <a name="configure-spark-magic-to-connect-to-hdinsight-spark-cluster"></a>Spark-magic verbinding maken met HDInsight Spark-cluster configureren

In deze sectie configureert u de Spark-magie die u eerder hebt geïnstalleerd voor de verbinding met een Apache Spark-cluster moet u al hebt gemaakt in Azure HDInsight.

1. De Jupyter-configuratie-informatie wordt meestal opgeslagen in de basismap van gebruikers. Als u wilt vinden uw basismap op een besturingssysteem of platform, typ de volgende opdrachten.

    Start de Python-shell. Op een opdrachtvenster, typt u het volgende:

        python

    Voer de volgende opdracht om erachter te komen de basismap op de shell Python.

        import os
        print(os.path.expanduser('~'))

2. Navigeer naar de basismap en maak een map met de naam **.sparkmagic** als deze niet al bestaat.
3. Maak een bestand met de naam in de map **config.json** en voeg de volgende JSON-fragment hierin.

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

4. Vervang **{USERNAME}**, **{CLUSTERDNSNAME}**, en **{BASE64ENCODEDPASSWORD}** met de juiste waarden. U kunt een aantal hulpprogramma's in uw favoriete programmeertaal of online gebruiken voor het genereren van een base64-gecodeerd wachtwoord voor uw huidige wachtwoord.

5. Configureer de juiste Heartbeat-instellingen in `config.json`. U moet deze instellingen toevoegen op hetzelfde niveau als het `kernel_python_credentials` en `kernel_scala_credentials` codefragmenten uw toegevoegde eerder. Zie voor een voorbeeld van hoe en waar u de heartbeatinstellingen toe te voegen, [voorbeeld config.json](https://github.com/jupyter-incubator/sparkmagic/blob/master/sparkmagic/example_config.json).

    * Voor `sparkmagic 0.2.3` (clusters met v3.4) omvatten:

            "should_heartbeat": true,
            "heartbeat_refresh_seconds": 5,
            "heartbeat_retry_seconds": 1

    * Voor `sparkmagic 0.11.2` (clusters v3.5 en v3.6), zijn onder andere:

            "heartbeat_refresh_seconds": 5,
            "livy_server_heartbeat_timeout_seconds": 60,
            "heartbeat_retry_seconds": 1

    >[!TIP]
    >Heartbeats worden verzonden om ervoor te zorgen dat sessies niet gelekt. Wanneer een computer overschakelt naar de slaapstand of wordt afgesloten, wordt de heartbeat is niet verzonden, wat resulteert in de sessie wordt opgeschoond. Voor clusters v3.4, als u wilt uitschakelen, dit gedrag kunt u instellen Livy config `livy.server.interactive.heartbeat.timeout` naar `0` van de Ambari UI. Voor clusters v3.5, als de bovenstaande 3.5 configuratie niet is ingesteld worden de sessie niet verwijderd.

6. Jupyter starten. Gebruik de volgende opdracht vanaf de opdrachtprompt.

        jupyter notebook

7. Controleer of u verbinding kunt maken met het cluster via de Jupyter-notebook en dat u de Spark-magie beschikbaar met de kernels kunt gebruiken. De volgende stappen uitvoeren.

    a. Maak een nieuwe notebook. Klik in de rechterhoek op **nieuw**. U ziet nu de standaard-kernel **Python2** en de twee nieuwe kernels die u installeert, **PySpark** en **Spark**. Klik op **PySpark**.

    ![Kernels in Jupyter-notebook](./media/apache-spark-jupyter-notebook-install-locally/jupyter-kernels.png "Kernels in Jupyter-notebook")

    b. Voer het volgende codefragment.

        %%sql
        SELECT * FROM hivesampletable LIMIT 5

    Als u de uitvoer met succes ophalen kunt, wordt de verbinding met het HDInsight-cluster getest.

    >[!TIP]
    >Als u de configuratie van de notebook verbinding maken met een ander cluster bijwerken wilt, bijwerken van de config.json met de nieuwe set waarden, zoals u in stap 3 hierboven.

## <a name="why-should-i-install-jupyter-on-my-computer"></a>Waarom zou ik Jupyter installeren op mijn computer?
Er is een aantal redenen waarom u wilt mogelijk Jupyter op uw computer installeren en deze vervolgens verbinding met een Spark-cluster in HDInsight.

* Hoewel Jupyter-notebooks al beschikbaar op het Spark-cluster in Azure HDInsight zijn, biedt installeert Jupyter op uw computer u de optie voor het maken van uw notitieblokken lokaal door uw toepassing testen op een actief cluster en vervolgens de laptops uploaden naar het cluster. Als u wilt uploaden de laptops aan het cluster, kunt u uploaden met de Jupyter-notebook die wordt uitgevoerd of het cluster of opslaan op de map /HdiNotebooks in het opslagaccount die is gekoppeld aan het cluster. Zie voor meer informatie over hoe notitieblokken worden opgeslagen op het cluster, [waar zijn Jupyter-notebooks opgeslagen](apache-spark-jupyter-notebook-kernels.md#where-are-the-notebooks-stored)?
* Met de beschikbare laptops lokaal, kunt u verschillende Spark-clusters op basis van uw vereisten voor toepassingsimplementatie.
* U kunt GitHub gebruiken voor het implementeren van een bronbeheersysteem en versiebeheer voor de laptops hebben. U kunt ook een samenwerkingsomgeving waar meerdere gebruikers met de dezelfde laptop werken kunnen hebben.
* U kunt lokaal werken met notitieblokken zonder zelfs een cluster van. U hoeft alleen een cluster voor het testen van uw notitieblokken tegen, niet handmatig te beheren uw notitieblokken of een ontwikkelomgeving.
* Het is wellicht eenvoudiger te configureren van uw eigen lokale ontwikkelingsomgeving dan de Jupyter-installatie op het cluster configureren.  U kunt profiteren van alle software die u lokaal hebt geïnstalleerd zonder het configureren van een of meer externe clusters.

> [!WARNING]
> Jupyter op uw lokale computer geïnstalleerd, kunnen meerdere gebruikers kunnen de dezelfde notebook in hetzelfde Spark-cluster uitvoeren op hetzelfde moment. In een dergelijke situatie worden meerdere Livy sessies gemaakt. Als u een probleem ondervindt en foutopsporing uitvoeren wilt, zal zijn voor een complexe taak bijhouden welke sessie Livy waarmee de gebruiker behoort.
>
>

## <a name="seealso"></a>Zie ook
* [Overzicht: Apache Spark in Azure HDInsight](apache-spark-overview.md)

### <a name="scenarios"></a>Scenario's
* [Spark met BI: interactieve gegevensanalyses uitvoeren met behulp van Spark in HDInsight met BI-tools](apache-spark-use-bi-tools.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken voor het analyseren van de gebouwtemperatuur met behulp van HVAC-gegevens](apache-spark-ipython-notebook-machine-learning.md)
* [Spark met Machine Learning: Spark in HDInsight gebruiken om voedselinspectieresultaten te voorspellen](apache-spark-machine-learning-mllib-ipython.md)
* [Spark-streaming: Spark in HDInsight gebruiken voor het bouwen van realtime streamingtoepassingen](apache-spark-eventhub-streaming.md)
* [Websitelogboekanalyse met Spark in HDInsight](apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Toepassingen maken en uitvoeren
* [Een zelfstandige toepassing maken met behulp van Scala](apache-spark-create-standalone-application.md)
* [Taken op afstand uitvoeren in een Spark-cluster met behulp van Livy](apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools en uitbreidingen
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om Spark Scala-toepassingen te maken en in te dienen](apache-spark-intellij-tool-plugin.md)
* [De invoegtoepassing HDInsight Tools for IntelliJ IDEA gebruiken om op afstand fouten in Spark Scala-toepassingen op te lossen](apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Zeppelin-notebooks gebruiken met een Spark-cluster in HDInsight](apache-spark-zeppelin-notebook.md)
* [Beschikbare kernels voor Jupyter-notebook in Spark-cluster voor HDInsight](apache-spark-jupyter-notebook-kernels.md)
* [Externe pakketten gebruiken met Jupyter-notebooks](apache-spark-jupyter-notebook-use-external-packages.md)

### <a name="manage-resources"></a>Resources beheren
* [Resources beheren voor het Apache Spark-cluster in Azure HDInsight](apache-spark-resource-manager.md)
* [Taken die worden uitgevoerd in een Apache Spark-cluster in HDInsight, traceren en er fouten in oplossen](apache-spark-job-debugging.md)
