---
title: Oplossingen voor HDInsight-cluster toevoegen aan Azure Log Analytics | Microsoft Docs
description: Informatie over het gebruik van Azure-logboekanalyse voor het maken van aangepaste weergaven voor HDInsight-clusters.
services: hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: nitinme
ms.openlocfilehash: caa10682f8419cfbc0837e5069357e02ee1e5f64
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="add-hdinsight-cluster-management-solutions-to-log-analytics"></a>Oplossingen voor HDInsight-cluster toevoegen aan Log Analytics

HDInsight biedt clusterspecifieke oplossingen die u voor Azure-logboekanalyse toevoegen kunt. [Oplossingen voor](../log-analytics/log-analytics-add-solutions.md) functionaliteit toevoegen aan Log Analytics, bieden aanvullende gegevens hulpprogramma's en analyses. Deze oplossingen belangrijk prestatiegegevens verzamelen van uw HDInsight-clusters en bieden de hulpprogramma's om te zoeken naar de metrische gegevens. Deze oplossingen bieden ook visualisaties en -dashboards voor de meeste clustertypen worden ondersteund in HDInsight. Met behulp van de metrische gegevens die u verzamelt van de oplossing, kunt u aangepaste regels voor bewaking en waarschuwingen. 

In dit artikel leert u hoe clusterspecifieke beheeroplossingen toevoegen aan een werkruimte voor logboekanalyse.

## <a name="prerequisites"></a>Vereisten

* U moet een HDInsight-cluster voor het gebruik van Azure-logboekanalyse hebt geconfigureerd. Zie voor instructies [gebruik Azure Log Analytics met HDInsight-clusters](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="add-cluster-specific-management-solutions"></a>Oplossingen voor het beheer van de cluster-specifieke toevoegen

In deze sectie kunt u een oplossing voor beheer van HBase-cluster toevoegen aan een bestaande werkruimte voor logboekanalyse.

1. Open een HDInsight-cluster in de Azure-portal klikt u op **bewaking**, en klik vervolgens op **Open OMS-Dashboard**.

    ![Open Operations Management Suite-dashboard](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-log-analytics-open-oms-dashboard.png "Open OMS-dashboard")

1. Klik in het dashboard op **galerie met oplossingen** of de **ontwerper** pictogram in het linkerdeelvenster.

    ![Oplossing voor het beheer in logboekanalyse toevoegen](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/hdinsight-add-management-solution-oms-portal.png "beheeroplossing invoegtoepassing voor Operations Management Suite")

2. In de galerie met oplossingen, klikt u op een van de volgende tegels:

    - HDInsight Hadoop-bewaking
    - HDInsight HBase Monitoring (Preview)
    - HDInsight Kafka bewaking
    - HDInsight Storm bewaking
    - HDInsight Spark bewaking

3. Klik in het volgende scherm op **toevoegen**.  De volgende schermafbeelding ziet de knop toevoegen voor de bewaking van HBase.

     ![Toevoegen van de oplossing voor het beheer van HBase](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/add-hbase-management-solution.png "oplossing voor het beheer van HBase toevoegen")

4. U ziet een tegel op het dashboard voor het beheersysteem voor HBase. Als het cluster dat hebt gekoppeld met Operations Management Suite (als onderdeel van de vereiste voor dit artikel) een HBase-cluster is, wordt de naam van het cluster en het aantal knooppunten in de tegel weergegeven in het cluster.

    ![HBase-beheersysteem toegevoegd](./media/hdinsight-hadoop-oms-log-analytics-management-solutions/added-hbase-management-solution.png "HBase-beheersysteem toegevoegd")

## <a name="next-steps"></a>Volgende stappen

* [Query uitvoeren op Azure-logboekanalyse voor het bewaken van HDInsight-clusters](hdinsight-hadoop-oms-log-analytics-use-queries.md)

## <a name="see-also"></a>Zie ook

* [Werken met logboekanalyse](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Maken van regels voor waarschuwingen in Log Analytics](../log-analytics/log-analytics-alerts-creating.md)
