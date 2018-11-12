---
title: StreamSets Data Collector - Azure HDInsight voor gepubliceerde toepassing - installeren
description: Installeren en gebruiken van de toepassing StreamSets Data Collector van derden Apache Hadoop.
services: hdinsight
author: ashishthaps
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: f963ae53e1396b1ef6279f2bd6502e5ab0cd23a1
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2018
ms.locfileid: "51034554"
---
# <a name="install-published-application---streamsets-data-collector"></a>StreamSets Data Collector voor gepubliceerde toepassing - installeren

In dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [StreamSets Data Collector voor HDInsight](https://streamsets.com/) gepubliceerd Apache Hadoop-toepassing in Azure HDInsight. Zie voor een overzicht van het HDInsight-platform voor toepassingen en een lijst van beschikbare Independent Software Vendor (ISV) gepubliceerde toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-streamsets-data-collector"></a>Over StreamSets Data Collector

De StreamSets Data Collector wordt boven op een Azure HDInsight-toepassing geïmplementeerd. StreamSets Data Collector biedt een door de complete geïntegreerde ontwikkelomgeving (IDE), kunt u ontwerpen, testen, implementeren en beheren van any-to-any opname van pijplijnen. Deze pijplijnen kunnen net stroom- en batchgegevens en bevatten een verscheidenheid aan in-stream transformaties, allemaal zonder te hoeven schrijven van aangepaste code.

StreamSets Data Collector kunt u build gegevensstromen met behulp van talrijke Big Data-componenten zoals HDFS, Kafka, Solr, Hive, HBASE en Kudu. Zodra StreamSets Data Collector wordt uitgevoerd op een edge-server of in uw Hadoop-cluster, krijgt u realtime-controle voor zowel gegevens als gegevens afwijkingen stroom bewerkingen. Deze bewaking bevat op basis van een drempelwaarde voor waarschuwingen, detectie van afwijkingen en automatisch herstel van foutrecords.

StreamSets Data Collector is ontworpen voor elke fase van een pijplijn logisch geïsoleerd, zodat u kunt voldoen aan nieuwe bedrijfsbehoeften door slepen en neerzetten in nieuwe processors en connectors zonder te coderen en met minimale downtime.

### <a name="streamsets-resource-links"></a>StreamSets resourcekoppelingen

* [Documentatie](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html)
* [Blog](https://streamsets.com/blog/)
* [Zelfstudies](https://github.com/streamsets/tutorials)
* [Ondersteuningsforum voor ontwikkelaars](https://groups.google.com/a/streamsets.com/forum/#!forum/sdc-user)
* [Kanaal voor Slack openbare StreamSets](https://streamsetters.slack.com/)
* [Broncode](https://github.com/streamsets)

## <a name="prerequisites"></a>Vereisten

Als u wilt deze app installeren op een nieuw HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Cluster-laag of lagen toe: Standard of Premium
* Cluster-versie (s): 3.5 en hoger

## <a name="install-the-streamsets-data-collector-published-application"></a>Gepubliceerde toepassing installeren de StreamSets Data Collector

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-streamsets-data-collector"></a>StreamSets Data Collector starten

1. Na de installatie, kunt u StreamSets starten vanuit het cluster in Azure portal door te gaan naar de **instellingen** deelvenster, klikt u vervolgens selecteren **toepassingen** onder de **algemene** de categorie. Het deelvenster met geïnstalleerde Apps worden de geïnstalleerde toepassingen.

    ![Geïnstalleerde StreamSets app](./media/hdinsight-apps-install-streamsets/streamsets.png)

2. Wanneer u StreamSets Data Collector selecteert, ziet u een koppeling naar de webpagina wordt weergegeven en het pad van SSH-eindpunt. Selecteer de koppeling naar de webpagina.

3. In het dialoogvenster voor aanmelding, kunt u de volgende referenties gebruiken om aan te melden: `admin` en `admin`.

4. Klik op de pagina aan de slag **nieuwe pijplijn maken**.

    ![Nieuwe pijplijn](./media/hdinsight-apps-install-streamsets/get-started.png)

5. Voer een naam op voor de pijplijn ("Hallo wereld") in het venster nieuwe pijplijn, (optioneel) Voer een beschrijving in en selecteer **opslaan**.

6. De gegevensverzamelaar-console wordt weergegeven. Het deelvenster met eigenschappen geeft eigenschappen van de pijplijn.
 
    ![Data Collector-console](./media/hdinsight-apps-install-streamsets/pipeline-canvas.png)

7. U bent nu klaar om te volgen de [StreamSets zelfstudie](https://streamsets.com/documentation/datacollector/latest/help/#Tutorial/Tutorial-title.html). De zelfstudie bevat stapsgewijze instructies voor het maken van uw eerste pijplijn.

## <a name="next-steps"></a>Volgende stappen

* [StreamSets Data Collector documentatie](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html#concept_htw_ghg_jq).
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [HDInsight Linux gebaseerde clusters aanpassen met Script Action](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik scriptacties om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een lege edge-knooppunt voor toegang tot HDInsight-clusters, en voor het testen en HDInsight-toepassingen hosten.
