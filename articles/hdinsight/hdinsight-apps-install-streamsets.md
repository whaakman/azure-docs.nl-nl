---
title: Installatie gepubliceerde toepassing - gegevensverzamelaar StreamSets - Azure HDInsight | Microsoft Docs
description: Installeren en gebruiken van de toepassing StreamSets gegevensverzamelaar van derden Hadoop.
services: hdinsight
documentationcenter: ''
author: ashishthaps
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ''
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: ashish
ms.openlocfilehash: e433de82576f8b943988881ed0b6673c0dccd77e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="install-published-application---streamsets-data-collector"></a>Installeren van de gepubliceerde toepassing - StreamSets gegevensverzamelaar

Dit artikel wordt beschreven hoe u wilt installeren en uitvoeren de [StreamSets gegevensverzamelaar voor HDInsight](https://streamsets.com/) gepubliceerde toepassing in Azure HDInsight Hadoop. Zie voor een overzicht van het HDInsight-toepassing-platform en een lijst van beschikbare Independent Software Vendors (ISV's) gepubliceerde toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md). Zie voor instructies voor de installatie van uw eigen toepassing [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).

## <a name="about-streamsets-data-collector"></a>Over StreamSets gegevensverzamelaar

De gegevensverzamelaar StreamSets implementeert boven op een Azure HDInsight-toepassing. Gegevensverzamelaar StreamSets biedt een complete integrated development environment (IDE), of kunt u ontwerpen, testen, implementeren en beheren any-to-any pijplijnen opnemen. Deze pijplijnen kunnen net stroom en de batch-gegevens en tal van transformaties-stream bevatten alle zonder aangepaste code schrijven.

Gegevensverzamelaar StreamSets kunt u gegevensoverdrachten build talrijke Big Data-componenten zoals HDFS, Kafka, Solr, Hive, HBASE en Kudu gebruiken. Zodra de gegevensverzamelaar StreamSets wordt uitgevoerd op een edge-server of in de Hadoop-cluster, krijgt u real-time bewaking van zowel afwijkingen van de gegevens als gegevens stroom bewerkingen. Deze bewaking bevat waarschuwingen op basis van drempelwaarden afwijkingsdetectie en automatisch herstel van foutrecords.

Gegevensverzamelaar StreamSets is ontworpen voor elke fase van een pijplijn logisch isoleren zodat u kunt voldoen aan de nieuwe bedrijfsbehoeften door slepen en neerzetten in nieuwe processors en connectors zonder coderen en met minimale downtime.

### <a name="streamsets-resource-links"></a>Koppelingen naar hulpbronnen over StreamSets

* [Documentatie](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html)
* [Blog](https://streamsets.com/blog/)
* [Zelfstudies](https://github.com/streamsets/tutorials)
* [Forum voor ondersteuning van ontwikkelaars](https://groups.google.com/a/streamsets.com/forum/#!forum/sdc-user)
* [Vertraging openbare StreamSets kanaal](https://streamsetters.slack.com/)
* [Broncode](https://github.com/streamsets)

## <a name="prerequisites"></a>Vereisten

Om deze app installeren op een nieuwe HDInsight-cluster of een bestaand cluster, hebt u de volgende configuratie:

* Cluster tier(s): Standard of Premium
* Cluster-versie (s): 3.5 en hoger

## <a name="install-the-streamsets-data-collector-published-application"></a>Installeer de gegevensverzamelaarset StreamSets gepubliceerde toepassing

Lees voor stapsgewijze instructies over het installeren van deze en andere beschikbare ISV-toepassingen [Hadoop-toepassingen van derden installeren](hdinsight-apps-install-applications.md).

## <a name="launch-streamsets-data-collector"></a>Gegevensverzamelaar StreamSets starten

1. Na de installatie, kunt u StreamSets starten van het cluster in Azure-portal door te gaan naar de **instellingen** deelvenster selecteren **toepassingen** onder de **algemene** categorie. In het deelvenster geïnstalleerde Apps worden de geïnstalleerde toepassingen.

    ![Geïnstalleerde StreamSets app](./media/hdinsight-apps-install-streamsets/streamsets.png)

2. Als u de gegevensverzamelaarset StreamSets selecteert, ziet u een koppeling naar de webpagina en het pad van SSH-eindpunt. Selecteer de koppeling van de webpagina.

3. Klik in het dialoogvenster aanmelding kunt u de volgende referenties gebruiken om aan te melden: `admin` en `admin`.

4. Klik op de pagina aan de slag **nieuwe pijplijn maken**.

    ![Nieuwe pijplijn maken](./media/hdinsight-apps-install-streamsets/get-started.png)

5. Voer een naam voor de pijplijn ('Hallo wereld') in het venster nieuwe pijplijn, eventueel een beschrijving invoeren en selecteer **opslaan**.

6. De gegevensverzamelaar-console wordt weergegeven. Het deelvenster Eigenschappen weergeven pipeline-eigenschappen
 
    ![Data Collector console](./media/hdinsight-apps-install-streamsets/pipeline-canvas.png)

7. U bent nu klaar om te volgen de [StreamSets zelfstudie](https://streamsets.com/documentation/datacollector/latest/help/#Tutorial/Tutorial-title.html). De zelfstudie biedt stapsgewijze instructies voor het maken van uw eerste pijplijn.

## <a name="next-steps"></a>Volgende stappen

* [Gegevensverzamelaar StreamSets documentatie](https://streamsets.com/documentation/datacollector/latest/help/#Getting_Started/GettingStarted_Title.html#concept_htw_ghg_jq).
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md): informatie over het implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* [HDInsight-toepassingen publiceren](hdinsight-apps-publish-applications.md): informatie over het publiceren van aangepaste HDInsight-toepassingen in Azure Marketplace.
* [MSDN: een HDInsight-toepassing installeren](https://msdn.microsoft.com/library/mt706515.aspx): informatie over het definiëren van HDInsight-toepassingen.
* [Met behulp van de scriptactie Linux gebaseerde HDInsight-clusters aanpassen](hdinsight-hadoop-customize-cluster-linux.md): informatie over het gebruik van scriptactie om extra toepassingen te installeren.
* [Lege edge-knooppunten gebruiken in HDInsight](hdinsight-apps-use-edge-node.md): informatie over het gebruik van een leeg edge-knooppunt voor toegang tot HDInsight-clusters en voor het testen en die als host fungeert voor HDInsight-toepassingen.
