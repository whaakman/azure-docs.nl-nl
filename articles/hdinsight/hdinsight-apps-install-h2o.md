---
title: Gepubliceerde toepassing - H2O Sparkling Water - Azure HDInsight installeren
description: Installeren en gebruiken van de Hadoop-toepassing van H2O Sparkling Water van derden.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: d2aeb41c2fcc11c37ec56707932eabcf63e026a0
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52497212"
---
# <a name="install-published-application---h2o-sparkling-water"></a>Gepubliceerde toepassing - H2O Sparkling Water installeren

In dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [H20 Sparkling Water](http://www.h2o.ai/) gepubliceerd [Apache Hadoop](https://hadoop.apache.org/) toepassing voor Azure HDInsight. Zie voor een overzicht van het HDInsight-platform voor toepassingen en een lijst van beschikbare Independent Software Vendor (ISV) gepubliceerde toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-h2o-sparkling-water"></a>Over H2O Sparkling Water

H2O Sparkling Water is een open source, volledig gedistribueerde in-memory-machine learning-platform met lineaire schaalbaarheid. H2O Sparkling Water kunt u de snelle, schaalbare machine learning-algoritmen van H2O met de mogelijkheden van combineren [Apache Spark](https://spark.apache.org/). Met Sparkling Water kunnen gebruikers berekening van station. [Scala](https://www.scala-lang.org/), R en Python met behulp van de gebruikersinterface van H2O Flow.

H2O Sparkling Water biedt:

* **Eenvoudig te gebruiken webinterface en vertrouwde interfaces** : Stel boven en aan de slag snel met behulp van een van beide H2O intuïtieve web gebaseerde Flow GUI of omgevingen zoals R, Python, Java, Scala, JSON en de API's van H2O programmeren.
* **Gegevens-agnostische ondersteuning voor alle algemene database en het bestand typen** – eenvoudig verkennen en modelleren van Big Data uit binnen Microsoft Excel, R Studio en Tableau. Verbinding maken met gegevens uit HDFS, S3, SQL en NoSQL-gegevensbronnen.
* **Zeer schaalbare munging Big Data en analyse** – H2O grote samenvoegingen kunt 7 x sneller dan R data.table bewerkingen uitvoeren en lineair worden geschaald naar 10 miljard x 10 miljard rij joins.
* **Realtime gegevens scoren** – snel modellen te implementeren voor productie met behulp van onbewerkte oude Java-objecten (POJO), geoptimaliseerd voor model Java objecten (MOJO), of de REST-API van H2O.

### <a name="resource-links"></a>Resourcekoppelingen

* [H2O.ai Engineering Roadmap](http://jira.h2o.ai/)
* [H2O.AI thuis](http://www.h2o.ai/)
* [H2O.AI-documentatie](http://docs.h2o.ai/)
* [Ondersteuning voor H2O.AI](https://support.h2o.ai/)
* [H2O.AI Open-Source Codebase](https://github.com/h2oai/)

## <a name="prerequisites"></a>Vereisten

Als u wilt deze app installeren op een nieuw HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Cluster-laag of lagen toe: Standard of Premium
* Clustertype: Spark
* Cluster-versie (s): 3.5 of 3.6

## <a name="install-the-h2o-sparkling-water-published-application"></a>Gepubliceerde toepassing installeren de H2O Sparkling Water

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Apache Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-h2o-sparkling-water"></a>Starten van H2O Sparkling Water

1. Na de installatie, u kunt beginnen met H2O Sparkling Water (h2o-sparklingwater) van het cluster in Azure portal door te openen [Jupyter-Notebooks](https://jupyter.org/) (`https://<ClusterName>.azurehdinsight.net/jupyter`). U kunt ook naar Jupyter ophalen door het selecteren van **clusterdashboard** in uw cluster deelvenster in de portal, selecteert u vervolgens **Jupyter-Notebook**. U wordt gevraagd uw referenties in te voeren. Voer referenties in van het cluster Hadoop zoals opgegeven op het maken van clusters.

2. In Jupyter, ziet u drie mappen: H2O-PySparkling-voorbeelden, voorbeelden van de PySpark en Scala-voorbeelden. Selecteer de **H2O-PySparkling-voorbeelden** map.

    ![Jupyter-Notebooks thuis](./media/hdinsight-apps-install-h2o/jupyter-home.png)

3. De eerste stap bij het maken van een nieuwe notebook is de Spark-omgeving configureren. Deze informatie is opgenomen in de **Sentiment_analysis_with_Sparkling_Water** voorbeeld. Bij het configureren van de Spark-omgeving, moet de juiste jar gebruiken en geeft u het IP-adres dat is opgegeven door de uitvoer van de eerste cel.

    ![Jupyter-Notebooks thuis](./media/hdinsight-apps-install-h2o/spark-config.png)

4. Het Cluster H2O starten.

    ![Cluster starten](./media/hdinsight-apps-install-h2o/start-cluster.png)

5. Nadat het Cluster H2O actief en werkend is, opent u H2O stromen door te gaan naar **`https://<ClusterName>-h2o.apps.azurehdinsight.net:443`**.

    > [!NOTE]
    > Als u niet kunt openen H2O Flow, wist u de browsercache van uw. Als u nog steeds niet bereikbaar het, u waarschijnlijk geen onvoldoende bronnen in uw cluster. Verhoog het aantal Worker-knooppunten onder de **cluster schalen** optie in het deelvenster van het cluster.

    ![Dashboard H2O Flow](./media/hdinsight-apps-install-h2o/h2o-flow.png)

6. Selecteer de **Million_Songs.flow** voorbeeld in het menu aan de rechterkant. Wanneer u hierom wordt gevraagd een waarschuwing wordt weergegeven, klikt u op **Load Notebook**. In deze demo is ontworpen om uit te voeren in een paar minuten met echte gegevens. Het doel is om te voorspellen van de gegevens of het nummer is uitgebracht vóór of na 2004 met behulp van de binaire indeling.

    ![Selecteer Million_Songs.flow](./media/hdinsight-apps-install-h2o/million-songs.png)

7. Zoek het pad dat **milsongs-cls-train.csv.gz**, en vervang het volledige pad met **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-train.csv.gz**.

8. Zoek het pad dat **milsongs-cls-test.csv.gz** en vervang deze door **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-test.csv.gz**.

9. Voor het uitvoeren van alle instructies in de notebook cellen, selecteer de **alles uitvoeren** op de werkbalk.

    ![Alle uitvoeren](./media/hdinsight-apps-install-h2o/run-all.png)

10. Na een paar minuten ziet u uitvoer die vergelijkbaar is met het volgende.

    ![Uitvoer](./media/hdinsight-apps-install-h2o/output.png)

Dat is alles. U hebt een worden ingezet voor kunstmatige intelligentie in Spark binnen een paar minuten. U kunt nu voorbeelden die laten zien van verschillende soorten machine learning-algoritmen in H2O Flow verkennen.

## <a name="next-steps"></a>Volgende stappen

* [Documentatie van H2O](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/index.html)
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [HDInsight Linux gebaseerde clusters aanpassen met Script Action](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik scriptacties om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een lege edge-knooppunt voor toegang tot HDInsight-clusters, en voor het testen en HDInsight-toepassingen hosten.
