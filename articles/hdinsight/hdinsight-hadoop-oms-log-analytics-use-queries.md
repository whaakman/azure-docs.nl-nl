---
title: Query uitvoeren op Azure-logboekanalyse Azure HDInsight-clusters bewaken | Microsoft Docs
description: Informatie over het uitvoeren van query's op Azure-logboekanalyse voor het bewaken van de taken die worden uitgevoerd in een HDInsight-cluster.
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
ms.date: 09/11/2017
ms.author: nitinme
ms.openlocfilehash: 8fe91bed69a1c06367346041d8caba4aaee4c82a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="query-azure-log-analytics-to-monitor-hdinsight-clusters-preview"></a>Query uitvoeren op Azure-logboekanalyse voor het bewaken van HDInsight-clusters (Preview)

In dit artikel leert kijken u sommige scenario's voor het gebruik van Azure Log Analytics met Azure HDInsight-clusters. Er zijn drie meest voorkomende scenario's:

* HDInsight-cluster metrische gegevens in OMS analyseren
* Zoeken naar specifieke logboekberichten voor HDInsight-clusters
* Waarschuwingen op basis van gebeurtenissen in de clusters maken

## <a name="prerequisites"></a>Vereisten

* U moet een HDInsight-cluster voor het gebruik van Azure-logboekanalyse hebt geconfigureerd. Zie voor instructies [gebruik Azure Log Analytics met HDInsight-clusters](hdinsight-hadoop-oms-log-analytics-tutorial.md).

* U moet toevoegen de HDInsight-cluster-specifieke beheeroplossingen naar de OMS-werkruimte zoals beschreven in [toevoegen HDInsight-cluster beheeroplossingen met logboekanalyse](hdinsight-hadoop-oms-log-analytics-management-solutions.md).

## <a name="analyze-hdinsight-cluster-metrics-in-oms"></a>HDInsight-cluster metrische gegevens in OMS analyseren

In deze sectie doorlopen we de stappen voor het zoeken naar specifieke metrische gegevens voor uw HDInsight-cluster.

1. Open de OMS-dashboard. Open de HDInsight-cluster-blade die u aan Azure-logboekanalyse gekoppeld in de Azure-portal, klikt u op het tabblad controle en klik op **OMS-Dashboard Open**.

    ![Open OMS-dashboard](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "Open OMS-dashboard")

2. Klik in het dashboard OMS vanuit het startscherm op **logboek zoeken**.

    ![Open logboek zoeken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "logboek zoeken openen")

3. In het venster logboek zoeken in de **Begin zoeken hier** in het tekstvak `*` om te zoeken naar alle metrische gegevens voor alle beschikbare metrische gegevens voor alle HDInsight-clusters geconfigureerd voor het gebruik van Azure-logboekanalyse. Druk op ENTER.

    ![Zoek alle metrische gegevens](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "alle metrische gegevens zoeken")

4. Hier ziet u de volgende uitvoer.

    ![Alle uitvoer van de metrische gegevens zoeken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "alle uitvoer van de metrische gegevens zoeken")

5. In het linkerdeelvenster onder **Type** categorie, zoek een metriek die u wilt zich diep in. Voor deze zelfstudie we kiezen `metrics_resourcemanager_queue_root_default_CL`. Schakel het selectievakje overeenkomt met de metrische gegevens en klik vervolgens op **toepassen**.

    > [!NOTE]
    > Mogelijk moet u op de **[+] meer** om te zoeken van de metrische gegevens die u zoekt. Ook de **toepassen** knop aan de onderkant van de lijst is dus u moet Schuif omlaag om het te bekijken.
    > 
    >    
    U ziet dat de query in het tekstvak nu verandert in een indeling die wordt weergegeven in het gemarkeerde in de volgende schermafbeelding:

    ![Zoeken naar specifieke metrische gegevens](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "zoeken naar specifieke metrische gegevens")

6. U kunt nu verdiepen in deze specifieke metrische gegevens. U kunt nu bijvoorbeeld de uitvoer van de bestaande op basis van het gemiddelde van de resources die worden gebruikt in een interval van 10 minuten gecategoriseerd door de clusternaam verfijnen. Typ de volgende query in het tekstvak voor de query.

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure avg(UsedAMResourceMB_d) by ClusterName_s interval 10minute

    ![Zoeken naar specifieke metrische gegevens](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics.png "zoeken naar specifieke metrische gegevens")

7. In plaats van verfijnen op basis van het gemiddelde van de resources die worden gebruikt, kunt u de volgende query te verfijnen van de resultaten op basis van wanneer het maximum aantal bronnen (evenals 90 en 95e percentiel) zijn gebruikt in een venster 10 minuten.

        * (Type=metrics_resourcemanager_queue_root_default_CL) | measure max(UsedAMResourceMB_d) , pct95(UsedAMResourceMB_d), pct90(UsedAMResourceMB_d)  by ClusterName_s interval 10minute

    ![Zoeken naar specifieke metrische gegevens](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-more-specific-metrics-1.png "zoeken naar specifieke metrische gegevens")

## <a name="search-for-specific-log-messages-in-hdinsight-clusters"></a>Zoeken naar specifieke logboekberichten in HDInsight-clusters

In deze sectie doorlopen we de stappen voor het zoeken van foutberichten tijdens een specifiek tijdvenster. De stappen die hier zijn slechts één voorbeeld van hoe u kunt aankomt bij het foutbericht u bent geïnteresseerd. U kunt de eigenschap die beschikbaar is op zoek naar de fouten die u probeert te vinden.

1. Open de OMS-dashboard. Open de HDInsight-cluster-blade die u aan Azure-logboekanalyse gekoppeld in de Azure-portal, klikt u op het tabblad controle en klik op **OMS-Dashboard Open**.

    ![Open OMS-dashboard](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "Open OMS-dashboard")

2. Klik in het dashboard OMS vanuit het startscherm op **logboek zoeken**.

    ![Open logboek zoeken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "logboek zoeken openen")

3. In het venster logboek zoeken in de **Begin zoeken hier** in het tekstvak `"Error"` (met aanhalingstekens) om te zoeken naar alle foutberichten voor alle HDInsight-clusters geconfigureerd voor het gebruik van Azure-logboekanalyse. Druk op ENTER.

    ![Zoek alle fouten](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors.png "zoeken van alle fouten")

4. Hier ziet u de volgende uitvoer.

    ![Zoek alle fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "alle fouten uitvoer zoeken")

5. In het linkerdeelvenster onder **Type** categorie, zoek een fouttype die u wilt zich diep in. Voor deze zelfstudie we kiezen `log_sparkappsexecutors_CL`. Schakel het selectievakje overeenkomt met de metrische gegevens en klik vervolgens op **toepassen**.

    ![Zoeken naar specifieke fouten](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error.png "zoeken naar specifieke fout")

        
6. U ziet dat de query in het tekstvak nu verandert in een indeling die wordt weergegeven in het onderstaande vak gemarkeerde en de resultaten zijn verfijnd alleen de fout van het geselecteerde type worden weergegeven.

    ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-output.png "zoeken naar specifieke fouten uitvoer")

7. U kunt nu verdiepen in deze lijst van de specifieke fout met behulp van de beschikbare opties in het linkerdeelvenster. U kunt bijvoorbeeld de query om te kijken alleen foutberichten van een specifieke werkrolknooppunt verfijnen.

    ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "zoeken naar specifieke fouten uitvoer")

8. U kunt meer zone van de tijd die u denkt dat de fout is opgetreden dat door de relevante tijdwaarde te selecteren in het linkerdeelvenster.

    ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "zoeken naar specifieke fouten uitvoer")

9. U bent nu naar beneden op de specifieke fout die u zoekt. U kunt klikken op **[+] meer** om te kijken naar het daadwerkelijke foutbericht weergeeft.

    ![Zoeken naar specifieke fouten uitvoer](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "zoeken naar specifieke fouten uitvoer")

## <a name="create-alerts-to-track-events"></a>Waarschuwingen voor gebeurtenissen bijhouden maken

Er is de eerste stap bij het maken van een waarschuwing moet worden uitgevoerd op een query op basis van de waarschuwing wordt geactiveerd. Gebruik voor eenvoud, gaan we de volgende query waarmee een lijst met mislukte toepassingen op HDInsight-clusters.

    * (Type = metrics_resourcemanager_queue_root_default_CL) AppsFailed_d > 0 

U kunt een query die u wilt dat een waarschuwing wilt maken.

1. Open de OMS-dashboard. Open de HDInsight-cluster-blade die u aan Azure-logboekanalyse gekoppeld in de Azure-portal, klikt u op het tabblad controle en klik op **OMS-Dashboard Open**.

    ![Open OMS-dashboard](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-open-oms-dashboard.png "Open OMS-dashboard")

2. Klik in het dashboard OMS vanuit het startscherm op **logboek zoeken**.

    ![Open logboek zoeken](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-click-log-search.png "logboek zoeken openen")

3. In het venster logboek zoeken in de **Begin zoeken hier** tekst vak, plak de query die u wilt maken van een waarschuwing, druk op ENTER en klik vervolgens op de **waarschuwing** knop.

    ![Voer query voor het maken van een waarschuwing](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Enter query voor het maken van een waarschuwing")

4. In de **waarschuwingsregel toevoegen** venster, voer de query en andere details maken van een waarschuwing en klik vervolgens op **opslaan**.

    ![Voer query voor het maken van een waarschuwing](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Enter query voor het maken van een waarschuwing")

    In deze schermafbeelding sturen we alleen een e-mailmelding als de Waarschuwingsquery uitvoer haalt.

5. U kunt ook bewerken of verwijderen van een bestaande waarschuwing. Om dit te doen vanuit elke pagina in de OMS-portal klikt u op de **instellingen** pictogram.

    ![Voer query voor het maken van een waarschuwing](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png "Enter query voor het maken van een waarschuwing")

6. Van de **instellingen** pagina, klikt u op **waarschuwingen** om te zien van de waarschuwingen die u hebt gemaakt. U kunt ook inschakelen of uitschakelen van een waarschuwing, bewerken of verwijderen. Zie voor meer informatie [werken met regels voor waarschuwingen in logboekanalyse](../log-analytics/log-analytics-alerts-creating.md).

## <a name="see-also"></a>Zie ook

* [Werken met OMS Log Analytics](https://blogs.msdn.microsoft.com/wei_out_there_with_system_center/2016/07/03/oms-log-analytics-create-tiles-drill-ins-and-dashboards-with-the-view-designer/)
* [Maken van regels voor waarschuwingen in Log Analytics](../log-analytics/log-analytics-alerts-creating.md)
