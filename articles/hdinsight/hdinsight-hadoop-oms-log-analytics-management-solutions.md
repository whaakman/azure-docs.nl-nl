---
title: Oplossingen voor HDInsight-cluster toevoegen aan Azure Log Analytics | Microsoft Docs
description: Informatie over het gebruik van Azure-logboekanalyse voor het maken van aangepaste weergaven voor HDInsight-clusters.
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/08/2017
ms.author: nitinme
ms.openlocfilehash: dc959f763e9a84199130bae845cb62c493676977
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="add-hdinsight-cluster-management-solutions-to-log-analytics"></a>Oplossingen voor HDInsight-cluster toevoegen aan Log Analytics

HDInsight biedt clusterspecifieke oplossingen die u voor Azure-logboekanalyse toevoegen kunt. [Beheeroplossingen](../log-analytics/log-analytics-add-solutions.md) voegen functionaliteit toe aan OMS, waardoor er aanvullende gegevens- en analysefuncties beschikbaar komen voor Log Analytics. Deze oplossingen belangrijk prestatiegegevens verzamelen van uw HDInsight-clusters en bieden de hulpprogramma's om te zoeken naar de metrische gegevens. Deze oplossingen bieden ook visualisaties en -dashboards voor de meeste clustertypen worden ondersteund in HDInsight. Met behulp van de metrische gegevens die u verzamelt van de oplossing, kunt u aangepaste regels voor bewaking en waarschuwingen. 

In dit artikel leert u hoe clusterspecifieke beheeroplossingen toevoegen aan een OMS-werkruimte.

## <a name="prerequisites"></a>Vereisten

* U moet een HDInsight-cluster voor het gebruik van Azure-logboekanalyse hebt geconfigureerd. Zie voor instructies [gebruik Azure Log Analytics met HDInsight-clusters](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="add-cluster-specific-management-solutions"></a>Oplossingen voor het beheer van de cluster-specifieke toevoegen

In deze sectie kunt u een oplossing voor beheer van HBase-cluster toevoegen aan een bestaande OMS-werkruimte.

1. Open een HDInsigt-cluster in de Azure-portal klikt u op **bewaking**, en klik vervolgens op **Open OMS-Dashboard**.

    ![Open OMS-dashboard](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-log-analytics-open-oms-dashboard.png "Open OMS-dashboard")

1. Klik in het dashboard OMS **galerie met oplossingen** of de **ontwerper** pictogram in het linkerdeelvenster.

    ![Oplossing voor het beheer toevoegen in OMS](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-add-management-solution-oms-portal.png "beheeroplossing in OMS toevoegen")

2. In de galerie met oplossingen, klikt u op een van de volgende tegels:

    - HDInsight Hadoop-bewaking
    - HDInsight HBase Monitoring (Preview)
    - HDInsight Kafka bewaking
    - HDInsight Storm bewaking
    - HDInsight Spark bewaking

3. Klik in het volgende scherm op **toevoegen**.  De volgende schermafbeelding ziet de knop toevoegen voor de bewaking van HBase.

     ![Toevoegen van de oplossing voor het beheer van HBase](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/add-hbase-management-solution.png "oplossing voor het beheer van HBase toevoegen")

4. U ziet een tegel op OMS-dashboard voor het beheersysteem voor HBase. Als het cluster dat OMS gekoppeld (als onderdeel van de vereiste voor dit artikel) een HBase-cluster is, wordt de naam van het cluster en het aantal knooppunten in de tegel weergegeven in het cluster.

    ![HBase-beheersysteem toegevoegd](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/added-hbase-management-solution.png "HBase-beheersysteem toegevoegd")

## <a name="next-steps"></a>Volgende stappen

* [Query uitvoeren op Azure-logboekanalyse voor het bewaken van HDInsight-clusters](hdinsight-hadoop-oms-log-analytics-use-queries.md)

## <a name="see-also"></a>Zie ook

* [Werken met OMS Log Analytics](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Maken van regels voor waarschuwingen in Log Analytics](../log-analytics/log-analytics-alerts-creating.md)
