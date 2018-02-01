---
title: Installatie gepubliceerde toepassing - H2O mineraalwater - Azure HDInsight | Microsoft Docs
description: Installeren en gebruiken van de toepassing H2O mineraalwater van derden Hadoop.
services: hdinsight
documentationcenter: 
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: 8734daa5303aa76e9f8a074b5f709727cabb58b2
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="install-published-application---h2o-sparkling-water"></a>Gepubliceerde toepassing - H2O mineraalwater installeren

Dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [H20 mineraalwater](http://www.h2o.ai/) gepubliceerde toepassing in Azure HDInsight Hadoop. Zie voor een overzicht van het HDInsight-toepassing-platform en een lijst van beschikbare Independent Software Vendors (ISV's) gepubliceerde toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-h2o-sparkling-water"></a>Over H2O mineraalwater

H2O mineraalwater is een open-source, volledig gedistribueerde in het geheugen machine learning-platform lineaire schaalbaarheid. H2O mineraalwater kunt u de snelle, schaalbare machine learning-algoritmen van H2O met de mogelijkheden van Spark combineren. Met mineraalwater, kunnen gebruikers berekening station van Scala, R en Python via de gebruikersinterface H2O stromen.

H2O mineraalwater biedt:

* **Eenvoudig te gebruiken WebUI en bekend interfaces** – Stel boven en snel met behulp van beide H2O intuïtieve webgebaseerde stromen GUI of programmeeromgevingen zoals R, Python, Java, Scala, JSON en de H2O API's aan de slag.
* **Ondersteuning voor alle gemeenschappelijke database en -bestand typen gegevens networkdirect** – eenvoudig verkennen en model van Big Data uit binnen Microsoft Excel, R Studio en Tableau. Verbinding maken met gegevens uit HDFS, S3, SQL en NoSQL-gegevensbronnen.
* **Sterk schaalbare Big Data munging en analyse** – H2O Big Joins kunt 7 x sneller dan R data.table bewerkingen uitvoeren, en lineair geschaald naar miljard 10 x 10 miljard rij joins.
* **Score berekenen voor realtime-gegevens** – snel modellen implementeren in productieomgevingen met oude onbewerkte Java-objecten (POJO), Java-objecten model geoptimaliseerd (MOJO) of de H2O REST-API.

### <a name="resource-links"></a>Koppelingen naar hulpbronnen

* [H2O.ai Engineering Roadmap](https://jira.h2o.ai/)
* [H2O.ai Home](http://www.h2o.ai/)
* [H2O.ai Documentation](http://docs.h2o.ai/)
* [H2O.AI-ondersteuning](https://support.h2o.ai/)
* [H2O.AI Open-Source Codebase](https://github.com/h2oai/)

## <a name="prerequisites"></a>Vereisten

Om deze app installeren op een nieuwe HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Cluster tier(s): Standard of Premium
* Cluster-type: Spark
* Cluster-versie (s): 3.5 of 3.6

## <a name="install-the-h2o-sparkling-water-published-application"></a>Installeer de mineraalwater H2O gepubliceerde toepassing

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-h2o-sparkling-water"></a>H2O mineraalwater starten

1. Na de installatie kunt u beginnen met behulp van H2O mineraalwater (h2o sparklingwater) bij uw cluster in Azure-portal door het openen van Jupyter-Notebooks (`https://<ClusterName>.azurehdinsight.net/jupyter`). U kunt ook naar Jupyter ophalen door het selecteren van **Cluster-dashboard** in het deelvenster cluster in de portal selecteren **Jupyter-Notebook**. U wordt gevraagd uw referenties in te voeren. Geef referenties op van het cluster Hadoop die is opgegeven op het maken van het cluster.

2. In Jupyter, ziet u drie mappen: H2O-PySparkling-voorbeelden, PySpark voorbeelden en Scala voorbeelden. Selecteer de **H2O-PySparkling-voorbeelden** map.

    ![Jupyter-Notebooks thuis](./media/hdinsight-apps-install-h2o/jupyter-home.png)

3. De eerste stap bij het maken van een nieuwe notebook is om de Spark-omgeving te configureren. Deze informatie is opgenomen in de **Sentiment_analysis_with_Sparkling_Water** voorbeeld. Bij het configureren van de Spark-omgeving, moet u de juiste jar gebruiken en geeft u het IP-adres opgegeven door de uitvoer van de eerste cel.

    ![Jupyter-Notebooks thuis](./media/hdinsight-apps-install-h2o/spark-config.png)

4. Het Cluster H2O starten.

    ![Cluster starten](./media/hdinsight-apps-install-h2o/start-cluster.png)

5. Nadat het Cluster H2O actief en werkend is, open H2O stromen door te gaan naar  **`https://<ClusterName>-h2o.apps.azurehdinsight.net:443`** .

    > [!NOTE]
    > Als u niet wilt openen H2O stromen, probeer uw browsercache wissen. Als u nog niet deze bereiken, u waarschijnlijk geen onvoldoende bronnen op het cluster. Verhoog het aantal Worker-knooppunten onder de **Scale cluster** optie in het deelvenster van uw cluster.

    ![Dashboard H2O stromen](./media/hdinsight-apps-install-h2o/h2o-flow.png)

6. Selecteer de **Million_Songs.flow** voorbeeld in het menu aan de rechterkant. Wanneer u wordt gevraagd met een waarschuwing, klikt u op **Load Notebook**. Deze demo is ontworpen om te worden uitgevoerd in een paar minuten met echte gegevens. Het doel is het voorspellen van de gegevens of het nummer is uitgebracht vóór of na 2004 met behulp van de binaire indeling.

    ![Selecteer Million_Songs.flow](./media/hdinsight-apps-install-h2o/million-songs.png)

7. Zoeken naar het pad dat bevat **milsongs-cls-train.csv.gz**, en vervang het gehele pad tussen **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-train.csv.gz** .

8. Zoeken naar het pad dat bevat **milsongs-cls-test.csv.gz** en vervang ze door **https://h2o-public-test-data.s3.amazonaws.com/bigdata/laptop/milsongs/milsongs-cls-test.csv.gz**.

9. Voor het uitvoeren van alle instructies binnen de cellen laptop, selecteer de **alles uitvoeren** op de werkbalk.

    ![Alles uitvoeren](./media/hdinsight-apps-install-h2o/run-all.png)

10. Na een paar minuten ziet u uitvoer ziet er als volgt.

    ![Uitvoer](./media/hdinsight-apps-install-h2o/output.png)

Dat is alles. U hebt om deze om kunstmatige intelligence in Spark binnen een paar minuten. U kunt nu meer voorbeelden in H2O stromen die laten zien verschillende soorten machine learning-algoritmen verkennen.

## <a name="next-steps"></a>Volgende stappen

* [H2O documentatie](http://docs.h2o.ai/h2o/latest-stable/h2o-docs/index.html)
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [Met behulp van de scriptactie Linux gebaseerde HDInsight-clusters aanpassen](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van scriptactie om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een leeg edge-knooppunt voor toegang tot HDInsight-clusters en voor het testen en die als host fungeert voor HDInsight-toepassingen.
